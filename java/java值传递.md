# Java的传值调用

## 引子

之所以写这篇文章是因为前些天写了一篇《Java中真的只有值传递么？》探讨了网上关于Java只有值传递的说法，写这篇文章的缘由是因为之前看的文章讲解的Java只有值传递，讲的不是让我很明白，也没有拿出来专业的解释或定义，没有说服我。而我在《Java中真的只有值传递么？》这篇文章中又做了一些解读，发现自己也是没有抓住重点，这才有了今天这篇文章。

从那篇文章后，我了解到Java的参数传递牵涉到了Java语言的设计中的参数传递方式，所以在工作之余自己研究了一下，最终也能解释一下关于Java是值传递还是引用传递的说法。

Java 是引用传递还是值传递现在有以下这些说法：

>1、值传递和引用传递，区分的条件是传递的内容，如果是个值，就是值传递。如果是个引用，就是引用传递。
>
>2、传递的参数如果是普通类型，那就是值传递，如果是对象，那就是引用传递。
>
>3、Java中只有值传递。

关于这个问题应该是分情况回答的，或许在不同的认识下有不同的说法，不能简单的就说是值传递还是引用传递。

对或错都是相对的。

## 回顾

在谈这个问题之前我们先了解下值传递和引用传递的概念及现象。可以简单的通过几个例子来讲解的，大概是这样的。

### 值传递

例子1：

```java
public static void main(String[] args){
	TestJavaParamPass() tjpp = new TestJavaParamPass();
    int num = 10;
    tjpp.change(num);
    System.out.println("num in main():"+i);
}
public void change(int param){
    param = 20;
    System.out.println("param in change():"+param);
}
```

控制台输出:

```bash
param in change():10
num in main():20
```

mian()方法中的int变量num传递给change()方法，change()方法接收到后将值改变为20。通过看控制台输出，main()方法中的num变量的值没有改变。

**结论：实参没有被形参影响，基本类型是值传递。**

### 引用传递

例子2：

```java
public static void main(String[] args){
	TestJavaParamPass() tjpp = new TestJavaParamPass();
    User user = new User();
    user.setName("Jerry");
    tjpp.change(user);
    System.out.println("user in mian():"+user);
}
public void change(User param){
    param.setName("Tom");
    System.out.println("param in change():"+param);
}
```

控制台输出：

```bash
param in change():User(name=Tom}
user in mian():User(name=Tom}
```

main()方法中的user变量传递给change()方法，change()方法改变了其name属性值。通过看控制台输出，main()方法中的user变量的name属性值发生改变。

**结论：形参变了实参也变了，引用类型是引用传递。**

### 特殊的值传递

例子3：

```java
public static void main(String[] args){
	TestJavaParamPass() tjpp = new TestJavaParamPass();
    String name = "Jerry";
    tjpp.change(name);
    System.out.println("name in main():"+i);
}
public void change(String param){
    param = "Tom";
    System.out.println("param in change():"+param);
}
```

控制台输出：

```bash
param in change():Tom
name in mian():Jerry
```

String也是引用类型的数据类型，为什么值没改变？因为在change()方法里`param = "Tom";`相当于`param = new String("Tom");`就相当于param被重新赋值指向了另外一个对象。所以，其实String类型传的是引用，只不过被重新赋值指向了别的对象了，没有修改原对象。即，String本质上还是引用传递，表像上是值传递。

**结论：基本类型是值传递，引用类型是引用传递，String是特殊的值传递。**

看到这样的结论，没有去深究过，可能大部分程序员的认知都是这样的。

根据上面的例子我们先初步给值传递和引用传递下个定义，以及解释为什么大多数程序员都将String理解为是特殊的值传递。

### 概念提取

与其叫概念提取还不如叫结论总结呢。

- **值传递：**基本类型的变量在被传递给方法时，传递的是该变量的值（即复制自己的值传递给方法）。
- **引用传递：**引用类型的变量在被传递给方法时， 传递的是该变量的引用（即自己所指向的内存地址）。

- 为什么说**String是特殊的值传递**：是因为String和基本类型从表象来说表现出来的结果是一样，大概是为了便于记忆这个结果才这样说的吧。但是要知道String也是传递的引用，只不过它的引用被重新赋值，指向了别的对象了，所以不会影响原值。所以String不能简单的说是值传递。

而仅仅根据上面的实验就给值传递，引用传递下这样的结论是不是太草率了？

## 解析

对于文章开始时提到的哪些说法，前两中可以这样解释：

大概是因为int没有因为change方法而改变原值，所以就说它传过去的是自身的值，因而叫值传递；User对象经过change方法后，对象的数据变了，就认为是因为实参和形参指向的是同一片内存空间，内存空间的数据变了就都变了，传过去的是引用所以就说对象是引用传递。这样说的侧重点是传递的东西。

