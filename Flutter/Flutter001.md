本文对应github地址[Flutter1](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter001.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)

# 环境搭建 

> ### 准备

* 安装Xcode

1. 下载最新版[Xcode](https://developer.apple.com/download/more)
2. 安装并同意协议后打开(这里默认名字Xcode.app，下同)
3. 打开Xcode，按快捷键 'command' + '<'，进入设置面板，找到locations，选择高版本Command Line Tools
4. 执行 ``` sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer ```
5. 输入 ``` sudo xcode-select --print-path ``` 查看Xcode路径
6. 在Xcode.xip所在目录执行 ``` xattr -d com.apple.quarantine Xcode_11.xip ``` 解除验证
	
* 安装AndroidStudio 

1. 下载最新版[AndroidStudio](http://www.maczapp.com/15045849668668),  国内直访[网站](http://www.android-studio.org) / [网站2](https://developer.android.google.cn/studio) / [网站3](https://mac.softpedia.com/get/Development/Editors/Android-Studio.shtml#download)或爬梯[官网](https://developer.android.com/studio/index.html)
2. 不需要像windows那样配置环境变量，直接拖动安装，打开(不发送统计信息，不导入配置，cancel不能连接SDK,一路默认next，一直到下载完相关组件)
3. 欢迎界面选择 configure -> plugins -> 分别搜索Dart Flutter进行安装


* 升级homebrew 

1. 查看版本 ``` brew --version ```
2. 卸载brew ``` ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)" ```  
3. 安装brew ``` ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" ```
4. 查看版本 ``` brew --version ``` 
5. 安装目录	 ``` which brew ```
6. 执行下面代码 

	```
	brew update
	brew install --HEAD usbmuxd
	brew link usbmuxd
	brew install --HEAD libimobiledevice
	brew install ideviceinstaller
	brew install ios-deploy
	```
7. 如果Xcode 版本低,需要安装高版本(可以安装多版本然后选择高版本)
8. 卸载重装可能需要加路径
	```
	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	```

* 升级rvm
	
1. 查看版本 ``` rvm --version ```
2. 升级RVM ``` curl -L https://get.rvm.io | bash -s stable ```
3. 生效RVM ``` source ~/.rvm/scripts/rvm ```
4. 重载RVM ``` rvm reload ```
5. 查看版本 ``` rvm --version ```
6. 安装目录 ``` which rvm ```

* 升级ruby

1. 查看版本 ``` ruby --version ```
2. 已知版本 ``` rvm list known ```
3. 安装指定 ``` rvm install 2.6.3 ``` 或者 ``` rvm install ruby-2.6.3 ```
4. 已安列别 ``` rvm list ```
5. 设定默认 ``` rvm use 2.6.3 --default ```
6. 删除多余 ``` rvm remove 2.0.0 ```
7. 安装目录 ``` which ruby ```

* 手动安装ruby
	
1. 下载ruby [ruby下载地址](http://www.ruby-lang.org/en/downloads/)
2. 进入解压文件夹执行 ``` ./configure ```
3. 编译 ``` sudo make ```
4. 安装 ``` sudo make install ```

* 升级openssl 
	
1. 查看版本 ``` openssl version ```
2. 升级版本 ``` brew upgrade openssl ```
3. 链接新版 ``` brew link openssl --force ```
4. 查看版本 ``` openssl version ```
5. 安装目录 ``` which openssl ```

* 升级gem
	
1. 查看版本 ``` gem --version ```
2. 升级版本 ``` gem update --system ``` 
3. 查看版本 ``` gem --version ```

* 升级cocoaPods
	
1. 查看版本 ``` pod --version ```
2. 升级版本 ``` sudo gem update cocoapods ```
3. 查看版本 ``` pod --version ```
4. 安装目录 ``` which pod ```

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
8. 查看是否成功 ``` flutter --version ```
9. 安装依赖 ``` flutter doctor ``` 有叉号提示的需要执行提示命令

	```
	flutter doctor --android-licenses
	```
	如果提示错误，可能要执行 '升级homebrew' 的命令

> ### 方案   

* 如果不存在 .bash_profile     
创建 ``` vim ~/.bash_profile ```,可能需要执行 '升级homebrew' 的命令

* 如果使用的是其他如 zsh，终端启动时 ~/.bash_profile 将不会被加载    
解决办法就是修改 ~/.zshrc ，在其中添加：source ~/.bash_profile

* ERROR: Could not connect to lockdownd, error code -17  

	先执行必要卸载命令
	
	```
	brew uninstall --ignore-dependencies libimobiledevice
	brew uninstall --ignore-dependencies usbmuxd
	brew install --HEAD usbmuxd
	brew unlink usbmuxd
	```
	然后执行 '升级homebrew' 的命令






[Flutter中文网](https://flutterchina.club/flutter-for-ios/)   
[Flutter开发环境配置](https://segmentfault.com/a/1190000016878485)    
[搭建Flutter-iOS开发环境](https://www.cnblogs.com/lovestarfish/p/10628205.html)    
[Mac系统安装AndroidStudio](https://www.jianshu.com/p/d6421d2d62df)   
[flutter环境配置详解MAC版](https://www.jianshu.com/p/b50a92afbef1)


[下一页 Flutter2 入门知识](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter002.md)