title: "C++ virtual destructor"
date: 2018-08-09 15:00:00 +0800
Category: C++
Tags: Virtual
Has_Math: True

曾經以為很熟練的 C++ virtual function, 最近用起來遇到 compiler warning,
溫故知新, 趁機會再澄清一些模糊的觀念.
<!-- more -->
用簡單的程式說明比較清楚：

	:::cpp

	#include <iostream>
	using namespace std;

	enum ShapeColor
	{
		Red,
		Green,
		Blue
	};

	class Shape
	{
	public:
		void draw(ShapeColor color=Red) const {
			doDraw(color);
		}

	private:
		virtual void doDraw(ShapeColor color) const {
			cout << "Shape's " << __func__ << " color = " << color << endl;
		}
	};

	class Rectangle : public Shape
	{
		private:
			virtual void doDraw(ShapeColor color) const;
	};


	void Rectangle::doDraw(ShapeColor color) const
	{
		cout << "Rectangle's " << __func__ << ", color = " << color << endl;
	}

如果這樣寫, 我們會遇到 compiler 抱怨:

	warning: delete called on non-final 'Shape' that has virtual functions but non-virtual destructor

因為我們使用 polymorphism, 就是說 當我們使用一個 base 物件指標指到一個衍生物件, 當呼叫 base class member function,
就會執行屬於衍生物件相同的 function 實作, 這是 polymorphism 的主要精神.

如果我們沒有將 base 物件的 destructor 宣告為 virtual, 那麼 delete 指標時, 只有 base 物件的 destructor 會
被執行. 因此,我們若希望 衍生物件的 destructor 被執行, 那麼它們的 destructor 就必須要宣告成 virtual. 以上的
warning 就是警告我們這件事.

修改 Shape 跟 Rectangle 如下(Shape, Rectangle 都加上 destructor), 就可以解決這個 warning:

	:::cpp
	class Shape
	{
	public:
		void draw(ShapeColor color=Red) const {
			doDraw(color);
		}
		virtual ~Shape() { cout << "Destructor of Shape" << endl;};
	private:
		virtual void doDraw(ShapeColor color) const {
			cout << "Shape's " << __func__ << " color = " << color << endl;
		}
	};


	class Rectangle : public Shape
	{
	public:
		virtual ~Rectangle() { cout << "Destructor of Rectangle" << endl;};

	private:
		virtual void doDraw(ShapeColor color) const;
	};


	void Rectangle::doDraw(ShapeColor color) const
	{
		cout << "Rectangle's " << __func__ << ", color = " << color << endl;
	}

	int main()
	{
		Shape *p = new Shape;
		p->draw();
		delete p;
		Shape *q = new Rectangle;
		q->draw();
		delete q;
		return 0;
	}

沒有 warning 了, 執行結果這樣:

	Shape's doDraw color = 0
    Destructor of Shape
    Rectangle's doDraw, color = 0
    Destructor of Rectangle
    Destructor of Shape

順便一提, 這裡有趣的觀察是執行 `delete q` 後,
可以看到 base 以及衍生物件的 destructor 都被呼叫了.

c++ 是無窮無盡的功夫, 能做的, 就是：遇到不懂就補觀念, 大家一同修煉吧～
