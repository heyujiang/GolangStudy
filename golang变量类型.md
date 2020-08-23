## golang变量类型：

### 五种变量定义方法：

#### 1：一行声明一个变量: 

​	var <name> <type>

​	var 是关键字固定不变，name是变量名，type是变量类型。

​	使用var，虽然至指定了类型，但是Go会对其进行隐式初始化。比如string类型是初始化为空字符串，int类型初始化为0，float类型初始化为0.0，bool类型初始化为flase，指正类型初始化为nil。



​	若是在想在声明过程中顺便初始化：

```go
var name string = "heyujiang"
```

​	从右值（等号右边的值）来看，明显是string类型。因此可以简化成：

```go
var name = "heyujiang"
```

若右边是带有小数点，在不指定类型的情况下，编译器会将其声明成float64。但多数情况下不需要这么高的精度（占用空间内存更大）。



#### 2.多个变量一起声明

声明多个变量可以按着第一种方法写多行，也可以携程下面这样：

```go
var {
	name string
	age int
	gender string
}
```

#### 3：声明和初始化一个变量

​	使用 `:=` (推导声明写法或短类型声明法：编译器会自动根据右值推断出左值的对应类型。)，可以声明一个变量并对其进行初始化。

```go
name := "heyujiang"
//等价于  var name string = "heyujiang"
//等价于  var name = "heyujinag"
```

*这种方法有个限制就是，智能用于函数内部*

#### 4.声明和初始化多个变量

```go
name,age := "何豫疆",18
```

#### 5.new函数创建执政变量

变量分为 `普通变量` 和 `指针变量`

- 普通变量，存放的是数据本身。

- 指针变量，存放的是数据的地址。

  **例如，age是一个普通变量，存放的是内容18，而ptr是存放的变量age值的内存地址：0x00001200545**

```go
package main

import "fmt"

func  main(){
    var age int = 19
    var ptr = &age //&后面接变量名，表示取出变量的内存地址
    fmt.Println("age : ",age)  //19
    fmt.Pringln("ptr : ",ptr)  //0x00001200545
}
```



new函数是Go里的一个内建函数。



使用表达式new（Type） 将创建一个Type类型的匿名变量，初始化Type类型的的零值，然后返回变量地址，返回的指正类型为*Type

```go
package main
import "fmt"
func main(){
	str := new(int)
    fmt.Println("str address : ",str)   //0x10004545
    fmt.Println("str value : ".*str)  //0
}
```

用new创建变量和普通变量声明语句没有什么区别，除了不需要声明一个临时变量的名字外，我们还可以再表达式中使用new（Type）。换言之，new函数类似是一种语法糖，而不是一个新的概念。



下面两种写法是等价的：

```go
func newInt() *int{
    return new(int)
}

func newInt() *int{
    int i int
    return &i
}
```

**以上不管哪种方法，变量/常量都智能声明一次，声明多次，编译就会报错。***

但是也有例外，有一个特殊变量：**匿名变量**，也称作占位符，或者空白标识符。用下划线（_）表示。



匿名变量有点：

 - 不分配内存，不占用内存空间

 - 不需要为它命名无用的变量名为纠结

 - 多次声明不会有任何问题

     	**通常用匿名变量接受必须接受但是又不会用到的值**

```go
func GetData() (int int){
    return 100,200
}

func main(){
    a,_ = GetData()
    _,b = GetData()
    fmt.Prinfln(a,b) //100 200
}
```



### 整型

Go语言中整数类型可以再分为10个类型。

| 种类 | 符号 | 数据类型 | 类型宽度（bit） | 类型宽度（byte） | 备注                       |
| ---- | ---- | -------- | --------------- | ---------------- | -------------------------- |
| int  | 有   | int      | 32或64          | 4或8             | 与计算器位数有关           |
| int  | 有   | int8     | 8               | 1                |                            |
| int  | 有   | int16    | 16              | 2                |                            |
| int  | 有   | int32    | 32              | 4                | 相当于32位系统下的int      |
| int  | 有   | int64    | 64              | 8                | 相当于64位系统下的int      |
| uint | 无   | uint     | 32或64          | 4或8             | 与计算器位数有关           |
| uint | 无   | uint8    | 8               | 1                |                            |
| uint | 无   | uint16   | 16              | 2                |                            |
| uint | 无   | uint32   | 32              | 4                | 相当于32位系统下的uint类型 |
| uint | 无   | uint64   | 64              | 8                | 相当于64位系统下的uint类型 |

