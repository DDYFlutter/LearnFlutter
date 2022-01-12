> ##### 通用属性

* textColor 文本颜色 
* disabledTextColor 禁用状态时文本颜色
* color 背景颜色
* disabledColor 禁用状态时背景颜色
* highlightColor 高亮状态时背景，如按下时
* splashColor 水波纹颜色

> ##### RaisedButton

material风格的凸起按钮

```
RaisedButton _raisedButton = RaisedButton(
    onPressed: () { // 点击回调，required，当为空时则禁用状态
      print('RaisedButton OnPressed');
    },
    onLongPress: () { // 长按回调
      print('RaisedButton OnLongPress');
    },
    onHighlightChanged: (highLight) { // 高亮状态改变回调
      print('RaisedButton OnHighlightChanged ${highLight}');
    },
    textColor: Colors.black, // 正常状态文本颜色
    disabledTextColor: Colors.white, // 禁用状态文本颜色
    color: Colors.green, // 正常状态背景颜色
    disabledColor: Colors.grey, // 禁用状态背景颜色
    splashColor: Colors.transparent, // 水波纹颜色
    colorBrightness: Brightness.light, // 明暗主题
);
```

> ##### ElevatedButton

Flutter 1.22后新增的material风格的凸起按钮，对应的旧版RaisedButton，但主题管理不同

```
ElevatedButton _elevatedButton = ElevatedButton(
  onPressed: () {
    print('ElevatedButton OnPressed');
  },
  onLongPress: () {
    print('ElevatedButton OnlongPress');
  },
  style: ButtonStyle( // 按钮样式
    textStyle: MaterialStateProperty.all(TextStyle(color: Colors.blue, fontSize: 14)),
    backgroundColor: MaterialStateProperty.all(Colors.green), // 背景色
    foregroundColor: MaterialStateProperty.all(Colors.grey), // 字体颜色通过通过前景色设置，通过textStyle设置无效
    overlayColor: MaterialStateProperty.all(Colors.red), // 高亮色，按钮处于focused, hovered, or pressed时的颜色
    shadowColor: MaterialStateProperty.all(Colors.black), // 阴影颜色
    elevation: MaterialStateProperty.all(5), // 阴影值
  ),
);
```

> ##### FlatButton

扁平按钮

> ##### TextButton

Flutter 1.22后新增的扁平按，对应旧版FlatButton，但主题管理不同

> ##### OutlineButton

带边框的按钮

```
OutlineButton _outlineButton = OutlineButton(
  onPressed: () {
    print('OutlineButton OnPressed');
  },
  borderSide: BorderSide(width: 1, color: Colors.red.shade500),
  disabledBorderColor: Colors.grey,
  highlightedBorderColor: Colors.red,
);
```

> ##### OutlinedButton

Flutter 1.22后新增的带边框的按钮，对应旧版OutlineButton，当主题管理不同

> ##### DropdownButton

下拉选择菜单按钮

> ##### RawMaterialButton

RawMaterialButton是基于Semantic，Material和InkWell创建的组件，不使用当前系统主题和按钮主题，用于自定义按钮或者合并现有的样式。
RaisedButton和FlatButton都是基于RawMaterialButton配置了系统主题和按钮主题的。

> ##### PopupMenuButton

弹出菜单的按钮

> ##### IconButton

图标按钮

> ##### BackButton

material风格的返回按钮，本身是一个IconButton，点击默认执行Navigator.maybePop，即如果路由栈有上一页则返回上一页。

> ##### CloseButon

material风格的关闭按钮，本身是一个IconButton，点击默认执行Navigator.maybePop，即如果路由栈有上一页则返回上一页。
使用场景：BackButton一般适用于全屏页面，CloseButton一般适用于弹出的Dialog页面

> ##### CupertinoButton

iOS风格按钮

> ##### ButtonBar

ButtonBar不是一个单独的按钮控件，而是末端对齐的容器类控件。当在水平方向没有足够空间的时候，按钮将整体垂直排列，而不是换行。

> ##### ButtonStyle

```
const ButtonStyle({
  this.textStyle, //字体
  this.backgroundColor, //背景色
  this.foregroundColor, //前景色
  this.overlayColor, // 高亮色，按钮处于focused, hovered, or pressed时的颜色
  this.shadowColor, // 阴影颜色
  this.elevation, // 阴影值
  this.padding, // padding
  this.minimumSize, //最小尺寸
  this.side, //边框
  this.shape, //形状
  this.mouseCursor, //鼠标指针的光标进入或悬停在此按钮的[InkWell]上时。
  this.visualDensity, // 按钮布局的紧凑程度
  this.tapTargetSize, // 响应触摸的区域
  this.animationDuration, //[shape]和[elevation]的动画更改的持续时间。
  this.enableFeedback, // 检测到的手势是否应提供声音和/或触觉反馈。例如，在Android上，点击会产生咔哒声，启用反馈后，长按会产生短暂的振动。通常，组件默认值为true。
});
```