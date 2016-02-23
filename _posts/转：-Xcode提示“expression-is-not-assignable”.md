title: 转： Xcode提示“expression is not assignable”
date: 2015-07-28 22:46:54
categories: iOS
tags:
---
		你的问题是：

		self.view.frame.size.height = 100f;
		这样写没法通过编译，编译器会报错"expression is not assignable"

		原因是，这句话里面的几个点有两种不同的含义。self.view.frame是Objective-C语法，是读取view属性的frame属性，在Objective-C中使用点来访问属性只是一种语法糖，所以self.view.frame这句话会被转换成：

		[[self view] frame]
		也就是说，实际上这是消息传递。

		而frame属性是一个CGRect结构，所以frame.size.height是C语言的语法，就是访问CGRect结构中的size字段，同样，height是CGSize结构的一个字段。所以，你这句话实际上等于：

		[[self view] frame].size.height = 100f;
		而Objective-C只是对C语言的一个扩展，所以，上面这句话会被转成C语言的函数调用形式，类似于这种形式：

		getframe().size.height = 100f;
		而在C语言里，函数的返回值是一个R-Value，是不能直接给它赋值的（所谓的R-Value，就是只能出现在等号的右边，你可以理解成是一个常量；而可以被赋值的是L-Value，可以出现在等号的左边，通常是变量）。因此，当你打算直接给函数的返回值赋值的时候，编译器告诉你"这个表达式无法被赋值"。这就是这个错误的出现原因。

		所以，解决办法就是，用一个临时变量保存这个函数的返回值，修改这个临时变量，然后再赋给frame：

		// 1. 用一个临时变量保存返回值。
		CGRect temp = self.view.frame;

		// 2. 给这个变量赋值。因为变量都是L-Value，可以被赋值
		temp.size.height = 100f;

		// 3. 修改frame的值
		self.view.frame = temp;
		我知道这样写看起来有点笨，也许有一天，Objective-C的编译器会变的智能一点，自动完成这种转换。
参:[http://segmentfault.com/q/1010000000177216](http://segmentfault.com/q/1010000000177216)