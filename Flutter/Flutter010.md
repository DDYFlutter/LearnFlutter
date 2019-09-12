本文对应github地址[Flutter10](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter010.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)

> ### Icon

1. Icon
	
	```
	// 继承自 StatelessWidget
	const Icon(
		this.icon, {
		Key key,
		this.size, // 图标尺寸
		this.color, // 图标颜色
		this.semanticLabel, // 语义描述,向Andoid上TalkBack和iOS上VoiceOver提供图像描述
		this.textDirection, // 呈现图标的文本方向
	})
	```
	
2. IconButton(可交互的Icon)	

	```
	// 继承自 StatelessWidget，默认无背景
	const IconButton({
		Key key,
		this.iconSize = 24.0,
		this.padding = const EdgeInsets.all(8.0),
		this.alignment = Alignment.center,
		@required this.icon,
		this.color, 
		this.focusColor,
		this.hoverColor,
		this.highlightColor,
		this.splashColor,
		this.disabledColor,
		@required this.onPressed,
		this.focusNode,
		this.tooltip,
	})
	```
3. IconTheme(Icon主题)
4. ImageIcon(通过AssetImage或其他图片显示Icon)
5. Icons(系统Icon集合，参见[官方图标网站](https://material.io/resources/icons/?icon=account_balance&style=baseline))

> ### Color/Colors

1. Color() 

	* 使用8个16进制(如果6个则透明)
	* Color(0xFFAABBFF)
	
2. Color.fromARGB() 

	* 使用整数[0-255]或16进制[0x00-0xFF]
	* Color.fromARGB(255, 66, 165, 245) 或 Color.fromARGB(0xFF, 0x42, 0xA5, 0xF5)

3. Colors

	* 直接加颜色名 Colors.blue
	* 加颜色名和阴影 Colors.green[400] 或 Colors.green.shade400


[上一页 Flutter9 Container、Text、Image](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter009.md)   
[下一页 Flutter11 ](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter011.md)



[Flutter中的国际化：如何写一个多语言的App](https://blog.csdn.net/yumi0629/article/details/81873141)   
[Flutter 中的国际化](https://www.jianshu.com/p/8356a3bc8f6c)