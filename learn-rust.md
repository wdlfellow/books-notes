通用编程概念
变量与可变性
变量默认不可变，如需要改变，可在变量名前加 mut 使其可变。例如：let mut a=1;。
常量总是不能改变，使用 const 声明，并且必须注明值的类型。例如：const MAX_NUM: u32 = 100;。
可以定义一个与之前变量同名的新变量，从而“遮盖”之前的变量，如下：
    let x = 5;
    let x = 6;
    let x = x * 2;
    println!("The value of x is: {}", x); // 12
mut 与“遮盖”的区别是，当再次使用 let 时，实际上创建了一个新变量，可以改变值的类型。而 mut 还是原来的变量，不可以改变类型。

数据类型
Rust是静态类型语言，任何值都属于一种明确的类型，内建类型分为：标量（scalar） 和 复合（compound）。

标量类型
标量 类型代表一个单独的值。Rust 有四种基本的标量类型：整型、浮点型、布尔类型、和字符类型。

整型
整数是一个没有小数部分的数字，每一种变体都可以是有符号或无符号的，并有一个明确的大小。有符号和无符号代表数字能否为负值。
长度	有符号	无符号
8-bit	i8	u8
16-bit	i16	u16
32-bit	i32	u32
64-bit	i64	u64
arch	isize	usize
isize 和 usize 类型依赖运行程序的计算机架构：64 位架构上它们是 64 位的， 32 位架构上它们是 32 位的。

数字字面值	示例
十进制	98_222
十六进制	0xff
八进制	0o77
二进制	0b111_000
字节(u8)	b'a'
可以使用表格中的任何一种形式编写数字字面值。除字节以外 的其它字面值允许使用 类型后缀，例如57u8，允许使用 _ 做为分隔符以方便读数，例如 1_000 (分隔符的数量与位置并不影响实际的数字)。

Rust 默认数字类型是 i32：它通常是最快的，甚至在 64 位系统上也是。isize 或 usize 的主要作为集合的索引。

浮点型
Rust 同样有两个主要的浮点数类型：f32(单精度浮点数) 和 f64(双精度浮点数)，分别占 32 位和 64 位比特。默认类型是 f64。因为它与 f32 速度差不多，然而精度更高。在 32 位系统上也能够使用 f64。不过比使用 f32 要慢。

布尔型
Rust 中的布尔类型有两个：true 和 false。Rust 中的布尔类型使用 bool 表示。

字符类型
Rust 的 char 类型代表了一个 Unicode 标量值。这意味着它可以比 ASCII 表示更多内容。拼音字母、中文/日文/汉语等象形文字、emoji（絵文字）以及零长度的空白字符对于 Rust char类型都是有效的。Unicode 标量值包含从 U+0000 到 U+D7FF 和 U+E000 到 U+10FFFF 之间的值。“字符”并不是一个 Unicode 中的概念，Rust 中的 char类型与直觉上的“字符”可能并不符合。

复合类型
复合类型 可以将多个其它类型的值组合成一个类型。Rust 有两个原生的复合类型：元组（tuple） 和 数组（array）。

元组
元组是一个将多个其它类型的值组合进一个复合类型的主要方式。

元组中的每一个位置都有一个类型，类型不必相同。

为了从元组中获取单个的值，可以使用模式匹配（pattern matching）来解构（destructure ）元组。

    let tup = (500, 6.4, 1);
    let (x, y, z) = tup;
    println!("The value of y is: {}", y); // 6.4
除了使用模式匹配解构之外，也可以使用点号（.）后跟值的索引来直接访问它们，元组的第一个索引值是 0。例如：

    let x: (i32, f64, u8) = (500, 6.4, 1);
    let five_hundred = x.0;
    let six_point_four = x.1;
    let one = x.2;
数组
数组中的每个元素的类型必须相同。

Rust 中的数组是 固定长度 的：一旦声明，它们的长度不能增长或缩小。

let a = [1, 2, 3, 4, 5];
数组在想要在 栈 （stack）上分配空间，或者是想要确保总是有固定数量的元素时十分有用。虽然它并不如 vector 类型那么灵活。vector 类型是标准库提供的一个允许增长和缩小长度的类似数组的集合类型。当不确定是应该使用数组还是 vector 的时候，你可能应该使用 vector。

数组是一整块分配在栈上的内存。可以使用索引来访问数组的元素，像这样：

let a = [1, 2, 3, 4, 5];
let first = a[0];
let second = a[1];
当尝试用索引访问一个元素时，Rust 会检查指定的索引是否小于数组的长度。如果索引超出了数组长度，Rust 会panic，它用于程序因为错误而退出的情况。

函数
Rust 使用 main 函数作为程序的入口。使用 fn 关键字类声明函数。使用 “snake case” 作为函数和变量的命名规范——所有字母都小写并使用下划线分割。

在函数签名中，必须声明每个参数的类型。这是 Rust 设计中一个经过慎重考虑的决定：要求在函数定义中提供类型注解意味着编译器再也不需要在别的地方要求你注明类型就能知道你的意图。

语句与表达式
Rust 是一个基于表达式的语言。

语句（Statements） 是执行一些操作但不返回值的指令。函数定义也是语句，语句并不返回值。不能把语句赋值给一个变量。

表达式（Expressions） 计算并产生一个值。函数调用是一个表达式，宏调用是一个表达式，用来创新建作用域的大括号（代码块）{} 也是一个表达式。表达式并不包含结尾的分号。如果在表达式的结尾加上分号，就变成了语句。

函数的返回值
可以向调用它的代码返回值，并不对返回值命名，不过会在一个箭头（->）后声明它的类型。在 Rust 中，函数的返回值等同于函数体最后一个表达式的值。这是一个有返回值的函数的例子：

