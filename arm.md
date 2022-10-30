# 开发板

    电脑：开发主机，称为上位机
    开发板：运行主机，成为下位机
    ->核心三大件
        CPU ：分析CPU型号，方便后续寻找CPU的芯片手册
        内存：物理基地址 和大小
                将来软件可以直接访问内存
                基地址：物理内存的起始地址(0x40000000)大小：1GB
                    0x40000000 ~ 0x40000000 + 1GB
                有效物理地址空间范围：
                    0x40000000 ~ 0x7fffffff
                    1GB = 1024 * 1024 * 1024 = 0x40000000
                向内存地址为0x48000000写入4字节数据0x100;
                    (unsigned long *)0x48000000 ->0x48000000存储的是地址
                                                ->该地址存储了unsigned long类型数据
                    *(unsigned long *)0x48000000 ->获取地址上的数据
                    *(unsigned long *)0x48000000 ->0x100;
                从内存地址为0x48000000读取1字节数据，存储到变量data中
                    char data = *(unsigned char *)0x48000000;
                地址指针的方式访问内存
        闪存：
            常用的三类闪存
            1.norflash
                访问的形式和内存一样
                软件以地址指针的方式访问
            2.nandflash
                访问必须通过nand控制器
                软件不可以以地址指针的方式访问
            3.emmc(常用于手机)
                访问必须通过emmc控制器
                软件不可以以地址指针的方式访问
            X6818开发板闪存的类型为emmc 容量大小为8GB
              举例:
                int val = 100;
                0x100 101 102 104 ->4字节来储存val变量
        程序存储在闪存中，运行在内中
            内存：数据掉电丢失
            闪存：数据掉电不丢失
    ->外围设备
        根据用户的需求来决定需要分析哪个接口
        网卡：用于将上位机上的可执行程序下载到下位机上
        串口：实现下位机和上位机的通
            UART串口的传输速度很慢
        举例：
            uart串口的传输速度为115200bps(bit per second)
        假设要传输100MB文件
            1Byte = 8bit
            100 * 1024 * 1024 * 8 / 115200 = 7281s 约等于2小时
    原理图和芯片手册
        1.底板原理图分析：X6818vb2.pdf
            外设内部连接情况
        2.核心板原理图分析：x4418cv3_release20150713
            外设和CPU的连接情况
        3.CPU芯片手册分析：SEC_S5P6818X_Users_Manual_preliminary_Ver_0.00.pdf
            CPU的使用说明情况
    嵌入式处理器分类：
        arm处理器
        PowerPC处理器
        EPGA：Xilinx
        DSP：TI，达芬奇系列
        MIPS：北京君正
        单片机：51 430 stm32
    嵌入式处理器内部组成：
    CPU核
        功能：数据运算，访问外设(从外设读取数据/向外设写入数据)
        数据流：数据来自外设 -> CPU运算 -> 回归到外设
    控制器
        功能：CPU核不允许直接访问外设，要想访问外设需要通过
            对应的控制器来间接访问
    CPU核访问控制器内部的寄存器
        寄存器：各个硬件控制器内部都包含了一堆寄存器，每个寄存器都有一个唯一的物理地址
        寄存器本质是一个硬件单元，和内存是一样的，用于暂存数据
            CPU的寄存器，每个寄存器为4字节大小
        和内存一样 -> 怎么访问内存，就怎么访问寄存器
    编程：
        1.CPU核 -> 寄存器(软件)
        2.寄存器 -> 控制器(硬件)
        3.控制器 -> 外设(硬件)
    VCC3P3_SYS：3.3V电源
    3.3V/5V = 高电平(数字1)
    0V = 接地 = 低电平(数字0)
    引脚 = 管脚 = 外设和CPU连接点的名字
    复用功能：具备多种功能，但是同一时刻只能使用一个
    GPIO:General Purpose Input/Output 普通输入/输出功能
    作为输出或者输入都是相对CPU而言的
        输出：CPU输出高电平/低电平
            引脚电平由CPU控制为输出
        输入:外设输入高电平/低电平
            引脚电平由外设控制为输入
    alternate:复用的
    function:功能
    register:寄存器
    specify:指定
    点亮led灯
        1.三大件
            led灯 在开发板的位置
        2.底板原理图：分析外设的内部连接状况
            D26灯常亮 -> 电源指示灯
            D22：一头接3.3V，一头接GPIO_B26硬件连接线
                如果GPIO_B26为高电平，D22灯 灭
                如果GPIO_B26为低电平，D22灯 亮
            硬件连接线连接到CPU上
        3.核心板原理图：分析外设和CPU的连接情况
            3.3V - D22 - GPIO_B26 - SPI_WP - AC25 - CPU
            SD10/GPIOB26/TSIDATAO[2]
            选择：GPIOB26 (三个功能选一个)
                选择为输出(两个功能选一个)
                输入高低电平(控制led灯亮灭)
            CPU芯片手册：
            GPIO共分为5组：A/B/C/D/E
                GPIO引脚共160 -> 每组32个引脚 GPIOX[31,0]
                GPIO_B26 B组的第27个引脚
                1.GPIO -> GPIOX复用功能选择寄存器
                    GPIOBALTFN1     0XC001B024(地址,32bit)
                        [21:20]
                    芯片手册：搜索AC25
                    GPIOB26为复用功能1
                2.输出 -> GPIO输出使能寄存器
                    GPIOBOUTENB     0XC001B004(地址,32bit)
                    B组引脚：       B31  ... B0
                    GPIOBOUTENB:   31 30 ... 0
                        [26]        0,GPIOB26输入
                                    1,GPIOB26输出
                3.高低电平 -> GPIOX输出寄存器
                    GPIOBOU
                    T        0XC001B000(地址,32bit)
                        [26]        0,GPIOB26输出低电平
                                    1,GPIOB26输出高电平3.编辑程序
        此时所编辑的是裸板程序
        裸板程序：不使用任何的标准c库提供的函数
        编程框架：
            void ABC(void){
                ABC_init();
                //led_init();
                //uart_init();
                //i2c_init();
                //lcd_init();
                while(1){
                    //根据用户需求完成业务逻辑
                    led_on();
                    delay();
                    led_off();
                    delay();
                }
            }
            ABC：就是当前程序的入口函数，切记：裸板程序的入口函数名称不一定叫main函数
            ABC_init：初始化函数
                裸板程序上来先做各种外设的硬件初始化的工作
            根据用户需求完成业务逻辑：
                裸板程序最终必然进入到死循环
        GPIOBOUT        0XC001B000(地址,32bit)
            以地址指针的方式表示该寄存器的数据
                `#define GPIOBOUT (*(unsigned long *)0XC001B000)`
                `#define GPIOBOUTENB (*(unsigned long *)0XC001B004)`
                `#define GPIOBALTFN1 (*(unsigned long *)0XC001B024)`
        分析：
            1.想要修改寄存器中的数据
                前提 - 获取寄存器中的数据
            2.如何访问内存 - 如何访问寄存器
                以地址指针的方式访问内存 - 以地址指针的方式访问寄存器
                `#define GPIOBOUT (*(unsigned long *)0XC001B000)`
                `#define GPIOBOUTENB (*(unsigned long *)0XC001B004)`
                `#define GPIOBALTFN1 (*(unsigned long *)0XC001B024)`
                1.GPIO -> GPIOX复用功能选择寄存器
                    GPIOBALTFN1     0XC001B024(地址,32bit)
                           [21:20] = 01
                    芯片手册：搜索AC25
                    GPIOB26为复用功能1
                    GPIOBALTFN1 &= ~(1<<21);//[21]=0
                    GPIOBALTFN1 |=(1<<20);//[21=1]
                2.输出 -> GPIO输出使能寄存器
                    GPIOBOUTENB     0XC001B004(地址,32bit)
                    B组引脚：       B31  ... B0
                    GPIOBOUTENB:   31 30 ... 0
                        [26]        0,GPIOB26输入
                                    1,GPIOB26输出
                    GPIOBOUTENB |=(1<<26);
                3.高低电平 -> GPIOX输出寄存器
                    GPIOBOUT        0XC001B000(地址,32bit)
                        [26]        0,GPIOB26输出低电平     亮
                                    GPIOBOUT &= ~(1<<26);
                                    1,GPIOB26输出高电平     灭
                                    GPIOBOUT |=(1<<26);
    知识点: & |
        按位与 &
            An  Bn 结果 (An 数据A的第n位)
            1   1   1
            1   0   0
            0   1   0
            0   0   0
            两个数据进行按位与运算，两个bit都为1，结果为1；否则结果为0
        总结：
            1 & 1 = 1
            0 & 1 = 0
            任何数据和1做按位与，结果保持不变
            1 & 0 = 0
            0 & 0 = 0
                任何数据和0做按位与，结果一定为0
        按位或 |
            An  Bn 结果 (An 数据A的第n位)
            1   1   1
            1   0   1
            0   1   1
            0   0   0
            两个数据进行按位或运算，只要有一个bit为1，结果就为1；否则结果为0
        总结：
            1 | 1 = 1
            0 | 1 = 1
                任何数据和1做按位或，结果为1
            1 | 0 = 1
            0 | 0 = 0
                任何数据和0做按位或，结果保持不变
        给bit位清0和置1操作
            假设int类型变量a,a[5] = 0,其他bit保持不变
                1111 1111 1101 1111     a
                ————————————————————    &
                xxxx xxxx xx0x xxxx     a[5]=0
                    a &= 0xffffffdf;
                1111 1111 1101 1111     0xffffffdf
                ————————————————————    ~
                0000 0000 0010 0000     0x20
                    a &= ~0x20;
                0000 0000 0010 0000     0x20
                等价于
                0000 0000 0010 0000     1<<5
                    a &= ~(1<<5);
    结论：
    按位与 &
        a[n] = 0;
            a &= ~(1<<n);
    按位或 |
        a[n] = 1;
            a |= ~(1<<n);
    
    GPIOCOUT[12] = 0;
    GPIOCOUT &= ~(1<<12);

代码:
led.h

```c
#ifdef _LED_H
#define _LED_H
//寄存器的宏定义
#define GPIOBOUT (*(unsigned long *)0XC001B000)
#define GPIOBOUTENB (*(unsigned long *)0XC001B004)
#define GPIOBALTFN1 (*(unsigned long *)0XC001B024)
//声明操作函数
extern void led_init(void);//初始化函数
extern void led_on(void);//开灯
extern void led_off(void);//关灯
extern void delay(int n);//延时函数声明
#endif 
```

    分析：
        初始化函数
        1.将AC25引脚配置为GPIO功能
        2.将AC25引脚配种为输出模式
        配置为GPIO输出模式
            GPIOBALTFN1[21:20] = 01;
                -> GPIO模式
            GPIOBOUTENB[26] = 1;
                ->输出模式

代码:
led.c

```c
#include"led.h"
//作为整个程序的入口函数
void led_test(void){
    //硬件的初始化
    led_init();
    while(1)[
        led_on();
        delay(0x1000000);
        led_off();
        delay(0x1000000);
    ]
}
void led_init(void){
    //1.GPIO功能
    GPIOBALTFN1 &= ~(1<<>21);//[21]=0
    GPIOBANTFN1 |= (1<<20);//[20]=1
    //2.输出模式
    GPIOOUTENB |= (1<<26);//[26]=1
    //3.输出高电平 - 灭
    GPIOBOUT |= (1<<26);
}
void led_on(void){
    GPIOBOUT &= ~(1<<26);//[26]=0
}
void led_off(void){
    GPIOBOUT |= ~(1<<26);//[26]=1
}
//delay(10000);
void delay(int n){
    int i=n;
    for(; i != 0;i--);
    //while(i--); -> while(0);
}
```

