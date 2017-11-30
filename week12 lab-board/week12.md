该实验报告的GitHub链接为：
https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/week12.md

### 嵌入式系统导论实验报告

------

|  姓名  |    学号    |  班级  |     电话      |        邮箱         |
| :--: | :------: | :--: | :---------: | :---------------: |
|  林丹  | 15352204 | 1509 | 13609754376 | lindan113@163.com |

------

### 1. 实验题目: week12：lab-board

### 2. 实验过程与结果

#### 2.1 InputOutput_4C123project

实验要求：需要修改程序改变按键对应的驱动灯颜色，做个前后对比，并作调试分析。

##### 2.1.1 修改代码前，simulator仿真：

- 点击左上角的`Build`进行编译，或者按快捷键`F7`；
- 选择导航栏`Debug` ，点击start Debug；
- 选择导航栏`Peripheral`，点击Port F；

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab7%20Keil%20InputOutput_4C123asm/images/start%20Debug.png?raw=true"/>



- 点击左上角的`Run`进行编译，或者按快捷键`F5`；


- 开始运行，没有按下按键：

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab7%20Keil%20InputOutput_4C123asm/images/%E5%BC%80%E5%A7%8B%E8%BF%90%E8%A1%8C%20%E4%B8%8D%E6%8C%89%E4%B8%8B.png?raw=true"/>



- 点击TExas Launchad 中的输入按键，观察输出端口。

  - 按下switch1：Port 2的LED亮蓝色BLUE

    <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab7%20Keil%20InputOutput_4C123asm/images/%E6%8C%89%E6%8C%89%E9%94%AE1.png?raw=true"/>

    ​

  - 按下switch2：Port 1的LED亮红色RED

    <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab7%20Keil%20InputOutput_4C123asm/images/%E6%8C%89%E6%8C%89%E9%94%AE2.png?raw=true"/>

    ​

  - 两个按键都按下：Port 3的LED亮绿色GREEN

    <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab7%20Keil%20InputOutput_4C123asm/images/%E9%83%BD%E6%8C%89%E4%B8%8B.png?raw=true"/>

    ​

  ##### 2.1.2 修改代码，使得输出灯的颜色有改变。并在实验板上debug看结果。

- 代码修改为：

  <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/%E4%BF%AE%E6%94%B9%E7%81%AF%E9%A2%9C%E8%89%B2%E6%94%B9%E4%BB%A3%E7%A0%81.png?raw=true"/>

- 实验现象：

  - 按下switch1：LED亮粉色PINK

    <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/pink.jpg?raw=true"/>

    ​

  - 按下switch2：LED天蓝色SKY_BLUE

    <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/blue.jpg?raw=true"/>

    ​

  - 两个按键都按下：LED亮黄色YELLOW

    <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/yellow.jpg?raw=true"/>

    ​



##### 2.1.3 代码分析

初始化Init流程图：

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/init%E6%B5%81%E7%A8%8B%E5%9B%BE.png?raw=true"/>

main函数流程图：

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/main%E6%B5%81%E7%A8%8B%E5%9B%BE.png?raw=true"/>



#### 2.2 Not_gate project

实验要求：

- 修改输出位到portd.2后，与之前的portd.3截图做对比，
- 然后修改程序，使得脉宽有所变化，也把修改前后截图做对比，并做程序修改分析对比



- 修改代码前运行，逻辑分析仪看输出结果：

![img](https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/PD3%E8%BE%93%E5%87%BA.png?raw=true)



##### 2.2.1 修改输出位到portD.2

修改代码分析：

GPIO_PORTD_DIR_R设置输入输出。

0x08 = 0000 1000 ，即portD.3为输出

0x04 = 0000 0100 ，即portD.2为输出

```assembly
    LDR R1, =GPIO_PORTD_DIR_R       ; R1 = &GPIO_PORTD_DIR_R
    LDR R0, [R1]                    ; R0 = [R1]
    ORR R0, R0, #0x04               ; R0 = R0|0x04 (make PD2 output)
;	ORR R0, R0, #0x08               ; R0 = R0|0x08 (make PD3 output)
	BIC R0, R0, #0x01				; R0 = R0 & NOT(0x01) (make PD0 input)
    STR R0, [R1]                    ; [R1] = R0
```



GPIO_PORTD_AFSEL_R作用是disable alter function.

涉及到输入输出端口。PD2,PD0就是0x04+0x01=0x05