fn five() -> i32 {
    5
}

fn main() {
    let x = five();
    println!("The value of x is: {}", x);
}
控制流
Rust 代码中最常见的用来控制执行流的结构是if表达式和循环。

if 表达式
所有if表达式以 if 关键字开头，后跟一个条件，条件必须是 bool(不需要圆括号)。Rust 并 不会 自动将非布尔值转换为布尔值。

let number = 6;

if number % 3 == 0 {
    println!("number is divisible by 3");
} else if number % 2 == 0 {
    println!("number is divisible by 2");
} else {
    println!("number is not divisible by 4, 3, or 2");
}
使用过多的 else if 表达式会使代码显得杂乱无章，所以如果有多于一个 else if ，最好使用 match 重构代码。

由于if是表达式，所以可以用在let变量声明中，但是必须注意值的类型一致，必须包含有 else 块，并且别忘了结尾的分号。

循环
rust 有三种循环类型：loop、while、for。

无限循环loop
loop {
    println!("again!");
}
可以使用 break 关键字来告诉程序何时停止执行循环。

条件循环 while
let mut number = 3;
while number != 0  {
    println!("{}!", number);
    number = number - 1;
}
println!("LIFTOFF!!!");
集合遍历 for
let a = [10, 20, 30, 40, 50];
for element in a.iter() {
    println!("the value is: {}", element);
}
for 循环的安全性和简洁性使得它在成为 Rust 中使用最多的循环结构。即使是在想要循环执行代码特定次数时，使用 Range，它是标准库提供的用来生成从一个数字开始到另一个数字结束的所有数字序列的类型。例如：

for i in 1..10 {
  println!("value is:{}",i);
}
还可以使用 rev 方法来反转 Range：

for i in (1..10).rev() {
    println!("value is:{}",i);
}
所有权
堆与栈
栈： 后进先出，增加数据叫进栈，移除数据叫出栈。
操作栈是非常快的，因为它访问数据的方式：永远也不需要寻找一个位置放入新数据或取出数据，因为这个位置总是在栈顶。所有数据都必须是一个已知的固定大小。

堆：访问堆上的数据要比访问栈上的数据要慢，因为必须通过指针来访问。
当调用一个函数，传递给函数的值（包括可能指向堆上数据的指针）和函数的局部变量被压入栈中。当函数结束时，这些值被移除栈。

所有权规则
每一个值都被它的所有者（owner）变量拥有。
值在任意时刻只能被一个所有者拥有。
当所有者离开作用域，这个值将被丢弃。
String 类型
String 类型存储在堆上，可以用 from 从字符串字面值来创建 String 如：let s = String::from("hello");

对于String类型，为了支持一个可变，可增长的文本片段，需要在堆上分配一块在编译时未知大小的内存来存放内容。这意味着:

内存必须在运行时向操作系统请求。

需要一个当我们处理完String 时将内存返回给操作系统的方法。

Rust 采取了一个不同的策略：内存在拥有它的变量离开作用域后就被自动释放。当变量离开作用域，Rust 为其调用一个特殊的函数。这个函数叫做 drop。在这里String的作者可以放置释放内存的代码。Rust 在结尾的}处自动调用 drop。

String 由三部分组成，如下图左侧所示：一个指向存放字符串内容内存的指针，一个长度，和一个容量。这一组数据储存在栈上。右侧则是堆上存放内容的内存部分。
堆 栈 内存分配
长度代表当前String的内容使用了多少字节的内存。容量是String从操作系统总共获取了多少字节的内存。

拷贝指针、长度和容量而不拷贝数据可能听起来像浅拷贝。在Rust中这个操作被称为移动（move），而不是浅拷贝，移动是指所有权的转移。Rust 永远也不会自动创建数据的“深拷贝”。当 move 发生的时候，所有权被转移的变量，将会被释放。

如果我们确实需要深度复制String中堆上的数据，而不仅仅是栈上的数据，可以使用一个叫做clone()的通用函数。

任何简单标量值的组合可以是Copy的，对于实现了Copy Trait的类型来说，当移动发生的时候，它们可以Copy的副本代替自己去移动，而自身还保留着所有权。

所有整数类型，比如u32。
布尔类型，bool，它的值是true和false。
所有浮点数类型，比如f64。
元组，当且仅当其包含的类型也都是Copy的时候。如：(i32, i32)是Copy的，不过(i32, String)就不是。
值得注意的是，Rust 不允许自身或其任何部分实现了Drop trait 的类型使用Copy trait。

将值传递给函数在语言上与给变量赋值相似。向函数传递值可能会移动或者复制，就像赋值语句一样。所以如 String 类型，如果将它作为参数传递给函数后，就会产生移动，之后不能在使用。

fn main() {
    let s = String::from("hello");

    takes_ownership(s); // 传递参数和变量赋值相似 s产生了移动，所有权被转移
    println!("{}", s); // error： value used here after move
}

fn takes_ownership(some_string: String) {
    println!("{}", some_string);
}

变量的所有权总是遵循相同的模式：将值赋值给另一个变量时移动它。当持有堆中数据值的变量离开作用域时，其值将通过drop被清理掉，除非数据被移动为另一个变量所有。

作用域
作用域在Rust中的作用就是制造一个边界，这个边界是所有权的边界。变量走出其所在作用域，所有权会move。如果不想让所有权 move ，则可以使用 “引用” 来“出借”变量，而此时作用域的作用就是保证被“借用”的变量准确归还。

在当前作用域中由 let 开启的作用域是隐式作用域。在 Rust 中，也有一些特殊的宏，比如 println!()，也会产生一个默认的作用域，并且会隐式借用变量。

