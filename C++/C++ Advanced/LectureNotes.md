<h1 style="text-align:center;">C++ 进阶</h1>

主要内容是 Linux 下的网络编程，操作系统是 Linux 系统 (CentOS)。接下来的内容都是我的个人笔记。

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
2. Xshell 下载地址：https://www.xshell.com/zh/free-for-home-school/ 。
3. 创建一个连接到 Linux 服务器。

### 1.1.3 Vim 使用点回顾

1. Vim 是什么：Vim 是 Linux 系统下的文本编辑器，可以对不同文本进行编辑，功能是对文件内容的编辑、保存、更改等操作。
2. Vim 有三种模式：命令行模式、插入模式、底行模式。
3. 如何打开 Vim 编辑器？
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
      1. `w` 保存文本内容。
      2. `q` 退出编辑器。
      3. `q!` 强制退出。
      4. `wq` 保存后退出。
      5. `x` 保存退出。
      6. `set number` 在编辑器左侧显示行号。
      7. `set nonumber` 不显示行号。
      8. `/string` 查找字符串 `string`，并将光标定位在包含该字符串的行首。
      9. `%s/string1/string2/g` 表示将所有的 `string1` 更换成 `string2`。
      10. `m,ns/string1/string2/g` 表示 `[m,n]` 行闭区间内的所有 `string1` 更换成 `string2`。
   4. `wqa` 保存所有打开的文件并退出 vim 编辑器 。

### 1.1.4 C++ 环境的搭建

略

### 1.1.5 第一个 Linux 下 C++ 程序

分步编译（重要）：`ESc` 命令会分别生成文件名后缀为 `iso` 的文件

1. 预处理 (Pre_processing)
   1. 功能：将源程序头文件展开、删除注释、宏替换。
   2. 语法格式：`g++ -E ***.cpp -o ***.i`
2. 编译
   1. 功能：将程序编译生成汇编语言。
   2. 语法格式：`g++ -S ***.i -o **.s`
3. 汇编
   1. 功能：将汇编语言编译，生成二进制文件。
   2. 语法格式：`g++ -c ***.s -o ***.o`
4. 链接
   1. 功能：链接相关库文件，生成可执行程序。
   2. 语法格式：`g++ ***.o -o ***`

运行可执行程序：`./***`

## 1.2 sys 库的使用

### 1.2.1 man 手册的解析以及使用方法

1. 什么是 `man` 手册：Linux 系统提供的有关函数或指令介绍的相关帮助手册，可以在该手册页中查看函数、指令的功能，其实就是相关操作的使用说明书，一共有七章内容，主要使用前三章，第一章是 `shell` 指令相关说明，第二章是系统调用函数相关说明（重点），第三章是库函数（重要）。
2. `man` 手册的使用：输入 `man + 相关函数/指令` 进入手册，输入 `q` 退出手册。

### 1.2.2 常用的内核提供的函数库 sys

1. 文件的操作：`open()`, `read()`, `write()`, `close()`, `lseek()` 等。
2. 进程控制函数：`fork()`, `exit()`, `wait()`, `execl()` 等。
3. 信号操作：`kill()`, `signal()` 等。
4. 网络通信：`socket()`, `bind()`, `listen()` 等。

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

1. 在 Linux 系统下的警告和错误，当程序出现 bug 时，Linux 终端会给大家两种不同的信息。
   - 警告 (warning)：有时的警告是不影响可执行程序的产生。
   - 错误 (error)：错入如果不改正，是不能生成可执行程序的。
   - 总结：相比于 gcc 编译器，g++ 编译器要求更加严格。警告可以被忽略，继续生成可执行程序，但是错误必须更改后才能生成可执行程序。
