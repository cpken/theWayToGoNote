### 第四章 基本结构和基本数据类型

- 4.1 文件名、关键字与标识符
- 4.2 Go 程序的基本结构和要素
- 4.3 常量
- 4.4 变量
- 4.5 基本类型和运算符
- 4.6 字符串
- 4.7 strings 和 strconv 包
- 4.8 时间和日期
- 4.9 指针


#### 4.1 文件名、关键字与标识符

Go 文件命名规则
- 小写字母
- 小写字母+下划线
- 文件名不包含空格或其他特殊字符

~~~
scanner.go
scanner_test.go
~~~

Go 语言区分大小写。  

以下是无效的标识符：
- 1ab（以数字开头）
- case （Go 语言的关键字）
- a+b（运算符是不允许的）

*_* 被称为空白标识符。可用于变量的声明或赋值（任何类型都可赋值于它），但赋予的值将被抛弃，因此这些值不能在后续的代码中使用，也不可以使用这个标识符作为变量对其它变量进行赋值或运算。  

#### Go 的 25 个关键字或保留字

<table>
    <tbody>
        <tr>
            <td>break</td>
            <td>default</td>
            <td>func</td>
            <td>interface</td>
            <td>select</td>
        </tr>
        <tr>
            <td>case</td>
            <td>defer</td>
            <td>go</td>
            <td>map</td>
            <td>struct</td>
        </tr>
        <tr>
            <td>chan</td>
            <td>else</td>
            <td>goto</td>
            <td>package</td>
            <td>switch</td>
        </tr>
        <tr>
            <td>const</td>
            <td>fallthrough</td>
            <td>if</td>
            <td>range</td>
            <td>type</td>
        </tr>
        <tr>
            <td>continue</td>
            <td>for</td>
            <td>import</td>
            <td>return</td>
            <td>var</td>
        </tr>
    </tbody>
</table>


#### Go 的 36 个预定义标识符

<table>
    <tbody>
        <tr>
            <td>append</td>
            <td>bool</td>
            <td>byte</td>
            <td>cap</td>
            <td>close</td>
            <td>complex</td>
            <td>complex64</td>
            <td>complex128</td>
            <td>uint16</td>
        </tr>
        <tr>
            <td>copy</td>
            <td>false</td>
            <td>float32</td>
            <td>float64</td>
            <td>imag</td>
            <td>int</td>
            <td>int8</td>
            <td>int16</td>
            <td>uint32</td>
        </tr>
        <tr>
            <td>int32</td>
            <td>int64</td>
            <td>iota</td>
            <td>len</td>
            <td>make</td>
            <td>new</td>
            <td>nil</td>
            <td>panic</td>
            <td>uint64</td>
        </tr>
        <tr>
            <td>print</td>
            <td>println</td>
            <td>real</td>
            <td>recover</td>
            <td>string</td>
            <td>true</td>
            <td>uint</td>
            <td>uint8</td>
            <td>uintptr</td>
        </tr>
    </tbody>
</table>


程序一般由关键字、常量、变量、运算符、类型和函数组成。  

程序中用到的分隔符：<code>()</code>、<code>[]</code>、<code>{}</code>。  

程序中用到的标点符号：<code>.</code>、<code>,</code>、<code>;</code>、<code>:</code> 和 <code>...</code>。  

Go 语言不鼓励多行代码写在同一行。


#### 4.2. Go 程序的基本结构和要素
##### 4.2.1 包的概念、导入与可见性

程序由包组成，包可是自身或从外部导入。  
源文件第一行必须要指明该文件属于哪个包，如：<code>package main</code>。<code>package main</code> 表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 
<code>main</code> 的包。  
所有的包名都应该使用小写字母。

##### 标准库

可以直接使用的包称为标准库。  

标准库目录

| windows        | Linux 64位      |  Linux 32位    |
| :------------: |:---------------:| :------------:|
| pkg\windows_386| pkg\linux_amd64 | pkg\linux_386 |

标准包存放在 <code>$GOROOT/pkg/$GOOS_$GOARCH/</code> 目录下。  

属于同一个包的源文件必须全部被一起编译，一个包即是编译时的一个单元，因此根据惯例，每个目录都只包含一个包。  

如果对一个包进行更改或重新编译，所有引用了这个包的客户端程序都必须全部重新编译。  

Go 中的包模型采用了显示依赖关系的机制来达到快速编译的目的，编译器会从后缀名为 <code>.o</code> 的对象文件（需要且只需要这个文件）中提取传递依赖类型的信息。  

如果 <code>A.go</code> 依赖 <code>B.go</code>，而 <code>B.go</code> 又依赖 <code>C.go</code>：  
- 编译<code>C.go</code>，<code>B.go</code>，然后是 <code>A.go</code>。
- 为了编译 <code>A.go</code>，编辑器读取的是 <code>B.go</code> 而不是 <code>C.go</code>。

这种编译机制对于编译大型的项目时可以显著地提升编译速度。  

每一段代码只会被编译一次。  

一个 Go 程序是通过 <code>import</code> 关键字将一组包链在一起。  

导入多个包：
~~~go
import "fmt"
import "os"
~~~
或
~~~go
import "fmt"; import "os"
~~~

但是还有更短且更优雅的方法（被称为因式分解关键字，该方法同样适用于 const、var 和 type 的声明或定义）：
~~~go
import (
  "fmt"
  "os"
)
~~~

更短的形式，但使用 gofmt 后将会被强制换行：
~~~go
import ("fmt"; "os")
~~~

如果包名不是以 <code>.</code> 或 <code>/</code> 开头，如 <code>"fmt"</code> 或者 <code>"container/list"</code>，则 Go 会在全局文件进行查找；如果包名以
 <code>./</code> 开头，则 Go 会在相对目录中查找；如果包名以 <code>/</code> 开头（在 Windows 
下也可以这样使用），则会在系统的绝对路径中查找。  

导入包即等同于包含了这个包的所有的代码对象。  

除了符号 <code>_</code>，包中所有代码对象的标识符必须是唯一的，以避免名称冲突。但是相同的标识符可以在不同的包中使用，因为可以使用包名来区分它们。  


##### 可见性规则

当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 
public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 private ）。  

包也可以作为命名空间使用，帮助避免命名冲突（名称冲突）：两个包中的同名变量的区别在于他们的包名，例如 <code>pack1.Thing</code> 和 <code>pack2.Thing</code>。  

包名冲突可以通过设置包的别名来解决。

~~~go
package main

import fm "fmt" // alias3

func main() {
   fm.Println("hello, world")
}
~~~

##### 注意事项
如果你导入一个包而没有使用它，会在构建程序时报错，如 "imported and not used: os"，这正是遵循了 Go 的格言："没有不必要的代码！"。  

##### 包的分级声明和初始化

