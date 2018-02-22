

* 数组定义
n表示数组的长度，type表示存储元素的类型。

```
var arr [n] type
```

由于长度也是数组类型的一部分，因此[3]int 和 [4]int是不同的类型。数组之间的赋值是值的赋值，如果把一个数组作为参数传入函数的时候，传入的其实是数组的副本，不是指针，在函数内部修改数组对原数组无影响。

golang语言数组支持嵌套，表示多维数组。

```
doubleArray := [2][4]int {{1,2,3,4}, {-1,-2,-3,-4}}或者

doubleArray := [2][2]int {[2]int{1,2}, [2]int{-1,-2}}
```

* slice 指针传递
slice 也称为动态数组，和声明数组一样，少了长度。

```
var aSlice []int

bSlice := []int {1,2,3}

cSlice := []byte {'a','b','c'}

var dSlice,eSlice []byte


```

### slice的简单操作
slice 的默认开始位置是0， ar[:n] 等价于 ar[0:n]

ar[m:] 等价于 ar[m:len(ar)]

ar[:] 等价于 ar[0:len(ar)]



