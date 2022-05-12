### 上：

#### 1、推荐书籍：

C++ Primer第五版   The C++ programming language C++11

Effective C++ 第三版

The Standard Library STL源码剖析

#### 2、头文件定义与声明

complex.h

complex-test.cpp

```c++
#indef _COMPLEX_
#define _COMPLEX
#include<cmath>
//前置声明
class ostream;
class complex;
complex& _doapl(complex* ths,const comples& r);
//类声明
class complex {
    ...
};
//类定义
complex::function ...
#endif
```

#### 3、构造函数

- inline函数

函数若在class 体内定义完成，便成为inline候选人

能否成为inline由编译器决定

类内声明，类外定义的函数加关键字inline也有机会成为inline,最终也是由编译器决定。

函数太复杂不能成为inline

- 访问级别 

```c++
class copmlex 
{
public:
	complex(double r = 0, double i = 0):re(r),im(i) {}
	complex& operator += (const complex&);
	double real () const {return re;}
	double imag () const {return im;}
private:
	double re, im;
	friend complex& _doapl (complex*, const complex&);
};
void test01()
{
 	complex c1(2,1);//创建对象会调用构造函数
    //通过对象调用某一个函数,public可以访问
	cout << c1.real();
	cout << c1.imag();   
}
//数据放进private 函数放在public
```

- 构造函数

当创建一个对象时，构造函数会被调用

构造函数可以有很多个，叫做重载，这个重载在编译器看来函数是不同名的。

```c++
class copmlex 
{
public:
	complex(double r = 0, double i = 0):re(r),im(i) {} //构造函数 初始化
    //函数名和类名相同（默认参数）
    //complex(double r = 0, double i = 0){
    //	  re = r; im = i; 
	//} 此种赋值写法不提倡
    complex (): re(0),im(0) {} //构造函数，和上一个构造函数是冲突的
	complex& operator += (const complex&);
	double real () const {return re;}
	double imag () const {return im;}
private:
	double re, im;
	friend complex& _doapl (complex*, const complex&);
};
void test01(){
    //创建对象时调用构造函数
	complex c1(2,1);
    complex c2;
    complex *p = new complex(4);
    void real(double r) {re = r;} //函数重载
}
```

构造函数放在private里，就不允许外界创建这个类的对象

设计模式Singleton（单体）会使用这种

```c++
class A {
public:
    static A& getInstance();
    steup() {.....}
private:
    A(); //构造函数放在private里面
    A(const A& rhs); //构造函数重载
    //说明只创建了一份对象
    ...
};
A& A::getInstance(){
    static A a;
    return a;
}
//外界如何调用这一份？
A::getInstance().setup();
```

- 常量成员函数

函数后面加const     {}前面

```c++
class copmlex 
{
public:
	complex(double r = 0, double i = 0):re(r),im(i) {} //构造函数 初始化
	complex& operator += (const complex&);
	double real () const {return re;}
	double imag () const {return im;}
private:
	double re, im;
	friend complex& _doapl (complex*, const complex&);
};
void test01(){
//类内的函数 不会改变数据(读数据)内容用const
{
    const complex c1(2,1);
    //此处有const上面也需要有const
    //如果上面不加const，那么改变了就使下面不能使用
    cout << c1.real();
    cout << c1.imag();
}
```

#### 4、参数传递与返回值

（传值和传引用&） 返回值传递

传值：整包传递    传引用类似于传指针，推荐使用

```c++
class copmlex 
{
public:
	complex(double r = 0, double i = 0):re(r),im(i) {}
	complex& operator += (const complex&);//传引用+返回值传引用
	double real () const {return re;}
	double imag () const {return im;}
private:
	double re, im;
	friend complex& _doapl (complex*, const complex&);
};
{
    complex c1(2,1);
    complex c2;
    c2 += c1;
    cout << c1.imag();
}
```

参数传递使用传引用，如果传过去不想要对方改变值的话 加const

- friend友元：

封装 private  友元可以直接使用private,不需要通过函数

友元打破了封装。

- 相同class的各个对象互为友元

```c++
class complex {
public:
	complex(double r = 0, double i = 0) : re(r),im(i) {}
	int func(const complex& parm) {
		return param.re + param.im;
        //这里为什么函数可以直接使用private的变量
        //类内的成员之间互为友元
	}
private:
	double re, im;
};
void test01(){
    complex c1(2,1);
    complex c2;
    c2.func(c1);
}
```

- 类外的各种定义

什么情况下传值 什么时候传引用

