[TOC]

# QT C++ 培训

## **Day1 环境安装和入门（2025.03.05）**

### **Qt 自带的编译器**
Qt 使用 MinGW 作为其默认的编译器。MinGW 是基于 GCC 的编译器，而在 Java 和 Python 中，分别可以使用 Maven 和 `pip install` 等工具安装依赖或第三方库。

C++ 由标准委员会指定标准，不同厂商会实现自己的编译器和标准库。Qt 致力于跨平台开发，封装了一套跨平台的库，但并不是“一次编译，到处运行”。

C++ 参考手册指出，从 C++11 开始，C++ 进入了一个较快的发展阶段，每三年就会有一个新版本发布。

### **Qt 的编译脚本：qmake / CMake**
`qmake` 是 Qt 官方维护的成熟构建工具，但从 Qt 5.15 版本开始，官方推荐使用 `CMake` 作为默认的构建工具。

#### **示例：Test.pro 文件**
```ini
TEMPLATE = app
CONFIG += console c++11
CONFIG -= app_bundle
CONFIG -= qt

SOURCES += \
        main.cpp
```

### **Qt 的版本控制系统**
推荐使用 **Git** 进行版本控制。在实际开发中，每个开发人员通常有各自的分工，合理的 Git 流程可以减少代码冲突，提高协作效率。

### **C++ 中的头文件**
- 头文件主要用于**声明**（declaration）。
- 源文件（.cpp）主要用于**定义和实现**（definition & implementation）。

### **C++ 中的命名空间**
```cpp
using namespace std;
```

这行代码会将整个 `std` 标准库暴露在源代码中。一般推荐使用具体引用，例如：
```cpp
using std::cout;
using std::cin;
```
这样可以避免命名冲突，提高代码的可读性。

### **C++ 中的编译、链接、运行**
在 C++ 开发过程中，程序的构建主要包括以下几个阶段：

1. **构建（Build）**
   - **预处理**：处理 `#include` 头文件，展开宏定义。
   - **编译**：将每个 `.cpp` 或 `.c` 文件编译成 `.o`（目标文件），这些目标文件是二进制格式的编译单元。
   - **链接**：将所有 `.o` 目标文件链接在一起，生成 `.exe` 可执行文件。

2. **运行（Run）**
   - 运行编译好的可执行文件，执行程序逻辑。


## **Day2 C++语法和工程实践（2025.03.06）**
---
### **C++中的内置类型**

C++ 提供了一系列内置数据类型，用于存储不同类型的值。常见的内置类型包括：

#### **1. 基本数据类型**
- **整数类型**：
  - `int`：用于存储整数，大小通常为 4 字节（具体大小依赖于系统）。
  - `short`：较小的整数类型，通常为 2 字节。
  - `long`：较大的整数类型，通常为 4 或 8 字节。
  - `long long`：更大的整数类型，通常为 8 字节。
- **字符类型**：
  - `char`：用于存储字符，通常为 1 字节。
  - `wchar_t`：用于存储宽字符（Unicode 字符），通常为 2 或 4 字节。
- **浮点类型**：
  - `float`：用于存储单精度浮点数，通常为 4 字节。
  - `double`：用于存储双精度浮点数，通常为 8 字节。
  - `long double`：用于存储更高精度的浮点数，大小依赖于系统，通常为 10 或 12 字节。

#### **2. 布尔类型**
- **`bool`**：用于存储布尔值，取值为 `true` 或 `false`，通常为 1 字节。

#### **3. 空类型**
- **`void`**：表示没有类型，常用于函数返回类型或指针声明。

#### 4. **类型限定符**
- **`const`**：常量类型，表示变量的值不可修改。
- **`volatile`**：表示变量可能会被外部因素修改，告诉编译器避免优化。
- **`mutable`**：允许即使在 `const` 成员函数中也可以修改类的某些成员变量。

```cpp
//C++内置类型
//int char float double
void builtin_type()
{
	bool a = true;//1个字节 布尔型

	char b; sizeof(b);//1个字节 字符型
	unsigned char c; sizeof(c);//1个字节

	short d = 0; sizeof(c);//2个字节 短整型
	unsigned short d1; sizeof(d);//2个字节	

	int e; sizeof(e);//4个字节 整型
	unsigned int f; sizeof(f);//4个字节

	long g; sizeof(g);	//4个字节 长整型
	unsigned long h; sizeof(h);//4个字节	

	long long i; sizeof(i);//8个字节 长长整型
	unsigned long long j; sizeof(j);//8个字节

	const char* str = "hello";//str指向的内容不能改变
	sizeof(str);//4个字节 指针的大小 32位系统4个字节 64位系统8个字节 C语言字符串以'\0'结尾 1个字节
	std::cout << strlen(str) << std::endl;//5个字节
	std::cout << sizeof(str) << std::endl;

	const char* str1 = "xxx";
	std::cout << sizeof(str1) << std::endl;
	std::cout << sizeof("xxx") << std::endl;//"xxx"是一个字符串常量，占用4个字节 因为C语言字符串以'\0'结尾 1个字节


  //数组
	//定义：类型 数组名[元素个数] = {元素1, 元素2, ...};
	int arr[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };//40个字节
	std::cout << sizeof(arr) << std::endl;//40个字节
	//不用new[]分配的数组是栈上的数组，编译时就确定了数组的大小，也就是要求个数必须是常量（已知的）
	const int aa = 10;
	int a_stack_arr[aa] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };//40个字节
	//访问数组元素（第一个值）
	std::cout << arr[0] << std::endl; //1
	std::cout << *arr << std::endl; //1
	//访问第二个值
	std::cout << a_stack_arr[1] << std::endl;		//2
	std::cout << *(a_stack_arr + 1) << std::endl; //2
}

int main() {
    builtin_type();
    return 0;
}
```

---

### **C++中的自定义类型**

C++ 允许开发者定义自定义类型，以便构建更复杂的程序结构。常见的自定义类型包括：

#### **1. 类（Class）**
类是面向对象编程（OOP）中最常见的自定义类型，允许开发者定义对象的属性和方法。

```cpp
class Person {
public:
    std::string name;
    int age;

    // 构造函数
    Person(std::string n, int a) : name(n), age(a) {}

    // 方法
    void introduce() {
        std::cout << "Hello, my name is " << name << " and I am " << age << " years old." << std::endl;
    }
};
```

#### **2. 结构体（Struct）**
结构体与类相似，但结构体的成员默认是公共的，且通常用于存储数据。

```cpp
struct Point {
    int x, y;
    Point(int a, int b) : x(a), y(b) {}
};
```

#### **3. 枚举（Enum）**
枚举用于表示一组常量值，通常用于有限的离散值。

```cpp
enum Color { RED, GREEN, BLUE };
Color color = GREEN;
```

#### **4. 类型别名（Type Aliases）**
C++ 允许使用 `typedef` 或 `using` 为现有类型创建别名，使代码更易读。

```cpp
typedef unsigned long ulong;
using uint = unsigned int;
```

#### **5. 模板（Template）**
模板允许定义类型参数化的函数或类，使代码更加通用。

```cpp
template <typename T>
T add(T a, T b) {
    return a + b;
}
```

---

### **左值/右值**

在 C++ 中，**左值（Lvalue）** 和 **右值（Rvalue）** 是两个重要的概念，它们与表达式的求值位置和类型紧密相关。

#### **1. 左值（Lvalue）**
左值表示可以出现在赋值语句左侧的对象，它表示一个可以被修改的内存位置。

- **左值的特点**：
  - 有持久的内存地址，可以取地址（`&`）。
  - 可以作为赋值的目标。
  - 示例：变量、数组元素、解引用指针等。

```cpp
int a = 10;
a = 20;  // a 是一个左值
```

#### **2. 右值（Rvalue）**
右值表示没有持久内存地址的对象，通常是表达式的临时值，不能作为赋值语句的左侧。

- **右值的特点**：
  - 没有明确的内存地址，通常是临时值。
  - 右值可以被用来初始化左值。
  - 示例：常量、临时对象、返回值等。

```cpp
int a = 10;
a = 20 + 30;  // 20 + 30 是右值
```

#### **3. 左值引用与右值引用**
- **左值引用（Lvalue Reference）**：绑定到左值，使用 `&`。
  ```cpp
  int a = 10;
  int& ref = a;  // 左值引用
  ```
- **右值引用（Rvalue Reference）**：绑定到右值，使用 `&&`。右值引用允许移动语义，避免不必要的复制。
  ```cpp
  int&& rref = 20;  // 右值引用
  ```

- **普通引用/非常量引用/左值引用**： 变量引用，只能引用左值
  ```cpp
  int c = 0;
  int& ref = c;  // 左值引用
  ```

- **常量引用** ：可以引用左值和右值
  ```cpp
  int c = 0 ;
  const int& crc = c;  // 左值引用
  const int& crc2 = 1;   // 右值引用
  ```

#### **4. 重要概念**
- **完美转发（Perfect Forwarding）**：通过右值引用和 `std::forward` 可以将传递给函数的参数“完美”转发给另一个函数，保持其值类别（左值或右值）。
- **移动语义（Move Semantics）**：通过右值引用，C++ 可以“移动”资源，而不是复制，减少不必要的内存分配和提高性能。

---


### **指针/引用**

在 C++ 中，**指针（Pointer）** 和 **引用（Reference）** 是两个用于间接访问变量的强大工具。它们在内存管理、函数参数传递、动态分配和面向对象编程中起着重要作用。下面是它们的详尽比较和使用方法：

---

#### 📌 **1. 指针（Pointer）**

##### **1.1 什么是指针？**
- 指针是一个变量，存储的是另一个变量的内存地址。
- 通过指针可以间接访问和修改该地址上的数据。

##### **1.2 指针的语法**
```cpp
int a = 10;
int* p = &a; // p 是一个指向 a 的指针，& 取 a 的地址

std::cout << "a 的值: " << a << std::endl;       // 10
std::cout << "a 的地址: " << &a << std::endl;    // 0x7ffeed5c (示例地址)
std::cout << "p 指针存储的地址: " << p << std::endl; // 0x7ffeed5c
std::cout << "通过 p 访问 a 的值: " << *p << std::endl; // 10
```

##### **1.3 常见操作**
- **取地址符 `&`**：获取变量的地址。
- **解引用符 `*`**：通过指针访问（读取/修改）变量的值。
```cpp
*p = 20; // 修改 a 的值为 20
std::cout << "a 的新值: " << a << std::endl; // 20
```

##### **1.4 指针的种类**
- **空指针（nullptr）**：
```cpp
int* p = nullptr; // 指针不指向任何有效地址
```
- **野指针（Dangling Pointer）**：
```cpp
int* p; // 未初始化的指针，指向未知内存，可能会引发未定义行为
```
- **常量指针（指针常量）**：
```cpp
const int a = 10;
const int* p1 = &a; // p1 指向的值不能修改
int* const p2 = &a;  // p2 本身的指向不能修改
const int* const p3 = &a; // p3 指向和值都不能修改
```
- **指针数组** 和 **数组指针**：
```cpp
int arr[3] = {1, 2, 3};
int* pArr = arr; // 指针指向数组首元素
int (*pArray)[3] = &arr; // 数组指针，指向整个数组
```

##### **1.5 动态内存分配**
```cpp
int* p = new int(10); // 动态分配内存并初始化为 10
delete p;             // 释放内存，避免内存泄漏

int* arr = new int[5]; // 动态数组
delete[] arr;          // 释放数组内存
```

---

#### 📌 **2. 引用（Reference）**

##### **2.1 什么是引用？**
- 引用是一个变量的别名（Alias），创建后始终指向初始化的对象，无法更改。
- 使用 `&` 符号定义，但与取地址操作不同。
```cpp
int a = 10;
int& ref = a; // ref 是 a 的引用

ref = 20; // 相当于 a = 20
std::cout << "a 的值: " << a << std::endl; // 20
```
**
##### **2.2 引用的特点**
- **必须初始化**：引用在创建时必须绑定到一个有效变量。
- **不可更改绑定**：引用绑定后无法更改指向其他变量。
- **没有 `null` 引用**：不存在空引用，必须总是绑定有效对象。
- **不占用额外内存**：引用本质上是变量的一个别名，不分配新内存。

##### **2.3 常量引用**
- **保护数据不被修改**，常用于函数参数传递中以避免复制大对象。
```cpp
void showValue(const int& ref) {
    std::cout << "值: " << ref << std::endl;
}

int a = 50;
showValue(a); // 安全，不会修改 a 的值
```

---

#### 📌 **3. 指针与引用的区别**

| 特性                | 指针（Pointer）                 | 引用（Reference）               |
|-------------------|-----------------------------|-----------------------------|
| 初始化               | 可以不初始化（野指针）               | 必须初始化                     |
| 重新绑定             | 可以指向不同地址                   | 绑定后不可改变引用对象               |
| 是否为空             | 可以为 `nullptr`                | 不能为 `null`                 |
| 内存占用             | 需要内存存储地址                   | 不占用内存，作为别名存在              |
| 指针运算             | 支持算术运算 (如 `++`, `--`)        | 不支持指针运算                   |
| 用途                | 动态分配、数组、函数指针、复杂数据结构       | 主要用于安全、简单的别名和参数传递        |
| 安全性               | 容易产生野指针、空指针问题               | 更安全，避免指针操作的风险             |

---

#### 📌 **4. 指针和引用在函数中的应用**

##### **4.1 通过指针修改参数**
```cpp
void modify(int* p) {
    *p = 20;
}

int a = 10;
modify(&a); 
std::cout << "a = " << a << std::endl; // 20
```

##### **4.2 通过引用修改参数**
```cpp
void modify(int& ref) {
    ref = 30;
}

int a = 10;
modify(a);
std::cout << "a = " << a << std::endl; // 30
```

##### **4.3 指针和引用的混合使用**
```cpp
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}
```

---

#### 📌 **5. 什么时候用指针？什么时候用引用？**

- **使用指针的场景**：
  - 需要动态内存分配（例如 `new/delete`）。
  - 需要修改指针的指向（例如链表、树结构）。
  - 用于实现复杂数据结构（例如数组、函数指针、回调函数）。
  - 指针数组或数组指针的操作。

- **使用引用的场景**：
  - 用作函数参数，避免对象拷贝，提高效率（尤其是大对象）。
  - 常量引用用于只读访问对象（例如 `const T&`）。
  - 简化语法（例如范围 `for` 循环、运算符重载）。

---

#### 🎯 **6. 小结**

- **指针** 提供了灵活的内存操作能力，但容易出错，需要小心避免野指针、空指针、内存泄漏。
- **引用** 更安全、语法简洁，但缺乏指针的灵活性。
- 在实际项目中，优先使用引用，只有在需要指针特性时才使用指针，尤其是涉及动态分配和复杂数据结构时。

---
### **优秀实践**

```
class MyClass;

MyClass test(const MyClass& a,MyClass &b)
{

}
```
好的做法是加const关键字的参数，通常是作为传入参数（只读），其他参数作为传出参数。

在 C++ 中，**指针（Pointer）** 和 **引用（Reference）** 是两个用于间接访问变量的强大工具。它们在内存管理、函数参数传递、动态分配和面向对象编程中起着重要作用。下面是它们的详尽比较和使用方法：

---
## **Day3 移动语义和构造/析构/拷贝构造（2025.03.07）**
---
### **进程的内存映像**
![alt text](进程的内存映像-1.jpeg)

### **栈和堆**
- 栈：传递给函数的参数，是函数里面定义的，不通过malloc/new 分配的变量，都在栈上，对这些变量做拷贝是不计代价的。栈的内容和大小是已知的，编译时才确定。
  
```cpp
void readFile(const std::string& filename)
{
	//读文件的大小
  int size = getSizeofFile();

  //分配堆内存
	char* buf = new char[size];
  /** buf本身存放在栈上，buf指向的是堆内存*/

	//把文件内容读到buf中
	readFileToBuf(buf, size);
	//处理buf中的内容
	process(buf, size);
	//释放堆内存
	delete[] buf;
}
```

- 堆：编译时不知道，只有运行时才确定。堆是计量代价的，如果要使用堆内存，必须手动分配和手动回收，否则会造成内存泄漏。
- ***内存泄漏***： 一直分配内存，但从不手动释放，就会造成操作系统无可用内存，new 分配的内存直到通过delete还给操作系统才会失效，而且使用delete后的内存是非法的。
  - 内存泄露典型案例：在函数分配的堆内存赋给函数的局部变量，而忘记将该局部变量传出来，内存泄露发生在程序运行时，程序结束后，操作系统会回收所有程序的内存。所以内存泄漏往往在长期运行的服务器上发生。
```C++
char* flle = new int[100];
//把堆内存还给操作系统
delete file;
file  = nullptr;
```


### **new和malloc**
- new 一般是用malloc实现的
```bash
//C++风格
int* pInt = new int[100];//100×4个字节
//C风格
int* pInt2 = (int*)malloc(100 * sizeof(int));//100×4个字节
```
### **类和对象**

#### **1. 类（Class）**
类是面向对象编程（OOP）中最常见的自定义类型，允许开发者定义对象的属性和方法。

```cpp
class Person {
public:
    std::string name;
    int age;

    // 构造函数
    Person(std::string n, int a) : name(n), age(a) {}

    // 方法
    void introduce() {
        std::cout << "Hello, my name is " << name << " and I am " << age << " years old." << std::endl;
    }
};
```
**面试常见**
```cpp
//MSVC
//指针： 8字节
//总长度 4字节/8字节
//使用长度 4字节/8字节

//GCC
//指针： 8字节
//指向末尾的指针 8字节
//指向最后一个元素的指针 8字节
```

#### **2. 结构体（Struct）**
结构体与类相似，但结构体的成员默认是公共的，且通常用于存储数据。



## **Day4-1 智能指针（2025.03.18）**

### **智能指针的用法**

#### **MyClass类头文件**

```cpp
//MyClass.h
#ifndef MYCLASS_H
#define MYCLASS_H


class MyClass
{
public:
  MyClass(int val);
  ~MyClass();
  void show();

private:
  int value;
};

#endif // MYCLASS_H

```

```

```

#### **MyClass类实现文件**

```cpp
//MyClass.cpp
#include "MyClass.h"
#include <iostream>

MyClass::MyClass(int val)
  :value(val)
{
  std::cout << "Constructor: " << value << std::endl;
}

MyClass::~MyClass()
{
  std::cout << "Destructor: " << value << std::endl;
}

void MyClass::show()
{
  std::cout << "Value: " << value << std::endl;
}
```

#### **主程序main.cpp**

```cpp
#include "myclass.h"
#include <memory>
#include <iostream>
#include <vector>

//#include <QApplication>

// 1️.尝试返回一个 std::unique_ptr<MyClass> 的函数
std::unique_ptr<MyClass> createInstance(int val) {
  return std::make_unique<MyClass>(val); // ✅ 这样是安全的，返回临时 unique_ptr
}

// ❌ 错误示范（不能返回局部变量的 unique_ptr，否则会导致悬空指针）
// std::unique_ptr<MyClass> createInvalidInstance() {
//     std::unique_ptr<MyClass> ptr = std::make_unique<MyClass>(20);
//     return ptr;  // ❌ 这样会尝试拷贝 unique_ptr，而 unique_ptr 不允许拷贝
// }

void testuniqueptr()
{
  std::unique_ptr<MyClass> ptr1 = std::make_unique<MyClass>(10);  // 创建 unique_ptr
  ptr1->show(); // 访问成员函数

  // std::unique_ptr<MyClass> ptr2 = ptr1; // ❌ 错误！unique_ptr 不支持拷贝

  //std::unique_ptr<MyClass> ptr2 = std::move(ptr1);  // ✅ 使用 std::move 转移所有权

  if (!ptr1)
    std::cout << "ptr1 is now nullptr" << std::endl; //ptr1失去所有权

  // 3️⃣ 使用 createInstance() 返回 unique_ptr
  std::unique_ptr<MyClass> ptr3 = createInstance(30);
  ptr3->show();

  // 4️⃣ 使用 std::vector<std::unique_ptr<MyClass>> 存储多个 unique_ptr
  std::vector<std::unique_ptr<MyClass>>   myVector;
  myVector.push_back(std::make_unique<MyClass>(100));
  myVector.push_back(std::make_unique<MyClass>(200));
  myVector.push_back(std::make_unique<MyClass>(300));

  std::cout << "Vector Contentd" << std::endl;
  for (const auto& item : myVector)
  {
    item->show();
  }
}

void testsharedptr()
{
  std::shared_ptr<MyClass> ptr1 = std::make_shared<MyClass>(10);  // 创建 shared_ptr
  {
    std::shared_ptr<MyClass> ptr2 = ptr1;  // 共享 ptr1 的对象，引用计数+1

    std::cout << "ptr2 use count: " << ptr2.use_count() << std::endl;
    ptr2->show();
  }  // 退出作用域，ptr2 释放，引用计数-1

  std::shared_ptr<MyClass> ptr3 = ptr1;
  std::cout << "ptr3 use count: " << ptr3.use_count() << std::endl;
  ptr3->show();


  std::shared_ptr<MyClass> ptr4 = ptr1;
  std::cout << "ptr4 use count: " << ptr4.use_count() << std::endl;


  std::vector<std::shared_ptr<MyClass>> myVector2;
  myVector2.push_back(std::make_shared<MyClass>(500));
  myVector2.push_back(std::make_shared<MyClass>(600));
  myVector2.push_back(std::make_shared<MyClass>(700));

  std::cout << "Vector Contentd" << std::endl;
  for (const auto& item : myVector2)
  {
    item->show();
  }


  ptr3.reset();  // 释放 ptr3，引用计数 -1
  ptr4.reset();  // 释放 ptr4，引用计数 -1
  ptr1.reset();  // 释放 ptr1，引用计数 -1，此时应该调用析构函数

  std::cout << "ptr1 use count: " << ptr1.use_count() << std::endl;

  // 程序结束，ptr1 释放，引用计数归 0，析构对象
}

class A;

class B {
public:
  std::weak_ptr<A> a_ptr;  // ❗改成 weak_ptr
  ~B() { std::cout << "B destroyed" << std::endl; }
};

class A {
public:
  std::shared_ptr<B> b_ptr;
  ~A() { std::cout << "A destroyed" << std::endl; }
};

void testweakptr()
{
  std::shared_ptr<A> a = std::make_shared<A>();
  std::shared_ptr<B> b = std::make_shared<B>();

  a->b_ptr = b;  // A 持有 B
  b->a_ptr = a;  // B **弱引用** A（不会增加计数）

  //return 0之后 A 和 B 都会被正确释放
}

int main(int argc, char* argv[])
{

	//testuniqueptr();

	//testsharedptr();
  
	testweakptr();
  return 0;  
  //return a.exec();
}

```



## **Day4-2移动构造函数和移动赋值运算符 (C++)（2025.03.19）**