2. 什么是 GDB
   - [GDB 官网](https://sourceware.org/gdb/)
   - GNU，项目调试器，允许您查看另一个程序在执行时 “内部” 发生了什么，或者另一个程序崩溃时正在做什么。
3. GDB 是干什么的
   GDB 可以做四种主要的事情（加上其他支持这些事情的事情）来帮助您捕获行为中的 bug：
   
   - 启动程序，指定可能影响其行为的任何内容。
   - 使程序在指定条件下停止。
   - 检查程序停止时发生的情况。
   - 更改程序中的内容，这样您就可以尝试更正一个 bug 的影响，并继续了解另一个 bug。

   这些程序可能与 GDB（本机）在同一台机器上、在另一台机器上（远程）或在模拟器上执行。GDB 可以在最流行的 UNIX 和 Microsoft Windows 变体上运行，也可以在 macOS 上运行。

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
	2. 编译程序，编译选项中需要加上 `-g`
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
  		- `delete breakpoint num`：删除断点编号为 `num` 的断点
  		- `step (s)`：能够跳入到指定函数中查看相关函数内部代码
  		- `print (p) 变量名/地址`：表示输出对应的 变量名/地址 对应的信息
  		- `set variable 变量名=值`：表示给某个变量设置相关的值
	5. gdb 使用技巧
		- `shell`：后面可以跟终端指令，表示执行终端相关操作
		- `set logging on`：设置开启日志功能，会在当前目录中生成一个 `gdb.txt` 文件记录接下来的调试内容
		- `watch point`：观察点，如果设置的观察点的值发生改变，则会将该值的旧值和新值全部展示出来
	6. gdb 调试出错的文件
      	- 当一个可执行程序出现错误时，会产生一个 `core` 文件，用于查看相关错误信息
      	- linux 系统默认是不产生 `core` 文件，需要进行相关设置后才能产生
      	- 通过 `ulimit -a` 查看所有 Linux 的限制内容
      	- 通过 `ulimit -c unlimited` 来设置 `core` 文件的个数
      	- 输入 `gdb core文件名` 查看错误原因
      	- `./可执行程序 &`：表示将可执行程序后台运行，不占用当前终端

```
[nuo@01-Desktop]$ g++ -g 05test.cpp // 编译程序
[nuo@01-Desktop]$ ./a.out & // 后台运行程序
[1] 26837 // 显示当前后台运行程序的作业号和进程号
[nuo@01-Desktop]$ gdb -p 26837 // 调试指定的运行的进程
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
17 int main(int argc, const char *argv[]) // 进程运行在某处
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

### 1.2.4 库的制作（动态库与静态库）

**准备程序**

**add.h**

```cpp
#ifndef _ADD_H // 防止头文件重复包含
#define _ADD_H

int add(int m, int n);

#endif
```

**add.cpp**

```cpp
int add(int m, int n) {
	return m + n;
}
```

**main.cpp**

```cpp
#include <iostream>
#include "add.h"
using namespace std;

int main(int argc, const char *argv[]) {
	cout << add(3, 8) << endl;
	return 0;
}
```

1. 为什么引入库？
   
	在上述案例中，主程序要是有的源程序代码，在 `add.cpp` 中，如果项目结束后，到了交付阶段，由于主程序的生成需要其他程序联合编译，那么就要将源程序打包一起发给老板，这样该程序的开发者自身的价值就不大了，该项目的知识产权就很容易被窃取。为了保护我们的知识产权，我们引入了库的概念。

2. 什么是库？
	库在 linux 中是一个二进制文件，它是由 `.cpp` 文件（不包含 `main` 函数）编译而来，其他程序如果想要使用该源文件中的函数时，只需在编译生成可执行程序时，链接上该源文件生成的库文件即可。库中存储的是二进制文件，不容易被窃取知识产权，做到了保护作用。

库在 Linux 系统中分为两类，分别是 **静态库** 和 **动态库**。

- Windows 系统
  - `***.lib`：静态库
  - `***.dll`：动态库
- Linux 系统
  - `***.a`：静态库
  - `***.so`：动态库

3. 静态库

   	概念：将一个 `***.cpp` 的文件编译生成一个 `lib***.a` 的二进制文件，当你需要使用该源文件中的函数时， 只需要链接该库即可，后期可以直接调用。

  	静态体现在：在使用 `g++` 编译程序时，会将你的文件和库最终生成一个可执行程序（把静态库也放入到可执行程序中），每个可执行程序单独拥有一个静态库，体积较大，但是，执行效率较高。

	**编译生成静态库**

	```
	gcc -c ***.cpp -o ***.o // 只编译不链接生成二进制文件，最后会生成一个 .o 文件
	ar -crs lib***.a ***.o // 编译生成静态库，静态库为 lib***.a

	如果有多个 .o 文件共同编译生成静态库：ar -crs lib***.a ***.o xxx.o ...

	ar：用于生成静态库的指令
	c：(create)，用于创建静态库
	r：(recover)，将文件插入或者替换静态库中同名文件
	s：(reset)，重置金泰克索引
	```

   	**使用静态库**

	```
	gcc main.cpp -o main -L 库的路径 -l库名 -I 头文件的名字
	// 如果在当前路径，则路径输入 .
	// 例如，g++ main.cpp -o main -L . -ladd -I .

	./main
	```

4. 动态库

	概念：将一个 `***.cpp` 的文件编译生成一个 `lib***.so` 的二进制文件，当你需要使用该源文件中的函数时， 只需要链接该库即可，后期可以直接调用。

	动态体现在：在使用 `g++` 编译程序时，会将你的文件和库中的相关函数的索引表一起生成一个可执行程序，每个可执行程序只拥有函数的索引表，当程序执行到对应函数时，会根据索引表，动态寻找相关库所在位置进行调用，体积较小，执行效率较低，但是可以多个程序共享同一个动态库，所以，动态库也叫共享库。

	**编译生成动态库**

	```
	g++ -fPIC -c ***.cpp -o ***.o    // 编译生成二进制文件
	g++ -shared ***.o -o lib***.so   // 依赖于二进制文件生成一个动态库

	// 上述两个指令可以合成一个指令
	g++ -fPIC -shared ***.cpp -o lib***.so
	```

	**使用动态库**

	```
	gcc main.cpp -L 库的路径 -l库名 -I 头文件的名字
	```

	**出现错误**

	```
	./main: error while loading shared libraries: libadd.so: cannot open shared object file: No such file or directory
	// 可执行程序已经生成，运行时报错了
	```

	**解决方法**

	1. 更改路径的宏
	```
	export LD_LIBRARY_PATH=库的路径
	```
	2. 将自己的动态库放入到系统的库函数目录中（`lib64` 或者 `/usr/lib64`）

	**动态库和静态库编译生成的可执行程序大小：动态库 < 静态库**

### 1.2.5 如何使用第三方库

1. `C/C++` 语言默认是支持标准输入输出库的，但是其他的相关函数库需要第三方引入，并且，编译程
序时，需要链接上对应的库。
2. 使用数学库：`#include <math.h>`

	```
	g++ main.cpp -o main -lm -lc
	// -lm: 使用数学库，需要连接上对应的数学库
	// -lc: 链接标准的 c 库
	```
3. 线程支持类库
	如果不能使用线程支持库，通过 `sudo yum install manpages-posix manpages-posix-dev` 指令安装一下即可。

	**test.cpp**

	```cpp
	#include <iostream>
	#include <pthread.h>
	#include <unistd.h>
	using namespace std;

	void *task(void *arg) {
		while (1) {
			cout << "Hello World" << endl;
			sleep(1);
		}
	}

	int main(int agrc, const char *agrv[]) {
		pthread_t tid;

		if (pthread_create(&tid, NULL, task, NULL) != 0) {
			cout << "pthread_create error" << endl;
		}

		while (1);
		return 0;
	}
	```

	对于线程支持库的相关函数，必须链接相关的库：`g++ test.cpp -o test -lpthread`。

## 1.3 Makefile 文件

### 1.3.1 什么是 Makefile

- 用于工程项目管理的一个文本文件，文件名为 `Makefile` 的文本文件。
- `Makefile` 可以大写，也可以小写，一般 `Makefile` 首字母使用大写。
- 如果 `Makefile` 和 `makefile` 两个文件都存在，系统会默认使用小写的。

### 1.3.2 什么是 make

- `make` 是一个执行 `Makefile` 的工具，是一个解释器，用来对 `Makefile` 中的命令进行解析并执行一个 `shell` 指令。
- `make` 这个指令在 `/usr/bin` 中，默认 Linux 系统中都已经安装。
- 如果没有安装 `make`，安装指令如下 `sudo yum install make`。
- 查看是否安装成功：`make --version`。

### 1.3.3 Makefile 的用途

- 描述了整个工程的编译、链接规则。
- 软件项目的自动化编译，相当于给软件编译写一份脚本文件。

### 1.3.4 学习 Makefile 的必要性

- Linux/Unix 环境下开发的必备技能。
- 系统架构师、项目经理的核心技能。
- 研究开源项目、Linux 内核原码的必需品。
- 加深对底层软件构造系统及过程的理解。

### 1.3.5 如何学习 Makefile

1. 理论基础

- 软件的构造过程、程序的编译和链接：预处理 $\to$ 编译 $\to$ 汇编 $\to$ 链接
- 面向依赖的思想：依赖一个 `.cpp` 文件的程序
```
可执行程序 <--依赖于-- .o文件 <--依赖于-- .s文件 <--依赖于-- .i文件 <--依赖于-- .cpp文件
```
- 依赖多个 `.cpp` 文件的程序
```
可执行程序 <--依赖于-- .o文件 <--依赖于-- .s文件 <--依赖于-- .i文件 <--依赖于-- .cpp文件 
	         | -- .o文件 <--依赖于-- .s文件 <--依赖于-- .i文件 <--依赖于-- .cpp文件
	         | -- .o文件 <--依赖于-- .s文件 <--依赖于-- .i文件 <--依赖于-- .cpp文件
	         | -- .o文件 <--依赖于-- .s文件 <--依赖于-- .i文件 <--依赖于-- .cpp文件
```

2. 项目编程基础

- C++ 程序语言基础
- C 语言基础
- 多文件原码管理、头文件包含、函数声明与定义

### 1.3.6 Makefile 的工作过程

- `Makefile` 本身是面向依赖进行编写的。
```
源文件 ---> 编译 ----> 目标文件 ---> 链接 --->可执行文件
hello.cpp ----> hello.o ---> hello
```
- 本质上一步就可以生成可执行程序，但是，由于在生成可执行程序时，可能会有多个文件进行参与，后期其他文件可能要进行更改，更改后，会影响到可执行程序的执行，其他没有更改的文件也要参与编译，浪费时间。

### 1.3.7 第一个 Makefile

1. 编写一个 `Hello.cpp` 文件

```cpp
#include <iostream>
using namespace std;

int main(int argc, const char *argv[]) {
	cout << "Hello World" << endl;
	return 0;
}

```

2. 通过终端编译程序

```
[nuo@localhost 02Make]$ g++ -E Hello.cpp -o Hello.i // 预处理
[nuo@localhost 02Make]$ g++ -S Hello.i -o Hello.s // 编译
[nuo@localhost 02Make]$ g++ -c Hello.s -o Hello.o // 汇编
[nuo@localhost 02Make]$ g++ Hello.o -o Hello // 链接
[nuo@localhost 02Make]$ ls
Hello  Hello.cpp  Hello.i  Hello.o  Hello.s
[nuo@localhost 02Make]$ ./Hello
```

3. 使用 Makefile 管理工程文件

编写 Makefile 文件

```Makefile
# Makefile 中的注释是以 # 开头
# 语法格式：
# 目标：依赖
# 	通过依赖生成目标的指令
# 注意：指令前面必须使用同一个 TAB 键隔开，不能使用多个空格顶过来

Hello: Hello.o
        g++ Hello.o -o Hello

Hello.o: Hello.s
        g++ -c Hello.s -o Hello.o

Hello.s: Hello.i
        g++ -S Hello.i -o Hello.s

Hello.i: Hello.cpp
        g++ -E Hello.cpp -o Hello.i
```

简化的 Makefile 文件

```Makefile
Hello: Hello.o
	g++ Hello.o -o Hello

Hello.o: Hello.cpp
	g++ -c Hello.cpp -o Hello.o
```

4. 执行 Makefile 文件

```
make ---> 默认找到 Makefile 中的第一个目标开始进行执行
make 目标 ---> 找到对应的目标进行执行
```

```Makefile
Hello: Hello.o
	g++ Hello.o -o Hello

Hello.o: Hello.cpp
	g++ -c Hello.cpp -o Hello.o

# 此处一个规则是没有以依赖的，只有目标和指令
clean:
	rm Hello.[^cpp] Hello
```

```
[nuo@localhost 02Make]$ make // 默认找第一个目标开始执行
g++ -c Hello.cpp -o Hello.o
g++ Hello.o -o Hello
[nuo@localhost 02Make]$ make clean // 直接找 clean 目标开始执行
rm Hello.[^cpp] Hello
```

### 1.3.8 Makefile 的语法规则

1. 规则
   - 构成 Makefile 的基本单元，构成依赖关系的核心部件。
   - 其他内容可以看做为规则的服务。
2. 变量
   - 类似于 C++ 中的宏， 使用变量：`$(变量名)` 或者 `${变量名}`。
   - 作用：使得 Makefile 更加灵活。
3. 条件执行
   - 根据某一变量的值来控制 make 执行或者忽略 Makefile 的某一部分。
4. 函数
   - 文本处理函数：字符串的替换、查找、过滤等等。
   - 文件名的处理：读取文件/目录名、前后缀等等。
5. 注释
   - Makefile 中的注释，是以 `#` 号开头。

### 1.3.9 规则

1. 规则的构成：目标、目标依赖、命令
2. 语法格式

```
目标：目标依赖
	命令
```

注意事项：命令必须使用 TAB 键开头，一般是 shell 指令。
- 一个规则中，可以无目标依赖，仅仅是实现某种操作。
- 一个规则中可以没有命令，仅仅描述依赖关系。
- 一个规则中，必须要有一个目标。

```Makefile
# 只有目标和目标依赖，没有命令
all: Hello

# 三者都有
Hello: Hello.o
	g++ Hello.o -o Hello

# 三者都有
Hello.o: Hello.cpp
	g++ -c Hello.cpp -o Hello.o

# 只有目标和命令，没有目标依赖
clean:
	rm Hello.[^cpp] Hello
```

3. 目标详解

(1) 默认目标
  - 一个 Makefile 里面可以有多个目标，一般会选择第一个当做默认目标，也就是make默认执行的目标。

(2) 多目标

- 一个规则中可以有多个目标，多个目标拒用相同的生成命令和依赖文件。

```Makefile
# 多目标规则
clean distclean:
	rm hello.[^cpp] hello
```

(3) 多规则目标

- 多个规则可以是同一个目标。

```Makefile
all: test1
all: test2
test1:
	@echo "Hello"
test2:
	@echo "World"
```
(4) 伪目标

- 并不是一个真正的文件名，可以看做是一个标签。
- 无依赖，相比一般文件，不会重新生成、执行。
- 伪目标：可以无条件执行，相当于对应的指令。

eg:

```Makefile
.PHONY: clean	#设置伪目标
clean:
	rm hello.[^cpp] hello
```

4. 目标依赖

(1) 文件时间戳

- 根据时间戳来判断目标依赖是否要进行更新（目标依赖是否被更新掉，取决于依赖文件是否已经被更改。如果目标依赖比依赖文件时间更早，需要重新编译；如果目标文件是最新的，不会再执行相关的编译过程了）。
- 所有文件都更改过，则对所有文件进行编译，生成可执行程序。
- 在上次 make 之后修改过的 `cpp` 文件，会被重新编译。
- 在上次 make 只写修改过的头文件，依赖该头文件的目标依赖也会重新编译。

(2) 模式匹配

```
% ----> 通配符匹配
$@ ----> 目标
$^ ----> 依赖
$< ----> 第一个依赖
* ----> 普通通配符
```

注意：`%` 是 Makefile 中的规则通配符，`*` 是普通通配符

```cpp
// 头文件 operator.h
#ifndef _OPERATOR_H
#define _OPERATOR_H

int add(int m, int n);

#endif
```

```cpp
// 源文件

// 主程序
#include <iostream>
#include "operator.h"
using namespace std;

int add(int m, int n) {
	return m + n;
}

int main(int argc, const char *argv[]) {
	cout << add(3,5) << endl;
	return 0;
}
```

```Makefile
# 第一个依赖关系
all: operator

operator: main.o operator.o
	# g++ main.o operator.o -o operator
	g++ $^ -o $@

# main.o: main.cpp
#	g++ -c $< -o $@

# operator.o: operator.cpp
#	g++ -c $^ -o $@
# 将上述两个规则统一成一个规则

# 此时的 % 表示所有的 Makefile 内容，不能换成 *
%.o: %.cpp
	g++ -c $< -o $@
	
clean:
	rm -f *.o operator	# 此处的 * 是普通通配符
```

5. 命令

- 命令的组成
  - 由 shell 命令组成，以 TAB 键开头。
- 命令的执行
  - 每条命令，make 会开一个进程。
  - 每条命令执行完，make 会检测这个命令的返回码。
  - 如果返回成功，则继续执行后面的命令。
  - 如果返回失败，则 make 会终止当前执行的规则并退出。
- 并发执行命令
  - `make -j4`：表示开辟 4 个线程执行。
  - `time make`：执行 make 时，显示当前时间。

### 1.3.10 变量

(1) 变量基础

  - 变量定义：`变量名 = 变量值`，不需要给定数据类型。
  - 变量的赋值
    - 追加赋值：`+=` $\to$ 在原有的基础上追加相关内容。
    - 条件赋值：`?=` $\to$ 如果之前没有值，则为变量赋值，如果之前有值，则不进行赋值。
  - 变量的使用：`$(变量名)` 或者 `${变量名}`，一般使用第一种。

(2) 变量的分类

  - 立即展开变量
    - 使用 `:=` 操作符进行赋值。
    - 作用：在解析阶段直接赋值常量字符串。
  - 延迟展开变量
    - 使用 `=` 操作符进行赋值。
    - 作用：将最后一次赋值的结果给变量名使用。
  - 注意事项
    - 一般在目标、目标依赖中使用立即展开赋值。
    - 在命令中一般使用延迟展开赋值变量。

(3) 变量的外部传递

  - 可以通过命令行给变量进行赋值操作：`make ARCH=g++`。

```Makefile
var1 = main.o	#定义一个变量并赋值
var1 += operator.o	#给变量追加值

CC:=g++

#第一个依赖关系
all: operator

operator: $(var1)
#	g++ main.o operator.o -o operator
	$(CC) $^ -o $@

# main.o: main.cpp
#	g++ -c $< -o $@
# operator.o: operator.cpp
#	g++ -c $^ -o $@

# 将上述两个规则统一成一个规则
# 此时的 % 表示所有的 Makefile 内容，不能换成 *
%.o: %.cpp
	$(CC) -c $< -o $@

clean:
	rm -f *.o operator	# 此处的 * 是普通通配符
```

### 1.3.11 条件执行

(1) 关键字

```
ifeq else endif		# 判断相等
ifneq else endif	# 判断不相等
```

(2) 使用

```
ifeq (要判断的变量, 要判断的值)
	Makefile 语句
else
	Makefile 语句
endif
```

**注意**：条件语句从 `ifeq` 开始执行，括号与关键字自减使用空格隔开。括号里面爱着括号处，不允许加空格

```Makefile
# 条件选择
# 如果变量 COMPILE 的值是 g++，那么 CC 的值就是 g++，否则是 gcc
ifeq ($(COMPILE), g++)
	CC:=g++
else
	CC:=gcc
endif
```

### 1.3.12 总结

- 执行过程
  - 进入编译目录、执行 make 命令。
  - 依赖关系解析阶段
  - 命令执行阶段
- 依赖关系解析阶段
  - 解析 Makefile，建立依赖关系树
  - 控制解析过程，引入 Makefile，变量展开、条件执行
  - 生成依赖关系书
- 命令执行阶段
  - 把解析生成的依赖关系树加载到内存
  - 按依赖关系，按顺序生成这些文件
  - 再次编译 make 会检测文件的时间戳，判断是否过期
    - 如果无过期，则不再编译
    - 如果文件有更新，则依赖该文件的所有依赖关系上的目标都会重新编译
- make 执行结果
  - make 的退出码
    - $0$ 表示成功执行
    - $1$ 表示允许错误、退出

### 1.3.13 参考源码

(1) 头文件

```c
#ifndef _OPERATOR_H
#define _OPERATOE_H

int add(int m, int n);

#endif
```

(2) 源文件

```c
int add(int m, int n) {
	return m + n;
}
```

(3) 主程序

```cpp
#include <iostream>
#include "operator.h"
using namespace std;

int main(int argc, const char *argv[]) {
	cout << add(3,5) << endl;
	return 0;
}
```

(4) Makefile

```Makefile
var1 = main.o	#定义一个变量并赋值
var1 += operator.o	#给变量追加值

# 条件选择
# 如果变量 COMPILE 的值是 g++，那么 CC 的值就是 g++，否则是 gcc
ifeq ($(COMPILE), g++)
	CC:=g++
else
	CC:=gcc
endif

#第一个依赖关系
all: operator

operator: $(var1)
#	g++ main.o operator.o -o operator
	$(CC) $^ -o $@

# main.o: main.cpp
#	g++ -c $< -o $@
# operator.o: operator.cpp
#	g++ -c $^ -o $@

# 将上述两个规则统一成一个规则
# 此时的 % 表示所有的 Makefile 内容，不能换成 *
%.o: %.cpp
	$(CC) -c $< -o $@

clean:
	rm -f *.o operator	# 此处的 * 是普通通配符
```

## 1.4 VSCode 的配置

略

## 1.5 VSCode 中使用 CMake

### 1.5.1 前言

1. CMake 是一个跨平台的安装编译工具，可以使用简单的语句来描述所有平台的安装（编译过程）。
2. CMake 可以说是已经成为大部分的 C++ 开发项目的标配。
3. 可以使用几行或者几十行的代码来完成非常冗长的 Makefile 代码。

### 1.5.2 为什么要使用 CMake

1. 在不使用 CMake 时，编译工程如下，不同平台的工具使用了不同的编译器，操作比较复杂。

![img1](./img/img1.png#pic_center)

2. 在上面的机制中，如果要向工程文件中添加一个源文件 `bar.cpp` 需要手动的向每一个工具中添加，操作非常麻烦。

![img2](./img/img2.png#pic_center)

3. 使用 CMake 来管理工程的状态。

![img3](./img/img3.png#pic_center)

4. 使用 CMkake 管理工程，向工程中添加一个新文件 `bar.cpp`，CMake 指令会根据平台将文件加到对应的工具中。

![img4](./img/img4.png#pic_center)

### 1.5.3 语法特性介绍

1. 基本语法：`指令 (参数1 参数2 ...)`
   - 参数使用括号括起来。
   - 参数之间使用空格或分号隔开。
2.  注意：**指令** 是大小写 **无关** 的，但是 **参数** 和 **变量** 是大小写 **相关** 的。
```cmake
set(HELLO hello.cpp)	# 定义一个变量名叫 HELLO 变量的值为 hello.cpp
add_executable(hello main.cpp hello.cpp)  # 通过main.cpp 和 hello.cpp 编译生成 hello 可执行程序
ADD_EXECUTABLE(hello main.cpp ${HELLO})   # 作用同上
```
1. 变量使用 `${}` 进行取值，但是在 `if` 控制语句中，是直接使用变量名的。
   - `if(HELLO)` 是正确的
   - `if(${HELLO})` 是不正确的
2. 语句不以分号结束。

### 1.5.4 重要的指令

1. `cmake_minimum_required`：指定 CMake 的最小版本支持，一般作为第一条 CMake 指令。
```cmake
# CMake设置最小支持版本为 2.8
cmake_minimum_required(VERSION 2.8)
```
2. `project`：定义工程的名称，并可以指定工程支持的语言。
```cmake
# 指定工程的名称为 HELLOWORLD
project(HELLOWORLD CXX)	# 表示工程名为 HELLOWORLD， 使用的语言为 C++
```
3. `set`：显式定义变量。
```cmake
# 定义变量 SRC 其值为 sayhello.cpp hello.cpp
set(SRC sayhello.cpp hello.cpp)
```
4. `add_executable`：通过依赖生成可执行程序。
```cmake
# 编译 main.cpp 生成 main 的可执行程序
add_executable(main main.cpp)
```
5. `include_directories`：向工程添加多个特定的头文件搜索路径，类似于 `g++` 编译指令中的 `-I`。
```cmake
# 将 /usr/lib/mylibfolder 和 ./lib 添加到工程路径中
include_directories(/usr/lib/mylibfolder ./lib)
```
6. `link_directories`：向工程中添加多个特定的库文件搜索路径，类似于 `g++` 编译指令的 `-L` 选项。
```cmake
# 将 /usr/lib/mylibfolder 和 ./lib 添加到库文件搜索路径中
link_directories(/usr/lib/mylibfolder ./lib)
```
7. `add_library`：生成库文件（包括动态库和静态库）。
```cmake
# 通过 SRC 变量中的文件，生成库
add_library(hello SHARED ${SRC})	# 该语句生成的是动态库
add_library(hello STATIC ${SRC})	# 该语句生成的是静态库
```
8. `add_compile_options`：添加编译参数。
```cmake
# 添加编译参数： -Wall -std=c++11
add_compile_options(-Wall -std=c++11)
```
9. `target_link_libraries`：为 `target` 添加需要链接的共享库,类似于 `g++` 编译中的 `-l` 指令。
```cmake
# 将 hello 动态库文件链接到可执行程序main 中
target_link_libraries(main hello)。
```

### 1.5.5 CMake 常用变量

1. `CMAKE_C_FLAGS`：gcc 编译选项的值。
2. `CMAKE_CXX_FLAGS`：g++ 编译选项的值。
```cmake
# 在 CMAKE_CXX_FLAGS 编译选项后追加 -std=c++11
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
```
3. `CMAKE_BUILD_TYPE`：编译类型（Debug 、Release）
```cmake
# 设定编译类型为 Debug，调试时需要选择该模式
set(CMAKE_BUILD_TYPE Debug)

# 设定编译类型为 Release，发布需要选择该模式
set(CMAKE_BUILD_TYPE Release)
```

### 1.5.6 CMake 编译工程

CMake 目录结构：项目主目录中会放一个 `CMakeLists.txt` 的文本文档，后期使用 cmake 指令时，依赖的就是该文档。
1. 包含源文件的子文件夹中包含 `CMakeLists.txt` 文件时，主目录的 `CMakeLists.txt` 要通过 `add_subdirector` 添加子目录。
2. 包含源文件的子文件夹中不包含 `CMakeLists.txt` 文件时，子目录编译规则，体现在主目录中的 `CMakeLists.txt`。

**两种构建方式**

(1) 内部构建：不推荐使用

内部构建会在主目录下，产生一大堆中间文件，这些中间文件并不是我们最终所需要的，和工程源文件放在一起时，会显得比较杂乱无章。

```cmake
## 内部构建

# 在当前目录下，编译主目录中的 CMakeLists.txt 文件生成 Makefile 文件
cmake .		# . 表示当前路径
# 执行make命令，生成目标文件
make
```

(2) 外部构建：推荐使用

将编译输出的文件与源文件放到不同的目录下，进行编译，此时，编译生成的中间文件，不会跟工程源文件进行混淆。

```cmake
## 外部构建步骤

# 1、在当前目录下，创建一个 build 文件，用于存储生成中间文件
mkdir build

# 2、进入build文件夹内
cd build

# 3、编译上一级目录中的 CMakeLists.txt，生成 Makefile 文件以及其他文件
cmake ..	# .. 表示上一级目录

# 4、执行 make 命令，生成可执行程序
make
```

### 1.7.7 CMake 代码实战

**1. 同一目录下的文件进行编译**

(1) 源文件

`Hello.cpp`

```cpp
#include <iostream>

int main(int argc, const char * argv[]) {
	std::cout << "Hello World!" << std::endl;
	return 0;
}
```

编译命令：

```
g++ Hello.cpp -o Hello
./Hello
```

(2) `CmakeLists.txt` 文件

```cmake
# 设置最小版本中支持
cmake_minimum_required(VERSION 2.8)

# 项目名称
project(HELLO)

# 生成可执行程序，依赖于 hello.cpp 生成 hello 可执行程序
add_executable(hello hello.cpp)
```

(3) 内部构建

```
cd CMake/test1
cmake .
make
./hello
```

会额外生成 `cmake_install.cmake`，`CMakeCache.txt`，`Makefile	` 这两个中间文件。

(4) 外部编译

```
mkdir build
cd build
cmake ..
make
./hello
```

所有生成的中间文件都会储存在 build 文件夹下。

**2. 分文件编译**

(1) 头文件 `swap.h`

```cpp
#ifndef SWAP_H
#define SWAP_H

// 声明一个自定义交换类
class MySwap {
public:
    int a, b; // 两个成员变量

    // 定义构造函数
    MySwap(int a, int b) {
        this->a = a;
        this->b = b;
    }

    // 声明交换函数 
    void run();

    // 声明打印函数
    void printInfo();
};

#endif
```

(2) 源文件 `swap.cpp`

```cpp
#include <iostream>
#include "swap.h"

// 对交换函数的定义
void MySwap::run() {
    a = a ^ b;
    b = a ^ b;
    a = a ^ b;
}

void MySwap::printInfo() {
    std::cout << a << " " << b << std::endl;
}
```

(3) 测试文件 `main.cpp`

```cpp
#include "swap.h"

int main(int argc, const char * argv[]) {
    MySwap mySwap(10, 20);
    mySwap.printInfo();
    mySwap.run();
    mySwap.printInfo();
    return 0;
}
```

(4) 分文件编译使用 `g++` 编译器生成可执行程序

```
// 联合编译，-I 是指定头文件路径
g++ main.cpp src/swap.cpp -I include -o main
./main
```

(5) 创建工程管理文件 `CMakelist.txt`

```cmake
# 设置最小版本支持
cmake_minimum_required(VERSION 3.5)

# 指定项目名称
project(SWAP)

# 指定头文件路径，相当于 g++ 编译命令中的 -I 指令
include_directories(include)

# 生成可执行文件
add_executable(main main.cpp src/swap.cpp)
```  

# 二、IO 操作（外部文件操作）

## 2.1 标准 IO 和文件 IO 的区别

### 2.1.1 IO 的概念

1. IO：顾名思义就是输入输出，程序与外部设备进行信息交换的过程称为 IO 操作。
2. 最先接触的IO：`#include <stdio.h>` 标准的缓冲输入输出头文件。

### 2.1.2 IO 的分类

1. 标准 IO：使用系统提供的库函数实现。
2. 文件 IO：基于系统调用完成，每进行一次系统调用，进程会从用户空间向内核空间进行一次切换，当用户空间与内核空间进行切换时，进程就会进入一次挂起状态，从而导致进程执行效率低。
3. 文件 IO 与标准 IO 的区别：标准 IO 相比于文件 IO 而言，提供了缓冲区，用户可以将数据先放入缓冲区中，等到缓冲区时机到了后，统一进行一次系统调用，将数据刷入内核空间。

![img5](./img/img5.png#pic_center)

左边的图是标准 IO 的执行过程，右边的图左边分支是文件 IO，右边分支是标准 IO。

1. 标准 IO 接口：`printf`/`scanf`、`fopen`/`fclose`、`fgetc`/`fputc`、`fgets`/`futs`、`fprintf`/`fscaf`、`fread`/`fwrite`、`fseek`/`ftell`/`rewind`。
2. 文件 IO 接口：`open`/`close`、`read`/`write`、`lseek`。

## 2.2 标准 IO 相关内容

### 2.2.1 标准 IO 的实现原理

![img6](./img/img6.png#pic_center)

### 2.2.2 FILE 结构体的介绍

1. `FILE` 结构体是系统提供的用于描述一个文件的全部信息的结构体
2. `FILE` 结构体的原型
```cpp
struct FILE {
	char * _IO_buf_base;	// 缓冲区的起始地址
	char * _IO_buf_end; 	// 缓冲区的终止地址

	int _fileno;		// 文件描述符，用于系统调用
};
```
3. 特殊的 `FILE` 指针，这三个指针，全部都是针对于终端文件而言的，当程序启动后，系统默认打开的三个特殊文件指针。
   - `stderr`：标准出错指针
   - `stdin`：标准输入指针
   - `stdout`：标准输出指针

### 2.2.3 打开文件：fopen

1. 函数所在头文件：`<stdio.h>`
2. `FILE *fopen(const char *path, const char *mode);`
3. 功能：打开一个文件，并返回当前文件的文件指针。
4. 参数 1：表示文件路径，是一个字符串
5. 参数 2：打开文件的方式，也是一个字符串，字符串必须以以下的字符开头。
   - `r`：以只读的形式打开文件，文件光标定位在开头部分。
   - `r+`：以读写的形式打开文件，文件光标定位在开头部分。
   - `w`：以只写的形式打开文件，如果文件不存在，就创建文件，如果文件存在，就清空，光标定位在开头。
   - `w+`：以读写的形式打开文件，如果文件不存在，就创建文件，如果文件存在，就清空，光标定位在开头。
   - `a`：以追加（结尾写）的形式打开文件，如果文件不存在就创建文件，光标定位在文件末尾。
   - `a+`：以读写的形式打开文件，如果文件不存在就创建文件，如果文件存在，读取操作光标定位在开头，写操作光标定位在结尾。
6. 返回值：成功返回打开的文件指针，失败返回NULL并置位错误码

```cpp
#include "stdio.h" //标准的输入输出头文件

int main(int argc, const char *argv[]) {
	//1、定义一个文件指针
	FILE *fp = NULL;
	
	// 以只读的形式打开一个不存在的文件，并将返回结果存入到 fp 指针中
	// fp = fopen("./file.txt", "r");
	// 此时会报错，原因是以只读的形式打开一个不存在的文件，是不允许的
	
	// 以只写的形式打开一个不存在的文件，如果文件不存在就创建一个空的文件，如果文件存在就清空
	fp = fopen("./file.txt", "w");
	
	if(fp == NULL) {
		printf("fopen error\n");
		return -1;
	}
	printf("fopen success\n");
	return 0;
}
```

### 2.2.4 关闭文件：fclose

1. 函数所在头文件：`<stdio.h>`
2. `FILE *fopen(const char *path, const char *mode);`
3. 功能：关闭指定的文件。
4. 参数：要关闭的文件指针（由 `fopen` 返回的结果）。
5. 返回值：成功执行返回 `0`，失败返回 `EOF` 并置位错误码。

```cpp
#include "stdio.h" // 标准的输入输出头文件

int main(int argc, const char *argv[]) {
	// 1、定义一个文件指针
	FILE *fp = NULL;
	
	// 以只读的形式打开一个不存在的文件，并将返回结果存入到fp指针中
	// fp = fopen("./file.txt", "r");
	// 此时会报错，原因是以只读的形式打开一个不存在的文件，是不允许的
	
	// 以只写的形式打开一个不存在的文件，如果文件不存在就创建一个空的文件，如果文件存在就清空
	fp = fopen("./file.txt", "w");
	
	if(fp == NULL) {
		printf("fopen error\n");
		return -1;
	}
	printf("fopen success\n");
	
	// 2、关闭文件
	fclose(fp);
	return 0;
}
```

### 2.2.5 关于错误码的问题

1. 概念：当内核提供的函数出错后，内核空间会向用户空间反馈一个错误信息，由于错误信息比较多
也比较复杂，系统就给每种不同的错误信息起了一个编号，用一个整数表示，这个整数就是错误码。
2. 错误码都是大于或等于 $0$ 的数字：

```
#define EPERM		1	/* Operation not permitted */	操作受限
#define ENOENT		2	/* No such file or directory */	没有当前文件或目录
#define ESRCH		3	/* No such process */		没有进程
#define EINTR		4	/* Interrupted system call */	中断系统调用
#define EIO 		5	/* I/O error */
#define ENXIO		6	/* No such device or address */
#define E2BIG		7	/* Argument list too long */
#define ENOEXEC 	8	/* Exec format error */
#define EBADF		9	/* Bad file number */
#define ECHILD		10	/* No child processes */
#define EAGAIN		11	/* Try again */
#define ENOMEM		12	/* Out of memory */
#define EACCES		13	/* Permission denied */
#define EFAULT		14	/* Bad address */
#define ENOTBLK		15	/* Block device required */
#define EBUSY		16	/* Device or resource busy */
#define EEXIST		17	/* File exists */
#define EXDEV		18	/* Cross-device link */
#define ENODEV		19	/* No such device */
#define ENOTDIR		20	/* Not a directory */
#define EISDIR		21	/* Is a directory */
#define EINVAL		22	/* Invalid argument */
#define ENFILE		23	/* File table overflow */
#define EMFILE		24	/* Too many open files */
#define ENOTTY		25	/* Not a typewriter */
#define ETXTBSY		26	/* Text file busy */
#define EFBIG		27	/* File too large */
#define ENOSPC		28	/* No space left on device */
#define ESPIPE		29	/* Illegal seek */
```

3. 关于错误码的处理函数 (`strerror`、`perror`)

```c
// 错误码所在的头文件，里面定义了一个全局变量 errno 储存错误码
#include <errno.h>
int errno;

// 将错误码对应的错误信息转换处理
#include <string.h>
char *strerror(int errnum);
/*
	功能：将错误码转换为错误信息描述
	参数：错误码
	返回值：错误信息字符串描述
*/

// 只打印错误码对应的错误信息的函数
#include <stdio.h>
void perror(const char *s);
/*
	功能：输出当前错误码对应的错误信息
	参数：提示符号，会原样打印出来，并且会在提示数据后面加上冒号，并输出完后，自动换行
	返回值：无
*/
```

```c
#include "stdio.h"	// 标准的输入输出头文件
#include <errno.h>	// 错误码所在的头文件
#include <string.h>	// 字符串处理的头文件

int main(int argc, const char *argv[]) {

	// 1、定义一个文件指针
	FILE *fp = NULL;

	fp = fopen("./file.txt", "r");
	if(fp == NULL) {
		// printf("fopen error: %d, errmsg:%s\n", errno, strerror(errno));	// 打印错误信息，并输出错误码 2
		perror("fopen error");	// 打印当前错误码对应的错误信息
		// output：fopen error: ...
		return -1;
	}
	printf("fopen success\n");

	// 2、关闭文件
	fclose(fp);

	return 0;
}
```

### 2.2.6 单字符读写：fputc/fgetc

```c
// 函数所在头文件
#include <stdio.h>

fgetc(FILE *stream);
/*
	功能：从指定文件中读取一个字符数据，并以无符号整数的形式返回
	参数：文件指针
	返回值：成功返回读取的字符对应的无符号整数，失败返回 EOF 并置位错误码
*/

int fputc(int c, FILE *stream);
/*
	功能：将指定的字符 c 写入到 stream 指向的文件中
	参数 1：要写入的字符对应的无符号整数
	参数 2：文件指针
	返回值：成功返回写入字符对的无符号整数，失败返回 EOF 并置位错误码
*/
```

**EOF 其实就是 -1**

```c
#include <stdio.h>

int main(int argc, const char *argv[]) {
	// 打开文件
	FILE *fp = NULL;
	// 以只写的方式打开文件
	if ((fp = fopen("./file.txt", "w")) == NULL) {
		perror("fopen error");
		return -1;
	}

	// 使用程序对文件进行读写操作（单个字符进行操作）
	// 本次操作的结果是在外部文件中显示 Hello，说明每写入一次，文件光标都会向后偏移
	fputc('H', fp);
	fputc('e', fp);
	fputc('l', fp);
	fputc('l', fp);
	fputc('o', fp);

	// 能否从该处读取数据？为什么？
	// 不可以，文件是只写的且光标此时在文件结尾处，没有数据可以被读取

	// 关闭文件
	fclose(fp);
	fp = NULL;

	// 以只读的方式打开文件
	if ((fp = fopen("./file.txt", "r")) == NULL) {
		perror("open error");
		return -1;
	}

	char ch = 0; // 字符的搬运工，将文件的字符搬运到终端
	while ((ch = fgetc(fp)) != EOF) {
		putchar(ch);
	}
	puts("");

	// 关闭文件
	fclose(fp);
	fp = NULL;
	return 0;
}
```

练习：使用 `fgetc` 和 `fputc` 完成两个文件的复制工作，实现指令 `cp` 的功能：`cp srcfile destfile`

```c
/*
    目的：利用 fgetc 跟 fputc 实现文件的复制粘贴
*/
#include <stdio.h>

int main(int argc, const char * argv[]) {
	// 声明文件指针
	FILE *src = NULL, *dst = NULL;

	// 以只读方式打开资源文件
	if ((src = fopen("./src.txt", "r")) == NULL) {
		perror("Failed to open source file");
		return -1;
	}

	// 以只写方式打开目标文件
	if ((dst = fopen("./dst.txt", "w")) == NULL) {
		perror("Failed to open destination file");
		return -1;
	}

	// 用于接收资源文件字符的变量
	char ch = -1;

	// 将资源文件中所有字符依次输出到目标文件中
	while ((ch = fgetc(src)) != EOF) fputc(ch, dst);

	// 关闭资源文件
	fclose(src);
	src = NULL;

	// 关闭目标文件
	fclose(dst);
	dst = NULL;
	return 0;
}
```

### 2.2.7 字符串读写：fgets/fputs

```c
// 函数所在头文件
#include <stdio.h>

int fputs(const char *s, FILE *stream);
/*
	功能：将指定的字符串，写入到指定的文件中
	参数 1：要被写入的字符串
	参数 2：文件指针
	返回值：成功返回本次写入字符的个数，失败返回 EOF
*/

char *fgets(char *s, int size, FILE *stream);
/*
	功能：从 stream 指向的文件中最多读取 size - 1 个字符到 s 容器中，遇到回车或文件结束，会结束一次读取，并且会将回车放入容器，最后自动加上一个字符串结束标识 '\0'
	参数 1：字符数组容器起始地址
	参数 2：要读取的字符个数，最多读取 size - 1 个字符
	参数 3：文件指针
	返回值：成功返回容器 s 的起始地址，失败返回 NULL
*/
```

```c
#include <stdio.h>
#include <string.h>

int main(int argc, const char * argv[]) {
	// 打开文件
	FILE *fp = NULL;
	if ((fp = fopen("./file.txt", "w")) == NULL) {
		perror("fopen error");
		return -1;
	}

	// 向文件中写入多个字符串
	char wbuf[128];         // 定义一个字符数组
	while (1) {
		fgets(wbuf, sizeof(wbuf), stdin);   // 从终端输入一个字符串
		// 终端输入的时候会把 quit 后面的换行一起读到字符数组 wbuf 里面
		if (!strcmp(wbuf, "quit\n")) break;
		fputs(wbuf, fp);    // 将上述字符串写入文件中
	}

	// 关闭文件
	fclose(fp);
	fp = NULL;
	return 0;
}
```

练习：使用 `fgets` 和 `fputs` 完成两个文件的复制。

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
	// 声明文件指针
	FILE *src = NULL, *dst = NULL;

	// 以只写的方式打开资源文件
	if ((src = fopen("./src.txt", "r")) == NULL) {
		perror("Failed to open source file");
		return -1;
	}

	// 以只读的方式打开目标文件
	if ((dst = fopen("./dst.txt", "w")) == NULL) {
		perror("Failed to open destination file");
		return -1;
	}

	// 将资源文件中的字符串依次读入到目标文件中
	char buf[1024];
	while (fgets(buf, sizeof(buf), src) != NULL)
		fputs(buf, dst);

	// 关闭资源文件
	fclose(src);
	src = NULL;

	// 关闭目标文件
	fclose(dst);
	dst = NULL;
	return 0;
}
```

### 2.2.8 关于标准 IO 的缓冲区问题

1. 缓冲区分为三种

   - 行缓存：和终端文件相关的缓冲区叫做行缓存，行缓冲区的大小为 $1024$ 字节，对应的文件指针：`stdin`、`stdout`。
   - 全缓存：和外界文件相关的缓冲区叫做全缓存，全缓冲区的大小为 $4096$ 字节，对应的文件指针：`fp`。
   - 不缓存：和标准出错相关的缓冲区叫不缓存，不缓存的大小为 $0$ 字节，对应的文件指针：`stderr`。

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
	// 验证行缓存大小为 1024
	// 输出行缓存大小
	printf("%d\n", stdout->_IO_buf_end - stdout->_IO_buf_base); // 0
	printf("%d\n", stdout->_IO_buf_end - stdout->_IO_buf_base); // 1024
	// 原因：如果缓冲区没有被使用时，求出大小为 0，只有至少被使用了一次后，缓冲区的大小才会被分配

	int num = 0;
	scanf("%d", &num); // 输入一个整数到 num 变量中
	// 输出行缓存大小
	printf("%d\n", stdin->_IO_buf_end - stdin->_IO_buf_base);	// 1024


	// 验证全缓存大小为 4096
	FILE *fp = NULL;
	if ((fp = fopen("./file.txt", "r")) == NULL) {
		perror("fopen error");
		return -1;
	}

	printf("%d\n", fp->_IO_buf_end - fp->_IO_buf_base);	// 0
	// 原因：未使用的全缓存大小为 0

	fgetc(fp);	// 从文件中读取一个字符
	printf("%d\n", fp->_IO_buf_end - fp->_IO_buf_base);	// 4096

	// 关闭文件
	fclose(fp);
	fp = NULL;


	// 验证不缓存的大小为 0
	printf("%d\n", stderr->_IO_buf_end - stderr->_IO_buf_base);	// 0
	// 未使用的不缓存大小为 0
	perror("error"); // 使用一次不缓存
	printf("%d\n", stderr->_IO_buf_end - stderr->_IO_buf_base);	// 0
	// 不缓存大小为 0

	return 0;
}
```

2 . 缓冲区的刷新时机

缓冲区刷新函数：`fflush`。

```c
#include <stdio.h>

int fflush(FILE *stream);
/*
	功能：刷新给定的文件指针对应的缓冲区
	参数：文件指针
	返回值：成功返回 0，失败返回 EOF 并置位错误码
*/
```

行缓存刷新时机

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
	// 1. 验证缓冲区如果没有达到刷新时机就不会将数据进行刷新
	printf("Hello World!"); // 在终端上输出一个 Hello World

	perror("Error"); // 在终端输出错误信息
	/*
		Output: Error: Success
		原因：要输出的 Hello World 被储存在缓存区中，后面的 while (1); 阻塞进程不让进程结束，
			缓存区没有到刷新时机，所以没有输出 Hello World
	*/

	while (1); // 阻塞不让进程结束

	// 2. 在程序结束时才会刷新缓存区

	return 0;
}
```

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
	// 3. 遇到换行时会刷新行缓存
	printf("Hello World\n");
	while (1); // 阻塞不让进程结束
	/*
		Output: Hello World
	*/
	return 0;
}
```

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
	// 4. 当输入输出发生切换时，会刷新行缓存
	int num = 0;
	printf("Input: ");
	scanf("%d", &num);
	/*
		Output: Input: 
	*/

	// 5. 当关闭行缓存对应的文件指针时，会刷新对应指针（实际上是调用了 fflush 进行刷新）
	printf("Hello World");
	fclose(stdout); // 关闭标准输出指针
	while (1);
	/*
		Output: Hello World
	*/

	return 0;
}
```

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
	// 6. 使用 fflush 函数手动刷新缓冲区时，行缓存会被刷新
	printf("Hello World");
	fflush(stdout);
	while (1);
	/*
		Output: Hello World
	*/
	return 0;
}
```

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
	// 7. 当缓冲区满了后，会刷新缓冲区
	// for (int i = 0; i < 1000; i++) putchar('A');
	// while (1);
	// // 无输出
	for (int i = 0; i < 1030; i++) putchar('A');
	while (1);
	// 输出 1030 个 A
	return 0;
}
```

全缓存刷新时机

```c
#include <stdio.h>

int main(int argc, const char *argv[]) {
	// 打开一个文件
	FILE *fp = NULL;
	if ((fp = fopen("file.txt", "w+")) == NULL) {
		perror("fopen error");
		return -1;
	}

	// 1. 当缓冲区刷新时机未到时，不会刷新全缓存
	fputs("Hello World", fp);
	while (1);
	// 无输出

	// 2. 当遇到换行时，不会刷新全缓存
	fputs("Hello World\n", fp);
	while (1);
	// 无输出

	// 3. 当程序结束后，会刷新全缓存
	fputs("Hello World", fp);
	while (1);
	// Output: Hello World

	// 4. 当输入输出发生切换时，会刷新全缓存
	fputs("Hello World", fp);
	fgetc(fp);
	while (1);
	// Output: Hello World

	// 5. 当关闭缓冲区对应的文件指针时，会刷新全缓存
	fputs("Hello World", fp);
	// 关闭文件
	fclose(fp);
	fp = NULL;
	while (1);
	// Output: Hello World

	// 6. 当手动使用 fflush 手动刷新缓冲区时，会刷新全缓存
	fputs("Hello World", fp);
	fflush(fp);
	while (1);
	// Output: Hello World

	// 7. 当缓冲区满了后，再向缓冲区存放数据时，会刷新全缓存
	for (int i = 0; i < 4097; i++) putchar('A');
	while (1);
	// 输出 4097 个 A

	// 关闭文件
	fclose(fp);
	fp = NULL;
	return 0;
}
```

对于不缓存的刷新时机，只要放入数据，立马进行刷新。

```c
#include <stdio.h>

int main(int argc, const char *argv[]) {
	perror("a"); // 向标准出错中放入数据
	fputs("A", stderr); // 向标准出错缓冲区写入字符串 A
	while (1); // 阻塞
	/*
		Output: a
			A
	*/
	return 0;
}
```

### 2.2.9 格式化读写：fprintf/fscanf

```c
#include <stdio.h>

int fprintf(FILE *stream, const char *format, ...);
/*
	功能：向指定的文件中输出一个格式串
	参数 1：文件指针
	参数 2：格式串，可以包含格式控制符，%d (整数)、%s (字符串)、%f (小数)、%lf (小数)
	参数 3：可变参数，输出项列表，参数个数由参数 2 中的格式控制符的个数决定
	返回值：成功返回输出的字符个数，失败返回一个负数
*/

int fscanf(FILE *stream, const char *format, ...);
/*
	功能：从指定的文件中以指定的格式读取数据，放入程序中
	参数 1：文件指针
	参数 2：格式串，可以包含格式控制符，%d (整数)、%s (字符串)、%f (小数)、%lf (小数)
	参数 3：可变参数，输入项地址列表，参数个数由参数 2 中的格式控制符的个数决定
	返回值：成功返回读入的项数，失败返回 EOF 并置位错误码
*/
```

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
	// 向标准输出文件中写入数据
	fprintf(stdout, "%d %lf %s\n", 1, 3.1415926, "Hello World");

	// 从标准输入中读取数据
	int num;
	fscanf(stdin, "%d", &num);

	// 输出读入的数据
	printf("num = %d\n", num);

	FILE *fp = NULL;
	// 以只写的形式打开外部文件
	if ((fp = fopen("./file.txt", "w")) == NULL) {
		perror("fopen error");
		return -1;
	}

	// 向文件中写入数据
	fprintf(fp, "%s %d\n", "admin", 123456);

	// 关闭文件
	fclose(fp);
	fp = NULL;

	// 以只读的形式重新打开文件
	if ((fp = fopen("./file.txt", "r")) == NULL) {
		perror("fopen error");
		return -1;
	}

	char usr[30];
	int pwd = 0;
	fscanf(fp, "%s %d", usr, &pwd); // 从文件中读取数据
	printf("%s %d\n", usr, pwd);    // 将读取到的数据输出到命令行

	// 关闭文件
	fclose(fp);
	fp = NULL;

	return 0;
}
```

练习：使用 `fprintf` 跟 `fscanf` 完成注册和登录功能，要求做个小菜单，三个功能，功能 0 是退出；功能 1 是注册；功能 2 是登录，用户输入登录账号和密码后，如果跟文件中匹配，则提示登陆成功，如果不全部匹配，则提示登陆失败；菜单可以循环调用。

```c
#include <stdio.h>
#include <string.h>

// 储存用户信息的结构体
struct User {
	char usrName[50], pwd[50], phone[15], sex;
	int age;
} users[20];

// 用户总数
int tot = 0;

/*
*   功能：展示主菜单，在 main 函数中被调用
*   参数：无
*   返回值：void
*/
void mainMenu();

/*
*   功能：创建一个新账号，在 mainMenu 函数中被调用
*   参数：无
*   返回值：int
*       0 表示创建失败
*       1 表示创建成功
*/
int create();

/*
*   功能：登录账号，在 mainMenu 函数中被调用
*   参数：无
*   返回值：int
*       0 表示登陆失败
*       1 表示登陆成功
*/
int login();

/*
*   功能：从 data.txt 文件中读取用户数据，在 main 函数中调用
*   参数：无
*   返回值：int
*       -1 代表读取失败
*       0 代表读入成功
*/
int read();

/*
*   功能：将数据储存到 data.txt 文件中，在 main 函数中调用
*   参数：无
*   返回值：int
*       -1 代表保存失败
*       0 代表保存成功
*/
int save();

int main(int argc, const char * argv[]) {
	// 读取数据
	if (read() == -1) {
		perror("Failed to read data");
		return -1;
	}

	// 展示菜单
	mainMenu();

	// 保存数据
	if (save() == -1) {
		perror("Failed to save data");
		return -1;
	}
	return 0;
}

void mainMenu() {
	int op;
	while (1) {
		printf("Please input an interer (0: exit, 1: register, 2: login): ");
		fflush(stdout);
		scanf("%d", &op);
		if (op == 0) break;
		else if (op == 1) create();
		else if (op == 2) login();
		else puts("Invalid Input");
		fflush(stdout);
	}
}

int create() {
	if (tot >= 20) {
		perror("Users Full");
		return 0;
	}
	char usrName[50], pwd[50], phone[15], sex;
	int age;
	puts("Please input your information");
	printf("(Format: userName password phone sex age): ");
	fflush(stdout);
	int flag = scanf("%s %s %s %c %d", usrName, pwd, phone, &sex, &age);
	fflush(stdout);
	if (flag < 5) {
		perror("Input Failed");
		return 0;
	}
	strcpy(users[tot].usrName, usrName);
	strcpy(users[tot].pwd, pwd);
	strcpy(users[tot].phone, phone);
	users[tot].sex = sex, users[tot].age = age;
	tot++;
	return 1;
}

int login() {
	char usrName[50], pwd[50];
	puts("Please input your User Name and PassWord");
	printf("(Format: userName password): ");
	fflush(stdout);
	int flag = scanf("%s %s", usrName, pwd);
	if (flag < 2) {
		perror("Invalid Input");
		return 0;
	}
	for (int i = 0; i < tot; i++) {
		if (!strcmp(users[i].usrName, usrName) && !strcmp(users[i].pwd, pwd)) {
			puts("Login Success");
			fflush(stdout);
			return 1;
		}
	}
	perror("User Not Found or Password Not match");
	return 0;
}

int read() {
	FILE *fp = NULL;
	if ((fp = fopen("./data.txt", "r")) == NULL) {
		perror("Open Error");
		return -1;
	}

	while (fscanf(fp, "%s %s %s %c %d",
			users[tot].usrName,
			users[tot].pwd,
			users[tot].phone,
			&users[tot].sex,
			&users[tot].age
		) != EOF
	) tot++;

	fclose(fp);
	fp = NULL;
	return 0;
}

int save() {
	FILE *fp = NULL;
	if ((fp = fopen("./data.txt", "w")) == NULL) {
		perror("Open Error");
		return -1;
	}

	for (int i = 0; i < tot; i++)
		fprintf(fp, "%s %s %s %c %d\n",
			users[i].usrName,
			users[i].pwd,
			users[i].phone,
			users[i].sex,
			users[i].age
		);

	fclose(fp);
	fp = NULL;
	return 0;
} 
```

### 2.2.10 格式串转字符串存入字符数组中：sprintf/snprintf

1. 目前所学的函数中，`printf` 是向终端打印一个格式串，`fprintf` 是向外部文件中打印一个格式串。
2. 有时候，想要将多个不同数据类型的数据，组成一个字符串放入字符数组中，此时我们就可以使用 `sprintf` 或 `snprintf`。

```c
#include <stdio.h>

int sprintf(char *str, const char *format, ...);
/*
	功能：将指定的格式串转换为字符串，放入字符数组中
	参数 1：字符数组的起始地址
	参数 2：格式串，可以包含多个格式控制符
	参数 3：可变参数
	返回值：成功返回转换的字符个数，失败返回 EOF

	对于上述函数而言，用一个小的容器去存储一个大的转换后的字符时，可能会出现指针越界的段错误，为了安全起见，引入了 snprintf
*/

int snprintf(char *str, size_t size, const char *format, ...);
/*
	功能：将格式串中最多 size - 1 个字符转换为字符串，存放到字符数组中
	参数 1：字符数组的起始地址
	参数 2：要转换的字符个数，最多为 size - 1
	参数 3：格式串，可以包含多个格式控制符
	参数 4：可变参数
	返回值：如果转换的字符小于 size 时，返回值就是成功转换字符的个数，如果大于 size 则只转换 size - 1 个字符，返回值就是 size，失败返回 EOF
*/
```

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
	char buf[128];
	sprintf(buf, "%s %d %lf", "name", 20, 99.5);
	printf("%s\n", buf);
	// Output: name 20 99.500000
	// 存在的问题：转换之后的字符串长度超过 buf，可能会出现段错误

	snprintf(buf, sizeof(buf), "%s %d %lf", "name", 20, 99.5);
	printf("%s\n", buf);
	// Output: name 20 99.500000
	return 0;
}
```

### 2.2.11 模块化读写：fread/fwrite

```c
#include <stdio.h>

size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
/*
	功能：从 stream 指向的文件中，读取 nmemb 项数据，每一项的大小为 size，将整个结果放入 ptr 指向的容器中
	参数 1：容器指针，是一个 void* 类型，表示可以存储任意类型的数据
	参数 2：要读取数据每一项的大小
	参数 3：要读取数据的项数
	参数 4：文件指针
	返回值：成功返回 nmemb，就是成功读取的项数，失败返回小于项数的值，或者是 0
*/

size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
/*
	功能：向 stream 指向的文件中写入 nmemb 项数据，每一项的大小为 size，数据的起始地址为 ptr
	参数 1：要写入数据的起始地址
	参数 2：每一项的大小
	参数 3：要写入的总项数
	参数 4：文件指针
	返回值：成功返回写入的项数，失败返回小于项数的值，或者是 0
*/
```

1. 字符串的读写

```c
#include <stdio.h>
#include <string.h>

int main(int argc, const char * argv[]) {

	// 定义文件指针，以只写的形式打开文件
	FILE *fp = NULL;
	if ((fp = fopen("./file.txt", "w")) == NULL) {
		perror("fopen error");
		return -1;
	}

	// 定义要存储的字符串
	char wbuf[128];
	while (1) {
		printf("Please Input: ");
		fgets(wbuf, sizeof(wbuf), stdin);
		wbuf[strlen(wbuf) - 1] = '\0'; // 将换行换成字符串结束标志

		// 判断输入的是否为 quit
		if (!strcmp(wbuf, "quit")) break;

		fwrite(wbuf, strlen(wbuf), 1, fp);
		fwrite("\n", 1, 1, fp);
		fflush(fp); // 刷新缓冲区
		puts("Input Success");
	}

	// 关闭文件
	fclose(fp);
	fp = NULL;

	if ((fp = fopen("./file.txt", "r")) == NULL) {
		perror("fopen error");
		return -1;
	}

	char rbuf[10];
	int res = fread(rbuf, 1, sizeof(rbuf), fp); // 从文件中读取字符串
	fwrite(rbuf, 1, res, stdout); // 将读入的字符串输出到终端
	/*
		Output: Hello
			Worl
		原因：Hello 后面有换行符 \n，所以第二行只读入了四个字符
	*/
	return 0;
}
```

2. 整数的读写

```c
#include <stdio.h>

int main(int argc, const char * argv[]) {
	FILE *fp = NULL;

	// 以只写的方式打开文件
	if ((fp = fopen("./file.txt", "w")) == NULL) {
		perror("fopen error");
		return -1;
	}

	// 定义写入文件中的数据
	int num = 16;
	fwrite(&num, sizeof(num), 1, fp);
	// 该整数在文件中并不是以整数的形式储存，而是以二进制的形式存储

	// 关闭文件
	fclose(fp);
	fp = NULL;

	// 以只读的形式再次打开文件
	if ((fp = fopen("./file.txt", "r")) == NULL) {
		perror("fopen error");
		return -1;
	}

	// 定义接收数据的变量
	int key = 0;
	fread(&key, sizeof(key), 1, fp);

	printf("%d\n", key);

	// 关闭文件
	fclose(fp);
	fp = NULL;
	return 0;
}
```

3. 结构体数据的读写

```c
#include <stdio.h>

// 定义学生结构体
typedef struct {
	char name[20];
	int age;
	double gpa;
} Student;

int main(int argc, const char * argv[]) {
	FILE *fp = NULL;

	// 以只写的方式打开文件
	if ((fp = fopen("./file.txt", "w")) == NULL) {
		perror("fopen error");
		return -1;
	}

	// 定义三个学生
	Student s[3] = {
		{ "Alice", 10, 3.5 },
		{ "Bob", 20, 3.8 },
		{ "Charlie", 30, 4.0 }
	};

	// 将学生信息写入文件
	fwrite(s, sizeof(Student), 3, fp);

	// 关闭文件
	fclose(fp);
	fp = NULL;

	// 以只读的形式再次打开文件
	if ((fp = fopen("./file.txt", "r")) == NULL) {
		perror("fopen error");
		return -1;
	}

	// 从文件中读取学生信息
	Student tmp;
	for (int i = 0; i < 3; i++) {
		fread(&tmp, sizeof(Student), 1, fp);
		printf("name: %s, age: %d, gpa: %lf\n", tmp.name, tmp.age, tmp.gpa);
	}
	/*
		Output: name: Alice, age: 10, gpa: 3.500000
			name: Bob, age: 20, gpa: 3.800000
			name: Charlie, age: 30, gpa: 4.000000
	*/

	// 关闭文件
	fclose(fp);
	fp = NULL;
	return 0;
}
```

### 2.2.12 关于文件光标：fseek/ftell/rewind

```c
int fseek(FILE *stream, long offset, int whence);
/*
	功能：移动文件光标位置，将光标从指定位置处进行前后偏移
	参数 1：文件指针
	参数 2：偏移量
		> 0：表示从指定位置向后偏移 n 个字节
		< 0：表示从指定位置向前偏移 n 个字节
		= 0：在指定位置处不偏移
	参数3：偏移的起始位置
		SEEK_SET：文件起始位置
		SEEK_CUR：文件指针当前位置
		SEEK_END：文件结束位置
	返回值：成功返回 0，失败返回 -1 并置位错误码
*/

long ftell(FILE *stream);
/*
	功能：获取文件指针当前的偏移量
	参数：文件指针
	返回值：成功返回文件指针所在的位置，失败返回 -1 并置位错误码
*/

// eg:
fseek(fp, 0, SEEK_END);		// 将光标定位在结尾
ftell(fp);	// 该函数的返回值，就是文件大小

void rewind(FILE *stream);
/*
	功能：将文件光标定位在开头，相当于 fseek(fp, 0, SEEK_SET);
	参数：文件指针
	返回值：无
*/
```

```c
#include <stdio.h>

// 定义学生结构体
typedef struct {
	char name[20];
	int age;
	double gpa;
} Student;

int main(int argc, const char * argv[]) {
	FILE *fp = NULL;

	// 以只写的方式打开文件
	if ((fp = fopen("./file.txt", "w")) == NULL) {
		perror("fopen error");
		return -1;
	}

	// 定义三个学生
	Student s[3] = {
		{ "Alice", 10, 3.5 },
		{ "Bob", 20, 3.8 },
		{ "Charlie", 30, 4.0 }
	};

	// 将学生信息写入文件
	fwrite(s, sizeof(Student), 3, fp);

	// 关闭文件
	fclose(fp);
	fp = NULL;

	// 以只读的形式再次打开文件
	if ((fp = fopen("./file.txt", "r")) == NULL) {
		perror("fopen error");
		return -1;
	}

	fseek(fp, 0, SEEK_END);
	printf("The size of file is %ld.\n", ftell(fp));
	// Output: The size of file is 96.

	// 读取第二个学生的信息
	Student tmp;
	fseek(fp, sizeof(Student), SEEK_SET);
	fread(&tmp, sizeof(tmp), 1, fp);
	printf("name: %s, age: %d, gpa: %lf\n", tmp.name, tmp.age, tmp.gpa);
	// Output: name: Bob, age: 20, gpa: 3.800000

	// 关闭文件
	fclose(fp);
	fp = NULL;
	return 0;
}
```

### 2.2.13 补充知识

1. 关于追代码工具 ctags 的使用
   - 安装 ctags：`sudo yum install ctags`
   - 进入 `/usr/include` 目录下，执行 `sudo ctags -R` 指令
   - 该目录下会多出一个 `tags` 的索引文件
   - 追相关内容的指令：`vi -t 内容`
     - 继续像后缀内容：`ctrl + ]`
     - 退出追代码工具：底行模式下输入 `q`，回车
2. vscode 中关于自定义片段的设置
   - 在vscode中使用组合键：`ctrl + shift + p` 调出控制面板
   - 输入 `user snippets` 回车
   - 输入自定义片段文件名称，打开自定义文件
   - 输入以下程序
	```json
	"Simple C++ Program": {
		"prefix": "main",
		"body": [
			"#include<iostream>",
			"using namespace std;",
			"int main(int argc, const char *argv[])",
			"{",
			"	$1",
			"	return 0;",
			"}"
		],
		"description": "Simple C++ program template"
	}
	```
3. 一个头文件中可以包含多个头文件内容
   - 创建一个头文件，例如 `myhead.h`
   - 将能用到的所有头文件都放入该文件中
   - 将 `myhead.h` 文件，移动到根目录下的 `usr` 目录中的 `include` 目录下：`sudo mv myhead.h /usr/include`
   - 常用的头文件
	```cpp
	#include <iostream>	// C++标准输入输出流
	#include <string>	// C++字符串类对象
	#include <iomanip>	// C++中的格式化输入输出
	#include <stdio.h>	// C语言的输入输出头文件
	#include <math.h>	// C语言的数据函数头文件
	#include <string.h>	// C语言字符串处理
	#include <stdlib.h> // C语言库头文件
	```
   - 重新设置用户的自定义片段
	```json
	"Simple C++ Program": {
		"prefix": "main",
		"body": [
			"#include <myhead.h>",
			"int main(int argc, const char *argv[])",
			"{",
			"	$1",
			"	return 0;",
			"}"
		],
		"description": "Simple C++ program template"
	}
	```

## 2.3 文件 IO

定义：文件 IO 就是通过系统调用（内核提供的函数）实现，只要使用的文件 IO 接口，那么进程就会从用户空间向内核空间进行一次切换。标准 IO 的实现中，也是在内部调用了文件 IO 操作。该操作效率较低，因为没有缓冲区的概念。文件 IO 常用的操作：`open`、`close`、`read`、`write`、`lseek`。

### 2.3.1 文件描述符

1. 文件描述符的本质是无符号整数，在使用 `open` 函数打开文件时，就会产生一个用于操作文件的句柄，这就是文件描述符。
2. 在一个进程中，能够打开的文件描述符是有限制的，一般是 $1024$ 个，范围在 $[0,1023]$，可以通过指令 `ulimit -a` 进行查看，如果要更改这个限制，可以通过指令 `ulimit -n` 数字，进行更改
3. 文件描述符的使用原则一般是最小未分配原则。
4. 特殊的文件描述符：$0$、$1$、$2$，这三个文件描述符在一个进程启动时就默认被打开了，分别表示标准输入、标准输出、标准错误。

```c
#include <stdio.h>

int main(int argc, const char *argv[]) {
	// 分别输出标准输入、标准输出、标准出错文件指针对应的文件描述符
	printf("stdin->_fileno = %d\n", stdin->_fileno);	// 0
	printf("stdout->_fileno = %d\n", stdout->_fileno);	// 1
	printf("stderr->_fileno = %d\n", stderr->_fileno);	// 2
	return 0;
}
```

输出

```
stdin->_fileno = 0
stdout->_fileno = 1
stderr->_fileno = 2
```

### 2.3.2 打开文件：open

```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);	// mode 是可变参数
/*
	参数 1：要打开的文件文件路径
	参数 2：打开模式
		以下三种方式必须选其一：O_RDONLY（只读）、O_WRONLY（只写）、O_RDWR（读写）
		以下的打开方式可以有零个或多个，跟上述的方式一起用位或运算连接到一起
		O_CREAT：用于创建文件，如果文件不存在，则创建文件，如果文件存在，则打开文件，如果 flag 中包含了该模式，则函数的第三个参数必须要加上
		O_APPEND：以追加的形式打开文件，光标定位在结尾
		O_TRUNC：清空文件
		O_EXCL：常跟 O_CREAT 一起使用，确保本次要创建一个新文件，如果文件已经存在，则 open 函数报错
		eg:
			"w": O_WRONLY | O_CREAT | O_TRUNC
			"w+": O_RDWR | O_CREAT | O_TRUNC
			"r": O_RDONLY
			"r+": O_RDWR
			"a": O_WRONLY | O_CREAT | O_APPEND
			"a+": O_RDWR | O_CREAT | O_APPEND
	参数 3：如果参数 2 中的 flag 中有 O_CREAT 时，表示创建新文件，参数 3 就必须给定，表示新创建的文件的权限
		如果当前参数给定了创建的文件权限，最终的结果也不一定是参数 3 的值，系统会用你给定的参数 3 的值，与系统的 umask 取反的值进行位与运算后，才是最终创建文件的权限 (mode & ~umask)
		当前终端的 umask 的值，可以通过指令 umask 来查看，一般默认为 0022，表示当前进程所在的组中对该文件没有写权限，其他用户对该文件也没有写权限
		当前终端的 umask 的值是可以更改的，通过指令：umask 数字进行更改，这种方式只对当前终端有效
		普通文件的权限一般为：0644，表示当前用户没有可执行权限，当前组中其他用户和其他组中的用户都只有读权限
		目录文件的权限一般为：0755，表示当前用户具有可读、可写、可执行，当前组中其他用户和其他组中的用户都没有可写权限
		注意：如果不给权限，那么当前创建的权限会是一个随机值
	返回值：成功返回打开文件的文件描述符，失败返回 -1 并置位错误码
*/
```

**Example 1**

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int main(int argc, const char *argv[]) {
	int fd = -1;
	// 以只写的形式打开文件，如果文件不存在则创建文件
	// 如果创建文件时没有给定权限，则该文件的权限是随机权限
	if ((fd = open("./file.txt", O_WRONLY | O_CREAT)) == -1) {
		perror("open error");
		return -1;
	}

	printf("open success: fd = %d\n", fd);  // 3
	// 由于 0, 1, 2 已经被使用了，所以 fd = 3
	return 0;
}
```

