# 一 内存布局

代码

```C
#include<stdio.h>
#include<stdlib.h>
int g = 10;
int g3;  //全局变量g3 未初始，bss段 bss段默认初始为0
int g2 = 20;
void add();
void add(){
  int c=30+20;
}
int main(int argc,char *argv[]){
    static int s=0; //局部可见 全局生命
    int a = 0;
    printf("argc地址=%p\n",&argc);
    printf("argv地址=%p\n",argv);
    printf("------------命令行参数-------------\n");
    printf("局部变量a地址 %p\n"&a);
    printf("---------------栈区---------------\n");
    int *pi=(int *)malloc(10*sizeof(int));
    printf("动态内存申请的内存地址 %p\n",pi);
    printf("---------------堆区---------------\n");
    printf("未初始全部变量g3地址 %p\n",&g3);
    printf("--------------bss段---------------\n");
    printf("静态局部变量s地址 %p\n",&s);
    printf("全部变量g2地址 %p\n",&g2);
    printf("全部变量g地址 %p\n",&g);
    printf("--------------数据段--------------\n");
    printf("字符串常量hello world内存地址 %p\n",pc);
    printf("------------只读数据段-------------\n");
    //函数就是指令首地址
    printf("main函数的内存地址 0x%p\n",main);
    printf("add函数的内存地址 0x%p\n",add);
    printf("--------------代码段--------------\n");
    printf("\n");
    printf("-----------------0---------------\n");
    return 0;
}
```

# 二 虚拟内存

## 1内存布局

    环境变量和命令行参数
    --------------------
    栈区
    ----
    V
    ^
    -
    堆区
    ----
    bss区
    -----数据区
    size命令:
    可以查看可执行程序代码区，数据区和bss区的大小
    代码区
    ------
    使用格式:
    size 可执行文件名称
    size a.out
    text    data    bss   edc   hex filename
    1182    552     8     1742  6ce a.out
    单位：字节
    代码区(test) 数据区(data) bss区
    dec:十进制
    hex:十六进制
    bin:二进制
    oct:八进制

## 2 内存壁垒

    虚拟内存大小为4GB
    将4GB字节大小的内存分为了两个部分：
    0 ~ 3G-1： 用户空间，存放程序员编写的程序中的代码和数据
    每个程序/进程 的用户空间都是0 ~ 3G-1，但不同进程所对应的物理内存确实各自独立的，
    Linux系统会为每个运行的程序的用户空间维护一张专属该程序的 内存映射表，
    记录的是 虚拟内存到物理内存之间的对应关系，因此在不同的进程（运行的程序）之间
    交换 虚拟内存的地址 是毫无意义的
    程序A 程序B
    3G ~ 4G-1：内核空间，存放Linux内核的代码和数据
    不同的程序运行的时候 访问的内核空间的代码和数据是同一份，
    用户空间的内存映射会随着进程的切换而切换
    内核空间的内存映射无需随着进程的切换而切换
    用户空间的代码不能直接访问内存空间的代码和数据
    但是可以通过系统调用，间接的和内核互换

### 2.1段错误

    一切对虚拟内存的 越权访问，都会导致 段错误
    试图访问没有映射到物理内存的虚拟内存
    试图以非法方式来访问虚拟内存，如：对只读内存做写操作 等

## 3虚拟内存的分配和释放

#### 3.1从底层硬件到上层应用，各层的逻辑

```
------------------------------------
应用程序           业务逻辑
stl               标准容器
c++               new/delete
c                 malloc/calloc/realloc/free
posix             sbrk/brk
Linux             mmap/munmap
------------------------------------
    ^---用户层
------------------------------------
    操作系统内核    kmalloc/vmalloc
    操作系统驱动    get_free_page
    硬件           硬件指令集
------------------------------------
    ^---系统层
从底层硬件到上层应用，各层提供了各自的 内存管理 接口，
    处于不同的开发层次，会使用不同层次的功能函数
```

### 3.2 sbrk函数

```
想要在虚拟内存分配一块空间，建立和物理内存之间的映射
    后续直接操作虚拟内存即可
后续不再想使用的时候，断开虚拟内存和物理内存之间的映射
```

```C
#include<unistd.h>
void *sbrk(intptr_t increment);
```

    功能：以相对的方式 分配 和 释放 虚拟内存
    参数：
    increment 内存的字节增量（单位：字节）
        >0    分配内存
        <0    释放内存
        =0    当前堆尾
    当前堆尾指针，需要向右移动increment个字节
    返回值：
    成功    返回调用该函数前的堆尾指针
    失败    返回-1
    分配和释放内存，操作的堆区
    由操作系统分配的，栈区/bss段/数据段
    由开发人员分配的，一定在堆区
    sbrk分配内存的底层实现；
    系统内存维护着一个指针，指向当前堆尾，即堆区最后一个字节的下一个位置
    是一个动态调整的指针
    sbrk函数根据增量参数来调整该指针的位置
    于此同时，会返回该指针在调整前的位置
    举例
    p1 = sbrk(4);  //BBBB ^--- ---- ---- ----
                    //BBBB BBBB ^--- ---- ----
    p2 = sbrk(4);  //BBBB BBBB ^--- ---- ----
                    //BBBB BBBB BBBB ^--- ----
    p3 = sbrk(0);  //BBBB BBBB BBBB ^--- ----
                    //BBBB BBBB BBBB ^--- ----
    p4 = sbrk(-8); //BBBB BBBB BBBB ^--- ----
                    //BBBB ^--- ---- ---- ----
    B；已分配的字节
    -；未分配的字节
    ^；当前堆尾位置
    int brk(void *addr);
    Linux终端：
    man 2 sbrk
    sbrk代码

```C
#include<stdio.h>
#include<string.h>
#include<unistd.h>
int main(void){
    void*p1 = sbrk(0);
    printf("p1 ：%p\n",p1);
    int*p2=sbrk(sizeof(int));
    *p2=520;
    printf("%d\n",*p2);
    double*p3 = sbrk(sizeof(double));
    *p3=13.14;
    printf("%lg\n",*p3);
    char*p4 = sbrk(16*sizeof(char));
    strcpy(p4,”hello,world");
    printf("%s\n",*p4);
    sbrk(-16*sizeof(char)-sizeof(double)-sizeof(int));
    void*p5 = sbrk(0);
    printf("%p\n",p5);
    return 0;
}
```

```C
#include<unistd.h>//unix standard
int brk(void *end_data_segment);
```

```
返回值：
    成功 返回 0
    失败 返回 -1
参数：
    直接来指定 新的堆尾指针，队尾指针前面的内存就是分配的内存
    >当前堆尾指针，表示分配虚拟内存
    <当前堆尾指针，表示释放虚拟内存
    =当前堆尾指针，空操作
注意：
    新堆尾指针 = 原堆尾指针 + 堆内存增量(可正可负)
分析：
    利用brk函数从 堆区 分配和释放内存的步骤
    void *p=sbrk(0);
    brk(p+4);
    brk(p);    //释放4字节内存
在分配和释放内存过程中，p没有发生变化
```

### 3.3 brk函数

    brk函数-虚拟内存的分配和释放
    该函数是以 绝对 的方式分配和释放内存
    brk代码

```C
#include<stdio.h>
#include<string.h>
#include<unistd.h>
int main(void){
    void*p1 = sbrk(0);
    int *p2 = p1;
    brk(p2 + 1);
    *p2 = 520;
    printf("%d\n",*p2);
    double*p3 = (double *)(p2 + 1);
    brk(p3 + 1);
    *p3 = 13.14;
    printf("%lg\n",*p3);
    brk(p2);
    return 0;
}
```

```C
#include <sys/mman.h>
//分配内存，并建立虚拟内存和物理内存映射关系
void *mmap(void *addr,size t length,int prot,int flags,
        int fd,off t offset);
```

    参数：
    addr：要建立映射关系的虚拟内存起始地址
    一般给NULL，操作系统在当前进程的3GB虚拟内存中的预留空间
    找一块空闲的虚拟内存来做映射
    length：要映射的虚拟内存区域的大小
    自动按页(4K)对齐映射
    10字节 - 4K - 4096
    4098字节 - 8K
    prot：对于映射区域的访问权限
    PROT_EXEC：  对于映射区可执行
    PROT_READ：  对于映射区可读
    PROT_WRITE： 对于映射区可写
    PROT_NONE：  对于映射区不可访问
    flags：映射标志，使用以下两个组合即刻：
    MAP_ANONYMOUS | MAP_PRIVATE：仅仅建立虚拟内存和物理内存的映射关系
    MAP_SHARED：建立虚拟内存和磁盘文件之间的映射关系
    fd：文件描述符
    offset：文件偏移量，自动按4KB对齐
    注意：如果建立虚拟内存和物理内存的映射，fd和offset参数都给0
    返回值：
    成功 放回映射的虚拟内存的起始地址
    失败 返回MAP_PAILED(-1)
    结果：
    一旦该函数调用成功，将来访问的虚拟地址就是对应的物理地址
    //释放内存，解除映射关系

```c
int munmap (void *addr,size t length);
```

参数：
addr：指定映射区的起始地址
length：指定映射区的长度
放回置：
成功 返回0
失败 返回1
注意：
p = 0x100;
int * p;p+1;  //0x104
char * p;p+1;  //0x101
double * p;p+1;  //0x108
short * p;p+1;  //0x102

### 3.4总结

    sbrk和brk函数都是在堆尾指针(malloc / free 也是在操作堆尾指针)
    使用sbrk 函数分配内存比较方便
    使用brk函数释放内存比较方便
    如果后续使用这两个函数从堆区分配内存，建议
    用sbrk 分配内存
    用brk释放内存

## 4分配内存

    1. 将一段 虚拟地址范围 标记为"占用"状态
    2. 将这段 虚拟内存和物理内存做映射，一次映射的最小单位是4KB
    每次映射至少4KB，如果用完了，继续4KB映射
    操作系统的功能：屏蔽硬件操作细节，让应用无需关注底层的硬件操作

## 5系统调用函数 mmap

mmap代码

```C
#include<stdio.h>
#include<sys/mman.h>
int main(void){
    //分配虚拟内存建立和物理内存的映射关系
    char*psz = mmap（NULL,//操作系统找一块空闲的虚拟内存做映射
                4096+4096,//映射区的大小为2页，8192字节
                PROT_READ | PROT_WRITE,//映射区访问权限可读可写
                MAP_PRIVATE | MAP_ANONYMOUS,//仅仅建立地址映射
                0,0);
    if(psz ==MAP_FAILED){
        perror("mmap");
        return -1;
        }
        //向缓冲区(内存)中写入字符串：sprintf
        //向映射区写入字符串
        sprintf（psz，"hello world");
        printf("%s\n",psz);
        if(munmap(psz,4096)==-1){
            perror("munmap");
            return 0;
        }
        //获取后面4096字节有效内存的首地址，保存到psz中
        psz += 4096;
        sprintf(psz,"hello,Saturday");
        printf("%s\n",psz);
        //释放剩余的4096字节，解除地址映射
        munmap(psz,4096) == 0-1){
            perror("munmap");
            return -1;
        }
        return 0;
}
```

    内存壁垒：
    0 ~ 3G-1  ： 用户空间
    3G ~ 4G-1：内核空间
    内核空间和用户空间不能够相互调用
    系统调用 - 互相访问
    Unix/Linux系统的大部分的功能都是通过系统调用来实现的
    Unix/Linux的系统调用被封装成c函数的形式，它们隶属于Linux/Unix系统，不属于标准c库函数
    这些系统调用函数也是位于库文件的：libgcc_s.so
    说明：
    man3+函数名
    man3所加函数，该函数属于c库函数
    man2+函数名
    man2所加函数，该函数属于系统调用函数
    系统调用函数层次关系：
    标准库函数大部分时间运行在用户态(用户空间)，但部分库函数偶尔会 调用系统调用，
    进入到内核态(内核空间)，
    程序员自己编写的代码可以跳过标准库，直接使用系统调用
    直接与操作系统内核交互，进入内核态
    程序：
    访问内核
    程序-库函数-系统调用-内核
    程序-系统调用-内核
    不访问内核
    程序-库函数
    系统调用在内核中实现，其外部接口定义在c库中，该接口的实现借助于软中断进入内核
    一个进程在运行中的时候，时而在用户空间，时而在内核空间，进程在这两种空间中所消耗的时候，
    分别称之为用户时间和系统时间
    通过time命令：测试进程所消耗的时间
    格式：time可执行程序的命令
    举例：time ls /  //查看ls /这个命令所消耗的时间
    real：总的执行时间
    user：用户时间
    sys  ：系统时间

# 三 文件系统

## 1 概念

    Linux理念：一切皆文件
    文件系统的分类：NTFS，FAT32，EXT2，EXT4，UTS，CRAMFDS，...
    不同的文件系统管理的方式不一样
    使用管理系统来管理文件，组织文件的流程
    硬盘：分区(windows)|分区|分区
    c盘        D盘 E盘

## 2文件系统的逻辑结构

    一个磁盘驱动器被分为多个分区
    拿出一个分区，解剖：
    分区：引导块|超级块|柱面组1|柱面组2|...|

### 概念

