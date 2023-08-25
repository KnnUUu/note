### 关键字

`Inline`：直接把里面的内容粘贴到函数被调用的地方，可以减少访问函数时花费的时间，但如果函数太长反而会使时间变长，所以一般只用于只有几行的函数。  
  
`Constexpr`（constant expression）： 编译时就可以算出来（前提是为了算出它所依赖的东西也是在编译期可以算出来的），花费更多编译时间但减少运行时间。  
  
`Const`： 保证变量运行时不直接被修改。  
  
`template` : 把类型当作变量传到函数，使得同一个函数可以处理多种类型的变量。在编译的时候将程序复制成几个对应不同变量类型的函数。与宏不同的地方在于template会对变量类型进行检查。 

`extern`：存储类用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的  
  
`decltype` 类型指示符  
从表达式的类型推断出要定义的变量的类型
```cpp
// 推断sum的类型是函数f的返回类型
decltype(f()) sum = x; 
```

`volatile`：声明类型变量可以被某些编译器未知的因素更改，编译器对访问该变量的代码就不进行优化,以确保每次访问时可以取到同样的值  
优化：假如某个变量被调用了多次并且在代码上没有进行更改，那么第二次以后编译器不会再次从地址读取该值而是直接从CPU寄存器使用上次的值  
这些情况应该使用  
- 多线程都要用到某一个变量且该变量的值会被改变时
- 可能由于外部原因导致变量改变时  