在使用 <code>import</code> 导入包之后定义或声明 0 个或多个常量（const）、变量（var）和类型（type），这些对象的作用域都是全局的（在本包范围内）。  

#### 4.2.2 函数

函数定义：

~~~go
func functionName()
~~~

在括号 <code>()</code> 中写入 0 个或多个函数的参数（使用逗号 <code>,</code> 分隔），每个参数的名称后面必须紧跟其类型。  

<code>main</code> 函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数（如果有 <code>init ()</code> 函数则会先执行该函数）。
如果你的 <code>main</code> 包的源代码没有包含 <code>main</code> 函数，则会引发构建错误 "undefined: main.main。main" 函数既没有参数，也没有返回类型（与 C 
家族中的其它语言恰好相反）。
如果你不小心为 <code>main</code> 函数添加了参数或者返回类型，将会引发构建错误：

~~~
func main must have no arguments and no return values results.
~~~

在程序开始执行并完成初始化后，第一个调用（程序的入口点）的函数是 <code>main.main()</code>（如：C 语言），该函数一旦返回就表示程序已成功执行并立即退出。  

函数里的代码（函数体）使用大括号 <code>{}</code> 括起来。

左大括号 <code>{</code> 必须与方法的声明放在同一行，这是编译器的强制规定，否则你在使用 gofmt 时就会出现错误提示：

~~~
`build-error: syntax error: unexpected semicolon or newline before {`
~~~

（这是因为编译器会产生 <code>func main() ;</code> 这样的结果，很明显这错误的）  


Go 语言虽然看起来不使用分号作为语句的结束，但实际上这一过程是由编译器自动完成，因此才会引发像上面这样的错误。

右大括号 <code>}</code> 需要被放在紧接着函数体的下一行。如果你的函数非常简短，你也可以将它们放在同一行：

~~~go
func Sum(a, b innt) int { return a + b }
~~~


只有当某个函数需要被外部包调用的时候才使用大写字母开头，并遵循 Pascal 命名法；否则就遵循骆驼命名法，即第一个单词的首字母小写，其余单词的首字母大写。  

当被调用函数的代码执行到结束符 <code>}</code> 或返回语句时就会返回，然后程序继续执行调用该函数之后的代码。  

程序正常退出的代码为 0 即 "Program exited with code 0"；如果程序因为异常而被终止，则会返回非零值，如：1。这个数值可以用来测试是否成功执行一个程序。


#### 4.2.3 注释

<code>//</code> 单行注释。  
<code>/**/</code> 多行注释【块注释】。  

多行注释不可以嵌套使用，多段注释之间应该用空行分阁加以区分。  

几乎所有全局作用域的类型、常量、变量、函数和被导出的对象都应该有一个合理的注释。如果这种注释（称为文档注释）出现在函数面前，例如函数 Abcd，则要以 <code>"Abcd...""</code>。


#### 4.2.4 类型

使用 var 声明的变量的值会自动初始化为该类型的的零值。  

类型定义了某个变量的值的集合与对其进行操作的集合。

#### 基本类型

- int
- float
- bool
- string


#### 结构化（复合）类型

- struct
- array
- slice
- map
- channel


#### 描述类型的行为

- interface


结构化的类型没有真正的值，它使用 nil 作为默认值。Go 语言中不存在类型继承。  

函数的返回类型声明要写在函数名和可选的参数列表之后，例如：

~~~go
func FunctionName (a typea, b typeb) typeFunc
~~~

你可以在函数体的某处返回使用类型为 typeFunc 的变量 var：
~~~go
return var
~~~

一个函数可以拥有多返回值，返回类型之间需要使用逗号分割，并使用小括号 <code>()</code> 将它们括起来，如：

~~~go
func FunctionName (a typea, b typeb) (t1 type1, t2 type2)
~~~

返回的形式：

~~~go
return var1, var2
~~~

这种多返回值一般用于判断某个函数是否执行成功（true/false）或与其它返回值一同返回错误消息。  

使用 type 关键字定义类型，也可以定义已存在的类型的别名，如：

~~~go
type IZ int
~~~

这里并不是真正意义上的别名，因为使用这种方法定义之后的类型可以拥有更多的特性，且在类型转换时必须显示=式转换。  

下面的声明方式：

~~~go
var a IZ = 5
~~~

int 是变量 a 的底层类型，这也使得它们之间存在相互转换的可能。  

如果你有多个类型需要定义，可以使用因式分解关键字的方式，例如：

~~~go
type (
  IZ  int
  FZ  float64
  STR string
)
~~~

每个值都必须在经过编译后属于某个类型（编译器必须能够推断出所有值的类型），因为 Go 语言是一种静态型语言。


#### 4.2.5 Go 程序的一般结构

示例 4.4 gotemplate.go

~~~go
package main

import (
  "fmt"
)

const c = "C"

var v int = 5

type T struct{}

func init() { // initialization of package
}

func main() {
  var a int
  Func1()
  // ...
  fmt.Println(a)
}

func (t T) Method1() {
  // ...
}

func Func1() {// exported function Func1
  // ...
}
~~~

Go 程序的执行（程序启动）顺序如下：

1. 按顺序导入所有被 main 包引用的其它包，然后在每个包中执行如下流程：

2. 如果该包又导入了其它的包，则从第一步开始递归执行，但是每个包只会被导入一次。

3. 然后以相反的顺序在每个包中初始化常量和变量，如果该包含有 init 函数的话，则调用该函数。

4. 在完成这一切之后，main 也执行同样的过程，最后调用 main 函数开始执行程序。


#### 4.2.6 类型转换

Go 语言不存在隐式转换。

~~~go
valueOfTypeB = typeB(valueOfTypeA)
~~~

类型 B 的值 = 类型 B（类型 A 的值）  

示例：

~~~go
a := 5.0
b := int(a)
~~~

以上代码只能在定义正确的情况下转黄成功。
例如从一个取值范围较小的类型转换到一个取值范围较大的类型（例如将 int16 转换为 int32）。
当从一个取值范围较大的转换到取值范围较小的类型时（例如将 int32 转换为 int16 或将 float32 转换为 int），会发生精度丢失（截断）的情况。
当编译器捕捉到非法的类型转换时会引发编译时错误，否则将引发运行时错误。  

具有相同底层类型的变量之间可以互相转换：

~~~go
var a IZ = 5
c := int(a)
d := IZ(c)
~~~


#### 4.2.7 Go 命名规范

返回某个对象的函数或方法的名称一般使用名词，没有 <code>Get...</code> 之类的字符。  

修改对象，使用 <code>SetName</code>。  

大小写混合，如 MixedCaps 或 mixedCaps，不使用下划线分隔多个名称。


#### 4.3 常量

常量使用关键字 <code>const</code> 定义，用于存储不会改变的数据。  

存储在常量中的数据类型
- 布尔型
- 数字型（整数型、浮点型、复数）
- 字符串型 

