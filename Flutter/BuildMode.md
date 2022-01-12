#### 四种运行模式

Flutter在编译App时有4种模式：Debug、Release、Profile、test，且编译阶段完全独立。但在移动端开发中只支持前三种，test只适用于桌面环境

> ##### Debug

Dubug模式特点

* App可被安装到真机、模拟器、仿真器上进行调试
* 开启断言
* 开启服务扩展(源码runApp->WidgetsFlutterBinding->initServiceExtension)
* 开启debuger aids 类似DevTools的工具可以连接到应用程序的进程中(如 Dart Observatory用来分析和调试，可以debugger，还可以检测性能等)
* 开启debugging信息
* 采用JIT编译，支持热重载HotRload
* 优化了快速develop/run循环，但没有优化执行速度、二进制大小和部署
* 可能存在性能上低效甚至卡顿
* 通过sky/tools/gn --android 或 sky/tools/gn --ios 来build
* 有时候也被叫做 checked 模式 或 slow模式
* 构建方式：flutter run 或 IDE中Debug->Start Debugging/Start Without Debugging


> ##### Release

Release模式特点

* 只能真机上运行，不能再模拟器、仿真器上调试
* 采用AOT编译，不能热重载(HotRload)
* 会关闭所有断言和debugging信息，及关闭所有debugger工具
* 禁用所有debugging aids 和服务扩展
* 优化了快速启动、快速执行和减小包体积
* 该模式目的是为了部署给最终用户使用
* 通过sky/tools/gn --android/--ios --runtime-mode=release来build
* 构建方式：
	1. 命令flutter run --release 就是运行的该模式
	2. IDE点击设备选择后面的main.dart按钮(默认文件)Edit Configurations 然后在Additional arguments加上 --release
	3. 还可以用上面步骤增加主文件 如 main_debug.dart main_release.dart然后选择对应文件执行
	4. 还可以Run->Flutter Run 'main.dart' in Release Mode
	5. 使用flutter build apk/ios 构建相应release包

> ##### Profile

Profile模式是专门监控性能的。在Debug模式下，开启了很多额外功能，不能反映实际性能，但在Release模式下却没有监控功能，所以就有了Profile模式。

* 基本和Release一致，但启用了服务扩展和tracing，以及一些为了最低限度支持tracing运行的东西（如可以连接observatory到进程）
* Profile模式采用AOT编译，没有热重载(HotRload)
* web平台：资源文件没有被压缩，但整体性能已经优化
* flutter run --profile就是以这种模式运行的
* 通过sky/tools/gn --android/--ios --runtime-mode=profile来build
* 因为模拟器不能代表真实场景，所以不能在模拟器上运行


> ##### test

test模式特点

* test模式只能在桌面环境运行，基本和Debug模式一致
* flutter test 就是以该模式运行的


> ##### 判断是Debug还是Release 

实际开发中，上面四种模式又分为两种：未优化的模式和优化过的模式。默认时未优化模式，如果要开启优化模式，build的时候命令行后面添加 --unoptimized 参数

我们可以用 dart.vm.product环境标识得知运行环境是否是Production
```
const bool inProduction = bool.fromEnviroment(dart.vm.product')
```

当运行在Release环境时，inProduction为true，否则(Debug/Profile)为false

* Release：const bool.fromEnvironment("dart.vm.product") = true；
* Debug：assert(() { ...; return true; });断言语句会被执行；
* Profile：上面的两种情况均不会发生。


> ##### 参考文章
[github flutter mode](https://github.com/flutter/flutter/wiki/Flutter's-modes)
[flutter 1.2 官方开发文档](https://www.bookstack.cn/read/flutter-1.2-zh/32f04f19acb7665f.md)
[flutter.dev build-modes](https://flutter.dev/docs/testing/build-modes)