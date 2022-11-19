# 一、go语言版hello world
```
package main//包声明

import "fmt"//引入包

func main(){
	fmt.println("Hello world!")//语句
}
```
# 二、数据类型

## 2.1 变量创建方法

`var a int = 1`

`var a int //没有初始化，默认零值`

`var a = true //没有指定类型，根据初始值判定`

`a := 1 //:= 也是定义变量的语句`

`var a,b int = 3,4 //同时初始化多个变量`

```
var(
a int
b bool
)
//全局变量
```

`_, a := 10, 20 //匿名变量，抛弃一个变量（？）`

## 2.2 指针

~~看起来和C++差不多（而且我也不是太会）~~

## 2.3 数组

### 2.3.1 数组声明

`var abalabala [10] int`

### 2.3.2 数组初始化

1. 直接初始化
2. 长度不确定，在[]里填`...`
3. 长度确定，指定下标初始化

### 2.3.3 数组名意义

表示整个数组

### 2.3.4 数组指针

```
var a = [3]int{1,2,3}
var b = &a //b就是一个指向数组的指针
```

## 2.4 结构体

~~看起来和C++差不多（是的我有点子懒来着）~~

> 结构体指针访问结构体成员依然用`.`操作符

## 2.5 字符串

其实是一个结构体

```
type StringHeader struct{
	Data uintptr //指向底层的数组指针
	Len int //长度
}
```

## 2.6 slice

一种简化版**动态数组**~~（看别人说的）~~

### 2.6.1 slice定义
```
type SliceHeader struct {
	Data uintptr  //指向底层的数组指针
	Len int //长度
	Cap int //最大长度
}
```
### 2.6.2 示例
```
package main
import "fmt"

func main() {
    var arr1 [6]int
    var slice1 []int = arr1[2:5] // item at index 5 not included!

    // load the array with integers: 0,1,2,3,4,5
    for i := 0; i < len(arr1); i++ {
        arr1[i] = i
    }

    // print the slice
    for i := 0; i < len(slice1); i++ {
        fmt.Printf("Slice at %d is %d\n", i, slice1[i])
    }

    fmt.Printf("The length of arr1 is %d\n", len(arr1))
    fmt.Printf("The length of slice1 is %d\n", len(slice1))
    fmt.Printf("The capacity of slice1 is %d\n", cap(slice1))

    // grow the slice
    slice1 = slice1[0:4]
    for i := 0; i < len(slice1); i++ {
        fmt.Printf("Slice at %d is %d\n", i, slice1[i])
    }
    fmt.Printf("The length of slice1 is %d\n", len(slice1))
    fmt.Printf("The capacity of slice1 is %d\n", cap(slice1))

    // grow the slice beyond capacity
    //slice1 = slice1[0:7 ] 
    // panic: runtime error: slice bound out of range
}
```

~~很显然不是我写的~~

### 2.6.3 生成切片的两种~~（在尝试理解了的）~~方法

```
make([]type, len, cap)
new([cap]type)[0:len]
```
示例：
```
package main
import "fmt"

func main() {
    var slice1 []int = make([]int, 10)
    // load the array/slice:
    for i := 0; i < len(slice1); i++ {
        slice1[i] = 5 * i
    }
}
```

~~就是说，相关数组在哪（爬）~~

### 2.6.4 切片重组

`sl = sl[0:len(sl)+1]`

~~失去逻辑了这个笔记已经~~

### 2.6.5 切片的复制和追加

示例：
```
package main
import "fmt"

func main() {
    sl_from := []int{1, 2, 3}
    sl_to := make([]int, 10)

    n := copy(sl_to, sl_from)
    
    sl3 := []int{1, 2, 3}
    sl3 = append(sl3, 4, 5, 6)
}
```

~~尝试意会（指我）~~

# 函数

>可以多返回值

>可以传递变长参数
>>"如果函数的最后一个参数是采用 `...type` 的形式，那么这个函数就可以处理一个变长的参数"

示例：
```
package main

import "fmt"

func main() {
    x := min(1, 3, 2, 0)
    fmt.Printf("The minimum is: %d\n", x)
    slice := []int{7,9,3,5,1}
    x = min(slice...)
    fmt.Printf("The minimum in the slice is: %d", x)
}

func min(s ...int) int {
    if len(s)==0 {
        return 0
    }
    min := s[0]
    for _, v := range s {
        if v < min {
            min = v
        }
    }
    return min
}
```

>关键字 defer 允许我们推迟到函数返回之前（或任意位置执行 `return` 语句之后）一刻才执行某个语句或函数

>函数可以作为其它函数的参数进行传递，然后在其它函数内调用执行，一般称之为回调。

# 控制结构
## if
```
if condition {
    // do something 
} else {
    // do something 
}
```
```
if condition1 {
    // do something 
} else if condition2 {
    // do something else    
} else {
    // catch-all or default
}
```
## switch
```
switch var1 {
    case val1:
        ...
    case val2:
        ...
    default:
        ...
}
```
## for
```
for 初始化语句; 条件语句; 修饰语句 {}
```
>可以同时有多个计数器

```
for i, j := 0, N; i < j; i, j = i+1, j-1 {}
```