```
4.编译程序
    交叉编译：在一个平台生成另一个平台的可执行程序
        x86 -> arm运行
    步骤：
    1.安装交叉编译器
        将交叉编译器拷贝到虚拟机中
        //向上位机Linux系统添加交叉编译命令的环境变量
        sudo vim /etc/environment
        将交叉编译器文件的bin目录添加到PATH环境变量中
            PATH是由一个一个的路径构成，路径之间使用分号隔开
        保存退出
        重启上位机
        重启后验证：
            arm-cortex_a9-linux-gnueabi-acc -v(可以arm+tab键)
    2.交叉编译程序
        1.arm-cortex_a9-linux-gnueabi-gcc -nostdlib -c -o led.o led.c
        2.arm-cortex_a9-linux-gnueabi-ld -nostartfiles -nostdlib -Ttext=0x48000000 -eled_test -o led.elf led.o
        3.arm-cortex_a9-linux-gnueabi-objcopy -O binary led.elf led.bin
        分析：
            1.arm-cortex_a9-linux-gnueabi-gcc -nostdlib -c -o led.o led.c
                arm-...-gcc：c语言交叉编译器
                -nostdlib：no standard library - 编译的时候不使用标准c库的内容
                -c：只编译不链接
                -o led.o led.c：由led.c得到led.o，二进制内容
            2.arm-cortex_a9-linux-gnueabi-ld -nostartfiles -nostdlib -Ttext=0x48000000 -eled_test -o led.elf led.
                arm-...-le：链接器
                -nostartfiles：链接的时候，不要链接开始相关代码
                -nostdlib：链接的时候，不链接标准库的内容
                -Ttext=0x48000000：将来运行在开发板的内存地址为0x48000000
                                   将来程序要下载到下位机内存为0x48000000地址上，并且从0x48000000地址上运行
                -eled_text：告诉链接器，裸板程序的入口函数为led_test
                -o led.elf：最初生成一个ELF格式的可执行二进制文件
                                该elf格式的二进制文件只能运行在Linux操作系统
                        构成：ELF信息|纯二进制信息
                            Linux系统：识别ELF信息 - 剥离ELF信息 - 执行纯二进制信息
                            裸板程序：无法识别ELF信息并剥离，无法执行该程序
            3.arm-cortex_a9-linux-gnueabi-objcopy -O binary led.elf led.bin
                arm-...-objcopy：二进制文件拷贝处理命令
                -O binary：只保留二进制的内容
                led.elf：带elf信息的二进制可执行文件
                led.bin：纯正的二进制文件
5.测试运行
    D25灯 - 3.3V -- D25 -- GPIOC12 --w15 --CPU -- SA12/GPIOC12/SPITXD2/SDnRST2
        GPIOC12 为高电平 灭
                为低电平 亮
        将w15引脚配置为：
            GPIO功能
                复用功能选择寄存器
                GPIOCALTFN0     0XC001C020
                搜索：w15   GPIOC12 - alternate function 1
                    选择复用功能1
                    GPIOCALTFN0[25:24]=01
            输出模式
                GPIO输出使能寄存器
                GPIOCOUENB      0XC001C004
                    [12]        1输出
            输出高底电平
                GPIO输出寄存器
                GPIOCOUT        0XC001C000
                    [12]        0,low level
                    [12]        1,high level
```





# 设置蜂鸣器的鸣叫

    3.3V电源 - 蜂鸣器 - 三极管(Q1) - GND
    Q1 / Q2 / Q3 / ...
        Q三极管，下标数字代表三极管的序号
    D1 / D2 / D3 / ...
        D二极管，下标数字代表二极管的序号
    三极管的应用：
        电流放大
        无触点开关
            原理：基极电流控制集电极电流导通和截止
       材料：
        硅管和锗管
       分类：
        NPN PNP
        工作原理相同：使用两个PN节将半导体分为三部分
        NPN：
                    c集电极
                      |
                      |
                     /
            B基极————
                     \
                      |
                      V
                      |
                    E发射极
         PNP：
                     c集电极
                      |
                      |
                      /
            B基极————
                    ^
                      \
                      |
                      |
                    E发射极
        使用最多的：
            硅管 - NPN
            锗管 - PNP
    基极电流 - 流过来 - 三极管接通
    基极电流 - 没过来 - 三极管截止
                3.3V
                 |
                 LS(蜂鸣器)
                 |
                 V
                 |
        PWM2 ——> Q1
                 |
                 V
                 |
                GND
    PWM2连接到了CPU - 核心板原理图
    PWM2 - AD12 - SA14/GPIOC14/PWM2/VICLK2
    选择：GPIOC14 功能
        配置为输出
                       3.3V
                        |
                        LS(蜂鸣器)
                        |
                        V
                        |
    CPU - AD12 - PWM2 - Q1
                        |
                       V
                       |
                       GND
    输出高电平，输出电流到Q1 - 接通 - 蜂鸣器鸣叫
    输出低电平，没有电流到Q1 - 截止 - 蜂鸣器不叫
    配置给引脚 为GPIO功能
        GPIOCALTFN0     0XC001C020
            [29:28] = 01
        GPIOC14引脚为复用功能1
    配置该引脚 为输出模式
        GPIOCOUTENB     0X001C004
            [14] = 1 输出模式
    配置该引脚 输出高低电平
        GPIOCOUT        0XC001C000
            [14] = 0 输出低电平 - 截止 - 蜂鸣器不叫
            [14] = 1 输出高电平 - 接通 - 蜂鸣器鸣叫
        - CPU芯片手册

# UART通信

    常见CPU和外设之间的通信方式
        GPIO接口：LED灯 蜂鸣器 按键等
        UART串口接口：BT蓝牙 GPS(定位) GPRS(打电话 发短信)
        I2C接口：触摸屏 重力传感器
        SPI接口：NORFLASH SD卡等
        1 - Wire(一线式)接口：DS18B20温度传感器 EEPROM存储器
    UART串口 定义：
        九个字：通用串行异步收发器
        通用：UART串口应用非常广泛
        串行：说明CPU和外设通信只需要一根信号线，该信号线一定是数据线
            用于传输数据
                数据传输时一个bit位一个bit位传输
            UART数据传输从低位开始
                CPU通过UART给BT发送数据0X95(10010101)
                    数据线上的电平序列(波形图)为：
                        1 - 0 - 1 - 0 - 1 - 0 - 0 - 1
                        高——底——高——底——高——底——底——高
                CPU通过UART给BT发送数据0x76(01110110)
                    数据线上的电平序列(波形图)为：
                        0 - 1 - 1 - 0 - 1 - 1 - 1 - 0
                        底——高——高——底——高——高——高——底
                CPU通过UART给BT发送数据0x58(01011000)
                    数据线上的电平序列(波形图)为：
                        0 - 0 - 0 - 1 - 1 - 0 - 1 - 0
                        底——底——底——高——高——底——高——底
            明确：信号线分类
                    数据线：此信号线只能传输数据
                        如果要传递的数据为1，将数据线拉成高电平
                        如果要传递的数据为0，将数据线拉成低电平
                    地址线：该信号线只能传输地址
                    控制信号线：该信号先用于实现数据双方同步数据，例如：时钟控制信号线
        并行：多根信号线(数据线)
            CPU和外设之间连接了多根信号线(8/16/32)
                CPU和外设一次性传输1/2/4字节
        并行和串行的对比：
            传输速度：并行>串行
            传输距离：串行>并行
            抗干扰性：串行>并行
        异步：同步数据的一种方式
            明确：计算机中，CPU的数据出来速度要远远快于外设的数据处理速度
                如果保证双方的数据传输正确，需要考虑到数据同步
                数据同步：一方发送数据，要保证对方接收到数据之后才能发送下一个
            计算机中同步数据的方式有两种：
                异步：异步靠协议(CPU和外设达成协议，规范，标准)
                    UART为异步方式
                    UART必然会有对应的数据传输协议
                同步：同步依靠时钟控制信号线(12c接口)
        收发器：接收和发送的硬件单元
            CPU -> 外设：CPU为发送器；外设为接收器
            外设 -> CPU：外设为发送器；CPU为接收器
    UART数据传输协议
    空闲位：如果CPU和外设不进行数据传输，此时数据线持续传输空闲位
        位数为1位，为高电平
    起始位：如果CPU和外设要进行数据传输，首先发送起始位
        位数为1位，为低电平
    数据位：表示双方数据传输时的数据有效位数，形式：5/6/7/8
        常用8bit
    奇偶校验位：用于检测双方数据传输时是否发生了错误
        校验的方式有三种：奇校验(odd)偶校验(even)不校验(none)
        位数位1位，可以时高电平，可以是低电平
        如果采用的是不校验，双方无需传输校验位
        奇校验：
            数据位"1"的个数+校验位"1"的个数=奇数
        偶校验:
            数据位"1"的个数+校验位"1"的个数=偶数
        举例：CPU给BT发送数据0x95(10010101),数据位为8位，校验采用奇校验
        校验位：高电平，低电平
        CPU端：
            1.CPU首先将数据0x95发送到数据线上
            2.判断数据0x95中"1"的个数-4个-数据位"1"的个数-偶数
            3.数据位"1"的个数+校验位"1"的个数=奇数
                    4       +      0/1     =奇数
                    校验位发送：1-高电平(4+1=5 - 奇数)
            4.CPU段最终给BT发送校验位为1
        BT端：
            1.BT首先从数据线上接收数据0x95
            2.判断数据0x95中"1"的个数-4个-数据位"1"的个数-偶数
            3.采用奇校验，判断校验位中应该是高电平(1)
            4.读取校验位的值，判断数据传输是否发生了错误
    停止位：如果CPU和外设数据传输结束，最后发送停止位即可
        位数1位/2位为高电平
    波特率：CPU和外设数据传输的速率
        常用的两个波特率为 9600bps和 115200bps
            bps = bit per second
    RTS/CTS:RTS和CTS分别对应两根信号线类似于时钟控制信号线CLK
        如果CPU和外设连接了这两根信号线
        UART会以同步的方式来同步数据
    使用UART传输多个字节
        如果CPU和外设传输多个字节，一次性也只能传输1个字节
        具体传输规则如下：
            数据a 数据b 数据c ...
            空闲-起始-数据a-(校验)-停止-起始-数据b-(校验)-停止-...-空闲位
    CPU和外设的配置必须一致
    UART的三种工作方式：
        单工：数据传输用于一个方向传输
        半双工：数据传输可以双向，但同一时刻只能朝一个方向
        全双工：数据传输可以同时双向传输
            一般UART采用全双工方式，所以UART数据传输时，需要两根数据线：
                TX:transmit,发送数据线，永远用于发送数据
                RX:receive,接收数据线，永远用于接收数据
                GND:CPU和外设之间需要一根地线

