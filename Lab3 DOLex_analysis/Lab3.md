该实验报告的GitHub链接为：
[https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/Lab3.md](https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/Lab3.md)

## 嵌入式系统导论实验报告
-------

|  姓名  |  学号  |  班级  |  电话  |  邮箱  |
| :--: | :--: | :--: | :--: | :--: |
|  林丹 | 15352204 | 1509 | 13609754376  | lindan113@163.com |

-----

### 1.实验题目：DOL 实例分析& 编程


1. 修改example2，让3个square模块变成2个, tips:修改xml的iterator
2. 修改example1，使其输出3次方数，tips:修改square.c

### 2.实验结果

#### 2.1 修改example2 
- 编译运行原来的example2，结果如下：

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/%E8%BF%90%E8%A1%8C%E7%AC%AC%E4%BA%8C%E4%B8%AA%E4%BE%8B%E5%AD%90.png?raw=true"/>

- 进入example2文件夹修改example2.xml

```
$	cd /home/linda/Downloads/DOL_LAB/dol/examples/example2
$	sudo gedit example2.xml
```

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/%E4%BF%AE%E6%94%B9exmple2%E4%BB%A3%E7%A0%81.png?raw=true"/>

- 重新编译example2

```
$	cd /home/linda/Downloads/DOL_LAB/dol
$	sudo ant -f build_zip.xml all clean
$	sudo ant -f build_zip.xml all
```
注：因为之前编译过，所以要先clean再build一次。

- 运行修改的example2.

```
$	cd build/bin/main
$	sudo ant -f runexample.xml -Dnumber=2
```

- 运行结果。（两次迭代）
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/T1%E4%BF%AE%E6%94%B9%E7%AC%AC%E4%BA%8C%E4%B8%AA%E4%BE%8B%E5%AD%90.png?raw=true"/>

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/T1%E4%BF%AE%E6%94%B9%E7%AC%AC%E4%BA%8C%E4%B8%AA%E4%BE%8B%E5%AD%90dot.png?raw=true"/>

注：dot图中样本3个square模块变成2个。


#### 2.2 修改example1

- 编译运行原来的example1，结果如下：

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/%E8%BF%90%E8%A1%8C%E7%AC%AC%E4%B8%80%E4%B8%AA%E4%BE%8B%E5%AD%90.png?raw=true"/>

- 修改square.c文件

```
$	cd /home/linda/Downloads/DOL_LAB/dol/examples/example1/src
$	sudo gedit square.c
```

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/T2%E4%BF%AE%E6%94%B9exmple1.png?raw=true"/>

- 重新编译example1

```
$	cd /home/linda/Downloads/DOL_LAB/dol
$	sudo ant -f build_zip.xml all clean
$	sudo ant -f build_zip.xml all
```
注：因为之前编译过，所以要先clean再build一次。

- 运行修改的example1.

```
$	cd build/bin/main
$	sudo ant -f runexample.xml -Dnumber=1
```

- 运行结果。（求立方）

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/T2%E4%BF%AE%E6%94%B9exmple1%E7%BB%93%E6%9E%9C.png?raw=true"/>

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/T2%E4%BF%AE%E6%94%B9exmple1%20dot%E5%9B%BE.png?raw=true"/>
注：dot图没有变。

### 3.实验心得

#### 编译过程（简述）

Compile:

<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/%E7%BC%96%E8%AF%91_2.PNG?raw=true"/>



论文内容：

**Overview of the DOL programming environment.**

The contributions of this article are indicated by the shaded boxes: generation and calibration of formal performance analysis models.
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/figure1_overview.png?raw=true"/>


1. The design cycle starts with a system specification consisting of an application, an architecture, and a mapping specification. 
	- The application specification based on the dataflow process network model of computation is platform-independent and needs to be related to a concrete architecture by explicit mapping.



2. The second step in the design flow is the automated generation of a functional simulation of the application. 
	- The functionals imulation allows for testing and debuging the parallel application code with standard debugging tools on a standard workstation.


3. Once the application is functionally correct, it can be mapped onto the target platform. 
	- Based on the architecture and the mapping specification, software synthesis generates the corresponding binaries. This basically involves the generation of mapping-dependent source code for the processors, compilation, and the linking to the platform-specific libraries and runtime environment.


**Application Programming, Architecture and Mapping Modeling**
The dataflow model can be seen as a coordination model which allows for the consideration the programming of a parallel system as the combination of two distinct activities: the computation comprising a number of processes involved in manipulating data and the coordination describing the connections of processes.


<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/figure2(a).png?raw=true"/>
Figure2(a)

- The DOL architecture specification consists of basic system components, their attributes, and the way they are connected, that is, computation resources like programmable processors and hardware IPs, or communication components like buses and NoCs. Figure 2(a), for instance, represents a simplified view of the MPARM architecture.
 

- Specify dataflow applications
	- DOL uses two distinct languages, namely C/C++ to program actors and XML for describing the topology of the dataflow process network. Examples for both are shown in Listing 1 and Listing 2. The choice of these languages has pragmatic reasons, as using C/C++ allows to reuse of existing legacy code, and XML is easy to handle, due to the large number of available tools. 
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab3%20DOLex_analysis/images/figure2_XML_C.png?raw=true"/>


- The mapping describes the binding of processes and software channels to architecture components and the scheduling on shared resources. 
	- For example, scheduling policies like time-division multiple access, (non-)preemptive fixed priority, or earliest deadline first and the corresponding parameters, can be specified.
	- Customized XML schemata are used for describing the format of architecture and mapping specifications. These specifications are used as inputs for both software synthesis and analysis model generation.