```c++
inline complex& 
_doapl(complex& ths, const complex& r) {
	ths->re += r.re;//第一参数改变，第二参数不改变
	ths->im += r.im;
    //另一种情况是创建了一个局部变量将结果存进去，局部变量保存在栈区，由编译器自动分配释放，函数执行完就被释放了，这种情况不能使用传引用，本体已经不存在，传引用没意义。
	return *ths;
}
inline complex&
complex::operator += (const complex& r) {
	return _doapl (this,r);
}
```

#### 5、操作符重载与临时对象

##### 5.1、操作符重载1-成员函数

操作符如何被编译器看待 

```c++
{
	complex c1(2,1);
	complex c2(5);
	c2 += c1; //二元操作符
}
```

```c++
inline complex& 
_doapl(complex& ths, const complex& r) {
	ths->re += r.re;
	ths->im += r.im;
	return *ths;
}
inline complex& 
complex::operator += (const complex& r) {
	return _doapl (this,r);
}
```

```c++
inline complex& 
complex::operator += (this,const complex& r) {
	return _doapl (this,r);
}
//所有成员函数都带有一个隐藏的参数this pointer,代码里不能写出来
//谁调用这个函数，this就指向谁  上面c2是this
```

- 传引用语法分析

传送者无需知道接收者是以引用形式接收

```c++
inline complex&
_doapl(complex* this, const complex& r) {
	...
	return *ths;//此处返回的是value，接收端以引用方式接收，此时不需要管接收端是以什么形式接收的，只需要送出去即可。
    //如果用指针传，必须知道是以指针接收
}
inline complex&
complex::operator += (const complex& r) {
	return _doapl(this,r);
}
c2 += c1;//此句执行完使用了上面的重载运算符函数，无需管接收返回值以什么方式接收
c3 += c2 += c1; //执行顺序，c1加到c2，c2加到c3
//此时接收方式不能随意设置，c2还需继续使用
```

- 类body外的各种定义

```c++
inline double imag(const complex& x){
	return x.imag();
}
inline double real(const complex& x){
	return x.real();
}
{
	complex c1(2,1);
	cout << imag(c1);
	cout << real(c1);
}
```

##### 5.2、操作符重载2-非成员函数

针对三种不同用法，这里对应三个函数

全局函数没有 this pointer

- +加号重载

```c++
inline complex operator+(const complex& x,const complex& y) {
	return complex(real(x) + real(y),imag(x) + imag(y));
}
inline complex operator+(const complex& x,double y) {
	return complex(real(x)+y, imag(x));
}
inline complex operator+(double x,const complex& y) {
	return complex(x + real(y), imag(y));
}
void test01(){
    complex c1(2,1);
    complex c2;
    c2 = c1 + c2;//虚数+虚数
    c2 = c1 + 5;//虚数+实数
    c2 = 7 + c1;//实数+虚数
}
//这里三个函数为什么不返回引用？
//此处三个函数体内 相加的结果会存入一个局部变量中，局部变量在函数体执行完后被释放，因此不能返回引用，本体已经不存在
```

- 临时对象 typename(); 

```c++
{
	int(7);
	complex c1(2,1);
	complex c2;
	complex();//临时对象
	complex(4,5);//临时对象
	cout << complex(2);
}
```

只是创建一个临时对象（空间）存放运算结果 ，运算结果马上作为函数返回值被返回

- 正号、负号重载

```c++
inline complex operator+(const complex& x) {
	return x;
}
//取正所有结果不变，返回引用也是可以的
inline complex operator-(const complex& x) {
	return complex(-real(x),-imag(x));
}
//取负时创建了新的对象，要存入局部变量保存
{
	complex c1(2,1);
	complex c2;
	cout << -c1;
	cout << +c2;
}
//如何区分 正号和加？ 根据参数的个数
```

- ==重载

```c++
inline bool operator==(const complex& x, const complex& y) {
	return real(x) == real(y) && imag(x) == imag(y);
}
inline bool operator==(const complex& x, double y) {
	return real(x) == real(y) && imag(x) == 0;
}
inline bool operator==(double x, const complex& y) {
	return real(x) == 0 && imag(x) == imag(y);
}
//这个实例中也没有返回引用
{
    complex c1(2,1);
    complex c2;
    cout << (c1 == c2);
    cout << (c1 == 2);
    cout << (1  == c2);
}
```

- 共轭复数

```c++
inline complex conj(const complex& x){
	return complex(real(x), -imag(x));
}
```

- <<操作符重载

