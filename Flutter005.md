本文对应github地址[Flutter5](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter005.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> ### 抽象类 abstract class

* 抽象类不能实例化，可以当做抽象类来 extends 也可以当做接口来 implements，dart 中没有 interface 这个关键字，接口也是抽象类实现的

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

> ### 接口

* 接口也是 abstrack 关键字声明的，并且需要使用 implements 关键字来实现




