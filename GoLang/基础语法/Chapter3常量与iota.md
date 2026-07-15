# 常量

常量的声明需要用到`const`关键字，常量在声明时就必须初始化一个值，并且常量的类型可以省略。如果仅仅只是声明而不指定值，将会无法通过编译。
```go
const name string = "Jack" // 字面量

const msg = "hello world" // 字面量

const num = 1 // 字面量

const numExpression = (1+2+3) / 2 % 100 + num // 常量表达式
```
批量声明常量可以用`()`括起来以提升可读性，可以存在多个`()`达到分组的效果。
```go
const (
   Count = 1
   Name  = "Jack"
)
```

# iota

`iota` 是 Go 语言中专门用于 `const` 常量声明的一个自动计数器。

它最常见的用途是批量定义一组连续常量，例如：

```go
const (
    A = iota
    B
    C
)
```

结果为：

```text
A = 0
B = 1
C = 2
```

可以先把 `iota` 理解为：

> `iota` 表示当前常量声明项在这个 `const` 块中的序号，从 `0` 开始。

---

## 1. 基本用法

```go
package main

import "fmt"

func main() {
    const (
        A = iota
        B
        C
    )

    fmt.Println(A, B, C)
}
```

输出：

```text
0 1 2
```

上面的代码相当于：

```go
const (
    A = 0
    B = 1
    C = 2
)
```

在 `const` 声明块中，如果某一行没有写表达式，就会自动重复上一行的完整表达式。

因此：

```go
const (
    A = iota
    B
    C
)
```

可以理解为：

```go
const (
    A = iota
    B = iota
    C = iota
)
```

但是每到一个新的常量声明项，`iota` 的值都会增加 `1`。

---

## 2. `iota` 从 0 开始

每个新的 `const` 声明块中，`iota` 都从 `0` 开始。

```go
const (
    A = iota // 0
    B        // 1
)

const (
    C = iota // 0
    D        // 1
)
```

结果：

```text
A = 0
B = 1
C = 0
D = 1
```

`iota` 不会在整个程序中持续增长，它只在当前 `const` 声明块中计数。

---

## 3. `iota` 只能用于常量声明

正确写法：

```go
const A = iota
```

错误写法：

```go
var A = iota
```

下面的写法也错误：

```go
fmt.Println(iota)
```

因为 `iota` 不是普通变量，而是 Go 编译器在处理常量声明时提供的特殊标识符。

---

## 4. 按常量声明项递增

```go
const (
    A = iota // 0
    B        // 1
    C        // 2
)
```

如果同一行声明多个常量，它们使用相同的 `iota` 值：

```go
const (
    A, B = iota, iota
    C, D = iota, iota
)
```

结果：

```text
A = 0
B = 0
C = 1
D = 1
```

不是：

```text
A = 0
B = 1
C = 2
D = 3
```

因此，`iota` 是按常量声明项递增，而不是按常量个数递增。

---

## 5. 对 `iota` 进行运算

`iota` 是整数常量，因此可以参与常量表达式的计算。

### 从 1 开始

```go
const (
    A = iota + 1
    B
    C
)
```

结果：

```text
A = 1
B = 2
C = 3
```

计算过程：

```text
A = 0 + 1
B = 1 + 1
C = 2 + 1
```

### 乘法

```go
const (
    A = iota * 10
    B
    C
)
```

结果：

```text
A = 0
B = 10
C = 20
```

### 复杂表达式

```go
const (
    A = iota*iota + 1
    B
    C
)
```

结果：

```text
A = 1
B = 2
C = 5
```

计算过程：

```text
A = 0 × 0 + 1 = 1
B = 1 × 1 + 1 = 2
C = 2 × 2 + 1 = 5
```

---

## 6. 使用 `_` 跳过某个值

如果不想使用 `iota = 0`，可以使用空白标识符 `_`。

```go
const (
    _ = iota
    A
    B
    C
)
```

结果：

```text
A = 1
B = 2
C = 3
```

第一行：

```go
_ = iota
```

虽然没有定义一个可使用的常量，但仍然消耗了 `iota = 0`。

这是一种非常常见的写法。

---

## 7. 使用 `iota` 模拟枚举

Go 没有专门的 `enum` 枚举关键字，通常使用 `const + iota` 实现类似效果。

```go
type Weekday int

const (
    Monday Weekday = iota
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
    Sunday
)
```

结果：

```text
Monday    = 0
Tuesday   = 1
Wednesday = 2
Thursday  = 3
Friday    = 4
Saturday  = 5
Sunday    = 6
```

使用：

```go
func main() {
    var day Weekday = Wednesday
    fmt.Println(day)
}
```

输出：

```text
2
```

这里：

```go
type Weekday int
```

表示定义一个新的类型 `Weekday`，它的底层类型是 `int`。

---

