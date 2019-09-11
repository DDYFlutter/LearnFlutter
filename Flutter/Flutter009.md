本文对应github地址[Flutter9](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter009.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> ### Container

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



> ### 参考

* [基本组件之容器组件Container](https://www.cnblogs.com/lxlx1798/p/11058794.html)









[Flutter中的国际化：如何写一个多语言的App](https://blog.csdn.net/yumi0629/article/details/81873141)   
[Flutter 中的国际化](https://www.jianshu.com/p/8356a3bc8f6c)

