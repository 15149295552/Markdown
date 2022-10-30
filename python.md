# print()输出函数

```python
#输出数字
print(520)
print(98.5)
#输出字符串
print('hello world')
print("hello world")
#输出含有运算符的表达式
print(3+1)
#将数据输出到文件中，如果文件不存在就创建，存在就继续追加
fp=open('D/text.txt','a+')
print('hello world'.file=fp)
fp.close()
#不进行换行输出(输出内容在一行中)
print('hello','world','Python')
```

# 转义字符

```python
print('hello\nworld') #\n表示换行
print('hello\tworld') #\t制表符
print('helloooo\tworld')
print('hello\rworld')#\r表示world将hello覆盖
print('hello\bwordl')#\b表示退一个格，将o退没了
print('http:\\\\baidu.com')
print('老师说:\'大家好\'')
#原字符,不希望字符串中的转义字符起作用，就使用原字符，就是在字符串之前加上r/R
print(r'hello\nworld')
#注意事项：最后一个字符不能是反斜线
print(r'hello\nworld\')
```

# 字符编码

```py
print(chr(0b100111001011000))
print(ord('乘'))
```

# 标识符和保留字

```py
import keyword
print(keyword.kwlist)
```

# 变量

```python
name='玛丽亚'
print(name)
print('标识',id(name))
print('类型',type(name))
print('值',name)
```

```python
name='玛丽亚'
print(name)
name='楚溜冰'
print(name)
```

# 数据类型

## int：整数类型，表示正数，负数，0

```python
n1=90
n2=-2
n3=0
print(n1)
print(n2)
print(n3)
print('十进制',118)
print('二进制',0b01110110)
print('八进制',0o166)
print('十六进制',0x76)
```

## float：浮点类型

```python
a=3.13159
print(a,type(a))
n1=1.1
n2=2.2
n3=2.1
print(n1+n2)
print(n1+n3)
from decimal import Decimal
print(Decimal('1.1')+Decimal('2.2))
```

## bool布尔类型

```python
f1=Ture
f2=False
print(f1,type(f1))
print(f2,type(f2))
#布尔值可以转成整数计算
print(f1+1)#2	1+1的结果为2  True表示1
print(f2+1)#1	0+1的结果为2  False表示0
```

## 字符串类型

```python
str1='我用python'
str2="我用python"
str3="""我用
python"""
str4='''我用
python'''
print(str1,type(str1))
print(str2,type(str2))
print(str3,type(str3))
print(str4,type(str4))
```

# 数据类型转换

```python
name='张三'
age=20
print(type(name),type(age))
print('我叫'+name+'今年'+age+'岁')#当将str类型与int类型进行连接时，报错，解决方案，类型转换
print('我叫'+name+'今年'+str(age)+'岁')#将int类型通过str()函数转成了str类型
#str()将其它类型转成str类型
a=10
b=198.8
c=False
print(type(a),type(b),type(c))
print(str(a),str(b),str(c),type(str(a)),type(str(b)),type(str(c)))
#int()将其他类型转成int类型
s1='126'
f1=98.6
s2='67.44'
ff=Ture
s3='hello'
print(type(s1),type(f1),type(s2),type(ff),type(s2))
print(int(s1),type(int(s1)))#将str转成int，字符串为数字串
print(int(f1),type(int(f1)))#将float转成int，截取整数部分，舍掉小鼠
print(int(s2),type(int(s2)))#将str转成int类型，报错，因为字符串为小数串
print(int(ff),type(int(ff)))
print(int(s3),type(int(s3)))#将str转成int类型时，字符串必须为数字串(整数)，非数字串是不允许转换
#float()将其它数据类型转成float类型
s1='126.25'
s2='67.'
ff=Ture
s3='hello'
i=98
print(type(s1),type(s2),type(ff),type(s3),type)
print(float(s1),type(float(s1)))
print(float(s2),type(float(s2)))
print(float(ff),type(float(ff)))
print(float(s3),type(float(s3)))#字符串中的数据如果是非数字串，则不允许转换
print(float(i),type(float(i)))
```

# 函数

## input()函数

```python
present=input('想要什么礼物呢')
print(present,type(present))
a=int(input('请输入一个加数'))
#a=int(a)#将转换之后的结果储存a中
b=int(input('请输入另一个加数'))
#b=int(b)
print(type(a),type(b))
print(a+b)
```

## 运算符

### 算数运算符

```python
print(1+1)#加法运算
print(1-1)#减法运算
print(2*3)#乘法运算
print(1/2)#除法运算
print(11//2)# =5 整除运算
print(11%2)# =1 取余运算
print(2**3)# =8 表示2的3次方
print(9//4)# =2
print(-9//-4)# =2
print(9//-4)# =-3
print(-9//4)# =-3 一正一负的整数公式，向下取整
print(9%-4)# =-3 余数=被除数-除数*商  9-(-4)*(-3)
print(-9%4)# =3	-9-(4)*(-3) 商为10行，11行的//
```

### 赋值运算符

```python
#顺序从右到左
i=3+4
print(i)
a=b=c=20#链式赋值
print(a,id(a))
print(b,id(b))
print(c,id(c))
#参数赋值
a=20
a+=30#a=a+30
print(a)
a-=10#a=a-=10
print(a)
a*=2#a=a*2
print(a)
a/=3
print(a)
a//=2
print(a)
a%=3
print(a)
a,b,c=20,30,40
print(a,b,c)
#a,b=20,30,40  报错，因为左右变量的个数和值的个数不对应
#交换两个变量的值
a,b=10,20
print('交换之前：',a,b)
#交换
a,b=b,a
print('交换之后：',a,b)
```

### 比较运算符

```python
a,b=10,20
print('a>b?',a>b)#False
print('a<b?',a<b)#True
print('a<=b?',a<=b)#True
print('a>=b?',a>=b)#False
print('a==b?',a==b)#False
print('a!=b?',a!=b)#True
a=10
b=10
print(a==b)#True  说明a与b的value相等
print(a is b)#True  说明a与b的id相等
lst1=[11,22,33,44]
lst2=[11,22,33,44]
print(lst1==lst2)#value -> True
print(lst1 is lst2)#id -> False
print(a is not b)#False
print(lst1 is not lst2)#True
```

### 布尔运算符

```python
a,b=1,2
#and 并且
print(a==1 and b==2)#True
print(a==1 and b<2)#False
print(a!=1 and b==2)#False
print(a!=1 and b!=2)#False
#or或者
print(a==1 or b==2)#True
print(a==1 or b<2)#True
print(a!=1 or b==2)#True
print(a!=1 or b!=2)#False
#not  对bool类型的操作数取反
f=True
f2=False
print(not f)
print(not f2)
#in与not in
s='helloworld'
print('w' in s)
print('k' in s)
print('w' not in s)
print('k' not in s)
```

### 位运算符

```python
print(4&8)#按位&，同为1时，结果为1
print(4|8)#按位或，同为0时，结果为0
print(4<<1)#向左移动1位(移动一个位置)
print(4<<2)#向左移动2位
print(4>>1)#向右移动1位
print(4>>2)#向右移动2位
```

### 运算符优先级

先算术运算，然后位运算，最后比较运算

# 对象的布尔值d

```python
print(bool(False))#False
print(bool(0))#False
print(bool(0.0))#False
print(bool(None))#False
print(bool(''))#False
print(bool(""))#False
print(bool([]))#空列表 - False
pinrt(bool(list()))#空列表 - False
print(bool(()))#空元组 - False
print(bool(tuple()))#空元组 - False
print(bool({}))#空字典 - False
print(bool(dict()))#空字典 - False
print(bool(set(g)))#空集合 - False
#除了上面的其他的都为True
print(bool(18))
print(bool(True))
print(bool('helleworld'))
```

# 结构

## 单分支结构

```python
money=1000
s=int(input('请输入取款金额'))
#判断余额是否充足
if money >= s:
    money-=s
    print('取款成功，余额为；',money)
```

## 双分支结构

```python
num=int(input('请输入一个整数'))
if num % 2 == 0:
    print(num,'是偶数')
else:
    print(num,'是奇数')
```

## 多分支结构

```python
score=input('请输入一个成绩：')
if score >= 90 and score <= 100:
    print('A')
elif score >= 80 and score <=89:
    print('B')
elif score >= 70 and score <=79:
    print('C')
elif score >= 60 and score <=69:
    print('D')
elif score >=0 and score <= 59:
    print('不及格')
else:
    print('成绩有误，请重新输入')
```

## 嵌套if

```python
answer=input('你是会员吗?y/n')
money=float(input('请输入购物金额'))
if answer=='y':
    if money>=200:
        print('打8折，付款金额为：',money*0.8)
    elif money>=100:
        print('打9折，付款金额为：',money*0.9)
    else:
        print('打9.5折，付款金额为：'，money*0.95)
else:
    if money>=150:
        print('打9.9折，付款金额为：',money*9.9)
    else:
        print('不打折，付款金额为：',money)
```

## 条件表达式

```python
num_a=int(input('请输入第一个整数'))
num_b=int(input('请输入第二个整数'))
if num_a>num_b:
    print(num_a,'大于等于',num_b)
else:
    print(num_a,'小于'，num_b)
print(str(num_a+'大于等于'+num_b) if num_a>=num_b else str(num_a+'小于'+num_b))
```

## pass语句

```python
answer=input('你是会员吗？y/n')
if answer=='y':
    pass
else:
    pass
```

```python
age=int(input('请输入年龄：'))
if age:
    print(age)
else:
    print('年龄为：',age)
```