所以，如果从传递的东西的角度来看这两种说法也是没问题的呀。

至于`Java只有值传递`的说法，我查阅了一些资料结合网上的文章了解到了求值策略这个名词，这大概牵涉到了语言本身的设计。所以就从这些名词来探究Java的方法调用时参数传递的奥秘。

我们先来看看这些编程语言里关于参数传递函数调用有关的术语。

（以下术语来自Wiki： [https://zh.wikipedia.org/wiki/%E6%B1%82%E5%80%BC%E7%AD%96%E7%95%A5](https://zh.wikipedia.org/wiki/求值策略) ）

### 求值策略（Evaluation strategy）

> 在[计算机科学](https://zh.wikipedia.org/wiki/计算机科学)中，**求值策略**（英语：Evaluation strategy）是确定[编程语言](https://zh.wikipedia.org/wiki/编程语言)中[表达式](https://zh.wikipedia.org/wiki/表达式)的求值的一组（通常确定性的）规则。 重点典型的位于函数或算子上——求值策略定义何时和以何种次序求值给函数的实际参数，什么时候把它们代换入函数，和代换以何种形式发生。 



**求值策略**：是一组求值规则，用来定义如何为函数的实际参数求值。它是用来规定程序语言在方法、函数或过程调用时的传参策略，是在程序语言设计时就应该考虑的问题。而下面的这几个调用方式都属于求值策略。

### 传值调用（Call by value）

> “传值调用”求值是最常见的求值策略，[C](https://zh.wikipedia.org/wiki/C语言)和[Scheme](https://zh.wikipedia.org/wiki/Scheme)这样差异巨大的语言都在使用。在传值调用中实际参数被求值，其值被绑定到函数中对应的变量上（通常是把值复制到新内存区域）。如果函数或过程能把值赋给它的形式参数，则被赋值的只是局部拷贝——就是说，在函数返回后调用者作用域里的曾传给函数的任何东西都不会变。
>
> 传值调用不是一个单一的求值策略，而是指一类函数的实参在被传给函数之前就被求值的求值策略。 尽管很多使用传值调用的编程语言（如Common Lisp、Eiffel、Java）从左至右的求值函数的实际参数，某些语言（比如[OCaml](https://wiki.tw.wjbk.site/baike-OCaml)）从右至左的求值函数和它们的实际参数，而另一些语言（比如Scheme和C）未指定这种次序（尽管它们保证[顺序一致性](https://wiki.tw.wjbk.site/w/index.php?title=顺序一致性&action=edit&redlink=1)）。 



**传值调用**：在传值调用中，实际参数被求值后传递给被调函数。也就是说传值调用是**实参在被传给函数之前就被求值**的一种求值策略。

### 在Java中的体现

那什么叫**实参在被传给函数之前就被求值**呢？求的是谁的值呢？这个值又是什么呢？是怎么求得呢？

带着这些疑问，我们来看下面的例子。

如下，在调用change()方法时实参为`i`，当程序执行到change(i)这一行时，`i`是实参，这时`i`就要被求值了，会求出`i`的值即4传给change()方法；change()的形参`a`拿到的是实参`i`的值，是一个拷贝副本。

```java
change(int a){//拿到求得的实参的值
      a = a/2;
}
int i = 4;
change(i);
System.out.print(i);
```

因为是值的副本，所以在函数内对形参操作不会影响实参，所以输出是4。

这里我们举的例子是基本类型**int**类型的。那对于引用类型呢？

同样需要对实参求值，这时得到的值是实参的地址值，形参拿到的是实参的地址值，这个地址值指向的是u1等号后面使用new关键字开辟出来的那片内存空间，所以此时u2也指向这片内存空间，所以打印出来u2将会和u1输出同样的内容。

```java
change(User u2){//拿到求得的实参的地址值
      System.out.print(u2);
      u2 = getNewUser();
      u2.setName("$%#@*")
      System.out.print(u2);
}
User u1 = new User();
u1.setName("1234");
change(u1);
System.out.print(u1);
```

然后，我们模仿上面的change(int a)的方法里，**对形参接收到的值进行改变**。注意，**是形参的值**，对change(User u2)来说，形参u2接收到的值是地址值，我们咋改变它呢？我们可以让u2指向另一个内存空间，即通过getNewUser()方法获取一个新的User对象，用这种方式给u2一个新的地址值，这不就改变了吗。

此时我们看输出，发现经过change()方法实参u1打印信息没变，为什么？因为u1的地址值没变，且u2是获得新地址后（指向另一片内存），在新的这片内存里操作的，故而不会影响到之前的那片内存空间的数据。

这样基本类型和引用类型的实验方法是一样的，看到的效果也是一样的，即实参没有随形参的改变而改变。

## 总结

最后得出的结论：从语言设计的角度，Java的方法调用时参数的求值策略是**传值调用(Call by value)**的。

如果我们想表达引用类型传递的是引用，仅仅是想说传的是引用不是别的东西的话，我们可以说的明确点：引用类型传的是引用，和程序语言中的求值策略不沾边。那你说的引用传递就和取值策略中的传引用调用没关系，只是想表达传的是引用的话也没人会说你错。由此来看文章开头提到的前2种说法是不是也有解释的余地？

存在即合理，不同的说法有不同的前提条件不同的解释方式。如果是从程序语言设计的求值策略角度来问Java是哪种求值策略的话，那可以肯定的说是传值调用（Call by value）。



(以下术语摘抄自Wiki。能力有限，对这样专业名词还无法完美解读，仅供参考)

## 附录

传引用调用和传共享对象调用都是求值策略的一种。

### 传引用调用（Call by reference）

> 在“传引用调用”求值中，传递给函数的是它的实际参数的隐式[引用](https://zh.wikipedia.org/wiki/引用)而不是实参的拷贝。通常函数能够修改这些参数（比如赋值），而且改变对于调用者是可见的。因此传引用调用提供了一种调用者和函数交换数据的方法。传引用调用的语言中追踪函数调用的副作用比较难，易产生不易察觉的bug。
>
> 很多语言支持某种形式的传引用调用，但是很少有语言默认使用它。[FORTRAN II](https://zh.wikipedia.org/wiki/Fortran) 是一种早期的传引用调用语言。一些语言如[C++](https://zh.wikipedia.org/wiki/C%2B%2B)、[PHP](https://zh.wikipedia.org/wiki/PHP)、[Visual Basic .NET](https://zh.wikipedia.org/wiki/Visual_Basic_.NET)、[C#](https://zh.wikipedia.org/wiki/C_Sharp_(programming_language))和[REALbasic](https://zh.wikipedia.org/wiki/REALbasic)默认使用传值调用，但是提供一种传引用的特别语法。
>
> 在那些使用传值调用又不支持传引用调用的语言里，可以用[引用](https://zh.wikipedia.org/wiki/參照)（引用其他对象的对象），比如[指针](https://zh.wikipedia.org/wiki/指標_(電腦科學))（表示其他对象的内存地址的对象）来模拟。[C](https://zh.wikipedia.org/wiki/C语言)和[ML](https://zh.wikipedia.org/wiki/ML語言)就用了这种方法。这不是一种不同的求值策略（语言本身还是传值调用）。它有时被叫做“传地址调用”（call by address）。这可能让人不易理解。在C之类不安全的语言里会引发解引用[空指针](https://zh.wikipedia.org/wiki/空指针)之类的错误。但ML的引用是类型安全和内存安全的。
>
> 类似的效果可由[传共享对象调用](https://zh.wikipedia.org/wiki/求值策略#传共享对象调用（Call_by_sharing）)（传递一个可变对象）实现。比如Python、Ruby。
>
> 例：[C](https://zh.wikipedia.org/wiki/C语言)用指针模拟的传引用调用 
>
>  ```c
>void modify(int p, int* q, int* r) {
>  p = 27; // passed by value: only the local parameter is modified
>  *q = 27; // passed by value or reference, check call site to determine which
>     *r = 27; // passed by value or reference, check call site to determine which
>    }
>    
> int main() {
>  int a = 1;
>  int b = 1;
>     int x = 1;
>     int* c = &x;
>     modify(a, &b, c); // a是传值调用， b通过创建指针实现引用传递，c是按值传递的指针
>     //b and x are changed
>     return 0;
>    }
>    ```

### 传共享对象调用（Call by sharing）

> 此方式由Barbara Liskov命名[[1\]](https://zh.wikipedia.org/wiki/求值策略#cite_note-CLU_Reference_Manual-1)，并被Python、Java（对象类型）、JavaScript、Scheme、OCaml等语言使用。
>
> 与传引用调用不同，对于调用者而言在被调用函数里修改参数是没有影响的。如果要达成传引用调用的效果就需要传一个共享对象，一旦被调用者修改了对象，调用者就可以看到变化（因为对象是共享的，没有拷贝）。比如这段Python代码：
>
> ```Python
>def f(l):
>  l.append(1)
>  l = [2]
>    m = []
>    f(m)
> print(m)
> ```
> 
> 会输出[1]而不是[2]。因为列表是可变的，append方法改变了m。而赋值局部变量l的行为对外面作用域没有影响（在这类语言中赋值是给变量绑定一个新对象，而不是改变对象）。
>
> 使用C/C++语言的程序员可能因不能用指针等使函数返回多个值而感到不便，但是像Python这样的语言提供了替代方案：函数能方便的返回多个值，比C++11的std::tie更加简单。