```
      1. 引导块：计算机通上电之后，自动运行存储在这里的代码，(BIOS,BOOTLOADER),加载和启动操作系统(操作系统自举)
      2. 超级块：记录文件系统的整体信息，例如：文件系统格式，分区大小，i节点和数据块的总量等
      3. 若干柱面组：就是一大块储存区域，将来文件的信息就是存于此处
         柱面组：超级块副本|柱面组信息| i节点映射表|块位图| i节点表|数据块集
         1. 超级块副本：就是将分区中的超级块拷贝了一份，将来通过超级块副本来检索文件可以提高效率
         2. 柱面组信息：对这个柱面组的整体描述
         3. i节点映射表：通过一个文件名称来寻找文件在软件中比较难实现，文件名就是一个字符串，字符串的长度有长有短，无形中浪费了内存，给i节点指定了一个编号(i节点：用来描述文件本身信息的，一个文件有对于的一个i节点，通过i节点来找到对应的文件)
            使用一个int类型的数据来表示这个文件对应的i节点的编号，长度是固定的，容易表示，节省内存
            每个文件对应的i节点编号简称为i节点号，通过该i节点号获取文件的信息
            命令：ls -i  //查看文件的i节点号
         4. 块位图：数据块，储存区域是由一个一个的数据块来组成，一个数据的大小为512字节/1KB
            也是一个大数组，这个数组中每个元素的每个bit位对应一个数据块，使用0/1表示该块处于空闲还是占用的状态
         5. i节点表：大数组，包含一堆i节点，每个i节点记录着文件的元数据和数据块索引表
            通过i节点号在i节点映射表中找到的就是i节点在i节点表的下标位置
            通过下标可以找到对应的i节点 - 找到文件
            i节点：元数据|数据块索引表
            元数据：描述文件的信息，文件类型，文件权限，文件的硬链接数，用户和组的信息
            三组
            用户       RW—
            组         R—
            其他人     R—
            数据块索引表：大数组，索引就是下标，就是将数据块的编号(下标)放在一起，把这些下标对应的数据块最终合并在一起组成一个完整的文件
            通过i节点的数据块索引表的数据块索引号找到对应的数据块
         6. 数据块集：数据块的集合
            数据块的大小512/1024/4096个字节，一个i节点最多包含13个数据块
            数据块分类
            直接块：储存文件的实际数据
            间接块：储存下级文件数据块索引表
```

### 文件访问流程:

文件名 - 目录 - i节点号 - i节点映射表 - i节点位置(下标) - i节点 - 数据块索引 - 数据块+数据块(文件内容)

### 文件类型：

```
普通文件
源代码文件；编译器，汇编器，链接器 - 汇编文件(.s)，目标文件(.o)，可执行文件
系统配置文件；shell脚本文件；音频/视频  数字内容的多媒体文件；数据库文件
文件的大小：最大值受限于Linux内核中用于管理文件的c语言代码数据类型的大小，某些文件系统还可能强加自己的限制，将其限定在更小的值
操作系统内核没有堆并发文件访问强加限制，不同的进程可以同时读写一个文件，并发访问的结果取决于独立操作的顺序，通常不可预测
一个文件分为两部分

1. 元数据    ——i节点
2. 内容数据 ——数据块
   目录文件
   目录本质是一个普通文件，存储文件名和i节点号的映射，每一个这样的映射，使用目录中的一个条目表示，叫做硬链接
   目录也是文件，目录文件的i节点号储存在它的父目录中
   每个目录的i节点号都是存储在其父目录中，而在根目录/是没有父目录的
   根目录的i节点号i节点表中的位置是固定的，即使没有父目录，根目录也能被正常检索
   每个目录中 . 和 ..
   . 映射该目录本身的i节点号 ..映射其父目录的i节点号
   符号链接文件
   看上去像普通文件，每个符号链接文件都有自己的i节点和包含被链接文件完整路径名的数据块
   命令：ln - s
   特殊文件
   特殊文件以文件形式表示的内核对象
   本地套接字：网络编程的基础
   字符设备：char  键盘，摄像头
   块设备：block  硬盘
   有名管道：是进程间通信的一种机制(IPC)
   通过ls -l查看文件类型
   —         普通文件
   d         目录文件
   s         本地套接字
   c         字符设备
   b         块设备
   l         符号链接
   p         有名管道
```

## 3文件的打开和关闭

open    -  打开文件

```C
#include <fcntl.h>
int open (const char *pathname,int flags,mode t mode);
参数：
    pathname：文件路径名
    flags：
        O_RDONLY    - 只读
        O_WRONLY    - 只写
        O_RDWR      - 可读可写
        O_CREAT     - 不存在就创建，已存在 打开
        O_EXCL      - 不存在就创建，已存在 报错
        O_TRUNC     - 不存在就创建，已存在 清空
        O_APPEND    - 追加
    mode：权限模式
        仅仅在创建新文件有效
        使用形如OXYZ的三位八进制表示
        X  拥有者用户权限
        Y  同组用户的权限
        Z  其他用户的权限
        读权限    4 表示
        写权限    2 表示
         执行权限  1 表示
    返回值：
        成功    返回未使用的，最小的文件描述符 - 使用文件描述符来表示该文件[0，∞(整数)]
        失败    返回-1
```

close    -  关闭文件

```C
#include <unistd.h>
int close(int fd);
参数：
    fd：open的返回值，关闭某个文件
```

代码:

```C
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
int main(void){
    int fd1 = open("open.txt",O_RDWR | O_CREAT | O_TRUNC,0666);
    if(-1 == fd1){
        perror("open");
        return -1;
    }
    printf("fd1=%d\n",fd1);
    int fd2 = open("open.txt",O_RDONLY);
    if(-1 == fd2){
        perror("open");
        return -1;
    }
    printf("fd2=%d\n",fd2);
    close(fd2);
    close(fd1);
    return 0;
}
```

```
使用open函数打开某个文件的时候，会使用文件描述符来表示某个文件
fd  --文件
i节点  --文件
文件的内核结构：
V节点和V节点表：
进程打开某个文件的时候，将文件的节点读入内存，并且辅以其他的表要信息形成一个专门的数据结构，
使用数据结构来存储这些必要信息，包含1节点信息的数据结构称为V节点
所有的V节点会以链表的形式构成V节点表
文件表项：
V节点指针 + 文件状态标志(open函数flags参数) + 文件读写位置(最后一次读写的最后一个自己的下一个位置)
由上述三项组成的新的数据结构称为文件表项
优点：通过文件表项可以记录每次读写的位置，也可以通过V节点指针访问改文件的各种元数据和节点信息
open("a.txt",...)
特性：
1.多次打开同一个文件，无论是在同一个进程个进程还是在不同的进程中，都只会在系统内核中产生一个V节点
2.每次打开文件都会产生一个新的文件表项，每一个文件表项各自维护各自的文件状态标志和文件读写位置
可能因为打开的是同一个文件而共享同一个V节点
3.打开一个文件意味着内存资源(V节点，文件表项等)的分配，而关闭一个文件就是为了释放这些资源
但是如果要关闭的文件处于打开状态（其他进程也在使用），此时其V节点不会被释放，仅仅使用该进程独有的文件表项被释放的文件表项被释放，直到系统中打开该文件的进程都显式或者隐式的将其关闭，此时v节点才会被真正释放
4.一个处于打开状态的文件也可以被删除，但其所占用的磁盘空间直到其V节点彻底消失后才会被标记为自由
进程a:open("b.txt",...)
进程b:open("b.txt",...)
进程b关闭了文件，b.txt对应的v节点不会被释放，释放的仅仅是文件表项
文件描述符：
由文件的内核结构可知，一个被打开的文件在系统内核中通过文件表项和V节点加以表识
有关该文件的所以操作，都需要依赖于文件表项和V节点
需要将文件表项和V节点体现在完成这些后续操作的函数的参数中
内核空间的内存地址会暴露给运行与用户空间的程序代码
一旦某个用户进程出现操作失误，极有可能造成系统内核失稳，进而影响其他用户进程的运行，对操作系统的稳定性造成极大的威胁
解决内核对象在可访问性和安全性之间的矛盾，内核会维护一张存有各进程信息的列表，称为进程表
系统中的每个进程在进程表中都有一个表项，每个表项中包含了对于特定进程的描述信息，例如 进程id 线程id 组id...，其中也包含了一个被称为文件描述符表的数据结构
作为文件描述表项在文件描述符表的下标，称为文件描述符
合法的文件描述符一定是一个大于0的整数
文件描述符0 1 2 3都是处于使用状态
使用进程打开一个文件 - 分配文件描述符为4
每次一个新的文件描述符表项，系统总是从下标0开始在文件描述符表中寻找最小的未使用项
每关闭一个文件描述符，无论其被索引的文件表项和V节点是否被删除，与之对应的文件描述符表项一定会被标记为未使用，并且在后续操作中为新的文件描述符所占用
系统内核缺省(默认)为每个进程打开了三个文件描述符，它们在unistd.h头文件中被定义为三个宏
#define STDIN_FILENO        0//标准输入
int val = 0;
scanf("%d",&val);
STDIN_FILENO - 0 - 文件表项指针 - 文件表项 - v节点 - i节点 - 标准输入文件 - 键盘
#define STDOUT_FILENO       1//标准输出
printf("hello,world\n");
STDOUT_FILENO - 1 - 文件表项指针 - 文件表项 - v节点 - i节点 - 标准输出文件 - 显示器
#define STDERR_FILENO       2//标准错误输出
malloc(0xfffffffff); ->分配0xffffffffff字符 -> 无法分配 -> 报错
STDERR_FILENO - 2 - 件表项指针 - 文件表项 - v节点 - i节点 - 标准错误
输出文件 - 显示器
系统内核缺省(默认)为每个进程打开了三个文件描述符,它们在 unistd.h 头文件中被定义为了三个宏
#define STDIN FILENO 	0 //标准输入
#define STDOUT FILENO	1 //标准输出
#define STDERR FTLENO	2 //标准错误输出
打开a.txt文件,分配一个新的文件描述符表项,分配的下标为最小的未使用项，分配的文件描述符为3 - 目前的最小的未使用项
打开b.txt文件,分配一个新的文件描述符表项,分配的下标为最小的未使用项，分配的文件描述符为4 - 目前的最小的未使用项
关闭文件描述符0，打开i.txt，该文件对应的文件描述符为0
    1.关闭文件描述符0-文件描述符0处于空闲/未使用状态
    2.打开1.txt,分配最小的未使用的文件描述符-
```

```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
void redir(void){
    close(STDIN_FILENO);//close(0)
    open("i.txt",O_RDINLY);//0 -> i.txt
    close(STDOUT_FILENO);//close(1)
    creat("o.txt",0644);//1 -> o.txt
    close(STDERR_FILENO);//close(2)
    creat("e.txt",0644);//2 -> e.txt
}
void doio(void){
    int x,y;
    //标准输入
    scanf("%d%d",&x,&y);
    //标准输出
    printf("%d+%d=%d\n",x,y,x+y);
    //标准错误输出
    malloc(0XFFFFFFF);
    perror("malloc");
}
int main(void){
    redir();
    doio();
    return 0;
}
```

    分析
    int creat (const char *pathname,mode t mode);
    open(pathname,O_WRONLY | O_CREAT | O_TRUNC,mode);
    creat("o.txt",0644)
    open("o.txt",O_WRONLY | O_CREAT | O_TRUNC,0644);

## 文件的读写

```c
#include<unistd.h>
ssize_t write(int fd,const void *buf,size_t count);
    功能：向指定文件写入数据
    参数：
        fd：文件描述符
        buf：内存缓冲区,要写入的数据
        count：期望写入的字节数
    返回值：
        成功    返回实际写入的字节数
        失败    返回-1
int fd=open("a.txt");
const char* text = "hello,world";
write(fd,text,100);
```

代码:

```c
//write.c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(void){
    // == int fd = creat("write.txt",0644)
    int fd = open("write.txt",O_WRONLY | O_CREAT | O_TRUNC,0644);
    if(-1==fd){
        perror("open");
        return -1;
    }
    const char* text="helle,world";
    printf("要写入内容：%s\n",text);
    size_t towrite = strlen(text) * sizeof(text[0]);
    size_t written = write(fd,text,towrite);
    if(-1 == written){
        perror("write");
        return -1;
    }
    printf("期望写入%u个字节，实际写入%d个字节\n",towrite,written);
    write(fd,text,);
    close(fd);
    return 0;
}
```

    向xx.txt文件写入非字符串类型内容
    char name[128]="James";
    unsigned int age=35;
    double salary=2000000;
        方法：将非字符串类型内容 - 转换为字符串类型
    printf("%s %u %.2lf\n",name,age,salary);
        将这些非字符串类型内容输出到屏幕上
        - 前提：已经将这些内容转换为字符串
    将这些内容转化为字符串，然后输出到字符数组
    char buf[1024];
    sprintf(buf , "%s %u %.2lf\n",name,age,salary);
        s=string

代码:

