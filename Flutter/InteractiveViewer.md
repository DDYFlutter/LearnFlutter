#### InteractiveViewer

Flutter 1.20 新增组件 InteractiveViewer，主要对移动、缩放的手势交互进行封装，方便使用。

> ##### 属性

```
alignPanAxis = false, // 只沿轴拖动[不能同时上下和左右]
boundaryMargin = EdgeInsets.zero, // 边界边距
constrained = true, // 默认true，为false时，子组件将被赋予无限的约束
maxScale = 2.5, // 最大放大倍数
minScale = 0.8, // 最小缩小倍数
onInteractionEnd, // 手指滑动结束回调
onInteractionStart, // 手指滑动开始回调
onInteractionUpdate, // 手势滑动更新回调
panEnabled = true, // 是否可以拖动
scaleEnabled = true, // 是否可以缩放
transformationController, // 变换控制器
@required child, // 子组件
```

> ##### 移动

```
Container testMovePanInteractiveViewer = Container(
  height: 200,
  width: 200,
  color: Colors.grey,
  child: InteractiveViewer(
    alignPanAxis: false, // 轴的方向拖动，true表示不能歇着拖动
    panEnabled: true, // 是否可以拖动，默认true
    scaleEnabled: false,
    boundaryMargin: EdgeInsets.all(40), // 边界边距
    child: Container(
      color: Colors.white,
      child: Image.asset('image/RainDou.gif'),
    ),
  ),
);
```

> ##### 缩放

```
/// 缩放
var testScaleInteractiveViewer = Container(
  height: 300,
  width: 300,
  color: Colors.red,
  child: InteractiveViewer(
    panEnabled: false, // 是否可以拖动，默认true
    boundaryMargin: EdgeInsets.all(100), // 边界边距
    scaleEnabled: true, // 是否可以缩放
    maxScale: 2.5, // 最大放大倍数 默认最大2.5
    minScale: 0.2, // 最小缩小倍数 默认最小0.8
    child: Container(
      color: Colors.lightGreen,
      child: Image.network('http://image.5you.com/attachment/soft/2020/0717/104943_17395542.png'),
    ),
  ),
);
```

https://www.colabug.com/2020/0810/7604447/