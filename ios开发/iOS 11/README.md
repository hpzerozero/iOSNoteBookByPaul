# 踩坑

#####问题描述：iOS11上读写相册的照片时发生崩溃。     
原因：由于相册相关权限的key发生了变化。用户在没有权限的情况下，访问相册导致崩溃。 
解决方案： 
iOS11之前相册对应的key是NSPhotoLibraryUsageDescription，iOS11对应的Key是NSPhotoLibraryAddUsageDescription。同定位的Key一样，由于key没有兼容性，所以需要保留原key以兼容iOS10及之前版本。

##### tableView滚动条高度跳动、上下拉刷新问题
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