**Example 2**

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int main(int argc, const char *argv[]) {
	int fd = -1;
	// 以只写的形式打开文件，如果文件不存在则创建文件
	// 如果创建文件时没有给定权限，则该文件的权限是随机权限
	if ((fd = open("./file.txt", O_WRONLY | O_CREAT, 0777)) == -1) {
		perror("open error");
		return -1;
	}
	// 0777 & ~0022 = 0755

	printf("open success: fd = %d\n", fd);  // 3
	// 由于 0, 1, 2 已经被使用了，所以 fd = 3
	return 0;
}
```

### 2.3.3 文件关闭：close

```c
#include <unistd.h>
int close(int fd);
/*
	功能：关闭文件描述符对应的文件
	参数：文件描述符
	返回值：成功返回 0，失败返回 -1 并置位错误码
*/
```

```cpp
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

int main(int argc, const char *argv[]) {
	int fd = -1;
	// 以只读的形式打开文件，如果文件不存在则创建文件
	// 如果创建文件时没有给定权限，则该文件的权限是随机权限
	if ((fd = open("./file.txt", O_WRONLY | O_CREAT, 0777)) == -1) {
		perror("open error");
		return -1;
	}
	// 0777 & ~0022 = 0755

	printf("%d\n", close(fd));  // 关闭 fd 引用的文件
	// Output: 0
	fd = -1;
	return 0;
}
```

### 2.3.4 读写操作：read/write

```c
#include <unistd.h>
ssize_t read(int fd, void *buf, size_t count);
/*
	功能：从 fd 文件描述符引用的文件中读取 count 的字符放入 buf 对应的容器中
	参数 1：已经打开文件对应的文件描述符
	参数 2：容器的起始地址
	参数 3：要读取的字符个数
	返回值：成功返回读取的字符个数，这个个数可能会小于 count 的值，失败返回 -1 并置位错误码
*/

