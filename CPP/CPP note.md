## C++笔记

### 一、开始学习C++

#### 封装继承与多态

封装：将函数和变量封装为一个类，该类对外提供访问的接口，外界只能通过接口访问和修改私有的数据

继承：子类继承父类所有的数据和函数，其中**构造函数、析构函数、友元函数、静态数据成员、静态成员函数**无法被继承，私有还是公有代表子类能否访问，`final`关键字修饰的类无法继承，修饰的函数无法重写

多态：不同继承类的对象对同一消息做出不同的响应，提高了代码的拓展性。

- 动态多态：通过虚函数实现的，虚函数是类的成员函数，存在存储虚函数指针的表叫做虚函数表，虚函数表是一个存储类成员虚函数的指针，每个指针都指向调用它的地方，当子类调用虚函数时，就会去虚表里面找自己对应的函数指针，从而实现“谁调用、实现谁”从而实现多态。
- 静态多态：函数重载（函数名相同，参数不同，两个函数在同一作用域），运算符重载，和重定义（又叫隐藏，指的是在继承关系中，子类实现了一个和父类名字一样的函数，（只关注函数名，和参数与返回值无关）这样的话子类的函数就把父类的同名函数隐藏了。隐藏只与函数名有关，与参数没有关系.）来实现的。



#### 预备知识

```c++
#include <iostream>  //不使用<iostream.h>是为了和c的标准区分开
using nameapcae std; 
```

名称空间，在不同的产品中有相同的wander()函数，那如果要正确使用

```c++
Microflop::wander("go dancing");
Piscine::wander("a fish named Desire");
```

使用`using nameapcae std;`之后可以忽略using



#### `cout`输出

`endl`和`\n`是等价的



##### `cout.setf`

类似于控制符，有两者函数原型：

```c++
fmtflags setf(fmtflage) //第一原型
fmtflags setf(fmtflags, fmtflags)  //第二原型
//fmtflags 是 bitmask 类型(一种用来存储各个位值的类型)的 typedef 名
//显而易见，两个原型的主要区别在于参数数量不同
```



##### `cout.precision(n)`

控制小数位数



### 二、处理数据

`dec、hex、oct//十进制，十六进制，八进制`

`unsigned`是无符号类型

`const type name =value`用来限定，例如：

`const int month = 12`这样可以用month来代替12，注意定义的时候初始化



### 三、复合类型

#### 结构体

如果初始化比成员少，余下的成员会被设置为0



#### 使用new来分配内存

c++中依然可以使用c中的 `malloc()`函数来分配内存，但是c++有更好的方法就是`new`函数

```c++
int * pt =new int 
//typename * pointer_name = new typename;
```

**`delete`**:

与free对应，`delete pointer_name`

1. 配对的使用 `new`和 `delete`，不然可能造成内存泄漏
2. 不要使用 `delete`来删除不是 `new`定义的内存
3. 不要两次 `delete`同一个函数
4. 对空指针 `delete`是安全的
5. 如果定义的是动态内存数组

   ```c++
   short * ps=new short [100];
   delete [] ps;
   ```

一种节约内存的方法：

```c++
char *getname()
{
	char temp[80];
	cout<<"Enter a string:";
	cin>> temp;
	char *pn = new char[strlen(temp)+1];
	strcpy(pn,temp);
	return pn;
}
```



#### 数组的代替

##### vector

```c++
#include <vector> //包含头文件
vector<int> vi;
```

##### array

```c++
#include <array>
array<int 5>ai;
```

**ps:**

1. array对象可以直接复制
2. 用数组a1[-2]时等于*(a1-2)会跑到奇怪的地方去

#### `ofstream与ifstream`类

##### 1.打开文件

函数原型：

```c++
void open(const char * filename, int mode,int access)
//filename 文件名
//mode 打开文件方式
//access打开文件属性
```

打开文件的方式在类ios(是所有流式I/O类的基类)中定义，常用的值如下：

