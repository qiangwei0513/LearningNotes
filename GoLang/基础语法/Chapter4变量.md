# Go 语言变量：声明、赋值与注意事项

变量是用于保存数据的存储位置。变量中保存的值可以在程序运行期间发生变化。

声明变量时，需要明确变量的名称和类型。Go 会为变量分配相应的存储空间，并将其初始化为对应类型的零值。

---

## 1. 变量声明

Go 使用 `var` 关键字声明变量，基本格式为：

```go
var 变量名 类型
```

例如：

```go
var intNum int
var str string
var char byte
```

这里分别声明了：

- 一个 `int` 类型变量 `intNum`
- 一个 `string` 类型变量 `str`
- 一个 `byte` 类型变量 `char`

---

## 2. 同时声明多个同类型变量

如果多个变量的类型相同，可以放在同一条声明中：

```go
var numA, numB, numC int
```

等价于：

```go
var numA int
var numB int
var numC int
```

此时三个变量都是 `int` 类型。

---

## 3. 使用括号批量声明变量

如果需要声明多个不同类型的变量，可以使用 `var (...)`：

```go
var (
    name    string
    age     int
    address string
)
```

还可以在程序中存在多个变量声明块：

```go
var (
    name    string
    age     int
    address string
)
```

这种写法适合集中声明多个变量。

---

## 4. 变量的零值

变量声明后，如果没有主动赋值，Go 会自动给它设置对应类型的零值。

```go
var name string
var age int
var enabled bool
```

它们的初始值分别为：

| 类型 | 零值 |
| --- | --- |
| `int` 等数字类型 | `0` |
| `string` | `""` |
| `bool` | `false` |
| 指针 | `nil` |
| 切片 | `nil` |
| 映射表 | `nil` |
| 通道 | `nil` |
| 函数 | `nil` |
| 接口 | `nil` |

例如：

```go
package main

import "fmt"

func main() {
    var name string
    var age int
    var enabled bool

    fmt.Printf("name=%q\n", name)
    fmt.Println("age =", age)
    fmt.Println("enabled =", enabled)
}
```

输出：

```text
name=""
age = 0
enabled = false
```

---

# 变量赋值

变量赋值使用赋值运算符 `=`。

## 5. 声明后再赋值

```go
var name string

name = "jack"
```

第一行声明变量，第二行把字符串 `"jack"` 赋给变量 `name`。

---

## 6. 声明时直接赋值

也可以在声明变量时直接赋值：

```go
var name string = "jack"
```

由于右侧的 `"jack"` 是字符串，Go 还可以自动推断变量类型：

```go
var name = "jack"
```

此时 `name` 会被推断为 `string` 类型。

---

## 7. 同时给多个变量赋值

```go
var name string
var age int

name, age = "jack", 18
```

赋值时按照左右两边的位置一一对应：

```text
name ← "jack"
age  ← 18
```

左右两边的数量必须相同。

错误示例：

```go
name, age = "jack"
```

因为左边有两个变量，右边只有一个值。

---

# 短变量声明

## 8. 使用 `:=` 声明并初始化变量

Go 提供了短变量声明语法：

```go
name := "jack"
```

这条语句同时完成了三件事：

1. 声明变量 `name`
2. 根据右边的值推断类型
3. 将 `"jack"` 赋给 `name`

它等价于：

```go
var name string = "jack"
```

也可以理解为：

```go
var name = "jack"
```

---

## 9. 短变量声明的类型推断

```go
name := "jack"
age := 18
score := 95.5
enabled := true
```

编译器会推断出：

| 变量 | 推断类型 |
| --- | --- |
| `name` | `string` |
| `age` | `int` |
| `score` | `float64` |
| `enabled` | `bool` |

可以使用 `%T` 查看变量类型：

```go
package main

import "fmt"

func main() {
    name := "jack"
    age := 18

    fmt.Printf("%T\n", name)
    fmt.Printf("%T\n", age)
}
```

输出：

```text
string
int
```

---

## 10. 批量短变量声明

可以同时声明并初始化多个变量：

```go
name, age := "jack", 18
```

等价于：

```go
var name string
var age int

name, age = "jack", 18
```

也可以声明三个变量：

```go
name, age, score := "jack", 18, 95.5
```

---

## 11. `:=` 只能在函数内部使用

正确：

```go
package main

func main() {
    name := "jack"
    _ = name
}
```

错误：

```go
package main

name := "jack"

func main() {
}
```

在函数外，也就是包级作用域中，不能使用 `:=`。

函数外应该使用：

```go
package main

var name = "jack"

func main() {
}
```

---

## 12. 不能使用 `:= nil`

下面的代码不能通过编译：

```go
name := nil
```

原因是 `nil` 本身没有确定的具体类型，编译器无法判断 `name` 应该是什么类型。

