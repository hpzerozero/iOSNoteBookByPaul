# 强制类型转换

Swift 语言比较严格里不同类型是不能进行运算的,必须把他们转化到相同的数据类型

```
let three = 3  
let pointOneFourOneFiveNine = 0.14159  
let pi = Double(three) + pointOneFourOneFiveNine  
let pi2 = Float(pi)  
let pi3 = CGFloat(pi)  
  
let number = 3.6234  
let value = Int(number)  
debugPrint(value) 
```

总结：不同类型不能相加，需要类型转化。转化用`类型(待转化的数据)`这样的格式即可