#include <unistd.h>
ssize_t write(int fd, const void *buf, size_t count);
/*
	功能：将 buf 容器中的 count 个数据，写入到 fd 引用的文件中
	参数 1：已经打开的文件的文件描述符
	参数 2：要写入的数据的起始地址
	参数 3：要写入数据的个数
	返回值：成功返回写入的字符个数，这个个数可能小于 count，失败返回 -1 并置位错误码
*/
```

```cpp
#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

int main(int argc, const char *argv[]) {
	int fd = -1;    // 文件描述符

	// 以只写的形式打开文件，如果文件不存在则创建文件
	// 如果创建文件时没有给定权限，则该文件的权限是随机权限
	if ((fd = open("./file.txt", O_WRONLY | O_CREAT, 0644)) == -1) {
		perror("open error");
		return -1;
	}
	// 0644 & ~0022 = 0644

	// 对文件进行读写操作
	char wBuf[128] = "Hello World\n";
	write(fd, wBuf, strlen(wBuf));

	// 关闭 fd 指向的文件
	close(fd);
	fd = -1;

	// 以只读的形式打开文件
	if ((fd = open("./file.txt", O_RDONLY)) == -1) {
		perror("open error");
		return -1;
	}

	// 从文件中读取数据
	char rBuf[15];
	int res = read(fd, rBuf, sizeof(rBuf));

	// 向终端输出数据
	write(1, rBuf, res);
	printf("%s", rBuf);
	/*
		Output: Hello World
			Hello World
	*/

	// 关闭 fd 指向的文件
	close(fd);
	fd = -1;
	return 0;
}
```

### 2.3.5 关于光标的操作：lseek

```c
#include <sys/types.h>
#include <unistd.h>