```
1.需求
  	下位机X6818通过UART0串口循环给上位机发送一个字符串,
    	上位机通过串口工具来捕获下位机发送的字符串信息 

2.确认下位机硬件信息 
  			确认UART0的位置 
  			USB转串口线 :
   		   串口(9针) : 连接到UART0位置上
    		  USB口 : 连接到PC机上   
  	1.串口座形式
   		 优点 : 结实 稳定 可靠 
    		缺点 : 占用空间比较大 
 	 2.USB转串口 
  		  优点 : 占用空间比较小 
 		   缺点 : 不牢靠 
  	3.插针式 
		    只有三根插针, TX/RX/GND 
			    优点 : 省空间 
  			  缺点 : 不牢靠, 关键是无法区分RX/TX/GND的线序(除非标注)
      			 如果插反了, 很容易烧毁 
细看 : 
  1.底板原理图 
    串口座有九根插针, 实质上只有三根插针是使用的
    确认UART0的连接位置
    确认上位机和下位机通过UART0实际的硬件连接线的连接方式 : 
    上位机的RX->串口线->UART0串口座的PC_TXD1->SP3232E芯片的T2OUT引脚->
      SP3232E芯片的T2IN->S5P6818芯片的UARTTXD0,作为数据发送引脚 
    上位机的TX->串口线->UART0串口座的PC_RXD1->SP3232E芯片的R2IN引脚 ->
      SP3232E芯片的R2OUT引脚->S5P6818芯片的UARTRXD0,作为数据接收引脚 
    分析 : 核心板原理图, 分析CPU提供的功能

  2.核心板原理图 
    UARTRXD0 - AE19 - GPIOD14/UARTRXD0/ISO7816 
    UARTTXD0 - AD19 - GPIOD18/UARTTXD0/ISO7816/SDWP2
    对于UART,应该选择为 : UARTTXD0 和 UARTRXD0 
    问 : 使用什么寄存器来配置为UARTTXD0 UARTRXD0 
    答 : 使用 复用功能选择寄存器来配置 
      GPIODALTFN0 GPIODALTFN1 

    芯片手册 :
      搜索 : AE19 复用功能1 UARTTXD0 
        AE19引脚配置为复用功能1 
      问 : 配置谁为复用功能1呢 ?
      答 : GPIOD14 配置为复用功能1 
      GPIODALTFN0   0XC001D020 
        [29:28] = 01 配置为复用功能1 -> 配置为了UARTRXD0 
  2.1.SP3232E芯片功能 
    电平转换线芯片, 电平转换, 将TTL电平转换为EIA电平,将EIA电平转换为TTL电平 
    TTL电平 : 开发板上电压/PC机, CPU和芯片所需要的电压 
      0-1.4V  对应数字0 
      1.8-3.3V 对应数字1
    EIA电平 : 串口线上的电压 
      3-15V  对应数字0 
      -3~-15V 对应数字1 
  	pc发送数据0给CPU, 到串口线上数据0(3V),没有电平转换, 直接将3V给CPU,
    CPU接收到后发现是3V, 认为是高电平, 为数据1 

    如果pc发送数据0给CPU, 到串口线上数据0(15V),没有电平转换, CPU直接烧了

  转换过程 : 
  开发板上TTL电压-SP3232E(升压)-串口线EIA电平-SP3232E-PC机主板上的TTL电平(降压)
  PC机主板的TTL电压-SP3232E-串口线上EIA电平-SP3232E-开发板上的TTL电平 

  3.芯片手册 
  3.1.S5P6818共支持6个串口 
    目前使用的是UART0 
  3.2.每个UART控制器内部包含了两个64字节容量的FIFO缓冲区 
    一个用于暂存接收到的数据
    一个用于暂存要发送的数据 
  3.3.每个UART控制器内部包含 
   	一个发送器
	   一个64字节的FIF0发送缓冲区
	   一个发送移位器
	   一个接收器
       一个64字节的FIFo接收缓冲区
	   一个接收移位器
	   一个波特率产生器
	   一个控制单元
  3.4.发送器
		1.CPU核以地址指针的方式将要发送的数据放到发送缓冲区
		2.UART控制器硬件在硬件上自动将发送缓冲区中的数据拷贝到发送移位器中
		3.发送移位器根据给定的波特率将数据一位一位的放到TX发送数据线上
  3.5.接收器
		1.接收移位器根据给定的波特率从X接收数据线一位一位的将数据接收到
		2.UART控制器硬件上将接收移位器接收到的数据拷贝到接收缓冲区中
		3.CPU核最终以地址指针的方式从接收缓冲区中奖数据读走
  3.6.注意
		接收缓冲区和发送缓冲区，处于FIFO模式，缓冲区的大小最多64字节
		目前NON-FIFO模式，缓冲区大小为1字节
  3.7.波特率配置公式
		UBRDIVn和UFRACVALn两个数据决定了波特率
		B UBRDIVn
		F =UFRACVALn
		S=SCLK UART
		bps=波特率
		DIV VAL B F/16					公式1
		DIV VAL (S/(bps * 16))-1	  公式2
		B+F/16=(S/(bps * 16))-1	   公式3
  3.8.寄存器分析
		1.UART寄存器
				ULCONO0		0XC00A1000
					[1:0]=11,配置数据位为8位
					[2]=0,配置停止位为1位
					[5:3]=000,配置不校验
					[6]=0,配置为普通模式
				UCON0		0XC00A1004
					[1:0]CPU核以轮训方式读取数据
						明确：计算机中，CPU核的数据处理速度（指针）要远远快于外设（接收移位器以115200的速度读取数据)
									如果此时外设还没准备好数据，CPU可以采用三种方式
											1.轮训方式(po1ling)：CPU核什么也不错，就是原地空转死等，直到外设准备好了数据
											2.中断方式(interrupt):CPU核先去做其他事情，等到外设准备好书记，再来读取数据
											3.DMA(direct memory access):基于硬件，速度最快，只能读取数据;中断基于软件，读取数据，还能处理异常
					[3:2]=01 - CPU核以轮训方式读取数据
							采用NoN-FIFo模式，如果发送缓冲区中目前有数据，不能再向其中放入数据，只能等到发送缓冲区为空的时候才能放数据
					[4]=0，普通模式
					[5]=0，普通模式
						loop-back mode:回环模式（自发自收，应用于调试阶段）
UTRSTAT0		0XC00A1010
	[0]=0：缓冲区为空
	   =1：缓冲区有数据了
		while(!(UTRSTAT0 & 0X01));
			[0]=0,按位与结果为0，!取反，结果为真，陷入到死循环
				接收缓冲区为空，陷入到死循环
			[0]=1,按位与结果非0，!取反，结果为假，跳出循环
				接收缓冲区非空，跳出循环，读取数据
	[1]=0,发送缓冲区有数据，
       		死等，直到发送缓冲区中没有数据
	   =1,发送缓冲区为空
			向发送缓冲区中写入数据
		while(!(UTRSTAT0 & 0X02));
UTXH0:发送缓冲区寄存器
地址：0XC00A1020
UTXH0=要发送的数据
将字符'h'发送到PC机
	//将'h'字符写入到发送缓冲区，后续操作无需关注，硬件自动完成
	UTXHO ='h';
UTXH0:接收缓冲寄存器
地址：0XC00A1024
URXH0[7:0]:保存接收到的数据
	unsigned char data =URXH0;
UBRDIV0:波特率配置寄存器
地址：0XC00A1028
UBRDIVO=根据公式计算
UFRACVAL0：波特率配置寄存器
地址：0XC00A102C
UFRACVALO=根据公式计算
```

2.GPIO寄存器

​	AD19 - UARTTXD0
​		GPIODALTFN1
​		[5:4]=01 UARTTXD0功能
​		选择复用功能1，UARTTXD0

​	AE19 - UARTTXD0
​		GPIODALTFN0
​		[29:28]=01 UARTRXD0功能
​		选择复用功能1，UARTTXD0

​	复用功能配置 使用GPIO复用功能选择寄存器

3.时钟寄存器

​	UART：控制器适宜频率50MHz

UART使用的时钟源PLL[0]/PLL[1]，为800MHz，被UBOOT指定了

UARTCLKENB 0XC00A9000
[2]=0,禁止时钟
   =1,使能时钟

UARTCLKGENOL OXCO0A9004
[4:2]时钟源选择
	000p11[0]
	001p11[1]
		->ok uboot初始化为了800MHz
	要SCLK UART=50MHz
	分频系数n=800/50=16

分频系数计算
	公式：分频系数n=CLKDIV0+1
			      =[12:5]+1

代码：
vim uart.c //定义
void uart_init(void){
	//1.关闭uart时钟
	//2.配置复用功能：UARTTXD0 UARTRXDO
	//3.配置数据线功能：ULCON0
	//4.配置读写方式（轮训）UCoN0
	//5.配置波特率为
	//5.1.配置时钟为50MHz
	//5.2.配置UBRDIV0 UFRACVAL0
	//n.打开uart时钟
}

代码：
uart.h

```c
#ifndef  _UART_H
#define  _UART_H
//寄存器声明 
//uart
#define     ULCON0       (*(unsigned long *)0XC00A1000)
#define     UCON0        (*(unsigned long *)0XC00A1004)
#define     UTRSTAT0     (*(unsigned long *)0XC00A1010)
#define     UTXH0        (*(unsigned long *)0XC00A1020)
#define     URXH0        (*(unsigned long *)0XC00A1024)
#define     UBRDIV0      (*(unsigned long *)0XC00A1028)
#define     UFRACVAL0    (*(unsigned long *)0XC00A102C)
//gpio
#define     GPIODALTFN0  (*(unsigned long *)0XC001D020)
#define     GPIODALTFN1  (*(unsigned long *)0XC001D024)
//时钟
#define     UARTCLKENB   (*(unsigned long *)0XC00A9000)
#define     UARTCLKGEN0L (*(unsigned long *)0XC00A9004)
//函数声明
extern void uart_init(void);
extern void uart_putc(char c);
extern void uart_puts(char* str);
#endif
```

uart.c

```c
#include"uart.h"
void uart_init(void){
    //关闭uart时钟
    UARTCLKENB &= ~(1<<2);
    //配置复用功能
    GPIODALTFN1 &= ~(3<<4);
    GPIODALTFN1 |= (1<<4);
    GPIODALTFN0 &= ~(3<<28);
    GPIODALTFN0 |= (1<<28);
    //配置ULCON0
    ULCON0=3;
    //配置UCON0
    UCON0=5;
    //配置波特率
    //配置SCLK_UART 50MHz
    UARTCLKGEN0L &= ~(0x7<<2);
    UARTCLKGEN0L &= ~(0xff<<5);
    //配置UBRDIV0
    UBRDIV0=26;
    UFRACVAL0=2;
    //打开uart时钟
    UARTCLKENB |= (1<<2);
}
void uart_putc(char c){
    whil(!(UTRSTAT0 & 0X02))
    //将要发送的字符放到发送缓冲区寄存器中
    UTXH0=0;
    //追加回车字符
    if('\n'==c)
        uart_putc('\r');
}
//发送字符串函数定义
void uart_puts(char* str){
    while(*str){
        uart_putc(*str);
        str++;
    }
}
```

```c
#include "uart.h"
void main(void){
    uart_init();
    while(1){
        uart_puts("hello, everyone\n");
    }
}
```

main.c

```c
#include "uart.h"
static char buf[32];
void main(void){
	uart init();
	while(1){
		uart puts("\n shell#")
		uart gets(buf,32);
		uart puts("\n you input msg is ");
		uart puts(buf);
        return 0;
    }
}
```

char buf[] = {'h' 'e' 'l' 'l' 'o'};
  此时buf数组中存储的并不是字符串, 一串字符

代码：
2.0
uart.h

```c
#ifndef  _UART_H
#define   _UART_H
//寄存器声明 
//uart
#define     ULCON0      (*(unsigned long *)0XC00A1000)
#define     UCON0       (*(unsigned long *)0XC00A1004)
#define     UTRSTAT0    (*(unsigned long *)0XC00A1010)
#define     UTXH0       (*(unsigned long *)0XC00A1020)
#define     URXH0       (*(unsigned long *)0XC00A1024)
#define     UBRDIV0     (*(unsigned long *)0XC00A1028)
#define     UFRACVAL0   (*(unsigned long *)0XC00A102C)
//gpio
#define     GPIODALTFN0 (*(unsigned long *)0XC001D020)
#define     GPIODALTFN1 (*(unsigned long *)0XC001D024)
//时钟
#define     UARTCLKENB  (*(unsigned long *)0XC00A9000)
#define     UARTCLKGEN0L (*(unsigned long *)0XC00A9004)
//函数声明
extern void uart_init(void);
extern void uart_putc(char c);
extern void uart_puts(char* str);
extern char uart_getc(void); // 读取一个字符
extern void uart_gets(char buf[], int len); // 读取一个字符串,存储起来
#endif
```

