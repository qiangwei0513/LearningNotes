数组是定长的，不可扩容；切片是不定长的，在容量不够时可以自行扩容。
Go中的数组是值类型，也就是说数组是一个单独的类型，并不是指向头部元素的指针。
## 数组
初始化数组的方式：
```go
var nums [5]int
nums := [5]int{1,2,3}
nums := new([5]int)
```
以上几种方式都会给`nums`分配一片固定大小的内存，区别只是最后一种得到的值是指针。

切割数组的格式为`arr[startIndex:endIndex]`，切割的区间为**左闭右开**

## 切片
初始化切片的方式：
```go
var nums []int
nums := []int{1,2,3}
nums := make([]int,3,5)
nums := new([]int)
```
通常情况下，推荐使用`make`来创建一个空切片，只是对于切片而言，`make`函数接收三个参数：类型，长度，容量。

## append

`append`的函数签名：

```go
func append(slice []Type, elems ...Type) []Type
```

`slice []Type`表示被插入的数组，`elems`是可变参数，`...`是GO语言的展开操作符，用于将切片展开为独立的参数。
### 插入元素

切片元素的插入也是需要结合`appned`函数来使用，现有切片如下，

```go
nums := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
```

从头部插入元素

```go
nums = append([]int{-1, 0}, nums...)
fmt.Println(nums) // [-1 0 1 2 3 4 5 6 7 8 9 10]
```

从中间下标i插入元素

```go
nums = append(nums[:i+1], append([]int{999, 999}, nums[i+1:]...)...)
fmt.Println(nums) // i=3，[1 2 3 4 999 999 5 6 7 8 9 10]
```

从尾部插入元素，就是`append`最原始的用法

```go
nums = append(nums, 99, 100)
fmt.Println(nums) // [1 2 3 4 5 6 7 8 9 10 99 100]
```

### 删除元素
切片元素的删除需要结合`append`函数来使用，现有如下切片

```go
nums := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
```

从头部删除n个元素

```go
nums = nums[n:]
fmt.Println(nums) //n=3 [4 5 6 7 8 9 10]
```

从尾部删除n个元素

```go
nums = nums[:len(nums)-n]
fmt.Println(nums) //n=3 [1 2 3 4 5 6 7]
```

从中间指定下标i位置开始删除n个元素

```go
nums = append(nums[:i], nums[i+n:]...)
fmt.Println(nums)// i=2，n=3，[1 2 6 7 8 9 10]
```

删除所有元素

```go
nums = nums[:0]
fmt.Println(nums) // []
```

### 拷贝

切片在拷贝时需要确保目标切片**有足够的长度**，例如

```go
func main() {
	dest := make([]int, 0)
	src := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}
	fmt.Println(src, dest)
	fmt.Println(copy(dest, src))
	fmt.Println(src, dest)
}
```

```
[1 2 3 4 5 6 7 8 9] []
0                     
[1 2 3 4 5 6 7 8 9] []
```

将长度修改为10，输出如下

```
[1 2 3 4 5 6 7 8 9] [0 0 0 0 0 0 0 0 0 0]
9                                        
[1 2 3 4 5 6 7 8 9] [1 2 3 4 5 6 7 8 9 0]
```

### 遍历

切片的遍历与数组完全一致，`for`循环

```go
func main() {
   slice := []int{1, 2, 3, 4, 5, 7, 8, 9}
   for i := 0; i < len(slice); i++ {
      fmt.Println(slice[i])
   }
}
```

`for range`循环

```go
func main() {
	slice := []int{1, 2, 3, 4, 5, 7, 8, 9}
	for index, val := range slice {
		fmt.Println(index, val)
	}
}
```

### 多维切片

```go
var nums [5][5]int
for _, num := range nums {
   fmt.Println(num)
}
fmt.Println()
slices := make([][]int, 5)
for _, slice := range slices {
   fmt.Println(slice)
}
```

输出结果为

```
[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]

[]
[]
[]
[]
[]
```

可以看到，同样是二维的数组和切片，其内部结构是不一样的。数组在初始化时，其一维和二维的长度早已固定，而切片的长度是不固定的，切片中的每一个切片长度都可能是不相同的，所以必须要单独初始化，切片初始化部分修改为如下代码即可。

```go
slices := make([][]int, 5)
for i := 0; i < len(slices); i++ {
   slices[i] = make([]int, 5)
}
```

最终输出结果为

```
[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]

[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]
[0 0 0 0 0]
```

### 拓展表达式

切片与数组都可以使用简单表达式来进行切割，但是拓展表达式只有切片能够使用，该特性于Go1.2版本添加，主要是为了解决切片共享底层数组的读写问题，主要格式为如下，需要满足关系`low<= high <= max <= cap`，使用拓展表达式切割的切片容量为`max-low`

```go
slice[low:high:max]
```

`low`与`high`依旧是原来的含义不变，而多出来的`max`则指的是最大容量，例如下方的例子中省略了`max`，那么`s2`的容量就是`cap(s1)-low`

```go
s1 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9} // cap = 9
s2 := s1[3:4] // cap = 9 - 3 = 6
```

那么这么做就会有一个明显的问题，`s1`与`s2`是共享的同一个底层数组，在对`s2`进行读写时，有可能会影响的`s1`的数据，下列代码就属于这种情况

```go
s1 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9} // cap = 9
s2 := s1[3:4]                          // cap = 9 - 3 = 6
// 添加新元素，由于容量为6.所以没有扩容，直接修改底层数组
s2 = append(s2, 1)
fmt.Println(s2)
fmt.Println(s1)
```

最终的输出为

```
[4 1]
[1 2 3 4 1 6 7 8 9]
```

可以看到明明是向`s2`添加元素，却连`s1`也一起修改了，拓展表达式就是为了解决此类问题而生的，只需要稍微修改一下就能解决该问题

```go
func main() {
   s1 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9} // cap = 9
   s2 := s1[3:4:4]                        // cap = 4 - 3 = 1
   // 容量不足，分配新的底层数组
   s2 = append(s2, 1)
   fmt.Println(s2)
   fmt.Println(s1)
}
```

现在得到的结果就是正常的

```
[4 1]
[1 2 3 4 5 6 7 8 9]
```