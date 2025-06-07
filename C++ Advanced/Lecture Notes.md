<h1 style="text-align:center;">C++ 进阶 —— 01 University</h1>

主要内容是 Linux 下的网络编程。

# 一、Linux 入门

## 1.1 Linux 环境搭建

### 1.1.1 虚拟机安装

VMware 17.6.3

1. 切换到管理员模式：`su root`
2. 加上写权限：`chmod u+w /etc/sudoers`
3. 修改该文件中的内容：`vim /etc/sudoers`
4. 使用上下键找到 `root  ALL=(ALL)` 开头一行，按 `yy` `p`
5. 复制出来的那行，按 `a` 键进入编辑模式
6. 按 `Esc` `Shift + :` `wq`
7. 切换镜像源

### 1.1.2 安装远程连接终端与远程连接 Centos

1. Xshell 是什么：一款 Windows 平台上的 SSH (Secure Shell) 的客户端软件，能够完成远程连接和管理 Linux 服务，支持 SSH1 和 SSH2 协议，提供了安全的加密连接，能够防止数据被窃取和篡改。
2. Xshell 下载地址：https://www.xshell.com/zh/free-for-home-school/
3. 创建一个连接到 Linux 服务器

### 1.1.3 Vim 使用点回顾

1. Vim 是什么：Vim 是 Linux 系统下的文本编辑器，可以对不同文本进行编辑，功能是对文件内容的编辑、保存、更改等操作。
2. Vim 有三种模式：命令行模式、插入模式、底行模式
3. 如何打开 Vim 编辑器
   1. 先 touch 一个文件，然后使用指令：`vi/vim + 文件名` 即可，这种方式打开文件，无论是否编辑文件，都会在文件系统中产生一个普通文件。
   ```
   touch demo.cpp
   vim demo
   ```
   2. 直接使用 `vi/vim + 不存在的文件名`，如果对文本内容进行更改，则保存退出后，则会在文件系统上产生一个普通文件，如果没有更改数据，保存后退出，则不会在文件系统上创建该文件。
    ```
    vim demo.cpp
    ```
4. 命令行模式
   1. 使用 `vim + 文件名`， 刚进入编辑器的模式就是命令行模式。
   2. 命令行模式的作用：主要完成对文本内容整体的复制、粘贴、剪切、删除、光标移动等操作。
   3. 如何进入命令行模式：刚进入编辑器的模式就是命令行模式，其他模式进入命令行模式只需要按 `Esc` 键即可。
   4. 主要操作：
      1. `yy`：复制当前行的文本内容
      2. `nyy`：复制从光标所在行及以后的 $n$ 行文本内容
      3. `p`：将剪切板中的内容进行粘贴
      4. `dd`：删除光标所在的一行文本内容（剪切）
      5. `ndd`：删除光标所在行及以后的 $n$ 行数据（剪切）
      6. `u`：撤销上一步操作
      7. `ctrl + r`：反撤销
      8. `gg`：将光标直接跳到首行位置
      9. `nG`：表示跳到地 $n$ 行
      10. `0`：表示光标移动到当前行的行首位置
      11. `$`：表示光标移动到当前行的行尾位置
5. 插入模式
   1. 功能：用于文件内容的编辑。
   2. 如何进入插入模式，只能从命令行模式进入插入模式，不能直接从底行模式进入插入模式。
      1. 键盘上特殊的键能从命令行模式进入插入模式 
      2. `INSERT` 键：从光标所在的字符前进入插入模式
      3. `i` 键：从光标所在的字符前进入插入模式   
      4. `I` 键：从光标所在的行行首进入插入模式
      5. `a` 键：从光标所在的字符后面进入插入模式
      6. `A` 键：从光标所在行的行尾进入插入模式
      7. `o` 键：从光标所在行的下一行进入插入模式
      8. `O` 键：从光标所在行的上一行进入插入模式
      9. `s` 键：删除光标所在的字符后，从光标位置进入插入模式
      10. `S` 键：删除光标所在的一行文本后，从当前行进入插入模式
