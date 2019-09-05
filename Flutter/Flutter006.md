本文对应github地址[Flutter6](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter006.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)

> ### 异常

* throw 抛出异常用
* Exception 异常(内置异常和自定义异常)
* try {} 包装可能抛出异常的代码，后面必须有on/catch/finally中一个或多个
* on exceptionOfKnown{} 捕获指定异常
* catch(e){} 捕获异常(如果前面有指定的则排除指定的后的异常)
* finally{} 是否异常都执行 

	```
	testCatchException() {
	  // try...on 方式捕获某些类型异常(知道异常类型)
	  try {
	    testThrowException(0);
	  } on IntegerDivisionByZeroException {
	    print('捕获除0异常');
	  }
	  // 不确定甚至不知道异常类型 catch
	  try {
	    testThrowException(-1);
	  } on IntegerDivisionByZeroException {
	    print('捕获除0异常');
	  } catch (e) {
	    print(e);
	  } finally {
	    print('是否异常都执行finally');
	  }
	}
	
	testThrowException(int exceptionIndex) {
	  if (exceptionIndex == 0) {
	    throw new IntegerDivisionByZeroException(); // 抛出内置异常
	  } else if (exceptionIndex < 0) {
	    throw new Exception('Index need more then 0'); // 抛一个自定义异常
	  } else {
	    print(exceptionIndex);
	  }
	}
	```
	
* 常见内置异常类型

	* DeferredLoadException 延迟库无法加载时抛出。
	* FormatException 数据没有预期格式且无法解析或处理
	* IntegerDivisionByZeroException 当数字除以零时抛出
	* IOException 所有与输入输出相关的异常的基类
	* IsolateSpawnException  无法创建隔离时抛出
	* Timeout 在等待异步结果时发生计划超时时抛出

* zone

	* 一般try{}catch{}能捕获到异常，但有时候无效
	* Dart语言中有个zone概念，类似沙盒(sandbox),不同zone代码互不影响，zone还能创建子zone。zone可以重新定义自己的print,timer,microtasks等，还可以解决未捕获的异常

```
testZone() {
  // 未捕获的异常
  try {
    Future.error('asynchronous error');
  } catch(e) {
    print(e);
  }

  // 将上面代码注释后用下面代码捕获异常	
  runZoned((){
    Future.error('asynchronous error');
  }, onError: (Object obj, StackTrace stack){
    // 这里可以调用日志上报函数
    print(obj); // asynchronous error
    print(stack); // null
  });
}
```

	* 可以给runZoned注册方法，在需要时回调

	```
	handleData(result) {
	  print('handleData $result'); // handleData 2
	}
	var onData = Zone.current.registerUnaryCallback<dynamic, int>(handleData);
	Zone.current.runUnary(onData, 2);
	
	// 异步逻辑
	Zone.current.scheduleMicrotask((){
	  print('Todo something'); // Todo something
	});
	```


> ### 异步

* 简介

	* Flutter 默认单线程处理任务，如果不开启新线程，那么默认在主线程处理。
	* Dart中有个介于进程和线程之间的概念被称为Isolate，且更偏向进程概念。
	* Dart中Isolate不等同于线程，Isolate有线程和独立内存构成，又比进程低级
	* Dart异步调用中三个关键字 async、await、Future。async和await要配套使用。
	

* Isolate

	* 定义在 ``` import 'dart:isolate'; ```
	* 每个Isolate包含一个事件循环(event loop)和两个事件队列(event queue和microtask queue)    
	* event queue负责I/O、绘制、手势等事件或接收其他Isolate消息的外部事件。      
	* microtask queue则可以自己向Isolate内部添加事件，事件优先级高于event queue。   
	* Isolate开始执行后优先microtask queue，处理完才处理event queue，且处理microtask queue时阻塞event queue，所以尽量把耗时操作放event queue。   
	* Isolate拥有独立内存，线程之间不共享，所以不存在抢夺资源，即不需要锁。   
	* Isolate之间通过port来通信，且port消息传递过程异步。   
	* Isolate实例化过程其实是实例化Isolate+堆内分配内存+配置port等过程。
	* Isolate对象允许其他Isolate监听、控制它代表的事件循环，如当这个Isolate发生未捕获错误时，可以暂停(pause)此Isolate或获取错误信息(addErrorListener)
	* controlPort识别并授予Isolate控制权限，pauseCapability和terminateCapability会对某些控制操作进行权限保护。如暂停一个无pauseCapability的Isolate对象是不生效的。
	* 由spawn操作创建的Isolate对象具有控制接口和控制该对象的能力。当然，Isolate构造方法创建的Isolate对象可以不必带这些能力。
	* Isolate对象不能用SendPort发送给另一个Isolate对象，但是控制接口和能力capability是可以发送的，并且可以在另一个Isolate对象中用发送的接口和capability创建一个新Isolate对象。
	* SendPort和ReceivePort是Isolate隔离对象唯一通信方式
	* 调用需要顶级函数(类似main这样不被包裹的函数)或静态函数

	```
	// 可以在顶级函数main()中调用 startInsolate(); 
	Isolate isolate;
	int i = 0;
	Capability capability = new Capability();
	
	runTimer(SendPort port) {
	  int counter = 0;
	  Timer.periodic(const Duration(seconds: 1), (_){
	    counter++;
	    i++;
	    var msg = 'Notification $counter';
	    print('Send message:$msg i:$i');
	    port.send(msg);
	  });
	}
	
	startIsolate() async {
	  final receivePort = ReceivePort();
	
	  isolate = await Isolate.spawn(runTimer, receivePort.sendPort);
	  receivePort.listen((data){
	    print('Receive message:$data i:$i');
	  });
	}
	
	resumeIsolate() {
	  isolate.resume(capability);
	}
	
	pauseIsolate() {
	  isolate.pause(capability);
	}
	
	stopInsolate() {
	  isolate.kill(priority: Isolate.immediate);
	  isolate = null;
	}
	```
	
	* Compute函数对isolate的创建和底层的消息传递进行了封装，使得我们不必关系底层的实现，只需要关注功能实现
	
	```
	void main() async{
	  //调用compute函数，compute函数的参数就是想要在isolate里运行的函数，和这个函数需要的参数
	  print( await compute(syncFibonacci, 20)); // 6765
	  runApp(MyApp());
	}
	// 计算斐波那契数列
	int syncFibonacci(int n){
	  return n < 2 ? n : syncFibonacci(n-2) + syncFibonacci(n-1);
	}
	```	
	