```
ios::app：　　		以追加的方式打开文件 
ios::ate：　　	　  文件打开后定位到文件尾，ios:app就包含有此属性 
ios::binary：　  	以二进制方式打开文件，缺省的方式是文本方式。两种方式的区别见前文 
ios::in：　　　    文件以输入方式打开（文件数据输入到内存） 
ios::out：　　	   文件以输出方式打开（内存数据输出到文件） 
ios::nocreate： 	不建立文件，所以文件不存在时打开失败 
ios::noreplace:   不覆盖文件，所以打开文件时如果文件存在失败 
ios::trunc：　　	如果文件存在，把文件长度设为0 
```

可以用“或”把以上属性连接起来，如 `ios::out|ios::binary `
**打开文件的属性取值是：**
0：普通文件，打开访问
1：只读文件
2：隐含文件
4：系统文件

`fstream`还有和open()一样的构造函数，在定义的时侯就可以打开文件了：
`fstream file1("c:\\config.sys"); `

`fstream`有两个子类：`ifstream(input file stream)和ofstream(outpu file stream)`，`ifstream`默认以输入方式打开文件，而 `ofstream`默认以输出方式打开文件

##### 2.关闭文件

`file1.close();`



### 四、循环结构

`cout.self(ios_base::boolalpha);`此语句可以将真假输出为false和ture

前缀递增和解除引用的优先级一样，所以

```c++
double array[5]={1.2,123,1.23,43.1,1.3};
double *a1=arr;
//*a1   == 1.2;
//*++a1 == 123;
//++*a1 == 2.2;
```

#### 基于范围的for循环

```c++
double array[5]={1.2,123,1.23,43.1,1.3};
for(double &x :array)
	x = x*0.08;//让array中每一个数都×0.08；
```

`cin`在读取字符串的时候会忽略换行符和空格

`cin.get()`可以输入每一个字符

```c++
cin.get(array,20).get();
```

和

```c++
char ch;
cin.get(ch);
```

很奇怪，`cin.get`可以接受两种参数形式的变量，在c语言中不被允许，这是函数重载的OOP特性

#### 键盘模拟EOF

`cin.eof()`和 `cin.fail()`两个函数，如何检测到EOF，返回的bool值为ture，否则为false

`cin.get(char)`的返回值是一个 `cin`对象。然而，`istream`类提供一个可以将 `istream对象`转换为bool值的函数

```c++
while(cin)   //更好的写法
```



### 五、条件结构

**可以用`3==number`这种方法来避免赋值错误**

#### 字符库函数cctype

![image-20220112075314616](pics/image-20220112075314616.png)

#### 简单的文件输入输出

##### 输入：

准备：

1. 包含头文件 `fstream`
2. 声明一个或多个 `ofstream`对象，并且命名
3. 使用 `using`或 `std::`
4. 关联使用 `open`和 `close`

```c++
ofstream outfile; //声明变量
outfile.open("fish.txt");  //关联文件

double wt=5.12;
outfile << wt;     //可以使用和cin相同的任何函数
outfile.close      //已经关联所以可以直接关闭
```

**ps：**打开以存在的文件时，会使文件内容丢失

##### 输出：

与上面相同，主要是声明 `ifstream`对象

`is_open`检测是否打开文件



### 六、函数

#### 函数与结构

结构比较小的时候可以直接按值传递



#### 传递结构的地址

需要修改三个地方：

- 调用函数时，将结构的地址传递
- 将形参声明为指向polar的指针，即polar *类型，由于函数不修改结构，因此使用了`const`修饰符
- 由于形参是指针不是结构，因此应使用间接成员运算符

#### 函数指针

##### 1.获取函数地址

只要使用函数名，如果think()是一个函数，think就是该函数的地址

#### 匿名函数

匿名函数通常被称为**Lambda表达式**。Lambda表达式是可以定义在任何地方的函数对象，并且不需要为其指定名字。它们可以捕获周围作用域中的变量，从而提供更灵活的函数定义方式。Lambda表达式的基本语法如下：

```cpp
[capture](parameters) -> return_type {
    // Function body
};
```

其中：
- `capture` 是一个捕获列表，用来指定哪些变量可以从外部作用域中捕获。
- `parameters` 是可选的参数列表，类似于普通函数的参数列表。
- `return_type` 是可选的返回类型，如果省略，编译器会自动推导返回类型。
- `{}` 内部是函数体。

