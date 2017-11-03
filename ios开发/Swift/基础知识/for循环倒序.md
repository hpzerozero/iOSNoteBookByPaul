#for循环倒序


* 第一种
```
for idx in (0...5).reversed() {
print(i)
}
```
* 第二种
Swift 的 stride 函数返回一个任意可变步长 类型值的序列。可变步长类型是可以设置偏移量的一维标量。
他有两个变种，
from，to，最后一个值将会严格小(大)于to的值
stride(from:3, to:0, by:-1) 表示3，2，1

from，through，最后一个值将会小(大)于等于through的值
stride(from:3, through:0, by:-1) 表示3，2，1，0

```
for idx in stride(from:3, to:0,by:-1) {
print(i)
}
```

```
for idx in stride(from:3, through:0,by:-1) {
print(i)
}

```