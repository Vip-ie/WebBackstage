# 第四章 循环语句
1. 程序在一般情况下是按顺序执行的。
2. 编程语言提供了各种控制结构，允许更复杂的执行路径。
3. 循环语句允许我们执行一个语句或语句组多次，下面是在大多数编程语言中的循环语句的一般形式。

## 1 while循环
在给定的判断条件为 true 时执行循环体，否则退出循环体。

while 语句用于循环执行程序，即在某条件下，循环执行某段程序，以处理需要重复处理相同任务
```
while 判断条件：
    执行语句……
```
执行语句可以是单个语句或语句块。判断条件可以是任何表达式，任何非零、或非空（null）值均为true。

当判断条件假false时，循环结束。

实例
```
count = 0
while (count < 9):
   print 'The count is:', count
   count = count + 1
 
print "Good bye!"
```

### 1 continue(跳过该次循环)
1. `continue`语句跳出本次循环，而`break`跳出整个循环。
2. `continue`语句用来告诉Python跳过当前循环的剩余语句，然后继续进行下一轮循环。
3. `continue`语句用在`while`和`for`循环中。

实例1
```
for  letter in "python":     # 循环取出一个单词的每个字母
    if letter == "h":        # 判断条件成立
        continue             # 判断条件成立跳过此次循环
    print("当前字母:",letter)
```

实例2
```
var = 10                    # 定义一个变量
while var > 0:              # 循环体
    var = var -1            # 变量减一
    if var == 5:            # 判断条件是否成立
        continue            # 条件成立跳过此次循环
    print("当前变量值:",var)  # 打印每个字母
print("Good bye!")
```

实例3
```
i = 1             # 定义一个变量并赋值为1
while i < 10:     # 循环条件是否成立
    i += 1       
    if i%2 > 0:   # 非双数时跳过输出
        continue
    print(i)      # 输出双数 2、4、6、8、10
```

### 2 break(退出循环) 
1. break语句用来终止循环语句，即循环条件没有False条件或者序列还没被完全递归完，也会停止执行循环语句
2. break语句用在while和for循环中
3. 如果您使用嵌套循环，break语句将停止执行最深层的循环，并开始执行下一行代码

实例1
```
for letter in "python":      # 循环取出当前单词每个字母
    if letter == "h":        # 判断条件条件是否成立
        break                # 条件成立跳出循环，条件不成立继续执行
    print("点前字母:",letter)  # 打印成立字母 
```

实例2
```
var = 10
while var > 0:
    print("当前变量值:",var)   # 打印变量
    var = var -1             # 变量减一
    if var  == 5:            # 判断变量是否相当
        break                # 变量相等停止循环
print("Good bye!")
```

实例3
```
i  = 1
while 1:        # 循环条件为1必定成立
    print(i)    # 输出1~10
    i += 1
    if i > 10:  # 当i大于10时跳出循环
        break
```

### 3 无限循环
如果条件判断语句永远为 true，循环将会无限的执行。
```
var = 1
while var == 1 :                    # 条件成立执行
    num = 20

    print("You entered :", num)

print("Good bye!")
```

### 4 循环使用else语句
`while … else`在循环条件为`false`时执行`else`语句块

实例
```
count = 0
while count < 5 :                       # 条件成立执行
    print('is less than :',count)
    count = count + 1
else:
    print ('is not less than :', count) # 条件满足执行
```

### 5 简单语句组
类似`if`语句的语法，如果你的`while` 循环体中只有一条语句，你可以将该语句与`while`写在同一行中
```
flag = 1
while (flag): print('Given flag is really true!')  # 无限循环
print('Good bye!')
```

### 6 pass语句
1. pass 是空语句，是为了保持程序结构的完整性。
2. pass 不做任何事情，一般用做占位语句。
实例
```
for letter in "python":      # 循环体
    if letter == 'h':        # 判断条件是否成立
        pass                 # 跳过
        print("这是pass块")   # 打印出指定内容
    print("当前字母:",letter)  # 打印当前字母
print("Good bye!")

```

## 2 for 循环
重复执行语句

for循环的语法
```
for i in python:   # 循环体
    print(i)       # 打印条件
```

实例
```
for i in "python":                     # 循环取出一个英文单词单个字母
    print('当前字母:',i)
```

### 2.1 通过序列索引迭代
```
fruits = ["banana","apple","mango"]    # 定义个列表
for i in fruits:                       # 循环取出一个列表值
    print("当前水果:",i)
print("Good bye!")
```

### 2.2 循环使用else语句
`for … else`表示这样的意思，`for`中的语句和普通的没有区别，`else`中的语句会在循环正常执行完（即 for 不是通过 break 跳出而中断的）的情况下执行，while … else 也是一样。

实例
```
for num in range(10,20):                         # 迭代10到20之间的数字
    for i in range(2,num):                       # 根据因子迭代
        if num%i == 0:                           # 确定第一个因子
            j = num/i                            # 计算第二个因子
            print("%d 等于 %d * %d" % (num,i,j))
            break                                # 跳出当前循环
        else:                                    # 循环的else部分
            print("是一个质数:",num)               
```

## 3 嵌套循环
你可以在while循环体中嵌套for循环

Python 语言允许在一个循环体里面嵌入另一个循环

Python for 循环嵌套语法
```
for iterating_var in sequence:
   for iterating_var in sequence:
      statements(s)
   statements(s)
```

Python while 循环嵌套语法
```
for iterating_var in sequence:
   for iterating_var in sequence:
      statements(s)
   statements(s)
```
你可以在循环体内嵌入其他的循环体，如在while循环中可以嵌入for循环， 反之，你可以在for循环中嵌入while循环。

实例
```
i = 2
while(i < 100):
    j = 2
    while(j <= (i/j)):             
        if not (i%j): break
        j = j + 1
    if (j > i/j): print("是素数",i)
    i = i + 1

print("Good bye!")
```


