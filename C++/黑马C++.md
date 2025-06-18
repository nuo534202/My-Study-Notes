<h1 style="center">黑马 C++ 学习笔记</h1>

课程链接：[黑马 C++](https://www.bilibili.com/video/BV1et411b73Z/?spm_id_from=333.337.search-card.all.click&vd_source=50b6a3c38c5aaa453fe6c02940b9f25f)

# C++ 基础语法

## 指针

### 空指针和野指针

- 空指针：指针变量中指向内存中编号为0的空间
  - 用途：初始化指针变量
  - 注意：空指针指向的内存是不可访问的
- 野指针：指针变量指向非法的内存空间

```cpp
#include <iostream>
using namespace std;

int main() {
    int *p = NULL;
    *p = 100; // wrong，不能访问

    int *pt = (int*)0x1100; // maybe wrong，可能是野指针，指向了非法内存空间
    return 0;
}
```

结论：空指针跟野指针都不是自己申请的的空间，不要随意访问

## const 修饰指针

- 常量指针（const 修饰指针）：指针的**指向可以改**，指针指向的**值不可以改**
    ```cpp
    int a = 10, b = 20;
    const int *p = &a;
    *p = 20; // wrong
    p = &b; // correct
    ```
- 指针常量（const 修饰常量）：指针的**指向不可以改**，指针指向的**值可以改**
    ```cpp
    int a = 10, b = 20;
    int* const p = &a;
    *p = 20; // correct
    p = &b; // wrong
    ```
- const 既修饰指针，又修饰常量：指针的**指向和值都不可以改**
    ```cpp
    int a = 10, b = 20;
    const int* const p = &a;
    *p = 20; // wrong
    p = &b; // wrong
    ```

# C++ 核心编程

## 程序的内存分区模型

C++ 程序在指向时，将内存大方向划分为 **4 个区域**。

- 代码区：存放函数体的二进制代码，由操作系统进行管理
- 全局区：存放**全局变量**和**静态变量**以及**常量**
- 栈区：由编译器自动分配释放，存放函数的参数值、局部变量等
- 堆区：由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收

### 程序运行前

在程序编译后，生成了 exe 可执行程序，未执行该程序前分为两个区域。

- 代码区
  - 存放 CPU 执行的机器指令
  - 代码区时**共享**的，共享的目的是对于频繁被执行的程序，只需要在内存中由一份代码即可
  - 代码区是**只读**的，使其只读的原因是防止程序意外地修改了它的指令
- 全局区
  - 全局变量和静态变量存放在这里
  - 全局区还包含了常量区，字符串常量和其他常量也存放在这里
  - 该区域的数据在程序结束后由操作系统释放

### 程序运行后

- 栈区
  - 由编译器自动分配释放，存放函数的参数值、局部变量等
  - 注意：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放

```cpp
#include <iostream>
using namespace std;

int* func() {
    int a = 10;
    return &a;
}

int main() {
    int *p = func();
    cout << *p << endl; // 第一次能正确输出 10 是因为编译器做了保留
    cout << *p << endl; // 第二次不再保留
    return 0;
}
```

- 堆区
  - 由程序员分配释放，若程序员不释放，程序结束时由操作系统回收
  - 在 C++ 中国主要利用 new 在堆区开辟内存

```cpp
#include <iostream>
using namespace std;

int* func() {
    int* a = new int(10); // 利用 new 关键字，可以将数据开辟到堆区
    return a;
}

int main() {
    int *p = func();
    cout << *p << endl;
    cout << *p << endl;
    // 都能正确输出
    return 0;
}
```

### new 操作符

C++ 中利用 new 操作符在堆区开辟数据。堆区开辟的数据，由程序员手动开辟、手动释放，释放利用操作符 **delete**。

语法：`new 数据类型`

利用 new 创建的数据，会返回该数据对应类型的指针。

```cpp
#include <iostream>
using namespace std;

int* func() {
    int *p = new int(10);
    return p;
}

void test1() {
    int *p = func();
    cout << *p << endl;
    cout << *p << endl; // 堆区数据没有释放
    delete p;
    cout << *p << endl; // 报错，Runtime Error
}

void test2() {
    int *a = new int[10]; // 初始化一个长度为 10 的数组
    for (int i = 0; i < 9; i++) a[i] = i + 1;
    for (int i = 0; i < 9; i++) cout << a[i] << ' '; cout << endl;
    delete[] a; // 释放数组时要加 []
}

int main() {
    test1();
    return 0;
}
```

## 引用

### 引用的基本使用

作用：给变量起别名
语法：`数据类型 &别名 = 原名`

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10;
    int &b = a;
    b = 20;
    cout << a << endl; // 20
    return 0;
}
```

### 引用的注意事项

- 引用必须初始化
- 引用在初始化之后不可以再改变了

```cpp
#include <iostream>
using namespace std;

int main() {
    int a = 10, c = 20;
    int &b = a;
    b = c; // 赋值操作，不是更改引用
    cout << a << endl; // 20
    return 0;
}
```

### 引用作函数参数

作用：函数传参时，可以利用引用的技术让形参修饰实参

有点：可以简化指针修改实参

```cpp
#include <iostream>
using namespace std;

void mySwap(int &x, int &y) {
    int tmp = x;
    x = y;
    y = tmp;
}

int main() {
    int a = 10, b = 20;
    mySwap(a, b);
    cout << a << ' ' << b << ' ' << endl;
    return 0;
}
```

### 引用作函数返回值

作用：引用时可以作为函数的返回值存在的

注意：**不要返回局部变量引用**

用法：函数调用作为左值

```cpp
#include <iostream>
using namespace std;

int& test1() {
    int a = 10; // 存放在栈区
    return a;
}

int& test2() {
    static int a = 10; // 静态变量，存放在全局区
    return a;
}

int main() {

    int &ref1 = test1();
    cout << ref1 << endl; // 结果正确，编译器做了保留
    cout << ref1 << endl; // 乱码

    int &ref2 = test2();
    cout << ref2 << endl;
    cout << ref2 << endl;
    // 两次输出结果都正确
    return 0;
}
```

### 引用的本质

本质：**引用的本质在 C++ 内部的实现是一个指针常量**

### 常量引用

作用：常量引用主要用来修饰形参，防止误操作

在函数形参列表中，可以加 const 修饰形参，防止形参改变实参

```cpp
#include <iostream>
using namespace std;

void print(int &x) {
    cout << x << endl;
    x = 100; // wrong
}

int main() {
    int a = 10;
    return 0;
}
```

## 函数的提高

### 函数默认参数
在 C++ 中，函数的形参列表中的形参是可以有默认值的。

语法：`返回值类型 函数名(参数 = 默认值, ...) {}`

1. 如果某个位置已经有了默认参数，那么从这个位置往后都必须要有默认值（有默认值的参数要写在函数参数列表的最后）
2. 如果函数声明有默认参数，那么函数的实现就不能有默认参数
    ```cpp
    int add(int a = 10, b = 20);
    int add(int a, int b) {
        return a + b;
    }
    ```

### 函数默认参数

C++ 中函数的形参列表可以有占位参数，用来做占位，调用函数时必须填补该位置

语法：`返回值类型 函数名(...,数据类型) {}`

```cpp
#include <iostream>
using namespace std;