```c++
ostream& operator<<(ostream& os,const complex& x) {
    //os不加const，是因为os在cout时状态一直被改变
	return os << '(' << real(x) << ',' << imag(x) << ')';
}
{
	complex c1(2,1);
	cout << conj(c1);//(2,-1)
	cout << c1 << conj(c1);//(2,1)(2,-1)
    //操作符<<作用于左边的cout
    //因此不能写成成员函数，只能写成全局函数
    //cout是ostream 
    //为了实现连串的输出，使用返回引用
}
```

步骤：

1、考虑参数是否传引用

2、考虑参数是否需要加const

3、考虑返回是否需要传引用

4、考虑返回是否需要加const

总结上述复数的例子：设计一个class的注意事项：

构造函数的初始化、

函数是否需要加const、

参数传递尽量使用传引用,要不要加const、

return 是否传引用，是否加const

数据放进private、函数放进public

#### 6、复习complex类的实现过程

```c++
#indef _COMPLEX_
#define _COMPLEX_

class complex
{
public:
    //构造函数 初值列
    complex(double r = 0, i = 0):re(r),im(i){}
    complex& operator += (const complex&);//重载+=运算符的声明
    double real() const { return re; }//只是取出来不要改变
    double imag() const { return im; }
private:
    double re,im;
    friend comlpex& _doapl(complex*, const complex&);
};
//两个成员函数
inline complex& _doapl(complex* ths, cosnt complex& r) {
    ths->re += r.re;
    ths->im += r.im;
    return *ths;
}
inline complex& complex::operator += (const complex& r) {
 //+=两个对象，成员函数作用在左边，左边成为隐藏参数，参数只需要右边的一个
    return _doapl(this, r);
}
//两个非成员函数 +重载
inine complex operator + (const complex& x, const complex& y) {
    //结果放在局部变量中，返回不能传引用
    return complex(real(x) + real(y),
                   imag(y) + imag(y));
    //类名加()就是创建一个位置,临时对象
}
// 非成员函数 重载<<操作符
#include<iostream.h>
inline ostream& operator << (ostream& os, const complex& x) {
    os << '(' << real(x) << ')' << ',' << imag(x) << ')';
    //cout << c1 << endl; 由于会出现连续使用的情况，返回值类型不能是void，应该是cout
}
#endif
```

以上获得的代码是 complex.h   complex_test.cpp

#### 7、三大函数、拷贝构造、拷贝复制、析构 big three

重点：深拷贝与浅拷贝

类内有指针类型的成员变量,就要做动态分配，一定要有拷贝构造和拷贝赋值

```c++
//string.h
#indef _MYSTRING_
#DEFINE _MYSTRING_
class String{
public:
    String(const char* catr = 0);//构造函数
    String(const String& str);//拷贝构造
    String& operator=(const String& str);//操作符=重载 拷贝复制
    ~String();//析构函数
    char* get_c_str()const {
        return m_data;
    }
private:
    char* m_data;
};
//构造函数
inline String::String(const char* cstr = 0)
{
    if(cstr) {
        m_data = new char[strlen(cstr) + 1];
        //字符串结尾有结束符
        strcpy(m_data, cstr);
    }
    else { //未指定初值
        m_data = new char[1];
        *my_data = '\0';
    }
}
//析构函数
inline String::~String(){
    delete[] m_data;
}
{
    String s1;
    String s2("hello");
    String *p = new String("hello");
    delele p;
    //这里会调用三次析构函数
}
```

如果自己不写拷贝构造，编译器默认也会给一套默认拷贝构造函数。默认拷贝构造函数只是进行单纯bit位的复制。b = a编译器将a的内容复制给b，此时b原本的内容还在，b和a都指向A的内容，但是没有对象指向b原本的内容，造成内存泄露，这种情况叫做浅拷贝。

![1640850140861](.assets\1640850140861-1652325252232.png)

```c++
inlin String& String::operator=(const String& str) {
	if(this == str) { //检测是不是自我赋值
		return *this;
	}
	else {
		delete[] m_data;
		m_data = new char[strlen(str.m_data) + 1];
		strcpy(m_data, str.m_data);
		return *this;
	}
}
{
    String s1("hello");
    String s2(s1);
    s2 = s1;//和上一句是一样的   
}
```

深拷贝：b = a。a和b都有自己的初始值，b先把自己的内容清空，然后b创建一个和a相同的空间，然后将a的内容复制过来，这样就完成了深拷贝。

![1640850727459](.assets\1640850727459.png)

为什么写自我赋值？

<img src=".assets\1640850759818.png" alt="1640850759818" style="zoom:50%;" />

```c++
#include<iostream.h>
ostream& operator<<(ostream& os, const String& str) {
	os << str.get_c_str();
	return os;
}
{
	String s1("hello")
	cout << s1;
}
```