引用和借用
使用&符号，就是引用，允许使用其值但不获取它的所有权。将获取引用作为函数参数称为借用。

正如变量默认是不可变的，引用也一样。不允许修改引用的值。

可变引用
fn main() {
    let mut s = String::from("hello");
    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
可变引用有一个很大的限制：在特定作用域中一个数据有且只有一个可变引用,也不能在拥有不可变引用的同时拥有可变引用。这个限制的好处是 Rust 可以在编译时就避免数据竞争（data races）。

数据竞争是一种特定类型的竞争状态，它可由这三个行为造成：

两个或更多指针同时访问同一数据。
至少有一个指针被写入。
没有同步数据访问的机制。
引用的规则
在任意给定时间，只能拥有如下中的一个：
一个可变引用。
任意数量的不可变引用。
引用必须总是有效的。
Slices
slice数据类型，没有所有权。它允许引用集合中一段连续的元素序列，而不用引用整个集合。

字符串 slice
字符串 slice（string slice）是 String 中一部分值的引用，它看起来像这样：

let s = String::from("hello world");

let hello = &s[0..5]; // 也可以写成 &s[..5]
let world = &s[6..11]; // &s[6..]
let hw = &s[..] // 截取整个字符串
不同于整个String的引用，这是一个包含String内部的一个位置和所需元素数量的引用。

字符串字面值就是 slice 比如：let s = "Hello, world!"; 这里 s 的类型就是 &str：它是一个指向二进制程序特定位置的slice。这也就是为什么字符串字面值是不可变的；&str 是一个不可变引用。

可以对其它所有类型的集合使用 slice 例如：let a = [1, 2, 3, 4, 5];let slice = &a[1..3]; 这个 slice 的类型是 &[i32]。

结构体
struct User {
    username: String,
    email: String,
    sign_in_count: u64,
    active: bool,
}

let user1 = User {
    email: String::from("someone@example.com"),
    username: String::from("someusername123"),
    active: true,
    sign_in_count: 1,
};
为了从结构体中获取某个值，可以使用点号 . 。如果我们只想要用户的邮箱地址，可以用 user1.email。

在 User 结构体的定义中，我们使用了自身拥有所有权的String类型而不是 &str 字符串 slice 类型。因为我们想要这个结构体拥有它所有的数据，为此只要整个结构体是有效的话其数据也应该是有效的。

声明结构体，字段必须要声明类型，否则会报错。

可以使结构体储存被其它对象拥有的数据的引用，不过这么做的话需要用上 生命周期（lifetimes）。生命周期确保结构体引用的数据有效性跟结构体本身保持一致。

方法
方法与函数类似：使用fn关键字和名字声明，可以拥有参数和返回值，同时包含一些代码会在某处被调用时执行。

与函数不同的是，它们在结构体（或者枚举或者 trait 对象）的上下文中被定义，并且它们第一个参数总是 self，它代表方法被调用的结构体的实例。

struct Rectangle {
    length: u32,
    width: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.length * self.width
    }
}
可以在 impl 块中定义不以 self 作为参数的函数，称为关联函数，因为它们和结构体相关联。即便如此它们也是函数而不是方法，因为它们并不作用于一个结构体的实例。例如：String::from 就是一个关联函数。

关联函数经常被用作返回一个结构体新实例的构造函数。

impl Rectangle {
    fn square(size: u32) -> Rectangle {
        Rectangle { length: size, width: size }
    }
}
使用结构体名和 :: 语法来调用这个关联函数：比如 let sq = Rectangle::square(3); 。这个方法位于结构体的命名空间中： :: 语法用于关联函数和模块创建的命名空间。

枚举
使用 enum 关键字声明枚举，可以将任意类型的数据放入枚举成员中：例如字符串、数字类型或者结构体。甚至可以包含另一个枚举！

枚举同结构体一样使用 impl 声明其方法，方法体使用了 self 来调用方法的值。

Option 枚举
Rust 并没有很多其它语言中有的空值功能。不过它确实拥有一个可以编码存在或不存在概念的枚举。这个枚举是 Option<T>，而且它定义于标准库中，如下:

enum Option<T> {
    Some(T),
    None,
}
空值（Null） 是一个值，它代表没有值。在有空值的语言中，变量总是这两种状态之一：空值和非空值。

Option<T> 已被 prelude 自动引用，不需要显式导入它，它的成员也是如此，不需要 Option::前缀来直接使用Some和None。即便如此 Option<T> 也仍是常规的枚举，Some(T)和None仍是Option<T>的成员。

如果使用 None 而不是 Some，需要告诉 Rust Option<T>是什么类型的，因为编译器只通过None值无法推断出Some成员的类型。

let some_number = Some(5);
let some_string = Some("a string");
let absent_number: Option<i32> = None; // 这里必须声明类型
当有一个 Some 值时，我们就知道存在一个值，而这个值保存在 Some 中。当有个 None 值时，在某种意义上它跟空值是相同的意义：并没有一个有效的值。

Option<T> 和 T（这里T可以是任何类型）是不同的类型，编译器不允许像一个被定义的有效的类型那样使用 Option<T>。例如，这些代码不能编译，因为它尝试将 Option<i8>与i8相比：

let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y; // error no implementation for `i8 + Option<i8>`
当在 Rust 中拥有一个像i8这样类型的值时，编译器确保它总是有一个有效的值。我们可以自信使用而无需判空。只有当使用Option<i8>（或者任何用到的类型）是需要担心可能没有一个值，而编译器会确保我们在使用值之前处理为空的情况。