int func(int a, int) { // 占位参数
    cout << "This is a function" << endl;
}

int main() {
    func(10, 10);
    return 0;
}
```

### 函数重载

#### 函数重载概述

作用：函数名可以相同，提高复用性

函数重载满足条件：

- 同一个作用域下
- 函数名称相同
- 函数参数 **类型不同** 或者 **个数不同** 或者 **顺序不同**

**注意**：函数的返回值不可以作为函数重载的条件

```cpp
#include <iostream>
using namespace std;

void func() {
    cout << "func1" << endl;
}

void func(int a) {
    cout << "func2" << endl;
}

void func(int a, double b) {
    cout << "func3" << endl;
}

void func(double a, int b) {
    cout << "func4" << endl;
}

void func(double a) {
    cout << "func5" << endl;
}

int main() {
    func();
    func(10);
    func(10, 20.0);
    func(10.0, 20);
    func(1.0);
    return 0;
}
```

输出结果：

```
func1
func2
func3
func4
func5
```

#### 函数重载注意事项

- 引用作为重载条件
    ```cpp
    #include <iostream>
    using namespace std;

    void func(int &a) {
        cout << "func1" << endl;
    }

    void func(const int &a) {
        cout << "func2" << endl;
    }

    int main() {
        int a = 10;
        const int b = 10;
        func(a);
        func(10), func(b);
        return 0;
    }
    ```
- 函数重载碰到函数默认参数
    ```cpp
    #include <iostream>
    using namespace std;

    void func(int a) {
        cout << "func1" << endl;
    }

    void func(int a, int b = 10) {
        cout << "func2" << endl;
    }

    int main() {
        func(10); // wrong
        func(10, 20);
        return 0;
    }
    ```

## 类和对象

C++ 面向对象的三大特性：封装、继承、多态

C++ 认为万事万物都皆为对象，对象上尤其属性和行为

### 封装

#### 封装的意义

- 将属性和行为作为一个整体，表现生活中的事物
- 将属性和行为加以权限控制：public, protected, private
  - public: 成员、类内、类外可以访问
  - protected: 成员、类内、子类可以访问，非子类以外的类不可以访问
  - private: 成员、类内可以访问，类外不可以访问

**语法**：`class 类名 { 访问属性： 属性 / 行为 }`

#### struct 和 class 区别

在 C++ 中 struct 和 class 的唯一区别就是 **默认访问权限的不同**。

- struct 默认权限为 public
- class 默认权限为 private

#### 成员属性设为私有

**优点1**：可以自己控制读写权限

**优点2**：对于写权限，我们可以检测数据的有效性

### 对象的初始化和清理

#### 构造函数和析构函数

如果我们不提供构造函数和析构函数，编译器会提供的构造函数和析构函数是空实现

- 构造函数：主要作用在于创建对象时为对象的成员属性赋值，构造函数由编译器自动调用，无需手动调用
- 析构函数：主要作用在于对象 **销毁前** 系统自动调用，执行一些清理工作

**构造函数语法**： `类名() {}`

1. 构造函数没有返回值也不写 `void`
2. 函数名称与类名相同
3. 构造函数可以有参数，因此可以进行重载
4. 程序在调用对象时候会自动调用构造函数，无需手动调用，而且只会调用一次

**析构函数语法**： `~类名() {}`

1. 析构函数没有返回值也不写 `void`
2. 函数名称与类名相同，要在函数名称前加上符号 `~`
3. 析构函数不可以有参数，因此不可以进行重载
4. 程序在对象销毁前会自动调用析构函数，无需手动调用，而且只会调用一次

```cpp
#include <iostream>
using namespace std;

class Person {
private:
    int m_id;
public:
    Person(int id) {
        this -> m_id = id;
        cout << "Constructor is called" << endl;
    }

    ~Person() {
        cout << "Destructor is called" << endl;
    }
};

int main() {
    Person p1(1);
    return 0;
}
```

**Output:**

```
Constructor is called
Destructor is called
```

#### 构造函数的分类及调用

按参数分为：有参构造和无参构造

按类型分为：普通构造和拷贝构造

三种调用方法：括号法、显示法、隐式转换法

```cpp
#include <iostream>
using namespace std;

class Person {
private:
    int m_id;
public:
    Person() {
        this -> m_id = -1;
        cout << "Constructor without parameters is called" << endl;
    }

    Person(int id) {
        this -> m_id = id;
        cout << "Constructor with parameters is called" << endl;
    }

    Person(const Person &p) {
        this -> m_id = p.m_id;
        cout << "Copy Constructor is called" << endl;
    }

    ~Person() {
        cout << "Destructor is called" << endl;
    }

    int getId() {
        return this -> m_id;
    }
};

int main() {
    Person p1;
    Person p2(2);
    Person p3(p2);
    cout << p3.getId() << endl;

    Person p4(); // 编译器认为是函数的声明，而不是创建对象

    cout << 'a' << endl;
    Person(10); // 匿名对象，当前行执行结束后，系统会立即回收掉匿名对象
    cout << 'a' << endl;

    //Person(p3);
    // 不要利用拷贝构造函数初始化匿名函数，编译器会认为是 Person p3 的对象声明

    Person p5 = 5; // 相当于 Person p5 = Person(5)
    Person p6 = p5; // 相当于拷贝构造
    return 0;
}
```

**Output:**

```
Constructor without parameters is called
Constructor with parameters is called
Copy Constructor is called
2
a
Constructor with parameters is called
Destructor is called
a
Constructor with parameters is called
Copy Constructor is called
Destructor is called
Destructor is called
Destructor is called
Destructor is called
Destructor is called
```

#### 拷贝构造函数的调用时机

- 使用一个已经创建完毕的对象来初始化一个新对象
- 值传递的方式给函数参数传值
- 以值方式返回局部对象

```cpp
#include <iostream>
using namespace std;

class Person {
private:
    int m_id;
public:
    Person() {
        this -> m_id = -1;
        cout << "Constructor without parameters is called" << endl;
    }

    Person(int id) {
        this -> m_id = id;
        cout << "Constructor with parameters is called" << endl;
    }

    Person(const Person &p) {
        this -> m_id = p.m_id;
        cout << "Copy Constructor is called" << endl;
    }

    ~Person() {
        cout << "Destructor is called" << endl;
    }

    int getId() {
        return this -> m_id;
    }
};

void test1() {
    cout << "test1" << endl;
    Person p1(20);
    Person p2(p1);
}

void solve2(Person p) {}

void test2() {
    cout << "test2" << endl;
    Person p;
    solve2(p);
}

Person solve3() {
    Person p;
    return p;
}

void test3() {
    cout << "test3" << endl;
    Person p = solve3(); // 如果编译器启用了 RVO/NRVO，拷贝构造函数的调用会被优化掉
}

