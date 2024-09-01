# C语言

## 编程环境

## 基础语法

### 头文件

在C语言中，`#include`指令用于包含头文件，`#include <filename>`和`#include "filename"`的区别在于编译器搜索头文件的路径方式不同：

#### 1. `#include <filename>`

- **用途**: 用于包含**标准库**或**系统级**的头文件。
- **搜索路径**: 编译器首先在**系统默认的头文件目录**（通常是标准库路径，如`/usr/include`或编译器自带的头文件路径）中搜索指定的头文件。如果未找到，可能会查找其他预定义的目录。
- **常见用法**:
  ```c
  #include <stdio.h>
  #include <stdlib.h>
  ```
  这里，`stdio.h`和`stdlib.h`是C标准库的头文件，编译器会在标准库路径中找到它们。

#### 2. `#include "filename"`

- **用途**: 通常用于包含**自定义**或**项目内**的头文件。
- **搜索路径**: 编译器首先在**当前源文件所在的目录**（即工作目录）中搜索指定的头文件。如果未找到，再搜索系统默认的头文件目录（类似`#include <filename>`的搜索路径）。
- **常见用法**:
  ```c
  #include "myheader.h"
  ```
  如果有一个自定义的头文件`myheader.h`与当前源文件在同一目录，编译器会先在当前目录查找该文件。

#### 总结
- 使用`#include <filename>`：编译器搜索系统和标准库的头文件路径，用于标准或系统头文件。
- 使用`#include "filename"`：编译器先在当前目录搜索头文件，用于自定义或项目特定的头文件。

## 控制结构

## 数组

## 函数

传递参数时总是传递值（值拷贝），而不是引用。

1. **值传递（拷贝）**：
   - 当传递基本数据类型（如`int`、`float`、`char`等）或结构体时，传递的是这些数据的拷贝。函数内部对参数的修改不会影响到函数外部的原始数据。
   - 例如：
     ```c
     void modifyValue(int x) {
         x = 10;
     }
     
     int main() {
         int a = 5;
         modifyValue(a);
         // a 仍然是 5
     }
     ```

2. **指针传递（间接值传递）**：
   - 当传递指针时，传递的是指针的拷贝。虽然传递的是拷贝，但由于指针指向的是某个地址，通过这个地址可以间接修改指向的数据。因此，函数内部可以通过这个指针修改原始数据。
   - 例如：
     ```c
     void modifyPointerValue(int *x) {
         *x = 10;
     }
     
     int main() {
         int a = 5;
         modifyPointerValue(&a);
         // a 被修改为 10
     }
     ```

3. **指针的指针（用于动态分配内存或修改指针本身）**：
   - 当你希望函数能够修改指针本身（例如分配内存或改变指针指向的位置），需要传递指针的指针（`char**`、`int**`等）。这样函数内部可以通过解引用修改主函数中的指针。
   - 例如：
     ```c
     void allocateMemory(char **p) {
         *p = (char *)malloc(100);
         strcpy(*p, "hello world");
     }
     
     int main() {
         char *str = NULL;
         allocateMemory(&str);
         printf("%s\n", str);
         free(str);
     }
     ```



## 指针

## 结构体与联合体

### C和C++中的结构体不同点

c和c++ 中struct的主要区别是c中的struct不可以含有成员函数，而c++ 中的struct可以。c++ 中struct和class的主要区别在于默认的存取权限不同，struct默认为public ，而class默认为private

### 位域（bit-field）

在C语言中，位域用于定义结构体中的变量所占用的位数，而不是默认的字节数。位域的这种定义方式允许你指定一个字段只占用一定数量的二进制位。

例如：

```c
struct {
    unsigned char t : 4;
} myStruct;
```

在这个例子中，`t` 是一个`unsigned char`类型的位域变量，但它只占用4个位（半个字节）。剩下的4个位可以用于同一个字节中的其他位域字段。这种方式常用于内存有限或需要对数据进行位级控制的场景，比如嵌入式系统或协议实现。

### 结构体内存分配

结构体（`struct`）在内存中的字节分配方式主要取决于两个因素：结构体中各成员的类型大小和成员之间的**内存对齐**（memory alignment）规则。以下是结构体内存分配的一些关键点：