match 运算符
match 允许将一个值与一系列的模式相比较并根据匹配的模式执行代码。模式可由字面值、变量、通配符和许多其它内容构成。

match 关键字后跟一个任意类型的表达式，之后是 match 分支：一个模式和一些代码。使用 => 运算符将模式和将要运行的代码分开。每个分支相关联的代码是一个表达式，而表达式的结果值将作为整个 match 表达式的返回值。例如：

match coin {
    Coin::Penny => {
        println!("Lucky penny!");
        1
    },
    Coin::Nickel => 5,
    Coin::Dime => 10,
    Coin::Quarter => 25,
}
Rust 中的匹配是穷尽的：必须穷举到最后的可能性来使代码有效。

_ 通配符
_ 模式会匹配所有的值。通过将其放置于其它分支之后，将会匹配所有之前没有指定的可能的值。如果希望匹配后不做任何处理可以这样 _ =>(),

if let
if let是match的一个语法糖，它当只匹配某一模式时执行代码而忽略所有其它值。但这样会失去match强制要求的穷尽性检查。

if let Some(3) = some_u8_value {
    println!("three");
}
可以在if let中包含一个else。else块中的代码与match表达式中的_分支块中的代码相同。

let mut count = 0;
if let Coin::Quarter(state) = coin {
    println!("State quarter from {:?}!", state);
} else {
    count += 1;
}
模块
模块（module）是一个包含函数或类型定义的命名空间，你可以选择这些定义是能（公有）还是不能（私有）在其模块外可见。一个模块按照如下工作：

使用mod关键字声明模块
默认所有内容都是私有的（包括模块自身）。可以使用pub关键字将其变成公有并在其命名空间外可见。
use关键字允许引入模块、或模块中的定义到作用域中以便于引用它们。
模块文件系统的规则
如果一个叫做foo的模块没有子模块，应该将foo的声明放入叫做 foo.rs 的文件中。
如果一个叫做foo的模块有子模块，应该将foo的声明放入叫做 foo/mod.rs 的文件中。模块自身则应该使用mod关键字定义于父模块的文件中。
可见性规则
如果一个模块是公有的，它能被任何父模块访问。
如果一个模块是私有的，它只能被当前模块或其子模块访问。
同位于根模块，私有模块的公有函数可以被访问，私有模块和私有函数都不可被访问。
除上述以外，私有模块的公有函数，公有模块的私有函数都不可被访问。
我们创建的所有模块都位于一个与 crate 同名的模块内部。这个顶层的模块被称为 crate 的根模块（root module）。

另外注意到即便在项目的子模块中使用外部 crate，extern crate也应该位于根模块（也就是 src/main.rs 或 src/lib.rs）。接着，在子模块中，我们就可以像顶层模块那样引用外部 crate 中的项了。

使用 use 简化导入
use 关键字的工作是缩短冗长的函数调用，通过将想要调用的函数所在的模块引入到作用域中。例如：

pub mod a {
    pub mod series {
        pub mod of {
            pub fn nested_modules() {}
        }
    }
}

use a::series::of; // 每当想要引用of模块时，不用使用完整的a::series::of路径，直接使用of

fn main() {
    of::nested_modules();
}
use 关键字只将指定的模块引入作用域；它并不会将其子模块也引入。

可以将函数本身引入到作用域中，通过如下在use中指定函数的方式：

use a::series::of::nested_modules;

fn main() {
    nested_modules();
}
因为枚举也像模块一样组成了某种命名空间，也可以使用use来导入枚举的成员。对于任何类型的use语句，如果从一个命名空间导入多个项，可以使用大括号和逗号来列举它们，像这样：

enum TrafficLight {
    Red,
    Yellow,
    Green,
}

use TrafficLight::{Red, Yellow};

fn main() {
    let red = Red;
    let yellow = Yellow;
    let green = TrafficLight::Green;
}
为了一次导入某个命名空间的所有项，可以使用 * 语法。例如：

enum TrafficLight {
    Red,
    Yellow,
    Green,
}

use TrafficLight::*;

fn main() {
    let red = Red;
    let yellow = Yellow;
    let green = Green;
}
*被称为 全局导入（glob），它会导入命名空间中所有可见的项。全局导入应该保守的使用：它们是方便的，但是也可能会引入多于你预期的内容从而导致命名冲突。

使用 super 访问父模块
使用 super 关键字获取当前模块的父模块，如果是用::mod1::mod2 则要从根模块开始列出整个路径。

use 关键字是相对于根模块开始的，可以通过 use super:child1 相对于父模块开始。

通用集合类型
不同于内建的数组和元组类型，这些集合指向的数据是储存在堆上的，这意味着数据的数量不必在编译时就可知并且可以随着程序的运行增长或缩小。

vector
Vec<T> 类型，也被称为 vector。允许我们在一个单独的数据结构中储存多于一个值，它在内存中彼此相邻的排列所有的值。vector 只能储存相同类型的值。

使用 Vec::new 函数创建空的 vector：let v: Vec<i32> = Vec::new(); 由于没有向这个 vector 中插入任何值，Rust 并不知道我们想要储存什么类型的元素。所以需要添加类型注解。

更常见的做法是使用初始值来创建一个 vector，这样可以省去类型注解。为了方便，Rust 提供了 vec! 宏。它会根据提供的值来创建新的vector，例如：let v = vec![1,2,3];

可以使用 push方法向vector添加元素，但必须声明是 mut：

let mut v = vec![];
v.push(1);
在 vector 的结尾增加新元素时，在没有足够空间将所有所有元素依次相邻存放的情况下，可能会要求分配新内存并将旧的元素拷贝到新的空间中。这时，对之前元素的引用就指向了被释放的内存，会引发错误，例如：