## 内置函数

range()

```python
r=range(10)#从0开始，一直到10(不包括10)，步长默认1
print(r)
print(list(r))
r=range(1,10)#从1开始
print(list(r))
r=range(1,10,2)#步长为2
print(list(r))
print(10 in r)#False，10不在r整数列中
print(9 in r)#True，9在r整数列中
print(9 not in r)#False
print(range(1,20,1))
```

## 循环结构

```python
sum=0
a=0
while a<5:
    sum+=a
    a+=1
print(sum)
```

```python
sum=0
a=1
while a<=100:
    if not bool a%2:
        sum+=a
     a+=1
print(sum)
```

```python
for item in 'python':
    print(item)
for i in range(10):
    print(i)
for _ in range(5):
    print()
for item in range(1,10):
    if item %2==0:
        sum+=item
print(sum)
```

```python
for item in range(100,1000):
    fe=item%10
    shi=item
    bai=item
    print(bai,shi,ge)
    if ge**3+shi**3+bai**3==item:
        print(item)
```

## 流程控制语句

```python
for item in range(3):
    pwd=input('请输入密码')
    if pwd == '8888':
        print('密码正确')
        break
    else:
        print('密码不正确')
```

```python
a=0
while a<3:
     pwd=input('请输入密码')
    if pwd == '8888':
        print('密码正确')
        break
    else:
        print('密码不正确')
```

```python
for item in range(1,51):
    if item%5 == 0:
        print(item)
for item in range(1,51):
    if item%5 != 0:
        continue
    print(item)
```

```python
for item in range(3):
    pwd=input('请输入密码')
    if pwd=='8888':
        print('密码正确')
        break
    else:
        print('密码不正确')
else:
    print('三次密码均错误')
```

```python
a=0
while a<3:
    pwd=input('请输入密码')
    if pwd=='8888':
        print('密码正确')
        break
    else:
        print('密码不正确')
    a+=1
else:
    print('三次密码均错误')
```

```python
for i in range(1,4):
    for j in range(1,5):
        print('*',end='\t')
    print()
```

```python
for i in range(1,10):
    for j in range(1,i+1):
        print(i,'*',j,'=',i*j,end='\t')
	print()
```

```python
for i in range(5):
    for j in range(1,11):
        if j%2 == 0:
            continue
		print(j,end='\t')
	print()
```

# 列表

```python
a=10
lst=['hello','world',98]
print(id(lst))
print(type(lst))
print(lst)
```

## 列表创建

```python
lst=['hello','world',98]
lst2=list(['hello','world',98])
```

# 类与对象

## 定义类

```python
class Student:#Student - 类名：由一个或多个单词组成，每个单词第一个字母大写
    pass
#一切皆对象
print (id(Student))#2926222013040
print(type(Student))#<class type'>
print (Student)#<class'_main_.Student'>
```

```python
class Student:
    native_pace='abc'#直接写在类里的变量，称为类属性
    def __init__(self,name,age):#初始化方法
        self.name=name#self.name - 实体属性，进行了一个赋值操作，将局部变量的name的值赋给实体属性
        self.age=age
    def eat(self):#实例方法
        print('在吃饭')
    @staticmethod
    def method():#静态方法
        print('我使用了static method进行修饰，所以我是静态方法')
    @classmethod
    def cm(cls):#类方法
        print('我是类方法，因为我使用了class method进行修饰')
#在类之内称为方法，类之外称为函数
def dreak:#函数
    print('水')
```

## 对象的创建

```python
class Student:
    native_pace='abc'#直接写在类里的变量，称为类属性
    def __init__(self,name,age):
        self.name=name#self.name - 实体属性，进行了一个赋值操作，将局部变量的name的值赋给实体属性
        self.age=age
    def eat(self):#实例方法
        print('在吃饭')
stu1=Student('zhangsan',20)#stu1 -> Student：根据类对象(Student)创建实例对象(stu1)
stu1.eat()#实例对象名.方法名()
print(stu1.name)
print(stu1.age)
Student.eat(stu1)#类名.方法名(类的对象) -> 实际就是方法定义处的self
#Student.eat(stu1)与stu1.eat()代码功能相同，都是调用Student中的eat方法
```

## 类属性

```python
class Student:
    native_pace='abc'#直接写在类里的变量，称为类属性
    def __init__(self,name,age):
        self.name=name#self.name - 实体属性，进行了一个赋值操作，将局部变量的name的值赋给实体属性
        self.age=age
    def eat(self):#实例方法
        print('在吃饭')
print(Student.native_pace)
stu1=Student('zhangsan',20)
stu2=Student('lisi',30)
print(stu1.native_pace)
print(stu2.native_pace)
```

## 类方法

```py
Student.cm()
```



## 静态方法

```py
Student.method
```



## 动态绑定属性和方法

```python
class Student:
    def __init__(self,name,age):
        self.name=name
        self.age=age
    def eat(self):
        print(self.name+'在吃饭')
stu1=Student('zhangsan',20)
stu2=Student('lisi',30)
print(id(stu1))
print(id(stu2))
#为stu1动态绑定属性
stu1.gender='nan'
print(stu1.name,stu1.age,stu1.gender)
print(stu2.name,stu2.age)#stu2未绑定
stu1.eat()
stu2.eat()
def show():
    print('定义在类之外的，成为函数')
#stu1绑定show方法
stu1.show=show
stu1.show()
```

# 面向对象

## 封装

```python
class Car:
    def __init__(self,brand):
        self.brand=brand
    def start(self):
        print('汽车启动了')
car=Car('x5')
car.start()
print(car.brand)
```

```python
class Student:
    def __init__(self,name,age):
        self.name=name
        self.__age=age#年龄不希望在类的外部别使用，所以加了两个__
    def show(self):
        print(self.name,self.__age)
stu=Student('zhangsan',20)
stu.show()
#在类的外部使用name与age
print(stu.name)
print(stu.__age)#error
print(dir(stu))
print(stu._Student__age)#在类之外通过 _Student__age 进行访问
```

## 继承

```python
class Person(object):#Person继承object类
    def __init__(self,name,age):
        self.name=name
        self.age=age
    def info(self):
        print(self.name,self.age)
class Student(Person):
    def __init__(self,name,age,sut_no):
        super().__init__(name,age)
        self.stu_no=stu_no
class Teacher(Person):
    def __init__(self,name,age,teachofyear):
        super().__init__(name,age)
        self.teachofyear=teachofyear
stu=Student('zhangsan',20,'1001')
teacher=Teacher('lisi',34,10)
stu.info()
teacher.info()
```

```python
class A(object):
    pass
class B(object):
    pass
class C(A,B):
    pass
```

### 方法重写

```python
class Person(object):#Person继承object类
    def __init__(self,name,age):
        self.name=name
        self.age=age
    def info(self):
        print(self.name,self.age)
class Student(Person):
    def __init__(self,name,age,sut_no):
        super().__init__(name,age)
        self.stu_no=stu_no
    def info(self):
        super().info()
        print(self.stu_no)
class Teacher(Person):
    def __init__(self,name,age,teachofyear):
        super().__init__(name,age)
    def info(self):
        super().info()
        print(self.teachofyear)
        self.teachofyear=teachofyear
stu=Student('zhangsan',20,'1001')
teacher=Teacher('lisi',34,10)
stu.info()
teacher.info()
```

```python
class Student:
    def __init__(self,name,age):
        self.name=name
        self.age=age
    def __str__(self):
        return'mz{0},jn{1}'.format(self.name,self.age)
stu=Student('zhangsan',20)
print(dif(stu))
print(stu)
```

## 多态

```python
class Animal(object):
    def eat(self):
        print('dwhc')
class Dog(Animal):
    def eat(self):
        print('gcgt')
class Cat(Animal):
    def eat(self):
        print('mcy')
class Person:
    def eat(self):
        print('rcwgzl')
#定义一个函数
def fun(obj):
    obj.eat()
#开始调用函数
fun(Cat())
fun(Dog())
fun(Animal())
fun(Person())
```

```python
class A:
    pass
class C:
    pass
class C(A,B):
    def __init__(self,name,age):
        self.name=name
        self.age=age
class D(A):
    pass
x=C('jack',20)#x是C类型的一个实例对象
print(x.__dict__)#实例对象的属性字典
print(C.__dict__)
print(x.__class__)#输出对象所属的类
print(C.__bases__)#C类的父类类型的元素
print(C.__base__)#类的基类：谁在前输出谁
print(C.__mro__)#类的层次结构
print(A.__subclasses__())#子类列表
```

```python
a=20
b=100
c=a+b#两个整数类型的对象的相加操作
d=a.__add__(b)
print(c)
print(d)
class Student:
    def __init(self,name):
        self.name=name
    def __add__(self,other):
        return self.name+other.name
    def __len__(self,name):
        return len(self.name)
stu1=Student('zhangsan')
stu2=Student('lisi')
s=stu1+stu2#实现两个对象的加法运算(因为在Student类中，编写 __add__() 特殊方法)
print(s)
s=stu1.__add__(stu2)
print(s)
lst=[11,22,33,44]
print(len(lst))#len是内容函数
print(lst.__len__())
print(len(stu1))
```