int main() {
    test1();
    test2();
    test3();
    return 0;
}
```

**Output:**

```
test1
Constructor with parameters is called
Copy Constructor is called
Destructor is called
Destructor is called
test2
Constructor without parameters is called
Copy Constructor is called
Destructor is called
Destructor is called
test3
Constructor without parameters is called
Copy Constructor is called
Destructor is called
Copy Constructor is called
Destructor is called
Destructor is called
```

#### 构造函数的调用规则

默认情况下，C++ 编译器至少给一个类添加 3 个函数

1. 默认构造函数（无参，函数体为空）
2. 默认析构函数（无参，函数体为空）
3. 默认拷贝构造函数，对属性进行值拷贝

构造函数调用规则：

- 如果用户定义有参构造函数，C++ 不再提供默认无参构造，但是会提供默认拷贝构造函数    
- 如果用户定义拷贝构造函数，C++ 不再提供其他构造函数

#### 深拷贝与浅拷贝

- 深拷贝：在堆区重新申请空间，进行拷贝操作
- 浅拷贝：简单的赋值拷贝操作（编译器提供的拷贝构造函数内容都是浅拷贝操作）

**Sample1:**

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mAge, *mHeight; // 身高数据开到堆区

    Person() {
        cout << "Default Constructor is called" << endl;
    }

    Person(int age, int height) {
        this -> mAge = age;
        this -> mHeight = new int(height);
        cout << "Constructor with parameters is called" << endl;
    }

    ~Person() {
        if (mHeight != nullptr) delete mHeight, mHeight = nullptr; // 释放堆区内存
        cout << "Destructor is called" << endl;
    }
};

void test1() {
    Person p1(18, 180);
    cout << p1.mAge << ' ' << *p1.mHeight << endl;
    Person p2(p1); // 浅拷贝
    cout << p2.mAge << ' ' << *p2.mHeight << endl;
    // 浅拷贝带来的问题：p1.mHeight 跟 p2.mHeight 指向同一块内存，导致堆区的内存重复释放
}

int main() {
    test1();
    return 0;
}
```

**Sample2:**

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mAge, *mHeight; // 身高数据开到堆区

    Person() {
        cout << "Default Constructor is called" << endl;
    }

    Person(int age, int height) {
        this -> mAge = age;
        this -> mHeight = new int(height);
        cout << "Constructor with parameters is called" << endl;
    }

    Person(const Person &p) {
        this -> mAge = p.mAge;
        //this -> mHeight = p -> mHeight; // 编译器默认实现的拷贝函数代码
        this -> mHeight = new int(*p.mHeight); // 深拷贝操作
    }

    ~Person() {
        if (mHeight != nullptr) delete mHeight, mHeight = nullptr; // 释放堆区内存
        cout << "Destructor is called" << endl;
    }
};

void test1() {
    Person p1(18, 180);
    cout << p1.mAge << ' ' << *p1.mHeight << endl;
    Person p2(p1); // 深拷贝，不会导致堆区内存重复释放
    cout << p2.mAge << ' ' << *p2.mHeight << endl;
}

int main() {
    test1();
    return 0;
}
```

**总结：** 如果有属性 (attribute) 是在堆区开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来的问题。

#### 初始化列表

**作用：** initialize attributes

**语法：** `Constructor(): attribute1(value1), attribute2(value2), ... {}`

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mA, mB, mC;
    Person(int a, int b, int c): mA(a), mB(b), mC(c) {}
};

int main() {
    return 0;
}
```

#### 类对象作为类成员

C++ 类中的成员可以是另一个类的对象，我们称该成员是对象成员

```cpp
#include <iostream>
#include <string>
using namespace std;

class Phone {
private:
    string mName;
public:
    Phone(string name): mName(name) {
        cout << "Phone's Constructor is called" << endl;
    }

    ~Phone() {
        cout << "Phone's Destructor is called" << endl;
    }
};

class Person {
private:
    string mName;
    Phone mPhone;
public:
    Person(string name, string phone): mName(name), mPhone(phone) {
        cout << "Person's Constructor is called" << endl;
    } // this -> mPhone = phone 隐式转换法

    ~Person() {
        cout << "Person's Destructor is called" << endl;
    }
};

void test1() {
    Person p1("newbee", "IPhone");
}

int main() {
    test1();
    return 0;
}
```

```
Phone's Constructor is called
Person's Constructor is called
Person's Destructor is called
Phone's Destructor is called
```

**Notice:** 构造时先构造类成员的对象，再构造类对象；析构时先析构类对象，再析构类成员对象

#### 静态成员

静态成员就是再成员变量和成员函数前加上关键字 **static**，称为静态成员

- 静态成员变量
  - 所有对象共享同一份数据
  - 在编译阶段分配内存（内存分配在全局区）
  - 类内声明，类外初始化
- 静态成员函数
  - 所有对象共享同一个函数
  - 静态成员函数只能访问静态成员变量

**Sample1:**

```cpp
#include <iostream>
using namespace std;

class  Person {
private:
    static int mTotal;
public:
    static int mNum;
};

int Person::mTotal = 0;
int Person::mNum = 0;

void test1() {
    Person p;
    cout << p.mNum << endl; // 通过对象访问
    cout << Person::mNum << endl; // 通过类名访问

    Person p1;
    p1.mNum++;
    cout << p.mNum << endl; // 所有对象共享一个数据
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
0
0
1
```

**Sample2:**

```cpp
#include <iostream>
using namespace std;

class  Person {
public:
    static void func() {
        cout << "func is called" << endl;
    }
};

void test1() {
    Person p;
    p.func(); // 通过对象访问
    Person::func(); // 通过类名访问
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
func is called
func is called
```

### C++ 对象模型和 this 指针

#### 成员变量和成员函数分开存储

- 在 C++ 中，类内的成员变量和成员函数分开储存
- 只有非静态成员变量才属于类的对象上

**Sample1:**

```cpp
#include <iostream>
using namespace std;

class Person {
};

void test1() {
    Person p;
    cout << sizeof(p) << endl; // 1
    // 空对象占用内存是 1
    // C++ 编译器会给每个空对象也分配 1 byte 的空间，是为了区分空对象占内存的位置
    // 每个空对象也应该有一个独一无二的内存地址
}

int main() {
    test1();
    return 0;
}
```

**Sample2：**

```cpp
#include <iostream>
using namespace std;

class Person {
    int id;
};

int Person::mNum = 0;

void test1() {
    Person p;
    cout << sizeof(p) << endl; // 4
}

int main() {
    test1();
    return 0;
}
```

**Sample3:**

```cpp
#include <iostream>
using namespace std;

class Person {
    int id;
    static int mNum;
};

int Person::mNum = 0;

void test1() {
    Person p;
    cout << sizeof(p) << endl; // 4
}

int main() {
    test1();
    return 0;
}
```

**Sample4:**

```cpp
#include <iostream>
using namespace std;

class Person {
    int id;
    static int mNum;
    void func() {} // 实际上只有一份，通过 this 区分谁在调用
};

int Person::mNum = 0;

void test1() {
    Person p;
    cout << sizeof(p) << endl; // 4
}

int main() {
    test1();
    return 0;
}
```

**Sample5:**

```cpp
#include <iostream>
using namespace std;

class Person {
    int id;
    static int mNum;
    void func1() {} // 实际上只有一份，通过 this 区分谁在调用
    static void func2() {}
};

int Person::mNum = 0;

void test1() {
    Person p;
    cout << sizeof(p) << endl; // 4
}

int main() {
    test1();
    return 0;
}
```

#### this 指针概念