常量的定义格式
~~~go
const identifier [type] = value
const Pi = 3.1415926
~~~

在 Go 语言中，可以省略类型说明符 <code>[type]</code>，因为编译器可以根据变量的值来推断类型。

- 显示类型定义：<code>const b string = "abc""</code>
- 隐式类型定义：<code>const b = "abc""</code>

一个没有指定类型的常量被使用时，会根据其使用环境而推断出它所需要具备的类型。
换句话说，未定义类型的常量会在必要时刻根据上下文来获得相关类型。

~~~go
var n int
f(n + 5) // 无类型的数字型常量 "5" 它的类型在这里变成了 int
~~~

常量的值必须在编译时就确定。

- 正确的做法：<code>const c1 = 2/3</code>
- 错误的做法：<code>const c2 = getNumber()</code> // 引发构建错误：<code>getNumber() used as value</code>

因为在编译期间自定义函数均属于未知，因此无法用于常量的赋值，但内置函数可以使用，如：len()。  

数字型的常量是没有大小和符号的，并且可以使用任何精度而不会导致溢出：

~~~go
const Ln2= 0.693147180559945309417232121458\
            176568075500134360255254120680009
const Log2E= 1/Ln2 // this is a precise reciprocal
const Billion = 1e9 // float constant
const hardEight = (1 << 100) >> 97
~~~

反斜杠 <code> \ </code> 可以在常量表达式中作为多行的连接符使用。  

常量赋值给一个精度过小的数字型变量时，会导致数值溢出，而引发错误。  

常量允许并行赋值的形式：

~~~go
const beef, two, c = "eat", 2, "veg"
const Monday, Tuesday, Wednesday, Thursday, Friday, Saturday = 1, 2, 3, 4, 5, 6
const (
    Monday, Tuesday, Wednesday = 1, 2, 3
    Thursday, Friday, Saturday = 4, 5, 6
)
~~~

常量枚举：

~~~go
const (
  Unknown = 0
  Female  = 1
  Male    = 2
)
~~~

例子，<code>iota</code> 可以被用作枚举值：

~~~go 
const (
  a = iota
  b = iota
  c = iota 
)
~~~

在没遇到一个新的常量块或单个常量声明时，<code>iota</code> 都会重置为0（每遇到一次 const 关键，iota 就重置为 0）。  

常量是恒定不变的，无法在程序运行过程中修改它的值；修改怎会引发编译错误。  


#### 4.4 变量

变量声明 <code>var</code> 关键字：<code>var identifier type</code>。  

Go 声明变量时将变量的类型放在变量的名称的之后。  

指针类型声明：

~~~go
var a, b *int
~~~

示例

~~~go
var a   int
var b   bool
var str string
~~~

另一种写法：

~~~go
var (
  a   int
  b   bool
  str string
)
~~~

这种因式分解关键字的写法一般用于声明全局变量。  

当一个变量被声明之后，系统自动赋予它该类型的零值：int 为 0，float 为 0.0，bool 为 false，string 为空字符串，指针为 nil。
记住，所有的内存在 Go 中都是经过初始化的。  

变量命名规则：驼峰命名法，首字母小写，每个新单词的首字母大写。如 <code>numShips</code>。  

全局变量能够被外部所使用，需要将首字母也大写。


一个变量（常量、类型或函数）在程序中都有一定的作用范围，称之为作用域。
如果一个变量在函数体外声明，则被认为是全局变量，可以在整个包甚至外部包（被导出后）使用，不管你声明在哪个源文件里或在哪个源文件里调用该变量。  

在函数体内声明的变量称之为局部变量，它们的作用域只在函数体内，参数和返回值变量也是局部变量。
一般情况下，局部变量的作用域可以通过代码块（用大括号括起来的部分）判断。  

变量的标识符必须是唯一的。  

如果内部代码块出现了与外部相同名称的变量，则会使用内部代码块的变量的值（结果内部代码块的执行后隐藏的外部同名变量又会出现，而内部同名变量则被释放），只会影响内部代码块的局部变量。

变量可以编译期间就被赋值，赋值给变量使用运算符号 <code>=</code>，也可以在运行时对变量进行赋值操作。  

示例：

~~~go
a = 15
b = false
~~~

一般情况下，当变量 a 和变量 b 之间类型相同时，才能进行如 <code>a = b</code> 的赋值。  

声明与赋值（初始化）语句也可以组合起来。
示例：

~~~go
var identifier [type] value
var a int = 15
var i = 5
var b bool = false
var str string = "Go says hello to the world~"
~~~

Go 在编译时就完成了类型推断过程，使用下面的代码形式来声明及初始化变量：

~~~go
var a   = 15
var b   = false
var str = "Go says hello to the world!" 
~~~

或：

~~~go
var (
  a        = 15
  b        = false
  str      = "Go says hello to the world!"
  numShips = 50
  city string
)
~~~

自动推断类型非任何时候都适用，当你想要给变量的类型并不是自动推断出的某种类型时，你需要显式指定变量的类型，例如：

~~~go
var n int64 = 2
~~~

<code>var a</code> 语法是错误的，因为编辑器无自动推断类型的一句。  

声明包级别的全局变量，例如：

~~~go
var (
  HOME   = os.Getenv("HOME")
  USER   = os.Getenv("USER")
  GOROOT = os.Getenv("GOROOT")  
)
~~~

在函数体内声明局部变量，可以使用简短声明语法 <code>:=</code>，例如：

~~~go
a := 1
~~~

下面例子
- <code>runtime</code> 获取操作系统类型
- <code>os</code> 包中的 <code>os.Getenv()</code> 获取环境变量中的值，并保存到 string 类型的局部变量 path 中。

~~~go
package main

import (
  "fmt"
  "runtime"
  "os"
)

func main() {
  var goos string = runtime.GOOS
  fmt.Printf("The operating system is: %s\n", goos)
  path := os.Getenv("PATH")
  fmt.Printf("Path is %s\n", path)
}
~~~


#### 4.4.2 值类型和引用类型

通过 &i 来获取变量 i 的内存地址。值类型的变量的值存储在栈中。  

内存地址被称为指针。  

同一个引用类型的指针指向的多个字可以是在连续的内存地址中（内存布局是连续的），
也会将这些字分散存放在内存中，每个字都指示了下一个字所在的内存地址。  

当使用赋值语句 <code>r2 = r1</code> 时，只有引用（地址）被复制。  

如果 r1 的值被改变了，这个值的所有引用都会指向被修改后的内容。  

在 Go 语言中，指针属于引用类型。  

其他引用类型
- slices
- maps
- channel

被引用的变量会存储在堆中，以便进行垃圾回收，且比栈拥有更大的内存空间。


#### 4.4.3 打印

