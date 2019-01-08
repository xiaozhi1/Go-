# Go程序设计语言

## 序言
> 优点：有垃圾回回收、包系统、一等函数公民、词法作用域、系统调用接口、默认UTF-8编码不可变字符串。

> 缺点：没有隐式数值类型强制转换、没有构造或析构函数、没有运算符重载、没有形参默认值、没有继承、没有泛型、没有异常、没有宏、没有函数注解、没有线程局部存储。

[本书代码](https://www.gopl.io/) `gopl.io`

[go官网](https://golang.org)  `https://golang.org`

[线上运行](https://play.golang.org/) `play.golang.org`

## 第一章 入门


### 1.1 hello,world
Go是编译型的语言。Go的工具链江程序的源文件转变成机器相关的原生二进制指令。

最简单的子命令是`run`，它将一个或多个以`.go`为后缀的源文件进行编译、链接，然后运行生成的可执行文件（本章使用`$`符合作为命令提示符）

` $ go run helloworld.go ` 输出 `hello，世界`

`Go`支持`Unicode`，所以它可以处理所以国家的语言。

如果这个程序不是一次性的实验，那么编译输出成一个可复用的程序比较。
可以通过`go build`来实现：` go build helloworld.go `
这条命令生成一个叫作`helloworld`的二进制程序，它可以不用进行任何处理，随时执行： ` $ ./helloworld`

` go run gopl.io/ch1/helloworld/main.go ` 会把源代码取到相对于的目录，后面讨论。

一般一个包由一个或多个`.go`源文件组成，放在一个文件夹中，该文件夹的名字描述了包的作用。
每个源文件的开始都用`package`声明， `helloworld.go` 文件中的 `package main` 指明了这个文件属于 `main` 包的。后面跟着它导入的其他包的列表，然后是存储在文件中的程序声明。

`fmt`包中的函数用来格式化输出和扫描输入。

名为 `main` 包比较特殊。它是一个独立的可执行程序，不是库。 `main` 包的 `main` 函数也是特殊的， `mian` 是程序开始执行的地方。当然 `main` 通常调用其他包中的函数做更多事，比如`fmt.Println`

必须准确的导入需要的包，不然会报错。`import`声明必须跟在`package`声明之后。

一个函数有`func`关键字、函数名、参数列表、返回列表、放在大括号内的函数体组成。

例： `{`符合必须和关键字`func`在同一行，不能单独成行。`x+y`表达式中，换行符可以在`+`操作符后面，但不能再`+`前面

`gofmt`工具将代码以标准形式重写，养成对代码使用`gofmt`的习惯
`go get golang.org/x/tools.cmd.goimports`可以获取配置






### 1.2 命令行参数

```
    package main

    import (
        "fmt"
        "os"
    )

    func main() {
        var s, sep string
        for i := 1; i < len(os.Args); i++ {
            s += sep + os.Args[i]
            sep = " "
        }
        fmt.Println(s)
    }
```
`os`包提供一些函数和变量，命令行参数以`os`包中的`Arag`名字的变量供程序访问，在`os`包外面，使用`os.Args`这个名字

变量`os.Args`是一个字符串`slice`。它是一个动态容量的顺序数组s。`s[m:n]`访问一段区间，`go`是使用半开区间，即包含第一个索引，不包含最后一个索引。
例： `slice s[m:n]` 包含`n-m`个元素。
表达式`s[m:n]`表示一个从`m`到`n-1`个元素的`slice`

如果变量什么的时候没有明确的初始化，那么会隐式的初始化这个类型的空值，例：数组初始化的`0`，字符串初始化是空字符串`""`

循环的索引变量  ` i ` 在 ` for ` 循环开始处声明。 ` := ` 是段变量声明
递增语句 ` i++ ` 等价于 ` i += `  ，是语句，不是表达式，所以 ` j = i++ ` 不合法

` for ` 是 ` Go ` 唯一的循环语句 三个组成部分两边不用小括号，大括号是必须的。

如果省略 ` for ` 的三个组件就是传统的无限循环了。
```
    for {
        // ...
    }
```

` range ` 会产生一对值：索引值和这个索引处元素的值，不需要可以用 ` 空标识符 _ ` 替换
```
    package main
    
    import (
        "fmt"
        "os"
    )

    func main() {
        s, sep := "", ""
        for _, arg := range os.Args[1:] {
            s += sep + arg
            sep = " "
        }
        fmt.Println(s)
    }
```