6. 底行模式
   1. 功能：完成对文本内容的保存、退出操作或者替换、查找工作。
   2. 如何进入底行模式：从命令行模式中键入 `shift + 冒号`，就是输入一个冒号。
   3. 主要操作
      1. `w` 保存文本内容
      2. `q` 退出编辑器
      3. `q!` 强制退出
      4. `wq` 保存后退出
      5. `x` 保存退出
      6. `set number` 在编辑器左侧显示行号
      7. `set nonumber` 不显示行号
      8. `/string` 查找字符串 `string`，并将光标定位在包含该字符串的行首
      9. `%s/string1/string2/g` 表示将所有的 `string1` 更换成 `string2`
      10. `m,ns/string1/string2/g` 表示 `[m,n]` 行闭区间内的所有 `string1` 更换成 `string2`
   4. `wqa` 保存所有打开的文件并退出 vim 编辑器 

### 1.1.4 C++ 环境的搭建

略

### 1.1.5 第一个 Linux 下 C++ 程序

分步编译（重要）：`ESc` 命令会分别生成文件名后缀为 `iso` 的文件

1. 预处理 (Pre_processing)
   1. 功能：将源程序头文件展开、删除注释、宏替换
   2. 语法格式：`g++ -E ***.cpp -o ***.i`
2. 编译
   1. 功能：将程序编译生成汇编语言
   2. 语法格式：`g++ -S ***.i -o **.s`
3. 汇编
   1. 功能：将汇编语言编译，生成二进制文件
   2. 语法格式：`g++ -c ***.s -o ***.o`
4. 链接
   1. 功能：链接相关库文件，生成可执行程序
   2. 语法格式：`g++ ***.o -o ***`

运行可执行程序：`./***`

## 1.2 sys 库的使用

### 1.2.1 man 手册的解析以及使用方法

1. 什么是 `man` 手册：Linux 系统提供的有关函数或指令介绍的相关帮助手册，可以在该手册页中查看函数、指令的功能，其实就是相关操作的使用说明书，一共有七章内容，主要使用前三章，第一章是 `shell` 指令相关说明，第二章是系统调用函数相关说明（重点），第三章是库函数（重要）。
2. `man` 手册的使用：输入 `man + 相关函数/指令` 进入手册，输入 `q` 退出手册。

### 1.2.2 常用的内核提供的函数库 sys

1. 文件的操作：`open()`, `read()`, `write()`, `close()`, `lseek()` 等
2. 进程控制函数：`fork()`, `exit()`, `wait()`, `execl()` 等
3. 信号操作：`kill()`, `signal()` 等
4. 网络通信：`socket()`, `bind()`, `listen()` 等

```cpp
#include <iostream>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>
#include <stdio.h>
#include <error.h>
#include <unistd.h>
using namespace std;

int main() {
	//1、定义文件描述符，并打开文件
	int fd = -1;
	if((fd = open("./test.txt", O_WRONLY|O_CREAT|O_TRUNC, 0664)) == -1) {
		perror("open error");
		return -1;
	}

	//2、读写文件
	write(fd, "hello", strlen("hello"));

	//3、关闭文件
	close(fd);
	return 0;
}
```

### 1.2.3 使用 GDB 调试程序

1. 在 linux 系统下的警告和错误，当程序出现 bug 时，linux 终端会给大家两种不同的信息
   - 警告 (warning)：有时的警告是不影响可执行程序的产生。
   - 错误 (error)：错入如果不改正，是不能生成可执行程序的。
   - 总结：相比于 gcc 编译器，g++ 编译器要求更加严格。警告可以被忽略，继续生成可执行程序，但是错误必须更改后才能生成可执行程序。
