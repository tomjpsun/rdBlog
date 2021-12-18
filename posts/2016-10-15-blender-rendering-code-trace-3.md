Title: Blender Rendering Code Trace(3)
Date: 2016-10-15 10:20
Modified: 2016-10-15 19:30
Category: Blender
Tags: Study, Cycle
Summary: trace note
Has_Math: True

上一篇有提到 `transform_multi_closure()`, 其實裡面細節蠻值得一看的, 今天本篇就來細說她所做的動作, program 如下：

	:::c++
	 1 		void ShaderGraph::transform_multi_closure(ShaderNode *node, ShaderOutput *weight_out, bool volume)
	 2 		{
	 3 			/* for SVM in multi closure mode, this transforms the shader mix/add part of
	 4 			 * the graph into nodes that feed weights into closure nodes. this is too
	 5 			 * avoid building a closure tree and then flattening it, and instead write it
	 6 			 * directly to an array */
	 7
	 8 			if(node->special_type == SHADER_SPECIAL_TYPE_COMBINE_CLOSURE) {
	 9 				ShaderInput *fin = node->input("Fac");
	10 				ShaderInput *cl1in = node->input("Closure1");
	11 				ShaderInput *cl2in = node->input("Closure2");
	12 				ShaderOutput *weight1_out, *weight2_out;
	13
	14 				if(fin) {
	15 					/* mix closure: add node to mix closure weights */
	16 					MixClosureWeightNode *mix_node = new MixClosureWeightNode();
	17 					add(mix_node);
	18 					ShaderInput *fac_in = mix_node->input("Fac");
	19 					ShaderInput *weight_in = mix_node->input("Weight");
	20
	21 					if(fin->link)
	22 						connect(fin->link, fac_in);
	23 					else
	24 						mix_node->fac = node->get_float(fin->socket_type);
	25
	26 					if(weight_out)
	27 						connect(weight_out, weight_in);
	28
	29 					weight1_out = mix_node->output("Weight1");
	30 					weight2_out = mix_node->output("Weight2");
	31 				}
	32 				else {
	33 					/* add closure: just pass on any weights */
	34 					weight1_out = weight_out;
	35 					weight2_out = weight_out;
	36 				}
	37
	38 				if(cl1in->link)
	39 					transform_multi_closure(cl1in->link->parent, weight1_out, volume);
	40 				if(cl2in->link)
	41 					transform_multi_closure(cl2in->link->parent, weight2_out, volume);
	42 			}
	43 			else {
	44 				ShaderInput *weight_in = node->input((volume)? "VolumeMixWeight": "SurfaceMixWeight");
	45
	46 				/* not a closure node? */
	47 				if(!weight_in)
	48 					return;
	49
	50 				/* already has a weight connected to it? add weights */
	51 				float weight_value = node->get_float(weight_in->socket_type);
	52 				if(weight_in->link || weight_value != 0.0f) {
	53 					MathNode *math_node = new MathNode();
	54 					add(math_node);
	55
	56 					if(weight_in->link)
	57 						connect(weight_in->link, math_node->input("Value1"));
	58 					else
	59 						math_node->value1 = weight_value;
	60
	61 					if(weight_out)
	62 						connect(weight_out, math_node->input("Value2"));
	63 					else
	64 						math_node->value2 = 1.0f;
	65
	66 					weight_out = math_node->output("Value");
	67 					if(weight_in->link)
	68 						disconnect(weight_in);
	69 				}
	70
	71 				/* connected to closure mix weight */
	72 				if(weight_out)
	73 					connect(weight_out, weight_in);
	74 				else
	75 					node->set(weight_in->socket_type, weight_value + 1.0f);
	76 			}
	77 		}

line 8: 判斷 `node->special_type` 是否為 `SHADER_SPECIAL_TYPE_COMBINE_CLOSURE` 分別跑 Line 9-31 或是 Line 32-76

Line 9-12: 找到 node input array 上名為 `Fac`, `Closure1`, `Closure2` 的 input (圓圈)

Line 14-31: `ShaderGraph::nodes` 其實是一個 `boost:vector` 的物件, 存放所有 `ShaderGrpah` 裡面的 nodes.
這裡我們新增一個 node: `MixClosureWeightNode`, `fac_in` 與 `weight_in` 分別指向它的兩個 input
