快速穿越Haskell荆棘
====================
![github](imag/magritte_pleasure_principle.jpg "Title")

一篇短而不失技术含量的Haskell教程。

目录
----

*	[1.引言](#1.引言)
	*	[1.1安装](#1.1安装)
	*	[1.2莫要惊慌](#1.2莫要惊慌)
	*	[1.3Haskell基础](#1.3Haskell基础)
*	[致谢](#致谢)

我认为所有的开发者都应该去学习一下Haskell。虽然没有必要成为Haskell方面的大牛，但是了解Haskell所<br>
支持的特性能够开阔你的视野。

主流的语言通常都有一些共同特点：

-变量 <br>
-循环 <br>
-指针[<sup>1</sup>](#致谢) <br>
-数据结构，对象和类（更多...） <br>

Haskell非常独特。这门语言使用的很多概念是我之前从未接触过的。而这些编程思想能够使你成为优秀的程序员。

对我来，说学习Haskell确实有难度。在文章中，我会把我学习过程中遇见的困惑呈现出来，以便大家提高。

学习Haskell没有捷径可走，正是这样，这篇文章理解起来还是存在难度的。Haskell的掌握带有难度和挑战，我 <br>
认为这是一件好事。由于Haskell不容易理解，学习起来才充满意义。

通常学习Haskell方法是去学习两本经典的书，一本是[Learn You a Haskell](http://learnyouahaskell.com/),另一本则是[Real World Haskell](http://www.amazon.cn/dp/0596514980) 。<br>
我非常赞同，如果想你全面掌握Haskell，精读这两部著作是必不可少的。

然而，这篇文章非常简单的囊括了Haskell所带有的主要特点。同时文中也会包涵我在学习Haskell的疑惑之处。

文章内容分成五部分：

*	引言：一些简短的例子展示Haskell人性化之处
*	Haskell基础：Haskell语法以及一些重要的概念
*	困难部分：
	*	函数式风格；包含一个更深入的例子，展示了从命令式到函数式风格的转变
	*	类型；包含类型和标准二叉书的例子
	*	无限结构体；包含修改一棵无限的二叉树的例子！
*	超级困难部分：
	*	IO 处理；包含一个短小的例子
	*	IO 技巧展示；由于我对IO了不够了解，其中没有涉及具体的IO细节
	*	单体；难以置信的运行结果；
*	附录：
	*	更多关于无限树的知识；从数学方面来讨论无限树的概念

				提示：当看到一个文件名以.lhs后缀结尾，你可以单击单击文件名然后取得该文件。如果你将该文件另存为
				filename.lhs，可以通过如下命令运行
					runhaskell filename.lhs	
				除了一小部分外，大部分源码文件能够正常运行。
	
[00_hello_world.lhs](code/00_hello_world.lhs)
###1.引言###
####1.1安装####
![github](imag/Haskell-logo.png)

[Haskell Platform](http://www.haskell.org/platform/)上提供各平台上的Haskell安装方式。

工具：

-ghc：编译器类似于C语言的编译器gcc。<br>
-ghci：Haskell的交互解释器 (REPL)<br>
-runhaskell：用于直接运行一个事先没有编译的脚本程序，这样虽然比较方便但比起先编译再运行的方式要慢得多<br>

####1.2莫要惊慌####
![github](imag/munch_TheScream.jpg)

许多介绍Haskell的书或文章通常会以引入深奥的公式开篇(如快排，斐波那契等公式)。我会以另外一种方式进行。一开始
我不会介绍Haskell的强大之处。在最前面提到的都是一些Haskell和其他编程语言相似部分。下面我们看下最常见的
“Hello World”。

		main = putStrLn "Hello World!"
		
为了运行它，你可以把上面的代码保存在一个hello.hs的文件中然后运行如下命令：
	
		~ runhaskell ./hello.hs
		Hello World!

或者可下载文书式的Haskell源码，会看到一个如之前介绍的链接名。下载该文件储存为00_hello_world.lhs然后运行它

		~ runhaskell 00_hello_world.lhs
		Hello World!

[00_hello_world.lhs](code/00_hello_world.lhs)

[10_hello_you.lhs](code/10_hello_you.lhs)

接下来，程序会要求你输入你的名字，然后回显“Hello”加上你的名字：

		main = do
    	print "What is your name?"
    	name <- getLine
    	print ("Hello " ++ name ++ "!")
    	
让我们来对比一下类似功能在其他几种命令式语言中的实现：

		#Python
		print "What is your name?"
		name=raw_input()
		print "Hello %s!" % name


		# Ruby
		puts "What is your name?"
		name = gets.chomp
		puts "Hello #{name}!"
		
		
		// In C
		#include <stdio.h>
		int main (int argc, char **argv) {
			char name[666]; // <- An Evil Number!
			// What if my name is more than 665 character long?
			printf("What is your name?\n"); 
			scanf("%s", name);
			printf("Hello %s!\n", name);
			return 0;
		}
		
这几种语言在结构上是相似的，但存在一些语法声明上的差异。教程的主要部分会解释产生不同之处的原因。

在Haskell中会有一个main函数并且每个对象都有一个对应的类型。main的类型是IO()。这表明main会带来一些副作用。

总之，对于Haskell来说有很多和主流命令式语言相似的地方。

[10_hello_you.lhs](code/10_hello_you.lhs)

[20_very_basic.lhs](code/20_very_basic.lhs)

####1.3Haskell基础####
![github](imag/picasso_owl.jpg)

为了深入学习，首先要了解一些Haskell的核心属性。

函数式

Haskell是一种函数式语言。如果你习惯于其他命令式编程语言，要掌握函数式编程必须学习许多新东西。庆幸的是这些新概念同样有助
于命令式编程。

智能的静态类型

与C，C++和Java的方式不同，Haskll的类型系统能够帮助你完成许多工作。

纯洁性

通常你的函数不会修改任何外部的东西。这意味着不能修改变量的值，不能获取用户输入，不能向屏幕输出，不能发射导弹(囧)。另一方面
并发也非常容易实现。对于纯洁的代码，Haskell能够保证没有歧义的结果。同时也能够更容易审查你的程序。具有纯洁性的程序能够预防
bug的产生。

而且，纯洁的函数遵循Haskell中的一个基本原则：

		给函数传人相同的参数总是产生相同的结果。

惰性

一般来说，惰性在语言设计中并不常见。通常Haskell并不会主动演算某一部分，直到该部分需要使用时，才对其求值。因此，Haskell提供了
非常优雅的方式来操控无穷的数据结构。

在最后，关于如何阅读Heskell源码，对我来说，这和阅读科技论文很像，在其中某些部分是很容易理解的，但遇见公式，你会慢下来去理解。
对于Haskell，不要太在意你不了解的语法细节。如果你遇见>>=， <$>， <-或其他一些奇怪的符号，跳过他们继续阅读代码。 


###致谢###
1.尽管大多数现代编程语言尽力去规避它，但是它还是会以另一种形式表示出来。