**this 指针指向被调用的成员函数所属的对象**

- this 指针式隐含在每一个非静态成员函数内的一种指针
- this 指针不需要定义，直接使用即可

this 指针用途：

- 当形参和成员变量同名时，可以用 this 指针来区分
- 当类的非静态成员函数中返回对象本身，可使用 `return *this`

**Sample1:**

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mId;
    Person(int id) {
        this -> mId = id;
    }
};

void test1() {
    Person p(1);
    cout << p.mId << endl;
}

int main() {
    test1();
    return 0;
}
```

**Sample2:**

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mId;
    Person(int id) {
        this -> mId = id;
    }

    Person& add(int x) {
        this -> mId += x;
        return *this;
    }
};

void test1() {
    Person p(1);
    p.add(5).add(10).add(15);
    cout << p.mId << endl; // 31
}

int main() {
    test1();
    return 0;
}
```

**Sample3:**

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mId;
    Person(int id) {
        this -> mId = id;
    }

    Person add(int x) {
        this -> mId += x;
        return *this;
    }
};

void test1() {
    Person p(1);
    p.add(5).add(10).add(15);
    cout << p.mId << endl; // 31
}

int main() {
    test1();
    return 0;
}
```

#### 空指针访问成员函数

要注意有没有用到 this 指针

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mAge;
    void show() {
        cout << "show" << endl;
        if (this == nullptr) return;
        cout << mAge << endl; // this 指针为空时无法访问成员变量
    }
};

void test1() {
    Person *p = nullptr;
    p -> show();
}

int main() {
    test1();
    return 0;
}
```

#### const 修饰成员函数

常函数：

- 成员函数后加 const 我们成这个函数为 **常函数**
- 常函数内不可以修改成员属性
- 成员属性声明时加关键字 `mutable` 后，在常函数中依然可以修改

常对象：

- 声明对象前加 `const` 成该对象为 **常对象**
- 常对象只能调用常函数

**Sample1:**

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mA;

    void show() const {
        // const 修饰的是 this 指针，让 this 指针指向的值不可修改
        this -> mA = 100; // wrong
        this = nullptr; // wrong
        // this 指针的本质是指针常量，指针的指向是不可以修改的
    }
};

int main() {
    return 0;
}
```

**Sample2:**

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mA;
    mutable int mB;

    Person() {}

    void change() const {
        mA = 10; // wrong
        mB = 20; // correct
    }
};

void test1() {
    const Person p;
}

int main() {
    test1();
    return 0;
}
```

### 友元

- 目的：让一个函数或者类访问另一个类中私有成员 (private member)
- 关键字：`friend`
- 三种实现
  - 全局函数作友元
  - 类做友元
  - 成员函数作友元

#### 全局函数做友元

```cpp
#include <iostream>
#include <string>
using namespace std;

class Building {
};

class 

void test1() {
}

int main() {
    test1();
    return 0;
}
```

#### 类做友元

```cpp
#include <iostream>
#include <string>
using namespace std;

class Building; // GoodFriend 类里要用，提前声明

class GoodFriend {
public:
    GoodFriend();
    void visit();
    Building *mBuilding;
};

class Building {
    friend class GoodFriend;
private:
    string mBedroom;
public:
    string mSittingRoom;
    Building();
};

GoodFriend::GoodFriend() {
    mBuilding = new Building();
}

void GoodFriend::visit() {
    cout << "GoodFriend class is called: " << mBuilding -> mSittingRoom << endl;
    cout << "GoodFriend class is called: " << mBuilding -> mBedroom << endl;
}

Building::Building() {
    mSittingRoom = "Sitting Room";
    mBedroom = "Bedroom";
} // 类外实现成员函数

void test1() {
    GoodFriend gf;
    gf.visit();
}

int main() {
    test1();
    return 0;
}
```

```
GoodFriend class is called: Sitting Room
GoodFriend class is called: Bedroom
```

#### 成员函数做友元

```cpp
#include <iostream>
using namespace std;

class Building;
class GoodFriend {
public:
    GoodFriend();
    void visit1();
    void visit2();
    Building *mBuilding;
};

class Building {
    friend void GoodFriend::visit1();
private:
    string mBedroom;
public:
    string mSittingRoom;
    Building();
};

Building::Building() {
    mSittingRoom = "Sitting Room";
    mBedroom = "Bedroom";
}

GoodFriend::GoodFriend() {
    mBuilding = new Building;
}

void GoodFriend::visit1() {
    cout << mBuilding -> mBedroom << endl; // wrong
}

void GoodFriend::visit2() {
    cout << mBuilding -> mBedroom << endl; // correct
}

void test1() {
    GoodFriend gf;
    gf.visit1();
}

int main() {
    test1();
    return 0;
}
```

### 运算符重载

概念：对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

#### 加号运算符重载

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mA, mB;
    Person(int a, int b): mA(a), mB(b) {}

    // Person operator+ (const Person &p) const {
    //     Person tmp(0, 0);
    //     tmp.mA = this -> mA + p.mA;
    //     tmp.mB = this -> mB + p.mB;
    //     return tmp;
    // } // 成员函数重载 + 号，本质：p1.operator+(p2)

    void showInfo() {
        cout << "mA: " << mA << ", mB: " << mB << endl;
    }
};

Person operator+(const Person &p1, const Person &p2) {
    Person tmp(0, 0);
    tmp.mA = p1.mA + p2.mA;
    tmp.mB = p1.mB + p2.mB;
    return tmp;
} // 全局函数重载 + 号，本质：operator+(p1, p2)

Person operator+(const Person &p, int num) {
    Person tmp(0, 0);
    tmp.mA = p.mA + num;
    tmp.mB = p.mB + num;
    return tmp;
}

