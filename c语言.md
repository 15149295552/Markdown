# 初识c语言

```c
#include<stdio.h>//包含一个叫stdio.h的文件
                 //std - 标准输入输出
int main()//主函数 - 程序的入口 -有且仅有一个
          //int 整型的意思
          //main前面的int表示main函数调用返回一个整型值，可以写void main()(过时)
{
    printf("hello world\n");//打印函数
                            //库函数 - c语言本身提供给我们使用的函数
    return 0;//返回 0 
}
```
## 数据类型
### char
    字符数据类型，1字节
```c
#include<stdio.h>
int main(){
    char ch='A';
    printf("%c\n",ch);
    return 0;
}
```
### short
    短整型，2字节
### int
    整型，4字节
```c
#include<stdio.h>
int main(){
    int age=20;
    printf("%d\n",age);
    return 0;
}
```
### long
    长整型，4/8字节
```c
#include<stdio.h>
int main(){
    long num=100;
    printf("%d\n",num);
    return 0;
}
```
### float
    单精度浮点数，4字节
```c
#include<stdio.h>
int main(){
    float f=5.0;
    printf("%f\n",f);
    return 0;
}
```
### double
    双精度浮点数，8字节
```c
#include<stdio.h>
int main(){

    double d=3.14;
    printf("%lf\n",d);
    return 0;
}
```
    %d - 打印整型
    %c - 打印字符
    %f - 打印浮点数字
    %p - 以地址的形式打印
    %x - 打印16进制数字
### 单位
    bit - 比特位
        最小的单位，只能存放二进制(0/1)
    byte - 字节
        1字节=8个比特位大小
    kb / mb / gb / tb / pb (1024倍)//最大2的32次方-1(从0开始)
```c
#include<stdio.h>
int main(){
    short age=20;//向内存申请2个字节，存放20
    float weight=95.6；//向内存申请4个字节

    return 0;
}
```
## 变量 常量
```c
#include<stdio.h>
int num2=20;//全局变量 - 定义在代码块({})外的变量
int main(){
    int num1=10;//局部变量 - 定义在代码块内的变量
    //全局变量和局部变量可以同时存在 - 就近原则，局部优先(全局和局部变量名字相同时)
    return 0;
}
```
```c
#include<stdio.h>
int main(){
    int num1=0;
    int num2=0;
    int sum=0;
    //c语言语法规定，变量要定义在当前代码最前面
    //输入数据 - 使用输入函数scanf
    scanf("%d%d",&num1,&num2);//&取地址符号
    sum=num1+num2;
    printf("sum=%d\n",sum);
    return 0;
}
```
## 变量的作用域和生命周期
    作用域：限定名字的可用性的代码范围  
        1.局部变量的作用域是变量所在的局部范围
        2.全局变量的作用域是整个工程
    生命周期：变量的生命周期指的是变量的创建到变量的销毁之间的一个时间段
        1.局部变量的生命周期：进入作用域生命周期开始，出作用域生命周期结束
        2.全局变量的生命周期：整个程序的生命周期