**int 有符号，uint无符号**

uint8表示无符号的，能表示的都是整数，0~255，刚好256（2^8）个数。

int8 是有符号的，既可以是正数，也可以是负数。-128~127 （-2^7 ~ 2^7 - 1）。有一位表示符号。



int8 int16 int32 int64 uint8 uint16 uint32 uint64 这几个类型最后都有一个数值，表名数值的位数是固定的。

二int没有指定它的位数，说明大小是可以变化的，根据系统变化。

- 在32位系统下，int和uint都是占用4个字节，32位
- 在64位系统下，int和uint都是占用8个字节，64位

出于这个原因，在某些场景下应当避免int和uint的使用，而是用更加精确的int32和int84。比如在二进制传输、读写文件的结构描述（为了保持文件的结构不会受到不同编译目标平台字节长度的影响）



##### 不同进制的表示方法

默认10进制：

```go
var num int = 10
```

**2进制**：以`0b` 或 `0B` 为前缀

```go
var num01 int = 0b1100
```

**8进制**：以`0o`或`0O`为前缀

```go
var num02 int = 0o12
```

**16进制**：以`0x`为前缀

```go
var num03 int = 0xA
```



### 浮点型

##### float32 单精度，存储占用4个字节，即4*8=32位。其中一位用来表示符号，8位用来表示指数，剩下的23位表示尾数

##### float64 双精度，存储占用8个字节，即8*8=64位。其中一位用来表示符号，11位用来表示指数，剩下的52位表示尾数。



### byte 

byte，占用一个字节，就是8个比特位，所以它和uint8本质上没有区别，它表示的是ACSII表中的一个字符。

```go
import "fmt"

func main(){
  var a byte = 65
  //8进制写法：var c byte = '\101'
  //16进制写法：var d byte = '\x41'
  var b uint8 = 66
  fmt.Printf("a的值：%c \nb 的值",a,b)
}
```

在ASCII表中，由于字母A的ASCII的编号为65，字母B的ASCII编号为66，所以上面的代码也可以写成这样：

```go
import "fmt"

func main(){
  var a byte = 'A'
  var b uint = 'B'
  fmt.Printf("a的值：%cb的值：%c",a,b)
}
```

它们的输出结果都是一样的：

```go
a的值：A
b的值：B
```



### rune

rune，占用4个字节，功32位比特位，所以它和uint32本质上没有区别。它表示的是一个Unicode字符。（Unicode是一个可以表示世界范围内绝大部分字符的编码规范）。

```go
import (
  "fmt"
  "unsafe"
)

func main(){
  var a byte = 'A'
  var b rune = 'B'
  fmt.Printf("a占用%d个字节数\nb占用%d个字节数",unsafe.Sizeof(a),unsafe.Sizeof(b))
}

//输出结果：
//a占用1个字节数
//b占用4个字节数
```

**由于byte类型能表示的值是有限的，只有2^8=256个。所以如果想表示中文的化，值呢个使用rune类型。**

```go
var name rune = '中'
```

**定义字符：**不管是byte还是rune，都是实用单引号。

在Go 语言中，单引号表示字符，在上门的例子中，如果使用双引号，就意味着定义一个字符串，赋值时雨前面的声明会不一致，变异会报错。

**byte和uint8没有区别，rune和uint32没有区别，那为什么海妖多出byte和rune类型？**

​	*因为uint8和uint32，直观上让人认为这是一个数值，但是实际上，特也可以表示一个字符，所以为了消除这种直观错觉，就诞生了byte和rune这两个别名类型。*