```assembly
; 4) regular port function
    LDR R1, =GPIO_PORTD_AFSEL_R     ; R1 = &GPIO_PORTD_AFSEL_R
    LDR R0, [R1]                    ; R0 = [R1]
    BIC R0, R0, #0x05               ; R0 = R0&~0x05 (disable alt funct on PD2,PD0)
;	BIC R0, R0, #0x09               ; R0 = R0&~0x09 (disable alt funct on PD3,PD0)
    STR R0, [R1]                    ; [R1] = R0
```



GPIO_PORTD_DEN_R作用是允许数字信号。

涉及到输入输出端口。PD2,PD0就是0x04+0x01=0x05

```assembly
; 5) enable digital port
    LDR R1, =GPIO_PORTD_DEN_R       ; R1 = &GPIO_PORTD_DEN_R
    LDR R0, [R1]                    ; R0 = [R1]
	ORR R0, R0, #0x05               ; R0 = R0|0x09 (enable digital I/O on PD2,PD0)
;   ORR R0, R0, #0x09               ; R0 = R0|0x09 (enable digital I/O on PD3,PD0)
    STR R0, [R1]                    ; [R1] = R0
```



函数主体

```assembly
Start
    BL  GPIO_Init					;函数调用GPIO_Init，初始化 
    LDR R0, =GPIO_PORTD_DATA_R  	  ; R0=GPIO_PORTD_DATA_R，R0得到portD的data
```

```assembly
loop
	LDR R1,[R0]			; R1=地址为GPIO_PORTD_DATA_R的值，即R1得到portD的data
	AND	R1,#0x01		; Isolate PD0，得到portD.0输入的值,结果R1=0x00
	EOR	R1,#0x01		; NOT state of PD0 read into R1，取反，结果R1=0x01=0000 0001
	STR R1,[R0]			; STR R1,[R0],把R1写到端口portD，其中PD3-1 = 0，PD0是输入不变。
;   *************** 此时产生了方波的下降沿 ***************
	nop				    ; 延时
	nop					; 延时
;   此处的时间影响了方波处于“0”的时间长

	LSL R1,#2			; SHIFT left negated state of PD0 read into R1，使得R1=0x40=00000100
;	LSL R1,#3			; SHIFT left negated state of PD0 read into R1
	STR R1,[R0]			; Write to PortD DATA register to update LED on PD3
	; 把R1(00000100)写到端口portD，其中PD2 = 1
;   *************** 此时产生了方波的上升沿 ***************
    B loop               ; unconditional branch to 'loop',继续循环
```

逻辑分析仪结果：

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/PD2%E8%BE%93%E5%87%BA%E6%96%B9%E6%B3%A2.png?raw=true"/>

可以看到输出端口修改为PD2, PD3输出为0



##### 2.2.2 修改程序，使得脉宽有所变化。

```assembly
loop
	LDR R1,[R0]
	AND	R1,#0x01		; Isolate PD0
	EOR	R1,#0x01		; NOT state of PD0 read into R1
	STR R1,[R0]			; STR R1,[R0],
;   *************** 此时产生了方波的下降沿 ***************	
	nop
	nop
;  增加了STR等语句，STR是存储数据，一个耗时操作	
	STR R1,[R0]
	STR R1,[R0]
	STR R1,[R0]
	nop
	nop
	nop
	nop
	LSL R1,#2			; SHIFT left negated state of PD0 read into R1
;	LSL R1,#3			; SHIFT left negated state of PD0 read into R1
	STR R1,[R0]			; Write to PortD DATA register to update LED on PD3
;   *************** 此时产生了方波的上升沿 ***************	
    B loop                          ; unconditional branch to 'loop'
```

实验结果：

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/PD2%E8%BE%93%E5%87%BA%E6%96%B9%E6%B3%A2%E6%94%B9%E8%84%89%E5%AE%BD.png?raw=true"/>







#### 2.3. 补充：week11 InputOutput_4C123asm project

##### 2.3.1 代码分析

main.s文件是我们要分析的汇编语言代码。

```assembly
Start
BL  PortF_Init                  ; initialize input and output pins of Port F
```

BL 跳转指令, 跳转到函数Port_Init;

函数Port_Init进行端口的初始化，设置GPIO口，定义输入输出，分析略；

执行**loop循环**，以检测按键输入，

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

这里的LDR不是arm指令，而是伪指令。将FIFTHSEC 的值，即1066666，赋给寄存器R0,