举个例子：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // Lambda表达式，计算并输出所有偶数的平方
    std::for_each(numbers.begin(), numbers.end(), [](int x) {
        if (x % 2 == 0) {
            std::cout << x * x << " ";
        }
    });

    return 0;
}
```

在这个例子中，`[](int x) {}` 就是一个匿名函数，它被传递给了 `std::for_each` 函数，用来处理 `numbers` 向量中的每一个元素。

##### 捕获列表
Lambda表达式的捕获列表可以用于捕获周围作用域中的变量。捕获方式可以是按值捕获或按引用捕获：
- `[=]`：按值捕获，捕获外部作用域中的所有变量，并在Lambda表达式中使用它们的副本。
- `[&]`：按引用捕获，捕获外部作用域中的所有变量，并在Lambda表达式中使用它们的引用。
- `[x]` 或 `[&x]`：只捕获变量 `x`，按值或按引用。

例如：

```cpp
int a = 10;
auto lambda = [a](int b) { return a + b; };
```

在这个例子中，`lambda` 是一个捕获变量 `a` 的匿名函数，并将其与参数 `b` 相加。



### 七、函数谈幽

#### 内联函数

函数前加上 `inline`，将整个定义（函数头和函数代码）放在本应该提供原型的地方，内联函数不能递归，算是一种用空间换时间的方法，当函数简短切调用很多次的时候有时间优势

**内联与宏**

宏不是通过参数传递实现的，而是通过文本替换实现的

#### 左值引用

- **定义**：左值引用是指向内存中某个已命名的对象的引用，即绑定到一个具名的变量或对象。
- **语法**：使用 `&` 来声明左值引用。

- 创建引用变量：


```c++
int rats;
int & rodents = rats;
```

特点：

- 与原变量值相同，地址相同
- 不能用表达式作为变量

- 返回引用变量的时候不能返回临时变量

##### 左值引用和指针的区别

- **语法**：左值引用使用 `&` 声明，而指针使用 `*` 声明。
- **绑定方式**：左值引用直接引用对象，而指针是保存对象地址的变量。
- **使用方式**：引用通过 `.` 访问对象成员，而指针需要通过 `->` 访问对象成员。
- **空引用**：引用必须在声明时初始化，并且不能为 `nullptr`，而指针可以为空或指向 `nullptr`。

示例：

```cpp
int a = 10;
int* p = &a;  // 指针 p 指向 a 的地址
int& r = a;   // 引用 r 绑定到 a
```

#### 右值引用（Rvalue Reference）

**定义**：右值引用是指向临时对象或字面量的引用，即绑定到一个右值（通常是没有名字的临时对象）。

- **语法**：使用 `&&` 来声明右值引用。
- **示例**：
  
  ```cpp
  int&& rref = 10;  // rref 是绑定到临时值 10 的右值引用
  ```
- **特点**：
  - 右值引用只能绑定到右值。
  - 右值引用允许你在保证安全的情况下修改临时对象，可以在不创建不必要拷贝的前提下进行操作。
  

##### 引入右值引用的原因
C++11引入右值引用是为了支持**移动语义**，这是一种新特性，允许对象的资源从一个临时对象“移动”到另一个对象，而不是进行深度拷贝。例如，当一个对象拥有动态内存分配（如 `std::vector`），移动语义可以直接“移动”内存指针，而不是拷贝内存，从而大大提高了性能。

示例

```cpp
#include <iostream>
#include <vector>

std::vector<int> createVector() {
    std::vector<int> vec = {1, 2, 3, 4};
    return vec;  // 这里触发了移动语义，而不是拷贝
}