let mut v = vec![1, 2, 3, 4, 5];
let first = &v[0]; // 这里的引用会引发错误，直接用 v[0] 则不会。
v.push(6);
还可以是用 pop 方法移除并返回 vector 的最后一个元素。

vector 在其离开作用域时会被释放，其内容也会被丢弃。

读取 vector 中的元素可以使用索引或get方法，如：

let v = vec![1, 2, 3, 4, 5];

let third: &i32 = &v[2];
let third: Option<&i32> = v.get(2);
使用索引不能访问不存在的元素，否则会造成 panic!，而使用get方法访问不存在的元素会返回None，因为它返回的是Option<T>类型，要么是Some(T)要么是None。

由于 vector 只能存储相同类型的值，需要存储不同类型的值时，可以使用枚举，因为枚举的成员都是枚举类型。例如：

enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
字符串
Rust 核心语言中事实上只有一种字符串类型：str 字符串 slice，它通常以被借用的形式出现——&str。它们是一些存储在别处的 UTF-8 编码字符串数据的引用。比如字符串字面值被存储在程序的二进制输出中，字符串 slice 也是如此。

称作 String 的类型是由标准库提供的，而没有写进核心语言部分， 它是可增长的、可变的、有所有权的、UTF-8编码的字符串类型。当谈到 Rust 的“字符串”时，它们通常指的是 String 和字符串 slice &str 类型，而不是其中一个。

String和字符串 slice 都是 UTF-8编码的。

Rust 标准库中还包含一系列其他字符串类型，比如 OsString、OsStr、CString和CStr。

新建字符串
使用 new()函数创建空字符串。let s = String::new();

可以使用 to_string方法创建有内容的字符串。它能用于任何实现了 Display trait 的类型，对于字符串字面值：

let data = "initial contents";
let s = data.to_string();
// 也可以直接对字符串字面值使用
let s = "initial contents".to_string();
还可以使用 String::from() 函数来从字符串字面值创建 String。等同于使用 to_string：let s = String::from("initial contents");

更新字符串
String 的大小可以增长其内容也可以改变。

使用push_str方法添加字符串 slice。
let mut s = String::from("foo");
let s2 = String::from("bar");
s.push_str("bar");
s.push_str(&s2) // 这里是&s2 而不是s2
使用 push 方法添加字符。
let mut s = String::from("lo");
s.push('l');
使用 + 运算符将两个已知的字符串合并在一起，只能将&str 和 String 相加，不能将两个String值或两个&str相加。
let s1 = String::from("Hello, ");
let s2 = String::from("world!");
// let s3 = s1 + "wold";
let s3 = s1 + &s2; // s1 会发生转移 之后将不能使用

使用 format! 宏，将多个 String 或 &str 合并在一起。
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");
let s = format!("{}-{}-{}", s1, s2, s3);
索引字符串
Rust 的字符串 不支持索引。因为 String 是一个 Vec<u8> 的封装。采用 UTF-8 编码，不同的语言文字，一个字符串字节值的索引并不总是对应一个有效的 Unicode 标量值。Rust不得不检查从字符串的开头到索引位置的内容来确定这里有多少有效的字符，这就损害了性能，同时，为了避免返回意想不到的值造成意外的bug，Rust禁止使用索引。但如果确实需要的话可以使用字符串 slice。例如：

let s1 = String::from("tic");
let s2= "wold";
println!("{}", &s1[..1],&s2[1..2]); // 不要忘了 & 符号
遍历字符串
chars 方法，操作单独的 Unicode 标量值。
for c in "नमस्ते".chars() {
    println!("{}", c); // न म स  ् त  े
}
bytes 方法返回每一个原始字节。
for b in "नमस्ते".bytes() {
    println!("{}", b); // 224 164 168 224 ...
}
HashMap
HashMap<K,V> 类型储存了一个键类型 K 对应一个值类型 V 的映射。它通过一个 哈希函数（hashing function）来实现映射，决定如何将键和值放入内存中。

使用 new 创建一个空的 HashMap，并使用 insert 来增加元素。
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
HashMap 是同质的：所有的键必须是相同类型，值也必须都是相同类型。

使用 collect 方法，结合 zip 方法，将 元祖 vector 转换成 HashMap。
use std::collections::HashMap;

let teams  = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];

let scores: HashMap<_, _> = teams.iter().zip(initial_scores.iter()).collect(); // HashMap<_, _>类型注解是必要的
HashMap 和所有权
use std::collections::HashMap;

let field_name = String::from("Favorite color");
let field_value = String::from("Blue");

let mut map = HashMap::new();
map.insert(field_name, field_value); // field_name field_value 被插入后所有权转移到 map，之后不能使用这两个绑定
// 如果将值的引用插入哈希 map，这些值本身将不会被移动进哈希 map。但是这些引用指向的值必须至少在哈希 map 有效时也是有效的
访问 HashMap 中的值
通过 get 方法并提供对应的键来从 HashMap 中获取值：
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

let team_name = String::from("Blue");
let score = scores.get(&team_name); // 返回的是 Option<T>类型
使用与 for 循环遍历 HashMap 中的每一个键值对:
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

for (key, value) in &scores {
    println!("{}: {}", key, value); // 以任意顺序打印出每一个键值对：
}
更新 HashMap
使用 insert 插入一个键值对，如果之前有值，新值会代替旧值：
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);

println!("{:?}", scores); // {"Blue": 25}
使用 entry 和or_insert 方法，如果存在就忽略，如果不存在就插入，or_insert 方法会返回这个键的值的一个可变引用（&mut V）：
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);