函数 <code>Printf</code> 主要用于打印输出到控制台。大写字母表示可以外部使用。  

~~~go
func Printf(format string, list of variables to be printend)
~~~

格式化字符串可以含有一个或多个的格式化标识符。

- <code>%..</code> 可以被不同类型所对应的标识符替换
- <code>%s</code> 代表字符串标识符
- <code>%v</code> 代表使用类型的默认输出格式的标识符

这些标识符所对应的值从格式化字符串后的第一个逗号开始按照相同顺序添加，如果参数超过 1 个则同样需要使用逗号分隔。  


#### 4.4.4 简短形式，使用 := 赋值操作符

<code>:=</code> 赋值操作符只能被用在函数体内，而不能用于全局变量的声明与赋值。
其可以高效地创建一个新的变量，称之为初始化声明。

##### 注意事项

在同一代码块中，不可以再次对于相同名称的变量使用初始化声明，例如：<code>a := 50</code>，再进行 <code>a := 20</code>不是不允许的，
编辑器会提示错误 <code>no new variables on left side of :=</code>，但是 <code>a = 20</code> 是可以的，
因为这是给相同的变量赋予一个新的值。  

如果你在定义变量 a 之前使用它，则会得到编译错误 <code>undefined: a</code>。  

如果你声明一个局部变量却没有在相同的代码块中使用它，同样会得到编译错误，例如下例当中的变量 a：

~~~go
func main() {
  var a string = "abc"
  fmt.Println("hello, world")
}
~~~

尝试编译这段代码将得到错误 <code>a declared and not used</code>。  

此外，单纯地给 a 赋值也是不够的，这个值必须被使用，所以使用 <code>fmt.Println("hello, world", a)</code> 会移除错误。  

但是全局变量是允许声明但不使用。

简短形式的变量声明：

~~~go
var a, b, c int
~~~

多变量同一行赋值：

~~~go
a, b, c = 5, 7, "abc"
~~~

或

~~~go
a, b, c := 5, 7, "abc"
~~~

右边的这些值以相同的顺序赋值给左边的变量，所以 a 的值是 <code>5</code>，b 的值是 <code>7</code>，c 的值是 <code>"abc"</code>。

这被称为 **并行** 或 **同时** 赋值。  

如果你想要交换两个变量的值，则可以简单地使用 <code>a, b = b, a</code>。  

（在 GO 语言中，这样省去了使用交换函数的必要）  

空白标识符 <code> _ </code> 也被用于抛弃值，如值 <code>5</code> 在：<code>_, b = 5, 7</code> 中被抛弃。  

<code>_</code> 是一个只写变量，不能得到它的值。因为 Go 语言中必须使用所有被声明的变量，但是有时并不需要使用从一个函数得到的所有返回值。  

并行赋值也被用于当一个函数返回多个返回值时，
比如这里的 <code>val</code> 和错误 <code>err</code> 是通过调用 <code>Func1</code> 函数同时得到：<code>val, err 
= Func1(var1)</code>。


#### 4.4.5 init 函数

init 函数会在每个包完成初始化后自动执行，并且执行优先级高于 main 函数。  

每个源文件都只能包含一个 init 函数。初始化总是以单线程执行，并且按照包的依赖关系顺序执行。  

示例 4.6 init.go ：

~~~go
package trans

import "math"

var Pi float64

func init() {
  Pi = 4 * math.Atan(1) // init() function computes Pi
}
~~~

在它的 init 函数中计算变量 Pi 的初始值。  

示例 4.7 user_init.go 中导入了包 trans（需要 init.go 目录为 ./trans/init.go）并且使用到了变量 Pi：

~~~go
package main

import (
  "fmt"
  "./trans"
)

var twoPi = 2 * trans.Pi

func main() {
  fmt.Printf("2*Pi = %g\n", twoPi) // 2*Pi = 6.283185307179586
}
~~~

init 函数也经常被用在当一个程序开始之前调用后台执行的 goroutine，如下例子中的 <code>backend</code>：

~~~go
func init() {
  // setup preparations
  go backend()
}
~~~


#### 4.5 基本类型和运算符

一元运算符只可以用于一个值的操作（作为后缀），而二元运算符则可以和两个值或者操作数结合（作为中缀）。

#### 4.5.1 布尔类型 bool

布尔型的值只可以是常量 true 或 false。  

运算符相等 <code>==</code> 或不等 <code>!=</code> 进行比较并获得一个布尔型的值。在比较时，会同时比较类型与值，
相同返回 true，反则 false。  

示例：

~~~go
var aVar = 10
aVar == 5  -> false
aVar == 10 -> true
~~~

Go 对于值之间的比较有非常严格的限制，只有两个类型相同的值才可以进行比较，
接口比较必须都实现了相同的接口。
常量，必须和该常量类型相兼容的。
如果条件都不满足，则其中一个值的类型必须在被转换为和另外一个值的类型相同之后才可以进行比较。  

Go 语言中包含以下逻辑运算符：  

**非**运算符：<code>!</code>

~~~go
!T -> false
!F -> true
~~~

**非**运算符用于取得和布尔值相反的结果。  

**和**运算符：<code>&&</code>

~~~go
T && T -> true
T && F -> false
F && T -> false
F && F -> false
~~~

只有当两边的值都为 true 的时候，**和**运算符的结果才是 true。  

**或**运算符：<code>||</code>

~~~go
T || T -> true
T || F -> true
F || T -> true
F || F -> false
~~~

只有当两边的值都为 false 的时候，**或**运算符的结果才是 false，其中任意一边的值为 true 就能够使得该表达式的结果为 true。  

在 Go 语言中，&& 和 || 是具有快捷性质的运算符，当运算符左边表达式的值已经能够决定整个表达式的值的时候（&& 左边的值为 false，|| 左边的值为 true），运算符右边的表达式将不会被执行。
利用这个性质，如果你有多个条件判断，应当将计算过程较为复杂的表达式放在运算符的右侧以减少不必要的运算。  

利用括号同样可以升级某个表达式的运算优先级。  

在格式化输出时，你可以使用 %t 来表示你要输出的值为布尔型。  

布尔值（以及任何结果为布尔值的表达式）最常用在条件结构的条件语句中，例如：if、for 和 switch 结构（第 5 章）。  

对于布尔值的好的命名能够很好地提升代码的可读性，例如以 <code>is</code> 或者 <code>Is</code> 开头的 
<code>isSorted</code>、<code>isFinished</code>、<code>isVisible</code>，使用这样的命名能够在阅读代码的获得阅读正常语句一样的良好体验，例如标准库中的 
<code>unicode.IsDigit(ch)</code>（第 4.5.5 节）。


#### 4.5.2 数字类型

#### 4.5.2.1  整型 int 和浮点型 float

Go 语言支持整型和浮点型数字，并且原生支持复数，其中位的运算采用补码。  

