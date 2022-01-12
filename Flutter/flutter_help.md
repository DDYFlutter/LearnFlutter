#### Flutter常用命令

> ##### 查看帮助 

```
flutter -h/–help/ help 常看帮助信息
-h/--help可以作为别的命令的后缀使用 打印详细的命令使用指南 如 flutter run -h 
```

> ##### 创建项目

```
flutter create projectName 创建flutter项目
```

> ##### 运行项目

```
flutter run
运行项目   
--v 查看APP所有日志的输出
--release/--debug/--profile/--test 分别是以release/debug/profile/test模式运行
--hot 热重载方式启动 方便调试 
	1) r 热重载(重载，程序中发生改变的state无法重置) 
	2) R (shift+r)热重启(重新启动APP，程序中的state等全部重置)  
		R和r的区别演示：默认程序计数器，点击数字变大，使用r热重载，数字不会重置
				      使用R热重启 则数字重置
	3) h 查看更多帮助
	4) d 结束终端，但不结束程序
	5) c 清空屏幕，但其实只是跳到底部 让你看不到上面的内容而已
	6) o 切换安卓和IOS的显示模式
	7) p 切换模式 Widget会有线条作基准 方便布局
	8) q 退出程序以及终端
	

flutter run -d 设备名称/设备ID
根据设备名称或者设备ID切换设备
```

> ##### 查看设备列表

```
flutte devices
输出当前连接的设备列表
 设备名称  •  设备ID   •  安卓系统架构  •  系统版本  (API版本)
vivo X21A • e685da69 • android-arm64 • Android 9 (API 28)
```

> ##### 将程序安装到连接中的设备

```
flutter install
```

> ##### 查看当前配置粗略情况

```
flutter doctor
包含 flutter安装情况、当前环境配置情况、不同的编译器的情况、当前可用设备情况
使用 -v 后缀 查看详细情况
```

> ##### 升级Flutter

```
flutter upgrade
```

> ##### 打包项目

```
flutter build apk/ios
生成安装文件
```

> ##### 查看模拟器列表

```
flutter emulators
```

> ##### 获取或升级依赖包

```
flutter packages get/upgrade
```

> ##### 使用分析器检查代码是否存在问题

```
flutter analyze
默认情况只检查一次 –-watch 一直运行检查   
默认情况下会每次分析都会执行pub get –-no-pub
不需要检查的文件可以在analysis_options.yaml中排除:
analyzer:
	exclude:
   		- flutter/**
```

> ##### 配置信息操作

```
flutter config
如修改安卓SDK路径:flutter config --android-sdk “新的SDK路径” 去掉路径就是查看
```

> ##### 清除缓存

```
flutter clean
删除`build/`和`.dart_tool/`目录,清除缓存信息,避免之前不同版本代码的影响
```

> ##### 版本信息

```
flutter version 查看版本，加上版本名称则切换
```

> ##### 代码格式化（项目所有代码）

```
flutter format 
```

> ##### attach

```
flutter attach 和 flutter run 类似，但是很多执行需要手动，如热重载
```

> ##### 分支信息

```
flutter channel 输出分支信息，默认stable
```

> ##### 切换分支

```
flutter channel 分支名 切花到指定分支
```

> ##### 截屏

```
flutter screenshot 截屏 默认是在home目录下 -o 指定目录
```

> ##### 单元测试

```
flutter test –start-paused 运行flutter单元测试 并会等待调试器的连接
```

> ##### 

```

```

> ##### 

```

```

> ##### 

```

```

