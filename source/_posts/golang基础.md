---
title: golang基础
date: 2019-01-21 18:02:11
tags: go
---
![image](https://images.pexels.com/photos/1319839/pexels-photo-1319839.jpeg?auto=compress&cs=tinysrgb&dpr=2&w=500)

### 运行go文件
- go run xxx.go

### 语言教程
- Go 是一个开源的编程语言，它能让构造简单、可靠且高效的软件变得容易。
- Go是从2007年末由Robert Griesemer, Rob Pike, Ken Thompson主持开发，后来还加入了Ian Lance Taylor, Russ Cox等人，并最终于2009年11月开源，在2012年早些时候发布了Go 1稳定版本。现在Go的开发已经是完全开放的，并且拥有一个活跃的社区。
- 语言特色：
    - 简洁、快速、安全
    - 并行、有趣、开源
    - 内存管理、数组安全、编译迅速
- 语言用途：
    - Go 语言被设计成一门应用于搭载 Web 服务器，存储集群或类似用途的巨型中央服务器的系统编程语言。
    - 对于高性能分布式系统领域而言，Go 语言无疑比大多数其它语言有着更高的开发效率。它提供了海量并行的支持，这对于游戏服务端的开发而言是再好不过了。

Go语言接受了函数式编程的一些想法，支持匿名函数与闭包。再如，Go语言接受了以Erlang语言为代表的面向消息编程思想，支持goroutine和通道，并推荐使用消息而不是共享内存来进行并发编程。总体来说，Go语言是一个非常现代化的语言，精小但非常强大。

##### Go 语言最主要的特性：

 - 自动垃圾回收
 - 更丰富的内置类型
 - 函数多返回值
 - 错误处理
 - 匿名函数和闭包
 - 类型和接口
 - 并发编程
 - 反射
 - 语言交互性

### 语言环境安装
 - 所有系统都支持

### 语言结构
 - Go Hello World 实例：
    - 包声明
    - 引入包
    - 函数
    - 变量
    - 语言 & 表达式
    - 注释

            package main
    
            import "fmt"
    
            func main() {
            /* 这是我的第一个简单的程序 */
            fmt.Println("Hello, World!")
            }

        注意：需要注意的是 { 不能单独放在一行，所以以下代码在运行时会产生错误

### 语言基础语法
 - Go 标记 ： Go 程序可以由多个标记组成，可以是关键字，标识符，常量，字符串，符号。如以下 GO 语句由 6 个标记组成
 - 行分隔符：  在 Go 程序中，一行代表一个语句结束。
 - 注释： 注释不会被编译，每一个包应该有相应的注释。
 - 单行注释是最常见的注释形式，你可以在任何地方使用以 // 开头的单行注释。多行注释也叫块注释，均已以 /* 开头，并以 */ 结尾。如：


        // 单行注释
        /*
        Author by 菜鸟教程
        我是多行注释
        */

 - 标识符：标识符用来命名变量、类型等程序实体。一个标识符实际上就是一个或是多个字母(A~Z和a~z)数字(0~9)、下划线_组成的序列，但是第一个字符必须是字母或下划线而不能是数字。
 - go 语言的空格： Go 语言中变量的声明必须使用空格隔开。 `var age int;`

 ### 语言数据类型
 - 数据类型用于声明函数和变量： 数据类型的出现是为了把数据分成所需内存大小不同的数据，编程的时候需要用大数据的时候才需要申请大内存，就可以充分利用内存。
 - go 语言类别：
    - 布尔型： 布尔型的值只可以是常量 true和false， 一个简单的例子： var bool = true
    - 数字类型： 整型 int 和浮点型 float32、float64，Go 语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。
    - 字符串类型： 字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。Go 语言的字符串的字节使用 UTF-8 编码标识 Unicode 文本。
    - 派生类型： 
        - 指针类型（Pointer）
        - 数组类型
        - 结构化类型
        - channel 类型
        - 函数类型
        - 切片类型
        - 接口类型
        - Map 类型

### 语言变量
 - Go 语言变量名由字母、数字、下划线组成，其中首个字符不能为数字。
 - 声明变量的一般形式是使用 var 关键字： `var identifter type`
 - 变量声明： 
    - 第一种，指定变量类型，声明后若不赋值，使用默认值。
      
            var v_name v_type
            v_name = value
    - 第二种， 根据自行判定变量类型

            var v_name= value

    - 第三种， 省略var, 注意 :=左侧的变量不应该是已经声明过的，否则会导致编译错误。

            v_name := value
    
            // 例如
            var a int = 10
            var b = 10
            c := 10  
        
    - 多变量声明

            //类型相同多个变量, 非全局变量
            var vname1, vname2, vname3 type
            vname1, vname2, vname3 = v1, v2, v3
            var vname1, vname2, vname3 = v1, v2, v3 //和python很像,不需要显示声明类型，自动推断
            vname1, vname2, vname3 := v1, v2, v3 //出现在:=左侧的变量不应该是已经被声明过的，否则会导致编译错误
            // 这种因式分解关键字的写法一般用于声明全局变量
            var (
                vname1 v_type1
                vname2 v_type2
            )

### 语言常量
 - 常量是一个简单值的标识符，在程序运行时，不会被修改的量。
 - 常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型
 - 常量的定义格式： `const identifier [type] = value`

    显式类型定义： `const b string = “abc”`
    隐式类型定义： `const b = “abc”`
- 多个相同类型的声明可以简写：

        const c_name1, c_name2 = value1, value2
        eg： const a, b, c = 1, false, "str" //多重赋值
- iota 特殊常量，可以认为是一个可以被编译器修改的常量。
    - iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每新增一行常量声明将使 iota 计数一次(iota 可理解为 const 语句块中的行索引)。
    - 第一个 iota 等于 0，每当 iota 在新的一行被使用时，它的值都会自动加 1；所以 a=0, b=1, c=2 可以简写为如下形式：

            const (
                a = iota
                b
                c
            )
    - 用法
    
            package main
            import "fmt"
            func main() {
                const (
                        a = iota   //0
                        b          //1
                        c          //2
                        d = "ha"   //独立值，iota += 1
                        e          //"ha"   iota += 1
                        f = 100    //iota +=1
                        g          //100  iota +=1
                        h = iota   //7,恢复计数
                        i          //8
                )
                fmt.Println(a,b,c,d,e,f,g,h,i)
            }

        out ：  `0 1 2 ha ha 100 100 7 8`

##### 再看个有趣的的 iota 实例：

    package main
    import "fmt"
    const (
        i=1<<iota
        j=3<<iota
        k
        l
    )

    func main() {
        fmt.Println("i=",i)
        fmt.Println("j=",j)
        fmt.Println("k=",k)
        fmt.Println("l=",l)
    }

- 以上实例运行结果为：

        i= 1
        j= 6
        k= 12
        l= 24
- iota 表示从 0 开始自动加 1，所以 i=1<<0, j=3<<1（<< 表示左移的意思），即：i=1, j=6，这没问题，关键在 k 和 l，从输出结果看 k=3<<2，l=3<<3。

        简单表述:

        i=1：左移 0 位,不变仍为 1;
        j=3：左移 1 位,变为二进制 110, 即 6;
        k=3：左移 2 位,变为二进制 1100, 即 12;
        l=3：左移 3 位,变为二进制 11000,即 24。

- 字符串类型在 go 里是个结构, 包含指向底层数组的指针和长度,`a = "hello" unsafe.Sizeof(a) ` 这两部分每部分都是 8 个字节，所以字符串类型大小为 16 个字节。

### 语言运算符
 - 运算符用于在程序运行时执行数学或逻辑运算。

##### 内置的运算符

 - 算术运算符 ： + - * / % 求余  ++ 自增 -- 自减 
 - 关系运算符 ： == != > < >= <=  
 - 逻辑运算符 ： 
    - `&&`: 逻辑 AND 运算符。 如果两边的操作数都是 True，则条件 True，否则为 False。
    - `||`: 逻辑 OR 运算符。 如果两边的操作数有一个 True，则条件 True，否则为 False。
    - `！`: 逻辑 NOT 运算符。 如果条件为 True，则逻辑 NOT 条件 False，否则为 True。
 - 位运算符 `假定 A 为60，B 为13`
    - `&`: 
        1. 按位与运算符"&"是双目运算符。 其功能是参与运算的两数各对应的二进位相与。
        2. (A & B) 结果为 12, 二进制为 0000 1100
    - `|`:
        1. 按位或运算符"|"是双目运算符。 其功能是参与运算的两数各对应的二进位相或
        2. (A | B) 结果为 61, 二进制为 0011 1101
    - `^`:
        1. 按位异或运算符"^"是双目运算符。 其功能是参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。
        2. (A ^ B) 结果为 49, 二进制为 0011 0001
    - `<<`:
        1. 左移运算符"<<"是双目运算符。左移n位就是乘以2的n次方。 其功能把"<<"左边的运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。
        2. A << 2 结果为 240 ，二进制为 1111 0000
    - `>>`:
        1. 右移运算符">>"是双目运算符。右移n位就是除以2的n次方。 其功能是把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数。
        2. A >> 2 结果为 15 ，二进制为 0000 1111
 - 赋值运算符
 - 其他运算符 `&` 返回变量存储地址， &a 返回变量的实际地址。  `*` 指针变量， *a 是一个指针变量
 - 运算符优先级： 由高到低
 `^ !` > `* / % << >> & &^` > `+ - | ^` >`== != < <= >= >` > `<-` > `&&` > `||`

 ### 语言条件语句
 
 ### 语言循环语句
 
 ### 语言函数
 - 函数是基本的代码块，用于执行一个任务。至少有个main()函数。
 - 函数定义： 函数定义格式：


    func function_name( [parameter list] ) [return_types] {
    函数体
    }
    
    -  func：函数由 func 开始声明
    -  function_name: 函数名称，函数名和列表一起构成了函数签名。
    - parameter list: 参数列表，参数就像一个占位符。当函数被调用时，你可以将值传递给参数，这个值被称为实际参数。参数列表指定的是参数类型、顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数。
    - return_type: 返回类型，函数返回一列值。return_types 是该列值的数据类型。有些功能不需要返回值，这种情况下 return_types 不是必须的。
    - 函数体： 函数定义的代码集合。

### 语言变量作用域
 - 作用域为已声明标识符所表示的常量、类型、变量、函数或包在源代码中的作用范围。
##### 变量可以在三个地方声明：
 - 函数内定义的变量成为局部变量
 - 函数外定义的变量为全局变量
 - 函数定义中的变量称为形式参数

 ### 语言数组
 - 数组是具有相同唯一类型的一组已编号且长度固定的数据项序列，这种类型可以是任意的原始类型例如整形、字符串或者自定义类型。
 - 声明数组：  Go 语言数组声明需要指定元素类型及元素个数， `var variable_name [SIZE] variable_type  eg:定义了数组 balance 长度为 10 类型为 float32  var balance [10] float32`
 - 初始化数组： `var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}`
    - 初始化数组中 {} 中的元素个数不能大于 [] 中的数字。
    - 如果忽略 [] 中的数字不设置数组大小，Go 语言会根据元素的个数来设置数组的大小： `var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}`
- 访问数组元素： 数组元素可以通过索引（位置）来读取。格式为数组名后加中括号，中括号中为索引的值。例如： `var salary float32 = balance[9]`
- 多维数组：`var variable_name [SIZE1][SIZE2]...[SIZEN] variable_type` `eg:  var threedim [5][10][4]int`
- 访问二维数组：` val := a[2][3] ` 或 `var value int = a[2][3]`
- 像函数传递数组： 
    - 方式1:

            void myFunction(param [10]int)
            {
            .
            .
            }
    
    - 方式2:

            void myFunction(param []int)
            {
            .
            .
            }

### 语言指针
- 变量是一种使用方便的占位符，用于引用计算机内存地址。Go 语言的取地址符是 &，放到一个变量前使用就会返回相应变量的内存地址。
- 一个指针变量指向了一个值的内存地址。类似于变量和常量，在使用指针前你需要声明指针。指针声明格式如下：`var var_name *var-type`
- var-type 为指针类型，var_name 为指针变量名，* 号用于指定变量是作为一个指针。

        以下是有效的指针声明： 
        var ip *int        /* 指向整型*/
        var fp *float32    /* 指向浮点型 */

- 指针使用流程：

            定义指针变量。
            为指针变量赋值。
            访问指针变量中指向地址的值。
            在指针类型前面加上 * 号（前缀）来获取指针所指向的内容。

            package main

            import "fmt"

            func main() {
            var a int= 20   /* 声明实际变量 */
            var ip *int        /* 声明指针变量 */

            ip = &a  /* 指针变量的存储地址 */

            fmt.Printf("a 变量的地址是: %x\n", &a  )

            /* 指针变量的存储地址 */
            fmt.Printf("ip 变量储存的指针地址: %x\n", ip )

            /* 使用指针访问值 */
            fmt.Printf("*ip 变量的值: %d\n", *ip )
            }
            
        - 以上实例执行输出结果为：

                a 变量的地址是: 20818a220
                ip 变量储存的指针地址: 20818a220
                *ip 变量的值: 20

- 空指针：当一个指针被定义后没有分配到任何变量时，它的值为 nil。 nil 指针也称为空指针。nil在概念上和其它语言的null、None、nil、NULL一样，都指代零值或空值。一个指针变量通常缩写为 ptr。
- 指针数组： 定义一个指针数组来存储地址
- 指针的指针： 支持指向指针的指针
- 函数传递指针参数： 通过引用或地址传参，在函数调取时可以改变其值。

### 语言结构体
- 数组可以存储同一类型的数据，但在结构体中我们可以为不同项定义不同的数据类型。
- 结构体是由一系列具有相同类型或不同类型的数据构成的数据集合。
- 定义结构体： 结构体定义需要使用 type 和 struct 语句。struct 语句定义一个新的数据类型，结构体有中有一个或多个成员。type 语句设定了结构体的名称。结构体的格式如下：

        type struct_variable_type struct {
        member definition;
        member definition;
        ...
        member definition;
        }
- 访问结构体成员： 要访问结构体成员，需要使用点号 . 操作符，格式为：`结构体.成员名`
- 结构体作为函数参数： 像其他数据类型一样将结构体类型作为参数传递给函数。并以以上实例的方式访问结构体变量
- 结构体指针：定义指向结构体的指针类似于其他指针变量，格式如下：   `var struct_pointer *Books`

### 语言切片
- 语言切片是对数组的抽象。
- 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go中提供了一种灵活，功能强悍的内置类型切片("动态数组"),与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。
- 定义切片： 
    - 可以声明一个未指定大小的数组来定义切片
    - 切片不需要说明长度 或 使用make()函数来创建切片
    - 可以指定容量，其中capacity为可选参数。 `make([]T, length, capacity)`  len 是数组的长度并且也是切片的初始长度。
- 切片初始化 `s :=[] int {1,2,3 } ` `s := arr[:] ` `s := arr[startIndex:endIndex] ` `s := arr[startIndex:] `
- len() 和 cap() 函数 : 切片是可索引的，并且可以由 len() 方法获取长度。 切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少。
- append() 和 copy() 函数: 拷贝切片的 copy 方法和向切片追加新元素的 append 方法

### 语言范围
- 语言中 range 关键字用于 for 循环中迭代数组(array)、切片(slice)、通道(channel)或集合(map)的元素。在数组和切片中它返回元素的索引和索引对应的值，在集合中返回 key-value 对的 key 值。

### 语言Map（集合）
- Map 是一种无序的键值对的集合。Map 最重要的一点是通过 key 来快速检索数据，key 类似于索引，指向数据的值。
- Map 是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，Map 是无序的，我们无法决定它的返回顺序，这是因为 Map 是使用 hash 表来实现的。
- 定义 Map ： 可以使用内建函数 make 也可以使用 map 关键字来定义 Map。
- 实例（部分）：

        func main() {
            var countryCapitalMap map[string]string /*创建集合 */
            countryCapitalMap = make(map[string]string)

            /* map插入key - value对,各个国家对应的首都 */
            countryCapitalMap [ "France" ] = "Paris"
            countryCapitalMap [ "Italy" ] = "罗马"
            countryCapitalMap [ "Japan" ] = "东京"
            countryCapitalMap [ "India " ] = "新德里"

            /*使用键输出地图值 */ for country := range countryCapitalMap {
                fmt.Println(country, "首都是", countryCapitalMap [country])
            }

            /*查看元素在集合中是否存在 */
            captial, ok := countryCapitalMap [ "美国" ] /*如果确定是真实的,则存在,否则不存在 */
            /*fmt.Println(captial) */
            /*fmt.Println(ok) */
            if (ok) {
                fmt.Println("美国的首都是", captial)
            } else {
                fmt.Println("美国的首都不存在")
            }
        }

- delete 函数：delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key。 eg： /*删除元素*/  `delete(countryCapitalMap, "France")`

###  语言递归函数
- 递归，就是运行的过程中调用自己。

        语法格式如下：

        func recursion() {
        recursion() /* 函数调用自身 */
        }

        func main() {
        recursion()
        }

- 语言支持递归。但我们在使用递归时，开发者需要设置退出条件，否则递归将陷入无限循环中。递归函数对于解决数学上的问题是非常有用的，就像计算阶乘，生成斐波那契数列等。

### 语言类型转换
 - 类型转换用于将一种数据类型的变量转换为另外一种类型的变量。 `type_name(expression)`
 - 将整型转化为浮点型  `var sum int = 17 ` ` float32(sum)`

 ### 语言接口
 - 语言提供了另外一种数据类型即接口，它把所有的具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口。
 - 实例1：


        /* 定义接口 */
        type interface_name interface {
        method_name1 [return_type]
        method_name2 [return_type]
        method_name3 [return_type]
        ...
        method_namen [return_type]
        }

        /* 定义结构体 */
        type struct_name struct {
        /* variables */
        }
        /* 实现接口方法 */
        func (struct_name_variable struct_name) method_name1() [return_type] {
        /* 方法实现 */
        }
        ...
        func (struct_name_variable struct_name) method_namen() [return_type] {
        /* 方法实现*/
        }

- 实例2:


        package main
        import (
            "fmt"
        )
        type Phone interface {
            call()
        }
        type NokiaPhone struct {
        }
        func (nokiaPhone NokiaPhone) call() {
            fmt.Println("I am Nokia, I can call you!")
        }
        type IPhone struct {
        }
        func (iPhone IPhone) call() {
            fmt.Println("I am iPhone, I can call you!")
        }
        func main() {
            var phone Phone

            phone = new(NokiaPhone)
            phone.call()

            phone = new(IPhone)
            phone.call()

        }

- 我们定义了一个接口Phone，接口里面有一个方法call()。然后我们在main函数里面定义了一个Phone类型变量，并分别为之赋值为NokiaPhone和IPhone。然后调用call()方法，输出结果如下：


        I am Nokia, I can call you!
        I am iPhone, I can call you!

### 语言错误处理
 - error类型是一个接口类型，这是它的定义：
       
        type error interface {
        Error() string
        }
- 可以在编码中通过实现 error 接口类型来生成错误信息。
- 函数通常在最后的返回值中返回错误信息。使用errors.New 可返回一个错误信息：

        func Sqrt(f float64) (float64, error) {
            if f < 0 {
                return 0, errors.New("math: square root of negative number")
            }
            // 实现
        }

- 实例：在调用Sqrt的时候传递的一个负数，然后就得到了non-nil的error对象，将此对象与nil比较，结果为true，所以fmt.Println(fmt包在处理error时会调用Error方法)被调用，以输出错误，请看下面调用的示例代码：

        result, err:= Sqrt(-1)

        if err != nil {
        fmt.Println(err)
        }

### 并发
- 支持并发，只需要通过 go 关键字来开启 goroutine 即可。 goroutine 是轻量级线程，goroutine 的调度是由 Golang 运行时进行管理的
- 多线程会引入线程之间的同步问题，在 golang 中可以使用 channel 作为同步的工具。
- 通过 channel 可以实现两个 goroutine 之间的通信。
- goroutine 语法格式：`go 函数名( 参数列表 )` `eg: go f(x,y,z)`
- go 语句开启一个新的运行期线程， 即 goroutine，以一个不同的、新创建的 goroutine 来执行一个函数。 同一个程序中的所有 goroutine 共享同一个地址空间。
- channel: 通道（channel）是用来传递数据的一个数据结构。通道可用于两个 goroutine 之间通过传递一个指定类型的值来同步运行和通讯。操作符 <- 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。

        ch <- v    // 把 v 发送到通道 ch
        v := <-ch  // 从 ch 接收数据
                // 并把值赋给 v

- 声明一个通道,使用chan关键字即可，通道在使用前必须先创建： `ch := make(chan int)`  注意：默认情况下，通道是不带缓冲区的。发送端发送数据，同时必须又接收端相应的接收数据。
- 通道缓冲区,通道可以设置缓冲区，通过 make 的第二个参数指定缓冲区大小：`ch := make(chan int, 100)`

    - 带缓冲区的通道允许发送端的数据发送和接收端的数据获取处于异步状态，就是说发送端发送的数据可以放在缓冲区里面，可以等待接收端去获取数据，而不是立刻需要接收端去获取数据。

    - 不过由于缓冲区的大小是有限的，所以还是必须有接收端来接收数据的，否则缓冲区一满，数据发送端就无法再发送数据了。

    - 注意：如果通道不带缓冲，发送方会阻塞直到接收方从通道中接收了值。如果通道带缓冲，发送方则会阻塞直到发送的值被拷贝到缓冲区内；如果缓冲区已满，则意味着需要等待直到某个接收方获取到一个值。接收方在有值可以接收之前会一直阻塞。
- 遍历通道与关闭通道
    - 通过 range 关键字来实现遍历读取道的数据，类似于与数组或切片。格式如下：`v, ok := <-ch`
    - 如果通道接收不到数据后 ok 就为 false，这时通道就可以使用 close() 函数来关闭。
