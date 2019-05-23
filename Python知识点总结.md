**声明**：本文的是自己在Python学习中个人感觉重要和困难的点进行了整理，主要参考了书籍《流畅的Python》、菜鸟教程Python、廖雪峰老师的Python教程博客。



## 第 2 章 序列构成的数组**

#### **1.[filter、map、reduce、lambda](https://www.cnblogs.com/longdouhzt/archive/2012/05/19/2508844.html)的用法**

​	·**filter(function, sequence)**：对sequence中的item依次执行function(item)，将执行结果为True的item组成一个List/String/Tuple（取决于sequence的类型）返回：

```python
>>> def f(x): return x % 2 != 0 and x % 3 != 0 
>>> filter(f, range(2, 25)) 
[5, 7, 11, 13, 17, 19, 23]
>>> def f(x): return x != 'a' 
>>> filter(f, "abcdef") 
'bcdef'
```

​	·**map(function, sequence)** ：对sequence中的item依次执行function(item)，将执行结果组成一个List返回：

```python
>>> def cube(x): return x*x*x 
>>> map(cube, range(1, 11)) 
[1, 8, 27, 64, 125, 216, 343, 512, 729, 1000]
>>> def cube(x) : return x + x 
... 
>>> map(cube , "abcde") 
['aa', 'bb', 'cc', 'dd', 'ee']
```

​	·**reduce(function, sequence, starting_value)**：对sequence中的item顺序迭代调用function，如果有starting_value，还可以作为初始值调用，例如可以用来对List求和：

```python
>>> def add(x,y): return x + y 
>>> reduce(add, range(1, 11)) 
55 （注：1+2+3+4+5+6+7+8+9+10）
>>> reduce(add, range(1, 11), 20) 
75 （注：1+2+3+4+5+6+7+8+9+10+20）
```

#### **2.用*来处理剩下的元素，用“_”作为占位符**

在Python中，函数用**args**来获取不确定数量的参数。

```python
>>> import os
>>> _, filename = os.path.split('/home/luciano/.ssh/idrsa.pub')
>>> filename
'idrsa.pub'
```

在进行拆包的时候，我们不总是对元组里所有的数据都感兴趣，_ 占位符能帮助处理这种情况。

```python
>>> a, b, *rest = range(5)
>>> a, b, rest
(0, 1, [2, 3, 4])
>>> a, b, *rest = range(3)
>>> a, b, rest
(0, 1, [2])
>>> a, b, *rest = range(2)
>>> a, b, rest
(0, 1, [])
```

#### 3.切片

s[a：b：c]的形式对s在a和b之间以c为间隔取值。c的值还可以使负，负值意味着反向取值。

```python
>>> s = 'bicycle'
>>> s[::3]
'bye'
>>> s[::-1]
'elcycib'
>>> s[::-2]
'eccb'
```

**尤其注意，s[：：-1]可以对S反转**。

#### 4.**list.sort**方法和内置函数**sorted**

**list.sort 方法会就地排序列表**，也就是说不会把原列表复制一份。与 list.sort 相反的是内置函数 **sorted，它会新建一个列表作为返回值**。这个方法可以接受任何形式的可迭代对象作为参数，而不管 sorted 接受的是怎样的参数，它最后都会返回一个列表。

#### 5.[线程和进程](https://www.runoob.com/python/python-multithreading.html)

Python中使用线程有两种方式：**函数或者用类来包装线程对象**。

函数式：调用**thread模块**中的start_new_thread()函数来产生新线程。语法如下:

```python
thread.start_new_thread ( function, args[, kwargs] )
```

参数说明:

- function - 线程函数。
- args - 传递给线程函数的参数,他必须是个tuple类型。
- kwargs - 可选参数。

示例：

```python
import thread
import time
 
# 为线程定义一个函数
def print_time( threadName, delay):
   count = 0
   while count < 5:
      time.sleep(delay)
      count += 1
      print "%s: %s" % ( threadName, time.ctime(time.time()) )
 
# 创建两个线程
try:
   thread.start_new_thread( print_time, ("Thread-1", 2, ) )
   thread.start_new_thread( print_time, ("Thread-2", 4, ) )
except:
   print "Error: unable to start thread"
 
while 1:
   pass
```

执行以上程序输出结果如下：