#### 8、堆、栈与内存管理

栈satck：存放在作用于的一堆内存空间，当调用函数时，函数本身会形成一个stack用来放置它所接收的参数，以及返回地址。在函数体内声明的任何变量（局部变量）所使用的内存都来自于stack。

栈区的数据所占内存在函数体结束之后就被自动释放。

堆heap：由操作系统提供的一堆global内存空间，由程序员动态分配空间，需要手动释放，若不释放程序会在结束后释放。

```c++
class Complex {...};
{
	Complex c1(1,2);  //c1所占的空间来自于栈
	Complex* p = new Complex(3);//初值是3，动态分配的空间
}
```

```c++
//全局变量static
class Complex {...};
{
	static Complex c2(1,2);//局部变量前加关键字static变成静态变量，存放在全局区，其生命在作用于结束之后仍然存在，直到整个程序结束。
}
```

```c++
//global全局变量 其生命在整个程序结束之后才结束，也可以视为静态对象，作用域是整个程序。
class Complex {...};
Complex c3(1,2);//global全局变量
int main(){
    ...
}
```

```c++
//堆对象heap
{
    Complex* p = new Complex;
    ...
    delete p;//不delete会造成内存泄漏
    //内存泄漏：当作用域结束，p所指的heap堆对象仍然存在，但是指针p的生命却结束了，作用域之外再也看不到p，也就没机会delete p.
}

```

new的过程：先分配一块空间，再调用构造函数

```c++
Complex* pc = new Complex(1,2);
//编译器将上句转化为：
void* mem = operator new(sizeof(Complex));//分配空间
pc = static_cast<Complex*>(mem);//转型
pc->Complex::Complex(1,2); //构造函数
```

![1640852608389](.assets\1640852608389.png)

delete的过程：首先调用析构函数，在释放内存空间

```c++
String *ps = new String("hello");
delete ps;
//编译器将上句代码转换为：
String::~String(ps);//析构函数
operator delete(ps);//释放内存
```

![1640852812193](.assets\1640852812193.png)

动态分配new的内存块

以Complex复数为例，new的空间布置两个double 8个字节，在实际的编译器中，为便于系统回收，在区域块的上面和下面也会开辟一些其他的空间。

![1640853235116](.assets\1640853235116.png)

array new 要搭配 array delete

```c++
//array new 要搭配 array delete
String* p = new String[3];
...
delete[] p; //唤起三次析构函数
delete p; // 唤起一次析构函数，也会造成内存泄漏
```

#### 9、复习String类的实现过程

```c++
//string.h复习 完整代码
class String
{
public:
    String(const char* cstr = 0);//构造函数
    String(const String& str);//class带指针，big three1:拷贝构造
    String& operator=(const String& str);//big three2:拷贝复制
    ~String();//析构函数
    //成员函数
    char* get_c_str() const { return m_data; }//m_data只是返回没有改变，函数后面加了const
private:
    char* m_data;
};
//构造函数
inline String::String(const char* cstr = 0) {
    if(cstr) {
        m_data = new char[strlen(cstr) + 1];
        strcpy(m_data, cstr);
    }
    else {
        m_data = new char[1];
        *m_data = '\0';
    }
}
//析构函数
inline String::~String(){
    delete[] m_data;
}
//拷贝构造函数
inline String::String(const String& str) {
    m_data = new char[strlen(str.m_data) + 1];
    strcpy(m_data, str.m_data);
}
//拷贝复制函数
inline String& String:: operator=(const String& str) {//&引用
    if(this == &str) {//&取地址
        return *this;
    delete[] m_data;
	m_data = new char[strlen(str.m_data) + 1];
	strcpy(m_data,str.m_data);
	return *this;
}
```

#### 10、扩展补充：类模板、函数模板及其他

static补充：静态函数没有this指针

![1640857625543](.assets\1640857625543.png)

```c++
class Account
{
public:
    static double m_rate;//静态数据
    //静态函数
    static void set_rate(const double& x) { m_rate = x; }
};
double Account::m_rate = 8.0; //静态变量在类内声明，类外定义
int main(){
    Account::set_rate(5.0); //通过类名调用
    Account a; //通过对象调用
    a.set_rate(7.0);
}
//调用static函数的方式有两种：通过对象调用；通过类名调用
```

类模板

```c++
template<typename T>
class complex
{
public:
	complex(T r = 0, T i = 0) :re(r), im(i) { }
	complex& operator+= (const complex&);
	T real() const {return re;}
	T imag() const {return im;}
private:
	T re, im;
	friend complex& _doapl(complex*, const complex&)
};
{
    complex<double> c1(2.5,1.5);
    complex<int> c2(1,2);
}
```

