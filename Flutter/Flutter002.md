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

* 这是一个纯 C++实现的 SDK，其中囊括了 Skia引擎、Dart运行时、Text文字排版引擎等.
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

1. abstract(抽象类修饰符关键字)

* abstract修饰符定义抽象类(抽象类就是无法实例化的类)
* 抽象类可以自定义一些接口，抽象类通常有抽象方法(只声明无实现的方法)

```
// 该声明为抽象类，不可实例化
abstract class AbstractTestClass {
	// 定义构造函数、变量、方法等
	
	// 其他
	
	// 抽象方法[只声明不实现]
	void updateChildren();
}

// 实现抽象方法的例子
class TestImpClass extends AbstractTestClass {
	// 实现继承抽象类的抽象方法
	void updateChildren() {
		// TODO: 实现具体逻辑
	}
}
```

2. dynamic (动态类型关键字)

* 和Object类似，不指定具体类型，可类型推导具体类型
* 使用dynamic可处理更复杂的不确定类型，如超出了Dart的类型系统，或者值来自互操作或者在静态类型系统范围之外的情况

```
// void judge(Object arg) {
void judge(dynamic arg) {
	if (arg is bool){
      print('arg is bool');
    } else if (arg is String){
      print('arg is String');
    }
}
```

3. implements

* implements 不继承而实现接口
* implements 可以同时实现多个类的接口

```
// Person类，包含方法greet().
class Person {
  //在该类中，属于私有，仅对当前类可见
  final _name;

  // 不是接口，只是构造函数
  Person(this._name);

  // 接口
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// 实现Person类接口的类
class Impostor implements Person {
  //只是一个普通的get方法，可忽略
  get _name => '';

  //实现Person的greet方法
  String greet(String who) => 'Hi $who. Do you know who I am?';
}

//只是一个测试方法
String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy')));  //打印 -> Hello, Bob. I am Kathy.
  print(greetBob(Impostor())); //打印 -> Hi Bob. Do you know who I am?
}

// 实现多个类的接口
// class Test implements A, B, C { ... } 
```

4. extends(继承关键字)

* Flutter中的继承是单继承(如果要实现类似多继承则 使用 mixin + with)
* 构造函数不能继承
* 子类重写超类的方法，要用@override
* 子类调用超类的方法，要用super


5. Scaffold(实现了基本的 Material Design 布局结构)

* appBar: 界面顶部导航条
* body: 当前界面所显示的主要内容 Widget
* floatingActionButton: Material设计中定义的一个功能按钮
* persistentFooterButtons: 固定在下方显示的按钮，如对话框下方的确定、取消按钮
* drawer: 抽屉菜单控件
* backgroundColor: 背景色，默认是 ThemeData.scaffoldBackgroundColor 的值
* bottomNavigationBar: 显示在页面底部的导航栏
* resizeToAvoidBottomPadding: 默认true,控制body是否重新布局来避免底部被覆盖了，如当键盘显示的时候，重新布局避免被键盘盖住内容

6. show & hide (引入库部分选择关键字)

* show 导入指定部分
* hide 导入除了给定后剩余部分

```
// 只导入foo
import 'package:lib1/lib1.dart' show foo;
//导入整个库除了foo
import 'package:lib2/lib2.dart' hide foo;
```

7. as / is / is! (类型相关关键字)

* as: 类型转换 或 指定库前缀
* is: 是类型判断
* !is: 不是某种类型

```
if (emp is Person) {
  	// 类型检查
	(emp as Person).firstName = 'Bob';
}

// 如果导入两个具有冲突标识符(class)的库，则可以为一个或两个库指定前缀。 例如，如果library1和library2都有一个Element类，那么as可以这样使用

import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2; //指定库的前缀为lib2

// Uses Element from lib1.
Element element1 = Element();
// Uses Element from lib2.
lib2.Element element2 = lib2.Element();
```
8. if & else (条件语句关键字)
9. import (导入关键字)