void test1() {
    Person p1(10, 20), p2(40, 50);
    p1 = p1 + p2;
    p1.showInfo();
    p1 = p1 + 10;
    p1.showInfo();
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
mA: 50, mB: 70
mA: 60, mB: 80
```

**总结**：对于内置的数据类型的表达式的运算符不能发生改变

#### 左移运算符重载

作用：可以输出自定义数据类型

**Notice:** 一般不会利用成员函数重载 `<<` 运算符，因为无法实现 `cout` 在左侧；一般利用全局函数重载左移运算符

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int mA, mB;
};

ostream& operator<<(ostream &cout, const Person &p) {
    // cout 必须要用引用，因为只能有一个 cout 对象，不能创造多个 cout 对象
    cout << p.mA << " " << p.mB;
    return cout;
}

void test1() {
    Person p;
    p.mA = 10, p.mB = 10;
    cout << p << endl;
}

int main() {
    test1();
    return 0;
}
```

#### 递增/减运算符重载

**1. 重载递减运算符**

```cpp
#include <iostream>
using namespace std;

class MyInteger {
    friend ostream& operator<<(ostream& cout, const MyInteger& myInt);
private:
    int mNum;
public:
    MyInteger(int num): mNum(num) {}

    // 返回引用是为了能正确多次执行递增
    MyInteger& operator++() {
        mNum++;
        return *this;
    } // 重载前置递增运算符

    MyInteger operator++(int) {
        // int 是占位参数，用于区分前置和后置递增
        MyInteger oup = *this;
        mNum++;
        return oup;
    } // 重载后置递增运算符
};

ostream& operator<<(ostream& cout, const MyInteger& myInt) {
    cout << myInt.mNum;
    return cout;
}

void test1() {
    MyInteger myInt(10);
    cout << myInt++ << endl;
    cout << myInt << endl;
    cout << ++myInt << endl;
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
10
11
12
```

**2. 重载递减运算符**

```cpp
#include <iostream>
using namespace std;

class MyInteger {
    friend ostream& operator<<(ostream& cout, const MyInteger& myInt);
private:
    int mNum;
public:
    MyInteger(int num): mNum(num) {}

    MyInteger& operator--() {
        this -> mNum--;
        return *this;
    }

    MyInteger operator--(int) {
        MyInteger oup = *this;
        this -> mNum--;
        return oup;
    }

};

ostream& operator<<(ostream& cout, const MyInteger& myInt) {
    cout << myInt.mNum;
    return cout;
}

void test1() {
    MyInteger myInt(10);
    cout << myInt-- << endl;
    cout << myInt << endl;
    cout << --myInt << endl;
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
10
9
8
```

#### 赋值运算符重载

C++ 编译器至少给一个类添加 4 个函数

1. 默认构造函数（无参，函数体为空）
2. 默认析构函数（无参，函数体为空）
3. 默认拷贝构造函数，对属性进行值拷贝
4. 赋值运算符 `operator=`，对属性进行**值拷贝**

如果类中有属性指向堆区，做赋值操作时也会出现深浅拷贝问题

```cpp
#include <iostream>
using namespace std;

class Person {
public:
    int *mAge;
    Person(int age) {
        this -> mAge = new int(age);
    }
    ~Person() {
        if (mAge != nullptr) {
            delete mAge;
            mAge = nullptr;
        }
    }
    Person& operator=(const Person &p) {
        if (mAge != nullptr) {
            delete mAge;
            mAge = nullptr;
        } // 释放原来的内存
        mAge = new int(*p.mAge);
        return *this;
    }
};

void test1() {
    Person p1(18), p2(20);
    cout << *p1.mAge << ' ' << *p2.mAge << endl;
    p2 = p1;
    cout << *p2.mAge << endl;
}

int main() {
    test1();
    return 0;
}
```

#### 关系运算符重载

**作用：** 重载关系运算符可以让两个自定义类型的对象进行比较

```cpp
#include <iostream>
#include <string>
using namespace std;

class Person {
public:
    string mName;
    int mAge;
    Person(string name, int age): mName(name), mAge(age) {}
    bool operator==(const Person &p) const {
        return mAge == p.mAge && mName == p.mName;
    }
    bool operator!=(const Person &p) const {
        return mAge != p.mAge || mName != p.mName;
    }
};

void test1() {
    Person p1("Tom", 18), p2("Tom", 18);
    cout << (p1 == p2) << endl;
    cout << (p1 != p2) << endl;
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
1
0
```

#### 函数调用运算符重载

- 函数调用运算符 `()` 也可以重载
- 由于重载后使用的方式非常想函数的调用，因此称为仿函数
- 仿函数没有固定写法，非常灵活

```cpp
#include <iostream>
#include <string>
using namespace std;

class MyAdd {
public:
    int operator()(int num1, int num2) {
        return num1 + num2;
    }
};

class MyPrint {
public:
    void operator()(string str) {
        cout << str << endl;
    }
};

void test1() {
    MyPrint myPrint;
    myPrint("Hello World");
    MyAdd myAdd;
    cout << myAdd(10, 20) << endl;
    cout << MyAdd()(10, 20) << endl; // 匿名对象
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
Hello World
30
30
```

### 继承

继承是面向对象的三大特性质疑

#### 继承的基本语法

语法：`class 子类 : 继承方式 父类`

```cpp
#include <iostream>
using namespace std;

class BasePage {
public:
    // code
};

class Java : public BasePage {
public:
    void content() {
        cout << "Java" << endl;
    }
};

class Python : public BasePage {
public:
    void content() {
        cout << "Python" << endl;
    }
};

int main() {
    return 0;
}
```

#### 继承方式

- `public`：能访问父类中 `public` 跟 `protected` 权限的成员，父类中 `private` 权限的成员无法访问
- `protected`：父类中 `private` 权限的成员无法访问，剩余成员的权限都变为 `protected`
- `private`：父类中 `private` 权限的成员无法访问，剩余成员的权限都变为 `private`

#### 继承中的对象模型

Q：从父类继承过来的成员，哪些属于子类对象中？

A：父类中所有非静态成员属性都会被子类继承下去，父类私有成员属性是被编译器隐藏了访问不到，但是也继承到父类了。

VS命令行中检查类的布局

```
c1 /d1 reportSingleClassLayout类名 fileName.cpp
```

#### 继承中的构造和析构顺序

1. 先构造父类，再构造子类
2. 先析构子类， 再析构父类

#### 继承同名成员处理方式

- 访问子类同名成员：直接访问
- 访问父类同名成员：添加作用域
- 如果子类中出现和父类同名的成员函数，子类的同名成员会隐藏掉父类中所有同名的成员函数；如果想要访问到父类中被隐藏的同名成员函数需要添加作用域

**Sample:**

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    int mA;
    Base(): mA(100) {}
    void func() {
        cout << "Base" << endl;
    }
    void func(int a) {
        cout << a << endl;
    }
};

class Son : public Base {
public:
    int mA;
    Son(): mA(200) {}
    void func() {
        cout << "Son" << endl;
    }
    void func(int a) {
        cout << a + 1 << endl;
    }
};


