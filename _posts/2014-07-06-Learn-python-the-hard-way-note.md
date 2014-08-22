---
layout: post
title: "Learn Python The Hard Way 学习笔记"
category: python
tags: [python, 笔记]
image:
  feature: pythonbanner.png
comments: True
share: true
---

[例程代码和部分答案@Github](https://github.com/hanxiaomax/LPTHW_code_examples)

目录

* Table of Contents
{:toc}

##LEARN PYTHON THE HARD WAY 自学笔记

###1.开发环境搭建

###2.第一个程序

###3.注释

###4.算数

###5.变量和变量名

###6.更多变量和打印

###7.字符串和文本

**%r和%s的区别**

{% highlight python %}
x="There are %d type of people." % 10 
binary="binary"
do_not="don't"
y="Those who know %s and those who %s." %(binary,do_not)


**字符串赋值时既可以使用占位符，并且可以引用变量**
print x
print y

print "I said: %r."% x
print "I also said: '%s'." %y

""" %r和%s是不一样的，%r使用rper()方法处理字符串，返回结果会多一个引号,前者通常用来debug"""

hilarious=False
joke_evaluation="Isn't that joke so funny?! %r"

print joke_evaluation% hilarious


w="This is the left side of..."
e="a string with a right side."

print w + e #字符串合并
{% endhighlight %}~

###8.打印，打印

**占位符使用和中文输出**

{% highlight python %}
formatter = "%r %r %r %r"

print formatter % (1, 2, 3, 4)
print formatter % ("one", "two", "three", "four")
print formatter % (True, False, False, True)
print formatter % (formatter, formatter, formatter, formatter)
print formatter % (
    "I had this thing.",
    "That you could type up right.",
    "But it didn't sing.",
    "So I said goodnight."
)
print "%r" %"哦"#如果用%r会打印'\xe5\x93\xa6\xef\xbc\x9
print "%s" %"哦"#仍然有乱码，但是已经是中文了
print u"你好世界" #可以正常输出中文
{% endhighlight %}

###9.打印，打印，再打印

**多行文本打印**

{% highlight python %}
# Here's some new strange stuff, remember type it exactly.

months = "Jan\nFeb\nMar\nApr\nMay\nJun\nJul\nAug"
print "Here are the months: ", months
print "Here are the days:%r"% (months)#不会换行，而是打印\n

print """
There's something going on here.
With the three double-quotes.
We'll be able to type as much as we like.
Even 4 lines if we want, or 5, or 6.
"""
{% endhighlight %}

**两种使用变量的打印方式**

{% highlight python %}
print "Here are the months: ", months
print "Here are the days:%r"% (months)
{% endhighlight %}

**两种多行打印方式**

{% highlight python %}
print """
There's something going on here.
With the three double-quotes.
We'll be able to type as much as we like.
Even 4 lines if we want, or 5, or 6.
"""
print "\n123\n123\n123"
{% endhighlight %}

###10.转义符 

###11.输入

**会自动转换的input()和更加安全的raw_input()**

raw_input()
输入默认是字符串，如果需要int需要强制转换

{% highlight python %}
print type(raw_input())#返回str

print type(input())
"""
会自动转换类型，但是有安全风险，比如
abc
是会出现错误的，应该是"abc"，需要让用户小心输入
"""

{% endhighlight %}

###12.提醒用户输入

**查看函数手册**

{% highlight python %}
python -m pydoc raw_input
{% endhighlight %}


###13.参数，参数解包，变量

**命令行参数**

[命令行参数介绍](http://learnpythonthehardway.org/book/appendixa.html)

{% highlight python %}
"""
unpack:
Take whatever is in argv, unpack it, and assign it to all of these variables on the left in order
"""

from sys import argv
script, first, second, third = argv
print "The script is called:", script
print "Your first variable is:", first
print "Your second variable is:", second
print "Your third variable is:", third
{% endhighlight %}

$ python 13.py a b c
1.注意这里一共是4个命令行参数
2.命令行参数是作为str类型进入的，可以强制转换

###14.模拟一个命令行程序

{% highlight python %}
from sys import argv

script, user_name = argv
prompt = '> '

print "Hi %s, I'm the %s script." % (user_name, script)
print "I'd like to ask you a few questions."
print "Do you like me %s?" % user_name
likes = raw_input(prompt)

print """
Alright, so you said %r about liking me.
""" % (likes)
{% endhighlight %}

###15.读取文件

`.read()`

{% highlight python %}
from sys import argv
script, filename = argv
txt = open(filename)
print "Here's your file %r:" % filename
print txt.read()
txt.close()
{% endhighlight %}

###16.读写文件

**close** -- Closes the file. Like File->Save.. in your editor.  
**read** -- Reads the contents of the file. You can assign the result to a variable.  
**readline** -- Reads just one line of a text file.  
**truncate** -- Empties the file. Watch out if you care about the file.  
**write(stuff)** -- Writes stuff to the file.  

{% highlight python %}
from sys import argv
script, filename = argv
print "We're going to erase %r." % filename
print "If you don't want that, hit CTRL-C (^C)."
print "If you do want that, hit RETURN."
raw_input("?")
print "Opening the file..."
target = open(filename, 'w')
print "Truncating the file.  Goodbye!"
target.truncate()

print "Now I'm going to ask you for three lines."

line = raw_input("line: ")
print "I'm going to write these to the file."

target.write(line)
target.write("\n")

print "And finally, we close it."
target.close()
{% endhighlight %}

###17.更多有关文件的内容
**结合os模块操作函数**

文件与目录处理：[File and Directory Access](https://docs.python.org/2/library/filesys.html)

操作系统模块：[Module os](https://docs.python.org/2/library/os.html#module-os)

{% highlight python %}
from sys import argv
from os.path import exists

script, from_file, to_file = argv

print "Copying from %s to %s" % (from_file, to_file)
# we could do these two on one line too, how?
in_file = open(from_file)
indata = in_file.read()
print "The input file is %d bytes long" % len(indata)
print "Does the output file exist? %r" % exists(to_file)
raw_input()

out_file = open(to_file, 'w')
out_file.write(indata)

out_file.close()
in_file.close()
{% endhighlight %}


**os.path和time.ctime**

{% highlight python %}
#返回值是炒年糕1970年1月1日算起的
print os.path.getctime('C:\Users\\asus\Desktop\python\\1.txt')
#转换为可理解形式
print time.ctime(os.path.getctime('C:\Users\\asus\Desktop\python\\1.txt'))
{% endhighlight %}

###18.命名，变量，函数
**任意个数的函数参数**

>What does the * in *args do?
That tells Python to take all the arguments to the function and then put them in args as a list. It's like argv that you've been using, but for functions. It's not normally used too often unless specifically needed.

{% highlight python %}
def print_two(*args):
    arg1, arg2 = args 
    print "arg1: %r, arg2: %r" % (arg1, arg2)
{% endhighlight %}

{% highlight python %}
# this one is like your scripts with argv
def print_two(*args):
    arg1, arg2 = args
    print "arg1: %r, arg2: %r" % (arg1, arg2)

# ok, that *args is actually pointless, we can just do this
def print_two_again(arg1, arg2):
    print "arg1: %r, arg2: %r" % (arg1, arg2)

# this just takes one argument
def print_one(arg1):
    print "arg1: %r" % arg1

# this one takes no arguments
def print_none():
    print "I got nothin'."


print_two("Zed","Shaw")
print_two_again("Zed","Shaw")
print_one("First!")
print_none()
{% endhighlight %}

####19.函数和变量

###20.函数和文件
[文件对象](https://docs.python.org/2/library/stdtypes.html#file-objects)

{% highlight python %}
#coding: utf-8
from sys import argv

script, input_file = argv

def print_all(f):
    print f.read()

def rewind(f):
    f.seek(0)

def print_a_line(line_count, f):
    print line_count, f.readline()

current_file = open(input_file)

print "First let's print the whole file:\n"

print_all(current_file)

print "Now let's rewind, kind of like a tape."

rewind(current_file)

print "Let's print three lines:"
"""
current_line = 1
print_a_line(current_line, current_file)

current_line = current_line + 1
print_a_line(current_line, current_file)

current_line = current_line + 1
print_a_line(current_line, current_file)
"""
#文件迭代器
line_num=1
for current_line in current_file:
    print line_num , current_line
    line_num+=1
{% endhighlight %}

###21.函数返回值

{% highlight python %}
def secret_formula(started):
    jelly_beans = started * 500
    jars = jelly_beans / 1000
    crates = jars / 100
    return jelly_beans, jars, crates
"""注意多返回值"""
{% endhighlight %}

###22.复习

###23.读代码

- github.com
- launchpad.net
- gitorious.org
- sourceforge.net
- freecode.com

###24.更多练习

- 转义符
- 多行文字打印
- print
- 函数

###25.更多更多的练习

**很容易的创建模块和调用**

ex25.py

{% highlight python %}
def break_words(stuff):
    """This function will break up words for us."""
    words = stuff.split(' ')
    return words

def sort_words(words):
    """Sorts the words."""
    return sorted(words)

def print_first_word(words):
    """Prints the first word after popping it off."""
    word = words.pop(0)
    print word

def print_last_word(words):
    """Prints the last word after popping it off."""
    word = words.pop(-1)
    print word

def sort_sentence(sentence):
    """Takes in a full sentence and returns the sorted words."""
    words = break_words(sentence)
    return sort_words(words)

def print_first_and_last(sentence):
    """Prints the first and last words of the sentence."""
    words = break_words(sentence)
    print_first_word(words)
    print_last_word(words)

def print_first_and_last_sorted(sentence):
    """Sorts the words then prints the first and last one."""
    words = sort_sentence(sentence)
    print_first_word(words)
    print_last_word(words)
{% endhighlight %}

25-1.py

{% highlight python %}
#coding: utf-8
import ex25 #不能是25.py
sentence = "All good things come to those who wait."
words = ex25.break_words(sentence)
print words

sorted_words = ex25.sort_words(words)
print sorted_words

print ex25.print_first_word(words)
print ex25.print_last_word(words)

print words
{% endhighlight %}

分割字符串

{% highlight python %}
print "All good things come to those who wait.".split(' ')
print "All,good,things,come,to,those,who,wait.".split(',')
print "All<>good<>things<>come<>to<>those<>who<>wait.".split('<>')
print "All<>good<>things<>come<>to<>those<>who<>wait.".split(' ')
print '1<>2<>3'.split('<>')
{% endhighlight %}

###26.测试

###27.逻辑运算

###28.布尔操作符

###29.如果

**if语句**

###30.如果，或者

{% highlight python %}
if cars > people:
    print "We should take the cars."
elif cars < people:
    print "We should not take the cars."
else:
    print "We can't decide."
{% endhighlight %}

###31.做决定

###32.循环和列表

{% highlight python %}
the_count = [1, 2, 3, 4, 5]
for number in the_count:
    print "This is count %d" % number
# we can also build lists, first start with an empty one
elements = []
# then use the range function to do 0 to 5 counts
for i in range(0, 6):
    print "Adding %d to the list." % i
    #列表方法：追加
    elements.append(i)
{% endhighlight %}


**同时遍历多个列表**

{% highlight python %}
#同时遍历多个列表
for i in mylist,the_count:
    print i
{% endhighlight %}

**How do you make a 2-dimensional (2D) list?**  

That's a list in a list like this: [[1,2,3],[4,5,6]]

###33.while循环

###34.获取列表元素

###35.分支和函数

**1. exit(0)**

`from sys import exit`

**2. in关键字**

{% highlight python %}
if "c" in string:
    print string
{% endhighlight %}

###36.设计和debug

**Rules For If-Statements**

1. Every if-statement must have an else.
2. If this else should never be run because it doesn't make sense, then you must use a die function in the else that prints out an error message and dies, just like we did in the last exercise. This will find many errors.
3. Never nest if-statements more than two deep and always try to do them one deep. This means if you put an if in an if then you should be looking to move that second if into another function.
4. Treat if-statements like paragraphs, where each if elif else grouping is like a set of sentences. Put blank lines before and after.
5. Your boolean tests should be simple. If they are complex, move their calculations to variables earlier in your function and use a good name for the variable.
 
**Rules For Loops**

1. Use a while-loop only to loop forever, and that means probably never. This only applies to Python; other languages are different.
while仅仅用于无限循环
2. Use a for-loop for all other kinds of looping, especially if there is a fixed or limited number of things to loop over.
其他情况用for

**Tips For Debugging**

1. Do not use a "debugger." A debugger is like doing a full-body scan on a sick person. You do not get any specific useful information, and you find a whole lot of information that doesn't help and is just confusing.
2. The best way to debug a program is to use print to print out the values of variables at points in the program to see where they go wrong.
3. Make sure parts of your programs work as you work on them. Do not write massive files of code before you try to run them. Code a little, run a little, fix a little.

###37.符号复习


**Keywords**

| 关键字 | 功能 |
|:----:|:-----:|
|and|逻辑与|
|del|**解除变量和特性的绑定**|
|from|**指明导入模块来源**|
|not|否|
|while|循环|
|as||
|elif|else if|
|global|**标记一个变量为全局变量**|
|or|或|
|with|**使用上下文管理器对代码块进行包装**|
|assert|**断言**|
|else||
|if||
|pass|空语句|
|yield|**暂时中止生成器的执行并且生成一个值**|
|break|跳出|
|except||
|import|导入|
|print|打印|
|class|类|
|exec|**执行包含Python语句的字符串**|
|in|**成员资格测试**|
|raise|**引发异常**|
|continue|**继续**|
|finally||
|is|**一致性测试**|
|return|返回|
|def|定义函数|
|for|for循环|
|lambda|lambda表达式|
|try|**封闭一段可能发生异常的代码**|

**Data Types** ：有二十几种 

- True
- False
- None
- strings
- numbers：
- floats
- lists


**String Formats**

- %d
- %i
- %o
- %u
- %x
- %X
- %e
- %E
- %f
- %F
- %g
- %G
- %c
- %r
- %s
- %%

**Operators**

- `**` ：**乘方**
- `{ }`
- `@` 
- `,`
- `:`
- `.`
- `;`

**Reading Code**

**如何阅读代码**

1. Functions and what they do.
2. Where each variable is first given a value.
3. Any variables with the same names in different parts of the program. These may be trouble later.
4. Any if-statements without else clauses. Are they right?
5. Any while-loops that might not end.
6. Finally, any parts of code that you can't understand for whatever reason.

###38.操作列表

{% highlight python %}
"""
.join()
.append()
.pop()
"""
ten_things = "Apples Oranges Crows Telephone Light Sugar"

print "Wait there's not 10 things in that list, let's fix that."

stuff = ten_things.split(' ')
more_stuff = ["Day", "Night", "Song", "Frisbee", "Corn", "Banana", "Girl", "Boy"]

while len(stuff) != 10:
    next_one = more_stuff.pop()
    print "Adding: ", next_one
    stuff.append(next_one)
    print "There's %d items now." % len(stuff)

print "There we go: ", stuff

print "Let's do some things with stuff."

print stuff[1]
print stuff[-1] # whoa! fancy
print stuff.pop()
print ' '.join(stuff) # what? cool!
print '#'.join(stuff[3:5]) # super stellar!
{% endhighlight %}

**复制列表**

{% highlight python %}
dist=src[:]
{% endhighlight %}

**When To Use Lists**

- If you need to maintain order.
- If you need to access the contents randomly by a number.
- If you need to go through the contents linearly (first to last).

###39.字典，可爱的字典

- 获取字典值
 
{% highlight python %}
stuff['city'] = "San Francisco"
{% endhighlight %}
- 添加字典元素
 
{% highlight python %}
cities['NY'] = 'New York'
{% endhighlight %}
- 删除字典元素

{% highlight python %}
del stuff['city']
{% endhighlight %}

- **两种获取可能不存在元素的方法**  

{% highlight python %}
# safely get a abbreviation by state that might not be there
state = states.get('Texas')

if not state:
    print "Sorry, no Texas."

# get a city with a default value
city = cities.get('TX', 'Does Not Exist')
print "The city for the state 'TX' is: %s" % city
{% endhighlight %}


- **Making Your Own Dictionary Module**
**创建自己的字典模组**

**没完成**

###40.模组，类和对象

1.模块就像字典
2.类就像模块
3.对象就像迷你import

{% highlight python %}
class MyStuff(object):
    def __init__(self):#类似构造函数
        self.tangerine = "And now a thousand years between"
    def apple(self):
        print "I AM CLASSY APPLES!"
{% endhighlight %}

**一个简单的类**

{% highlight python %}
class Song(object):

    def __init__(self, lyrics):
        self.lyrics = lyrics

    def sing_me_a_song(self):
        for line in self.lyrics:
            print line
# you're passing a list of strings as the lyrics.
happy_bday = Song(["Happy birthday to you",
                   "I don't want to get sued",
                   "So I'll stop right there"])

bulls_on_parade = Song(["They rally around the family",
                        "With pockets full of shells"])

happy_bday.sing_me_a_song()
bulls_on_parade.sing_me_a_song()
{% endhighlight %}


###41.面向对象

**一些名词**  

- **class :**  Tell Python to make a new kind of thing.  
- **object :** Two meanings: the most basic kind of thing, and any instance of some thing.  
- **instance :** What you get when you tell Python to create a class.  
- **def :** How you define a function inside a class.  
- **self :** Inside the functions in a class, self is a variable for the instance/object being accessed.  
- **inheritance :** The concept that one class can inherit traits from another class, much like you and your parents.  
- **composition :** The concept that a class can be composed of other classes as parts, much like how a car has wheels.  
- **attribute :** A property classes have that are from composition and are usually variables.  
- **is-a :** A phrase to say that something inherits from another, as in a "salmon" is-a "fish."  
- **has-a :** A phrase to say that something is composed of other things or has a trait, as in "a salmon has-a mouth."  

###42.对象和类

###43.基本面向对象分析与设计思想

- **Write or draw about the problem.**
- **Extract key concepts from #1 and research them.**
- **Create a class hierarchy and object map for the concepts.**
- **Code the classes and a test to run them.**
- **Repeat and refine.**.

###44.继承和聚合

####继承

**1.成员函数继承**

*you define a function in the parent, but not in the child.*

{% highlight python %}~
class Parent(object):
    def implicit(self):
        print "PARENT implicit()"
class Child(Parent):
    pass
dad = Parent()
son = Child()
dad.implicit()
son.implicit()#子类同样可以使用implicit方法
{% endhighlight %}~

**2.成员函数重载**

{% highlight python %}
class Parent(object):
    def override(self):
        print "PARENT override()"
class Child(Parent):
    def override(self):
        print "CHILD override()"

dad = Parent()
son = Child()
dad.override()
son.override()
{% endhighlight %}

**3.事先/事后改变重载**

{% highlight python %}
class Parent(object):
    def altered(self):
        print "PARENT altered()"
class Child(Parent):
    def altered(self):
        print "CHILD, BEFORE PARENT altered()"
        super(Child, self).altered()
        print "CHILD, AFTER PARENT altered()"
dad = Parent()
son = Child()
dad.altered()
son.altered()
{% endhighlight %}

**4.`super()`的意义**

**5.在`__init__`中使用`super()`**

这是最常见的用法

{% highlight python %}
class Child(Parent):
    def __init__(self, stuff):
        self.stuff = stuff
        super(Child, self).__init__()
{% endhighlight %}

####聚合

{% highlight python %}
class Other(object):
    def override(self):
        print "OTHER override()"
    def implicit(self):
        print "OTHER implicit()"
    def altered(self):
        print "OTHER altered()"
class Child(object):
    def __init__(self):
        self.other = Other()
    def implicit(self):
        self.other.implicit()
        #调用另一个类的函数来实现自己
    def override(self):
        print "CHILD override()"
    def altered(self):
        print "CHILD, BEFORE OTHER altered()"
        self.other.altered()
        print "CHILD, AFTER OTHER altered()"
son = Child()
son.implicit()
son.override()
son.altered()
{% endhighlight %}

####何时使用聚合，何时使用继承
>You don't want to have duplicated code all over your software, since that's not clean and efficient

- Inheritance solves this problem by creating a mechanism for you to have implied features in base classes.

- Composition solves this by giving you modules and the ability to simply call functions in other classes.


###45.编写一个游戏

###46.项目框架

**1.安装python包**

>I am warning you; this will be frustrating. In the business we call this "yak shaving." Yak shaving is any activity that is mind numblingly, irritatingly boring and tedious that you have to do before you can do something else that's more fun. You want to create cool Python projects, but you can't do that until you set up a skeleton directory, but you can't set up a skeleton directory until you install some packages, but you can't install packages until you install package installers, and you can't install package installers until you figure out how your system installs software in general, and so on.

**2.建立框架**

**3.使用框架**

**Using the Skeleton**

- Make a copy of your skeleton directory. Name it after your new project.
- Rename (move) the NAME module to be the name of your project or whatever you want to call your root module.
- Edit your setup.py to have all the information for your project.
- Rename tests/NAME_tests.py to also have your module name.
- Double check it's all working by using nosetests again.
- Start coding

1. Copy skeleton to "your_project".
2. Rename everything with NAME to your_project.
3. Change the word NAME in all the files to your_project.
4. Finally, remove all the *.pyc files to make sure you're clean.


>$ ls -R  
.:  
bin  docs  ex48  setup.py  tests  
./bin:  
./docs:  
./ex48:  
__init__.py  __init__.pyc  lexicon.py  lexicon.pyc  parser.py  parser.pyc  
./tests:  
__init__.py   lexicon_test.py   parser_test.py  
__init__.pyc  lexicon_test.pyc  parser_test.pyc  


###47.自动化测试

**测试指导**

1. Test files go in `tests/` and are named `BLAH_tests.py` otherwise nosetests won't run them. This also keeps your tests from clashing with your other code.
2. Write one test file for each module you make.
3. Keep your test cases (functions) short, but do not worry if they are a bit messy. Test cases are usually kind of messy.
4. Even though test cases are messy, try to keep them clean and remove any repetitive code you can. Create helper functions that get rid of duplicate code. You will thank me later when you make a change and then have to change your tests. Duplicated code will make changing your tests more difficult.
5. Finally, do not get too attached to your tests. Sometimes, the best way to redesign something is to just delete it and start over.

**自动化测试前要检查一下，是已经给定输入(正常情况)，还是需要用户输入，一般需要注释掉原来的输入语句，或者可以这样说，自动化测试一般只测试类或者函数这些封装后的代码。**

{% highlight python %}

{% endhighlight %}
class ParserError(Exception):
    pass
    
def parse_verb(word_list):
    skip(word_list, 'stop')

    if peek(word_list) == 'verb':
        return match(word_list, 'verb')
    else:
        raise ParserError("Expected a verb next.")
{% highlight python %}

"""
这是lexicon_test.py中的一个函数，nose会依次调用，
每个函数负责一个测试，但是一个测试可以包含不同的项目
"""
def test_directions():
    assert_equal(lexicon.scan("north"), [('direction', 'north')])
    #项目1：assert_equal(para1,para2)
    #当para1=para2时，测试通过
    result = lexicon.scan("north south east")
    assert_equal(result, [('direction', 'north'),
                          ('direction', 'south'),
                          ('direction', 'east')])
{% endhighlight %}

###48.高级用户输入

**1. 分割句子**

{% highlight python %}
stuff = raw_input('> ')
words = stuff.split()
#split()可以带参数，不带参数默认是以任意空白符分割
{% endhighlight %}

**2.元组**

- 类似于一个你不能修改的列表
- 元组由不同的元素组成，每个元素可以存储不同类型的数据，例如，字符串、数字和元组
- 元组通常代表一行数据，而元组中的元素则代表不同的数据项 
- 创建元组，不定长，但一旦创建后则不能修改长度
- 元组是由`，`创建的，只是放在`()`里面而已，如果只有一个元素，逗号也不能省

{% highlight python %}
first_word = ('direction', 'north')
second_word = ('verb', 'go')
sentence = [first_word, second_word]
{% endhighlight %}

**3.异常**

**`Except` 和 `raise`**
An exception is an error that you get from some function you may have run. 

{% highlight python %}
def convert_number(s):
    try:
        return int(s)
    except ValueError:
        return None
{% endhighlight %}

**try-except和if-else的选择**

`if-else`是用一个返回值进行判断，`try-except`则是根据异常来进行判断

{% highlight python %}
def scan(user_sentence):
    sentence=[]
    user_words=user_sentence.split()
    words_copy=user_words
    for word in words_copy:
        if word in lexicon["direction"] :
            sentence.append(('direction',word))
        elif  word in lexicon["verb"]:
            sentence.append(('verb',word))
        elif word in lexicon["stop"]:
            sentence.append(('stop',word))
        elif word in lexicon["noun"]:
            sentence.append(('noun',word))
        else:
            try:
                int(word) #如果不是数字的话，int()会引发异常
                #这里如果用if来判断的话，是没办法进行的，因为int失败时并不会返回异常，用if无法检测
                sentence.append(('number',int(word)))
            except:
            #如果发生异常，说明word不是数字，也不是前面那些分类，就是错误的词语了
                sentence.append(('error',word))
        #else :
    return sentence
{% endhighlight %}

**4.如果保证自己写的模组能够正确导入**  
**Why do I keep getting ImportErrors?**

Import errors are caused by usually four things.

1. You didn't make a __init__.py in a directory that has modules in it.
2. you are in the wrong directory. 
3. You are importing the wrong module because you spelled it wrong. 
4. Your PYTHONPATH isn't set to . so you can't load modules from your current directory.

###49.造句

**查表和配对**

1. A way to loop through the list of tuples. That's easy.
2. A way to "match" different types of tuples that we expect in our Subject Verb Object setup.
3. A way to "peek" at a potential tuple so we can make some decisions.
4. A way to "skip" things we do not care about, like stop words.

>The Sentence Grammar
With our tools we can now begin to build Sentence objects from our list of tuples. What we do is a process of:
Identify the next word with peek.
If that word fits in our grammar, we call a function to handle that part of the grammar, say parse_subject.
If it doesn't, we raise an error, which you will learn about in this lesson.
When we're all done, we should have a Sentence object to work with in our game.


**使用`class`来创建一个异常并且`raise`**

{% highlight python %}
class ParserError(Exception):
    pass
    
def parse_verb(word_list):
    skip(word_list, 'stop')

    if peek(word_list) == 'verb':
        return match(word_list, 'verb')
    else:
        raise ParserError("Expected a verb next.")
{% endhighlight %}

**使用`assert_raises`来测试一个异常**
{% highlight python %}
assert_raises(exception, callable, parameters)
#使用callable调用parameters，必须引发exception才能通过测
{% endhighlight %}