```python
Thread-1: Thu Jan 22 15:42:17 2009
Thread-1: Thu Jan 22 15:42:19 2009
Thread-2: Thu Jan 22 15:42:19 2009
Thread-1: Thu Jan 22 15:42:21 2009
Thread-2: Thu Jan 22 15:42:23 2009
Thread-1: Thu Jan 22 15:42:23 2009
Thread-1: Thu Jan 22 15:42:25 2009
Thread-2: Thu Jan 22 15:42:27 2009
Thread-2: Thu Jan 22 15:42:31 2009
Thread-2: Thu Jan 22 15:42:35 2009
```

线程的结束一般依靠线程函数的自然结束；也可以在线程函数中调用thread.exit()，他抛出SystemExit exception，达到退出线程的目的。

#### 线程模块

Python通过两个标准库thread和threading提供对线程的支持。thread提供了低级别的、原始的线程以及一个简单的锁。

threading 模块提供的其他方法：

- threading.currentThread(): 返回当前的线程变量。
- threading.enumerate(): 返回一个包含正在运行的线程的list。正在运行指线程启动后、结束前，不包括启动前和终止后的线程。
- threading.activeCount(): 返回正在运行的线程数量，与len(threading.enumerate())有相同的结果。

除了使用方法外，线程模块同样提供了Thread类来处理线程，Thread类提供了以下方法:

- **run():** 用以表示线程活动的方法。

- start():

  启动线程活动。

  

- **join([time]):** 等待至线程中止。这阻塞调用线程直至线程的join() 方法被调用中止-正常退出或者抛出未处理的异常-或者是可选的超时发生。

- **isAlive():** 返回线程是否活动的。

- **getName():** 返回线程名。

- **setName():** 设置线程名。

使用Threading模块创建线程，直接从threading.Thread继承，然后**重写init方法和run**方法：

```python
import threading
import time
 
exitFlag = 0
 
class myThread (threading.Thread):   #继承父类threading.Thread
    def __init__(self, threadID, name, counter):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name
        self.counter = counter
    def run(self):                   #把要执行的代码写到run函数里面 线程在创建后会直接运行run函数 
        print "Starting " + self.name
        print_time(self.name, self.counter, 5)
        print "Exiting " + self.name
 
def print_time(threadName, delay, counter):
    while counter:
        if exitFlag:
            (threading.Thread).exit()
        time.sleep(delay)
        print "%s: %s" % (threadName, time.ctime(time.time()))
        counter -= 1
 
# 创建新线程
thread1 = myThread(1, "Thread-1", 1)
thread2 = myThread(2, "Thread-2", 2)
 
# 开启线程
thread1.start()
thread2.start()
 
print "Exiting Main Thread"
```

以上程序执行结果如下；

```python
Starting Thread-1
Starting Thread-2
Exiting Main Thread
Thread-1: Thu Mar 21 09:10:03 2013
Thread-1: Thu Mar 21 09:10:04 2013
Thread-2: Thu Mar 21 09:10:04 2013
Thread-1: Thu Mar 21 09:10:05 2013
Thread-1: Thu Mar 21 09:10:06 2013
Thread-2: Thu Mar 21 09:10:06 2013
Thread-1: Thu Mar 21 09:10:07 2013
Exiting Thread-1
Thread-2: Thu Mar 21 09:10:08 2013
Thread-2: Thu Mar 21 09:10:10 2013
Thread-2: Thu Mar 21 09:10:12 2013
Exiting Thread-2
```

##### 线程同步

如果多个线程共同对某个数据修改，则可能出现不可预料的结果，为了保证数据的正确性，需要对多个线程进行同步。

使用Thread对象的**Lock和Rlock**可以实现简单的线程同步，这两个对象都有**acquire方法和release**方法，对于那些需要每次只允许一个线程操作的数据，可以将其操作放到acquire和release方法之间。如下：

多线程的优势在于可以同时运行多个任务（至少感觉起来是这样）。但是当线程需要共享数据时，可能存在数据不同步的问题。

考虑这样一种情况：一个列表里所有元素都是0，线程"set"从后向前把所有元素改成1，而线程"print"负责从前往后读取列表并打印。

那么，可能线程"set"开始改的时候，线程"print"便来打印列表了，输出就成了一半0一半1，这就是数据的不同步。为了避免这种情况，引入了锁的概念。

锁有两种状态——锁定和未锁定。

#### 6.Python标准的数据类型

Python有五个标准的数据类型：

- Numbers（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Dictionary（字典）

## 第 **5** 章 一等函数

#### 1.高阶函数