Go 基于架构的类型，例如：int、uint 和 uintptr。  

架构类型的长度是根据运行程序所在的操作系统类型所决定的：

- <code>int</code> 和 <code>uint</code> 在 32 位操作系统上，均使用 32 位（4 个字节），在 64 位操作系统上，均使用 64 位（8 个字节）。
- <code>uintptr</code> 的长度被设定为足够存放一个指针即可。

Go 语言中没有 float 类型。 // 应该是 go 没有 double 类型吧？  

##### 整数：

- int8（-128 -> 127）
- int16（-32768 -> 32767）
- int32（-2,147,483,648 -> 2,147,483,647）
- int64（-9,223,372,036,854,775,808 -> 9,223,372,036,854,775,807）


##### 无符号整数：

- uint8（0 -> 255）
- uint16（0 -> 65,535）
- uint32（0 -> 4,294,967,295）
- uint64（0 -> 18,446,744,073,709,551,615）


##### 浮点型（IEEE-754 标准）：

- float32（+- 1e-45 -> +- 3.4 * 1e38）
- float64（+- 5 1e-324 -> 107 1e308）


int 型是计算最快的一种类型。  

整型的零值为 0，浮点型的零值为 0.0。  

float32 精确到小数点后 7 位，float64 精确到小数点后 15 位。  

应该尽可能地使用 float64，因为 math 包中所有有关数学运算的函数都会要求接收这个类型。  

通过增加前缀 0 来表示 8 进制数（如：077），增加前缀 0x 来表示 16 进制数（如：0xFF），以及使用 e 来表示 10 的连乘（如： 1e3 = 1000，或者 6.022e23 = 6.022 x 1e23）。  

使用 <code>a := uint64(0)</code> 来同时完成类型转换和赋值操作，这样 a 的类型就是 uint64。  

Go 中不允许不同类型之间的混合使用，但是对于常量的类型限制非常少，因此允许常量之间的混合使用，如下（该程序无法通过编译）：  

示例 4.8 type_mixing.go 

~~~go
package main

func main() {
    var a int
    var b int32
    a = 15
    b = a + a    // 编译错误
    b = b + 5    // 因为 5 是常量，所以可以通过编译
}
~~~

如果你尝试编译该程序，则将得到编译错误 <code>cannot use a + a (type int) as type int32 in assignment</code>。  

同样地，int16 也可以被隐式转换为 int32.  

下面这个程序展示了通过显式转换来避免这个问题（第 4.2 节）。  

示例 4.9 casting.go

~~~go
package main

import "fmt"

func main() {
    var n int16 = 34
    var m int32
    // compiler error: cannot use n (type int16) as type int32 in assignment
    //m = n
    m = int32(n)

    fmt.Printf("32 bit int is: %d\n", m)
    fmt.Printf("16 bit int is: %d\n", n)
}
~~~

输出：

~~~go
32 bit int is: 34
16 bit int is: 34
~~~

#### 格式化说明符

- %d  格式化整数（%x 和 %X 用于格式化 16 进制表示的数字）
- %g  格式化浮点型（%f 输出浮点数，%e 输出科学计数表示法）
- %0d 规定输出定长的整数，其中开头的数字 0 是必须的。

<code>%n.mg</code> 用于表示数字 n 并精确到小数点后 m 位，除了使用 g 之外，还可以使用 e 或者 f，例如：使用格式化字符串 <code>%5.2e</code> 来输出 3.4 的结果为 <code>3
.40e+00</code>。


#### 数字值转换

当进行类似 <code>a32bitInt = int32(a32Float)</code> 的转换时，小数点后的数字将被丢弃。
这种情况一般发生当从取值范围较大的类型转换为取值范围较小的类型时，或者你可以写一个专门用于处理类型转换的函数来确保没有发生精度的丢失。
下面这个例子展示如何安全地从 int 型转换为 int8：

~~~go
func Uint8FromInt(n int) (uint8, error) {
    if 0 <= n && n <= math.MaxUint8 { // conversion is safe
        return uint8(n), nil
    }
    return 0, fmt.Errorf("%d is out of the uint8 range", n)
}
~~~

或者安全地从 float64 转换为 int：

~~~go
func IntFromFloat64(x float64) int {
    if math.MinInt32 <= x && x <= math.MaxInt32 { // x lies in the integer range
        whole, fraction := math.Modf(x)
        if fraction >= 0.5 {
            whole++
        }
        return int(whole)
    }
    panic(fmt.Sprintf("%g is out of the int32 range", x))
}
~~~

不过如果你实际存的数字超出你要转换到的类型的取值范围的话，则会引发 panic（第 13.2 节）。


#### 4.5.2.2 复数

Go 复数类型：

~~~go
complex64  (32 位实数和虚数)
complex128 (64 位实数和虚数)
~~~

复数使用 <code>re+imI</code> 来表示，其中 <code>re</code> 代表实数部分，<code>im</code> 代表虚数部分，<code>I</code> 代表根号负 1。

示例：

~~~go
var c1 complex64 = 5 + 10i
fmt.Printf("The value is: %v", c1)
// 输出： 5 + 10i
~~~

函数 <code>real(c)</code> 和 <code>imag(c)</code> 可以分别获得相应的实数和虚数部分。


#### 4.5.2.3 位运算

位运算只能用于整数类型的变量，且需当它们拥有等长位模式时。  

<code>%b</code> 表示位的格式化标识符。

#### 二元运算符

- 按位与 <code>&</code>：

对应位置上的值经过和运算结果，具体参见和运算符，第 4.5.1 节，并将 T（true）替换为 1，将 F（false）替换为 0

~~~go
1 & 1 -> 1
1 & 0 -> 0
0 & 1 -> 0
0 & 0 -> 0
~~~

- 按位或 <code> | </code>：

对应位置上的值经过或运算结果，具体参见或运算符，第 4.5.1 节，并将 T（true）替换为 1，将 F（false）替换为 0

~~~go
1 | 1 -> 1
1 | 0 -> 1
0 | 1 -> 1
0 | 0 -> 0
~~~

- 按位异或 <code>^</code>：

对应位置上的值根据以下规则组合：

~~~go
1 ^ 1 -> 0
1 ^ 0 -> 1
0 ^ 1 -> 1
0 ^ 0 -> 0
~~~

- 位清除 <code>&^</code>：将指定位置上的值设置为 0。


#### 一元运算符

- 按位补足 <code>^</code>：

该运算符与异或运算符一同使用，即 <code>m^x</code>，对于无符号 x 使用 “全部位设置为 1”，对于有符号 x 时使用 <code>m=-1</code>。例如：

~~~go
^2 = ^10 = -01 ^ 10 = -11
~~~


- 位左移 <code> << </code>

>用法：<code> bitP << n </code>。