本主题介绍如何为 C++ 类编写*移动构造函数*和移动赋值运算符。 移动构造函数使右值对象拥有的资源无需复制即可移动到左值中。 有关移动语义的详细信息，请参阅 [Rvalue 引用声明符：&&](https://learn.microsoft.com/zh-cn/cpp/cpp/rvalue-reference-declarator-amp-amp?view=msvc-170)。

此主题基于用于管理内存缓冲区的 C++ 类 `MemoryBlock`。

```cpp
// MemoryBlock.h
#pragma once
#include <iostream>
#include <algorithm>

class MemoryBlock
{
public:

   // Simple constructor that initializes the resource.
   explicit MemoryBlock(size_t length)
      : _length(length)
      , _data(new int[length])
   {
      std::cout << "In MemoryBlock(size_t). length = "
                << _length << "." << std::endl;
   }

   // Destructor.
   ~MemoryBlock()
   {
      std::cout << "In ~MemoryBlock(). length = "
                << _length << ".";

      if (_data != nullptr)
      {
         std::cout << " Deleting resource.";
         // Delete the resource.
         delete[] _data;
      }

      std::cout << std::endl;
   }

   // Copy constructor.
   MemoryBlock(const MemoryBlock& other)
      : _length(other._length)
      , _data(new int[other._length])
   {
      std::cout << "In MemoryBlock(const MemoryBlock&). length = "
                << other._length << ". Copying resource." << std::endl;

      std::copy(other._data, other._data + _length, _data);
   }

   // Copy assignment operator.
   MemoryBlock& operator=(const MemoryBlock& other)
   {
      std::cout << "In operator=(const MemoryBlock&). length = "
                << other._length << ". Copying resource." << std::endl;

      if (this != &other)
      {
         // Free the existing resource.
         delete[] _data;

         _length = other._length;
         _data = new int[_length];
         std::copy(other._data, other._data + _length, _data);
      }
      return *this;
   }

   // Retrieves the length of the data resource.
   size_t Length() const
   {
      return _length;
   }

private:
   size_t _length; // The length of the resource.
   int* _data; // The resource.
};
```

以下过程介绍如何为示例 C++ 类编写移动构造函数和移动赋值运算符。



### **1.为 C++ 创建移动构造函数**

1. 定义一个空的构造函数方法，该方法采用一个对类类型的右值引用作为参数，如以下示例所示：

   ```cpp
   MemoryBlock(MemoryBlock&& other)
      : _data(nullptr)
      , _length(0)
   {
   }
   ```

2. 在移动构造函数中，将源对象中的类数据成员添加到要构造的对象：

   ```cpp
   _data = other._data;
   _length = other._length;
   ```

3. 将源对象的数据成员分配给默认值。 这可以防止析构函数多次释放资源（如内存）:

   ```cpp
   other._data = nullptr;
   other._length = 0;
   ```



### **2.为 C++ 类创建移动赋值运算符**

1. 定义一个空的赋值运算符，该运算符采用一个对类类型的右值引用作为参数并返回一个对类类型的引用，如以下示例所示：

   C++复制

   ```cpp
   MemoryBlock& operator=(MemoryBlock&& other)
   {
   }
   ```

2. 在移动赋值运算符中，如果尝试将对象赋给自身，则添加不执行运算的条件语句。

   C++复制

   ```cpp
   if (this != &other)
   {
   }
   ```

3. 在条件语句中，从要将其赋值的对象中释放所有资源（如内存）。

   以下示例从要将其赋值的对象中释放 `_data` 成员：

   ```cpp
   // Free the existing resource.
   delete[] _data;
   ```

   执行第一个过程中的步骤 2 和步骤 3 以将数据成员从源对象转移到要构造的对象：

   ```cpp
   // Copy the data pointer and its length from the
   // source object.
   _data = other._data;
   _length = other._length;
   
   // Release the data pointer from the source object so that
   // the destructor does not free the memory multiple times.
   other._data = nullptr;
   other._length = 0;
   ```

4. 返回对当前对象的引用，如以下示例所示：

   ```cpp
   return *this;
   ```



### **3.示例：完成移动构造函数和赋值运算符**

以下示例显示了 `MemoryBlock` 类的完整移动构造函数和移动赋值运算符：

```cpp
// Move constructor.
MemoryBlock(MemoryBlock&& other) noexcept
   : _data(nullptr)
   , _length(0)
{
   std::cout << "In MemoryBlock(MemoryBlock&&). length = "
             << other._length << ". Moving resource." << std::endl;

   // Copy the data pointer and its length from the
   // source object.
   _data = other._data;
   _length = other._length;

   // Release the data pointer from the source object so that
   // the destructor does not free the memory multiple times.
   other._data = nullptr;
   other._length = 0;
}

// Move assignment operator.
MemoryBlock& operator=(MemoryBlock&& other) noexcept
{
   std::cout << "In operator=(MemoryBlock&&). length = "
             << other._length << "." << std::endl;

   if (this != &other)
   {
      // Free the existing resource.
      delete[] _data;

      // Copy the data pointer and its length from the
      // source object.
      _data = other._data;
      _length = other._length;

      // Release the data pointer from the source object so that
      // the destructor does not free the memory multiple times.
      other._data = nullptr;
      other._length = 0;
   }
   return *this;
}
```



### 4.**示例 使用移动语义来提高性能**

以下示例演示移动语义如何能提高应用程序的性能。 此示例将两个元素添加到一个矢量对象，然后在两个现有元素之间插入一个新元素。 `vector` 类使用移动语义，通过移动矢量元素（而非复制它们）来高效地执行插入操作。

```cpp
// rvalue-references-move-semantics.cpp
// compile with: /EHsc
#include "MemoryBlock.h"
#include <vector>

using namespace std;

int main()
{
   // Create a vector object and add a few elements to it.
   vector<MemoryBlock> v;
   v.push_back(MemoryBlock(25));
   v.push_back(MemoryBlock(75));

   // Insert a new element into the second position of the vector.
   v.insert(v.begin() + 1, MemoryBlock(50));
}
```

该示例产生下面的输出：

```Output
In MemoryBlock(size_t). length = 25.
In MemoryBlock(MemoryBlock&&). length = 25. Moving resource.
In ~MemoryBlock(). length = 0.
In MemoryBlock(size_t). length = 75.
In MemoryBlock(MemoryBlock&&). length = 75. Moving resource.
In MemoryBlock(MemoryBlock&&). length = 25. Moving resource.
In ~MemoryBlock(). length = 0.
In ~MemoryBlock(). length = 0.
In MemoryBlock(size_t). length = 50.
In MemoryBlock(MemoryBlock&&). length = 50. Moving resource.
In MemoryBlock(MemoryBlock&&). length = 25. Moving resource.
In MemoryBlock(MemoryBlock&&). length = 75. Moving resource.
In ~MemoryBlock(). length = 0.
In ~MemoryBlock(). length = 0.
In ~MemoryBlock(). length = 0.
In ~MemoryBlock(). length = 25. Deleting resource.
In ~MemoryBlock(). length = 50. Deleting resource.
In ~MemoryBlock(). length = 75. Deleting resource.
```

在 Visual Studio 2010 之前，此示例生成了以下输出：

```Output
In MemoryBlock(size_t). length = 25.
In MemoryBlock(const MemoryBlock&). length = 25. Copying resource.
In ~MemoryBlock(). length = 25. Deleting resource.
In MemoryBlock(size_t). length = 75.
In MemoryBlock(const MemoryBlock&). length = 25. Copying resource.
In ~MemoryBlock(). length = 25. Deleting resource.
In MemoryBlock(const MemoryBlock&). length = 75. Copying resource.
In ~MemoryBlock(). length = 75. Deleting resource.
In MemoryBlock(size_t). length = 50.
In MemoryBlock(const MemoryBlock&). length = 50. Copying resource.
In MemoryBlock(const MemoryBlock&). length = 50. Copying resource.
In operator=(const MemoryBlock&). length = 75. Copying resource.
In operator=(const MemoryBlock&). length = 50. Copying resource.
In ~MemoryBlock(). length = 50. Deleting resource.
In ~MemoryBlock(). length = 50. Deleting resource.
In ~MemoryBlock(). length = 25. Deleting resource.
In ~MemoryBlock(). length = 50. Deleting resource.
In ~MemoryBlock(). length = 75. Deleting resource.
```

使用移动语义的此示例版本比不使用移动语义的版本更高效，因为前者执行的复制、内存分配和内存释放操作更少。



### **5.可靠编程**

若要防止资源泄漏，请始终释放移动赋值运算符中的资源（如内存、文件句柄和套接字）。

若要防止不可恢复的资源损坏，请正确处理移动赋值运算符中的自我赋值。

如果为你的类同时提供了移动构造函数和移动赋值运算符，则可以编写移动构造函数来调用移动赋值运算符，从而消除冗余代码。 以下示例显示了调用移动赋值运算符的移动构造函数的修改后的版本：

```cpp
// Move constructor.
MemoryBlock(MemoryBlock&& other) noexcept
   : _data(nullptr)
   , _length(0)
{
   *this = std::move(other);
}
```

[std::move](https://learn.microsoft.com/zh-cn/cpp/standard-library/utility-functions?view=msvc-170#move) 函数将左值 `other` 转换为右值。

## **Day4-3 C++四大函数回顾（构造/拷贝构造/析构/赋值运算符函数）（2025.03.19）**

### **Point类**

```cpp
#include <iostream>

using std::cout;
using std::endl;

class Point
{
public:
	Point(int ix = 0, int iy = 0)
		: _ix(ix)
		, _iy(iy)
	{
		cout << "Point(int,int)" << endl;
	}
	~Point() {
		cout << "~Point()" << endl;
	}

	//拷贝构造函数
	Point(const Point& rhs)
		: _ix(rhs._ix)
		, _iy(rhs._iy)
	{
		cout << "Point(const Point&)" << endl;
	}
	//问题1：拷贝构造函数中的引用符号能不能去掉？
	//问题2：拷贝构造函数中的const关键字能不能去掉？

	//解答1：去掉引用符号，会导致无穷递归调用，直到栈溢出（满足拷贝构造函数的调用时机1：形参与实参结合）
	//解答2：去掉const关键字，会导致无法传递右值，即无法传递临时对象

	//左值和右值
	int number = 10;
	int& ref = number;//左值引用

	//int& ref1 = 10;//错误，不能将右值赋值给左值引用
	int&& ref2 = 10;//右值引用
	const int& ref3 = 10;//常量左值引用


	//赋值运算符函数
	Point& operator=(const Point& rhs)
	{
		cout << "Point& operator=(const Point&)" << endl;
		if (this != &rhs)
		{
			_ix = rhs._ix;
			_iy = rhs._iy;
		}
	}

	void print() const
	{
		cout << "(" << _ix
			<< "," << _iy
			<< ")" << endl;
	}

private:
	int _ix;
	int _iy;

};


class Line
{
public:
	Line(int x1, int y1, int x2, int y2)
		:_pt1(x1, y1)//如果不显示调用构造函数，会调用默认构造函数，所以最好可以显示初始化子对象
		, _pt2(x2, y2)
	{
		//cout << "Line(int,int,int,int)" << endl;
	}
	//问题：为什么没有定义析构函数，也没有调用析构函数，就能释放资源？
	// 定义析构函数
	~Line()
	{
		cout << "~Line()" << endl;
	}

public:
	void print() const
	{
		_pt1.print();
		cout << "---->";
		_pt2.print();
	}

private:
	Point _pt1;
	Point _pt2;

};

void testLine()
{
	Line line(1, 2, 3, 4);
	line.print();
}


void test() {
	Point pt;//栈对象
	cout << "pt = ";
	pt.print();

	//Point* p = new Point();//堆对象

	Point pt2(3, 4);//栈对象
	cout << "pt2 = ";
	pt2.print();

	Point pt3 = pt2;//拷贝构造函数
	cout << "pt3 = ";
	pt3.print();
}

Point func()
{
	Point pt3(1, 2);
	cout << "pt3 = ";
	pt3.print();
	return pt3;
}

void test1()
{
	Point pt4 = func();
	cout << "pt4 = ";
	pt4.print();
}

int main(int argc, char* argv[])
{
	cout << "begin test..." << endl;
	//test();

	//test1();
	cout << "end test..." << endl;

	cout << "sizeof(Point)= " << sizeof(Point) << endl;

	testLine();
	return 0;
}
```

### **Computer类**

```cpp
//Computer.h
#pragma once
#include <iostream>
using std::cout;
using std::endl;
class Computer
{
public:
	Computer(const char* brand,float price);
	Computer(const Computer& rhs);
	Computer operator=(const Computer& rhs);
	~Computer();
	void setBrand(const char* brand);
	void setPrice(int price);
	void print();
	void print() const;
	static void printTotalPrice();//静态成员函数void printTotalPrice();
private:
	char* _brand;
	float _price;
	static float _totalPrice;//静态数据成员
};


```

**实现文件**

```cpp
#include "Computer.h"
#define _CRT_SECURE_NO_WARNINGS

float Computer::_totalPrice = 0.0f;

/*
1.	构造函数：
•	Computer(const char* brand, float price)：分配内存并复制品牌字符串。
2.	拷贝构造函数：
•	Computer(const Computer& rhs)：分配新内存并复制 rhs 的品牌字符串，而不是简单地复制指针。
3.	析构函数：
•	~Computer()：释放分配的内存。
*/
Computer::Computer(const char* brand, float price)
  : _brand(new char[strlen(brand) + 1]())
  , _price(price)
{
  std::cout << "Computer(const char*,float)" << std::endl;
  strcpy_s(_brand, strlen(brand) + 1, brand);

	_totalPrice += _price;
}


//Computer::Computer(const Computer& rhs) 
//  : _brand(rhs._brand)  // 浅拷贝
//  , _price(rhs._price)

Computer::Computer(const Computer& rhs)
  : _brand(new char[strlen(rhs._brand) + 1]()) // 深拷贝
  , _price(rhs._price)
{
  std::cout << "Computer(const Computer&)" << std::endl;
  strcpy_s(_brand, strlen(rhs._brand) + 1, rhs._brand); // 复制内容
}

/*赋值运算符函数中需要关注的几个问题*/
//赋值运算符函数参数中的引用符号能不能去掉？
//赋值运算符函数参数中的const关键字能不能去掉？
//赋值运算符函数返回类型中的引用能不能去掉？
//赋值运算符函数的返回类型可以不是对象吗？

//解答1：在形参与实参结合的时候，会多执行一次拷贝构造函数
//解答2：去掉const关键字，会导致无法传递右值，即无法传递临时对象
//解答3：函数的返回类型是类类型的时候，会满足拷贝构造函数的调用条件
//解答4：返回类型可以不是对象，但是会导致无法连续赋值 pc3 = pc2 = pc1
Computer Computer::operator=(const Computer& rhs)
{
	// TODO: 在此处插入 return 语句
  std::cout << "Computer& operator=(const Computer&)" << std::endl;
	// 防止自复制 pc = pc
  if (this != &rhs) {
		delete[] _brand;
		_brand = new char[strlen(rhs._brand) + 1]();
		strcpy_s(_brand, strlen(rhs._brand) + 1, rhs._brand);
		_price = rhs._price;
  }
	//_brand = rhs._brand;// 浅拷贝
	//_price = rhs._price;// 浅拷贝
	/*
  * 问题1：两个指针指向同一块内存，当其中一个对象释放内存后，另一个对象的指针就变成了野指针（double free）
	* 例如：pc2 = pc；pc2析构释放内存后，pc的析构函数再次释放内存，导致double free
  */
  
	//double free 的解决办法：使用深拷贝

	/*
  * 问题2：两个指针指向同一块内存，当其中一个对象释放内存后，另一个对象的指针就变成了悬空指针（dangling pointer）
  * pc2改变指针指向之后，原来pc2指向的那块堆内存就找不到了
  */

	//内存泄漏的解决办法：delete[] _brand; _brand = new char[strlen(rhs._brand) + 1](); strcpy(_brand, rhs._brand);
	return *this;
}

Computer::~Computer()
{
  std::cout << "~Computer()" << std::endl;
	_totalPrice -= _price;
  if (_brand) {
		delete[] _brand;
		_brand = nullptr;
  }
}

void Computer::setBrand(const char* brand)
{
  // 这里可以实现设置品牌的逻辑
  // 释放旧的品牌内存
  delete[] _brand;
  // 分配新内存并复制品牌
  _brand = new char[strlen(brand) + 1]();
  strcpy_s(_brand, strlen(brand) + 1, brand);
}



void Computer::setPrice(int price)
{
  // 这里可以实现设置价格的逻辑
	_price = price;
}

void Computer::print()
{
	cout << "print()" << endl;
  std::cout << "brand:" << _brand << std::endl;
  std::cout << "price:" << _price << std::endl;
}

void Computer::print() const
{
	cout << "print() const" << endl;
	std::cout << "brand:" << _brand << std::endl;
	std::cout << "price:" << _price << std::endl;
}

void Computer::printTotalPrice()
{
	std::cout << "total price:" << _totalPrice << std::endl;
}

void testStaticMember() {
	cout << "购买第一台电脑前的总价：" << endl;
	Computer::printTotalPrice();
	Computer pc("lenovo", 5555);
	pc.print();
	cout << "购买第一台电脑后的总价：" << endl;
	pc.printTotalPrice();
	Computer pc2("Mac", 8888);
	pc2.print();
	cout << "购买第二台电脑后的总价：" << endl;
	pc2.printTotalPrice();
	cout << endl;
	pc2.setBrand("Mac");
	pc.print();
	pc2.print();
}

void testCopyConstructor() {
  Computer pc("lenovo", 5555);
  /*pc.setBrand("ThinkPad");
  pc.setPrice(8888);*/
  pc.print();

  Computer pc2 = pc;
  pc2.print();

  cout << endl;
	pc2.setBrand("Mac");
	pc.print();
	pc2.print();
}


void testAssignmentOperator() {
	Computer pc("lenovo", 5555);
	cout << "pc: " << endl;
	pc.print();
	Computer pc2("Mac", 8888);
	cout << "pc2: " << endl;
	pc2.print();
	pc2 = pc;
  cout << "pc2: " << endl;
	pc2.print();

  pc2 = Computer("lenovo", 6666);
  pc2.print();
}

void testConst()
{
	//const对象只能调用const成员函数，非const对象可以调用任意成员函数
	const Computer pc("lenovo", 5555);
	pc.print();
	cout << endl << endl;

	Computer pc2("Mac", 8888);
	pc2.print();
}

int main(int argc, char* argv[])
{
  //testCopyConstructor();

	//testAssignmentOperator();

	//testStaticMember();

	testConst();
  return 0;
}

```
---

## **Day4-4 New 与 delete 表达式（2025.03.20）**

在 C++ 中，`new` 和 `delete` 是用于动态内存管理的关键字。它们分别用于在堆上分配和释放对象或数组。

### **1. `new` 表达式的三个步骤**

1. **调用 `operator new` 标准库函数**
   - `operator new` 负责在堆上申请一块原始的、未初始化的内存空间，以便存储对象。
   - 如果申请失败，会抛出 `std::bad_alloc` 异常（除非使用 `nothrow` 版本的 `new`）。

2. **调用构造函数**
   - 在申请到的内存空间上调用构造函数，初始化对象的数据成员。

3. **返回指针**
   - 返回指向新分配对象的指针，以供程序使用。

示例代码：
```cpp
class A {
public:
    A() { cout << "A() constructor" << endl; }
    ~A() { cout << "~A() destructor" << endl; }
};

int main() {
    A* p = new A;  // 调用 operator new 申请内存 -> 调用构造函数 -> 返回指针
    delete p;      // 调用析构函数 -> 调用 operator delete 释放内存
    return 0;
}
```

### **2. `delete` 表达式的两个步骤**

1. **调用析构函数**
   - 在释放对象所占的内存前，先执行析构函数，以回收对象的数据成员所占用的资源（如动态分配的内存、文件句柄等）。

2. **调用 `operator delete` 库函数**
   - 释放对象本身所占用的内存空间，使其可被重新分配。

示例代码：
```cpp
A* p = new A;
delete p; // 先调用析构函数，再释放内存
```

### **3. `new[]` 与 `delete[]`**

- `new[]` 用于动态分配对象数组，必须使用 `delete[]` 释放。
- `delete` 只能释放单个对象，而 `delete[]` 释放的是整个数组。

```cpp
A* arr = new A[5];  // 申请 5 个对象的数组

delete[] arr;       // 释放数组
```

如果用 `delete` 释放 `new[]` 申请的数组，可能会导致未完全析构的对象泄漏。

---

## **Day5 类的定义和关键字再探（2025.03.24）**

### **1. C++ 关键字 `const`、`static`、`extern`**

```cpp
const int global_a = 10;  // 只读常量
static int s_b = 10;      // 静态全局变量（仅限当前文件可见）
extern int SIZE = 10;     // 外部变量（多个文件可共享）
```

- `const`：用于定义只读变量，防止意外修改。
- `static`：修饰局部变量时，延长变量生命周期，修饰全局变量时，限制其作用域。
- `extern`：用于声明外部变量，使其可以在多个文件中访问。

示例：
```cpp
void test() {
    static int fun_c = 0;  // 仅初始化一次，生命周期贯穿整个程序
    fun_c++;
    cout << "fun_c = " << fun_c << endl;
}
```

### **2. 类的定义：Circle 和 Rect**

```cpp
class Circle {
public:
    Circle(double r = 0) : _r(r) {
        cout << "Circle()" << endl;
    }
    
    ~Circle() {
        cout << "~Circle()" << endl;
    }

    double getArea() const {
        return M_PI * _r * _r;
    }

private:
    double _r;
};
```

**要点：**
- **构造函数（Constructor）：** 负责初始化对象，可以带默认参数。
- **析构函数（Destructor）：** 负责清理资源，名称前带 `~`。
- **成员函数（Member Function）：** `const` 关键字保证不会修改对象数据。
  
### **3. 完整代码**

```cpp
#define _USE_MATH_DEFINES
#include <iostream>
#include <cmath>

using namespace std;
//关键字
//const
const int global_a = 10;
//static
static int s_b = 10;
//extern
extern int SIZE  = 10;
//三者的用途：const 修饰常量，static 修饰全局变量，extern 修饰外部变量

void test()
{
	int local_v = 0;
	static int fun_c;//静态局部变量 可以延长变量的生命周期
	fun_c++;
	cout << "local_v = " << " " << local_v << " ," << "fun_c = " << " " << fun_c << endl;
}

//定义一个圆
class Circle
{
public:
	Circle(double r = 0)
		: _r(r)
	{
		cout << "Circle(double r = 0)" << endl;
	}

	Circle(double r,char* name)
		: _r(0)
		, _name(new char[strlen(name) + 1])
	{
		strcpy_s(_name, strlen(name) + 1,name);
		cout << "Circle(double r = 0)" << endl;
	}
	~Circle()
	{
		cout << "~Circle()" << endl;
	}

	double getRadius() const
	{
		return _r;
	}

	//setRadius
	void setRadius(double r)
	{
		_r = r;
	}

	double getArea() const
	{
		return M_PI * _r * _r;
	}

private:
	double _r;
	char* _name;
};

//定义一个矩形
class Rect
{
public:
	Rect(double l = 0, double w = 0)
		: _l(l)
		, _w(w)
	{
		cout << "Rect(double l = 0, double w = 0)" << endl;
	}
	~Rect()
	{
		cout << "~Rect()" << endl;
	}

private:
	double _l;
	double _w;
};

void testCircle()
{
	Circle c1(10);
	cout << "c1.getRadius() = " << c1.getRadius() << endl;
	cout << "c1.getArea() = " << c1.getArea() << endl;
	cout << endl << endl;
	Circle c2;
	c2.setRadius(20);
	cout << "c2.getRadius() = " << c2.getRadius() << endl;
	cout << "c2.getArea() = " << c2.getArea() << endl;
}

int main()
{
	test();
	test();
	test();
	cout << endl << endl;

	testCircle();
	return 0;
}
```

---

## **Day5-1 运算符重载（2025.03.24）**

### **1. 复数类 `Complex` 及其运算符重载**

```cpp
class Complex {
    friend Complex operator*(const Complex& lhs, const Complex& rhs);
public:
    Complex(double r = 0, double i = 0) : _real(r), _imag(i) {}
    
    Complex operator-(const Complex& rhs) const {
        return Complex(_real - rhs._real, _imag - rhs._imag);
    }

    Complex& operator++() {  // 前置++
        ++_real;
        ++_imag;
        return *this;
    }

    Complex operator++(int) {  // 后置++
        Complex tmp(*this);
        _real++;
        _imag++;
        return tmp;
    }
    
    Complex& operator+=(const Complex& rhs) {
        _real += rhs._real;
        _imag += rhs._imag;
        return *this;
    }

    void display() const {
        cout << _real << " + " << _imag << "i" << endl;
    }

private:
    double _real;
    double _imag;
};
```

### **2. 运算符重载的几种方式**

- **成员函数重载**（适用于 `+=`、`++`、`-`）：
  ```cpp
  Complex operator-(const Complex& rhs) const;
  Complex& operator++();
  Complex operator++(int);
  Complex& operator+=(const Complex& rhs);
  ```

- **友元函数重载**（适用于 `+`、`*`）：
  ```cpp
  friend Complex operator+(const Complex& lhs, const Complex& rhs);
  friend Complex operator*(const Complex& lhs, const Complex& rhs);
  ```

### **3. 关键区别**

| 运算符 | 适用方式 | 说明 |
|--------|---------|------|
| `+` | 普通函数 | 需要访问两个对象的数据，可用友元函数 |
| `-` | 成员函数 | 只需要访问当前对象数据 |
| `++` | 成员函数 | 影响当前对象，前置++返回引用，后置++返回值 |
| `*` | 友元函数 | 需要访问两个对象的数据 |

示例代码：
```cpp
Complex c1(1, 2), c2(3, 4);
Complex c3 = c1 + c2;
c3.display();
```

**总结：**
- 友元函数适用于二元运算（如 `+`、`*`）。
- 成员函数适用于一元运算（如 `++`）和复合赋值运算（如 `+=`）。


### **4.完整代码**

```cpp
#include <iostream>
#include <limits.h>

using namespace std;

//复数
class Complex
{
	friend Complex operator*(const Complex& lhs, const Complex& rhs);
	//Complex operator/(const Complex& rhs) const;
public:
	Complex(double r = 0, double i = 0)
	: _real(r), _imag(i) 
	{
		cout << "Complex(double r = 0, double i = 0)" << endl;
	}

	~Complex()
	{
		cout << "~Complex()" << endl;
	}

	double getReal() const
	{
		return _real;
	}

	double getImag() const
	{
		return _imag;
	}

	//运算符重载之成员函数
	Complex operator-(const Complex& rhs) const
	{
		return Complex(_real - rhs._real, _imag - rhs._imag);
	}
	
	//自增运算符重载
	Complex& operator++() //前置++
	{
		++_real;
		++_imag;
		return *this;
	}

	//后置自增运算符重载
	Complex operator++(int) //后置++
	{
		Complex tmp(*this);
		_real++;
		_imag++;
		return tmp;//返回类型是一个临时对象，所以不适用引用类型
	}

	//前置++和后置++的区别？
	//解答：前置++返回是对象的引用，是左值可以取地址
	//后置++返回的是局部对象，是右值不能取地址
	
	//+=运算符重载
	//复合赋值运算符重载，推荐以成员函数的形式重载
	Complex& operator+=(const Complex& rhs)
	{
		_real += rhs._real;
		_imag += rhs._imag;
		return *this;
	}



	void display() const
	{
		
		if (_imag > 0)
		{
			cout << _real << " + " << _imag << "i" << endl;
		}
		else if (_imag == 0)
		{
			cout << _real << endl;
		}
		else
		{
			cout << _real << " - " << -_imag << "i" << endl;
		}
	}
private:
	double _real;
	double _imag;
};	

//“operator + ”必须至少有一个类类型的形参
//int operator+(int lhs, int rhs)
//{
//	//return _real + rhs;
//}

//运算符重载之普通函数
Complex operator+(const Complex& lhs, const Complex& rhs)
{
	return Complex(lhs.getReal() + rhs.getReal(), lhs.getImag() + rhs.getImag());
}


//运算符重载之友元函数（推荐使用）
Complex operator*(const Complex& lhs, const Complex& rhs)
{
	return Complex(lhs._real * rhs._real - lhs._imag * rhs._real,
		lhs._real * rhs._real + lhs._real * rhs._real);
}

void test()
{
	Complex c1(1, 2);
	cout << "c1 = ";
	c1.display();
	cout << endl << endl;

	Complex c2(3, 4);
	cout << "c2 = ";	
	c2.display();
	cout << endl << endl;

	Complex c3 = c1 + c2;
	cout << "c3 = c1 + c2 = ";
	c3.display();
}

void test2()
{
	Complex c1(1, 2);
	cout << "c1 = ";
	c1.display();
	cout << endl << endl;
	Complex c2(3, 4);
	cout << "c2 = ";	
	c2.display();
	cout << endl << endl;

	Complex c3 = c1 - c2;
	cout << "c3 = c1 - c2 = ";
	c3.display();

}

void test3()
{
	Complex c1(1, 2);
	cout << "c1 = ";
	c1.display();
	cout << endl << endl;
	Complex c2(3, 4);
	cout << "c2 = ";
	c2.display();
	cout << endl << endl;
	Complex c3 = c1 * c2;
	cout << "c3 = c1 * c2 = ";
	c3.display();

	c3 += c1;
	cout << "c3 += c1 = ";
	c3.display();

	
}

void test4()
{
	Complex c3(4, 6);
	cout << "c3 = ";
	c3.display();
	cout << endl << endl;
	++c3;
	cout << "++c3 = ";
	c3.display();

	cout << endl << endl;
	c3++;
	cout << "c3++ = ";
	c3.display();

	Complex c4(4, 6);
	cout << "c4 = ";
	c4.display();
	c4++;
	cout << "c4++ = ";
	c4.display();
	cout << "前置++返回是对象的引用，是左值可以取地址，后置++返回的是局部对象，是右值不能取地址";
}

int main(int argc, char*  argv[])
{
	//test();
	//test2();
	//test3();
	test4();
	return 0;
}
```
---

## **Day5-2 文件IO（2025.03.24）**

### **1. 缓冲区与刷新**

在C++的标准输入输出流 (`iostream`) 中，I/O 操作通常会涉及缓冲区，以提高效率。默认情况下，`cout` 采用缓冲模式，输出内容会暂存到缓冲区，只有在缓冲区满了或遇到某些特殊情况（如换行）时，数据才会被刷新到屏幕。

#### **1.1 常见的缓冲刷新方式**

| 方法 | 作用 | 例子 |
|------|------|------|
| `cout << endl;` | 输出换行并刷新缓冲区 | `cout << "Hello" << endl;` |
| `cout << flush;` | 只刷新缓冲区，但不换行 | `cout << "Hello" << flush;` |
| `cout << ends;` | 在缓冲区加入一个空字符（`\0`），但不会刷新 | `cout << "Hello" << ends;` |

```cpp
#include <iostream>
#include <thread>
#include <chrono>

using namespace std;

void test()
{
    for (size_t i = 0; i < 1024; i++)
    {
        cout << "a";
    }

    cout << 'b' << endl; // 刷新缓冲区并换行
    cout << "c" << flush; // 只刷新，不换行
    cout << "d" << ends;  // 只插入空字符，不刷新

    this_thread::sleep_for(chrono::seconds(5));
}

int main()
{
    test();
    return 0;
}
```

### **2. 文件读写操作**

C++标准库提供了 `<fstream>` 头文件，其中包含三个主要的文件流类：

| 类名 | 作用 |
|------|------|
| `ifstream` | 读文件（input file stream） |
| `ofstream` | 写文件（output file stream） |
| `fstream` | 读写文件（file stream） |

#### **2.1 读取文件**

```cpp
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

void readFile()
{
    ifstream ifs("test.txt"); // 打开文件
    if (!ifs.is_open())
    {
        cerr << "文件打开失败" << endl;
        return;
    }

    string line;
    while (getline(ifs, line))
    {
        cout << line << endl;
    }
    ifs.close(); // 关闭文件
}

int main()
{
    readFile();
    return 0;
}
```

#### **2.2 写入文件**

```cpp
void writeFile()
{
    ofstream ofs("output.txt");
    if (!ofs)
    {
        cerr << "文件打开失败" << endl;
        return;
    }

    ofs << "Hello, world!" << endl;
    ofs.close();
}
```

#### **2.3 追加模式写入**

使用 `ios::app` 选项可以在文件末尾追加内容，而不会覆盖原有数据。

```cpp
void appendToFile()
{
    ofstream ofs("log.txt", ios::app);
    if (!ofs)
    {
        cerr << "文件打开失败" << endl;
        return;
    }
    ofs << "新日志记录" << endl;
    ofs.close();
}
```

#### **2.4 完整代码**

```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <vector>

using namespace std;

void test()
{
	ifstream ifs("test.txt");
	if (!ifs.good())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//文件流读文件
	//对于文件输入流而言，默认以空格作为分隔符
	string line;
	while (ifs >> line)//cin >> line
	{
		cout << line << endl;
	}
}

void test2()
{
	ifstream ifs("test.txt");
	if (!ifs.good())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//文件流读文件
	//对于文件输入流而言，默认以空格作为分隔符
	string line;
	while (getline(ifs,line))
	{
		cout << line << endl;
	}
}

//打印前几行的内容
void test3()
{
	ifstream ifs("test.txt");
	if (!ifs.good())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//文件流读文件
	//对于文件输入流而言，默认以空格作为分隔符
	string line;
	int lineNum = 0;
	while (getline(ifs, line))
	{
		cout << line << endl;
		lineNum++;
		if (lineNum == 5)
		{
			break;
		}
	}
}

//打印指定行的内容
void test4()
{
	string fileName = "test.txt";
	ifstream ifs(fileName);
	if (!ifs.good())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//文件流读文件
	//对于文件输入流而言，默认以空格作为分隔符
	string line[120];
	size_t lineNum = 0;

	vector<string> vec;
	while (getline(ifs, line[lineNum]))
	{
		++lineNum;
	}
	cout << "line[22] = " << line[22] << endl;
}

void test5()
{
	string fileName = "test.txt";
	ifstream ifs(fileName);
	if (!ifs.good())
	{
		cout << "文件打开失败" << endl;
		return;
	}
	//文件流读文件
	//对于文件输入流而言，默认以空格作为分隔符
	string line;
	vector<string> vec;
	while (getline(ifs, line))
	{
		vec.push_back(line);
	}
	cout << "vec[22] = " << vec[22] << endl;
}

void test6()
{
	ifstream ifs("test.txt");
	if (!ifs.good())
	{
		cerr << "ifstream is not good!" << endl;
		return;
	}

	ofstream ofs("wd.txt");
	if (!ofs.good())
	{
		cerr << "ofstream is not good!" << endl;
		ifs.close();//在程序异常退出时，关闭前面的ifs
		return;
	}

	string line;
	while (getline(ifs, line))
	{
		ofs << line << endl;
	}

	ifs.close();
	ofs.close();
}

void test7()
{
	//对于文件的输出输出流而言，当文件不存在的时候就打开失败
	fstream fs("wenchang.txt");
	if (!fs.good())
	{
		cerr << "fstream is not good!" << endl;
		return;
	}
	//业务逻辑
	//通过键盘输入数据,使用fs进行读文件，随后输出到屏幕
	int number = 0;
	cout << "请输入5次数字： " << endl;
	for (size_t idx = 0; idx != 5; ++idx)
	{
		cin >> number;
		fs << number << " ";
	}

	size_t len = fs.tellp();
	cout << "len= " << len << endl;
	fs.seekp(0);
	len = fs.tellp();
	cout << "len= " << len << endl;

	for (size_t idx = 0; idx != 5; ++idx)
	{
		cout << "fs.failbit = " << fs.fail() << endl
			<< "fs.eofbit = " << fs.eof() << endl
			<< "fs.goodbit = " << fs.good() << endl;
		fs >> number;
		cout << number << " ";
	}
	cout << endl;

	fs.close();
}

//追加模式
void test8()
{
	ifstream ifs("test.txt", std::ios::in | std::ios::ate);
	if (!ifs.good())
	{
		cerr << "ifstream is not good!" << endl;
		return;
	}

	cout << "ifs.tellg() = " << ifs.tellg() << endl;

	ifs.close();
}

void test9()
{
	ofstream ofs("wenchang.txt",std::ios::out | std::ios::app);
	if (!ofs.good())
	{
		cerr << "ofstream is not good!" << endl;
		return;
	}

	cout << "ofs.tellg() = " << ofs.tellp() << endl;

	ofs.close();
}


int main(int argc, char* argv[])
{	
	//test();
	//test5(); //读取文件内容到vector容器中
	test9();
	return 0;
}

```

### **3. 文件定位操作**

文件流支持 `seekg()`（get）和 `seekp()`（put）来定位文件指针。

```cpp
void filePositioning()
{
    fstream fs("test.txt", ios::in | ios::out);
    if (!fs)
    {
        cerr << "文件打开失败" << endl;
        return;
    }

    fs.seekg(0, ios::end); // 移动到文件末尾
    cout << "文件大小: " << fs.tellg() << " 字节" << endl;
    fs.close();
}
```

### **4. 字符串IO**

C++ 提供了 `stringstream` 以便在字符串中进行 I/O 操作。

```cpp
#include <sstream>

void stringStreamDemo()
{
    stringstream ss;
    int num = 42;
    ss << "数字: " << num;
    string output = ss.str();
    cout << output << endl;
}
```

### **5. 配置文件解析示例**

```cpp
void readConfig(const string& filename)
{
    ifstream ifs(filename);
    if (!ifs)
    {
        cerr << "打开文件失败: " << filename << endl;
        return;
    }

    string line;
    while (getline(ifs, line))
    {
        istringstream iss(line);
        string key, value;
        iss >> key >> value;
        cout << key << " -> " << value << endl;
    }
    ifs.close();
}
```

### **6. 完整代码**

```cpp
#include <iostream>
#include <sstream>
#include <fstream> // 添加此行以包含 ifstream 的定义

using namespace std;
using std::ostringstream;
using std::istringstream;
using std::stringstream;

string int2string(int value)
{
	ostringstream oss;//中转
	oss << value;

	return oss.str();//获取底层的字符串
}

void  test()
{
	int number = 10;
	string s1 = int2string(number);
	cout << "s1 = " << s1 << endl;
}

void test2()
{
	int number1 = 10;
	int number2 = 20;
	stringstream ss;
	ss << "number1 = " << number1
	 	<< "number2 = " << number2;
	string s1 = ss.str();
	cout << "s1 =" <<  s1 << endl;

	string key;
	string value;
	while (ss >> key >> value)
	{
		cout << key << "--->" << value << endl;
	}

}

void readConfig(const string& filename)
{
	ifstream ifs(filename);
	if (!ifs)
	{
		cerr << "open" << filename << "error!" << endl;
		return;
	}

	string line;
	while (getline(ifs, line))
	{
		istringstream iss(line);
		string key, value;
		iss >> key >> value;

		 
		cout << key << "  " << value << endl;
	}
	ifs.close();
}

void test3()
{
	readConfig("my.conf");
}

int main()
{
	test3();
	return 0;
}
```


### **7. 二进制文件操作**

```cpp
#include <iostream>
#include <fstream>
using namespace std;

struct Data {
    int id;
    double value;
};

// 写入二进制文件
void writeBinaryFile(const string &filename)
{
    ofstream ofs(filename, ios::binary);
    if (!ofs)
    {
        cerr << "文件打开失败!" << endl;
        return;
    }
    Data d = {1, 3.14};
    ofs.write(reinterpret_cast<const char *>(&d), sizeof(d));
    ofs.close();
}

// 读取二进制文件
void readBinaryFile(const string &filename)
{
    ifstream ifs(filename, ios::binary);
    if (!ifs)
    {
        cerr << "文件打开失败!" << endl;
        return;
    }
    Data d;
    ifs.read(reinterpret_cast<char *>(&d), sizeof(d));
    cout << "id: " << d.id << ", value: " << d.value << endl;
    ifs.close();
}

int main()
{
    string filename = "data.bin";
    writeBinaryFile(filename);
    readBinaryFile(filename);
    return 0;
}
```

### **总结**
1. **文本文件**
   - `ifstream` 读取文件，`ofstream` 写入文件，`fstream` 读写文件。
   - `seekg()` 和 `seekp()` 控制文件指针。
   - `stringstream` 适用于字符串的 I/O 操作。
   - `ios::app` 可用于追加模式写入文件。
2. **二进制文件**
   - 读写方式：`ifstream`、`ofstream` 需使用 `ios::binary` 模式
   - 读取方式：`read()` 和 `write()`
   - 适用于存储结构化数据，如 `struct` 类型的存储和读取。

---




## **Day6-1 输入输出流运算符重载（2025.03.25）**

```plaintext
1. 拷贝构造函数的调用时机
2. 友元
   2.1 友元函数
3. 输入输出流运算符重载
   3.1 关键知识点
   3.2 代码
   3.3 关键问题
   3.4 完整代码
4. 下标访问运算符 `operator[]`
   4.1 关键知识点
   4.2 代码
5. 函数调用运算符 `operator()`
   5.1 关键知识点
   5.2 代码
   5.3 示例
   5.4 完整代码
6. 总结
```

---

### **1. 回顾**
#### **1.1 拷贝构造函数的调用时机**
拷贝构造函数在以下情况会被调用：
1. **对象初始化**  
   - 当用一个已经存在的对象去初始化一个刚刚创建的对象时，调用拷贝构造函数。  
   - 例：
     ```cpp
     Complex c1(1, 2);
     Complex c2 = c1; // 调用拷贝构造函数
     ```
   
2. **函数参数传递**
   - 当形参与实参都是对象时，在函数调用时会调用拷贝构造函数。  
   - 例：
     ```cpp
     void func(Complex c) { }
     func(c1); // 形参与实参结合时调用拷贝构造函数
     ```

3. **函数返回对象**
   - 当函数返回一个对象时，可能调用拷贝构造函数（但现代 C++ 编译器会尝试优化此过程，如返回值优化 RVO）。
   - 例：
     ```cpp
     Complex func() {
         Complex c(3, 4);
         return c; // 可能调用拷贝构造函数
     }
     ```

---

### **2. 友元**
#### **2.1 友元函数**
- 友元函数可以访问类的私有成员。
- 友元声明可以出现在类的 `public`、`protected` 或 `private` 部分，不影响其权限。
- 友元关系是**单向的、不可传递的**：
  - **单向**：如果 `A` 是 `B` 的友元，`B` 并不会自动成为 `A` 的友元。
  - **不可传递**：如果 `A` 是 `B` 的友元，`B` 是 `C` 的友元，`A` 并不会自动成为 `C` 的友元。

---

### **3. 输入输出流运算符重载**
#### **3.1 关键知识点**
1. **`operator<<` 必须是友元函数**
   - 由于 `cout << c1;` 左操作数是 `std::ostream`，不能修改 `std::ostream`，所以 `operator<<` 不能是 `Complex` 的成员函数。
   
2. **`operator>>` 不能是 `const` 成员函数**
   - 因为 `operator>>` 需要修改对象的值，因此不能加 `const`。

#### **3.2 代码**
```cpp
std::ostream& operator<<(std::ostream& os, const Complex& rhs) {
    if (rhs._imag > 0) {
        os << rhs._real << " + " << rhs._imag << "i";
    } else if (rhs._imag == 0) {
        os << rhs._real;
    } else {
        os << rhs._real << " - " << -rhs._imag << "i";
    }
    return os;
}

std::istream& operator>>(std::istream& is, Complex& rhs) {
    std::cout << "请输入复数的实部和虚部：" << std::endl;
    is >> rhs._real >> rhs._imag;
    return is;
}
```

#### **3.3 关键问题**
- **为什么 `operator<<` 和 `operator>>` 的返回值是 `std::ostream&` 和 `std::istream&` ？**  
  - 这样可以实现**连续输入输出**：
    ```cpp
    cout << c1 << c2 << endl;  // 连续输出
    cin >> c1 >> c2;           // 连续输入
    ```

- **为什么 `operator<<` 的 `ostream&` 参数不能去掉 `&` ？**  
  
  - 因为 `ostream` 的拷贝构造函数已被 `delete`，不能复制 `ostream` 对象。

#### **3.4 完整代码**

```cpp
#include <iostream>
#include <limits.h>
#include <ostream>

using namespace std;

//复数
class Complex
{
	friend Complex operator*(const Complex& lhs, const Complex& rhs);
	//Complex operator/(const Complex& rhs) const;
public:
	Complex(double r = 0, double i = 0)
		: _real(r), _imag(i)
	{
		cout << "Complex(double r = 0, double i = 0)" << endl;
	}

	~Complex()
	{
		cout << "~Complex()" << endl;
	}

	

	void display() const
	{

		if (_imag > 0)
		{
			cout << _real << " + " << _imag << "i" << endl;
		}
		else if (_imag == 0)
		{
			cout << _real << endl;
		}
		else
		{
			cout << _real << " - " << -_imag << "i" << endl;
		}
	}

	//成员函数，operator输出运算符重载 
	//cout << c1 << endl; 
	//第一个参数cout ,第二个参数c1
	//std::ostream& operator<<(std::ostream& os, const Complex& rhs); //error！隐含this指针

	//对于输出流运算符函数而言，不能写成成员函数的形式，因为违背了运算符重载的原则，不能改变操作数的顺序
	//std::ostream& operator<<(std::ostream& os);//error！this指针在参数列表的第一个位置

	

		/*友元函数可以放在类的 任何位置（public / protected / private），不影响其功能。
		从代码规范角度，建议统一放在类定义的开头或结尾，以提高可读性。
		友元关系是单向的，且不具备传递性（即类 A 的友元函数不会自动成为类 B 的友元）。
		*/
	friend std::ostream& operator<<(std::ostream& os, const Complex& rhs);
	//输入流运算符重载
	friend std::istream& operator>>(std::istream& is, Complex& rhs);

private:
	double _real;
	double _imag;
};
//问题1 ：参数列表中ostrream的引用符号&能不能去掉？
//解答1 ：不能去掉因为形参"os"和实参"cout"结合的时候会满足拷贝构造函数的调用时机，但是ostream中的拷贝构造函数已被delete

//问题2 ： 函数返回类型中的引用符号&能不能删除？
//解答2 ： 不能取掉，因为return os，返回类型满足拷贝构造函数的调用时机3

//注释：basic_ifstream( const basic_ifstream& rhs ) = delete; (7)	(since C++11)
//	   basic_ofstream( const basic_ofstream& rhs ) = delete; (7)	(since C++11)
//ifstram 和 ofstream 的拷贝构造函数已经从C++11开始删除了
std::ostream& operator<<(std::ostream& os, const Complex& rhs)
{
	cout << "std::ostream& operator<<(std::ostream& os, const Complex& rhs)" << endl;

	if (rhs._imag > 0)
	{
		os << rhs._real << " + " << rhs._imag << "i" << endl;
	}
	else if (rhs._imag == 0)
	{
		os << rhs._real << endl;
	}
	else
	{
		os << rhs._real << " - " << -rhs._imag << "i" << endl;
	}

	return os;
}

void readDouble(std::istream& is, double& rhs)
{
	while (is >> rhs, !is.eof())
	{
		if (is.bad())
		{
			std::cerr << "istream is bad" << endl;
			return;
		}
		else if (is.fail())
		{
			is.clear();//重置流的状态
			is.ignore(std::numeric_limits<::std::streamsize>::max(), '\n');//清空缓冲区
			cout << "请注意：需要输入double类型的数据！";
		}
		else
		{
			cout << "rhs = " << rhs << endl;
			break;
		}
	}
}

//输入流运算符重载
std::istream& operator>>(std::istream& is, Complex& rhs)//因为要修改rhs 的值，所以不能加const
{
	cout << "std::istream& operator>>(std::istream& is, Complex& rhs)" << endl;
	cout << "请分别输入复数的实部和虚部：" << endl;
	//is >> rhs._real >> rhs._imag;
	readDouble(is, rhs._real);
	readDouble(is, rhs._imag);
	return is;
}


void testOutputOperator()
{
	Complex c1(1, 2);
	cout << "c1 = ";
	c1.display();
	cout << endl << endl;


	//cout << "c1 = " << c1.display();
	//为什么没有做输出流运算符重载之前上面的写法 不可行呢？ 解答：二元“<<”: 没有找到接受“void”类型的右操作数的运算符(或没有可接受的转换)

	cout << "c1 = " << c1 << endl;

	cout << endl;
	Complex c2;
	cin >> c2;
	cout << "c2 = " << c2 << endl;
}

int main(int argc, char* argv[])
{
	testOutputOperator();
	return 0;
}
```
---

### **4. 下标访问运算符 `operator[]`**
#### **4.1 关键知识点**
- `operator[]` 主要用于自定义数组类型，使得 `obj[idx]` 访问数组元素。
- 返回值应为 `T&`，以保证能够修改数组内容。
- 必须进行越界检查，以防止访问非法内存。

#### **4.2 代码**
```cpp
char& CharArrar::operator[](size_t idx) {
    if (idx < _size) {
        return _data[idx];
    } else {
        static char charNull = '\0';
        return charNull;
    }
}
```

---

### **5. 函数调用运算符 `operator()`**
#### **5.1 关键知识点**
- 使对象像函数一样调用，称为**仿函数（函数对象）**。
- 可存储状态，例如调用次数。

#### **5.2 代码**
```cpp
class FunctionObject {
public:
    int operator()(int x, int y) {
        ++_cnt;
        return x + y;
    }

    int operator()(int x, int y, int z) {
        ++_cnt;
        return x * y * z;
    }

private:
    int _cnt = 0;
};
```

#### **5.3 示例**
```cpp
FunctionObject fo;
cout << "fo(3, 4) = " << fo(3, 4) << endl;
cout << "fo(3, 4, 5) = " << fo(3, 4, 5) << endl;
```

#### **5.4 完整代码**

```cpp
//bracket.h
#pragma once
#include <iostream>
#include <string.h>

using namespace std;
class CharArrar
{
public:
	CharArrar(size_t sz = 10)
		:_size(sz)
		, _data(new char[_size])
	{
		cout << " CharArrar(size_t sz = 10)" << endl;
	}

	~CharArrar()
	{
		cout << "~CharArrar()" << endl;
		if (_data)
		{
			delete _data;
			_data = nullptr;
		}
	}

	size_t size() const
	{
		return _size;
	}

	//下标访问运算符的重载
	//int arr[10] = { 1,2,3,4,5 };
	//arr.operator[](idx);
	char& operator[](size_t idx);
private:
	size_t _size;
	char* _data;
};

```
---

```cpp
//bracket.cpp
#include "bracket.h"

/*
要修复错误 C2106: “=”: 左操作数必须为左值，需要确保 CharArrar 类的
下标运算符 operator[] 返回一个可修改的左值。当前的 operator[] 声明
返回的是一个 char，这不是一个可修改的左值。我们需要将其修改为返回一个 char&。
*/
char& CharArrar::operator[](size_t idx)
{
	if (idx < _size)
	{
		return _data[idx];
	}
	else
	{
		static char charNUll = '\0';//静态变量延长生命周期
		return charNUll;   //使得返回的实体生命周期比函数的生命周期长
	}
}

//C++中优势：重载的下标访问运算符考虑了越界的问题
//引用什么时候需要加上？
//1.如果返回类型是类型的时候，可以减少拷贝构造函数的执行
//2.有可能需要一个左值（来调用右操作数），而不是拷贝后的右值、
//3.cout << "111" << c1 << endl << 10 << 1 << endl,像这种情况下连续使用的时候可以加上引用 
```
---

```cpp
//parenthese.h
#pragma once
#include <iostream>

using namespace std;

class FunctionObject
{
public:
	int operator()(int x, int y);
	

	int operator()(int x, int y, int z);
	
private:
	int _cnt;//记录被调用的次数（函数对象状态）
};

```

---
```cpp
parenthese.cpp
#include <iostream>
#include "parenthese.h"

using namespace std;



int FunctionObject::operator()(int x, int y)
{
	++_cnt;
	cout << "int operator()(int x, int y)" << endl;
	return x + y;
}

int FunctionObject::operator()(int x, int y, int z)
{
	cout << "int operator()(int x, int y,int z)" << endl;
	++_cnt;
	return x * y * z;
}

```

---
```cpp
//main.cpp
#include <iostream>
#include "bracket.h"
#include "parenthese.h"


using namespace std;

int add(int x, int y)
{
	cout << "int add(int x,int y)" << endl;
	static int cnt = 0;
	++cnt;
	return x + y;
}


void testFunctionObject()
{
	FunctionObject fo;
	int a = 3;
	int b = 4;
	int c = 5;
	//fo本质上是一个对象，但是他的使用
	cout << "fo(a,b) = " << fo(a, b) << endl;
	cout << "fo(a, b ,c) = " << fo(a, b, c) << endl;

	cout << endl;
	//正经的函数
	cout << "add(a, b) = " << add(a, b) << endl;
}

void testCharArrar()
{
	
	//把字符串中的内容拷贝到CharArray
	const  char* pstr = "hello cpp";
	CharArrar ca(strlen(pstr) + 1);

	for(size_t idx = 0; idx != ca.size(); ++idx)
	{
		//ca[idx] = pstr[idx];
		//上面和下面两条代码等价
		ca.operator[](idx) = pstr[idx];
	}

	for (size_t idx = 0; idx != ca.size(); ++idx)
	{
		cout << ca[idx] << "  ";
	}
	cout << endl;
}

int main(int argc, char** argv)
{
	testFunctionObject();

	testCharArrar();
	return  0;
}
```

---

## **Day6-2 成员访问运算符重载（2025.03.25）**

### **1. 复习**
在上一节中，我们学习了 C++ 中的 **输入输出流运算符重载（`<<` 和 `>>`）** 以及 **下标运算符 `[]` 和函数调用运算符 `()`** 的重载。本节我们将重点学习 **成员访问运算符（`->` 和 `*`）的重载**。

---

### **2. 成员访问运算符重载**
C++ 允许用户自定义类的成员访问方式，其中 `->`（箭头运算符）和 `*`（解引用运算符）是最常见的运算符之一。它们通常用于模拟智能指针或多层指针访问。

#### **2.1 箭头运算符 (`->`) 重载**
#### **(1) 语法**
```cpp
class A {
public:
    B* operator->();
};
```
**作用**：  
- 允许 `A` 类的对象 **像指针一样访问 `B` 类的成员**。
- 典型用途是在 **封装指针** 的类（如智能指针）中重载 `operator->`，使得用户无需手动解引用即可访问目标对象的成员。

**注意事项：**
1. **返回值必须是一个指针或者引用**，否则无法继续访问成员变量或函数。
2. **如果返回的是对象的引用**，则可以实现多层 `->` 重载。
3. `operator->` **不能改变调用对象自身**，因此通常 **不应该声明为 `const` 成员函数**。

---

#### **2.2 解引用运算符 (`*`) 重载**
#### **(1) 语法**
```cpp
class A {
public:
    B& operator*();
};
```
**作用**：
- 允许 `A` 类的对象 **像指针一样进行解引用**。
- 常见于智能指针的实现，使 `*ptr` 直接返回对象的引用，方便访问其成员。

**注意事项：**
1. **返回值一般是引用**（如 `B&`），这样不会产生额外的拷贝。
2. 适用于 **封装指针** 的类，如智能指针或代理类。

---

### **3. 代码分析**

```cpp
#include <iostream> 
using namespace std;

class Data
{
public:
	Data(int data = 0)
		:_data(data)
	{
		cout << "Data(int data = 0)" << endl;
	}

	~Data()
	{
		cout << "~Data()" << endl; 
	}

	int getData() const
	{
		return _data;
	}
private:
	int _data;
};

class SecondLayer
{
public:
	SecondLayer(Data* pData)
	:_pData(pData)
	{
		cout << "SecondLayer(Data* pData)" << endl;
	}

	//重载 -> 运算符
	Data* operator->()
	{
		return _pData;
	}

	//解引用重载运算符
	Data& operator*()
	{
		return *_pData;
	}


	~SecondLayer()
	{
		cout << "~SecondLayer()" << endl;
		if (_pData)
		{
			delete _pData;
			_pData = nullptr;
		}
	}


private:
	Data* _pData;
};

class ThirdLayer
{
public:
	ThirdLayer(SecondLayer* pSecond)
		:_pSecond(pSecond)
	{
		cout << "ThirdLayer(SecondLayer* pSecond)" << endl;
	}

	//重载 -> 运算符
	SecondLayer& operator->() 
	{
		return *_pSecond;
	}

	~ThirdLayer()
	{
		cout << "~ThirdLayer()" << endl;
		if (_pSecond)
		{
			delete _pSecond;
			_pSecond = nullptr;
		}
	}

private:
	SecondLayer* _pSecond;
};


void test()
{
	/*Data* data = new Data(1);
	SecondLayer* second = new SecondLayer(data);
	ThirdLayer* third = new ThirdLayer(second);*/

	SecondLayer second(new Data(10));//栈对象
	//A类的对象调用B类的成员函数
	/*cout << "&second : " << &second << endl;
	cout << "second.operator->() :" << second.operator->() << endl;*/

	//  重载operator->  
	cout << "second.operator->()->getData() :" << second.operator->()->getData() << endl;
	cout << "second->getData() :" << second->getData() << endl;

	//  重载operator* 
	cout << "(*second).getData()" << (*second).getData() << endl;


	ThirdLayer third(new SecondLayer(new Data(30)));//栈对象
	cout << "third->getData() : " << third->getData() << endl;
	//还原
	cout << "third.operator->().operator->()->getData()" << third.operator->().operator->()->getData();
}

int main(int argc, char** argv)
{
	test();
	test();
	return 0;
}
```

#### **3.1 代码结构**
上面的代码实现了一个 **三层封装** 的指针代理类，分别是：
- `Data`：数据类，包含一个 `_data` 成员变量。
- `SecondLayer`：封装 `Data*` 指针，并重载 `operator->` 和 `operator*`。
- `ThirdLayer`：封装 `SecondLayer*` 指针，并重载 `operator->`。

---

#### **3.2 代码解析**
#### **(1) `Data` 类**
```cpp
class Data
{
public:
    Data(int data = 0) : _data(data)
    {
        cout << "Data(int data = 0)" << endl;
    }

    ~Data()
    {
        cout << "~Data()" << endl;
    }

    int getData() const
    {
        return _data;
    }
private:
    int _data;
};
```
- `Data` 类封装了一个 `int` 类型的数据 `_data`，提供了 `getData()` 方法用于获取数据值。
- 构造函数、析构函数用于跟踪对象的创建和销毁。

---

#### **(2) `SecondLayer` 类**
```cpp
class SecondLayer
{
public:
    SecondLayer(Data* pData) : _pData(pData)
    {
        cout << "SecondLayer(Data* pData)" << endl;
    }

    // 重载 -> 运算符
    Data* operator->()
    {
        return _pData;
    }

    // 解引用运算符 *
    Data& operator*()
    {
        return *_pData;
    }

    ~SecondLayer()
    {
        cout << "~SecondLayer()" << endl;
        if (_pData)
        {
            delete _pData;
            _pData = nullptr;
        }
    }

private:
    Data* _pData;
};
```
- **封装 `Data*` 指针，并提供访问 `Data` 成员的方式**。
- `operator->()` 返回 `_pData` 指针，使得 `SecondLayer` 对象 **可以像指针一样使用 `->` 访问 `Data` 的方法**。
- `operator*()` 返回 `_pData` 所指向的 `Data` 对象的引用，使 `*second` 直接返回 `Data` 对象。

**示例：**
```cpp
SecondLayer second(new Data(10));
cout << second->getData() << endl;  // 等价于 second.operator->()->getData()
cout << (*second).getData() << endl; // 等价于 second.operator*().getData()
```

---

#### **(3) `ThirdLayer` 类**
```cpp
class ThirdLayer
{
public:
    ThirdLayer(SecondLayer* pSecond) : _pSecond(pSecond)
    {
        cout << "ThirdLayer(SecondLayer* pSecond)" << endl;
    }

    // 重载 -> 运算符
    SecondLayer& operator->()
    {
        return *_pSecond;
    }

    ~ThirdLayer()
    {
        cout << "~ThirdLayer()" << endl;
        if (_pSecond)
        {
            delete _pSecond;
            _pSecond = nullptr;
        }
    }

private:
    SecondLayer* _pSecond;
};
```
- `ThirdLayer` 封装了 `SecondLayer*` 指针，并提供 `operator->()` 使其 **可以像 `SecondLayer` 一样使用 `->` 访问 `Data` 的方法**。
- **实现两层 `->` 重载**，使得 `ThirdLayer` 可以连续访问 `Data` 成员。

**示例：**
```cpp
ThirdLayer third(new SecondLayer(new Data(30)));
cout << third->getData() << endl;  // 等价于 third.operator->().operator->()->getData()
```

---

#### **3.3 运行 `test()` 方法**
```cpp
void test()
{
    SecondLayer second(new Data(10));
    cout << "second->getData() :" << second->getData() << endl;
    cout << "(*second).getData()" << (*second).getData() << endl;

    ThirdLayer third(new SecondLayer(new Data(30)));
    cout << "third->getData() : " << third->getData() << endl;
    cout << "third.operator->().operator->()->getData() : " << third.operator->().operator->()->getData();
}
```
**输出：**
```
Data(int data = 0)
SecondLayer(Data* pData)
second->getData() : 10
(*second).getData() : 10
Data(int data = 0)
SecondLayer(Data* pData)
ThirdLayer(SecondLayer* pSecond)
third->getData() : 30
third.operator->().operator->()->getData() : 30
~ThirdLayer()
~SecondLayer()
~Data()
~SecondLayer()
~Data()
```
- `SecondLayer` 允许访问 `Data` 对象。
- `ThirdLayer` 允许访问 `SecondLayer`，最终可访问 `Data`。
- **多层指针访问的代理模式生效**，并且析构时正确释放了内存。

---

### **4. 总结**
| 运算符 | 作用 | 适用场景 | 返回值类型 |
|--------|------|---------|------------|
| `operator->()` | 允许对象像指针一样访问成员 | 智能指针、代理类 | 指针或引用 |
| `operator*()` | 允许对象像指针一样解引用 | 智能指针、代理类 | 引用 |

**关键点：**
1. **`operator->()` 需要返回指针或引用**，可以连续调用 `->`。
2. **`operator*()` 需要返回对象的引用**，避免拷贝，提高性能。
3. **适用于封装指针的类，如智能指针和代理类**。

本节学习了 **成员访问运算符 `->` 和 `*` 的重载**，掌握它们的用法可以更好地理解 **智能指针** 和 **代理模式**。

---

## **Day6-3 类型转换函数和类域**

### **1. 类型转换函数（Type Conversion Functions）**

#### **1.1 概述**
**类型转换函数**用于将类的对象转换为其他类型，满足以下特点：
- 是**类的成员函数**，不能是友元函数或非成员函数。
- **没有参数列表**（即必须是 `()` 空参数）。
- **没有返回类型声明**，即不能显式声明 `void` 或其他返回类型。
- **使用 `return` 语句返回目标类型的变量**。

#### **1.2 代码示例**
```cpp
#include <iostream>

using namespace std;

class Complex
{
    friend std::ostream& operator<<(std::ostream& os, const Complex& rhs);

public:
    Complex(int real = 0, int imag = 0)  
        : _real(real), _imag(imag)
    {
        cout << "Complex(int, int) 构造函数" << endl;
    }

    ~Complex()
    {
        cout << "~Complex 析构函数" << endl;
    }

    void display() const
    {
        cout << _real << " + " << _imag << "i" << endl;
    }

    int getReal() const { return _real; }
    int getImag() const { return _imag; }

private:
    int _real;
    int _imag;
};

// 重载输出运算符，使 Complex 可以被 cout 直接输出
std::ostream& operator<<(std::ostream& os, const Complex& rhs)
{
    os << rhs._real << " + " << rhs._imag << "i";
    return os;
}

class Point
{
    friend std::ostream& operator<<(std::ostream& os, const Point& rhs);

public:
    Point(int x = 0, int y = 0) : _x(x), _y(y)
    {
        cout << "Point(int, int) 构造函数" << endl;
    }

    // **类型转换函数**
    operator int() const
    {
        cout << "operator int()" << endl;
        return _x + _y;
    }

    operator double() const
    {
        cout << "operator double()" << endl;
        return _y == 0 ? 0.0 : static_cast<double>(_x) / _y;
    }

    operator Complex() const
    {
        cout << "operator Complex()" << endl;
        return Complex(_x, _y);
    }

    ~Point()
    {
        cout << "~Point 析构函数" << endl;
    }

    // **类型转换构造函数**
    explicit Point(const Complex& rhs)
        : _x(rhs.getReal()), _y(rhs.getImag())
    {
        cout << "Point(const Complex&) 类型转换构造函数" << endl;
    }

    void print() const
    {
        cout << "(" << _x << ", " << _y << ")" << endl;
    }

private:
    int _x;
    int _y;
};

// 重载输出运算符，使 Point 可以被 cout 直接输出
std::ostream& operator<<(std::ostream& os, const Point& rhs)
{
    os << "(" << rhs._x << ", " << rhs._y << ")";
    return os;
}

void testTypeConversion()
{
    Point pt(3, 4);
    cout << "Point pt: " << pt << endl;

    int intValue = pt;  
    cout << "转换为 int: " << intValue << endl;

    double doubleValue = pt;
    cout << "转换为 double: " << doubleValue << endl;

    Complex comValue = pt;
    cout << "转换为 Complex: " << comValue << endl;
}

int main()
{
    testTypeConversion();
    return 0;
}
```

#### **1.3 关键优化**
✅ **类型转换构造函数**增加 `explicit`，防止隐式转换导致潜在错误。  
✅ **类型转换函数**添加 `const`，保证 `Point` 的成员变量不会被修改。  
✅ **`operator double()`** 处理 `_y == 0` 情况，避免除零异常。  
✅ **优化 `Complex` 相关代码**，消除 `friend` 冗余，提高可读性。

---

### **2. 类域（Class Scope）**

#### **2.1 作用域 vs 可见域**
- **作用域（Scope）**：指变量在代码中的**可访问范围**，影响变量的解析方式。
- **可见域（Visibility）**：变量是否在当前上下文可见，受 `private`、`protected`、`public` 修饰符影响。
- **作用域 ≥ 可见域**，如果没有**命名冲突**，二者相等。

---

#### **2.2 代码示例**
```cpp
#include <iostream>

using namespace std;

int globalNumber = 50;  // **全局作用域变量**

namespace ceshi
{
    int namespaceNumber = 20;  // **命名空间作用域**

    class Test
    {
    public:
        Test(int value = 100) : number(value) {}

        void print(int number)
        {
            cout << "局部变量 number: " << number << endl;
            cout << "成员变量 number: " << this->number << endl;
            cout << "类作用域 number: " << Test::number << endl;
            cout << "命名空间 ceshi::number: " << ceshi::namespaceNumber << endl;
            cout << "全局变量 globalNumber: " << ::globalNumber << endl;
        }

    private:
        int number;  // **类成员变量**
    };
} // namespace ceshi

void testScope()
{
    int localValue = 3000;
    ceshi::Test obj;
    obj.print(localValue);
}

int main()
{
    testScope();
    return 0;
}
```

#### **2.3 关键优化**
✅ **清晰划分不同作用域**：全局作用域 `::`、命名空间 `ceshi::`、类作用域 `this->` 和 `Test::`。  
✅ **增强 `print()` 方法输出格式**，更直观理解变量的作用域。  
✅ **消除不必要的析构函数**，提升代码简洁性。  
✅ **代码格式调整**，增强可读性。

---

### **3. 内部类（Nested Class）**

**内部类**（Nested Class）是指在一个类的定义内部再定义一个类。内部类在逻辑上属于外部类的一部分，但具有自己的作用域。

#### **3.1 内部类的定义**
```cpp
#include <iostream>
using namespace std;

class Outer {
public:
    class Inner { // 内部类
    public:
        void display() {
            cout << "This is Inner class" << endl;
        }
    };
};

void test() {
    Outer::Inner obj; // 创建内部类对象
    obj.display();
}

int main() {
    test();
    return 0;
}
```
**特点：**
1. 内部类的作用域受外部类的限制。
2. 内部类的对象可以通过 `外部类::内部类` 的方式创建。
3. 内部类可以访问外部类的 `public` 和 `protected` 成员，但不能直接访问 `private` 成员（除非使用 `friend` 关键字）。

#### **3.2 访问外部类的 `private` 成员**
如果内部类需要访问外部类的 `private` 成员，可以将其声明为 `friend`。
```cpp
class Outer {
private:
    int _data = 10;

public:
    class Inner {
    public:
        void display(Outer& obj) {
            cout << "Outer data: " << obj._data << endl;
        }
    };
};

void test2() {
    Outer outerObj;
    Outer::Inner innerObj;
    innerObj.display(outerObj);
}
```

#### **3.3 静态内部类**
静态内部类不能访问外部类的非静态成员。
```cpp
class Outer {
public:
    static class StaticInner {
    public:
        void show() {
            cout << "This is a static inner class" << endl;
        }
    };
};

void test3() {
    Outer::StaticInner obj;
    obj.show();
}
```

---

### **4. 作用域运算符（Scope Resolution Operator `::`）**
作用域运算符 `::` 用于指定变量、函数或类所属的作用域，主要应用如下：

#### **4.1 访问全局变量**
```cpp
#include <iostream>
using namespace std;

int number = 100;

void test() {
    int number = 50;
    cout << "Local number: " << number << endl;
    cout << "Global number: " << ::number << endl;
}
```

#### **4.2 访问命名空间内的变量**
```cpp
namespace A {
    int value = 10;
}

namespace B {
    int value = 20;
}

void test2() {
    cout << "A::value = " << A::value << endl;
    cout << "B::value = " << B::value << endl;
}
```

#### **4.3 访问类的静态成员**
```cpp
class Demo {
public:
    static int data;
};

int Demo::data = 50;

void test3() {
    cout << "Static data: " << Demo::data << endl;
}
```

---

### **5. 类型转换运算符（Type Casting Operators）**
C++ 提供了四种类型转换运算符：
1. `static_cast`：用于基本类型之间的转换。
2. `dynamic_cast`：用于多态类型的安全转换。
3. `const_cast`：用于去除 `const` 或 `volatile` 修饰符。
4. `reinterpret_cast`：用于不同类型指针之间的转换。

#### **5.1 `static_cast` 用法**
```cpp
void test_static_cast() {
    double num = 10.5;
    int intNum = static_cast<int>(num);
    cout << "Converted value: " << intNum << endl;
}
```

#### **5.2 `dynamic_cast` 用法**
```cpp
class Base {
public:
    virtual void show() {}
};

class Derived : public Base {
public:
    void show() {
        cout << "Derived class" << endl;
    }
};

void test_dynamic_cast() {
    Base* basePtr = new Derived();
    Derived* derivedPtr = dynamic_cast<Derived*>(basePtr);
    if (derivedPtr) {
        derivedPtr->show();
    }
    delete basePtr;
}
```

#### **5.3 `const_cast` 用法**
```cpp
void test_const_cast() {
    const int num = 42;
    int* ptr = const_cast<int*>(&num);
    *ptr = 10;
    cout << "Modified value: " << *ptr << endl;
}
```

#### **5.4 `reinterpret_cast` 用法**
```cpp
void test_reinterpret_cast() {
    int num = 65;
    char* ch = reinterpret_cast<char*>(&num);
    cout << "Reinterpreted value: " << *ch << endl;
}
```

---

### **6. 关键字 `explicit` 与类型转换**
默认情况下，C++ 允许构造函数进行隐式类型转换，但 `explicit` 关键字可以禁止这种行为。

#### **6.1 隐式转换**
```cpp
class Demo {
public:
    Demo(int x) { cout << "Demo(int)" << endl; }
};

void test_implicit_conversion() {
    Demo obj = 10; // 隐式调用 Demo(int)
}
```

#### **6.2 使用 `explicit` 禁止隐式转换**
```cpp
class Demo {
public:
    explicit Demo(int x) { cout << "Demo(int)" << endl; }
};

void test_explicit() {
    // Demo obj = 10; // 错误！隐式转换被禁止
    Demo obj(10);    // 正确
}
```

---

### **7. 总结**
1. **内部类** 是定义在另一个类内部的类，具有自己的作用域，并且可以是 `static`。
2. **作用域运算符 `::`** 用于访问不同作用域的变量或函数。
3. **类型转换运算符** 提供更安全、可控的转换方式。
4. **`explicit` 关键字** 可以防止构造函数的隐式转换。

---
## **Day7-1 智能指针雏形：独占语义与共享语义**

### **1. 独占语义与共享语义**

在 C++ 中，拷贝构造的语义可以分为**独占语义**和**共享语义**。

- **独占语义（Unique Ownership）**：每个对象拥有一块独立的内存，拷贝时进行**深拷贝**。
- **共享语义（Shared Ownership）**：多个对象共享同一块内存，拷贝时进行**浅拷贝**，通常需要**引用计数**来管理资源。

C++ 标准库提供了 `std::unique_ptr` 和 `std::shared_ptr` 来实现这两种语义。

#### **1.1 Circle 类：示例类**

`Circle` 类是一个普通的对象类，包含构造函数、析构函数、成员变量和方法。

```cpp
// Circle.h
#pragma once
#include <iostream>
#include <cmath>
using namespace std;

//关键字
//const
const int global_a = 10;
//static
static int s_b = 10;
//extern
extern int SIZE = 10;
//三者的用途：const 修饰常量，static 修饰全局变量，extern 修饰外部变量

class Circle
{
public:
    Circle(double r = 0)
        : _r(r)
    {
        cout << "Circle(double r = 0)" << endl;
    }

    Circle(double r, char* name)
        : _r(0), _name(new char[strlen(name) + 1])
    {
        strcpy_s(_name, strlen(name) + 1, name);
        cout << "Circle(double r, char* name)" << endl;
    }

    ~Circle()
    {
        cout << "~Circle()" << endl;
        delete[] _name;
    }

    double getRadius() const { return _r; }
    void setRadius(double r) { _r = r; }
    double getArea() const { return M_PI * _r * _r; }

private:
    double _r;
    char* _name;
};
```

------

### **2. 拷贝构造：独占语义（Unique Ownership）**

`UniqueClass` 实现了独占语义，即对象的拷贝会创建新的独立对象，保证每个对象都拥有独立的内存。

```cpp
// ShareClass.h
class UniqueClass
{
    int times = 0;
public:
    UniqueClass(int a)
        : _data(new int(a))
    {
    }
    
    ~UniqueClass()
    {
        delete _data;
        _data = nullptr;
    }

    // 深拷贝构造函数
    //UniqueClass(const UniqueClass& rhs)
	//	：_data(new int(*rhs.data))
	//{
	//	cout << "Call desctructor";
	//	times++;
	//	cout << times << endl;
	//}


	//在这段代码中，rhs 不是指针，而是一个 UniqueClass 类型的引用。
	// *rhs._data 是对 rhs 对象的 _data 成员指针进行解引用。
	UniqueClass(const UniqueClass& rhs)
		: _data(new int(*rhs._data)) // 修复了错误的冒号和成员变量名称
	{
		cout << "Call constructor";
		times++;
		cout << times << endl;
	}

	  /*
		1.	UniqueClass(const UniqueClass & rhs) 是 UniqueClass 的拷贝构造函数。它接受一个 UniqueClass 类型的常量引用 rhs 作为参数。
		2.	: _data(new int(*rhs._data)) 是成员初始化列表的一部分。它的作用是初始化 _data 成员变量。
		3.	rhs._data 是对 rhs 对象的 _data 成员指针的访问。
		4. * rhs._data 是对 rhs._data 指针的解引用，获取指针指向的整数值。
		5.	new int(*rhs._data) 创建了一个新的 int 对象，并将 * rhs._data 的值复制到这个新的 int 对象中。然后将新创建的 int 对象的指针赋值给 _data 成员变量。
		这样做的目的是在拷贝构造函数中为新对象分配一个新的内存空间，并将原对象的 _data 成员
		指针指向的值复制到新对象的 _data 成员指针指向的内存中。这确保了每个 UniqueClass 对象
		都有自己独立的 _data 内存空间，避免了多个对象共享同一块内存，从而防止潜在的内存管理问题。
		*/


	/*传入的参数可以修改，修改后不可以再使用，最好当成右值使用*/
	//移动构造函数的写法(C++11 移动构造函数，接受右值)
		// 拷贝指针，并断掉源对象对内存的引用（置空）
		// 如果传入参数本身是右值，没有任何副作用，因为调用结束，它就不在了
		// 但传入的如果是个左值转换来的右值，那要注意，充当了右值，右值的语义就是临时的，
		// 转瞬即时的，所以这个左值也应看成已逝的事物，不可以再使用
	UniqueClass(UniqueClass&& rhs)
		:_data(rhs._data)
	{
		rhs._data = nullptr;
	}

	// C++11 移动赋值运算符，接受右值，和移动构造同理
	UniqueClass& operator=(UniqueClass&& rhs)
	{
		_data = rhs._data;
		rhs._data = nullptr;
	}

private:
    int* _data;
};
```

#### **2.1 代码解析**

- **拷贝构造函数**：创建新对象时，为 `_data` 申请新的内存，保证对象的独占性。
- **移动构造函数**：从右值拷贝指针，并将原对象的指针置空，避免重复释放内存。
- **移动赋值运算符**：清理当前对象的内存，然后从右值拷贝指针，并清空右值的指针。

------

### **3. 拷贝构造：共享语义（Shared Ownership）**

`SharedClass` 采用引用计数管理共享的内存。

```cpp
class SharedClass
{
public:
    static int count; // 静态引用计数
    int* _data;

    SharedClass(int a) : _data(new int(a)) {}
	// 拷贝构造，拷贝指针，增加引用计数
    SharedClass(const SharedClass& r)
        : _data(r._data)
    {
        count++;
    }
    
    // 拷贝赋值，拷贝指针，增加引用计数
    SharedClass& operator=(const SharedClass& r)
    {
        if (this != &r)
        {
            _data = r._data;
            count++;
        }
        return *this;
    }

    // 析构时注意减少引用计数，归0时释放堆内存
    ~SharedClass()
    {
        --count;
        if (count == 0)
        {
            delete _data;
        }
    }
};

int SharedClass::count = 1;
```

#### **3.1 代码解析**

- **引用计数 `count`**：确保多个对象共享同一块内存。
- **拷贝构造函数**：增加引用计数。
- **析构函数**：当 `count == 0` 时，释放 `_data`。

------

### **4. 智能指针 std::unique_ptr 和 std::shared_ptr**

C++ 提供 `std::unique_ptr` 和 `std::shared_ptr`，分别对应独占语义和共享语义。

```cpp
void testSmartPointer()
{
    std::unique_ptr<int> unique1 = std::make_unique<int>(10);
    // std::unique_ptr<int> unique2 = unique1; // 错误：无法复制 unique_ptr

    std::shared_ptr<int> share1 = std::make_shared<int>(10);
    std::shared_ptr<int> share2 = share1;
    cout << "share2 use_count() = " << share2.use_count() << endl;
}
```

#### **4.1 代码解析**

- `std::unique_ptr` **禁止拷贝**，确保对象独占内存。
- `std::shared_ptr` **可以拷贝**，使用引用计数管理内存。

------

### **5. 移动语义（Move Semantics）**

```cpp
void testMoveSemantic()
{
	UniqueClass u1(10);
	cout << "u1.data = " << u1._data << endl; 
	cout << "*(u1)._data = " << * (u1)._data << endl; 

	//UniqueClass u2 = (UniqueClass&&)u1;
	//等价于
	UniqueClass u2 = std::move(u1);// move将uc1这个左值转为右值，即(UniqueClass&&)uc1;
	cout << "u2.data = " << u2._data << endl;
	cout << "*(u2)._data = " << *(u2)._data << endl;


	if (u1._data = nullptr)
	{
		cout << "u1 is gone " << endl;
	}
}
```

#### **5.1 代码解析**

- `std::move(u1)` 将 `u1` 变为右值，调用**移动构造函数**。
- `u1._data = nullptr`，原对象 `u1` 失效。

------

### **总结**

| 方式              | 语义      | 内存管理      | 适用场景             |
| ----------------- | --------- | ------------- | -------------------- |
| `std::unique_ptr` | 独占      | 不能拷贝      | 资源独占，如文件句柄 |
| `std::shared_ptr` | 共享      | 计数管理      | 共享资源，如缓存     |
| 拷贝构造          | 独占/共享 | 深拷贝/浅拷贝 | 具体需求             |
| 移动构造          | 独占      | 资源转移      | 避免临时对象开销     |

通过这些方法，我们可以更高效地管理 C++ 中的资源分配和释放，避免内存泄漏和重复释放。

**深拷贝（独占）**

- 拷贝的是 **值**，拷贝后 **新对象与源对象完全独立**。
- 析构一方 **不影响** 另一方。

**浅拷贝（共享）**

- 拷贝的是 **指针/引用**，拷贝后 **新对象与源对象共享同一份内存**。
- 析构一方后 **另一方会受到影响**。

为了防止 **重复 delete**，需要引入 **引用计数**，只有当 **没有任何对象引用这块堆内存时，才释放它**。

`std::shared_ptr` 实现的思路：

`std::shared_ptr` 通过 **内部的引用计数** 记录有多少个 `shared_ptr` 共享同一块内存，只有当 **引用计数归零时，才释放资源**。

```cpp
#include <iostream>
#include <memory>

using namespace std;

void sharedPtrDemo()
{
	shared_ptr<int> p1 = make_shared<int>(10);
	{
		shared_ptr<int> p2 = p1;
		cout << "引用计数: " << p1.use_count() << endl;
	} // p2 作用域结束，引用计数减少
	cout << "引用计数: " << p1.use_count() << endl;
}
```



## **Day7-2 继承-基类和派生类**

### **一、继承**

#### **1、继承的基础**

面向对象的四大基本特征：**抽象、封装、继承、多态**。

- **继承**：从既有类（基类）产生新类（派生类）的过程。
- **基类（父类）**：已有的类。
- **派生类（子类）**：从基类继承并扩展的新类。

#### **2、继承的定义**

```C++
class 子类(派生类) 
: public/protected/private 父类(基类)   //类派生列表
{
    //数据成员
    //成员函数
};
```

#### **3. 派生类的构造过程**

1. **吸收** 基类的成员。
2. **改造** 基类的成员。
3. **新增** 自己的成员。

示例代码：

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    Base() : _base(0) { cout << "Base()" << endl; }
    ~Base() { cout << "~Base()" << endl; }
private:
    long _base;
};

class Derived : public Base {
public:
    //如果派生类显式定义了构造函数，而基类没有显式定义构造函数
	//则创建派生类的对象时，派生类相对应的构造函数会被自动调用，
	//此时都自动调用了基类缺省的构造函数
    Derived(long derived = 0) : _derived(derived) { cout << "Derived()" << endl; }
    ~Derived() { cout << "~Derived()" << endl; }
private:
    long _derived;
};

int main()
{
	//在创建派生类对象的时候，会先调用基类的构造函数，然后调用派生类的构造函数 ？？--> 
	//上面的说法是典型的错误，正确的如下：
	//在创建派生类对象的时候，会调用派生类自己的构造函数，但是为了完成从基类吸收过来的数据成员的初始化，所以才会调用基类的构造函数
	Derived d1(10);
	cout << "When creating a derived class object, the derived class's own constructor is called, "
		<<"but in order to complete the initialization of the data members absorbed from the base class,"
		<<" the base class's constructor is called" << endl;
	return 0;
}
```

**派生类对象的构造顺序：**

- 先调用**基类的构造函数**。
- 然后调用**派生类的构造函数**。

#### **4、继承的局限**

以下内容不会被继承：

- **基类的构造函数和析构函数**
- **基类的友元关系**
- **基类的 `operator new/delete` 操作符**

```cpp
#include <string>
#include <iostream>

/*
从 Circle 中提取一些公有的成员到基类 Shape 中
*/
class Shape
{
private: // 如须让子类可见，应使用 protected
  double _x;
  double _y;
  std::string _name;

public:
  Shape() : Shape(0.0, 0.0) {}
  Shape(double x, double y, const std::string& name = "")
    : _x(x), _y(y), _name(name) {}
  Shape(const std::string& name) : Shape(0.0, 0.0, name) {}
  ~Shape() = default;

  double x() const { return _x; }
  double y() const { return _y; }
  const std::string& name() const { return _name; }

  void setX(double v) { _x = v; }
  void setY(double v) { _y = v; }
};

const double M_PI = 3.141592653;

/*
一般使用公有继承，私有继承很少，保护继承从没见过
final 关键字可以作用在类名后面，令该类成为一个不可以被继承的类，它不允许有子类
*/
class Circle final: public Shape
{
private:
  double _radius;

public:
  // 子类的构造函数会先调用基类的构造函数，再调用自己的
  // 不管有没有显式调用基类的构造函数，编译器都会安插代码去调用基类的构造函数，这是完整性构造保证的
  Circle() : Circle(0.0, 0.0, 0.0) {}
  Circle(double x, double y, double radius, const std::string& name = "")
    : Shape(x, y, name), _radius(radius) {} // 显式调用基类的构造函数，然后继续初始化自己的成员
  explicit Circle(double radius) : Circle(0.0, 0.0, radius) {}
  // 析构和构造顺序相反，先析构自己的成员，再调用基类的构造函数
  ~Circle() = default;

  // 由于子类的构造函数会覆盖（隐藏）基类的构造函数
  // 比如由于没有定义 Circle(const std::string& name) 导致无法使用传入名字的构造函数
  // 可以使用 using 显式引入基类的构造函数，让所有基类的构造函数在子类可见
  // 普通函数也存在同名覆盖的问题，同样可以使用 using 引入
  // 但一般只用来引入构造函数，因为普通函数理论上应该通过继承，子类可以使用基类的普通函数
  // 如果子类中存在与基类的同名非虚函数，导致看不见基类的函数而失去对该函数的继承性，这是设计问题！应该修改名字而避免冲突
  using Shape::Shape;

  double radius() const { return _radius; }

  void setRadius(double v) { _radius = v; }

  double area() const { return M_PI * _radius * _radius; }
};

```

#### **5. 访问权限**

| 继承方式    | 基类 `public` | 基类 `protected` | 基类 `private` |
| ----------- | ------------- | ---------------- | -------------- |
| `public`    | `public`      | `protected`      | 不可访问       |
| `protected` | `protected`   | `protected`      | 不可访问       |
| `private`   | `private`     | `private`        | 不可访问       |

#### **6. `protected` 与 `private` 继承的区别**

1. `protected` 继承可以被后续子类继续继承，而 `private` 继承只能继承一代。
2. 默认继承方式：**类的继承方式默认是 `private`**。

#### **7. 完整示例代码：**

```cpp
#include <iostream>

using std::cout;
using std::endl;

class Point
{
public:
	Point(int ix = 0 , int iy = 0)
		: _ix(ix)
		, _iy(iy)
	{
		/*_ix = ix;
		_iy = iy;*/
		cout << "Point(int,int)" << endl;
	}
	~Point() {
		cout << "~Point()" << endl;
	}

	//拷贝构造函数
	Point(const Point& rhs)
		: _ix(rhs._ix)
		, _iy(rhs._iy)
	{
		cout << "Point(const Point&)" << endl;
	}
	//问题1：拷贝构造函数中的引用符号能不能去掉？
	//问题2：拷贝构造函数中的const关键字能不能去掉？

	//解答1：去掉引用符号，会导致无穷递归调用，直到栈溢出（满足拷贝构造函数的调用时机1：形参与实参结合）
	//解答2：去掉const关键字，会导致无法传递右值，即无法传递临时对象

	//左值和右值
	int number = 10;
	int& ref = number;//左值引用

	//int& ref1 = 10;//错误，不能将右值赋值给左值引用
	int&& ref2 = 10;//右值引用
	const int& ref3 = 10;//常量左值引用


	//赋值运算符函数
	Point& operator=(const Point& rhs)
	{
		cout << "Point& operator=(const Point&)" << endl;
		if (this != &rhs)
		{
			_ix = rhs._ix;
			_iy = rhs._iy;
		}
	}

	int getY() const
	{
		return _iy;
	}

	void print() const
	{
		cout << "(" << _ix
			<< "," << _iy
			<< ")" << endl;
	}

protected:
	int _ix;
private:
	
	int _iy;

};

//不管以什么继承方式，基类中的私有成员都不能在派生类中被访问,其他的都可以被访问
//不管以什么继承方式，派生类对象只能访问公有继承中的共有成员

class Point3D : protected Point
{
public:
	Point3D(int ix = 0,int iy = 0 ,int iz = 0)
		:Point(ix ,iy)
		,_iz(iz)
	{ 
		cout << "Point3D(int = 0,int = 0,int= 0)" << endl;
	}
	~Point3D()
	{
		cout << "~Point3D()" << endl;
	}

	void print() const
	{
		cout << "ix = " << _ix << endl
			<< "iy = " << getY() << endl
			<< "iz = " << _iz << endl;
	}
private:
	int _iz;
};

class Point4D : private Point3D
{
public:
	Point4D()
	{

	}
	~Point4D()
	{

	}

	void display() const
	{
		cout << "ix = " << _ix << endl
			<< "iy = " << getY() << endl
			/*<< "iz = " << _iz << endl*/
			<< "im = " << _im << endl;
	}

private:
	int _im;
};


void test() {
	Point pt;//栈对象
	cout << "pt = ";
	pt.print();


	Point3D pt3d(1, 2, 3);
	pt3d.print();
}


int main(int argc, char* argv[])
{
	test();
	return 0;
}
```


### **二、派生类对象的构造**

#### **1. 构造顺序**

1. **基类的构造函数先执行**。
2. **派生类的构造函数后执行**。
3. **如果基类没有显式构造函数，派生类默认调用基类的无参构造函数**。

#### **2. 析构顺序**

1. **派生类的析构函数先执行**。
2. **基类的析构函数后执行**。



总结：**当创建派生类对象的时候**，**会调用派生类的构造函数**，为了完成从基类吸收过来的数据成员的初始化，会调用基类的构造函数，而如果基类没有显示定义构造函数，就会调用基类的无参构造函数，而如果基类有显示定义构造函数，那么也会默认调用基类的无参构造函数，完成初始化，如果不在派生类的构造函数的初始化列表中显示调用基类有参构造函数，就会报错。



总结：当创建派生类对象的时候，不管基类与派生类有没有显示写出构造函数，肯定默认会调用无参的，有时无参又没有，所以最好在派生类构造函数的初始化列表中，显示写出基类构造函数就可以。



派生类构造函数的功能：（执行顺序）

1、完成对象所占整块内存的开辟，由系统在调用构造函数时自动完成。
2、调用基类的构造函数完成基类成员的初始化。
3、若派生类中含对象成员、const成员或引用成员，则必须在初始化表中完成其初始化。
4、派生类构造函数体执行  



### **三、派生类对象的销毁**

派生类对象销毁的时候，会执行派生类的析构函数，然后**基类析构函数会自动调用**。

析构函数执行顺序：

1、先调用派生类的析构函数
2、再调用派生类中成员对象的析构函数
3、最后调用普通基类的析构函数  

### **四、多继承**

#### **1. 多继承基础**

多继承：一个类可以继承多个基类。

示例代码：

```cpp
#include <iostream>
using namespace std;

class A {
public:
    A() { cout << "A()" << endl; }
    ~A() { cout << "~A()" << endl; }
};

class B {
public:
    B() { cout << "B()" << endl; }
    ~B() { cout << "~B()" << endl; }
};
//对于多继承而言，每个基类前面都要加上继承方式，否则就是默认继承方式，也就是私有继承

//多继承下，基类构造函数的执行顺序与其在派生类构造函数中的初始化顺序没有关系
//只与基类被继承的顺序有关
class C : public A, public B {
public:
    C() { cout << "C()" << endl; }
    ~C() { cout << "~C()" << endl; }
};

int main() {
    C obj;
    return 0;
}
```

**输出：**

```
A()
B()
C()
~C()
~B()
~A()
```

**多继承的顺序：**

- **构造时：按照继承顺序（从左到右）调用基类构造函数。**
- **析构时：按照继承顺序的相反方向调用基类析构函数。**

#### **2. 多继承的二义性问题**

如果多个基类有相同的成员，派生类会出现二义性。

解决方案：

1. **作用域限定符**
2. **虚拟继承（`virtual` 关键字）**

示例：

```cpp
class A {
public:
    void show() { cout << "A::show()" << endl; }
};
class B {
public:
    void show() { cout << "B::show()" << endl; }
};
class C : public A, public B {
};
int main() {
    C obj;
    // obj.show(); // 错误，二义性
    //多继承的问题1：成员函数访问冲突
	//解决方式：使用类型+作用域限定符::
    obj.A::show(); // 解决方案1
    obj.B::show(); // 解决方案1
    
    //多继承的问题2:数据成员的存储二义性
	//解决办法：让派生类通过虚拟继承的方法去继承基类
    return 0;
}
```

##### **1、成员函数访问冲突  **

解决方案：使用类名 + 作用域限定符

##### **2、数据成员的存储二义性**

解决方案：虚拟继承。

```cpp
#include <iostream>

using namespace std;

class A
{
public:
	virtual void a()
	{
		cout << "virtual void A::a()" << endl;
	}

	virtual void b()
	{
		cout << "virtual void A::b()" << endl;
	}

	virtual void c()
	{
		cout << "virtual void A::c()" << endl;
	}

private:

};


class B
{
public:
	virtual void a()
	{
		cout << "virtual void B::a()" << endl;
	}

	virtual void b()
	{
		cout << "virtual void B::b()" << endl;
	}

	void c()
	{
		cout << "virtual void B::c()" << endl;
	}

	void d()
	{
		cout << "virtual void B::d()" << endl;
	}

private:

};


class C
	:public A
	,public B
{
public:
	virtual void a()
	{
		cout << "virtual void C::a()" << endl;
	}


	void c()
	{
		cout << "virtual void C::c()" << endl;
	}

	void d()
	{
		cout << "virtual void C::d()" << endl;
	}

private:

};

void test()
{

	cout << "sizeof(A) = " << sizeof(A) << endl;
	cout << "sizeof(B) = " << sizeof(B) << endl;
	cout << "sizeof(C) = " << sizeof(C) << endl;
	C c;

	A* pa = &c;
	pa->a();
	pa->b();
	pa->c();
	cout << endl;

	B* pb = &c;
	pb->a();
	pb->b();
	pb->c();
	pb->d();
	cout << endl;

	C* pc = &c;
	pc->a();
	//二义性
	//pc->b();//C::b 对“b”的访问不明确
	pc->A::b();
	pc->B::b();
	pc->c();
	pc->d();
	cout << endl;


}
int main()
{
	test();
	return 0;
}
```

---

## **Day7-3 C++ 中的虚拟继承详解**

### **一、为什么虚拟继承能解决存储二义性？**

#### **1. 问题的根源：菱形继承**

假设存在以下继承关系：

```cpp
class A { public: int x; };
class B : public A {};
class C : public A {};
class D : public B, public C {};
```

##### **问题1：数据冗余**

`D` 通过 `B` 和 `C` 间接继承了两次 `A`，导致 `D` 对象中包含两份 `A` 的成员（`B::A::x` 和 `C::A::x`）。

##### **问题2：访问二义性**

当直接访问 `D` 中的 `x` 时，编译器无法确定是通过 `B` 还是 `C` 路径继承的 `x`，导致编译错误：

```cpp
D d;
d.x = 10;  // 报错：对“x”的访问不明确
```

#### **2. 虚拟继承的解决方案**

将 `B` 和 `C` 对 `A` 的继承改为虚拟继承：

```cpp
class A { public: int x; };
class B : virtual public A {};  // 虚拟继承
class C : virtual public A {};  // 虚拟继承
class D : public B, public C {};
```

##### 效果：

- `B` 和 `C` 共享同一个 `A` 实例，`D` 中仅保留一份 `A` 的成员。
- 访问 `x` 时不再有二义性：

```cpp
D d;
d.x = 10;  // 合法
```

#### **3. 实现原理**

##### **虚基类指针（vbptr）**

虚拟继承的类（如 `B` 和 `C`）会生成一个虚基类表指针（`vbptr`），指向虚基类表（`vbtable`）。

##### **虚基类表（vbtable）**

表中记录了虚基类成员在对象中的偏移量，确保通过不同路径访问时，最终指向同一块内存。

##### **构造顺序**

虚基类的构造函数仅调用一次（由最底层派生类 `D` 直接调用），避免重复初始化。

------

### **二、什么场景需要用到虚拟继承？**

#### **1. 菱形继承结构**

当需要让多个派生类共享同一个基类实例时，必须使用虚拟继承。典型场景包括：

##### **公共基类的复用**

例如，所有图形类（`Circle`、`Square`）可能共享一个公共的 `Shape` 基类。

##### **接口类继承**

若多个接口类继承自同一抽象基类，子类需通过虚拟继承合并接口。

#### **2. 示例场景**

```cpp
// 公共基类：图形属性
class Shape {
public:
    virtual void draw() = 0;
};

// 中间派生类（虚拟继承）
class Circle : virtual public Shape {
public:
    void draw() override { /* 绘制圆形 */ }
};

class Square : virtual public Shape {
public:
    void draw() override { /* 绘制方形 */ }
};

// 最终子类：组合圆形和方形
class HybridShape : public Circle, public Square {
public:
    void draw() override { /* 自定义绘制逻辑 */ }
};
```

如果 `Circle` 和 `Square` 未使用虚拟继承，`HybridShape` 会包含两份 `Shape` 的成员，导致二义性。

------

### **三、注意事项**

#### **1. 性能开销**

虚基类通过指针间接访问成员，可能略微降低效率。

#### **2. 构造函数调用**

虚基类的构造函数由最底层派生类直接调用，中间类的构造函数中无法直接初始化虚基类成员。

#### **3. 避免滥用**

仅在必要时使用虚拟继承，过度使用会增加代码复杂度。

------

### 总结

- **虚拟继承** 通过共享基类实例，解决了**菱形继承**中的**数据冗余**和**访问二义性**。
- 适用场景是需要**多继承**且存在**公共基类**的情况，如**接口组合**或**公共属性复用**。

---

## **Day7-4 基类与派生类之间的转换**
### **一、问题回顾**

1. 继承的形式有哪些？
   - 三种继承方式的特点？
   - 派生类的生成过程？
   - 继承的局限？
2. 派生类对象的创建和销毁的特点是什么？
3. 多继承的问题有哪些？如何解决？

------

### **二、基类与派生类间的转换**

#### **1. 类型适应（Upcasting）**

![alt text](基类派生类之间的转换-1.png)

派生类对象可以适用于基类对象，即一个派生类的实例可以用基类的引用或指针来操作。C++ 允许以下转换：

1. **赋值转换**：

   ```cpp
   Base base;
   Derived derived;
   base = derived; // 允许，但发生对象切片
   ```

   **注意**：派生类特有的数据会被丢弃。

2. **引用转换**（推荐）：

   ```cpp
   Base& ref = derived; // 允许，ref 绑定到 derived 的基类部分
   ```

3. **指针转换**（推荐）：

   ```cpp
   Base* pBase = &derived; // 允许，pBase 指向 derived 的基类部分
   ```

#### **2. 逆向转换（Downcasting）**

基类对象不能直接转换为派生类对象，除非使用**强制转换**：

```cpp
Derived* pDerived = static_cast<Derived*>(&base); // 不安全！
```

**原因**：基类对象可能不包含派生类的数据，强制转换可能导致非法访问。

使用 `dynamic_cast` 在多态类中更安全：

```cpp
Derived* pDerived = dynamic_cast<Derived*>(&base);
if (pDerived) {
    // 转换成功，pDerived 可用于 Derived 类型的操作
}
```

------

### **三、代码示例**

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    Base(double base = 0.0) : _base(base) {
        cout << "Base(double base = 0.0)" << endl;
    }
    virtual ~Base() { cout << "~Base()" << endl; }
    void print() const { cout << "Base::_base = " << _base << endl; }
private:
    double _base;
};

class Derived : public Base {
public:
    Derived(double base = 0.0, double derived = 0.0) : Base(base), _derived(derived) {
        cout << "Derived(double = 0.0, double = 0.0)" << endl;
    }
    ~Derived() { cout << "~Derived()" << endl; }
    void show() const { cout << "Derived::_derived = " << _derived << endl; }
private:
    double _derived;
};

int main()
{
	Base base(11.11);
	base.print();

	Derived derived(22.22, 33.33);
	derived.show();

	cout << "派生类向基类转换";
	base = derived;//1.可以将派生类对象赋值给基类对象
	base.print();

	/*与用基类对象直接初始化不同（这时会发生对象切片），引用绑定不会拷贝对象，只是为派生类对象的基类部分创建了一个别名。*/
	Base& ref = derived;//2.基类的引用可以绑定到派生类对象
	ref.print();

	Base* pbase = &derived;//3.基类的指针可以指向派生类对象（向上转型）
	pbase->print();

	cout << " 基类向派生类转化" << endl;
	Base base1(100);
	Derived derived1(200, 300);
	//derived1 = base1;//error! 1.不能将基类对象赋值给派生类对象
	//等价转换的形式如下：ERROR!!!
	//Derived& operator=(const Derived & rhs);
	//derived1.operator=(base1);//不存在用户定义的从 "Base" 到 "const Derived" 的适当转换
	//const Derived& rhs = base1;

	//Derived& dref = base1; //error! 2.派生类的引用不能绑定到基类对象
	//Derived* pref = &base1;//error! 3.派生类的指针不能指向基类对象,只指向Base基类的前8个字节，后面的指针操纵了非法内存，有内存越界的风险

	//语法层面不支持向下转型 基类-->派生类 ,使用强制转换（本质上是不安全的）
	Derived* pderived1 = static_cast<Derived*>(&base1) ;

	//间接的表明了pderived2指向pderived1(可以认为是安全的)
	Derived* pderived2 = static_cast<Derived*>(&base1);

	test();
	return 0;
}
```

**输出示例**：

```
Base(double base = 0.0)
Derived(double = 0.0, double = 0.0)
Base::_base = 22.22
Base::_base = 22.22
~Derived()
~Base()
```

------

### **四、派生类间的复制控制**

在继承体系中，**复制控制**（拷贝构造、赋值运算符）需要正确处理基类部分，否则可能导致资源泄漏或未定义行为。

**要点**：

1. 在拷贝构造和赋值运算符中，**必须显式调用基类的对应函数**，否则基类部分不会被正确复制。
2. 避免 **资源泄露**，在赋值运算符中先删除旧数据，再分配新数据。
3. 析构函数要**释放动态分配的资源**，否则会导致内存泄漏。

```cpp
//DerivedCopyControl1.cpp
#include <iostream>
#include <string.h>

using namespace std;

class Base
{
	friend std::ostream& operator<<(std::ostream& os, const Base& rhs);
public:
	Base()
		:_pbase(nullptr)
	{
		cout << "Base()" << endl;
	}

	Base(const char* pbase)
		:_pbase(new char[strlen(pbase) + 1])
	{
		cout << "Base(const char* pbase)" << endl;
		strcpy_s(_pbase, strlen(pbase) + 1, pbase);
	}
	Base(const Base& rhs)
		:_pbase(new char[strlen(rhs._pbase) + 1]())
	{
		cout << "Base(const Base& rhs)" << endl;
		strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
	}
	Base& operator=(const Base& rhs)
	{
		cout << "Base& operator=(const Base& rhs)" << endl;
		if (this != &rhs)
		{
			delete[] _pbase;
			_pbase = nullptr;
			_pbase = new char[strlen(rhs._pbase) + 1]();
			strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
		}
		return *this;
	}

	~Base()
	{
		cout << "~Base()" << endl;
		if (_pbase)
		{
			delete[] _pbase;
			_pbase = nullptr;
		}
	}

private:
	char* _pbase;
};

std::ostream& operator<<(std::ostream& os, const Base& rhs)
{
	if (rhs._pbase)
	{
		os << rhs._pbase;
	}
	return os;
}

class Derived : public Base
{
	friend std::ostream& operator<<(std::ostream& os, const Derived& rhs);
public:
	Derived(const char* pbase,const char* pDerived)
		:Base(pbase)
		,_pDerived(new char[strlen(pDerived) + 1])
	{
		cout << "Derived(const char* pbase,const char* pDerived)" << endl;
		strcpy_s(_pDerived, strlen(pDerived) + 1, pDerived);
	}

	Derived(const Derived& rhs)
		:_pDerived(new char[strlen(rhs._pDerived) + 1]())
	{
		cout << "Derived(const Derived& rhs)" << endl;
		strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
	}

	Derived& operator=(const Derived& rhs)
	{
		cout << "Derived& operator=(const Derived& rhs)" << endl;
		if (this != &rhs)
		{
			delete[] _pDerived;
			_pDerived = nullptr;

			_pDerived = new char[strlen(rhs._pDerived) + 1]();
			strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
		}
		return *this;
	}
	~Derived()
	{
		cout << "~Derived()" << endl;
		if (_pDerived)
		{
			delete[] _pDerived;
			_pDerived = nullptr;
		}
	}

private:
	char* _pDerived;
};

std::ostream& operator<<(std::ostream& os, const Derived& rhs)
{
	//Base& ref = rhs;//error!将 "Base &" 类型的引用绑定到 "const Derived" 类型的初始值设定项时，限定符被丢弃
	const Base& ref = rhs;//正确写法
	os << ref << "," << rhs._pDerived;
	
	return os;
}

void test()
{
	Derived derived("hello","world");
	cout << "derived = " << derived << endl;

	cout << endl;
	//用一个已经存在的对象去初始化一个刚刚创建的对象
	Derived derived2(derived);
	cout << "derived = " << derived << endl;
	cout << "derived2 = " << derived2 << endl;

	cout << endl;
	Derived derived3("cpp", "difficult");
	cout << "derived3 = " << derived3 << endl;
	
	cout << endl;
	//两个派生类对象之间进行赋值
	derived3 = derived;
	cout << "derived = " << derived << endl;
	cout << "derived3 = " << derived3 << endl;
}

int main() 
{
	test();
	return 0;
}



```

---
**`DerivedCopyControl2.cpp`**版本的cpp文件是为了解决上一个版本(**`DerivedCopyControl1.cpp`**)的潜在问题，主要问题如下：
- Base的拷贝构造函数和赋值运算符未处理源对象的_pbase为nullptr的情况，导致潜在崩溃。
- Derived的拷贝构造函数未调用Base的拷贝构造函数，导致基类部分未被正确复制。
- Derived的赋值运算符未调用Base的赋值运算符，导致基类部分未被正确赋值。

```cpp
//DerivedCopyControl2.cpp
#include <iostream>
#include <string.h>

using namespace std;

class Base
{
	friend std::ostream& operator<<(std::ostream& os, const Base& rhs);
public:
	Base()
		:_pbase(nullptr)
	{
		cout << "Base()" << endl;
	}

	Base(const char* pbase)
		:_pbase(new char[strlen(pbase) + 1])
	{
		cout << "Base(const char* pbase)" << endl;
		strcpy_s(_pbase, strlen(pbase) + 1, pbase);
	}

	/*Base(const Base& rhs)
		:_pbase(new char[strlen(rhs._pbase) + 1]())
	{
		cout << "Base(const Base& rhs)" << endl;
		strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
	}*/

	// 拷贝构造函数
	Base(const Base& rhs) : _pbase(nullptr) {
		cout << "Base(const Base& rhs)" << endl;
		if (rhs._pbase) {
			_pbase = new char[strlen(rhs._pbase) + 1];
			strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
		}
	}

	/*Base& operator=(const Base& rhs)
	{
		cout << "Base& operator=(const Base& rhs)" << endl;
		if (this != &rhs)
		{
			delete[] _pbase;
			_pbase = nullptr;
			_pbase = new char[strlen(rhs._pbase) + 1]();
			strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
		}
		return *this;
	}*/

	// 赋值运算符
	Base& operator=(const Base& rhs) {
		cout << "Base& operator=(const Base& rhs)" << endl;
		if (this != &rhs) {
			delete[] _pbase;
			_pbase = nullptr;
			if (rhs._pbase) {
				//base = new char[strlen(rhs._pbase) + 1]();
				//使用new char[size]()会进行零初始化，随后strcpy_s会覆盖这些零。此举虽无害但影响性能。
				_pbase = new char[strlen(rhs._pbase) + 1]; // 无需()
				strcpy_s(_pbase, strlen(rhs._pbase) + 1, rhs._pbase);
			}
		}
		return *this;
	}

	~Base()
	{
		cout << "~Base()" << endl;
		if (_pbase)
		{
			delete[] _pbase;
			_pbase = nullptr;
		}
	}

private:
	char* _pbase;
};

std::ostream& operator<<(std::ostream& os, const Base& rhs)
{
	if (rhs._pbase)
	{
		os << rhs._pbase;
	}
	return os;
}

class Derived : public Base
{
	friend std::ostream& operator<<(std::ostream& os, const Derived& rhs);
public:
	Derived(const char* pbase, const char* pDerived)
		:Base(pbase)
		, _pDerived(new char[strlen(pDerived) + 1])
	{
		cout << "Derived(const char* pbase,const char* pDerived)" << endl;
		strcpy_s(_pDerived, strlen(pDerived) + 1, pDerived);
	}

	
	/*Derived(const Derived& rhs)
		:_pDerived(new char[strlen(rhs._pDerived) + 1]())
	{
		cout << "Derived(const Derived& rhs)" << endl;
		strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
	}*/

	// 拷贝构造函数
	Derived(const Derived& rhs)
		: Base(rhs), // 调用基类拷贝构造
		_pDerived(new char[strlen(rhs._pDerived) + 1]) 
	{
		cout << "Derived(const Derived& rhs)" << endl;
		strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
	}

	

	/*Derived& operator=(const Derived& rhs)
	{
		cout << "Derived& operator=(const Derived& rhs)" << endl;
		if (this != &rhs)
		{
			delete[] _pDerived;
			_pDerived = nullptr;

			_pDerived = new char[strlen(rhs._pDerived) + 1]();
			strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
		}
		return *this;
	}*/

	// 赋值运算符
	Derived& operator=(const Derived& rhs) {
		if (this != &rhs) {
			Base::operator=(rhs); // 调用基类赋值运算符
			delete[] _pDerived;
			_pDerived = new char[strlen(rhs._pDerived) + 1];
			strcpy_s(_pDerived, strlen(rhs._pDerived) + 1, rhs._pDerived);
		}
		return *this;
	}

	~Derived()
	{
		cout << "~Derived()" << endl;
		if (_pDerived)
		{
			delete[] _pDerived;
			_pDerived = nullptr;
		}
	}

private:
	char* _pDerived;
};

std::ostream& operator<<(std::ostream& os, const Derived& rhs)
{
	//Base& ref = rhs;//error!将 "Base &" 类型的引用绑定到 "const Derived" 类型的初始值设定项时，限定符被丢弃
	const Base& ref = rhs;//正确写法
	os << ref << rhs._pDerived;

	return os;
}

void test()
{
	Derived derived("hello", "world");
	cout << "derived = " << derived << endl;

	cout << endl;
	//用一个已经存在的对象去初始化一个刚刚创建的对象
	Derived derived2(derived);
	cout << "derived = " << derived << endl;
	cout << "derived2 = " << derived2 << endl;

	cout << endl;
	Derived derived3("cpp", "difficult");
	cout << "derived3 = " << derived3 << endl;

	cout << endl;
	//两个派生类对象之间进行赋值
	derived3 = derived;
	cout << "derived = " << derived << endl;
	cout << "derived3 = " << derived3 << endl;

}

int main()
{
	test();
	return 0;
}



```
**总结：**
1、如果基类实现了拷贝构造函数或赋值运算符函数，但是派生类没有实现拷贝构造函数或赋值运算符函数，那么在将一个已经存在的派生类对象初始化一个刚刚创建的派生类对象，或者将两个派生类对象进行赋值，那么派生部分会执行缺省行为，而基类部分会执行基类的拷贝构造函数或者赋值运算符函数。

2、如果基类实现了拷贝构造函数或赋值运算符函数，并且派生类也实现拷贝构造函数或赋值运算符函数，那么在将一个已经存在的派生类对象初始化一个刚刚创建的派生类对象，或者将两个派生类对象进行赋值，那么派生部分会执行派生类自己的拷贝构造函数或者赋值运算符函数，而基类部分**不会自动**执行基类的拷贝构造函数或者赋值运算符函数，除非显示在派生类中调用基类的拷贝与赋值。

---

### **五、总结**

#### **1. `base = derived`（对象赋值，发生切片）**

```cpp
Base base = derived;  // 对象赋值，发生切片（slicing）
base.print();         // 调用 Base::print()
```
##### **特点：**
- **对象切片（Object Slicing）**：`derived` 的派生类部分被丢弃，只保留 `Base` 部分，`base` 是一个全新的 `Base` 对象。
- **不推荐**：除非你明确只需要基类部分，否则通常应该避免这种写法，因为它丢失了派生类的信息。

---

#### **2. `Base& ref = derived`（引用绑定，无切片）**
```cpp
Base& ref = derived;  // 引用绑定，无切片
ref.print();          // 如果 print() 是虚函数，调用 Derived::print()
```
##### **特点：**

✅ **优点：**
1. **无对象切片**：`ref` 只是 `derived` 的基类部分的引用，不会复制数据，派生类信息仍然完整。
2. **支持多态**：如果 `Base::print()` 是 `virtual` 的，调用 `ref.print()` 会正确调用 `Derived::print()`（动态绑定）。
3. **更高效**：避免了不必要的拷贝（特别是当 `Base` 较大时）。

❌ **缺点：**
1. **只能访问基类成员**：通过 `ref` 无法直接访问 `Derived` 的特有方法（如 `derived.extra()`）。
2. **必须确保 `derived` 的生命周期**：如果 `derived` 被销毁，`ref` 会变成悬空引用（dangling reference），导致未定义行为（UB）。

---

#### **3. 推荐程度**
| 写法 | 是否推荐 | 适用场景 |
|------|---------|---------|
| `Base base = derived;`（赋值） | ❌ 不推荐 | 除非明确只需要基类部分 |
| `Base& ref = derived;`（引用） | ✅ **推荐** | 需要多态、避免切片时 |
| `Base* ptr = &derived;`（指针） | ✅ **更推荐** | 更灵活，可以存储、传递 |

##### **更推荐的替代方案：`Base*`（指针）**
```cpp
Base* ptr = &derived;  // 基类指针指向派生类对象
ptr->print();          // 多态调用 Derived::print()
```
**优点：**
- 比引用更灵活（可以设为 `nullptr`，可以修改指向的对象）。
- 常用于运行时多态（如工厂模式、容器存储不同派生类对象）。

---

#### **4. 要点**
- **`Base& ref = derived` 是推荐的**，因为它避免了切片并支持多态，但必须确保 `derived` 的生命周期足够长。
- **`Base* ptr = &derived` 更推荐**，因为指针更灵活，适用于更多场景（如存储在不同容器中）。
- **`Base base = derived` 不推荐**，除非你明确只需要基类部分（但通常应该使用组合而非继承）。

#### **最佳实践**
```cpp
// ✅ 推荐：指针方式（更灵活）
Base* ptr = &derived;
ptr->print();

// ✅ 也可以：引用方式（适用于局部作用域）
Base& ref = derived;
ref.print();

// ❌ 不推荐：赋值方式（切片问题）
Base base = derived;
base.print();
```

如果你需要在函数参数中传递派生类对象，通常使用 **`const Base&`**（避免拷贝）或 **`Base*`**（更灵活）。

---

请帮我整理和完善这个笔记，使得内容完整，条理清晰！

## **Day7-5 多态（重要）**

### **1. 多态的基础**

面向对象的四大基本特征：**抽象、封装、继承、多态**。

**多态**：对于同一种指令（如警车鸣笛），针对不同对象（警察、普通人、嫌疑犯），产生不同的行为（行动、正常行走、藏起来）。

### **2. 多态的分类**

1. **静态多态**（编译时确定）：
   - 函数重载
   - 运算符重载
   - 模板
2. **动态多态**（运行时确定）：
   - 通过**虚函数**实现

示例代码：

```cpp
int add(int x, int y) {}
double add(double dx, double dy) {}
std::string add(std::string s1, std::string s2) {}

add(1, 2);
add("hello", "world");
```

### **3. 虚函数**

#### **概念**

虚函数是**成员函数**，在前面加上 `virtual` 关键字，使其在派生类中可以被覆盖，实现动态绑定。

#### **性质**

- 在基类中声明为虚函数的函数，在派生类中仍然是虚函数（即使没有显式声明 `virtual`）。
- **重定义（重写、覆盖）**：派生类中的虚函数必须保持**相同的名称、返回类型、参数列表**。
- **构造函数不能是虚函数**，但**析构函数应该是虚函数**，否则可能会导致**内存泄漏**。

#### **示例代码**

```cpp
#include <iostream>

/*
决不能在基类的构造/析构函数里调用虚函数
因为在构造基类时，子类还没构造，无法调用子类的虚函数
在析构基类时，子类已经被析构了，无法调用子类的虚函数
*/
class Shape {
protected:
    double x = 0.0, y = 0.0;
public:
    // 如果这个类有被继承的可能性，必须把析构函数声明为虚的
    // 否则会调用编译时的静态版本，即子类可能错误调用基类的析构函数，导致内存泄漏
    virtual ~Shape() = default;  // 虚析构函数，防止内存泄漏
    // 普通函数，编译时确定，如果指针实际指向子类，但此时被转为了基类，也只是调用基类的函数，即静态绑定
    void print() const { std::cout << "(" << x << ", " << y << ")" << std::endl; }
    // virtual 关键字使得函数成为虚函数
    // 如果没有实现函数体，可以加上 = 0 使得成为纯虚函数，纯虚函数会导致类成为虚类，而无法实例化，即无法调用构造函数
    // 虚函数，运行时决定，如果指针实际指向子类，但此时被转为了基类，可以调用正确的版本，即子类的版本，这种特性称为多态，也叫动态绑定
    virtual void v_print() const { std::cout << "virtual(" << x << ", " << y << ")" << std::endl; }
};

// 多态必须经继承实现
class Circle : public Shape {
private:
    double radius = 100.0;
public:
    // 覆盖了基类的同名版本
    void print() const { std::cout << "(" << x << ", " << y << ") " << radius << std::endl; }
    // override 是可选的，但强烈建议加上，表明自己的意图，也能在编译时发现一些错误，比如重写了一个非虚函数，或者忘记继承
    // final 关键字可以禁止子类重新实现该虚函数
    void v_print() const override final { std::cout << "virtual(" << x << ", " << y << ") - " << radius << std::endl; }
};

int main()
{
  Circle c;
  Shape* s = &c;
  s->print(); // 编译时决定，静态绑定，总是调用基类，因为表面上这是个指向基类的指针
  s->v_print(); // 运行时决定，动态绑定，调用子类的版本，因为实际上这个指针指向了子类

  // 为了实现多态，编译器不得不安插一些代码到类里，运行时才能调用正确的虚函数，细节请阅读《深度探索C++对象模型》
  sizeof(Shape); // 24字节 = 2个double + 为了实现多态的一个指针
  sizeof(Circle); // 32字节 = 基类的2个double + 自己的1个double + 为了实现多态的一个指针

  // RTTI 称为运行时类型识别，这是和多态一起诞生的概念
  // dynamic_cast 可以尝试将基类指针转为子类，如果不是对应的子类，转换结果为空指针
  // dynamic_cast 必须在存在虚函数的情况下，即多态下，如果没有虚函数，只是普通的继承，没法使用dynamic_cast，编译报错
  auto pC = dynamic_cast<Circle*>(s);
  if (pC)
  {
    pC->v_print();
  }
  // RTTI 的运行时信息，typeid(类名)可以生成对应的 type_info 类，name()是其中一个成员函数
  // 这是非常有限的反射
  // 对反射感兴趣可以去学习 Java/C# 等纯面向对象语言，其强大的运行时系统提供了完备的类型系统，包括反射
  std::cout << typeid(Shape).name() << std::endl;
  std::cout << typeid(Circle).name() << std::endl;

  return 0;
}
```

### **4. 虚函数原理（重要）**

**虚函数表（vtable）和虚函数指针（vptr）**

- **基类**定义虚函数后，每个对象都会有一个**虚函数指针（vptr）**，指向**虚函数表（vtable）**。
- **派生类**继承后，vptr 指向其自己的 vtable。
- 如果派生类重写了虚函数，vtable 中的对应函数入口地址会被替换为派生类的版本。

#### **示例代码**

```cpp
//virtual1.cpp
#include <iostream>

using namespace std;

#if 0

class Base
{
public:
	Base(double base = 0.0)
		:_base(base)
	{
		cout << "Base(double base = 0.0)" << endl;
	}
	~Base()
	{
		cout << "~Base()" << endl;
	}

	virtual void print() const
	{
		cout << "void Base::_base = " << _base << endl;
	}

private:
	double _base;
};

class Derived : public Base
{
public:
	Derived(double base = 0.0, double derived = 0.0)
		:Base(base)
		,_derived(derived)
	{
		cout << "Derived(double base = 0.0, double derived = 0.0)" << endl;
	}
	~Derived()
	{
		cout << "~Derived()" << endl;
	}

	void print() const override
	{
		cout << "Derived::_derived = " << _derived << endl;
	}

private:
	double _derived;
};

void func(Base* pbase)
{
	pbase->print();
}

void test()
{
	//注意：
	/*构造函数不能被继承，但是虚函数是可以被继承的；构造函数发生的时机在编译的时候，而虚函数要体现多态
	，发生的时机在运行的时候。如果构造函数被设置为虚函数，那么要体现出多态，就要放在虚表中，需要使用
	虚函数指针找到虚表，而如果构造函数都不调用，那对象是完全没有创建出来的，对象都不完整，此时有没有
	虚函数指针都不一定。*/
	Base base(11.11);
	Derived derived(22.22, 33.33);

	cout << endl;
	func(&base); //Base *pbase = &base;
	func(&derived);//Base *pbase = &derived;
}


int main()
{
	test();
	return 0;
}

#endif // 0
```

当基类定义了虚函数，就会在该类创建的对象的存储布局的前面，新增一个虚函数指针，该指针指向虚函数表（简称虚表），虚表中存的是虚函数的入口地址（有多少虚函数都会入虚表）。当派生类继承基类的时候，会满足吸收的特点，那么派生类也会有该虚函数，所以派生类创建的对象的布局的前面，也会新增一个虚函数指针，该指针指向派生类自己的虚函数表（简称虚表），虚表中存的是派生类的虚函数的入口地址（有多少虚函数都会入虚表），如果此时派生类重写了从基类这里吸收过来的虚函数，那么就会用派生类自己的虚函数的入口地址覆盖从基类这里吸收过来的虚函数的入口地址。

```c++
//virtual2.cpp
#include <iostream>

using namespace std;

class Base
{
public:
	Base(double base = 0.0)
		:_base(base)
	{
		cout << "Base(double base = 0.0)" << endl;
	}
	~Base()
	{
		cout << "~Base()" << endl;
	}

	virtual void print() const
	{
		cout << "void Base::_base = " << _base << endl;
	}

private:
	double _base;
};

class Derived : public Base
{
public:
	Derived(double base = 0.0, double derived = 0.0)
		:Base(base)
		, _derived(derived)
	{
		cout << "Derived(double base = 0.0, double derived = 0.0)" << endl;
	}
	~Derived()
	{
		cout << "~Derived()" << endl;
	}

	void print() const override
	{
		cout << "Derived::_derived = " << _derived << endl;
	}

private:
	double _derived;
};

void func(Base* pbase)
{
	pbase->print();
}

void func1(Base& ref)
{
	ref.print();
}

void test_func1()
{
	
	Base base(11.11);
	Derived derived(22.22, 33.33);

	cout << endl;
	func1(base); //Base& ref = base;
	func1(derived);//Base& ref = derived;
}

void testObjection()
{
	Base base(11.11);
	Derived derived(22.22, 33.33);

	cout << endl;
	base.print();//对象调用
	derived.print();//对象调用
}

 
int main() 
{
	//test_func1();
	testObjection();
	return 0;
}
```

### **5. 虚函数机制的激活条件（动态多态的五个条件）**

1. **基类定义虚函数**
2. **派生类重写（重定义、覆盖）该虚函数**
3. **创建派生类对象**
4. **用基类指针（或引用）指向派生类对象**
5. **通过基类指针（或引用）调用该虚函数**

### **6. `Base& ref = derived;` 的相似写法**

在C++中，`Base& ref = derived`（基类引用绑定到派生类对象）是一种 **推荐** 的写法，但具体是否适用取决于你的需求。

`int &ref = number;` 和 `Base &ref = derived;` 这两种引用声明方式在**语法形式上是相似的**，但在**语义和底层行为上有重要区别**。下面我们详细对比它们的异同：

---

#### **1. 语法形式相似**

两种写法都遵循 C++ 引用的基本语法：

```cpp
T& ref = obj;  // T 是类型，obj 是已存在的对象
```

- `int& ref = number;`：`ref` 是 `int` 的引用，绑定到 `number`。
- `Base& ref = derived;`：`ref` 是 `Base` 的引用，绑定到 `derived`（派生类对象）。

---

#### **2. 核心区别**

##### **(1) 引用绑定的对象类型**

| 引用声明               | 绑定对象类型                        | 是否允许                       |
| ---------------------- | ----------------------------------- | ------------------------------ |
| `int& ref = number;`   | 相同类型（`int` → `int`）           | ✅ 允许                         |
| `Base& ref = derived;` | 派生类 → 基类（`Derived` → `Base`） | ✅ 允许（多态）                 |
| `Derived& ref = base;` | 基类 → 派生类（`Base` → `Derived`） | ❌ 错误（除非基类实际是派生类） |

**关键点**：

- C++ 允许 **基类引用绑定到派生类对象**（向上转型，Upcasting），这是多态的基础。
- 但反过来（派生类引用绑定基类对象）是禁止的，除非通过 `dynamic_cast` 且基类具有多态性（含虚函数）。

---

##### **(2) 可访问的成员**

| 引用类型    | 能访问的成员                                              |
| ----------- | --------------------------------------------------------- |
| `int& ref`  | 可访问 `int` 的所有操作（如加减乘除）                     |
| `Base& ref` | 只能访问 `Base` 的成员，无法直接访问 `Derived` 的特有成员 |

**示例**：

```cpp
class Base {
public:
    void base_func() { cout << "Base\n"; }
};

class Derived : public Base {
public:
    void derived_func() { cout << "Derived\n"; }
};

int main() {
    Derived derived;
    Base& ref = derived;

    ref.base_func();    // ✅ 允许
    // ref.derived_func(); // ❌ 错误：Base 没有 derived_func
}
```

---

##### **(3) 多态行为（虚函数）**

如果 `Base` 有虚函数，且 `Derived` 覆盖了它，通过 `Base& ref` 调用时会触发动态绑定：

```cpp
class Base {
public:
    virtual void print() { cout << "Base\n"; }
};

class Derived : public Base {
public:
    void print() override { cout << "Derived\n"; }
};

int main() {
    Derived derived;
    Base& ref = derived;
    ref.print(); // 输出 "Derived"（多态调用）
}
```

而 `int&` 不涉及多态，因为内置类型（如 `int`）没有虚函数。

---

#### **3. 底层机制**

- `int& ref = number;`：
  - `ref` 直接绑定到 `number` 的内存地址，操作 `ref` 等同于操作 `number`。
- `Base& ref = derived;`：
  - `ref` 绑定到 `derived` 对象中的 **基类子对象部分**（即 `Base` 的部分）。
  - 如果 `Base` 有虚函数，`ref` 会通过虚表（vtable）实现多态。

---

#### **4. 总结**

| 特性             | `int& ref = number;`      | `Base& ref = derived;`              |
| ---------------- | ------------------------- | ----------------------------------- |
| **绑定类型**     | 相同类型（`int` → `int`） | 派生类 → 基类（`Derived` → `Base`） |
| **是否支持多态** | 否（`int` 无虚函数）      | 是（如果 `Base` 有虚函数）          |
| **可访问成员**   | 全部 `int` 操作           | 仅 `Base` 的成员                    |
| **底层行为**     | 直接别名                  | 绑定到派生类对象的基类部分          |

#### **关键结论**

1. **语法形式相同**，但 `Base& ref = derived;` 利用了 C++ 的多态特性。
2. **推荐使用基类引用/指针绑定派生类对象**，以实现运行时多态（比直接赋值 `Base base = derived;` 更高效且安全）。
3. 如果不需要多态，优先使用 **值类型或模板**，而非继承体系。

### **7. 虚函数的限制**

#### **7.1 不能被设置为虚函数的情况**

1. **普通函数（自由函数、全局函数）**：虚函数必须是成员函数，而普通函数是非成员函数。

2. **内联成员函数**：

   - 内联函数在编译时进行函数替换，而虚函数的多态性在运行时体现。
   - 如果一个函数被声明为 `virtual`，编译器不会强制它内联，内联优化可能会被取消。

3. **静态成员函数**：

   - 静态成员函数属于整个类，而非某个对象。
   - 静态成员函数没有 `this` 指针，而虚函数通常依赖 `this` 进行多态调用。

4. **友元函数**：

   - 友元函数不是成员函数，因此不能是虚函数。
   - 但如果友元函数是另一个类的成员函数，则该成员函数可以是虚函数。

5. **构造函数**：

   - 构造函数不能是虚函数。
   - 在对象构造过程中，虚表尚未初始化，导致无法正确解析虚函数。
   - 如果构造函数是虚函数，基类指针在对象构造前可能指向错误的虚函数表。

   > 构造函数：构造函数不能被继承，但是虚函数是可以被继承的；构造函数发生的时机在编译的时候，而虚函数要体现动态多态，发生的时机在运行的时候；**如果构造函数被设置为虚函数，那么要体现出多态，就需要放在虚表中，需要使用虚函数指针找到虚表，而如果构造函数都不调用，那对象是没有完全创建出来的，对象都不完整，此时有没有虚函数指针都不一定。（重要）**

------

### **8. 虚函数的访问规则**

1. **使用指针或引用访问时，可体现动态多态**。
2. **使用对象直接访问时，不体现动态多态**，而是调用对象所属类中的普通函数。
3. **普通成员函数内部调用虚函数时，可能体现动态多态**，但要注意调用时的对象类型。

示例：

```cpp
class Base {
public:
    virtual void show() { std::cout << "Base::show()" << std::endl; }
};
class Derived : public Base {
public:
    void show() override { std::cout << "Derived::show()" << std::endl; }
};

int main() {
    Derived d;
    d.show();  // 调用 Derived::show()
    Base* pBase = &d;
    pBase->show();  // 调用 Derived::show()，表现出动态多态
}
```

------

### **9. 抽象类**

抽象类通常用于定义接口，不能直接实例化。

```cpp
//抽象类是作为接口使用的
//抽象类的第二种表现形式：构造函数用protected 限定
//这种情况下，抽象类也是不能创建对象的，但是该构造函数是可以被派生类访问的
class Base {
protected:
    Base() {}
public:
    virtual ~Base() {}
    //纯虚函数，在基类中只是声明该虚函数，但是不实现，将该函数的实现放在派生类中
    virtual void show() const = 0;
    virtual void display() const = 0;
    //如果抽象类的派生类没有将所有的纯虚函数都实现，那么抽象类的派生类也是抽象类
};

class Derived : public Base {
public:
    void show() const override { std::cout << "Derived::show()" << std::endl; }
    void display() const override { std::cout << "Derived::display()" << std::endl; }
};

void test()
{
	//Base base;//不允许使用抽象类类型 "Base" 的对象:
	Derived derived;
}
```

**关键点**：

- 纯虚函数 (`= 0`) 使类成为抽象类。
- 如果派生类未实现所有纯虚函数，则它仍然是抽象类。
- 可以创建抽象类的指针或引用。

------

### **10. 纯虚函数**

纯虚函数只在基类中声明，不提供实现，其实现交由派生类完成。

```cpp
//抽象类是作为接口使用的
class Base {
public:
    virtual void show() const = 0;
    virtual void display() const = 0;
};

class Derived : public Base {
public:
    void show() const override { std::cout << "Derived::show()" << std::endl; }
    void display() const override { std::cout << "Derived::display()" << std::endl; }
};

void test()
{
	//Base base;//不允许使用抽象类类型 "Base" 的对象:
	Derived derived;
	derived.show();

	Base* pbase = &derived;
	pbase->show();
	pbase->display();
	//虽然Base 是抽象类，是不完整的类型，不能创建对象，但是可以创建该种类型的指针或引用
	cout << endl << endl;
	Derived* pderived = &derived;
	pderived->show();
	pderived->display();
}
```

------

### **11. 虚析构函数**

```cpp
class Base {
public:
    Base(const char* pBase) : _pBase(new char[strlen(pBase) + 1]) {
        strcpy(_pBase, pBase);
    }
    virtual ~Base() {
        delete[] _pBase;
    }
private:
    char* _pBase;
};

class Derived : public Base {
public:
    Derived(const char* pBase, const char* pDerived) : Base(pBase), _pDerived(new char[strlen(pDerived) + 1]) {
        strcpy(_pDerived, pDerived);
    }
    ~Derived() {
        delete[] _pDerived;
    }
private:
    char* _pDerived;
};
```

如果 `Base` 的析构函数不是 `virtual`，`delete Base* pBase` 只会调用 `Base` 的析构函数，而不会调用 `Derived` 的析构函数，导致内存泄漏。

**正确做法**：

```cpp
virtual ~Base() {}
```

------

### **12.构造函数和析构函数中的虚函数调用**

---

#### **问题描述**

我们有以下C++代码：

```cpp
#include <iostream>

class B {
public:
    B() {
        test();
    }

    virtual void test() {
        std::cout << "B\n" << std::endl;
    }
private:
};

class D : public B {
public:
    D() = default;
    virtual void test() override {
        std::cout << "D\n" << std::endl;
    }
private:
};

int main() {
    D d;
    return 0;
}
```

运行这段代码时，输出的是 `B` 而不是 `D`。这是为什么？

#### **初步观察**

首先，我们有一个基类 `B` 和一个派生类 `D`。`B` 的构造函数中调用了虚函数 `test()`，而 `D` 重写了这个虚函数。当我们创建一个 `D` 的对象时，`B` 的构造函数会被调用，然后在 `B` 的构造函数中调用了 `test()`。然而，输出的是 `B` 的 `test()` 实现，而不是 `D` 的。

#### **理解对象构造过程**

在C++中，对象的构造是从基类到派生类的顺序进行的。具体到这段代码：

1. 当创建 `D` 的对象 `d` 时，首先调用 `B` 的构造函数。
2. 在 `B` 的构造函数中，调用了 `test()`。
3. 之后，`D` 的构造函数被调用（这里 `D` 的构造函数是默认的，没有额外操作）。

#### **虚函数在构造函数中的行为**

关键在于：**在构造函数中调用虚函数时，虚函数的动态绑定（多态）行为不会发生**。也就是说，在 `B` 的构造函数中调用 `test()` 时，它调用的是 `B` 的 `test()`，而不是 `D` 的 `test()`。

这是因为：

- 当 `B` 的构造函数正在执行时，`D` 的部分还没有被构造。此时对象的 `D` 部分还不完整，因此不能安全地调用 `D` 的重写函数。
- C++ 的设计决定了在基类的构造函数中，虚函数调用被解析为当前类（即基类）的实现，而不是派生类的实现。这是为了避免在对象未完全构造时调用未初始化的派生类成员。

#### **C++标准的规定**

根据C++标准（如ISO/IEC 14882:2017）：

> When a virtual function is called directly or indirectly from a constructor or from a destructor, including during the construction or destruction of the class’s non-static data members, and the object to which the call applies is the object under construction or destruction, the function called is the one defined in the constructor or destructor’s own class or in one of its bases, but not a function overriding it in a class derived from the constructor or destructor’s class, or overriding it in one of the other base classes of the most derived object.

简单来说，在构造函数或析构函数中调用虚函数时，调用的版本是当前构造函数或析构函数所在类的版本，而不是派生类的重写版本。

#### **示例验证**

让我们稍微修改代码，以更清楚地看到构造顺序：

```cpp
#include <iostream>

class B {
public:
    B() {
        std::cout << "B constructor\n";
        test();
    }

    virtual void test() {
        std::cout << "B::test\n" << std::endl;
    }
};

class D : public B {
public:
    D() {
        std::cout << "D constructor\n";
        test();
    }
    virtual void test() override {
        std::cout << "D::test\n" << std::endl;
    }
};

int main() {
    D d;
    return 0;
}
```

输出：

```
B constructor
B::test
D constructor
D::test
```

可以看到：

1. `B` 的构造函数先被调用，此时 `test()` 调用的是 `B::test()`。
2. 然后 `D` 的构造函数被调用，此时 `test()` 调用的是 `D::test()`。

#### **为什么这样设计？**

这种设计是为了保证对象构造的安全性。考虑以下情况：

```cpp
class B {
public:
    B() {
        test();  // 如果这里调用 D::test()，而 D 的成员还未初始化
    }
    virtual void test() {}
};

class D : public B {
    int x;
public:
    D() : x(42) {}
    virtual void test() override {
        std::cout << x;  // 如果 B::B() 中调用 D::test()，x 还未初始化
    }
};
```

如果在 `B` 的构造函数中调用 `D::test()`，而 `D` 的成员 `x` 还未初始化（因为 `D` 的构造函数还未执行），那么访问 `x` 将是未定义行为。因此，C++ 规定在构造函数中虚函数调用不进行动态绑定，而是使用当前类的实现。

#### **类似行为：析构函数**

类似的情况也适用于析构函数：

```cpp
class B {
public:
    virtual ~B() {
        test();  // 调用 B::test()，即使是在 ~D() 中
    }
    virtual void test() {
        std::cout << "B::test\n";
    }
};

class D : public B {
public:
    ~D() {
        // D 的部分已经被销毁
    }
    virtual void test() override {
        std::cout << "D::test\n";
    }
};

int main() {
    B* b = new D();
    delete b;  // 输出 B::test
    return 0;
}
```

在析构函数中，派生类的部分已经被销毁，因此虚函数调用也会被解析为当前类的实现，以避免访问已销毁的派生类成员。

#### **解决方案**

如果需要在构造时调用派生类的虚函数实现，可以考虑以下方法：

1. **不要在构造函数中调用虚函数**：这是最直接的解决方案。将需要在构造后进行的操作移到单独的初始化函数中，由用户显式调用。

   ```cpp
   class B {
   public:
       B() {}
       void init() { test(); }  // 构造后由用户调用
       virtual void test() { std::cout << "B::test\n"; }
   };
   
   class D : public B {
   public:
       virtual void test() override { std::cout << "D::test\n"; }
   };
   
   int main() {
       D d;
       d.init();  // 输出 D::test
       return 0;
   }
   ```

2. **使用工厂模式**：通过工厂函数创建对象，并在构造后调用虚函数。

   ```cpp
   class B {
   public:
       virtual void test() { std::cout << "B::test\n"; }
   };
   
   class D : public B {
   public:
       virtual void test() override { std::cout << "D::test\n"; }
   };
   
   template <typename T>
   T* createAndTest() {
       T* obj = new T();
       obj->test();  // 安全调用派生类的实现
       return obj;
   }
   
   int main() {
       D* d = createAndTest<D>();  // 输出 D::test
       delete d;
       return 0;
   }
   ```

#### **总结**

- 在构造函数中调用虚函数时，调用的版本是当前构造函数所在类的实现，而不是派生类的重写版本。
- 这是因为派生类的部分在基类构造函数执行时还未构造完成，调用派生类的虚函数可能导致未定义行为。
- 这种设计是C++语言为了保证对象构造的安全性而做出的规定。
- 如果需要多态行为，应避免在构造函数中调用虚函数，或通过其他方式（如初始化函数或工厂模式）实现。

#### **最终答案**

在 `D d;` 的构造过程中，`B` 的构造函数首先被调用。在 `B` 的构造函数中调用虚函数 `test()` 时，由于 `D` 的部分尚未构造完成，C++ 的规则规定此时虚函数的调用被静态绑定到当前类（即 `B`）的实现，而不是派生类 `D` 的重写版本。因此，输出的是 `B` 而不是 `D`。这是C++为了保证对象构造的安全性而设计的特性。

### **13、虚表**

```cpp
//vtable.cpp
#include <iostream>

using namespace std;

class Base
{
public:
	Base(long base = 0)
		:_base(base)
	{
		cout << "Base(long base = 0)" << endl;
	}

	~Base()
	{
		cout << "~Base()" << endl;
	}

	virtual void f()
	{
		cout  << "Base::f()" << endl;
	}

	virtual void g()
	{
		cout << "Base::g()" << endl;
	}

	virtual void h()
	{
		cout << "Base::h()" << endl;
	}
private:
	long _base;
};


class Derived : public Base
{
public:
	Derived(long base = 0 ,long derived = 0)
		:Base(base)
		,_derived(derived)
	{
		cout << "Derived(long base = 0 ,long derived = 0)" << endl;
	}

	~Derived()
	{
		cout << "~Derived()" << endl;
	}

	virtual void f()
	{
		cout << "Derived::f()" << endl;
	}

	virtual void g()
	{
		cout << "Derived::g()" << endl;
	}

	virtual void h()
	{
		cout << "Derived::h()" << endl;
	}
private:
	long _derived;
};

int add(int a, int b)
{
	return a + b;
}
void testAdd()
{
	typedef int (*pFunc)(int, int);
	pFunc pf = add;
	cout << pf(2,3);

}
void test_typedef()
{
	Derived derived(10, 20);
	typedef void (*pFunc)(void);
	pFunc pf = nullptr;
	pf = (pFunc)* ((long*)*(long*)&derived);
	pf();//调用pf
	printf("第一个虚函数的入口地址: %p\n", pf);
}



//函数指针
void testFunctionPointer()
{
	Derived derived1(10, 20);
	typedef void (*FuncPtr)();  // 定义函数指针类型
	FuncPtr* vtable = *(FuncPtr**)&derived1;  // 获取虚函数表
	printf("对象derived1的地址: %p\n", &derived1);
	printf("虚函数表的地址: %p\n", (void*)vtable);
	printf("第一个虚函数的入口地址: %p\n", (void*)vtable[0]);
	printf("第二个虚函数 (g): %p\n", (void*)vtable[1]);
	printf("第三个虚函数 (h): %p\n", (void*)vtable[2]);


	Derived derived2(100, 200);
	typedef void (*FuncPtr)();  // 定义函数指针类型
	FuncPtr* vtable2 = *(FuncPtr**)&derived2;  // 获取虚函数表
	printf("对象derived2的地址: %p\n", &derived2);
	printf("虚函数表的地址: %p\n", (void*)vtable2);
	printf("第一个虚函数的入口地址: %p\n", (void*)vtable2[0]);
	printf("第二个虚函数 (g): %p\n", (void*)vtable2[1]);
	printf("第三个虚函数 (h): %p\n", (void*)vtable2[2]);

	//打印_base 和 _derived的值
	cout << "_base = " << (long) *((long*)&derived2 + 1) << endl;
	cout << "_derived = " << (long) *((long*)&derived2 + 2) << endl;
}

void testFunctionPointer2()
{
	// FuncPtr 是一个指向“返回 void 且无参数的函数”的指针类型
	typedef void (*FuncPtr)(void);//typedef void (*)(void) FuncPtr;  // 逻辑上等价，但语法上不合法
	/*正确的 typedef 语法要求 类型名必须紧跟在* 后面。typedef 返回类型 (*别名)(参数列表); */
}

int main()
{
	testAdd();
	//test_typedef();
	testFunctionPointer();
	cout << endl << endl;

	Derived derived1(10, 20);
	printf("对象derived1的地址: %p\n", &derived1);
	printf("对象derived1的地址: %p\n", (long*) &derived1);//把derived1的地址强转成long*

	//将derived1的地址进行解引用，得到的就是虚函数表的地址
	printf("虚函数表的地址: %p\n", *(long*)&derived1);
	printf("虚函数表的地址: %p\n", (long*) *(long*)&derived1);//把虚函数表的地址强转成long*

	//printf("虚表中第一个虚函数的入口地址: %p\n", (long*) *(long*)*(long*)&derived1);//第一个虚函数的入口地址强转成long*
	printf("虚表中第一个虚函数的入口地址: %p\n", (void*)*(*(long**)&derived1));
	
	//&derived1是对象的地址，
	//*&derived1是虚函数表的地址
	//*(*&derived1)是虚表中第一个虚函数的入口地址
	/*printf("虚表中第一个虚函数的入口地址: % p\n", *(*&derived1));*/

	
	return 0;
}
```

### **14、函数指针**
在 C/C++ 中，`typedef void (*FuncPtr)(void);` 的真实含义是 **`typedef void(*)(void) FuncPtr;`** 的简写形式，但语法上更倾向于：

#### **正确解析**
```cpp
typedef void (*FuncPtr)(void);
```
等价于：
```cpp
typedef void (*)(void) FuncPtr;  // 逻辑上等价，但语法上不合法
```
或更准确地说：
```cpp
// FuncPtr 是一个指向“返回 void 且无参数的函数”的指针类型
typedef void (FuncPtr)(void);    // 错误，这样定义的是函数类型，不是指针
typedef void (*FuncPtr)(void);   // 正确，定义的是函数指针
```

---

#### **详细解释**
1. **`typedef void (*FuncPtr)(void);`**  
   - 定义了一个名为 `FuncPtr` 的类型，它是指向 **“返回 `void` 且无参数的函数”** 的指针。
   - 用法：
     ```cpp
     FuncPtr ptr = &someFunction;  // ptr 是一个函数指针
     ptr();  // 调用函数
     ```

2. **`typedef void(*)(void) FuncPtr;`（逻辑等价，但语法错误）**  
   - 这种写法在逻辑上是等价的，但 C/C++ 语法不允许直接这样写。
   - 正确的 `typedef` 语法要求 **类型名必须紧跟在 `*` 后面**。

3. **`typedef void()(void) *FuncPtr;`（完全错误）**  
   - 这是非法的语法：
     - `void()(void)` 不是合法的类型声明。
     - `*FuncPtr` 不能放在末尾。

---

#### **对比正确 vs 错误写法**
| 写法 | 含义 | 合法性 |
|------|------|--------|
| `typedef void (*FuncPtr)(void);` | 定义函数指针类型 `FuncPtr` | **合法** |
| `typedef void(*)(void) FuncPtr;` | 逻辑等价，但语法错误 | **非法** |
| `typedef void()(void) *FuncPtr;` | 无意义，语法错误 | **非法** |

---

#### **为什么 `typedef void (*FuncPtr)(void);` 是正确的？**
- `void (*)(void)` 是一个 **函数指针类型**（指向无参数、返回 `void` 的函数）。
- `typedef` 的作用是为这个类型定义一个别名 `FuncPtr`。
- 语法规则要求 `*` 必须紧跟在类型名之前：
  ```cpp
  typedef 返回类型 (*别名)(参数列表);
  ```

---

#### **实际应用示例**
```cpp
#include <iostream>

void hello() {
    std::cout << "Hello, World!" << std::endl;
}

int main() {
    typedef void (*FuncPtr)(void);  // 定义函数指针类型
    FuncPtr ptr = &hello;           // 赋值函数地址
    ptr();                          // 调用函数
    return 0;
}
```
输出：
```
Hello, World!
```

---

#### **总结**
- **正确写法**：`typedef void (*FuncPtr)(void);`  
  - 定义 `FuncPtr` 为“无参数、返回 `void` 的函数指针”类型。
- **错误写法**：
  - `typedef void(*)(void) FuncPtr;`（语法错误，`*` 不能放在中间）
  - `typedef void()(void) *FuncPtr;`（完全非法）

如果你想要更直观的理解，可以想象：
```cpp
void (*FuncPtr)(void);   // FuncPtr 是一个函数指针
typedef void (*FuncPtr)(void);  // 现在 FuncPtr 是一个类型
```
---

## **Day8-1 操作符重载再探（2025.03.31）**

### **一、 操作符重载深入解析**

#### **1.1 操作符重载的应用场景**

1. **自定义数学类型**（向量、矩阵、复数等）
2. **实现类对象的自然语义操作**
3. **流操作定制化**

#### **1.2 Int类完整实现示例**

```cpp
class Int {
    friend std::ostream& operator<<(std::ostream& os, Int i);
public:
    // 构造函数
    Int() = default;
    Int(int i) : _i(i) {}
    
    // 算术运算符
    Int operator+(const Int& i) const { return _i + i._i; }
    Int operator+(int i) const { return _i + i; }
    
    // 赋值运算符
    Int& operator=(const Int& i) { _i = i._i; return *this; }
    Int& operator=(int i) { _i = i; return *this; }
    
    // 类型转换运算符
    operator int() const { return _i; }

private:
    int _i = 0;
};

// 流输出运算符
std::ostream& operator<<(std::ostream& os, Int i) {
    os << i._i;
    return os;
}
```

#### **1.3 操作符重载关键要点**

| 运算符类型 | 返回值类型 | 参数传递    | 备注               |
| ---------- | ---------- | ----------- | ------------------ |
| 算术运算符 | 值类型     | const引用   | 应保证不修改操作数 |
| 赋值运算符 | 左值引用   | const引用   | 需返回*this        |
| 流运算符   | 流引用     | 非const引用 | 通常定义为友元     |

#### **1.4 字符串操作示例**

```cpp
void stringDemo() {
    string a = "cpp";
    string b = a + " java";  // 调用operator+(const char*)
    string c = "python " + b; // 调用operator+(const char*, string)
}
```

#### **1.5示例代码**

```cpp
#include <iostream>

/*
操作符重载

常见的操作符：
1. 数值运算 + - * /
2. 赋值 =
3. 流操作符 << >>
4. 其他，比如取地址&，解引用*（和乘法一样），打开域::，类指针成员访问->，类实例成员访问. 等

编译器为内置类型定义了基本的操作符，比如以上的 1 & 2 & 3 项

每个操作符只是个函数，即 + 运算符只是个以 operator+ 为函数名的普通函数而已

我们需要用到操作符重载的地方：
1. 实现一些自定义的数学类型，比如 向量，矩阵，复数等，这些类也可以进行数值运算，那就需要为这些类定义这些操作符
2. 拷贝赋值运算符和移动赋值运算符是编译器默认生成的两个重要函数，我们理应掌握这两个函数

常见地，我们更多需要的是重载数值运算符，赋值和流操作符

对于数值运算符，计算的结果是个右值，意思是计算结果的返回值是不可以赋值的，比如
int a = 1;
int b = 2;
(a + b) = 100; // 报错，右值不能赋值
对于自定义类型，比如T
T a;
T b;
要支持 a + b，有两种定义方法，要么定义T类的成员函数，要么定义类外的普通函数
class T
{
public:
  T operator+(const T& r); // 默认该类本身是左操作数，传入的参数是右操作数
};
或者
T operator(const T& l, const T& r); // 分别传入左操作数和右操作数

对于赋值运算符，计算的结果是个左值，比如
int a = 1;
(a = 2) = 3; // 正确，a = 2的返回值就是 a 本身，是个左值，可以继续赋值
对于自定义类型T，比如
T a;
T b;
要支持 a = b，一般就定义拷贝赋值运算符，即
class T
{
public:
  // 这是个编译器会默认生成的函数，非常重要
  // 请牢记它的样子，返回类的左值引用，参数是类的常量引用
  // 返回值需要返回 *this，即它本身，必须是个引用，如果不是引用会发生拷贝，就和当前这个类对象没关系了
  // 返回值还要支持修改，即左值属性，所以必须是左值引用
  // 传入参数不应该发生拷贝，即传引用，也不可以修改，所以使用常量引用
  T& operator=(const T& r);
};

流操作符的左边一般是一个输出流对象，右边是某种类型，计算结果仍是流对象本身，比如
int a = 1;
int b = 2;
(std::cout << a) << b; // std名称空间中的cout是个全局变量，cout << a 后返回就是 cout 本身，可以继续 << b
对于自定义类型T，没法定义成员函数，因为左操作数不是T，所以一般定义类外函数
std::ostream& operator<<(std::ostream& os, T r);

现在我们定一个自己的int，名为Int，希望它表现得像int
*/
// 第一个版本
namespace v1
{
class Int
{
  // 由于流运算符要访问 _i，需要声明为友元，友元可以突破访问控制，访问类的 private 成员
  // 友元声明无所谓访问控制，放在 public / private 下面都可
  friend std::ostream& operator<<(std::ostream& os, Int r);

private:
  int _i;

public:
  // 可以使用同类型变量构造（特殊函数，拷贝构造函数）
  // 为了支持 Int a = b; // b也是Int
  Int(const Int& r) : _i(r._i) {} // 编译器会生成这个版本，可以不实现，或者直接 = default
  // 也可以使用普通int构造
  // 为了支持 Int a = 1;
  Int(const int& r) : _i(r) {}

  // 它应该支持和同类型变量相加
  // 让 a + b 可以工作
  Int operator+(const Int& r) const { return Int(_i + r._i); }
  // 也应该和普通int相加
  // 让 a + 1 可以工作
  Int operator+(const int& r) const { return Int(_i + r); }

  // - * / 这里就省略了

  // 可以把同类型变量赋给它（特殊函数，拷贝赋值运算符）
  // 让 a = b 工作
  Int& operator=(const Int& r) { _i = r._i; return *this; } // 编译器会生成这个版本，可以不实现，或者直接 = default
  // 也可以把普通int赋给它
  // 让 a = 1 工作
  Int& operator=(const int& r) { _i = r; return *this; }
};

// 流运算符
// << 会改变流，所以要传左值引用，把r写入流后，继续把流以左值引用传出去
// 流不可以拷贝，其拷贝构造函数和拷贝赋值运算符被定义为 delete，所以只能以引用传递
std::ostream& operator<<(std::ostream& os, Int r)
{
  os << r._i;
  return os;
}
}

void demo1()
{
  using namespace v1;
  Int a = 1;
  Int b = 2;
  b = a;
  Int c = a + b;
  std::cout << c << std::endl;
}

/*
为了少写点，我们希望 Int 能隐式转换为 int，这样能少写一半成员函数，
即调用 operator+(Int r) 时先把 Int转为 int，再调用 operator+(int r)
为了支持转换，需要定义类型转换操作符，比如想把 T1 转为 T2
class T1
{
public:
  operator T2() const;
};
*/
//第二个版本
namespace v2
{

class Int
{
  friend std::ostream& operator<<(std::ostream& os, Int r);

private:
  int _i;

public:
  Int(const int& r) : _i(r) {}

  Int operator+(const int& r) const { return Int(_i + r); }

  Int& operator=(const int& r) { _i = r; return *this; }

  // 让 Int 在需要转换的地方自动转为 int，所以以上函数的 Int 参数版本都被删除了
  operator int() const { return _i; }
};

std::ostream& operator<<(std::ostream& os, Int r)
{
  os << r._i;
  return os;
}

}

void demo2()
{
  using namespace v2;
  Int a = 1;
  Int b = 2;
  b = a;
  Int c = a + b;
  std::cout << c << std::endl;

  // 由于定义类型转换操作符，这句可以编译通过
  int d = c;
}

int main()
{
  demo1();
  demo2();
  return 0;
}
```

### **二、 可调用对象详解**

#### **2.1 可调用对象类型**

1. **普通函数**
2. **函数指针**
3. **重载operator()的类对象（函数对象）**
4. **lambda表达式**
5. **std::function对象**

#### **2.2 函数对象实现**

```cpp
class Callable {
public:
    // 函数调用运算符重载
    void operator()() { cout << "Function call" << endl; }
    bool operator()(int a, int b) { return a < b; }
    
    // 静态成员函数
    static bool compare(int a, int b) { return a > b; }
};
```

#### **2.3 实际应用场景**

**排序算法示例：**

```cpp
int main() {
    vector<int> v = {5, 3, 7, 1};
    
    // 使用函数对象
    Callable comp;
    sort(v.begin(), v.end(), comp);
    
    // 使用lambda表达式
    sort(v.begin(), v.end(), [](int a, int b) {
        return a > b;
    });
    
    // 使用静态成员函数
    sort(v.begin(), v.end(), Callable::compare);
}
```

**多线程示例：**

```cpp
void threadDemo() {
    Callable c;
    
    // 使用成员函数
    thread t1(&Callable::sum, &c, 100);
    
    // 使用函数对象
    thread t2(c);
    
    // 使用lambda
    thread t3([](){
        cout << "Lambda thread" << endl;
    });
}
```

**附录：测试代码**

```cpp
void testAll() {
    // 操作符重载测试
    Int a = 10;
    Int b = a + 5;
    cout << "Result: " << b << endl;
    
    // 可调用对象测试
    Callable callable;
    callable();
    cout << "Compare: " << callable(3, 5) << endl;
}
```

#### **2.4 最佳实践建议**

1. **算术运算符**应定义为非成员函数以支持对称性
2. **赋值运算符**必须定义为成员函数
3. **流运算符**应定义为友元非成员函数
4. **函数对象**应设计为无状态（或明确管理状态）
5. **lambda表达式**适合简单的一次性操作

#### **2.5完整示例代码**

```cpp
/*
可调用对象就是可以用括号并传参数调用的东西
函数就是可调用对象
类对象重载括号运算符 operator() 也可以成为可调用对象
*/

#include <iostream>
#include <algorithm>
#include <thread>

// 关于算法库和线程库的细节，后面再说

class CallableClass
{
public:
  void test()
  {
    std::cout << "test" << std::endl;
  }

  // 重载 operator() 使得该类成为可调用对象
  void operator()()
  {
    std::cout << "Callable" << std::endl;
  }

  bool operator()(int l, int r)
  {
    return l < r;
  }

  static bool compare(int l, int r)
  {
    return l > r;
  }

  void operator()(int start)
  {
    int sum = 0;
    for (int i = start; i <= start + 100; ++i)
    {
      sum += i;
    }
    std::cout << sum << std::endl;
  }

  void sum(int start)
  {
    int sum = 0;
    for (int i = start; i <= start + 100; ++i)
    {
      sum += i;
    }
    std::cout << sum << std::endl;
  }
};

void thread_func(int start)
{
  int sum = 0;
  for (int i = start; i <= start + 100; ++i)
  {
    sum += i;
  }
  std::cout << sum << std::endl;
}

bool compare(int l, int r)
{
  // 左边的数大于右边的数是排序的依据，只有满足这个要求，函数才返回 true
  return l > r;
}

int main()
{
  CallableClass c;
  c.test();
  c(); // 调用 operator()

  // 使用 auto 自动推断类型，因为 lambda 表达式的类型比较复杂
  // 本质上是一个重载了 operator() 的类
  auto lambda_compare = [](int l, int r) -> bool
  {
    return l > r;
  };

  int array[10] { 2, 4, 5, 6, 9, 8, 1, 3, 7, 0 };
  // std::sort
  // 第一个参数是数组的首元素的地址
  // 第二个参数是数组最后一个元素的下一个位置的地址
  // 第三个参数是一个可调用对象，接受两个参数，参数类型是要排序的元素的类型，返回bool值
  // 使用普通函数作为可调用对象
  std::sort(array, array + 10, compare);
  // 使用重载了operator()的类实例作为可调用对象
  std::sort(array, array + 10, c);
  // 使用类的静态函数作为可调用对象
  std::sort(array, array + 10, CallableClass::compare);
  // 使用 lambda 表达式（左值）作为可调用对象
  std::sort(array, array + 10, lambda_compare);
  // 就地写一个 lambda 表达式（右值）作为可调用对象
  std::sort(array, array + 10, [](int l, int r) { return l > r; });

  // C++11新语法 范围for
  for (int i : array)
  {
    std::cout << i << ' ';
  }
  std::cout << std::endl;

  // 使用类实例和非静态成员函数作为可调用对象
  std::thread t1(&CallableClass::sum, c, 1);
  t1.join();
  // 使用重载了operator()的类实例作为可调用对象
  std::thread t2(c, 1);
  t2.join();
  // 使用普通函数作为可调用对象
  std::thread t3(thread_func, 1);
  t3.join();
  // 同样，可以使用 lambda 表达式作为可调用对象

  return 0;
}
```

## **Day8-2 拓展：C++ 中创建线程并传入类类型的正确写法**

在 C++ 中创建线程并传入类类型有几种正确的方式，主要取决于你想如何传递类实例以及如何使用它。以下是几种常见的正确写法：

### **1. 传递成员函数（使用对象指针）**

```cpp
#include <iostream>
#include <thread>

class MyClass {
public:
    void memberFunction(int x) {
        std::cout << "Member function called with " << x 
                  << " (thread ID: " << std::this_thread::get_id() << ")\n";
    }
};

int main() {
    MyClass obj;
    std::thread t(&MyClass::memberFunction, &obj, 10);
    t.join();
    return 0;
}
```

### **2. 传递 lambda 表达式捕获对象**

```cpp
#include <iostream>
#include <thread>

class MyClass {
public:
    void operator()(int x) const {
        std::cout << "Functor called with " << x 
                  << " (thread ID: " << std::this_thread::get_id() << ")\n";
    }
};

int main() {
    MyClass obj;
    std::thread t([&obj]() { obj(20); });
    t.join();
    return 0;
}
```

### **3. 传递临时对象（移动语义）**

```cpp
#include <iostream>
#include <thread>
#include <memory>

class MyClass {
public:
    void operator()() const {
        std::cout << "Temporary object called"
                  << " (thread ID: " << std::this_thread::get_id() << ")\n";
    }
};

int main() {
    std::thread t(MyClass());  // 注意：这里可能会被解析为函数声明
    // 更好的写法：
    // std::thread t{MyClass()};
    // 或
    // std::thread t((MyClass()));
    t.join();
    return 0;
}
```

### **4. 使用 std::ref 传递引用**

```cpp
#include <iostream>
#include <thread>
#include <functional>

class MyClass {
public:
    void operator()() {
        std::cout << "Object called by reference"
                  << " (thread ID: " << std::this_thread::get_id() << ")\n";
    }
};

int main() {
    MyClass obj;
    std::thread t(std::ref(obj));
    t.join();
    return 0;
}
```

### **5. 使用智能指针**

```cpp
#include <iostream>
#include <thread>
#include <memory>

class MyClass {
public:
    void operator()() {
        std::cout << "Shared object called"
                  << " (thread ID: " << std::this_thread::get_id() << ")\n";
    }
};

int main() {
    auto obj = std::make_shared<MyClass>();
    std::thread t([obj]() { (*obj)(); });
    t.join();
    return 0;
}
```

### **注意事项**

1. 线程的生命周期必须长于或等于它所引用的对象
2. 如果需要共享对象，确保线程安全性（使用互斥锁等）
3. 避免悬垂引用（不要传递局部变量的引用给可能比它生命周期长的线程）
4. 考虑使用 join() 或 detach() 明确线程的结束方式

选择哪种方式取决于你的具体需求，如对象生命周期管理、是否需要修改对象状态等。

## **Day8-3 C++模板编程（2025.03.31）**

---

### **1. 模板基础概念**

**模板类型**：

1. 函数模板（生成模板函数）
2. 类模板（生成模板类）
3. 变量模板（C++14引入）

**核心特点**：

- 编译期代码生成
- 类型安全的多态
- 减少代码重复

### **2 .函数模板详解**

**基本语法**：

```cpp
template <typename T>  // 或 template <class T>
T max(T a, T b) {
    return a > b ? a : b;
}
```

**使用示例**：

```cpp
void template_demo() {
    cout << max(1, 2);          // 类型推导
    cout << max<double>(1.5, 2); // 显式指定类型
}
```

**数组处理技巧**：

```cpp
template <typename T, size_t N>
void printArray(T (&arr)[N]) {
    for (size_t i = 0; i < N; ++i) {
        cout << arr[i] << " ";
    }
}
```

### **3. 类模板实战**

**基本实现**：

```cpp
template <class T, size_t SIZE>
class MyArray {
public:
    T& operator[](size_t index) {
        return data[index];
    }
private:
    T data[SIZE];
};
```

**标准库对比**：

```cpp
void array_compare() {
    MyArray<int, 5> myArr;
    std::array<int, 5> stdArr;  // 更完善的实现
}
```

### **4 .模板高级特性**

**类型别名**：

```cpp
template<typename T>
using Vec = std::vector<T>;

Vec<int> v;  // 等价于 std::vector<int>
```

**非类型模板参数**：

```cpp
template<int N>
struct Factorial {
    static const int value = N * Factorial<N-1>::value;
};

template<>
struct Factorial<0> {
    static const int value = 1;
};
```

### **完整代码示例**
```cpp
#include <functional>
#include <algorithm>
#include <array>
#include <iostream>

/*
模板

模板的内容非常多，是类的好多倍，这里只做了解
感兴趣可以阅读《C++程序设计语言》有关模板的章节（比《C++ Primer》详细），足够掌握基本的模板用法
如果想成为模板专家，请阅读《C++ templates 第二版》，提供了所有的细节

写好模板更多的是奇技淫巧，以及对C和C++语言的详细掌握，那是类库编写者的工作
我们应该关注业务逻辑，主要的工具还是C++基础和面向对象的知识，以及标准库
*/

/*
函数模板和类模板成员函数的定义应放在头文件中
*/

/*
函数模板 或 模板函数
*/
template <typename T> // typename 可以用 class 替换
bool less(const T& l, const T& r)
{
  return l < r;
}
void less_demo()
{
  less(1, 2); // T 为 int
  less(1.0, 2.0); // T 为 double
  less<std::string>("abc", "abd"); // 强制 T 为 std::string
}
/*
这个例子充当排序的可调用对象
其实标准库有实现好的 std::less 在头文件 <functional>
只不过它是用类重载operator()实现的
*/
void i_don_t_want_to_reimplement_less()
{
  int a[10]{ 0, 9, 8, 7, 6, 5, 4, 3, 2, 1 };
  // std::begin(a) == a
  // std::end(a) == a + 10
  // std::less<int>() 构造一个右值，它是一个可调用对象
  std::sort(std::begin(a), std::end(a), std::less<int>());
}
// 以上的 std::end 是怎么实现的呢，它怎么知道要 + 10，问题是如何获取数组的尺寸
void array_type()
{
  int a[10]{};
  // 对于强类型的C++而言，数组的尺寸也是类型的一部分
  using T = decltype(a); // T 是 int[10];

  // 如何声明数组的引用，不知道怎么写就借助 auto 的力量
  auto& ra = a; // ra 的类型为 int (&)[10]，即绑定到数组的引用
}
// 打印数组的尺寸，这里体现了模板在编译时的强大
template <typename T, unsigned int ARRAY_SIZE> // T 称为类型参数，ARRAY_SIZE称为非类型参数
void print_array_size(T (&)[ARRAY_SIZE]) // 用引用绑定到数组，尺寸使用非类型参数
{
  std::cout << "The size of array is: " << ARRAY_SIZE << std::endl;
}

int main()
{
  int a[10];
  print_array_size(a);

  // 抛开模板，怎么做？
  int array_size1 = sizeof(a) / sizeof(*a);
  // 或
  int array_size2 = sizeof(a) / sizeof(a[0]);
}

/*
类模板 或 模板类
*/
template <class T, unsigned int SIZE>
class MyArray
{
};
void myarray_demo()
{
  MyArray<int, 10> array;
}
/*
标准库实现了std::array
*/
void stdarray_demo()
{
  std::array<int, 10> array;
}

/*
using 的用法
1. 引用命名空间里的东西 比如 using namespace std;
2. 声明类型别名，比如 using T = decltype(main);
*/

```

## **Day8-4 C++语法知识复习（2025.03.31）**
---

### **1. 运算符重载回顾**

**常见重载场景**：

| 运算符    | 典型应用 | 返回值建议 |
| --------- | -------- | ---------- |
| `+ - * /` | 数学运算 | 值类型     |
| `=`       | 赋值操作 | 左值引用   |
| `<< >>`   | 流操作   | 流引用     |
| `()`      | 函数对象 | 任意       |
| `[]`      | 容器访问 | 引用       |

### **2.现代枚举系统**

**传统枚举问题**：

```cpp
enum Color { Red, Green };  // 污染全局命名空间
int Red = 5;  // 冲突！
```

**枚举类优势**：

```cpp
enum class Weekday : uint8_t { 
    Monday = 1,
    Tuesday  // 自动递增
};

void use_enum() {
    Weekday day = Weekday::Monday;
    uint8_t value = static_cast<uint8_t>(day);  // 需要显式转换
}
```

### **3. 断言机制详解**

**断言类型对比**：

| 类型            | 检查时机 | 头文件      | 示例                            |
| --------------- | -------- | ----------- | ------------------------------- |
| `assert`        | 运行时   | `<cassert>` | `assert(ptr != nullptr)`        |
| `static_assert` | 编译时   | 语言内置    | `static_assert(sizeof(int)==4)` |

**最佳实践**：

```cpp
constexpr int BUFFER_SIZE = 1024;
static_assert(BUFFER_SIZE > 0, "Buffer size must be positive");

void safe_operation() {
    int* ptr = get_pointer();
    assert(ptr && "Pointer cannot be null");
}
```

### **4. 数值字面量增强**

**现代字面量语法**：

```cpp
int decimal = 1'000'000;    // 千分位分隔
int hex = 0xFF'FF'FF;       // 颜色值
int binary = 0b1010'0101;   // 二进制
double sci = 1.23e-10;      // 科学计数法
```

### **5. 字符系统**

**字符类型对比**：

| 类型       | 大小    | 编码    | 字符串类型       | 输出流       |
| ---------- | ------- | ------- | ---------------- | ------------ |
| `char`     | 1字节   | ASCII   | `std::string`    | `std::cout`  |
| `wchar_t`  | 2/4字节 | Unicode | `std::wstring`   | `std::wcout` |
| `char16_t` | 2字节   | UTF-16  | `std::u16string` | -            |
| `char32_t` | 4字节   | UTF-32  | `std::u32string` | -            |

### **6.附录：模板元编程入门**

```cpp
// 编译期计算斐波那契数列
template<int N>
struct Fibonacci {
    static const int value = Fibonacci<N-1>::value + Fibonacci<N-2>::value;
};

template<>
struct Fibonacci<0> { static const int value = 0; };

template<>
struct Fibonacci<1> { static const int value = 1; };

static_assert(Fibonacci<5>::value == 5, "Compile-time check");
```

### **7. 完整测试代码**

```cpp
#include <cassert>
#include <type_traits>

/*
查漏补缺
*/

/*
运算符重载存在于C++的每个角落，要习惯
  1. + - * / 数值运算符，服务于数学类型
  2. = 赋值运算符，比如拷贝赋值，移动赋值，或者其他类型向该类型赋值
  3. << >> 流输出和流输入，多用于打印，或转为字符串
  4. () 可调用对象，供容器、算法、线程等库使用
  5. [] 下标运算符，供容器使用，比如std::map(有序树)，std::unordered_map(哈希表)有重载，用于访问键值对
  6. -> ++ -- 实现行为像指针的类，比如智能指针，迭代器
  7. new delete 自己实现内存分配和释放，除非你是内存管理专家
  8*. operator"" 可以为数学值添加“单位” https://zhuanlan.zhihu.com/p/111369693
*/

/*
数值类型补充

C++14起，可以使用二进制数值字面值 以0b打头。
常用的还有十六进制，以0x打头。
*/
int bin_a = 0b0101;
int hex_a = 0xFF;
/*
数值字面值可以添加 ' 以提高可读性
*/
int bin_b = 0b0101'0101;
int b = 12'3456'7890;


/*
枚举
enum关键字，本质定义了一个命名的常量
也称为 弱枚举，
*/
enum Color // 默认用 int 实现，可以修改
{
  Red, // 默认从 0 开始，可以修改
  Blue, // 自动从上一个累加 1，如果上一个是 0，该值为 1
};
/*
枚举类
enum class关键字，定义了有范围的枚举，本质加了命名空间，以及增强了类型（不可以隐式转换为实现类型）
也称为 强枚举，范围枚举
*/
enum class Weekend : unsigned short // 修改默认实现类型
{
  Saturday = 6,
  Sunday, // 7
};
// 编译器实际上把该强枚举翻译为命名空间里的常量
namespace _Weekend
{
  const unsigned short Saturday = 6;
  const unsigned short Sunday = 7;
}
void enum_demo()
{
  Color r = Color::Red;
  // underlying_type_t 可以查看枚举和枚举类的实现类型
  using Tc = std::underlying_type_t<Color>; // int
  int ir = r; // 隐式转为 整型

  Weekend w = Weekend::Saturday;
  using Tw = std::underlying_type_t<Weekend>; // unsigned short
  int iw = (int)w; // 无法隐式转换，需要显式转换

  // 枚举多见于 if else / switch case 做匹配
  switch (w)
  {
  case Weekend::Saturday:
    // ...
    break;
  case Weekend::Sunday:
    // ...
    break;
  }
}

/*
窄字符与宽字符
窄字符，ANSI编码，即普通 ASCII 字符，用 char 表示，对应的C++字符串为 std::string，对应的标准输出为 std::cout
宽字符，Unicode编码，用 wchar_t 表示，对应的C++字符串为 std::wstring，对应的标准输出为 std::wcout
*/

/*
断言 用来写测试
包含头文件 <cassert>
*/
void assert_test()
{
  // 运行时断言
  int a = 1; // 变量，运行时赋值
  assert(a == 1);
  // 变量不可以做栈数组的尺寸，但可以做堆数组的

  // 编译时断言
  const int b = 1; // 使用 const 声明常量，编译时赋值
  static_assert(b == 1);
  int array1[b]; // 常量才能做栈数组的尺寸
  constexpr int c = 1; // 使用 constexpr 声明编译时常量（比const更强的编译时要求），编译时赋值
  static_assert(c == 1);
  int array2[c]; // 编译时常量更有资格做栈数组的尺寸
}

```

---

## Day9 数据结构学习笔记（2025.04.01）

### 一、C++基础快速回顾

- **内存模型**
  - 栈（stack）与堆（heap）分配
  - 指针与引用（左值引用、右值引用、const 引用）
- **类型系统**
  - 内置类型与自定义类型
  - 构造与析构（RAII）
  - 拷贝与赋值（深拷贝/浅拷贝，共享/独占语义）
  - 移动语义（C++11）
  - 操作符重载
  - 继承与多态（虚函数、RTTI）
- **模板**
  - 泛型编程基础
  - 模板函数与模板类

------

### 二、STL（标准模板库）

STL由以下五个核心部分组成：

1. **容器（Containers）**
   - 存储和管理数据结构（如数组、链表、哈希表、树等）
   - 常见限制：不能使用引用类型或函数作为元素
2. **迭代器（Iterators）**
   - 提供统一方式访问容器元素
   - 本质行为类似指针（重载 `*`, `->`, `++`, `--` 等）
3. **算法（Algorithms）**
   - 提供常见操作如排序、查找、复制、删除等
   - 与容器无关，仅依赖迭代器
4. **函数对象与适配器**
   - 如 `std::less`，可定制算法行为
5. **工具类与API封装**
   - 如智能指针、时间、线程、文件系统等

------

### 三、常见容器及其对应的数据结构

| 容器                       | 数据结构       | 特点                       |
| -------------------------- | -------------- | -------------------------- |
| `std::array`               | 固定大小数组   | 编译期大小，栈分配         |
| `std::vector`              | 动态数组       | 支持随机访问，自动扩容     |
| `std::forward_list`        | 单向链表       | 内存使用少，不能反向遍历   |
| `std::list`                | 双向链表       | 可双向遍历，插入删除效率高 |
| `std::set/map`             | 红黑树（有序） | 元素有序，插入删除O(logn)  |
| `std::unordered_set/map`   | 哈希表（无序） | 查询效率高，O(1)平均复杂度 |
| `std::stack`               | 栈（适配器）   | LIFO，默认基于`deque`实现  |
| `std::queue`               | 队列（适配器） | FIFO，默认基于`deque`实现  |
| `std::priority_queue`      | 堆（适配器）   | 默认大顶堆                 |
| `std::deque`               | 双端队列       | 头尾高效插入删除           |
| `std::tuple` / `std::pair` | 异构数据结构   | 用于存储不同类型的多个值   |

------

### 四、容器操作演示

#### 1. 基本容器使用

```cpp
std::array<int, 10> arr = {};      // 固定数组
std::vector<int> vec;              // 动态数组
std::forward_list<int> flist;      // 单向链表
std::list<int> dlist;              // 双向链表
std::set<int> s;                   // 红黑树（有序）
std::unordered_set<int> uset;      // 哈希集合（无序）
std::map<int, std::string> m;      // 红黑树（键值对）
std::unordered_map<int, std::string> umap; // 哈希键值对
std::stack<int> st;                // 栈（基于 deque）
std::queue<int> q;                 // 队列（基于 deque）
std::deque<int> dq;                // 双端队列
```

#### 2. 异构类型容器

```cpp
std::tuple<int, double, float> t;
std::pair<int, std::string> p;
```

------

### 五、迭代器详解

#### 特点

- 类似指针，可使用 `*` 取值、`->` 访问成员、`++` 移动
- `begin()`：指向首元素；`end()`：指向末元素之后的位置
- 容器变化后迭代器可能失效（如 `resize()`）

#### 示例

```cpp
std::vector<int> v = {1, 2, 3};
auto it = v.begin();
std::cout << *it; // 1
v.resize(100);    // it 失效
it = v.begin();   // 重新获取
```

#### 用户自定义结构体访问成员

```cpp
struct RAE { double R, A, E; };
std::vector<RAE> raes;
raes.push_back({1.0, 2.0, 3.0});
raes.begin()->R = 100.0; // 使用 operator-> 访问
```

------

### 六、算法库与排序演示

#### STL算法特点

- 以迭代器为输入
- 解耦容器与算法实现
- 多数算法在 `<algorithm>` 中定义

#### 示例：排序

```cpp
std::array<int, 10> arr = {9, 7, 8, 2, 5, 4, 3, 1, 2, 0};
std::sort(arr.begin(), arr.end(), std::less<int>());
// 默认升序排序，std::less<int>() 可省略

for (int i : arr) {
    std::cout << i << " ";
}
```

#### C++20 ranges（了解）

- 新增 `ranges::sort`, `views::filter`, `views::transform` 等函数式风格
- 示例：`ranges::sort(arr);`（需要头文件 `<ranges>`）

------

### 七、完整测试代码

```c++
#include <iostream>
#include <vector>
#include <array>
#include <forward_list>
#include <list>
#include <set>
#include <map>
#include <unordered_set>
#include <unordered_map>
#include <stack>
#include <queue>
#include <deque>

#include <algorithm>//std::sort
#include <functional>//std::less

#include <tuple>

/*
C++基础快速回顾
	指针
	引用（左值，右值，常量）
	栈
	堆
	内置类型
	自定义类型
	1. 构造 析构 RAII
	2. 拷贝 独占（深拷贝）共享（浅拷贝）
	3. 移动
	4. 操作符重载
	5. 继承
	6. 多态 RTTI
	模板
	查漏补缺
*/



/*
STL: standard template library
标准模板库，简称标准库
标准库包含
1. 容器，是可以放进容器的元素的集合（什么不能做容器的元素类型？引用和函数）
2. 迭代器，提供一种访问不同容器元素的统一接口，隐藏了容器组织元素的数据结构细节
3. 算法，基于迭代器实现，以统一接口操作不同容器，比如排序算法
4. 跨平台的操作系统API封装，比如文件系统，IO操作，区域化，时间，线程等
5. 通用工具和其他，比如智能指针用于帮助自动释放堆内存，同时提供独占和共享语义
*/

/*
容器实现了各种数据结构：
- 不变的数组，array
- 动态数组，vector
- 字符串，string / wstring，可看成 vector<char> / vector<wchar_t>
- 单向链表，forward_list
- 双向链表，list
- 二叉平衡搜索树（红黑树），set & map
- 哈希表，unordered_set & unordered_map
- 双端队列，deque
- 栈，stack，容器适配器，使用 deque 实现
- 队列，deque，容器适配器，使用 deque 实现
- 堆，最小/大堆，也称优先队列，priority_queue，容器适配器，使用 vector 实现
- 异构体，tuple

常见数据结构的复杂度：
1. 数组 索引 O(1) 插入删除 O(n)
2. 链表 索引 O(n) 插入删除 O(1)
3. 红黑树（有序） 索引 O(logn) 插入删除 O(logn)
4. 哈希（无序） 索引 O(1) 插入删除 O(n)

按所需的数据结构选择恰当的容器
*/

/*
迭代器
	一种设计模式，用于隐藏容器内元素的组织方式，以一种统一的接口来访问
	行为类似指针，重载了 operator-> ，operator++ 等
	注意：之前赋值的迭代器在容器发生改变后可能会失效，指向非法内存，需要重新取迭代器
*/
static void iterator_demo()
{
	std::vector<int> v{ 1, 2, 3 };
	auto iter = v.begin(); // *iter == 1 == v[0]
	iter++; // *iter == 2 == v[1]
	iter = v.end(); // 指向最后一个元素的下一个位置，禁止对其使用operator*解引用！！！
	iter--; // *iter == 3 == v[2]
	// 看看v的第一个元素的地址在哪里，那是一个堆地址
	std::cout << v.data() << ' ';
	// 或者
	// 由于 cout 无法打印迭代器，因为迭代器没实现operator<<
	// 迂回一下，对迭代器解引用再取地址，可得到普通指针int*
	int* p = &(*v.begin());
	std::cout << p << ' ';
	// 或者
	// 回想下数组名其实就是个指针，那迭代器表现得像指针，是不是也可以看成数组名？
	// 的确，可以使用下标作用到迭代器上（只有数组的迭代器有这个特权）
	// 注意[]的优先级比&高，先调用[]然后&
	std::cout << &v.begin()[0] << ' '; // 不放心就加括号 &(v.begin()[0])
	// 或者
	// v本身就是个数组，可以直接取下标
	std::cout << &v[0] << std::endl;

	// 把v的尺寸改为100个元素，势必引起迭代器失效
	// 需要在堆上重新找容纳100个int的空间，使用new分配
	// 然后把当前这3个int拷过去
	// 最后释放这3个的int的空间（形成内存碎片），不可以再访问，即不可以再操作原来的迭代器
	// 前3个是1，2，3，剩下全部填0
	v.resize(100);
	// 再看看第一个元素在哪，和上面已经不一样了
	std::cout << v.data() << std::endl;

	// 缩小改为3个元素，没有引发迭代器失效（出于性能考虑）
	// 后面97个元素的空间还给操作系统了（形成内存碎片），不可以再访问
	// 但是，不要管缩小会不会引起迭代器失效，只要修改了容器，就总是重新取迭代器
	v.resize(3);
	std::cout << v.data() << std::endl;

	struct RAE
	{
		double R;
		double A;
		double E;
	};
	std::vector<RAE> raes;
	raes.push_back({ 1.0, 2.0, 3.0 });
	// 由于重载了operator-> 迭代器可以像使用指针一样访问成员变量
	raes.begin()->R = 100.0;
}

static std::ostream& operator<<(std::ostream& os, const std::vector<int>& v)
{
	// 使用迭代器遍历容器，其实编译器看到 for (int i : v) 就是转成以下代码
	for (auto iter = v.cbegin(); iter != v.cend(); ++iter)
	{
		os << *iter << ' ';
	}
	return os;
}

int main()
{
	//不变的数组

	// 修正：避免越界访问
	std::array<int, 10> arr1 = {};  // 初始化为全0
	arr1[0] = 1;                    // 合法索引 0~9
	arr1[9] = 1;                    // 合法
	// arr1[10] = 1;                // 错误：注释掉越界访问  保持和原生C语言一样的语义，不做任何检查
	// arr1.at(10) = 1;             // 错误：注释掉越界访问  做检查，运行时报错

	//动态数组
	std::vector<int> vector;
	vector.push_back(1);

	//单向链表
	std::forward_list<int> forwardList;
	forwardList.push_front(1);

	//双向链表
	std::list<int> list;
	list.push_back(1);
	list.push_front(1);

	//平衡树,(有序)单个元素
	std::set<int> set;//默认从小到大排序
	set.insert(1);

	//平衡树,(默认有序) 键值对 ，按照键的大小来排序
	std::map<int, std::string> map;
	map[0] = "zhangsan";
	map[1] = "lisi";

	//无序关联式容器
	//哈西（无序）
	std::unordered_set<int>  unorder_set;
	unorder_set.insert(1);

	//哈希 （无序）键值对
	std::unordered_map<int, std::string> unordered_map;
	unordered_map[0] = "abc";
	unordered_map[1] = "def";

	//栈（使用双端队列实现）
	std::stack<int> stack;
	/*入栈*/
	stack.push(1);
	/*出栈*/
	int top = stack.top();
	stack.pop();

	//队列
	std::queue<int> queue;
	queue.push(1);
	queue.front();
	queue.pop();

	//双端队列
	std::deque<int> deque;
	deque.push_back(1);
	deque.push_front(2);
	deque.pop_back();
	deque.pop_front();

	//异构 ： 支持不同类型
	struct Tuple {
		int a;
		double b;
		float c;

	};

	std::tuple<int, double, float> tuple;//元组
	std::get<0>(tuple) = 1;

	//异构之std::pair,只存放两个值，是map unorder_map的存储单元
	//键值对
	std::pair<int, double> pair;

	//迭代器 设计模式
	//目的：隐藏容器中元素的组织形式，用提供统一的接口来访问
	//最常见的方法：eg:获取第一个元素begin ,访问下一个元素next  
	//迭代器是行为指针的类型
	auto firstElement = vector.begin();
	//访问vector容器中元素的几种方式
	int a = *firstElement;
	int b = vector[0];
	int c = vector.at(0);
	firstElement++;

	auto firstElement2 = vector.begin();
	firstElement2++;

	/*
算法
	在C++20之前，算法库一般接收迭代器
	使用 begin 获取起始迭代器，指向第一个元素
	end 获取末迭代器，指向最后一个元素的下一个位置

	函数式编程是C++20的一个重要发展方向
	C++20起，引入了 ranges 库，为算法提供了不同的访问方式
	C++23继续完善 ranges 库
	C++20及之后的内容，权当了解最新的发展，可以完全不管
*/

	//算法
	/*由于迭代器隐藏了容器的具体数据结构，算法进而使用迭代器，从而使用统一的方式，操作不同的容器*/
	std::array<int, 10> arrayRandom= { 9,7 ,8 ,2 ,5 ,4 ,3 ,1 ,2 ,0};
	//arrayRandom.end() 是指向容器中最后一个元素的下一个位置
	// 排序（升序，std::less<int>() 是默认行为，可省略）
	std::sort(arrayRandom.begin(), arrayRandom.end(), std::less<int>());

	// 打印排序后的元素
	for (auto i : arrayRandom) {
		std::cout << i << " "; // 输出每个元素，用空格分隔
	}
	std::cout << std::endl; // 换行

	return 0;
}
```



### 八、总结

- STL 提供统一的数据结构与算法接口，核心三件套是容器、迭代器、算法
- 学会根据实际使用场景（性能、顺序、插入/删除频率）选择合适容器
- 操作容器时，要注意迭代器失效问题
- 算法库配合迭代器提供高效、可复用的操作方式

------



## Day10 标准模板库学习笔记（2025.04.02）

### 一、函数和数组能否放入 STL 容器？

#### 1. 引用不能直接作为容器元素类型

C++ 的容器如 `std::vector` 不支持存储引用类型（例如 `int&`），因为引用并非对象本身，不能被复制或赋值。

```cpp
std::vector<int&> rv; // ❌ 编译失败
```

##### ✅ 推荐做法：使用 `std::reference_wrapper<T>`

标准库提供了 `std::reference_wrapper` 类模板，它可以将引用包装为对象，从而实现“间接”存储引用。

```cpp
#include <functional>
std::vector<std::reference_wrapper<int>> rv1;
rv1.push_back(i); // i 是 int 类型变量
```

你也可以自定义类似的封装类，但通常没必要：

```cpp
template <typename T>
class my_reference_wrapper {
  T& r;
public:
  my_reference_wrapper(T& i) : r(i) {}
};
```

#### 2. 函数不能直接作为容器元素类型

函数名本身不是对象，而是**函数类型**，不能直接作为 `std::vector` 的元素类型。

```cpp
std::vector<decltype(add)> fv; // ❌ 编译失败
```

#### ✅ 推荐做法一：使用函数指针类型

函数名在赋值或传参时会自动“衰变”（decay）为函数指针。

```cpp
auto f = add;  // 类型为 void(*)()
using func_ptr = decltype(f);

std::vector<func_ptr> fv1;
fv1.push_back(add);
fv1.push_back(minus);
```

#### ✅ 推荐做法二：使用 `std::function`

`std::function` 是更通用、灵活的可调用对象封装器，可以容纳函数、函数指针、Lambda 表达式、函数对象等。

```cpp
std::vector<std::function<void()>> fv2;
fv2.push_back(add);
fv2.push_back(minus);
fv2.push_back([](){});
fv2.push_back(Callable{}); // 假设 Callable 重载了 operator()
```

#### 补充知识：函数和数组的“衰变”行为

C++ 中，**数组和函数**在很多场景下都会自动转换为**指针类型**，这种现象称为“衰变”（decay）。

```cpp
int array[10];
auto p1 = array;     // int*
auto&& r1 = array;   // 引用仍保留 int[10] 类型
```

衰变行为可以用 `<type_traits>` 里的 `std::decay_t<T>` 模拟：

```cpp
static_assert(std::is_same_v<decltype(array), std::remove_reference_t<decltype(r1)>>);
static_assert(std::is_same_v<decltype(p1), std::decay_t<decltype(array)>>);
```

函数也会衰变为函数指针：

```cpp
static_assert(std::is_same_v<decltype(add), std::remove_reference_t<decltype(function_ref)>>);
```

------

### 二、数组能否放入容器？

#### ❌ 直接存放原生数组类型 `T[N]` 不被支持

```cpp
std::vector<int[10]> v;  // 编译失败
```

因为数组不能被拷贝或赋值。

#### ✅ 推荐做法一：使用数组指针

```cpp
std::vector<int*> v1;
v1.push_back(array);  // array 是 int[10]
```

但这种方式存在**数组退化为指针**的问题，可能引发不安全行为。

#### ✅ 推荐做法二（更推荐）：使用 `std::array`

`std::array` 是标准库提供的定长数组类型，兼具 C 数组的性能与 STL 容器的接口。

```cpp
std::vector<std::array<int, 10>> v2;
v2.push_back(std::to_array(array)); // C++20 提供的安全转换
```

------

### 三、测试代码

```c++
#include <type_traits>
#include <functional>
#include <vector>
#include <algorithm>
#include <iostream>
#include <array>

void add()
{
  std::cout << "add" << std::endl;
}

void minus()
{
  std::cout << "minus" << std::endl;
}

// 普通类定义可以写在函数里，模板类不行
template <typename T>
class my_reference_wrapper
{
  T& r;
public:
  my_reference_wrapper(T& i) : r(i) {}
};

/*
容器无法容纳引用和函数？
*/
void container_can_not_contain_reference_and_function()
{
  int i = 1;
  int& ri = i;
  int& ri1 = i;
  // 想把这两个引用放到容器里

  // 想把上面两个函数 add 和 minus 放到容器里

  //std::vector<int&> rv; // 编译出错
  //std::vector<decltype(add)> fv; // 编译出错

  // 怎么办...

  // 对于引用，标准库提供了个包装类，把引用藏到自定义类型里
  std::vector<std::reference_wrapper<int>> rv1;
  rv1.push_back(ri);
  rv1.push_back(ri1);
  // 自己写也可以
  std::vector<my_reference_wrapper<int>> rv2;
  rv2.push_back(ri);
  rv2.push_back(ri1);
  // 你永远也不需要这么做...
  // 要么把对象拷贝进容器，要么移动进容器，把引用放进去是做什么？？？

  // 但是函数对象是确实有放到容器里的需求
  // 这里只是没写对元素的类型，试着拷贝函数给一个变量，看看变量的类型
  auto f = add; // f 是个函数指针
  using func_pointer = decltype(f);
  // 所以元素可以用函数指针类型
  std::vector<func_pointer> fv1;
  fv1.push_back(add);
  fv1.push_back(minus);

  // 标准库提供了更好的方案 std::function
  // 首先指定的还是函数类型，而不是函数指针类型
  // 其次接收可调用对象，即函数和重载了operator()的类都可以放进容器，包括lambda
  std::vector<std::function<decltype(add)>> fv2;
  fv2.push_back(add);
  fv2.push_back(minus);
  class Callable
  {
  public:
    void operator()() {}
  };
  fv2.push_back(Callable());
  fv2.push_back([]() {});

  // 发生了什么？为什么函数赋值后成了函数指针？
  // 这里涉及C++比较麻烦的地方，为了保持和C语言的兼容性
  // 所谓 decay 衰变
  // C语言有数组和函数，数组和函数在赋值/传参时会自动衰变为指针
  // 这是C编译器的行为，C++编译器为了兼容C，无奈照搬了，这也导致了C++的类型系统有瑕疵
  // 模板专家很多时候就是在处理这种奇怪的兼容性问题
  // 这也是为什么数组名是个指针，而函数自动会转为函数指针的原因
  int array[10];
  auto array_decay = array; // 类型衰变为 int*
  auto&& array_ref = array; // 万能引用可以看到精确类型，其实任何引用都可以
  auto function_decay = add; // 类型衰变为 void(*)()
  auto&& function_ref = add; // 万能引用可以看到精确类型，其实任何引用都可以
  static_assert(std::is_same_v<
    decltype(array_decay),
    std::decay_t<decltype(array)>>); // 自动decay和手动调用decay得到一样的类型
  static_assert(std::is_same_v<
    decltype(function_decay),
    std::decay_t<decltype(add)>>); // 自动decay和手动调用decay得到一样的类型
  static_assert(std::is_same_v<
    decltype(array),
    std::remove_reference_t<decltype(array_ref)>>); // 把引用移除就得到原来的类型
  static_assert(std::is_same_v<
    decltype(add),
    std::remove_reference_t<decltype(function_ref)>>); // 把引用移除就得到原来的类型

  // C数组会衰变，还允许做元素，C++这帮人...
  std::vector<int[10]> array_array;

  // 但，怎么塞元素...
  //array_array.push_back(array); // 编译出错

  // 完蛋，没法塞进去...
  // 都不让塞为什么允许C数组做元素啊...

  // 要么妥协，使用衰变类型，即指针
  std::vector<int*> array_array1;
  array_array1.push_back(array);

  // 要么改成标准库的定长数组std::array，推荐做法
  std::vector<std::array<int, 10>> array_array2;
  // 使用 std::to_array 从C数组构造标准库数组
  array_array2.push_back(std::to_array(array));

  // 记住
  // 永远不需要把引用放进容器
  // 函数放进容器是有需求的，推荐用std::function代替函数指针
  // 数组放进容器也有需求，推荐用std::array代替指针
  // 碰到问题先去找标准库的解决方案，不要自己（瞎）实现
}

```

### 四、总结

| 数据类型      | 是否能放入 STL 容器 | 推荐做法                                |
| ------------- | ------------------- | --------------------------------------- |
| 引用 `T&`     | ❌                   | 使用 `std::reference_wrapper<T>`        |
| 函数 `void()` | ❌                   | 使用 `std::function<void()>` 或函数指针 |
| 数组 `T[N]`   | ❌                   | 使用 `std::array<T, N>` 或 `T*`         |

<img src="D:\QQ\Users\DOC\Desktop\Qt\容器中存储引用、函数和数组.png" style="zoom: 50%;" />

#### 实践建议

- 遇到“不能放入容器”的情况，**优先查标准库**是否已有合适的封装工具。
- 避免自己写低质量、重复的封装类，标准库几乎总有更好的方案。
- 多理解“类型衰变”在函数和数组上的行为，有助于掌握模板编程细节。



## Day11 移动语义回顾

### 一、移动语义基础概念

- **移动语义**：通过“转移资源所有权”而非“复制资源”，提升程序效率，尤其适用于临时对象或大对象的处理。
- **移动构造函数**和**移动赋值运算符**：C++11引入的特殊成员函数，分别用于从右值中构造对象或赋值给现有对象。
- **与拷贝语义的区别**：
  - 移动语义不会拷贝内存，只是指针赋值，效率更高。
  - 编译器不会自动生成移动构造和移动赋值，需要手动实现。

------

### 二、自定义 `String` 类的移动语义实现

```cpp
class String
{
    friend std::ostream& operator<<(std::ostream& os, const String& rhs);

public:
    // 默认构造函数
    String()
        : _pstr(nullptr) {
        cout << "String()" << endl;
    }

    // 构造函数：深拷贝字符串
    String(const char* pstr)
        : _pstr(new char[strlen(pstr) + 1]()) {
        cout << "String(const char* pstr)" << endl;
        strcpy_s(_pstr, strlen(pstr) + 1, pstr);
    }

    // 拷贝构造函数：深拷贝另一个 String 对象
    String(const String& rhs)
        : _pstr(new char[strlen(rhs._pstr) + 1]()) {
        cout << "String(const String& rhs)" << endl;
        strcpy_s(_pstr, strlen(rhs._pstr) + 1, rhs._pstr);
    }

    // 移动构造函数：资源转移，不再复制字符串内容
    String(String&& rhs)
        : _pstr(rhs._pstr) {
        cout << "String(String&& rhs)" << endl;
        rhs._pstr = nullptr;  // 清空源对象
    }

    // 拷贝赋值运算符
    String& operator=(const String& rhs) {
        cout << "String& operator=(const String& rhs)" << endl;
        if (this != &rhs) //1、防止自复制
        {
            delete[] _pstr;//2、释放左操作数
            //3、深拷贝
            _pstr = new char[strlen(rhs._pstr) + 1]();
            strcpy_s(_pstr, strlen(rhs._pstr) + 1, rhs._pstr);
        }
        //4、返回*this
        return *this;
    }

    // 移动赋值运算符
    String& operator=(String&& rhs) {
        cout << "String& operator=(String&& rhs)" << endl;
        if (this != &rhs)//1、自移动
        {
            delete[] _pstr;//2、释放左操作数
            //3、浅拷贝
            _pstr = rhs._pstr;
            rhs._pstr = nullptr;
        }
        //返回*this
        return *this;
    }

    ~String() {
        cout << "~String()" << endl;
        delete[] _pstr;
        _pstr = nullptr;
    }

private:
    char* _pstr;
};
```

#### 输出运算符重载：

```cpp
std::ostream& operator<<(std::ostream& os, const String& rhs)
{
    if (rhs._pstr) {
        os << rhs._pstr;
    }
    return os;
}
```

------

### 三、测试函数：验证移动与拷贝行为

```cpp
void test() {
    String s1("hello");
    cout << "s1 = " << s1 << endl;

    String s2 = s1;  // 拷贝构造
    cout << "s2 = " << s2 << endl;

    String s3 = "world";  // 隐式调用构造函数，然后移动构造
    //上面这条代码有个隐式转换的过程，"world" --> String("world")
	//String("world")是临时对象/匿名对象
    cout << "s3 = " << s3 << endl;

    String s4("hainan");
    cout << "s4 = " << s4 << endl;

    s4 = String("shangfa");  // 临时对象移动赋值
    cout << "s4 = " << s4 << endl;

    s4 = std::move(s4);  // 自移动，测试保护机制
    cout << "s4 = " << s4 << endl;//warning!使用已移动的 from 对象: s4 (lifetime.1)。
	//移动赋值运算符函数中没有判断if(this != &rhs)之前，
	// 打印s4的值，输出结果为空，因为在输出流重载运算符函数中，已经进行了判空操作，若指向为空，直接执行return 
	cout << "11111" << endl;

    /*std::move可以将左值转换为右值，实质上没有做任何移动，只是在底层做了强制转换static_cast<T &&>(lvalue)
	如果以后不想再使用某个左值，可以使用std::mobe将其转换为右值，以后就不再使用了*/
    s2 = std::move(s1);  // s1 被移动后已失效
    cout << "s1 = " << s1 << endl;
    cout << "s2 = " << s2 << endl;
}
```

------

### 四、左值与右值的补充说明

```cpp
void testRightValue()
{
	int a = 10;
	int b = 20;
	int* pflag = &a;
	string s1("hello");
	string s2("world");

	int& ref = a;
	//int& ref2 = 10;//error!非常量引用的初始值必须为左值
	const int& ref2 = 10;
	/*const左值引用既可以绑定到左值，也可以绑定到右值*/
	const int& ref3 = a;

	&a;//左值
	&b;//左值
	&pflag;//左值
	& *pflag;//左值
	&s1;//左值
	&s2;//左值
	//表达式必须为左值或函数指示符
	//&(a + b);//左值
	//&(s1 + s2);//左值

	&ref;//左值

	//C++11之前是不能识别右值的，C++11之后新增语法可以识别右值
	int&& rref = 10;//右值引用
	//int&& rref2 = b;//error!无法将右值引用绑定到左值

	//还有一个问题，左值引用是左值还是右值？
	&rref;//右值引用在此处是左值（右值引用在作为函数参数的时候，体现出来的是左值的含义）

}

//那么，右值引用可以是右值吗？(右值引用作为函数返回类型的时候是右值)
int&& func()
{
	return 10;
}

int main()
{
	test();
	//&func();//error!“&”要求左值
	return 0;
}
```

#### 右值引用作为函数返回值

```cpp
int&& func() {
    return 10;
}
```

右值引用返回的对象仍然是右值，不能取地址。

------

### 五、知识总结

| 类型                        | 是否可绑定左值 | 是否可绑定右值 | 备注                             |
| --------------------------- | -------------- | -------------- | -------------------------------- |
| 左值引用 (`T&`)             | ✅              | ❌              | 常用于一般变量引用               |
| const 左值引用 (`const T&`) | ✅              | ✅              | 可以绑定临时对象，常用于函数参数 |
| 右值引用 (`T&&`)            | ❌              | ✅              | 主要用于移动语义                 |

#### 如何区分左值与右值？

- **能否取地址（&）** 是判断左值的重要标准。
- **表达式结果是否具名或可持久存在**。

------

### 六、附加说明：关于 `std::move`

- `std::move(x)` 实质上是 `static_cast<T&&>(x)`，并不真正“移动”。
- 它只是告诉编译器：“我不再使用这个对象了，可以安全地‘偷走’资源”。
- 右值的存放位置（冷知识）：右值短暂的存在于栈上
、
