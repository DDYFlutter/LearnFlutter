本文对应github地址[Flutter8](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter008.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)

> ### Everything is widget

* 屏幕上元素由Widget层实现，Widget分为两种风格：Materia风格和Cupertino风格
* Flutter中存在Widget、Element、RenderObject、Layer四棵树，其中Widget和Element是一对多关系
* Element中持有Widget和RenderObject,而Element和RenderObject一一对应
* 不是每个RenderObject都有Layer，只有isRepaintBoundary为true才能形成Layer
* Flutter中Widget不可变，每次保持一帧，如果发生改变要通过State实现跨帧状态保存，而真实完成布局和绘制的是RenderObject，Element充当两者桥梁，State就是保存在Element中
* Flutter中BuildContext只是接口，而Element实现了该接口
* Flutter中setState其实是调用markNeedBuild，该方法内部标记此Element为Dirt，然后在下一帧WidgetBinding.drawFrame才会被绘制，且setState并不是立即生效
* Flutter中RenderObject在attch/layout之后会通过markNeedPaint()使页面重绘
* Flutter中一般json数据从String转Object过程都先经过Map类型
* Flutter中InheritedWidget一般用于状态共享，如Theme、Localizations、MediaQuery等，都是通过它实现共享状态，可通过context去获取共享的状态，如ThemeData theme = Theme.of(context);
* statelessWidget是无状态组件，statefulWidget是有状态组件，inheritedWidget可以向子Widget树传递状态和信息
* Flutter中大部分组件继承于statelessWidget、statefulWidget或inheritedWidget
* MeterialApp是Flutter中Material风格组件入口
* Scaffold是快速开发的脚手架

```
-->1 RenderObject markNeedsPaint()   -->2 PipelineOwner requestVisualUpdate()  
-->3 RendererBinding scheduleFrame() -->4 ui.Window scheduleFrame()          
-->5 ui.Window onDrawFrame()         -->6 SchedulerBinding handleDrawFrame()   
-->7 RendererBinding drawFrame()     -->8 RenderObject paint                  
```  

> ### MeterialApp


1. navigatorKey 

* 导航键 
* 类型为 GlobalKey<NavigatorState>

	```
	// navigatiorKey.currentState 相当于 Navigator.of(context)
	// 使用
	GlobalKey<NavigatorState> _navigatorKey = new GlobalKey();
	
	// MaterialApp
	navigatorKey: _navigatorKey,
	```	
	
2. home 

* 主页 
* 类型为 Widget
* 进入程序显示的第一个Widget,这个Widget需要包装一个Scaffold以显示Material Design风格

	```
	// MeterialApp
	home: DDYBottomBar(),
	
	
	class DDYBottomBar extends StatefulWidget {
	  @override
	  State<StatefulWidget> createState() => new _tabbarState();
	}
	
	class _tabbarState extends State<DDYBottomBar> {
	  // 此处省略万行代码
	  @override
	  Widget build(BuildContext context) {
	    return Scaffold(body: _buildBody(), bottomNavigationBar: _buildBottomBar(),);
	  }
	}
	```

3. routes 

* 路由 
* 类型为 const Map<String, WidgetBuilder>
* 页面跳转规则，当使用Navigator.pushNamed来路由的时候，会在routes查找路由名字，然后使用对应WidgetBuidler来构建一个带页面切换动画的MaterialPageRouter
* 如果home不为null且routes中包含Navigator.defaultRouteName('/')时会冲突
* 如果所查找的routes中不存在，则会通过onGenerateRoute来查找
* key:路由名字 value:对应Widget

4. initialRoute 

* 初始路由 
* 类型为 String
* 用户进入程序自动打开对应路由(home位于一级)，传入的是上面routes的key
* 跳转对应Widget(如果Widget有Scaffold.AppBar，不做修改，左上角返回键)

	```
	// MeterialApp
	
	routes: {
	  '/home':(BuildContext context) => HomePage(),
	  '/home/one':(BuildContext context) => OnePage(),
	  // 此处省略万行代码
	}
	
	initialRoute: '/home/one',
	```

5. onGenerateRoute 

* 生成路由 
* 类型为 RouteFactory
* 当通过Navigator.of(context).pushNamed跳转路由，在routes找不到时调用
	
	```
	new MaterialApp(
	  routes: {
	    '/home':(BuildContext context) => HomePage(),
	    '/home/one':(BuildContext context) => OnePage(),
	    // 此处省略万行代码
	  },
	  onGenerateRoute: (setting) {
	    // setting.isInitialRoute; bool类型 是否初始路由
	    // setting.name; 要跳转的路由名key
	    return new PageRouteBuilder(
	      pageBuilder: (BuildContext context, _, __) {
	        // 这里为返回的Widget
	        return HomePage();
	      },
	      opaque: false,
	      // 跳转动画
	      transitionDuration: new Duration(milliseconds: 200),
	      transitionsBuilder: (___, Animation<double> animation, ____, Widget child) {
	        return new FadeTransition(
	          opacity: animation,
	          child: new ScaleTransition(
	            scale: new Tween<double>(begin: 0.5, end: 1.0).animate(animation),
	            child: child,
	          ),
	       );
	    });
	  }
	);
	```

6. onUnknownRoute 

* 未知路由 
* 类型为 RouteFactory
* 效果和onGenerateRoute一样，调用顺序onGenerateRoute略高

7. navigatorObservers 
* 导航观察器(路由观察器)，调用Navigator相关方法会回调相关操作 
* 类型为 List<NavigatorObserver>

	```
	new MaterialApp(
	      navigatorObservers: [
	        MyObserver(),
	      ],
	    );
	// 继承NavigatorObserver
	class MyObserver extends NavigatorObserver{
	  @override
	  void didPush(Route route, Route previousRoute) {
	    // 当调用Navigator.push时回调
	    super.didPush(route, previousRoute);
	    // 可通过route.settings获取路由相关内容
	    // route.currentResult获取返回内容
	    // 此处省略万行代码
	    print(route.settings.name);
	  }
	}
	```

8. builder 

* 建造者，当构建一个Widget前调用
* 类型为 TransitionBuilder
* 一般做字体大小，方向，主题色等配置

	```
	new MaterialApp(
	  builder: (BuildContext context, Widget child) {
	    return MediaQuery(
	      data: MediaQuery.of(context).copyWith(
	        // 字体大小
	        textScaleFactor: 1.4,
	      ),
	      child: child,
	    );
	  },
	);
	```

* title 标题 String



* onGenerateTitle 生成标题 GenerateAppTitle



* color 颜色 Color



* theme 主题 ThemeData



* darkTheme 暗黑主题 ThemeData



* locale 地点 Locale
* localizationsDelegates 本地化代理 Iterable<LocalizationsDelegate<dynamic>>
* localeListResolutionCallback 
* localeResolutionCallback,
* supportedLocales = const <Locale>[Locale('en', 'US')],
* debugShowMaterialGrid = false,
* showPerformanceOverlay = false,
* checkerboardRasterCacheImages = false,
* checkerboardOffscreenLayers = false,
* showSemanticsDebugger = false,
* debugShowCheckedModeBanner = true,





[Flutter7](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter007.md)
[Flutter9](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter009.md)