* async await	

	* Dart中可通过async和await进行异步操作，async开启异步操作，返回一个Future结果(没有返回值则返回null的Future)。

* Future
	
	* Future 简单了说就是对 Zone 的封装使用，如Future.microtask主要执行了Zone的 scheduleMicrotask，而result._complete最后调用的是 _zone.runUnary等
	* 定义在 ``` dart:async ```中，基于观察者模式。
	* 支持范型
	* Future 需要 import 'dart:io';（'dart:async';）
	
	```
	getChatMessage1(String userName) {
	  print('111: ${queryDatebase(userName)}');
	}
	
	getChatMessage2(String userName) async {
	  var msg1 = await queryDatebase(userName);
	  print('222: $msg1');
	}
	
	// 范型
	Future<String> queryDatebase(String userName) {
	  return Future.delayed(Duration(seconds: 3), () => '20190808 $userName');
	}
	
	// 调用
	getChatMessage1('LiLei'); // 111: Instance of 'Future<String>'
	getChatMessage2('LiBai'); // 3秒后打印  333: 20190808 LiBai
	```

* Stream

	* Stream中也涉及到Zone
	* Dart另一种异步操作 ```async*/yield``` 或 Stream异步(```async*/yield```是语法糖，最终被编译器转为Stream)
	* Stream也支持同步操作
	* Stream中有Stream、StreamController、StreamSink、StreamSubscription四个关键对象
	* Stream:事件源本身，可用于监听事件或转换事件，如listen、where
	* StreamController:用于整个Stream过程的控制，提供各类接口用于创建各种事件流
	* StreamSink:一般作为事件入口，提供如add、addStream等
	* StreamSubscription:事件订阅后对象，管理订阅等各类操作，如cancel、pause，同时在内部也是事件中专的关键
	* 一般通过StreamController创建Steam,通过SteamSink添加事件，通过Stream监听事件，通过StreamSubscription管理订阅
	* Stream中支持各种变化，比如map、expand、where、take等操作，同时支持转换为Future


* 订阅 

	* Stream单播只能有一个订阅者

	```
	Duration interval = Duration(seconds: 1);
	Stream<int> stream1 = Stream.periodic(interval, (data) => data);
	stream1.listen((_){
	print("订阅者01");
	});
	// Stream has already been listened to
	//    stream.listen((_){
	//      print("订阅者2");
	//    });
	```	
	
	* Stream广播可以多个订阅者

	```
	var stream2 = Stream.fromIterable([1,2,3,4]);
	var broadcastStream = stream2.asBroadcastStream();
	broadcastStream.listen((i){
		print("订阅者11：${i}");
	});
	broadcastStream.listen((i){
		print("订阅者12：${i}");
	});
	```

* delayed 延时函数
 
	```
	Future.delayed(Duration(seconds: 3), () => print('1234'));
	``` 
 
* Timer 定时器

	```
	Timer _countdownTimer;
	  String _codeCountdownStr = '获取验证码';
	  int _countdownNum = 59;
	
	  void reGetCountdown() {
	    setState(() {
	      if (_countdownTimer != null) {
	          return;
	      }
	      // Timer的第一秒倒计时是有一点延迟的，为了立刻显示效果可以添加下一行。
	      _codeCountdownStr = '${_countdownNum--}重新获取';
	      _countdownTimer =
	          new Timer.periodic(new Duration(seconds: 1), (timer) {
	        setState(() {
	          if (_countdownNum > 0) {
	            _codeCountdownStr = '${_countdownNum--}重新获取';
	          } else {
	            _codeCountdownStr = '获取验证码';
	            _countdownNum = 59;
	            _countdownTimer.cancel();
	            _countdownTimer = null;
	          }
	        });
	      });
	    });
	  }
	
	 // 不要忘记在这里释放掉Timer
	 @override
	  void dispose() {
	    _countdownTimer?.cancel();
	    _countdownTimer = null;
	    super.dispose();
	  }
	```

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


[Flutter5 类、范型和流程控制](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter005.md)   
[下一页 Flutter7 网络和数据解析](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter007.md)