## 普通字符串

普通字符串由`""`双引号表示，支持转义，不支持多行书写

```go
"这是一个普通字符串\n"
"abcdefghijlmn\nopqrst\t\\uvwxyz"
```


## 原生字符串

原生字符串由反引号表示，不支持转义，支持多行书写，原生字符串里面所有的字符都会原封不动的输出，包括换行和缩进。

```go
`这是一个原生字符串，换行
	tab缩进，\t制表符但是无效,换行
	"这是一个普通字符串"
	
	结束
`
```

## 访问

字符串本身是一个字节数组，字符串的访问与数组切片相似

```go
func main(){
	str := "This is a string"
	fmt.Println(str[0]) //输出“116”
	fmt.Println(string(str[0:4])) //输出“This”
	
	str[0] = 't' //无法通过编译，无法通过下标修改字符串
	str = "That is a string" //可以通过覆盖修改字符串
}
```

## 字符串与切片/数组之间的转换

```go
func main() {
   str := "this is a string"
   // 显式类型转换为字节切片
   bytes := []byte(str) //将str的字符串类型转换成[]byte的字节类型
   fmt.Println(bytes) //输出[116 104 105 ... 105 110 103]
   // 显式类型转换为字符串
   fmt.Println(string(bytes))
}
```

```go
func main() {
	str := "this is a string"
	bytes := []byte(str)
    // 修改字节切片
	bytes = append(bytes, 96, 97, 98, 99)
    // 赋值给原字符串
	str = string(bytes)
	fmt.Println(str)
}
```

## 长度

在Golang中，`len()`返回字节数

```go
str1 := "This is a string"
str2 := "这是一个字符串"

fmt.Println(len(str1)) //16
fmt.Println(len(str2)) //21,在unicode中一个汉字大约是三个字节

fmt.Println(str1[0]) //T
fmt.Println(str2[0]) //è
fmt.Println(str2[0:3]) //这

```

## 拷贝 copy/clone

使用`copy()`

```go
func main() {
   var dst, src string
   src = "this is a string"
   desBytes := make([]byte, len(src))
   copy(desBytes, src)
   dst = string(desBytes)
   fmt.Println(src, dst)
}
```

使用`string.Clone()`

```go
func main() {
   var dst, src string
   src = "this is a string"
   dst = strings.Clone(src)
   fmt.Println(src, dst)
}
```

## 字符串拼接

```go
func main() {
   str := "this is a string"
   str = str + " that is a int" //使用+来拼接
   
   bytes := []byte(str)
   bytes = append(bytes, "that is a int"...) //转换成切片，再使用append拼接

}

```

以上两种方式性能较差，如果对应性能有更高要求，可以使用`strings.Builder`

```go
func main() {
   builder := strings.Builder{}
   builder.WriteString("this is a string ")
   builder.WriteString("that is a int")
   fmt.Println(builder.String())
}
```