### 字符串

字符串定义方法很简单：

```go
var mystr string = "helle world"
```

上面说的byte和rune都是字符类型，若多个字符放在一起，就可以组成字符串。



比如`hello`，对照ASCII编码表，每个字母对应的编号是：104,101,108,108,111

```go
import (
	"fmt"
)

func main(){
  var mystr01 string = "hello"
  var mystr02 []byte = [5]byte{104,101,108,108,111}
  fmt.Printf("mystr01:%s\n",mystr01)  //hello
  fmt.Printf("mystro2:%s",mystr02)  //hello
}
```

mystr01和mystr02输出一样，说明string的本质，其实是一个byte数值。



**hello,中国**占用几个字节？

​	*Go语言中string使用utf-8进行编码的，英文字母占用一个字节，而中文占用3个字节，所以`hello,中国`的长度为5+1+（3\*2）=12个字节。*

```go
import "fmt"

func main(){
  var str string = "hello,中国"
  fmt.Println(len(str))   //12
}
```



### 数组

​	**数组是一个由固定长度的特定类型元素组成的序列，一个数组可以有零个或多个元素组成。**

​	*因为数组的长度是固定的，所以在Go语言中很少直接使用数组。*



声明数组，并给该数组里的每一个元素赋值（索引值的最小有效值和其他大多数语言一样是0）：

```go
var arr [3]int
arr[0] = 1
arr[1] = 2
arr[2] = 3
```

声明数组并直接初始化数组：

```go
var arr [3]int = [3]int{1,2,3}

//or 

arr := [3]int{1,2,3}
```



上面定义数组时的3表示数组的元素个数，玩意想往数组中增加元素，的对应修改这个数字，为了避免这种硬编码，可以使用 `...` 让Go语言自己根据实际情况来分配空间。

```go
arrr := [...]int{1,2,3}
```



 `[3]int` 和 `[4]int` 虽然都是数组，但是它们不是相同的类型，使用fmt的%T可以查得。如果使用 `==`来比较，结果会是false。

```go
import(
  "fmt"
)
func main(){
  arr01 := [...]int{1,2,3}
  arr02 := [...]int{1,2,3,4}
  fmt.Printf("%d的类型是：%T",arr01,arr01)
  fmt.Printf("%d的类型是：%T",arr02,arro2)
}

//输出结果如下：
[1,2,3]的类型是：[3]int
[1,2,3,4]的类型是:[4]int
```



发现每次写 `[3]int` 优点麻烦，可以为 `[3]int` 定义一个类型字面亮，也就是别名类型。



使用 `type` 关键字可以定义一个类型字面量，后面只要想定义一个容器大小为3，元素类型为int的数组，都可以使用这个别名类型。

```go
func mian(){
  type arr3 [3]int
  
  myarr := arr3{1,2,3}
  fmt.Printf("%d的类型是：%T",myarr,myarr)
}

//输出：
//[1,2,3]的类型是：main.arr3
```



### 切片

切片（Slice）和数组一样，也是可以容纳若干个类型相同的元素的容器。与数组不同的是，无法通过切片类型来确定其值的长度。每个切片的值都会将数组作为其底层数据结构。把这样的数组称为切片的底层数组。



切片是对数组的一个连续片段的饮用，所以切片是一个引用类型，这个切片可以是整个数组，也可以是由起始和终止索引表示的一些项的子集，需要注意的是，终止索引的标示项不包括在切片内

```go
func main(){
  myarr := [...]{1,2,3,4,5}
  fmt.Printf("%d的类型是：%T",myarr[0:2],myarr[0:2])
  //[1,2]的类型是：[]int
}
```



切片的构造有三种方式：

- 对数组进行片段截取

  上面例子已经战士：myarr[0:2]。0是索引起始值，2是索引终止值，区间左闭右开。

- 从头声明赋值：

  ```go
  //声明字符串切片
  var string []string
  //声明整形切片
  var numList []int
  //声明一个空切片
  var numListEmpty = []int{}
  ```