```
import 'dart:xxx'; 引入Dart标准库
import 'xxx/xxx.dart';引入绝对路径的Dart文件
import 'package:xxx/xxx.dart'; 引入Pub仓库pub.dev(或者pub.flutter-io.cn)中的第三方库
import 'package:project/xxx/xxx.dart';引入自定义的dart文件
import 'xxx' show compute1，compute2 只导入compute1，compute2
import 'xxx' hide compute3 除了compute都引入
import 'xxx' as compute4 将库重命名，当有名字冲突时
library compute5; 定义库名称
part of compute6; 表示文件属于某个库
文件导入顺序（从上到下依次）

dart sdk 内的库
flutter内的库
第三方库
自己的库（文件）
相对路径引用

import 'dart:io';
import 'package:material/material.dart';
import 'package:dio/dio.dart';
import 'package:project/common/uitls.dart';
import 'xxx/xxx/xxx/xxx.dart';
```

10. static (静态关键字)

* 可修饰属性变量或方法，表示类范围的变量或方法(通过类名调用)

11. assert (断言关键字)

* 不满足条件则中断执行

```
assert(urlString.startsWith('https'),
    'URL ($urlString) should start with "https".');
```

12. enum (枚举关键字)

* 枚举中的每个值都有一个索引getter，它返回枚举声明中值的从零开始的位置
* 获取所有枚举值 ``` List<Color> colors = Color.values; ```
* switch 时没有列举出的用default表示，每个条件用break;终止
* 枚举不能显式实例化，不能子类化(extends)，不能混合(Mixin with)和实现(implements)

```
enum Color { red, green, blue }

print('red index: \${Color.red.index}'); // -> 打印red index: 0
  print('green index: \${Color.green.index}'); // -> 打印: green index: 1
  print('blue index: \${Color.blue.index}');· //-> 打印: blue index: 2
```

13. for...in (遍历关键字)

```
var list = [0, 1, 2];
// 标准下标循环
for (int i = 0; i < list.length; i++){
	print(list[i]); // 0 1 2
}
// for...in 循环
for (var x in list) {
	print(x); // 0 1 2
}
```

14. extends & super

* extends 继承父类
* super 本类实例调用父类方法

15. async & await (异步与等待)

16. factory (工厂关键字)

* 用来修饰构造函数，描述该构造函数作为一个工厂构造函数功能，在实现不用总是创建新实例的构造函数的时候，可以使用factory关键字
* 使用factory修饰的构造函数不能使用this

```
class Logger {
  final String name;
  bool mute = false;

  //一个维护Logger类的map
  static final Map<String, Logger> _cache =
      <String, Logger>{};

  //根据不同的name获取对应的Logger，
  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  //一个内部构造函数
  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
```

17. Mixin with & Mixin on (混入与限定)

* 使用Mixin功能需要 with关键字，后跟一个或多个类名
* 跟随的可被混入的类不能有构造函数和命名函数

```
//程序员喜欢写代码
class Programmer{
  code(){
    print('I am a programmer, i like coding.');
  }
}

//歌唱家喜欢唱歌
class Singer{
  singing(){
    print('I am a singer, i like singing.');
  }
}

//既爱编码，也爱唱歌
class Mixin with Programmer, Singer{

}

void main() {
  Mixin mixin = Mixin();
  mixin.code();
  mixin.singing();
}

//打印：
I am a programmer, i like coding.
I am a musician, i like singing.

// 这里类Programmer和Singer不能声明构造函数包括命名函数
```

* 当调用的不同类有相同接口时，混入的后者类的接口生效

```
class A {
  name(){
    print('I am a student.');
  }
}

class B{
  name(){
    print('I am a teacher.');
  }
}

class AB with A, B{

}

class BA with B, A{

}

void main() {
  new AB().name();

  new BA().name();
}

//打印：
I am a teacher.
I am a student.
```

* 如果限定什么类能被混入，使用 mixin + on 方式

```
//mixn定义了类Flutter要求只有实现Programmer类的才能被混入
mixin Flutter on Programmer{
  flu(){
    print('This is in flutter.');
  }
}

//会如下面图片报错
//class A  with Flutter{
//}

//可以混入
class B extends Programmer with Flutter{
}

new B ().flu();  //打印This is in flutter.
```

18. deferred

* 用于声明一个延迟加载一个库，通常叫懒加载。允许你只有在需要使用该库的时候，再加载该库。 

