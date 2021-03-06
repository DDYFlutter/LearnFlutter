本文对应github地址[Flutter9](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter009.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> ### Container

继承自StatelessWidget

* Container是一种常用控件，由负责布局、绘画、定位和大小调整的几个控件组成
* 具体为LimitedBox、ConstrainedBox、Align、Padding、DecoratedBox、Transform组成，而不是Container子类化
* 继承关系 Object > Diagnosticable > DiagnosticableTree > Widget > StatelessWidget > Container

* 参数

1. alignment
	* AlignmentGermetry，实现类为Alignment,控制child对齐方式
	* Alignment.topLeft 顶部左对齐 Alignment(-1.0, -1.0)
	* Alignment.topCenter 顶部居中对齐 Alignment(0.0, -1.0)
	* Alignment.topRight 顶部右对齐 Alignment(1.0, -1.0)
	* Alignment.centerLeft 垂直居中水平居左对齐 Alignment(-1.0, 0.0)
	* Alignment.center 垂直居中水平居中对齐 Alignment(0.0, 0.0)
	* Alignment.centerRight 垂直居中水平居右对齐 Alignment(1.0, 0.0)
	* Alignment.bottomLeft 底部居左对齐 Alignment(-1.0, 1.0)
	* Alignment.bottomCenter 底部居中对齐 Alignment(0.0, 1.0)
	* Alignment.bottomRight 底部居右对齐 Alignment(1.0, 1.0)
	* Alignment原点为父组件中心(左为x正方向，下为x负方向，x[-1, 1], y[-1, 1])
	* FractionalOffset原点为父组件左上角，所以Alignment.center等价于FractionalOffset(0.5, 0.5)
	* TextDirection.ltr时
	* AlignmentDirectional方式可能会与TextDirection相关(有Text要注意)
	* AlignmentDirectional.bottomCenter：底边中点
	* AlignmentDirectional.bottomEnd:右下角
	* AlignmentDirectional.bottomStart:左下角
	* AlignmentDirectional.center：垂直中心点
	* AlignmentDirectional.centerEnd：右边中点
	* AlignmentDirectional.centerStart：左边中点
	* AlignmentDirectional.topCenter：上边中点
	* AlignmentDirectional.topEnd：右上角
	* AlignmentDirectional.topEnd : 左上角

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
	* BoxConstraints 添加到child上额外约束条件，指定组件占据空间大小的属性
	* BoxConstraint只关注minWidth、maxWidth、minHeight和maxHeight
	* 如果想把Container扩大，可使用expand()函数

	```
	// Container没子控件，只能占据maxWidth和maxHeight指定的空间
	Container(
	  color: Colors.green,
	  constraints: BoxConstraints(
	    maxHeight: 300.0,
	    maxWidth: 200.0,
	    minWidth: 150.0,
	    minHeight: 150.0
	  ),
	),
	
	// Container有子控件，只能指定的空间(不小于最小值且不大于最大值)

	Container(
	  color: Color.fromARGB(255, 66, 165, 245),
	  alignment: AlignmentDirectional(0.0, 0.0),
	  child: Container(
	    color: Colors.green,
	    child: Text("Flutter Cheatsheet Flutter Cheatsheet"),
	    constraints: BoxConstraints(
	      maxHeight: 300.0,
	      maxWidth: 200.0,
	      minWidth: 150.0,
	      minHeight: 150.0
	    ),
	  ),
	),
	
	// 单纯扩大,会扩大到最大
	constraints: BoxConstraints.expand(),
	// 可以指定宽高
	constraints: BoxConstraints.expand(width: 350.0,height: 400.0)
	```

9. margin
	* 外边距，一般用EdgetInsets
	* 四周都加上特定边距 margin: EdgeInsets.all(20.0)
	* 横竖不同边距 EdgeInsets.symmetric(vertical: 20.0,horizontal: 50.0)
	* 左上右下不同边距 EdgeInsets.fromLTRB(20.0, 30.0, 40.0, 50.0)
10. transform
	* 设置container的变换矩阵，类型为Matrix4
	* 可按需调整旋转角度，如 transform: Matrix4.rotationZ(0.2)
11. child
	* 子组件，Container只能有一个子组件
	* 若想布局多个子组件，可用如Row、Column或Stack等，可将组件添加到children

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
15. SelectableText 内容可选择的文本

    ```
    const SelectableText(
        this.data, {
        Key key,
        this.focusNode,
        this.style,
        this.strutStyle,
        this.textAlign,
        this.textDirection,
        this.showCursor = false,
        this.autofocus = false,
        ToolbarOptions toolbarOptions,
        this.maxLines,
        this.cursorWidth = 2.0,
        this.cursorRadius,
        this.cursorColor,
        this.dragStartBehavior = DragStartBehavior.start,
        this.enableInteractiveSelection = true,
        this.onTap,
        this.scrollPhysics,
        this.textWidthBasis,
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

	```
	Image.memory(
		Uint8List bytes, {
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

* 主要参数详解

1. image 

	* ImageProvider，ImageProvider实现类有AssetBundleImageProvider(子类AssetImage),FileImage,MemoryImage,NetworkImage
	* AssetImage需要配置pubSpec.yaml文件
	```
	flutter:
		
		assets:
			- images/xxx.png
			- images/2.0x/xxx.png
			- images/3.0x/xxx.png
	```
	
2. colorBlendMode

	* color属性会覆盖image,可设置colorBlendMode属性与image混合
	* BlendMode枚举，默认BlendMode.srcIn

3. excludeFromSemantic

	* 包括图像语义描述

4. fit

	* BoxFit，图片布局方式
	* BoxFit.fill, 不按比例缩放铺满控件，类似Android的fitXY
	* BoxFit.contain, 按比例缩放完整居中显示，类似Android的centerInside
	* BoxFit.cover, 按比例放大居中显示，类似Android的centerCrop
	* BoxFit.fitWidth, 把高按照组件的高显示，宽等比例缩放
	* BoxFit.fitHeight, 把宽按照组件的宽显示，高等比例缩放
	* BoxFit.none, 将图片的内容按原大小居中显示
	* BoxFit.scaleDown, 若宽高大于组件宽高同contain，若宽高小于组件宽高同none。

5. centerSlice

	* Rect,中心拉伸，如 centerSlice: Rect.fromLTWH(10, 10, 10, 10)

6. gaplessPlayback

	* bool, true保留旧图到新图显示，false则不保留，显示新图过程空白

7. repeat

	* ImageRepeat，小图填充时重复方式
	* repeat, x,y轴方向都重复
	* repeatX, 只x轴方向重复
	* repeatY, 只y轴方向重复
	* noRepeat, 只单图，不重复

* 圆角

1. 使用裁剪方式

	```
	new ClipRRect(
	  child: Image.network(imageUrl),
	  borderRadius: BorderRadius.only(
	    topLeft: Radius.circular(20),
	    topRight: Radius.circular(20),
	  ),
	)
	```

2. 使用边框方式

	```
	new Container(
	  width: 120,
	  height: 60,
	  decoration: BoxDecoration(
	    shape: BoxShape.rectangle,
	    borderRadius: BorderRadius.circular(10.0),
	    image: DecorationImage(
	        image: NetworkImage(imageUrl),
	        fit: BoxFit.cover), // 该方式要设置fit
	  ),
	)
	```
* 原形

1. 使用裁剪方式

	```
	new ClipOval(
	    child: Image.network(
	    imageUrl,
	    scale: 8.5,
	  ),
	)
	```


2. 使用CircleAvatar 

	```
	new CircleAvatar(
	  backgroundImage: NetworkImage(imageUrl),
	  radius: 50.0,
	)
	```
* 渐入或占位 FadeInImage



> ### 参考

* [基本组件之容器组件Container](https://www.cnblogs.com/lxlx1798/p/11058794.html)
* [SizedOverflowBox、Transform、CustomSingleChildLayout详解](https://www.jianshu.com/p/4fe7573b1fc7)
* [Container详解](https://www.jianshu.com/p/366b2446eaab)
* [图片组件Image](https://www.cnblogs.com/lxlx1798/p/11060601.html)
* [官方讲解Image](https://api.flutter.dev/flutter/widgets/Image-class.html)



[上一页 Flutter8 widget、MeterialApp、Scaffold、AppBar](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter008.md)   
[下一页 Flutter10 Icon、Tabbar、BottomNavigationBar](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter010.md) 