off_t lseek(int fd, off_t offset, int whence);
/*
	功能：移动光标的位置，并返回光标现在所在的位置
	参数 1：文件描述符
	参数 2：偏移量
		> 0：表示从指定位置向后偏移 offset 个字节
		< 0：表示从指定位置向前偏移 offset 个字节
		= 0：在指定位置处不偏移
	参数3：偏移的起始位置
		SEEK_SET：文件起始位置
		SEEK_CUR：文件指针当前位置
		SEEK_END：文件结束位置
	返回值：光标现在所在的位置
	注意：lseek = fseek + ftell
*/
```

```cpp
#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

int main(int argc, const char *argv[]) {
	int fd = -1;    // 文件描述符

	// 以只写的形式打开文件，如果文件不存在则创建文件
	// 如果创建文件时没有给定权限，则该文件的权限是随机权限
	if ((fd = open("./file.txt", O_RDWR | O_TRUNC | O_CREAT, 0644)) == -1) {
		perror("open error");
		return -1;
	}
	// 0644 & ~0022 = 0644

	// 对文件进行读写操作
	char wBuf[128] = "Hello World\n";
	write(fd, wBuf, strlen(wBuf));

	// 将光标移动到文件起始位置
	lseek(fd, 0, SEEK_SET);

	// 从文件中读取数据
	char rBuf[15];
	int res = read(fd, rBuf, sizeof(rBuf));

	// 向终端输出数据
	write(1, rBuf, res);
	printf("%s", rBuf);
	/*
		Output: Hello World
			Hello World
	*/

	// 关闭 fd 指向的文件
	close(fd);
	fd = -1;
	return 0;
}
```

练习：将一个 bmp 格式的图片信息读取出来，并将该图片的相关内容（颜色）进行更改。关于 bmp 图像格式可以参考：[bmp图片](https://upimg.baike.so.com/doc/5386615-5623077.html)。

如何将 Windows 下的文件，复制到 Linux 系统的 Centos 下？
1. 在 Windows 下调出 cmd 窗口
2. 输入指令：`scp 要拷贝的文件路径 Linux下的用户名@ip地址:Linux下存放的地址路径`
	eg: `scp C:\Users\鹏程万里\Desktop\wukong.bmp zpp@192.168.31.88:/home/zpp/IO/day2`

```cpp
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