函数模板

```c++
class stone
{
public:
    stone(int w, int h, int we) : _w(w), _h(h), _weight(we) {}
    bool operator< (const stone& rhs) const{ //重载<运算符
        return _weight < rhs._weight;
    }
    private:
    	int _w, _h, _weight;
};
template <class T>
inline const T& min(const T& a, const T& b) {
    return b < a ? b : a;
}
```

进一步补充：命名空间namespace

```c++
namespace std 
{
	...
}
//三种方式
//using directive
#include<iostream.h>
using namespace std;
int main(){
    cin >> ...;
    cout << ...;
    return 0;
}
//using declaration
#include<iostream.h>
using std::cout;
int main(){
    std::cin >> ...;
    cout << ...;
    return 0;
}
//
#include<iostream.h>
int main(){
    std::cin >> ...;
    std::cout << ...;
    return 0;
}
```

上面介绍如何写一个类？

下面是类之间的关系

#### 11、组合与继承

面向对象 三大关系 复合 委托 继承  

- composition复合![1640929100269](.assets\1640929100269.png)

queue和deque是同时创建的

```c++
template<class T, class Sequence = deque<T> >
class queue
{
    ...
protected:
    //表明queue里面有deque,由deque的功能实现queue 
    Sequence c;//底层容器
public:
    //函数
    bool empty() const{ return c.empty(); }
    size_type size() const{ return c.size(); }
    reference front() { return c.front(); }
    reference back() { return c.back(); }
    //deque是两端进出，queue是先进先出
    void push(const value_type& x) { c.push_back(x);}
    void pop() { c.pop_front(); }
};
```

复合关系下的构造函数和析构函数<img src=".assets\1640929475159.png" alt="1640929475159" style="zoom:50%;" />

构造由内而外，container的构造函数首先用component的默认构造函数，然后才执行自己。

```
Container::Container(...) : Component() {...};
```

析构由外而内，Container的析构函数首先执行自己，然后才调用Component的析构函数。

```
Container::~Container(...) {... ~Compenent() };
```

Component()和~Compenent()是编译器加的

- Delegation委托 ：Component by reference <img src=".assets\1640931986626.png" alt="1640931986626" style="zoom: 50%;" />

第一个类作为对外的接口，第二个类函数才做具体的函数实现

pointer to implementation

一个类的指针成员变量指向另一个类，两个类用指针相连

![1640932298210](.assets\1640932298210.png)

```c++
//String.hpp
class StringRep;
class String{
public:
    String();
    String(const char* s);
    String(const String& s);
    String& operator= (const String& s);
    ~String();
private:
    StringRep* rep;
};
```

```c++
//String.cpp
#include "String.hpp"
namespace{
class StringRep{
	friend class String;
    StringRep(const char* s);
    ~StringRep();
    int count;
    char* rep;
};   
}
```

-  Inheritance继承<img src=".assets\1640933290933.png" alt="1640933290933" style="zoom:50%;" />

```c++
struct _List_node_base
{
	_List_node_base* _M_next;
    _List_node_base* _M_prev;
};
template<typename _Tp>
struct _List_node : public _List_node_base
{
	_Tp _M_data;  
};
```

继承下的构造和析构函数：<img src=".assets\1640932979693.png" alt="1640932979693" style="zoom:50%;" />

父类的构造函数必须是virtual，否则会出现undefined behavior。

在设计类时，只要是父类，或者之后可能成为父类，构造函数之前都加上virtual

构造由内而外，析构由外而内。

```c++
Derived::Derived(...): Base() {...};
Derived::~Derived(...) {... ~Base() }
```

#### 12、虚函数与多态

继承搭配虚函数  成员函数的三种形态

非虚函数：不希望子类重新定义（覆盖）它

虚函数：希望子类重新定义（覆盖），且他有默认定义

纯虚函数：希望子类一定要重新定义，你对它没有默认定义。

```c++
class Shape{
public:
	virtual void draw() const = 0; //纯虚函数
    virtual void error(const std::string& msg); //虚函数
    int objectID() const; //非虚函数
    ...
};
class Rectangle : public Shape{...};
class Ellipse : public Shape{...};
```

实例：

```c++
class Cdocument
{
public:
    void OnFileOpen()
    {
        cout << "dialog..." << endl;
        Serialize();
        cout << "close file.." << endl;
    }
    virtual void Serialize() { };//空函数
};
class CMyDoc : public Cdocument
{
public:
    virtual void Serialize()
    {
        cout << "CMyDoc:Serialize()" << endl;
    }
};
```