int main() {
    std::vector<int> myVec = createVector();  // 使用移动构造函数
    return 0;
}
```
在这个例子中，`createVector` 函数返回一个临时的 `std::vector`，该临时对象会被移动到 `myVec`，而不是通过拷贝构造函数创建副本，这样就避免了不必要的资源消耗。



### 八、内存与函数空间

`static`修饰函数中的变量时，只在初始化程序是定义，再次引用函数时不会重置

#### 动态分配

1. 使用new初始化

`int *pi= new int(6)`

#### 名称空间

```c++
namespace Jack{
	double t1；
	int hell;
}
```

#### 静态变量的初始化位置：
- **静态局部变量**：在函数、方法或块中的静态变量会在函数第一次被调用时初始化。它们的存储空间是在全局数据区分配的，但初始化在第一次使用时进行。
- **静态成员变量**：类的静态成员变量通常在类外定义时初始化，它们的存储空间也是在全局数据区。

#### 初始化阶段：
- **全局/静态对象**的初始化顺序遵循以下规则：
  1. **全局范围内的静态变量**：在`main`函数执行之前就会初始化，顺序与它们在文件中出现的顺序一致。
  2. **静态局部变量**：在它们所属的函数第一次被调用时初始化，且在程序的整个生命周期中保持已初始化的状态。

### 九、对象和类

```c++
class stock{
private:
	int num;
	std::string company;
public:
	void show();
	stock();//构造函数
	stock(std::string &co,int num,long n)//构造函数
}

//使用构造函数：
stock first;
stock first = stock();
stock *first = new stock;

stock second();//注意这是构造一个函数，而不是使用默认的构造方式

//const成员函数
//对于不修改对象的函数应该按如下方式声明和定义
void show() const;
void stock::show() const;
```

#### Class和Struct的区别

默认继承权限不同：class默认继承的是private继承，struct默认是public继承。

Class还可用于定义模板参数，但是关键字struct不能同于定义模板参数，C++保留struct关键字，原因是保证与C语言的向下兼容性，为了保证百分百的与C语言中的struct向下兼容，，C++把最基本的对象单元规定为class而不是struct，就是为了避免各种兼容性的限制。

#### 运算符的重载

`operator +()`//通过这样的方式可以对运算符进行重载

#### 友元函数

##### 创建友元函数

`friend Time operator* (double m,const Time *t);`

- operator*不是成员变量，不能使用成员运算符
- operator*与成员函数访问权限相同

##### 常用友元：重载<<

```c++
void opeartor<<(ostream & os, const Time &t){
	os << t.hours<<"hours" << t.mins << "minutes";
}
//这样可以使用如下语句
cont << trip;