接受函数为参数，或者把函数作为结果返回的函数是高阶函数（higher order function）。最为人熟知的高阶函数有 map、filter、reduce。

#### 2.匿名函数

lambda关键字在Python表达式内创建匿名函数。

然而，Python 简单的句法限制了 lambda 函数的定义体只能使用纯表达式。换句话说，lambda 函数的定义体中不能赋值，也不能使用 while 和 try 等 Python 语句。 

在参数列表中最适合使用匿名函数。

示例 **5-7** 使用 lambda 表达式反转拼写，然后依此给单词列表排序 

```python
>>> fruits = ['strawberry', 'fig', 'apple', 'cherry', 'raspberry', 'banana']
>>> sorted(fruits, key=lambda word: word[::-1])
['banana', 'apple', 'fig', 'raspberry', 'strawberry', 'cherry']
>>>
```

#### 3.可调用对象

调用类时会运行类的__new__方法创建一个实例，然后运行 __init__方法，初始化实例，最后把实例返回给调用方。因为 **Python没有 new 运算符**，所以调用类相当于调用函数。

如果类定义了 __call__ 方法，那么它的实例可以作为函数调用。

#### 4.pass 语句

Python pass 是空语句，是为了保持程序结构的完整性。

**pass** 不做任何事情，一般用做占位语句。

Python 语言 pass 语句语法格式如下：

```python
pass
```

## 第 **7** 章 函数装饰器和闭包

#### 1.装饰器

函数装饰器用于在源码中“标记”函数，以某种方式增强函数的行为。若想掌握，必须理解闭包。

**装饰器是可调用对象，其参数是另一个函数（被装饰的函数）。**装饰器可能会处理被装饰的函数，然后返回，也可能将其替换成另一个函数或可调用对象。

假设有个名为decorate的装饰器：

```python
@decorate
def target():
print('running target()')
```

上述代码的效果和下述写法一样：

```python
def target():
print('running target()')
target = decorate(target)
```

**装饰器有两大特性：能把被装饰的函数替换成其他函数；装饰器在加载模块时立即执行**。

#### 2.变量作用域规则

示例：一个函数，读取一个局部变量和一个全局变量

```python
>>> b = 6
>>> def f2(a):
... print(a)
... print(b)
... b = 9
...
>>> f2(3)
3
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "<stdin>", line 3, in f2
UnboundLocalError: local variable 'b' referenced before assignment
```

Python不要求声明变量，但是假定在函数定义题中给它复制的变量是**局部变量**。

如果在函数中赋值时想让解释器把b当成全局变量，要使用**global**声明：

```python
>>> b = 6
>>> def f3(a):
... global b
... print(a)
... print(b)
... b = 9
...
>>> f3(3)
3
6
>>> b
9
>>> f3(3)
3
9
>>> b = 30
>>> b
30
>>>
```

#### 3.闭包

**闭包指延伸了作用域的函数，其中包含函数定义体中引用、但是不在定义体中定义的非全局变量**。

**只有涉及嵌套函数时才有闭包问题**。

实例：计算移动平均值的类

```python
class Averager():
	def __init__(self):
		self.series = []
	def __call__(self, new_value):
		self.series.append(new_value)
		total = sum(self.series)
		return total/len(self.series)
```

Averager 的实例是可调用对象：

```python
>>> avg = Averager()
>>> avg(10)
10.0
>>> avg(11)
10.5
>>> avg(12)
11.0
```

另一种实现方式，示例 average.py：计算移动平均值的**高阶函数**

```python
def make_averager():
	series = []
	def averager(new_value):
		series.append(new_value)  //series是自由变量
		total = sum(series)
		return total/len(series)
	return averager
```

调用 make_averager 时，返回一个 averager 函数对象。每次调用averager 时，它会把参数添加到系列值中，然后计算当前平均值，如下所示。

```python
>>> avg = make_averager()
>>> avg(10)
10.0
>>> avg(11)
10.5
>>> avg(12)
11.0
```

注意，这两个示例有共通之处：调用 Averager() 或make_averager() 得到一个可调用对象 avg，它会更新历史值，然后计算当前均值。

在 averager 函数中，series 是**自由变量**（free variable）。这是一个技术术语，指未在本地作用域中绑定的变量，参见下图。

![<https://github.com/XU-ZHOU/Python/blob/master/pictures/QQ%E6%88%AA%E5%9B%BE20190523134805.png>]()