#### 1. 成员的顺序
结构体中成员的顺序会影响内存分配。编译器会按照声明的顺序依次分配每个成员的内存。

#### 2. 内存对齐
内存对齐是指数据在内存中按照其类型的对齐要求存储。一般来说，编译器会确保每个成员的起始地址是该成员类型大小的整数倍，这样可以提高内存访问的效率。对齐规则具体来说：

- 对于一个`int`（假设大小为4字节），它的地址通常应该是4的倍数。
- 对于一个`double`（假设大小为8字节），它的地址通常应该是8的倍数。

如果下一个成员的起始地址不满足其对齐要求，编译器会在前一个成员和下一个成员之间插入填充字节（padding），以满足对齐要求。

#### 3. 结构体的总大小
结构体的总大小必须是其最大成员对齐大小的倍数。编译器可能在结构体的末尾添加填充字节，使结构体的总大小满足这一要求。

#### 举例说明

```c
#include <stdio.h>

struct Example {
    char c;       // 1字节
    int i;        // 4字节
    double d;     // 8字节
};

int main() {
    struct Example ex;
    printf("Size of struct Example: %lu\n", sizeof(ex));
    return 0;
}
```

假设`char`是1字节，`int`是4字节，`double`是8字节：

- `c`占用1字节，起始地址为0。
- 接下来是`int`类型的`i`，需要4字节对齐。所以，在`c`之后会有3字节的填充字节，然后`i`从地址4开始，占用4字节。
- 然后是`double`类型的`d`，需要8字节对齐。由于当前地址为8，刚好符合对齐要求，所以直接占用接下来的8字节。

因此，`struct Example`的总大小为16字节（1 + 3（填充）+ 4 + 8）。

```c
struct Example {
    char c;       // 1字节
    char pad[3];  // 填充字节，使得下一个成员地址对齐到4字节
    int i;        // 4字节
    double d;     // 8字节
};
```



## 文件操作

## 预处理器

## 高级主题

## 动态内存分配

### malloc

----

头文件：<stdlib.h>

```c
# include <stdlib.h>
void *malloc(unsigned long size);
```

用malloc动态分配内存

```c
int *p=(int *)malloc(4)
```

意思是：请求系统分配4个字节内存，并且返回第一个字节地址

上述语句可以将void* 转换为int*

**如果不free的话会造成内存泄漏**



### free

------

free的原型是

```c
# include <stdlib.h>
void free(void *p);
```

free没有返回值

指针释放以后，它仍然是指向那一片空间，只是那一块不属于它

所以释放完之后一定要NULL，不然可能会变成**野指针**



### ps:

1. 一个动态内存只能释放一次
2. **有malloc就有free**



# 常见函数

### gets：

```c#
# include <stdio.h>
char *gets(char *str);

gets(str);
scanf("%s", string);
```

此外，关于使用 gets() 函数需要注意：使用 gets() 时，系统会将最后“敲”的换行符从缓冲区中取出来，然后丢弃，所以缓冲区中不会遗留换行符。这就意味着，如果前面使用过 gets()，而后面又要从键盘给字符变量赋值的话就不需要吸收回车清空缓冲区了，因为缓冲区的回车已经被 gets() 取出来扔掉了。下面写一个程序验证一下：



### pow()

```c
#include <math.h>
double pow(double,double);//返回以第一个参数为底，第二个参数为幂的值

```



# ASCII

