#iOS中如何使用多个Target去管理你的项目环境版本（测试环境与线上环境）

详见文章：
[iOS中如何使用多个Target去管理你的项目环境版本（测试环境与线上环境）](http://www.jianshu.com/p/23cc84d40423)

###前言
在实际开发中，为了保证所开发的产品能够尽量完美上线，在上线前会特意测试几轮，保证所开发的应用没有问题。但是又能保证测试的一些垃圾数据不影响线上的版本数据，此时就需要区分生产环境了，一般在测试的时候会专门为测试而准备一个测试环境，而上线的时候将测试环境改成对应的线上环境以达到上线目的。

在进行环境切换的时候，最简单的方法就是修改全局的公共接口，这样做在环境切换上确实能够满足需求，但是，如果此时测试人员要求你在产品的图标上面也有所区分，例如App的icon、启动图等，当线上的图标与测试环境的图标不一致的时候，就变得略显麻烦了，因为你每次不仅要切换接口，还要去来回的更换环境的图标。

除了上述情况之外，有一些App还分为专业版与普通版，而专业版与普通版的区别在于一些功能的有无，对于这样的需求，难道要专门去独立出来两个项目吗？如果要是专门去独立出来两个项目，那以后迭代的话，两个项目都得同时去迭代，工作量是如此浩大，而单一的去copy也不是设计中的一个好的方法。

所以为了解决这样的问题，我们可以通过使用今天所提到的方法，使用多个Target进行项目的版本管理（测试版与线上版本等）。

###定义
在使用它之前，我们先看一下苹果官方文档是如何阐述Target的，如下：
```
A target specifies a product to build and contains the instructions for 
building the product from a set of files in a project or workspace. A 
target defines a single product; it organizes the inputs into the build 
system—the source files and instructions for processing those source 
files—required to build that product. Projects can contain one or more 
targets, each of which produces one product.
```
含义也很简单，它是一个项目环境的设置文件，一个Target定义了一个单一项目环境，在一个项目工程中可以包含一个或者多个Target。也就是说一个项目中可以设置多种环境。

###使用
其实使用起来还是很方便的，在使用之前要说明一下，创建Target的方式有两种：

- 直接copy之前项目中的Target配置；
- 创建新的Target配置；
接下来，按照步骤来即可。

- 步骤一：创建Target
在工程中对已存在的target进行复制，点击`Duplicate`即可
![创建Target](http://upload-images.jianshu.io/upload_images/1611317-675c3b53aba3d010.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    创建Target
![选择](http://upload-images.jianshu.io/upload_images/1611317-fdac39b2216fad00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    选择

![创建成功](http://upload-images.jianshu.io/upload_images/1611317-11b6dca388221dda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果你想创建一个新的Target，可以使用下面的方法：


![新建一个Target](http://upload-images.jianshu.io/upload_images/1611317-4d4667a811b75826.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![选择需要的模板配置](http://upload-images.jianshu.io/upload_images/1611317-0e4d9d92c738b1a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 步骤二：更改Target的名称
创建完Target以后你会发现名字后面有个copy的字样，顿时觉得业余的不行有没有？此时我们可以通过下面的方法进行名字的修改。


![配置Target的名字](http://upload-images.jianshu.io/upload_images/1611317-ecb7b9243fc3f469.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![修改名字](http://upload-images.jianshu.io/upload_images/1611317-f23e047a459c75ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 步骤三：添加不同Target下的应用图标
为了满足项目中不同环境下的图标的更换需求，我们可以使用这种方法来进行。


![创建测试图标](http://upload-images.jianshu.io/upload_images/1611317-60d7bf7d8faeb3c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![创建测试图标](http://upload-images.jianshu.io/upload_images/1611317-fd7f4bf071a6faa0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![选择对应的Target](http://upload-images.jianshu.io/upload_images/1611317-8b08872e5bbb7cb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![在对应的Target中选取对应的应用图标](http://upload-images.jianshu.io/upload_images/1611317-ecfd518414f237fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 步骤四：配置全局宏，目的是在代码中进行环境的区分

![OC中添加对应的全局宏，以便代码中版本的区分](http://upload-images.jianshu.io/upload_images/1611317-8966cb09959e3514.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![OC中测试环境添加宏](http://upload-images.jianshu.io/upload_images/1611317-d9c47ef16f29c7ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而在OC中只需要使用以下方法进行环境的区分
```
#import "ViewController.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    UIView *view1 = [[UIView alloc]initWithFrame:CGRectMake(100, 100, 150, 150)];
    UIView *view2 = [[UIView alloc]initWithFrame:CGRectMake(100, 100, 150, 150)];

    view1.backgroundColor = [UIColor blackColor];
    view2.backgroundColor = [UIColor yellowColor]

#if TARGET_VERSION == 1

    [self.view addSubview:view1];   
#else

    [self.view addSubview:view2];
#endif


}
```

Swift中的测试环境下的配置
![在Swift中使用如下的方法去区分对应的环境](http://upload-images.jianshu.io/upload_images/1611317-0fe7fa9fe6948a38.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.

        let view1 = UIView.init(frame: .init(x: 100, y: 100, width: 150, height: 150));
        let view2 = UIView.init(frame: .init(x: 100, y: 100, width: 150, height: 150));

        view1.backgroundColor = UIColor.black;
        view2.backgroundColor = UIColor.yellow;

        #if DEVELOPMENT
            self.view.addSubview(view1);
        #else
            self.view.addSubview(view2);

        #endif
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }


}
```
对应的通过编译不同的Target，我们也就得到了不同环境下的App了，如下所示：


![App的icon1](http://upload-images.jianshu.io/upload_images/1611317-6ed4bff93db460a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![App的icon2](http://upload-images.jianshu.io/upload_images/1611317-de4c1cb51174761f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###扩展（对于cocopads的使用）
相信很多时候我们的项目离不开pods的框架管理，在使用cocopods管理的时候，我们不要忘了将这些框架添加到对应的Target中，否则，可能使用的时候找不到对应的框架，对于pod的使用，可以参考以下代码进行构建：
```
platform :ios, '7.0'
workspace 'TestTargetDemo'
link_with 'TestTargetDemo', 'TestTargetDemoDev'
pod 'SDWebImage'
pod 'AFNetworking'
```