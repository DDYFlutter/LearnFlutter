本文对应github地址[Flutter12](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter012.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> ### ListBody(列表组件，不常用)

* 在指定轴方向按顺序排列子节点，一般配合ListView和Column等控件使用
* 在主轴上，子节点顺序布局，且给予子节点的空间必须不受限制(unlimited),使子节点全部被容纳
* ListBody不会裁剪或缩放子节点。
* 在交叉轴上，子节点尺寸会被拉伸，以适应交叉轴的区域
    
    构造函数

    ```
    ListBody({
        Key key,
        this.mainAxis = Axis.vertical, // 排列的主轴方向
        this.reverse = false, // 是否反向
        List<Widget> children = const <Widget>[],
    })
    ```
    
    无状态
    
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
    
    有状态
    
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

> ### GridView、


> ### 参考

https://juejin.im/post/5bab35ff5188255c3272c228
http://codingdict.com/blog/article/2019/2/11/795.html