[参考](https://www.runoob.com/w3cnote/c-volatile-keyword.html)  

## 函数 function
### 基础
函数定义时叫形参（parameter）  
函数调用时叫实参（argument）  
形参和实参的个数和类型必须匹配上  

### main处理命令行选项
`int main(int argc, char *argv[]){...}`  
`argc` 参数的个数  
`argv` 参数C风格字符串数组  

### 有返回值函数
- return语句的返回值的类型必须和函数的返回类型相同，或者能够隐式地转换成函数的返回类型  
- 不要返回局部对象的引用或指针  

### OOP
基类 base class 、派生类 subclass
|访问|public|protected|private|
|---|---|---|---|
|同一个类|:o:|:o:|:o:|
|派生类|:o:|:o:|:x:|
|外部类|:o:|:x:|:x:|

多继承 mulitple inheritance：继承多个父类的特性
```cpp
class <派生类名>:<继承方式1><基类名1>,<继承方式2><基类名2>,…
```
### 多态
虚函数 virtual function：virtual dataType functionName()  
派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数。定义虚函数是为了允许用基类的指针来调用子类的这个函数。  
纯虚函数 pure virtual function：virtual dataType functionName() = 0  
在基类中又不能对虚函数给出有意义的实现时使用  
抽象类 abstract class：含有一个以上纯虚函数的类，不能被实体化否则会编译错误  
```cpp
class A 
{ 
public:     
    virtual void foo()     
    {    	 
    cout<<"A::foo() is called"<<endl;     
    }
}; 
class B:public A { 
public:     
    void foo()     
    {    	 
    cout<<"B::foo() is called"<<endl;     
    } 
}; 
int main(void) 
{     
    A *a = new B();     
    a->foo();   // 在这里，a虽然是指向A的指针，但是被调用的函数(foo)却是B的!     
    return 0; 
}
```

### 编译
[C/C++项目中.h和.inc文件区别](https://www.cnblogs.com/kelamoyujuzhen/p/10226493.html)  
原问题：[Difference between .h files and .inc files in c]ttps://stackoverflow.com/questions/38402525/difference-between-h-files-and-inc-files-in-c)  
C/C++的标准惯例是将class、function的声明信息写在.h文件中。.c文件写class实现、function实现、变量定义等等。然而对于template来说，它既不是class也不是function，而是可以生成一组class或function的东西。  
编译器（compiler）为了给template生成代码，他需要看到声明（declaration ）和定义（definition ），因此他们必须不被包含在.h里面。  
为了使声明、定义分隔开，定义写在自己文件内部，即.inc文件，然后在.h文件的末尾包含进来。当然除了.inc的形式，还可能有许多其他的写法.inc, .imp, .impl, .tpp, etc.  

### 分离式编译 separate compilation
分离编译模式是指：一个程序（项目）由若干个源文件共同实现，而每个源文件单独编译生成目标文件，最后将所有目标文件连接起来形成单一的可执行文件的过程。  
分离编译模式是C/C++组织源代码和生成可执行文件的方式。在实际开发大型项目的时候，不可能把所有的源程序都放在一个头文件中，而是分别由不同的程序员开发不同的模块，再将这些模块汇总成为最终的可执行程序。  
每个源文件生成独立的目标文件（obj文件），然后通过连接（Linking）将目标文件组成最终的可执行文件。  
简要过程  
1. 处理(Preprocessing)  
.cpp -> .i  
基本只处理#开头的文件  
2. 编译(Compilation)  
.i -> .s  
把预处理完的文件，进行语法分析、词法分析、语义分析及优化后生成相应的汇编代码文件  
3. 汇编(Assembly)  
.s -> .o  
将汇编代码文件转变成机器可以执行的目标文件  
4. 连接(Linking)  
.o -> .exe  
将obj链接在一起组成可执行文件  

### 静态链接与动态链接 Static and Dynamic linking
- 静态链接  
 由linker在链接时将库的内容加入到可执行程序  
 静态链接库，Windows下以.lib为后缀，Linux下以.a为后缀  
  - pro  
    - 代码装载速度快，执行速度略比动态链接库快  
    - 只需保证在开发者的计算机中有正确的.lib文件，在以二进制形式发布程序时不需考虑在用户的计算机上.lib文件是否存在及版本问题  
  - con  
    - 使用静态链接生成的可执行文件体积较大，包含相同的公共代码，造成浪费

- 动态链接  
  在可执行文件装载时或运行时，由操作系统的装载程序加载库  
  动态链接库，Windows下以.dll为后缀，Linux下以.so为后缀  
  - pro  
    - 生成的可执行文件较静态链接生成的可执行文件小  
    - 适用于大规模的软件开发，使开发过程独立、耦合度小，便于不同开发者和开发组织之间进行开发和测试  
    - 不同编程语言编写的程序只要按照函数调用约定就可以调用同一个DLL函数  
    - DLL文件与EXE文件独立，只要输出接口不变（即名称、参数、返回值类型和调用约定不变），更换DLL文件不会对EXE文件造成任何影响，因而极大地提高了可维护性和可扩展性  
  - con  
    - 使用动态链接库的应用程序不是自完备的，它依赖的DLL模块也要存在，如果使用载入时动态链接，程序启动时发现DLL不存在，系统将终止程序并给出错误信息
    - 速度比静态链接慢
    
### 指针和引用的区别 pointer & reference  
引用本身并非一个对象，引用定义后就不能绑定到其他的对象了  
指针并没有此限制，相当于变量一样使用  

### 指针和const  
pointer to const（指向常量的指针）：不能用于改变其所指对象的值
```cpp
const double pi = 3.14; const double *cptr = &pi;。
```   
const pointer：指针本身是常量，也就是说指针固定指向该对象，（存放在指针中的地址不变，地址所对应的那个对象值可以修改）
```cpp
int i = 0; int *const ptr = &i;  
```
顶层 ( Top-level ) const：指针本身是个常量  
底层 ( Low-level ) const：指针指向的对象是个常量,拷贝时严格要求相同的底层const资格  


### 头文件保护符 header guard
存储于头文件内， 防止多个头文件include时候互相交叉，重复定义
```cpp
#ifndef SALES_DATA_H  //SALES_DATA_H未定义时为真
#define SALES_DATA_H
struct Sale_data{
    ...
}
#endif
```

### 显式类型转换
- `static_cast`  
  任何明确定义的类型转换，只要不包含底层const，都可以使用。  
  `double slope = static_cast<double>(j); `
- `dynamic_cast`  
  支持运行时类型识别  
- `const_cast`  
  只能改变运算对象的底层const，一般可用于去除const性质。  
  ```cpp
  const char *pc; 
  char *p = const_cast<char*>(pc);
  ```  
- `reinterpret_cast`  
  通常为运算对象的位模式提供低层次上的重新解释  

## 内存
### 栈内存与堆内存
Heap memory只是一个名字，和heap的数据结构没有关系
### 内存碎片化
### 智能指针 Smart pointer
传统c++里用`new`跟`delete`或者RAII进行内存管理容易出错造成内存泄漏，所以现代c++引入了智能指针。（`RAII：Resource Acquisition Is Initialization`，指经由类的构造函数跟析构函数进行资源的获得与释放）  
自带计数器，每次都引用时增加，反之指针被删除时减少，在变为0的时候销毁释放内存。  
- `shared_ptr`:允许多个指针指向同一个对象
```cpp  
auto pointer = std::make_shared<int>(10);
auto pointer2 = pointer; // 引用计数+1
auto pointer3 = pointer; // 引用计数+1
```
- `unique_ptr`:独占所指向的对象  
```cpp
  std::unique_ptr<int> pointer = std::make_unique<int>(10); // make_unique 从 C++14 引入  
  std::unique_ptr<int> pointer2 = pointer; // 非法
```
虽然无法共享，但是可以使用`std::move`转移给其他`unique_ptr`
```cpp
std::unique_ptr<int> p2(std::move(p1)); //将p1指向的对象变成p2指向，此后p1为空
```
- `weak_ptr`:引用时不会引起计数增加
下面这种用法会导致离开mian后，ab内部依然互相指向导致无法被删除。
这种情况下用`weak_ptr`则可以避免内存泄漏
```cpp
struct A { std::shared_ptr<B> pointer; };
struct B { std::shared_ptr<A> pointer; };
int main() {
    auto a = std::make_shared<A>();
    auto b = std::make_shared<B>();
    a->pointer = b;
    b->pointer = a;
}
```

### try语句块和异常处理
throw表达式：异常检测部分使用 throw表达式来表示它遇到了无法处理的问题。我们说 throw引发 raise了异常。  
try语句块：以 try关键词开始，以一个或多个 catch字句结束。 try语句块中的代码抛出的异常通常会被某个 catch捕获并处理。 catch子句也被称为异常处理代码。  
异常类：用于在 throw表达式和相关的 catch子句之间传递异常的具体信息。  

### namespace
- unnamed namespace
  允许不给namespace定义名字，在这种情况下，定义的变量只能在本文件内调用。  
  ```cpp
  namespace
  {
      void f(){cout<<"a";}
  }
  
  int main()
  {
      f();
  }
  ```
  
### 概念
运算符重载 operator overloading：重定义运算符  
常量 literals：的固定值  
左值 L-value：可以取地址，位于等号左边  
右值 R-value：没法取地址，位于等号右边  