scores.entry(String::from("Yellow")).or_insert(50);
scores.entry(String::from("Blue")).or_insert(50); // Blue 已存在 不会改变

println!("{:?}", scores);
根据旧值更新一个值:
use std::collections::HashMap;

let text = "hello world wonderful world";

let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1;
}

println!("{:?}", map); // {"world": 2, "hello": 1, "wonderful": 1}
错误处理
Rust 将错误组合成两个主要类别：可恢复错误（recoverable）和 不可恢复错误（unrecoverable）。可恢复错误 通常代表向用户报告错误和重试操作是合理的情况，比如未找到文件。不可恢复错误 通常是 bug 的同义词，比如尝试访问超过数组结尾的位置。

Rust 并没有异常。对于可恢复错误有 Result<T, E> 值，对于不可恢复错误有 panic!。

panic！宏
当执行这个宏时，程序会打印出一个错误信息，展开并清理栈数据，然后接着退出。出现这种情况的场景通常是检测到一些类型的 bug 而且程序员并不清楚该如何处理它。

当出现 panic! 时，程序默认开始 展开(unwinding)，Rust 会回溯栈并清理它遇到的每一个函数的数据，不过这个回溯并清理的过程有很多工作。如果需要最终发布的二进制文件越小越好,可以选择：直接 终止(abort) ——不清理数据就退出程序。那么程序所使用的内存需要由操作系统来清理。通过在 Cargo.toml 的 [profile] 部分增加 panic = 'abort'，将展开切换为终止。例如：

[profile.release]
panic = 'abort'
主动调用 panic!
fn main() {
    panic!("crash and burn");
}
运行程序将会出现类似这样的输出：

Compiling panic v0.1.0 (file:///projects/panic)
Finished debug [unoptimized + debuginfo] target(s) in 0.25 secs
    Running `target/debug/panic`
thread 'main' panicked at 'crash and burn', src/main.rs:2 // 显示错误信息及出现位置
note: Run with `RUST_BACKTRACE=1` for a backtrace.
error: Process didn't exit successfully: `target/debug/panic.exe` (exit code: 101)
因为代码中的 bug 引起的别的库中 panic!
fn main() {
    let v = vec![1, 2, 3];
    v[100];
}
运行程序会出现如下错误信息：

Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target\debug\panic.exe`
thread 'main' panicked at 'index out of bounds: the len is 3 but the index is 11', C:\projects\rust\src\libcollections\vec.rs:1552 // 显示别人代码中 错误信息及位置
note: Run with `RUST_BACKTRACE=1` for a backtrace.
error: process didn't exit successfully: `target\debug\panic.exe` (exit code: 101)
上述情况可以设置 RUST_BACKTRACE=1来显示更多信息：

cmd下，输入 set RUST_BACKTRACE=1
power shell下，输入 $env:RUST_BACKTRACE=1
之后再执行 cargo run，显示如下信息：

Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target\debug\panic.exe`
thread 'main' panicked at 'index out of bounds: the len is 3 but the index is 11', C:\projects\rust\src\libcollections\vec.rs:1552
stack backtrace:
   0: std::sys_common::backtrace::_print
             at C:\projects\rust\src\libstd\sys_common\backtrace.rs:71
   1: std::panicking::default_hook::{{closure}}
             at C:\projects\rust\src\libstd\panicking.rs:354
   2: std::panicking::default_hook
             at C:\projects\rust\src\libstd\panicking.rs:371
   3: std::panicking::rust_panic_with_hook
             at C:\projects\rust\src\libstd\panicking.rs:549
   4: std::panicking::begin_panic<collections::string::String>
             at C:\projects\rust\src\libstd\panicking.rs:511
   5: std::panicking::begin_panic_fmt
             at C:\projects\rust\src\libstd\panicking.rs:495
   6: std::panicking::rust_begin_panic
             at C:\projects\rust\src\libstd\panicking.rs:471
   7: core::panicking::panic_fmt
             at C:\projects\rust\src\libcore\panicking.rs:69
   8: core::panicking::panic_bounds_check
             at C:\projects\rust\src\libcore\panicking.rs:56
   9: collections::vec::{{impl}}::index<i32>
             at C:\projects\rust\src\libcollections\vec.rs:1552
  10: panic::main
             at .\src\main.rs:3 // 自己程序中引起错误的行
  11: panic_unwind::__rust_maybe_catch_panic
             at C:\projects\rust\src\libpanic_unwind\lib.rs:98
  12: std::rt::lang_start
             at C:\projects\rust\src\libstd\rt.rs:52
  13: main
  14: __scrt_common_main_seh
             at f:\dd\vctools\crt\vcstartup\src\startup\exe_common.inl:259
  15: BaseThreadInitThunk
error: process didn't exit successfully: `target\debug\panic.exe` (exit code: 101)
上面显示了程序执行到目前位置所有被调用的函数的列表。从头开始读直到发现自己的文件。这就是问题的发源地。这一行往上是调用的代码；往下则是被调用的代码。这些行可能包含核心 Rust 代码，标准库代码或用到的 crate 代码。

Result
enum Result<T, E> {
    Ok(T),
    Err(E),
}
T 和 E 是泛型类型参数: T 代表成功时返回的
Ok 成员中的数据的类型，而 E 代表失败时返回的 Err 成员中的错误的类型。

use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let f = File::open("hello.txt"); // 成功时，f的值是一个包含文件句柄的Ok实例；
                                    // 失败时，f会是一个包含更多关于出现了何种错误信息的Err实例

    let f = match f {
        Ok(file) => file,
        // 如果因为文件不存在而失败，创建文件并返回新文件的句柄
        // if error.kind() == ErrorKind::NotFound被称作 match guard：
        // 它是一个进一步完善match分支模式的额外的条件。这个条件必须为真才能使分支的代码被执行；
        // 否则，模式匹配会继续并考虑match中的下一个分支。
        // 模式中的ref是必须的，这样error就不会被移动到 guard 条件中而只是仅仅引用它
        // 在模式的上下文中，&匹配一个引用并返回它的值，而ref匹配一个值并返回一个引用。
        Err(ref error) if error.kind() == ErrorKind::NotFound => match File::create("hello.txt") {
            Ok(fc) => fc,
            Err(e) => panic!("Tried to create file but there was a problem: {:?}", e),
        },
        Err(error) => panic!("There was a problem opening the file: {:?}", error),
    };
}
File::open 函数的返回值类型是 Result<T, E>，成功值的类型 std::fs::File，它是一个文件句柄；失败值的类型是 std::io::Error，它是标准库中提供的结构体，这个结构体有一个返回 io::ErrorKind 值的 kind 方法可供调用。io::ErrorKind 是一个标准库提供的枚举，它的成员对应 io 操作可能导致的不同错误类型。ErrorKind::NotFound 代表尝试打开的文件不存在。

