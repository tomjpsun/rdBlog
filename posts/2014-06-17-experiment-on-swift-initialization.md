layout: post
title: "Experiment on Swift Initialization"
date: 2014-06-17 16:19:36 +0800
comments: true
Category: Ios
Tags: Swift, Initialization
Has_Math: True

The [*Swift Programming Language*](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/)  has a chapter about the __Initialization__. There are __Designate initializer__ and __Convenient initializer__. Here we make a simple experiment to observe how it works.

<!-- more -->
	:::Objective-C

	import UIKit

	class Food {
	    var name: String
	    init(name: String) {
	        println("[desig] Food init ")
	        self.name = name
	    }
	    convenience init() {
	        println("[conve] Food init")
	        self.init(name: "[Unnamed]")
	    }
	}

	class RecipeIngredient: Food {
	    var quantity: Int
	    init(name: String, quantity: Int) {
	        println("[desig] Recp init")
	        self.quantity = quantity
	        super.init(name: name)
	    }
	    convenience init(name: String) {
	        println("[conve] Recp init")
	        self.init(name: name, quantity: 1)
	    }
	}



## Designator-Designator

For the statement:

{% codeblock lang:objc %}

	let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)

{% endcodeblock %}

We get the output:

{% codeblock lang:objc %}

	[desig] Recp init
	[desig] Food init

{% endcodeblock %}

## Convenient-Designator

For the statement:

{% codeblock lang:objc %}

	let oneBacon = RecipeIngredient(name: "Bacon")

{% endcodeblock %}

We get the output:

{% codeblock lang:objc %}

	[conve] Recp init
	[desig] Recp init
	[desig] Food init

{% endcodeblock %}

## Implicit inheritent

For the statement:

{% codeblock lang:objc %}

	let oneMysteryItem = RecipeIngredient()

{% endcodeblock %}

We get the output:

{% codeblock lang:objc %}

	[conve] Food init
	[conve] Recp init
	[desig] Recp init
	[desig] Food init

{% endcodeblock %}

This is because the `RecipeIngredient` class inherit all initializers from the `Food`, including the one without parameter, and it triggers up the sequences of other initializers.

#