> 这个时候LDR与MOV数据传送指令很相似，只不过MOV指令后的立即数是有限制的。这个立即数必须是0X00-OXFF范围内的数经过偶数次右移得到的数，所以MOV用起来比较麻烦，因为有些数不那么容易看出来是否合法。



```assembly
BL  PortF_Input                 ; read all of the switches on Port F
```

跳转到PortF_Input函数，读取开关信息；

**PortF_Input函数**

```assembly
LDR R1, =GPIO_PORTF_DATA_R ; pointer to Port F data
LDR R0, [R1]               ; read all of Port F
AND R0,R0,#0x11            ; just the input pins PF0 and PF4
BX  LR                     ; return R0 with inputs
```

分析：

1. 第一句：`GPIO_PORTF_DATA_R  EQU 0x400253FC	;`

   0x400253FC为GPIODATA数据寄存器地址;

   LDR伪指令，把GPIODATA数据寄存器地址给了R1寄存器，GPIO_PORTF_DATA_R为Port F数据寄存器地址，R1的值和GPIO_PORTF_DATA_R的值一样，类比C语言，可以理解为R1和GPIO_PORTF_DATA_R指针都指向同一块内容，即Port F端口数据；

2. 第二句：`LDR R0, [R1]`

   将R1的地址的内容赋给R0，即R0得到了Port F端口的输入数据；

3. 第三句：`AND R0,R0,#0x11`

   `0x11`转为二进制数为`0001 0001` ，可以看到只有第0位和第4位是1其他都是0，注意输入的switch1和switch2分别为端口Port 0和Port 4；将得到的端口输入数据与`0x11`相与，只保留了第0位和第4位数据，其他位为0，意味着R0得到按键输入数据；

4. 第四句：`BX  LR`

   `BX  LR ` 是带状态切换的跳转指令，类似于`mov  PC,LR`，下一步

   LR就是连接寄存器(Link Register)，在ARM体系结构中LR可以是用来保存子程序返回地址，即返回函数调用；

PortF_Input函数执行完，继续**回到loop循环**

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

| 动作                  |   R0寄存器   |
| :------------------ | :-------: |
| 按下switch1（Port 4）   | 0000 0001 |
| 按下switch2（Port 0）   | 0001 0000 |
| 同时按下switch1和switch4 | 0000 0000 |
| 都不按                 | 0001 0001 |

总结，按键按下端口输出为0，没有按下时候为1，

`BEQ`为相同则跳转指令，根据按键按下信息，跳转到相应的函数；

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



**函数PortF_Output**

```assembly
LDR R1, =GPIO_PORTF_DATA_R ; pointer to Port F data
STR R0, [R1]               ; write to PF3-1
BX  LR                    
```

1. 第一句：`LDR R1, =GPIO_PORTF_DATA_R ;`

   LDR伪指令，把GPIO_DATA数据寄存器地址给了R1寄存器，R1的值和GPIO_PORTF_DATA_R的值一样，类比C语言，可以理解为R1指针指向和GPIO_PORTF_DATA_R同一块内容，即Port F数据；

2. 第二句：`STR R0, [R1]  `

   将R0的值付给R1地址的值，R1地址和Port F数据端口地址一样，所以Port F端口的输出数据修改为BLUE；

   当按下switch1后，地址R1保存的值为BLUE，即0x04（0000 0100），把Port 3的LED灯点亮。

3. 第三句：`BX  LR`

   返回函数调用；

执行完函数PortF_Output，**继续执行循环loop**：

```assembly
B   loop
```

跳转到loop，执行新的一次循环，隔一段时间（delay）就检测时候有输入改变；



##### 2.3.2 修改代码：使得三个灯都亮。

- 不修改代码，正常情况下，不会出现三个灯都亮。

  再看一遍loop循环函数：

  ```assembly
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
  ```

  在四个比较跳转语句之后，还有三句，意味着如果R0不是以上四种情况的时候，会执行下面的语句：

  ```assembly
  MOV R0, #(RED+GREEN+BLUE)       ; R0 = (RED|GREEN|BLUE) (all LEDs on)
  BL  PortF_Output                ; turn all of the LEDs on
  B   loop
  ```

  但是实际上对于两个按键（只有开和关两种状态），我们一共四种情况，所以这段代码不会执行。


