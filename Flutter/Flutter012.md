本文对应github地址[Flutter12](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter012.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> ### ListBody(列表组件，不常用)

* 在指定轴方向按顺序排列子节点，一般配合ListView和Column等控件使用
* 在主轴上，子节点顺序布局，且给予子节点的空间必须不受限制(unlimited),使子节点全部被容纳
* ListBody不会裁剪或缩放子节点。
* 在交叉轴上，子节点尺寸会被拉伸，以适应交叉轴的区域
    
    ##### 构造函数

    ```
    ListBody({
        Key key,
        this.mainAxis = Axis.vertical, // 排列的主轴方向
        this.reverse = false, // 是否反向
        List<Widget> children = const <Widget>[],
    })
    ```
    
    ##### 无状态
    
    ```
    class ListBodyLessDefault extends StatelessWidget {
      final widget;
      final parent;
    
      const ListBodyLessDefault([this.widget, this.parent]): super();
    
      @override
      Widget build(BuildContext context) {
        return ListBody(
              mainAxis: Axis.vertical, 
              reverse: false, 
              children: <Widget>[
                Container(color: Colors.red, width: 50.0, height: 150.0, child: Text('111')),
                Container(color: Colors.yellow, width: 50.0, height: 50.0, child: Text('222')),
                Container(color: Colors.green, width: 50.0, height: 50.0, child: Text('333')),
                Container(color: Colors.blue, width: 50.0, height: 50.0, child: Text('444')),
              ],
            );
      }
    }
    ```
    
    ##### 有状态
    
    ```    
    class ListBodyFullDefault extends StatefulWidget {
      const ListBodyFullDefault() : super();
    
      @override
      State<StatefulWidget> createState() => _ListBodyFullDefault();
    }
    
    class _ListBodyFullDefault extends State {
      @override
      Widget build(BuildContext context) {
        return ListBody(
          // 如果没有,就是不需要有状态的 StatefulWidget
        );
      }
    }
    ```
    

> ### ListView

* ListView继承自BoxScrollView，主要用来展示列表性数据
* 一般一个slivers里面包含一个SliverList的CustomScrollView
* ListView在主轴方向可以滚动，在交叉轴方向则是填满ListView
* ListView一般会配合ListTitle使用，ListTitle是使用非常多的子项Widget
* 列表分类:水平列表、垂直列表、大数据量列表、矩阵式列表
* ListView有四种构建方式

1. new ListView()

    此构造器适用于少量(有限个)子项的列表视图，该构造器需要构造每个子项，而不仅仅可见子项
    
    ```
    ListView({
      Key key,
      Axis scrollDirection = Axis.vertical,
      bool reverse = false,
      ScrollController controller,
      bool primary,
      ScrollPhysics physics,
      bool shrinkWrap = false,
      EdgeInsetsGeometry padding,
      this.itemExtent,
      bool addAutomaticKeepAlives = true,
      bool addRepaintBoundaries = true,
      bool addSemanticIndexes = true,
      double cacheExtent,
      List<Widget> children = const <Widget>[],
      int semanticChildCount,
      DragStartBehavior dragStartBehavior = DragStartBehavior.start,
    })
    ```
    
    ##### 该构造器示例代码
    
    ```
    import 'package:flutter/material.dart';
    
    class TestListView extends StatefulWidget {
    
      TestListView({Key, key}) : super(key: key);
    
      @override
      State<StatefulWidget> createState() => _TestListViewState();
    }
    
    class _TestListViewState extends State<TestListView> {
      
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(title: Text('测试ListView'),),
          body: Center(
            child: ListView(
              children: <Widget>[
                ListTile(title: Text('item1'),),
                ListTile(title: Text('item2'),),
                ListTile(title: Text('item3'), trailing: Icon(Icons.chevron_right, color: Colors.blue,),),
                ListTile(title: Text('item4'), subtitle: Text('subTitle'),),
              ],
            ),
          ),
        );
      }
    }
    ```    
    
    ##### 如果想加分割线 
    
    ```
    // 显示分割线
    
    class _TestListViewState extends State<TestListView> {
    
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(title: Text('测试ListView'),),
          body: Center(
            child: ListView(
              children: ListTile.divideTiles(
                context: context,
                tiles: [
                  ListTile(title: Text('item1'), trailing: Icon(Icons.chevron_right, color: Colors.blue.shade600,),),
                  ListTile(title: Text('item2'), leading: Icon(Icons.adb),),
                  ListTile(title: Text('item3'),),
                  ListTile(title: Text('item4'), subtitle: Text('subTitle'),),
                ],
              ).toList(), 
            ),
          ),
        );
      }
    }
    ```   
    

2. new ListView.builder()

    此构造器含一个IndexedWidgetBuilder，适用于大量或无限子项数的列表，仅为可见子项调用构造器(控件viewport加上头尾的cacheExtent范围的子item才被构造，传递一个builder,按需加载，提高性能)
    
    ```
    ListView.builder({
        Key key,
        Axis scrollDirection = Axis.vertical,
        bool reverse = false,
        ScrollController controller,
        bool primary,
        ScrollPhysics physics,
        bool shrinkWrap = false,
        EdgeInsetsGeometry padding,
        this.itemExtent,
        @required IndexedWidgetBuilder itemBuilder,
        int itemCount,
        bool addAutomaticKeepAlives = true,
        bool addRepaintBoundaries = true,
        bool addSemanticIndexes = true,
        double cacheExtent,
        int semanticChildCount,
        DragStartBehavior dragStartBehavior = DragStartBehavior.start,
    })
    ```
    
    ##### 示例代码

    ```
    class _TestListViewState3 extends State<TestListView> {
    
      final cities = ['BeiJing', 'ShangHai', 'ChongQing', 'HeFei', 'HanDan', 'JiLin', 'BaoDing', 'ShenYang', 'ZhengZhou', 'LaSa',
        'ChangChun', 'GuiYange', 'KunMing', 'TaiYuan', 'ChengDu', 'JiNan', 'QingDao', 'MoHe', 'TengChong', 'ShenZhen', 'TianJin',
      'SuZhou', 'CangZhou', 'MianYang'];
    
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(title: Text('测试 ListView.builder()'),),
          body: ListView.builder(
            itemCount: cities.length, // 必须给定数量，否则变成无限造成越界
              itemBuilder: (context, index) {
                return ListTile(title: Text(cities[index]),);
          }),
        );
      }
    }
    ```

3. new ListView.separated()

    含两个IndexedWidgetBuilder，itemBuilder按需构建子项，separatorBuilder类似地构建出在子项之间的分割子项，适用于具有固定数量子项的列表。列表中需要分割线时可以自定义复杂的分割线
    
    ```
    ListView.separated({
        Key key,
        Axis scrollDirection = Axis.vertical,
        bool reverse = false,
        ScrollController controller,
        bool primary,
        ScrollPhysics physics,
        bool shrinkWrap = false,
        EdgeInsetsGeometry padding,
        @required IndexedWidgetBuilder itemBuilder,
        @required IndexedWidgetBuilder separatorBuilder,
        @required int itemCount,
        bool addAutomaticKeepAlives = true,
        bool addRepaintBoundaries = true,
        bool addSemanticIndexes = true,
        double cacheExtent,
    })
    ```
    
    ##### 示例代码
    
    ```
    // 测试ListView.separated()
    class _TestListViewState4 extends State<TestListView> {
    
      List cities = ['BeiJing', 'ShangHai', 'ChongQing', 'HeFei', 'HanDan', 'JiLin', 'BaoDing', 'ShenYang', 'ZhengZhou', 'LaSa',
        'ChangChun', 'GuiYange', 'KunMing', 'TaiYuan', 'ChengDu', 'JiNan', 'QingDao', 'MoHe', 'TengChong', 'ShenZhen', 'TianJin',
        'SuZhou', 'CangZhou', 'MianYang'];
    
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(title: Text('测试 ListView.builder()'),),
          body: ListView.separated(
            itemCount: cities.length, // 必须给定数量，否则变成无限造成越界
            itemBuilder: (context, index) {
              return ListTile(
                title: Text(cities[index]),
                onTap: () {
                  print('onTap $index');
                },
                onLongPress: () {
                  print('onLongPress $index and add new item \'888888\'');
                  setState(() { // setState(){} 后重绘
                    cities.insert(0, '888888');
                  });
                },
              );
            },
            separatorBuilder: (context, index) => Divider(),
          ),
        );
      }
    }
    
    // 水平布局
    class _TestListViewState5 extends State<TestListView> {
    
      final cities = ['BeiJing', 'ShangHai', 'ChongQing', 'HeFei', 'HanDan', 'JiLin', 'BaoDing', 'ShenYang', 'ZhengZhou', 'LaSa',
        'ChangChun', 'GuiYange', 'KunMing', 'TaiYuan', 'ChengDu', 'JiNan', 'QingDao', 'MoHe', 'TengChong', 'ShenZhen', 'TianJin',
        'SuZhou', 'CangZhou', 'MianYang'];
    
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(title: Text('测试 ListView.builder()'),),
          body: ListView.separated(
            scrollDirection: Axis.horizontal,
            itemCount: cities.length, // 必须给定数量，否则变成无限造成越界
            itemBuilder: (context, index) {
              return Container(
                margin: const EdgeInsets.symmetric(horizontal: 1.0),
                color: Colors.grey.shade100,
                child: Text(cities[index]),
              );
            },
            separatorBuilder: (context, index) => Divider(),
          ),
        );
      }
    }
    ```


4. new ListView.custom()

    需要SliverChildDelegate提供自定义子项的其他能力，如SliverChildDelegate可以控制用于估计不可见子项大小的算法。适用于想做额外设置(如列表最大滚动范围)或滑动时每次布局的子Item范围
    
    ```
    const ListView.custom({
        Key key,
        Axis scrollDirection = Axis.vertical,
        bool reverse = false,
        ScrollController controller,
        bool primary,
        ScrollPhysics physics,
        bool shrinkWrap = false,
        EdgeInsetsGeometry padding,
        this.itemExtent,
        @required this.childrenDelegate,
        double cacheExtent,
        int semanticChildCount,
    })
    ```
    
    ##### 示例代码
    
    ```
    class _TestListViewState6 extends State<TestListView> {
    
      final cities = ['BeiJing', 'ShangHai', 'ChongQing', 'HeFei', 'HanDan', 'JiLin', 'BaoDing', 'ShenYang', 'ZhengZhou', 'LaSa',
        'ChangChun', 'GuiYange', 'KunMing', 'TaiYuan', 'ChengDu', 'JiNan', 'QingDao', 'MoHe', 'TengChong', 'ShenZhen', 'TianJin',
        'SuZhou', 'CangZhou', 'MianYang'];
    
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(title: Text('测试 ListView.builder()'),),
          body: ListView.custom(
            childrenDelegate: SliverChildBuilderDelegate((context, index) {
              return Container(
                height: 60.0,
                alignment: Alignment.center,
                color: Colors.deepPurple[100*Random().nextInt(2)+100],
                child: Text(cities[index], style: TextStyle(color: Colors.red),),
              );
            },
              childCount: cities.length,
            ),
          ),
        );
      }
    } 
    ```   
    
    ##### 参数释意    

    * scrollDirection 
    
    Axis,滚动方向，默认Axis.vertical纵向，Axis.horizontal表示横向
    
    * reverse 
    
    bool,是否反向滚动，默认false
    
    * controller
    
    ScrollController,滑动监听，可以用它做下拉刷新和上拉加载更多
    
    * primary
    
    bool,是否是与父级PrimaryScrollController关联的主滚动视图，如果true则必须设置controller
    
    * physics
    
    ScrollPhysics,设置ListView如何响应用户滑动行为，值为ScrollPhysics对象，其实现类有：
    
    ```
    AlwaysScrollableScrollPhysics：总是可以滑动。
    NeverScrollableScrollPhysics：禁止滚动。
    BouncingScrollPhysics：内容超过一屏，上拉有回弹效果。
    ClampingScrollPhysics：包裹内容，不会有回弹，跟AlwaysScrollableScrollPhysics差不多。
    ```
    * shrinkWrap
    
    bool,默认false,是否应该由正在查看的内容确定scrollDirection中滚动视图的范围。多用于嵌套ListView,内容大小不确定，如垂直布局先放文字ListView（需要Expend包裹否则无法显示无穷大高度 但是需要确定listview高度 shrinkWrap使用内容适配不会有这样的影响）
    
    * padding
    
    EdgeInsetsGeometry,滚动视图和子项之间的内边距
    
    * itemExtent
    
    double,子项范围
    
    * indexedWidgetBuilder
    
    @required,itemBuilder位置构造器
    
    * itemCount
    
    子item数量，只有使用ListView.builder()或ListView.separated()构造函数时才能指定，且后者必须指定
    
    * addAutomaticKeepAlives
    
    bool,默认true,可以理解成是否自动保存视图缓存，对应于 SliverChildBuilderDelegate.addAutomaticKeepAlives属性。即是否将每个子项包装在AutomaticKeepAlive中。
    
    * addRepaintBoundaries
    
    bool,默认true,可以理解成是否添加重绘边界，对应于 SliverChildBuilderDelegate.addRepaintBoundaries属性。是否将每个子项包装在RepaintBoundary中
    
    * addSemanticIndexes
    
    bool,默认true,对应于 SliverChildBuilderDelegate.addSemanticIndexes属性。是否将每个子项包装在IndexedSemantics中。
    
    * cacheExtent
    
    视口在可见区域之前和之后有一个区域，用于缓存在用户滚动时即将变为可见的项目。
    
    * semanticChildCount    
        
    提供语义信息的孩子的数量    
    
> ### ListTile

    ```
    const ListTile({
        Key key,
        this.leading,              // item 前置图标
        this.title,                // item 标题
        this.subtitle,             // item 副标题
        this.trailing,             // item 后置图标
        this.isThreeLine = false,  // item 是否三行显示
        this.dense,                // item 直观感受是整体大小
        this.contentPadding,       // item 内容内边距
        this.enabled = true,       // item 是否可用(点击)
        this.onTap,                // item onTap 点击事件
        this.onLongPress,          // item onLongPress 长按事件
        this.selected = false,     // item 是否选中状态
    })
    ```


> ### GridView、


> ### 参考

* [基本组件之基本列表LisetView组件](https://www.cnblogs.com/lxlx1798/p/11063520.html)
* [ListView基于数据动态渲染，横向和纵向列表嵌套](http://www.ptbird.cn/flutter-listview-listview-builder.html)
* [ListView](https://www.jianshu.com/p/8ae901efdebc)
* [ListView 列表进阶](https://www.jianshu.com/p/e6dafb114855)

https://juejin.im/post/5bab35ff5188255c3272c228
http://codingdict.com/blog/article/2019/2/11/795.html