```python
class Person(object):
    def __init__(self,name,age):
        self.name=name
        self.age=age
    def __new__(cls,*args,**kwargs):
        print('__new__被调用执行了，cls的id值为{0}'.format(id(cls)))
        obj=super().__new__(cls)
        print('创建的对象的id为：{0}'.format(id(obj)))
        return obj
    def __init__(self,name,age):
        print('__init__被调用了，self的id为：{0}'.formage(id(self)))
        self.name=name
        self.age=age
print('object这个类对象的id为：{0}'.format(id(object)))
print('Person这个对象的id为：{0}'.format(id(Person)))
p1=Person('zhangsan',20)
print('p1这个Person类的实例对象的id：{0}'.format(id(p1)))
```

```python
class CPU:
    pass
class Disk:
    pass
class Computer:
    def __init__(self,cpu,disk):
        self.cpu=cpu
        self.disk=disk
cpu1=CPU()
cpu2=cpu1
print(cpu1)
print(cpu2)
#类的浅拷贝
disk=Disk()
computer=Computer(cpu1,disk)
#浅拷贝：源对象和拷贝对象引用同一个子对象
import copy
computer2=copy.copy(computer)
print(computer,computer.cpu,computer.disk)
print(computer2.computer2.cpu,computer2.disk)
#深拷贝：同时拷贝源对象的子对象
computer3=copy.deepcopy(computer)
print(computer,computer.cpu,computer.disk)
print(computer3.computer3.cpu,computer3.disk)
```

## 模块

```python
def fun():
    pass
def fun2():
    pass
class Student:
    native_place='abf'#类属性
    def eat(self,name,age):
        self.name=name
        self.age=age
    @classmethod
    def cm(cls):
        pass
    @staticmethod
    def sm():
        pass
a=10
b=20
print(a+b)
#导入模块
import math
print(math.pi)
print(dir(math))
from math import  pi
print(pi)
```

```python
#新建模块：可以是创建一个.py的文件
#新建模块：calc.py
def add(a,b):
    return a+b
def div(a,b):
    return a/b
#导入自定义模块,在另一个.py文件打开
from calc import add
print(add(10,20))
```

## 以主程序运行

```python
#新建模块：calc2.py
def add(a,b):
    return a+b
if __name__=='__main__':#只有点击运行calc2时，才会运行
    print(add(10,20))
#导入模块
import calc2
print(calc2.add(100,200))
```

## 包

```python
#包包含模块
#directory：目录
#创建包 - package：包
#directory和package区别：package有__init__.py的文件，而directory没有
#创建模块
#module_A.py
a=10
#module_B.py
b=100
#导入pageage包
import pageage.module_A as ma#ma是pageage.module_A的别名
print(ma.a)
#使用import方式导入时，只能跟包名或模块名,使用from...import导入包，模块，函数，变量
```

## 常用模块

```python
import sys
import time
import urllib.request
print(sys.getsizeof(24))
print(sys.getsizeof(True))
print(sys.getsizeof(False))
print(time.time())
print(time.localtime(time.time()))
print(urllib.request.urlopen('http://www.baidu.com').read)
```

# 1 变量

变量就是一个存储数据的时候当前数据所在的内存地址的名字

## 1.1 标识符

+ 由数字、字母、下划线组成
+ 不能数字开头
+ 不能使用内置关键字
+ 严格区分大小写

## 1.2 命名习惯

- 大驼峰：每个单词首字母都大写，例如：MyName
- 小驼峰：第二个(含)以后的单词首字母大写，例如：myName
- 下划线：例如：my_name

## 1.3 使用变量

语法：变量名 = 值

代码

~~~python
#定义变量：存储数据TOM
my_name = 'TOM'
print(my_name)
#定义变量：存储数据 程序员
schoolName = '程序员'
print(schoolName)
~~~

## 1.4 认识bug

所谓bug，就是程序中的错误

代码

~~~python
#定义变量：存储数据TOM
my_name = 'TOM'
print(my_name)
#定义变量：存储数据 程序员
schoolName = '程序员'
print(schoolName)
#bug
schoolName = '程序员'
print(schoolname)#NameError:名字错误
schoolName = '程序员'
 print(schoolName)#IndentationError:缩进错误
print(schoolName)
schoolName = '程序员'#NameError
~~~

## 1.5 Debug工具

使用步骤

1. 打断点
2. Debug调试

# 2 数据类型

* int	整型
* float  浮点型
* str	字符串
* bool   布尔型
* list   列表
* set    集合
* dict   字典

# 3 格式化输出

## 3.1 格式符号

| 格式符号 |               转换                |
| :------: | :-------------------------------: |
|    %s    |              字符串               |
|    %d    |         有符号十进制整数          |
|   %02d   | 显示位数，不足以0补全超出原样输出 |
|    %f    |              浮点数               |
|   %.2f   |           小数点后两位            |
|    %c    |               字符                |
|    %u    |         无符号十进制整数          |
|    %o    |            八进制整数             |
|    %x    |       十六进制整数(小写ox)        |
|    %X    |       十六进制整数(大写OX)        |
|    %e    |        科学计数法(小写'e')        |
|    %E    |        科学计数法(大写'E')        |
|    %g    |           %f和%e的简写            |
|    %G    |           %f和%E的简写            |

~~~py
age = 18
name = 'TOM'
weight = 75.5
stu_id = 1
#1.今年我的年龄是xxx岁
print('今年我的年龄是%d岁' % age)
#2.我的名字是xxx
print('我的名字是%s' % name)
#3.我的体重是xxx公斤
print('我的体重是%.2f公斤' % weight)
#4.我的学号是
print('我的学号是%d' % stu_id)
#5.我的名字是xxx，今年xxx岁了
print('我的名字是%s，今年%d岁了' % (name, age))
#5.1我的名字是xxx，明年xxx岁了
print('我的名字是%s，明年%d岁了' % (name, age + 1))
#6.我的名字是xxx，今年xxx岁了，体重xxx公斤，学号是xxx
print('我的名字是%s，今年%d岁了，体重%.02f公斤，学号是%06d' % (name, age, weight, stu_id))
print('我的名字是%s，今年%s岁了，体重%s公斤' % (name, age, weight))
~~~

语法：f'{表达式}'

~~~python
print(f'我的名字是{name}')
~~~

## 3.2 转义字符

\n:换行
\t:制表符

## 3.3 结束符

print('输出的内容'， end="\n")
默认自带end="\n"

~~~python
print('hello', end="\n")
print('world', end="\t")
print('python', end="...")
~~~

# 4 输入

程序接收用户输入的数据的功能

语法：input("提示信息")

~~~python
password = input("输入密码：")
print(f'密码:{password}')
~~~

# 5 转换数据类型

|     函数      |                         说明                         |
| :-----------: | :--------------------------------------------------: |
| int(x[,base]) |                  将x转换为一个整数                   |
|   float(x)    |                 将x转换为一个浮点数                  |
|    str(x)     |                 将对象x转换为字符串                  |
|   eval(str)   | 用来计算在字符串中的有效Python表达式，并返回一个对象 |
|   tuple(s)    |                将序列s转换为一个元组                 |
|    list(s)    |                将序列s转换为一个列表                 |

~~~python
num = input("请输入一个数")
print(num)#str
print(int(num))#int
~~~

# 6 运算符

- 算数运算符
- 赋值运算符
- 复合赋值运算符
- 比较运算符
- 逻辑运算符

# 7 条件语句

## 7.1 if语句

语法
if 条件:
	条件成立执行代码1
	条件成立执行代码2

代码
~~~python
age = 20
if age >= 18:
    print('已经成年，可以上网')    
~~~

~~~python
age = int(input('请输入您的年龄：'))
if age >= 18:
    print(f'您输入的年龄是{age}, 已经成年, 可以上网')
~~~

## 7.2 if…else…语句

~~~python
age = int(input('请输入您的年龄：'))
if age >= 18:
    print(f'您输入的年龄是{age}, 已经成年, 可以上网')
else:
    print(f'您输入的年龄是{age}, 小朋友, 回家写作业去')
~~~

## 7.3 多重判断

语法
if 条件1:
	条件1成立执行的代码1
	条件1成立执行的代码2
elif 条件2:
	条件2成立执行的代码1
	条件2成立执行的代码2
else:
	以上条件都不成立执行的代码

~~~python
age = int(input('请输入您的年龄：'))
if age < 18:
    print(f'您输入的年龄是{age}, 童工')
elif (age >= 18) and (age <= 60):
    print(f'您输入的年龄是{age}, 合法')
elif age > 60:
    print(f'您输入的年龄是{age}, 退休年龄')
~~~

## 7.4 if嵌套

语法
if 条件1:
	条件1成立执行的代码1
	条件1成立执行的代码2
	if 条件2:
		条件2成立执行的代码1
		条件2成立执行的代码2

~~~python
money = 1
seat = 1
if money == 1:
    print('土豪，请上车')
    if seat == 1:
        print('有空座，坐下了')
    else:
        print('没有座位，站着等')
else:
    print('朋友，没带钱，跟着跑，跑快点')
~~~

### 7.4.1 猜拳游戏

~~~python
import random
player = int(input('请出拳:0--石头:1--剪刀:2--布:'))
computer = random.randint(0, 2)
if ((player == 0) and (computer == 1)) or ((player == 1) and (computer == 2)) or ((player == 2) and (computer == 0)):
    print('玩家获胜，哈哈哈哈')
elif player == computer:
    print('平局，别走，再来一局')
else:
    print('电脑获胜')
~~~

### 7.4.2 随机数

~~~python
import random
num = random.randint(0, 2)
print(num)
~~~

## 7.5 三目运算符

条件成立执行的表达式 if 条件 else 条件不成立执行的表达式

~~~python
a = 1
b = 2
c = a if a > b else b
print(c)
aa = 10
bb = 6
cc = aa - bb if aa > bb else bb - aa
print(cc)
~~~

