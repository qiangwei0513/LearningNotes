## 布尔类型

布尔类型只有真值和假值。

| 类型 | 描述 |
| --- | --- |
| `bool` | `true` 为真值，`false` 为假值 |

> [!TIP] 提示
> 在 Go 中，整数 `0` 并不代表假值，非零整数也不能代表真值。
>
> 数字无法代替布尔值进行逻辑判断，数字类型和布尔类型是完全不同的类型。

## 整型

Go 为不同位数的整数分配了不同的类型，主要分为：

- 无符号整型
- 有符号整型

| 类型 | 类型和描述 |
| --- | --- |
| `uint8` | 无符号 8 位整型 |
| `uint16` | 无符号 16 位整型 |
| `uint32` | 无符号 32 位整型 |
| `uint64` | 无符号 64 位整型 |
| `int8` | 有符号 8 位整型 |
| `int16` | 有符号 16 位整型 |
| `int32` | 有符号 32 位整型 |
| `int64` | 有符号 64 位整型 |
| `uint` | 无符号整型，至少 32 位 |
| `int` | 有符号整型，至少 32 位 |
| `uintptr` | 一个足够大的无符号整数类型，足以存放指针的数值，主要用于特殊用途 |

### 有符号整数与无符号整数

有符号整数可以表示正数、负数和零，例如：

```go
var a int8 = -10
```

无符号整数只能表示零和正数，例如：

```go
var b uint8 = 10
```

下面的写法会报错：

```go
var b uint8 = -10
```

因为 `uint8` 不能保存负数。

## 浮点型

Go 的浮点数采用 IEEE 754 标准，主要分为单精度浮点数和双精度浮点数。

| 类型 | 类型和描述 |
| --- | --- |
| `float32` | IEEE 754 32 位浮点数，即单精度浮点数 |
| `float64` | IEEE 754 64 位浮点数，即双精度浮点数 |

示例：

```go
var a float32 = 3.14
var b float64 = 3.1415926535
```

一般情况下，推荐使用 `float64`，因为它可以表示更高精度的小数。

## 复数类型

复数由实部和虚部组成。

| 类型 | 描述 |
| --- | --- |
| `complex128` | 实部和虚部均为 64 位浮点数 |
| `complex64` | 实部和虚部均为 32 位浮点数 |

示例：

```go
var a complex64 = 1 + 2i
var b complex128 = 3 + 4i
```

其中：

- `1` 和 `3` 是实部
- `2i` 和 `4i` 是虚部

## 字符类型

Go 对 Unicode 编码提供完整支持。

| 类型 | 描述 |
| --- | --- |
| `byte` | 等价于 `uint8`，通常用于表示 ASCII 字符或原始字节 |
| `rune` | 等价于 `int32`，通常用于表示 Unicode 字符 |
| `string` | 字符串，即字符序列，可以转换为 `[]byte` 字节切片 |

### byte

`byte` 实际上是 `uint8` 的别名。

```go
var b byte = 'A'
```

字符 `'A'` 对应的 ASCII 编码值是 `65`。

```go
fmt.Println(b)
```

输出：

```text
65
```

### rune

`rune` 实际上是 `int32` 的别名，主要用于保存 Unicode 字符。

```go
var r rune = '中'
fmt.Printf("%c\n", r)
```

输出：

```text
中
```

### string

字符串使用双引号表示：

```go
var name string = "Golang"
```

字符串可以转换为字节切片：

```go
data := []byte("Golang")
```

也可以将字节切片转换回字符串：

```go
text := string(data)
```

## 派生类型

派生类型是在基础类型之上构造出来的类型。

| 类型 | 示例 |
| --- | --- |
| 数组 | `[5]int`，长度为 5 的整型数组 |
| 切片 | `[]float64`，元素类型为 `float64` 的切片 |
| 映射表 | `map[string]int`，键为字符串、值为整数的映射表 |
| 结构体 | `type Gopher struct{}`，定义一个名为 `Gopher` 的结构体 |
| 指针 | `*int`，指向整数的指针 |
| 函数 | `func()`，一个没有参数和返回值的函数类型 |
| 接口 | `type Gopher interface{}`，定义一个名为 `Gopher` 的接口 |
| 通道 | `chan int`，用于传递整数的通道 |

