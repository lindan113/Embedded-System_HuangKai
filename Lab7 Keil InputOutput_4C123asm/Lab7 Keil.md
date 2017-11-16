
##嵌入式系统导论实验报告
-------

|  姓名  |    学号    |  班级  |     电话      |        邮箱         |
| :--: | :------: | :--: | :---------: | :---------------: |
|  林丹  | 15352204 | 1509 | 13609754376 | lindan113@163.com |

-----


###1.实验题目

lab7：keil安装和仿真

###2.实验过程与结果

点击左上角的`Build`进行编译，或者按快捷键`F7`；

选择导航栏`Debug` 



代码分析



```assembly
Start
BL  PortF_Init                  ; initialize input and output pins of Port F
```

BL 跳转指令, 跳转到函数Port_Init;



函数Port_Init进行端口的初始化，设置GPIO口，定义输入输出；

执行loop循环，以检测按键输入，

```assembly
loop
  LDR R0, =FIFTHSEC               ; R0 = FIFTHSEC (delay 0.2 second)
  BL  delay                       ; delay at least (3*R0) cycles
  BL  PortF_Input                 ; read all of the switches on Port F
  CMP R0, #0x01                   ; R0 == 0x01?
  BEQ sw1pressed                  ; if so, switch 1 pressed
  CMP R0, #0x10                   ; R0 == 0x10?
  BEQ sw2pressed                  ; if so, switch 2 pressed
  CMP R0, #0x00                   ; R0 == 0x00?
  BEQ bothpressed                 ; if so, both switches pressed
  CMP R0, #0x11                   ; R0 == 0x11?
  BEQ nopressed                   ; if so, neither switch pressed
                                  ; if none of the above, unexpected return value
  MOV R0, #(RED+GREEN+BLUE)       ; R0 = (RED|GREEN|BLUE) (all LEDs on)
  BL  PortF_Output                ; turn all of the LEDs on
  B   loop
```




```assembly
FIFTHSEC           EQU 1066666      ; approximately 0.2s delay at ~16 MHz clock
```

```assembly
LDR R0, =FIFTHSEC               ; R0 = FIFTHSEC (delay 0.2 second)
```

这里的LDR不是arm指令，而是伪指令。

将FIFTHSEC 的值，即1066666，赋给寄存器R0,



这个时候与MOV数据传送指令很相似，只不过MOV指令后的立即数是有限制的。这个立即数必须是0X00-OXFF范围内的数经过偶数次右移得到的数，所以MOV用起来比较麻烦，因为有些数不那么容易看出来是否合法。



```assembly
BL  PortF_Input                 ; read all of the switches on Port F
```

跳转到PortF_Input函数，读取开关信息；





```assembly
LDR R1, =GPIO_PORTF_DATA_R ; pointer to Port F data
LDR R0, [R1]               ; read all of Port F
AND R0,R0,#0x11            ; just the input pins PF0 and PF4
BX  LR                     ; return R0 with inputs
```


分析：

```assembly
GPIO_PORTF_DATA_R  EQU 0x400253FC	;
```

0x400253FC为GPIODATA数据寄存器地址;

LDR伪指令，把GPIODATA数据寄存器地址给了R1寄存器，GPIO_PORTF_DATA_R为Port F数据寄存器地址，R1的值和GPIO_PORTF_DATA_R的值一样，类比C语言，可以理解为R1和GPIO_PORTF_DATA_R指针都指向同一块内容，即Port F端口数据；

`LDR R0, [R1]`将R1的地址的内容赋给R0，即R0得到了Port F端口的输入数据；

`0x11`转为二进制数为`0001 0001` ，可以看到只有第0位和第4位是1其他都是0，注意输入的switch1和switch2分别为端口Port 0和Port 4；

`AND R0,R0,#0x11`就讲得到的端口输入数据与`0x11`相与，只保留了第0位和第4位数据，其他位为0，意味着R0得到按键输入数据；



