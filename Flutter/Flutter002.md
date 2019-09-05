本文对应github地址[Flutter2](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter002.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> ### 为什么Flutter选择了Dart语言(不用js)
 
1. Dart 的性能更好。JIT模式速度与JavaScript持平，AOT模式性能好于JavaScript。
2. 缩短开发周期。Debug时默认JIT，且支持热重载。
3. Native Binding。JavaScript在Android上Native Binding可以很好地实现，但iOS上的 JavaScriptCore不可以，Dart的 Native Binding可以很好地通过 Dart Lib实现。
4. Fuchsia OS。Fuchsia OS内置的应用浏览器就是使用 Dart语言作为 App的开发语言。而且实际上，Flutter是 Fuchisa OS的应用框架概念上的一个子集。（Flutter源代码和编译工具链也充斥了大量的 Fuchsia宏）
5. Dart是类型安全的语言，拥有完善的包管理和诸多特性。Google召集了如此多个编程语言界的设计专家开发出这样一门语言，旨在取代 JavaScript，所以 Fuchsia OS内置了 Dart。Dart可以作为 embedded lib嵌入应用，而不用只能随着系统升级才能获得更新，这也是优势之一。 
6. 
 
 
> ### Skia是什么

* Skia是一个 2D的绘图引擎库，其前身是一个向量绘图软件，Chrome和 Android均采用 Skia作为绘图引擎。
* Skia提供了非常友好的 API，并且在图形转换、文字渲染、位图渲染方面都提供了友好、高效的表现。
* Skia是跨平台的，所以可以被嵌入到 Flutter的 iOS SDK中，而不用去研究 iOS闭源的 Core Graphics / Core Animation。
* Android自带了 Skia，所以 Flutter Android SDK要比 iOS SDK小很多。


> ### Flutter Framework

* 这是一个纯 Dart实现的 SDK，类似于 React在 JavaScript中的作用，它实现了一套基础库， 用于处理动画、绘图和手势。
* 其绘图封装了一套 UI库(dart:ui)，有 Material 和Cupertino两种视觉风格。我们在使用 Flutter写 App的时候，直接导入这个库即可使用组件等功能


> ### Flutter Engine

* 这是一个纯 C++实现的 SDK，其中囊括了 Skia引擎、Dart运行时、文字排版引擎等.
* 它可以以 JIT 或 AOT的模式运行 Dart代码。
* 在代码调用 dart:ui库时，提供 dart:ui库中 Native Binding 实现。 
* 他还控制着 VSync信号的传递、GPU数据的填充等，并且还负责把客户端的事件传递到运行时中的代码

> ### Flutter命名规范

* 文件名: 小写+下划线
* 类型名(类名、函数类型名):大写开头驼峰
* 变量名(包含const final 常量):使用小写开头驼峰
* const可以使用大写+下划线的方式
* 方法名:使用小写开头驼峰
* 导包as后的名称为小写+下划线
* 不要用匈牙利命名法中的kXXXX 这样的命名方式,应该去掉k
* 超过两位的英文缩写一律按该单词为普通小写单词处理,使用小写

> ### 一些关键字

1. extends(继承关键字)

* Flutter中的继承是单继承
* 构造函数不能继承
* 子类重写超类的方法，要用@override
* 子类调用超类的方法，要用super


2. Scaffold(实现了基本的 Material Design 布局结构)

* appBar: 界面顶部导航条
* body: 当前界面所显示的主要内容 Widget
* floatingActionButton: Material设计中定义的一个功能按钮
* persistentFooterButtons: 固定在下方显示的按钮，如对话框下方的确定、取消按钮
* drawer: 抽屉菜单控件
* backgroundColor: 背景色，默认是 ThemeData.scaffoldBackgroundColor 的值
* bottomNavigationBar: 显示在页面底部的导航栏
* resizeToAvoidBottomPadding: 默认true,控制body是否重新布局来避免底部被覆盖了，如当键盘显示的时候，重新布局避免被键盘盖住内容

> ### package导入要求

* 导包最好按如下顺序排序(可以空行分隔)

1. dart sdk内的库
2. flutter内的库
3. 第三方库
4. 自己的库
5. 相对路径引用

> ### 一些语法特性

* => expr 等同于{ return expr; } 如果无返回值省略return

	```
	// forEach
	list4.forEach((itemValue) => print(itemValue));
	list4.forEach((itemValue) {
	  print(itemValue);
	});
	```

* .. 级联符号 在同一对象上进行一系列操作

	```
	class StarInfo {
	  String name;
	  printInfo() {
	    print("${name}");
	  }
	}
	
	// 常规
	var starInfo1 = StarInfo();
	starInfo1.name = "Li Xiaolong";
	starInfo1.printInfo();
	// 级联
	var starInfo2 = StarInfo();
	starInfo2
	  ..name = "Li XiaoLong"
	  ..printInfo(); // 结尾才有分号
	```
* Dart 的类不支持public、private这样限制词。_下划线代表私有。

	```
	class TestGetSet {
	  // 私有
	  String _userName; 
	  String _nickName;
	
	  String get userName {
	    return _userName ?? _nickName;
	  }
	  set userName(String newName) {
	    _userName = newName;
	  }
	  set nickName(String newName) {
	    _nickName = newName;
	  }
	}
	
	// 使用
	var testGetSet = new TestGetSet();
	testGetSet.nickName = "LiXiaolong";
	print("${testGetSet.userName}"); // LiXiaolong
	```
* 运算符(操作符)

1. 算数运算符(加、减、乘、除、商、余、自加、自减)

    ```
    print(20 + 2);  // 22   加
    print(20 - 2);  // 18   减
    print(20 * 2);  // 40   乘
    print(20 / 2);  // 10   除
    print(40 ~/ 3); // 13   商
    print(40 % 3);  // 1    余
    
    // i++
    int i = 3;
    var a = i++; // 赋值后i才自加,故a=3
    print('a=$a,i=$i'); // a=3,i=4
    
    // ++i
    int i = 3;
    var a = ++i; // 执行赋值前i已经自加,故a=4
    print('a=$a,i=$i'); // a=4,i=4
    
    // i--
    int i = 3;
    var a = i--; // 执行赋值后i才自减,故a=3
    print('a=$a,i=$i'); // a=3,i=2
    
    --i
    int i = 3;
    var a = --i; // 执行赋值前i已经自减,故a=2
    print('a=$a,i=$i'); // a=2,i=2
    ```
    
2. 关系运算符(大于、小于、等于、不等于、大于等于、小于等于)

    ```
    print(10 > 20);     // false    大于
    print(10 < 20);     // true     小于
    print(10 == 20);    // false    等于
    print(10 != 20);    // true     不等
    print(10 >= 3);     // true  大于等于
    print(10 <= 3);     // false 小于等于
    ```

3. 位运算符(位与、位或、位非、异或、左移、右移)

    ```
    // 位与:& 两个都是1为1 
    // 位或:| 只要有1就是1
    // 位非:~ 全取反
    // 异或:^ 位都不一样为1
    // 左移:<< 整体左移，不足补0             
    // 右移:>> 整体右移
    ```

4. 逻辑运算符(逻辑与、逻辑或、逻辑非)

    ```
    bool isGreen = true;
    bool isBlack = false;
    print("${isGreen && isBlack}"); // false 逻辑与
    print("${isGreen || isBlack}"); // true  逻辑或
    print("${!isBlack}");           // true  逻辑非
    ```
5. 赋值运算符

    ```
    =
    ??      // AA ?? "99" 表示如果 AA 为空，返回99
    ??=     // AA ??= "99"如果 AA 为空，给 AA 设置成 99
    +=
    -=
    *=
    /=
    ~/=
    %=
    <<=
    =>>
    &=
    |=
    ^=
    ```
6. 条件表达式

    ```
    expr1 ? expr2 : expr3 // 三目运算符 如果 if(expr1) expr2 else expr3
    expr1 ?? expr2 // expr1 ? expr1 : expr2
    ```   
    
7. 类型测试操作符
 
 	as : 类型转换    
 	is : 当对象是相应类型时返回 true   
 	is! : 当对象不是相应类型时返回 true   

 
> ### JIT 和 AOT

* JIT(Just In Time):即时编译，在程序运行中将热点代码编译成机器码，以提升开发效率。JIT可以充分利用解释型语言的优点，动态执行源码，而不用考虑平台差异性，JIT模式需要配合DartVM使用。
* AOT(Ahead Of Time):运行前编译，在程序运行之前，已经编译成对应平台的机器码，不需要在运行中解释编译就可以直接运行，AOT需要使用runtime来执行。
* 四种编译模式
1. Script:最常见的JIT模式，可以直接在虚拟机中执行Dart源码，像解释型语言一样使用。通过 ``` Dart xxx.dart ```直接运行，适合写一些临时脚本。
2. Kernel Snapshots:也是JIT模式，又称为Script Snapshots，和Script不同的是，这种模式执行的是Kernel AST的二进制数据，这里不包含解析后的类和函数以及编译后的代码，所以不能在不同平台之间移植。 Dart Kernel是Dart程序中一种中间语言，可通过 ``` dart --snapshot-kind=kernel --snapshot=xxx.snapshot xxx.dart``` 生成
3. JIT Application Snapshots:也是JIT模式，执行的是解析过的类和函数以及编译后的代码，所以运行更快，但只能针对32位或64位架构运行，可通过 ``` dart --snapshot-kind=app-jit --snapshot=xxx.snapshot xxx.dart ``` 生成
4. AOT Application Snapshots:AOT模式，源码被提前编译成特定平台的二进制文件。要用AOT模式，需要```dart2aot```命令，如``` dart2aot xxx.dart xxx.dart.aot ```然后用 ``` dartaotruntime ```命令执行
 
> 断言 assert

* assert(条件) 如果条件不满足，会中断程序的执行，否则继续执行 
 
> ### 参考文章
 
* [Dart学习-操作符](https://www.jianshu.com/p/64a6ed7581aa)  
* [Flutter 原理简解](https://juejin.im/entry/5afa9769518825428630a61c)   
* [浅谈Flutter构建](https://juejin.im/post/5d68fb1af265da03d063b69e)


[上一页 Flutter1 环境搭建](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter001.md)    
[下一页 Flutter3 基本类型](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter003.md)