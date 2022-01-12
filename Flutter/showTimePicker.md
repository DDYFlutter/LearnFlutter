#### showTimePicker

Flutter提供了一些日期选择组件，如DayPicker、MonthPicker、YearPicker、showDatePicker、CupertinoDatePicker等

> ##### DayPicker

显示给定月份的日期并允许选择一天。这些天以矩阵网格排列，每行代表1周7天。

```
DayPicker({
    Key key,
    @required this.selectedDate,
    @required this.currentDate,
    @required this.onChanged,
    @required this.firstDate,
    @required this.lastDate,
    @required this.displayedMonth,
    this.selectableDayPredicate,
    this.dragStartBehavior = DragStartBehavior.start,
})
```

DayPicker 参数详解

* selectedDate: 选中的日期(有圆形背景的那天)[DateTime]
* currentDate: 当前日期(文字高亮的那天)[DateTime]
* onChanged: 用户选择的日期发生变化的回调[void Function(DateTime value)]
* firstDate: 可选择日期的开始值[DateTime]
* lastDate: 可选择日期的结束值[DateTime]
* displayedMonth: 显示的月份[DateTime]
* selectableDayPredicate:可选日期依据，返回false指不可选[bool Function(DateTime day)]
* dragStartBehavior: 有down和start两种[enum DragStartBehavior]

显示2020年10月

```
DateTime _selectedDate = DateTime.now();

DayPicker(
	selectedDate: _selectedDate,
	currentDate: DateTime.now(),
	onChanged: (date) {
		setState(
			_selectedDate = date;
		);
	},
	firstDate: DateTime(2020, 10, 1),
	lastDate: DateTime(2020, 10, 31),
	displayedMonth: DateTime(2020, 10),
	selectableDayPredicate: (date) {
		// 只选今天以前的日期(之后的灰色不可选)
		return date.difference(DateTime.now()).inMilliceconds < 0;
	},
)
// Predicate: 断言，断定，以...为依据
```

> ##### MonthPicker

可改变月份的日期选择器，顶部有一个滚动的月份列表，下面为展示月份的天。可以理解成MonthPicker = 滚动的月份列表 + DayPicker
用法和DayPicker差不多

```
MonthPicker({
    Key key,
    @required this.selectedDate,
    @required this.onChanged,
    @required this.firstDate,
    @required this.lastDate,
    this.selectableDayPredicate,
    this.dragStartBehavior = DragStartBehavior.start,
}) 
```

> ##### YearPicker

年份选择器，显示效果为上下滚轮

```
YearPicker({
    Key key,
    @required this.selectedDate,
    @required this.onChanged,
    @required this.firstDate,
    @required this.lastDate,
    this.dragStartBehavior = DragStartBehavior.start,
})
```

> ##### showDatePicker

showDatePicker差不多就是封装了YearPicker和MonthPicker，并进行联动

```
Future<DateTime> showDatePicker({
  @required BuildContext context,
  @required DateTime initialDate,
  @required DateTime firstDate,
  @required DateTime lastDate,
  DateTime currentDate,
  DatePickerEntryMode initialEntryMode = DatePickerEntryMode.calendar,
  SelectableDayPredicate selectableDayPredicate,
  String helpText,
  String cancelText,
  String confirmText,
  Locale locale,
  bool useRootNavigator = true,
  RouteSettings routeSettings,
  TextDirection textDirection,
  TransitionBuilder builder,
  DatePickerMode initialDatePickerMode = DatePickerMode.day,
  String errorFormatText,
  String errorInvalidText,
  String fieldHintText,
  String fieldLabelText,
}) async
```

相关参数详解

* initialDate: 初始化事件，通常设置为当前时间
* firstDate: 可选择范围的起始时间
* lastDate: 可选择范围的截止时间
* selectableDayPredicate: 可选日期依据，返回false指不可选
* builder: 包装对话框窗口小部件以添加继承的窗口小部件，如Theme

用法举例

```
RaisedButton(
	onPressed: () async {
		var selectedResult = await showDatePicker(
			context: context,
			initialDate: DateTime.now(),
			firstDate: DateTime(2020, 10, 1),
			lastDate: DateTime(2020, 12, 30),
			builder: (ctx, child) {
				return Theme(
					data: ThemeData.dark(), // 深色主题
					child: child,
				);
			},
		);
		print('$selectedResult');
	},
)
```

> ##### CupertinoDatePicker

iOS风格的日期选择器

```
CupertinoDatePicker({
    Key key,
    this.mode = CupertinoDatePickerMode.dateAndTime,
    @required this.onDateTimeChanged,
    DateTime initialDateTime,
    this.minimumDate,
    this.maximumDate,
    this.minimumYear = 1,
    this.maximumYear,
    this.minuteInterval = 1,
    this.use24hFormat = false,
    this.backgroundColor,
}) 
```

参数详解

* mode: 设置日期展示效果的格式
	* time: 只显示时间，效果： 1 59 PM
	* date: 只显示日期，效果： July 31 2020
	* dateAndTime: 时间和日期都显示，效果：Sun, July 31 0 59 PM
* mininumDate: 最小日期
* maxinumDate: 最大日期 	
* use24Formate: 是否使用24小时制

用法举例

```
CupertinoDatePicker(
	onDateTimeChanged: (selectedDate) {
	
	}
	mininumDate: DateTime.now().add(Duration(days: -1)),
	maxinumDate: DateTime.now().add(Duration(days: 1)),
	user24Formate: true
)
```

> ##### showTimePicker

