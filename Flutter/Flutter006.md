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

	* Dart是单线程模型，默认单线程处理任务。
	* Dart中有个介于进程和线程之间的概念被称为Worker-Isolate(实际更偏向进程概念)，但一般理解成带有独立内存的线程，防止歧义，统一称作Isolate、隔离。
	* Dart中Isolate不等同于线程，Isolate有线程和独立内存构成，又比进程低级
	* Dart中的异步机制涉及到的关键字有Future、async、await、async、sync、Iterator、Iterable、Stream、Timer等
	* Dart异步调用中async和await要配套使用,且await可以多个。
	

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

	* Dart中可通过async和await进行异步操作
	* async开启异步操作，返回一个Future结果(没有返回值则返回null的Future，耗时操作要执行一段时间但Future却立即返回)。
	* await表达式通常返回Future,若不是Future则Dart把该值放到Future中返回，await会阻塞当前执行直到对象返回
	* await可以多次使用(只能在async包装内)

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

* Future
	
	* Future 简单了说就是对 Zone 的封装使用，如Future.microtask主要执行了Zone的 scheduleMicrotask，而result._complete最后调用的是 _zone.runUnary等
	* 定义在 ``` dart:async ```中，基于观察者模式。
	* 支持范型
	* Future 需要 import 'dart:io';（'dart:async';）
	* Future中常用方法then(),whenComplete(),wait(),catchError()
	* 一般来说，如果需要监听“完毕”这个状态，那么用whenComplete,需要监听“成功”这个状态，用then，需要监听“失败”这个状态，用catchError
	

	```
	class _TestFutureFunction {
	
	  // then()回调中带参数，此参数为Future对象中包含的值
	  testThen() {
	    getNumber1().then((aNum) {
	      print('a = $aNum');
	      return getNumber2(aNum);
	    }).then((_) {
	      print('_ = $_');
	      getResult(_);
	    });
	  }
	
	  // whenComplete()中不带参数,只指定了顺序，此方法抛出异常后使用相当于finally
	  testWhenComplete() {
	    getNumber1().whenComplete((){
	      getNumber2(9).whenComplete((){
	        getResult(7).then((_){
	          print('TestWhenComplete');
	        });
	      });
	    });
	  }
	
	  // 使用async await
	  testAwait() async {
	    var number1 = await getNumber1();
	    var number2 = await getNumber2(number1);
	    print(await getResult(number2));
	  }
	  
	  // wait()方法, wait()方法内的异步方法，要有await表达式
	  testWait() {
	    Future.wait([getNumber1(), getNumber2(2), getResult(4)]).then((List result){
	      result.forEach((num) => print(num));
	    });
	  }
	
	  Future<int> getNumber1() async {
	    await print('getNumber1 10086');
	    return 10086;
	  }
	
	  Future getNumber2(int num) async {
	    await print('getNumber2 ${num + 2}');
	    return num + 2;
	  }
	
	  Future getResult(int num) async {
	    await print('getRusult');
	  }
	}
	```
	
	下面是一个then()和whenComplete()结合使用的例子
	
	```
	var client = new http.Client();
	client.post(
	    "http://example.com/fruit",
	    body: {"name": "apple", "color": "red"})
	  .then((response) => client.get(response.bodyFields['uri']))
	  .then((response) => print(response.body))
	  .whenComplete(client.close);
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
* export重新导入库
    * 当导入的库过多或要重新整合库，可以通过export重新导入，把部分库或全部库来组合或重新打包库
    * export也有show和hide，但没有as库前缀
    * 使用export重新导入多个库，同名成员(类，方法，变量)会冲突
    * export重新导入的库相当于把库内代码复制到当前文件，但在当前文件并不能使用，因为Dart不存在重载机制，所以会出现冲突
    * 部分冲突可以用hide来将冲突部分隐藏来解决
* part和part of关联文件和库

3. 库的拆分

* 我们可以将较大的库文件拆成几个比较小的文件，通过part关联
* part关联的文件，共享所有的命名空间，共享所有对象(包括私有对象)
* 如果使用part拆分，则必须用library声明
* part引入的文件可以使用url但不推荐，如part "https://xxx/xx.dart";
* part优先级高于import，所以当前库可以直接用part中的内容

4. 延迟加载

* 使用deferred as 可以实现库的延迟加载。
* 使用loadLibrary()进行加载可以多次调用但只执行一次，返回Future对象
* 延迟加载能1.减少App启动时间，2.延迟加载那些较少使用的功能

5. 依赖第三方库

* 在pubspec.yaml文件内dependencies:标签下添加依赖如```cupertino_icons: ^0.1.2```
* 添加依赖后可以用```flutter packages get```命令或工具栏packages get 按钮 下载包
* pubspec.lock文件可查看导入依赖的兼容版本
* ^version 表示version<=libVersion < big version
* 常规依赖：dependencies:此标签下依赖在调试和发版后都会生效
* Dev依赖：dev_dependencies:此标签下的依赖均在调试时生效
* 重写依赖：dependency_overrides:强制下载依赖包，不管是否兼容，不推荐使用

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
	var today = DateTime.now();
	print('当前时间是：$today');
	
	var date1 = today.millisecondsSinceEpoch;
	print('当前时间戳：$date1');
	
	var date2 = DateTime.fromMillisecondsSinceEpoch(date1);
	print('时间戳转日期：$date2');
	
	// 拼接成date
	var dentistAppointment = new DateTime(2019, 6, 20, 17, 30,20);
	print(dentistAppointment);
	
	// 字符串转date
	DateTime date3 = DateTime.parse("2019-06-20 15:32:41");
	print(date3);
	// DateTime转formatString 
	// formatDate(DateTime ,[yyyy,'-',mm,'-',dd]);
	
	// 时间比较
	print(today.isBefore(date3));// 在之前
	print(today.isAfter(date3)); // 在之后
	print(date3.isAtSameMomentAs(date3));// 相同
	
	print(date3.compareTo(today));// 大于返回1；等于返回0；小于返回-1。
	// print(DateTime.now().toString());
	// print(DateTime.now().toIso8601String());
	
	// 时间增加
	var fiftyDaysFromNow = today.add(new Duration(days: 5));
	print('today加5天：$fiftyDaysFromNow');
	
	// 时间减少
	DateTime fiftyDaysAgo = today.subtract(new Duration(days: 5));
	print('today减5天：$fiftyDaysAgo');
	
	// 时间差 两个时间相差 小时数
	print('比较两个时间 差 小时数：${fiftyDaysFromNow.difference(fiftyDaysAgo)}');
	
	print('本地时区简码：${today.timeZoneName}');
	
	print('返回UTC与本地时差 小时数：${today.timeZoneOffset}');
	
	print('获取年月日：${today.year}');//month、day、hour、minute、second、millisecond、microsecond
	
	print('星期：${today.weekday}');// 返回星期几
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

> ### 搜集一些三方库

* 网络请求[http](https://pub.dartlang.org/packages/http)

	包含一组高级函数和类，可轻松使用HTTP资源。与平台无关，可在命令行和浏览器上用

* 网络请求[dio](https://pub.dartlang.org/packages/dio)

	一个强大的Http客户端，支持拦截器、全局配置、FormData、请求取消、文件下载、超时等

* 图片选择[photo](https://pub.dev/packages/photo)

	用于选择图像，支持多选，而且这个是用Flutter做的UI，可以很方便的自定义修改

* 图片加载[image](https://pub.dartlang.org/packages/image)

	提供以各种不同的文件格式加载、保存和操作图像的能力。该库不依赖于dart:io
	
* svg图片[flutter_svg](https://pub.dartlang.org/packages/flutter_svg)	
	加载svg图像

* 图片查看[zoomable_image](https://pub.dartlang.org/packages/zoomable_image)

	提供图像查看和手势缩放操作功能
	
* 日历[flutter_calendar](https://pub.dartlang.org/packages/flutter_calendar)	


[参考](https://www.jianshu.com/p/06d195d14ce1)



[上一页 Flutter5 类、范型和流程控制](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter005.md)      
[下一页 Flutter7 网络和数据解析](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter007.md)