| 二进制   | 十进制 | 十六进制 | 字符/缩写                                    | 解释                               |
| -------- | ------ | -------- | -------------------------------------------- | ---------------------------------- |
| 00000000 | 0      | 00       | NUL (NULL)                                   | 空字符                             |
| 00000001 | 1      | 01       | SOH (Start Of Headling)                      | 标题开始                           |
| 00000010 | 2      | 02       | STX (Start Of Text)                          | 正文开始                           |
| 00000011 | 3      | 03       | ETX (End Of Text)                            | 正文结束                           |
| 00000100 | 4      | 04       | EOT (End Of Transmission)                    | 传输结束                           |
| 00000101 | 5      | 05       | ENQ (Enquiry)                                | 请求                               |
| 00000110 | 6      | 06       | ACK (Acknowledge)                            | 回应/响应/收到通知                 |
| 00000111 | 7      | 07       | BEL (Bell)                                   | 响铃                               |
| 00001000 | 8      | 08       | BS (Backspace)                               | 退格                               |
| 00001001 | 9      | 09       | HT (Horizontal Tab)                          | 水平制表符                         |
| 00001010 | 10     | 0A       | LF/NL(Line Feed/New Line)                    | 换行键                             |
| 00001011 | 11     | 0B       | VT (Vertical Tab)                            | 垂直制表符                         |
| 00001100 | 12     | 0C       | FF/NP (Form Feed/New Page)                   | 换页键                             |
| 00001101 | 13     | 0D       | CR (Carriage Return)                         | 回车键                             |
| 00001110 | 14     | 0E       | SO (Shift Out)                               | 不用切换                           |
| 00001111 | 15     | 0F       | SI (Shift In)                                | 启用切换                           |
| 00010000 | 16     | 10       | DLE (Data Link Escape)                       | 数据链路转义                       |
| 00010001 | 17     | 11       | DC1/XON (Device Control 1/Transmission On)   | 设备控制1/传输开始                 |
| 00010010 | 18     | 12       | DC2 (Device Control 2)                       | 设备控制2                          |
| 00010011 | 19     | 13       | DC3/XOFF (Device Control 3/Transmission Off) | 设备控制3/传输中断                 |
| 00010100 | 20     | 14       | DC4 (Device Control 4)                       | 设备控制4                          |
| 00010101 | 21     | 15       | NAK (Negative Acknowledge)                   | 无响应/非正常响应/拒绝接收         |
| 00010110 | 22     | 16       | SYN (Synchronous Idle)                       | 同步空闲                           |
| 00010111 | 23     | 17       | ETB (End of Transmission Block)              | 传输块结束/块传输终止              |
| 00011000 | 24     | 18       | CAN (Cancel)                                 | 取消                               |
| 00011001 | 25     | 19       | EM (End of Medium)                           | 已到介质末端/介质存储已满/介质中断 |
| 00011010 | 26     | 1A       | SUB (Substitute)                             | 替补/替换                          |
| 00011011 | 27     | 1B       | ESC (Escape)                                 | 逃离/取消                          |
| 00011100 | 28     | 1C       | FS (File Separator)                          | 文件分割符                         |
| 00011101 | 29     | 1D       | GS (Group Separator)                         | 组分隔符/分组符                    |
| 00011110 | 30     | 1E       | RS (Record Separator)                        | 记录分离符                         |
| 00011111 | 31     | 1F       | US (Unit Separator)                          | 单元分隔符                         |
| 00100000 | 32     | 20       | (Space)                                      | 空格                               |
| 00100001 | 33     | 21       | !                                            |                                    |
| 00100010 | 34     | 22       | "                                            |                                    |
| 00100011 | 35     | 23       | #                                            |                                    |
| 00100100 | 36     | 24       | $                                            |                                    |
| 00100101 | 37     | 25       | %                                            |                                    |
| 00100110 | 38     | 26       | &                                            |                                    |
| 00100111 | 39     | 27       | '                                            |                                    |
| 00101000 | 40     | 28       | (                                            |                                    |
| 00101001 | 41     | 29       | )                                            |                                    |
| 00101010 | 42     | 2A       | *                                            |                                    |
| 00101011 | 43     | 2B       | +                                            |                                    |
| 00101100 | 44     | 2C       | ,                                            |                                    |
| 00101101 | 45     | 2D       | -                                            |                                    |
| 00101110 | 46     | 2E       | .                                            |                                    |
| 00101111 | 47     | 2F       | /                                            |                                    |
| 00110000 | 48     | 30       | 0                                            |                                    |
| 00110001 | 49     | 31       | 1                                            |                                    |
| 00110010 | 50     | 32       | 2                                            |                                    |
| 00110011 | 51     | 33       | 3                                            |                                    |
| 00110100 | 52     | 34       | 4                                            |                                    |
| 00110101 | 53     | 35       | 5                                            |                                    |
| 00110110 | 54     | 36       | 6                                            |                                    |
| 00110111 | 55     | 37       | 7                                            |                                    |
| 00111000 | 56     | 38       | 8                                            |                                    |
| 00111001 | 57     | 39       | 9                                            |                                    |
| 00111010 | 58     | 3A       | :                                            |                                    |
| 00111011 | 59     | 3B       | ;                                            |                                    |
| 00111100 | 60     | 3C       | <                                            |                                    |
| 00111101 | 61     | 3D       | =                                            |                                    |
| 00111110 | 62     | 3E       | >                                            |                                    |
| 00111111 | 63     | 3F       | ?                                            |                                    |
| 01000000 | 64     | 40       | @                                            |                                    |
| 01000001 | 65     | 41       | A                                            |                                    |
| 01000010 | 66     | 42       | B                                            |                                    |
| 01000011 | 67     | 43       | C                                            |                                    |
| 01000100 | 68     | 44       | D                                            |                                    |
| 01000101 | 69     | 45       | E                                            |                                    |
| 01000110 | 70     | 46       | F                                            |                                    |
| 01000111 | 71     | 47       | G                                            |                                    |
| 01001000 | 72     | 48       | H                                            |                                    |
| 01001001 | 73     | 49       | I                                            |                                    |
| 01001010 | 74     | 4A       | J                                            |                                    |
| 01001011 | 75     | 4B       | K                                            |                                    |
| 01001100 | 76     | 4C       | L                                            |                                    |
| 01001101 | 77     | 4D       | M                                            |                                    |
| 01001110 | 78     | 4E       | N                                            |                                    |
| 01001111 | 79     | 4F       | O                                            |                                    |
| 01010000 | 80     | 50       | P                                            |                                    |
| 01010001 | 81     | 51       | Q                                            |                                    |
| 01010010 | 82     | 52       | R                                            |                                    |
| 01010011 | 83     | 53       | S                                            |                                    |
| 01010100 | 84     | 54       | T                                            |                                    |
| 01010101 | 85     | 55       | U                                            |                                    |
| 01010110 | 86     | 56       | V                                            |                                    |
| 01010111 | 87     | 57       | W                                            |                                    |
| 01011000 | 88     | 58       | X                                            |                                    |
| 01011001 | 89     | 59       | Y                                            |                                    |
| 01011010 | 90     | 5A       | Z                                            |                                    |
| 01011011 | 91     | 5B       | [                                            |                                    |
| 01011100 | 92     | 5C       | \                                            |                                    |
| 01011101 | 93     | 5D       | ]                                            |                                    |
| 01011110 | 94     | 5E       | ^                                            |                                    |
| 01011111 | 95     | 5F       | _                                            |                                    |
| 01100000 | 96     | 60       | `                                            |                                    |
| 01100001 | 97     | 61       | a                                            |                                    |
| 01100010 | 98     | 62       | b                                            |                                    |
| 01100011 | 99     | 63       | c                                            |                                    |
| 01100100 | 100    | 64       | d                                            |                                    |
| 01100101 | 101    | 65       | e                                            |                                    |
| 01100110 | 102    | 66       | f                                            |                                    |
| 01100111 | 103    | 67       | g                                            |                                    |
| 01101000 | 104    | 68       | h                                            |                                    |
| 01101001 | 105    | 69       | i                                            |                                    |
| 01101010 | 106    | 6A       | j                                            |                                    |
| 01101011 | 107    | 6B       | k                                            |                                    |
| 01101100 | 108    | 6C       | l                                            |                                    |
| 01101101 | 109    | 6D       | m                                            |                                    |
| 01101110 | 110    | 6E       | n                                            |                                    |
| 01101111 | 111    | 6F       | o                                            |                                    |
| 01110000 | 112    | 70       | p                                            |                                    |
| 01110001 | 113    | 71       | q                                            |                                    |
| 01110010 | 114    | 72       | r                                            |                                    |
| 01110011 | 115    | 73       | s                                            |                                    |
| 01110100 | 116    | 74       | t                                            |                                    |
| 01110101 | 117    | 75       | u                                            |                                    |
| 01110110 | 118    | 76       | v                                            |                                    |
| 01110111 | 119    | 77       | w                                            |                                    |
| 01111000 | 120    | 78       | x                                            |                                    |
| 01111001 | 121    | 79       | y                                            |                                    |
| 01111010 | 122    | 7A       | z                                            |                                    |
| 01111011 | 123    | 7B       | {                                            |                                    |
| 01111100 | 124    | 7C       | \|                                           |                                    |
| 01111101 | 125    | 7D       | }                                            |                                    |
| 01111110 | 126    | 7E       | ~                                            |                                    |
| 01111111 | 127    | 7F       | DEL (Delete)                                 | 删除                               |



# 结构体

### 小tips：

1.只有结构体变量才分配地址

2.结构体变量的声明需要在主函数之上或者主函数中声明，如果在主函数之下则会报错

3.结构体不能直接强制转换

4.sizeof求结构体长度时是最大结构的整数倍

![op ](data:image/png;base64,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)

 

 

 

### 1.定义方法：

第一种：

```c#
#include<stdio.h> 
struct UDP_Server_Thread_Para
{
  void *pData;
  int len;
};

