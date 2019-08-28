本文对应github地址[Flutter3](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter003.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)

这里用的Dart2


> ### 基本知识

* 打印函数 ``` print('123'); ```
* 取变量值 ``` ${varName} ``` 字符串插值表达式($expression),取变量可省略{}
* 声明变量 ``` var varName = "hello world"; ``` 
* 明确指定数据类型方式定义变量 ``` int age = 18; ``` 
* 双引号字符串，分号结尾
* 函数可以不显式声明返回类型(不写返回类型就成了dynamic类型，自动推导类型)，无return则void类型
* static声明类变量
* const和final声明常量，const只能通过静态数据赋值

	```
	const lastName = 'postbird';
	final firstName = 'bird';
	// lastName = '123';  // 报错
	// firstName = '123'; // 报错
	final time = new DateTime.now();
	// const time2 = new DateTime.now(); // 报错
	```
* Dart没有public、private等修饰符，_下划线代表private ，但有 @protected 注解 。

> ### 定义类型

* 直接明确指定数据类型

	```
	int intNumber = 18; // 确认int类型
	// intNumber = "20"; // 更改类型，报错
	intNumber = 22; // 变量重新赋值
	print(intNumber); // 打印，22
	```
* var 声明变量，类型推导

	```
	var doubleNum = 11.1; // 类型推导，double类型
	// doubleNum = "22.2"; // 更改类型，报错
	doubleNum = 33; // 变量重新赋值
	print(doubleNum); // 打印，33.0```
	```
	
* Object，所有对象根基类

	```
	Object objectNum = 66; // Object类型，不推导具体类型
	objectNum = "88"; // Object类型，因为无具体类型，可赋值不同类型，但结果仍不推导类型
	print(objectNum); // 打印，88
	// print(objectNum.length); // 最终仍Obejct类型，仍不推导类型，所以不能调用String方法，报错	
	```
* dynamic，动态类型，可重新赋值后重新推导

	```
	dynamic dynamicNum = 10086; // 特殊关键字，可能不自动补全
	print(dynamicNum.runtimeType); // 打印类型，int
	dynamicNum = "1008611"; // 重新赋值，重新推导类型，String
	print(dynamicNum.length); // 此时为String,可调用length
	```


> ### Numbers(int double)

* Dart中提供了int和double,但没有提供float

	```
	testNumbers() {
		int numberInt1 = 10; // int
		double numberDouble1 = 11.1; // double
		var numberInt2 = numberInt1; // 自动推导类型，int
		var numberDouble2 = numberDouble1; // 自动推导类型，double
		var numberString1 = numberInt1.toString(); // 强制类型转换，int转String
		var numberString2 = numberDouble1.toString(); // 强制类型转换，double转String
		print('numberString1:' + numberString1 + ' ' + 'numberString2:' + numberString2); // 字符串可以用 + 拼接
		print('${numberInt1}, ${numberInt2}'); // ${}中包含
		print(' $numberDouble1, $numberDouble2'); // 单纯变量打印，可以省略{}
	}
	```
	
> ### String

* String定义时可用单引号或者双引号，如果创建多行(保留格式)的String可用三个单引号(双引号)

	```
	var nickName = "Rain"; // 双引号，自动推导类型
	String address = 'BeiJing'; // 单引号，显式指定类型
	// String zipCode = 10000; // 显式指定类型，如果类型不匹配报错
	
	// 使用三个单引号或三个双引号 创建多行字符串,保留内在格式，如换行和缩进等
    String formatStr = '''asd
     11111
       22222
          33333
    ''';
    
   	// 使用r创建原始raw字符串（转义字符等特殊字符会输出出来，而不会自动被转义）
   	String profile = 'Hello \n world';
   	
   	// 可以函数嵌套
   	getTime() {
      return "10";
   	}
   	const firstName = 'Li'; // 字符串，可用单引号
   	final lastName = "Xiaolong"; // 字符串，也可用双引号
   	// firstName = 'Liang'; // const修饰表示常量，重新赋值会报错
   	// lastName = "Sicheng"; // final修饰表示常量，重修赋值会报错
   	// const time1 = getTime(); const 只能静态赋值
   	final String time2 = getTime(); // 可加上String修饰 10
   	var convertInt = double.parse(time2); // 强制类型转换，String转double 10.0
	print(firstName + lastName + time2 + '${convertInt}'); // 打印，LiXiaolong1010.0
	```
	
* 增删改查(拼接、截取、替换、查找)	
	
	```
	// 拼接
	String userName = "Li" 'Lei'; // 两个字符串拼接(不能定义的变量)直接连接，可省略+
	String allName = userName + "(John)"; // + 拼接(有定义的变量不能省略)
	String personInfo = "Info: " + "${10.toString()} $allName"; // ${} 可嵌套表达式和变量(变量时可省略大括号)
	print("${firstName.padLeft(6, "#*")}"); // 用#*补齐6位(#*算一个整体) #*#*#*#*Li
	
	// 截取
	print("${"Xiaolong".substring(2)}"); // 从下标2截取字符串到尾(0下标起始，包含2), aolong
	print("${"Xiaolong".substring(1, 3)}"); // 含头不含尾, ia
	print("${"a,d,d d,c,,".split(',')}"); // 分割，返回的是一个数组, [a, d, d d, c, , ]
	print("${'abfba'.split(new RegExp(r"b*"))}"); //正则匹配(r指取原始字符串,不转义)，拆分字符串, [a, ba]
	print("${3.12345.toStringAsFixed(3)}"); // 保留精度，3.123
	
	// 替换
	print("${"Xiaolong".replaceAll("ol", "L")}"); // 替换字符串,XiaLong
	print("${"LiXiaolong".replaceFirst("o", "o ")}"); // 只替换第一个符合条件的, LiXiao long
	print("${"LiiLei".replaceFirst("i", "i Lei", 1)}"); // 从下标1(包含)开始替换第一个符合条件的, Li LeiiLei
	print("${"Xiaolong".replaceRange(0, 4, "Wang")}"); // 范围替换(含头不含尾)，Wanglong
	// XiaOOlOOng
	print("Xiaolong".replaceAllMapped("o", (Match match){ //用方法返回值替换指定的字符串
	  return "OO";
	}));
	// 从下标3(包含)开始替换，XiaolOOng
	print("Xiaolong".replaceAllMapped("o", (Match match){ //用方法返回值替换指定的字符串
	  return "OO";
	}));
	// 查询并替换
	String originalStr = "a b,c d,e";
	String checkReplaceStr = originalStr.splitMapJoin(',',
	    onMatch: (Match match) {return "&";}, // 匹配到的是一个整体
	    onNonMatch: (String nonMatch) {return "*";}); // 没匹配到的是一个整体
	print(checkReplaceStr); // *&*&*
	
	// 查找
	print("${"1008611".indexOf("0")}"); // 第一个符合条件的起始下标 1 找不到返回-1
	print("${"1008611".lastIndexOf("0")}"); // 最后一个符合条件的起始下标 2 找不到返回-1
	print("${"3.1415926".indexOf("1", 3)}"); // 从下标3(包含)开始 第一个符合条件的起始下标 4 找不到返回-1
	print("${"bcabacb".lastIndexOf("a", 2)}"); // 从下标2(包含)开始 第一个符合条件的起始下标 2 找不到返回-1
	print("abacacabacab".indexOf(new RegExp(r'a(b|c)'))); // 正则 第一个符合条件的起始下标 0 找不到返回-1
	print("abacacabacab".lastIndexOf(new RegExp(r'a(b|c)'))); // 正则 最后一个符合条件的起始下标 10 找不到返回-1
	```	

* 其他方法

	```
	/** 其他函数 */
	print("${"Xiaolong".length}"); // 字符串长度, 8
	print("${"Xiaolong".isEmpty}"); // 是否为空, false
	print("${"Xiaolong".isNotEmpty}"); // 是否不为空, true
	print("${"Xiaolong".contains('ia')}"); // 包含关系，true
	print("${"Xiaolong".startsWith("Xi")}"); // 以Xi开头, true
	print("${"Xiaolong".endsWith("g")}"); // 以g结尾, true
	print("${'${"Xiaolong".toLowerCase()}'}"); // 转小写 , xiaolong
	print("${'${"Xiaolong".toUpperCase()}'}"); // 转大写 , XIAOLONG
	print("${"Xiaolong".trim()}"); // 去两端空格
	print("${"Xiaolong".trimLeft()}"); // 去左边空格
	print("${"Xiaolong".trimRight()}"); // 去右边空格
	print("${"Li".padLeft(6, "#*")}"); // 左边用#*补齐6位(#*算一个整体) #*#*#*#*Li
	print("${"Li".padRight(6)}"); // 右边补齐6位, 默认补" "(若要补齐位数已经小于实际位数，则不需要补齐，返回原值)
	print("${"a".compareTo("b")} ${"a".compareTo("a")} ${"aa".compareTo("a")}"); // -1 0 1
	print("${"Li".codeUnitAt(0)} ${"Li".codeUnits}"); // 返回Unicode: 76 [76, 105]
	```

* 字符串运算符

	```
	// +：加号运算符，拼接功能
	// *：乘法运算符，字符串按照因子N次重复拼接
	// ==：等号运算符，比较两个字符串是否相同
	// []：取值运算符，取出字符串索引位指向的单个字符
	print("a" + "b"); // 拼接运算符 ab
	print("a" + "b" * 3); // 乘法运算符，字符串按照因子N次重复拼接 abbb
	print("${"Li" == "Xiaolong"}"); // 相等关系,false
	print("${"Xiaolong"[0]}"); ;// 下标取值，X
	```
	

* 参考文章

	[Dart之字符串(String)](https://www.cnblogs.com/lxlx1798/p/11280106.html)   
	[Dart字符串判空](https://cloud.tencent.com/developer/article/1370380)   
	[Dart 基本数据类型与类型归属判断](www.ptbird.cn/dart-variable-operation.html)   
	[Dart：修饰符 static final const](https://www.jianshu.com/p/91c2511d104f)   
	[Dart4Flutter -01 – 变量, 类型和 函数](https://juejin.im/post/5b2bafdaf265da597c772819)

> ### bool

* 只有true/false(0/1，null不能作为判断条件)

	```
	bool selected = true;
	if (selected) // 省略了{}
	  print("1");
	else
	  print("0");
	```

> ### List

* List默认可以添加不同类型的值，有增、删、改、查、遍历、排序操作

	```
	var list0 = const ["stringList"];
	// list0.add("10086"); // const定义常量list，内容不可变
	var list1 = List<double>(1); // 固定长度1, 指定类型double（无值却null）
	var list2 = List(1); // 固定长度(不为空，1个值null)
	// list2.add("1"); 已经两个null,再添加报错 固定长度的list不能通过add添加数据
	list2[0] = "000"; // 改值
	List list3 = List<String>(); // 相当于长度0
	// list3.add(1); // 类型错误不可添加
	var list4 = [1, "2", 3, 4, 5, list0, list1, list2, list3]; // 可以嵌套不同类型
	list4.add("6"); // 添加新值
	list4.removeAt(3); // 移除下标3的值
	list4.remove(5); // 移除特定值5
	list4.insert(2,'好人'); // 固定位置添加
	list4.insertAll(0, ["#"]); // 固定位置插入新List
	print("${list4.length} ${list4}"); // 5 [1, 2, 3, [stringList], [], 6]
	print("${list1.isEmpty} ${list3.isNotEmpty}"); // 判断是否为空或不为空 false false
	print("${list4.first} ${list4.last}"); // 第一个/最后一个元素
	// 遍历
	for (var value in list4) {
	  print(value);
	}
	// forEach
	list4.forEach((itemValue) => print(itemValue));
	```
* 参考文章

	[Dart之数组(List)](https://www.cnblogs.com/lxlx1798/p/11104618.html)


> ### Map

* 声明定义

	```
	// 常规声明
	var map1 = {'1':"1", 2 : 2}; // 不指定类型，Key-Value 均为 dynamic
	var map2 = new Map(); // 不指定类型
	map2["name"] = "LiLei";
	map2[18] = 18;
	var map3 = <String, String>{}; // 指定类型，大括号内可以有值
	map3["nick"] = "Lee";
	var map4 = Map<String, int>(); // 指定类型，省略new
	map4["age"] = 18;
	print("$map1 $map2 $map3 $map4"); // {1: 1, 2: 2} {name: LiLei, 18: 18} {nick: Lee} {age: 18}
	
	// 复制形式
	Map map5 = Map.castFrom(map4); // 从map4复制
	// map5[19] = 19; // 运行报错，map4类型<String, int>，这里<int, int>，不匹配
	map5["19"] = 19; // 类型满足，可以添加
	// Map map6 = Map.castFrom<String, int>(map3); // 报错，map3为<String, String> 这里<String, int>，不匹配
	print("$map5");
	
	// const修饰，不可变Map
	Map map7 = const {"1":"1"};
	// map7["2"] = "2";
	print("$map7");
	// 根据list创建key-value
	List<String> keys = ['one','two'];
	List<String> values = ['Android','IOS'];
	Map map8 = Map.fromIterables(keys, values); // key-value数量一致，否则报错
	print(map8);
	```

* 增删改查
	
	```
	 // 增删改查
	Map map9 = {"1":"1", "2":"2", "3":"3", "4":"4"};
	map9["1"] = 2; // 如果key不存在则增加，存在则修改
	var resultValue1 = map9.update("1", (value) => (value*5)); // 更新值(类型不匹配则报错)
	var resultValue2 = map9.update("2", (value) => (value+"999"));// 更新值(类型不匹配则报错)
	//map9.remove("3"); // 删除键值对
	map9["4"] = ""; // 只是赋空值，不删除
	map9.updateAll((dynamic key, dynamic value) {
	  if (key == "4") return '4444';
	  return value;
	});
	print("${resultValue1} ${resultValue2} ${map9}"); // 10 2999 {1: 10, 2: 2999, 3: 3, 4: 4444}
	
	Map<String,int> map10 = {"a":1,"b":2,"c":3,"d":4,"e":5};
	// map10.removeWhere((key,value)=>(value>3));//删除掉 符合参数函数的keyvalue对
	map10.removeWhere((key,value){
	  return value>3;
	});
	print(map10);//{a: 1, b: 2, c: 3}
	print("${{"1":"2"}.containsKey("1")} ${{"1":"2"}.containsValue("1")}"); // 是否包含key/value true false
	
	// 遍历
	map10.forEach((String key, int value) => print("$key:$value"));
	map10.keys.forEach((String key) => print(key));
	map10.values.forEach((int value) => print(value));
	// 遍历时可以修改值，不可删除值
	map10.forEach((String key, int value) {
	  if (value > 1) map10[key] = 10086;
	});
	print(map10); // {a: 1, b: 10086, c: 10086}
	```

* 常用属性与方法

	```
	// 常营属性
	print("${{"1":"1"}.length} ${{"1":"1"}.isEmpty} ${{"1":"1"}.isNotEmpty}"); // 长度，是否空，是否非空 1 false true
	print("${{"10":"12", "11":"13"}.keys} ${{"11":"13"}.values}"); // key和value各自集合(List) (10, 11) (13)
	print("${{"10":"12", "11":"13"}.entries}"); // 迭代的键值对集合 (MapEntry(10: 12), MapEntry(11: 13))
	// 遍历更改范型
	Map<String,int> map19 = {"a":1,"b":2,"c":3};
	Map<int,int> map20 = map19.map((String key,int value){
	  return new MapEntry(value, value);
	});
	print(map20); // {1: 1, 2: 2, 3: 3}
	map19.clear(); // 清空
	print("$map19"); // {}
	
	// 合并（类型一致）
	Map mapAdd1 = <String, String>{"1":"2"};
	Map<String, String> mapAdd2 = {"1":"2121", "3":"4"};
	Map mapAdd3 = <String, String>{"1":"2"};
	Map<String, String> mapAdd4 = {"1":"2121", "3":"4"}; // key相同则覆盖
	mapAdd1.addAll(mapAdd2); // key相同则覆盖
	mapAdd3.addEntries(mapAdd4.entries); // key相同则覆盖
	print("$mapAdd1 $mapAdd3"); // {1: 2121, 3: 4} {1: 2121, 3: 4}
	
	Map putMap = {"1":"1"};
	String result1 = putMap.putIfAbsent("1", () => "10086"); // 取值，取到则返回取到的值 "1"
	String result2 = putMap.putIfAbsent("2", () => "10086"); // 取值，取不到返回给定值"10086" 并在原map中添加keyvalue
	print("$result1 $result2 $putMap"); // 1 10086 {1: 1, 2: 10086}
	```