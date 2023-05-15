# Python学习笔记

**<a style="font-size:1.25rem;color:rgba(229, 60, 50,0.75)">Indention:在python中空格是很重要的，其和yaml中空格的作用类似，其标识一个代码块的概念</a>**

```python
1: if 5 > 2:
2:   print("Five is greater than two!") #这里第一行和第二行表示为一个代码块，如果说第二行没有添加空格将会编译异常。空格一般是四格，但最少要存在一个空格，同一个代码块内的空格数应当是保持一致的，否则将编译异常
```

## 变量

### 注释

python中的注释采用了shell的注释方案:

```python
#This is a comment.
print("Hello, World!")
```

### 弱类型的语言

python是一个弱类型的语言，这一点和JavaScript是类似的，但是实际上它比JavaScript更弱类型，在JavaScript中我们尚且需要使用var,let或者const指定一个变量，而在python中我们甚至仅仅只需要直接创建一个变量即可，可以不加声明:

```python
x = 5
y = "Hello, World!"
```

python除了无需声明变量的操作外，甚至可以随意更改变量的类型，(**这一点JavaScript也可以实现**):

```python
x = 4       # x is of type int
x = "Sally" # x is now of type str
print(x)
```

我们注意到此时的x类型有整型变成了字符类型

#### 指定数据的类型

虽然python是具有弱类型的特征的，但是有时我们需要定义指定类型的数据，比如说我们需要一个字符类型的数字，那么我们便需要指定类型了:

```python
x = str(3)    # x will be '3'
y = int(3)    # y will be 3
z = float(3)  # z will be 3.0
```

### 变量的名称

在python中，变量名**仅可包含阿拉伯数字，英文字母和下划线**，且变量名仅支持以英文字母和下划线开头。

### 同时声明多个变量

#### 多变量对多值

```python
x, y, z = "Orange", "Banana", "Cherry"
print(x)
print(y)
print(z)
```

#### 多变量对单个值

```python
x = y = z = "Orange"
print(x)
print(y)
print(z)
```

#### 数组解构赋值(与JavaScript ES6中的解构赋值类似)

```python
fruits = ["apple", "banana", "cherry"]
x, y, z = fruits
print(x)
print(y)
print(z)
```

### 输出变量

在python中我们可以使用print方法来输出变量，并且python支持使用逗号分隔来输出多个变量:

```python
x = "Python"
y = "is"
z = "awesome"
print(x, y, z)
```

我们也可以使用+来将变量相加:

#### 字符串相加

```python
x = "Python "
y = "is "
z = "awesome"
print(x + y + z)
#结果将是Pythonisawesome
```

#### 数字相加

```python
x = 5
y = 10
print(x + y)
#结果将输出15
```

#### 数字和字符串相加

```python
x = 5
y = "John"
print(x + y)
#我们将得到一个错误
```

## 数据类型

### **python的内置数据类型**

|       类型        |          类型名称          |
| :---------------: | :------------------------: |
|     文本类型      |            str             |
|     数字类型      |     int,float,complex      |
|     数组类型      |      list,tuple,range      |
| 映射(mapping)类型 |            dict            |
|      set类型      |       set,frozenset        |
|     布尔类型      |            bool            |
|    二进制类型     | bytes,bytearray,memoryview |
|      无类型       |          NoneType          |

### python不同数据类型数据

|                  实例                  |  数据类型  |
| :------------------------------------: | :--------: |
|            x="Hello World"             |    str     |
|                  x=20                  |    int     |
|                 x=20.5                 |   float    |
|                  x=2j                  |  complex   |
|     x=["apple","banana","orange"]      |    list    |
|     x=("apple","banana","orange")      |   tuple    |
|               x=range(5)               |   range    |
|     x={"name":"rongxin","age":23}      |    dict    |
|     x={"apple","banana","cherry"}      |    set     |
| x=frozenset("apple","banana","cherry") | frozenset  |
|                 x=True                 |    bool    |
|               x=b"Hello"               |   bytes    |
|             x=bytearray(5)             | bytearray  |
|         x=memoryview(bytes(5))         | memoryview |
|                 x=None                 |  NoneType  |