```c
#include<stdio.h>
int main(){
    {
        int a=10;
        printf("a=%d\n",a);//ok
    }
    //进入作用域生命周期({})开始，出作用域生命周期({})结束
    printf("a=%d\n",a);//error
    return 0;
}
```
```c
#include<stdio.h>
int main(){
    //extern声明外部函数的
    extern int g_val;
    printf("g_val=%d\n",g_cal);
    return 0;
}
```
## 常量
```c
#include<stdio.h>
int main(){
    //const - 常属性
    const int num=4;//字面常量，int num - const修饰的常变量
    //num是变量，但是又有常属性，所以n为常变量
    printf("%d\n",num);
    num=8;//const时不改变
    printf("%d\n",num);
    return 0;
}
```
```c
#include<stdio.h>
//#define定义的标识符常量
#define MAX 10
int main(){
    int arr[MAX]={0};
    printf("%d\n",MAX);
    return 0;
}
```
```c
#include<stdio.h>
//枚举常量：一一列举常量,关键字 - enum
enum Sex{
    MALE,
    FEMALE,
    SECRET
};
//MALE,FEMALE,SECRET - 枚举常量
int main(){
    printf("%d\n",MALE);
    printf("%d\n",FEMALE);
    printf("%d\n",SECRET);
    return 0;
}
```
```c
#include<stdio.h>
enum Color{
    RED,
    YELLOW,
    BLUE
}
int main(){
    enum Color color = BLUE;//获得蓝色
    BLUE = 6;//error
    return 0;
}
```
## 字符串
```c
#include<stdio.h>
int main(){
    "abcdefg";
    "";//空字符串
    char arr1[]="abc";
    char arr2[]={'a','b','c',0};//0 -> \0：字符串的结束标志
    printf("%s\n",arr1);
    printf("%s\n",arr2);
    return 0;
}
```
```c
#include<stdio.h>
int main(){
    char arr1[]="abc";
    char arr2[]={'a','b','c','\0'};
    printf("%d\n",strlen(arr1));//strleh - 计算字符串长度
    printf("%d\n",strlen(arr2));//\0不算字符串内容
    return 0;
}
```
## 转义字符
```c
#include<stdio.h>
int main(){
    printf("abc\n");
    printf("c:\test\32\test.c");
    printf("c:\\test\\32\\test.c");
    //\n - 换行,\t - 水平制表符,\ - 防止转义
    return 0;
}
```
```c
#include<stdio.h>
#include<string.h>
int main(){
    printf("%d\n",strlen("c:\test\32\test.c"))
    //\32 - 32是两个八进制数字
    return 0;
}
```
## 选择语句
```c
#include<stdio.h>
int main(){
    int input=0;
    printf("学习\n");
    printf("学不学?(1/0):");
    scanf("%d",&input);
    if(input==1){
        printf("ok");
    }
    else{
        printf("no");
    }
    return 0;
}
```
```c
#include<stdio.h>
int main(){
    printf("学习\n");
    while(line<20000){
    printf("敲代码:%d\n",line);
    line++;
    }
    if(line>=20000){
    printf("工作");
    }
    return 0;
}
```
## 函数
```c
#include<stdio.h>
int Add(int x,int y){
    int z=x+y;
    return z;
}
int main(){
    int num1=10;
    int num2=20;
    int sum=0;
    int a=100;
    int b=200;
    //sum=num1+num2;
    sum=Add(num1,num2);
    //sum=a+b;
    sum=Add(a,b);
    printf("sum=%d\n",sum);
    return 0;
}
```
## 数组
```c
#include<stdio.h>
int main(){
    int arr[10]={1,2,3,4,5,6,7,8,9,10};//定义一个存放10个整数数字的数组
    printf("%d\n",arr[4]);//下标的方式访问元素，arr[下标]
    int i=0;
    while(i<10){
        printf("%d\n",arr[i]);
        i++;
    }
    char ch[20];
    float arr2[5];
    return 0;
}
```
## 操作符
```c
#include<stdio.h>
int main(){
    int a=2/5;
    printf("%d\n",a);
    return 0;
}
```
```c
//移位操作符
#include<stdio.h>
int main(){
    //<<左移，>>右移
    int a=1;
    int b=a<<1;//左移一位
    printf("%d\n",n)
    return 0;
}
```
```c
//位操作
#include<stdio.h>
int main(){
    int a=3;
    int b=5;
    int c=a&b;
    printf("%d\n",c);
    int d=a|b;
    printf("%d\n",d);
    return 0;
}
```
## 逻辑操作符

```c
//单目操作符
#include<stdio.h>
int main(){
    int a=10;
    printf("%d\n",a);
    printf("%d\n",!a);
    return 0;
}
```
```c
#include<stdio.h>
int main(){
    int a=0;
    int b=~a;//~ - 按位取反
    //负数在内存中存储的时候，存储的是二进制的补码
    printf("%d\n",b);
    return 0;
}
```
```c
//逻辑操作符
#include<stdio.h>
int main(){
    int a=3;
    int b=5;
    int c=a&&b;//&& - 逻辑与，有一个为假就为假
    printf("c=%d\n",c);
    int d=a||b;//|| - 逻辑或，有一个为真就为真
    printf("d=%d\n",d);
    return 0;
}
```
```c
//条件操作符
#include<stdio.h>
int main(){
    int a=10;
    int b=20;
    int max=0;
    max=(a > b ? a : b);//a > b为真，表达a；为假，表达b
    return 0;
}
```
## 关键字

```c
#include<stdio.h>
int main(){
    register int a=0;//把a定义成寄存器变量
    return 0;
}
```
```c
#include<stdio.h>
int main(){
    //typedef - 类型定义 - 类型重定义
    typedef unsigned in u_int;
    unsigned int num1=20;
    u_int num2=20;
    return 0;
}
```