uart.c
```c
#include "uart.h"
// 初始化函数定义 
void uart_init(void){
    // 1. 关闭uart时钟
    UARTCLKENB &= ~(1<<2);
    // 2. 配置复用功能 
    GPIODALTFN1 &= ~(3<<4);
    GPIODALTFN1 |= (1<<4);
    GPIODALTFN0 &= ~(3<<28);
    GPIODALTFN0 |= (1<<28); 
    // 3. 配置 ULCON0
    // 7654 3210
    //  000 0011
    ULCON0 = 3;// 0000 0011
    // 4. 配置 UCON0
    // 7654 3210
    // 0000 0101
    UCON0 = 5;
    // 5. 配置波特率 
    // 5.1.配置SCLK_UART 50MHz
    // [4:2]=000
    UARTCLKGEN0L &= ~(0x7<<2);
    // [12:5]=0000 1111 ->n = 16 -> 50MHz 
    UARTCLKGEN0L &= ~(0XFF<<5);//[12:5]=00000000
    UARTCLKGEN0L |= (0XF<<5);//[12:5]=00001111 15
    // 5.2.配置 UBRDIV0 UFRACVAL0 
    // 115200bps
    // B + F/16 = (S/(bps*16)) -1  公式3
    // B + F/16 = (50000000/(115200*16)) - 1 
    //          = 26.13
    // B = 26
    // F/16 = 0.13 
    // F = 2
    UBRDIV0 = 26;
    UFRACVAL0 = 2;
    // 6. 打开uart时钟
    UARTCLKENB |= (1<<2);
}
//发送字符函数定义
void uart_putc(char c){
    // 当发送缓冲区为空,发送数据
    // 非空, 死等
    while(!(UTRSTAT0 & 0X02));

    // 将要发送的字符放到发送缓冲区寄存器中
    UTXH0 = c;

    //追加回车字符
    if('\n' == c)
        uart_putc('\r');
}
// 发送字符串函数定义
// char * str = "hello,world\n";
void uart_puts(char* str){
    while(*str){
        uart_putc(*str);
        str++;
    }
}
// 为什么参数为void,而非char类型??
char uart_getc(void){
    //前提:接收缓冲区中有有效数据
    // UTRSTAT0 
    // 为空, 死等
    // 非空, 直接读取
    while(!(UTRSTAT0 & 0X01));
    //1.从接受缓冲区中读取一个字节的数据
    //unsigned char data = URXH0;
    //return data;
    return URXH0 & 0XFF;
}
//接受字符串函数定义 
//char buf[32]; 
//uart_gets(buf, 32);
void uart_gets(char buf[], int len){
    int i;
    for(i=0; i<(len-1); i++){
        buf[i] = uart_getc();
        // 添加回显
        uart_putc(buf[i]);
        // 添加回车退出
        if('\r' == buf[i])
            break;
    }
    buf[i] = '\0';
}
```

main.c
```c
#include "uart.h"
static char buf[32];
void main(void){
    uart_init();
    while(1){
        uart_puts("\n shell#");
        uart_gets(buf, 32);
        uart_puts("\n you input msg is : ");
        uart_puts(buf);
    }
}
```


编译程序：
arm-cortex_a9-linux-gnueabi-gcc -nostdlib -c -o main.o main.c
arm-cortex_a9-linux-gnueabi-gcc -nostdlib -c -o uart.o uart.c
arm-cortex_a9-linux-gnueabi-ld -nostartfiles -nostdlib -Ttext=0x48000000 -emain -o shell.elf main.o uart.o
arm-cortex_a9-linux-gnueabi-objcopy -O binary shell.elf shell.bin

下位机->上位机
上位机->下位机
终端
->终端->1获取输入的字符串1s-匹配->1显示目录的内容

通过上位机给下位机发送一个字符串->"1ed on”->下位机接收字符串->判断匹配->上位机给下位机发送的是"1ed on"->开灯

给shell裸板程序添加硬件操作功能
上位机通过UART串口给下位机发送硬件操作命令，下位机一旦接收到了操作命令，会操作硬件

明确：
不管是在串口工具还是Liux终端，只要输入了信息，这些信息最终都是以字符串的形式存在的

字符串
char * str = "hello";
str就是字符串的首地址，就是'h'字符的地址
通过地址获取地址上的数据，*str

str1 = "hello";
str2 = "world";
比较规则
先比较两个字符串第一个字符, 不相等, 哪个字符大, 哪个字符串答;
相等 -> 比较第二个字符 -> 不相等 -> 哪个字符大, 哪个字符串大 
相等 -> 比较第三个字符 -> 直到遇到 '\0' 结束
int my_strcmp(const char* str1, const char* str2){
	while(*str1 == '\0'){
		if( *str1 == *str2){
			str1++;
			str2++;
			continue;
		}
		return *str1 - *str2;
	}
	return *str1 - *str2
}

if(strl == str2) return 0;
if(strl > str2) return >0;
if(strl < str2) return <0;

特殊的字符'\0' 对应的ASCII码为0

代码：
3.0
uart.h

```c
#ifndef  _UART_H
#define   _UART_H
//寄存器声明 
//uart
#define     ULCON0      (*(unsigned long *)0XC00A1000)
#define     UCON0       (*(unsigned long *)0XC00A1004)
#define     UTRSTAT0    (*(unsigned long *)0XC00A1010)
#define     UTXH0       (*(unsigned long *)0XC00A1020)
#define     URXH0       (*(unsigned long *)0XC00A1024)
#define     UBRDIV0     (*(unsigned long *)0XC00A1028)
#define     UFRACVAL0   (*(unsigned long *)0XC00A102C)
//gpio
#define     GPIODALTFN0 (*(unsigned long *)0XC001D020)
#define     GPIODALTFN1 (*(unsigned long *)0XC001D024)
//时钟
#define     UARTCLKENB  (*(unsigned long *)0XC00A9000)
#define     UARTCLKGEN0L (*(unsigned long *)0XC00A9004)
//函数声明
extern void uart_init(void);
extern void uart_putc(char c);
extern void uart_puts(char* str);
extern char uart_getc(void); // 读取一个字符
extern void uart_gets(char buf[], int len); // 读取一个字符串,存储起来
#endif
```

uart.c

```c
#include "uart.h"
// 初始化函数定义 
void uart_init(void){
    // 1. 关闭uart时钟
    UARTCLKENB &= ~(1<<2);
    // 2. 配置复用功能 
    GPIODALTFN1 &= ~(3<<4);
    GPIODALTFN1 |= (1<<4);
    GPIODALTFN0 &= ~(3<<28);
    GPIODALTFN0 |= (1<<28); 
    // 3. 配置 ULCON0
    // 7654 3210
    //  000 0011
    ULCON0 = 3;// 0000 0011
    // 4. 配置 UCON0
    // 7654 3210
    // 0000 0101
    UCON0 = 5;
    // 5. 配置波特率 
    // 5.1.配置SCLK_UART 50MHz
    // [4:2]=000
    UARTCLKGEN0L &= ~(0x7<<2);
    // [12:5]=0000 1111 ->n = 16 -> 50MHz 
    UARTCLKGEN0L &= ~(0XFF<<5);//[12:5]=00000000
    UARTCLKGEN0L |= (0XF<<5);//[12:5]=00001111 15
    // 5.2.配置 UBRDIV0 UFRACVAL0 
    // 115200bps
    // B + F/16 = (S/(bps*16)) -1  公式3
    // B + F/16 = (50000000/(115200*16)) - 1 
    //          = 26.13
    // B = 26
    // F/16 = 0.13 
    // F = 2
    UBRDIV0 = 26;
    UFRACVAL0 = 2;
    // 6. 打开uart时钟
    UARTCLKENB |= (1<<2);
}
//发送字符函数定义
void uart_putc(char c){
    // 当发送缓冲区为空,发送数据
    // 非空, 死等
    while(!(UTRSTAT0 & 0X02));

    // 将要发送的字符放到发送缓冲区寄存器中
    UTXH0 = c;

    //追加回车字符
    if('\n' == c)
        uart_putc('\r');
}
// 发送字符串函数定义
// char * str = "hello,world\n";
void uart_puts(char* str){
    while(*str){
        uart_putc(*str);
        str++;
    }
}
// 为什么参数为void,而非char类型??
char uart_getc(void){
    //前提:接收缓冲区中有有效数据
    // UTRSTAT0 
    // 为空, 死等
    // 非空, 直接读取
    while(!(UTRSTAT0 & 0X01));
    //1.从接受缓冲区中读取一个字节的数据
    //unsigned char data = URXH0;
    //return data;
    return URXH0 & 0XFF;
}
//接受字符串函数定义 
//char buf[32]; 
//uart_gets(buf, 32);
void uart_gets(char buf[], int len){
    int i;
    for(i=0; i<(len-1); i++){
        buf[i] = uart_getc();
        // 添加回显
        uart_putc(buf[i]);
        // 添加回车退出
        if('\r' == buf[i])
            break;
    }
    buf[i] = '\0';
}
```

led.h
```c
#ifdef _LED_H
#define _LED_H
//寄存器的宏定义
#define GPIOBOUT (*(unsigned long *)0XC001B000)
#define GPIOBOUTENB (*(unsigned long *)0XC001B004)
#define GPIOBALTFN1 (*(unsigned long *)0XC001B024)
//声明操作函数
extern void led_init(void);//初始化函数
extern void led_on(void);//开灯
extern void led_off(void);//关灯
extern void delay(int n);//延时函数声明
#endif
```

led.c

```c
#include"led.h"
//作为整个程序的入口函数
void led_test(void){
    //硬件的初始化
    led_init();
    while(1)[
        led_on();
        delay(0x1000000);
        led_off();
        delay(0x1000000);
    ]
}
void led_init(void){
    //1.GPIO功能
    GPIOBALTFN1 &= ~(1<<>21);//[21]=0
    GPIOBANTFN1 |= (1<<20);//[20]=1
    //2.输出模式
    GPIOOUTENB |= (1<<26);//[26]=1
    //3.输出高电平 - 灭
    GPIOBOUT |= (1<<26);
}
void led_on(void){
    GPIOBOUT &= ~(1<<26);//[26]=0
}
void led_off(void){
    GPIOBOUT |= ~(1<<26);//[26]=1
}
//delay(10000);
void delay(int n){
    int i=n;
    for(; i != 0;i--);
    //while(i--); -> while(0);
}
```

strcmp.h

```c
#ifndef STRCMP_H
#define STRCMP_H
extern int my_strcmp(const char* str1, const char* str2);
#endif
```

strcmp.c
```c
#include "strcmp.h"
int my_strcmp(const char* str1,const char* str2){
    while(*str1){
        if(*str1 == *str2){
            str1++;
            str2++;
            continue;
        }
        return *str1 - *str2;
	}
	return *str1 - *str2
}
```

main.c

```c
#include "uart.h"
#include "led.h"
#inlclude "strcmp.h"
static char buf[32];
void main(void){
    uart_init();
    led_init();
    while(1){
        uart_puts("\n shell#");
        uart_gets(buf, 32);// 从上位机获取操作命令 
        if(!strcmp(buf, "led on"))
            led_on();
        else if(!strcmp(buf, "led off"))
            led_off();
        else 
            uart_puts("please input valid command!\n");
    }
}
```