# 8 循环

1. 作用：让代码更高效的重复执行
2. 分类：while和for

## 8.1 while循环

语法
while条件:
	条件成立重复执行的代码1
	条件成立重复执行的代码2
	.\.\.\.\.\.

~~~python
i = 0
while i < 5:
    print('1')
    i += 1
print('2')
~~~

#### 8.1.0.1 while循环应用

 ~~~python
 i = 1
 result = 0
 while i <= 100:
     result = result + i
     i += 1
 print(result)
 ~~~

```python
i = 1
result = 0
while i <= 100:
    if i % 2 == 0:
        result += i
	i += 1
print(result)
```

~~~python
i = 2
result = 0
while i <= 100:
    result += i
    i += 2
print(result)
~~~

### 8.1.1 break和continue终止while循环

break和continue是循环中满足一定条件退出循环的两种不同方式

break：终止此循环
continue：退出当前一次循环继而执行下一次循环代码

~~~python
i = 1
while i <= 5:
    if i == 4:
        print('吃饱了，不吃了')
        break
    print(f'吃了第{i}个苹果')
    i += 1
~~~

```python
i = 1
while i <= 5:
    if i == 3:
        print('吃出一个大虫子，这个苹果不吃了')
        i += 1
        continue
    print(f'吃了第{i}个苹果')
    i += 1
```

### 8.1.2 while循环嵌套

语法
while 条件1:
	条件1成立执行的代码
	while 条件2:
		条件2成立执行的代码

~~~python
j = 0
while j < 3:
    i = 0
    while i < 3:
        print('1')
        i += 1
    print('2')
    print('3')
    j += 1
~~~

#### 8.1.2.1 while嵌套应用

~~~python
j = 0
while j < 5:
    i = 0
    while i <= j:
        print('*', end='')
        i += 1
    print()
    j += 1
~~~

~~~python
j = 1
while j <= 9:
    i = 1
    while i <= j:
    	print(f'{i} * {j} = {i*j}', end='\t')
    	i += 1
    print()
    j += 1
~~~

## 8.2 for循环

语法
for 临时变量 in 序列:
	重复执行的代码1
	重复执行的代码2

~~~python
str1 = 'abcdef'
for a in str1:
    print(a)
~~~

### 8.2.1 break和continue终止for循环

~~~python
str1 = 'abcdef'
for a in str1:
    if a == 'd':
        print('遇到d不打印')
        break
    print(a)
~~~

~~~python
str1 = 'abcdef'
for a in str1:
    if a == 'd':
        print('遇到d不打印')
        continue
	print(a)
~~~

## 8.3 else

循环可以和else配合使用，else下方缩进的代码指的是当循环正常结束之后要执行的代码

语法
while 条件:
	条件成立重复执行的代码
else:
	循环正常结束之后要执行的代码

~~~python
i = 1
while i <= 5:
    print('1')
    i += 1
else:
    print('2')
~~~

### 8.3.1 while…else

#### 8.3.1.1 退出while循环

~~~python
i = 1
while i <= 5:
    if i == 3:
        break
    print('1')
    i += 1
else:
    print('2')
~~~

~~~python
i = 1
while i <= 5:
    if i == 3:
        continue
    print('1')
    i += 1
else:
    print('2')
~~~

break  不执行 else
continue 执行 else

### 8.3.2 for…else

语法
for 临时变量 in 序列:
	重复执行的代码
else:
	循环正常结束之后要执行的代码

~~~python
str1 = 'abcdef'
for a in str1:
    print(a)
else:
    print('循环正常结束之后执行的代码')
~~~

#### 8.3.2.1 退出for循环

~~~python
str1 = 'abcdef'
for a in str1:
    if a == 'd':
        break
	print(a)
else:
    print('循环正常结束之后执行的代码')
~~~

~~~python
str1 = 'abcdef'
for a in str1:
    if a == 'd':
        continue
	print(a)
else:
    print('循环正常结束之后执行的代码')
~~~

break  不执行 else
continue 执行 else

# 9 字符串

## 9.1 下标

下标作用是通过下标快速找到对应的数据
~~~python
str1 = 'abcdefg'
print(str1)
print(str1[0])
print(str1[1])
~~~

## 9.2 切片

语法：序列[开始位置下标:结束位置下标:步长]

~~~python
str1 = 'abcdefg'
print(str1)
print(str1[2])
print(str1[0:3:1])
~~~

## 9.3 常用操作方法

字符串的常用操作方法有查找、修改、判断三大类

### 9.3.1 查找

字符串查找方法即是查找子串在字符串中的位置或出现的次数

* find():检测某个子串是否包含在这个字符串中，如果在返回这个子串开始的位置下标，否则返回-1
  语法：字符串序列.find(子串, 开始位置下标, 结束位置下标)
  注意：开始和结束位置下标可以省略，表示在整个字符串序列中查找

  ```python
  mystr = "hello world and it and zx and Python"
  print(mystr.find('and'))# 12
  print(mystr.find('and', 15, 30))# 23
  print(mystr.find('ands'))# -1
  ```

* index()
  ```python
  mystr = "hello world and it and zx and Python"
  print(mystr.index('and'))# 12
  print(mystr.index('and', 15, 30))# 23
  print(mystr.index('ands'))# index查找子串不存在，报错
  ```

* count()
  ```python
  mystr = "hello world and it and zx and Python"
  print(mystr.count('and', 15, 30))# 1
  print(mystr.count('and'))# 3
  print(mystr.count('ands'))# 0
  ```

rfind()和find功能相同，从右侧开始
rindex()和index功能相同，从右侧开始

### 9.3.2 修改

所谓修改字符串，指的就是通过函数的形式修改字符串中的数据

* replace():替换
  语法：字符串序列.replace(旧子串, 新子串, 替换次数)

  ```python
  mystr = "hello world and it and zx and Python"
  new_str = mystr.replace('and', 'he')
  new_str = mystr.replace('and', 'he', 1)
  new_str = mystr.replace('and', 'he', 10)
  print(new_str)
  ```

  注意：数据按照是否能直接修改分为可变类型和不可变类型两种。字符串类型的数据修改的时候不能改变原有字符串，属于不能直接修改数据的类型即是不可变类型

* split()：按照指定字符分割字符串
  语法：字符串序列.split(分割字符, num)

  ```python
  mystr = "hello world and it and zx and Python"
  list1 = mystr.split('and')
  list1 = mystr.split('and', 2)
  print(list1)# 丢失分割字符
  ```

  注意：如果分割字符是原有字符串中的子串，分割后则丢失该子串

* join()：用一个字符或子串合并字符串，即是将多个字符串合并为一个新的字符串
  语法：字符串.join(过字符串组成的序列)

  ```python
  mylist = ['aa', 'bb', 'cc']
  new_str = '...'.join(mylist)
  print(mylist)
  ```

* capitalize()：将字符串第一个字符转换成大写
* lower()：将字符串中大写转小写
* upper()：将字符串中小写转大写
* lstrip()：删除字符串左侧空白字符
* rstrip()：删除字符串右侧空白字符
* strip()：删除字符串两侧空白字符

