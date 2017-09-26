# 踩坑

* #### 系统相册相关 

#####问题描述：iOS11上读写相册的照片时发生崩溃。     
原因：由于相册相关权限的key发生了变化。用户在没有权限的情况下，访问相册导致崩溃。 
解决方案： 
iOS11之前相册对应的key是NSPhotoLibraryUsageDescription，iOS11对应的Key是NSPhotoLibraryAddUsageDescription。同定位的Key一样，由于key没有兼容性，所以需要保留原key以兼容iOS10及之前版本。

#### 滚动条高度跳动、上下拉刷新问题
```
self.tableView.estimatedRowHeight = 0;
self.tableView.estimatedSectionHeaderHeight = 0;
self.tableView.estimatedSectionFooterHeight = 0;
```