//更好的方式
std::ostream & opeartor<<(std::ostream & os, const Vector &v)
{
    os << v.ang * Red_to_deg ;
    return os;
}//这样可以连续使用cout
```

#### 转换函数

##### 从类转换到其他

`operator double()`

- 转换函数必须是类方法
- 不能返回指定的类型
- 不能有参数

如果在构造函数前加上了explicit，那么该构造函数只能够显示转换

##### 从其他转换到类

```c++
//类似与构造函数的方式
north = "polaries";					//not allowed
north = star("polaries"); 			//allowed
```

#### 复制构造函数（拷贝构造函数）

```c++
//定义一个显示复制构造函数以解决问题
StringBad::StringBad(const StringBad & st)
{
    num_strings++;
    len = st.len;
    str = new char[len+1];
    std::strcpy(str, st.str);
    cout << num_strings
}
```



### 十、类和动态内存分配

无论创建多少个对象，程序都只共享一个静态类变量副本

不能在类声明里面创建

如果类中包含了new初始化的指针，应当定义一个复制构造函数，以复制指向的数据，而不是指针，这被称为深拷贝

同样的，对于含有指针的类，也要重写=，避免指针的重复

```c++
StringBad & StringBad::operator=(const StringBad & st){
	if(this = st)
		return *this;
	delete [] str;
    len = st.len;
    str = new char [len+1];
    std::strcpy(str, st.str);
    return *this;
}
```

**`nullptr`**可以表示空指针，在c++中常用

初始化的时候不一定会使用赋值运算符，而是使用复制构造函数

#### 在构造函数中使用new的注意事项

- 在构造函数中使用new，必须在析构函数中使用delete
- new对应delete，new []对应delete []
- 如果有多个构造函数，应该统一使用，因为析构函数只有一个

#### 指针

空对象指针也是能够调用静态成员函数的，因为静态成员函数使用的都是静态变量。



### 十一、继承

```c++
class RatedPlayer : public TableTennisPlayer
```

- 派生类需要自己的构造函数
- 可以添加数据以及成员函数


```c++
//RatedPlayer的构造代码
RatedPlayer::RatePlayer(unsign int i, const string & fn, 
     const string & ln, bool ht ) : TableTennisPlayer (fn, ln, ht)
{
    rating = i;
}
```

常量只能够初始化而无法赋值，构造函数为了解决这个问题可使用下面的方法

```c++
Queue::Queue(int qs) : qsize(qs){
	front = rear = NULL;
	items = 0;
}
//更离谱的方法
Queue::Queue(int qs) : qsize(qs)，front(NULL), rear(NULL), item(0)
{
}
```

对于 `const`类成员以及被声明为引用的类成员

#### 虚函数

虚函数目的是为了让**指向基类的指针**可以**调用子类的函数**

实现原理：虚函数表和虚函数指针

- 虚函数表：每个包含虚函数的类都会生成一个虚函数表，其中存储着该类中所有虚函数的地址。虚函数表是一个由指针构成的数组，每个指针指向一个虚函数的实现代码。子类的虚函数表会包含继承自基类的虚函数指针，但会指向子类的实现。
- 虚函数指针：在对象的内存布局中，编译器会添加一个额外的指针，称为**虚函数指针**或**虚表指针**。这个指针指向该对象对应的虚函数表，从而让程序能够动态的调用虚函数。

使用 `virtual`关键字：

- 构造函数不能是虚函数
- 析构函数要是虚函数
- 友元函数不能是虚函数


一些经验：

- 如果重新定义继承的方法，要保证参数完全一致
- 如果基类声明被重载了，则需要重新定义所有的基类版本



#### 访问控制：protect

派生类可以访问protect修饰的变量，外部不能直接访问

#### 抽象基类

ex：为圆和椭圆创造一个共同的类

##### 纯虚函数：在函数声明结尾处添加=0

当类声明中存在纯虚函数时，则无法创建该类的对象

所有为了创建抽象类，则需要至少使一个函数变为纯虚函数

`virtual int vir_fun()=0;`

### 字符串

#### string类

可变长度的字符串，可以加减，等于

**计算长度**：str1.size()

输入：`getline(cin,str)`

为了避免在字符串输出中出现太多

```c++
R"("the king" is me)";
R"+*("(sdafaf?)",sadafas.)+*";
```



### 头文件

#### stdlib.h

stdlib.h头文件中 定义了两个变量：

\#define EXIT_SUCCESS 0
\#define EXIT_FAILURE 1

```c++
#ifndef COORDIN
#define COORDIN //s
...
#endif //避免重复包含头文件
```



## C++ STL

有六大组件分别是：仿函数，算法，迭代器，空间配置器，容器，配接器

### 容器

#### vector

##### 使用vector

```c++
#include <vector>
```

##### 创建vector

```c++
vector<int> arr1;								//一个空数组
vector<int> arr2 {1, 2, 3, 4, 5};				//包含1、2、3、4、5五个变量
// 不要先创建空间然后 push_back 会导致vector大小多很多
vector<int> arr3(4);							//开辟4个空间，值默认为0
vector<int> arr4(5, 3);							//5个值为3的数组
vector<int> arr5(arr4);							//将arr4的所有值复制进去，和arr4一样
vector<int> arr6(arr4.begin(), arr4.end());		//将arr4的值从头开始到尾复制
vector<int> arr7(arr4.rbegin(), arr4.rend());	//将arr4的值从尾到头复制
```

##### 遍历访问

```c++
//迭代器：vector<int>::iterator
for (vector<int>::iterator it = arr.begin(); it != arr.end(); it++)
{
    cout << *it << endl;
}
//迭代器：vector<int>::reverse_iterator
for (vector<int>::reverse_iterator it = arr.rbegin(); it != arr.rend(); it++)
{
    cout << *it << endl;
}
// 下标访问
for (int i = 0; i < arr.size(); i++)
{
	cout << arr[i] << endl;
}
// for循环
for (auto num : arr)
{
	cout << num << endl;
}
```

##### 容量和大小

```c++
vec.size();					// 返回当前存储了多少数字
vec.capacity();				// 返回容量
vec.max_size();				// 返回最大可设置的大小
vec.empty();				// 判断是否为空
vec.resize();				// 改变size的大小
vec.reserve();				// 改变capacity的大小
vec.shrink_to_fit();		// 收缩到合适
```

##### 常用操作

```c++
// 尾部插入
push_back();
emplace_back();
pop_back 
```

`push_back()`的参数是左值，则使用它拷贝构造新对象，如果是右值，则使用它移动构造新对象.。



## C++ 11

### 时间工具chrono库

chrono定义了三种主要类型以及常用工具：

- 时钟
- 时长
- 时间点



***时钟* *(Clock)***要求：



***平凡时钟* *(TrivialClock)***要求：

- 满足*时钟*要求



#### 时钟

时钟由起点（或纪元）及计次频率(1分钟，1秒或者1毫秒)组成。

##### `system_clock`

std::chrono::system_clock 表示系统范围的实时壁钟。其不一定单调，系统时间可以被调节，它是**唯一有能力映射到c风格时间**(ctime.h)的C++时钟，满足**平凡时钟要求**

***纪元***：从协调世界时 (UTC) 1970 年 1 月 1 日星期四 00:00:00 开始的时间，不计闰秒

成员函数：

> `std::chrono::system_clock::now()`
>
> 返回值：当前时间的时间点
>
> `std::chrono::system_clock::to_time_t( const time_point& t )`
>
> 参数：要转换的时间点
>
> 返回值：返回t的`std::time_t`值，注意`time_t`的精度很低，只有秒



##### `std::chrono::steady_clock`

表示单调时钟。此时钟的时间点无法减少，因为物理时间向前移动。此时钟与壁钟时间无关，且最适于度量间隔。





#### 时间点

##### `std::chrono::time_point`

`std::chrono::time_point` 表示时间中的一个点。它被实现成如同存储一个 `Duration` 类型的自 `Clock` 的纪元起始开始的时间间隔的值。



时间点与时间点相减返回时长

时间点与时长加减返回时间点



#### 时长

时长由**时间跨度**组成，定义为某时间单位的某个计次数。例如，“42 秒”可表示为由 42 个 1 秒时间点位的计次所组成的时长。

##### `std::chrono::duration`

```c++
template<
    class Rep,
    class Period = std::ratio<1>
