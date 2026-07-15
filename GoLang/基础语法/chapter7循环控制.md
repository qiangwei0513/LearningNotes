Go中只有一种循环语句：`for`,可以被当作`while`语句使用

for语句格式：
```go
for statement; expression ;statement{
	execute statement
}

ej:
for i:= 0; i <= 20; i++{
	fmt.Println(i)
}
```

while()循环
```go
for statement{
	execute statement
}

ej:
num := 1
for num < 100{
	num *= 2
}
```

while(1)循环
```go
for{
	execute statement
}
```

for range
```go
for index, value := range iterable{
	
}

```

break和continue与其他语言无差异，其后面可以加label控制

```go
Outer:
for i := 0; i < 3; i++ {
	for j := 0; j < 3; j++ {
		if i == 1 && j == 1 {
			break Outer
		}

		fmt.Println(i, j)
	}
//break Outer不是结束内层循环，而是直接结束Outer标记的外层循环。
```

```go
Outer:
for i := 0; i < 3; i++ {
	for j := 0; j < 3; j++ {
		if j == 1 {
			continue Outer
		}

		fmt.Println(i, j)
	}
}
//continue Outer会立即结束当前的内层循环执行，并开始外层循环的下一次迭代。
```
