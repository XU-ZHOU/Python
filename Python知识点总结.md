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

#### 5.列表生成式

列表生成式即List Comprehensions，是Python内置的非常简单却强大的可以用来**创建list的生成式**。

示例：生成list `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`

```python
>>> list(range(1, 11))
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

#### 6.迭代器和生成器

#### 迭代器

迭代器是一个可以记住遍历的位置的对象。

迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。

迭代器有两个基本的方法：**iter()** 和 **next()**。

**字符串，列表或元组**对象都可用于创建迭代器：

示例：

```python
>>>list=[1,2,3,4]
>>> it = iter(list)    # 创建迭代器对象
>>> print (next(it))   # 输出迭代器的下一个元素
1
>>> print (next(it))
2
>>>
```

迭代器对象可以使用常规for语句进行遍历：

```python
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
for x in it:
    print (x, end=" ")
```

执行以上程序，输出结果如下：

```
1 2 3 4
```

也可以使用 next() 函数：

```python
import sys         # 引入 sys 模块
 
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
 
while True:
    try:
        print (next(it))
    except StopIteration:
        sys.exit()
```

执行以上程序，输出结果如下：

```python
1
2
3
4
```

#### 创建一个迭代器

把一个类作为一个迭代器使用需要在类中**实现两个方法 iter() 与 next()** 。

如果你已经了解的面向对象编程，就知道类都有一个构造函数，Python 的构造函数为 __init__(), 它会在对象初始化的时候执行。

__iter__() 方法返回一个特殊的迭代器对象， 这个迭代器对象实现了 __next__() 方法并通过 StopIteration 异常标识迭代的完成。

__next__() 方法（Python 2 里是 next()）会返回下一个迭代器对象。

示例：创建一个返回数字的迭代器，初始值为 1，逐步递增 1：

```python
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    x = self.a
    self.a += 1
    return x
 
myclass = MyNumbers()
myiter = iter(myclass)
 
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
```

执行输出结果为：

```python
1
2
3
4
5
```

#### StopIteration

StopIteration 异常用于标识迭代的完成，防止出现无限循环的情况，在 __next__() 方法中我们可以设置在完成指定循环次数后触发 StopIteration 异常来结束迭代。

#### 生成器

在 Python 中，使用了 **yield** 的函数被称为生成器（generator）。

在Python中，这种**一边循环一边计算**的机制，称为生成器：generator。

跟普通函数不同的是，**生成器是一个返回迭代器的函数**，只能用于迭代操作，更简单点理解生成器就是一个迭代器。

在调用生成器运行的过程中，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。

调用一个生成器函数，返回的是一个迭代器对象。

以下实例使用 yield 实现斐波那契数列：

```python
import sys
 
def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
 
while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()
```

执行以上程序，输出结果如下：

```
0 1 1 2 3 5 8 13 21 34 55
```

另一种从创建generator的方式是，只要把列表生成式的[]改成（），就创建了一个generator。

```python
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
```

创建`L`和`g`的区别仅在于最外层的`[]`和`()`，`L`是一个list，而`g`是一个generator。

我们可以直接打印出list的每一个元素，但我们怎么打印出generator的每一个元素呢？

如果要一个一个打印出来，可以通过`next()`函数获得generator的下一个返回值：

## 第 **7** 章 函数装饰器和闭包

#### 1.装饰器

函数装饰器用于在源码中“标记”函数，以某种方式增强函数的行为。若想掌握，必须理解闭包。

**装饰器是可调用对象，其参数是另一个函数（被装饰的函数）。**装饰器可能会处理被装饰的函数，然后返回，也可能将其替换成另一个函数或可调用对象。返回的是函数。

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

![](https://github.com/XU-ZHOU/Python/blob/master/pictures/1.png)

​				上图：**averager** 的闭包延伸到那个函数的作用域之外，包含自由变量 **series** 的绑定 

**综上：闭包是一种函数，它会保留定义函数时存在的自由变量的绑定，这样调用函数时，虽然定义作用域不可用了，但是仍能使用那些绑定。**

#### 4.**nonlocal**声明

前面实现make_averager函数的方法效率不高，因为把所有值存储在历史数列中，更好的实现方式是，只存储目前的总值和元素个数。下面示例计算移动平均值的高阶函数，不保存所有历史值，但是是**有问题**的。

```python
def make_averager():
	count = 0
	total = 0
	def averager(new_value):
		count += 1
		total += new_value
		return total / count
	return averager