```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(void){
    // == int fd = creat("write.txt",0644)
    int fd = open("write.txt",O_WRONLY | O_CREAT | O_TRUNC,0644);
    if(-1==fd){
        perror("open");
        return -1;
    }
    const char* text="helle,world";
    printf("要写入内容：%s\n",text);
    size_t towrite = strlen(text) * sizeof(text[0]);
    char name[128]="James";
    unsigned int age=30;
    double salary=200000;
    printf("%s%u%.2lf\n",name,age,salary);
    if(write(fd.buf,strlen(buf)*sizeof(buf[0]))==-1){
        perror("write");
        return -1;
    }
    size_t written = write(fd,text,towrite);
    if(-1 == written){
        perror("write");
        return -1;
    }
    printf("期望写入%u个字节，实际写入%d个字节\n",towrite,written);
    write(fd,text,);
    close(fd);
    return 0;
}
```

```
读取-read
#include <unistd.h>
    ssize t read(int fd,void *buf,size t count);
	    功能：从文件中读取内容
        参数：
           fd：文件描述符，表示从哪个文件中读取数据
           buf：存储读取数据的内存缓冲区首地址
           count:期望读取的字节数
       返回值：
           成功
               返回实际读取的字节数
               返回0，读取到了文件尾部
           失败返回-1
```




代码:

```c
//read.c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(void){
    int fd = open("read.txt",O_RDONLY);
    if(-1 == fd){
        perror("open");
        return -1;
    }
    char text[256];
    size_t toread=sizeof(text)-sizeof(text[0]);
    ssize_t readed=read(fd,text,toread);
    printf("期望读取%u字节，实际读取%d字节\n",toread,readed);
    text[readed/sizeof(text[0])]='\0';
    printf("读取内容为%s\n",text)
    close(fd);
    return 0;
}
```

    writei和read都是基于系统调用的文件读写，是面向于二进制字节流的
    对二进制文件读写,无需做额外的工作
    对文本文件读写，需要按照特定的格式，将二进制形式的数据转换为文本字符串

代码:

```c
//binary.c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
int main(void){
    int fd = open("binary.dat", O_WRONLY|O_CREAT|O_TRUNC, 0644);
    if(-1 == fd){
        perror("open");
        return -1;
    }
    char name[128] = "James";
    if(write(fd, name, sizeof(name)) == -1){
        perror("write");
        return -1;
    }
    unsigned int age = 30;
    if(write(fd, &age, sizeof(age)) == -1){
        perror("write");
        return -1;
    }
    double salary = 200000;
    if(write(fd, &salary, sizeof(salary)) == -1){
        perror("write");
        return -1;
    }
    close(fd);
    if((fd = open("binary.dat",  O_RDONLY)) == -1){
        perror("open");
        return -1;
    }
    if(read(fd, name, sizeof(name)) == -1){
        perror("read");
        return -1;
    }
    printf("name:%s\n", name);
    if(read(fd, &age, sizeof(age)) == -1){
        perror("read");
        return -1;
    }
    printf("age:%u\n", age);
    if(read(fd, &salary, sizeof(salary)) == -1){
        perror("read");
        return -1;
    }
    printf("salary:%.2lf\n", salary);
    close(fd);
    return 0;
}
```

## 顺序读写和随机读写

````
lseek - 调整文件读写位置
每个打开的文件都有一个与其相关的文件读写位置保存在文件表项中，用以记录从文件头开始计算的字节便宜
	文件读写位置往往是一个非负的整数，
	使用off_t类型表示
		32位系统，long int
		64位系统，long long int

每一次读写操作都是从当前的文件读写位置开始，且根据所读写的字节数，同步增加文件读写位置，为下一次读写做准备

打开一个文件的时候，设置O_APPEND标志，否则文件的读写位置一律被设为0，也就是文件首字节的位置

设置文件都读写位置

```c
#include <sys/types.h>
#include <unistd.h>
off_t lseek(int fd,off_t offset,int whence);
```

功能：认为调整文件读写位置
参数：
	fd：文件描述符
	offset：文件读写位置便宜字数
			0 返回当前读写位置
			正数 向后偏移字节数
			负数 向前便宜字节数
	whence:offset参数的扁移延点，可以取以下三个值
		SEEK_SET-从文件头开始
		SEEK_CUR-从当前位置开始,最后被读写位置的下一个字节
		SEEK_END-从文件尾开始
返回值：
	成功	返回-1
	失败	返回调整后的文件读写位置

int fd=open(“hello.txt”);

	0	1	2	3	4	5	6	7	8	9	10

	^

默认在0位置上，文件的起始位置

read(fd,buf,3);

	0	1	2	3	4	5	6	7	8	9	10

---------------------------------------

				   ^
					此时读写位置变了3个位置

read(fd,buf,1);

	0	1	2	3	4	5	6	7	8	9	10

---------------------------------------

				   	 ^
						此时读写位置变了3个位置

lseek(fd,-4,SEEK_CUR);//文件读写位置从当前位置开始向左(文件头部)偏移4字节

	0	1	2	3	4	5	6	7	8	9	10

---------------------------------------

	^

lseek(fd,5,SEEK_SET);//从文件开始向右移动5个字节

	0	1	2	3	4	5	6	7	8	9	10

---------------------------------------

				   	  	^

lseek(fd,8,SEEK_CUR);

	0	1	2	3	4	5	6	7	8	9	10

---------------------------------------

				   	  							  				 ^
				可以通过lseek函数将文件读写位置设置到文件尾之后，在超过文件尾的位置上写入数据，将在文件中形成空洞，位于文件中但没有被写过的字节都被设置为0，文件空洞不会占用磁盘空间，但是会被计算在文件大小之内

write(fd,”789”,3);

	0	1	2	3	4	5	6	7	8	9	10	\0	\0	7	8	9	

---------------------------------------

				   	  											   			   ^

lseed(fd,-7,SEEK_END);

	0	1	2	3	4	5	6	7	8	9	10	\0	\0	7	8	9	

---------------------------------------

				   	  						^
												 从文件末尾向前移动7个字节，到了9这个位置上
````

代码:
seek.c

```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(void){
    int fd = open("seek.txt",O_RDWR|O_CREAT|O_TRUNC,0644);
    if(-1 == fd){
        perror("open");
        return -1;
    }
    const char* text = "hello,world";
    if(write(fd,text,strlen(text)*sizeof(text[0]))==-1){
        perror("write");
        return -1;
    }
    if(lseek(fd,-6,SEEK_CUR)==-1){
        perror("lseek");
        return -1;
    }
    off_t pos = lseek(fd,0,SEEK_CUR);
    if(-1 == pos){
        perror("lseek");
        return -1;
    }
    printf("当前读写位置%d\n",pos);
    text = "Linux";
    if(write(fd,text,strlen(text)*sizeof(text[0]))==-1){
        perror("write");
        return -1;
    }
    if(lseek(fd,8,SEEK_END)==-1){
        perror("lseek");
        return -1;
    }
    text="这里有一圈洞洞"
        if(write(fd,text,strlen(text)*sizeof(text[0]))==-1){
        perror("write");
        return -1;
    }
    off_t size = lseek(fd,0,SEEK_END);
    if(-1 == size){
        perror("lseek");
        return -1;
    }
    printf("文件大小:%ld字节\n", size);
    close(fd);
    return 0;
}
```

```
h	e	l	l	o	,	w	o	r	l	d
													   ^

h	e	l	l	o	,	w	o	r	l	d
							  ^

h	e	l	l	o	,	L	i	n	u	x
							  					^

off_t pos = lseek(fd, 0,SEEK_END);
	pos 意思 文件大小

文件的读写位置不能位于文件头之前
—lseek(fd,-8,SEEK_SET);—

系统I/O和标准I/O
系统调用提供的文件I/O函数
	open/close read /write/lseek ..
标准C语言库提供了文件I/O函数：
	fopen/fclose/fread/fwrite/...

当系统调用函数被执行的时候，需要在用户态和内核态之间来回切换，频繁执行影响程序的运行性能
标准库函数做了必要的优化，内核会维护一个缓冲区，只有在满足特定条件的时候才将缓冲区和系统内核同步
	减少进程在用户态和内核态之间来回切换的次数，提高运行性能
```

代码:
sysio.c

```c
#include<stdio.h>
#include<unistd.h>
#incluse<fcntl.h>
int main(void){
    int fd = open("sysio.dat",O_WRONLY|O_CREAT|O_TRUNC,0644);
    if(-1 == fd){
       perror("open");
           return -1;
    }
    unsigned int i;
    for(i=0;i<1000000,i++){
        write(fd,&i,sizeof(i));
    }
    close(fd);
    re
}
```

## 文件描述符的复制

```c
include<unistd.h>
int dup(int oldfd);
```

```
功能：复制文件描述符表的特定条目到最小可用项
参数：
	oldfd:源文件描述符
返回值：
	失败 返回-1
	成功 返回目标文件描述符
int oldfd open ("a.txt");
int newfd=dup(oldfd);//在文件描述符表找最小的未使用的文件描述符分配，存储的是oldfd标识的文件描述符表项
					 //最终newfd和oldfd都会指向同一个文件表项，最终指向同一个文件
```

代码：
dup.c

```c
#include<sudio.h>
#include<fcntl.h>
#include<unistd.h>
#include<string.h>
int mian(void){
    int fd1=open("dup.txt",O_RDWR|O_CREAT|O_TRUNC,0644);
    printf("fd1=%d\n",fd1);
    int fd2=dup(fd1);
    printf("fd2=%d\n",fd2);
    int fd3=dup(fd2);
    const char* text="hello,world!";
    write(fd2,text,strlen(text)*sizeof(text[0]));
    return 0;
}
```

```c 
int dup2(int oldfd,int newfd);
```

```
功能：复制文件描述符表的特定条目到指定项
参数:
	oldfd:源文件描述符
	newfd:目标文件描述符
返回值:
	失败返回-1
	成功返回目标文件描述符(newfd)
dup2函数在复制oldfd参数所标识的源文件描述符表项时，会检查由newfd	参数所标识的目标文件描述符表项
	是否空闲.
		若空闲则直接将前者复制给后者
		若非空闲，先将目标文件描述符newfd关闭，使之成为空闲项，再行复制
int fdl open ("a.txt");
dup2(fd1,2);//将fd1的文件表项复制一份到文件描述符2对应的位置上
```

代码：
dup2.c

```c
#include<sudio.h>
#include<fcntl.h>
#include<unistd.h>
#include<string.h>
int mian(void){
    int fd1=open("dup.txt",O_RDWR|O_CREAT|O_TRUNC,0644);
    printf("fd1=%d\n",fd1);
    int fd2=dup(fd1);
    printf("fd2=%d\n",fd2);
    int fd3=dup(fd2);
    printf("fd3=%d\n",fd3);
    const char* text="hello,world!";
    write(fd2,text,strlen(text)*sizeof(text[0]));
    off_t pos=lseek(fd2,-6,SEEK_CUR);
    printf("%ld\n",pos);
    text="linux";
    fprintf(stderr,"%s",text);
    return 0;
}
```

```
不管使用的是dup函数，还是dup2函数，一样的是
	oldfdi和newfd二者共享同一个文件表项-二者共享同一个读写位置
打开一个文件
	fd-文件表项指针-文件表项-v节点指针-v节点-v/i节点信息-文件

多次打开同一个文件，文件表项各自独立，只是v节点共享
	不同的文件描述符标识的文件读写位置各自独立
```


代码：
sane.c

```c
#include<unistd.h>
#include<string.h>
int mian(void){
    int fd1=open("same.txt",O_RDWR|O_CREAT|O_TRUNC,0644);
    printf("fd1=%d\n",fd1);
    int fd2=open("same.txt",O_RDWR);
    printf("fd2=%d\n",fd2);
    int fd3=open("same.txt",O_RDWR);
    printf("fd3=%d\n",fd3);
    const char* text="hello,world";
    write(fd1,text,strlen(text)*sizeof(text[0]));
    off_t pos=lseek(fd2,-6,SEEK_CUR);
    printf("pos=%ld\n",pos);
    text="linux";
    write(fd3,text,strlen(text)*sizeof(text[0]));
    return 0;
}
```

文件控制
文件描述符的复制，fcntl

```c
#inlcude<fcntl.h>
int fcntl(int oldfd,F_DUPFD,int new fd);
```

功能：复制文件描述符
参数：
	oldfd:源文件描述符
	newfd:目标文件描述符
返回值：
	失败  返回-1
	成功  返回目标文件描述符

int fd =open(“hello.txt”);

代码：
fcntl.c

```c
#include<stdio.h>
#include<unistd.h>
#include<string.h>
#include<fcntl.h>
int main(void){
    int fd1 open("fcntl.txt",0 RDWR|O CREAT|O TRUNC,0644);
	printf("fd1=%d\n",fd1);//3
	int fd2 dup(fdl);
	printf ("fd2=%d\n",fd2);//4
	//fcntl:文件描述符的复制
	int fd3 fcntl(fd2,F DUPFD,2);
	printf ("fd3=%d\n",fd3);//5
	const char*text="Hello,world";
    write(fd1,text,strlen(text)*sizeof(text[0]));
    off_t pos=lseek(fd2,-6,SEEK_CUR);
    printf("pos=%ld\n",pos);
    text="linux";
    write(fd3,text,strlen(text)*sizeof(text[0]));
	return 0;
}
```