- 修改功能，在没有按下按键的时候，三个灯都亮。

  修改nopressed函数：

  ```assembly
  nopressed
    MOV R0, #(RED+GREEN+BLUE)       ; R0 = (RED|GREEN|BLUE) (all LEDs on)
  ; MOV R0, #0                      ; R0 = 0 (no LEDs on)
    BL  PortF_Output                ; turn all of the LEDs off
    B   loop
  ```

  实验结果：没有按下按键的时候三个灯都亮了。

  <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab7%20Keil%20InputOutput_4C123asm/images/%E4%B8%89%E4%B8%AA%E7%81%AF%E4%BA%AE.png?raw=true"/>

  ​

### 3. 实验心得

- **实验遇到的问题：PortF_Output函数中，通过指令`STR R0, [R1]`点亮灯的时候，GPIO_PORTF_DATA_R端口的数据是BLUE (0000 0100) 吗？**

  在PortF_Output函数中设置断点，观察结果：

  <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab7%20Keil%20InputOutput_4C123asm/images/%E7%96%91%E9%97%AE.png?raw=true"/>

  结果看到R0寄存器和DATA的值不一样，此时DATA数据寄存器的值是0x05不是BLUE（0x04），

  虽然`STR R0, [R1]  `将R0地址的值付给了R1，但是在数据初始化的时候，设定了端口是输入还是输出，输入端口没有办法修改，所以即使“指针”R1和GPIO_PORTF_DATA_R指向同一块地址，也不能改变端口数据寄存器第0位和第4位的值。

  ​

- **指令BL和B的区别**

  - B或BL指令引起处理器转移到“子程序名”处开始执行。两者的不同之处在于BL指令在转移到子程序执行之前，将其下一条指令的地址拷贝到LR（链接寄存器，R14）。
  - 由于BL指令保存了下条指令的地址，因此在子程序最后通过`BX LR`或者是`MOV PC, LR`可实现子程序的返回。而B指令则无法实现子程序的返回，只能实现单纯的跳转。


- LDR指令总结

  - LDR加载指令

    LDR指令的格式为：

    ```LDR{条件}  目的寄存器，<存储器地址>```

    LDR指令用亍从存储器中将一个32位的字数据传送到目的寄存器中。该指令通常用亍从存储器中读取32位的字数据到通用寄存器，然后对数据迕行处理。当程序计数器PC作为目的寄存器时，指令从存储器中读取的字数据被当作目的地址，从而可以实现程序流程的跳转。

    指令示例：

    ```assembly
    LDR R0，[R1]         ;将存储器地址为R1的字数据读入寄存器R0。
    LDR R0，[R1，R2]  	;将存储器地址为R1+R2的字数据读入寄存器R0。
    LDR R0，[R1，＃8]   	;将存储器地址为R1+8的字数据读入寄存器R0。
    LDR R0，[R1，R2]		 ;将存储器地址为R1+R2的字数据读入寄存器R0,幵将新地址R1＋R2写入R1。
    LDR R0，[R1，＃8]  	 ;将存储器地址为R1+8的字数据读入寄存器R0，幵将新地址R1＋8写入R1。 
    LDR R0，[R1]，R2  	  ;将存储器地址为R1的字数据读入寄存器R0，幵将新地址R1＋R2写入R1。
    LDR R0，[R1，R2，LSL＃2]  ;将存储器地址为R1＋R2×4的字数据读入寄存器R0，并将新地址R1＋R2×4写入R1。
    LDR R0，[R1]，R2，LSL＃2  ;将存储器地址为R1的字数据读入寄存器R0，幵将新地址R1＋R2×4写入R1。”
    ```

    ​

  - LDR伪指令

    LDR伪指令的形式是`LDR Rn, =expr;`

    指令示例

    ```assembly
    LDR R1, =GPIO_PORTF_DATA_R ; pointer to Port F data
    LDR R0, [R1]               ; read all of Port F
    ```

  ​	在前面代码分析中已经提过，不再赘述：


- 修改了代码但是debug之后没有看到应有的现象，结果和原来一样。
  - 原因：修改了代码要先rebuild！！！


- 逻辑分析仪的使用：

  - 查看端口输出，发现1几乎看不到。原因在于纵坐标的取值范围。

    <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/analog.png?raw=true"/>

    在纵坐标，右键，修改为“Bit”显示。之后就可以看到正常的方波了。

    <img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/week12%20lab-board/images/%E6%94%B9%E4%B8%BAbit%E7%9C%8B.PNG?raw=true"/>