```

运行会得到如下结果：

```python
>>> avg = make_averager()
>>> avg(10)
Traceback (most recent call last):
...
UnboundLocalError: local variable 'count' referenced before assignment
>>>
```

问题是，当 count 是数字或任何不可变类型时，count += 1 语句的作用其实与 count = count + 1 一样。因此，我们在 averager 的定义体中为 count 赋值了，这会把 count 变成局部变量。total 变量也受 这个问题影响。前面没遇到这个问题，因为我们没有给 series 赋值，我们只是调用 series.append，并把它传给 sum 和 len。也就是说，我们利用了**列表是可变的对象**这一事实。 

但是对数字、字符串、元组等不可变类型来说，只能读取，不能更新。如果尝试重新绑定，例如 count = count + 1，其实会**隐式创建局部变量** count。这样，count 就不是自由变量了，因此不会保存在闭包中。 

为了解决这个问题，Python 3 引入了 **nonlocal** 声明。它的作用是把变 量标记为自由变量，即使在函数中为变量赋予新值了，也会变成自由变量。

示例：计算移动平均值，不保存所有历史（使用 nonlocal 修正） 

```python
def make_averager():
	count = 0
	total = 0
	def averager(new_value):
		nonlocal count, total
		count += 1
		total += new_value
		return total / count
	return averager
```

#### 5.标准库中的装饰器

Python 内置了三个用于装饰方法的函数：property、classmethod 和 staticmethod。

另一个常见的装饰器是 functools.wraps，它的作用是协助构建行为良好的装饰器。我们在示例 7-17 中用过。标准库中最值得关注的两个装饰器是 **lru_cache**。

functools.lru_cache 是非常实用的装饰器，它实现了**备忘** （memoization）功能。这是一项优化技术，它把耗时的函数的结果保存起来，避免传入相同的参数时重复计算。LRU 三个字母是“Least Recently Used”的缩写，表明缓存不会无限制增长，一段时间不用的缓存条目会被扔掉。 

生成第 *n* 个斐波纳契数这种慢速递归函数适合使用 lru_cache，如示例：

​	使用缓存实现，速度更快

```python
import functools
from clockdeco import clock
@functools.lru_cache() # 
def fibonacci(n):
	if n < 2:
		return n
	return fibonacci(n-2) + fibonacci(n-1)
if __name__=='__main__':
	print(fibonacci(6))