继承和复合关系下的构造和虚构函数：![1640935835350](.assets\1640935835350.png)

构造函数由内而外，首先调用基类的构造函数，然后是component的构造函数，最后执行自己。

析构函数由外而内，首先执行自己，然后是component的析构函数，最后调用基类的析构函数。？？

委托+继承：

```c++
class Subject
{
    int m_value;
    vector<Observer*> m_views;
public:
    void attach(Observer* obs) 
    {
        m_views.push_back(obs);
    }
    void set_val(int value)
    {
        m_value = value;
        notify();
    }
    void notify()
    {
        for(int i = 0; i < m_views.size(); i++) {
            m_views[i]->update(this, m_value);
        }
    }
};
class Observer
{
public:
    virtual void update(Subject* sub, int value) = 0;
};
```

#### 13、委托相关设计

### 下：C++程序设计兼谈对象模型

#### 2、转换函数

没有return type

```c++
class Fraction
{
public:
	Fraction(int num, int den = 1) : m_numerator(num), m_denominator(den) { }
    operator double() const {
    return (double) (m_numerator/m_denominator);
    }  //转换函数
private:
    int m_numerator;  //分子
    int m_denominator;//分母
};
Fraction f(3,5);
double d = 4 + f; //调用operator double()将f转为double
```

#### 3、non-explicit-one-argument ctor

非显式单实参构造函数

two parameters one argument

```c++
class Fraction
{
public:
    //一个实参，另一个参数有默认值，3/1=3
	Fraction(int num, int den = 1) : m_numerator(num), m_denominator(den) { }
    Fraction operator+(const Fraction& f) {
    	return Fraction(.....);
    } 
    //设计的函数是分数+分数 Fraction+Fraction
private:
    int m_numerator;  //分子
    int m_denominator;//分母
};
Fraction f(3,5);
Fraction d2 = f + 4; //调用non-explicit ctor将4转为Fraction(4,1);然后调用operator+
```

explicit-one-argument ctor

explicit用在构造函数之前，explicit关键字的作用就是防止类构造函数的隐式自动转换.

```c++
class Fraction
{
public:
	explicit Fraction(int num, int den = 1) : m_numerator(num), m_denominator(den) { }
    operator double() const {
    return (double) (m_numerator/m_denominator);
    } 
    Fraction operator+(const Fraction& f) {
    	return Fraction(.....);
    } 
private:
    int m_numerator;  //分子
    int m_denominator;//分母
};
Fraction f(3,5);
double d = f + 4; //不加explicit报错，模糊；
Fraction d = f + 4;//加上之后4不能自动转换为4/1,报错
```

#### 4、pointer-like classes智能指针

  将类设计的像一个指针 shared_ptr<img src=".assets\1641044072721.png" alt="1641044072721" style="zoom:50%;" />

```c++
template<class T>
class shared_ptr
{
public:
    T& operator*() const { return *px; }
    T& operator->() const { return px; }
    shared_ptr(T* p):px(p) { }
private:
    T* px;
    long* pn;  
};
```

迭代器

```c++
template<class T, class Ref, class Ptr>
struct _List_itertor {
    reference operator*() const {
        return (*node).data;
    }
    pointer operator->() const {
        return &(operator* ());
    }
};
```

C++中操作符重载非常重要

#### 5、function-like classes

将类设计的像一个函数  所谓仿函数

类内含有operator(),类的对象也叫做函数对象

```c++
template <class T>
struct identity {
    const T& operator()(const T& x) const {
        return x;
    }
};
template <class Pair>
struct select1st {
	const typename Pair::first_type& operator() (const Pair& x) const{
        return x.first;
    }  
};
template <class Pair>
struct select2nd {
    const typename Pair::second_type& operator() (const Pair& x) const {
        return x.second;
    }
};
```

```c++
template <class T1, class T2>
struct pair {
    T1 first;
    T2 second;
    pair() : first(T1()), second(T2()) {}
    pair(const T1& a, const T2& b) : first(a), second(b) {}
};
```

标准库中，仿函数所使用的奇特的基类

#### 6、namespace

```c++
using namespace std;

#include<iostream>
#include<memory> //shared_ptr
namespace jj01
{
    void test_member_template()
    {...}
}
namespace jj02
{
    void test_member_template_param()
    {...}
}
int main(int argc, char** argv)
{
    jj01::test_member_template();
    jj02::test_member_template_param();
}
```

#### 7、类模板

#### 8、函数模板

#### 9、成员模板

在类模板里面，成员函数也是模板

