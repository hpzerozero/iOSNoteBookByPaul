# 使用Safari查看webview内的网页源文件  
```  
    在很多时候做JS和iOS进行交互的时候，想要去观察webview内的HTML源码，就像我们使用浏览器的开发者工具查看运行在网页上的内容，这时候强大的Apple给了开发者一个利器，简单而又强大。
```  
下面我们下模拟器下的效果图（真机也是可以的，需要另做处理，下面会介绍的）

```
// 此处使用WKWebview,UIWebview也是一样的
- (void)viewDidLoad {
    [super viewDidLoad];
    WKWebViewConfiguration * config = [[WKWebViewConfiguration alloc] init];
    
    self.wkWebView  = [[WKWebView alloc] initWithFrame:self.view.frame configuration:config];
    NSURL *url = [NSURL URLWithString:@"http://c.b1wt.com/h.SeL1Fn?cv=91mTtAqb8W&sm=e6cf6e"];
    [self.wkWebView loadRequest:[NSURLRequest requestWithURL:url]];
    [self.view addSubview:self.wkWebView];
}
```

![image description](http://upload-images.jianshu.io/upload_images/2030645-4b82c7ffdeabda85.gif?imageMogr2/auto-orient/strip)  

做法：
    xcode使用模拟器运行代码，运行起来后，打开电脑上的Safari，找到窗口顶部的`开发`选项，如果找不到，进入`偏好设置`  
    ![打开开发选项](http://upload-images.jianshu.io/upload_images/2030645-5bb33f2be4912f1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
![image description](http://upload-images.jianshu.io/upload_images/2030645-498801bec29d3900.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
![image description](http://upload-images.jianshu.io/upload_images/2030645-9f1c8cec63998ef7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

如果此时模拟器已经运行到含有webView的部分，这时候点开开发者选项就会看到`Simulator`，进入就可以看到当前webview当前正在加载的网址，选一个进入就会看到如下需要的信息。  


![image description](http://upload-images.jianshu.io/upload_images/2030645-83a1604c243b20ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

![image description](http://upload-images.jianshu.io/upload_images/2030645-9c44c370b465a18a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
我们可以看到很轻松地看到运行在模拟器上的网页内容，不仅可以看到，如果浏览器支持实时修改HTML的内容和CSS样式，那么反映在模拟器上的也会同步修改，是不是很方便啊

但是有人说模拟器上运行不厉害，毕竟模拟器和Safari都是在Mac上运行的，可以监测到webview内容很容易，那如果Mac能监测真机上的webview运行是不是很屌，我一脸崇拜😱。

将手机通过数据线与Mac相连，使用xcode选择真机运行代码，然后在Safari开发中选择真机的名字，而不是`simulator`,其余进行同样的操作就可以实时查看手机上运行的网址的源码了👯👯👯👯  。  

如果Safari上没有显示手机的名称，打开手机的`设置`，在设置的首页找到`Safari`选项，进入后，在最下面有个`高级`的选项进入后，打开`Web检查器`，然后再运行就会看到了。

祝各位开发者工作顺利，玩得开心！🤑🤑🤑🤑🤑🤑🤑