><code>bitP</code> 的位向左移动 n 位，右侧空白部分使用 0 填充；如果 n 等于 2，则结果是 2 的相应倍数，即 2 的 n 次方。例如：

~~~go
1 << 10 // 等于 1 KB
1 << 20 // 等于 1 MB
1 << 30 // 等于 1 GB
~~~

- 位右移 <code> >> </code>：

> 用法：<code> bitP >> n </code>

><code>bitP</code> 的位向右移动 n 位，左侧空白部分使用 0 填充；如果 n 等于 2，则结果是当前值除以 2 的 n 次方。

当希望把结果赋值给第一个操作数时，可以简写为 <code> a <<= 2 </code> 或者 <code> b ^= a & 0xffffffff </code>。


#### 位左移常见实现存储单位的用例

使用位左移与 iota 计数配合可优雅地实现存储单位的常量枚举：

~~~go
type ByteSize float64
const (
    _ = iota // 通过赋值给空白标识符来忽略值
    KB ByteSize = 1<<(10*iota)
    MB
    GB
    TB
    PB
    EB
    ZB
    YB
)
~~~

#### 在通讯中使用位左移表示标识的用例

~~~go
type BitFlag int
const (
    Active BitFlag = 1 << iota // 1 << 0 == 1
    Send // 1 << 1 == 2
    Receive // 1 << 2 == 4
)

flag := Active | Send // == 3
~~~


#### 4.5.2.4 逻辑运算符

Go 逻辑运算符：<code> == </code>、<code> != </code>（第 4.5.1 节）、<code> < </code>、<code> <= </code>、<code> > </coode>、<code> 
>= </code>。

逻辑运算符的运算结果总是为布尔值 bool。例如：

~~~go
b3:= 10 > 5 // b3 is true
~~~


#### 4.5.2.5 算术运算符

Go 在进行字符串拼接时允许使用对运算符 + 的重载，但 Go 本身不允许开发者进行自定义的运算符重载。  

<code> / </code> 对于整数运算而言，结果依旧为整数，例如：<code>9 / 4 -> 2</code>。  

取余运算符只能作用于整数：<code>9 % 4 -> 1</code>。  

整数除以 0 可能导致程序崩溃，将会导致运行时的恐慌状态（如果除以 0 的行为在编译时就能被捕捉到，则会引发编译错误）。  

浮点数除以 0.0 会返回一个无穷尽的结果，使用 <code> +Inf </code> 表示。  

#### 练习 4.4 尝试编译 divby0.go。

你可以将语句 <code> b = b + a </code> 简写为 <code> b+=a </code>，同样的写法也可用于 <code> -= </code>、<code> *= </code>、<code> /= 
</code>、<code> %= </code>。

对于整数和浮点数，你可以使用一元运算符 <code> ++ </code>（递增）和 <code> -- </code>（递减），但只能用于后缀：

~~~go
i++ -> i += 1 -> i = i + 1
i-- -> i -= 1 -> i = i - 1
~~~

同时，带有 <code> ++ </code> 和 <code> -- </code> 的只能作为语句，而非表达式，因此 <code> n = i++ </code> 这种写法是无效的，其它像 <code> f(i++) </code>
 或者 <code> a[i]=b[i++] </code> 这些可以用于 C、C++ 和 Java 中的写法在 Go 中也是不允许的。  
 
在运算时 溢出 不会产生错误，Go 会简单地将超出位数抛弃。
如果你需要范围无限大的整数或者有理数（意味着只被限制于计算机内存），你可以使用标准库中的 <code> big </code> 包，该包提供了类似 <code> big.Int </code> 和 <code> big.Rat 
</code> 这样的类型（第 9.4 节）。
 
 
#### 4.5.2.6 随机数

<code>rand</code> 包实现了伪随机数的生成。  

示例 4.10 random.go 演示了如何生成 10 个非负随机数：

~~~go
package main
import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    for i := 0; i < 10; i++ {
        a := rand.Int()
        fmt.Printf("%d / ", a)
    }
    for i := 0; i < 5; i++ {
        r := rand.Intn(8)
        fmt.Printf("%d / ", r)
    }
    fmt.Println()
    timens := int64(time.Now().Nanosecond())
    rand.Seed(timens)
    for i := 0; i < 10; i++ {
        fmt.Printf("%2.2f / ", 100*rand.Float32())
    }
}
~~~

可能的输出：

~~~go
816681689 / 1325201247 / 623951027 / 478285186 / 1654146165 /
1951252986 / 2029250107 / 762911244 / 1372544545 / 591415086 / / 3 / 0 / 6 / 4 / 2 /22.10
/ 65.77 / 65.89 / 16.85 / 75.56 / 46.90 / 55.24 / 55.95 / 25.58 / 70.61 /
~~~

函数 <code> rand.Float32 </code> 和 <code> rand.Float64 </code> 返回介于 [0.0, 1.0) 之间的伪随机数，其中包括 0.0 但不包括 1.0。函数 <code> rand
.Intn </code> 返回介于 [0, n) 之间的伪随机数。    

你可以使用 <code> Seed(value) </code> 函数来提供伪随机数的生成种子，一般情况下都会使用当前时间的纳秒级数字（第 4.8 节）。


#### 4.5.3 运算符与优先级

二元运算符的运算方向均是从左至右。下表列出了所有运算符以及它们的优先级，由上至下代表优先级由高到低：

~~~go
优先级  运算符
 7      ^ !
 6      * / % << >> & &^
 5      + - | ^
 4      == != < <= >= >
 3      <-
 2      &&
 1      ||
~~~

当然，你可以通过使用括号来临时提升某个表达式的整体运算优先级。


#### 4.5.4 类型别名

当你在使用某个类型时，你可以给它起另一个名字，然后你就可以在你的代码中使用新的名字（用于简化名称或解决名称冲突）。  
 
在 <code> type TZ int </code> 中，TZ 就是 int 类型的新名称（用于表示程序中的时区），然后就可以使用 TZ 来操作 int 类型的数据。  

示例 4.11 type.go

~~~go
package main
import "fmt"

type TZ int

func main() {
    var a, b TZ = 3, 4
    c := a + b
    fmt.Printf("c has the value: %d", c) // 输出：c has the value: 7
}
~~~

实际上，类型别名得到的新类型并非和原类型完全相同，新类型不会拥有原类型所附带的方法（第 10 章）；TZ 可以自定义一个方法用来输出更加人性化的时区信息。


#### 4.5.5 字符类型

严格来说，这并不是 Go 语言的一个类型，字符只是整数的特殊用例。<code> byte </code> 类型是 <code> uint8 </code> 的别名，对于只占用 1 个字节的传统 ASCII 
编码的字符来说，完全没有问题。例如：<code> var ch byte = 'A' </code>；字符使用单引号括起来。

在 ASCII 码表中，A 的值是 65，而使用 16 进制表示则为 41，所以下面的写法是等效的：