fcntl(fd,F_DUPFD,2);dup2区别
dup2(fd,2);//将fd的文件表项复制一份给到文件描述符2的位置上
				如果2空闲，将d的文件表项复制过去
				如果2不空闲，也会将fd的文件表项复制过去

fcntl.c(fd,F_DUPFD,2);//将f的文件表项复制一份给到文件描述符2的位置上
							如果2空闲，将fd的文件表项复制过去
							如果2不空闲，不会将原有的文件描述符关闭，而是找另一个最小的空闲的文件描述符作为复制目标

fd1=open(“fcntl.txt”);
fd2=dup(fd1);

const char*text="Hello,world";
write(fd1,text,strlen(text)*sizeof(text[0]));
off_t pos=lseek(fd2,-6,SEEK_CUR);
	fd2的读写位置 - w位置上
	fd3的读写位置 - w位置上

text=“linux”;
write(fd3 ,text,strlen(text))

fd1 3 \
fd2 4 ——文件表项指针 - 文件表项（读写位置，v节点指针） - v节点 - i节点 - 文件
fd3 5 /

文件锁

读写冲突
-写和写->有冲突
如果两个或两个以上的进程同时向同一个文件的某个特定区域写入数据，最终写入文件的数据极有可能因为写操作的交错而产生混乱

读和写->有冲突
如果一个进程写而其他进程同时在读一个文件的某个特定区域，那么读出的数据有可能因为读写操作的交错而不完成

-读和读->没冲突
多个进程同时读取一个文件的某个特定区域，不会有任何问题，它们只是各自把文件中的数据拷贝到各自的缓冲区中，并不会改变文件的内容，相互之间就不会有冲突

结论
为了避免读写冲突
	如果一个进程正在写，那么其他的进程不能读也不能写
	如果一个进程正在读，那么其他的进程不能写，但是可以读

为了避免多个进程在读写同一个文件的同一个区域发生冲突，Unix/Linux系统引入了文件锁机制，分为读锁和写锁
区别：
	对一个文件的特定区域可以加多把读锁
	对一个文件的特定区域只能加一把写锁

基于锁的操作模型
读/写文件中的特定区域之前，先加读/写锁，锁成功了再读/写，读/写以后再解锁

读锁：
要读取内容，加一把锁，不要别人打扰
写锁：要写入内容，加一把锁，不要别人打扰

多把读锁：
有多个进程在读取文件内容，在读取的时候加上了锁
加读锁：
新的进程要读取内容，并且加锁ok
加写锁：新的进程要写入内容，并且加锁no

读取内容 :
加读锁 
读取 
解读锁 

写入内容 :
加写锁 
写入 
解写锁 

进程A正在写, 进程B也想写 
     进程A                   进程B 
  打开文件,写A区        打开文件, 准备写B区
  给A区加写锁, 成功          
  写入A区             给B区加写锁, 失败, 阻塞
  写完, 解锁A区       从阻塞中恢复, B区被加上写锁
                               写B区 
                            写完,解锁B区
  关闭文件                     关闭文件 

进程A正在写, 进程B想读
      进程A                 进程B 
  打开文件,写A区       打开文件, 准备读B区
  给A区加写锁, 成功           
  写入A区             给B区加读锁, 失败, 阻塞
  写完, 解锁A区        从阻塞中恢复, B区被加上读锁
                            读B区 
                         读完,解锁B区
  关闭文件                  关闭文件 

进程A正在读，进程B想写
进程A					进程B
打开文件，准备读A区     打开文件，准备写B区
给A区加读锁，成功
读A区                  给B区加写锁，失败，阻塞
读完解锁A区            从阻塞中恢复，B区被加上写锁
						 写B区
						写完，解锁B区
关闭文件				 关闭文件
进程A正在读，进程B想读
进程A					进程B
打开文件，准备读A区	打开文件，准备读B区
给A区加读锁，成功
读A区				  给B区读完解锁A区
读完解锁A区				读B区
						读完解锁B区
关闭文件				  关闭文件

加锁和解锁

```c
#include<unistd.h>
#include<fcntl.h>
int fcntl(int fd,int cmd,struct flock* lock);
```

功能：对某一文件或者某个特点区域加和解锁
参数：
fd：文件描述符
cmd：锁模式
	F_SETLKW - 阻塞模式
		加不上锁不返回，什么时候加上什么时候返回
	F_SETLK - 非阻塞模式
		成功加锁返回0，加不上锁返回-1
		errno == EAGAIN
lock：锁的属性
	struct flock{
	short 1 type;
	short 1 whence;
	off t l start;
	off t l len;
	pid t 1 pid;
	}；
	1_type：锁操作类型
		F_RDLCK:加读锁
		F_WRLCK:加写锁
		F_UNLCK:解锁
	1_whence：锁区偏移起点
		SEEK_SET
		SEEK_CUR
		SEEK_END
	1_start - 相对于1_whence的锁区偏移
	1_len - length - 锁区字节数，0表示锁到文件尾
	1_pid - 加锁进程标识，加解锁置-1

相对文件头10字节开始的20字节以阻塞模式加读锁 
struct flock lock;
lock.l_type = F_RDLCK;
lock.l_whence = SEEK_SET;
lock.l_start = 10;
lock.l_len = 20;
lock.l_pid = -1;
fcntl(fd, F_SETLKW, &lock);

相对当前位置10字节开始到文件尾以非阻塞模式加写锁
struct flock lock;
lock.1_type = F_WRLCK;
lock.l_whence = SEEK_CUR;
lock.l_start = 10;
lock.l_len = 0;
lock.l_pid = -1;
fcntl(fd, F_SETLK, &lock);

对整个文件解锁
struct flock lock;
lock.1_type = F_UNLCK;
lock.1_whence = SEEK_SET;
lock.l_start = 0;
lock.l_len = 0;
lock.l_pid = -1;
fcntl(fd, F_SETLK, &lock);

代码：
wlock.c

```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
#include<errno.h>
//加写锁
int wlock(int fd ,int wait){
    struct flock lock;
    lock.l_type = F_WRLCK;
    lock.l_whence = SEEK_SET;
    lock.l_start = 0;
    lock.l_len = 0;
    lock.l_pid = -1;
    return fcntl(fd, wait?F_SETLKW:F_SETLK, &lock);
}
//解锁
int ulock(int fd){
    struct flock lock;
    lock.l_type = F_UNLCK;
    lock.l_whence = SEEK_SET;
    lock.l_start = 0;
    lock.l_len = 0;
    lock.l_pid = -1;
    return fcntl(fd, F_SETLK, &lock);
}
int main(int argc,char** argv){
    if(argc<2){
        fprintf(stderr,"用法：%s<字符串>\n",argv[0]);
        return -1;
    }
    int fd = open("shared.txt", O_WRONLY|O_CREAT|O_APPEND,0644);
    if(-1 == fd){
        perror("open");
        return -1;
    }
    //阻塞模式加写锁
    if(wlock(fd, 1) == -1){
        perror("wlock");
        return -1;
    }
    //以非阻塞的模式加写锁
    while(wlock(fd, 0) == -1){
        if(errno != EACCES && errno != EAGAIN){
            perror("wlock");
            return -1;
        }
        printf("该文件已经被锁定，稍后重试...\n");
        //空闲处理
    }
    //写入数据
    size_t i, len=strlen(argv[1]);
    for(i=0; i<len; ++i){
        if(write(fd, &argv[1][i], sizeof(argv[1][i])) == -1){
            perror("write");
            return -1
        }
        sleep(1);
    }
    //解锁
    if(ulock(fd) == -1){
        perror("ulock");
        return -1;
    }
    close(fd);
    return 0;
}
```

加读锁 - 读 - 此时，再加读锁，ok - 其他进程再读
加读锁 - 读 - 此时，再加写锁，no - 其他进程要写
加写锁 - 写 - 此时，再加读锁，no - 其他进程要读
加写锁 - 写 - 此时，再加写锁，no - 其他进程要写

读取内容
加读锁
读取
解读锁

写入内容
加写锁
写入
解写锁

代码：
给文件加读锁
rlock.c

```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
#include<errno.h>
//加读锁
//wait=1,阻塞
//wait=0.非阻塞
int rlock(int fd ,int wait){
    struct flock lock;
    lock.l_type = F_RDLCK;
    lock.l_whence = SEEK_SET;
    lock.l_start = 0;
    lock.l_len = 0;
    lock.l_pid = -1;
    return fcntl(fd, wait?F_SETLKW:F_SETLK, &lock);
}
//解锁
int ulock(int fd){
    struct flock lock;
    lock.l_type = F_UNLCK;
    lock.l_whence = SEEK_SET;
    lock.l_start = 0;
    lock.l_len = 0;
    lock.l_pid = -1;
    return fcntl(fd, F_SETLK, &lock);
}
int main(int argc,char** argv){
    int fd = open("shared.txt", O_RDONLY);
    if(-1 == fd){
        perror("open");
        return -1;
    }
    //加读锁
    if(rlock(fd, 1) == -1){
        perror("rlock");
        retrun -1;
    }
    //读取内容
    /*char buf[1024];
    ssize_t readed;
    while(readed = read(fd ,buf, sizeof(buf)) > 0)
        write(STDOUT_FILENO, buf, readed);
    printf("\n");*/
    char buf[1];
    ssize_t readed;
    while((readed = read(fd, buf, sizeof(buf))) > 0){
        write(STDOUT_FILENO, buf, readed);
        sleep(1);
    }
    if(-1 == readed){
        perror("read");
        return -1;
    }
    //解锁
    if(ulock(fd) == -1){
        perror("ulock");
        return -1;
    }
    close(fd);
    return 0;
}
```

读取文件读到文件末尾的时候，返回值为0

write(STDOUT FILENO,buf,readed);
	STDOUT FILENO:标准输出文件描述符
	buf:要输出的缓冲区首地址
	readed:要输出的字节个数

想法：将文件中的内容，一个字符一个字符的读出来

当一个文件加上写锁，写入内容，
	如果此时要去读取文件内容的时候，无法读取该文件的内容
	等到其写完内容，解锁，然后才能读取内容

当一个文件加上读锁，读取内容
	如果此时要去向文件中写入内容，无法向文件中写入内容
	等到其读完内容，解锁，然后才能读取内容

当一个文件加上读锁，读取内容
	如果此时要去读取文件内容，可以读取文件的内容
	同时加读锁，读取，二者是不冲突的

总结：
当通过c1os函数关闭文件描述符的时候，调用进程（程序）在该文件描述符上添加的一切锁都将被自动解除
当程序（进程）终止的时候，该进程在所有文件描述符上添加的一切锁都将会自动解除
文件锁仅仅在不同进程之间起作用，同一个进程的不同线程不能通过文件描述符来解决读写冲突问题

为何文件锁可以避免读写冲突
关键，参与读写的多个进程都会遵循一套标准：先加锁，再读写，再解锁-形成了一套协议
只要参与者都遵循这套协议，读写就是安全的.
反观之，如果哪个进程不遵循这套协议，完全无视锁的存在，想读就读，想写就写
	就算是有锁，没有任何的约束作用
锁的机制又称为劝谏锁或者协议锁

每次给文件的特定区域加锁，都会通过fcntl函数向系统内核传递flock结构体，该结构体中包含了有关锁的一切细节
系统内核会手机所有进程对该文件所加的所有锁，并且把这些f1ock结构体中的信息，以链表的形式组织成一张锁表
此时任何一个进程想要通过fct1函数对该文件加锁，系统内核都会遍历这张锁表，一旦发现有和想要添加的锁沟通冲突的锁即阻塞或者报错
	否则就将锁添加到锁表，而解锁的过程就是删除锁表中的相应节点
锁表的起始地址保存在该文件的v节点中

已经添加写锁：
wlock(fd,1); -> 写锁

想要添加读锁：
	rlock(fd,1); -> 想要添加读锁 - 系统内核会遍历该文件的锁表 - 发现已经有进程给该文件添加了一般写锁 - 和想要添加的读锁 - 冲突 - 阻塞 - 或者 - 报错

锁表：一个一个的锁节点
	struct flock
		short 1 type;
		short 1 whence;
		off t l start;
		off t l len;
		pid t 1 pid;
		struct flock*pnext;
	}；

添加的三把锁
struct flock fl;
	f1.pnext &f2;
struct flock f2;
	f2.pnext &f3;
struct flock f3;
	f3.pnext NULL;

[…|pnext=&f2]  […|pnext=&f3]  […|pnext=NULL]
	  f1			 f2			 f3
	  f1 -> pnext -> f2 -> pnext -> f3

访问测试
\#include<unistd.h>
int access{const char *pathname,int mode};
功能：判断当前程序是否可以对某个特定的文件执行某种操作
参数：
pathname:path 路径 路径名，文件的路径
	要给哪个文件判断某种操作
mode：对于某个文件的特定操作
	R_OK 是否可读
	W_OK 是否可写
	X_OK 是否可执行
	F_OK 是否存在
返回值
成功 返回0
失败 返回-1

代码：
access.c

