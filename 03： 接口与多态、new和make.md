## 接口与多态 、 new和make

#### 接口是什么？

在面相对象的领域里，接口一般是这样定义的：接口定义一个对象的行为。接口至指定对象应该做什么，止于如何实现这个行为（即实现细节），则有对象本身去确定。



在Go语言中，接口就是方法签名的集合。当一个类型定义了接口中所有方法，我们称它实现了该接口。这与面向对象编程的说法很类似。 **接口指定了一个类型应该具有的方法，并由该类型决定如何实现这些方法**。



#### 如何定义接口

使用type关键字来定义接口。

如下代码。定义一个电话接口，要求实现call方法。

```go
type Phone interface {
    call()
}
```



#### 如何实现接口

如果一个类型/结构体实现了一个接口要求的所有方法，这里Phone接口只有call方法，所以只要实现了call方法，就可以称它实现了Phone接口。

这个接口实现是隐式的，不想java中要用implements显示说明。
接着上面的例子：

```go
type Nokia struct{
    name string
}
//接收者为 nokia
func (phone Nokia) call(){
    fmt.Println("Nokia call")
}
```



#### 接口实现多态

鸭子类型（Duck typing）的定义是，只要你长的像鸭子，叫起来也像鸭子，那我就认为你就是一只鸭子。

在一个接口下，在不同对象上的不同表现，就是多态。



**在go语言中，就是通过接口来实现的多态。**

```go
//定义一个商品接口
type Goods interface{
    setAcount() int
    getInfo() string
}

//定义手机和电脑两个结构体
type Phone struct{
    name string
    quantity int
    price int
}

type Compute struct{
    name string
    quantity int
    price int
}

//手机电脑两个结构体实现Good接口
func (Phone phone) setAccount() int{
    return phone.auantity * phone.price
}
func (Phone phone) getInfo() string{
    return "购买" + strconv.Itoa(phone.quantity) + "个" + phpne.name + ";共计："+strconv.Itoa(phone.setAccount()) + "元"
}

func (Compute compute) setAccount() int{
    return compute.auantity * compute.price
}
func (Compute compute) getInfo() string{
    return "购买" + strconv.Itoa(compute.quantity) + "个" + compute.name + ";共计："+strconv.Itoa(compute.setAccount()) + "元"
}

func main(){
    iphone := Phone{
        name :"iphone"
        quantity:2
        price:4545
    }
    
    compute := Compute{
        name : "macbook"
        quantity : 2
        price : 8989
    }
    
    goods := []Goods{phone,compute}  //创建一个购物车 就是类型为Goods的切片
    
    allPrice := getAllPrice(goods)
    fmt.Println(allPrice)  //27068
}

func getAllPrice(goods []Goods) int{
    var allPrice int
    for _,goodInfo := range goods{
        goodInfo.Println(goodInfo.getInfo())
        allPrice += goodInfo.setAccount()
    } 
    return allPrice
}
```







### new 和 make ：

#### new ：

new只能传递一个蚕食，该参数为一个任意类型，可以是Go语言的内建类型，也可以是自定义的类型。

new做的事情：

- 分配内存
- 设置零值
- 返回指针（重要）



eg:

```go
type Student struct{
    name String
    age int
}

func main(){
    num := new(int)
    fmt.Println(*num)  //打印零值 ： 0
    
    s := new(Student)  //new 一个自定类型
    s.name = "Tom"
    fmt.Println(*s.name)
}
```



#### make：

1.内建函数make用来为slice（切片）、map 或者 channel 分配内存和初始化一个对象。（只能用在这三种类型上）

2.make返回类型的本身而不是指针，而返回值依赖于具体传入的类型，因为这三种类型（slice、mao和channel）本身就是应用类型，所以就没有必要返回他们的指针了。



由于这三种类型都是应用类型，所以必须得初始化（size和cap），但是不是置为零值，这个和new是不一样的。

```go
a := make([]int,2,10) //切片 slice
b := make(map[string]int) //字典 map
c := make(chan,int,10) //信道 channel
```



#### 总结：

new ： 为所有类型分配内存，并初始化为零值，返回指针。

make： 只能为slice、map、channel 分配内存，并初始化，返回的是类型。



目前看来，new函数并不常用，大家更喜欢使用短声明的方式。

但是make不一样了，它的地位无可替代，在使用slice、map、channel的时候，还是要使用make进行初始化，然后才可以对他们精心操作。