> class duration;
```

**成员类型：**

- rep：计次数的数据类型
- period：计次周期的 [std::ratio](https://zh.cppreference.com/w/cpp/numeric/ratio/ratio)（即每次的秒数）(默认是1秒)



###### 成员函数

构造函数：

1. 构造函数的 `const Rep2&`能隐式转换为rep（时常的计次类型）且满足下列条件才参与重载决议

   `std::chrono::treat_as_floating_point<rep>::value` 为 true ，或
   `std::chrono::treat_as_floating_point<Rep2>::value` 为 false 。



`count()`

计数函数，返回此 duration 的计次数。



### 移动（Move）

移动操作是为了避免不必要的复制，从而提高性能。`std::move` 实际上并不移动对象的数据，它只是将对象的所有权从一个对象转移到另一个对象，而不进行数据的复制。移动操作通常包括移动构造函数和移动赋值运算符。比如：

```cpp
std::string str1 = "Hello";
std::string str2 = std::move(str1); // 移动 str1 到 str2
```

在这个例子中，`str1` 的数据被移动到 `str2` 中，`str1` 可能会变成一个空的或无效的状态。这里没有进行数据的复制操作，只是数据的所有权从 `str1` 转移到了 `str2`。

#### 和复制的主要区别

1. **资源管理**：
   - 复制操作会分配新的资源并复制数据。
   - 移动操作不会分配新的资源，而是转移资源的所有权。

2. **性能**：
   - 复制可能会很昂贵，特别是对于大对象。
   - 移动通常更高效，因为它避免了不必要的数据复制。

3. **对象状态**：
   - 复制后，原对象和新对象都是有效且独立的。
   - 移动后，原对象处于一种未指定但有效的状态（通常是空的或被清理的）。

使用 `std::move` 可以显著提高程序性能，特别是在处理需要频繁传递的临时对象时。因此，了解并合理使用这两种操作方式对于优化C++程序的性能是非常重要的。



### 智能指针

智能指针的本质是模板类

`unique_ptr`、`shared_ptr` 和 `weak_ptr`

#### `unique_ptr`

unique_ptr 代表的是专属所有权，即由 unique_ptr 管理的内存，只能被一个对象持有。
所以，**unique_ptr 不支持复制和赋值**，如下：

```cpp
unique_ptr<string> p1(new string("I'm Li Ming!"));
unique_ptr<string> p2(new string("I'm age 22."));
	