```c
#include<stdio.h>
#include<unistd.h>
int main(int argc, char** argv){
    if(argc<2){
        fprintf(stderr,"usage:%s<文件名>\n",argv[0]);
    }
    printf("访问测试的文件为：%s\n",argv[1]);
    if(access(argv[1],F_OK)==1){
        printf("不存在");
    }else{
        if(access(argv[1],R_OK) == -1)
            printf("不可读\n");
        else
            printf("可读\n");
        if(access(argv[1],W_OK) == -1)
            printf("不可写\n");
        else
            printf("可写\n");
        if(access(argv[1],X_OK) == -1)
            printf("不可执行\n");
        else
            printf("可执行\n");
    }
    return 0;
}
```

权限掩码
一个文件的权限
三个部分
			r	w	x
个人用户	 1	1	0	6
组用户	   1	0	0	4
其他用户	 1	0	0	4
八进制的方式来表示权限：0644

权限掩码：默认0002
文件的权限：给定的权限&~权限掩码

\#include<sys/types.h>
\#include<sys/stat.h>
mode_t umask(mode_t mask);
功能：设置调用程序的权限掩码
参数：
mask：新的权限掩码
返回值：
返回原来的权限掩码
代码：
umask.c

```c
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/stat.h>
int main(void){
    mode_t old = umask(0333);
    int fd = open("./umask.txt",O_RDONLY|O_CREAT|O_TRUNC,0777);
    if(-1 == fd){
        perror("open");
        return -1;
    }
    close(fd);
    umask(old);
    return 0;
}
```

修改文件大小
truncate
\#include<unistd.h>
\#include<sys/types.h>
int truncate(const char *path,off_t length);
功能：设置文件的大小
参数：
path：文件路径
length：文件大小
返回值：
成功 返回0
失败 返回-1

int fturncate(int fd, off_t length);
功能：设置文件的大小
参数：
fd:文件描述符
length：文件大小
返回值：
成功 返回0
失败 返回-1

代码：
trunc.c

```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<sys/types.h>
int main(void){
    int fd = open("./truncate.txt",O_WRONLY|O_CREAT|O_TRUNC,0644);
    if(-1 == fd){
        perror("open");
        return -1;
    }
    char* buf = "hello world";
    if(write(fd,buf,strlen(buf)) == -1){
        perror("write");
        return -1;
    }
    if(truncate("./truncate.txt",3) == -1){
        perror("truncate");
        return -1;
    }
    if(ftruncate(fd, 5) == -1){
        perror("ftruncate");
        return -1;
    }
    close(fd);
    return 0;
}
```

该函数可以将文件截断，也可以将文件加长，所有的变化都发生在文件的尾部
	新增加的部分使用数字0填充

文件的元数据 - stat
文件的元数据就是文件的属性信息 - 存在于i节点中
\#include<sys/types.h>
\#include<sys/stat.h>
\#include<unistd.h>
int stat(const char *pathname, struct stat *buf);
int fstat(int fd, struct stat *buf);
int lstat(const char *pathname, struct stat *buf);
功能：从i节点中提取文件的元数据
参数：
pathname：文件路径
fd：文件描述符
buf：文件元数据结构
返回值：
成功 返回0
失败 返回-

lstat：link stat遇到软链接文件
	lstat不跟踪符号链接

struct stat{
		dev_t   st_dev;     	 /\* 设备ID */
        ino_t   st_ino;     	 /\* i节点号 */
        mode_t  st_mode;    	 /\* 文件的类型和权限 */
        nlink_t  st_nlink;       /\* 硬链接数 */
        uid_t   st_uid;     	 /\* 拥有者用户ID */
        gid_t   st_gid;     	 /\* 拥有者组ID */
        dev_t   st_rdev;    	 /\* 特殊设备ID */
        off_t   st_size;    	 /\* 总字节数 */
        blksize_t st_blksize;    /\* I/O块字节数 */
        blkcnt_t st_blocks;      /\* 存储块数 */
        struct timespec st_atim; /\* 最后访问时间 */
        struct timespec st_mtim; /\* 最后修改时间 */
        struct timespec st_ctim; /\* 最后状态改变时间 */
}

一块为512字节
mode t	st mode;	/*文件的类型和权限*/
数据类型：mode_t 原始类型 unsigned int(32位系统下)
	32位的无符号整数
	只有其中的低16位是有意义的

B15-B0
15-12:文件类型
11-9:1设置用户ID,组ID,粘滞
8-6:个人用户的读，写和执行权限
5-3:组用户的读，写和执行权限
2-0:其他用户的读，写和执行权限

11-9:
程序运行-进程
两个用户ID-实际用户ID-有效用户ID
	实际用户ID-继承其父进程的实际用户ID-当一个用户使用合法的用户名和密码登录，
		系统为其启动一个shel1进程，shel1的进程的实际用户的ID就是该登录用户的ID,
		该用户在shel1下启动的任何进程都是shel1的子进程，继承了实际用户ID
	一个进程的用户身份-决定-可以访问哪些资源-文件 读/写/执行权限
		但真正权限验证的是有效用户ID,而非实际用户ID
	有效用户ID：一般情况下，有效用户ID取自于实际用户ID,认为二者等价
		但是如果自己设置用户ID位，B11 - 1，该进程的有效用户ID就不是
	粘滞：带有粘滞位(9)的可执行文件，与其首次运行并结束后，代码区会被连续的保存在磁盘交换区中，而一般的磁盘文件都是你上存放的，下次运行该程序的时候可以获取较快的载入速度

S_ISREG()	是否普通文件
S_ISDIR()	是否目录文件
S_ISSOCK()   是否本地套接字
S_ISCHR()	是否字符设备
S_ISBLK()	是否块设备
S_ISLNK()	是否符号链接
S_ISFIFO()   是否有名管道

```c
struct tm *localtime(const time_t *timep);
struct tm {
               int tm_sec;    /* Seconds (0-60) */
               int tm_min;    /* Minutes (0-59) */
               int tm_hour;   /* Hours (0-23) */
               int tm_mday;   /* Day of the month (1-31) */
               int tm_mon;    /* Month (0-11)  */
               int tm_year;   /* Year - 1900 */
               int tm_wday;   /* Day of the week (0-6, Sunday = 0) */
               int tm_yday;   /* Day in the year (0-365, 1 Jan = 0) */
               int tm_isdst;  /* Daylight saving time */
           };
```

代码：
stat.c

```c
#include<stdio.h>
#include<string.h>
#include<time.h>
#include<unistd.h>
#include<sys/stat.h>
char* mtos(mode_t m){
    static char s[11];
    if(S_ISDIR(m))
        strcpy(s,"d");
    else if(S_ISSOCK(m))
        strcpy(s,"s");
    else if(S_ISCHR(m))
        strcpy(s,"c");
    else if(S_ISBLK(m))
        strcpy(s,"b");
    else
        strcpy(s,"-");
    strcat(s , m & S_IRUSR ? "r" : "-");
    strcat(s , m & S_IWUSR ? "w" : "-");
    strcat(s , m & S_IXUSR ? "x" : "-");
    strcat(s , m & S_IRGRP ? "r" : "-");
    strcat(s , m & S_IWGRP ? "w" : "-");
    strcat(s , m & S_IXGRP ? "x" : "-");
    strcat(s , m & S_IROTH ? "r" : "-");
    strcat(s , m & S_IWOTH ? "w" : "-");
    strcat(s , m & S_IXOTH ? "x" : "-");
    return s;
}
char* ttos(time_t time){
    static char t[20];
    struct tm * l = localtime(&time);
    sprintf(t, "%04d-%02d-%02d %02d-%02d-%02d", l->tm_year+1900, l->tm_mon+1, l->tm_mday, 
                    l->tm_hour, l->tm_min, l->tm_sec);
    return t;
}
int main(int argc, char** argv){
    if(argc < 2){
        fprintf(stderr, "用法:./a.out <文件名>.\n");
        return -1;
    }
    struct stat buf;
    if(stat(argv[1], &buf) == -1){
        perror("stat");
        return -1;
    }
    printf("设备ID:%lu\n",buf.st_dev);
    printf("i节点号:%ld\n",buf.st_ino);
    printf("类型和权限:%s",mtos(buf.st_mode));
    printf("硬链接数:%lu\n",buf.st_nlink);
    printf("用户ID:%u\n",buf.st_uid);
    printf("组ID:%u\n",buf.st_gid);
    printf("特殊设备ID:%lu\n",buf.st_rdev);
    printf("总字节数:%ld\n",buf.st_size);
    printf("I/O字节数:%ld\n",buf.st_blksize);
    printf("存储块数:%ld\n",buf.st_blocks);
    printf("最后访问时间:%s\n",ttos(buf.st_atime));
    printf("最后修改时间:%s\n",ttos(buf.st_mtime));
    printf("最后改变时间:%s\n",ttos(buf.st_ctime));
    return 0;
}
```

进程：进程就是一个程序执行是的一个实例
程序是被存储在磁盘上，包括机器指令和数据的文件
当这些指令和数据被装在到内存并被CPU所执行，形成了进程
一个程序可以用啥被运行为多个进程
在Linux源码中常常将进程称为任务(task)
从内核的观点看，进程的目的就是为了担当分配系统资源(CPU时间，内存)的实体

相关命令
pstree - 进程树，以树状结构显示当前所有进程的关系
ps -aux - 以详细的方式将当前终端的进程信息都输出打印

进程的信息
pid:进程的id,用于标识进程
STAT:status
状态描述进程的状态
S:sleep可唤醒修改
R:run运行状态
D:dead死亡，不可唤醒休眠

父子进程
Unix系统中的进程存在父子关系
	一个父进程可以创建一到多个子进程
	但是每个子进程只有一个父进程
整个系统中只有一个根进程，即p1d为0的调度进程
init - 1号进程 - 隐藏 - systemd

进程标识
每个进程都会有一个非负整数形式的唯一编号，叫PID(process identification,进程标识)
PID在任意时刻都是唯一的，但是可以重用，当进程终止并且被回收后，他的即id就可以被其他进程所有
系统中有些pid是专用的
0号进程-调度进程
1号进程-init进程
除了调度进程之外，系统中的每一个进程都有唯一的父进程，对任意一个子进程而言，其父进程的id就是他的ppid
相关函数
pid t getpid(void);
获取当前进程的pid
pid t getppid(void);
获取当前进程的父进程的pid

进程的创建-fork函数
\#include <unistd.h>
pid t fork(void);
功能：创建调用进程的子进程
返回值：
失败 返回-1
成功 父进程返回子进程的pid,子进程返回0

明确
程序运行变为了进程，进程会占据0-3G所有的内存空间，
从微观的角度上来讲，在同一个时刻只能由一个进程在运行

结果
调用完成fork函数后，创建一个新的进程，此时新的进程就是已有进程的子进程，此时两个进程的内容是完全一样的
此时父进程和子进程的执行顺序是随机的
创建子进程成功的那一刻，父子进程的执行位置是一样的

代码：
fork.c

```c
#include<stdio.h>
#include<unistd.h>
int main(void){
    printf("%d进程：我是父进程，我要创建子进程\n",getpid());
    pid_d pid = fork();
    if(-1 == pid){
        perror("fork");
        return -1;
    }
    if(pid == 0){
        printf("%d进程：我是子进程\n",getpid());
    }else{
        printf("%d进程：我是父进程\n",getpid());
    }
    printf("%d进程：哈哈哈\n",getpid());
    return 0;
}
```

为父进程和子进程设计先不同后相同的执行过程
  pid_t pid = fork();
  if(-1 == pid){
    perror("fork");
    return -1;
  }

  if(pid == 0){
    printf("%d进程:我是子进程.\n", getpid());
  }else{
    printf("%d进程:我是父进程.\n", getpid());
  }
  printf("%d进程:哈哈哈哈.\n", getpid());

为父子进程设计不同的执行过程
  pid_t pid = fork();
  if(-1 == pid){
    perror("fork");
    return -1;
  }

  if(pid == 0){//子进程执行代码
    printf("%d进程:我是子进程.\n", getpid());
    return 0;
  } 
  //父进程执行代码
  printf("%d进程:我是父进程.\n", getpid());
  printf("%d进程:哈哈哈哈.\n", getpid());


为父子进程设计相同的执行过程
  pid_t pid = fork();
  if(-1 == pid){
    perror("fork");
    return -1;
  }
  //父子进程执行代码
  printf("%d进程:我是父/子进程.\n", getpid());
  printf("%d进程:哈哈哈哈.\n", getpid());

思考 :
  以下代码总共产生了多少个进程
  for(int i = 0; i < 3; i++)
  {
    fork();
  }

  for() -父进程 - fork - 父进程 - fork - 父进程 
                      \- 子进程
             \- 子进程 - fork - 子进程 
                     \- 子进程的子进程 
        -子进程 - fork - 子进程 - fork - 子进程 
                 \- 子进程的子进程 
          \- 子进程的子进程 fork - 子进程的子进程 
                   \- 子进程的子进程的子进程
最终产生了8个进程

网络通信

1. 网络和网络协议
   明确：计算机网络的概念
   计算机网络就是将地理位置不同的具有独立功能的多台计算机和其外部设备，通过通信线路链接起来，在网络操作系统，网络管理软件和网络通信协议的管理和协调下，实现资源共享和信息传递的计算机系统