* ljust()：返回一个原字符串左对齐，并使用指定字符（默认空格）填充至对应长度的新字符串
  语法：字符串序列.ljust(长度, 填充字符

* rjust()：左对齐
* center()：居中对齐

### 9.3.3 判断

所谓判断即是判断真假，返回的结果是布尔型数据类型：True或False

* startwith()：检查字符串是否是以指定子串开头，是则返回True,否则返回False。如果设置开始和结束位置下标，则在指定范围内检查。
  语法：字符串序列.startwith(子串, 开始位置下标, 结束为止下标)

  ```python
  mystr = "hello world and it and zx and Python"
  print(mystr.startwith('hello'))
  ```

* endwith()
  ```python
  mystr = "hello world and it and zx and Python"
  print(mystr.endwith('Python'))
  ```

* isalpha()：如果字符串至少有一个字符并且所有字符都是字母则返回True,否则返回False
* isdigit()：如果字符串只包含数字则返回True否则返回False
* isalnum()：如果字符串至少有一个字符并且所有字符都是字母或数字则返回True,否则返回False
* isspace()：如果字符串中只包含空白，则返回True,否则返回False

# 10 列表

[数据1, 数据2, 数据3, 数据4 \.\.\.]
列表可以一次性存储多个数据，且可以为不同数据类型

## 10.1 列表的常用操作

### 10.1.1 查找

#### 10.1.1.1 下标

~~~python
name_list = ['TOM', 'Lily', 'ROSE']
print(name_list[1])
print(name_list[0])
~~~

#### 10.1.1.2 函数

* index()：返回指定数据所在位置的下标
  语法：列表序列.index(数据, 开始位置下标, 结束位置下标)

* count()：统计指定数据在当前列表中出现的次数
* len()：访问列表长度，即列表中数据的个数

~~~python
name_list = ['TOM', 'Lily', 'ROSE']
print(name_list.index('TOM'))
print(name_list.count('TOM'))
print(len(name_list))
~~~

### 10.1.2 判断是否存在

* in：判断指定数据在某个列表序列，如果在返回True,否则返回False
* not in：判断指定数据不在某个列表序列，如果不在返回True,否则返回False

~~~python
name_list = ['TOM', 'Lily', 'ROSE']
print('TOM' in name_list)
pritn('TOM' not in name_list)
~~~

#### 10.1.2.1 判断案例

~~~python
name_list = ['TOM', 'Lily', 'ROSE']
name = input('请输入您的邮箱账号名')
if name in name_list:
    print(f'您输入的名字是{name}, 此用户已经存在')
else:
    print(f'您输入的名字是{name}, 名字不存在')
~~~

### 10.1.3 增加

增加指定数据到列表中

* append()：列表结尾追加数据
  语法：列表序列.append(数据)
  注意：如果append()追加的数据是一个序列，则追加整个序列到列表

  ```python
  name_list = ['TOM', 'Lily', 'ROSE']
  name_list.append('xiaoming')
  name_list.append([11, 22f])
  print(name_list)
  ```

* extend()：列表结尾追加数据，如果数据是一个序列，则将这个序列的数据逐一添加到列表
  语法：列表序列.extend(数据)

  ~~~python
  name_list = ['TOM', 'Lily', 'ROSE']
  name_list.extend('xiaoming')
  name_list.extend('xiaoming', 'xiaohong')
  print(name_list)
  ~~~

  

* insert()：指定位置新增数据
  语法：列表序列.insert(位置下标, 数据)

  ```python
  name_list = ['TOM', 'Lily', 'ROSE']
  name_list.insert(1, 'aaa')
  print(name_list)
  ```

### 10.1.4 删除

* del
  语法：del 目标

* pop()：删除指定下标的数据（默认为最后一个），并返回该数据
  语法：列表序列.pop(下标)

* remove()：移除列表中某个数据的第一个匹配项
  语法：列表序列.remove(数据)

* clear()：清空

~~~python
name_list = ['TOM', 'Lily', 'ROSE']
del(name_list)
del(name_list[0])
print(name_list)
del_name = name_list.pop()
print(del_name)
print(name_list)
name_list.remove('ROSE')
print(name_list)
name_list.clear()
print(name_list)
~~~

### 10.1.5 修改

* 逆置：reverse()
* 排序：sort()
  语法：列表序列.sort(key=None, reverse=False)
  注意：reverse表示排序规则，reverse=True降序，reverse=False升序（默认）

~~~python
name_list = ['TOM', 'Lily', 'ROSE']
name_list[0] = 'aaa'
print(name_list)
list1 = [1, 3, 2, 5, 4, 6]
list1.reverse()
print(list1)
list1.sort()
print(list1)
list1.sort(reverse=True)
print(list1)
~~~

### 10.1.6 复制

函数：copy()

~~~python
name_list = ['TOM', 'Lily', 'ROSE']
list1 = name_list.copy
print(list1)
print(name_list)
~~~

## 10.2 列表的循环遍历

### 10.2.1 while

~~~python
name_list = ['TOM', 'Lily', 'ROSE']
i = 0
while i < len(name_list):
    print(name_list[i])
    i += 1
~~~

### 10.2.2 for

~~~python
name_list = ['TOM', 'Lily', 'ROSE']
for i in name_list:
    print(i)
~~~

## 10.3 列表嵌套

所谓列表嵌套指的就是一个列表里面包含了其他的子列表

~~~python
name_list = [['TOM', 'Lily', 'Rose'], ['张三', '李四', '王五'], ['xiaoming', 'xiaohong', 'xiaolv']]
print(name_list)
print(name_list[0])
print(name_list[0][1])
~~~

## 10.4 综合练习

随机分配办公室
~~~python
import random
teachers = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
offices = [[], [], []]
for name in teachers:
    num = random.randint(0, 2)
    offices[num].append(name)
i = 1
for office in offices:
    print(f'办公室{1}的人数是{len(office)}, 老师分别是:')
    for name in office:
        print(name)
~~~

# 11 元组

一个元组可以存多个数据，元组内的数据时不能修改的

## 11.1 定义元组

元组特点：定义元组使用小括号，且逗号隔开各个数据，数据可以是不同的数据类型

多个数据元组：t1 = (10, 20, 30)
单个数据元组：t2 = (10,)

## 11.2 元组的常见操作

元组数据不支持修改，只支持查找

* 按下标查找数据
* index()：查找某个数据，如果数据存在返回对应的下标，否则报错，语法和列表、字符串的index方法相同。
* count()：统计某个数据在当前元组出现的次数
* len()：统计元组中数据的个数

~~~python
t1 = ('aa', 'bb', 'cc', 'bb')
print(t1[0])
print(t1.index('bb'))
print(t1.count('aa'))
print(len(t1))
~~~

~~~python
t1 = ('aa', 'bb', 'cc', 'bb')
t2 = ('aa', 'bb', ['cc', 'dd'])
print(t2[2][0])
t2[2][0] = 'TOM'
print(t2)
~~~

注意：元组里的列表数据可以修改

# 12 字典

字典里面的数据是以键值对形式出现，字典数据和数据顺序没有关系，即字典不支持下标后期无论数据如何变化，只需要按照对应的键的名字查找数据即可

## 12.1 创建字典的语法

字典特点

* 符号为大括号
* 数据为键值对形式出现
* 各个键值对之间用逗号隔开

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
print(dict1)
# 空字典
dict2 = {}
dict3 = dict()
~~~

## 12.2 字典常见操作

### 12.2.1 增

写法：字典序列[key] = 值
注意：如果key存在则修改这个key对应的值；如果key不存在则新增此键值对
	  字典为可变类型

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
dict1['id'] = 110
print(dict1)
dict1['name'] = 'ROSE'
print(dict1)
~~~

### 12.2.2 删

* del()/del：删除字典或删除字典中指定键值对
* clear()：清空字典

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
del(dict1)
print(dict1)
del dict1['name']
print(dict1)
dict1.clear()
print(dict1)
~~~

### 12.2.3 改

写法：字典序列[key] = 值
注意：如果key存在则修改这个key对应的值；如果key不存在则新增此键值对

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
dict1['name'] = 'Lily'
print(dict1)
dict1['id'] = 110
print(dict1)
~~~

### 12.2.4 查

#### 12.2.4.1 key值查找

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
print(dict1['name'])
~~~

#### 12.2.4.2 get

语法：字典序列.get(key, 默认值)
注意：当前查找的key不存在则返回第二个参数（默认值），如果省略第二个参数，则返回None

```python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
print(dict1.get('name'))
```

#### 12.2.4.3 keys

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
print(dict1.keys())
~~~

#### 12.2.4.4 values

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
print(dict1.values())
~~~

#### 12.2.4.5 items

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
print(dict1.items())
~~~

## 12.3 字典的循环遍历

### 12.3.1 key

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
for key in dict1.keys():
    print(key)
~~~

### 12.3.2 value

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
for value in dict1.values():
    print(value)
~~~

### 12.3.3 遍历字典的元素

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
for item in dict1.items():
    print(item)
~~~

### 12.3.4 遍历字典的键值对

~~~python
dict1 = {'name': 'TOM', 'age': 20, 'gender': '男'}
for key, value in divt1.items():
    print(f'{key}={value}')
~~~

# 13 集合

## 13.1 创建集合

创建集合使用{}或set()，但是如果要创建空集合只能使用set()，因为{}用来创建空字典

~~~python
s1 = {10, 20, 30, 40, 50}
print(s1)
s1 = set()
~~~

## 13.2 集合常见操作方法

### 13.2.1 增加数据

* add()

* update()：追加的数据是序列

~~~python
s1 = {10, 20}
s1.add(100)
print(s1)
s1.update([10, 20, 30, 40, 50])
print(s1)
~~~

### 13.2.2 删除数据

* remove()：删除集合中的指定数据，如果数据不存在则报错
* discard()：删除集合中的指定数据，如果数据不存在也不会报错
* pop()：随机删除集合中的某个数据，并返回这个数据

~~~python
s1 = {10, 20, 30, 40, 50}
s1.remove(10)
print(s1)
s1.discard(10)
print(s1)
del_num = s1.pop()
print(del_num)
~~~

### 13.2.3 查找数据

* in：判断数据在集合序列
* not in：判断数据在集合序列

~~~python
s1 = {10, 20, 30, 40, 50}
print(10 in s1)
print(10 not in s1)
~~~

# 14 公共操作

## 14.1 运算符

| 运算符 |      描述      |      支持的容器类型      |
| :----: | :------------: | :----------------------: |
|   +    |      合并      |    字符串、列表、元组    |
|   *    |      复制      |    字符串、列表、元组    |
|   in   |  元素是否存在  | 字符串、列表、元组、字典 |
| not in | 元素是否不存在 | 字符串、列表、元组、字典 |

~~~python
str1 = 'aa'
str2 = 'bb'
list1 = [1, 2]
list2 = [10, 20]
t1 = (1, 2)
t2 = (10, 20)
dict1 = {'name': 'Python'}
dict2 = {'age': 30}
print(str1 + str2)
print(list1 + list2)
print(t1 + t2)
~~~

~~~python
str1 = 'a'
list1 = ['hello']
t1 = ('world',)
print(str1 * 5)
print('-' * 10)
print(list1 * 5)
print(t1 * 5)
~~~

~~~python
str1 = 'abcd'
list1 = [10, 20, 30, 40]
t1 = (100, 200, 300, 400)
dict1 = {'name': 'Python', 'age': 30}
print('a' in str1)
print('a' not in str1)
print(10 in list1)
print(10 not in list1)
print(100 not in t1)
print(100 in t1)
print('name' in dict1)
print('name' not in dict1)
print('name' in dict1.keys())
print('name' in dict1.values())
~~~

## 14.2 公共方法

|         函数          |                             描述                             |
| :-------------------: | :----------------------------------------------------------: |
|         len()         |                      计算容器中元素个数                      |
|      del或del()       |                             删除                             |
|         max()         |                     返回容器中元素最大值                     |
|         min()         |                     返回容器中元素最小值                     |
| range(start,end,step) |      生成从start到end的数字，步长为step，供for循环使用       |
|       enumerate       | 函数用于将一个可遍历的数据对象（如列表、元组或字符串）组合为一个索引序列，同时列出数据和数据下标，一般用在for循环当中。 |

enumerate()
语法
enumerate(可遍历对象, start=0)
注意：stat参数用来设置遍历数据的下标的起始值，默认为0

~~~python
list1 = ['a', 'b', 'c', 'd', 'e']
for i in enumerate(list1):
    print(i)
for i in enumerate(list1, start=1):
    print(i)
~~~

## 14.3 容器类型转换

* tuple()：将某个序列转换成元组
* list()：将某个序列转换成列表
* set()：将某个序列转换成集合
  注意：集合可以快速完成列表去重；集合不支持下标

~~~python
list1 = [10, 20, 30, 40, 50]
s1 = {100, 300, 200, 500}
t1 = ('a', 'b', 'c', 'd', 'e')
print(tuple(list1))
print(tuple(s1))
print(list(s1))
print(list(t1))
print(set(list1))
print(set(t1))
~~~

# 15 推导式

## 15.1 列表推导式

作用：用一个表达式创建一个有规律的列表或控制一个有规律列表

* while循环实现
  ~~~python
  list1 = []
  i = 0
  while i < 10:
      list1.append(i)
      i += 1
  print(list1)
  ~~~

* for循环实现
  ~~~python
  list1 = []
  for i in range(10):
      list1.append(i)
  print(list1)
  ~~~

* 列表推导式实现
  ~~~python
  list1 = [i for i in range(10)]
  print(list1)
  ~~~

### 15.1.1 带if的列表推导式

~~~python
list1 = [for i in range(0, 10, 2)]
print(list1)
list2 = []
for i in range(10):
    if i % 2 == 0:
        list2.append(i)
print(list2)
list3 = [i for i in range(10) if i % 2 == 0]
print(list3)
~~~

### 15.1.2 多个for循环实现列表推导式

~~~python
list1 = []
for i in range(1, 3):
    for j in range(3):
        list1.append((i, j))
print(list1)
list2 = [(i, j) for i in range(1, 3) for j in range(3)]
print(list2)
~~~

## 15.2 字典推导式

作用：快速合并列表为字典或提取字典中目标数据

~~~python
dict1 = {i: i**2 for i in range(1, 5)}
print(dict1)
list1 = ['name', 'age', 'gender']
list2 = ['Tom', 20, 'man']
dict1 = {list1[i]: list2[i] for i in range(len(list1))}
print(dict1)
counts = {'MBP': 268, 'HP': 125, 'DELL': 201, 'Lenovo': 199, 'acer': 99}
count1 = {key: value for key, value in counts.items() if value >= 200}
print(count1)
~~~

## 15.3 集合推导式

创建一个集合，数据为下方列表的2次方

~~~python
list1 = [1, 1, 2]
set1 = {i ** 2 for i in list1}
print(set1)
~~~

# 16 函数

## 16.1 函数的使用步骤

### 16.1.1 定义函数

语法
def 函数名(参数)
	代码1
	代码2

### 16.1.2 调用函数

函数名(参数)

### 16.1.3 快速体验

~~~python
def sel_func():
	print('显示余额')
	print('存款')
	print('取款')
print('恭喜您登录成功')
sel_func()
print('您的余额是10000')
sel_func()
print('取了100元钱')
sel_func()
~~~

## 16.2 函数的说明文档

* 定义说明文档
  语法
  def sum_num(a, b):
  	"""求和函数"""
  	return a + b
  help(sum_num)
* 查看说明文档
  语法：help(函数名)

## 16.3 函数嵌套调用

所谓函数嵌套调用指的是一个函数里面又调用了另外一个函数

~~~python
def testB():
    print('B函数开始')
    print('这是B函数')
    print('B函数结束')
def testA():
    print('A函数开始')
    testB()
    print('A函数结束')
testA()
~~~

### 16.3.1 打印图形

~~~python
def print_line():
    print('-' * 20)
def print_lines(num):
    i = 0
    while i < num:
        print_line()
        i += 1
print_lines(5)
~~~

### 16.3.2 函数计算

~~~python
def sum_num(a, b, c):
    return a + b + c
result = sum_num(1, 2, 3)
print(result)
def average_num(a, b, c):
    sumResult = sum_num(a, b, c)
    return sumResult / 3
averageResult = average_num(1, 2, 3)
print(averageResult)
~~~

## 16.4 变量作用域

变量作用域指的是变量生效的范围，主要分为两类：局部变量和全局变量

* 局部变量：定义在函数体内部的变量，即只在函数体内部生效
  作用：在函数体内部，临时保存数据，即当函数调用完成后，则销毁局部变量

  ~~~python
  def testA():
      a = 100
      print(a)
  testA
  print(a)
  ~~~

* 全局变量：指的是在函数体内、外都能生效的变量
  ~~~python
  a = 100
  print(a)
  def testA():
      print(a)
  def testB():
      print(a)
  testA()
  testB()
  ~~~

~~~python
a = 100
print(a)
def testA():
    print(a)
def testB():
    global a
    a = 200
    print(a)
testA()
testB()
print(a)
~~~

## 16.5 多函数程序执行流程

一般在实际开发过程中，一个程序往往由多个函数（后面知识中会讲解类）组成，并且多个函数共享某些数据

* 共用全局变量
  ~~~python
  glo_num = 0
  def test1():
      global glo_num
      glo_num = 100
  def test2():
      print(glo_num)
  print(glo_num)
  test1()
  test2()
  print(glo_num)
  ~~~

* 返回值作为参数传递
  ~~~python
  def test1():
      return 50
  def test2(num):
      print(num)
  result = test1()
  test2(result)
  ~~~

## 16.6 函数的返回值

~~~python
def return_num():
    return 1, 2
result = return_num()
print(result)
~~~

## 16.7 函数的参数

### 16.7.1 位置参数

调用函数时根据函数定义的参数位置来传递参数
注意：传递和定义参数的顺序及个数必须一致

~~~python
def user_info(name, age, gender):
    print(f'您的姓名{name}, 年龄是{age}, 性别是{gender}')
user_info('TOM', 20, '男')
~~~

### 16.7.2 关键字参数

函数调用，通过“键=值"形式加以指定。可以让函数更加清晰、容易使用，同时也清除了参数的顺序需求
注意：函数调用时，如果有位置参数时，位置参数必须在关键字参数的前面，但关键字参数之间不存在先后顺序

~~~python
def user_info(name, age, gender):
    print(f'您的姓名{name}, 年龄是{age}, 性别是{gender}')
user_info('ROSE', age=20, gender='女')
user_info('小明', gender='男', age=19)
~~~

### 16.7.3 缺省参数

缺省参数也叫默认参数，用于定义函数，为参数提供默认值，调用函数时可不传该默认参数的值（注意：所有位置参数必须出现在默认参数前，包括函数定义和调用)
注意：函数调用时，如果为缺省参数传值则修改默认参数值；否则使用这个默认值