- 使用make函数构造，make函数的格式 ： 类型（Type） ，长度（size），容量（cap）

  ```go
  func main(){
    a := make([]int ,2)
    b := make([]int ,2 ,10)
    fmt.Println(a,b)
    fmt.Println(len(a),len(b))
    fmt.Println(cap(a),cap(b))
  }
  //输出：
  //[0 0][0 0]
  //2 2
  //2 10
  ```

关于len和cap的概念，可能不好理解，举例：

- 公司名，就是变量名
- 公司里的所有工位，相当于已经分配到的内存空间
- 公司里的员工，相当于元素
- cap代表你这个公司最多可以容纳多少员工
- len代表你这个公司现在有多少员工，所以 `len` < ` cap`

由于切片是引用类型，所以不对它赋值的话就是零值（默认值）是nil

```go
var myarr []int
fmt.Println(myarr == nil)  //true
```

数组 与 切片有相同点是它们都可以容纳若干类型相同的元素的容器

不同点是，数组的容器大小固定，而切片本身是引用类型，可以对它append进行元素的添加。

```go
func main(){
  myarr := []int{1}
  //追加一个元素
  myarr := append(myarr,2)
  //追加多个元素
  myarr = append(myarr,3,4)
  //追加一个切片 ，... 表示解包
  myarr = append(myarr,[]int{7,8}...)
  //在第一个位置插入元素
  myarr = append([]int{0},myarr...)
  //在中间插入一个切片
  myarr = append(myarr[:5],append([]int{5,6},myarr[5:]...)...)
  fmt.Println(myarr) 
  // [0 1 2 3 4 5 6 7 8]
}
```



### 字典

字典（Map类型），是由若干个 `key:value` 这样的简直对映射组合在一起的数据结构。



是一个哈希表的一个实现，就是要求它的每个映射的key是唯一的。可以用 `==` 和 `!=`来进行判等操作。



声明字典的时候必须指定好key 和 value是什么类型的，然后使用map关键字来告诉Go是一个字典。

```go
map[KEY_TYPE]VALUE_TYPE
```



##### 声明出事话字典

三种方式：

```go
//第一种：
var score map[string]int = map[string]int{"elglish":80,"chinese":85}

//第二种
scores := map[string]int{"elglish":80,"chinese":85}

//第三种
scores := make[map[string]int]
scores["english"] = 80
scores["chinese"] = 85
```



**字典的相关操作**

添加元素：

```go
scores["math"] = 95
```

更新元素，若key已存在，则直接更新value：

```go
scores["english"] = 98
```

读取元素直接使用 [key] 即可，如果key不存在，也不报错，会返回value-type的零值：

```go
fmt.Println(scores["math"])
```

删除元素，使用delete函数，如果key不存在，delete函数会静默处理，不会报错：

```go
delete(scores,"math")
```



**判断key是否存在：**

当key不存在，会返回value-type的零值，所以不能通过返回的结果是否零值来判断对应key是否存在，因为key对应的value值可能恰好就是零值。



其实字典的下标读取可以返回两个值，使用第二个值表示对应key是否存在，若`key`存在`ok`为`true`，若不存在，则`ok`是`false`：

```go
func main(){
  scores := map[string]int{"english":80,"chinese":95}
  if math ,ok := scores["math"] ; ok {
    fmt.Printf("math的值是：%d",math)
  }else{
    fmt.Printf("math 不存在")
  }
}
```



**如何对字典进行循环**

循环分三种：

- 获取 `key` 和 `value` : 

  ```go
  func mian(){
    scores := map[string]int{"elglish":80,"chinese":85}
    
    for subject ,score := range scores {
      fmt.Printf("key : %s , value : %d\n",subject,value)
    }
  }
  ```

- 值获取`key`，这里注意不用使用占位符：

   ```go
  func main(){
    scores := map[string]int{"english":80,"chinese":85}
    
    for subject := range scores {
      fmt.Printf("key : %s\n",subject)
    }
  }
   ```

