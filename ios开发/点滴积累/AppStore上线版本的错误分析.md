废话不多说，看下面的crash信息
![QQ20170810-095110@2x.png](http://upload-images.jianshu.io/upload_images/2030645-bc26163304d5be9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

找到显示错误信息版本对应的archive包路径，`xcode-window-organizer`，点击打的包，右键`show in finder`,找到.xcarchive,显示包内容，找到`项目名.app.dSYM`，显示包内容，找到`DWARF`文件夹，打开终端，`cd 你的DWARF文件夹所在的绝对路径`：
```
MacBook-Pro:~ hp$ cd /Users/hello/Library/Developer/Xcode/Archives/2017-08-04/项目名\ 2017-8-4\ 09.32.xcarchive/dSYMs/项目名.app.dSYM/Contents/Resources/DWARF
```
然后使用命令`atos -arch (CPU type) -o 崩溃名 崩溃地址`，分析图中的错误命令如下：
```
MacBook-Pro:DWARF hp$ atos -arch arm64 -o xxxx(图中马赛克部分) 0x100090d74
```
解析后的结果如下，会显示错误的方法，出现错误的类，以及定位到代码的哪一行
```
-[HCPLongImageView didSelectedItemsWithType:] (in xxxx) (HCPLongImageView.m:95)
 ```

如有更好的办法可以互相交流，感谢阅读