```c++
template<class T1, class T2>
struct pair {
	typedef T1 first_type;
    typedef T2 second_type;
    
    T1 first;
    T2 second;
    
    pair():first(T1()),second(T2()) {}
    pair(const T1& a, const T2& b) : first(a), second(b) {}
    //成员模板
    template<class U1, class U2>
    pair(const pair(U1,U2)& p) : first(p.first), second(p.second) {}
};
```

```c++
class Base1 { };
class Derived1:public Base1{ };

class Base2 { };
class Derived2:public Base2{ };

pair<Derived1, Derived2> p;
pair<Base1, Base2> p2(p);

pair<Base1, Base2> p2(pair<Derived1, Derived2>());
//将继承类组成的pair可以放到两个基类组成的pair，反之不行。
//将继承类的成员写进构造函数作为基类的初值
```

```c++
template <typename _Tp>
class shared_ptr : public _shared_ptr<_Tp>
{
    template <typename _Tp1>
    explicit shared_ptr(_Tp1* _p) : _shared_ptr<_Tp1> (_p) {}
};
Base1 *ptr = new Derived1;
shared_ptr<Base1> sptr(new Derived1);
```

#### 10、模板特化   泛化(模板)

```c++
//泛化(全泛化)
template <class Key>
struct hash { };
```

```c++
template <>  
//上面Key（指定的任意类型）已经绑定，这里不用写
struct hash<char> //这里将Key指定
    size_t operator() (char x) const {
    	return x;
	}
}
template <>
struct hash<int>
    size_t operator() (int x) const {
    	return x;
	}
}
template <>
struct hash<long>
    size_t operator() (long x) const {
    	return x;
	}
}
cout << hash()(long 1000);
```

#### 11、模板偏特化

个数偏

```c++
template<typename T, typename Alloc=...>
class vector
{
    ...
};
template<typename, Alloc=...>
//其中一个绑定为bool
class vector<bool, Alloc>
{
    ...
}
```

范围偏

将任意类型缩小为指针

```c++
template <typename T>
class C { ... };
```

```c++
template <typename T>
class C<T*> { ... };
```

```c++
C<string> obj1;
C<string*>obj2;
```

#### 12、tempalte template parameter 模板模板参数

```c++
template<typename T, template <typename T> class Container>
class XCls
{
private:
    Container<T> c;
public:
    ...
};
```

```c++
template<typename T>
using Lst = list<T, allocator<T>>;
XCls<string, list> mylist1; //错误
XCls<string, Lst> mylist2; 
```

#### 13、关于C++标准库

容器、迭代器、算法（仿函数）

确认支持c++11

以下C++11新特性

#### 14、三个主题

- 数量不定的模板参数

```c++
void print(){
    
}
template <typename T, typename ... Types>
void print(const T& firstArg, const Types&... args) {
    //参数：一个+一包(不定个数)
    cout << firstArg << endl;
    print(args...);
}
```

- auto：自动推断返回值类型

```c++
list<string> c;
...
list<string>::iterator ite;
ite = find(c.begin(), c.end(), target);
```

→C++11:

```c++
list<string> c;
...
auto ite = find(c.begin(), c.end(), target);
```

- ranged-based for

```c++
for(decl : coll) {
	statement
}
vector<double> vec;
for(auto elem : vec) {
    cout << elem << endl;
} //传值
for(auto& elem : vec) {
    elem *= 3;
} //传引用
```

#### 15、reference 引用

内存角度

```c++
int x;
int* p = &x;
int& r = x;
int x2 = 5;
r = x2; //r不能代表其他，现在r和x都变成5
int& r2 = r;
```

sizeof (r) = sizeof(x);  &x = &r;

引用和原来的对象的大小相同，地址也相同。（都是假象）

```c++
typedef struct Stag {
    int a, b, c, d;
}S;
int main() {
    double x = 0;
    double* p = &x;
    double& r = x;
    
    cout << sizeof(x) << endl; //8
    cout << sizeof(p) << endl; //4
    cout << sizeof(r) << endl; //8
    cout << p << endl; //0065FDFC
    cout << *p << endl; //0
    cout << x << endl; //0
    cout << r << endl; //0
    cout << &x << endl;//0065FDFC
    cout << &r << endl;//0065FDFC
    S s;
    S& rs = s;
    cout << sizeof(s) << endl; //16
    cout << sizeof(rs) << endl;//16
    cout << &s << endl; //0065FDE8
    cout << &rs << endl;//0065FDE8
}
```

引用的常见用途：

引用就是一种很好的pointer,引用常用在参数传递和返回类型上。

