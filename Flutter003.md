本文对应github地址[Flutter3](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter3.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)

<center><font color=lightGray>这里用的Dart2</font></center>

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

	```
	testStrings() {
	    // 可以函数嵌套
	    getTime() {
	      return "10";
	    }
	    const firstName = 'Li'; // 字符串，可用单引号
	    final lastName = "Xiaolong"; // 字符串，也可用双引号
	    // firstName = 'Liang'; // const修饰表示常量，重新赋值会报错
	    // lastName = "Sicheng"; // final修饰表示常量，重修赋值会报错
	    // const time1 = getTime(); const 只能静态赋值
	    final String time2 = getTime(); // 可加上String修饰
	    var convertInt = double.parse(time2); // 强制类型转换，String转double
	    print(firstName + lastName + time2 + '${convertInt}'); // 打印，LiXiaolong1010.0
	
	    print("${lastName.contains('ia')}"); // 包含关系，true
	    print("${lastName == firstName}"); // 相等关系,false
	    print("${lastName[0]}"); ;// 下标取值，X
	    print("${lastName.replaceAll("ol", "L")}"); // 替换字符串,XiaLong
	    print("${lastName.length}"); // 字符串长度, 8
	    print("${lastName.isEmpty}"); // 是否为空, false
	    print("${lastName.isNotEmpty}"); // 是否不为空, true
	    print("${'100' "86"}"); // 相邻两个都为字符串(非变量),拼接可省略+ , 10086
	    print("${'${lastName.toLowerCase()}'}"); // 嵌套表达式，不能省略{} , xiaolong
	    print("${3.12345.toStringAsFixed(3)}"); // 保留精度，3.123
	    print("${lastName.substring(2)}"); // 从下标2截取字符串到尾(0下标起始，包含2), aolong
	    print("${lastName.substring(1, 3)}"); // 含头不含尾, ia
	    print("${"a,d,d d,c,,".split(',')}"); // 分割，返回的是一个数组, [a, d, d d, c, , ]
	    print("${'abfba'.split(new RegExp(r"b*"))}"); //正则匹配，拆分字符串, [a, ba]
	    print("${lastName.startsWith("Xi")}"); // 以Xi开头, true
	    print("${lastName.endsWith("g")}"); // 以g结尾, true
	    print("${firstName.padLeft(6, "#*")}"); // 用#*补齐6位(#*算一个整体) #*#*#*#*Li
	    // XiaOOlOOng
	    print(lastName.replaceAllMapped("o", (Match match){//abyydeab 用方法返回值替换指定的字符串
	      return "OO";
	    }));
	    // 使用三个单引号或三个双引号 创建多行字符串,保留内在格式，如换行和缩进等
	    String formatStr = '''asd
	     1111
	       2222
	          3333
	    ''';
	    print(formatStr);
	}	
	```

* 字符串运算符

	- +：加好运算符，字符串拼接功能
	- *：乘法运算符，字符串按照因子N次重复拼接
	- ==：等号运算符，比较两个字符串是否相同
	- []：取值运算符，取出字符串索引位指向的单个字符


[Dart之字符串](https://www.cnblogs.com/lxlx1798/p/11280106.html)   
[Dart字符串判空](https://cloud.tencent.com/developer/article/1370380)   
[Dart 基本数据类型与类型归属判断](www.ptbird.cn/dart-variable-operation.html)   
[Dart：修饰符 static final const](https://www.jianshu.com/p/91c2511d104f)   
[Dart4Flutter -01 – 变量, 类型和 函数](https://juejin.im/post/5b2bafdaf265da597c772819)