2. 网络协议的概念
   网络协议是一种规则，是计算机网络想要实现期中的最基本的机制
   网络协议的本质就是一种标准，即各种硬件和软件都必须遵守的共同守则
   网络协议并不是一套单独的软件，它融合于其他所有的软件甚至于硬件系统中，可以说网络协议无处不在

3. 协议栈的概念
   为了坚守网络设计的复杂性，绝大多数网络采用分层设计的方法，所谓分层设计，就死按照信息流动的过程将玩过的整体功能分解为一个个的相对独立的功能层，不同机器上同等的功能会采用相同的协议，同一机器的相邻功能层之间通过接口完成信息传递，各层的协议和接口统称为协议栈

4. ISO/OSI 国际标准化组织，开发系统互联，七层模式
   应用层  业务逻辑，http/ftp/telnet             \
   表示层  数据格式字符串/图片/视频流/音频流       | 应用层
   会话层  建立，管理和终止连接                   /

   ---

   传输层  数据传输的方式 TCP/UDP
   网络层  地址，路径，链接，IP/路由协议
   数据链路层  物理寻址数据的连接状态 错误检测
   物理层  网络设备和驱动程序
   物理层：实连接
   其他层：虚连接

5. 传输的时候 

   1. 发送数据过程
      通过将信息本身在加上封包，应用程序负责组织的基本都是业务相关的数据内容，如果想要将这些数据发送出去，就要自上而下的压入协议栈，每经过一个协议层, 就会对数据做一层封包，每一层的封包都是下一层输入的内容，消息包沿着协议栈就构成了消息流 

   2. 接收数据流程 
      当从网络上接收数据时，过程和发送数据相反，消息包自下而上的流过协议栈,每经过一个协议层，就会对数据的数据解一层封包，经过层层解包之后，应用程序最终得到的只有和业务相关的数据

      消息和地址
      消息包和信息流
      消息包 = 包头和包体
      消息流 = 消息包1 | 消息包 | \.\.\.

      发送数据：封包
      接收数据：解包

   3. 封包和解包的具体内容
      应用层                     用户数据
      传输层               TCP头+用户数据->TCP包
      网络层           IP头+TCP头+用户数据->IP包
      链路层 以太网包头+IP头+TCP头+用户数据->以太网帧
      物理层 直接传输

6. TCP/IP协议
   在ISO/OSI物理协议模型基础上，TCP/IP协议做了部分的合并和简化
   同时将网络编程的接口设定在传输层和绘画层之间
   发送数据：封包+解包12次 ISO/OSI
            封包+解包6次  TCP/IP

IP地址：全程网际协议地址(Internet Protocol Address)
是有IP协议提供的一种统一的地址格式，为互联网上每个网络和每台主机提供一个逻辑地址，借以消除物理地址差异带来的影响
IPv4 - 4字节表示IP地址
IPv6 - 8字节表示IP地址
常用一个32位无符号整数来表示IP地址
常用点分十进制的方式来表示
使用点将4个字节的内存每一个字节分开，使用十进制来表示

IP地址经过拆分：网络地址 | 本地地址
32位方式表示
A级地址 | 0\-\-\- \-\-\-\- | \-\-\-\- \-\-\-\- | \-\-\-\- \-\-\-\- | \-\-\-\- \-\-\-\- |
	    |  网络地址 |			本地地址				|
		以0为首的8位网络地址 + 24位的本地地址
B级地址 | 10\-\- \-\-\-\- | \-\-\-\- \-\-\-\- | \-\-\-\- \-\-\-\- | \-\-\-\- \-\-\-\- |
	    |  网络地址 |			本地地址				|
		以10为首的16位网络地址 + 16位的本地地址
C级地址 | 110\- \-\-\-\- | \-\-\-\- \-\-\-\- | \-\-\-\- \-\-\-\- | \-\-\-\- \-\-\-\- |
	    |  网络地址 |			本地地址				|
		以110为首的24位网络地址 + 8位的本地地址
D级地址 以1110为首的32位多播地址

子网掩码
IP地址 &  子网掩码 -> 网络地址
IP地址 & ~子网掩码 -> 本地地址

子网掩码的两种表示方式：

1. 普通方式：255.255.255.0
2. 斜杠方式：255.255.255.0 一共有24个二进制的1
   198.168.1.8/24等价于
   某台计算机的IP地址：192.168.1.8
   子网掩码：255.255.255.0

子网掩码可以更改

套接字 — socket：电源插座
将其引申为一个基于TCP/IP协议可以用于实现基本网络通信功能的逻辑对象
计算机和计算机之间的通信可以被抽象的看作为套接字和套接字之间的通信
编写应用程序的时候，不需要了解网络协议的任何细节，也无需知道系统内核好网络设备的运行机制只需要做一件事，把想发送的数据写入到套接字，或者从套接字中读取想接收的数据即可

从这个意义上将，此时的套接字相当于一个文件描述符，而网络就是一个特殊的文件，此时面向网络编程和面向文件编程几乎没有什么区别了
一句话-Unix下一切皆文件

进程 - 文件描述符(fd) - 文件 - 磁盘设备 - 设备文件
进程 - 套接字(fd) - 网络 - 网络设备 - 设备文件

套接字的绑定和连接
套接字是一个提供给开发人员使用的逻辑对象，表示的是对于ISO/OSI网络协议模型中传输层及以下多层的抽象
但是实际发送好接收数据的都是这些实实在在的物理设备
需要让物理对象和逻辑对象之间建立一种关联关系，是的后续所有对于逻辑对象的操作，最终需要反映到实际的物理设备上，建立的这种关联叫做绑定(bind)
绑定只是把套接字对象(逻辑对象)和自己的物理对象连接在一起，为了实现通信需要键值对物理对象和对方的物理对象关联起来，这种关联关系叫做连接
只有这样，才能建立以物理设备为媒介，跨越不同机器的多个套接字对象之间的联系

创建套接字
int socket(int domain, int type, int protocol);
参数：
domain：地址族(address family)表示IP地址类型
	AF_INET	IPV4
	AF_INET6   IPV6
type：套接字类型
	SOCK_STREAM 流式套接字，使用TCP协议通信
	SOCK_DGRAM 数据报套接字 使用UDP协议通信
protocol：协议 表示传输协议
	使用TCP或者UDP通信 取0即可
返回值
成功 返回套接字描述符
失败 返回-1

地址结构
套接字接口通过地址结构定位一个通信主体，可以是一个文件，可以使一台计算机
基本地址结构，用于泛华参数
	struct sockaddr{
		sa_family_t sa_family;
		char sa_data[14];
	};

本地地址结构
	struct sockaddr_un{
		sa_family_t sun_family;
		char sun_path[];
	};

网络地址结构
	struct sockaddr_in{
		sa_family_t sin_family;
		in_port_t sin_port;
		struct in_addr sin_addr;
	};

注意
端口号：不用计算机之间使用IP地址你啊确定目的之间，但是同一台主机可能会有不用的网络应用在运行

IP地址 - 确定目的主机
端口号 - 确定目的应用

端口号 - unsigned short 无符号short类型(16位) - 0~65535
其中0 ~ 1024 端口号已经被西永的一些网络应用占据
	21	ftp
	23	telnet
	80	www
别分配0 ~ 1024作为自己的端口号

IP地址
	struct in_addr{
		in_addr_r s_addr;
	};
	typedef uint32_t in_addr_t;
	typedef unsigned int uint32_t;

小端字节序：低地址放低数位，高地址放高数位
大端字节序：低地址放高数位，高地址放底数位

主机A -> 发送：0X12345678 -> 网络 -> 0X78563412 -> 主机B
主机字节序：小端
网络字节序：大端

发送：主机字节序 -> 网络字节序
接收：网络字节序 -> 主机字节序

通信函数

1. 创建套接字 - socket
2. 地址结构
3. 将套接字对象和自己的地址结构绑定在一起 - bind
   int bind(int sockfd, struct sockaddr *my_addr, socklen_t addrlen);
   参数
   sockfd	套接字描述符
   my_addr   自己的地址结构
   addrlen   地址结构的长度/字节数
   返回值
   成功 返回0
   失败 返回-1
4. 将套接字对象和对方的地址结构绑定在一起 - connect
   int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
   参数
   sockfd	套接字描述符
   addr	  对方的地址结构
   addrlen   地址结构的字节数
   返回值
   成功 返回0
   失败 返回-1
5. 通过read/write函数接收/发送数据
6. 通过close函数关闭套接字
7. 字节序转换
   uint32_t htonl(uint32_t hostlong);
   uint16_t htons(uint16_t hostshort);
   uint32_t ntohl(uint32_t netlong);
   uint16_t ntohs(uint16_t netshort);
   h - host      主机字节序
   t - to        到，转换为
   n - network   网络字节序
   l - long      32位整型
   s - short     16位整型
8. IP地址
   4字节整数来表示
     整数数据
   点分十进制的方式来表示
     字符串方式
   网络字节序无符号4字节整型<-\-\->点分十进制字符串
   in_addr_t inet_addr(const char *cp);
   功能 : 字符串 -> 整数 
   int inet_aton(const char *cp, struct in_addr *inp);
   功能 : 字符串 -> 结构体(包含整数)
   char *inet_ntoa(struct in_addr in);
   功能 : 整数 -> 字符串

通信编程模型
C/S模型：服务器/客户机模型
client：客户端/机
server：服务器

服务器：被动的接收请求，提供相应的服务功能
客户端：主动的发出请求，接收产生的服务结果

服务器						   客户端
创建套接字	   socket		 创建套接字
准备地址结构    sockaddr_in    准备地址结构
绑定地址	   bind/connect      建立连接
接收请求		read/write	   发送请求
业务逻辑						 等待结果
发送响应		write/read	   接收响应
关闭套接字		close		 关闭套接字

TCP客户端和服务器
TCP协议的基本特征

1. TCP保证数据传输的可靠性
2. TCP保证数据传输的有序性
3. TCP协议提供客户端和服务器连接
   1. 客户端必须建立和服务器的连接 - 虚电路
   2. 凭借已经建立好的连接管理 - 实现双方数据交互
   3. 客户端好服务器终止连接 - 结束通信
4. TCP是全双工的
   单工：数据只能朝一个方向传递
   半双工：数据可以双向传递，但是在同一时间只能超一个方向传递数据
   全双工：可以同时双向传递数据

TCP连接的生命周期
建立连接
建立连接的过程 - 三次握手
客户端 - SYN[N] -> 服务器 客户端向服务器提供建立连接请求，称为主动打开
客户端 <- ACK[N+1]SYN[N] - 服务器 服务器同意建立连接关系，同时客户端是否具备建立连接的条件
客户端 - ACK[M+1] -> 服务器 客户端向服务器表示满足建立连接的关系
ACK(应答)

交换数据
客户端 - 请求[N] -> 服务器
客户端 <- 确认[N+1] - 服务器
.\.\.
客户端 <- 响应[M] - 服务器
客户端 - 确认[M+1] -> 服务器
-\-\-\-\-\-\-\-\-\-\-\-\-\--\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
客户端 - 请求[N] -> 服务器
客户端 <- 确定[N+1]+响应[M] - 服务器
客户端 - 确认[M+1] -> 服务器

终止连接
断开连接的过程 - 四次分手
客户端 - FIN[N] -> 服务器
客户端 <- ACK[N+1] - 服务器
客户端 <- FIN[M] - 服务器
客户端 - ANK[M+1] -> 服务器

启动侦听 
int listen(int s, int backlog);
功能:在指定的套接字上启动对于连接请求的侦听功能 
参数:
  s : 套接字描述符,socket得到的套接字 
    在调用listen之前, 是无法感知请求, 但是在调用该函素之后, 是一个被动套接字
    具有感知连接请求的能力
  backlog :未决连接队列的最大长度, 一般取不小于1024的值
返回值:
  成功  返回0 
  失败  返回-1 

等待并接收连接
int accept(int s, struct sockaddr *addr, socklen_t *addrlen);
参数
s		侦听套接字描述符
addr	 输出连接请求者的地址信息
addrlen  输出连接请求者地址信息的字节数
返回值
成功 返回连接套接字描述符
失败 返回-1

接收数据
ssize_t recv(int sockfd, void *buf, size_t len, int flags);
参数
sockfd	套接字描述符
buf	   接收缓冲区
len       期望接收的字节数
flags     接收数据的标志，一般取0(等价于read)
	MSG_DONTWAIT	以非阻塞方式接收数据
	MSG_WAITALL	 等待所有数据，不接受到len个字节数据，函数就不返回
返回值
成功 返回实际读取的字节数
失败 返回-1

阻塞和非阻塞的区别
r = recv(\.\.\., len, \.\.\.);
					阻塞调用	非阻塞调用
可接收数据足够	   r == len	r == len
可接收数据不够	 0 < r < len  0 < r < len
没有可接收数据        不返回       r == 0 
TCP连接断开          r == 0      r == -1 
发生错误            r == -1      r == -1

发送数据
ssize_t send(int s, const void *msg, size_t len, int flags);
参数
s	 	套接字描述符
msg   	发送缓冲区
len   	期望发送的字节数
flags	 发送标志，一般取0(等价于write)
	MSG_DONTWAIT	以非阻塞方式发送数据