```

## 第 **8** 章 对象引用、可变性和垃圾回收

### 1.在**==**和**is**之间选择

**==运算符比较两个对象的值（对象中保存的数据），而is比较对象的标识**。

is运算符比==速度快，因为它不能重载。

**对可变的对象来说，+= 运算符就地修改列表。**

**对元组来说，+= 运算符创建一个新元组，然后重新绑定给变量。**

### 2.元组的相对不可变性

元组与多数 Python 集合（列表、字典、集，等等）一样，**保存的是对象的引用**。 如果引用的元素是可变的，即便元组本身不可变，元素依然可变。也就是说，**元组的不可变性其实是指 tuple 数据结构的物理内容（即保存的引用）不可变，与引用的对象无关**。 

示例 ：一开始，t1 和 t2 相等，但是修改 t1 中的一个可变元素后，二者不相等了 

```python
>>> t1 = (1, 2, [30, 40]) ➊
>>> t2 = (1, 2, [30, 40]) ➋
>>> t1 == t2 ➌
True
>>> id(t1[-1]) ➍
4302515784
>>> t1[-1].append(99) ➎
>>> t1
(1, 2, [30, 40, 99])
>>> id(t1[-1]) ➏
4302515784
>>> t1 == t2 ➐
False
```

### 3.直接赋值、浅拷贝、深拷贝

- **直接赋值：**其实就是对象的引用（别名）。
- **浅拷贝(copy)：**拷贝父对象，不会拷贝对象的内部的子对象。
- **深拷贝(deepcopy)：** copy 模块的 deepcopy 方法，完全拷贝了父对象及其子对象。

##### 字典浅拷贝实例

```python
>>>a = {1: [1,2,3]}
>>> b = a.copy()
>>> a, b
({1: [1, 2, 3]}, {1: [1, 2, 3]})
>>> a[1].append(4)
>>> a, b
({1: [1, 2, 3, 4]}, {1: [1, 2, 3, 4]})
```

**深度拷贝需要引入 copy 模块：**

```python
>>>import copy
>>> c = copy.deepcopy(a)
>>> a, c
({1: [1, 2, 3, 4]}, {1: [1, 2, 3, 4]})
>>> a[1].append(5)
>>> a, c
({1: [1, 2, 3, 4, 5]}, {1: [1, 2, 3, 4]})
```

#### 解析

1、**b = a:** 赋值引用，a 和 b 都指向同一个对象。

![](https://github.com/XU-ZHOU/Python/blob/master/pictures/3.png)

**2、b = a.copy():** 浅拷贝, a 和 b 是一个独立的对象，但他们的子对象还是指向统一对象（是引用）。默认做潜复制。

![](https://github.com/XU-ZHOU/Python/blob/master/pictures/2.png)

**3.b = copy.deepcopy(a):** 深度拷贝, a 和 b 完全拷贝了父对象及其子对象，两者是完全独立的。

![](https://github.com/XU-ZHOU/Python/blob/master/pictures/4.png)

### 4.**del**和垃圾回收

del语句删除名称，而不是对象。

python采用的是**引用计数**机制为主，**标记-清除**和**分代收集**两种机制为辅的策略。

在 CPython 中，垃圾回收使用的主要算法是**引用计数**。实际上，每个对象都会统计有多少引用指向自己。当引用计数归零时，对象立即就被销毁：CPython 会在对象上调用 __del__ 方法（如果定义了），然后释放分配给对象的内存。CPython 2.0 增加了**分代垃圾回收**算法，用于检测引用循环中涉及的对象组——如果一组对象之间全是相互引用，即使再出色的引用方式也会导致组中的对象不可获取。Python 的其他实现有更复杂的垃圾回收程序，而且不依赖引用计数，这意味着，对象的引用数量为零时可能不会立即调用 __del__ 方法。

#### 引用计数

- Python语言默认采用的垃圾收集机制是『引用计数法 `Reference Counting`』。
- 『引用计数法』的原理是：每个对象维护一个`ob_ref`字段，用来记录该对象当前被引用的次数，每当新的引用指向该对象时，它的引用计数`ob_ref`加`1`，每当该对象的引用失效时计数`ob_ref`减`1`，一旦对象的引用计数为`0`，该**对象立即被回收**，对象占用的内存空间将被释放。
- 它的缺点是需要额外的空间维护引用计数，这个问题是其次的，最主要的问题是它不能解决对象的“**循环引用”**。

循环引用：

```python
list1 = []
list2 = []
list1.append(list2)
list2.append(list1)
```

list1与list2相互引用，如果不存在其他对象对它们的引用，list1与list2的引用计数也仍然为1，所占用的内存永远无法被回收，这将是致命的。
对于如今的强大硬件，缺点1尚可接受，但是**循环引用导致内存泄露**，注定python还将引入新的回收机制。(标记清除和分代收集)。

#### 分代回收

- 分代回收是一种以空间换时间的操作方式，Python将内存根据对象的存活时间划分为不同的集合，每个集合称为一个代，Python将内存分为了3“代”，分别为年轻代（第0代）、中年代（第1代）、老年代（第2代），他们对应的是3个链表，它们的垃圾收集频率与对象的存活时间的增大而减小。
- 新创建的对象都会分配在**年轻代**，年轻代链表的总数达到上限时，Python垃圾收集机制就会被触发，把那些可以被回收的对象回收掉，而那些不会回收的对象就会被移到**中年代**去，依此类推，**老年代**中的对象是存活时间最久的对象，甚至是存活于整个系统的生命周期内。
- 同时，分代回收是建立在标记清除技术基础之上。分代回收同样作为Python的辅助垃圾收集技术处理那些容器对象。

有三种情况会触发垃圾回收：

1. 调用`gc.collect()`,需要先导入`gc`模块。
2. 当`gc`模块的计数器达到阀值的时候。
3. 程序退出的时候。

#### gc模块

gc模块提供一个接口给开发者设置垃圾回收的选项。上面说到，采用引用计数的方法管理内存的一个缺陷是循环引用，而**gc模块的一个主要功能就是解决循环引用的问题**。

**常用函数**：

1. `gc.set_debug(flags)` 设置gc的debug日志，一般设置为`gc.DEBUG_LEAK`
2. `gc.collect([generation])` 
   显式进行垃圾回收，可以输入参数，`0`代表只检查第一代的对象，`1`代表检查一，二代的对象，`2`代表检查一，二，三代的对象，如果不传参数，执行一个`full collection`，也就是等于传2。返回不可达（unreachable objects）对象的数目。
3. `gc.set_threshold(threshold0[, threshold1[, threshold2])`
   设置自动执行垃圾回收的频率。
4. `gc.get_count()` 获取当前自动执行垃圾回收的计数器，返回一个长度为3的列表

#### gc模块的自动垃圾回收机制

必须要import gc模块，并且is_enable()=True才会启动自动垃圾回收。这个机制的**主要作用就是发现并处理不可达的垃圾对象**。

```
垃圾回收=垃圾检查+垃圾回收
```

在Python中，采用分代收集的方法。把对象分为三代，一开始，对象在创建的时候，放在一代中，如果在一次一代的垃圾检查中，改对象存活下来，就会被放到二代中，同理在一次二代的垃圾检查中，该对象存活下来，就会被放到三代中。

#### 自动回收阈值

gc模快有一个自动垃圾回收的阀值，即通过`gc.get_threshold`函数获取到的长度为3的元组，例如`(700,10,10)`
每一次计数器的增加，gc模块就会检查增加后的计数是否达到阀值的数目，如果是，就会执行对应的代数的垃圾检查，然后重置计数器

注意：
如果循环引用中，两个对象都定义了`__del__`方法，gc模块不会销毁这些不可达对象，因为gc模块不知道应该先调用哪个对象的`__del__`方法，所以为了安全起见，gc模块会把对象放到`gc.garbage`中，但是不会销毁对象。

#### 标记清除

标记清除（Mark—Sweep）』算法是一种基于追踪回收（tracing GC）技术实现的垃圾回收算法。它分为两个阶段：第一阶段是标记阶段，GC会把所有的『活动对象』打上标记，第二阶段是把那些没有标记的对象『非活动对象』进行回收。那么GC又是如何判断哪些是活动对象哪些是非活动对象的呢？

![](https://github.com/XU-ZHOU/Python/blob/master/pictures/5.png)

对象之间通过引用（指针）连在一起，构成一个有向图，对象构成这个有向图的节点，而引用关系构成这个有向图的边。从根对象（root object）出发，沿着有向边遍历对象，可达的（reachable）对象标记为活动对象，不可达的对象就是要被清除的非活动对象。**根对象就是全局变量、调用栈、寄存器**。 mark-sweepg 在上图中，我们把小黑圈视为全局变量，也就是把它作为root object，从小黑圈出发，对象1可直达，那么它将被标记，对象2、3可间接到达也会被标记，而4和5不可达，那么1、2、3就是活动对象，4和5是非活动对象会被GC回收。

标记清除算法作为Python的辅助垃圾收集技术主要处理的是一些容器对象，比如list、dict、tuple，instance等，因为对于字符串、数值对象是不可能造成循环引用问题。Python使用一个双向链表将这些容器对象组织起来。不过，这种简单粗暴的标记清除算法也有明显的缺点：**清除非活动的对象前它必须顺序扫描整个堆内存，哪怕只剩下小部分活动对象也要扫描所有对象**。

### 5.弱引用

正是因为有引用，对象才会在内存中存在。当对象的引用数量归零后，垃圾回收程序会把对象销毁。但是，有时需要引用对象，而不让对象存在的时间超过所需时间。这经常用在**缓存**中。 

**弱引用不会增加对象的引用数量。引用的目标对象称为所指对象（referent）。因此我们说，弱引用不会妨碍所指对象被当作垃圾回收**。 





### Python常用模块

#### 1.Python 日期和时间

Python 程序能用很多方式处理日期和时间，转换日期格式是一个常见的功能。

Python 提供了一个 time 和 calendar 模块可以用于格式化日期和时间。

时间间隔是以秒为单位的浮点小数。

每个时间戳都以自从1970年1月1日午夜（历元）经过了多长时间来表示。

Python 的 time 模块下有很多函数可以转换常见日期格式。如函数time.time()用于获取当前时间戳, 如下实例:

```python
import time;  # 引入time模块
 
ticks = time.time()
print "当前时间戳为:", ticks
```

以上实例输出结果：

```
当前时间戳为: 1459994552.51
```

时间戳单位最适于做日期运算。

#### 获取当前时间

从返回浮点数的时间戳方式向时间元组转换，只要将浮点数传递给如localtime之类的函数。

```python
import time
 
localtime = time.localtime(time.time())
print "本地时间为 :", localtime
```

以上实例输出结果：

```
本地时间为 : time.struct_time(tm_year=2016, tm_mon=4, tm_mday=7, tm_hour=10, tm_min=3, tm_sec=27, tm_wday=3, tm_yday=98, tm_isdst=0)
```

