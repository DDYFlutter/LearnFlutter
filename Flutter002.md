本文对应github地址[Flutter1](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter1.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> 为什么Flutter选择了Dart语言
 
1. Dart 的性能更好。JIT模式速度与JavaScript持平，AOT模式性能好于JavaScript。
2. 缩短开发周期。Debug时默认JIT，且支持热重载。
3. Native Binding。JavaScript在Android上Native Binding可以很好地实现，但iOS上的 JavaScriptCore不可以，Dart的 Native Binding可以很好地通过 Dart Lib实现。
4. Fuchsia OS。Fuchsia OS内置的应用浏览器就是使用 Dart语言作为 App的开发语言。而且实际上，Flutter是 Fuchisa OS的应用框架概念上的一个子集。（Flutter源代码和编译工具链也充斥了大量的 Fuchsia宏）
5. Dart是类型安全的语言，拥有完善的包管理和诸多特性。Google召集了如此多个编程语言界的设计专家开发出这样一门语言，旨在取代 JavaScript，所以 Fuchsia OS内置了 Dart。Dart可以作为 embedded lib嵌入应用，而不用只能随着系统升级才能获得更新，这也是优势之一。 
6. 
 
 
> Skia是什么

* Skia是一个 2D的绘图引擎库，其前身是一个向量绘图软件，Chrome和 Android均采用 Skia作为绘图引擎。
* Skia提供了非常友好的 API，并且在图形转换、文字渲染、位图渲染方面都提供了友好、高效的表现。
* Skia是跨平台的，所以可以被嵌入到 Flutter的 iOS SDK中，而不用去研究 iOS闭源的 Core Graphics / Core Animation。
* Android自带了 Skia，所以 Flutter Android SDK要比 iOS SDK小很多。


> Flutter Framework

* 这是一个纯 Dart实现的 SDK，类似于 React在 JavaScript中的作用，它实现了一套基础库， 用于处理动画、绘图和手势。
* 其绘图封装了一套 UI库(dart:ui)，有 Material 和Cupertino两种视觉风格。我们在使用 Flutter写 App的时候，直接导入这个库即可使用组件等功能


> Flutter Engine

* 这是一个纯 C++实现的 SDK，其中囊括了 Skia引擎、Dart运行时、文字排版引擎等.
* 它可以以 JIT、JIT Snapshot 或者 AOT的模式运行 Dart代码。
* 在代码调用 dart:ui库时，提供 dart:ui库中 Native Binding 实现。 
* 他还控制着 VSync信号的传递、GPU数据的填充等，并且还负责把客户端的事件传递到运行时中的代码

> Flutter命名规范

* 文件名: 小写+下划线
* 类型名(类名、函数类型名):大写开头驼峰
* 变量名(包含const final 常量):使用小写开头驼峰
* const可以使用大写+下划线的方式
* 方法名:使用小写开头驼峰
* 导包as后的名称为小写+下划线
* 不要用匈牙利命名法中的kXXXX 这样的命名方式,应该去掉k
* 超过两位的英文缩写一律按该单词为普通小写单词处理,使用小写



















 
 
 
 [Flutter 原理简解](https://juejin.im/entry/5afa9769518825428630a61c)