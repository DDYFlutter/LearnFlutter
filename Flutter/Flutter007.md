本文对应github地址[Flutter7](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter007.md),如果由于github调整导致资源找不到，请访问[github](https://github.com/DDYFlutter/LearnFlutter)


> ### 原生

```
// 不建议用原生方法请求
_testNativeNetRequest() async {
  var url = 'https://raw.githubusercontent.com/DDYFlutter/flutter_test/master/interface.js';
  var httpClient = new HttpClient();
  try {
    var request = await httpClient.getUrl(Uri.parse(url));
    var response = await request.close();
    // 原生库禁掉了OK
    if (response.statusCode == 200) {
      print('原生 httpClient success ${await response.transform(Utf8Decoder()).join()}');
    } else {
      print('原生 httpClient Error: ${response.statusCode}');
    }
  } catch(exception) {
    print('原生 httpClient Error: ${exception}');
  }
}
```

> ### http

```
// 测试HTTP https://pub.dartlang.org/packages/http
_testLibraryHttp() async {
  var url = 'https://raw.githubusercontent.com/DDYFlutter/flutter_test/master/interface.js';
  var client = new http.Client();
  try {
    http.Response response = await client.get(url);
    if (response.statusCode == 200) {
      print('http success ${response.body.toString()}');
    } else {
      print('http Error: ${response.statusCode}');
    }
  } catch(exception) {
    print('http Error: ${exception}');
  }
}
```


> ### dio

```
// 测试dio https://pub.dartlang.org/packages/dio
_testDio() async {
  try {
    var url = 'https://raw.githubusercontent.com/DDYFlutter/flutter_test/master/interface.js';
    var dio = new Dio();
    var response = await dio.get(url);
    if (response.statusCode == 200) {
      print('dio success ${response.data.toString()}');
    } else {
      print('dio Error: ${response.statusCode}');
    }
  } catch(exception) {
    print('dio Error: ${exception}');
  }
}
```





> ### 参考文章

* [Flutter中好用的网络操作](http://www.flutterj.com/?post=98)
* [强大的Flutter Http请求开源库-dio](https://www.jianshu.com/p/bd4c2dc5e97f)



	
[上一页 Flutter6 包、异常、异步和库](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter006.md)    
[下一页 Flutter8 widget](https://github.com/DDYFlutter/LearnFlutter/blob/master/Flutter/Flutter008.md)