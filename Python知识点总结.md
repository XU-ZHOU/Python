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

在Python中，函数用*****args来获取不确定数量的参数。

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