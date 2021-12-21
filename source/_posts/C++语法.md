---
title: C++语法
date: 2021-12-21 15:08:34
tags:
---
[TOC]
## VC 配置
C/C++：
    常规->附加包含目录：寻找#include<xxxx.h>中的xxxx.h的搜索目录（每一项对应一个文件夹XXXX，文件夹中包含了编译时所需的头文件，使用时直接#include<XXXX>即可）
链接器：
    常规->附加库目录：寻找.lib文件的搜索目录
    输入->附加依赖项：lib库（C++的库会把函数、类的声明放在*.h中，实现放在*.cpp或*.cc中。编译之后，*.cpp，*.cc，*.c会被打包成一个.lib文件，这样可以保护源代码）

附件依赖项可以不填，在代码中声明，会自动在“附加库目录”里找，lib、.h在build后就 不需要了，只在开发时需要
#pragma comment（lib, “xxx.lib”）

## 预处理指令
预处理指令是以#号开头的代码行。#号必须是该行除了任何空白字符外的第一个字符。#后是指令关键字，在关键字和#号之间允许存在任意个数的空白字符。整行语句构成了一条预处理指令，该指令将在编译器进行编译之前对源代码做某些转换。

预处理过程扫描源代码，对其进行初步的转换，产生新的源代码提供给编译器。

可见预处理过程先于编译器对源代码进行处理。在很多编程语言中，并没有任何内在的机制来完成如下一些功能：在编译时包含其他源文件、定义宏、根据条件决定编译时是否包含某些代码(防止重复包含某些文件)。

要完成这些工作，就需要使用预处理程序。尽管在目前绝大多数编译器都包含了预处理程序，但通常认为它们是独立于编译器的。预处理过程读入源代码，检查包含预处理指令的语句和宏定义，并对源代码进行响应的转换。预处理过程还会删除程序中的注释和多余的空白字符。

## 语法
### 基础类型(7种)
- 布尔 bool
- 字符 char
- 整型 int
- 浮点 float
- 双精度浮点 double
- 无类型 void
- 宽字符 wchar_t

其实 wchar_t 是这样来的：typedef short int wchar_t;
所以 wchar_t 实际上的空间是和 short int 一样。

一些基本类型可以使用一个或多个类型修饰符进行修饰：
- signed
- unsigned
- short
- long

> 注意：不同系统会有所差异，一字节为 8 位。
> 注意：默认情况下，int、short、long都是带符号的，即 signed。
> 注意：long int 8 个字节，int 都是 4 个字节，早期的 C 编译器定义了 long int 占用 4 个字节，int 占用 2 个字节，新版的 C/C++ 标准兼容了早期的这一设定。


### 复杂类型
#### 字符串
C++ 提供了以下两种类型的字符串表示形式：
- C 风格字符串
- C++ 引入的 string 类类型

***C风格字符串***
C 风格的字符串起源于 C 语言，并在 C++ 中继续得到支持。字符串实际上是使用 null 字符 \0 终止的一维字符数组。因此，一个以 null 结尾的字符串，包含了组成字符串的字符。

下面的声明和初始化创建了一个 RUNOOB 字符串。由于在数组的末尾存储了空字符，所以字符数组的大小比单词 RUNOOB 的字符数多一个。
> char site[7] = {'R', 'U', 'N', 'O', 'O', 'B', '\0'};

依据数组初始化规则，您可以把上面的语句写成以下语句：
> char site[] = "RUNOOB";

以下函数可用来操作以null结尾的字符串：  
1.strcpy(s1,s2);  
复制字符串s2到字符串s1  
2.strcat(s1,s2);  
连接字符串s2到字符串s1末尾，也可以直接使用+号  
```c++
string str1 = "runoob";
string str2 = "google";
string str = str1 + str2;
```
3.strlen(s1);  
返回字符串s1的长度，不包含结尾的null(\0)，另外sizeof会返回包含结尾null的长度  
4.strcmp(s1,s2);  
如果 s1 和 s2 是相同的，则返回 0；如果 s1<s2 则返回值小于 0；如果 s1>s2 则返回值大于 0。  
5.strchr(s1, ch);  
返回一个指针，指向字符串 s1 中字符 ch 的第一次出现的位置。  
6.strstr(s1, s2);  
返回一个指针，指向字符串 s1 中字符串 s2 的第一次出现的位置。  

***C++中的String类***
C++ 标准库提供了 string 类类型，支持上述所有的操作，另外还增加了其他更多的功能。  
其中有size方法
```c++
#include <iostream>
#include <string>
 
using namespace std;
 
int main ()
{
   string str1 = "runoob";
   string str2 = "google";
   string str3;
   int  len ;
 
   // 复制 str1 到 str3
   str3 = str1;
   cout << "str3 : " << str3 << endl;
 
   // 连接 str1 和 str2
   str3 = str1 + str2;
   cout << "str1 + str2 : " << str3 << endl;
 
   // 连接后，str3 的总长度
   len = str3.size();
   cout << "str3.size() :  " << len << endl;
 
   return 0;
}

```

#### 枚举类型
创建枚举，需要使用关键字 enum。枚举类型的一般形式为：
```c++
enum 枚举名{ 
     标识符[=整型常数], 
     标识符[=整型常数], 
... 
    标识符[=整型常数]
} 枚举变量;
```
如果枚举没有初始化, 即省掉"=整型常数"时, 则从第一个标识符开始。第一个名称的值为 0，第二个名称的值为 1，第三个名称的值为 2，以此类推。但是，您也可以给名称赋予一个特殊的值，只需要添加一个初始值即可。
例如，在下面的枚举中，green 的值为 5。在这里，blue 的值为 6，因为默认情况下，每个名称都会比它前面一个名称大 1，但 red 的值依然为 0。
```c++
enum color { red, green=5, blue };
```

### 自定义类型


### 关键字
#### typedef 取别名
可以使用 typedef 为一个已有的类型取一个新的名字。下面是使用 typedef 定义一个新类型的语法：
> typedef type newname; 

例如，下面的语句会告诉编译器，feet 是 int 的另一个名称：
> typedef int feet;

现在，下面的声明是完全合法的，它创建了一个整型变量 distance：
> feet distance;

#### extern 变量声明
可以在 C++ 程序中多次声明一个变量，但变量只能在某个文件、函数或代码块中被定义一次。

# 参考
## MFC是什么
mfc是古代微软自己包装了一层，方便开发图形界面程序，不是WindowsSDK的一部分，仅随visual studio发布。

## 头文件
### Windows.h
直接或间接包含了所有Windows常用的API, 属于Windows SDK的一部分。任何Windows程序归根究底都要用到。
Windows文件系统默认不区分大小写。

excpt.h – Exception handling  
stdarg.h – variable-argument functions (standard C header)  
windef.h – various macros and types  
winnt.h – various macros and types (for Windows NT)  
basetsd.h – various types  
guiddef.h – the GUID type  
ctype.h – character classification (standard C header)  
string.h – strings and buffers (standard C header)  
winbase.h – kernel32.dll: kernel services; advapi32.dll:kernel services(e.g. CreateProcessAsUser function), access control(e.g. AdjustTokenGroups function).  
winerror.h – Windows error codes   
wingdi.h – GDI (Graphics Device Interface)  
winuser.h – user32.dll: user services  
winnls.h – NLS (Native Language Support)  
wincon.h – console services  
winver.h – version information  
winreg.h – Windows registry  
winnetwk.h – WNet (Windows Networking)  
winsvc.h – Windows services and the SCM (Service Control Manager)  
imm.hhh – IME (Input Method Editor)  

上述头文件都是被Windows.h间接包含的