```c
#include<stdio.h>
void test(){
    //static修饰局部变量，局部变量的生命周期
    static int a=1;
    a++;
    printf("a=%d\n",a)
}//没有static时，当a出来的时候a就不存在了，下次就还是2
//加static时，a不销毁
int main(){
    int i=0;
    while(i<5){
        test();
        i++;
    }
    return 0;
}
```

```c
#include<stdio.h>
int g_val=2020;//全局变量
//static 修饰全局变量
//改变了变量的作用域 - 让静态的全局变量只能在自己所在的源文件，出了源文件就没法再使用了
//static修饰函数改变了函数的链接属性
int main(){
    //extern - 声明外部符号
    extern int g_val;
    printf("g_val=%d\n",g_val)
    return 0;
}
```

```c
#include<stdio.h>
int main(){
    return 0;
}
```

## define定义常量和宏

```c
#include<stdio.h>
#define MAX 100//define定义的标识符常量
#define//定义宏 - 带参数
MAx(int x,int y){
    if (x>y)
        return x;
    else
        return y;
}
//宏的定义
#define MAx(X,Y)(X>Y?X:Y)
int main(){
    int a=MAX;
    int b=10;
    int c=20;
    //函数的方式
    int max =Max(b,c);
    printf("max=%d\n",max);
    //宏的方式
    max MAx(a,b);
    printf("max=%d\n",masx);
    return 0;
}
```

## 指针

```c
#include<stdio.h>
int main(){
    int a=10;//4个字节
    //& - 取地址
    int* p=&a;//有一种变量是用来存放地址的 - 指针变量
    //* - 解引用操作符
    printf("%p\n",&a);
    return 0;
}
```

```c
#include<stdio.h>
int main(){
    char ch='w';
    char*pc=&ch;
    *pc='a';
    printf("%c\n",ch)
    return 0;
}
```

```c
#include<stdio.h>
int main(){
    int a =10;
    int* p=&a;//p - 指针变量
    printf("%p\n",p);
    return 0;
}
```

## 结构体

```c
#include<stdio.h>
#include<string.h>
//创建一个结构体类型
struct Book{
    char name[20];
    short price;
}
int main(){
    //利用结构体类型，创建一个该类型的结构体变量
    struct Book b1={"c语言",55};
    struct Book* pb=&b1;
    // . 结构体变量.成员
    // -> 结构体指针->成员
    printf("%s\n",pb->name);
    printf("%s\n",pb->price);
    //printf("%s\n",(*pb).name);
    //printf("%s\n",(*pb).price);
    printf("书名：%s\n",b1.name);
    pringf("价格：%d\n",b1.price);
    b1.price=15;
    printf("修改后的价格：%d\n",b1.price);
    //strcopy - 字符串拷贝
    strcopy(b1.name,"c++");
    printf("%s\n"b1.name);
    return 0;
}
```

# 分支和循环语句

## 分支语句(选择结构)

### if语句

```c
#include<stdio.h>
int main(){
    int age=10;
    if(age<18){
        printf("未成年\n");
    }
    else{
        printf("成年\n");
    }
    return 0;
}
```

```c
#include<stdio.h>
int maint(){
    int i=0;
    while(i<=100){
        if(i%2==1){
            printf("%d \n",i);
        }
        i++;
    }
    return 0;
}
```

### switch语句

```c
#include<stdio.h>
int maint(){
    int day=0;
    scanf("%d",&day);
    switch(day){
        case 1:
            printf("星期一\n");
            break;
        case 2:
            printf("星期二\n");
            break;
        case 3:
            printf("星期三\n");
            break;
        case 4:
            printf("星期四\n");
            break;
        case 5:
            printf("星期五\n");
            break;
        case 6:
            printf("星期六\n");
            break;
        case 7:
            printf("星期日\n");
            break;
    }
    return 0;
}
```

```c
#include<stdio.h>
int maint(){
    int day=0;
    scanf("%d",&day);
    switch(day){
        case 1:
        case 2:
        case 3:
        case 4:
        case 5:
            printf("工作日\n");
            break;
        case 6:
        case 7:
            printf("休息日\n");
            break;
        default:
            printf("输入错误\n");
            brea
    }
    return 0;
}
```

```c
#include<stdio.h>
int main(){
    while(){
        printf("%d",i);
    i++;
    }
	return 0;
}
```