```c++
void func1(Cls* pobj) { pobj->xxx(); }
void func2(Cls obj)   { obj.xxx();   }
void func3(Cls& obj)  { obj.xxx();   }
Cls obj;
func1(& obj);
func2(obj);
func3(obj);//2和3相同，3传递的更快
```

#### 17、关于vptr和vtbl（对象模型）

vptr：虚指针 vtbl：虚表

```c++
class A {
public:
    virtual void vfunc1();
    virtual void vfunc2();
    		void func1();
    		void func2();
private:
    int m_data1, m_data2;
};
class B : public A {
public:
    virtual void vfunc1();
    		void func2();
private:
    int m_data3;
};
class C : public B {
public:
    virtual void vfunc1();
    		void func2();
private:
    int m_data1, m_data4;
};
```

![](.assets\1641128063626.png)

类内有虚函数，实际对象地址里就多了一个指针，无论几个虚函数，都只多一个指针。

```c++
(*(p->vptr) [n]) (p);
//通过指针调用它的虚指针再找到它的虚表，取出第n个当作函数指针来调用。（上面图中的路线）动态绑定
```

![1641132564969](.assets\1641132564969.png)

指定一个指针容器。

静态绑定：直接找到函数调用并返回

动态绑定三个条件：通过指针 向上转型（new一个子类 其实类型是父类） 调用虚函数<img src=".assets\1641131378611.png" alt="1641131378611" style="zoom:50%;" />

#### 18、动态绑定

![1641131589900](.assets\1641131589900.png)

#### 18、const

函数名后面+const

```c++
double real() const { return re; }
```

成员函数不打算改变class的data数据，只读不改

当成员函数的const和non-const同时存在，const object只会调用const版本，non-const object只会调用non-const版本

non-const对象可以调用const成员函数，反之不行。

const对象调用const函数不需要COW，non-const对象调用non-const对象需要COW

COW：copy on write

#### 19、关于this

通过对象调用一个函数，这个对象的地址就是this pointer

![1641133200833](.assets\1641133200833.png)

![1641133423345](.assets\1641133423345.png)

this 是图中红色的部分。

```c++
class Cdocument
{
public:
    void OnFileOpen()
    {
        cout << "dialog..." << endl;
        Serialize();  //由子类实现
        cout << "close file.." << endl;
    }
    virtual void Serialize() { };//空函数
};

class CMyDoc : public Cdocument
{
public:
    virtual void Serialize()
    {
        cout << "CMyDoc:Serialize()" << endl;
    }
};
int main() {
    CmyDoc myDoc;
    ...;
    myDoc.OnFileOpen();
}
```

#### 20、new、delete

new：先分配内存，再调用构造函数

delete:先调用析构函数，再释放内存

#### 21、重载new delete

全局重载::operator new  ::operator delete

::operator new[],  ::operator delete[]

```c++
void* myAlloc(size_t size) {
    return malloc(size);
}
void myFree(void *ptr) {
    return free(ptr);
}
```

```c++
//这四个函数不可声明在同一个namespace
//new的部分要定义一个大小
inline void* operator new(size_t size) {
    cout << "jjhou global new()  \n";
    return myAlloc(size);
}
inline void* operator new[](size_t size) {
    cout << "jjhou global new[]()  \n";
    return myAlloc(size);
}
//delete要传一个指针，归还内存
inline void operator delete(void *ptr) {
    cout << "jjhou global delete()  \n";
    return myFree(ptr);
}
inline void operator delete[](void *ptr) {
    cout << "jjhou global delete[]()  \n";
    return myFree(ptr);
}
```

重载成员  operator new/delete

operator new[]/delete[]

```c++
class Foo {
public:
    void* operator new(size_t);
    void operator delete(void*, size_t);
};

{
    Foo *p = new Foo;//3个步骤传给operator new
    ...
    delete p; //2个步骤传给operator delete
}
```

```c++
{
    Foo *p = new Foo[N];//N次
    ...
    delete []p; //N次
}
```

#### 22、示例

```c++

```

#### 23、重载new() delete()

可以重载类成员 operator new(),写出多个版本，但是每个声明都要有独特的参数列。

第一参数必须是size_t，其余参数就是new后面括号里的参数。

```c++
Foo* pf = new(300,'c') Foo;
```

也可以重载 operator delete(),

```c++
class Foo {
public:
    Foo(){
        cout << "Foo::Foo()" << endl;
    }
    Foo(int) {
        cout << "Foo::Foo(int)" << endl;
    }
    void* operator new(size_t size) {
        return malloc(size);
    }
    void* operator new(size_t size, void* start) {
        return start;
    }
};
```

