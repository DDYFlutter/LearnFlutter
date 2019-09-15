本文对应github地址[Flutter19](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter019.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


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


6. 定制路由


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



> ### 参考

* [flutter入门之使用PopupRoute自定义实现PopupWindow功能](https://blog.csdn.net/email_jade/article/details/87922051)