cout << "p1：" << p1.get() << endl;
cout << "p2：" << p2.get() << endl;

p1 = p2;					// 禁止左值赋值
unique_ptr<string> p3(p2);	// 禁止左值赋值构造

unique_ptr<string> p3(std::move(p1));
p1 = std::move(p2);	// 使用move把左值转成右值就可以赋值了，效果和auto_ptr赋值一样

cout << "p1 = p2 赋值后：" << endl;
cout << "p1：" << p1.get() << endl;
cout << "p2：" << p2.get() << endl;
```

因此，**unique_ptr 只支持移动**, 即如下：

```cpp
auto w = std::make_unique<Widget>();
auto w2 = std::move(w); // w2 获得内存所有权，w 此时等于 nullptr
```

##### 性能

因为 C++ 的 zero cost abstraction 的特点，unique_ptr 在默认情况下和裸指针的大小是一样的。
所以 **内存上没有任何的额外消耗，性能是最优的**

#### `shared_ptr`

存在一个引用计数，如果为0的时候则释放内存

##### 构造

1. `shared_ptr<T> sp1`： 空的shared_ptr，可以指向类型为T的对象

```c++
shared_ptr<Person> sp1;
Person *person1 = new Person(1);
sp1.reset(person1);	// 托管person1
```

2. `shared_ptr<T> sp2(new T());` 定义shared_ptr，同时指向类型为T的对象

```c++
shared_ptr<Person> sp2(new Person(2));
shared_ptr<Person> sp3(sp1);
```

3. `shared_ptr<T[]> sp4;` 空的shared_ptr，可以指向类型为T[]的数组对象 C++17后支持

```c++
shared_ptr<Person[]> sp4;
```

4. `shared_ptr<T[]> sp5(new T[] { … });` 指向类型为T的数组对象 C++17后支持

```c++
shared_ptr<Person[]> sp5(new Person[5] { 3, 4, 5, 6, 7 });
```

5. `shared_ptr< T > sp6(NULL, D()); `

```c++
shared_ptr<Person> sp6(NULL, DestructPerson()); //空的shared_ptr，接受一个D类型的删除器，使用D释放内存
```

6. `shared_ptr< T > sp7(new T(), D());` 

```c++
shared_ptr<Person> sp7(new Person(8), DestructPerson());//定义shared_ptr,指向类型为T的对象，接受一个D类型的删除器，使用D删除器来释放内存
```

##### 初始化

1. 方式一：构造函数

```c++
shared_ptr<int> up1(new int(10));  // int(10) 的引用计数为1
shared_ptr<int> up2(up1);  // 使用智能指针up1构造up2, 此时int(10) 引用计数为2
```

2. 方式二：使用make_shared 初始化对象，分配内存效率更高（推荐使用）


- make_shared函数的主要功能是在动态内存中分配一个对象并初始化它，返回指向此对象的shared_ptr
- 用法：make_shared<类型>(构造类型对象需要的参数列表);

```c++
shared_ptr<int> up3 = make_shared<int>(2); // 多个参数以逗号','隔开，最多接受十个
shared_ptr<string> up4 = make_shared<string>("字符串");
shared_ptr<Person> up5 = make_shared<Person>(9);
```

##### 赋值

```c++
shared_ptrr<int> up1(new int(10));  // int(10) 的引用计数为1
shared_ptr<int> up2(new int(11));   // int(11) 的引用计数为1
up1 = up2;	// int(10) 的引用计数减1,计数归零内存释放，up2共享int(11)给up1, int(11)的引用计数为2
```

##### 主动释放对象

```c++
shared_ptrr<int> up1(new int(10));
up1 = nullptr ;	// int(10) 的引用计数减1,计数归零内存释放 
up1 = NULL; // 作用同上 
```

##### 重置

p.reset() ; 将p重置为空指针，所管理对象引用计数 减1
p.reset(p1); 将p重置为p1（的值）,p 管控的对象计数减1，p接管对p1指针的管控
p.reset(p1,d); 将p重置为p1（的值），p 管控的对象计数减1并使用d作为删除器
p1是一个指针！

##### 交换

p1 和 p2 是智能指针

```c++
std::swap(p1,p2); // 交换p1 和p2 管理的对象，原对象的引用计数不变
p1.swap(p2);    // 交换p1 和p2 管理的对象，原对象的引用计数不变
```

#### weak_ptr

`std::weak_ptr` 是为了配合 `std::shared_ptr` 使用而设计的，用于解决共享资源时的循环引用问题。

##### 特性

1. **不拥有资源的弱引用**

`std::weak_ptr` 是对资源的**弱引用**，与 `std::shared_ptr` 不同，它并不增加资源的引用计数。这意味着 `weak_ptr` 不会影响资源的生命周期。

**示例**：
```cpp
#include <iostream>
#include <memory>