## 8. 为什么配合自定义类型使用

下面这种写法也可以：

```go
const (
    Monday = iota
    Tuesday
    Wednesday
)
```

但更推荐：

```go
type Weekday int

const (
    Monday Weekday = iota
    Tuesday
    Wednesday
)
```

这样可以让常量的语义更明确。

例如：

```go
type Weekday int
type Status int
```

```go
const (
    Monday Weekday = iota
    Tuesday
)

const (
    Pending Status = iota
    Finished
)
```

`Weekday` 和 `Status` 是两个不同的类型，不容易混淆。

---

## 9. `iota` 生成的仍然是数字

```go
type Weekday int

const (
    Monday Weekday = iota
    Tuesday
    Wednesday
)
```

直接打印：

```go
fmt.Println(Monday)
```

输出：

```text
0
```

不会自动输出：

```text
Monday
```

如果想输出名称，可以给类型定义 `String()` 方法：

```go
type Weekday int

const (
    Monday Weekday = iota
    Tuesday
    Wednesday
)

func (w Weekday) String() string {
    switch w {
    case Monday:
        return "Monday"
    case Tuesday:
        return "Tuesday"
    case Wednesday:
        return "Wednesday"
    default:
        return "Unknown"
    }
}
```

使用：

```go
fmt.Println(Monday)
fmt.Println(Tuesday)
```

输出：

```text
Monday
Tuesday
```

---

## 10. 同一行使用多个表达式

```go
const (
    A, B = iota, iota + 10
    C, D
    E, F
)
```

相当于：

```go
const (
    A, B = iota, iota + 10
    C, D = iota, iota + 10
    E, F = iota, iota + 10
)
```

结果：

```text
A = 0
B = 10

C = 1
D = 11

E = 2
F = 12
```

同一行中的多个 `iota` 值相同。

---

## 11. 中途修改表达式

```go
const (
    A = iota      // 0
    B             // 1
    C = iota * 10 // 20
    D             // 30
)
```

结果：

```text
A = 0
B = 1
C = 20
D = 30
```

因为 `D` 没有写表达式，所以它重复上一行的表达式：

```go
iota * 10
```

计算过程：

```text
A = 0
B = 1
C = 2 × 10 = 20
D = 3 × 10 = 30
```

---

## 12. 即使没有使用 `iota`，它也会继续增长

```go
const (
    A = iota // 0
    B = 100  // 此时 iota 为 1
    C = iota // 2
)
```

结果：

```text
A = 0
B = 100
C = 2
```

虽然 `B` 这一行没有使用 `iota`，但它仍然占用了一个常量声明项。

| 声明项 | 代码 | 当前 `iota` |
| --- | --- | ---: |
| 第 1 项 | `A = iota` | 0 |
| 第 2 项 | `B = 100` | 1 |
| 第 3 项 | `C = iota` | 2 |

---

## 13. 空行和注释不会增加 `iota`

```go
const (
    A = iota // 0

    // 这是一条注释

    B // 1
)
```

`B` 的值仍然是 `1`。

空行和注释不是常量声明项，因此不会使 `iota` 增长。

---

## 14. 使用 `iota` 定义容量单位

常见写法：

```go
const (
    _  = iota
    KB = 1 << (10 * iota)
    MB
    GB
    TB
)
```

计算过程：

```text
KB = 1 << 10
MB = 1 << 20
GB = 1 << 30
TB = 1 << 40
```

对应数值：

```text
KB = 1024
MB = 1024 × 1024
GB = 1024 × 1024 × 1024
TB = 1024 × 1024 × 1024 × 1024
```

完整代码：

```go
package main

import "fmt"

func main() {
    const (
        _  = iota
        KB = 1 << (10 * iota)
        MB
        GB
        TB
    )

    fmt.Println(KB)
    fmt.Println(MB)
    fmt.Println(GB)
    fmt.Println(TB)
}
```

这里的：

```go
1 << 10
```

表示将二进制数字 `1` 向左移动 10 位，结果为：

```text
1024
```

---

## 15. 使用 `iota` 定义位标志

`iota` 经常和左移运算符 `<<` 一起使用。

```go
const (
    Read = 1 << iota
    Write
    Execute
)
```

计算过程：

```text
Read    = 1 << 0 = 0001 = 1
Write   = 1 << 1 = 0010 = 2
Execute = 1 << 2 = 0100 = 4
```

每个常量只占一个独立的二进制位。

### 组合多个权限

```go
permission := Read | Write
```

二进制过程：

```text
Read  = 0001
Write = 0010
结果  = 0011
```

### 判断权限

```go
if permission&Read != 0 {
    fmt.Println("拥有读权限")
}
```

### 删除权限

```go
permission &^= Write
```

这里：

```go
permission &^= Write
```

表示把 `Write` 对应的二进制位清零。

---

