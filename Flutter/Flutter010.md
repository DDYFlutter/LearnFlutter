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
5. Icons(系统Icon集合，参见[官方图标网站](https://material.io/resources/icons/?icon=account_balance&style=baseline) or [fluttericon.com](http://fluttericon.com/))

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

> ### TabBar

1. TabController

* TabController是Tab的控制器，用于定义Tab标签页和内容页坐标，还可配置切换动画
* 若标签页固定格局，则TabController可放入StatelessWidget中，以节省资源，提升运行效率
* 若标签页内容或数量动态变化，可放入StatefulWidget中，以适应变化需求

    TabController
    
    ```
    TabController({ 
        int initialIndex = 0,   // 初始序列值
        @required this.length,  // 标签数，必须给定值
        @required TickerProvider vsync // TrickerProvider定义了可以发送Tricker对象的接口，必须给定值
     })
     // Tricker:获取每帧刷新通知，相当于给动画加了个动起来的引擎
    ```
    
    DefaultTabController

    ```
    const DefaultTabController({
        Key key,
        @required this.length, // 标签数，必须给定值
        this.initialIndex = 0, // 初始序列值
        @required this.child, // 子元素，必须给定值
    })
    ```

2. TabBar

* TabBar是Flutter Material Design中一种横向标签控件,通常底部结合TabBarView使用
* TabBar是标签Title控件，切换的入口，一般放入AppBar使用，其子元素水平排列
* 如果纵向排列则使用Column、ListView控件包装
* TabBar子元素为Tab类型数组

    TabBar
    
    ```
    const TabBar({
        Key key,
        @required this.tabs, // List<Widget>，标签数组(一般数组元素为Tab对象，也可以其他Widget)
        this.controller, // 自定义TabController，没提供将默认使用DefaultTabController.of的值
        this.isScrollable = false, // 标签栏可滚动性，true时可能改变默认标签宽度
        this.indicatorColor, // 指示器颜色
        this.indicatorWeight = 2.0, // 指示器厚度(高度)
        this.indicatorPadding = EdgeInsets.zero, // 指示器Padding 
        this.indicator, // Decoration 指示器的修饰器，用于背景或者边框(border)
        this.indicatorSize, // 指示器大小计算,TabBarIndicatorSize.label与文字等宽,TabBarIndicatorSize.tab与tab等宽
        this.labelColor, // Color,选中label颜色
        this.labelStyle, // TextStyle,选中label样式
        this.labelPadding, // 每个label的Padding
        this.unselectedLabelColor, // Color,未选中label颜色
        this.unselectedLabelStyle, // TextStyle,未选中label样式
        this.dragStartBehavior = DragStartBehavior.start, // 
        this.onTap,
    })
    ```

    Tab
    
    ```
    const Tab({
        Key key,
        this.text, // 文本
        this.icon, // 图标
        this.child, // 子元素
    })
    ```

3. TabBarView

* Tab内容容器，放置主题内容，子元素可以是多个各种不同类型控件

    ```
    const TabBarView({
        Key key,
        @required this.children, // List<Widget>,内容Widget数组
        this.controller, // 自定义TabController，没提供将默认使用DefaultTabController.of的值
        this.physics, // ScrollPhysics
        this.dragStartBehavior = DragStartBehavior.start,
    })
    ```
    
4. 实例

* 默认DefaultTabController

    ```
    class MinePage extends StatelessWidget {
    
      String title;
    
      MinePage({Key outerKey, this.title}) : super(key: outerKey);
    
      final List<Tab> _topTabs = [Tab(text: '我000', ), Tab(text: '是111',), Tab(text: '谁222',)];
    
      @override
      Widget build(BuildContext context) {
        print(MediaQuery.of(context).size.height);
        return DefaultTabController( // 默认的TabController
          length: _topTabs.length,
          child: Scaffold(
            appBar: AppBar(
              title: Text(title, style: TextStyle(color: Colors.white)),
              backgroundColor: Colors.deepPurple,
              bottom: TabBar(
                tabs: _topTabs, // 绑定标签数组
                indicatorColor: Colors.white,// 指示器颜色
                isScrollable: true,
                labelColor: Colors.white,
                labelStyle: TextStyle(fontSize: 16.0),
                unselectedLabelColor: Colors.red.shade400,
                unselectedLabelStyle: TextStyle(fontSize: 16.0),
              ),
            ),
            body: TabBarView(
              children: _topTabs.map((Tab item){
                return new Center(child: SelectableText(item.text, style: TextStyle(fontSize: 20.0),),);
              }).toList(),
            ),
          ),
        );
      }
    }
    ```

* 自定义TabController

    ```
    class FindPage extends StatefulWidget {
    
      String title;
    
      FindPage({Key outerKey, this.title}) : super(key: outerKey);
    
      @override
      State<StatefulWidget> createState() {
        return _FindPageTabBarState();
      }
    }
    
    class _FindPageTabBarState extends State<FindPage> with SingleTickerProviderStateMixin {
    
      TabController _tabController; // tabController变量
      final List<Tab> _topTabs = [Tab(text: '我000', ), Tab(text: '是111',), Tab(text: '谁222',)];
      List<Widget> _tabContents = [
        Center(child: SelectableText('这是一个能选择的文本\nThis is a SelectableText', style: TextStyle(fontSize: 25.0),),),
        Center(child: SelectableText('这是一个能选择的文本\nThis is a SelectableText', style: TextStyle(fontSize: 25.0),),),
        Center(child: SelectableText('这是一个能选择的文本\nThis is a SelectableText', style: TextStyle(fontSize: 25.0),),),];
      // 销毁函数
      // @override
      void dispose() {
        _tabController.dispose();
        super.dispose();
      }
    
      void initState() {
        super.initState();
        _tabController = TabController(length: _topTabs.length, vsync: this);
      }
    
      @override
      Widget build(BuildContext context) {
        return Scaffold(
          appBar: AppBar(
            title: Text(widget.title, style: TextStyle(color: Colors.white),),
            backgroundColor: Colors.teal.shade400,
            bottom: TabBar(
              tabs: _topTabs,
              indicatorColor: Colors.white,
              indicatorWeight: 3.0,
              labelColor: Colors.white,
              labelStyle: TextStyle(fontSize: 16.0),
              unselectedLabelColor: Colors.red,
              unselectedLabelStyle: TextStyle(fontSize: 16.0),
              controller: _tabController, // TabBar与自定义TabController绑定
            ),
          ),
          body: TabBarView(
            controller: _tabController,
            children: _tabContents,
          ),
        );
      }
    }
    ```

> ### BottomNavigationBar

1. BottomNavigationBar

    ```
    BottomNavigationBar({
        Key key,
        @required this.items, // List<BottomNavigationBarItem>, 底部导航列表
        this.onTap, // ValueChanged<int> ,点击子项回调
        this.currentIndex = 0, // int 当前选中的子项下标
        this.elevation = 8.0, // 分割阴影，默认8.0，设置0则没有阴影效果
        BottomNavigationBarType type, // 导航栏类型，有fixed和shifting两种,shifting时点击有移动效果
        Color fixedColor, // type为fixed时导航颜色，默认ThemeData.primaryColor
        this.backgroundColor, // 背景色
        this.iconSize = 24.0, // 图标大小
        Color selectedItemColor, // 选中项颜色
        this.unselectedItemColor, // 未选中项颜色
        this.selectedIconTheme = const IconThemeData(), // 选中项主题
        this.unselectedIconTheme = const IconThemeData(), //未选中项主题
        this.selectedFontSize = 14.0, // 选中项文本字号
        this.unselectedFontSize = 12.0, // 未选中项文本字号
        this.selectedLabelStyle, // 选中项文本样式
        this.unselectedLabelStyle, // 未选中项文本样式
        this.showSelectedLabels = true, // 选中项是否显示文本
        bool showUnselectedLabels, // 未选中项是否显示文本，type为fixed时默认true,为shifting时默认false
    })
    ```

2. BottomNavigationBarItem

    ```
    const BottomNavigationBarItem({
        @required this.icon, // Widget，一般为Icon图标
        this.title, // Widget，一般为Text文本
        Widget activeIcon, // 选中时显示
        this.backgroundColor, // 背景色
      })
    ```

3. 实例

    ```
    class DDYBottomBar extends StatefulWidget {
      @override
      State<StatefulWidget> createState() => new _tabbarState();
    }
    
    class _tabbarState extends State<DDYBottomBar> {
    
      /// 默认选中
      int _selectedIndex = 0;
      /// 按钮文字
      final _itemTextList = ['聊天', '通讯录', '发现', '我的'];
      /// 按钮图标
      final _itemIconList = ['chats', 'contacts', 'find', 'mine'];
      /// 主体页面
      final _bodyPageList = [ChatsPage(title: "聊天"), ContactsPage(title: "通讯录"), FindPage(title: "发现"), MinePage(title: "我的")];
    
      /// 构建按钮
      BottomNavigationBarItem _buildBottomBarItem(int index) {
    
        var isSelect = index == _selectedIndex;
        var itemText = _itemIconList[index];
        var itemIcon = 'image/main_tab_' + '${_itemIconList[index]}' + '${isSelect ? '_pre.png' : '_nor.png'}';
        var itemSize = isSelect ? 34.0 : 34.0;
        var itemColor = isSelect ? Colors.cyan : Colors.grey;
    
        return BottomNavigationBarItem(
            icon: new Image.asset(itemIcon, width: itemSize, height: itemSize,),
            title: Text(itemText, style: TextStyle(color: itemColor),)
        );
      }
      /// 构建底部导航条
      Widget _buildBottomBar() {
        return BottomNavigationBar(
          items: [_buildBottomBarItem(0), _buildBottomBarItem(1), _buildBottomBarItem(2), _buildBottomBarItem(3)],
          type: BottomNavigationBarType.shifting,
          onTap: _onTapToSelectItem,
          currentIndex: _selectedIndex,
          elevation: 10.0,
          showSelectedLabels: true,
          showUnselectedLabels: true,
        );
      }
    
      /// 构建主体页面
      Widget _buildBody() {
        return _bodyPageList[_selectedIndex];
      }
    
      /// 点击事件回调
      _onTapToSelectItem(int toIndex) {
        setState(() => _selectedIndex = toIndex);
      }
    
      @override
      Widget build(BuildContext context) {
        return Scaffold(body: _buildBody(), bottomNavigationBar: _buildBottomBar(),);
      }
    }
    ```


[上一页 Flutter9 Container、Text、Image](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter009.md)   
[下一页 Flutter11 FloatingActionButton、Row、Column、Stack](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter011.md)
