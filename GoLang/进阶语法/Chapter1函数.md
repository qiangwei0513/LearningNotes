在Go中，函数是一等公民，函数是Go最基础的组成部分，也是Go的核心。例如启动函数`main`

```
package main

import "fmt"

func main() {
	fmt.println("Hello 世界！")
}
```

  

## [#](https://golang.xiniushu.com/%E8%AF%AD%E8%A8%80%E5%85%A5%E9%97%A8/%E8%AF%AD%E6%B3%95%E8%BF%9B%E9%98%B6/75.func.html#%E5%A3%B0%E6%98%8E)声明

函数的声明格式如下

```
func 函数名([参数列表]) [返回值] {
	函数体
}
```

函数可以直接通过`func`关键字来声明，也可以声明为一个字面量，也可以作为一个类型。

```
// 直接声明
func DoSomething() {

}

// 字面量
var doSomthing func()

// 类型
type DoAnything func()
```

函数签名由函数名称，参数列表，返回值组成，下面是一个完整的例子

```
func Sum(a, b int) int {
   return a + b
}
```

函数名称`Sum`，有两个`int`类型的参数`a`，`b`，返回值类型为`int`。

提示

在Go中，不支持函数重载。

  

## [#](https://golang.xiniushu.com/%E8%AF%AD%E8%A8%80%E5%85%A5%E9%97%A8/%E8%AF%AD%E6%B3%95%E8%BF%9B%E9%98%B6/75.func.html#%E5%8F%82%E6%95%B0)参数

Go中的函数参数可以有名称，也可以没有名称。例如在声明一个函数字面量时可以省略名称，但是在赋值时依旧需要名称。

```
var sum func(int,int) int

sum = func(a int, b int) int {
   return a + b
}
```

对于一些类型相同且相邻的参数而言，可以只声明一次类型。例如

```
// a,b,c都是int类型的参数，所以只需要声明一次类型
func max(a, b, c int) int {
	if a < b {
		a, b = b, a
	}
	if a < c {
		a, c = c, a
	}
	return a
}
```

变长参数可以接收0个或多个值，必须声明在参数列表的末尾。

```
func max(args ...int) int {
   max := math.MinInt64
   for _, arg := range args {
      if arg > max {
         max = arg
      }
   }
   return max
}
```

提示

Go中的函数参数是传值传递，即在传递参数时会拷贝实参的值


==`func max(args ...int) int`表示接受可变数量的int类型，这里要传递int类型的变量，这些传递进来的变量组成一个`[]int`类型的切片。==
==如果`nums := []int{1,2,3}`,nums本身就是一个切片，那么不能使用`max(nums)`,但是可以使用`max(nums...)`,这里的...表示将切片展开为一个个参数。==