int main()
{
  struct UDP_Server_Thread_Para UDPThreadPara1,UDPThreadPara2;
  UDPThreadPara1.pData = "hello";
  UDPThreadPara1.len = 1;
  UDPThreadPara2.pData = "world";
  UDPThreadPara2.len = 2;
  printf("UDPThreadPara1: %s, len %d\n", (char*)UDPThreadPara1.pData, UDPThreadPara1.len);
  printf("UDPThreadPara2: %s, len %d\n", (char*)UDPThreadPara2.pData, UDPThreadPara2.len);
  return 0;
}
```



第二种：

**struct 结构体类型名**

**{**

**成员表列**

**}变量名表列；**

 

第三种：

**struct**

**{**

**成员表列**

**}变量名表列；**

```c#
struct
{
void *pData;
int Len;
}UDPThreadPara1,UDPThreadPara2;‘’
```

**赋值：**

1.定义时赋值

2.main里面依次赋值，未赋值的为0或'\0'

3.如果在定义结构体变量的时候没有初始化，那么后面就不能全部一起初始化了

 

**结构体数组**

```c#
struct Student
{ 
char name[20];
char sex;
int number;
}    stu1[5]={
 {"zhaozixuan",'M',12345},
 {"houxiaohong",'M',12306},
 {"qxiaoxin",'W',12546},
 {"wangwei",'M',14679},
 {"yulongjiao",'W',17857}
};
```

stu1[3].name[3]//表示stu1的第四个结构变量中姓名的第四个字符

//若初始化时已经是结构体数组全部元素[]中的数可以不写如stu1[]=

 

**结构体和指针**

1.指向结构体变量的指针

定义形式一般为

struct 结构体名* 指针名；

比如： struct Student* p；

以下三种方式等价：

p->cName

(*p).cName 

student1.cName

p->cName //可以进行正常的运算

 

### 2. 指向数组的指针

**struct Student stu1[5];**

**struct Student\*p;**

**p=stu[n];**

**(++p).number//****是指向了结构体数组下一个元素的地址**

 

**结构体作为函数参数修改是无效的**

 

###  **3.** 互换结构体的方法

```c
struct Student
{
 char cName[20];
 int number;
 char csex; 
}student1,student2;
struct Student student1={"Wang",12345,'W'};
struct Student student2={"Zhao",54321,'M'}; 
struct Student*stu1=&student1;
struct Student*stu2=&student2;
struct Student *student3;
student3=stu1;
stu1=stu2;
stu2=student3;//互换地址
```

#### **2.对于同类型结构体直接互换值就行**

**struct stu student3;**

**student3=student1;**

**student1=student2;**

**student2=student3;**

**//这里也可以写成应strcmp函数互换**

 

#### **3用memcpy()函数进行互换**

#### **4比较笨的方法： 用for循环互换**

 

### 4. 清空结构体

```c#
struct Student
{
 char cName[20];
 int number;
 char csex; 
}stu1;
```

 

#### 一般情况下，清空str的方法：

```c
str.cName[0]='\0';　
str.csex='0';
str.number=0;
```

　　**但是我们用memset就非常方便：**

```c#
memset(&str,0,sizeof(struct Student));
```

　　**如果是数组：**

```C
struct Student stu[10];
```

　　**就是**

```c
memset(stu,0,sizeof(struct Student)*10);
```



### 5.结构类型

除了声明结构标记，还可以用typedef来定义真是的类型名。例如可以照如下方式定义名为Part的类型：

```c
typedef struct {
	  int number;
	  char name[NAME_LEN+1];
	  int on_hand;
	} Part;
//可以像内置类型那样使用Part
Part part1, part2;
```

