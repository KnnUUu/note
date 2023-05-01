### 关键字

```Inline```：直接把里面的内容粘贴到函数被调用的地方，可以减少访问函数时花费的时间，但如果函数太长反而会使时间变长，所以一般只用于只有几行的函数。  
```Constexpr```（constant expression）： 编译时就可以算出来（前提是为了算出它所依赖的东西也是在编译期可以算出来的），花费更多编译时间但减少运行时间。  
```Const```： 保证变量运行时不直接被修改。  
```template``` : 把类型当作变量传到函数，使得同一个函数可以处理多种类型的变量。在编译的时候将程序复制成几个对应不同变量类型的函数。与宏不同的地方在于template会对变量类型进行检查。  
```extern```：存储类用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的  

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


### 概念
运算符重载 operator overloading：重定义运算符  
常量 literals：的固定值  