应当明确指定变量类型：

```go
var p *int = nil
```

也可以简写为：

```go
var p *int
```

因为指针类型的零值本来就是 `nil`。

---

# 多重赋值

## 13. 交换两个变量的值

Go 可以直接交换两个变量，不需要额外的临时变量。

```go
num1, num2 := 25, 36

num1, num2 = num2, num1
```

交换前：

```text
num1 = 25
num2 = 36
```

交换后：

```text
num1 = 36
num2 = 25
```

完整示例：

```go
package main

import "fmt"

func main() {
    num1, num2 := 25, 36

    num1, num2 = num2, num1

    fmt.Println(num1, num2)
}
```

输出：

```text
36 25
```

---

## 14. 同时交换多个变量

```go
num1, num2, num3 := 25, 36, 49

num1, num2, num3 = num3, num2, num1
```

交换后：

```text
num1 = 49
num2 = 36
num3 = 25
```

右边的表达式会先全部计算，然后再统一赋值给左边的变量。

因此不会发生前一个赋值影响后一个赋值的问题。

---

# 匿名变量

## 15. 使用 `_` 忽略不需要的值

在 Go 中，函数内部声明的局部变量如果没有使用，程序将无法通过编译。

当某个值不需要使用时，可以使用空白标识符 `_` 忽略它：

```go
_, b, _ := 1, 2, 3

fmt.Println(b)
```

输出：

```text
2
```

这里：

```text
_ 忽略 1
b 保存 2
_ 忽略 3
```

`_` 被称为空白标识符，也经常被称为匿名变量。

> [!NOTE]
> `_` 不是真正保存数据的普通变量。
>
> 赋给 `_` 的值会被直接忽略，因此 `_` 可以重复使用。

---

# 注意事项

## 16. 局部变量声明后必须使用

下面的代码无法通过编译：

```go
package main

func main() {
    var name string
    var age int

    name, age = "jack", 18
}
```

虽然给 `name` 和 `age` 赋了值，但是后面没有读取或使用它们，编译器仍然会报错：

```text
name declared and not used
age declared and not used
```

可以使用这些变量：

```go
package main

import "fmt"

func main() {
    var name string
    var age int

    name, age = "jack", 18

    fmt.Println(name, age)
}
```

也可以暂时使用 `_` 避免未使用错误：

```go
package main

func main() {
    var name string
    var age int

    name, age = "jack", 18

    _, _ = name, age
}
```

但在实际代码中，应该优先删除不需要的变量，而不是大量使用 `_`。

---

## 17. 包级变量可以暂时不使用

在函数外声明的变量称为包级变量。

```go
package main

var name string
var age int

func main() {
    name, age = "jack", 18
}
```

包级变量即使暂时没有被读取，也不会出现“声明但未使用”的编译错误。

但是导入的包如果没有使用，仍然会报错：

```go
package main

import "fmt"

func main() {
}
```

因为导入了 `fmt`，却没有使用它。

---

## 18. `:=` 不能重复声明同一个变量

下面的代码无法通过编译：

```go
package main

func main() {
    c := 1
    c := 2
}
```

编译器会提示：

```text
no new variables on left side of :=
```

原因是：

```go
c := 1
```

已经声明了变量 `c`。

后面只是修改它的值时，应该使用 `=`：

```go
c := 1
c = 2
```

---

## 19. `:=` 左边至少要有一个新变量

下面这种写法可以通过编译：

```go
package main

import "fmt"

func main() {
    a := 1

    a, b := 2, 3

    fmt.Println(a, b)
}
```

输出：

```text
2 3
```

分析：

```go
a, b := 2, 3
```

其中：

- `a` 已经存在，所以对 `a` 进行重新赋值
- `b` 是新变量，所以声明并初始化 `b`

因此，只要 `:=` 左边至少存在一个当前作用域中的新变量，就可以使用短变量声明。

不能这样写：

```go
a := 1
b := 2

a, b := 3, 4
```

因为 `a` 和 `b` 都已经在当前作用域中声明，没有任何新变量。

应该改成：

```go
a := 1
b := 2

a, b = 3, 4
```

---

# `var`、`:=` 和 `=` 的区别

| 写法 | 作用 | 使用位置 |
| --- | --- | --- |
| `var name string` | 声明变量 | 函数内、函数外 |
| `var name = "jack"` | 声明、赋值并推断类型 | 函数内、函数外 |
| `name := "jack"` | 短变量声明并推断类型 | 只能在函数内 |
| `name = "tom"` | 修改已经存在的变量 | 变量声明之后 |

示例：

```go
package main

import "fmt"

var city = "Beijing"

func main() {
    var name string
    name = "jack"

    age := 18
    age = 19

    fmt.Println(city, name, age)
}
```
