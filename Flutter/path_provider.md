#### path_provider

[path_provider](https://pub.flutter-io.cn/packages/path_provider)用于获取文件目录，支持多平台，但并非所有方法都适用于所有平台。

> ##### 使用

1. 在 pubspec.yaml 文件中添加依赖

```
dependencies:
  path_provider: ^1.6.22
```

2. 执行获取安装

```
flutter pub get
```

3. 在要使用的文件中导入

```
import 'package:path_provider/path_provider.dart';
```

> ##### 详解

* 获取临时文件路径[iOS Android]

```
// 获取不会备份并且随时会被删除的临时目录(IOS和安卓通用)
getTemporaryDirectory().then((value) => print(value));
// iOS: NSTemporaryDirectory()
// Android: getCacheDir()
```

或

```
Directory tempDir = await getTemporaryDirectory();
String tempPath = tempDir.path;
```

* 获取应用支持目录[iOS Android]

```
// 用于存储应用支持的目录 这个目录对于用户是不可见的 (IOS和安卓通用)
getApplicationSupportDirectory().then((value) => print(value));
```

* 获取应用文档目录[iOS Android]

```
// 仅该应用可以访问，删除应用时才清除 用户不可见(IOS和安卓通用)
getApplicationDocumentsDirectory().then((value) => print(value));
iOS: NSDocumentsDirectory目录
Android: AppData目录
```
* 获取应用持久存储目录路径[iOS]

```
// 应用程序可以存储持久化、备份和用户不可见的文件的目录路径
getLibraryDirectory().then((value) => print(value));
```

* 获取外部存储目录[Android]

```
// 获取外部存储目录 用户可见
getExternalStorageDirectory().then((value) => print(value));
```

* 获取外部存储目录列表[Android]

```
// 可以存储应用程序特定数据的目录 
// 这些路径通常驻留在外部存储上 用户可见 如单独的分区或SD卡(可以有多个 所以是列表)
getExternalStorageDirectories().then((value) => print(value));
```

* 获取外部缓存目录[Android]

```
// 可以存储应用程序特定外部存储数据的目录 
// 这些路径通常驻留在外部存储上，如单独的分区或SD卡(可以有多个 所以是列表)
getExternalCacheDirectories().then((value) => print(value));
```

* 获取下载目录[桌面系统(iOS与Android使用会报错)]

```
// 获取下载路径 
getDownloadsDirectory().then((value) => print(value));
```


#### 文件操作

插件的内容较少，使用也比较简单，仅仅只是用于获取路径，并没有操作文件和目录的功能，因此，需要搭配Director和File等进行操作(需要 import 'dart:io';)

> ##### Directory

* 实例

```
testDirectory() async {
    // 获取系统文件目录
    Directory dir1 = await getTemporaryDirectory();
    // 根据路径字符串创建目录对象
    Directory dir2 = Directory('assets\\files');
    // 根据uri对象创建目录对象
    Directory dir3 = Directory.fromUri(Uri(path: 'assets'));
    // 根据Uint8List路径创建实例
    Directory dir4 = Directory.fromRawPath(Utf8Encoder().convert('assets\\files'));

    /// 属性
    // 获取目录路径
    print(dir1.path);
    // 获取目录位置的uri
    print(dir1.uri);
    // 父目录Directory对象
    print(dir1.parent);
    // 返回一个绝对路径的Directory对象
    print(dir1.absolute);
    // 判断是否绝对路径
    print(dir1.isAbsolute);

    /// 方法
    // 根据路径创建目录[recursive: 是否递归创建(ture时路径中有目录不存在则递归创建出)]
    Directory createdDir = await dir1.create(); // dir1.createSync(recursive: true); 同步创建

    // 在此目录下创建一个临时目录[随机字符串附加到prefix以产生唯一路径名]
    dir1.createTemp(); // dir1.createTempSync('myPrefix');

    // 列出该目录下的子目录和文件[recursive:是否递归列出子孙目录和文件，followLinks:是否允许link]
    dir1.list().toList().then((value) => print(value)); // dir1.listSync(recursive: true, followLinks: false);

    // 重命名目录[同一父目录时仅仅重命名，否则成移动文件夹]
    dir1.rename('newPath'); // dir1.renameSync('newPath');

    // 目录是否存在
    dir1.exists(); // dir1.existsSync();
    // 解析文件系统对象相对于当前工作目录的路径[解析路径上所有符号链接，并解析..和. 路径段，返回一个类似绝对路径的字符串]
    await dir1.absolute.resolveSymbolicLinks().then((value) => print(value)); // resolveSymbolicLinksSync()
    // 删除文件目录
    await dir1.delete(recursive: true).then((value) => print(value)).catchError((error) => print(error)); // deleteSync()

    // 查看目录信息
    await dir1.stat().then((value) => print(value)); // statSync();
    // type directory                     类型：目录
    // changed 2020-05-29 11:14:50.000    文件系统对象的数据或元数据的最后更改时间
    // modified 2020-05-29 11:15:01.000   最后一次更改文件系统对象的数据的时间
    // accessed 2020-05-29 11:15:01.000   上次访问文件系统对象的数据的时间
    // mode rwxrwxrwx                     文件系统对象的模式 r(read) w(write) x(execute) 读写执行
    // size 0                             文件大小
}
```




> ##### File

* 实例

```
testFile() async {
    // 根据文件路径字符串创建文件实例
    File file = File('assets/myFile.txt');
    // 根据uri对象创建文件实例
    File file2 = File.fromUri(Uri(path: 'assets/myFile.txt'));
    // 根据Uint8List文件路径创建实例
    File file3 = File.fromRawPath(Utf8Encoder().convert('assets\\file\\myfile.txt'));

    /// 属性
    // 返回绝对路径
    print(file.absolute);
    // 获取文件路径
    print(file.path);
    // 是否绝对路径
    print(file.isAbsolute);
    // 获取父目录Directory对象
    print(file.parent);
    // 获取uri
    print(file.uri);

    /// 方法
    // 复制文件[新路径必须存在，否则报错，若新路径下有同名文件则覆盖]
    await file.copy('assets/newPath/newFileName.txt');
    // 创建文件[recursive:true 若路径中目录不存在则递归创建出目录，false时若路径中目录不存在则把报错]
    await file.create(recursive: true);
    // 获取文件上次访问时间[2020-05-30 10:39:35.000]
    await file.lastAccessed();
    // 修改上次访问时间
    await file.setLastAccessed(new DateTime.now());
    // 获取文件最后修改时间
    await file.lastModified();
    // 修改文件最后修改时间
    await file.setLastModified(new DateTime.now());
    // 获取文件长度(字节数)
    await file.length();
    // 重命名[同一父目录时仅仅重命名，否则成移动文件]
    file.rename('assets/newName.txt');
    // 以字符串形式读取文件内容[encoding默认utf8]
    print(await file.readAsString());
    // 以字节形式读取
    print(await file.readAsBytes());
    // 按行读取[encoding默认utf8]
    print(await file.readAsLines());
    // 以字符串形式写入
    file.writeAsString(
      'contents',
      mode: FileMode.writeOnlyAppend, // 写入模式 默认write(读写) read(只读) writeOnly(只写) append(追加) writeOnlyAppend(只追加)
      flush: true, // 默认false 为true时写入的数据将在返回之前刷新到文件系统
      encoding: utf8 // 编码
    );
    // 以字节数组写入
    file.writeAsBytes([1, 2, 3]);
    // 检查文件是否存在
    file.exists();
    // 解析文件系统对象相对于当前工作目录的路径
    file.resolveSymbolicLinks();
    // 查看文件详细信息
    file.stat();
    // type(类型)
    // changed(文件系统对象的数据的最后修改时间)
    // modified(最后一次更改文件系统对象的数据的时间)
    // accessed(上次访问时间)
    // size(文件大小)

    // 为文件创建一个Stream(流) start: end:
    file.openRead();
    // file.openWrite();
}
```

> ##### path_provider Directory File综合运用

```
Future<int> writeStringToFile(int input) async {
    // 获取目录
    Directory dir = await getTemporaryDirectory();
    // 文件路径
    String path = '${dir.path}/tempFile.txt';
    // 文件对象
    File file = File(path);
    // 写入操作
    file.writeAsString('$input');
    // 读取操作
    var content = await file.readAsString();
    // 解析转型
    try {
      return int.parse(content);
    } catch (error) {
      return -1;
    }
}
```