本文对应github地址[Flutter5](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter005.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> ### 类和抽象类 

* 抽象类(abstract class)不能实例化
* 可以当做抽象类来 extends 
* 抽象类也可以当做接口来 implements
* dart中没有interface关键字，接口是用抽象类实现的
* 如果一个类同时有继承、混入、接口实现，那么extens在前，with在中间，implements最后

> #### 继承(Inheritance) extends

* Dart的继承是单继承
* 构造函数不能继承
* 子类重写超类的方法，要用@override
* 子类调用超类的方法，要用super
* Dart中的子类可以访问父类中的所有变量和方法(只有下划线开头私有)
* 抽象类可以被继承，然后实现其抽象方法

	```
	abstract class Animal {
	  eat(); // 未实现，抽象方法，子类必须实现
	  printInfo() { // 已经实现，子类不必须实现
	    print("Animal printInfo");
	  }
	}
	
	class Cat extends Animal {
	  @override
	  eat() {
	    print("Cat eat fish");
	  }
	}
	
	class Dog extends Animal {
	
	  @override
	  eat() {
	    print('Dog eat bone');
	  }
	}
	
	// 抽象类测试
	testAbstractClass() {
	  Dog dog = new Dog();
	  Cat cat = new Cat();
	
	  dog.eat(); // Dog eat bone
	  cat.eat(); // Cat eat fish
	
	  dog.printInfo(); // Animal printInfo
	  cat.printInfo(); // Animal printInfo
	}
	```

> ### 接口实现 implements

* 接口也是 abstract 关键字声明的，并且需要使用 implements 关键字来实现

	```
	// 抽象类接口
	abstract class PersonDataBase {
	  String userName;
	  String passWord;
	
	  getUserNameWithPassWord(String passWord);
	  resetPassWordOfUserName(String userName);
	}
	
	// implements 实现抽象类接口
	class StudentDataBase implements PersonDataBase {
	  @override
	  String passWord;
	
	  @override
	  String userName;
	
	  @override
	  getUserNameWithPassWord(String newPassWord) {
	    if (newPassWord == passWord) {
	      print("${userName}");
	    } else {
	      print("PassWord Error");
	    }
	  }
	
	  @override
	  resetPassWordOfUserName(String userName) {
	    if (userName == this.userName) {
	      this.passWord = "admin";
	      print("reset success");
	    } else {
	      print("userName error");
	    }
	  }
	
	  StudentDataBase(this.userName, [this.passWord = "admin"]) {
	    print("userName: ${this.userName} passWord: ${this.passWord}");
	  }
	}
	
	// 接口实现测试
	testImplements() {
	  StudentDataBase studentDB = new StudentDataBase("LiBai", "10086"); // userName: LiBai passWord: 10086
	  studentDB.getUserNameWithPassWord("admin"); // PassWord Error
	  studentDB.getUserNameWithPassWord("10086"); // LiBai
	  studentDB.resetPassWordOfUserName("LiLei"); // userName error
	  studentDB.resetPassWordOfUserName("LiBai"); // reset success
	  studentDB.getUserNameWithPassWord("10086"); // PassWord Error
	  studentDB.getUserNameWithPassWord("admin"); // LiBai
	}
	```

> ### 多接口

* 多个接口用逗号,分隔

	```
	abstract class Person {
	  sleep();
	}
	
	abstract class Student {
	  study();
	}
	
	class Pupil implements Person, Student {
	  @override
	  sleep() {
	    print("Pupil also need sleep");
	  }
	
	  @override
	  study() {
	    print("Pupil also need study");
	  }
	}
	
	// 多接口测试
	testSomeImplements() {
	  Pupil pupils = Pupil();
	  pupils.sleep(); // Pupil also need sleep
	  pupils.study(); // Pupil also need study
	}
	```

> 混入 mixin
 
* mixins类只能继承自object
* mixins类不能有构造函数
* 一个类可以mixins多个mixins类
* 可以mixins多个类，不破坏Flutter的单继承

	```
	// 混入 mixin
	class Plant {
	  info() {
	    print("Plant don't move");
	  }
	}
	
	abstract class Tree {
	  leaf();
	  root() {
	    print("Tree has the root");
	  }
	}
	
	class AppleTree extends Object with Plant, Tree {
	  @override
	  leaf() {
	    print("AppleTree has leaves");
	  }
	}
	
	// 混入(mixin)测试
	testMixinWith() {
	  AppleTree appleTree = new AppleTree();
	  appleTree.info(); // Plant don't move
	  appleTree.root(); // Tree has the root
	  appleTree.leaf(); // AppleTree has leaves
	}
	```

> ### 范型

* 范型方法

	```
	T getData<T> (T val) {
	  return val;
	}
	
	getData<String>('123');
	getData<int>(123);
	// getData<int>(123.4); // 报错，类型不匹配
	```
*  泛型类

	1. 声明泛型类,如Array类，实际是List的别名，而List本身也支持泛型的实现

	```
	class Array<T> {
	  List _list = new List<T>();
	  Array();
	  void add<T>(T value) {
	    this._list.add(value);
	  }
	  get value {
	    return this._list;
	  }
	}
	```
	2. 使用泛型类

	```
	List l1 = new List<String>();
	// l1.add(12); // 类型不匹配
	l1.add('asd');
	```

* 泛型接口

	```
	// 范型接口
	abstract class Storage<T> {
	  Map mapIn = new Map();
	  void set(String key, T value);
	  T get(String key);
	}
	
	class Cache<T> implements Storage<T> {
	  @override
	  Map mapIn = new Map();
	
	  @override
	  T get(String key) {
	    return mapIn[key];
	  }
	
	  @override
	  void set(String key, T value) {
	    mapIn[key] = value;
	    print("success");
	  }
	}
	
	// 测试范型接口
	testGeneric() {
	  Cache cacheTest = new Cache<String>();
	  cacheTest.set("name", "LiXiaolong");
	  print("${cacheTest.get("name")}");
	}
	```
	
[上一页 Flutter4 函数和范型基础](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter004.md)	