2. 什么是 GDB
   - [GDB 官网](https://sourceware.org/gdb/)
   - GNU，项目调试器，允许您查看另一个程序在执行时“内部”发生了什么，或者另一个程序崩溃时正在做什么。
3. GDB 是干什么的
   GDB 可以做四种主要的事情（加上其他支持这些事情的事情）来帮助您捕获行为中的 bug：
   
   - 启动程序，指定可能影响其行为的任何内容。
   - 使程序在指定条件下停止。
   - 检查程序停止时发生的情况。
   - 更改程序中的内容，这样您就可以尝试更正一个bug的影响，并继续了解另一个bug。

   这些程序可能与 GDB（本机）在同一台机器上、在另一台机器上（远程）或在模拟器上执行。GDB可以在最流行的 UNIX 和 Microsoft Windows 变体上运行，也可以在 macOS 上运行。

   - gdb 可以调试指定的当前程序，向程序中传递参数
   - gdb 可以调试出错的程序，查看错误原因
   - gdb 还可以调试正在运行的进程
4. 如何使用 GDB
	1. 准备一个 C++ 程序
	```cpp
	#include <iostream>
	using namespace std;

	int main(int argc, const char *argv[]) {
		int arr[5] = { 1, 2, 3, 4, 5};
        for (int i = 0; i < 5; i++) cout << arr[i] << " ";
        cout << endl;
        return 0;
	}
	```
	2. 编译程序，编译选项中需要加上 -g
	```
	g++ -g test.cpp -o test
	```
	3. 启动 gdb 调试
	```
	gdb test
	```
	4. gdb 常用指令
		- `quit (q)`：退出 gdb 调试
		- `run (r)`：表示可执行程序，如果没有设置断点，则将整个程序从头到尾执行一遍
		- `list (l)`：展示可执行程序的相关行的信息，默认展示 10 行
    		- `list l, r`：展示 $l$ 行到 $r$ 行的信息
    		- `list func`：展示 `func` 函数的信息
  		- `break (b)` 表示设置断点，当调试器运行到断点所在位置后，会暂停于此。
    		- `break rowNumber`：在 `rowNumber` 行设置断点
  		- `info`：查看断点相关信息
  		- `next (n)`：执行下一条语句
  		- `continue (c)`：表示从断点处继续向后执行，直到下一个断电在停止
  		- `delete breakpoint num`：删除断点编号为 num 的断点
  		- `step (s)`：能够跳入到指定函数中查看相关函数内部代码
  		- `print (p) 变量名/地址`：表示输出对应的 变量名/地址 对应的信息
  		- `set variable 变量名=值`：表示给某个变量设置相关的值
	5. gdb 使用技巧
		- `shell`：后面可以跟终端指令，表示执行终端相关操作
		- `set logging on`：设置开启日志功能，会在当前目录中生成一个 gdb.txt 文件记录接下来的调试内容
		- `watch point`：观察点，如果设置的观察点的值发生改变，则会将该值的旧值和新值全部展示出来
	6. gdb 调试出错的文件
      	- 当一个可执行程序出现错误时，会产生一个 core 文件，用于查看相关错误信息
      	- linux 系统默认是不产生 core 文件，需要进行相关设置后才能产生
      	- 通过 `ulimit -a` 查看所有 linux 的限制内容
      	- 通过 `ulimit -c unlimited` 来设置 core 文件的个数
      	- 输入 `gdb core文件名` 查看错误原因
      	- `./可执行程序 &`：表示将可执行程序后台运行，不占用当前终端

```
[nuo@01-Desktop]$ g++ -g 05test.cpp // 编译程序
[nuo@01-Desktop]$ ./a.out & // 后台运行程序
[1] 26837 //显示当前后台运行程序的作业号和
进程号
[nuo@01-Desktop]$ gdb -p 26837 //调试指定的运行的进程
GNU gdb (GDB) Red Hat Enterprise Linux 7.6.1-120.el7
Copyright (C) 2013 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law. Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-redhat-linux-gnu".
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Attaching to process 26837
Reading symbols from /home/zpp/day2/a.out...done.
Reading symbols from /lib64/libstdc++.so.6...(no debugging symbols found)...done.
Loaded symbols for /lib64/libstdc++.so.6
Reading symbols from /lib64/libm.so.6...(no debugging symbols found)...done.
Loaded symbols for /lib64/libm.so.6
Reading symbols from /lib64/libgcc_s.so.1...(no debugging symbols found)...done.
Loaded symbols for /lib64/libgcc_s.so.1
Reading symbols from /lib64/libc.so.6...(no debugging symbols found)...done.
Loaded symbols for /lib64/libc.so.6
Reading symbols from /lib64/ld-linux-x86-64.so.2...(no debugging symbols found)...done.
Loaded symbols for /lib64/ld-linux-x86-64.so.2
main (argc=1, argv=0x7ffd31897908) at 05test.cpp:17
17 int main(int argc, const char *argv[]) //进程运行在某处
Missing separate debuginfos, use: debuginfo-install glibc-2.17-326.el7_9.x86_64 libgcc-4.8.5-44.el7.x86_64
libstdc++-4.8.5-44.el7.x86_64
(gdb) n
22		test();
(gdb) n
23		test1();
(gdb) step
test1 () at 05test.cpp:12
12 int num = 520;
(gdb) n
13		num++;
(gdb) c
Continuing.
```

## 1.3 Makefile 文件和 Cmake 文件

## 1.4 vscode 的配置



# 二、标准 IO 和文件 IO

## 2.1 标准 IO 和文件 IO 的区别

## 2.2 标准 IO 相关内容

## 2.3 文件 IO 相关内容



# 三、多进程

## 3.1 多进程实现

## 3.2 多进程实现

## 3.3 进程间通信 IPC



# 四、多线程



# 五、网络编程基础

## 5.3



# 六、制作 HTTP 服务器



# 七、项目篇

## 7.1 基于 UDP 的 TFTP 文件传输

### 7.1.1 TFTP 传输模型的介绍

基于 UDP 通信的 TFTP (Trivial File Transfer Protocol，简单文件传输协议)，是一种用于在网络中进行文件传输的简单协议。TFTP 设计简单，易于实现，通常用于传输小文件或在不支持复杂协议的环境中使用。

#### 7.1.1.1 协议概述

TFTP 使用的是 UDP (User Datagram Protocol，用户数据表协议) 作为传输层协议，默认端口号为 69。

TFTP 设计简单，仅支持五种格式的保温，分别是：

- RRQ (Read Request)：客户端请求读取文件
- WRQ (Write Request)：客户端请求写入文件
- DATA：数据传输报文
- ACK：确认报文
- ERROR：错误报文

#### 7.1.1.2 通信流程

TFTP 的通信流程分为两个阶段：请求阶段和传输阶段

- 请求阶段
  - 客户端向服务器发送 RRQ 或者 WRQ 报文，请求读取或者写入文件，RRQ 报文包含文件名和传输模式（通常是 "netascii" 或者 "octet"），WRQ 报文类似。
  - 服务器响应：服务器受到 RRQ 或者 WRQ 之后，会分配一个新的 UDP 端口用于数据传输，并发送 ACK 进行确认（对于 WRQ）或者直接发送 DATA（对于 RRQ）最为响应。
- 传输阶段
  - 数据传输
    - 在 RRQ 请求中，服务器开始发送 DATA 报文，每个 DATA 报文包含一个块编号和一个数据段，客户端受到 DATA 报文后，发送 ACK 报文确认收到块编号。
    - 在 WRQ 请求中，客户端发送 DATA 报文，服务器发送 ACK 报文确认。
  - 块编号：每个 DATA 报文和 ACK 报文都包含一个 16 位的块编号，从 1 开始递增，块编号用于确保数据的有序传输和确认。
  - 结束传输：当传输的文件大小小于 512 字节时，DATA 报文的数据部分小于 512 字节，表示传输结束。对于 WRQ 请求，客户端发送一个小于 512 字节的 DATA 报文表示传输结束。

#### 7.1.1.2 报文格式

![img1](./img/img1.png#pic_center)

从上往下

- 第一行（读写请求 RRQ 和 WRQ 格式）：操作码 1 代表 RRQ，2 代表 WRQ；文件名数据类型是字符串；第一个 0 作为文件名的结束；模式指的是通信模式（"netascii" 或者 "octet"）。
- 第二行（数据包 DATA 格式）：操作码 3 代表是 DATA 报文。
- 第三行（确认报文 ACK 格式）：操作码 4 代表是 ACK 报文，表示确认收到对应的块编号。
- 第四行（错误信息 ERROR 格式）：操作码 5 代表是 ERROR 报文。

有关错误码对应的信息：

```
0	未定义，差错错误信息
1	File Not Found
2	违反规定
3	磁盘满或者分配超出
4	非法操作
5	未知的传输 ID
6	文件已存在
7	没有该用户
```

#### 7.1.1.3 传输模型

![img2](./img/img2.png#pic_center)

### 7.1.2 项目流程图

## 7.2 基于 TCP 的网络聊天室

## 7.3 基于 TCP 的网络电子词典