### 声明时指定类型

|                   实例                   |  数据类型  |
| :--------------------------------------: | :--------: |
|           x=str("Hello World")           |    str     |
|                x=int(20)                 |    int     |
|              x=float(20.5)               |   float    |
|              x=complex(2j)               |  complex   |
|   x=list(("apple","banana","orange"))    |    list    |
|     x=(("apple","banana","orange"))      |   tuple    |
|                x=range(5)                |   range    |
|    x=dict("name":"rongxin","age":23)     |    dict    |
|    x=set(("apple","banana","cherry"))    |    set     |
| x=frozenset(("apple","banana","cherry")) | frozenset  |
|                x=bool(5)                 |    bool    |
|                x=bytes(5)                |   bytes    |
|              x=bytearray(5)              | bytearray  |
|          x=memoryview(bytes(5))          | memoryview |

###  srand((unsigned)time(NULL)); printf("%d \n",rand());c

#### python存在int,float,complex三种数字类型

##### Int

整数类型，其可以表示正数或者负数的整数，其**没有长度限制**

```python
x = 1
y = 35656222554887711
z = -3255522
# type(x) ：获取x的类型
print(type(x))
print(type(y))
print(type(z))
```

##### Float

浮点类型，其表示非整型的数字类型，其中可以使用e(E)表示10的次方

```py
x = 35e3 #=35*10的3次方
y = 12E4 
z = -87.7e100

print(type(x))
print(type(y))
print(type(z))
```

##### Complex

复杂类型，我们可以使用**数字j**的方式来声明一个复杂类型：

```python
x=2j
y=2.5j
```

##### 随机数生成

```python
import random

print(random.randrange(1,10)) #生成1~10的随机数，包含1，不包含10
print(random.randrange(10))   #生成0~10之间的随机数
```

### 文本类型

在python中，我们可以使用单引号或者双引号来声明字符变量，这一点与JavaScript是一致的:
```
print("Hello")
print('Hello')
```

#### 声明多行文本

我们可以使用三重引号来声明多行文本

```python
a = """Lorem ipsum dolor sit amet,
consectetur adipiscing elit,
sed do eiusmod tempor incididunt
ut labore et dolore magna aliqua."""
print(a)

b = '''Lorem ipsum dolor sit amet,
consectetur adipiscing elit,
sed do eiusmod tempor incididunt
ut labore et dolore magna aliqua.'''
print(b)
```

#### 字符串类型的数组特征

在python中，我们的字符串类型也是数组结构的，只是不像Java和C那样的语言，python并不存在字符类型，因此字符**串**并不是真的字符**类型**的数组，而是直接由Unicode中的特定字节所构成的数组。

不过本质上它还是一个数组，因此，我们可以将一个字符串直接作为一个数组来使用:

```python
a = "Hello, World!"
print(a[1]) #输出e
```

#### 我们也可以以数组的方式遍历字符串

```python
a = "Hello, World!"
print(a[1])
```

#### 获取字符串的长度

```python
a = "Hello, World!"
print(len(a)) #len(str)方法将获取字符串的长度
```

#### 判断字符串是否包含某个字段

当我们需要检查一个字符串中是否含有某一个字段时，我们可以使用**in**关键字

```python
txt = "The best things in life are free!"
print("free" in txt) #其返回布尔类型
```

#### 截取字符串片段

```python
b = "Hello, World!"
print(b[2:5]) #截取2到5之间的，不包含2和5
b = "Hello, World!"
print(b[:5])  #从开始截取到5
print(txt[3:])#从3截取到最后
```

####  反向截取

当截取时数字为负数，则采用反向截取:

```python
b = "Hello, World!"
print(b[-5:-2]) #结果为orl
```

