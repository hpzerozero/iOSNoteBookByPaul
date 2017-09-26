# Deep Link是什么

以下
转**[Deep Link是什么](http://www.jianshu.com/p/964f871dc716)
**

今天在看Google关于Android Studio 2.0的视频的时候，提到了一个feature：支持Deep Link提示。笔者在去年上半年时候略微接触了一下，之后8月又看到全家桶出了一个山寨版叫AppLink。但是似乎在国内不太看到有这方面的介绍，在微博搜了一下，也没有正式介绍Deep Link的文章，所以产生了写本文的念头。

##Deep Link是什么

Deep Link，又叫deep linking，中文翻译作深层链接。全家桶搜索的话你会发现第一个结果是AppLink。呵呵。

说回正题。

简单地从用户体验来讲，Deep Link，就是可以让你在手机的浏览器/Google Search上点击搜索的结果，便能直接跳转到已安装的应用中的某一个页面的技术。如果你想体验的话，可以在Android 4.1以上设备安装IMDB，然后在Google上搜索一部IMDB的影片，你就会发现点击后直接跳转到了App里的该电影介绍页面。

当然在这简简单单的操作背后，却隐藏着很多工作。

先让我们看看现在移动端的搜索体验问题：

## 搜索

就因为我司没有做Web版的页面，所以搜索引擎就没法找到我们提供的内容。

搜索结果是不是可以做得更好呢。对于爬虫，在我们的印象中都是去爬网站的数据。但是现在作为一个巨大内容载体的移动平台却被忽略了，"似乎"只能自己提供一个H5版本去让搜索引擎爬数据索引？就像把自己的网站加入robots协议一样，app是不是也能直接这么做呢？

##排名

我的App这么火爆，为什么搜索出来的结果都是别人的引用？！

本科在学校时候做过PageRank的实践，简单来说就是一个带权的树形有向图，用通俗的话来讲，大V关注了你，可以让你的价值提升。而在App的世界里，我们也经常会体验到在应用之间跳转的体验（尽管有些应用时灵时不灵的），这种跳转难道不也能拿来作为PageRank的有向边吗？

做个例子来说（绝不是广告）：手Q、QQ空间、QQ音乐都在应用里的某页面引用了腾讯新闻的某一条新闻的页面，而手Q、QQ空间、QQ音乐这三个应用的该页面本身在算法里排名就很靠前，那么我们就认为腾讯新闻的该页面是有价值的，在相同结果的页面中应该排在更前面。

搜索引擎应该对移动端的app也支持排名和链接关系解析。

##移动化

每次在百度搜好吃的，搜到点评的结果后，怎么就不能直接跳到app里呢。

我们知道，现在从全家桶、Google搜索关于我们自己app的内容，往往只能搜到一些相关介绍和下载的链接，然后我们就中断了。而在Web世界，搜索后我们可以直接打开网页查看内容，相比起来体验实在是差了太多。难道我们就不能直接点击跳到手机上已经安装的app上吗？或者干脆直接跳到某个页面？

其实这种体验也是一种个性化搜索：个性化这个词比较宽泛，早期来说，搜索引擎会根据IP所在地区的不同返回有差别的结果。后来在引入账号系统后，会让用户可以设置语言和地域，恩...还有safe search，你懂的，会让我们看不到一些日文的内容。

![](http://upload-images.jianshu.io/upload_images/1557345-cd6a37b0a8bd9d1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
deeplink-2
![](http://upload-images.jianshu.io/upload_images/1557345-d02314e116da068b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
deeplink-3
而对移动端来说，个性化则是移动化，不同于返回网页，搜索引擎会返回支持Deep Link的应用内部页面的链接，比如我们找一部电影，可以直接跳到IMDB应用里这部电影的详情页面，体验是不是比看网页好多了呢（明明我安装了应用，为什么要让我看H5呢）。

## After Deep Linked

而Deep Link则会解决以上的问题，搜索引擎可以直接用爬虫检索App的内容，对App也列入PageRank排名，安装了App的设备点击后，则可以直接跳到应用内的对应页面！！Cool！

既然这么酷炫，那我们要怎么才能让自家的应用支持Deep Link呢？

## 使用

乍一看，Deep Link不就是scheme么？不错，或者我们该说，目前app的scheme，就是Deep Link的一种雏形（仅仅是跳转，且没有标准化的体验，见下文"首次点击自由"体验）。且有的app处理scheme并不是各个activity去注册自己的path，而会去通过一个中心activity去集成处理比如鉴权、解出各种参数，并美其名曰Navigator。

废话不多说了，看看正确的姿势吧。下文以Android接入为例，iOS可以查看App Indexing on iOS9

##支持Deeplink

参考内容：
[启用指向你app的Deep Linking。](http://search-codelabs.appspot.com/codelabs/android-deep-linking#1)
[为纯app内容创建索引](https://developers.google.com/app-indexing/app-only)

以Google给的demo为例：search-samples

我们需要添加Intent Filter到manifest：
```
<activity android:name="com.recipe_app.client.RecipeActivity"
          android:label="@string/app_name" >
    <intent-filter android:label="@string/app_name">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <!-- 接受以"http://recipe-app.com/recipe"开头的URI -->
        <data android:scheme="http"
              android:host="recipe-app.com"
              android:pathPrefix="/recipe" />
    </intent-filter>
</activity>
```
然后为该intent filter添加处理代码：
```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_recipe);
    onNewIntent(getIntent());
}

/**
 * 检验该intent是否是deep link的intent。如果是则从intent数据从接触recipe的URI并调用
 * showRecipe()来展示菜谱。
 **/
protected void onNewIntent(Intent intent) {
    String action = intent.getAction();
    String data = intent.getDataString();
    if (Intent.ACTION_VIEW.equals(action) && data != null) {
        String recipeId = data.substring(data.lastIndexOf("/") + 1);
        Uri contentUri = RecipeContentProvider.CONTENT_URI.buildUpon()
                .appendPath(recipeId).build();
        showRecipe(contentUri);
    }
}
```
```
重要：通过deep link打开的app必须提供给用户"首次点击自由(First Click Free)"的体验。
这也就是说在第一次访问你的app的时候，用户必须能直接进入相关页面，而不是被插播式广告比如
提示、登陆、闪屏等打断。你可以提醒用户在该次点击之后再进行动作。

即便该应用未曾被启动过或者用户未曾登陆，也必须提供这种体验。
```
常见问题：
[如何避免通过deep link打开多个应用实例
](http://stackoverflow.com/a/25997627)
##测试该intent filter

上述demo运行后，在adb输入以下命令来trigger一个deep link:
```
adb shell am start -a android.intent.action.VIEW \
-d "http://recipe-app.com/recipe/pierogi-poutine" com.recipe_app
```
可以再替换以上url来打开其他菜谱
```
http://recipe-app.com/recipe/grilled-potato-salad
http://recipe-app.com/recipe/haloumi-salad
http://recipe-app.com/recipe/wedge-salad
```
常见问题：
* [为什么提示"Error: Activity not started, unable to resolve Intent"?](http://stackoverflow.com/a/24850504)
* [怎么让我的deep link在搜索结果中出现](https://support.google.com/googleplay/android-developer/answer/6041489)

##获得来源

从Google的应用中点击了指向你的应用的链接，你的应用的那个页面将会收到特定的intent extra：
```
应用引用站点 — android-app://{package_id}/{scheme}/{host_path}
Web 引用站点 — https://{host_path}
比如从Google应用点击到你的应用，则会有
```
```
android-app://com.google.android.googlequicksearchbox/https/www.google.com
```

App能在页面启动时获得引用站点的信息，具体如下：
```
import com.google.android.gms.appindexing.AndroidAppUri;
import android.net.ParseException;


public class MainActivity extends Activity {
    /** 返回启动该Activity的引用者. */
    @Override
    public Uri getReferrer() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP_MR1) {
            return super.getReferrer();
        }
        return getReferrerCompatible();
    }

    /** 在低于SDK 22版本时使用该方法获得引用者 */
    private Uri getReferrerCompatible() {
        Intent intent = this.getIntent();
        Uri referrerUri = intent.getParcelableExtra(Intent.EXTRA_REFERRER);
        if (referrerUri != null) {
            return referrerUri;
        }
        String referrer = intent.getStringExtra("android.intent.extra.REFERRER_NAME");
        if (referrer != null) {
            // 尝试parse引用者URL
            try {
                return Uri.parse(referrer);
            } catch (ParseException e) {
                return null;
            }
        }
        return null;
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        ...

        Intent intent = this.getIntent();
        Uri referrerUri = this.getReferrer();
        if (referrerUri != null) {
            if (referrerUri.getScheme().equals("http") || referrerUri.getScheme().equals("https")) {
                // App从浏览器打开
                String host = referrerUri.getHost();
                // host会包含host路径 (比如www.google.com)

                // 在这里增加分析的代码以记录从Web搜索点击进来的流量

                ...

            } else if (referrerUri.getScheme().equals("android-app")) {
                // App从另一个app被打开
                AndroidAppUri appUri = AndroidAppUri.newAndroidAppUri(referrerUri);
                String referrerPackage = appUri.getPackageName();
                if ("com.google.android.googlequicksearchbox".equals(referrerPackage)) {
                    // App从Google app被打开
                    String host = appUri.getDeepLinkUri().getHost();
                    // host会包含host路径 (比如www.google.com)

                    // 在这里增加分析的代码以记录从Google app点击进来的流量

                    ...

                } else if ("com.google.appcrawler".equals(referrerPackage)) {
                    // Google的爬虫来着，别把这个算作app使用了
                }
            }
        }

        ...

    }

    ...

}
```
##商业价值

对搜索引擎提供上来说：广告，你懂的。百毒会不会把一些钓鱼app的页面放到最前面呢？呵呵。

对App来说则一方面可以解决目前移动应用的孤岛局面，另一方面可以通过搜索分析报告来了解通过搜索引擎导流的点击次数、查询次数，以及最受欢迎的页面。

各全家桶app企业也能就此机会更加紧密地抱团在一起，由大公司投资的各创业公司则能就此机会表忠心或者抱大腿。

##最后

上文大多是从Google的Deeplink展开的，如果你的应用主打本土市场，且考虑到目前Google仍然未回归，可以参考全家桶的Applink，大都是雷同的，只需要换一下前缀罢了（我猜是这样的 哈哈)。

目前App本身和搜索还是没有结合起来，国内只有豌豆荚和全家桶开始了这种体验的尝试，App的体验仍然是一个个信息孤岛，远不如在Web上搜到哪儿去哪儿，希望Deep Link的逐渐推广和应用，可以帮助app们达到和网页一样的体验。

以后App也能和网页一样，不需要自己提供搜索功能，让搜索引擎去做一切索引，直接在手机浏览器里打开app页面。甚至可以像现在使用site指定搜索目标一样，去指定要搜索的app。

试想我能直接社工搜索到女神的信息，然后直接跳到微博app里的feed详情页。另外，现在这种听一首歌要装3个app还要一个个去搜想听的到底在哪家的情况是不是也能解决呢？

原文：http://blog.zhaiyifan.cn/2016/02/04/deeplink-intro/

作者：MarkZhai
链接：http://www.jianshu.com/p/964f871dc716