void test1() {
    Son s;
    cout << s.mA << " " << s.Base::mA << endl;
    s.func(), s.Base::func();
    s.func(10), s.Base::func(10);
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
200 100
Son
Base
11
10
```

#### 继承同名静态成员的处理方式

静态成员出现同名，和非静态成员的处理方式一致

**Sample:**

```
#include <iostream>
using namespace std;

class Base {
public:
    static int mA;
    static void func() {
        cout << "Base" << endl;
    }
};

class Son : public Base {
public:
    static int mA;
    static void func() {
        cout << "Son" << endl;
    }
};

int Base::mA = 100;
int Son::mA = 200;

void test1() {
    Son s;
    cout << s.mA << " " << Son::mA << endl;
    cout << s.Base::mA << " " << Son::Base::mA << endl;
    s.func(), Son::func();
    s.Base::func(), Son::Base::func();
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
200 200
100 100
Son
Son
Base
Base
```

#### 多继承语法

1. C++ 允许 **一个类继承多个类**
2. 语法：`class 子类 : 继承方式 父类 1, 继承方式 父类 2, ···`
3. 多继承可能会阴风父类中有同名成员出现，需要加作用域区分
4. **C++ 实际开发中不建议用多继承**

**Sample:**

```cpp
#include <iostream>
using namespace std;

class Base1 {
public:
    int mA;
    Base1() : mA(100) {}
    void func() {
        cout << "Base1" << endl;
    }
};

class Base2 {
public:
    int mA;
    Base2() : mA(200) {}    
};

class Son : public Base1, public Base2 {
public:
    int mC, mD;
    Son() : mC(300), mD(400) {}
};

void test1() {
    Son s;
    cout << sizeof(s) << endl;
    cout << s.Base1::mA << " " << s.Base2::mA << endl;
}

int main() {
    test1();
    return 0;
}
```

#### 菱形继承（钻石继承）

概念：两个派生类继承同一个基类，又有某个类同时继承两个派生类

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    int mTotalAnimal;
};
// 利用虚继承可以解决菱形继承带来的重复数据的问题
// 继承前加上关键字 virtual 变为虚继承
// Animal 类称为虚基类
class Sheep : virtual public Animal {
};

class Pig : virtual public Animal {
};

class Farm : public Sheep, public Pig {
};

void test1() {
    Farm fm;
    fm.Sheep::mTotalAnimal = 18, fm.Pig::mTotalAnimal = 28; // 两个父类有相同成员属性时要加作用域区分
    // 这份数据我们知道，只要有一份即可，菱形继承导致数据有两份，资源浪费
    cout << fm.Sheep::mTotalAnimal << " " << fm.Pig::mTotalAnimal << " " << fm.mTotalAnimal << endl;
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
28 28 28
```

`vbptr` 是虚基类指针，指向 `vbtable` 即虚基类表格。

### 多态

#### 多态的基本概念

- 静态多态：**函数重载** 和 **运算符重载** 属于静态多态，复用函数名
- 动态多态：**派生类** 和 **虚函数** 实现运行时多态

区别：

- 静态多态的函数地址早绑定 - 编译阶段确定函数地址
- 动态多态的函数地址晚绑定 - 运行阶段确定函数地址

**Sample:**

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void speak() {
        cout << "Animal is speaking" << endl;
    } // 虚函数，地址晚绑定
};

class Cat : public Animal {
public:
    void speak() {
        cout << "Cat is speaking" << endl;
    }
};

class Dog : public Animal {
public:
    void speak() {
        cout << "Dog is speaking" << endl;
    }
};

void doSpeakAction(Animal &animal) {
    animal.speak();
}
// 如果 Animal 类里的 speak() 函数没有 virtual 关键字，那么函数地址早绑定，在编译阶段确定函数地址
// 即使传入的对象是 Animal 的子类对象，调用的也是 Animal 类的 speak() 函数
// 如果想执行让猫说话，那么这个函数地址就不能提前绑定，需要在运行阶段进行绑定（地址晚绑定）

void test1() {
    Cat cat;
    doSpeakAction(cat);
    Dog dog;
    doSpeakAction(dog);
    cout << sizeof(Animal) << endl;
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
Cat is speaking
Dog is speaking
8
```

如果是 32 位操作系统最后一行输出的是 4，即当前操作系统下指针的大小。

动态多态条件：

1. 有继承关系
2. 重写 (Override: same function name and parameter list but different impletation) 父类虚函数

动态多态使用：

1. 父类的指针或者引用指向子类对象

#### 多态的原理剖析

| Class | Internal Structure |
| :---: | :----------------: |
| Animal | vfptr (virtual function pointer) 虚函数指针，指向 vftable (virtual function table) 虚函数表，表内会记录虚函数的地址 `&Animal::speak`。|
| Cat | 当子类重写父类的虚函数，子类中的虚函数表内部会替换成子类的虚函数地址 `&Cat::speak`。 |

当父类的指针或引用指向子类对象时，发生多态。

```cpp
Animal &animal = cat; // 指向 cat 对象
animal.speak(); // 调用 cat 的 speak 函数
```

#### 纯虚函数和抽象类

在多态中，通常父类中虚函数的实现是毫无意义的，主要都是调用子类重写的内容，因此可以将虚函数改为 **纯虚函数**。

**纯虚函数语法**： `virtual <return type> functionName(para1, para2, ···) = 0`

当类中有了纯虚函数，这个类也称为 **抽象类**，无法实例化对象。抽象类的子类必须要重写父类中的纯虚函数，否则也属于抽象类。

**Sample:**

```
#include <iostream>
using namespace std;

class Base {
public:
    virtual void func() = 0; // 纯虚函数
};

class Son : public Base {
public:
    void func() {
        cout << "func" << endl;
    }
};

void test1() {
    Base b; // wrong
    Base *base = new Son;
    base->func();
    Son s;
    s.func();
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
func
func
```

#### 虚析构和纯虚析构

多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用子类的析构代码。

解决方式：将父类中的析构函数改为虚析构或者纯虚析构。

虚析构和纯虚析构的共性：

- 可以解决父类指针释放子类对象
- 都要有具体的函数实现

虚析构和纯虚析构的区别：

- 如果是纯虚析构，该类属于抽象类，无法实例化对象。

虚析构语法： `virtual ~className() {}`

纯虚析构语法： `virtual ~className() = 0`

**Sample:**

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void speak() = 0;
    Animal() {
        cout << "Animal's constructor" << endl;
    }
    // virtual ~Animal() {
    //     cout << "Animal's destructor" << endl;
    // } // 利用虚析构解决父类指针释放对象时不干净的问题
    virtual ~Animal() = 0; // 纯虚析构
    // 有了纯虚析构之后，这个类也属于抽象类，无法实例化对象
};

Animal::~Animal() {
    cout << "Animal's destructor" << endl;
    // 纯虚析构既要声明也要实现
}

class Cat : public Animal {
public:
    string *mName;
    Cat(string name) {
        mName = new string(name);
        cout << "Cat's constructor" << endl;
    }
    virtual void speak() {
        cout << *mName << " is speaking" << endl;
    }
    ~Cat() {
        if (mName != nullptr) {
            delete mName, mName = nullptr;
            cout << "Cat's destructor" << endl;
        }
    }
};

void test1() {
    Animal *animal = new Cat("Tom");
    animal->speak();
    delete animal;
}

int main() {
    test1();
    return 0;
}
```

**Output:**

```
Animal's constructor
Cat's constructor
Tom is speaking
Cat's destructor
Animal's destructor
```

## 文件操作

程序运行时产生的数据都属于临时数据，程序一旦运行结束都会被释放，通过文件可以将数据持久化。

C++ 中对文件操作需要包含头文件 `<fstream>`。

文件类型：

1. 文本文件：文件以文本的 **ASCII码** 形式储存在计算机中。
2. 二进制文件：文件以文本的 **二进制** 形式储存在计算机中，用户一半不能直接读懂他们。

三大文件操作：

1. ofstream: 写操作
2. ifstream: 读操作
3. fstream: 读写操作

### 文本文件

#### 写文件

步骤：

1. 包含头文件： `#include <fstream>`
2. 创建流对象： `ofstream ofs;`
3. 打开文件： `ofs.open(const char *_Filename, std::ios_base::open_mode Mode)`
4. 写数据 ： `ofs << "data";`
5. 关闭文件： `ofs.close();`

| Open Mode | Explanation |
| :-------: | :---------: |
| `ios::in` | read the file |
| `ios::out` | write the file |
| `ios::ate` | initial location: end of file |
| `ios::app` | 追加方式写文件 |
| `ios::trunc` | 如果文件存在，先删除再创建 |
| `ios::binary` | 二进制方式 |

文件打开方式可以利用 `|` 操作符配合使用

例如，用二进制方式写文件： `ios::binary | ios::out`

**Sample:**

```cpp
#include <fstream>
using namespace std;

void test1() {
    ofstream ofs;
    ofs.open("test.txt", ios::out);
    ofs << "Hello World" << endl;
    ofs.close();
}

int main() {
    test1();
    return 0;
}
```

#### 读文件

步骤：

1. 包含头文件： `#include <fstream>`
2. 创建流对象： `ifstream ifs;`
3. 打开文件： `ifs.open(const char *_Filename, std::ios_base::open_mode Mode)`
4. 读数据 ： 四种方式读取。
5. 关闭文件： `ifs.close();`

**Sample:**

```
#include <iostream>
#include <string>
#include <fstream>
using namespace std;

void test1() {
    ifstream ifs;
    ifs.open("test.txt", ios::in);
    if (!ifs.is_open()) {
        cout << "fail to open" << endl;
    }
    char buf1[1024];
    while (ifs >> buf1) {
        cout << buf1 << endl;
    }
    while (ifs.getline(buf1, sizeof(buf1))) {
        cout << buf1 << endl;
    }
    string buf2;
    while (getline(ifs, buf2)) {
        cout << buf2 << endl;
    }
    char c;
    while ((c = ifs.get()) != EOF) {
        cout << c;
    }
}

int main() {
    test1();
    return 0;
}
```

### 二进制文件

#### 写文件

二进制写文件主要利用流对象调用成员函数 `write`

函数原型： `ostream& write(const char* buffer, int len);`

**Sample:**

```cpp
#include <fstream>
using namespace std;

class Person {
public:
    char mName[64]; // 最好用字符数组，不要用 string
    int mAge;
};

void test1() {
    ofstream ofs;
    ofs.open("test.txt", ios::binary | ios::out);
    Person p = { "Tom", 18 };
    ofs.write((const char*)&p, sizeof(Person));
    ofs.close();
}

int main() {
    test1();
    return 0;
}
```

#### 读文件

二进制写文件主要利用流对象调用成员函数 `read`

函数原型： `istream& read(char* buffer, int len);`

**Sample:**

```
#include <iostream>
#include <fstream>
using namespace std;

class Person {
public:
    char mName[64]; // 最好用字符数组，不要用 string
    int mAge;
};

void test1() {
    ifstream ifs;
    ifs.open("test.txt", ios::binary | ios::in);
    if (!ifs.is_open()) {
        cout << "fail to open file" << endl;
        return;
    }
    Person p;
    ifs.read((char*)&p, sizeof(Person));
    cout << p.mName << " " << p.mAge << endl;
    ifs.close();
}

int main() {
    test1();
    return 0;
}
```

# C++ 提高编程

## 模板

### 模板的概念

模板就是建立 **通用的模具**，大大 **提高复用性**。

### 函数模板

- C++ 另一种编程思想是泛型编程，主要利用的技术是模板。
- C++ 提供两种模板机制：**函数模板** 和 **类模板**。

#### 函数模板语法

作用：建立一个通用函数，其函数返回值类型和形参类型可以不具体指定，用一个 **虚拟的类型** 来代表

**语法：**

```cpp
template<typename T> // 函数声明或定义
```

解释：
1. template --- 声明创建模板
2. typename --- 表明其后面的符号是一种数据类型，可以用 class 代替
3. T --- 通用的数据类型，名称可以替换，通常为大写字母

**Sample**

```cpp
#include <iostream>
using namespace std;

template<typename T>

void mySwap(T &a, T &b) {
    T tmp = a;
    a = b;
    b = tmp;
} 

int main() {
    int a = 10, b = 20;
    mySwap(a, b);
    mySwap<int>(a, b);
    cout << a << " " << b << endl;
    return 0;   
}
```

使用方法

1. 自动类型推导 `mySwap(a, b)`
2. 显式指定类型 `mySwap<int>(a, b)`

#### 函数模板注意事项

- 自动类型推导：必须推导除一致的数据类型 `T` 才可以使用。
- 显式指定类型：模板必须要确定出 `T` 的一致数据类型才可以使用。

#### 普通函数与函数模板的区别

- 普通函数式可以发用自动类型转换（隐式类型转换）。
- 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换。
- 如果利用显式指定类型，可以发生隐式类型转换。

#### 普通函数与函数模板的调用规则

1. 如果函数模板和普通函数都可以实现，优先调用普通函数。
2. 可以通过空模板参数列表来强制调用函数模板。
3. 函数模板也可以发生重载。
4. 如果函数模板可以产生更好的匹配（如不用发生隐式类型转换），优先调用函数模板。

**Sample:**

```cpp
#include <iostream>
using namespace std;

template<typename T>
void myPrint(T a, T b) {
    cout << "template 1" << endl;
}

template<typename T>
void myPrint(T a, T b, T c) {
    cout << "template 2" << endl;
}

void myPrint(int a, int b) {
    cout << "int" << endl;
}

int main() {
    myPrint(10, 20);
    myPrint<>(10, 20);
    myPrint(10, 20, 30);
    myPrint('a', 'c');
    return 0;
}
```

**Output:**

```
int
template 1
template 2
template 1
```

#### 模板的局限性

- 模板的通用性不是万能的

1. 下面的 `f` 函数种，如果传入的 `a` 跟 `b` 都是一个数组就无法实现了
```cpp
template<typename T>
void f(T a, T b) {
    a = b;
}
```
2. 下面的 `f` 传入的如果是没有重载相关运算符的自定义数据类型，无法正常运行
```cpp
<template T>
bool f(T a, T b) {
    return a > b;
}
```
3. 利用具体化的模板实现
```cpp
#include <iostream>
using namespace std;

class Person {
public:
    string mName;
    int mAge;
};

template<typename T>
bool myCompare(T a, T b) {
    return a == b;
}

template<> bool myCompare(Person &p1, Person &p2) {
    if (p1.mName == p2.mName && p1.mAge == p2.mAge) return true;
    return false;
}

int main() {
    return 0;
}
```

### 类模板

#### 类模板语法

作用：建立一个通用类，类中成员数据类型可以不具体指定，用一个 **虚拟的类型** 来代表。

语法：

```cpp
template<typename T>
class
```

**Sample：**

```cpp  
#include <string>
using namespace std;

template<typename nameType, typename ageType>
class Person {
public:
    nameType mName;
    ageType mAge;
    Person(nameType name, ageType age) : mName(name), mAge(age) {}
};

int main() {
    Person<string, int> p("Tom", 10);
    return 0;
}
```

#### 类模板与函数模板区别

1. 类模板没有自动类型推导的使用方式
2. 类模板再模板参数列表种可以有默认参数

```cpp
#include <iostream>
#include <string>
using namespace std;

template<typename nameType = string, typename ageType = int>
class Person {
public:
    nameType mName;
    ageType mAge;
    Person(nameType name, ageType age) : mName(name), mAge(age) {}
};

int main() {
    Person<string, int> p1("Tom", 10);
    Person<> p2("Steven", 20);
    return 0;
}
```

#### 类模板中成员函数创建时机

- 普通类中的成员函数一开始就可以创建
- 类模板中的成员函数在调用时才创建


**Sample：**

```cpp
#include <iostream>
#include <string>
using namespace std;

class Person1 {
public:
    void showPerson1() { cout << "Person1" << endl; }
};

class Person2 {
public:
    void showPerson2() { cout << "Person2" << endl; }
};

template<typename T>
class myClass {
public:
    T obj;
    void func1() { obj.showPerson1(); }
    void func2() { obj.showPerson2(); }
};

int main() {
    myClass<Person1> m1;
    myClass<Person2> m2;
    m1.func1(), m2.func2();
    return 0;
}
```

**Output：**

```
Person1
Person2
```

#### 类模板对象做函数参数

1. 指定传入类型 --- 直接显示对象的数据类型
2. 参数模板化   --- 将对象中的参数变为模板进行传递
3. 整个类模板化 --- 将这个对象类型模板化进行传递

**Sample：**

```cpp
#include <iostream>
#include <string>
using namespace std;

template<typename T1, typename T2>
class Person {
public:
    T1 mName;
    T2 mAge;
    Person(T1 name, T2 age) : mName(name), mAge(age) {}
    void showPerson() {
        cout << mName << " " << mAge << endl;
    }
};

void printPerson1(Person<string, int>& p) {
    p.showPerson();
}

template<typename T1, typename T2>
void printPerson2(Person<T1, T2>& p) {
    p.showPerson();
    cout << typeid(T1).name() << endl;
    cout << typeid(T2).name() << endl;
}

template<typename T>
void printPerson3(T& p) {
    p.showPerson();
    cout << typeid(T).name() << endl;
}

int main() {
    Person<string, int> p1("Tom", 18);
    printPerson1(p1);
    printPerson2(p1);
    printPerson3(p1);
    return 0;
}
```

**Output：**

```
Tom 18
Tom 18
class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> >
int
Tom 18
class Person<class std::basic_string<char,struct std::char_traits<char>,class std::allocator<char> >,int>
```

#### 类模板与继承

- 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中 `T` 的类型
- 如果不指定，编译器无法给子类分配内存
- 如果想灵活指定出父类中 `T` 的类型，子类也需要变为类模板

**Sample：**

```cpp
#include <iostream>
#include <string>
using namespace std;

template<typename T>
class Base {
public:
    T m;
};

class Son : public Base<int> {
public:
};

template<typename T1, typename T2>
class Son2 : public Base<T1> {
public:
    T2 obj;
};

int main() {
    Son2<int, char> s2; // int 传给 T1，char 传给 T2
    return 0;
}
```

#### 类模板成员函数类外实现

```cpp
#include <iostream>
#include <string>
using namespace std;

template<typename T1, typename T2>
class Person {
public:
    T1 mName;
    T2 mAge;
    Person(T1 name, T2 age) {}
    void showPerson() {}
};

template<typename T1, typename T2>
Person<T1, T2>::Person(T1 name, T2 age) {
    this->mName = name
    this->mAge = age;
}

template<typename T1, typename T2>
void Person<T1, T2>::showPerson() {
    cout << mName << " " << mAge << endl;
} 

int main() {
    return 0;
}
```

#### 类模板份文件编写

问题：类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到

解决：

1. 直接包含 .cpp 源文件。   
2. 将声明和实现写道同一个文件中，并改后缀名为 .hpp，hpp 时约定的名称，并不是强制的要求。

**Sample1：**

**main.cpp**

```cpp
#include <string>
#include "person.h" // 只包含 person.h，不包含 person.cpp 会发生错误
#include "person.cpp"
using namespace std;

int main() {
	Person<string, int> p1("Tom", 18);
	p1.showPerson();
	return 0;
}
```

**错误原因：** 类模板的成员函数在调用时才创建，只调用 `person.h` 而不调用 `person.cpp` 不会创建 `Person` 和 `showPerson` 两个函数，导致在最后链接阶段无法链接到两个对应函数；如果只包含 `person.cpp`，由于 `person.cpp` 中 inclde 了 `person.h`，所以能正常运行。 

**person.h**

```cpp
#pragma once
#include <iostream>
#include <string>
using namespace std;

template<typename T1, typename T2>
class Person {
public:
	T1 mName;
	T2 mAge;
	Person(T1 name, T2 age);
	void showPerson();
};
```

**person.cpp**

```cpp
#include "person.h"

template<typename T1, typename T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->mName = name;
	this->mAge = age;
}

template<typename T1, typename T2>
void Person<T1, T2>::showPerson() {
	cout << mName << " " << mAge << endl;
}
```

**Sample2（较常用）：**

**main.cpp**

```cpp
#include <string>
#include "person.hpp"
using namespace std;

int main() {
	Person<string, int> p1("Tom", 18);
	p1.showPerson();
	return 0;
}
```

**person.hpp**

```cpp
#include <iostream>
#include <string>
using namespace std;

template<typename T1, typename T2>
class Person {
public:
	T1 mName;
	T2 mAge;
	Person(T1 name, T2 age);
	void showPerson();
};

template<typename T1, typename T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->mName = name;
	this->mAge = age;
}

template<typename T1, typename T2>
void Person<T1, T2>::showPerson() {
	cout << mName << " " << mAge << endl;
}
```

**Note：** 后缀 `.hpp` 只是约定俗成的一个后缀，并不是强制的要求。

#### 类模板与友元

- 全局函数类内实现：直接在类内声明友元。
- 全局函数类外实现：需要提前让编译器直到全局函数的存在。

**Sample1：全局函数类内实现**

```cpp
#include <iostream>
#include <string>
using namespace std;

template<typename T1, typename T2>
class Person {
	T1 mName;
	T2 mAge;
	friend void printPerson(Person<T1, T2>& p) {
		cout << p.mName << " " << p.mAge << endl;
	}
public:
	Person(T1 name, T2 age) : mName(name), mAge(age) {}
};

int main() {
	Person<string, int> p("Tom", 18);
	printPerson(p);
	return 0;
}
```

**Sample2 (wrong)：声明与实现无法对应**

```cpp
#include <iostream>
#include <string>
using namespace std;

template<typename T1, typename T2>
class Person {
	T1 mName;
	T2 mAge;
	friend void printPerson(Person<T1, T2>& p); // 普通函数的声明
public:
	Person(T1 name, T2 age) : mName(name), mAge(age) {}
};

template<typename T1, typename T2>
void printPerson(Person<T1, T2>& p) {
	cout << p.mName << " " << p.mAge << endl;
} // 函数模板的函数实现

int main() {
	Person<string, int> p("Tom", 18);
	printPerson(p);
	return 0;
}
```

**Sample3：**

```cpp
#include <iostream>
#include <string>
using namespace std;

template<typename T1, typename T2>
class Person;

template<typename T1, typename T2>
void printPerson(Person<T1, T2>& p) {
	cout << p.mName << " " << p.mAge << endl;
} // 函数模板的函数实现

template<typename T1, typename T2>
class Person {
	T1 mName;
	T2 mAge;
	friend void printPerson<>(Person<T1, T2>& p); // 函数模板的声明
	// 需要让编译器提前知道这个函数存在
public:
	Person(T1 name, T2 age) : mName(name), mAge(age) {}
};

int main() {
	Person<string, int> p("Tom", 18);
	printPerson(p);
	return 0;
}
```

<!-- nxt: 11模板 --> 