- 只获取value，用一个占位符替代：

  ```go
  func main(){
    scores := map[string]int{"english":80,"chinese":85}
    
    for _,score := range scores {
      fmt.Printf("vale : %d\n",score)
    }
  }
  ```

  

  ​	



### 布尔 

true 和 false。 零值是false

在Go 中，true不能与1 相等，false不能用0相比。强类型。

### 指针

#### 什么是指针：

当定义一个变量name：

```go
var name string = "GoLang"
```

当访问这个便签的时候，计算机会返回给我们它指向的内存地址里面存储的值：`GoLang`

出于某种需要，会将这个内存地址赋值给另一个变量名，通常叫做ptr（pointer的简写），而这个变量就被称为执政。

**指针变量的值是指针，也就是内存地址。**



根据变量指向的值，是否为内存地址，变量氛围两种：

- 普通变量：存数据值本身
- 指针变量：存值的内存地址



#### 指针的创建：

指针的创建有三种方式：

- 先定义对应的变量，再通过变量取得内存地址，创建指针：

  ```go
  //定义普通变量
  aint := 1
  //定义指针变量：
  ptr := &aint
  ```

- 先创建指针，分配好内存后，再给指针指向的内存地址写入对应的值：

  ```go
  //定义指针
  astr := new(string)
  //给指针赋值
  *astr = "Golang"
  ```

- 先声明一个指针变量，再从其它变量取得内存地址赋值给它：

  ```go
  aint := 1
  var bint *int //声明一个指针
  bint = &aint
  ```



上面三段代码指针的操作离不开两个符号 `&` 和 `*`

- `&` : 从一个普通变量中取得内存地址
- `*` : 当 * 在赋值操作值的右边，是从一个指针变量中取得变量值，当 * 在赋值操作值的左边，是指该指针指向的变量

```go
func main(){
  aint := 1
  ptr := &aint
  fmt.Println("普通变量存储的是："，aint)  //1
  fmt.Pringln("普通变量存储的是：",*ptr)  //1
  fmt.Println("指针变量存储的值："，&aint)  //0xc00001220
  fmt.Pringln("指针变量存储的值："，ptr)    //0xc00001220
} 
```

#### 指针的类型：

指针的类型是 `*` + 所指向变量的数据类型，就是对应的指针类型。

*int *string *bool *int32 *float64 …等等



#### 指针的零值：

切片和指针一样，都是引用类型。

如果想要通过一个函数改变一个数组的值，有两种方法：

- 将这个数组的切片作为参数传给函数
- 将这个数组的指针作为参数传给函数

尽管二者都可以实现目的，但是按照Go语言的使用习惯，建议使用第一种方法。

###### 使用切片：

```go
func modify(sls []int){
  sls[0] = 90
}

func main(){
  a := [3]int{12,23,45}
  modify(a[:])
  fmt.Println(a)   //[90 23 45]
}
```

##### 使用指针：

```go
func modify(arr *[3]arr){
  (*arr)[0] = 76
}

func main(){
  a := [3]int{12,34,23}
  modify(&a)
  fmt.Println(a)  //[76 34 23]
}
```





### 结构体

#### 什么是结构体

之前的数组、切片都只能存储同一类型的变量。若要存储多个类型的变量，就需要用到结构体，它是将多个容易类型的命令变量组合在一起去喝数据类型。



每个变量都称为该结构体的成员变量。



在Go语言中没有class类的概念，是有struct结构体的概念，因此也没有继承。



##### 定义结构体

声明结构体：

```go
type 机构体名 struct {
  属性名 属性类型
  属性名 属性类型
  。。。
}
```

比如定义一个Profile结构体：

```go
type Profile struct{
  name string 
  age int
  gender string
  mother *Profile  //指针
  father *Profile  //指针
}
```

##### 定义方法

Go语言总无法在结构体中定义方法。如果想要给结构体定义方法，可以使用组合函数的方式来定义结构体方法。和普通函数的定义方式有些不一样：