Result 枚举和其成员也被导入到了 prelude 中，所以就不需要 Ok 和 Err 之前指定 Result::。

失败时 panic 的捷径：unwrap和expect
Result<T, E> 类型定义了很多辅助方法来处理各种情况。

unwrap: 如果 Result 值是 Ok，返回 Ok 中的值，如果是 Err会自动调用 panic!。
use std::fs::File;

fn main() {
    let f = File::open("hello.txt").unwrap();
}
expect: 可以在参数中自定义要显示的错误信息，并在自动调用 panic! 时，显示在错误信息中，有助于追踪 panic 的根源。
use std::fs::File;

fn main() {
    let f = File::open("hello.txt").expect("Failed to open hello.txt");
}
传播错误
当编写一个其实现会调用一些可能会失败的操作的函数时，除了在这个函数中处理错误外，还可以选择让调用者知道这个错误并决定该如何处理，这被称为 传播（propagating）错误。简便的专用语法：?，可以在 ? 之后直接使用链式方法调用来进一步缩短代码:

use std::io;
use std::io::Read;
use std::fs::File;

fn read_username_from_file() -> Result<String, io::Error> {
    let mut s = String::new();

    File::open("hello.txt")?.read_to_string(&mut s)?;

    Ok(s)
}
? 只能用于返回值类型为 Result 的函数。

示例、代码原型和测试：非常适合 panic
当编写一个示例来展示一些概念时，在拥有健壮的错误处理代码的同时也会使得例子不那么明确。例如，调用一个类似 unwrap 这样可能 panic! 的方法可以被理解为一个你实际希望程序处理错误方式的占位符，它根据其余代码运行方式可能会各不相同。

unwrap 和 expect 方法在原型设计时非常方便，在决定该如何处理错误之前。它们在代码中留下了明显的记号，以便准备使程序变得更健壮时作为参考。
如果方法调用在测试中失败了，我们希望这个测试都失败，即便这个方法并不是需要测试的功能。因为 panic! 是测试如何被标记为失败的，调用 unwrap或 expect 都是非常有道理的。

泛型
Rust 通过在编译时进行泛型代码的 单态化（monomorphization）来保证效率。单态化是一个将泛型代码转变为实际放入的具体类型的特定代码的过程。这意味着在使用泛型时没有运行时开销；当代码运行，它的执行效率就跟好像手写每个具体定义的重复代码一样。

在函数定义中使用泛型
定义函数时可以在函数签名的参数数据类型和返回值中使用泛型。

fn largest<T>(list: &[T]) -> T {}
结构体定义中的泛型
使用 <> 来定义拥有一个或多个泛型参数类型字段的结构体。

struct Point<T, U> {
    x: T,
    y: U,
}
枚举定义中的泛型数据类型
enum Result<T, E> {
    Ok(T),
    Err(E),
}
方法定义中的枚举数据类型
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}
注意必须在 impl 后面声明 T，这样就可以在 Point<T> 上实现的方法中使用它了。

结构体定义中的泛型类型参数并不总是与结构体方法签名中使用的泛型是同一类型。

struct Point<T, U> {
    x: T,
    y: U,
}

impl<T, U> Point<T, U> {
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}
注意泛型参数 T 和 U 声明于 impl 之后，因为他们于结构体定义相对应。而泛型参数 V 和 W 声明于 fn mixup 之后，因为他们只是相对于方法本身的。

trait
使用 trait 关键字来定义一个 trait，后面是 trait 的名字。在大括号中声明描述实现这个 trait 的类型所需要的行为的方法签名。在方法签名后跟分号而不是在大括号中提供其实现。接着每一个实现这个 trait 的类型都需要提供其自定义行为的方法体，编译器也会确保任何实现这个 trait 的类型都拥有与这个签名的定义完全一致的方法。

trait 体中可以有多个方法，一行一个方法签名且都以分号结尾。

pub trait Summarizable {
    fn summary(&self) -> String;
}
实现 trait
pub struct Tweet {
    pub username: String,
    pub content: String,
    pub reply: bool,
    pub retweet: bool,
}

impl Summarizable for Tweet {
    fn summary(&self) -> String {
        format!("{}: {}", self.username, self.content)
    }
}
如果要实现外部定义的 trait 需要先将其导入作用域。不允许对外部类型实现外部 trait，可以对外部类型实现自定义的 trait，也可以对自定义类型上实现外部 trait。这个限制称为 孤儿规则(orphan rule)，即其父类型不存在。