#### 字符串操作

##### 大写化

当我们希望将字符串进行大写化，可以使用**upper()**方法

```python
a = "Hello, World!"
print(a.upper())
```

##### 小写化

当我们希望将字符串进行小写化，可以使用**lower()**方法

```python
a = "Hello, World!"
print(a.lower())
```

##### 去除空格

当我们希望去除前后空格时(Java的trim方法)，我们可以使用**strip()**方法

```python
a = " Hello, World! "
print(a.strip()) # returns "Hello, World!"
```

##### 字符串替代

当我们希望将字符串中的指定字符被替换为其他字符则可以使用**replace()**方法[<u>**会替代所有**</u>]

```python
a = "Hello, World!"
print(a.replace("H", "J"))
```

##### 使用分隔符转为数组

与其他语言一致，使用split方法将字符串转为数组:

```python
a = "Hello, World!"
print(a.split(",")) # returns ['Hello', ' World!']
```

##### format方法

在python中，我们无法向JavaScript那样将字符串于数字直接拼接为一个新的字符串，但是我们可以使用format方法实现类似于JavaScript那样的模板文本的效果:

```python
age = 36
txt = "My name is John, I am " + age
print(txt)
# 上面的代码将会编译失败
age = 36
txt = "My name is John, and I am {}"
print(txt.format(age))
# 上述代码将输出: My name is John, and I am 36
```

我们也可以通过参数直接指定所需参数

```python
quantity = 3
itemno = 567
price = 49.95
myorder = "I want to pay {2} dollars for {0} pieces of item {1}."
print(myorder.format(quantity, itemno, price))
# 输出：I want to pay 49.95 dollars for 3 pieces of item 567.
```

#### 转义符使用

```python
txt = "We are the so-called \"Vikings\" from the north."
print(txt)
# 输出: We are the so-called { Vikings } from the north.
```

#### 转义字符

| 代码 | 结果                                |
| ---- | ----------------------------------- |
| \'   | '                                   |
| \\\  | \                                   |
| \n   | 换行                                |
| \r   | 回车                                |
| \t   | 制表符                              |
| \b   | 空格                                |
| \000 | 获得ACCII表中指定的八进制对于字符   |
| \xhh | 获得ACCII表中指定的十六进制对于字符 |

#### 字符串方法

