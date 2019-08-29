本文对应github地址[Flutter4](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter004.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> 函数

* 函数包含返回值(可省略书写，依靠推断类型，无返回值则void)、函数名、参数组成
	
	```
	// 常规书写
	String fullName1(String firstName, String lastName) {
		return "$firstName $lastName";
	}
	
	// 省略返回类型 箭头函数
	fullName2(String firstName, String lastName) => "$firstName $lastName";
	
	// 参数还可以是var Object dynamic类型，通过实际赋值类型推导
	fullName3(var firstName, Object lastName) => "$firstName $lastName";
	
	// 调用并打印返回值   
	print(fullName1("Li", "Bai")); // Li Bai
	print(fullName2("Li", "Bai")); // Li Bai
	print(fullName3("Li", "Bai")); // Li Bai
	```

* 命名参数（大括号{}包含）

	```
	// 命名参数 {}包装(从尾部参数开始)，调用时也得指定参数名
  	fullName4(var firstName, {Object lastName}) => "$firstName $lastName";
	
	// 调用并打印返回值 
	print(fullName4("Li", lastName: "Bai")); // Li Bai
	```
* 可选参数(中括号[]包装，命名参数是特殊可选参数用大括号{]包装) 

	```
	// 可选参数 {}或[]包装都可选(从尾部参数开始) 没有默认值且不赋值则为null
	fullName5(var firstName, [Object lastName]) => "$firstName $lastName";
	
	// 调用并打印返回值 
	print(fullName5("Li"));// Li null
	```
	
* 参数默认值(没有默认值且不赋值则为null)

	```
	// 参数默认值 没有默认值且不赋值则为null
  	fullName6([var firstName, Object lastName = "Lei"]) => "$firstName $lastName";
	
	// 调用并打印返回值
	print(fullName6());// null Lei
	```

* 函数作参数(函数一等公民)

	```
	// 函数作参数(函数一等公民)
  	outerUse(String str, void innerPrint(String msg)) => innerPrint(str);
  	innerUse(String msg) => print("$msg ${msg.length}");
  	
  	// 调用
  	outerUse("print param", innerUse);
	```
* 匿名函数

	```
	// 匿名函数
  	anonymousUse() {
   		outerUse2(String str, void innerPrint(String msg)) => innerPrint(str);
    	var funcParam = (msg) => print("$msg ${msg.length}");
    	outerUse2("anonymousUse0", funcParam); // 写成变量然后作参数
    	outerUse2("anonymousUse1", (msg) => print("$msg ${msg.length}")); // 直接当参数
    	// 不用箭头函数
    	outerUse2("anonymousUse2", (msg){
    		print("$msg ${msg.length}");
    	});
  	}
	```



* 立即执行函数(自执行函数 即定义和调用合为一体)

	```
	// 创建的匿名函数立即执行，外部无法引用它的内部变量，因此执行完很快被释放，这种机制不会污染全局对象。
	useNowFunction() {
		// (function (/* 参数 */) { /* code */ }(/* 参数 */)) // 推荐
		// (function (/* 参数 */) { /* code */ })(/* 参数 */) // 也能
		((x, y){
			print("${x+y}");
		}(2, 3));
	
		((x, y){
			print("${x+y}");
		})(2, 3);
	
		((x, y) => print("${x+y}"))(2, 3);
	}	
	```

* 闭包(闭包是函数对象)

	```
	// num 类型是int double的父类， Function是返回类型(可省略，根据返回值推导类型)
	Function addClosure1(int addFrom) {
	  return (num addNum) {
	    return addFrom + addNum;
	  };
	}
	addClosure2(int addFrom) {
	  return (num addNum) => addFrom + addNum;
	}
	
	var addMethod = addClosure1(10);
	print(addMethod(3)); // 13
	print(addMethod(4)); // 17
	print(addClosure2(10)(3)); // 13
	```

* 关于重载

	Dart不支持方法重载(重载就是方法名相同，参数（个数或类型）不同（称之为签名不同）)但支持命名构造函数


```
```










































































































