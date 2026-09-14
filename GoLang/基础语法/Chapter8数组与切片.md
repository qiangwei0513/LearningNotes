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
