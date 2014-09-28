---
layout: post
title: "C语言有哪些鲜为人知的特性？"
category: trans
tags: [git,技术翻译]
image:
  feature: article.jpg
comments: True
share: true
---

*本文翻译自[Quora问答：What are some features of the C programming language that are not well-known?](http://www.quora.com/What-are-some-features-of-the-C-programming-language-that-are-not-well-known)*

*首发于[伯乐在线](http://blog.jobbole.com/77321/)*

**未经许可，禁止转载！**

##C语言有哪些鲜为人知的特性？

*译注:请在linux系统下测试本文中出现的代码*

###[Andrew Weimholt](http://www.quora.com/Andrew-Weimholt)的回答:

`switch`语句中的`case` 关键词可以放在`if-else`或者是循环当中
{% highlight c%}
switch (a)
{
    case 1:;
      // ...
      if (b==2)
      {
        case 2:;
        // ...
      }
      else case 3:
      {
        // ...
        for (b=0;b<10;b++)
        {
          case 5:;
          // ...
        }
      }
      break;

    case 4:
{% endhighlight %}

###[Brian Bi](http://www.quora.com/Brian-Bi)的回答:

####1. 声明紧随用途之后

理解声明有一条很简单的法则，不过不是什么“从左向右”这种没道理却到处宣传的法则。这一法则的观点是，一个声明是要告诉你，你所声明的对象要如何使用。例如：
{% highlight c%}
int *p; /* *p是int类型的, 因此p是指向int类型的指针 */
int a[5]; /* a[0], ..., a[4] 是int类型的, 因此a是int类型的数组 */
int *ap[5]; /* *ap[0], .., *ap[4] 是int类型的, 因此ap是包含指向int类型指针的指针数组 */
int (*pa)[5]; /* (*pa)[0], ..., (*pa)[4] 是int类型的, 因此pa是指向一个int类型数组的指针 */
{% endhighlight %}

更多详情请看这里: [Brian Bi's answer to C (programming language): Why doesn't C use better notation for pointers?](http://www.quora.com/C-programming-language/Why-doesnt-C-use-better-notation-for-pointers/answer/Brian-Bi)

####2. 指定初始化：

在C99之前，你只能按顺序初始化一个结构体。在C99中你可以这样做：

{% highlight c%}
struct Foo {
    int x;
    int y;
    int z;
};
Foo foo = {.z = 3, .x = 5};
{% endhighlight %}

这段代码首先初始化了`foo.z`,然后初始化了`foo.x`. `foo.y` 没有被初始化，所以被置为0。
这一语法同样可以被用在数组中。以下三行代码是等价的：
{% highlight c%}
int a[5] = {[1] = 2, [4] = 5};
int a[] = {[1] = 2, [4] = 5};
int a[5] = {0, 2, 0, 0, 5};
{% endhighlight %}

####3. 受限指针（C99）：
`restrict `关键词是一个限定词，可以被用在指针上。它向编译器保证，在这个指针的生命周期内，任何通过该指针访问的内存，都只能被这个指针改变。比如，在
{% highlight c%}
int f(const int* restrict x, int* y) {
    (*y)++;
    int z = *x;
    (*y)--;
    return z;
}
{% endhighlight %}

编译器可能会假设，`x`和`y` 所指的并不是同一个`int`对象，因为如果它们指向了同一个对象，则`*x`的值将可以通过`*y`修改，这正是你保证不会发生的。因此，将允许编译器来优化`f`,就好像函数原本被写做如下这样：
{% highlight c%}
int f(const int* restrict x, int* y) {
    return *x;
}
{% endhighlight %}

如果你违反协议向`f`传递两个指向同一int对象的指针时，将产生未定义行为。

我猜想，引入这一特性最初的动机之一是想让C语言在数值计算时可以Fortran一样快。在Fortran 中，默认假定数组不会重叠，因此只有你通过`restrict ` 限定词来显式的告诉编译器数组不能重叠，编译器才能在C语言中进行这样的优化。

####4. 静态数组索引(C99)

在
{% highlight c%}
void f(int a[static 10]) {
    /* ... */
}
{% endhighlight %}
中，你向编译器保证，你传递给`f` 的指针指向一个具有*至少*10个`int` 类型元素的数组的首个元素。我猜这也是为了优化；例如，编译器将会假定`a` 非空。编译器还会在你尝试要将一个可以被静态确定为null的指针传入或是一个数组太小的时候发出警告。

在
{% highlight c%}
void f(int a[const]) {
    /* ... */
}
{% endhighlight %}
你不能修改指针`a.`，这和说明符`int * const a.`作用是一样的。然而，当你结合上一段中提到的`static` 使用，比如在`int a[static const 10]` 中，你可以获得一些使用指针风格无法得到的东西。

####5. 泛型表达式（C11）

这个表达式会在编译期间根据控制表达式的类型，在一个含有一个或多个备选方案的集合中做出选择。下面这个例子可以很好的说明这一切：

{% highlight c%}
#define cbrt(X) _Generic((X), \
                        long double: cbrtl, \
                        default: cbrt, \
                        float: cbrtf \
                        )(X)
{% endhighlight %}

因此，如果`expr` 是`long double`类型的， `cbrt(expr)` 被转换为`cbrtl(expr)`，如果是`float`类型 则转换为`cbrtf(expr)` ，或是转换为`cbrt(expr)`，如果是其他不同的类型（比如说`double` ）。注意，` _Generic` 可以用在宏以外的地方，但是用在宏里面最好因为C不允许你进行函数重载。

###6. `wint_t` （C99）

我相信大家都知道`wint_t` 但是 `wint_t` 到底是个什么鬼东西呢？

好吧，记住`fgetc` 实际上并不会返回 `char` 。它会返回`int`。显然这是因为`fgetc` 必须返回返回一个与其他`char` 都不同的值，也就是`EOF`,表示到达文件末尾。基于相同的原因，`fgetwc` 并不返回`wchar_t`。它会返回一个类型，叫做`wint_t` 可以表示所有无效`wchar_t` 类型，包括`WEOF`，来表示到达文件末尾。


###[Michal Forišek](http://www.quora.com/Michal-Fori%C5%A1ek)的回答:

下面这段C程序可以准确的打印2的747次方而不产生误差。这是为什么呢？

程序：
{% highlight c%}
#include <stdio.h>
#include <math.h>
int main() {
    printf("%.0f\n",pow(2,747));
    return 0;
}
{% endhighlight %}

输出结果：
{% highlight c%}
740298315191606967520227188330889966610377319868419938630605715764070011466206019559325413145373572325939050053182159998975553533608824916574615132828322000124194610605645134711392062011527273571616649243219599128195212771328

{% endhighlight %}

答案：

这个问题包含两个部分。
其一，2的次方可以在`double` 中被准确的保存而不产生任何精度上的损失（这一结论直到2^1023都是对的，再往后就会产生上溢，得到一个正无穷的值）

另外一部分，很多人猜测是语言实现中的某些特殊情况导致的，但是实际上并非如此。的确，当输入的数据可以被2的某高次方整除时，有一部分代码被执行了，但是本质上这只是通常实现工作时的一个副作用。基本上，`printf` 在打印数字（任何类型）的时候只是做了从二进制到十进制的转换。并且由于结果对于浮点数可能会过大，`printf` 的内部实现包含和使用一个大整型实现，尽管在C中并没有大整型这种变量（在gcc源代码中，`vfprintf.c` 和`dtoa.c` 中包含了很多转换，如果你想要了解可以一看。）

如果你尝试打印3^474，

程序：

{% highlight c%}
#include <stdio.h>
#include <math.h>
int main() {
    printf("%.0f\n",pow(3,474));
    return 0;
}
{% endhighlight %}

输出结果：

{% highlight c%}
14304567688284661153278974752312031583901259203711201647725006924333106634519194823303091330277684776547167093155518867557708479462413116497799842448027156309852771422896137582164841870381535840058702788340257784498862132559872
{% endhighlight %}

结果仍然是一个很大的数且位数也正确，但是这一次却不够精确。这里会产生一个相对误差，因为3^474不能以双精度浮点数准确的表示。准确的数应该是这样的14304567688284660**3471**...

*译注：在linux系统上是可以的，在windows 64位上后面会有很多0*


###[Utkal Sinha](http://www.quora.com/Utkal-Sinha)的回答：

我发现一些C语言特性或者是小技巧，我觉得只有很少的人知道。

####1. 不使用加号来使数字相加

因为`printf()` 函数返回它所打印的字符的个数，我们可以利用这一点来使数字相加，代码如下：

{% highlight c%}
#include <stdio.h>

int add(int a,int b){
    if(a!=0&&b!=0)
        return printf("%*c%*c",a,'\r',b,'\r');
    else return a!=0?a:b;
}

int main(){
    int A = 0, B = 0;
    printf("Enter the two numbers to add\n");
    scanf("%d %d",&A,&B);
    printf("Required sum is %d",add(A,B));
{% endhighlight %}

利用位操作同样也可以做到：

{% highlight c%}
int Add(int x, int y)
{
    if (y == 0)
        return x;
    else
        return Add( x ^ y, (x & y) << 1);
}
{% endhighlight %}

####2. 条件运算符的用法

通常我们都这样使用它：
`x = (y < 0) ? 10 : 20;`
但是同样也可以这样用：
`(y < 0 ? x : y) = 20;`

####3. 在一个返回值为`void` 的函数中写一个`return` 语句

{% highlight c%}
static void foo (void) { }
static void bar (void) {
return foo(); // 注意这里的返回语句.
}

int main (void) {
bar();
return 0;
}
{% endhighlight %}

####4. 逗号表达式的使用

通常逗号表达式会这样使用：

{% highlight c%}
for (int i=0; i<10; i++, doSomethingElse())
{
  /* whatever */
}
{% endhighlight %}

但是你可以在其他任何地方使用逗号表达式：

`int j = (printf("Assigning variable j\n"), getValueFromSomewhere());`

每条语句都进行了求值，但是表达式的值是最后一个语句的值。

####5. 将结构体初始化为0

`struct mystruct a = {0};`

这将把结构体中全部元素初始化为0

####6. 多字符常量

`int x = 'ABCD';`

这会把x的值设置为**0x41424344**（或者**0x44434241**，取决于架构）

####7. `printf` 允许你使用变量来格式化格式说明符本身

{% highlight c%}
#include <stdio.h>

int main() {
    int a = 3;
    float b = 6.412355;
    printf("%.*f\n",a,b);
    return 0;
}
{% endhighlight %}

`*` 符号可以达到这一目的

希望这些可以帮助到大家
此致敬礼

###[Vivek Nagarajan](http://www.quora.com/Vivek-Nagarajan-1)

你可以在奇怪的地方使用`#include`

如果你写：
{% highlight c%}
#include <stdio.h>

void main()
{
    printf
#include "fragment.c"        
}
{% endhighlight %}

且`fragment.c` 包含：

{% highlight c%}
("dayum!\n");
{% endhighlight %}

这完全没有问题。只要`#include` 包含完整可解析的C表达式，预处理器并不在意它放在什么位置。

###[Vipul Mehta](http://www.quora.com/Vipul-Mehta-1)的回答：

####1. `printf` 格式限定符中指定（POSIX扩展语法）

`printf("%4$d %3$d %2$d %1$d", 1, 2, 3, 9); // 将会打印9 3 2 1`

####2. 在`scanf` 中忽略输入输入

`scanf("%*d%d", &a); // 如果输入1 2，则只会得到2`

####3. 在`switch` 中使用范围（gcc扩展语法）

{% highlight c%}
switch(c) {
  case 'A' ... 'Z': //do something
  break;
  case 1 ... 5 : //do something
}
{% endhighlight %}

####4. 使用前缀`ob` 来限定常数，使其被当做二进制数（gcc扩展语法）

`printf("%d",0b1101); // prints 13`

####5.完全正确的最短的C语言程序
`main;`

*译注：虽然编译没有error但是却不能执行*


###[Karan Bansal](http://www.quora.com/Karan-Bansal-3)的回答：

####**`scanf()`的力量**

假定我们有一个数组`char a[100]`
读取一个字符串：
`scanf("%[^\n]\n", a);//表示一直读取直到遇到'\n'，并且忽略掉'\n'`

读取字符串直到遇到逗号：
`scanf("%[^,]", a);//但是这次不会忽略逗号`

如果你想忽略掉某个输入，使用在`%` 后使用`*`，如果你想要得到`John Smith` 的姓:

{% highlight c%}
scanf("%s %s", temp, last_name); //典型答案，使用一个临时变量`
{% endhighlight %}

{% highlight c%}
scanf("%s", last_name);
scanf("%s", last_name);
// 另一种答案，使用一个变量但是调用两次 `scanf()`
{% endhighlight %}

{% highlight c%}
scanf("%*s %s", last);
//最佳答案，因为你不需要额外的变量或是调用两次`scanf()`

{% endhighlight %}

顺便提一句，你应该非常小心的使用`scanf` 因为它可能会是你的输入缓冲溢出！通常你应该使用`fgets` 和`sscanf` 而不是仅仅使用`scanf`,使用`fgets` 来读取一行，然后用`sscanf` 来解析这一行，就像上面演示的一样。

###[Afif Ahmed](http://www.quora.com/Afif-Ahmed)的回答：

`~-n` 等于`n-1`
`-~n` 等于`n+1`

原因：
当我们写`-n`时，实际上是以补码形式储存，所以`-n` 可以写成`~n + 1`，吧整个式子放在上面表达式的前面你就能明白原因了。
