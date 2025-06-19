<h1 style="text-align:center;">C++ 进阶 —— 01 University</h1>

主要内容是 Linux 下的网络编程，操作系统是 CentOS。接下来的内容都是我的个人笔记。

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

### 1.2.4 库的制作（动态库于静态库）

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

库在linux系统中分为两类，分别是静态库和动态库。

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
#define _OPERATOE_H

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

### 1.3.10 变量

(1) 变量基础

  - 变量定义：`变量名 = 变量值`。
  - 变量的赋值
    - 追加赋值：`+=` $\to$ 在原有的基础上追加相关内容。
    - 条件赋值：`?=` $\to$ 如果之前没有值，则为变量赋值，如果之前有值，则不进行赋值。
  - 变量的使用：`$(变量名)` 或者 `${变量名}`。

(2) 变量的分类

  - 立即展开变量
    - 使用 `:=` 操作符进行赋值。
    - 在解析阶段直接赋值常量字符串。
  - 延迟展开变量
    - 使用 `=` 操作符进行赋值。
    - 将最后一次赋值的结果给变量名使用。
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
ifeq else endif
ifneq else endif
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

1. 在不使用 CMake 时，编译工程如下

![img1](./img/img1.png#pic_center)

2. 在上面的机制中，如果要向工程文件中添加一个源文件 `bar.cpp`

![img2](./img/img2.png#pic_center)

3. 使用 CMake 来管理工程的状态

![img3](./img/img3.png#pic_center)

4. 使用 CMkake 管理工程，向工程中添加一个新文件 `bar.cpp`

![img4](./img/img4.png#pic_center)

### 1.5.3 语法特性介绍

1. 基本语法：`指令 (参数1 参数2 ...)`
   - 参数使用括号括起来。
   - 参数之间使用空格或分号隔开。
2.  注意：指令是大小写无关的，但是参数和变量是大小写相关的。
```cmake
set(HELLO hello.cpp) # 定义一个变量名叫 HELLO 变量的值为 hello.cpp
add_executable(hello main.cpp hello.cpp)  # 通过main.cpp 和 hello.cpp 编译生成 hello 可执行程序
ADD_EXECUTABLE(hello main.cpp ${HELLO})   # 作用同上
```
3. 变量使用 `${}` 进行取值，但是在 `if` 控制语句中，是直接使用变量名的。
   - if(HELLO) 是正确的
   - if(${HELLO}) 是不正确的
4. 语句不以分号结束。

### 1.5.4 重要的指令

1. `cmake_minimum_required`：指定 CMake 的最小版本支持，一般作为第一条 CMake 指令。
```cmake
# CMake设置最小支持版本为 2.8
cmake_minimum_required(VERSION 2.8)
```
2. `project`：定义工程的名称，并可以指定工程支持的语言。
```cmake
# 指定工程的名称为 HELLOWORLD
project(HELLOWORLD CXX)	# 表示工程名为 HELLOWORLD 使用的语言为 C++
```
3. `set`：显式定义变量。
```cmake
# 定义变量 SRC 其值为 sayhello.cpp hello.cpp
set(SRC sayhello.cpp hello.cpp)
```
4. `add_executable`：通过依赖生成可执行程序。
```cmake
# 编译main.cpp 生成 main 的可执行程序
add_executable(main main.cpp)
```
5. `include_directories`：向工程添加多个特定的头文件搜索路径，类似于 `g++` 编译指令中的 `-I`。
```cmake
# 将 /usr/lib/mylibfolder 和 ./include 添加到工程路径中
include_directories(/usr/lib/mylibfolder ./include)
```
6. `link_directories`：向工程中添加多个特定的库文件搜索路径，类似于 `g++` 编译指令的 `-L` 选项。
```cmake
# 将将 /usr/lib/mylibfolder 和 ./lib 添加到库文件搜索路径中
link_directories(/usr/lib/mylibfolder ./lib)
```
7. `add_library`：生成库文件（包括动态库和静态库）。
```cmake
# 通过 SRC 变量中的文件，生成动态库
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

(1) 内部构建：不推荐使用

内部构建会在主目录下，产生一大堆中间文件，这些中间文件并不是我们最终所需要的，和工程源文件放在一起时，会显得比较杂乱无章。

```cmake
## 内部构建

# 在当前目录下，编译主目录中的CMakeLists.txt 文件生成 Makefile 文件
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
cmake ..	# ..表示上一级目录

# 4、执行make命令，生成可执行程序
make
```

### 1.7.7 CMake 代码实战

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

## 7.1 基于 UDP 的 TFTP 文件传输项目

[TFTP 项目](https://github.com/nuo534202/My-01-University-Notes/tree/main/C%2B%2B%20Advanced/%E5%9F%BA%E4%BA%8E%20UDP%20%E7%9A%84%20TFTP%20%E6%96%87%E4%BB%B6%E4%BC%A0%E8%BE%93%E9%A1%B9%E7%9B%AE)

## 7.2 基于 TCP 的网络聊天室

## 7.3 基于 TCP 的网络电子词典