int main() {
    std::shared_ptr<int> sp = std::make_shared<int>(10);
    std::weak_ptr<int> wp = sp;  // wp 是 sp 的弱引用

    std::cout << "sp use count: " << sp.use_count() << std::endl;  // 输出 1
    std::cout << "wp use count: " << wp.use_count() << std::endl;  // 输出 1

    sp.reset();  // 释放 sp 所管理的资源

    if (wp.expired()) {
        std::cout << "The resource has been released.\n";
    }

    return 0;
}
```

在这个例子中，`wp` 是一个弱引用，不增加 `sp` 的引用计数。当 `sp` 被释放后，`wp` 仍然存在，但它指向的资源已经被释放。

2. **防止循环引用**

在使用 `std::shared_ptr` 时，如果两个对象相互引用，可能会导致循环引用，进而导致内存泄漏。`std::weak_ptr` 是解决这个问题的关键。

**循环引用示例**：
```cpp
#include <iostream>
#include <memory>

struct B;
struct A {
    std::shared_ptr<B> b_ptr;
    ~A() { std::cout << "A destroyed\n"; }
};

struct B {
    std::shared_ptr<A> a_ptr;
    ~B() { std::cout << "B destroyed\n"; }
};

int main() {
    std::shared_ptr<A> a = std::make_shared<A>();
    std::shared_ptr<B> b = std::make_shared<B>();

    a->b_ptr = b;
    b->a_ptr = a;

    // 此时 a 和 b 都不会被销毁，因为它们形成了循环引用
    return 0;
}
```

在上面的例子中，由于 `A` 和 `B` 之间的循环引用，`a` 和 `b` 的引用计数无法降到 0，导致资源无法释放。

**使用 `weak_ptr` 解决循环引用**：
```cpp
#include <iostream>
#include <memory>

struct B;
struct A {
    std::shared_ptr<B> b_ptr;
    ~A() { std::cout << "A destroyed\n"; }
};

struct B {
    std::weak_ptr<A> a_ptr;  // 使用 weak_ptr 代替 shared_ptr
    ~B() { std::cout << "B destroyed\n"; }
};

int main() {
    std::shared_ptr<A> a = std::make_shared<A>();
    std::shared_ptr<B> b = std::make_shared<B>();

    a->b_ptr = b;
    b->a_ptr = a;

    // 现在，a 和 b 可以正常销毁
    return 0;
}
```

在这个例子中，`b->a_ptr` 是一个 `weak_ptr`，不会影响 `a` 的引用计数，因此循环引用问题得以解决。

3. **检查对象是否还存在**

`std::weak_ptr` 可以通过 `expired()` 函数检查它指向的对象是否仍然存在。如果对象已经被销毁，`expired()` 返回 `true`。

```cpp
if (wp.expired()) {
    std::cout << "The resource has been released.\n";
}
```

4. **提升（Locking）为 `shared_ptr`**

`std::weak_ptr` 可以使用 `lock()` 函数临时获得一个 `std::shared_ptr`。如果 `weak_ptr` 指向的对象仍然存在，`lock()` 返回一个有效的 `shared_ptr`；否则，它返回一个空的 `shared_ptr`。

**示例**：
```cpp
if (auto sp = wp.lock()) {  // 获取 shared_ptr
    std::cout << "Resource is still alive.\n";
    // 使用 sp 操作资源
} else {
    std::cout << "Resource has been released.\n";
}
```
