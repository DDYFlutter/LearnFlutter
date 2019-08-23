# 环境搭建 

> ### 下载

1. 下载[Flutter](https://flutter.dev/docs/get-started/install/macos)
2. 创建flutter目录 ``` mkdir ~/flutter ```
3. 进入flutter目录 ``` open ~/flutter ```
4. 将步骤1下载的压缩包复制到创建的flutter目录并解压
5. 打开环境变量描述文件 ``` open -e ~/.bash_profile ```
6. 追加配置并保存
	
	```
	# Flutter
	# 临时镜像，如果变更请自己更新
	export PUB_HOSTED_URL=https://pub.flutter-io.cn
	export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
	# 以实际bin目录为准
	export PATH=~/flutter/flutter/bin:$PATH
	# 可添加Android相关配置
	# export ANDROID_HOME="xxx/android_sdk 目录" 
  	# export PATH=${PATH}:${ANDROID_HOME}/tools
  	# export PATH=${PATH}:${ANDROID_HOME}/platform-tools
	```
7. 使配置生效 ``` source ~/.bash_profile ```
8. 查看是否成功 ``` flutter -h ```
9. 安装依赖 ``` flutter doctor ```

注意:    
1. 如果使用的是其他如 zsh，终端启动时 ~/.bash_profile 将不会被加载，解决办法就是修改 ~/.zshrc ，在其中添加：source ~/.bash_profile
2. 选择高版本Command Line Tool ： 打开Xcode，按快捷键 'command' + '<'，进入设置面板，找到locations，选择高版本Command Line Tools






[Flutter中文网](https://flutterchina.club/flutter-for-ios/)   
[Flutter开发环境配置](https://segmentfault.com/a/1190000016878485)    
[搭建Flutter-iOS开发环境](https://www.cnblogs.com/lovestarfish/p/10628205.html)    