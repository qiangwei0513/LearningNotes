Go语言的关键字只有25个：
```go
break        default      func         interface    select
case         defer        go           map          struct
chan         else         goto         package      switch
const        fallthrough  if           range        type
continue     for          import       return       var
```

示例程序：
```go
package main

import "fmt"

func main() {
   fmt.Println("Hello 世界!")
}
```

package是表示当前文件属于哪个包，启动文件是main包，启动函数是main函数
import后跟被导入的包名
func后为定义的函数名

# package包

## import导入
例如创建一个文件，作为example包：
```go
package example

import "fmt"

func SayHello() {
   fmt.Println("Hello")
}
```
在main.go中导入example包：
```go
package main

import "example"

func main() {
   example.SayHello()
}
```
还可以给包起别名：
```go
import e "example"
```
或者批量导入：
```go
import(
	"fmt"
	"math"
)
```
如果只是为了只用包的init函数，可以在包前面加单下划线匿名导入：
```go
import _ "math"
```
## 导出
Go 不使用 `public`、`private` 这类关键字，而是直接看名字的首字母。
首字母大写意为可导出，小写为不可导出。
```go
package example

import "fmt"

func SayHello() {
    fmt.Println("Hello")
}

func sayBye() {
    fmt.Println("Bye")
}
```
# 运算符
```
Precedence    Operator
    5             *  /  %  <<  >>  &  &^
    4             +  -  |  ^
    3             ==  !=  <  <=  >  >=
    2             &&
    1             ||
```
Go语言中没有自增和自减运算符，++与--被降级成了语句（statement），只能放在变量的后面，且没有返回值
```go
a++
a--
--a//错误
```

# 字面量
```go
//字符字面量必须使用单引号括起来`''`，Go中的字符完全兼容`utf8`。
'a'
'ä'
'你'
'\t'
'\000'
'\007'
'\377'
'\x07'
'\xff'
'\u12e4'
'\U00101234'

//转义字符
\a   U+0007 响铃符号（建议调高音量）
\b   U+0008 回退符号
\f   U+000C 换页符号
\n   U+000A 换行符号
\r   U+000D 回车符号
\t   U+0009 横向制表符号
\v   U+000B 纵向制表符号
\\   U+005C 反斜杠转义
\'   U+0027 单引号转义 (该转义仅在字符内有效)
\"   U+0022 双引号转义 (该转义仅在字符串内有效)

//字符串字面量必须使用双引号`""`括起来或者反引号（反引号字符串不允许转义）
`abc`                // "abc"
`\n
\n`                  // "\\n\n\\n"
"\n"
"\""                 // `"`
"Hello, world!\n"
"今天天气不错"
"日本語"
"\u65e5本\U00008a9e"
"\xff\u00FF"
```