编译
arm-cortex_a9-linux-gnueabi-gcc -nostdlib -c -o main.o main.c
arm-cortex_a9-linux-gnueabi-gcc -nostdlib -c -o led.o led.c
arm-cortex_a9-linux-gnueabi-gcc -nostdlib -c -o uart.o uart.c
arm-cortex_a9-linux-gnueabi-gcc -nostdlib -c -o strcmp.o strcmp.c
arm-cortex_a9-linux-gnueabi-ld -nostartfiles -nostdlib -Ttext=0x48000000 -emain -o shell.elf main.o uart.o led.o strcmp.o
arm-cortex_a9-linux-gnueabi-objcopy -O binary shell.elf shell.bin

使用Makefile对程序进行优化
Makefile功能：制定编译规则，将来程序就会按照规则进行编译，并且简化程序的编译流程
Makefile的本质是普通的文本文件
Makefile语法
目标：依赖1 依赖2 依赖3 … 依赖n
(tab键)编译命令1
(tab键)编译命令2
(tab键)编译命令3
…
(tab键)编译命令n

注意：Makefile的注释使用"#"
	  make后，自动到当前目录去寻找Makefile

目标：hel1 oworld.c编译得到可执行程序he11 oworld
此时目标：he11 oworld
依赖：hel1 oworld.c
思路：利用he11 oworld.c得到he11 oworld可执行程序
vim Makefile
编译规则
helloworld:helloworld.c
	gcc helloworld.c -o helloworld
保存退出
或者
vim Makefile
编译规则
helloworld:helloworld.o
gcc helloworld.o -o
helloworld
helloworld.o:helloworld.c
gcc -c
helloworld.c -o
helloworld.o
保存退出
在终端同一目录执行make命令

make命令后执行Makefile文件指定的规则的流程
当执行make命令后，make命令首先在当前目录下找Makefile文件，一旦找到，打开该文件，然后make命令确认Makefile文件指定的所有的规则，然后确定最终的目标为：helloworld
最终的源文件是helloworld.c
1.如果helloworld不存在，make命令在当前目录下找helloworld的依赖文件helloworld.o，如果he11owor1d.o存在，直接gcc hel1 pworld.o-ohel1 oworld，如果helloworld.o不存在，make目录继续找helloworld.c，如果找到了.c文件，首先编译得到.o，然后再编译得到可执行程序
2.如果helloworld存在，make再检查最终源文件helloworld.c的时间戳时候比helloworld的时间戳更新，如果he1 loworld.c的时间戳更新，说明he1 loworld.c被更新过，make目录重新根据编译的规则，重新执行一遍，如果发现he1 loworld.c没有更新过，无需重新编译

Makefile小技巧
变量：类似于c程序中#define宏，让Makefile便于修改

OBJ=main.o#定义变量oBJ赋值为main.o
使用变量的时候：$(OBJ) == main.o
变量可以存储多个内容
OBJ=main.o uart.o led.o strcmp.o

$(OBJ) == main.o uart.o led.o strcmp.o

2)多个文件编译(.c->.o)
号.0：号.C
gcc -c $-o $
作用：将所有的.c文件生成对应的.o文件
号.c所有的.c文件
号.o所有的.o文件
s@ 目标文件
$< 依赖文件

add.o:add.c
gcc -c add.c -o add.o

伪目标
如果一个目标，没有任何的依赖，称这样的目标为伪目标
最常用的伪目标clean

clean:
	rm *.elf *.bin …

当执行make clean的时候，make命令会自动执行clean所指定的语句
rm *.elf *.bin

程序文件包含的三大段以及链接过程
  代码段text / 数据段data / bss
类比 : 
  各种.o类似于做饭的原材料 
  .elf文件就是饭菜
  arm...ld 就是厨师 
  -nostartfiles -nostdlib -Ttext=0x48000000 -emain 菜单 
链接脚本功能 
  本质就是一个菜单 
  制定一个链接的规则, 将来给arm...ld使用,
  将来arm...ld链接器根据规则将.o文件链接生成.elf文件 
  链接脚本给链接器来使用 
  本质上就是一个文本文件, 以.lds结尾
链接脚本语法(链接规则)
  cp 4.0 5.0 -fr 
  添加上对于链接脚本的支持

```c
ENTRY(main)
SECTIONS{
    . = 0x48000000;
    .text :
    {
        main.o(.text)
        *.o(.text)
    }
    .data :
    {
        *.o(.data)
    }
    .bss :
    {
        *.o(.bss)
    }
}
```

//告诉链接器将来shell.elf入口函数为main 
ENTRY(main)

//告诉链接器将来shell.elf的各个段的内容如下
SECTIONS
{
    // 告诉链接器将来shell.elf链接的起始地址为0x48000000
    . = 0x48000000;
	// 告诉链接器将来 shell.elf 中代码段的内容如下 
	// .text后面一定要跟空格
	.text : 
	{
    	// 告诉链接器将main.o的内容首先链接,因为
    	// 入口函数main函数在main.c中
    	main.o(.text)
		// 然后将其他的.o都链接进来
		*.o(.text)
	}
	// 告诉链接器将来数据段内容如下
	// .data后面跟空格
	.data :
	{
    	*.o(.data)
	}
	// 告诉链接器将来bss段内容如下
	// .bss后面跟空格
	.bss :
	{
    	*.o(.bss)
	}
}
保存退出

Makefile  
```
OBJ=main.o led.o uart.o strcmp.o cmd.o iic.o mma8653.o itoa.o
OBJCOPY=arm-cortex_a9-linux-gnueabi-objcopy
LD=arm-cortex_a9-linux-gnueabi-ld
GCC=arm-cortex_a9-linux-gnueabi-gcc
shell.bin:shell.elf 
  $(OBJCOPY) -O binary shell.elf shell.bin
shell.elf:$(OBJ)
  $(LD) -nostartfiles -nostdlib -Tshell.lds -o shell.elf $(OBJ)
%.o:%.c 
  $(GCC) -nostdlib -c -o $@ $<
clean:
  rm *.elf *.bin *.o 
```

优化shell裸板程序

目前裸板程序的问题
随着操作的硬件数量增多，whi1e(1)中的代码将会变得极其繁琐

typedef
使用形式
	typedef char s8;
	typedef int s32;
	typedef float f32;
	给函数指针类型起别名
	typedef void(*cb_t)(void);
	用法：cb_t callback = led_init;//定义函数指针callback指向led_init函数
			callback();// == led_init();

优化shell裸板程序
裸板程序中有一个“事物”：命令有一个属性和一个方法
属性：命令的名称，例如："led on"
方法：命令对应的执行函数，例如：led_on()
采用面向对象编程思想，c程序，立马想到结构体

//声明描述命令操作函数的数据类型
typedef void (*cb t)(void);
//声明描述命令的数据结构
typedef struct cmd{
	//命令的名称
	char *name;
	//命令的执行函数
	cb t callback;
}cmd t;

利用上述数据结构来初始化命令对象
cmd t cmd {"led on",led on};
cmd.cal1back();//相当于调用了1edon函数
cmd t cmd tbl[]={
	{"1edon",led on},/开灯命令对象
	{"1 ed off",1 ed off)//关灯命令对象
}

代码：
cmd.h

```c
#ifndef CMD_H
#define CMD_H
typedef void (*cb_t)(void);
typedef struct _cmd{
    const char* name;
    cb+t callback;
}cmd_t;
cmd_t* find_cmd(const char* name);q
#endif
```

cmd.c

```c
#include "cmd.h"
#include "led.h"
#include "strcmp.h"
#include "mma8653.h"
cmd_t cmd_tbl[] = {
    {"led on",led_on},
    {"led off",led_off},
    {"showid", mma8653_id},
    {"showacc", mma8653_acc}
};
#define ARRAY_SIZE(x)	(sizeof(x) / sizeof(x[0]))
cmd_t* find_cmd(const char* name){
    int i;
    for(i = 0;i<ARRAY_SIZE(cmd_tbl);i++){
        if(!my_strcmp(cmd_tbl[i].name,name))
            return &cmd_tbl[i];
    }
    return 0;
}
```

main.c
```c
#include "uart.h"
#include "led.h"
#include "strcmp.h"
#include "cmd.h"
static char buf[32];
void main(void){
    cmd_t* pcmd;
    uart_init();
    led_init();
    while(1){
        uart_puts("\n shell#");
        uart_gets(buf, 32);
        pcmd = find_cmd(buf);
        if(pcmd == 0)  
            uart_puts("please input valid command!\n");
        else
            pcmd->callback();
    }
}
```

I2C总线
GPIO/UART/I2C/SPI、1—Wire/.\.\.

七个字：两线式串行总线
两线式：说明CPU和外设只需要两根信号线
分别是数据线SDA(serial data)和时钟控制信号线SCL(serial clock)
	数据线SDA：用于实现CPU和外设之间的数据交互
		当CPU给外设发送数据，SDA由CPU来控制
		当外设给CPU发送数据，SDA由外设来控制
	时钟控制信号线SCL：用于实现CPU和外设之间的数据同步
		特点：会定期发送一个方波信号
			方波信号：高电平和低电平的宽度是一样的，时间是一样的
				一个高电平 + 一个低电平 的时间 = 一个时钟周期
				一个低电平 + 一个高电平 的时间 = 一个时间周期
		工作原理：底放高取
			CPU给LM77发送数据1和0：
			1.CPU在CLK为低电平的时候，将数据1放到数据线SDA上(拉高SDA)，然后LM77在同周期的CLK为高电平的时候，从数据线SDA上取出数据1(判断SDA高低电平)
			2.CPU在CLK为低电平的时候，将数据0放到数据线SDA上(拉低SDA)，然后LM77在同周期的CLK为高电平的时候，从数据线SDA上取出数据0(判断SDA高低电平)
	SCL和SDA必须分别连接了上拉电阻，默认SCL好SDA为高电平

​	总结：
​		谁配输出谁控制
​		谁配输入谁释放
​		可以同时配输入
​		不能同时配输出

串行：说明CPU和外设数据传输的时候是一个bit一个bit的传输
	注意：
		1.I2C总线传输从高位开始
			0x95(1001 0101)
				UART：1010 1001 从低位开始
				I2C： 1001 0101 从高位开始
		2.I2C总线传输是一次传输一个字节，如果是多个字节需要分拆传输
		3.由于I2C总线具有时钟控制信号线，I2C总线传输时一个时钟周期传输一个b1t位
		4.数据传输的速度由SCL来决定，SCL又有外设来决定
	总结：
		一次一字节
		一位一周期
		传输从高位
		速度看时钟
		时钟看外设

总线 :说明SDA和SCL两根信号线可以挂接多个外设 
	理论上也可以挂接多个CPU, 比较少见, 一般都是一个CPU挂接多个外设

I2C总线中的相关概念
START信号：起始信号
如果CPU要访问外设，CPU首先要向总线上发送一个START信号
该信号只能由CPU发起
信号时序：SCL为高电平，SDA由高电平向低电平跳变 -> START信号

STOP信号：结束信号
如果CPU要停止对外设的访问，CPU最后向总线发送一个STOP信号
该信号只能由CPU发起
信号时序：SCL为高电平，SDA由低电平向高电平跳变 -> STOP信号

读写位(R/W):用于表示CPU是向外设写入数据还是从外设读取数据
lbit
如果CPU要向外设写入数据，R/W = 0
如果CPU要从外设读取数据，R/W = 1

设备地址：用于表示外设在同一个总线上的唯一性
同一个总线上，每个外设都会有一个唯一的设备地址
类似于外设的身份证号
如果CPU要访问某个外设，CPU只需要向[总线]发送某个外设的设备地址即可
类似于老师叫某个同学的名字
如果外设在当前总线上，会向CPU回复的
注意：设备地址的有效位数为7位（常见）或者10位（几乎见不到）

