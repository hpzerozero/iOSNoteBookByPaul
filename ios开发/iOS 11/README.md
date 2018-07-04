# 踩坑

#####问题描述：iOS11上读写相册的照片时发生崩溃。     
原因：由于相册相关权限的key发生了变化。用户在没有权限的情况下，访问相册导致崩溃。 
解决方案： 
iOS11之前相册对应的key是NSPhotoLibraryUsageDescription，iOS11对应的Key是NSPhotoLibraryAddUsageDescription。同定位的Key一样，由于key没有兼容性，所以需要保留原key以兼容iOS10及之前版本。

####iOS11上如果只读相册不需要用户权限就可以打开
大家都知道访问相册需要申请用户权限。 相册权限需要在info.plist—Property List文件中添加NSPhotoLibraryUsageDescription键值对，描述文字不能为空。 iOS11之前：访问相册和存储照片到相册（读写权限），需要用户授权，需要添加NSPhotoLibraryUsageDescription。

iOS11之后：**默认开启访问相册权限（读权限**），无需用户授权，无需添加NSPhotoLibraryUsageDescription，适配iOS11之前的还是需要加的。 添加图片到相册（写权限），需要用户授权，需要添加 NSPhotoLibraryAddUsageDescription。

总结：iOS11之后如果需要写入权限需要添加`NSPhotoLibraryAddUsageDescription`字段。 
```
NSPhotoLibraryUsageDescription：

Property List：

Privacy - Photo Library Usage Description
```
Source Code：

`NSPhotoLibraryUsageDescription `是否允许此APP使用相册？
```
NSPhotoLibraryAddUsageDescription ：

Property List：

Privacy - Photo Library Additions Usage Description
```
Source Code:

`NSPhotoLibraryAddUsageDescription` 是否允许此APP保存图片到相册？

##### tableView滚动条高度跳动、上下拉刷新问题以及设置的段头段尾的高度不显示
```
self.tableView.estimatedRowHeight = 0;
self.tableView.estimatedSectionHeaderHeight = 0;
self.tableView.estimatedSectionFooterHeight = 0;
```
##### 列表/页面偏移
```
if (@available(iOS 11.0, *)){
        _tableView.contentInsetAdjustmentBehavior = UIScrollViewContentInsetAdjustmentNever;
    }
```
目前发现所有的Scrollview 及其子类都需要设置     `contentInsetAdjustmentBehavior = UIScrollViewContentInsetAdjustmentNever` ，工程中大量使用，因为UIView及其子类都遵循UIAppearance协议，我们可以进行全局配置

```
// AppDelegate 进行全局设置
if (@available(iOS 11.0, *)){
    [[UIScrollView appearance] setContentInsetAdjustmentBehavior:UIScrollViewContentInsetAdjustmentNever];
}
    
```

##### 修改UIBarButtonItem的自定义customView的位置大小
```
let button = UIButton(type: .custom)
     button.addTarget(self, action:#selector(self.toLogin(_:)), for: .touchUpInside)
     button.frame = CGRect(x: 0, y: 0, width: 32, height: 32)
     button.setTitleColor(UIColor.white, for: .normal)
     button.setTitleColor(UIColor.white, for: .highlighted)


    var buttomItem : UIBarButtonItem = UIBarButtonItem()
    buttomItem.customView = button
    buttomItem.target = self
    buttomItem.action = "ToLogin"
// 加上约束 
let widthConstraint = button.widthAnchor.constraint(equalToConstant: 32)
 let heightConstraint = button.heightAnchor.constraint(equalToConstant: 32)
 heightConstraint.isActive = true
 widthConstraint.isActive = true
 ```