`BX`是带状态切换的跳转指令，`BX  LR ` 左右类似于`mov  PC,LR`，下一步

LR就是连接寄存器(Link Register)，在ARM体系结构中LR可以是用来保存子程序返回地址，即返回函数调用；



PortF_Input函数执行完，继续回到loop循环

```assembly
CMP R0, #0x01                   ; R0 == 0x01?
BEQ sw1pressed                  ; if so, switch 1 pressed
CMP R0, #0x10                   ; R0 == 0x10?
BEQ sw2pressed                  ; if so, switch 2 pressed
CMP R0, #0x00                   ; R0 == 0x00?
BEQ bothpressed                 ; if so, both switches pressed
CMP R0, #0x11                   ; R0 == 0x11?
BEQ nopressed                   ; if so, neither switch pressed
```

CMP判断R0的值，即按键的输入，

|                     |           |
| ------------------- | --------- |
| 按下switch1（Port 4）   | 0000 0001 |
| 按下switch2（Port 0）   | 0001 0000 |
| 同时按下switch1和switch4 | 0000 0000 |
| 都不按                 | 0001 0001 |

总结，按键按下端口输出为0，没有按下时候为1，

`BEQ`为相同则跳转指令，根据按键按下信息，跳转到相应的函数；



假设什么按键都没有按下，





















以`sw1pressed`函数为例；

```assembly
sw1pressed
  MOV R0, #BLUE                   ; R0 = BLUE (blue LED on)
  BL  PortF_Output                ; turn the blue LED on
  B   loop
```
```assembly
RED       EQU 0x02
BLUE      EQU 0x04
GREEN     EQU 0x08
```



| LED变量 | 十六进制 |    二进制    | LED的端口 |
| :---: | :--: | :-------: | :----: |
|  RED  | 0x02 | 0000 0010 | Port 1 |
| BLUE  | 0x04 | 0000 0100 | Port 2 |
| GREEN | 0x08 | 0000 1000 | Port 3 |



`MOV`数据传输指令将BLUE变量的值付给寄存器R0，

BL指令，跳转到函数PortF_Output

补充：指令BL和B的区别

> B或BL指令引起处理器转移到“子程序名”处开始执行。两者的不同之处在于BL指令在转移到子程序执行之前，将其下一条指令的地址拷贝到LR（链接寄存器，R14）。
>
> 由于BL指令保存了下条指令的地址，因此在子程序最后通过`BX LR`或者是`MOV PC, LR`可实现子程序的返回。而B指令则无法实现子程序的返回，只能实现单纯的跳转。



函数PortF_Output

```assembly
LDR R1, =GPIO_PORTF_DATA_R ; pointer to Port F data
STR R0, [R1]               ; write to PF3-1
BX  LR                    
```
LDR伪指令，把GPIO_DATA数据寄存器地址给了R1寄存器，R1的值和GPIO_PORTF_DATA_R的值一样，类比C语言，可以理解为R1指针指向和GPIO_PORTF_DATA_R同一块内容，即Port F数据；

`STR R0, [R1]  `将R0的值付给R1地址的值，R1地址和Port F数据端口地址一样，所以Port F端口的输出数据修改为BLUE；

当按下switch1后，地址R1保存的值为BLUE，即0x04（0000 0100），把Port 3的LED灯点亮。



疑问：通过指令`STR R0, [R1]`点亮灯的时候，GPIO_PORTF_DATA_R端口的数据是0000 0100吗？

不是，此时DATA数据寄存器的值是0x05不是BLUE（0x04），

虽然`STR R0, [R1]  `将R0地址的值付给你R1，但是在数据初始化的时候，设定了端口是输入还是输出，输入端口没有办法修改，所以即使“指针”R1和GPIO_PORTF_DATA_R指向同一块地址，也不能改变端口数据寄存器第0位和第4位的值。

























###3.实验心得