问：如何来确定设备地址呢？
答：通过外设的芯片手册来确定
举例：获取MMA8653三轴加速度传感器的设备地址
打开mma8653fcr1.pdf - p17:
The MMA8653FC's standard slave address is 0011101 or 0x01D
MMA8653的设备地址 = 0011101(7bit) = 0x1D(十六进制)

问题：
概念1.I2C总线传输是一次传输一个字节
概念2.设备地址的有效位数为7位（常见）
	将设备地址好读写位结合起来

读设备地址 = 设备地址<<1 | 1; //1读写位，读
写设备地址 = 设备地址<<1 | 0; //0读写位，写
举例：MMA8653的设备地址 0X1D
	0011101 << 1 = 00111010
	00111010 | 1 = 00111011
	00111011 = 0011101 1
	[7:1] = 设备地址
	[0] = 读写位
设备地址：向哪个设备发送/接收
读写位：读/写

ack信号：应答信号 acknowledge
用于表示I2C总线数据传输是否发生了错误
有效位数为1位，低电平有效

片内寄存器：任何I2C总线接口的外设，芯片内部都有寄存器，这些寄存器都可以用于暂存数据，CPU通过I2C访问外设芯片就是在访问芯片内部的寄存器，且这些片内寄存器的地址编译都是从0x00开始的，例如：0x00 0x01 0x02 0x03.\.\. ，片内寄存器的地址和bit含义需要看外设的芯片手册
不能以地址指针的方式来访问，严格按照I2C的时序进行访问

以地址指针的方式来获取寄存器上的数据：控制器内部的寄存器 内存
unsigned char data = *(char\*)0x02;//no

举例：CPU读取MMA8653三轴加速度传感器的ID

1. 打开mma8653fcr1.pdf
   打开P19,得到片内寄存器的信息
   在片内寄存器的地址为0x0d这个寄存器中存储了其ID=0X5A
2. 读取的流程CPU要读取一个字节数据，单字节读取时序：Single--byteread
   [CPU]发送开始信号
   [CPU]发送写设备地址
   [外设]回复ack信号
   [CPU]发送要访问片内寄存器地址0x0d给外设
   [外设]回复ack信号
   [CPU]发送开始信号
   [CPU]发送读设备地址
   [外设]回复ack信号
   [外设]将0x0d寄存器中的数据0X5A发送给CPU
   [CPU]接收完数据后，CPU给外设一个高电平的NAK信号
   [CPU]发送结束信号
3. CPU向MMA8653片内寄存器0x0e写入数据0x55,单字节写入
   [CPU]发送开始信号
   [CPU]发送写设备地址
   [外设]回复ack信号
   [CPU]发送目的寄存器地址0x0e
   [外设]回复ack信号
   [CPU]发送数据0x55到片内寄存器0x0e中
   [外设]回复ack信号
   [CPU]发送结束信号

分析：
抓取时序图，重点分析第一个ck信号，该信号表示外设是否存在于总线上，
硬件没问题：
外设在总线上，ack信号就是低电平
外设不在总线上，ack信号就是高电平

如果ck信号为高电平，但是后续的读写时序完全正确，只能说问题势必是硬件问题

实战演练I2C总线相关内容

1. 确认需求
   读取MA8653三轴加速度传感器的ID并且在上位机打印输出
   
2. 确认硬件信息
   确认硬件位置
   1. 底板原理图
      SCL MCU SCL 2
      SDA MCU SDA 2
      
   2. 核心板原理图
      SCL MCU SCL 2 AC18 CPU GPIOD6/SCL2
      SDA -MCU SDA 2 -AB19 CPU GPIOD7/SDA2
      此时应该选择为SDA2和SCL2功能
      寄存器：GPIODALTFN0/1
      
   3. 芯片手册
      1. 外设芯片手册
         1. 片内寄存器
            1>片内寄存器的基地址0x0d
            2>片内寄存器的bit含义0x5a -> 设备ID
         2. 时序图
            单字节读取时序图
         
      2. CPU芯片手册
         1. S5P6818支持3路I2C总线
            MMA8653连接到了第三路
         
         2. 支持主设备发送，主设备接收，从设备发送，从设备接收模式
            CPU去挂接其他的CPU-当前CPU
            主设备 - 主设备发送/接收
            从设备 - 从设备发送/接收
         
         3. SCL的是时钟源为PCLK(uboot初始化为200Mz)
            降频 - 50Hz
         
         4. 主设备发送
            1. 配置I2C控制器为主发送模式
            2. 将从设备地址写入到虹2CDS寄存器中
            3. 将0XF0写入到I2 CSTAT寄存器，启动START信号
            4. I2CDS寄存器中的从设备地址会被自动发送到SDA上
            5. 通过判断中断是否到来判断外设是否给ACK信号
            6. 如果要接着发送新的数据，只需要将数据放到虹2CDS寄存器中即可
            7. 清除中断到来的标志位为0让I2C控制器继续将I2CDS寄存器中的数据放到sDA上
            8. 如何直到数据是否发送成功，通过判断中断是否到来
            9. 如果中断到来，代表数据发送成功，如果继续发送继续5-9
            10. 如果要停止发送数据，只需要向I2 CSTAT写入0xd0启动stop信号
            11. 清除中断到来的标志位为0
         
         5. 主设备接收
         
            1. 配置为主接收模式
            2. 将从设备地址写入到I2CDS寄存器中
            3. 进0XB0写入到I2CSTAT寄存器，启动STAT信号
            4. I2CDS寄存器中的重生地址会被自动发送到SDA上
            5. 通过判断中断是否到来判断外设是否给ACK信号
            6. 如果中断到来，并且还向继续读取数据，直接从I2CDS寄存器中读取新的数据即可
            7. 然后清除中断到来位，让I2C控制器继续从SDA上读取数据放到I2CDS
            8. 如何直到12cDS寄存器中的数据是否准备就绪
               判断中断是否到来
               重复5-8
            9. 如果要停止读取，向I2 CSTAT写入0X90,启动stop信号
            10. 清除中断到来位
         
         6. 寄存器分析
         
            1. I2C寄存器
               I2CCON 0XC00A6000
               [3:0] = 0xf
               公式：SCL = I2C CLK / (bit[3:0] + 1)
               让时钟最慢摒除硬件影响
               SCI最小
                   I2CCLK最小
                   bit[3:0]最大1111=0xf
               [4]判断中断是否到来
               READ : 0 - 中断么有到来
               	   1 - 中断已经到来
               WRITE: 0 - 将该b1t清0清除中断到来位
               判断数据是否发生成功或者接收完毕的轮训代码：
               whi1e( ! (I2CCON & (1<<4)));
               [5] = 1 使能中断产生
               [6] = 1 I2C CLK = PCLK / 256 PCLK = 200MHZ
               [7] = 0 让CPU产生一个无效的ACK信号(高电平)
                   = 1 让CPU产生一个有效的ACK信号(低电平)
               [8]
                   read : 0，没有中断到来
                   write : 1，清除中断到来
         
               默认I2CCON[4]=0
               CPU将数据a发送给了MMA8653,发送成功，中断到来，此时I2CCON[4]=1，数据发送不成功，不清除中断到来位，I2CCON[4]=1
               CPU将数据b发送给了MMA8653,怎么去判断数据是否发送成功呢
               无法判断
               发送失败 - 没有中断到来 - 之前没清除 - I2CCON[4]=1
               发送成功 - 有中断到来 - 之前没清除 - I2CCON[4]=1
               发送失败，发送成功 -> I2CCON[41]=1
         
               I2CSTAT 0XCO0A6004
               [0] = 0:收到了ACK信号
               	= 1:没收到ACK信号
               [4] = 1 使能X/TX
               [5]
               	READ: 0 空闲
               		  1 忙
               	WRITE: 0 产生ST0P信号
               		   1 产生START信号
               	写入0-stop信号-不再传数据-空闲
               	写入1-start信号-传递数据-忙
               [7:6]
               	10：主接收模式
               	11：主发送模式
         
               I2CDS 0XC00A600C
               数据移位寄存器
               bit[7:0] 保存要发送的数据或者接收到的数据
         
               I2CLC 0XCO0A6010
               [1:0]
               	SDA数据线的延时长度
               	01/10/11都可以不可以为00
               	10:10*1/200MHz
               [2] = 1 使能滤波器(取出干扰信号)
         
            2. GPIO寄存器 结果
               GPIODALTFNO 0XC001D020
               [15:12]=0101
                   配置为SCL2和SDA2功能
         
            3. 时钟寄存器
               P302
               I2CCLKENB 0XC00B0000
               [3]=1永远使能PCLK时钟源
            
            4. 复位寄存器
               IPRESETO 0XC0012000
               [22] = 1 复位
                    = 0 取消复位

代码：
iic.h

```c
#ifndef IIC_H
#define IIC_H
#define GPIOD_ALTFN0    *((volatile unsigned int *)0xC001D020)
#define I2CCLKENB2      *((volatile unsigned int *)0xC00B0000)
#define IPRESET0        *((volatile unsigned int *)0xC0012000)
#define I2CCON          *((volatile unsigned int *)0xC00A6000)
#define I2CSTAT         *((volatile unsigned int *)0xC00A6004)
#define I2CADD          *((volatile unsigned int *)0xC00A6008)
#define I2CDS           *((volatile unsigned int *)0xC00A600C)
#define I2CLC           *((volatile unsigned int *)0xC00A6010)
extern void iic_init(void);
extern void iic_start(unsigned char, unsigned int);
extern void iic_stop(void);
extern void iic_tx(unsigned char* , unsigned char);
extern void iic_rx(unsigned char* , unsigned char);
#endif
```

iic.c
```c
#include "iic.h"
void iic_init(void){
    GPIOD_ALTFN0 &= ~(0XF<<12);
    GPIOD_ALRFN0 |= (1<<14);
    GPIOD_ALTFN0 |= (1<<12);
    I2CCLKENB2 |= (1<<3);
    IPRESET0 &= ~(1<<22);
    IPRESET0 |= (1<<22);
    I2CCON = (1<<8)|(1<<7)|(1<<6)|(1<<5)|0X0F;
    I2CLC = (1<<2)|1;
}
void iic_start(unsigned char slave_addr, unsigned int rdwr){
    I2CSTAT |= (1<<4);
    I2CCON |= (1<<8)|(1<<7)|(1<<5);
    if(rdwr == 1){
        I2CSTAT &= ~(3<<6);
        I2CSTAT |= (1<<6);
        slave_addr = (slave_addr<<1) | 1;
    }else{
        I2CSTAT |= (3<<6);
        slave_addr = (slave_addr<<1) | 0;
    }
    I2CDS = slave_addr;
    I2CCON &= ~(1<<4);
    I2CSTAT |= (1<<5);
    whi1e( ! (I2CCON & (1<<4)));
    if(I2CSTAT & 0X01)
        uart_puts("\nnot received ACK signal");
}
void iic_stop(void){
    I2CSTAT &= ~(1<<4);
}
void iic_tx(unsigned char* buf, unsigned char len){
    int count = 0;
    for(; count < len; count++){
        I2CCON &= ~(1<<4);
        I2CDS = buf[count];
        while( ! (I2CCON & (1<<4)) );
        if(I2CSTAT & 0X01)
            uart_puts("\nnot received ACK signal");
    }
}
void iic_rx(unsigned char* buf, unsigned char len){
    int count = 0;
    while(count < len){
        if(count == (len-1)){
            I2CCON &= ~(1<<7);
        }
        I2CCON &= ~(1<<4);
        while(!(I2CCON&(1<<4)));
        buf[count] = I2CDS;
        count++;
    }
}
```

