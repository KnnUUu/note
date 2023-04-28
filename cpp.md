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