~~~go
var ch byte = 65 或 var ch byte = '\x41'
~~~

（\x 总是紧跟着长度为 2 的 16 进制数）  

另外一种可能的写法是 \ 后面紧跟着长度为 3 的八进制数，例如：\377。  

不过 Go 同样支持 Unicode（UTF-8），因此字符同样称为 Unicode 代码点或者 runes，并在内存中使用 int 来表示。
在文档中，一般使用格式 U+hhhh 来表示，其中 h 表示一个 16 进制数。其实 rune 也是 Go 当中的一个类型，并且是 int32 的别名。  

在书写 Unicode 字符时，需要在 16 进制数之前加上前缀 \u 或者 \U。  

因为 Unicode 至少占用 2 个字节，所以我们使用 int16 或者 int 类型来表示。
如果需要使用到 4 字节，则会加上 \U 前缀；前缀 \u 则总是紧跟着长度为 4 的 16 进制数，前缀 \U 紧跟着长度为 8 的 16 进制数。

示例 4.12 char.go

~~~go
var ch int = '\u0041'
var ch2 int = '\u03B2'
var ch3 int = '\U00101234'
fmt.Printf("%d - %d - %d\n", ch, ch2, ch3) // integer
fmt.Printf("%c - %c - %c\n", ch, ch2, ch3) // character
fmt.Printf("%X - %X - %X\n", ch, ch2, ch3) // UTF-8 bytes
fmt.Printf("%U - %U - %U", ch, ch2, ch3) // UTF-8 code point
~~~

输出：

~~~go
65 - 946 - 1053236
A - β - r
41 - 3B2 - 101234
U+0041 - U+03B2 - U+101234
~~~

格式化说明符 %c 用于表示字符；当和字符配合使用时，%v 或 %d 会输出用于表示该字符的整数；%U 输出格式为 U+hhhh 的字符串  

包 unicode 包含了一些针对测试字符的非常有用的函数（其中 ch 代表字符）：

- 判断是否为字母：unicode.IsLetter(ch)
- 判断是否为数字：unicode.IsDigit(ch)
- 判断是否为空白符号：unicode.IsSpace(ch)


这些函数返回一个布尔值。包 utf8 拥有更多与 rune 相关的函数。


#### 4.6 字符串

Go 中的字符串需要占用 1 至 4 个字节，这不仅减少了内存和硬盘的空间占用，同时也不用像其它语言那这样需要对使用 UTF-8 字符集的文本进行编码和解码。  

Go 支持 2 钟形式的字面值：

- 解释字符串

该类字符串使用双引号括起来，其中的相关的转义符将被替换。

1. <code> \n </code>：换行符
2. <code> \r </code>：回车符
3. <code> \t </code>：tab 键
4. <code> \u </code> 或 <code> \U </code>：Unicode 字符
5. <code> \\ </code>：反斜杠自身


- 非解释字符串

该类字符串使用反引号括起来，支持换行，例如：

