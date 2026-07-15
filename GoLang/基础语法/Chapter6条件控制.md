# if else
```go
if expression1 {

}else if expression2{

}else{

}
```

# switch case
```go
switch expression{ //switch后无expression等价于switch true
	case case1:
		statement1
		fallthrough//执行完该分支后继续执行下个分支
	case case2:
		statement2
	default://当所有case都不匹配的时候，执行default
		
}
```

# label与goto

```go
func main(){
	a := 1
	if a == 1{
		goto A
	}
A:
	a := 2	
}
```