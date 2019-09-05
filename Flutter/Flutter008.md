本文对应github地址[Flutter8](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter008.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)

> ### Everything is widget

* 屏幕上元素由Widget层实现，Widget分为两种风格：Materia风格和Cupertino风格
* Flutter中存在Widget、Element、RenderObject、Layer四棵树，其中Widget和Element是一对多关系
* Element中持有Widget和RenderObject,而Element和RenderObject一一对应
* 不是每个RenderObject都有Layer，只有isRepaintBoundary为true才能形成Layer
* Flutter中Widget不可变，每次保持一帧，如果发生改变要通过State实现跨帧状态保存，而真实完成布局和绘制的是RenderObject，Element充当两者桥梁，State就是保存在Element中
* Flutter中BuildContext只是接口，而Element实现了该接口
* Flutter中setState其实是调用markNeedBuild，该方法内部标记此Element为Dirt，然后在下一帧WidgetBinding.drawFrame才会被绘制，且setState并不是立即生效
* Flutter中RenderObject在attch/layout之后会通过markNeedPaint()使页面重绘
* Flutter中一般json数据从String转Object过程都先经过Map类型
* Flutter中InheritedWidget一般用于状态共享，如Theme、Localizations、MediaQuery等，都是通过它实现共享状态，可通过context去获取共享的状态，如ThemeData theme = Theme.of(context);

```
-->1 RenderObject markNeedsPaint()   -->2 PipelineOwner requestVisualUpdate()  
-->3 RendererBinding scheduleFrame() -->4 ui.Window scheduleFrame()          
-->5 ui.Window onDrawFrame()         -->6 SchedulerBinding handleDrawFrame()   
-->7 RendererBinding drawFrame()     -->8 RenderObject paint                  
```  

> ### MeterialApp