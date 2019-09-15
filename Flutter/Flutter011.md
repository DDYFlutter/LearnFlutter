本文对应github地址[Flutter11](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter011.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> ### 路由

1. 简介

    * iOS中页面跳转通过NavigationController,然后pushViewController，Android中可以初始化Intent然后startActivity，Flutter中则路由(Route)方式
    * 程序启动后路由栈第一个(最底部)实例 MaterialApp(home: Screen1())
    * 命名路由,对页面起别名(唯一字符串),但一般用有意义字符串，如 '/'表示根页面，'/login'表示登录页等

2. 静态路由push

    * 静态路由在MeterialApp初始化时配置routes参数，通过起的别名(命名路由)进行跳转
    * 新页面为配置的固定页面，且跳转时参数也是固定的
    * 调用方法为pushNamed()
    
    ```
    // MaterialApp初始化时配置Key-Value
    void main() => runApp(MyApp());
    
    class MyApp extends StatelessWidget {
    
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: 'Flutter Demo',
          theme: ThemeData(
            primaryColor: Colors.red,
            primarySwatch: Colors.blue,
          ),
          home: DDYBottomBar(),
          showPerformanceOverlay: false,
          routes: {
            '/home/qrcode': (BuildContext context) => QRCodeScanner(title: '0',),
            '/home_qrcode': (BuildContext context) => QRCodeScanner(title: '1',),
            '+home_qrcode': (BuildContext context) => QRCodeScanner(title: '2',),
          },
        );
      }
    }
    
    // 在需要的地方调用['/home/qrcode'只是字符串名字，并不是真正路径，这样仿路径是为了可读性]
    // Navigator.pushNamed(context, '/home/qrcode');
    // Navigator.pushNamed(context, '/home_qrcode');
    // Navigator.pushNamed(context, '+home_qrcode');
    Navigator.of(context).pushNamed('+home_qrcode');
    ```

3. 动态路由push

    * 动态路由在需要跳转时生成要跳转的页面对象，传递想要的参数，然后调用push()函数
    
    ```
    // Navigator.push(context, MaterialPageRoute(builder: (context) => QRCodeScanner()));
    // Navigator.of(context).push(MaterialPageRoute(builder: (_) => QRCodeScanner()));
    Navigator.of(context).push(MaterialPageRoute(builder: (context) => QRCodeScanner()));
    ```

4. 路由pop

    * pop()方法中可添加回调数据
    
    ```
    // Navigator.of(context).pop();
    // Navigator.pop(context);
    Navigator.of(context).pop('回调数据');
    ```

5. 页面传值

* 正向传值直接通过简单构造函数参数方式即可
* 反向传值，不同风格其实都一样，建议第一种
    
    ```
    // 风格1
    pushQRCodeScannerAndCallbackData1() {
      Navigator.of(context).push(MaterialPageRoute(builder: (context) => QRCodeScanner())).then((value){
        print('1 $value');
      });
    }
    // 风格2
    pushQRCodeScannerAndCallbackData2() {
      Future callbackFuture = Navigator.of(context).push(MaterialPageRoute(builder: (_) => QRCodeScanner()));
      callbackFuture.then((value){
        print('2 $value');
      });
    }
    // 风格3
    pushQRCodeScannerAndCallbackData3() async {
      // 如果知道类型
      String callbackString = await Navigator.of(context).push<String>(
          MaterialPageRoute(builder: (context){
            // 正向传值直接通过简单构造函数参数方式即可
            return QRCodeScanner(title:'I am 3');
          })
      );
      if (callbackString != null) {
        print('3 ${callbackString}');
      }
    }
    
    
    // pop()函数参数值即为反向传回的数据
    Navigator.of(context).pop('回调数据');
    ```
* 还可以通过闭包作为参数进行传值
    
    ```
    pushQRCodeGenerator() {
      Navigator.of(context).push(
        MaterialPageRoute(
          builder: (context) => QRCodeGenerator((String value) {
            print('QRCodeGenerator callback $value');
          }),
        ),
      );
    }
    
    
    // 调用
    IconButton(
      icon: Icon(Icons.print), 
        onPressed: () {
          if (widget.callbackFunction != null) {
            widget.callbackFunction('回调数据');
          }
        },
    ),
    ```


6. 定制路由（自定义路由，自定义转场）


* 继承路由子类，如：PopupRoute、ModalRoute 等
* 使用 PageRouteBuilder 类通过回调函数定义路由

    * 从下向上弹出，从上向下收回
    
    ```
    pushQRCodeScannerWithCustomAnimation() {
      Navigator.push(
        context,
        PageRouteBuilder(pageBuilder: (BuildContext context, Animation<double> animation, Animation<double> secondaryAnimation) {
          return QRCodeScanner();
        }, transitionsBuilder: (context, animation, secondaryAnimation, child) {
          return transitionAnimation(animation, child);
        }),
      );
    }
    
    static SlideTransition transitionAnimation(Animation<double> animation, Widget child) {
      return SlideTransition(
        position: Tween<Offset>(
          begin: const Offset(0.0, 1.0),
          end: const Offset(0.0, 0.0),
        ).animate(animation),
        child: child,
      );
    }
    ```
    
    * 页面旋转淡出的效果
    
    ```
    pushQRCodeScannerWithCustomAnimation2() async {
      // 页面旋转淡出的效果
      transitionAnimation(Animation<double> animation, Widget child) {
        return FadeTransition(
          opacity: animation,
          child: RotationTransition(
            turns: Tween<double>(begin: 0.7, end:1.0).animate(animation),
            child: child,
          ),
        );
      }
    
      Navigator.push(
        context,
        PageRouteBuilder(
          pageBuilder: (context, _, __) => QRCodeScanner(),
          transitionDuration: const Duration(milliseconds: 1000),
          transitionsBuilder: (_, animation, __, child) => transitionAnimation(animation, child),
        ),
      ).then((value){
        Scaffold.of(context).showSnackBar(
          SnackBar(
            content: Text(value),
            duration: const Duration(seconds: 3),
          ),
        );
      });
    }
    ```

7. 嵌套路由

* 一个应用程序可以使用多个路由导航器
* 可将一个导航器嵌套在另一个导航器下方使用，例如选项卡式导航，用户注册，订单与结账页等
    
    ```
    class MyApp extends StatelessWidget {
      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          initialRoute: '/',
          routes: {
            '/': (BuildContext context) => HomePage(),
            '/signup': (BuildContext context) => SignUpPage(),
          },
        );
      }
    }
    
    class SignUpPage extends StatelessWidget {
     @override
     Widget build(BuildContext context) {
       return Navigator(
         initialRoute: 'signup/personal_info',
         onGenerateRoute: (RouteSettings settings) {
           WidgetBuilder builder;
           switch (settings.name) {
             case 'signup/personal_info':
               // 个人信息完毕后到 'signup/choose_credentials'.
               builder = (BuildContext _) => CollectPersonalInfoPage();
               break;
             case 'signup/choose_credentials':
               // 选完credentials 调用 'onSignupComplete()'.
               builder = (BuildContext _) => ChooseCredentialsPage(
                 onSignupComplete: () {
                   // 回到路由根页面 '/' （HomePage）
                   Navigator.of(context).pop();
                 },
               );
               break;
             default:
               throw Exception('Invalid route: ${settings.name}');
           }
           return MaterialPageRoute(builder: builder, settings: settings);
         },
       );
     }
    }
    ```

> ### Navigator

1. 继承自StatefulWidget，路由堆栈的管理者
2. 静态方法详解

* pushNamed
* pushReplacementNamed
* popAndPushNamed
* pushNamedAndRemoveUntil
* push
* pushReplacement
* pushAndRemoveUntil
* replace
* replaceRouteBelow
* canPop
* maybePop
* popUntil
* removeRoute
* removeRouteBelow

##### push和pushName

pushName结合配置routes,利用命名路由形式将指定页面实例(带固定参数)入栈，而push则是在需要跳转时生成要跳转的页面对象，传递想要的参数，两者运行效果没差别。

##### pushReplacement和pushReplacementNamed

两者都是取代栈顶元素，运行效果没差别。

##### popAndPushNamed

顾名思义，先pop然后push

##### pushAndRemoveUntil 和 pushNamedAndRemoveUntil

两者都是入栈新页面删除除该页面实例外其他栈内实例，将该实例作为新栈底，运行效果没差别。可用在注册登录页登录后跳转，防止能返回注册登录页

##### canPop

判断是否可以pop，如果可以返回true,否则返回false

##### maybePop

表示在该页面尝试pop，如果可以则pop到前一页，否则仍停在该页面，丢弃该次pop

##### popUntil

pop回栈内(该页面实例以下)指定页面实例，如 ``` Navigator.popUntil(context, ModalRoute.withName('/home')); ```,回到配置名为home的页面。


> ### 其他知识点

1. 去除返回按钮 AppBar中

    ```
    automaticalImplyLeading: false
    ```

2. Popup routes (弹出路由)

路由不一定要遮挡整个屏幕。 PopupRoutes 使用 ModalRoute.barrierColor 覆盖屏幕，ModalRoute.barrierColor 只能部分不透明以允许当前屏幕显示。 弹出路由是“模态”的，因为它们阻止了对下面其他组件的输入。

有一些方法可以创建和显示这类弹出路由。 例如：showDialog，showMenu 和 showModalBottomSheet。 如上所述，这些函数返回其推送路由的 Future（异步数据，参考下面的数据部分）。 执行可以等待返回的值在弹出路由时执行操作。

还有一些组件可以创建弹出路由，如 PopupMenuButton 和 DropdownButton。 这些组件创建 PopupRoute 的内部子类，并使用 Navigator 的push 和 pop 方法来显示和关闭它们。


> ### 参考

* [flutter入门之使用PopupRoute自定义实现PopupWindow功能](https://blog.csdn.net/email_jade/article/details/87922051)



[上一页 Flutter10 Icon、Color、Tabbar、BottomNavigationBar](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter010.md)   
[下一页 Flutter12 IndexedStack、Table、Flow、Warp](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter012.md) 