~~~python
def user_info(name, age, gender='男'):
    print(f'您的姓名{name}, 年龄是{age}, 性别是{gender}')
user_info('TOM', 18)
user_info('TOM', 18, gender='女')
~~~

### 16.7.4 不定长参数

不定长参数也叫可变参数。用于不确定调用的时候会传递多少个参数（不传参也可以）的场景。此时，可用包裹(packing)位置参数，或者包裹关键字参数，来进行参数传递，会显得非常方便

* 包裹位置传递
  ~~~python
  def user_info(*args):
      print(args)
  user_info('TOM')
  user_info('TOM', 18)
  ~~~

  注意：传进的所有参数都会被args变量收集，它会根据传进参数的位置合并为一个元组(tuple)args是元组类型，这就是包裹位置传递

* 包裹关键字传递
  ~~~python
  def user_info(**kwargs):
      print(kwargs)
  user_info(name='TOM', age=18, id=110)
  ~~~

## 16.8 拆包和交换变量值

### 16.8.1 拆包

* 元组
  ~~~python
  def return_num():
      return 100, 200
  num1, num2 = return_num()
  print(num1)
  print(num2)
  ~~~

* 字典
  ```py
  dict1 = {'name': 'TOM', 'age': 18}
  a, b = dict1
  print(a)
  print(b)
  print(dict[a])
  print(dict[b])
  ```

### 16.8.2 交换变量值

~~~python
a = 10
b = 20
c = 0
c = a
a = b
b = c
print(a)
print(b)
~~~

~~~python
a, b = 1, 2
a, b = b, a
print(a)
print(b)
~~~

## 16.9 引用

在Python中，值是靠引用来传递来的
我们可以用id()来判断两个变量是否为同一个值的引用。我们可以将id值理解为那块内存的地址标识

~~~python
a = 1
b = a
print(b)
print(id(a))
print(id(b))
a = 2
print(b)
print(id(a))
print(id(b))
~~~

~~~python
aa = [10, 20]
bb = aa
print(id(aa))
print(id(bb))
aa.append(30)
print(bb)
print(id(aa))
print(id(bb))
~~~

## 16.9.1 引用当做实参

~~~python
def test1(a):
    print(a)
    print(id(a))
    a += a
    print(a)
    print(id(a))
b = 100
test1(b)
c = [11, 22]
test1(c)
~~~

## 16.10可变和不可变类型

