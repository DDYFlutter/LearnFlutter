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

* 命名参数（大括号{}包含, 这样的函数又叫具名函数，可选择名称的函数）

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
  	outerUse("print param", innerUse); // print param 11
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
		// (function () { }()) // 推荐
		// (function () { })() // 也能
		
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
	print(addMethod(4)); // 14
	print(addClosure2(10)(3)); // 13
	```

* 高级函数(参数不指定类型，默认Object)

	```
	// 函数可作为对象赋值给变量，也可作参数
	printElement(element) => print(element);
	var list = ['LiLei', 'HanMeimei', 'John'];
	list.forEach(printElement);
	```

* 命名构造函数

	Dart不支持方法重载(重载就是方法名相同，参数（个数或类型）不同（称之为签名不同）)但支持命名构造函数


	```
	// 测试类 三个命名构造函数
	class TestNameInitFunc {
	  int x;
	  int y;
	  TestNameInitFunc(){} // 不用分号
	
	  TestNameInitFunc.X(int x) {
	    this.x = x;
	  }
	  TestNameInitFunc.Y(int y) {
	    this.y = y;
	  }
	  TestNameInitFunc.XY(int x, int y) {
	    this.x = x;
	    this.y = y;
	  }
	  
	  TestNameInitFunc.X2(this.x);
	  TestNameInitFunc.Y2(this.y);
	  TestNameInitFunc.XY2(this.x, [this.y = 20]);
	
	  printParam() {
	    if (x != null && y != null) {
	      print("x:${x.toString()} y:${y.toString()}");
	    } else if (x != null) {
	      print("x:${x.toString()}");
	    } else if(y != null) {
	      print("y:${y.toString()}");
	    }
	  }
	}
	
	// 应用
	testNameInit() {
		TestNameInitFunc testX = new TestNameInitFunc.X(1);
		TestNameInitFunc testY = TestNameInitFunc.Y(2);
		var testXY = TestNameInitFunc.XY(3, 4);
		testX.printParam(); // x:1
		testY.printParam(); // y:2
		testXY.printParam(); // x:3 y:4
		
		var testX2 = TestNameInitFunc.X2(11);
		var testY2 = TestNameInitFunc.Y2(12);
		var testXY2 = TestNameInitFunc.XY2(13);
		testX2.printParam(); // x:11
		testY2.printParam(); // y:12
		testXY2.printParam(); // x:13 y:14
	}
	```

* 静态方法

	```
	class TestStaticFunction {
	  static printParam() {
	    print("10086");
	  }
	}
	
	// 调用
	TestStaticFunction.printParam(); // 10086
	```

* 单例

	```
	class Singleton {
	  /// 单例对象
	  static Singleton _instance;
	
	  /// 内部构造方法，可避免外部暴露构造函数，进行实例化
	  Singleton._internal();
	
	  /// 工厂构造方法，这里使用命名构造函数方式进行声明
	  factory Singleton.getInstance() => _getInstance();
	
	  /// 获取单例内部方法
	  static _getInstance() {
	    // 只能有一个实例
	    if (_instance == null) {
	      _instance = Singleton._internal();
	    }
	    return _instance;
	  }
	}	
	
	// 调用
	Singleton singleton  = Singleton. getInstance();
	```

* Getter/Setter

	```
	class TestGetSet {
	  // 私有
	  String _userName; 
	  String _nickName;
	
	  String get userName {
	    return _userName ?? _nickName;
	  }
	  set userName(String newName) {
	    _userName = newName;
	  }
	  set nickName(String newName) {
	    _nickName = newName;
	  }
	}
	
	// 使用
	var testGetSet = new TestGetSet();
	testGetSet.nickName = "LiXiaolong";
	print("${testGetSet.userName}"); // LiXiaolong
	```
* 重定向构造函数
	
	```
	class Point {
	  num x, y;
	 
	  // 主构造函数
	  Point(this.x, this.y);
	 
	  // 重定向构造函数
	  Point.alongXAxis(num x) : this(x, 0);
	}
	```


* 简写构造函数	 

    ```
    class Person {
    	String userName;
    	String passWord;
    	int age;
    	// 直接用类名加参数方式简写构造函数
    	Person(this. userName, this.age, [this. passWord = 'admin']);
    }
    ```	
* noSuchMethod()

1. 重载noSuchMethod()可以在对象调用不存在的方法或者变量时调用。
2. 你不能调用一个不存在的方法，除非是以下情况的一种：
    + 一个静态类型的dynamic的变量
    + 接收方有一个定义未实现的方法的静态类型(抽象的也可以)，并且接收方是dynamic并且有一个noSuchMethod()的实现，与类Object中的实现不同。

    ```
    @override
    void noSuchMethod(Invocation invocation) {
        print('You tried to use a non-existent member: ' +
            '${invocation.memberName}');
    }
    ```    

> ### 范型

* 泛型就是解决 类 接口 方法的复用性、以及对不特定数据类型的支持(类型校验)
* 范型可用在参数，返回值，局部变量等处

	1. 类型安全

	尖括号指定数据类型实际是范型的一种特殊应用
	
	```
	var userNames = List<String>();
	userNames.addAll(['Seth', 'Kathy', 'Lars']);
	userNames.add(42); // 类型不匹配，报错
	```

	2. 减少重复的代码

	```
	// 不同类型写同样功能的不同方法
	abstract class ObjectCache {
	  Object getByKey(String key);
	  void setByKey(String key, Object value);
	}
	
	abstract class StringCache {
	  String getByKey(String key);
	  void setByKey(String key, String value);
	}
	
	// 用范型归纳成一个方法
	abstract class Cache<T> {
	  T getByKey(String key);
	  void setByKey(String key, T value);
	}
	```
	 
> ### 运算符重载    
    
* 运算符重载(operator overloading)是多态的一种。运算符重载通常只是一种语法糖，这种语法对语言的功能没有影响，但是更方便程序员使用。让程序更加简洁，有更高的可读性 
   
    ```
    class TestOperator {
      startTest() {
        Cat cat1 = new Cat("10");
        String resultNum = cat1 + "20";
        print(resultNum);
      }
    }
    
    class Cat {
      String hadAteFishNum;
    
      Cat(this.hadAteFishNum);
    
      String operator +(String eatingFishNum) {
        int resultNum = int.parse(this.hadAteFishNum) + int.parse(eatingFishNum);
        return resultNum.toString();
      }
    }
    ```    
    
[上一页 Flutter3 基本类型](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter003.md)   
[下一页 Flutter5 类、范型和控制流](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter005.md)