flutter 中单例模式的创建

```
class Singleton {
  /// 单例
  // 工厂模式
  factory Singleton() => _getInstance();
  static Singleton get instance => _getInstance();
  static Singleton _instance;
  static Singleton _getInstance() {
    if (_instance == null) {
      _instance = Singleton._internalInit();
    }
    return _instance;
  }
  // 私有自定义命名构造方法[子类不能继承 _internalInit (非关键字，可以更改其它名字)]
  Singleton._internalInit() {
    // 初始化
  }
}
```

简写点

```
class Singleton {
  static final Singleton _singleton = Singleton._internal();
  // 工厂构造函数
  factory Singleton() {
    return _singleton;
  }
  // 构造函数私有化，防止被误创建
  Singleton._internal(){ 
  	 // 初始化 
  }
}
```

使用

```
main() {
  var s1 = Singleton();
  var s2 = Singleton();
  print(identical(s1, s2));  // 相等
  print(s1 == s2);           // 相同对象
}
```