int main(int argc, const char *argv[]) {
	int fd = -1;    // 文件描述符

	// 以读写的形式打开文件
	if ((fd = open("./AG.bmp", O_RDWR)) == -1) {
		perror("open error");
		return -1;
	}

	// 获取文件大小
	int sz1 = 0, sz2 = 0;

	// 获取文件大小方法 1
	sz1 = lseek(fd, 0, SEEK_END);

	// 获取文件大小方法 2
	lseek(fd, 2, SEEK_SET);
	read(fd, &sz2, 4);
	// 将文件头的 3 - 6 字节的内容读取出来：文件头的第 3 - 6 字节储存的是文件的大小

	printf("size1 = %d, size2 = %d\n", sz1, sz2);
	// Output: size1 = 17972234, size2 = 17972234
	// 宽度：3161 像素，高度：1895 像素
	// bmp 格式的文件要求每行像素数据的字节数必须是 4 的倍数，即 4 字节对齐。
	// 如果未对齐，需要在行末添加填充字节。
	// 实际文件大小：54 + ceil(3161.0 / 4) * 4 * 1895 = 17972234

	// 将文件光标向后偏移 54 字节，跳过文件头和信息头
	lseek(fd, 54, SEEK_SET);

	// 定义以像素的颜色，颜色的规律是 bgr
	unsigned char color[3] = { 0, 255, 0 }; // 定义一个绿色
	for (int i = 0; i < 100; i++) {
		for (int j = 0; j < 3161; j++) {
			// 此时的 (i, j) 定位的就是一个像素点，
			// 是一个三字节为单位的像素点
			write(fd, color, sizeof(color)); // 将当前光标所辖像素点变成绿色
		}
		// 在每一行最后添加一个填充字节，进行字节对齐
		unsigned char padding = 0;
		write(fd, &padding, 1);
	}

	// 关闭文件
	close(fd);
	fd = -1;
	return 0;
}
```

课后练习：使用文件 IO 来完成两个文件的拷贝，可以完成两个图像的拷贝工作

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>

int main(int argc, const char * argv[]) {
	int src = -1, dst = -1;
	// 以只读形式打开源文件
	if ((src = open("./AG.bmp", O_RDONLY)) == -1) {
		perror("Open Source Error");
		return -1;
	}

	// 以只写形式打开目标文件，如果不存在则创建新文件；否则，清空文件
	if ((dst = open("./newAG.bmp", O_WRONLY | O_TRUNC | O_CREAT, 0644)) == -1) {
		perror("Open Destination Error");
		return -1;
	}

	// 复制文件
	int size = lseek(src, 0, SEEK_END);
	lseek(src, 0, SEEK_SET);
	// 如果数组过大可能会导致爆栈，从而导致段错误
	// 如果数组过小则会进行频繁的用户空间与内核空间的切换，到时时间效率低
	unsigned char pic[size / 100];
	for (int i = 1; i <= 100; i++) {
		read(src, pic, sizeof(pic));
		write(dst, pic, sizeof(pic));
	}

	// 关闭文件
	close(src), close(dst);
	src = -1, dst = -1;
	return 0;
}
```

