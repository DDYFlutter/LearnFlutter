> ### 异常与调试

> ### 异步

* 简介

	Flutter 默认单线程处理任务，如果不开启新线程，那么默认在主线程处理。
	Dart中线程概念被称为isolate，那么主线程就可以称为main isolate。
	Dart异步调用中三个关键字 async、await、Future。async和await要配套使用。
	

* isolate

	每个isolate包含一个事件循环(event loop)和两个事件队列(event queue和microtask queue)    
	event queue负责I/O、绘制、手势等事件或接收其他isolate消息的外部事件。   
	microtask queue则可以自己向isolate内部添加事件，事件优先级高于event queue。
	isolate开始执行后优先microtask queue，处理完才处理event queue，且处理microtask queue时阻塞event queue，所以尽量把耗时操作放event queue。


* Future

	* 定义在 ``` dart:async ```中，基于观察者模式。
	* 支持范型

```

```
	
* async await	

	Dart中可通过async和await进行异步操作，async开启异步操作，返回一个Future结果(没有返回值则返回null的Future)。

 

> ### 包与库

1. 包
 
	* 包是一种封装编程单元机制
	* Dart的包管理器是Pub(Cocoapods for iOS, Gradle for Java)
	* Pub有助于在[存储库](https://pub.dartlang.org)中安装包。
	* 包元数据在pubsec.yaml中定义(Yet Another Markup Language)
	* pub get(获取应用程序所依赖的所有包)
	* pub upgrade(将所有依赖项升级到较新版本)
	* pub build(用于构建Web应用程序，创建一个包含所有相关脚本的构建文件夹)
	* pub help(将提供所有pub命令的帮助。)

2. 库

* Dart2中可以用library声明库名(也可以不用)
* 不同类型库的引入方式
	* Dart语言内部库，import 'dart:html'
	* Flutter库文件，import 'path/name.dart'
	* 三方库，import 'package:path/name.dart'
	* URI文件，import 'https://xxx/name.dart'
* 引入的库可以用as起别名，防止库变量名或函数名冲突(调用时用newName.methodName)
* 引入时用show/hide关键字可以单独引入或剔除库中某Api

> ### 知识点

* 获取一些尺寸

	```
	// 屏幕宽高
	var ScreenSize = MediaQuery.of(context).size;
	// 状态栏高度
	var StatusHeight = MediaQuery.of(context).padding.top;
	```

* 判断设备类型

	```
	import 'dart:io';
	
	if ( Platform.isIOS) { } else if (Platform.isAndroid) { } 
	// Platform.isMacOS 
	// Platform.isWindows
	// Platform.isLinux
	// Platform.isFuchsia
	```
* 时间	
	
	```
	testDate() {
	  var date1 = DateTime.now(); // 当前时间
	  var date2 = DateTime.now().millisecondsSinceEpoch; // 时间戳 1970年1月1日UTC以来的毫秒数
	  var date3 = DateTime(2019,3,31,10,59); //组装时间
	  var date4 = DateTime.utc(2022); //UTC
	  var date5 = DateTime.fromMillisecondsSinceEpoch(946684800000, isUtc: true); // 转化时间戳为UTC时间(2000-01-01)
	  var date6 = date5.add(Duration(days: 366)); // 加366天(闰年)
	
	  print(date1); // 2019-09-04 14:19:01.654525
	  print(date2); // 1567577941654
	  print(date3); // 2019-03-31 10:59:00.000
	  print(date4); // 2022-01-01 00:00:00.000Z
	  print(date5); // 2000-01-01 00:00:00.000Z
	  print(date6); // 2001-01-01 00:00:00.000Z
	}
	```
* 剪贴板 ClipBoard

	```
	testClipBoard() {
	  // 赋值
	  ClipboardData willSetData = new ClipboardData(text: "剪贴板的文字");
	  Clipboard.setData(willSetData);
	
	  // 取值
	  getClipboardData();
	}
	getClipboardData() async {
	  var clipboardData = await Clipboard.getData(Clipboard.kTextPlain);
	  if (clipboardData != null) {
	    // 一些操作，如setState()中给控件赋值
	    print(clipboardData.text); // 剪贴板的文字
	  }
	}
	```

* URI 编码解码与组装拆分

	```
	// <scheme>://<user>:<password>@<host>:<port>/<path>;<params>?<query>#<frag>
	// https://<host>:<端口>/<路径>?<查询>#<片段>
	var uri1 = 'https://github.com/starainDou?tab=repositories';
	var encodeFullURI = Uri.encodeFull(uri1);
	var encodeComponentURI = Uri.encodeComponent(uri1);
	
	var uri2 = Uri(scheme: 'https', host: 'google.com', path: '/News/today', fragment: 'frag');
	var uri3 = Uri.parse('htps://google.com/News/tody#frag');
	
	print('${Uri.decodeFull(encodeFullURI)}'); //  https://github.com/starainDou?tab=repositories
	print('${Uri.decodeComponent(encodeComponentURI)}'); //  https://github.com/starainDou?tab=repositories
	print('${uri2}'); //  https://google.com/News/today#frag
	print('${uri3}'); //  htps://google.com/News/tody#frag
	```


https://www.yiibai.com/dart/dart_programming_packages.html