~~~go
`This is a raw string \n` 中的 `\n\` 会被原样输出
~~~


Go 中的字符串根据长度限定，二非特殊字符 <code> \0 </code>。  

<code> string </code> 类型的零值为长度为零的字符串，即空字符串 <code> "" </code>。  

一般的比较运算符（<code>==</code>、<code>!=</code>、<code> < </code>、<code><=</code>、<code> >= </code>、<code> > 
</code>）通过在内存中按字节比较来实现字符串的对比。  

函数 <code>len()</code> 来获取字符串所占的字节长度，例如：<code> len(str) </code>。  

字符串的内容（纯字节）可以通过标准索引法来获取，在中括号 <code> [] </code> 内写入索引，索引从 0 开始计数：

- 字符串 str 的第 1 个字节：<code>str[0]</code>
- 第 i 个字节：<code>str[i - 1]</code>
- 最后 1 个字节：<code>str[len(str)-1]</code>

需要注意的是，这种转换方案只对纯 ASCII 码的字符串有效。  

注意事项 获取字符串中某个字节的地址的行为是非法的，例如：<code>&str[i]</code>。  


##### 字符串拼接符 <code>+</code>

两个字符串 s1 和 s2 可以通过 <code>s := s1 + s2</code> 拼接在一起。    

s2 追加在 s1 尾部并生成一个新的字符串 s。  

你可以通过以下方式来对代码中多行的字符串进行拼接：

~~~go
str := "Beginning of the string " +
    "second part of the string"
~~~

由于编译器行尾自动补全分号的缘故，加号 + 必须放在第一行。  

拼接的简写形式 += 也可以用于字符串：

~~~go
s := "hel" + "lo,"
s += "world!"
fmt.Println(s) //输出 “hello, world!”
~~~

在循环中使用加号 + 拼接字符串并不是最高效的做法，更好的办法是使用函数 strings.Join(),使用字节缓冲（bytes.Buffer）拼接更加好。  


#### 4.7 strings 和 strconv 包

Go 使用 <code>strings</code> 包来完成对字符串的主要操作。

#### 4.7.1 前缀和后缀

<code>HasPrefix</code> 判断字符串 s 是否以 prefix 开头：

~~~go
strings.HasPrefix(s, prefix string) bool
~~~

HasSuffix 判断字符串 s 是否以 prefix 开头：

~~~go
strings.HasSuffix(s, suffix string) bool
~~~


#### 4.7.2 字符串包含关系

Contains 判断字符串 s 是否包含 substr：

~~~go
strings.Contains(s, substr string) bool
~~~


#### 4.7.3 判断子字符串或字符在父字符串中出现的位置（索引）

Index 返回字符串 str 在字符串 s 中的索引（str 的第一个字符的索引），-1 表示字符串 s 不包含字符串 str：

~~~go
strings.Index(s, str string) int
~~~

如果 ch 是非 ASCII 编码的字符，建议使用以下函数来对字符进行定位：

~~~go
strings.IndexRune(s string, r rune) int
~~~


#### 4.7.4 字符串替换

Replace 用于将字符串 str 中的前 n 个字符串 old 替换为字符串 new，并返回一个新的字符串，如果 n = -1 则替换所有字符串 old 为字符串 new：

~~~go
strings.Replace(str, old, new, n) string
~~~


#### 4.7.5 统计字符串出现次数

Count 用于计算字符串 str 在字符串 s 中出现的非重叠次数：

~~~go
strings.Count(s, str string) int
~~~


#### 4.7.6 重复字符串

Repeat 用于重复 count 次字符串 s 并返回一个新的字符串：

~~~go
strings.Repeat(s, count int) string
~~~

#### 4.7.7 修改字符串大小写

ToLower 将字符串中的 Unicode 字符串全部转换为相应的小写字符：

~~~go
strings.ToLower(s) string
~~~

ToUpper 将字符串中的 Unicode 字符全部转换为相应的大写字符：

~~~go
strings.ToUpper(s) string
~~~

#### 4.7.8 修剪字符串

strings.TrimSpace(s) 删除字符串开头和结尾的空白符号；  
strings.TrimSpace(s, "cut") 删除开头或结尾的 cut 字符。
TrimLeft  删除左边。  
TrimRight 删除右边。


#### 4.7.9 分割字符串

strings.Fields(s) 将会利用 1 个或多个空白符号来作为动态长度的分隔符将字符串分割成若干小块，并返回一个 slice，如果字符串只包含空白符号，则返回一个长度为 0 的 slice。  

strings.Split(s, sep) 用于自定义分割符号来对指定字符串进行分割，同样返回 slice。


#### 4.7.10 拼接 slice 到字符串

Join 用于将元素类型为 string 的 slice 使用分割符号来拼接组成一个字符串：

~~~go
strings.join(sl []string, sep string) string
~~~

#### 4.7.11 从字符串中读取内容

strings.NewReader(str) 用于生成一个 Reader 并读取字符串中的内容，然后返回指向该 Reader 的指针。  

Read() 从 [] byte 中读取内容。  

ReadByte() 和 ReadRune() 从字符串中读取下一个 byte 或者 rune。


#### 4.7.12 字符串与其它类型的转换

与字符串相关的类型转换都是通过 strconv 包实现的。    

该包包含了一些变量用于获取程序运行的操作系统平台下 int 类型所占的位数，如：strconv.IntSize。  

任何类型 T 转换为字符串总是成功的。  

针对从数字类型转换到字符串，Go 提供了以下函数：

- strconv.Itoa(i int) string 返回数字 i 所表示的字符串类型的十进制数。
- strconv.FormatFloat(f float64, fmt byte, prec int, bitSize int) string 将 64 位浮点型的数字转换为字符串，
其中 fmt 表示格式（其值可以是 'b'、'e'、'f' 或 'g'），prec 表示精度，
bitSize 则使用 32 表示 float32，用 64 表示 float64。

将字符串转换为其它类型 tp 并不总是可能的，可能会在运行时抛出错误 parsing "…": invalid argument。  

针对从字符串类型转换为数字类型，Go 提供了以下函数：

- strconv.Atoi(s string) (i int, err error) 将字符串转换为 int 型。
- strconv.ParseFloat(s string, bitSize int) (f float64, err error) 将字符串转换为 float64 型。

从字符串到其它类型的转换：

~~~go
val, err = strconv.Atoi(s)
~~~


#### 4.8. 时间和日期

time 包提供一个数据类型 time.Time （作为值使用）以及显示和测量时间和日期的功能函数。  

当前时间可以使用 time.Now() 获取，或者使用 t.Day()、t.Minute() 等等来获取时间的一部分；你甚至可以自定义时间格式化字符串，例如： fmt.Printf("%02d.%02d.%4d\n", t.Day()
, t.Month(), t.Year()) 将会输出 21.07.2011。  

Duration 类型表示两个连续时刻所相差的纳秒数，类型为 int64。
Location 类型映射某个时区的时间，UTC 表示通用协调世界时间。  

~~~go
fmt.Println(t.Format("02 Jan 2006 15:04"))
~~~


#### 4.9 指针

Go 提供了控制数据结构的指针的能力，但是不能进行指针运算。  

Go 取地址符是 &，放在一个变量前使用就会返回相应变量的内存地址。  

声明指针：

~~~go
var intP *int
~~~

然后使用 intP = &il 是合法的，此时 intP 指向 il。（指针的格式化标识符为 %p）  

intP 存储了 il 的内存地址；它指向了 il 的位置，它引用了变量 il。  

一个指针变量可以指向任何一个值内存地址，它指向那个值的内存地址，在 32 位机器上占用 4 个字节，在 64 位机器上占用 8 个字节，
并且与它所指向的值的大小无关。当然，可以声明指针指向任何类型的值来表明它的原始性或结构性；
你可以在指针类型前面加上 * 号（前缀）来获取指针所指向的内容，这里的 * 号是一个类型更改器。
使用一个指针引用一个值被称为间接引用。

当一个指针被定义后没有分配到任何变量时，它的值为 nil。  

一个指针变量通常缩写为 ptr。  

##### 注意事项

在书写表达式类似 var p *type 时，切记在 * 号和指针名称间留有一个空格，因为 ` var ptype ` 是语法正确的，但是在更复杂的表达式中，它容易被误认为是一个乘法表达式！  

符号 * 可以放在一个指针前，如 *intP，那么它将得到这个指针指向地址上所存储的值；这被称为反引用（或者内容或者间接引用）操作符；另一种说法是指针转移。  

对于任何一个变量 var， 如下表达式都是正确的：var == *(&var)。  

程序 string_pointer.go 为我们展示了指针对 string 的例子。  

~~~go
package main
import "fmt"
func main() {
    s := "good bye"
    var p *string = &s
    *p = "ciao"
    fmt.Printf("Here is the pointer p: %p\n", p) // prints address
    fmt.Printf("Here is the string *p: %s\n", *p) // prints string
    fmt.Printf("Here is the string s: %s\n", s) // prints same string
}
~~~

输出

~~~go
Here is the pointer p: 0x2540820
Here is the string *p: ciao
Here is the string s: ciao
~~~

通过对 *p 赋另一个值来更改 “对象”，这样 s 也会随之更改。  

#### 注意事项

你不能得到一个文字或常量的地址，例如：

~~~go
const i = 5
ptr := &i //error: cannot take the address of i
ptr2 := &10 //error: cannot take the address of 10
~~~

Go 语言中的指针保证了内存安全，更像是 Java、C# 和 VB.NET 中的引用。  

因此 c = *p++ 在 Go 语言的代码中是不合法的。  

指针可以传递一个变量的引用（如函数的参数），这样不会传递变量的拷贝。指针传递只占用 4 个或 8 个字节。
当程序在工作中需要占用大量的内存，或很多变量，或者两者都有，使用指针会减少内存占用和提高效率。
被指向的变量也保存在内存中，直到没有任何指针指向它们，所以从它们被创建开始就具有相互独立的生命周期。  

另一方面（虽然不太可能），由于一个指针导致的间接引用（一个进程执行了另一个地址），指针的过度频繁使用也会导致性能下降。  

指针也可以指向另一个指针，并且可以进行任意深度的嵌套，导致你可以有多级的间接引用，但在大多数情况这会使你的代码结构不清晰。  

对一个空指针的反向引用是不合法的，并且会使程序崩溃：  

示例 4.23 testcrash.go:

~~~go
package main
func main() {
    var p *int = nil
    *p = 0
}
// in Windows: stops only with: <exit code="-1073741819" msg="process crashed"/>
// runtime error: invalid memory address or nil pointer dereference
~~~