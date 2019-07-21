# 第一章 数据类型

## 1 不可变数据类型

### 1.1 文本序列类型 --- string
* 单引号: '允许包含有 `"双"` 引号'
* 双引号: "允许包含有 `'单'` 引号"
* 三重引号: `'''三重单引号'''`、 `"""三重双引号"""`

使用三重引号的字符串可以跨越多行 —— 其中所有的空白字符都将包含在该字符串字面值中

实例
```

```
#### 1.1.1 字符串的方法




### 1.2 number（数字）
1. Python Number 数据类型用于存储数值。
2. 数据类型是不允许改变的,这就意味着如果改变 Number 数据类型的值，将重新分配内存空间。

#### 1.2.1 number支持四种不同数值类型
1. `int`整型 - 通常被称为是整型或整数，是正或负整数，不带小数点。
2. `long`长整型 - 无限大小的整数，整数最后是一个大写或小写的L。
3. `float`浮点型 - 浮点型由整数部分与小数部分组成，浮点型也可以使用科学计数法表示（2.5e2 = 2.5 x 102 = 250）
4. `complex`复数 - 复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型。

* 长整型也可以使用小写"L"，但是还是建议您使用大写"L"，避免与数字"1"混淆。Python使用"L"来显示长整型。
* Python还支持复数，复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型

#### 1.2.2 Number 类型转换
```

```

#### 1.2.3 math模块、cmath模块
1. Python 中数学运算常用的函数基本都在 math 模块、cmath 模块中。
2. Python math 模块提供了许多对浮点数的数学运算函数。
3. Python cmath 模块包含了一些用于复数运算的函数。
4. cmath 模块的函数跟 math 模块函数基本一致，区别是 cmath 模块运算的是复数，math 模块运算的是数学运算。

查看`math`包含的内容
```
import math

print(dir(math))

# 打印内容
['__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'acos', 'acosh', 'asin', 'asinh', 'atan', 'atan2', 'atanh', 'ceil', 'copysign', 'cos', 'cosh', 'degrees', 'e', 'erf', 'erfc', 'exp', 'expm1', 'fabs', 'factorial', 'floor', 'fmod', 'frexp', 'fsum', 'gamma', 'gcd', 'hypot', 'inf', 'isclose', 'isfinite', 'isinf', 'isnan', 'ldexp', 'lgamma', 'log', 'log10', 'log1p', 'log2', 'modf', 'nan', 'pi', 'pow', 'radians', 'remainder', 'sin', 'sinh', 'sqrt', 'tan', 'tanh', 'tau', 'trunc']
```

查看`cmath`包含的内容
```
import cmath

print(dir(cmath))

# 打印内容
['__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__', 'acos', 'acosh', 'asin', 'asinh', 'atan', 'atanh', 'cos', 'cosh', 'e', 'exp', 'inf', 'infj', 'isclose', 'isfinite', 'isinf', 'isnan', 'log', 'log10', 'nan', 'nanj', 'phase', 'pi', 'polar', 'rect', 'sin', 'sinh', 'sqrt', 'tan', 'tanh', 'tau']
```

实例
```
import cmath

cc =cmath.sqrt(-1)
print(cc)

cc1 = cmath.sqrt(9)
print(cc1)

cc2 = cmath.sin(1)
print(cc2)

cc3 = cmath.log10(100)
print(cc3)
```

#### 1.2.4 数学函数
1. `abs(x)` 返回数字的绝对值，如abs(-10) 返回 10
2. `ceil(x)`返回数字的上入整数，如math.ceil(4.1) 返回 5
3. `cmp(x,y)`如果 x < y 返回 -1, 如果 x == y 返回 0, 如果 x > y 返回 1
4. `exp(x)`返回e的x次幂(ex),如math.exp(1) 返回2.718281828459045
5. `fabs(x)`返回数字的绝对值，如math.fabs(-10) 返回10.0
6. `floor(x)`返回数字的下舍整数，如math.floor(4.9)返回 4
7. `log(x)`如math.log(math.e)返回1.0,math.log(100,10)返回2.0
8. `log10(x)`返回以10为基数的x的对数，如math.log10(100)返回 2.0
9. `max(x1,x2,...)`返回给定参数的最大值，参数可以为序列。
10. `min(x1,x2,...)`返回给定参数的最小值，参数可以为序列。
11. `modf(x)`返回x的整数部分与小数部分，两部分的数值符号与x相同，整数部分以浮点型表示。
12. `pow(x,y)`x**y 运算后的值。
13. `round(x[,n])`返回浮点数x的四舍五入值，如给出n值，则代表舍入到小数点后的位数。
14. `sqrt(x)`返回数字x的平方根

#### 1.2.5 随机数函数
随机数可以用于数学，游戏，安全等领域中，还经常被嵌入到算法中，用以提高算法效率，并提高程序的安全性。
1. `choice(seq`从序列的元素中随机挑选一个元素，比如random.choice(range(10))，从0到9中随机挑选一个整数。
2. `randrange([start,]stop[,step])`从指定范围内，按指定基数递增的集合中获取一个随机数，基数缺省值为1
3. `random()`随机生成下一个实数，它在[0,1)范围内。
4. `seed([x])`改变随机数生成器的种子seed。如果你不了解其原理，你不必特别去设定seed，Python会帮你选择seed。
5. `shuffle(lst`将序列的所有元素随机排序
6. `uniform(x,y)`随机生成下一个实数，它在[x,y]范围内。

