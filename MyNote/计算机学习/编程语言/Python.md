## 字符串的操作
---
``` python
 upper() # 字符串转大写
 strip() # 去掉字符串左右的空白
 replace() # 字符串替换
 split() # 对字符串进行切割
 join() # 拼接一个列表中的内容成为新字符串
 startswith() # 判断字符串开头
 len() # 获取字符串长度

# 字符串切片
s[start:end:step]
```

## 列表
---


## 函数
---
**全局变量&局部变量**
- 在函数中如果想修改一个全局变量的值，需要在函数中使用`global`关键字声明这个变量。
- 针对不可变类型，如果想在函数中修改，则需要使用`global`关键字声明，而对于可变数据类型则不需要
**lambda表达式**
```python
a = [1,2,3,4,5]

#常规函数
def fun(x,y):
    retuen x + y
    
#匿名函数
a = lambda x,y:x + y
a(1,2)


#map(fun , a)函数：对数据结构a中的每一个数据使用传入的fun函数进行处理
result = map(lambda x:x**3 , a)
print(list(result))#map函数返回的是一个对象引用。需要强转

#reduce(fun , a)函数：对数据结构a中的每个数据使用传入的fun函数进行累计处理 
from functools import reduce
result = reduce(lambda x,y:x*y,a)
print(result)#120

#filter(fun , a)函数：针对数据结构a使用fun函数对每个数据进行过滤操作
result = filter(lambda x:x%2 == 0 , a)
print(list(result))
```

**内置函数**
```pyhton
abs()：对传入参数取绝对值
bool()：对传入参数取布尔值
all()：所有传入参数为真，才为真
any()：任何一个传入参数为真，才为真
ascii()；自动执行传入参数的_repr_方法（将对象转化为字符串）
bin()：接收一个十进制转换为二进制
oct()：接收一个十进制转换为八进制
hex()：接收一个十进制转换为十六进制
bytes()：字符串转换成字节。第一个传入参数是要转换的字符串，第二个参数是按什么编码转换为字节
str()：字节转换为字符串，第一个参数是要转换的字节，第二个参数是按什么编码转换为字符串
chr()：数字转字母
ord()：字母转数字
compile()：接收.py文件或字符串作为传入参数，将其编译成python字节码
eval()：执行python代码，并返回其执行结果
exec()：执行python代码{可以是编译过的，也可以是未编译的}，没有返回结果
dir()：接收对象作为参数，返回该对象的所有属性和方法
```
![[Python内置方法.png]]
![[Python内置函数-2.png]]
![[Python内置函数-3.png]]

## 多线程
---
``` python
import os

```

