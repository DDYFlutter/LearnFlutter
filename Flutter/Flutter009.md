本文对应github地址[Flutter9](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter009.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> ### Container

继承自StatelessWidget

* Container是一种常用控件，由负责布局、绘画、定位和大小调整的几个控件组成
* 具体为LimitedBox、ConstrainedBox、Align、Padding、DecoratedBox、Transform组成，而不是Container子类化
* 继承关系 Object > Diagnosticable > DiagnosticableTree > Widget > StatelessWidget > Container

* 参数

1. alignment
	* 控制child对齐方式
	* Alignment.topCenter 顶部居中对齐
	* Alignment.topLeft 顶部左对齐
	* Alignment.topRight 顶部右对齐
	* Alignment.center 垂直居中水平居中对齐
	* Alignment.centerLeft 垂直居中水平居左对齐
	* Alignment.centerRight 垂直居中水平居右对齐
	* Alignment.bottomCenter 底部居中对齐
	* Alignment.bottomLeft 底部居左对齐
	* Alignment.bottomRight 底部居右对齐
2. padding
	* 内边距
3. color
	* Color 设置背景颜色
4. decoration
	* Decoration 容器的修饰器，用于背景或者border
	* 绘制在child下层的装饰，不能喝color同时使用
5. foregroundDecoration
	* 绘制在child上层的装饰
6. width
	* double 宽
7. height
	* double 高
8. constraints
	* BoxConstraints 添加到child上额外的约束条件
9. margin
	* 外边距
10. transform
	* 设置container的变换矩阵，类型为Matrix4
	* 可按需调整旋转角度，如 transform: Matrix4.rotationZ(0.2)
11. child
	* 子组件

> ### Text

继承自StatelessWidget

1. data

* String,内容文字

2. style

* 装饰物
* color,颜色 Color(0xFF000000)/Colors.black
* decoration,样式
	* TextDecoration.none 无，默认
	* TextDecoration.overline 上划线
	* TextDecoration.lineThrough 删除线
	* TextDecoration.underline 下划线
	* 也可调用 TextDecoration.combine() 传入集合设置多个装饰物  
* decorationColor,装饰物颜色，Color对象
* decorationStyle 设置装饰物的样式
	* TextDecorationStyle.dashed 装饰线为虚线
	* TextDecorationStyle.dotted 装饰线为点构成的线
	* TextDecorationStyle.double 装饰线为两根线
	* TextDecorationStyle.solid 装饰线为实心线
	* TextDecorationStyle.wavy 装饰线为波浪线
	* 也可调用 TextDecorationStyle.values()传入集合设置多种样式 
* fontSize,double,字号，默认10.0
* fontFamily,字体，自定义字体名字要搭配package使用
* package,自定义字体包路径
* fontStyle,字体样式
	* FontStyle.italic 斜体
	* FontStyle.normal 正常 
* 字体字重
	* FontWeight.bold 加粗
* inherit,true使用父类样式，false默认白色，10.0px,sans-serif
* letterSpacing，double,字间距
* wordSpacing，double,词间距
* height,double,行高系数，该值乘以fontSize作为行高

3. strutStyle



4. textAlign

* 文本对齐方式[left,right,center,justify,start,end(文本开头/结尾，和textDirection有关)]

5. textDirection

* 文本方向,rtl(从右向左),ltr(从左向右)
* TextAlign.start 时设置rtl，相当于又把 textAlign 设置为 TextAlign.end
* TextAlign.end 时设置rtl，相当于又把 textAlign 设置为 TextAlign.start

6. locale

* 文本区域Locale('zh', 'CH')

7. softWrap

* 超出是否换行,true换行，false不换行

8. overflow

* 超出文本处理
* TextOverflow.clip 直接切断，剩余文字没有了
* TextOverflow.fade 超出部分进行渐变消失效果(softWrap:false才有效果)
* TextOverflow.ellipsis 显示为省略号
* TextOverflow.visible 在容器外也呈现

9. textScaleFactor

* double,缩放因子

10. maxLines

* int,最大显示行数

11. semanticsLabel

* String,图像的语义描述,向Andoid上TalkBack和iOS上VoiceOver提供图像描述

12. textWidthBasis,


13. 富文本 Text.rich()

	```
	const Text.rich(
		this.textSpan, {
		Key key,
		this.style,
		this.strutStyle,
		this.textAlign,
		this.textDirection,
		this.locale,
		this.softWrap,
		this.overflow,
		this.textScaleFactor,
		this.maxLines,
		this.semanticsLabel,
		this.textWidthBasis,
	})
	```

14. 富文本 new RichText()

	```
	RichText({
		Key key,
		@required this.text,
		this.textAlign = TextAlign.start,
		this.textDirection,
		this.softWrap = true,
		this.overflow = TextOverflow.clip,
		this.textScaleFactor = 1.0,
		this.maxLines,
		this.locale,
		this.strutStyle,
		this.textWidthBasis = TextWidthBasis.parent,
	})
	```

> ### Image

继承自StatefulWidget

* 继承关系 Object > Diagnosticablet > DiagnosticableTreet > Widgett > StatefulWidgett > Image
* 支持的格式jpeg、png、gif、webP、bmp、webmp
* new Image: 从ImageProvider获取图像

	```
	const Image({
		Key key,
		@required this.image,
		this.frameBuilder,
		this.loadingBuilder,
		this.semanticLabel,
		this.excludeFromSemantics = false,
		this.width,
		this.height,
		this.color,
		this.colorBlendMode,
		this.fit,
		this.alignment = Alignment.center,
		this.repeat = ImageRepeat.noRepeat,
		this.centerSlice,
		this.matchTextDirection = false,
		this.gaplessPlayback = false,
		this.filterQuality = FilterQuality.low,
	}) 
	```

* new Image.asset: 加载本地图片文件

	```
	Image.asset(
		String name, {
		Key key,
		AssetBundle bundle,
		this.frameBuilder,
		this.semanticLabel,
		this.excludeFromSemantics = false,
		double scale,
		this.width,
		this.height,
		this.color,
		this.colorBlendMode,
		this.fit,
		this.alignment = Alignment.center,
		this.repeat = ImageRepeat.noRepeat,
		this.centerSlice,
		this.matchTextDirection = false,
		this.gaplessPlayback = false,
		String package,
		this.filterQuality = FilterQuality.low,
	})
	```

* new Image.file： 加载本地图片文件

	```
	Image.file(
		File file, {
		Key key,
		double scale = 1.0,
		this.frameBuilder,
		this.semanticLabel,
		this.excludeFromSemantics = false,
		this.width,
		this.height,
		this.color,
		this.colorBlendMode,
		this.fit,
		this.alignment = Alignment.center,
		this.repeat = ImageRepeat.noRepeat,
		this.centerSlice,
		this.matchTextDirection = false,
		this.gaplessPlayback = false,
		this.filterQuality = FilterQuality.low,
	})
	```
* new Image.network: 加载网络图片

	```
	Image.network(
		String src, {
		Key key,
		double scale = 1.0,
		this.frameBuilder,
		this.loadingBuilder,
		this.semanticLabel,
		this.excludeFromSemantics = false,
		this.width,
		this.height,
		this.color,
		this.colorBlendMode,
		this.fit,
		this.alignment = Alignment.center,
		this.repeat = ImageRepeat.noRepeat,
		this.centerSlice,
		this.matchTextDirection = false,
		this.gaplessPlayback = false,
		this.filterQuality = FilterQuality.low,
		Map<String, String> headers,
	})
	```
* new Image.memory: 加载内存Uint8List资源图片。

> ### 参考

* [基本组件之容器组件Container](https://www.cnblogs.com/lxlx1798/p/11058794.html)
* [SizedOverflowBox、Transform、CustomSingleChildLayout详解](https://www.jianshu.com/p/4fe7573b1fc7)
* [Container详解](https://www.jianshu.com/p/366b2446eaab)
* [图片组件Image](https://www.cnblogs.com/lxlx1798/p/11060601.html)
* [官方讲解Image](https://api.flutter.dev/flutter/widgets/Image-class.html)








[Flutter中的国际化：如何写一个多语言的App](https://blog.csdn.net/yumi0629/article/details/81873141)   
[Flutter 中的国际化](https://www.jianshu.com/p/8356a3bc8f6c)



[上一页 Flutter8 widget、MeterialApp、Scaffold、AppBar](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter008.md)   
[下一页 Flutter10 ](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter010.md) 