所谓可变类型与不可变类型是指：数据能够直接进行修改，如果能直接修改那么就是可变，否则是不可变

* 可变类型
  * 列表
  * 字典
  * 集合
* 不可变类型
  * 整型
  * 浮点数
  * 字符串
  * 元组

# 17 函数加强

## 17.1 学生管理系统

~~~python
def info_print():
    print('请选择功能')
    print('1、添加学员')
    print('2、删除学员')
    print('3、修改学员')
    print('4、查询学员')
    print('5、显示所有学员')
    print('6、退出系统')
    print('-' * 20)
info = []
def add_info():
    new_id = input('请输入学号：')
    new_name = input('请输入姓名：')
    nwe_tel = input('请输入手机号码：')
    global info
    for i in info:
        if new_name == i['name']
        print('此用户已经存在')
    info_dict = {}
    info_dict['id'] = new_id
    info_dict['name'] = new_name
    info_dict['tel'] = new_tel
    info.append(info_dict)
    print(info)
def del_info():
    del_name = input('请输入要删除的学员的姓名：')
    global info
    for i in info:
        if del_name == i['name']:
            info.remove(i)
            break
    else:
        print('该学员不存在')
    print(info)
def modify_info():
    modify_name = input('请输入要修改的学员的姓名：')
    global info
    for i in info:
        if modify_name == i['name']:
            i['tel'] = input('请输入新的手机号：')
            break
    else:
        print('该学员不存在')
    print(info)
def search_info():
    search_name = input('请输入要查询的学员的姓名：')
    global info
    for i in info:
        if search_name == i['name']:
            print('查询到的学员信息如下')
            print(f"学员的学号是{i['id']}, 姓名是{i['name']}, 手机号{i['tel']}")
            break
    else:
        print('查无此人')
def print_all():
    print('学号\t姓名\t手机号')
    for i in info:
        print(f'{i["id"]}\t{i["name"]}\t{i["tel"]}')
while True:
    info_print()
    user_num = int(input('请输入功能序号：'))
    if user_num == 1:
        add_info()
    elif user_num == 2:
        del_info()
    elif user_num == 3:
        modify_info()
    elif user_num == 4:
        search_info()
    elif user_num == 5:
        print_all()
    elif user_num == 6:
        input('确认要退出吗？y or s')
        if exit_flag == 'y':
            break
    else:
        print('请输入的功能序号有误')
~~~

## 17.2 递归

* 函数内部自己调用自己
* 必须有出口

~~~python
def sum_numbers(num):
    if num == 1:
        return 1
    return num + sum_numbers(num-1)
sum_result = sum_numbers(3)
print(sum_result)
~~~

### 17.2.1 递归代码实现

~~~python
def sum_numbers(num):
    return num + sum_numbers(num-1)
result = sum_numbers(3)
print(result)
~~~

# 18 lambda表达式

如果一个函数有一个返回值，并且只有一句代码，可以使用lambda简化
语法：lambda 参数列表: 表达式
注意：lambda表达式的参数可有可无，函数的参数在lambda表达式中完全适用
	  lambda表达式能接收任何数量的参数但只能返回一个表达式的值

~~~python
def fn1():
    return 100
result = fn1()
print(result)
fn2 = lambda: 100
pritn(fn2)
print(fn2())
~~~

注意：直接打印lambda表达式，输出的是此lambda的内存地址

## 18.1 lambda的参数形式

* 无参数
  ~~~python
  fn1 = lambda: 100
  print(fn1())
  ~~~

* 一个参数
  ~~~python
  fn1 = lambda a: a
  print(fn1('hello world'))
  ~~~

* 默认参数
  ~~~python
  fn1 = lambda a, b, c=100: a + b + c
  print(fn1(10, 20))
  ~~~

* 可变参数 *args
  ~~~python
  fn1 = lambda *args: args
  print(fn1(10, 20, 30))
  ~~~

  注意：这里的可变参数传入到lambda之后，返回值为元组

* 可变参数 *\*kwargs
  ~~~python
  fn1 = lambda **kwargs: kwargs
  print(fn1(name='Python', age=20))
  ~~~

## 18.2 lambda的应用

* 带判断的lambda
  ~~~python
  fn1 = lambda a, b: a if a > b else b
  print(fn1(1000, 500))
  ~~~

* 列表数据按字典key的值排序
  ~~~python
  students = [
      {'name': 'TOM', 'age': 20}
      {'name': 'ROSE', 'age': 19}
      {'name': 'Jack', 'age': 22}
  ]
  students.sort(key=lambda x: x['name'])
  print(students)
  students.sort(key=lambda x: x['name'], reverse=True)
  print(student)
  ~~~

# 19 高阶函数

把函数作为参数传入，这样的函数称为高阶函数，高阶函数是函数式编程的体现。函数式编程就是指这种高度抽象的编程范式

~~~python
def add_num(a, b):
    return abs(a) + abs(b)
result = add_sum(-1.1, 1.9)
print(result)
def sum_num(a, b, f):
    return f(a) + f(b)
result1 = sum_num(-1, 5, abs)
print(result1)
sum_num(1.1, 1.3)
~~~

## 19.1 内置高阶函数

### 19.1.1 map

map(func,lst),将传入的函数变量func作用到lst变量的每个元素中

~~~python
def func(x):
    return x ** 2
result = map(func, list1)
pritn(result)
print(list(result))
~~~

### 19.1.2 rduce

reduce(func,lst),其中func必须有两个参数。每次func计算的结果继续和序列的下一个元素做累积计算
注意：reduce()传入的参数func必须接收2个参数

~~~python
import functools
list1 = [1, 2, 3, 4, 5]
def func(a, b):
    return a + b
result = functools.reduce(func, list1)
print(result)
~~~

### 19.1.3 filter

filter(func, lst)函数用于过滤序列，过滤掉不符合条件的元素，返回一个filter对象。如果要转换为列表，可以使用list()
~~~python
list1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
def func(x):
    return x % 2 == 0
result = filter(func, list1)
print(result)
print(list(result))
~~~

# 20 文件操作

文件操作的作用就是把一些内容(数据)存储存放起来，可以让程序下一次执行的时候字节使用，而不必中心制作一份，省时省力

## 20.1 文件操作步骤

### 20.1.1 打开

在Python，使用open函数，可以打开一个已经存在的文件，或者创建一个新文件

~~~python
open(name, mode)
~~~

name：是要打开的目标文件名的字符串(可以包含文件所在的具体路径)
mode：设置打开文件的模式(访问模式)：只读、写入、追加

~~~python
f = open('test.txt', 'w')
~~~

### 20.1.2 文件对象方法

#### 20.1.2.1 写

~~~python
f.write('aaa')
~~~

#### 20.1.2.2 读

* read()
  ~~~python
  文件对象.read(num)
  ~~~

  num表示要从文件中读取的数据的长度（单位是字节），如果没有传入num,那么就表示读取文件中所有的数据。
  ~~~python
  f = open('test.txt', 'r')
  print(f.read())
  print(f.read(10))
  f.close()
  ~~~

* readlines()
  readlines可以按照行的方式把整个文件中的内容进行一次性读取，并且返回的是一个列表，其中每一行的数据为一个元素

  ~~~python
  f = open('test.txt', 'r')
  con = f.readlines()
  print(con)
  f.close()
  ~~~

* readline()
  readline一次读取一行内容

  ~~~python
  f = open('test.txt', 'r')
  con = f.readline()
  print(con)
  con = f.readline()
  print(con)
  con = f.readline()
  print(con)
  f.close()
  ~~~

* seek()
  作用：用来移动文件指针
  语法：文件对象.seek(偏移量, 起始位置)

  * 0 - 文件开头
  * 1 - 当前位置
  * 2 - 文件结尾

  ~~~python
  f = open('test.txt', 'r+')
  f.seek(2, 0)
  f.seek(0, 2)
  con = f.read()
  print(con)
  f.close()
  f = open('test.txt', 'a+')
  f.seek(0, 0)
  con = f.read()
  print(con)
  f.close()
  ~~~

### 20.1.3 关闭

~~~python
文件对象.close()
~~~

~~~python
f.close()
~~~

## 20.2 访问模式

| 模式 |                             描述                             |
| :--: | :----------------------------------------------------------: |
|  r   | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式 |
|  w   | 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件 |
|  a   | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入 |
|  r+  |       打开一个文件用于读写。文件指针将会放在文件的开头       |
| rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头 |
|  wb  | 以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件 |
|  w+  | 打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件 |
| wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件 |
|  ab  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入 |
|  a+  | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写 |
| ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写 |

~~~python
f = open('test.txt', 'r')
f.write('aa')# 不写入，r表示只读
f.close()
f = open('1.txt', 'w')
f.write('abc')
f.close()
f = open('2.txt', 'a')
f.write('xyz')
f.close()
f = open('100.txt')
f.close()
f = open('test.txt', 'r+')
con = f.read()
print(con)
f.close()
f = open('test.txt', 'w+')
con = f.read()
print(con)
f.close()
f = open('test.txt', 'a+')
con = f.read()
print(con)
f.close()
~~~

# 21 文件备份

## 21.1 步骤

1. 接收用户输入的文件名
   ~~~python
   old_name = input('请输入您要备份的文件名:')
   print(old_name)
   print(type(old_name))
   ~~~

2. 规划备份文件名

   1. 提取目标文件后缀

   2. 组织备份的文件名.xx[备份]后缀

      ~~~python
      old_name = input('请输入您要备份的文件名:')
      index = old_name.rfind('.')
      print(old_name[:index])
      print(old_name[index:])
      new_name = old_name[:index] + '[备份]' + old_name[index:]
      print(new_name)
      ~~~

