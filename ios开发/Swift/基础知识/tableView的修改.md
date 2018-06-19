#tableView的修改

首先我这样删除的时候

```
self.favoriteList.remove(at: indexPath.row)
tableView.beginUpdates()
tableView.deleteRows(at: [indexPath], with: .left)
tableView.endUpdates()
```    


```
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        // #warning Incomplete implementation, return the number of rows
        return favoriteList.isEmpty ? 1 : favoriteList.count
    } 
```

报下面的错误：

    The number of rows contained in an existing section after the update (1) must be equal to the number of rows contained in that section before the update (1), plus or minus the number of rows inserted or deleted from that section (0 inserted, 1 deleted) and plus or minus the number of rows moved into or out of that section (0 moved in, 0 moved out).




