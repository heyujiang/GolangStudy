## Go语言流程控制

### if - else 

#### 语句模型：

```go
if 条件1 {
    分支1
}else if 条件2 {
    分支2
}else if 条件3 {
    分支...
}else {
    分支...
}
	
```

**Go编译器，对于 `{` 和 `}` 的位置有严格的要求，它要求 else if （或者 else）和两边的花括号必须在同一行。 **



**由于Go是强类型，所以要求条件表达式必须严格返回布尔型的数据。（nil 、0 、 1 都不行）。**



#### 单分支判断：

只有一个if 没有else：

```go
func main(){
    age := 25
    if age > 18 {
        fmt.Println("成年人")
    }
}
```

如果条件里需要满足多个条件，可以使用 `&&`  和  `||` 



#### 多分支判断：

if-else 语句：

```go
func main(){
    age := 20
    if age > 18 {
        fmt.Println("成年人")
    } else {
        fmt.Println("未成年人")
    }
}
```

if-else if-else语句：

```go
func main(){
    age := 20
    if age > 18 {
        fmt.Println（"成年人"）
    } else if age > 12 {
        fmt.Println("青少年")
    } else {
        fmt.Println("儿童")
    }
}
```



#### 高级写法：

在if 里可以允许先运行一个表达式，取得变量后，再对其进行判断，比如第一个例子代码可以写成这样：

```go
func main(){
    if age := 20 ; age > 18 {
        fmt.Println("成年人")
    }
}
```



### switch - case

##### 语句模型：

```go
seitch 表达式 {
    case 表达式1：
    	代码块
    case 表达式2：
    	代码块
    case 表达式3：
    	代码块
    case 表达式4：
    	代码块
    default:
    	代码块
}
```



##### 简单示例：

```go
func main(){
    education := "本科"
    
    switch "博士"：
	    fmt.Println("博士")
    switch "硕士"：
	    fmt.Println("硕士")
    switch "博士"：
	    fmt.Println("本科"
    switch "硕士"：
        fmt.Println("专科")
    default:
        fmt.Println("学历未知")
}
```



##### 一个case后多个条件：

多个条件是或的关系：

```go
func main(){
    month i= 2
    switch i {
    case 2,3,4:
        fmt.Println("春天")
    case 5,6,7:
        fmt.Println("夏天")
    case 8,9,10：
        fmt.Println("秋天")
    case 11,12,1:
        fmt.Println("冬天")
    default:
        fmt.Println("未知")
    }
}
```

##### case条件常量不能重复

当case 后接的是常量时，该常量只能出现一次。



以下两种情况，在编译时，都会报错：duplicate case "male" in switch

```go
func main(){
    gender := "male"
    switch gender {
        case "male":
        	fmt.Println("男")
        case "male":
        	fmt.Println("男")
        case "famale":
        	fmt.Println("女")
        default:
        	fmt.Println("No")
    }
}
```



```go
func mian(){
    gender := "male"
    switch gender {
        case "male","male":
            fmt.Println("男")
        case "famale":
        	fmt.Println("女")
        default:
        	fmt.Println("No")
    }
}
```



##### switch 后可接函数：

switch后可以接一个函数，只要保证函数返回值类型和case后的类型一致即可。

```go
func getResult(scores ...int) bool{
    for _,score := range scores{
        if score < 60 {
            return false
        }
    }
    return true
}

func main(){
    english := 85
    chinese := 95
    math := 100
    
    switch getResult(english,chinese,math){
        case true:
	        fmt.Println("该同学所有成绩合格")
        case false:
	        fmt.Println("该同学有成绩不合格")
    }
}
```



##### switch 后可不接表达式

switch后可以不接任何表达式、变量、函数。等同于 if - else if - else 

```go
func main(){
    score := 85
    switch{
        case score >= 80:
	        fmt.Println("优秀")
        case score < 80 && score >= 70 ：
    	    fmt.Println("良好")
        case score < 70 && score >= 60 ：
        	fmt.Println("及格")
        case score < 60 :
	        fmt.Println("不及格")
    }
}
```



##### switch穿透能力：

正常情况下，switch - case 执行顺序是：只要一个case满足条件，就直接退出switch - case 。没有一个满足的执行default。

但是有一种情况是例外：使用关键字 `fallthrough`  开启穿透能力的时候：

```go
func mian(){
    str := "hello"
    switch {
    case str == "hello"：
	    fmt.Println("hello")
        fallthrough
    case str != "world":
        fmt.Println("world")
    }
}
```