3. 备份文件写入数据

   1. 打开源文件和备份文件

   2. 将源文件数据写入备份文件

   3. 关闭文件

      ~~~python
      old_name = input('请输入您要备份的文件名:')
      index = old_name.rfind('.')
      if index > 0:
          postfix = old_name[index:]
      new_name = old_name[:index] + '[备份]' + postfix
      old_f = open(old_name, 'rb')
      old_f = open(old_name, 'wb')
      while True:
          con = old_f.read(1024)
          if len(con) == 0:
              break
          new_f.write(con)
      old_f.close()
      new_f.close()
      ~~~

# 22 文件和文件夹的操作

导入模块os

~~~python
import os
os.函数名()
~~~

## 22.1 文件重命名

~~~python
import os
os.rename('1.txt', '10.txt')
~~~

## 22.2 文件删除

~~~python
import os
os.remove('10.txt')
~~~

## 22.3 创建文件夹

~~~python
import os
os.mkdir('aa')
~~~

## 22.4 删除文件夹

~~~python
import os
os.rmdir('aa')
~~~

## 22.5 获取当前目录

~~~python
import os
print(os.getcwd)
~~~

## 22.6 改变目录路径

~~~python
import os
os.mkdir('aa')
os.chdir('aa')
os.mkdir('bb')
~~~

## 22.7 获取目录列表

~~~python
import os
print(os.listdir())
print(os.listdir('aa'))
~~~

## 22.8 文件夹重命名

~~~python
import os
os.rename(旧文件名, 新文件名)
~~~

## 22.9 应用案例

~~~python
import os
flag = 2
file_list = os.listdir()
for i in file_list:
    if flag == 1:
        new_name = 'Python_' + i
    elif flag == 2:
        num = len('Python_')
        new_name = i[num:]
    os.rename(i, new_name)
~~~

# 23 面向对象

## 23.1 类和对象
关系：用类去实例化(创建)一个对象

### 23.1.1 类

类是对一系列具有相同特征和行为的事物的统称，是一个抽象的概念，不是真实存在的事物

* 特征既是属性
* 行为既是方法

### 23.2 定义类

语法
class 类名():
	代码
注意：类名要满足标识符命名规则，同时遵循大驼峰命名规则

~~~python
class Washer():
    def wash(self):
        print('洗衣服')
haier = Washer()
print(haier)
haier.wash()
~~~

### 23.2.1 self

~~~python
class Washer():
    def wash(self):
        print('洗衣服')
haier1 = Washer()
haier1.wash()
haier2 = Washer()
haier2.wash()
~~~

## 23.3 创建对象

语法：对象名 = 类名()

## 23.4 添加和获取对象属性

### 23.4.1 类外面添加对象属性

语法：对象名.属性名 = 值

~~~python
class Washer():
    def wash(self):
        print('洗衣服')
haier1 = Washer()
haier1.wash = 400
haier1.height = 500
~~~

### 23.4.2 类外面获取对象属性

语法：对象名.属性名
~~~python
class Washer():
    def wash(self):
        print('洗衣服')
haier1 = Washer()
haier1.wash = 400
haier1.height = 500
print(f'洗衣机的宽度是{haier1.width}')
print(f'洗衣机的高度是{haier1.height}')
~~~

### 23.4.3 类里面获取对象属性

语法：self.属性名
~~~python
class Washer():
    def wash(self):
        print('洗衣服')
    def print_info(self):
        print(self.width)
haier1 = Washer()
haier1.wash = 400
haier1.height = 500
haier1.print.info()
~~~

## 23.5 魔法方法

魔法方法指的是具有特殊功能的函数

### 23.5.1 __init\_\_()

_\_init__()方法的作用：初始化对象
~~~python
class Washer():
    def __init__(self):
        self.width = 500
        self.height = 500
	def print_info(self):
        print(f'洗衣机的宽度是{self.width}')
        print(f'洗衣机的高度是{self.height}')
haier1 = Washer()
haier1.print_info()
~~~

注意

* _\_init__()方法，在创建一个对象的默认被调用，不需要手动调用
* _\_init\_\_(self)中的self参数，不需要开发者传递，Python解释器会自动把当前的对象引用传递过去

~~~python
class Washer():
    def __init__(self, width, height):
        self.width = width
        self.height = height
    def print_info(self):
        print(f'洗衣机的宽度是{self.width}, 洗衣机的高度是{self.height}')
haier1 = Washer(10, 20)
haier1.print_info()
haier2 = Washer(100, 200)
haier2.print_info()
~~~

### 23.5.2 _\_str__()

当使用print输出对象的时候，默认打印对象的内存地址。如果类定义__str\_\_方法，那么就会打印从在这个方法中return的数据
~~~python
class Washer():
    def __init__(self, width, height):
        self.width = width
        self.height = height
    def __str__(self):
        return '这是海尔洗衣机的说明书'
haier1 = Washer(10, 20)
print(haier1)
~~~

### 23.5.3 __del\_\_()

当删除对象时，Python解释器也会默认调用del方法

~~~python
class Washer():
    def __init__(self):
        self.width = 300
	def __del__(self):
        print('对象已经删除')
haier = Washer()
~~~

## 23.6 综合应用

~~~python
class SweetPotato():
    def __init__(self):
        self.cook_time = 0
        self.cook_state = '生的'
        self.condiments = []
	def cook(self, time):
        self.cook_time += time
        if 0 <= self.cook_time < 3:
            self.cook_state = '生的'
        elif 3 <= self.cook_time < 5:
            self.cook_state = '半生不熟'
        elif 5 <+ self.cook_time < 8:
            self.cook_static = '熟了'
        elif self.cook_time >= 8:
            self.cook_static = '烤糊了'
	def add_coniments(self, condiment):
        self.condiments.append(coniment)
	def __str__(self):
        return f'这个地瓜烤了{self.cook_time}分钟, 状态{self.cook_static}'
digua1 = SweetPotato()
print(digua1)
digua1.cook(2)
digua1.add_condiments('酱油')
print(digua1)
digua1.cook(2)
digua1.add_condiments('辣椒面儿')
print(digua1)
~~~

~~~python
class Furniture():
    def __init__(self, name, area):
        self.name = name
        self.area = area
class Home():
    def __init__(self, address, area):
        self.address = address
        self.area = area
        self.free_area = area
        self.furniture = []
	def __str__(self):
        return f'房子地理位置{self.address}, 房屋面积是{self.area}, 剩余面积{self.free_area}, 家居有{self.furniture}'
    def add_furniture(self, item):
        if item.area <= self.free_area:
            self.furniture.append(item.name)
            self.free_area -= item.area
        else:
            print('用户家居太大，剩余面积不足，无法容纳')
bed = Furniture('双人床', 6)
sofa = Furniture('沙发', 10)
jia1 = Home('北京', 1000)
print(jia1)
jia1.add_furniture(bed)
print(jia1)
ball = Furniture('篮球场', 2000)
jia1.add_furniture(ball)
print(jia1)
~~~

# 24 继承

Python面向对象的继承指的是多个类之间的所属关系，即子类默认继承父类的所有属性和方法

~~~python
class A(object):
    def __init__(self):
        self.num = 1
    def info_print(self):
        print(self.num)
class B(A):
    pass
result =  B()
result.info_print()
~~~

## 24.1 单继承

~~~python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
class Prentice(Master):
    pass
daqiu = Prentice()
print(daqiu.kongfu)
~~~

## 24.2 多继承

~~~python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
class School(object):
    def __init__(self):
        self.kongfu = '[煎饼果子配方]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
class Prentice(School, Master):
    pass
daqiu = Prentice()
print(daqiu.kongfu)
daqiu.make_cake()
~~~

注意：当一个类有多个父类的时候，默认使用第一个父类的同名属性和方法

## 24.3 子类重写父类同名方法和属性

~~~python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
class School(object):
    def __init__(self):
        self.kongfu = '[煎饼果子配方]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
class Prentice(School, Master):
    def __init__(self):
        self.kongfu = '[独创煎饼果子技术]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
daqiu = Prentice()
print(daqiu.kongfu)
daqiu.make_cake()
print(Prentice.__mro__)
~~~

## 24.4 子类调用父类的同名方法和属性

~~~python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
class School(object):
    def __init__(self):
        self.kongfu = '[煎饼果子配方]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
class Prentice(School, Master):
    def __init__(self):
        self.kongfu = '[独创煎饼果子技术]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
	def make_master_cake(self):
        Master.__init__(self)
        Master.make_cake(self)
	def make_school_cake(self):
        School.__init__(self)
        School.make_cake(self)
daqiu = Prentice()
daqiu.make_cake()
daqiu.make_master_cake()
daqiu.make_school_cake()
daqiu.make_cake()
~~~

## 24.5 多层继承

~~~python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法煎饼果子配方]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
class School(object):
    def __init__(self):
        self.kongfu = '[煎饼果子配方]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
class Prentice(School, Master):
    def __init__(self):
        self.kongfu = '[独创煎饼果子技术]'
	def make_cake(self):
        print(f'运用{self.kongfu}制作煎饼果子')
	def make_master_cake(self):
        Master.__init__(self)
        Master.make_cake(self)
	def make_school_cake(self):
        School.__init__(self)
        School.make_cake(self)
class Tusun(Prentice):
    pas
daqiu = Prentice()
daqiu.make_cake()
daqiu.make_master_cake()
daqiu.make_school_cake()
daqiu.make_cake()
~~~