默认实现
有时为 trait 中的某些或全部提供默认的行为，而不是在每个类型的每个实现中都定义自己的行为是很有用的。这样当为某个特定类型实现 trait 时，可以选择保留或重载每个方法的默认行为。

pub trait Summarizable {
    fn summary(&self) -> String {
        String::from("(Read more...)")
    }
}
使用默认实现，可以指定一个空的 impl 块：impl Summarizable for NewsArticle {}

重载实现
重载一个默认实现的语法与实现没有默认实现的 trait 方法时完全一样的。默认实现允许调用相同 trait 中的其他方法，哪怕这些方法没有默认实现。通过这种方法，trait 可以实现很多有用的功能而只需实现一小部分特定内容。

pub trait Summarizable {
    fn author_summary(&self) -> String;

    fn summary(&self) -> String {
        format!("(Read more from {}...)", self.author_summary())
    }
}

impl Summarizable for Tweet {
    fn author_summary(&self) -> String {
        format!("@{}", self.username)
    }
}
注意在重载过的实现中调用默认实现是不可能的。

trait bounds
可以对泛型类型参数使用 trait，从而限制泛型不再适用于任何类型，编译器会确保其被限制为那些实现了特定 trait 的类型，由此泛型就会拥有我们希望其类型所拥有的功能。这被称为指定泛型的 trait bounds。

pub fn notify<T: Summarizable>(item: T) {
    println!("Breaking news! {}", item.summary());
}
可以通过 + 来为泛型指定多个 trait bounds: <T: Summarizable + Display>。在函数名和参数列表之间的尖括号中指定很多的 trait bound 信息将是难以阅读的，所以将其移动到函数签名后的 where 从句中。

fn some_function<T, U>(t: T, u: U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{}
生命周期与引用有效性
Rust 中的每一个 引用 都有其生命周期，也就是引用保持有效的作用域。生命周期的主要目标是避免悬垂引用，它会导致程序引用了并非其期望引用的数据。

未初始化变量不能被使用

借用检查器 用来比较作用域来确保所有的借用都是有效的。

生命周期注解语法
生命周期注解并不改变任何引用的生命周期的长短。与当函数签名中指定了泛型类型参数后就可以接受任何类型一样，当指定了泛型生命周期后函数也能接受任何生命周期的引用。生命周期注解所做的就是将多个引用的生命周期联系起来。

生命周期参数名称必须以单引号号（'）开头。生命周期参数的名称通常全是小写，而且类似于泛型类型，其名称通常非常短。'a 是大多数人默认使用的名称。生命周期参数注解位于引用的 & 之后，并有一个空格来将引用类型与生命周期注解分隔开。

&i32        // 没有生命周期的引用
&'a i32     // 有生命周期的引用
&'a mut i32 // 有生命周期的可变引用
生命周期注解告诉 Rust 多个引用的泛型生命周期参数如何相互联系。如果函数有一个生命周期 'a 的 i32 的引用的参数 first，还有另一个同样是生命周期 'a 的 i32 的引用的参数 second，这两个生命周期注解有相同的名称意味着 first 和 second 必须与这相同的泛型生命周期存在得一样久。

函数签名中的生命周期注解
泛型生命周期参数需要声明在函数名和参数列表间的尖括号中。

fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
通过在函数签名中指定生命周期参数，不会改变任何参数或返回值的生命周期，任何不坚持这个协议的类型都将被借用检查器拒绝。

当从函数返回一个引用，返回值的生命周期参数需要与一个参数的生命周期参数相匹配。如果返回的引用没有指向任何一个参数，那么唯一的可能就是它指向一个函数内部创建的值，它将会是一个悬垂引用，因为它将会在函数结束时离开作用域。

生命周期语法是关于如何联系函数不同参数和返回值的生命周期的。一旦他们形成了某种联系，Rust 就有了足够的信息来允许内存安全的操作并阻止会产生悬垂指针亦或是违反内存安全的行为。

结构体定义中的生命周期注解
可以定义存放引用的结构体，但需要为结构体定义中的每一个引用添加生命周期注解。

struct ImportantExcerpt<'a> {
    part: &'a str,
}
生命周期省略
函数或方法的参数的生命周期被称为 输入生命周期（input lifetimes），而返回值的生命周期被称为 输出生命周期（output lifetimes）。

译器用于判断引用何时不需要明确生命周期注解的规则。第一条规则适用于输入生命周期，而后两条规则则适用于输出生命周期。如果编译器检查完这三条规则并仍然存在没有计算出生命周期的引用，编译器将会停止并生成错误。

对于输入生命周期，每一个引用的参数都有它自己的生命周期参数。每一个被省略的函数参数成为一个不同的生命周期参数。
如果只有一个输入生命周期参数，它被赋给所有输出声明周期参数。
如果方法有多个输入生命周期参数，其中之一因为方法的缘故是 &self 或 &mut self，那么 self 的生命周期被赋给所有输出生命周期参数。这使得方法写起来更简洁。
方法定义中的生命周期注解
实现方法时，结构体字段的生命周期必须总是在 impl 关键字之后声明并在结构体名称之后被使用，因为这些生命周期是结构体类型的一部分。

impl 块里的方法签名中，引用可能与结构体字段中的引用相关联，也可能是独立的。另外，生命周期省略规则也经常让我们无需在方法签名中使用生命周期注解。

struct ImportantExcerpt<'a> {
   part: &'a str,
}


impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }

    fn announce_and_return_part(&self, announcement: &str) -> &str {
        println!("Attention please: {}", announcement);
        self.part
    }
}
静态生命周期
'static 生命周期存活于整个程序期间。所有的字符串字面值都拥有 'static 生命周期。let s: &'static str = "I have a static lifetime.";