返回值
成功 返回实际读取的字节数
失败 返回-1

创建TCP服务器 - 等待客户端来连接
代码
tcpser.c

```c
#include<stdio.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<unistd.h>
#include<string.h>
#include<arpa/inet.h>
#include <ctype.h>
int mian(void){
    printf("服务器 : 创建套接字\n");
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if(-1 == sockfd){
        perror("sockfd");
        return -1;
    }
    printf("服务器 : 组织地址结构体\n");
    struct sockaddr_in ser;
    ser.sin_family = AF_INET;
    ser.sin_port = htons(5566);
    ser.sin_addr.s_addr = INADDR_ANY;
    printf("服务器 : 绑定套接字和地址结构\n");
    if( bind(sockfd, (struct sockaddr*)&ser, sizeof(ser)) == -1){
        perror("bind");
        return -1;
    }
    printf("服务器 : 开启侦听\n");
    if( listen(sockfd, 1024) == -1){
        perror("listen");
        return -1;
    }
    printf("服务器 : 等待并接收连接\n");
    struct sockaddr_in cli;
    socklen_t len = sizeof(cli);
    int conn = accept(sockfd, (struct sockaddr*)&cli, &len);
    if(-1 == conn){
        perror("accept");
        return -1;
    }
    printf("服务器 : 接收到了%s:%hu的客户端的连接", inet_ntoa(cli,sin_addr), ntohs(cli.sin_port));
    printf("服务器 : 业务处理\n");
    char buf[128] = {0};
    ssize_t size = read(conn, buf, sizeof(buf) - sizeof(buf[0]));
    if(-1 == size){
        perror("read");
        return -1;
    }
    for(int i = 0; i < size; i++)
        buf[i] = toupper(buf[i]);
    if( write(conn, buf, size) == -1){
        perror("write");
        return -1;
    }
    printf("服务器 : 关闭套接字\n");
    close(conn);
    close(sockfd);
    return 0;
}
```

套接字需要和本地的地址/端口号绑定起来 - bind
当前的主机IP地址/端口号具体是多少 - 定义地址结构 - 描述 - IP地址 - 端口号

socket();	- 套接字
bind();
listen();
accept();	- 连接套接字
	服务器和客户端建立的连接 - 连接套接字 - 读取数据 - 从连接套接字读取数据即可

tcpcli.c

```c
#include<stdio.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<unistd.h>
#include<string.h>
#include<arpa/inet.h>
#include <ctype.h>
int main(void){
    printf("客户端 : 创建套接字\n");
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if(-1 == sockfd){
        perror("socket");
        return -1;
    }
    printf("客户端 : 组织服务器的地址结构\n");
    struct sockaddr_in ser;
    ser.sin_family = AF_INET;
    ser.sin_port = htons(5566);
    ser.sin_addr.s_addr = inet_addr("127.0.0.1");
    printf("客户端 : 发起连接\n");
    if( connect(sockfd, (struct sockaddr*)&ser, sizeof(ser)) == -1){
        perror("connect");
        return -1;
    }
    printf("客户端 : 业务处理\n");
    for(;;){
        char buf[128] = {0};
        fgets(buf, sizeof(buf), stdin);
        if(strcmp(buf, "!\n") == 0)
            break;
        if(send(sockfd, buf, strlen(buf), 0) == -1){
            perror("send");
            return -1;
        }
        ssize_t size = recv(sockfd, buf, sizeof(buf)-sizeof(buf[0]), 0);
        if(-1 == size){
            perror("recv");
            return -1;
        }
        printf(">> %s\n",buf);
    }
    printf("客户端 : 关闭套接字\n");
    close(sockfd);
    return 0;
}
```

并发服务器

|         服务器         |         客户端          |
| :--------------------: | :---------------------: |
|   创建套接字(socket)   |       创建套接字        |
|   准备地址绑定(bind)   | 准备地址并连接(connect) |
|    启动侦听(listen)    |                         |
| 等待并接收连接(accept) |                         |
|     接收请求(recv)     |        发送请求         |
|        业务处理        |                         |
|     发送响应(send)     |        接收响应         |
|     关闭连接套接字     |       关闭套接字        |
|     sockfd还没关闭     |         客户端2         |

子进程 - 父进程创建子进程

并发服务器

主进程阻塞在accept函数中
每当有一个客户端和服务器建立连接，accept函数返回
	主进程继续等待下一个客户端的连接
	通过fork函数创健子进程，子进程处理客户端业务
主进程(侦听套接字sockfd，连接套接字conn)
	负责继续侦听不需要连接套接字
子进程(侦听套接字sockfd,连接套接字conn)
	负责连接，处理客户端任务 - 不需要侦听套接字

```c
#include <stdio.h>
#include <sys/types.h>          
#include <sys/socket.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>
#include <ctype.h>
int main(void){
    printf("服务器 : 创建套接字.\n");
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if(-1 == sockfd){
        perror("sockfd");
        return -1;
    }
    printf("服务器 : 组织地址结构.\n");
    struct sockaddr_in ser;
    ser.sin_family = AF_INET;
    ser.sin_port = htons(5566);
    ser.sin_addr.s_addr = INADDR_ANY;
    printf("服务器 : 绑定套接字和地址结构.\n");
    if ( bind(sockfd, (struct sockaddr*)&ser, sizeof(ser)) == -1){
        perror("bind");
        return -1;
    }
    printf("服务器 :开启侦听．\n");
    if( listen(sockfd, 1024) == -1){
        perror("listen");
        return -1;
    }
    for(;;){
        printf("服务器:等待并接受连接.\n");
        struct sockaddr_in cli;
        socklen_t len = sizeof(cli);
        int conn = accept(sockfd, (struct sockaddr*)&cli, &len);
        if(-1 == conn){
            perror("accept");
            return -1;
        }
        printf("服务器:接收到了%s:%hu的客户端的连接", inet_ntoa(cli.sin_addr), ntohs(cli.sin_port));
        pid_t pid = fork();
        if(-1 == pid){
            perror("fork");
            return -1;
        }
        if(0 == pid){
            close(sockfd);
            printf("服务器:业务处理.\n");
            for(;;){
                char buf[128] = {0};
                ssize_t size = read(conn, buf, sizeof(buf) - sizeof(buf[0]));
                if(-1 == size){
                    perror("read");
                    return -1;
                }
                if(0 == size){
                    printf("服务器:客户端关闭套接字.\n");
                    break;
                }
                for(int i = 0; i < size; i++)
                    buf[i] = toupper(buf[i]);
                if( write(conn, buf, size)  == -1){
                    perror("write");
                    return -1;
                }
            }
            printf("服务器:子进程关闭套接字");
            close(conn);
            return 0;
        }
        printf("服务器:父进程关闭套接字.\n");
        close(conn);
    } 
    close(sockfd);
    return 0;
}
```

for(;;){
	父进程 - 侦听 - 阻塞 - 有新的客户端到来 - 向下运行 - 阻塞创建子进程
    子进程任务
        close sockfd;
        for{
            业务处理;
        }
        close 连接套接字
    父进程任务 
        关闭连接套接字
}

A.服务器的主进程阻塞于侦听套接字的accept调用, 客户端通过connect函数向服务器发起连接请求
    服务器主进程 - accept 函数阻塞 
    客户端 - connect 请求连接 
B.客户端的连接请求被系统内核接收,服务器的主进程从accept函数中返回, 通过得到用于通信的连接套接字 
    连接请求 - 内核 
    服务器主进程 - accept函数 - 返回 
                \- 侦听套接字 - sockfd 
                \- 连接套接字 - conn 
C.服务器主进程调用fork函数创建子进程, 子进程复制父进程的文件描述符表, 因此子进程具有侦听和连接
 两个套接字描述符 
    服务器主进程
                \- 侦听套接字 - sockfd 
                \- 连接套接字 - conn
    服务器子进程
                \- 侦听套接字 - sockfd 
                \- 连接套接字 - conn
D.服务器主进程关闭连接套接字, 服务器子进程关闭侦听套接字
    服务器主进程
                \- 侦听套接字 - sockfd
    服务器子进程
                \- 连接套接字 - conn
E.主进程通过循环继续阻塞于针对侦听套接字的accept调用,子进程则通过连接套接字和客户端通信此时
	如果有新的客户端到来 - 继续重复A-E

资源回收
子进程结束后 - 如果父进程还没有结束 - 需要父进程来回收子进程的资源
子进程结束后 - 如果父进程已经结束 - 子进程就会将init1号进程当做父进程 - init进程回收其该进程资源

进程再执行的时候 - 让进程做一些事情 - 给进程发送信号 - 进程接收到信号后 - 处理信号
默认情况 - 发送信号给进程 - 进程直接终止 
捕获信号 - 发送某个信号给进程 - 进程捕获该信号 - 做一些事情 - 信号 - 进程间通信方式 
做的方式 - 对特定的信号进行处理 - SIGCHLD

```c
#include <stdio.h>
#include <sys/types.h>          
#include <sys/socket.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>
#include <ctype.h>
#include <signal.h>
#include <errno.h>
#include <sys/wait.h>
void sigchild(int signum){
    for(;;){
        pid_t pid = waitpid(-1, NULL, WNOHANG);
        if(-1 == pid){
            if(errno == ECHILD){
                printf("服务器:没有子进程\n");
                break;
            }else{
                perror("waitpid");
                exit(-1);
            }
        }else if(0 == pid){
            printf("服务器:所有的子进程都在运行\n");
            break;
        }else{
            printf("服务器:回收了%d进程的僵尸资源\n",pid);
        }
    }
}
int main(void){
    if(signal(SIGCHLD, sigchild) == SIG_ERR){
        perror("signal");
        return -1;
    }
    printf("服务器 : 创建套接字.\n");
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if(-1 == sockfd){
        perror("sockfd");
        return -1;
    }
    printf("服务器 : 组织地址结构.\n");
    struct sockaddr_in ser;
    ser.sin_family = AF_INET;
    ser.sin_port = htons(5566);
    ser.sin_addr.s_addr = INADDR_ANY;
    printf("服务器 : 绑定套接字和地址结构.\n");
    if ( bind(sockfd, (struct sockaddr*)&ser, sizeof(ser)) == -1){
        perror("bind");
        return -1;
    }
    printf("服务器 :开启侦听．\n");
    if( listen(sockfd, 1024) == -1){
        perror("listen");
        return -1;
    }
    for(;;){
        printf("服务器:等待并接受连接.\n");
        struct sockaddr_in cli;
        socklen_t len = sizeof(cli);
        int conn = accept(sockfd, (struct sockaddr*)&cli, &len);
        if(-1 == conn){
            perror("accept");
            return -1;
        }
        printf("服务器:接收到了%s:%hu的客户端的连接", inet_ntoa(cli.sin_addr), ntohs(cli.sin_port));
        pid_t pid = fork();
        if(-1 == pid){
            perror("fork");
            return -1;
        }
        if(0 == pid){
            close(sockfd);
            printf("服务器:业务处理.\n");
            for(;;){
                char buf[128] = {0};
                ssize_t size = read(conn, buf, sizeof(buf) - sizeof(buf[0]));
                if(-1 == size){
                    perror("read");
                    return -1;
                }
                if(0 == size){
                    printf("服务器:客户端关闭套接字.\n");
                    break;
                }
                for(int i = 0; i < size; i++)
                    buf[i] = toupper(buf[i]);
                if( write(conn, buf, size)  == -1){
                    perror("write");
                    return -1;
                }
            }
            printf("服务器:子进程关闭套接字");
            close(conn);
            return 0;
        }
        printf("服务器:父进程关闭套接字.\n");
        close(conn);
    } 
    close(sockfd);
    return 0;
}
```

TCP客户端和服务器 - 通信
	可是连接
	业务处理
	通信终止

1. 客户端主动终止通信过程
   在某个特定的时刻，客户端认为此时不再需要服务器继续为其提供服务了
   于是客户端在接收完最后一个响应包后，通过c1ose函数关闭和服务器通信的套接字
   客户端的TCP协议栈向服务器发送FIN分节并且得到对方的ACK应答
   服务器中负责和客户端通信的子进程，此时正在试图通过xecv函数/xead函数接收下一个请求包，但是收到了来自于客户端的FIN分节，返回0
   于是该子进程退出收发循环，同时通过close函数关闭连接套接字
   导致服务器的TCP协议栈向客户端发送FIN分节，客户端接收到进入到TIME WAIT状态，并在收到对方的ack应答后，自己进入到c1osed状态
   随着收发循环的退出，服务器子进程终止，然后再服务器主进程的SIGCHLD(17)信号处理函数回收
   通信过程宣告结束

   断开连接的过程 - 四次分手
   客户端 - FIN[N] -> 服务器
   客户端 <- ACK[N+1] - 服务器
   客户端 <- FIN[M] - 服务器
   客户端 - ANK[M+1] -> 服务器

