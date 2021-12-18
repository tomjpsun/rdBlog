Title: Blender Rendering Code Trace(2)
Date: 2010-12-03 10:20
Modified: 2010-12-05 19:30
Category: Blender
Tags: Study, Cycle
Summary: trace note
Has_Math: True

上次 [Blender Rendering Code Trace(1)](http://rd.coding-addict.com/blender-rendering-code-trace1.html) 我們看到 load_kernel(), load_kernel() 做完之後就執行 run_cpu() or run_gpu(), 它們的過程, 都是透過切成多個 tiles, 然後每個 tile 請 thread 完成, 我們從 thread 執行的程序裡, 找一個 切入點 開始 trace:
<!--More-->

`Session::update_scene()`,	 `Session::run_cpu()`, `Session::run_gpu()`

以上三個 methods 都會呼叫 `Scene::device_update()`

就從這裡開始吧：


`Scene::device_update()` 裡面的  `shader_manager->device_update()` 會執行

`SVMShaderManager::device_update()` 或
`OSLShaderManager::device_update()`

user 若有選擇 OSL 就會呼叫後者, 否則內定為呼叫前者

從 `SVMShaderManager::device_update()` 追蹤進去,它對於 scene 裡面所有的 shader, 透過 boost::bind()
的機制執行 SVMShaderManager::device_update_shader(), 進而呼叫到 `SVMCompiler::compile()`:

	void SVMCompiler::compile(Scene *scene,
	                          Shader *shader,
	                          vector<int4>& global_svm_nodes,
	                          int index,
	                          Summary *summary)
	{
		/* copy graph for shader with bump mapping */
		ShaderNode *node = shader->graph->output();
		int start_num_svm_nodes = global_svm_nodes.size();

		const double time_start = time_dt();

		if(node->input("Surface")->link && node->input("Displacement")->link)
			if(!shader->graph_bump)
				shader->graph_bump = shader->graph->copy();

		/* finalize */
		{
			scoped_timer timer((summary != NULL)? &summary->time_finalize: NULL);
			shader->graph->finalize(scene,
			                        false,
			                        false,
			                        shader->has_integrator_dependency);
		}

		if(shader->graph_bump) {
			scoped_timer timer((summary != NULL)? &summary->time_finalize_bump: NULL);
			shader->graph_bump->finalize(scene,
			                             true,
			                             false,
			                             shader->has_integrator_dependency);
		}

		current_shader = shader;

		shader->has_surface = false;
		shader->has_surface_emission = false;
		shader->has_surface_transparent = false;
		shader->has_surface_bssrdf = false;
		shader->has_bssrdf_bump = false;
		shader->has_volume = false;
		shader->has_displacement = false;
		shader->has_surface_spatial_varying = false;
		shader->has_volume_spatial_varying = false;
		shader->has_object_dependency = false;
		shader->has_integrator_dependency = false;

		/* generate bump shader */
		if(shader->displacement_method != DISPLACE_TRUE && shader->graph_bump) {
			scoped_timer timer((summary != NULL)? &summary->time_generate_bump: NULL);
			compile_type(shader, shader->graph_bump, SHADER_TYPE_BUMP);
			global_svm_nodes[index].y = global_svm_nodes.size();
			global_svm_nodes.insert(global_svm_nodes.end(), svm_nodes.begin(), svm_nodes.end());
		}

		/* generate surface shader */
		{
			scoped_timer timer((summary != NULL)? &summary->time_generate_surface: NULL);
			compile_type(shader, shader->graph, SHADER_TYPE_SURFACE);
			/* only set jump offset if there's no bump shader, as the bump shader will fall thru to this one if it exists */
			if(shader->displacement_method == DISPLACE_TRUE || !shader->graph_bump) {
				global_svm_nodes[index].y = global_svm_nodes.size();
			}
			global_svm_nodes.insert(global_svm_nodes.end(), svm_nodes.begin(), svm_nodes.end());
		}

		/* generate volume shader */
		{
			scoped_timer timer((summary != NULL)? &summary->time_generate_volume: NULL);
			compile_type(shader, shader->graph, SHADER_TYPE_VOLUME);
			global_svm_nodes[index].z = global_svm_nodes.size();
			global_svm_nodes.insert(global_svm_nodes.end(), svm_nodes.begin(), svm_nodes.end());
		}

		/* generate displacement shader */
		{
			scoped_timer timer((summary != NULL)? &summary->time_generate_displacement: NULL);
			compile_type(shader, shader->graph, SHADER_TYPE_DISPLACEMENT);
			global_svm_nodes[index].w = global_svm_nodes.size();
			global_svm_nodes.insert(global_svm_nodes.end(), svm_nodes.begin(), svm_nodes.end());
		}

		/* Fill in summary information. */
		if(summary != NULL) {
			summary->time_total = time_dt() - time_start;
			summary->peak_stack_usage = max_stack_use;
			summary->num_svm_nodes = global_svm_nodes.size() - start_num_svm_nodes;
		}
	}

`SVMCompiler::compile()` 做三件重要的事:

1. finalize
2. 依序執行 `compile_type()`, type 為 _Bump, Surface, Volume, Displacement_ 四種
3. 每一次 compile_type() 後, 把產生的 4-tuples copy to `global_svm_nodes`
######note: global_svm_nodes 是 device_update() 裡面的 local variable, 由 compile() 的第三個參數傳入， 負責收集 compile() 產生的 4-tuples

##finalize:##
呼叫 `ShaderGraph::finalize()`, 它找出 ShaderGraph 的 output node 的兩個 input link: _Surface_, _Volume_ 並往前回溯所有的 nodes, 對它們做 `transform_multi_closure()`



<img src="http://coding-addict.com/pictures/rd/shadernode data structure.png" alt="Drawing" style="width: 500px;"/>

補充： 下圖: Node Graph 都有一個 output node, 上圖: Shader Node data structures 

`transform_multi_closure()` 大致上是把所有的 node 換成

1. `MixClosureWeightNode`(如果 node type 是`SHADER_SPECIAL_TYPE_COMBINE_CLOSURE`), 而且 self recursive 一路遞迴,解決所有此類的 nodes, 或是
2. `MathNode` (如果 node type 不是`SHADER_SPECIAL_TYPE_COMBINE_CLOSURE`) 化 closure 結果為某個數值.

為甚麼要分這兩種？ code 有些複雜, 等全部架構清楚後再來了解, 目前我仍不知道原因.

##compile type():##
##copy 4-tuples:##
