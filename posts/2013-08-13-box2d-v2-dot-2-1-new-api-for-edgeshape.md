title: "Box2d v2.2.1 New API for EdgeShape"
date: 2013-08-13 16:54
comments: true
Category: Box2D
Tags: Tutorial
Has_Math: True

目前找到 Box2d 最詳細的教學是 <http://www.iforce2d.net/b2dtut/> ，還有簡體中文翻譯版。但是附帶的教學範例 是使用 Box2d v2.1.2 版，到了 v2.2.1 版本裡面，有一些 API 更動，導致無法順利 compile.
<!-- more -->

這裡列出一些 v2.2.1 可使用的片段碼，以便於 Copy & Past 到教學範例。
v2.2.1 新增的 EdgeShape，畫邊框不再使用 PolygonShape

	:::Objective-C

        //add four walls to the static body
        b2BodyDef myWallDef;
        myWallDef.type = b2_staticBody;
        myWallDef.position.Set(0, 0);
        b2Body* wallBody = m_world->CreateBody(&myWallDef);

        b2FixtureDef myWallFixture;
        b2EdgeShape wallShape;

        b2Vec2 bl(-20, 0);
        b2Vec2 br( 20, 0);
        b2Vec2 tl(-20,40);
        b2Vec2 tr( 20,40);

        wallShape.Set(bl, br); // ground
        myWallFixture.shape = &wallShape;
        wallBody->CreateFixture(&myWallFixture);

        wallShape.Set(tl, tr); // ceiling
        myWallFixture.shape = &wallShape;
        wallBody->CreateFixture(&myWallFixture);

        wallShape.Set(bl, tl); // left wall
        myWallFixture.shape = &wallShape;
        wallBody->CreateFixture(&myWallFixture);

        wallShape.Set(br, tr); // right wall
        myWallFixture.shape = &wallShape;
        wallBody->CreateFixture(&myWallFixture);


