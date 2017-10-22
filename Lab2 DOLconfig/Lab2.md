该实验报告的GitHub链接为：


## 嵌入式系统导论实验报告
-------

|  姓名  |  学号  |  班级  |  电话  |  邮箱  |
| :--: | :--: | :--: | :--: | :--: |
|  林丹 | 15352204 | 1509 | 13609754376  | lindan113@163.com |

-----


### 1. 实验题目： DOL配置

#### 配置基本过程：

- 虚拟机安装Ubuntu


- 安装一些必要的环境(ubuntu为例)：
```
$	sudo apt-get update
$	sudo apt-get install ant
$ 	sudo apt-get install openjdk-7-jdk
$	sudo apt-get install unzip
```
下载文件
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab2%20DOLconfig/images/%E4%B8%8B%E8%BD%BD%E6%96%87%E4%BB%B6wget.png?raw=true"/>

- 解压文件

新建dol的文件夹 
```$	sudo mkdir dol```
将dolethz.zip解压到 dol文件夹中
```$	sudo unzip dol_ethz.zip -d dol```
解压systemc
$	sudo tar -zxvf systemc-2.3.1.tgz
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab2%20DOLconfig/images/%E8%A7%A3%E5%8E%8B%E6%96%87%E4%BB%B6.png?raw=true"/>

解压后进入systemc-2.3.1的目录下
```$	cd systemc-2.3.1```
新建一个临时文件夹objdir
```$	sudo mkdir objdir```
进入该文件夹objdir
```$	cd objdir```
- 运行configure
```$	sudo ../configure CXX=g++ --disable-async-updates```

**运行configure之后的截图**
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab2%20DOLconfig/images/%E8%BF%90%E8%A1%8Cconfigure%E4%B9%8B%E5%90%8E.png?raw=true"/>

- 编译
```$	sudo make install```
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab2%20DOLconfig/images/sudo%20make%20install.png?raw=true"/>

编译完后文件目录如下
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab2%20DOLconfig/images/%E7%BC%96%E8%AF%91%E5%AE%8C%E5%90%8E%E6%96%87%E4%BB%B6%E7%9B%AE%E5%BD%95.png?raw=true"/>
记录当前的工作路径
```$	sudo pwd```
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab2%20DOLconfig/images/%E5%BD%93%E5%89%8D%E7%9A%84%E5%B7%A5%E4%BD%9C%E8%B7%AF%E5%BE%84.png?raw=true"/>

进入刚刚dol的文件夹```
```$	cd ../dol```
修改build_zip.xml文件
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab2%20DOLconfig/images/%E4%BF%AE%E6%94%B9build_zip.xml%E6%96%87%E4%BB%B6.png?raw=true"/>

然后是编译
```$	sudo ant -f build_zip.xml all```
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab2%20DOLconfig/images/sudo%20ant%20-f%20build_zip.xml%20all.png?raw=true"/>

### 2. 实验结果
#### 运行第一个例子

进入build/bin/mian路径下
```$	cd build/bin/main```
然后运行第一个例子
```$	sudo ant -f runexample.xml -Dnumber=1```
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab2%20DOLconfig/images/%E8%BF%90%E8%A1%8C%E7%AC%AC%E4%B8%80%E4%B8%AA%E4%BE%8B%E5%AD%90.png?raw=true"/>


- dot文件截图
<img src="https://github.com/lindan113/Embedded-System_HuangKai/blob/master/Lab2%20DOLconfig/images/dot%E6%96%87%E4%BB%B6%E6%88%AA%E5%9B%BE.png?raw=true"/>

### 3. 实验心得


1.一开始自己的系统是中文系统，导致无法正确运行example1.中文系统会因为时间问题，导致runexample.xml文件编译出错。把系统改回英文系统，重新配置一次，正确。

2.一开始直接在文件里面打开xml文件双击打开后修改完无法保存
普通的用户权限无法进行修改，需要root权限。
可以在命令行用sudo gedit 文件名 指令来修改。

3.dot文件
可以直接到对应文件夹，双击打开，打开时会提醒安装xdot工具。安装后就能打开。