```
//文件calculate.dart
class Calculate{

  static String name = "Cal";

  static printf(String s){
    print('cal: $s');
  }

  int add(int a, int b) => a + b;
}

//文件test.dart
import 'calculate.dart' deferred as cal; //声明该库会延迟加载且命名为cal

void main() {
  print('1');
  greet();
  print('2');
  print('3');
}

//异步加载库calculate.dart
//加载完毕后再进行操作
Future greet() async {
  await cal.loadLibrary();
  print('cal name: ${cal.Calculate.name}');
  print('add(1, 2) = ${new cal.Calculate().add(1, 2)}');
  cal.Calculate.printf('ss');
}

//打印:
1
2
3
cal name: Cal
add(1, 2) = 3
cal: ss
```

19. set & get (setter & getter 关键字)

* set和get关键字实现 setter和getter，提供对象属性的访问权限

```
class Rectangle {
  num left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  //定义两个可计算的属性right 和bottom.
  num get right => left + width;
  set right(num value) => left = value - width;

  num get bottom => top + height;
  set bottom(num value) => top = value - height;
}

void main() {
  var rect = Rectangle(3, 4, 20, 15);
  
  print('right: ${rect.right}');
  rect.right = 100;
  print('right: ${rect.right}');

  print('bottom: ${rect.bottom}');
  rect.bottom = 120;
  print('bottom: ${rect.bottom}');
}

//打印：
right: 23
right: 100
bottom: 19
bottom: 120
```

20. covariant (协变关键字)

重构某个方法时，如果将父类型强制缩窄为子类型，会提示错误

```
//定义一个Animal类，其函数chase参数为Animal
class Animal {
  void chase(Animal x) {}
}

class Mouse extends Animal {
  getName(){
    return 'mouse';
  }
}

class Cat extends Animal {
  //强制将chase函数的Animal参数缩窄成Mouse 

  void chase(Mouse mouse) {
    print('cat chase ${mouse.getName()}');
  }
}

//报错, 提示重写类型不匹配
test/test.dart:12:20: Error: The parameter 'mouse' of the method 'Cat::chase' has type #lib1::Mouse, which does not match the corresponding type in the overridden method (#lib1::Animal).
Change to a supertype of #lib1::Animal (or, for a covariant parameter, a subtype).
  void chase(Mouse mouse) {
                   ^
test/test.dart:2:8: Context: This is the overridden method ('chase').
  void chase(Animal x) {}
       ^
```

使用协变关键字covariant告诉编译器这个参数缩窄变换是故意的，不用抛出异常
```
class Animal {
  void chase(Animal x) {}
}

class Mouse extends Animal {
  getName(){
    return 'mouse';
  }
}

class Cat extends Animal {
  void chase(covariant Mouse mouse) {
    print('cat chase ${mouse.getName()}');
  }
}

void main(){
  new Cat().chase(new Mouse());
}
//打印
cat chase mouse
```

21. Function (函数类型关键字)

* Dart是面向对象函数式语言，函数也是对象且为Function类型，可作为变量或参数

```
//定义一个add函数
int add(int a, int b) => a+b;

handle(Function function){
  print('handle: ${function(1, 2)}'); //打印function的结果
}

void main(){
  handle(add); //调用handle并传递add函数
}
//打印
handle: 3
```

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

##### Flutter两种编译模式

* JIT(Just In Time):即时编译，在程序运行中将热点代码编译成机器码，以提升开发效率。JIT可以充分利用解释型语言的优点，动态执行源码，而不用考虑平台差异性，JIT模式需要配合DartVM使用。
* AOT(Ahead Of Time):运行前编译，在程序运行之前，已经编译成对应平台的机器码，不需要在运行中解释编译就可以直接运行，AOT需要使用runtime来执行。

##### Dart 四种编译模式
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
* [Flutter学习之Dart语言基础(关键字)](https://www.jianshu.com/p/524e481ef3f6)
* [Dart | 什么是Mixin](https://www.jianshu.com/p/a578bd2c42aa)
* [[译]Flutter 的编译模式](https://zhuanlan.zhihu.com/p/61903658)
* https://dart.dev/guides/language/language-tour#adding-features-to-a-class-mixins


[上一页 Flutter1 环境搭建](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter001.md)    
[下一页 Flutter3 基本类型](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter003.md)