| Method                                                       | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [capitalize()](https://www.w3schools.com/python/ref_string_capitalize.asp) | Converts the first character to upper case                   |
| [casefold()](https://www.w3schools.com/python/ref_string_casefold.asp) | Converts string into lower case                              |
| [center()](https://www.w3schools.com/python/ref_string_center.asp) | Returns a centered string                                    |
| [count()](https://www.w3schools.com/python/ref_string_count.asp) | Returns the number of times a specified value occurs in a string |
| [encode()](https://www.w3schools.com/python/ref_string_encode.asp) | Returns an encoded version of the string                     |
| [endswith()](https://www.w3schools.com/python/ref_string_endswith.asp) | Returns true if the string ends with the specified value     |
| [expandtabs()](https://www.w3schools.com/python/ref_string_expandtabs.asp) | Sets the tab size of the string                              |
| [find()](https://www.w3schools.com/python/ref_string_find.asp) | Searches the string for a specified value and returns the position of where it was found |
| [format()](https://www.w3schools.com/python/ref_string_format.asp) | Formats specified values in a string                         |
| format_map()                                                 | Formats specified values in a string                         |
| [index()](https://www.w3schools.com/python/ref_string_index.asp) | Searches the string for a specified value and returns the position of where it was found |
| [isalnum()](https://www.w3schools.com/python/ref_string_isalnum.asp) | Returns True if all characters in the string are alphanumeric |
| [isalpha()](https://www.w3schools.com/python/ref_string_isalpha.asp) | Returns True if all characters in the string are in the alphabet |
| [isdecimal()](https://www.w3schools.com/python/ref_string_isdecimal.asp) | Returns True if all characters in the string are decimals    |
| [isdigit()](https://www.w3schools.com/python/ref_string_isdigit.asp) | Returns True if all characters in the string are digits      |
| [isidentifier()](https://www.w3schools.com/python/ref_string_isidentifier.asp) | Returns True if the string is an identifier                  |
| [islower()](https://www.w3schools.com/python/ref_string_islower.asp) | Returns True if all characters in the string are lower case  |
| [isnumeric()](https://www.w3schools.com/python/ref_string_isnumeric.asp) | Returns True if all characters in the string are numeric     |
| [isprintable()](https://www.w3schools.com/python/ref_string_isprintable.asp) | Returns True if all characters in the string are printable   |
| [isspace()](https://www.w3schools.com/python/ref_string_isspace.asp) | Returns True if all characters in the string are whitespaces |
| [istitle()](https://www.w3schools.com/python/ref_string_istitle.asp) | Returns True if the string follows the rules of a title      |
| [isupper()](https://www.w3schools.com/python/ref_string_isupper.asp) | Returns True if all characters in the string are upper case  |
| [join()](https://www.w3schools.com/python/ref_string_join.asp) | Joins the elements of an iterable to the end of the string   |
| [ljust()](https://www.w3schools.com/python/ref_string_ljust.asp) | Returns a left justified version of the string               |
| [lower()](https://www.w3schools.com/python/ref_string_lower.asp) | Converts a string into lower case                            |
| [lstrip()](https://www.w3schools.com/python/ref_string_lstrip.asp) | Returns a left trim version of the string                    |
| [maketrans()](https://www.w3schools.com/python/ref_string_maketrans.asp) | Returns a translation table to be used in translations       |
| [partition()](https://www.w3schools.com/python/ref_string_partition.asp) | Returns a tuple where the string is parted into three parts  |
| [replace()](https://www.w3schools.com/python/ref_string_replace.asp) | Returns a string where a specified value is replaced with a specified value |
| [rfind()](https://www.w3schools.com/python/ref_string_rfind.asp) | Searches the string for a specified value and returns the last position of where it was found |
| [rindex()](https://www.w3schools.com/python/ref_string_rindex.asp) | Searches the string for a specified value and returns the last position of where it was found |
| [rjust()](https://www.w3schools.com/python/ref_string_rjust.asp) | Returns a right justified version of the string              |
| [rpartition()](https://www.w3schools.com/python/ref_string_rpartition.asp) | Returns a tuple where the string is parted into three parts  |
| [rsplit()](https://www.w3schools.com/python/ref_string_rsplit.asp) | Splits the string at the specified separator, and returns a list |
| [rstrip()](https://www.w3schools.com/python/ref_string_rstrip.asp) | Returns a right trim version of the string                   |
| [split()](https://www.w3schools.com/python/ref_string_split.asp) | Splits the string at the specified separator, and returns a list |
| [splitlines()](https://www.w3schools.com/python/ref_string_splitlines.asp) | Splits the string at line breaks and returns a list          |
| [startswith()](https://www.w3schools.com/python/ref_string_startswith.asp) | Returns true if the string starts with the specified value   |
| [strip()](https://www.w3schools.com/python/ref_string_strip.asp) | Returns a trimmed version of the string                      |
| [swapcase()](https://www.w3schools.com/python/ref_string_swapcase.asp) | Swaps cases, lower case becomes upper case and vice versa    |
| [title()](https://www.w3schools.com/python/ref_string_title.asp) | Converts the first character of each word to upper case      |
| [translate()](https://www.w3schools.com/python/ref_string_translate.asp) | Returns a translated string                                  |
| [upper()](https://www.w3schools.com/python/ref_string_upper.asp) | Converts a string into upper case                            |
| [zfill()](https://www.w3schools.com/python/ref_string_zfill.asp) | Fills the string with a specified number of 0 values at the beginning |
