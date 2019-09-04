> ### 包

* 包是一种封装编程单元机制
* Dart的包管理器是Pub(Cocoapods for iOS, Gradle for Java)
* Pub有助于在[存储库](https://pub.dartlang.org)中安装包。
* 包元数据在pubsec.yaml中定义(Yet Another Markup Language)
* pub get(获取应用程序所依赖的所有包)
* pub upgrade(将所有依赖项升级到较新版本)
* pub build(用于构建Web应用程序，创建一个包含所有相关脚本的构建文件夹)
* pub help(将提供所有pub命令的帮助。)

> ### URI

* 编码解码与组装拆分

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

> ### 异常与调试

> ### 异步

* Future

	* 定义在 ``` dart:async ```中，基于观察者模式。
	* 支持范型

```

```
	
 
 
 

> ### 库




https://www.yiibai.com/dart/dart_programming_packages.html

[咸鱼技术](https://www.jianshu.com/u/cf5c0e4b1111)
[Flutter开源app](https://itsallwidgets.com/)
[Flutter开发者]( http://flutter.link/)
[Flutter布局控件](https://juejin.im/post/5bab35ff5188255c3272c228)