```go
func (Person Profile) FmtProfile(){
  fmt.Printf("名字：%s\n",person.name)
  fmt.Printf("年龄：%d\n",person.age)
  fmt.Printf("性别：%s\n",person.gender)
}
```

其中 `FmtProfile` 是方法名，而 `(person Profile)` 表示将FmtProfile方法与Profile的实例绑定。把Profile称为方法的接受者，而preson 表示实力本身，相当于java中的this ，在方法内部可以使用 `perosn.属性名` 的方式来访问实力属性。

完整代码： 

```go
package main
import "fmt"
//定义一个名为Profile的结构体
type Profile struct {
  name string 
  age int
  gender string
  mother *Profile
  father *Profile
}
//定义一个与Profile的绑定的方法
func (person Profile) FmtProfile(){
  fmt.Printf("名字：%s\n",person.name)
  fmt.Printf("年龄：%d\n",person.age)
  fmt.Printf("性别：%s\n",person.gender)
}

func main(){
  //实例化
  myself := Profile(name : "小明",age : 24,gender :"男")
  //调用函数
  myself.FmtProfile()
}
```

输出如下：

```go
名字：小明
年龄：24
性别：男
```



##### 方法的参数传递方式：

想要早方法内部改变实例的属性的时候，必须使用指针作为方法的接收者：

```go
package main

import "fmt"
//声明一个Profile的结构体
type Profile struct{
  name string
  age int
  gender string
  mother *Profile
  father *Profile
}
func （person *Profile）incr(){
  person.age += 1
}
func main(){
  myself := Profile(name :"小明",age :24,gender:"男")
  fmt.Printf("age:%d"，myself.age)
  myself.incr()
  fmt.Printf("age:%d"，myself.age)
}
```

输出结果：

```go
当前年龄：24
当前年龄：25
```

**什么时候使用指针作为方法的接受者呢？**

- 需要在方法内部改变结构体的内容的时候
- 处于性能的问题，当结构体过大的时候

建议都使用指针作为接收者。不管使用哪种方式定义方法，指针实例对象、值对象都可以直接调用。



#### 结构体实现 “继承”

Go语言并不支持继承，但是可以使用组合的方式实现类似继承的效果。



定义一个公司的结构体，定义一个公司职员的结构体：

```go
type company struct {
	companyName string
  companyAddr string
}

type staff struct{
  name string
  age int
  gender string
  position string
}
```

若要将公司信息与公司职员关联起来，一般会想到将company结构体的内容照抄到staff里。

```go
type staff struct{
  name string
  age int
  gender string
  position string
  companyName string
  companyAddr string
}
```

虽然实现上没有什么问题，但是在对同一个公司的出格staff初始化的时候都的重复初始化相同的公司信息，这样显然不好，借鉴继承的思想，可以将公司的属性都继承过来。



胆Go中没有类的思想，只有组合，可以加工company这个结构体嵌到staff中，作为staff的一个匿名字段，staff就直接拥有company的所有属性了。

```go
type staff struct{
  name string
  age int
  gender string
  position string
  company //匿名字段
}
```



完成程序：

 ```go
package main 
import "fmt"

type company struct {
  companyName string
  companyAddr string
}

type staff struct {
  name string
  age int
  gender string
  position string
  company
}

func mian(){
  mycom := company{
    companyName:"Tencent"
    companyAddr:"深圳南山",
  }
  
  
  selfstaff := staff{
    name : "小明"
    age : 28
    gender : "男"
    position :"云计算开发工程师"
    company:mycom,
  } 
  
  fmt.Printf("%s 在 %s 工作\n",selfstaff.name,selfstaff.companyName)
  fmt.Printf("%s 在 %s 工作\n",selfstaff.name,selfstaff.company.companyName)
}
 ```

输出结果：

 ```go
小明 在 Tencent 工作
小明 在 Tencent 工作
 ```

可见 `selfstaff.companyName` 和 `selfstaff.company.companyName` 的效果是一样的。



#### 内部方法和外部方法：