## 16. 为什么位标志使用 `1 << iota`

如果直接写：

```go
const (
    Read = iota
    Write
    Execute
)
```

结果为：

```text
Read    = 0
Write   = 1
Execute = 2
```

这些数值不能很好地作为独立的二进制标志。

使用：

```go
const (
    Read = 1 << iota
    Write
    Execute
)
```

得到：

```text
Read    = 0001
Write   = 0010
Execute = 0100
```

每个标志占用一个独立的二进制位，因此可以使用按位或运算符 `|` 组合。

---

## 17. 一行多个常量的技巧

```go
const (
    Bit0, Index0 = 1 << iota, iota
    Bit1, Index1
    Bit2, Index2
)
```

结果：

```text
Bit0   = 1
Index0 = 0

Bit1   = 2
Index1 = 1

Bit2   = 4
Index2 = 2
```

每一行重复使用表达式：

```go
1 << iota, iota
```

---

## 18. `iota` 的类型

`iota` 本身是一个无类型整数常量，即：

```text
untyped integer constant
```

例如：

```go
const A = iota
```

这里的 `A` 默认也是无类型整数常量。

可以显式指定类型：

```go
type Status int

const (
    Pending Status = iota
    Running
    Finished
)
```

也可以指定其他整数类型：

```go
const (
    A uint8 = iota
    B
    C
)
```

此时 `A`、`B`、`C` 都是 `uint8` 类型。

注意类型范围：

```go
const A uint8 = 300
```

这会报错，因为 `uint8` 的取值范围是：

```text
0 ~ 255
```

---

## 19. 独立的 `const` 会重新计数

```go
const A = iota
const B = iota
```

结果：

```text
A = 0
B = 0
```

因为它们是两个独立的常量声明。

而：

```go
const (
    A = iota
    B
)
```

结果：

```text
A = 0
B = 1
```

---

## 20. 常见错误

### 错误一：认为 `iota` 是普通变量

错误写法：

```go
iota = 10
```

`iota` 不能被赋值，也不能在普通代码中使用。

```go
fmt.Println(iota)
```

同样错误。

---

### 错误二：认为每出现一次 `iota` 就加 1

```go
const (
    A, B = iota, iota
)
```

结果：

```text
A = 0
B = 0
```

同一声明项中的多个 `iota` 值相同。

---

### 错误三：认为没有写 `iota` 就不会增长

```go
const (
    A = iota // 0
    B = 100  // iota 为 1
    C = iota // 2
)
```

结果：

```text
C = 2
```

---

### 错误四：认为空行会占用一个值

空行和注释不会增加 `iota`。

只有常量声明项才会让它递增。

---

### 错误五：认为 `iota` 会跨 `const` 块增长

每个新的 `const` 声明块都会从 `0` 重新开始。

---

## 21. 例题一

分析下面常量的值：

```go
const (
    A = iota
    B
    C = 100
    D
    E = iota
    F
)
```

逐项分析：

```text
A：iota = 0，所以 A = 0
B：重复上一项表达式 iota，iota = 1，所以 B = 1
C：iota = 2，但表达式为 100，所以 C = 100
D：重复上一项表达式 100，所以 D = 100
E：iota = 4，所以 E = 4
F：重复上一项表达式 iota，iota = 5，所以 F = 5
```

最终结果：

```text
A = 0
B = 1
C = 100
D = 100
E = 4
F = 5
```

---

## 22. 例题二

```go
const (
    A, B = iota + 1, iota + 10
    C, D
    E, F = 100, iota
    G, H
)
```

第一项，`iota = 0`：

```text
A = 0 + 1  = 1
B = 0 + 10 = 10
```

第二项，`iota = 1`，重复上一项表达式：

```text
C = 1 + 1  = 2
D = 1 + 10 = 11
```

第三项，`iota = 2`：

```text
E = 100
F = 2
```

第四项，`iota = 3`，重复上一项表达式：

```text
G = 100
H = 3
```

最终结果：

```text
A = 1
B = 10
C = 2
D = 11
E = 100
F = 2
G = 100
H = 3
```

---

## 23. 核心规律总结

1. `iota` 只能用于 `const` 常量声明。
2. 每个新的 `const` 声明块中，`iota` 从 `0` 开始。
3. 每经过一个常量声明项，`iota` 增加 `1`。
4. 同一声明项中的多个 `iota` 值相同。
5. 某一项省略表达式时，会重复上一项的完整表达式。
6. 即使某一项没有使用 `iota`，它仍然会继续计数。
7. 空行和注释不会使 `iota` 增长。
8. `_ = iota` 可以跳过某个值。
9. `iota + 1` 可以让编号从 `1` 开始。
10. `1 << iota` 常用于定义二进制位标志。

最简单的记忆方式：

> `iota` 是当前 `const` 声明块中的常量声明项序号，从 `0` 开始。
