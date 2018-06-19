#tableView的修改

首先我这样删除的时候

```
self.favoriteList.remove(at: indexPath.row)
tableView.beginUpdates()
tableView.deleteRows(at: [indexPath], with: .left)
tableView.endUpdates()
```    

返回的cell数量是这么写的，返回1是默认没数据显示空数据界面，而这个界面使用的cell
```
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        // #warning Incomplete implementation, return the number of rows
        return favoriteList.isEmpty ? 1 : favoriteList.count
    } 
```

报下面的错误：

    The number of rows contained in an existing section after the update (1) must be equal to the number of rows contained in that section before the update (1), plus or minus the number of rows inserted or deleted from that section (0 inserted, 1 deleted) and plus or minus the number of rows moved into or out of that section (0 moved in, 0 moved out).

修改如下没有问题

```
tableView.beginUpdates()
self.favoriteList.remove(at: indexPath.row)
tableView.deleteRows(at: [indexPath], with: .left)
tableView.endUpdates()
``` 

但是如果删除最后一个cell会出现问题崩溃，因为删除之后favoriteList是空的数组，tableView会认为numberOfRowsInSection会为0,而我返回的是1,因此会崩溃

需修改为：
```
if self.favoriteList.count > 1 {
                    
                    self.favoriteList.remove(at: indexPath.row)
                    tableView.beginUpdates()
                    tableView.deleteRows(at: [indexPath], with: .left)
                    tableView.endUpdates()
                } else {
                    self.favoriteList.remove(at: indexPath.row)
                    tableView.reloadData()
                }
```

注意：使用这种方式删除一定要和numberOfRowsInSection代理方法密切配合