在Go中函数名的首字母大小写非常重要，它被来实现控制方法的访问权限。

-  当方法的首字母为大写时，这个方法对于所有的包都是Public，其它包可以随意调用。
- 当方法的首字母为小写时，这个方法时Private，其它包是无法访问的。

 

### 函数

#### 关于函数：

函数是基于功能或逻辑进行封装的可复用的代码结构。将一段功能复杂、很长的一段代码封装成多个代码片段（即函数），有助于提高代码可读性和可维护性。

在Go中函数可以分为两种：

- 带有名字的普通函数
- 没有名字的匿名函数



#### 函数的声明

函数的声明，使用 `func` 关键字，后面依次接 `函数名`，`参数列表`，`返回值列表`  , 用 `{}` 包裹代码逻辑体

```go
func 函数名（形参列表）(返回值列表) {
  函数体
}
```



例子：

```go
func sum (a int,b int)(int){
  return a + b
}

func main(){
  fmt.Println(sum(2,4))  //6
}
```



#### 函数实现可变参数

可变参数可分为几种：

- 多个类型一致的参数 使用 `...int`

  ```go
  func sum (args ...int) int{
    var sum int 
    for _,v := range args {
      sum += v
    }
    return sum
  }
  
  func main(){
    fmt.Pringln(sum(1,2,3,4))   //10
  }
  ```

  

- 多个类型不一致的参数  `...interface{}` 

  ```go
  import "fmt"
  dunc MyPrintf(args ...interface{}){
    for _,arg := range args {
      switch arg.(type){
        case int :
        	fmt.Println("int")
        case string :
        	fmt.Println("string")
        case int64:
        	fmt.Println("int64")
        default:
        	fmt.Println("error ...")
      }
    }
  }
  ```

  #### 多个可变参数函数传递参数

  上面用 `...` 来接收多个参数，除此之外，还有一个用法，就是用来解序列，将函数的可变参数（一个切片）一个一个取出来，传递给另一个可变参数的函数，而不是传递可变参数变量本身。

   

  同样用法也只能在函数传递参数里使用。

  

  例子：

  ```go
  import "fmt"
  
  func sum(args ...int){
    var result int
    for _,v := range args {
      result += v
    }
    return result
  }
  
  func Sum(args ...int){
    //利用 ... 来解序列
    result := sum(args...)
    return result   
  }
  
  func mian(){
    fmt.Pringln(Sum(1,2,3))  //6
  }
  ```



#### 函数的返回zhi：

- 没有返回值

  当没有指明返回值的类型时，函数体也可以有return，外部不可以用变量来接收控制

- 返回几个值

  Go支持返回多个值

  ```go
  func double(a int)(int , int){
    b := a * 2
    return a,b
  }
  func mian(){
    a,b := double(2)
    fmt.Println(a,b)  //2 4
  }
  ```

- 怎么返回值

  Go支持返回带有变量名的值：

  ```go
  func double(a int)(b int){
    //不能使用 := ，因为在返沪值那里已经声明了喂int类型
  	b = a * 2
    //不需要知名写会哪个变量，在返回值类型那里已经指定了
  	return 
  }
  func main(){
    fmt.Pringln(double(2))   //4
  }
  ```



#### 方法与函数

方法是一种特殊的函数。当你一个函数和对象／结构体进行绑定的时候，我们就称为这个函数是一个方法。



#### 匿名函数

匿名函数就是没有名字的函数，只有逻辑体，没有函数名。

定义格式：

```go
func(参数列表)（返回参数列表）{
  函数体
}
```

作用：

- 定义后立马执行：

  ```go
  func(data int){
    fmt.Println("hello: ",data)
  }(100)  
  ```

- 作为回调函数使用:

  ```go
  //第二个参数为函数
  func visit(list []int ,f func(int)){
    for _,v := range list{
      f(v)
    }
  }
  
  func main(){
    //使用匿名函数直接作为参数
    visit([]int{1,2,3,4},func(v int){
      fmt.Prinfln(v)
    })
  }
  ```

  