#### 1.2.6 三角函数
1. `acos(x)`返回x的反余弦弧度值。
2. `asin(x)`返回x的反正弦弧度值。
3. `atan(x`返回x的反正切弧度值。
4. `atan2(y,x)`返回给定的 X 及 Y 坐标值的反正切值。
5. `cos(x)`返回x的弧度的余弦值。
6. `hypot(x,y)`返回欧几里德范数 sqrt(x*x + y*y)。
7. `sin(x)`返回的x弧度的正弦值。
8. `tan(x)`返回x弧度的正切值。
9. `degrees(x)`将弧度转换为角度,如degrees(math.pi/2) ， 返回90.0
10. `radians(x)`将角度转换为弧度

#### 1.2.7 数学常量
1. `pi`数学常量 pi（圆周率，一般以π来表示）
2. `e`数学常量 e，e即自然常数（自然常数）。

#### 1.2.8 Python运算符？
* Python里面怎么做简单的加减乘除
* 怎样方便的把计算结果保存下来，方便下次计算？
* Python中小数是怎么计算的呢？小数和整数的混合运算计算结果是怎样的呢？
* Python中总共有多少数值类型？

#### 1.2.9 Python的运算
* 运算符:`+、-、*、/、//、%、**`
* python做简单的加减乘除在解释器里面直输入即可

```
>>> 1+1   # 加 1+1输出2
2
>>> 5-2   # 减 5-2输出3 6-8输出-2
3
>>> 2*3   # 乘 2*3输出6
6
>>> 10/2  # 除 10/2输出5
5.0
>>> 10//3 # 取整 10//3输出3或10.0//3.0输出3.0
3
>>> 6 % 4 # 取余 10/2输出5
2
>>> 2**3  # 幂 2**3输出8
8
```

### 1.3 tupe
1. 一个元组由几个被逗号隔开的值或小括号包裹元素被逗号隔开组成。
2. 列表和字符串有很多共同特性，例如索引和切片操作。元组是序列数据类型

实例1: 创建一个空元组
```
tup2 = ()
```

实例2:元组中只包含一个元素时，需要在元素后面添加逗号
```
tup3 = (50,)
```

实例3:创建一个元组
```
tup1 =("physics","chemistry",1997,2000)
```

实例4 
```
tup = 12345,54321,"hello!"
```

#### 1.3.1 访问元组
元组可以使用下标索引来访问元组中的值

实例
```
tup1 =("physics","chemistry",1997,2000)
print(tup1[0])
```

#### 1.3.2 修改元组
元组中的元素值是不允许修改的，但我们可以对元组进行连接组合

实例
```
tup1 = (12,34,56)
tup2 = ("abc","xyz")

tup3 = tup1 + tup2    # 创建一个新的元组
print(tup3)
```

#### 1.3.3 删除元组
元组中的元素值是不允许删除的，但我们可以使用`del`语句来删除整个元组

实例
```
tup1 =("physics","chemistry",1997,2000)
print(tup1)

del tup1
print("after deleting tup1:")
print(tup1)

# 以上实例元组被删除后，输出变量会有异常信息，输出如下所示
('physics', 'chemistry', 1997, 2000)
after deleting tup1:
Traceback (most recent call last):
  File "/Users/leva/Desktop/Project/temp/while.py", line 6, in <module>
    print(tup1)
NameError: name 'tup1' is not defined
```

#### 1.3.4 元组运算符
与字符串一样，元组之间可以使用 + 号和 * 号进行运算。这就意味着他们可以组合和复制，运算后会生成一个新的元组。
```
lis = 1,2,3
print(len(lis))      # 打印计算元素个数

lis1 = 4,5,6

print(lis+lis1)      # 把两个元组拼接成一个新的元组


lis2 = ('Hi!',)
print(lis2*4)        # 复制获得一个新的元组

lis3 = 1,2,3
print(3 in lis3)     # 判断一个

for i in lis3:       # 循环体（迭代）
    print(i)         # 打印lis3 所有元素
```

#### 1.3.5 元组索引，截取
因为元组也是一个序列，所以我们可以访问元组中的指定位置的元素，也可以截取索引中的一段元素
```
lists = ('spam','Spam','SPAM')
正向索引值 [ 0 ]  [ 1 ]  [ 2 ]  正向取索引值从0开始
反向索引值 [ -3]  [ -2]  [ -1]  反向取索引值从-1开始
  
print(lists[2])   # 打印第三个元素
print(lists[-2])  # 反向打印倒数第二个元素
print(lists[1:])  # 打印截取元素
```

#### 1.3.6 无关闭分隔符
任意无符号的对象，以逗号隔开，默认为元组

实例
```
print("abc",-4.24e93,18+6.6j,"xyz")
x,y = 1,2
print("Value of x,y :",x,y)

# 运算结果
abc -4.24e+93 (18+6.6j) xyz
Value of x,y : 1 2
```

#### 1.3.7 元组内置函数
Python元组包含了以下内置函数
1. `cmp(tuple1,tuple2)`比较两个元组元素
2. `len(tuple)`计算元组元素个数
3. `max(tuple)`返回元组中元素最大值
4. `min(tuple)`返回元组中元组最小值
5. `tuple(seq)`将列表转换为元组


## 2 可变数据类型

### 2.1 list(列表)
### 2.2 set(集合)
### 2.3 dict(字典)


## 3 序列类型

### 3.1 序列类型 --- list、tuple、range
有三种基本序列类型：list, tuple 和 range 对象。 为处理 二进制数据 和 文本字符串 而特别定制的附加序列类型会在专门的小节中描述。

#### 3.1.1 二进制序列类型 --- bytes、bytearray、 memoryview
##### 3.1.1.1 bytes
##### 3.1.1.2 bytearray
##### 3.1.1.3 memoryview

#### 3.2 通用序列操作