`*注意`: `fallthrough 智能穿透一层，能够去条件执行下一个case ，无论是否成立都要退出`。



### for 循环

##### 语句模型：

```go
for [condition | (init ; condition ; increment) | Range]
{
    statement(s);
}
```



for 后面可以接三种类型的表达式：

- 接一个表达式
- 接三个表达式
- 接一个range表达式

也可以不接表达式。



##### 接一个表达式：

```go
//打印 1 到5
a := 1 
for a <= 5 {
    fmt.Println(a)
    a ++ 
}
```

##### 接三个表达式：

```go
for a := 1 ; a <= 5 ;a++{
    fmt.Println(a)
}
```

##### 不接表达式

不接表达式，无线循环。



Go中没有while 循环，要做到无限循环，可以使用 for{}

```go
for {
    
}
//等价于
for ;;{
    
}
```

```go
i := 1
for{
    if i > 5 {
        break
    }
    fmt.Println(i)
    i++
}
```



##### 接for-range语句：

遍历一个可以迭代的对象，在go中可以使用 `for-range` 。



由于 range 会返回两个值：索引和值。若后面不是用户索引可以用 `_` 接收。



```go
func main(){
    a := [...]int{"aaa","bbbb","ccc"}	
    for _,value := range a {
        fmt.Println(value)
    }
}
//aaa bbb ccc
```



### goto 无条件跳转

goto后接一个标签，这个标签告诉后面执行哪里的代码。

##### 最简单的示例：

```go
func main(){
	goto flag
    fmt.Println("A")
flag:
    fmt.Println("B")
}
```

输出结果为 ： `B`

##### 如何使用：

实现1到5的打印：

```go
func main(){
    i := 1
flag:    
    if i <= 5{
        fmt.Println(i)
        i++
        goto flag
    }
}
```

实现 `break` 的效果：

```go
func main(){
    i := 1
    for i <= 5 {
        if(i == 3){
            goto flag
        }
        i++
        fmt.Println(i)
    }
flag:    
}
```

实现 `continue` 效果：

```go
func main(){
    i := 1
flag:    
    if i <= 10 {
        if i%2 == 0 {
            i++
            goto flag
        }
        i++
        fmt.Println(i)
    }
}
```

##### 注意事项：

goto 语句和变量之间不能有变量声明。

```go
import "fmt"

func mian(){
    fmt.Println("hello")
    goto flag
    a := "world"    
    fmt.Println(a)
flag:
}
```

报错：

```go
.\main.go:7:7: goto flag jumps over declaration of say at .\main.go:8:6
```



### defer 延迟执行

 	**Go 独有的关键字。**

##### 延时调用

defer使用很简单，只要在defer后接一个函数，就可以在将函数延迟到当前函数执行完之后再执行。

```go
func test(){
    fmt.Println("world");
}

func main(){
    defer test()
    fmt.Println("hello，")
}
//hello，world
```

上面例子可以简写：

```go
func main(){
    defer fmt.Println("world")
    fmt.Println("，world")
}
```



##### 即时求值的变量快照

使用defer只是延时调用函数，此时传递给函数里的变量，不应该受到后续程序的影响。

```go
func test(str string){
    fmt.Println(str)
}
func main(){
    name := "java"
    defer test(name)
   	
    name = "Go"
    test(name)
}

//结果 
//Go  
//java
```

可见 name 被重新复制为 `go` ，后续调用defer的时候，仍然使用为重新赋值的变量值，就好像defer这里，给所有变量做了一个快照一样。



##### 多个defer反序调用

在一个函数中使用了多个defer ：

```go
func main(){
    name := "go"
    defer fmt.Println(name) 
    
    name := "python"
    defer fmt.Println(name)
    
    name := "java"
    defer fmt.Println(name)
}
//输出结果
//java
//python
//go
```

可见多个defer是反序调用的，类似栈一样，后进先出。



##### defer 和 return 孰先孰后

```go
import "fmt"

var name string = "go"

func myfunc() string{
    defer func(){
        name = "python"
    }()
    
    fmt.Printf("myfunc 函数里的name：%s\n",name)
    return name
}

func main(){
    myname := myfunc()
    fmt.Printf("main 函数里的name：%s\n",name)
    fmt.Printf("main 函数里的myname ：%s\n",myname)
}
//输出结果：
//myfunc 函数里的name：go
//main 函数里的name：python
//main 函数里的name：go
```

从执行结果来看，defer 是在 return后才调用的。所以在执行defer前，myname 已经被复制为 ‘go’ 了。

##### 为什么要有defer

不管在何处return都会执行refer后的函数。