# 三、多进程编程

## 3.1 多进程理论基础

### 3.1.1 引入目的

1. 实现 **多任务并发执行** 的一种途径
2. 可以实现数据在多个进程之间进行通信，共同处理整个程序的相关数据，提供工作效率

### 3.1.2 多进程相关概念

1. **进程是程序的一次执行过程**，有一定的生命周期，包含了创建态、就绪态、执行态、挂起态、死亡态。
2. **进程是计算机资源分配的基本单位**，系统会给每个进程分配 $0-4G$ 的虚拟内存，其中 $0-3G$ 是用户空间， $3-4G$ 是内核空间。
   - 其中多个进程中 $0-3G$ 的用户空间是相互独立的，但是， $3-4G$ 的内核空间是相互共享的。
   - 用户空间细分为：栈区、堆区、静态区。
3. 进程的调度机制：时间片轮询上下文切换机制。

![img7](./img/img7.png#pic_center)

4. 并发和并行的区别：
   - 并发：针对于单核 CPU 系统在处理多个任务时，使用相关的调度机制，实现多个任务进行细化时间片轮询时，在宏观上感觉是多个任务同时执行的操作，同一时刻，只有一个任务在被 CPU 处理。
   - 并行：是针对于多核 CPU 而言，处理多个任务时，同一时间，每个 CPU 处理的任务之间是并行的，实现的是真正意义上多个任务同时执行的。

### 3.1.3 进程的内存管理（重点了解）

![img8](./img/img8.png#pic_center)

1. 物理内存：内存条上（硬件上）真正存在的存储空间。
2. 虚拟内存：程序运行后，通过内存映射单元，将物理内存映射出 $4G$ 的虚拟内存，供进程使用。

### 3.1.4 进程和程序的区别

- 进程是动态的，进程是程序的一次执行过程，是有生命周期的，进程会被分配 $0-3G$ 的用户空间，进程是在内存上存着的。
- 程序是静态的，没有所谓的生命周期，程序存储在磁盘设备上的二进制文件
	hello.cpp $\to$ g++ $\to$ hello

### 3.1.5 进程的种类

进程一共有三种：交互进程、批处理进程、守护进程

1. 交互进程：它是由 shell 控制，可以直接和用户进行交互的，例如文本编辑器。
2. 批处理进程：内部维护了一个队列，被放入该队列中的进程，会被统一处理。例如 g++ 编译器的一步到位的编译。
3. 守护进程：脱离了终端而存在，随着系统的启动而运行，随着系统的退出而停止。例如：操作系统的服务进程。

### 3.1.6 进程 PID 的概念

1. PID (Process ID)：进程号，进程号是一个大于等于 $0$ 的整数值，是进程的唯一标识，不可以重复。
2. PPID (Parent Process ID)：父进程号，系统中允许的每个进程，都是拷贝父进程资源得到的。
3. 在 Linux 系统中的 `/proc` 目录下的数字命名的目录其实都是一个进程。

<!-- img -->

### 3.1.7 特殊的进程

1. $0$ 号进程 (idel)：它是由 Linux 操作系统启动后运行的第一个进程，也叫空闲进程，当没有其他进程运行时，会运行该进程。他也是 $1$ 号进程和 $2$ 号进程的父进程。
2. $1$ 号进程 (init)：他是由 $0$ 号进程创建出来的，这个进程会完成一些硬件的必要初始化工作，除此之外，还会收养孤儿进程。
3. $2$ 号进程 (kthreadd)：也称调度进程，这个进程也是由 $0$ 号进程创建出来的，主要完成任务调度问题。
4. 孤儿进程：当前进程还正在运行，其父进程已经退出了。
	说明：每个进程退出后，其分配的系统资源应该由其父进程进行回收，否则会造成资源的浪费。
5. 僵尸进程：当前进程已经退出了，但是其父进程没有为其回收资源。

### 3.1.8 有关进程操作的指令

1. `ps` 指令：能够查看当前运行的进程相关属性。

`ps -ef`：能够显示进程之间的关系。
```
// code here

UID：用户 ID 号
PID：进程号
PPID：父进程号
C：用处不大
STIME：开始运行的时间
TTY：如果是问号表示这个进程不依赖于终端而存在
CDM：名称
```

`ps -ajx`：能够显示当前进程的状态

```
// code here

PGID：进程组 ID
SID：会话组 ID
STAT：进程的状态
```

`ps -aux`：可以查看当前进程对 CPU 和内存的占用率

```
// code here

%CPU：CPU 占用率
%MEM：内存占用率
```

2. `top`：动态查看进程的相关属性

```
// code here
```

3. `kill` 指令：发送信号的指令
	使用方式：`kill -信号号 进程号`
	可以通过指令：`kill -l` 查看能够发送的信号有哪些

```
1) SIGHUP	2) SIGINT	3) SIGQUIT	4) SIGILL	5) SIGTRAP
6) SIGABRT	7) SIGBUS	8) SIGFPE	9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX

1、一共可以发射 62 个信号，前 32 个是稳定信号，后面是不稳定信号
2、常用的信号
SIGHUP：当进程所在的终端被关闭后，终端会给运行在当前终端的每个进程发送该信号，默认结束进程
SIGINT：中断信号，当用户键入 ctrl + c 时发射出来
SIGQUIT：退出信号，当用户键入 ctrl + / 是发送，退出进程
SIGKILL：杀死指定的进程
SIGSEGV：当指针出现越界访问时，会发射，表示段错误
SIGPIPE：当管道破裂时会发送该信号
SIGALRM：当定时器超时后，会发送该信号
SIGSTOP：暂停进程，当用户键入 ctrl+z 时发射
SIGTSTP：也是暂停进程
SIGTSTP、SIGUSR2：留给用户自定义的信号，没有默认操作
SIGCHLD：当子进程退出后，会向父进程发送该信号
3、有两个特殊信号：SIGKILL 和 SIGSTOP，这两个信号既不能被捕获，也不能被忽略
```

4. `pidof`：查看进程的进程号
	使用方式：`pidof 进程名`

<!-- img -->

### 3.1.9 进程状态的切换

1. 可以通过 `man ps` 进行查看进程的状态

```
进程主状态：
	D	uninterruptible sleep (usually IO)	不可中断的休眠态，通常是IO 操作
	R	running or runnable (on run queue)	运行态
	S	interruptible sleep (waiting for an event to complete)	可中断的休眠态
	T	stopped by job control signal	停止态
	t	stopped by debugger during the tracing	调试时的停止态
	W	paging (not valid since the 2.6.xx kernel)	已经弃用的状态
	X	dead (should never be seen)	死亡态
	Z	defunct ("zombie") process, terminated but not reaped by its parent	僵尸态
附加态：
	<	high-priority (not nice to other users)	高优先级进程
	N	low-priority (nice to other users)	低优先级进程
	L	has pages locked into memory (for real-time and custom IO)	锁在内存中的进程
	s	is a session leader	会话组组长
	l	is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)	包含多线程的进程
	+	is in the foreground process group	前台运行的进程
```

2. 状态切换的实例

	(1) 如果有停止的进程，可以在终端输入指令：`jobs -l` 查看停止进程的作业号。
	(2) 通过使用指令：`bg 作业号` 实现将停止的进程进入后台运行状态，如果只有一个停止的进程，输入 bg 不加作业号也可以。
	(3) 对后台运行的进程，输入 fg 作业号 实现将后台运行的进程切换到前台运行。
	(4) 直接将可执行程序后台运行：`./可执行程序 &`

<!-- img -->

<!-- img -->

## 3.2 多进程实现

进程的创建过程，是子进程通过拷贝父进程得到的，新进程的创建直接拷贝父进程的资源，只需改变很少部分的数据即可，保留了父进程的大部分的数据信息（遗传基因），所以这个拷贝过程，系统通过一个函数 `fork` 来自动完成。

### 3.2.1 进程的创建：fork

## 3.3 进程间通信 IPC



# 四、多线程



# 五、网络编程基础

## 5.3



# 六、制作 HTTP 服务器



# 七、项目篇

## 7.1 基于 UDP 的 TFTP 文件传输项目

[TFTP 项目](https://github.com/nuo534202/My-01-University-Notes/tree/main/C%2B%2B%20Advanced/%E5%9F%BA%E4%BA%8E%20UDP%20%E7%9A%84%20TFTP%20%E6%96%87%E4%BB%B6%E4%BC%A0%E8%BE%93%E9%A1%B9%E7%9B%AE)

## 7.2 基于 TCP 的网络聊天室

## 7.3 基于 TCP 的网络电子词典