2. 服务器主动终止通信过程
   服务器中专门辅助和某个特定的客户端通信的子进程，在运行过程中如果出现了问题，无法和客户端通信，不得不调用close函数关闭连接套接字/或者直接退出/或者被信号杀死
   服务器的TCP协议栈就会向客户端发送FIN分节并得到对方的ACK应答

   1. 如果客户端试图通过recv函数接收响应包
      此时该函数会返回0
      客户端就会根据返回值判断 - 当前服务器宕机 - 客户端直接通过c1ose函数关闭和服务器的连接套接字 - 终止通信
   2. 如果客户端试图通过send函数发送请求包
      此时该函数并不会失败，但是会导致对方以RST分节做出响应 - 该响应分节会先于FIN分节被紧随其后的rece函数收到并返回-1
      同时设置errno为ECONNRESET
      终止通信的条件

3. 服务器主机不可达 - 主机崩溃/网络中断/路由失效
   在服务器主机不可达的前提下 - 无论是客户端还是服务器 - 它们的协议栈都不会再有任何数据分节的交换
   客户端通过send函数发送完了请求包 - 会阻塞在recv函数上等待来自服务器的响应包
   此时客户端的TCP协议栈会持续的重传数据分节 - 试图得到对方的acd应答
   最终重传12次 - 最长等待9分钟
   当TCP最终决定放弃的时候，会通过recv函数向用户进程返回失败
   设置errno ETIMEOUT/EHOSTUNREACH/ENETUNREACH
   假设此时 网络ok了 - 因为失去了之前的连接 - 通信无法继续
   TCP - 面向于连接
       \- 流格式的连接
   UCP - 面向于无连接
       \- 数据包套接字

   通信：
   C1 - 1 - 4 - C2
   C1 - 1 - 3 - 5 - 4 - C2
   C1 - 1 - 3 - 4 - C2
   C1 - 1 - 2 - 4 - C2

   在进行通信的时候，数据包可以选择多条线路

   面向路连接，假设有五个数据包
   第1个数据包 - 路径1
   第2个数据包 - 路径2
   第3个数据包 - 路径4
   第4个数据包 - 路径2
   第5个数据包 - 路经1

   每一个数据包之间都是独立的，谁也不影响谁
   就会有很大不确定性 - 不确定数据是否可以真的传输到到目的主机
   C1发送消息给C2 - C1就先发送出去 - 到不到 - 不知道
   C2接收C1的消息 - 没有选择的权利 - 被动接收

   TCP - 面向连接 - 通信之前 - 先建立连接关系 - 通信
   通信线路-路由器维护
   如果断开连接 - 路由器将之前存储的路径信息 - 删除
   该条线路 - 虚电路

1. UDP协议的基本特性

   1. UDP不提供客户端和服务器之间的连接
   2. UDP不保证数据传输的可靠性和有序性
      不提供 确认 超时重传 RTT估算 序列号
   3. UDP不提供流量控制
   4. UCP是全双工
      应用程序在任何时候都可以发送也可以接收数据

2. 常用函数
   size_t recvfrom(int sockfd, void *buf, size_t len, int flags, struct sockaddr *src_addr, socklen_t *addrlen);
   参数
   sockfd	套接字描述符
   buf	   接收缓冲区
   len       期望接收的字节数
   flags     接收数据的标志，一般取0(等价于read)
   	MSG_DONTWAIT	以非阻塞方式接收数据
   	MSG_WAITALL	 等待所有数据，不接受到len个字节数据，函数就不返回
   src_addr  输出源主机的地址信息
   addrlen   输入输出源主机的地址信息字节数

   返回值
   成功 返回实际读取的字节数
   失败 返回-1

   ssize_t sendto(int sockfd, const *buf, size_t len, int flags, const struct sockaddr *dest_addr, socklen_t addrlen);
   参数
   s		 套接字描述符
   msg  	 发送缓冲区
   len  	 期望发送的字节数
   flags	 发送标志，一般取0(等价于write)
   	MSG_DONTWAIT	以非阻塞方式发送数据
   dest_addr 目的主机的地址信息

   返回值
   成功 返回实际读取的字节数
   失败 返回-1

服务器
udpser.c

```c
#include <stdio.h>
#include <sys/types.h>          
#include <sys/socket.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>
#include <ctype.h>
#include <signal.h>
#include <errno.h>
#include <sys/wait.h>
    printf("服务器 : 创建套接字.\n");
    int sockfd = socket(AF_INET, SOCK_STREAM, 0);
    if(-1 == sockfd){
        perror("sockfd");
        return -1;
    }
    printf("服务器 : 组织地址结构.\n");
    struct sockaddr_in ser;
    ser.sin_family = AF_INET;
    ser.sin_port = htons(5566);
    ser.sin_addr.s_addr = INADDR_ANY;
    printf("服务器 : 绑定套接字和地址结构.\n");
    if ( bind(sockfd, (struct sockaddr*)&ser, sizeof(ser)) == -1){
        perror("bind");
        return -1;
    }
    for(;;){
        char buf[128] = {0};
        struct sockaddr_in cli;
        socklen_t len = sizeof(cli);
        ssize_t size = recvfrom(sockfd, buf, sizeof(128)-1, 0, (struct sockaddr*)&cli, len);
        if(-1 == size){
            perror("recvfrom");
            return -1;
        }
        for(int i = 0; i < size; i++)
            buf[i] = touppr(buf[i]);
        if(sento(sockfd, buf, size, 0, (struct sockaddr*)&cli, len) == -1){
            perror("sento");
            return -1;
        }
    }
	pritnf()
    close(sockfd);
    return 0;
}
```

客户端
udpcli.c

```c
#include <stdio.h>
#include <sys/types.h>          
#include <sys/socket.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>
int main(void){
    printf("客户端:创建套接字.\n");
    int sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    if(-1 == sockfd){
        perror("socket");
        return -1;
    }
    printf("客户端:组织服务器的地址结构.\n");
    struct sockaddr_in ser;
    ser.sin_family = AF_INET;
    ser.sin_port = htons(5566);
    ser.sin_addr.s_addr = inet_addr("127.0.0.1");
    printf("客户端:业务处理.\n");
    for(;;){
        char buf[128] = {0};
        fgets(buf, sizeof(buf), stdin);
        if(strcmp(buf, "!\n") == 0)
            break;
        if(sendto(sockfd, buf, strlen(buf), 0, (struct sockaddr*)&ser, sizeof(ser)) == -1){
            perror("send");
            return -1;
        }
        ssize_t size = recv(sockfd, buf, sizeof(buf)-sizeof(buf[0]), 0);
        if(-1 == size){
            perror("recv");
            return -1;
        }
        printf(">> %s", buf);
    }
    printf("客户端:关闭套接字.\n");
    close(sockfd);
    return 0;
}
```

|         服务器          |      客户端      |
| :---------------------: | :--------------: |
|    创建套接字socket     |    创建套接字    |
| 准备地址结构sockaddr_in |   准备地址结构   |
|      绑定地址bind       |        \-        |
|    接收请求recvfrom     |  发送请求sendto  |
|        业务处理         |      \.\.\.      |
|     发送响应sendto      | 接收响应recvfrom |
|     关闭套接字close     |    关闭套接字    |

发送附近给服务器
sendto函数
因为服务器和客户端二者之间并没有建立连接关系 - 主动说明服务器的IP地址/端口

dns服务器
域名系统 - 完成域名好IP地址的互相映射
域名 - www.baidu.com
ip地址 - 192.168.1.8
域名系统：域名 - IP地址
上网 - 域名 - 助记符 - IP地址 - 登录网站

get host by name - 通过域名获取IP地址

struct hostent *gethostbyname(const char *name);
参数
name	字符串，主机名 - 域名
		 传递 - 域名作为参数，就回返回域名对应的IP地址
返回值
返回IP地址以及附属信息

```c
struct hostent{
    char *h_name;			//官方域名
    char **h_aliases;		//别名 可以通过多个域名访问同一主页
    int h_addrtype;			//地址类型	IPV4/IPV6
    int h_length;			//ipv4:4	ipv6:16
    char **h_addr_list;		//ip地址表
}
```

对于一些访问量比较大额服务器，分配多个ip地址给同一域名，利用多个服务器负载均衡

\#define h_addr h_addr_list[0] /\* for backward compatibility \*/
指针数组数组中每个元素都是一个二级指针 - 指向了一个字符串 - main函数中的argv
char* h_aliases[] = {"xxx", "yyy", \.\.\.,NULl};

代码
dns.c - 获取域名对应的相关信息

```c
#include<stdio.h>
#include<unistd.h>
#include<arpa/inet.h>
#include<netdb.h>
int main(void){
    if(argc < 2){
        fprintf(stderr,"usage : %s <域名>\n", argv[0]);
    }
    struct hostent* host = gethostbyname(argv[1]);
    if(NULL == hostent){
        perror("gethostbyname");
        return -1;
    }
    printf("官方主机名\n");
    printf("%s\n", host->h_name);
    printf("主机别名表\n");
    for(char** pp = host->h_aliases; *pp; PP++)
        printf("%s\n", *pp);
    printf("IP地址表\n");
    for(struct in_addr** pp = (struct in_addr**)host->h_addr_list; *pp; pp++)
        printf("%s\n", inet_ntoa(**pp));
    return 0;
}
```

HTTP协议 - Hyper Text Transfer Protocol 超文本传输协议
HTTP 1.0
HTTP 1.1
详细规定了浏览器和万维网服务器之间通信的一个规则，通过网络传输网络数据的数据传输协议

用于客户端和服务器之间的通信

通过http协议将网页下载下来
我们再通过浏览器获取网络资源的过程中，一直在蹲守HTTP协议

客户端终端和服务器终端请求和应答的标准
客户端发起了一个HTTP请求到服务器指定端口，默认80，应答服务器存储这一系资源
浏览器中输入URL，按下回车经历的流程

1. 浏览器向DNS的服务器请求解析该URL中域名所对应的IP地址
2. 根据解析后的IP地址好默认端口80好服务器建立TCP连接
3. 浏览器发出HTTP请求
4. 服务器对浏览器的请求作出响应

HTTP的请求和响应
请求行：
请求方法字段 URL字段 HTTP协议版本 使用空格将它们隔开
HTTP 1.0：三种请求方法 - GET POST HEAD
HTTP 1.1：新增五种方法 - OPTIONS PUT DELET TRACE CONNECT
其中GET是最常见的请求方法 - 用于回去服务器的数据

请求头部
由关键字/值对构成每行一队，关键字和值使用英文冒号":"分隔
请求头部告知服务器有观客户端请求的信息，典型的请求头有
Accept	  告诉服务器自己可以接收什么类型的介质，\*/\*表示任何类型，type/\*表示该类型下的所有子类型
Host	    客户端指定自己想要访问的web服务器的域名
User_Agent  浏览器表明自已的身份，是哪种浏览器
Referer	 浏览器web服务器表明自己是从哪个ur1获的点击当前请求中的网页
Connection  表示是否需要持久连接

HTTP响应
状态行 响应头[空行]响应正文

状态行
当浏览者访问一个网页的时候，浏览器会向网页所在的服务器发出请求
当浏览器接收并显示网页前，该网页所在的服务器返回一个包含HTTP状态码的信息头，
用以响应浏览器的请求
常见状态码：
200 OK				  客户端请求成功
400 Bad Request		 客户端请求的语法错误，不能被服务器理解
403 Forbidden		   服务器接收到请求，但是拒绝提供服务
404 Not Found		   请求资源不存在
503 Server Unavailable  服务器当前无法处理客户端请求，多一段时间可能会恢复正常

消息报头
Date			响应时间
Content_Type	响应类型
Content_Length  响应数据大小
Connection	  连接状态

代码
http.c

~~~c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>          
#include <sys/socket.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>
int main(int argc, char* argv[]){
    if(argc < 3){
        fprintf(stderr, "usage : %s <IP地址> <域名> [<路径>]\n", argv[0]);
        return -1;
    }
    char* ip = argv[1];
    char* name = argv[2];
    char* path = argc<4 ? "" : argv[3];
    printf("客户端:创建套接字.\n");
    int sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    if(-1 == sockfd){
        perror("socket");
        return -1;
    }
    printf("客户端:组织服务器的地址结构.\n");
    struct sockaddr_in ser;
    ser.sin_family = AF_INET;
    ser.sin_port = htons(80);
    ser.sin_addr.s_addr = inet_addr("ip");
    printf("客户端:发起连接.\n");
    if( connect(sockfd, (struct sockaddr*)&ser, sizeof(ser))  == -1){
        perror("connect");
        return -1;
    }
    char request[1024] = {0};
    sprintf(
    		"GET /%S HTTP/1.0\r\n"
        	"Host: %s\r\n"
        	"Accept: */*\r\n"
        	"User_Agent: Mozilla/5.0\r\n"
        	"Referer: %s\r\n"
        	"Connecttion: Close\r\n\r\n"
    		,path, name, name);
    if( send(sockfd, request, strlen(request), 0) == -1){
        perror("send");
        return -1;
    }
    for(;;){
        char respond[1024] = {};
        ssize_t size = recv(sockfd, respond, sizeof(respond)-1, 0);
        if(-1 == size){
            perror("recv");
            return -1;
        }
        if(0 == size)
            break;
        printf("%s", respond);
    }
    printf("\n");
    printf("客户端:关闭套接字.\n");
    close(sockfd);
    return 0;
}
~~~

