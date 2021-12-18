layout: post
title: "Anchor point and position of a Sprite node"
date: 2014-02-07 17:14:10 +0800
comments: true
Category: Cocos2D
Tags: Anchor,Sprite
Has_Math: True

This is an abstract of [this article](http://www.koboldtouch.com/display/IDCAR/How+The+Anchor+Point+Works+And+What+To+Use+It+For).

The meaning of the anchor point:

`The anchorPoint defines where the node's texture is drawn relative to the node's position.`


More explanation:
<!-- more -->
Let's say you have a CCSprite with a 100x100 texture, and you need to rotate the sprite. But the texture should rotate around the 25x25 point and not its center point.


	:::Objective-C

	sprite.anchorPoint = CGPointMake(0.25, 0.25);
	sprite.rotation = 60;


The above is not a good way to do so, since the sprite's position is no longer at the center, and it's bad for you to do collision detection. A correct way is :

	:::Objective-C

	// Create the CCNode and position it. Then add it to the sprite's parent.
	CCNode* centerOfRotation = [CCNode node];
	centerOfRotation.position = CGPointMake(200, 200);
	[self addChild:centerOfRotation];

	CCSprite* sprite = [CCSprite spriteWithFile:@"Icon.png"];
	// offset the sprite to the position where it should rotate around
	sprite.position = CGPointMake(25, 25);
	[centerOfRotation addChild:sprite];

	// rotate the CCNode, this rotates its children along with it
	centerOfRotation.rotation = 60;