### 数组

数组具有固定的长度。

```go
var numbers [5]int
```

这里定义了一个可以存放 5 个整数的数组。

### 切片

切片的长度可以动态变化。

```go
numbers := []int{1, 2, 3}
```

### 映射表

映射表用于保存键值对。

```go
scores := map[string]int{
    "Tom":  90,
    "Jack": 85,
}
```

### 结构体

结构体用于将多个字段组合到一起。

```go
type Gopher struct {
    Name string
    Age  int
}
```

### 指针

指针用于保存变量的内存地址。

```go
a := 10
p := &a
```

其中：

- `&a` 表示取得变量 `a` 的地址
- `p` 保存变量 `a` 的地址
- `*p` 表示通过指针访问变量 `a`

### 函数类型

函数本身也可以作为一种类型。

```go
var f func()
```

可以把一个没有参数和返回值的函数赋值给它：

```go
func sayHello() {
    fmt.Println("Hello")
}

f = sayHello
f()
```

### 接口

接口用于描述一个类型应该具有哪些方法。

```go
type Speaker interface {
    Speak()
}
```

### 通道

通道主要用于 Goroutine 之间的数据传递。

```go
ch := make(chan int)
```

## 零值

官方文档中将零值称为 `zero value`。

零值并不仅仅是字面上的数字 `0`，而是一个变量声明后，在没有显式赋值时获得的默认值。

例如：

```go
var a int
var b bool
var c string
```

这些变量虽然没有手动赋值，但 Go 会自动为它们设置对应类型的零值。

| 类型 | 零值 |
| --- | --- |
| 数字类型 | `0` |
| 布尔类型 | `false` |
| 字符串类型 | `""` |
| 数组 | 固定长度的对应元素类型的零值集合 |
| 结构体 | 所有内部字段均为零值的结构体 |
| 切片、映射表、函数、接口、通道、指针 | `nil` |

示例：

```go
package main

import "fmt"

func main() {
    var a int
    var b float64
    var c bool
    var d string

    fmt.Println(a)
    fmt.Println(b)
    fmt.Println(c)
    fmt.Println(d)
}
```

输出：

```text
0
0
false

```

最后一行是一个空字符串，因此看起来是空白的。

### 数组的零值

数组中每个元素都会被初始化为对应类型的零值。

```go
var numbers [3]int

fmt.Println(numbers)
```

输出：

```text
[0 0 0]
```

### 结构体的零值

结构体中的每个字段都会被初始化为对应类型的零值。

```go
type User struct {
    Name string
    Age  int
}

var user User

fmt.Println(user)
```

输出：

```text
{ 0}
```

其中：

- `Name` 的零值是空字符串 `""`
- `Age` 的零值是 `0`

## nil

在 Go 源代码中，`nil` 可以近似理解为一个预声明的标识符：

```go
var nil Type
```

Go 中的 `nil` 并不完全等同于其他语言中的 `null`。

`nil` 是以下类型的零值：

- 指针
- 切片
- 映射表
- 函数
- 接口
- 通道

例如：

```go
var p *int
var s []int
var m map[string]int
var f func()
var i interface{}
var ch chan int
```

上面这些变量的初始值都是 `nil`。

可以分别与 `nil` 比较：

```go
fmt.Println(p == nil)
fmt.Println(s == nil)
fmt.Println(m == nil)
fmt.Println(f == nil)
fmt.Println(i == nil)
fmt.Println(ch == nil)
```

但是不能直接写：

```go
nil == nil
```

这条语句无法通过编译，因为单独的 `nil` 没有确定的具体类型。

> [!IMPORTANT] 注意
> `nil` 不是一个独立的数据类型。
>
> 只有指针、切片、映射表、函数、接口和通道等特定类型的变量，才可以将 `nil` 作为零值。