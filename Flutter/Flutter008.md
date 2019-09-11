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

9. title 

* 标题 (Android任务管理器程序快照上，iOS导航控制器上)
* 类型为 String

	```
	// MeterialApp
	title: 'FirstTitle',
	```

10. onGenerateTitle 

* 生成标题 (和title一样，但有context参数用于本地化)
* 类型为 GenerateAppTitle

	```
	// MeterialApp
	onGenerateTitle: (context) { return 'SecondTitle';},
	```


11. color 

* 颜色 
* 类型为 Color

	```
	// MeterialApp
	color: Colors.blue
	```

12. theme 

* 主题 (应用程序的主题管理)
* 类型为 ThemeData
	
	```
	// MeterialApp
	theme: ThemeData( primarySwatch: Colors.red ),
	```


13. darkTheme 

* 暗黑主题 
* 类型为 ThemeData


14. locale 

* 地点 (当前区域,如果为null则使用系统区域，可用于语言切换)
* 类型为 Locale

	```
	locale: Locale('zh', 'CH');
	```


15. localizationsDelegates 
* 本地化代理 (用于更改Flutter Widget默认的提示语，按钮text等)
* Iterable<LocalizationsDelegate<dynamic>>


16. localeListResolutionCallback 
17. localeResolutionCallback (当传入不支持的语种，根据这个回调，返回相近且支持的语种)
18. supportedLocales = const <Locale>[Locale('en', 'US')], (传入支持的语种数组)
19. debugShowMaterialGrid = false, (debug模式是否显示MaterialGrid，使用就不写了)
20. showPerformanceOverlay = false,(显示GPU和UI曲线图查看当前流畅度情况)
21. checkerboardRasterCacheImages = false,(true打开光栅缓存图像的棋盘格)
22. checkerboardOffscreenLayers = false,(true打开呈现到屏幕位图的层的棋盘格)
23. showSemanticsDebugger = false,(true打开Widget边框)
24. debugShowCheckedModeBanner = true,(true在debug模式下显示右上角的debug字样)


> ### Scaffold


1. appBar

* 顶部栏，类似Android中ActionBar,ToolBar iOS中NavigationBar

2. body

* 当前界面所显示的内容Widget

3. floatingActionButton

* 主要功能浮动按钮(有人叫他 悬浮球、FAB)，默认56dp,mini为40dp

4. floatingActionButtonLocation

* 

5. floatingActionButtonAnimator

* 


6. persistentFooterButtons

* 固定在下方显示的按钮，比如对话框下方确定、取消按钮


7. drawer

* 侧边(左边)抽屉型控件


8. endDrawer

* 右侧抽屉型控件


9. bottomNavigationBar

* 底部导航栏，类似iOS中UITabbar


10. bottomSheet

* 


11. backgroundColor

* 内容背景色，默认ThemeData.scaffoBackgroundColor值


12. resizeToAvoidBottomPadding

* 控制界面内容是否重新布局来避免底部被覆盖了，默认值为 true
* 比如当键盘显示的时候，重新布局避免被键盘盖住内容。
* 类似android.windowSoftInputMode = "adjustResize"。


13. resizeToAvoidBottomInset

* 


14. primary = true

* 


15. drawerDragStartBehavior = DragStartBehavior.start

* 


16. .extendBody = false

* 


17. drawerScrimColor


> ### AppBar

1. leading

* Widget,标题左边显示的一个控件(二级界面一般为返回按钮)

2. automaticallyImplyLeading = true


3. title

* Widget,通常为当前页面标题

4. actions

* List<Widget>,Widget列表，代表所显示的菜单，通常用IconButton表示
* 也可用PopupMenuButton方式弹出二级菜单

5. flexibleSpace

* Widget,可理解成标题底部和TabBar导航菜单顶部之间一种控件
* 通常用在SliverAppBar时实现特殊效果

6. bottom

* PreferredSizeWidget,通常是TabBar导航菜单

7. elevation

* double,控件z坐标顺序，默认4
* 对于可滚动SliverAppBar,当SliverAppBar和内容同级，该值为0
* 当内容滚动，SliverAppBar变到ToolBar,修改elevation值

8. shape


9. backgroundColor

* Color,AppBar颜色，默认为ThemeData.primaryColor值
* 通常和brightness、iconTheme、textTheme一起使用

10. brightness

* Brightness,AppBar亮度，有白色和黑色两种主题
* 默认ThemeData.primaryColorBrightness

11. iconTheme

* IconThemeData,AppBar上图标颜色，透明度，尺寸等信息
* 默认ThemeData.primaryIconTheme

12. actionsIconTheme


13. textTheme

* TextTheme,AppBar上文字样式

14. primary = true


15. centerTitle

* bool,标题是否居中显示，可以根据不同系统显示不同方式

16. titleSpacing = NavigationToolbar.kMiddleSpacing


17. toolbarOpacity = 1.0


18. bottomOpacity = 1.0,

	```
	// 返回每个隐藏的菜单项
	SelectView(IconData icon, String text, String id) {
	    return new PopupMenuItem<String>(
	        value: id,
	        child: new Row(
	            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
	            children: <Widget>[
	                new Icon(icon, color: Colors.blue),
	                new Text(text),
	            ],
	        )
	    );
	}
	
	appBar: new AppBar(
	    title: new Text('首页'),
	    leading: new Icon(Icons.home),
	    backgroundColor: Colors.blue,
	    centerTitle: true,
	    actions: <Widget>[
	        // 非隐藏的菜单
	        new IconButton(
	            icon: new Icon(Icons.add_alarm),
	            tooltip: 'Add Alarm',
	            onPressed: () {}
	        ),
	        // 隐藏的菜单
	        new PopupMenuButton<String>(
	            itemBuilder: (BuildContext context) => <PopupMenuItem<String>>[
	                this.SelectView(Icons.message, '发起群聊', 'A'),
	                this.SelectView(Icons.group_add, '添加服务', 'B'),
	                this.SelectView(Icons.cast_connected, '扫一扫码', 'C'),
	            ],
	            onSelected: (String action) {
	                // 点击选项的时候
	                switch (action) {
	                    case 'A': break;
	                    case 'B': break;
	                    case 'C': break;
	                }
	            },
	        ),
	    ],
	),
	```

> ### 参考

* [Material Design设计规范](https://www.uisdc.com/material-design-knowledge)
* [Flutter AppBar 顶端栏](https://www.jianshu.com/p/77f8b7ee8460)


[Flutter7](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter007.md)
[Flutter9](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter009.md)