分析我外设的芯片手册
单字节读取的时序
CPU - mma8653是外设
开始信号 + 设备地址(读/写)
	读设备地址
		if(R/W == 1)
			主接收模式
		if(R/W == 0)
			主发送模式
发送开始信号
主设备发送：0XF0 -> I2CSTAT
	将0XF0写入到I2CSTAT，启动了开始信号，配置了主发送模式
主设备接收：0XB0 -> I2CSTAT

结束信号
CPU向外设发送单/多个字节
CPU从外设读取单/多个字节

读取数据
读取到最后一个字节，CPU回复nack 不应答 - 无效的ack信号

mma8653.h
```c
#ifndef _IIC_H
#define _IIC_H
extern void mma8653_id(void);
extern void mma8653_acc(void);
#endif
```

mma8653.c
```c
#include "mma8653.h"
#include "iic.h"
#include "itoa.h"
#define SLA_ADDR	(0X1D)
#define WFLAG		(0)
#define RFLAG		(1)
void mma8653_read(unsigned char reg_addr,unsigned char* buf,int len){
    iic_start(SLA_ADDR， WFLAG);
    iic_tx(&reg_addr, 1);
    iic_start(SLA_ADDR, RFLAG);
    iic_rx(buf, len);
    iic_stop();
}
#define WHO_AM_I	(0X0D)
void mma8653_id(void){
    unsigned char id = 0;
    unsigned char itoa_buf[11] = {0};
    mma8653_read(WHO_AM_I, &id, 1);
    itoa(itoa_buf, id);
    uart_puts("\nID:");
    uart_puts(itoa)
}
void mma8653_write(unsigned char reg_addr,const unsigned char* buf,int len){
    iic_stop(SLA_ADDR, WFLAG);
    iic_tx(&reg_addr, 1);
    iic_tx(buf, len);
    iic_stop();
}
#define CTRL_REG1	(0X2A)
void enter_active(void){
    unsigned char data = 0;
    mma8653_read(CTRL_REG1, &data, 1);
    data |= 1;
    mma8653_write(CTRL_REG1, &data, 1);
}
void print_acc(char *buf){
    int x = 0;
    int y = 0;
    int z = 0;
    int flag = 0;
    char itoa_buf[11] = {0};
    x = buf[0]<<24|buf[1]<<16;
    x = x>>22;
    if(x&(1<<31)){
        flag = 1;
        x = 0 -x;
    }
    itoa(itoa_buf, x);
    uart_puts("\nX: ");
    if(flag){
        uart_puts("-");
        flag = 0;
    }
    uart_puts(itoa_buf);
    y = buf[2]<<24|buf[3]<<16;
    y = y>>22;
    if(y&(1<<31)){
        flag = 1;
        y = 0 -y;
    }
    itoa(itoa_buf, y);
    uart_puts("\nY: ");
    if(flag){
        uart_puts("-");
        flag = 0;
    }
    uart_puts(itoa_buf);   
    z = buf[4]<<24|buf[5]<<16;
    z = z>>22;
    if(z&(1<<31)){
        flag = 1;
        z = 0 -z;
    }
    itoa(itoa_buf, z);
    uart_puts("\nZ: ");
    if(flag){
        uart_puts("-");
        flag = 0;
    }
    uart_puts(itoa_buf);
}
#define OUT_X_MSB	(0X01)
void mma8653_acc(void){
    unsigned char acc[6] = {0};
    enter_active();
    mma8653_read(OUT_X_MSB, acc, 6);
    print_acc(acc);
}
```

明确-寄存器0X0D中存储了想要读取的设备ID-0X5A
明确一数据在传递的时候传递的都是字符串
hel1o,world -> 实际 -> "he11o,wor1d"
读取到了0x5a -> 发送 -> 将数据0x5a转换为字符串 -> 字符'Z' -> 发送
如果去发送数据的话 -> 将数据0x5a -> 'Z' -> 发送给上位机

转换 -> 数字 -> 字符串
 0x5a -> "0x5a"

itoa.h

```c
#ifndef _ITOA_H
#define _ITOA_H
extern void itoa(char buf[], unsigned int num);
#endif
```

itoa.c
```c
#include "itoa.h"
void itoa(char* buf, unsigned int num){
    buf[0] = '0';
    buf[1] = 'X';
    int i = 9;
    unsigned int tmp;
    i = 9;
    while(num){
        tmp = num % 16;
        if(tmp >= 10){
            buf[i] = tmp - 10 + 'A';
        }else{
            buf[i] = tmp + '0';
        }
        i--;
        num /= 16;
    }
    while(i >= 2){
        buf[i--] = '0';
    }
    buf[10] = 0;
}
```

升级 - 读取mma8653三轴加速度的加速度值

底板原理图 + 核心板原理 + 芯片手(外设/CPU)
外设芯片手册：寄存器 + 读写时序

外设芯片手册分析

1. 10bit - 三轴加速度传感器读取到的数据为10bit

2. 芯片的车辆范围
   +-2g(默认)
   +-4g
   +-8g
   gravity 重力加速度
   在测量范围不断增大的时候，减小了灵敏度

3. 芯片的工作模式
   OFF      关机
   STANBY   待机
   ACTIVE   运行

   读取mma8653的设备id
   待机 - 可以读取
   运行 - 可以读取
   读取mma8653的加速度值
   待机 - 不可以读取
   运行 - 可以读取

4. 可以有8bit/10bit的加速度值
   F_READ - 8bit数据
   fast read快速读取

5. mma8653的设备id为0x1d

6. 读写时序

7. 该芯片大概有50个寄存器
   0x00 ~ 0x31
   0x31 = 49
   0 ~ 49 = 50个寄存器

8. 设备ID存储在0X0D寄存器中

9. 加速度值存储在哪个寄存器中
   X方向:
   0x01寄存器[7:0] 是10bit数据的高8位 [9:2]
   0x02寄存器[7:6] 是10bit数据的低2位 [1:0]
      |9-------2|1-------0|   X方向10bit数据
      \|0x01[7:0]|0x02[7:6]|
   Y方向:
   0x03寄存器[7:0] 是10bit数据的高8位 [9:2]
   0x04寄存器[7:6] 是10bit数据的低2位 [1:0]
      |9-------2|1-------0|   Y方向10bit数据
      \|0x03[7:0]|0x04[7:6]|
   Z方向:
   0x05寄存器[7:0] 是10bit数据的高8位 [9:2]
   0x06寄存器[7:6] 是10bit数据的低2位 [1:0]
      |9-------2|1-------0|   Z方向10bit数据
      \|0x05[7:0]|0x06[7:6]|
   如果要读取mma8653的设备加速度值, 需要读取地址0x01-0x06寄存器的值

10. 1
    如果想要读取0x01~0x06的值，将寄存器的地址给0x01,然后读取6个字节即可
    char buf[6]=(0);
    mma8653 read(0x01,buf,6)

如何让mma8653处于运行状态：
CTRL_REG1	0X2A
[0] 0:STANBY模式
    1:ACTIVE模式

只能向mma8653的寄存器中写入数据

如果想要修改0X2A寄存器中的值~写入
mma8653 write()

组合起来
void print acc(char){}

shell框架
输入命令 - led on -> 下位机 - 匹配 - 成功调用 - 失败，告知输入有效的指令

ARM汇编
对于ARM处理器的认识
其他的处理器架构
PowerPC	飞思卡尔
FPGA:Xilinx
DSP:TI
ARM
	汽车电子：飞思卡尔
	工控：Ateml
	网络：博通 马维尔
	消费类电子：高通 华为 三星 苹果
mips:北京君正
单片机：51 MSP430 STM32(arm架构 - 32 - 低端)

ARM的定义
认为arm是一类处理器
认为arm是一家公司(软银)
ARM公司不生产芯片，制作IP授权

ARM核版本划分
| 大版本 |       小版本        |   具体芯片    |
| :----: | :-----------------: | :-----------: |
| ARMV4  |       ARM720T       |    S3C4510    |
|        |       ARM920T       | S3C2440(三星) |
| ARMV5  |        ARM10        | S3C6410(三星) |
| ARMV6  |        ARM11        | S3C6640(三星) |
| ARMV7  |    ARM cortex-A8    | S5PV210(三星) |
|        | ARM cortex-M3/M0/M4 |   STM32(ST)   |
| ARMV8  |   ARM cortex-A53    |    S5P6818    |

A系列 - 高性能
R系列 - 低延迟
M系列 - 低功耗，性能较低

三级指令流水线
就是给CPU核下发的数据运算的命令或者外设访问(读/写)的命令
指令存在于编译链接生成的二进制可执行文件中
指令的二进制文件在运行时存在于内存中
指令在内存中分布
CPU核 - ARM状态 - ARM指令
每条指令对应的内存地址简称指令存储地址
	该地址由链接器来指令

CPU核如果从内存取出要执行的指令
	以地址的方式访问，获取指令到CPU核

指令流水线的目的
以流水线的方式从内存获取指
提高CPU核运行指令的效率和速度

ARMV7版本的三级指令流水线：取指F - 解码D - 执行E
取指：CPU核内部的取指器从内存中取出指令
解码：起初取出的指令，CPU核无法识别，需要CPU核内部解码器进行解码
		对指令进行"翻译"解码
执行：翻译完成 CPU核最终处理

执行 != 运行
运行：F - D-  E
执行：已经昨晚了F - D,直接去E
执行第n条指令，此时正在取指第n+2条指令

初始PC寄存器
切记：PC是ARM核内部的一个特殊寄存器
功能：永远只能存储取指器要取的那条指令的内存地址
如果当前处于arm状态下，要执行的指令地址+0x08 = 要取指的那条指令地址

三级流水线 - ldr指令
功能：从外设中将数据加载到CPU核内部
流水线为五级：F - D - E - M - W
M：memory，访存，线找到外设的地址
W：write back，写回，将数据从外设拷贝到CPU核内部

三级流水线 - bl指令
功能：跳转指令，能够让CPU核跑到某个指定的地址上去运行
流水线为五级：F - D - E - L - A
L：linkret，跳转之前保存返回地址(下一条指令的内存地址)
A：adjust，CPU核正式跳转到指定的地址上去运行
将来CPU要返回，只需要取出之前保存的内存地址，然后跳转到保存的地址上即可

中断指令
正在执行进程A，中断到来，执行中断对应的处理函数，执行结束，CPU核继续回到进程A去执行
流水线:DI-EI-L-A 
DI: D IRQ , 对中断进行解码 
EI: E IRQ , 处理中断 
L: linkret 保存返回地址 
A: adjust 正式跳转
切记: 当CPU执行一条指令的时候, 保证指令执行完之后才能处理中断

编程 - 遵循一个原则 - 不要让编译器为难 
执行add指令 - 执行到一半 - 中断到来 - 不执行 - 处理中断 - 回来 - 面对执行到一半的add指令
执行add指令 - 执行到一半 - 中断到来 - 先把add指令执行完 - 处理中断 - 回来 - 下一条指令 

五级流水线 
将所有的指令后面添加了M和W两个处理过程 
任何指令五级流水线:F - D - E - (M) - W
只有ldr指令由M处理, 其与指令没有M, 但是处理M的时钟周期必须存在

ARM核相关特性

1. ARM核的7种工作模式
2. CPU核两种工作状态
   ARM状态 - AR指令 - 每条指令4字节 - 32位
   THUMB状态 - THUMB状态 - 每条指令2字节 - 16位
