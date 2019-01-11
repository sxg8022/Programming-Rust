# 第2章. RUST之旅

### 第2章

## RUST之旅

_Toute l’expérience d’un individu est construit sur le plan de son langage._ 

_\(An individual’s experience is built entirely in terms of his language.\)_

       __—Henri Delacroix

 在本章中,我们将讨论几个小程序,以了解RUST的语法、类型和语义是如何组合在一起以实现安全、并发和高效的代码。我们先看看怎么下载和安装RUST，然后再看几个简单的数学方面的代码，接着尝试一个基于第三方库的web服务程序， 最后使用多个线程加速绘制曼德布洛特集合的过程。

### 下载和安装RUST

安装RUST最好的方法是使用rustup和rust installer，可以访问[https://rustup.r](https://rustup.rs) 网址，查看安装说明，跟着一步一步的做就可以了。

你也可以访问[https://www.rust-lang.or](https://www.rust-lang.org) 网站，点击下载，可以获得Linux，macOS和Windows不同平台的安装包。RUST已经有一些操作系统的分发包。我们更喜欢rustup，因为它是管理RUST安装的工具，它就像Ruby的RVM或Node的VM。当RUST有新版本发布时，你可以输入 rustup update命令一键升级。

不管怎样，你已经完成了安装，在命令行你应当由3条新命令可以使用：

```bash
$ cargo --version
cargo 0.18.0 (fe7b0cdcf 2017-04-24)
$ rustc --version
rustc 1.17.0 (56124baa9 2017-04-24)
$ rustdoc --version
rustdoc 1.17.0 (56124baa9 2017-04-24)
$
```

这里的$是命令行提示符；在window系统里，它是C:/&gt;或者类似的提示符。运行这三条命令行命令，我们要获得每个命令返回已经安装相应程序的版本信息。依次执行每个命令：

* cargo是RUST的编译，包管理器，它是一个通用的工具。你可以用cargo创建一个新项目，编译，运行程序，以及管理程序所依赖的所有外部包。
* rustc是RUST的编译器。通常都是cargo调用rustc，但是有时我们需要直接调用rustc编译代码，满足我们特殊的编译需求。
* rustdoc是RUST的文档工具。如果你在源代码中编写符合格式的注释，rustdoc可以把这些注释生成结构良好的HTML帮助文档。就像rustc一样，我们通常让cargo执行rustdoc。



为了方便，cargo会创建一个基本的可以运行的项目架构，包括一些元数据：

```bash
$ cargo new --bin hello
        Created binary (application) `hello` project
```

这个命令创建了一个新的包目录结构，命名为 hello 这个 --bin参数指示cargo生成一个可执行的程序目录结构，不是一个库文件。看一下生成程序目录的根目录信息：

```bash
$ cd hello
$ ls -la
total 24
drwxrwxr-x. 4 jimb jimb 4096 Sep 22 21:09 .
drwx------. 62 jimb jimb 4096 Sep 22 21:09 ..
drwxrwxr-x. 6 jimb jimb 4096 Sep 22 21:09 .git
-rw-rw-r--. 1 jimb jimb 7 Sep 22 21:09 .gitignore
-rw-rw-r--. 1 jimb jimb 88 Sep 22 21:09 Cargo.toml
drwxrwxr-x. 2 jimb jimb 4096 Sep 22 21:09 src
$
```

我们看到 cargo生成了一个Cargo.toml文件，它包含了程序的元数据。此时这个文件包含了少量的元数据：

```yaml
[package]
name = "hello"
version = "0.1.0"
authors = ["You <you@example.com>"]
[dependencies]
```

如果我们的程序需要依赖其它库，我们可以把依赖信息记录在这个文件里，然后，cargo可以自动为我们根据这文件下载，创建，更新这些库文件。在第8章节，我们将全面详细的讲解Cargo.toml 。

cargo为我们的代码配置了git，包括创建了一个 _.git_ 元数据子目录和一个_.gitignore_文件。 你可以通过在命令行上指定—vcs none参数告诉Cargo跳过这一步。

这个src子目录，包含了RUST的所有代码：

```bash
$ cd src
$ ls -l
total 4
-rw-rw-r--. 1 jimb jimb 45 Sep 22 21:09 main.rs
```

看起来，好像cargo已经为我们写了程序。这个main.rs 文件包含如下内容：

```rust
fn main() {
   println!("Hello, world!");
}
```

在RUST中，你甚至不需要自己写“Hello，world！”代码。这是RUST的样本程序：两个文件，总共9行。

我们可以在程序目录结构中，调用cargo的run命令来编译，运行我们的程序：

```bash
$ cargo run
     Compiling hello v0.1.0 (file:///home/jimb/rust/hello)
     Finished dev [unoptimized + debuginfo] target(s) in 0.27 secs
     Running `/home/jimb/rust/hello/target/debug/hello`
Hello, world!
$
```

这里，cargo调用了RUST的rustc编译代码，然后调用run运行编译的程序。cargo把编译的可执行文件放到工程目录的根目录下的_target_子目录里：

```bash
$ ls -l ../target/debug
total 580
drwxrwxr-x. 2 jimb jimb 4096 Sep 22 21:37 build
drwxrwxr-x. 2 jimb jimb 4096 Sep 22 21:37 deps
drwxrwxr-x. 2 jimb jimb 4096 Sep 22 21:37 examples
-rwxrwxr-x. 1 jimb jimb 576632 Sep 22 21:37 hello
-rw-rw-r--. 1 jimb jimb 198 Sep 22 21:37 hello.d
drwxrwxr-x. 2 jimb jimb 68 Sep 22 21:37 incremental
drwxrwxr-x. 2 jimb jimb 4096 Sep 22 21:37 native
$ ../target/debug/hello
Hello, world!
$
```

当我们完成时，运行cargo的clean命令，可以帮我们清除编译生成的文件：

```bash
$ cargo clean
$ ../target/debug/hello
bash: ../target/debug/hello: No such file or directory
$
```

### 一个简单的函数

RUST的语法并非都是全新的。如果你熟悉C，C++，Java或者Javascript，通过一个RUST程序的代码结构，你会发现你熟悉的语法结构。这个函数计算两个整数的最大公约数，使用欧几里得算法：

```rust
fn gcd(mut n: u64, mut m: u64) -> u64 {
   assert!(n != 0 && m != 0);
   while m != 0 {
      if m < n {
         let t = m;
         m = n;
         n = t;
      }
      m = m % n;
   }
   n
}
```

fn是关键字（发音：fun）声明了一个函数。这里我们定义了一个命名为gcd的函数，这个函数有n和m两个参数，两个参数都是unsigned 64位整数类型。-&gt;在返回类型前面：我们的函数返回一个unsigned 64位整数类型。4个空格的缩进是RUST代码的标准排版。

RUST的整型类型的名字表明了它的大小和符号：i32 表示有符号32位整型；u8表示无符号8位整型（用于‘byte’值），诸如此类。isize和usize类型用于保存有符号指针地址和无符号指针地址，32平台，32位是长整型，64平台，64位是长整型。RUST也有2个浮点类型，f32和f64，它们是IEEE标准的单精度和双精度浮点类型，就像C和C++里的 float和double类型。

默认情况下，一旦一个变量被初始化，它的值就不可以被改变，但是在参数n和m前放置 mut（发音“mute”，是_mutable_的简写）关键字就可以在函数体中改变它们的值。实践中，大部分的变量不需要被改变；mut关键字在我们阅读代码时，可以给我们很好的提示。

这个函数一开始调用 assert!宏，验证两个参数都不是零。！字符表明了它是一个宏调用，不是函数调用。就像C和C++的assert宏一样，RUST的assert！宏检查参数是不是true，如果不是true，就会中断程序的运行，并给出一个友好的信息输出，信息里包含检查失败处的源码位置；这种突然的终止被称作panic。不像C和C++可以跳过检查断言，RUST不论怎么编译的程序，总是检查每一个断言。为了提升编译速度，RUST会跳过debug\_assert!宏断言的检查。

 这个函数的核心是一个while循环，其中包含一个if语句和一个赋值语句。 与C和c++不同，Rust不需要在条件表达式周围使用园括号，但是它的执行语句体需要用大括号。

一个let语句声明了一个局部变量，t就是我们函数体内的局部变量。只要RUST能从使用的上下文中推断出变量的类型，我们并不需要显示的写出它的类型。 在我们的函数中，从上下文中m和n的类型，可以推断唯一适用于t的类型是u64。RUST只推断函数执行体内的变量类型：你必须显示的写出 函数参数和返回值的类型。如果我们想显示的写出t的类型，我们可以这样写：

```rust
let t: u64 = m;
```

RUST有一个返回语句，但是gcd函数并不需要它。如果一个函数体在结束的地方有一个没有以分号结尾的表达式，那这个表达式就是此函数的返回值。事实上，任何一对花括号内都是一个语句块，一个语句块可以作为一个表达式运行的。例如，这是一个表达式，它打印一条消息，然后生成x.cos\(\)作为其值:

```rust
{
    println!("evaluating cos x");
    x.cos()
}
```

在函数结束时，这种确定函数返回值的方式，在RUST中是一种很典型的方式，只有在函数中途需要返回值时，才会显示的使用 return语句。

### 写和运行单元测试

 Rust对语言级别内置的测试提供了简单的支持。 要测试gcd函数，我们可以编写：

```rust
#[test]
fn test_gcd() {
    assert_eq!(gcd(14, 15), 1);
    assert_eq!(gcd(2 * 3 * 5 * 11 * 17,
        3 * 7 * 11 * 13 * 19),
        3 * 11);
}
```

这里，我们定义了一个名叫test\_gcd\(\)的函数，它调用gcd并且验证其返回值的正确性。在函数定义的顶部\#\[test\]标准了此函数是一个测试函数。正常的编译时，可以跳过测试函数，不做编译，当我们用cargo test命令运行程序时，测试函数参与编译而且会自动运行。假设我们已经在本章开始的hello项目中，编写了gcd和test\_gcd两个函数。 如果当前目录位于hello项目的某个子目录，则可以按照以下方式运行测试：

```rust
$ cargo test
    Compiling hello v0.1.0 (file:///home/jimb/rust/hello)
      Finished dev [unoptimized + debuginfo] target(s) in 0.35 secs
        Running /home/jimb/rust/hello/target/debug/deps/hello-2375a82d9e9673d7

running 1 test
test test_gcd ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
$
```

 我们可以让测试函数分散在我们的源代码项目中，放置在它们所执行的代码旁边，cargo测试将自动收集它们并运行它们。

 \#\[test\]注解是众多属性中的一个，属性可以标记函数或者其他声明，就像C++和C\#中的属性或者java中的注解。它们用于编译器的告警和代码样式检查，还包括带条件的代码（例如C和C++中的\#ifdef），带条件的代码可以让RUST知道如何与其他语言编写的代码交互，诸如此类。接下来我们会看到更多带属性的例子。

### 处理命令行参数

 如果我们想让程序将一组数字作为命令行参数并输出它们的最大公约数，main函数可以像如下写：

```rust
use std::io::Write;
use std::str::FromStr;
fn main() {
    let mut numbers = Vec::new();
    for arg in std::env::args().skip(1) {
        numbers.push(u64::from_str(&arg)
        .expect("error parsing argument"));
    }

    if numbers.len() == 0 {
        writeln!(std::io::stderr(), "Usage: gcd NUMBER ...").unwrap();
        std::process::exit(1);
    }

    let mut d = numbers[0];
    for m in &numbers[1..] {
        d = gcd(d, *m);
    }

    println!("The greatest common divisor of {:?} is {}",
    numbers, d);
}
```

这是一大段代码，让我们一部分一部分的看：

```rust
use std::io::Write;
use std::str::FromStr;
```

用use声明关键字引入了 Write和FromStr两个特性。我们将在 第11章节详细介绍RUST的特性，但是现在我们只是简单的介绍一下：一个特性相信对于集合的一个方法，而且集合中的每个元素的类型都实现了这个特性。虽然我们在代码中不用写名字为Write和FromStr的特性或函数，但是特性必须先要用use引入代码域内才能被使用。在目前情况下：

* 实现Write特性的类型都具有write\_fmt方法，这个方法可以把格式化后的文本写入流。std::io::Stderr类实现了Write特性，并且我们使用writeln!宏打印错误信息；这个宏扩展了调用write\_fmt方法的代码。
* 实现FromStr特性的类型都具有from\_str方法，这个方法尝试把一个字符串类型转换成指定类型。u64类型实现了FromStr特性，我们调用u64::from\_str方法，尝试把命令行参数做转换成u64类型。

我们来看看main函数：

```rust
fn main() {
```

我们的main函数并没有一个返回值，因此我们可以简单的省略 -&gt;和返回类型列表。

```rust
let mut numbers = Vec::new();
```

我们声明了一个可修改的局部变量numbers，用一个空的vector初始化它。Vec是RUST中一个可变长的vector数组类型，类似 C++中的std::vector，一个Python 中的list，或者 Javascript 中的array。虽然vector被设计成可以动态增长或收缩的数组，但是在RUST中，我们仍然要标记这个变量为mut可修改，我们才能把数字添加到数组的尾部。

numbers的类型是Vec&lt;u64&gt;类型，也就是成员值为u64类型的vector数组，但是在这个变量的前面我们不需要显示的写出类型。RUST可以为我们推断出numbers的类型，因为我们给numbers添加的成员值都是u64类型的，同时也因为我们要把numbers的成员值传送给gcd函数，而gcd函数只接受 u64类型的参数。

```rust
for arg in std::env::args().skip(1) {
```

 在这里，我们使用for循环来处理命令行参数，依次把每个参数给变量arg赋值，并计算循环体，跳过第一个参数，因为第一个参数是运行程序的名字。

std::env::args 函数返回一个迭代器，这个迭代器会按需要生成每一个参数，并且指示什么时候完成。迭代器在RUST中是普遍存在的；标准库中还包含许多其他迭代器，这些迭代器可以处理vector的成员，读取文件对象的行，接收通讯通道的返回消息和处理几乎所有其他有意义的循环。RUST的迭代器是非常高效的： 编译器通常能够将它们转换成与手写循环相同的代码。 我们将在第15章中展示它的工作原理并给出示例。

 除了使用for循环之外，迭代器还包含大量可直接使用的方法。 例如，std::env::args返回的迭代器生成的第一个值总是正在运行的程序名称。我们需要跳过这个程序名称参数，因此我们可以调用迭代器的skip方法创建一个新的迭代器，这个新的迭代器可以忽略第一个参数。

```rust
numbers.push(u64::from_str(&arg)
             .expect("error parsing argument"));
```

这里，我们调用u64::from\_str尝试把我们的命令行参数arg转换成无符号64位数字， from\_str是一个与u64类型相关联的函数，类似于c++或Java中的静态方法，而不是我们在手边的某个u64值上调用的方法。 from\_str函数不直接返回u64，而是返回一个Result对象，它指示解析成功还是失败。一个Result对象具有2个变量值：

*  写入Ok\(v\)的值，表示解析成功，v是解析成功的值
*  写入Err\(e\)的值，表示解析失败，e是解析失败的原因

执行输入，输出或者其他的与操作系统交互的函数都会返回一个Result对象，它的OK变量指示了成功和成功的结果值，如打开文件等操作，而它的Err变量指示了失败和失败的原因。 与大多数现代语言不同，Rust没有exceptions： 如第7章所述，使用Result或panic处理所有错误。

 我们使用Result的expect方法检查解析是否成功。 如果Result是Err\(e\)， expect将打印包含e描述的错误信息，并立即退出程序。 但是，如果结果是Ok\(v\)， expect只返回v本身，我们最终可以将它添加到vector里。

```rust
if numbers.len() == 0 {
    writeln!(std::io::stderr(), "Usage: gcd NUMBER ...").unwrap();
    std::process::exit(1);
}
```

numbers的len等于0表示没有数字是空数组，它们也没有最大公约数，因此我们要检查vector最少要有一个成员值，如果满足不了条件我们就退出程序并打印错误信息。我们用writeln!宏打印错误到标准的错误输出流， 标准错误输出流由std::io::stderr\(\)提供。调用.unwrap\(\)方法是检查打印错误信息是否失败的简单方式；调用expect也可以实现相同的功能，但是没必要这样做。

```rust
let mut d = numbers[0];
for m in &numbers[1..] {
    d = gcd(d, *m);
}
```

 这个循环使用d作为它的最大公约数值，每次迭代计算更新它使它保持到目前为止我们处理过的所有数字的最大公约数。 和以前一样，我们必须将d标记为可变的，以便在循环中对其赋值。

 for循环有两个神奇的之处。首先，我们写for m in &numbers\[1..\]；这里的&符号是做什么用的？其次，我们写gcd\(d, \*m\)；期中\*m中的\*是做什么用的？这两个符合是互为逆运算。

 到目前为止，我们的代码只对一些简单的值进行操作，比如用于固定大小内存块的整数。 但是现在我们要遍历一个vector，这个vector可以是任意大小——可能非常大。 在处理这些值时，Rust非常谨慎： RUST希望让程序员控制内存消耗，使每个值的生命周期变得清晰，同时仍然确保在不再需要时及时释放内存。

 当我们迭代numbers的时候，我们想告诉Rust vector的所有权应该和numbers保持一致； 我们只是在循环中借用它的成员，而不是获得成员的所有权。&numbers\[1..\]中的&操作符借用vector中从第二个成员开始的引用。这个for循环迭代每个成员的引用， 让m依次借用每个成员。 \*m中的\*运算符解引用m，产生其引用的值；这是我们要传递给gcd函数的u64值。 最后，由于numbers拥有vector，所以在main结束时，当numbers超出作用域时，Rust会自动释放vector。

RUST的所有权和引用规则对RUST管理内存和线程安全是非常关键的，我们在第4章详细讨论它们，在第5章详细讨论引用规则。 你需要适应这些规则才能适应RUST， 但是对于这次介绍性的旅行， 你需要知道的是&x借用了对x的引用，而\*r是解引用r的值。

继续我们这个程序的旅行：

```rust
println!("The greatest common divisor of {:?} is {}",
         numbers, d);
```

 在遍历numbers成员之后，程序将结果打印到标准输出流。 println !宏使用一个模板字符串，将其余参数的格式化后替换{…}在模板字符串中显示，并将结果写入标准输出流。

 与C和c++不同，如果程序成功完成，则需要main返回0，或者如果出现错误，则需要非零退出程序，Rust假设如果main返回，则程序成功完成。 只有通过显式地调用expect或std::process::exit等函数，我们才能使用错误状态代码终止程序。

 cargo run命令允许我们向程序传递参数，这样我们就可以尝试命令行处理：

```rust
$ cargo run 42 56
    Compiling hello v0.1.0 (file:///home/jimb/rust/hello)
     Finished dev [unoptimized + debuginfo] target(s) in 0.38 secs
      Running `/home/jimb/rust/hello/target/debug/hello 42 56`
The greatest common divisor of [42, 56] is 14
$ cargo run 799459 28823 27347
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `/home/jimb/rust/hello/target/debug/hello 799459 28823 27347`
The greatest common divisor of [799459, 28823, 27347] is 41
$ cargo run 83
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `/home/jimb/rust/hello/target/debug/hello 83`
The greatest common divisor of [83] is 83
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `/home/jimb/rust/hello/target/debug/hello`
Usage: gcd NUMBER ...
$
```

 在本节中，我们使用了Rust标准库中的一些特性。 如果您对可用的其他内容感到好奇，我们强烈建议您尝试看Rust的在线文档。 它有一个实时搜索功能，使学习变得容易，甚至包括源代码的链接。 rustup命令在您安装Rust时自动在您的计算机上安装一个RUST在线文档的副本。 您可以使用以下命令在浏览器中查看标准库文档:

```bash
$ rustup doc --std
```

 你也可以在https://doc.rust.lang.org/网站上查看。

### 一个简单的WEB服务

 Rust的优势之一是可以在crats .io网站上免费获得许多发布的包。cargo命令可以很方便我们的代码使用crats .io上发布的包：cargo 将下载正确版本的包，构建它，并根据请求更新它。一个RUST包，要么是一个库要么是一个可执行文件，它们都被称为crate；Cargo 和crates.io它们的功能就如它们的名字。

为了演示这是如何运行的，我们用iron web框架创建一个WEB服务，他是一个超级HTTP 服务，以及依赖的几个crates。 如图2-1所示， 我们的网站会提示用户输入两个数字，并计算它们的最大公约数。

![&#x5982;&#x56FE;2-1  &#x63D0;&#x4F9B;&#x8BA1;&#x7B97;GCD&#x7684;&#x7F51;&#x9875;](.gitbook/assets/qq-tu-pian-20190109112146.png)

首先，我们让cargo为我们创建一个包，命名为iron-gcd:

```bash
$ cargo new --bin iron-gcd
     Created binary (application) `iron-gcd` project
$ cd iron-gcd
$
```

 然后，我们将编辑新项目的Cargo.toml文件。文件会列出我们想要使用的包； 其内容如下：

```markup
[package]
name = "iron-gcd"
version = "0.1.0"
authors = ["You <you@example.com>"]

[dependencies]
iron = "0.5.1"
mime = "0.2.3"
router = "0.5.1"
urlencoded = "0.5.0"
```

Cargo.toml中\[dependencies\]段里每一行都是一个crates.io上的包名和相应版本号。这里显示的版本可能不是最新的，在crates.io上有更新的版本， 即使有新版本的包被发布，我们依然可以使用指定的经过测试的版本，这样可以确保原有的代码依然能正确编译。在第8章，我们会详细讨论版本管理问题。

 注意，我们只需要写那些将直接使用的包;cargo负责将其他依赖的包自动下载。

 在我们的第一次迭代中，我们将保持web服务的尽量简单: 它只提供提示用户输入要计算的数字的页面。在iron-gcd/src/main.rs，我们将放置以下文本:

```rust
extern crate iron;
#[macro_use] extern crate mime;

use iron::prelude::*;
use iron::status;

fn main() {
    println!("Serving on http://localhost:3000...");
    Iron::new(get_form).http("localhost:3000").unwrap();
}

fn get_form(_request: &mut Request) -> IronResult<Response> {
    let mut response = Response::new();
    response.set_mut(status::Ok);
    response.set_mut(mime!(Text/Html; Charset=Utf8));
    response.set_mut(r#"
        <title>GCD Calculator</title>
        <form action="/gcd" method="post">
            <input type="text" name="n"/>
            <input type="text" name="n"/>
            <button type="submit">Compute GCD</button>
        </form>
    "#);
    
    Ok(response)
}
```

程序的开始用 extern crate指令引入iron 和 mime 两个crate包，它们是在我们程序文件Cargo.toml中引入的。在extern crate mime前面的\#\[macro\_use\]注解告诉RUST，我们要使用mime这个包里的宏，请导出这个包里的宏。 接下来，我们使用use声明来引入这些板条箱的一些公共特性。

声明 use iron::prelude::\*使iron::prelude模块的所有公共名称在我们的代码中直接可见和使用。通常，最好是写清楚你要使用的函数名等， 就像我们对iron::status所做的那样； 但是按照惯例，当一个模块被命名为prelude时，这意味着它的导出旨在提供crate的任何用户可能都需要的通用函数功能等。 在这种情况下，通配符\* use指令更有意义。

我们的main函数是简单的： 它打印一条消息，提醒我们如何连接到WEB服务器，然后调用Iron::new来创建WEB服务，最后在本地机器上设置TCP侦听，侦听端口是3000。 我们将get\_form函数传递给Iron::new，服务使用该函数处理所有请求； 我们很快就会改进。

 get\_form函数接受一个可变的引用 &mut，该引用表示被处理的HTTP请求对象。 虽然这个特定的处理程序函数从不使用它的\_request参数，但是稍后我们将看到一个使用\_request参数的函数。 目前，我们还不使用该参数，为了不让RUST警告我们，在参数名字前加下划线，消除警告。

 在函数体中，我们创建一个Response对象，作为请求的返回对象。 set\_mut方法使用它的参数类型来决定要设置响应的哪一部分，因此对set\_mut的每次调用实际上是设置Response的不同部分：通过status::Ok设置HTTP状态；通过内容的媒体类型\(使用mime!宏，我们从mime的crate导入的宏\)设置header的Content-Type值；设置一个字符串作为Response返回内容。

 由于Response返回内容文本包含双引号,我们RUST的“原始字符串“语法：字母r加零个或多个标记\(即\#字符\)再加一个双引号开始,然后连接字符串的内容，终止由另一个双引号和相同数量的标记作为结尾。 任何字符都可以在未转义的原始字符串中出现，包括双引号；实际上， 在原始字符串内无法识别像\"这样的转义序列。

 函数的返回类型IronResult&lt;Response&gt;是我们前面遇到的Result类型的另一种变体： 对于成功的响应值r，这是Ok\(r\)，对于错误值e，这是Err\(e\)。 我们在函数体的最后创建返回值Ok\(response\)，使用“在表达式结尾不加分号”语法隐式指定函数的返回值。

我们写了main.rs，现在可以用cargo命令运行这个程序： 获取所需的crates，编译它们，构建我们的程序，并将crates和我们的程序做链接，最后启动程序：

```bash
$ cargo run
    Updating registry `https://github.com/rust-lang/crates.io-index`
 Downloading iron v0.5.1
 Downloading urlencoded v0.5.0
 Downloading router v0.5.1
 Downloading hyper v0.10.8
 Downloading lazy_static v0.2.8
 Downloading bodyparser v0.5.0
...
 Compiling conduit-mime-types v0.7.3
 Compiling iron v0.5.1
 Compiling router v0.5.1
 Compiling persistent v0.3.0
 Compiling bodyparser v0.5.0
 Compiling urlencoded v0.5.0
 Compiling iron-gcd v0.1.0 (file:///home/jimb/rust/iron-gcd)
    Running `target/debug/iron-gcd`
Serving on http://localhost:3000...
```

 此时，我们可以在浏览器中访问给定的URL，并看到图2-1所示的页面。

 不幸的是，单击Compute GCD除了将浏览器导航到URL http://localhost:3000/gcd之外什么也做不了，然后URL http://localhost:3000/gcd将显示相同的页面；实际上，我们服务上的每个URL都这样，什么也做不了。 接下来让我们修复这个问题，使用Router类型将不同的处理程序与不同的路径关联起来。

 首先，让我们通过在iron-gcd/src/main.rs中添加以下声明来设置能够无条件地使用Router：

```rust
extern crate router;
use router::Router;
```

 Rust程序员通常会把所有extern crate和use声明一起放到文件的顶部，但这并不是绝对必要的： Rust允许声明以任何顺序出现，但是必须明确放到代码块的哪个嵌套层级。（ 宏定义和带有\#\[macro\_use\]注解的extern crate 是这个规则的例外:它们必须在使用之前出现。）

 然后我们可以修改main函数，如下所示：

```rust
fn main() {
    let mut router = Router::new();
    router.get("/", get_form, "root");
    router.post("/gcd", post_gcd, "gcd");
    println!("Serving on http://localhost:3000...");
    Iron::new(router).http("localhost:3000").unwrap();
}
```

 我们创建一个Router，为两个指定的路径建立处理函数，然后将该Router作为请求处理程序传递给Iron::new，从而生成一个web服务，该服务通过URL路径来决定调用哪个处理函数。

 现在我们可以编写post\_gcd函数：

```rust
extern crate urlencoded;

use std::str::FromStr;
use urlencoded::UrlEncodedBody;

fn post_gcd(request: &mut Request) -> IronResult<Response> {
    let mut response = Response::new();
    let form_data = match request.get_ref::<UrlEncodedBody>() {
        Err(e) => {
            response.set_mut(status::BadRequest);
            response.set_mut(format!("Error parsing form data: {:?}\n", e));
            return Ok(response);
        }
    
        Ok(map) => map
    };

    let unparsed_numbers = match form_data.get("n") {
        None => {
            response.set_mut(status::BadRequest);
            response.set_mut(format!("form data has no 'n' parameter\n"));
            return Ok(response);
        }
        
        Some(nums) => nums
    };
    
    let mut numbers = Vec::new();
    for unparsed in unparsed_numbers {
        match u64::from_str(&unparsed) {
            Err(_) => {
                response.set_mut(status::BadRequest);
                response.set_mut(
                    format!("Value for 'n' parameter not a number: {:?}\n",
                        unparsed));
                return Ok(response);
            }
            Ok(n) => { numbers.push(n); }
        }
    }
    
    let mut d = numbers[0];
    for m in &numbers[1..] {
        d = gcd(d, *m);
    }
    
    response.set_mut(status::Ok);
    response.set_mut(mime!(Text/Html; Charset=Utf8));
    response.set_mut(
        format!("The greatest common divisor of the numbers {:?} is <b>{}</b>\n",
            numbers, d));
    Ok(response)
}

```

 这个函数大部分是一系列match表达式，对于  
C、c++、Java和JavaScript程序员来说是陌生的，但对于使用Haskell和OCaml的人来说，这是很熟悉的写法。 我们已经提到Result对象，要么是成功值s的Ok\(s\)，要么是错误值e的Err\(e\)。 给定一些Result res，我们可以检查它是哪个成员变量，并使用match表达式访问它所包含的任何值:

```rust
match res {
    Ok(success) => { ... },
    Err(error) => { ... }
}
```











