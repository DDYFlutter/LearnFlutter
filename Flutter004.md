本文对应github地址[Flutter4](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter004.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> 函数

* 函数包含返回值(可省略书写，依靠推断类型，无返回值则void)、函数名、参数组成

	```
	String fullName(String firstName, String lastName) {
	  return "$firstName $lastName";
	}
	// 简写，省略返回类型，=>的使用
	// fullName(String firstName, String lastName) => "$firstName $lastName";
	// 参数还可以是var Object dynamic类型，通过实际赋值类型推导
	// fullName(var firstName, Object lastName) => "$firstName $lastName";
	print(fullName("Li", "Bai")); // Li Bai
	```

* 命名参数（大括号{}包含）

	```
	fullName(var firstName, {Object lastName}) => "$firstName $lastName";
	// 命名参数调用时也得指定参数名
	print(fullName("Li", lastName: "Bai")); // Li Bai
	```

* 参数默认值

	```
	fullName(var firstName, {Object lastName = "Lei"}) => "$firstName $lastName";
	print(fullName("Li", lastName: "Bai")); // Li Bai
	print(fullName("Li")); // Li Lei
	```

* 函数作参数(函数一等公民)

	```
	main(List<String> args) {
	  out(printOutLoud);
	}
	 
	out(void inner(String message)) {
	  inner('Message from inner function');
	}
	 
	printOutLoud(String message) {
	  print(message.toUpperCase());
	}
	```
* 匿名函数

	```
	main(List<String> args) {
	  out((message) {
	    print(message.toUpperCase());
	  });
	}
	 
	out(void inner(String message)) {
	  inner('Message from inner function');
	}
	```






















































































