3. ARM寄存器
   1. ARM处理器寄存器分类
      特殊功能寄存器：位于控制器内部
        通过地址指针来访问，不能随便保存数据
        数量庞大
      ARM寄存器：位于ARM核内部
        通过名称来访问
        数量为37个
   2. ARM寄存器(37)
      寄存器到下为4字节
      31个通用寄存器
        名称：r1 r2 r3 \.\.\.r14 r15
        r15：pc寄存器，永远保存取指器要取指的指令地址
        r14：lr寄存器，永远只能保存返回地址(bl/中断)
        r13：sp寄存器，永远只能保存栈指针
        r0-r3：参数传递
        r4-r11：保存局部变量
        r12：暂存寄存器ip
      6个状态寄存器
      一个cpsr：保存当前程序运行的状态信息
      五个spsr：用来备份cpsr，当有模式切换使用到cpsr
   3. 详解cpsr寄存器
      [4:0] mode 记录当前程序运行的时候，CPU核的工作模式
      [5] 状态位
               = 0 arm状态
               = 1 thumb状态
      [6] FIQ禁止位
                  = 0 使能FIQ
                  = 1 禁止FIQ
      [7] IRQ禁止位
                  = 0 使能IRQ
                  = 1 禁止IRQ
      [28] V位(场景不多)
      [29] C carry判折数据计算是否发生了溢出现象
           \- 0 没有进位/有借位
           \- 1 有进位/没有借位
           unsigned long data 0xffffffff;
           data+=10;//需要进一位 - 无法存储了 - C = 1
      [30] Z zero用于记录当前程序运算结果是否为0
           0 运算结果非0
           1 运算结果为0
      [31] N negative
           用于记录当前程序运算结果是否为负数
           0 不为负数
           1 为负数
   4. 影响cpsr的NZCV的场景
      1. 指令后加s，运算结果影响cpsr的NZCV位
      2. 比较测试指令后
         cmp本质：做减法运算
         ne，eq - 条件码
             eq z = 1，条件满足 - 执行该语句
             ne z = 0，条件满足 - 执行该语句

ARM寄存器
1个cpsr
5个spsr	备份状态信息

spsr使用场景
usr模式：运行程序 - helloworld - FIQ中断到来
fiq模式：运行中断函数 - 结束 - 返回被打断位置

ARM核的七种异常
异常：随机触发的事件

|         异常          | CPU核入口地址 |          触发场景           |
| :-------------------: | :-----------: | :-------------------------: |
|     复位异常(SVC)     |     0X00      |          系统复位           |
| 未定义指令异常(UNDEF) |     0X04      | CPU核执行了一个不认识的指令 |
|    软中断异常(SCV)    |     0X08      |      CPU核执行swi指令       |
|    取指异常(ABORT)    |     0X0C      |          取指F失败          |
|  数据处理异常(ABORT)  |     0X10      |          访存M失败          |
|   IRQ中断异常(IRQ)    |     0X18      |       触发IRQ中断信号       |
|   FIQ中断异常(FIQ)    |     0X1C      |       触发FIQ中断信号       |

异常处理过程(8步骤)
场景：usr -> 进程 -> 中断到来FIQ -> FIQ -> 恢复 -> usr:进程
硬件四步骤

1. 备份当前进程的cpsr到spsr
   [cpsr] = usr -> [spsr] = usr
2. 设置cpsr的对应bit
   [4:0] = 0b10001 FIQ模式
   [5] = 0 ARM状态
   [7:6] = 11 禁止CPU核响应FIQ好IRQ中断电信号，此时cpsr记录当前FIQ模式下的状态信息
3. 保存返回地址到lr寄存器，lr = pc - 4
   中断执行完毕，想要返回被打断任务继续执行，从lr寄存器中取出来保存的地址即可
4. 最终给pc赋值，也就是设置pc等于某个异常的入口地址
   结果：CPU核会直接跑到异常入口地址上去运行(F - D - E - (M) - W)
   开启软件进行异步处理异常的进程
   注意：只要给pc赋值，就是让CPU核跑到该地址上去运行

软件四步骤第一步 :
1.问:pc = 0x00/04/08/0c/10/18/1c, CPU核跑到这些入口地址去做什么呢?
答:是要执行程序, 七个地址对应着七个程序 
问:要运行的程序如何和这七个地址关联起来 
答:通过链接器进行关联 
  举例 :vim start.s //汇编文件 
    b  reset_func 
    b  undef_func 
    b  swi_func 
    b  fetch_abort_func 
    b  data_abort_func 
    b  . @类似于while(1), 死循环
    b  irq_func 
    b  fiq_func 

​    reset_func :
​      add ..
​      sub ..
​      orr ..
​      b . 

​    undef_func :
​      add ..
​      sub ..
​      orr ..
​      b . 
​    ...
​    保存退出 
​    arm...as -nostdlib -c start.s -o start.o 
​    arm...as:汇编器 .s->.o 
​    arm...ld -nostdlib -nostartfiles -Ttext=0x00 -o start.elf start.o 
​      -Ttext=0x00 汇编代码的起始地址从0x00开始
​    arm...objcopy -O binary start.elf start.bin 
​    结论 :
​    指令            指令地址
​    b  reset_func       0x00
​    b  undef_func       0x04
​    b  swi_func        0x08
​    b  fetch_abort_func    0x0c
​    b  data_abort_func     0x10
​    b  . @类似于while(1)    0x14
​    b  irq_func        0x18
​    b  fiq_func        0x1c

b就是bl的简化版本,不带返回的跳转 
b  reset_func 跳转到reset_func去运行,也是一条指令占据4字节

​    将来某个异常触发, CPU核跑到对应的异常入口地址上运行 
​    就会自己编写的指令代码(b fiq_func)
​    将以上8行2列的表格 - 异常向量表 
​    且在软件处理异常之前, 需要在内存中提前建立异常向量表 
​    建立过程:tftp 0x00 start.bin 

开发板 : 0x40000000 - 0x7fffffff 
  地址可以偏移 

2.保护现场 
  问:何为保护现场?
  答:将由异常大端的任务使用的arm寄存器备份到栈中,压栈 
  问:为何要压栈?
  答:
  举例 
  usr模式 : 运行程序 - helloworld - FIQ中断到来
    mov r0, #1 
    mov r1, #1 
    cmp r0, r1 @alu_out = r0 - r1, 影响cpsr Z=1  -> 中断到来 
    addeq r2, r0, r1  @该指令执行
  fiq模式 : 运行中断函数 - 结束 - 返回被打断位置 
    mov r0, #100
    mov r1, #567 
    ...
  usr模式 : 运行程序 - helloworld
    继续执行下一条语句 :

​    addeq r2, r0, r1  @会执行 r2 = r0 + r1 = 100 + 567 = 667
  处理方式
​    CPU核在执行fiq_func里面的代码之前将之前进程使用的arm寄存器保存到栈中即可 
​    将来中断执行结束, 再从栈中奖之前保存的数据恢复到ARM寄存器中 

  举例
    r0=1,r1=1  => 存储到栈中 
    中断执行 
    r0=1,r1=1  <= 从栈中恢复 
3.根据用户需求调用异常处理函数(一般使用C语言实现)
  举例
    bl uart_puts @汇编调用C语言实现对应的功能 
4.恢复现场, 状态恢复和跳转返回 
  何为恢复现场 - 将之前在栈中保存的数据恢复到arm寄存器中, 给被打断的进程使用 
  状态恢复 : cpsr = spsr 
    spsr - usr -> cpsr 
  跳转返回 : pc = lr 
  至此
    CPU又返回到原先被打断的位置继续运行 
  至此
    异常处理完毕

arm汇编编程框架
汇编文件.s或者.S作为结尾，不区分大小写
汇编代码使用@注释

start.c		添加汇编代码
.text	  	@修饰代码段的开始
.code 32   	@修饰 采用arm指令 == .arm
.global start  @声明全局标签 start

start: @标签start中的内容
	mov r0, #10	 @r0 = 10
	ldr r1, =3	  @r1 = 3
	add r0, r0, r1  @r0 = r0 + r1 = 10 + 3 = 13
	b   .		   @. 当前位置 跳转到当前位置 类似于while(1)

.end	@修饰代码段的结果

这些以.开头的内容 - 伪操作 - 不参与代码的运行，只是起到修饰作用

何为标签 - 类似于c语言中的函数和变量
	究竟表示函数 - 变量 - 取决于内容

main: @此时的标签就是 - 函数
	mov r0, #1 
	mov r1, #1000 
    sub r0, r0, r1 

data: @此时的标签就是 - 变量 
    .int 0x12345678

main: @变量 
    .int 100 

sub.s
```assembly
.text
.code 32
.global _start
_start:
	mov r0, #2		 @r0 = 2
	mov r1, #9		 @r1 = 9
	subs r0, r0, r1  @r0 = r0 - r1 = -7 N = 1(CPSR[31] = 1)
	b	.
.end
@判断cpsr的N位是否为1，判断数据计算机是否正确
```

arm-cortex_a9-linux-gnueabi-as -g -o sub.o sub.s
arm\.\.\.as  汇编器
-g		如果使用gdb调试，添加调试信息
sub.o	 二进制代码，调试信息
arm-cortex_a9-linux-gnueabi-ld -o sub sub.o

qemu：仿真器，仿真运行程序
sudo apt-get install qemu

使用gdb调试器对程序进行调试
在当前终端下
qemu-arm -g 1234 sub 启动模拟器qemu运行sub可执行程序
						-g 1234	指定端口号 1234

ctrl + shift + T 打开另一个终端，在用一个目录
arm-cortex_a9-linux-gnueabi-gdb sub
出现gdb命令行终端(gdb)执行以下调试命令
(gdb)target remote localhost:1234
(gdb)l		 //list 列出代码信息
(gdb)b 5  	 //设置断点 break point
(gdb)s		 //step 下一步，让CPU去执行下一条语句
(gdb)info reg  //information register 查看到寄存器的信息, 三列内容

| 寄存器的名 | 寄存器的值(HEX) | 寄存器的值(DEC)    |
| ---------- | --------------- | ------------------ |
| r0         | 0xfffffff9      | -7                 |
| r1         | 0x9             | 9                  |
| r2-r9      | 0x0             | 0                  |
| r10        | 0x8064          | 32868              |
| r11-r12    | 0x0             | 0                  |
| sp         | 0xf6fff060      | 0xf6fff060         |
| lr         | 0x0             | 0                  |
| pc         | 0x8060          | 0x8060 <_start+12> |
| cpsr       | 0x80000010      | -2147483632        |

(gdb)quit 	 //退出gdb调试命令, qemu模拟器也会跟着退出

target	 目标
remote	 远程
localhost  本地地址 127.0.0.1
1234	   端口号，让其大于1234即可

案例：编写汇编实现1+2+\.\.\.+10
sum.s

~~~assembly
.text 
.code 32
.global _start 
_start :
    mov r0, #0      @将求和的结果放到r0寄存器中
    mov r1, #10     @定义循环变量, 意味着需要循环10次

_sum :
    add r0, r0, r1  @依次的将r1中的数据放到r0寄存器中 
    sub r1, r1, #1  @r1=r1-1 
    cmp r1, #0  @比较r1的值和0的值的大小, 当r1==0, 结束循环; 反观之, 继续循环
                @alu_out = r1 - 0 
    bne _sum    @r1 != 0, alu_out!=0, Z=0, ne条件成立, 执行b _sum 
                @r1 == 0, alu_out ==0,Z=1, ne条件不成立, 不执行b _sum, 循环到此结束
                @将加和的结果放到了r0中 
    b   .
.end 
~~~

arm...as -g -o sum.o sum.s 
arm...ld -o sum sum.o 
从1加到10的值, 存储到了r0寄存器中, qemu+gdb调试, 查看r0的值
想要让循环变量r1从10减到0, 然后将其减的过程中的数字,加到r0寄存器中
