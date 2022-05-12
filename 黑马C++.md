### 1、C++基础入门

常量：#define  const int a =0;

数据类型存在意义：给变量分配合适的内存空间

short  2字节   int 4字节   long 4字节 longlong 8字节

sizeof关键字   sizeof求出数据类型占用内存大小 sizeof(数据类型/变量)

选择语句if  / switch 

```c++
switch( ? ) {

	case0 :   ,break;

	default :  ,break;

}
```

![1637565102166](.assets\1637565102166.png)

末尾元素下标：end = sizeof(a)/sizeof(a[0]) - 1;

#### 1、函数

##### 1、定义

将经常使用的一段代码封装起来，以便于重复使用。

返回值类型 函数值 参数列表 函数体语句 return 表达式

##### 2、使用

定义时是形参，使用时是实参，调用函数时，实参的值传递给形参

##### 3、值传递

当我们做值传递时，函数的形参发生改变，并不会影响实参。

形参分配了新的内存空间，不影响实参

##### 4、函数声明

提前告诉编译器函数的存在。函数定义在主函数之后

##### 5、函数的分文件编写

1、创建.h头文件   2、创建.cpp源文件

3、头文件中写函数的声明   4、源文件中写函数的定义

#### 2、指针

##### 1、基本概念、定义和使用

通过指针间接访问内存

指针的定义：

int a = 10;
int * p;  //定义指针
p = &a;  // 建立指针和变量之间的关系，p指向的是a的地址

指针的使用：可以通过解引用的方式找到指针指向的内存

指针前加*表示解引用 ，找到指针指向的内存中的数据

##### 2、指针所占的内存空间

在32位操作系统下，指针占用4个字节。

##### 3、空指针和野指针

①空指针：指针变量指向内存中编号为0的空间。 

int *p = nullptr; 不能对 * p操作

用途：初始化指针变量。 

空指针指向的内存变量不能被访问。0~255的内存编号被系统占用

②野指针：指针变量指向非法的内存空间

在程序中，避免使用野指针。

int * p = (int *)0X1100; //找不到报错

空指针和野指针都不是我们申请的空间，不要进行访问。

##### 4、const修饰指针

①const修饰指针  常量指针

```c++
int a = 10;
int b = 10;
int * p = &a;
const int * p = &a;  常量指针
```

特点：指针的指向可以修改，但是指针指向的值不可以修改

```c++
*p = 20;  //错误，指针指向的值不可以修改
p = &b;  //正确，指针的指向可以修改
```

②const修饰常量  指针常量

```c++
int * const p = &a;  指针常量
```

特点：指针的指向不可以改，指针指向的值可以改。

```
*p = 20;  //正确，指针指向的值可以修改
p = &b;  //错误，指针的指向不可以修改
```

③const既修饰指针，也修饰常量

```
const int * const p = &a; 
```

特点：指针的指向和指针指向的值都不可以改。

```
*p = 20;  //错误，指针指向的值不可以修改
p = &b;  //错误，指针的指向不可以修改
```

##### 5、指针和数组

利用指针访问数组中的元素

```c++
int arr[10] = {1,2,3,4,5};
int * p = arr; //arr就是数组首地址
cout << *p;  //解引用输出为第一个元素1
p++; //让指针偏移4个字节
cout << *p;  //输出为第二个元素2
```

<img src=".assets\1637571318107.png" alt="1637571318107" style="zoom:50%;" />

##### 6、指针和函数

①值传递

```c++
void swap01(int a, int b) {
	int temp = a;
	a = b;
	b = temp;
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
}
int main() {
	int a = 10;
	int b = 20;
	swap01(a, b);
	cout << "a = " << a << en;
	cout << "b = " << b << endl;
	return 0;
}

//输出的结果,形参的值改变，实参的值不被改变
a = 20
b = 10
a = 10
b = 20
```

②地址传递

利用指针作函数参数，可以修改实参的值

```c++
void swap02(int *a, int *b) {
	int temp = *a;
	*a = *b;
	*b = temp;
	cout << "a = " << a << endl;
	cout << "b = " << b << endl;
}
int main() {
	int a = 10;
	int b = 20;
	swap02(&a, &b); //传入ab的地址
	cout << "a = " << a << en;
	cout << "b = " << b << endl;
	return 0;
}

//输出的结果,形参的值改变，实参的值改变
a = 20
b = 10
a = 20
b = 10
```

![1637572409559](.assets\1637572409559.png)

##### 7、指针、数组和函数

案例：封装一个函数，利用冒泡排序，实现对整型数组的升序排序

#### 3、结构体

##### 1、基本概念、定义和使用

结构体是用户自定义的数据类型，允许用户存储不同的数据类型。

定义：struct关键字 结构体名{结构体成员列表}；

```c++
//1、创建一个学生的数据类型： 学生包括 姓名 年龄 分数
struct Student
{
	//成员列表
	//姓名
	string name;
	//年龄
	int age;
	//考试分数
	int store;
};
//2、通过学生类型创建具体学生（3种方式）
结构体定义时struct不能省略
创建时struct关键字可以省略
通过.访问结构体成员
(1)struct Student s1;
	//给s1属性赋值，通过.访问结构体变量中的属性
	s1.name = "张三";
	s1.age = 20;
	s1.store = 99;
	cout << s1.name << s1.age << s1.store;
(2)struct Student s1 = {"李四", 21,100 };
(3)在定义结构体时顺便创建结构体变量
struct Student
{
	//成员列表
	//姓名
	string name;
	//年龄
	int age;
	//考试分数
	int store;
}s3;
	s3.name = "王五";
	s3.age = 22;
	s3.store = 98;
	cout << s3.name << s3.age << s3.store;
```

##### 2、结构体数组

作用：将自定义的结构体放入数组方便维护。

struct 结构体名 数组名[元素个数] = { { }， { }, ... { }}；

```c++
1、定义结构体
struct Student
{
	string name;
	int age;
	int store;
};
2、创建结构体数组
int main() {
	struct Student arr[3] = 
	{
		{"张三", 21, 99},
		{"er", 22, 100},
		{"wu",19, 98}
	};
	3、给结构体的元素赋值
	arr[2].name = "HU";
	4、遍历结构体数组
	for(int i = 0; i < 3; i++) {
		cout << arr[i].name 
		<< arr[i].age << arr[i].store;
	}
}
```

##### 3、结构体指针

作用：通过指针访问结构体中的成员

利用操作符->可以通过结构体访问结构体属性

```c++
1、定义结构体
struct Student
{
	string name;
	int age;
	int store;
};
int main(){
	2、创建学生结构体变量
	Student s = {"李四", 21,100};
	3、通过指针指向结构体变量
	Student * p = &s;
	4、通过指针访问结构体变量中的数据
	cout << p->name << p->age << p->store;
}
```

##### 4、结构体嵌套结构体

作用：结构体的成员可以是另一个结构体

例如：每个老师辅导一个学员，一个老师的结构体中，记录一个学生的结构体

```c++
//先定义学生的结构体
struct Student
{
	string name;
	int age;
	int store;
};
struct Teacher
{
	string name;
	int id;
	int age;
	struct Student stu;
};
int main(){
	Teacher t;
    t.id = 054;
    t.name = "老郭";
    t.age = 48;
    t.stu.name = "胡";
    t.stu.age = 18;
    t.stu.store = 98;
}
```

##### 5、结构体做函数参数

传递方式：值传递、地址传递

```c++
//将学生传入到一个参数中，打印学生所有信息
struct student
{
	string name;
	int age;
	int store;
};
//值传递
void printStudent(struct student s) {
	cout << s.name << s.age << s.store;
}
//地址传递
void printStudent(struct student * s) {
	cout << s->name << s->age << s->store;
}
int main() {
	struct student s;
	s.name = "w12";
	s.age = 20;
	s.store = 99;
	printStudent(s); //值传递
	printStudent(&s);//地址传递
}
```

##### 6、结构体中const使用场景

作用：用const防止误操作

```c++
//结构体的使用场景
struct student {
	string name;
	int age;
	int store;
};
//值传递时拷贝了实参的数据，造成大量内存使用
void printStudent(student s) {
	cout << s.name << s.age << s.store;
}
//将函数中的形参改为指针，可以减少内存空间，而且不会拷贝新的副本出来
void printStudent(const student *s) {
    //加入const后对s的内容修改时报错，防止误操作
	cout << s->name << s->age << s->store;
}
int main() {
	student s = {"1", 12, 100};
	//通过函数打印结构体信息
	printStudent(s);
    printStudent(&s);
}
```

### 2、C++核心编程（面向对象）

#### 1、内存分区模型

将内存分为4个区域：

代码区：存放函数体的二进制代码，由操作系统管理

全局区：存放全局变量和静态变量以及常量

栈区：由编译器自动分配释放，存放函数的参数值，局部变量

堆区：由程序员分配，若程序员不释放，程序结束后操作系统回收。

##### 1.1、程序运行前

程序编译后生成可执行.exe文件，未执行程序之前内存分为两个区域：

①代码区：存放CPU的机器指令。

代码区是共享的，频繁执行的程序只存放一份代码

是只读的，防止被意外修改

②全局区：全局变量和静态常量static的存放，还包括常量区

常量区存放const修饰的全部变量、字符串常量。

该区域的数据在程序结束后由操作系统释放。

##### 1.2、程序运行后

栈区：由编译器自动分配释放，存放函数的参数值、局部变量等。

注意：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放



堆区：由程序员分配，若程序员不释放，程序结束后操作系统回收。

在C++中用new在堆区开辟数据

int * p = new int(10)

<img src=".assets\1637675779082.png" alt="1637675779082" style="zoom: 80%;" />

##### 1.3、new操作符

C++中用new在堆区开辟数据

堆区开辟的数据，由程序员管理开辟和释放，delete删除

```c++
1、new的基本语法
int * func() {
	//在堆区创建整形数据
	//new返回 是该数据类型的指针
	int *p = new int(10);
	return p;
}
void test01(){
	int * p = func();
    cout << * p; //输出 10
    delete p;
    cout << *p; //释放内存后，再次访问非法会报错
}
2、在堆区利用new开辟数组
void test02(){
    //在堆区创建含10个整型数据的数组
    int *arr = new int[10]; //10代表数组的10个元素
    for(int i = 0; i < 10; i++) {
        arr[i] = i + 100;
    }
    for(int i = 0; i < 10; i++) {
        cout << arr[i];
    }
    //释放堆区数组,要加上[]
    delete[] arr;
}
```

#### 2、引用

##### 2.1、引用的基本使用

作用：给变量起别名

语法：数据类型 &别名=原名

![1637676646314](.assets\1637676646314.png)

```c++
//引用基本语法
int a = 10;
int &b = a;
b = 20;
cout << a; //a=20
```

##### 2.2、引用注意事项

(1)引用必须初始化

int &b; //错误

(2)引用初始化之后不能改变

```c++
int a = 10;
int c = 20;
int &b = a;
//int &b = c; //错误，初始化之后不能改变
b = c; //赋值操作，而不是更改引用
cour << a << b << c; //此时a=b=c=20;
```

##### 2.3、引用做函数参数

作用：函数传参时，可以利用引用的技术让形参修饰实参

优点：可以简化指针修改实参

```c++
//值传递  形参未修饰实参
void swap01(int a, int b) {
	int temp = a;
	a = b;
	b = temp;
	cout << a << b;  //a=20,b=10形参改变
}
int main(){
	int a = 10;
	int b = 20;
	swap01(a,b);
	cout << a << b;  //a=10,b=20实参未改变
}
//地址传递  形参修饰实参
void swap02(int *a, int *b) {
	int temp = *a;
	*a = *b;
	*b = temp;
	cout << a << b;  //a=20,b=10形参改变
}
int main(){
	int a = 10;
	int b = 20;
	swap02(&a, &b);
	cout << a << b;  //a=20,b=10实参改变
}
//引用传递 形参修饰实参
void swap03(int &a, int &b) {
	int temp = a;
	a = b;
	b = temp;
	cout << a << b;  //a=20,b=10形参改变
}
int main(){
	int a = 10;
	int b = 20;
	swap03(a,b);
	cout << a << b;  //a=20,b=10实改变
}
```

总结：通过引用参数产生的效果同地址传递是一样的，引用的语法更清楚简单。

##### 2.4、引用做函数的返回值

作用：引用是可以做函数的返回值的

```c++
注意：不要返回局部变量引用
用法：函数调用作为左值
int& test01() {
    int a = 10; //局部变量存放在四区中的栈区
    return a;
}
int& test02() {
    static int a = 10; //静态变量存放在四区中的全局区，全局区的数据在程序结束后系统释放。
    return a;
}
int main(){
    int &ref = test01();
    cout << “test01” << ref; //ref=10,正确，编译器做了保留
    cout << “test01” << ref; //输出!=10，错误，a的内存已经释放
    
    int &ref = test02();
    cout << “test02” << ref; //ref=10
    cout << “test02” << ref; //ref=10
    
    test02() = 1000;
    //如果函数的返回值是引用，这个函数调用可以作为左值
    cout << “test02” << ref; //ref=1000
    cout << “test02” << ref; //ref=1000
    return 0;
}
```

##### 2.5、引用的本质

引用的本质在C++内部实现是一个指针常量

```c++
//发现引用，自动转换为int *const ref = &a;
void func(int &ref) {
    ref = 100; //ref是引用，转换为*ref = 100
}
int main(){
	int a = 10;
	int &ref = a;
    //自动转换为int *const ref = &a;
    //指针常量是指针的指向不可以改，也表明为什么引用不可更改
    ref = 20;
    //内部发现ref是引用，自动帮我们转换为*ref = 20
    cout << a;
    cout << ref;
}
```

总结：C++推荐引用技术，语法方便，引用本质是指针常量，所有指针操作编译器实现。

##### 2.6、常量引用

作用：常量引用主要用来修饰形参，防止误操作

```c++
void showvalue01(int &a){
    cout << a;  a = 10;
}
void showvalue02(int &a){
    a = 100;
    cout << a;  a = 100;
}
//为了防止误修改加入const
void showvalue02(const int &a){
    a = 100; //错误，不可被修改
    cout << a; 
}
int main(){
	int a = 10;
    int &ref = 10; //错误，引用必须引一块合法内存空间
    const int &ref = 10; //正确，加上const之后，编译器将代码修改为int temp =10; const int &ref = temp;
    ref = 20;//错误，加入const之后不可修改
    int &ref = a;//一般做法
    
    showvalue01(a);  
    cout << a; //a = 10;
    showvalue02(a);  
    cout << a; //a = 100;//别名改变，原名也改变
}
```

#### 3、函数提高

##### 3.1、函数默认参数

在C++中，函数的形参列表的形参是可以有默认值的

```c++
int func01(int a, int b, int c){
    
}
int func02(int a, int b=20, int c=30){
    
}
int main(){
    cout << func01(10,20,30); //输出是60
    cout << func02(10); //输出是60
    cout << func02(10,50); //输出是90
    //如果自己传入数据用自己的，如果没有用默认值
}
```

注意事项：

1、如果某个位置已经有了默认参数，那么从这个位置向右，从左到右都必须有默认值

2、如果函数声明有默认参数，函数实现就不能有默认参数，声明和实现只能有一个默认参数

##### 3.2、函数占位参数

C++函数的形参列表里可以有占位参数，用来做占位，调用函数时必须填补该位置。

语法：返回值类型 函数名（数据类型）{}

```c++
void func(int a, int){
	cout << 10;
}
//占位参数还可以有默认参数
void func(int a, int = 1){ //h后面占位参数不用传
	cout << 10;
}
int main(){
	func(1);//错误，占位参数没有填补
	func(1,1);//正确
}
```

##### 3.3、函数重载

作用：函数名可以相同，提高复用性

函数重载满足条件：

1、同一个作用域下

2、函数名称相同

3、函数参数 类型不同 或者 个数不同 或者 顺序不同

```c++
void func(){
	cout << 1;
}
void func(){
	cout << 2;
}
//错误，条件12满足，3不满足
    
void func(){
	cout << 1;
}
void func(int a){
	cout << 2;
}
//正确，根据传入的参数不同选择不同的函数

void func(){
	cout << 1;
}
int func(int a){
	cout << 2;
}
//错误，函数的返回值不可以作为函数重载的条件
```

3.4、函数重载注意事项

```c++
1、引用作为函数重载条件
void func(int &a){
    cout << 1;
}
void func(const int &a){
    cout << a;
}
int main(){
    int a = 10;
	func(a);//调用的是上面的函数，输出1
    //const只能读不能写
}
//如何调用下面的函数？
int main(){
	func(10);//调用的是下面的函数，输出10
}
2、函数重载碰到默认参数，出现二义性
void func(int a, int b = 10){
    cout << a;
}
void func(int a){
    cout << a;
}
int main(){
	func(10);//错误
    func(10，20);//正确
}
```

#### 4、类和对象

C++面向对象的三大特性：封装、继承、多态

C++认为万事万物皆为对象，对象有其属性和行为。

具有相同性质的对象，可以抽象为类。人属于人类，车属于车类

##### 4.1封装

###### 4.1.1封装的意义

封装是C++面向对象的三大特性之一

意义：

```c++
//封装意义1:将属性和行为作为一个整体，表现生活中的事物
//实例：设计一个圆类，求圆的周长
class Circle {
    //访问权限 公共权限
public:
    //属性（变量）
    int m_r; //半径
    //行为（函数） 获取圆的周长
    double calculateZC(){
        return 2 * PI * m_r;
    }
};
int main(){
    //通过圆类 创建具体的圆（对象）
    //实例化，通过一个类创建一个对象
    Circle c1;
    //给圆对象的属性赋值
    c1.m_r = 10;
    cout << c1.calculateZC();
}
类中的属性和行为，统一称为成员
属性：成员属性、成员变量
行为：成员函数、成员方法
class Student{
public:
	string name;
    int id;
    void printf(){
        cout << name << id;
    }
};
int main(){
    Student s1;
    s1.name = "sansan";
    s1.id = 12345;
    s1.printf();
}
//封装意义2、将属性和行为加以权限控制
//三种访问权限
公共权限public 成员 类内可以访问 类外可以访问
保护权限protected 成员 类内可以访问 类外不可以访问
私有权限private  成员 类内可以访问 类外不可以访问
保护和私有权限的区别在于继承 
儿子可以访问父亲中的保护内容、儿子不可以访问父亲中的私有内容
class Person{
public:
    string name;
protected:
    string car;
private:
    int password;
public:
    void func(){
        name = "33";
        car = "cc";
        password = 666;
    }
}; 
```

###### 4.1.2struct和calss的区别

struct默认权限是公有 public

class的默认权限是私有private

###### 4.1.3将成员属性设置为私有

优点1：可以控制读写权限

优点2：对于写权限，可以检测数据的有效性

```c++
class Person{
public:
	//写姓名 设置姓名
    void setName(string name) {
        m_name = name; 
    } 
    //读姓名 获取姓名
    string getName() {
        return m_name;
    }
    //检测数据的有效性
    void setAge(int age) {
        if(age < 0 || age > 150) {
            age = 0;
            return;
        }
        m_age = age;
    }
private:
	//姓名 可读可写
	string m_name;
    //年龄 只读
	int m_age;
};
int main(){
	Person p;
	p.setName("22");
    p.getName();
    p.m_name //错误,私有成员不能访问
}
```

##### 4.2对象的初始化和清理

C++面向对象，每个对象会有初始设置以及对象销毁前的清理数据的设置

构造函数：初始化   析构函数：清理

###### 4.2.1构造函数和析构函数

构造函数：创建对象时为对象的成员变量属性赋值，构造函数由编译器自动调用，无需手动调用

析构函数：对象销毁前系统自动调用，执行一些清理工作。

构造函数语法：类名（）{}

1、没有返回值也不写void

2、函数名称与类名相同

3、构造函数可以有参数，因此可以发生重载

4、程序在调用对象时自动调用构造，无需手动调用，而且只会调用一次。

析构函数语法：~类名（）{}

1、没有返回值也不写void

2、函数名称与类名相同，在名称前加上符号~

3、析构函数不可以有参数，因此不能重载

4、程序在对象销毁前会自动调用析构，无需手动调用，而且只会调动一次。

```c++
//对象的初始化和清理
class person{
	//1、构造函数，初始化
public:
	person(){
		cout << 1;
	}
	//2、析构函数，进行清理操作
	~person(){
		cout << 2;
	}
}
void test01(){
	person p;
}
//构造和函数是必须有的实现，如果没有写，编译器会提供一个空实现的构造和析构函数
```

###### 4.2.2构造函数的分类及调用

两种分类方式：

​	按参数分为：有参构造和无参构造（默认构造）

​	按类型分为：普通构造和拷贝构造

三种调用方式：

​	括号法、显示法、隐式转换法

1、括号法注意事项：

调用默认构造函数时，不要加()，person p(); 编译器会认为是一个函数声明,不会认为在创建对象。

```c++
class person {
public:
	//构造函数
	person(){
		cout << 1;
	}
	//拷贝构造
    person(const person &p) {
        age = p.age;
        //将传入的人身上的所有属性，拷贝到我身上
    }
};
//调用
void test01(){
	//括号法
    person p1; //默认构造函数调用
    person p2(10); //有参构造函数调用
    person p3(p2); //拷贝构造函数调用
    //显示法
    person p1;
    person p2 = person(10); //有参构造
    person p3 = person(p2); //拷贝构造
    
    person(10);//匿名对象 特点：当前行执行结束后，系统会立即回收匿名对象
    
    person(p3);//注意事项：不要使用拷贝构造函数初始化匿名对象,会引起重定义
    
    //隐式转换法
    person p2 = 10; //等价于person p2 = person(10);
    person p5 = p4; //拷贝构造
    
}
```

###### 4.2.3拷贝构造函数调用时机

C++拷贝构造函数调用时机的三种情况：

```c++
//1、使用一个已经创建完毕的对象来初始化一个对象

//2、值传递的方式给函数参数传值

//3、以值方式返回局部对象

class person{
public:
    person(){
    	cout << "默认构造";
    }
    person(int age){
        cout << "有参构造";
        m_age = age;
    }
    ~person(){
        cout << "析构";
    }
    person(const person & p){
        cout << "拷贝构造";
        m_age = p.m_age;
    }
    int m_age;
};
void test01(){
    person p1(20);
    //1、使用一个已经创建完毕的对象来初始化一个对象
    person p2(p1);
}

void dowork1(person p) {
    
}
void test02(){
    person p;
    dowork1(p);
    //2、值传递的方式给函数参数传值,值传递实质是形参的拷贝
}

void dowork2() {
    person p1;
    return p1;
}
void test02(){
    person p = dowork2();
    //3、以值方式返回局部对象
}
```

###### 4.2.4构造函数的调用规则

默认情况下C++编译器至少给一个类添加3个函数：

1、默认构造函数（无参、函数体为空）

2、默认析构函数（无参、函数体为空）

3、默认拷贝构造函数，对属性进行拷贝

构造函数调用规则：

1、如果用户定义有参构造函数，C++不再提供默认无参构造，但是会提供默认拷贝构造

2、如果用户定义拷贝构造函数，C++不再提供其他构造函数

###### 4.2.5深拷贝与浅拷贝

浅拷贝：简单的复制拷贝操作

深拷贝：在堆区重新申请空间，进行拷贝操作

```c++
class person{
public:
    person(){
        cout << "默认构造函数";
    }
    person(int age, int height){
   		m_age = age;
    	m_height = new int(height);
        cout << "有参构造函数";
	}
    ~person(){
        if(m_height != NULL){
            delete m_height;
            m_height = NULL;
        }
        cout << "析构函数";
    }
    //浅拷贝带来的问题是堆区的内存两次释放
    //使用深拷贝解决，自己写一个拷贝构造函数
    person(person const &p) {
        m_age = p.m_age;
        m_height = p.m_height;//编译器默认是这行代码
        m_height = new int(*p.m_height);//深拷贝自己写的代码
    }
    int m_age;
    int *m_height;
};
void test01() {
    person p(18,160);
    person p1(p);
}
```

![1638357576271](.assets\1638357576271.png) 

###### 4.2.6初始化列表

作用：C++提供了初始化列表语法，用来初始化属性

```
person():m_a(10),m_b(20),m_c(30){}
person(int a,int b,int c):m_a(a),m_b(b),m_c(c){}
```

###### 4.2.7类对象作为类成员

C++类中的成员可以是另一个类的对象，称该成员为对象成员

```c++
class A{};
class B{
	A a;
};
```

当其他类对象作为本类成员，构造时候先构造类对象，再构造自身，析构的顺序相反。

###### 4.2.8静态成员

静态成员就是在成员变量和成员函数前加上关键字static，称为静态成员

静态成员分为：

1、静态成员变量

所有对象共享同一份数据

在编译阶段分配内存

类内声明，类外初始化

```c++
class person{
public:
	static int m_age;
}
int person::m_age = 21;//注意如初始化静态成员变量，用：：
void test01(){
	person p;	
}
```

静态成员变量不属于某个对象，所有对象都共享同一份数据

因此静态成员变量有两种访问方式：

通过对象进行访问

person p;

p.m_age = 20;

通过类名进行访问

int person::m_age = 21;

2、静态成员函数

所有对象共享同一个函数

静态成员函数只能访问静态成员变量

##### 4.3、C++对象模型和this指针

###### 4.3.1成员变量和成员函数分开存储

在C++中，类的成员变量和成员函数分开存储

只有非静态成员变量才属于类的对象，其他都不属于

```c++
class person{
	int m_A;//非静态成员变量 属于类的对象
    static int m_B;//静态成员变量 不属于类的对象
    void func(){} //非静态成员函数
};
int person :: m_B = 0;
void test01(){
	person p;
	cout << "size of 空p = " << sizeof(p) << endl;
	//空对象占用内存空间为：1
    //C++编译器会给每个空对象也分配一个字节空间，是为了区分空对象占用内存的位置。
    //每个空对象也应该有一个独一无二的内存地址
    cout << "size of (int m_A)p = " << sizeof(p) << endl;
    //输出：4,说明非静态成员变量属于类的对象上
     cout << "size of (int m_A,static )p = " << sizeof(p) << endl;
    //依然输出：4,说明静态成员变量不属于类的对象上
    cout << "size of (int m_A,static，func())p = " << sizeof(p) << endl;
    //依然输出：4,说明非静态成员函数不属于类的对象上
}
int main(){
	test01();
}
```

###### 4.3.2、this指针概念

每个非静态成员函数只会诞生一份函数实例，多个同类型的对象会共用一块代码。代码如何区分哪个对象调用自己？

特点：

C++通过特殊的对象指针this指针

this指针指向被调用成员函数所属的对象

this指针是隐含每一个非静态成员函数内的一种指针，不需要定义，直接使用

用途：

（1）当形参和成员变量同名时，可用this指针区分

（2）在类的非静态成员函数中返回对象本身，可使用return *this.

```c++
class person(){
public:
    person(int age){
        this->age = age;
    }
	int age; //避免冲突
};
void test01(){
    person p1(18);
}
```

4.3.3、空指针访问成员函数

空指针可以调用成员函数，但是也要注意有没有用到this指针，如果用到需要加以判断保证代码的健壮性。

```c++
class person {
public:
	void showClassName(){
		cout << "class name is person"
	}
	void showpersonAge(){
		cout << "age = " << m_Age;
		//默认应该是cout << "age = " << this->m_Age;但是此时this是空的，所以会报错
		//加一句
		if(this == NULL){
			reutrn ;
		}
	}
	int m_Age;
};
vois test01(){
	person *p = NULL;
	p->showClassName();//正确
	p->showpersonAge();//错误
}
```

###### 4.3.4、const修饰成员函数

常函数：

成员函数后加const后我们称这个函数为常函数

常函数内不可以修改成员属性

成员属性声明加关键字mutable后，在函数中依然可以修改

常对象：

声明对象前加const称该对象为常对象

常对象只能调用常函数

```c++
class person{
publuc:
    //this指针的本质是指针常量 指针的指向不可以修改
    //成员函数之后加上const,修饰的是this指针
    void showperson() const
    {
        m_A = 10; //错
        m_B = 10; //正确
    }
    int m_A;
    mutable int m_B;
};
//常对象
void test02(){
    const person p;
    p.m_B = 100;//m_B是特殊值，在常对象下也可以修改
    p.showperson();//常对象只能调用常函数
}
```

##### 4.4、友元

例如客厅是共有，所有人可以进入。卧室私有，只有好朋友可以进去。

在程序里，有些私有属性也想让类外特殊的一些函数或类进行访问，就需要友元

友元的目的是让一个函数或类访问另一个类中的私有成员

关键字 friend

三种实现方式：全局函数做友元、类做友元、成员函数做友元

###### 4.4.1、全局函数做友元

```c++
class building{
    //goodGay全局函数是building类的友元，可以访问卧室。
   friend void goodGay(building *Building);
    
public:
	Building(){
        m_sittingroom = "客厅";
        m_bedroom = "卧室";
    }
	string m_sittingroom;
private:
	string m_bedroom;
};
//全局函数
void goodGay(building *Building) {
    cout << Building->sittingroom;
    cout << Building->m_bedroom;
}
void test01(){
	building Building;
    goodGay(&Building);
}
```

4.4.2、类做友元

```c++
class building;
class goodGay{
public:
    goodGay();
    void visit();
    building *Building;
};
class building{  
    //goodGay类是building类的友元，可以访问卧室。
    friend class goodGay;
public:
	Building();
	string m_sittingroom;
private:
	string m_bedroom;
};
//类外写成员函数
goodGay::goodGay(){
    //创建建筑物对象
    Building = new Building;
}
building:Building(){
	m_sittingroom = "客厅";
	m_bedroom = "卧室";
}
void goodGay::visit(){
    cout << Building->m_sittingroom;
    cout << Building->m_bedroom;
}
void test01(){
	goodGay gg;
    gg.visit();
}
```

4.4.3、成员函数做友元

```c++
class building;
class goodGay{
public:
    goodGay(); //构造函数
    void visit1(); //让visit函数访问building类中私有成员
    void visit2(); //让visit函数不可以访问building类中私有成员 
    
    building *Building; //建筑物类指针
};

class building{  
//告诉编译器 goodGay类下的visit成员函数作为本类的朋友，可以访问私有内容
friend void goodGay::visit1();
public:
	Building();//构造函数
	string m_sittingroom;
private:
	string m_bedroom;
};

//类外写成员函数
goodGay::goodGay(){
    //创建建筑物对象
    Building = new Building;
}
building::Building(){
	m_sittingroom = "客厅";
	m_bedroom = "卧室";
}
void goodGay::visit1(){
    cout << Building->m_sittingroom;
    cout << Building->m_bedroom;
}
void goodGay::visit2(){
    cout << Building->m_sittingroom;
    cout << Building->m_bedroom;
}
void test01(){
	goodGay gg;
    gg.visit1();
    gg.visit2();
}
```

##### 4.5、运算符重载

对已有的运算符重新进行定义，赋予另一种功能，以适应不同的数据类型。比如两个对象相加？

###### 4.5.1、加号运算符重载+

对于内置的数据类型，编译器知道如何运算

```c++
class person{
public:
    //1、成员函数重载+号
    person operator+(person &p){
        person temp;
        temp.m_A = this->m_A + p.m_A;
        temp.m_B = this->m_B + p.m_B;
        return temp;
    }   
	int m_A;
	int m_B;
};
//2、全局函数重载+号
person operator+(person &p1, person &p2) {
    person temp;
    temp.m_A = p1.m_A + p2.m_A;
    temp.m_B = p1.m_B + p2.m_B;
}
//3、函数重载的版本
person operator+(person &p1, int num) {
    person temp;
    temp.m_A = p1.m_A + num;
    temp.m_B = p1.m_B + num;
}
void test01(){
	person p1;
	p1.m_A = 10;
	p1.m_B = 10;
	person p2;
	p2.m_A = 10;
	p2.m_B = 10;
    //成员函数重载调用
    //person p3 = p1.operator+(p2)
    person p3 = p1 + p2;
    //全局函数重载本质调用
    //person p3 = operator+(p1 + p2);
    person p3 = p1 + p2;
    //运算符重载也可以发生函数重载
    person p3 = p1 + 10;
}
```

###### 4.5.2、左移运算符重载<<

作用：可以输出自定义的数据类型

```c++
class person{
public:
    //1、利用成员函数重载
    //这里需要两个对象，无法实现
    void operator<<(person &p){
        person temp;
        ？？？
    }   
	int m_A;
	int m_B;
};
//利用全局函数重载
ostream & operator<<(ostream cout, person &p){
	cout << m_A;
    cout << m_B;
}
void test01(){
	person p1;
	p1.m_A = 10;
	p1.m_B = 10;
	cout << p1.m_A << endl;
    //如何输出p1?
    cout << p1;
}
```

###### 4.5.3、递增运算符重载++

作用：通过重载递增运算符，实现自己的整型数据

前置递增返回引用，后置递增返回值。

```c++
//内置数据类型
int a = 10;
cout << ++a; //11
cout << a;   //11
int b = 10;  
cout << b++; //10
cout << b;  //11

```

```c++
//自定义整型
class myinteger{
friend ostream &operator (ostream& cout, myinteger& myint);
public:
    myinteger(){
        m_num = 0;
    }
    //重载前置++运算符 返回引用是为了一直对一个数据运算
    myinteger& operator++(){
        //先进行++运算
        m_num++;
        //再将自身作为返回
        return *this;
    }
    //重载后置++运算符 int代表占位参数，用于区分前置后置
   myinteger operator++(int){
        //先记录当时结果
        myinteger temp = *this;
        //再进行++运算
        m_num++；
        //最后将记录结果返回
		return *this;
    }
private:
    int m_num;
};
//重载<<运算符
ostream &operator (ostream& cout, myinteger & myint){
    cout << myint.m_num;
    return cout;
}
void test01(){
    myinteger myint;
    cout << ++myint << endl;
}
void test02(){
    myinteger myint;
    cout << myint++ << endl;
}

```

###### 4.5.4、赋值运算符重载=

C++编译器至少给一个类添加四个函数

1、默认构造函数（无参，函数体为空）

2、默认析构函数（无参，函数体为空）

3、默认拷贝构造函数，对属性进行值拷贝

4、赋值运算符operator=，对属性进行值拷贝

如果类中有属性指向堆区，做赋值操作时也会出现深浅拷贝问题

```c++
class person{
public:
	person(int age) {
		m_age = new int(age);
	}
	~person(){
		if(m_age != NULL){
			delete m_age;
			m_age = NULL;
		}
	}
	//重载赋值运算符
	person& operator=(person &p){
		//编译器是提供浅拷贝
		//m_age = p.m_age;
		
		//应该先判断是否有属性在堆区，如果有先释放再深拷贝
		if(m_age != NULL){
			delete m_age;
			m_age = NULL;
		}
		m_age = new int (*p.m_age);
        return *this;
	}
	int *m_age;
};
void test01(){
	person p1(18);
	person p1(20);
	person p1(30);
	p3 = p2 = p1;
}
```

###### 4.5.5、关系运算符重载 == 

作用：可以将两个自定义对象比较

```c++
//成员函数
bool operator==(person &p) {
	if(this->m_name == p.m_name){
		return true;
	}
	return false;
}
```

###### 4.5.6、函数调用运算符重载 （）

由于重载后使用的方式非常像函数的调用，也称为仿函数

仿函数没有固定写法，非常灵活

```c++
class myprint{
public:
	//重载函数调用运算符
    void operator() (string test){
        cout << test;
    }
};
void test01(){
    myprint myprint;
    myprint("hello");
}
```

##### 4.6、继承

面向对象三大特性之一

动物

猫 狗

加菲猫 布偶猫 哈士奇 京巴

下级成员拥有上一级的共性 还有自己的特性

可以使用继承，减少重复代码

###### 4.6.1、继承的基本语法

网站中拥有公共的头部、底部、公共的左侧列表 只有中心内容不同

分别利用普通写法和继承的写法实现网页中的内容，分析继承存在的意义和好处

继承的好处：减少重复代码

继承的语法：class 子类 ： 继承方式 父类

class A : public B 子类也称为派生类 父类也称为基类

派生类中成员包括两部分：一类是从基类继承过来的，一类是自己增加的成员

```c++
//普通实现 
//java界面
class java{
public:
    void header(){
        cout<<"首页、公开课、登录(公共头部)";
    }
    void footer(){
        cout<<"帮助中心、交流合作(公共底部)";
    }
    void left(){
        cout<<"java,pyhton,C++(公共分类列表)";
    }
    void content(){
        cout<< "java学科视频";
    }
};
//python界面
class python{
public:
    void header(){
        cout<<"首页、公开课、登录(公共头部)";
    }
    void footer(){
        cout<<"帮助中心、交流合作(公共底部)";
    }
    void left(){
        cout<<"java,pyhton,C++(公共分类列表)";
    }
    void content(){
        cout<< "python学科视频";
    }
};
void test01(){
    cout<<"java下载视频页面：";
    java ja;
    ja.header();
    ja.footer();
    ja.left();
    ja.content();
    cout<<"python下载视频页面：";
    python py;
    py.header();
    py.footer();
    py.left();
    py.content();
}
```

```c++
//继承实现
class basepage(){
public:
    void header(){
        cout<<"首页、公开课、登录(公共头部)";
    }
    void footer(){
        cout<<"帮助中心、交流合作(公共底部)";
    }
    void left(){
        cout<<"java,pyhton,C++(公共分类列表)";
    }
};
class java : public basepage{
public:
    void content(){
        cout<< "java学科视频";
    }
};
class python : public basepage{
public:
    void content(){
        cout<< "python学科视频";
    }
};
```

###### 4.6.2、继承方式

继承方式三种：公共继承、保护继承、私有继承

父类中的private 三种继承方式子类都不可访问

公共继承：父类中的public到子类中依然公共，protected到子类依然是保护，private子类私有

保护继承：父类的public到子类中变成保护,protected到子类变成保护，private私有不能访问

私有继承：父类的public到子类中变成私有,protected到子类变成私有，private私有不能访问

###### 4.6.3、继承的对象模型

从子类继承过来的成员，哪些属于子类对象中？

```c++
class base{
public:
    int m_A;
protected:
    int m_B;
private:
    int m_C;
};
class son : public base{
public:
    int m_D;
}
void test01(){
    cout << "size of son" << sizeof(son);
    //打印的结果 16.
    //说明父类的所有非静态成员属性都会被继承下去
    //私有的成员属性被编译器隐藏，因此不能访问
}
```

###### 4.6.4、继承中的构造和析构顺序

子类继承父类后，当创建子类对象，也会调用父类的构造函数

问题：父类和子类的构造和析构顺序是谁先谁后？

继承中的构造和析构顺序如下：

先构造父类，再构造子类，析构的顺序与构造的顺序相反

base的构造函数

son的构造函数

son的析构函数

base的析构函数

```c++
class base{
public:
	base(){
		cout<< "base的构造函数";
	}
	~base(){
		cout<< "base的析构函数";
	}
};
class son : public base{
public:
    son(){
		cout<< "";
	}
	~son(){
		cout<< "son的析构函数";
	}
};
void test01(){
    son a;
}
```

###### 4.6.5、继承同名成员处理方式

问题：当子类与父类出现同名的成员，如何通过在子类对象，访问到子类或父类中同名的数据呢？

- 访问子类同名成员 直接访问即可
- 访问父类同名成员 需要加作用域
- 当子类与父类拥有同名的成员函数，子类会隐藏父类中同名成员函数，加作用域可以访问到父类的同名成员函数（隐藏）

```c++
class base{
public:
	base(){
		m_A = 100;
	}
    func(){
        cout << "base函数调用";
    }
	int m_A;
};
class son : public base{
public:
    son(){
        m_A = 200;
    }
    func(){
        cout << "son函数调用";
    }
    int m_A;
};
//同名成员属性处理方式
void test01(){
    son s;
    cout << s.m_A;  //cout 200
    //直接输出是son下的m_A
    cout << s.base::m_A; //cout 100
    //如果通过子类对象 访问父类中的同名成员需要加作用域   
}
//同名成员函数处理方式
void test02(){
    son s;
    s.func();   //son函数调用
    //直接调用的是子类下的成员函数
    s.base::func(); //base函数调用
    //如果通过子类对象 访问父类中的同名成员函数需要加作用域   
}
```

###### 4.6.6、继承同名静态成员处理方式

问题：继承中同名的静态成员变量在子类对象上如何进行访问？

静态成员和非静态成员出现同名，处理方式一致

- 访问子类同名成员 直接访问即可
- 访问父类同名成员 需要加作用域

访问父类的同名成员时，son::base::m_A;第一个:表示通过类名方式访问，第二个:表示父类作用域下 

###### 4.6.7、多继承语法

C++允许一个类继承多个类

语法：class 子类：继承方式 父类1， 继承方式 父类2

多继承可能会引发父类中有同名成员出现，需要加作用域区分

C++实际开发不建议用继承

```c++
class base1{
public:
	base1(){
		m_A = 100;
	}
	int m_A;
};
class base2{
public:
	base2(){
		m_A = 200;
	}
	int m_A;
};
//子类需要继承base1和base2
class son:public base1,public base2{
public:
    son(){
		m_C = 300;
        m_D = 400;
	}
    int m_C;
    int m_D;
};
void test01(){
    son s;
    cout << sizeof(s); //16
    //当父类中出现同名成员，需要加作用域区分
    cout << s.base1::m_A;
}
```

###### 4.6.8、菱形继承

概念：

两个派生类继承同一个基类。

又有某个类同时继承两个派生类

这种称为菱形继承，或者钻石继承

<img src=".assets\1638861425691.png" alt="1638861425691" style="zoom:50%;" />

问题：

```c++
class animal{
public:
    int m_A;
};
//虚继承继承的是指针，只有一份数据
class sheep:virtual public animal{};
class tuo:virtual public animal{};
class sheeptuo:public sheep,public tuo{};
void test01(){
    sheeptuo s;
    s.sheep::m_A = 18;
    s.tuo::m_A = 28;
    //菱形继承，两个父类拥有相同数据，需要加作用域
    //菱形继承导致有两份数据，资源浪费
    //利用虚继承解决 此时都是28
}
```

![1638861964594](.assets\1638861964594.png)

##### 4.7、多态

面对对象三大特性之一

两类：

- 静态多态：函数重载和运算符重载属于静态多态，复用函数名

- 动态多态：派生类和虚函数实现运行时多态

区别：

- 静态多态的函数地址早绑定 编译阶段确定函数地址
- 动态多态的函数地址晚绑定 运行阶段确定函数地址

###### 4.7.1、多态的基本概念

```c++
class Animal{
public:
	void speak(){
		cout << "在说话";
	}
};
class cat:public animal{
public:
	void speak(){
		cout << "猫在说话";
	}
};
//执行说话的函数
//地址早绑定 在编译阶段确定函数地址
//如果想执行让猫说话，需要在运行阶段绑定地址
void dospeak(Animal &animal){
    animal.speak();
}
void test01(){
    cat cat;
    dospeak(cat);
}
//此段程序输出 动物在说话
```

```c++
class Animal{
public:
    //虚函数
	virtual void speak(){
		cout << "在说话";
	}
};
class cat:public animal{
public:
    //重写(覆盖)：函数返回值类型 函数名 参数列表 完全相同
	void speak(){
		cout << "猫在说话";
	}
};
//执行说话的函数
//如果想执行让猫说话，需要在运行阶段绑定地址
//动态多态满足条件：
1、有继承关系
2、子类重写父类的虚函数
//动态多态使用
父类的指针或者引用 指向子类对象
void dospeak(Animal &animal){ //Animal &animal=cat
    animal.speak();
}
void test01(){
    cat cat;
    dospeak(cat);
}
//此段程序输出 猫在说话
```

深层剖析：

```c++
class Animal{
public:
	void speak(){
		cout << "在说话";
	}
};
class cat:public animal{
public:
	void speak(){
		cout << "猫在说话";
	}
};
void dospeak(Animal &animal){ 
    animal.speak();
}
void test01(){
    cat cat;
    dospeak(cat);
}
void test02(){
    cout << sizeof(animal);  //输出1，空类大小是1
    //加上virtual后，输出4
    //4个字节是指针
}
```

重写之前

![1638950816648](.assets\1638950816648.png)

重写之后 子类的虚函数表会替换成子类的虚函数地址

![1638950864605](.assets\1638950864605.png)

###### 4.7.2、多态案例-计算机类

案例描述：分别使用普通写法和多态技术，设计实现两个操作数进行运算的计算器类

多态的优点：

- 代码组织结构清晰
- 可读性强
- 利于前期和后期的扩展以及维护

```c++
//普通写法
class Calculator{
public:
    int getresult(string oper){
        if(oper == "+"){
            return m_num1 + m_num2;
        }
        else if(oper == "-"){
            return m_num1 - m_num2;
        }
        else if(oper == "*"){
            return m_num1 * m_num2;
        }
    }
    int m_num1;
    int m_num2;
};
void test01(){
    Calculator c;
    c.m_num1 = 10;
    c.m_num2 = 10;
    cout << c.getresult("+");
    cout << c.getresult("-");
    cout << c.getresult("*");
}
如果要扩展新的功能，需要修改源码
在真实开发中，提倡 开闭原则
开闭原则：对扩展进行开发 对修改进行关闭
//多态实现
//实现计算器的抽象类
class AbstructCal{
public:
    virtual int getresult(){
        return 0;
    }
    int m_num1;
    int m_num2;
};
//加法计算器类
class AddCalculator:public AbstractCal{
public:
    int getresult(){
        return m_num1 + m_num2;
    }
};
//减法计算器类
class DecCalculator:public AbstractCal{
public:
    int getresult(){
        return m_num1 + m_num2;
    }
};
//乘法计算器类
class MulCalculator:public AbstractCal{
public:
    int getresult(){
        return m_num1 + m_num2;
    }
};
void test02(){
    AbstructCal *abc = new AddCalculator;
    abc->m_num1 = 10;
    abc->m_num2 = 10;
    cout << abs->getresult();
    //用完后销毁
    delete abc；
    //减法运算
    abc = new DecCalculator;
    abc->m_num1 = 10;
    abc->m_num2 = 10;
    cout << abs->getresult();
}
```

总结：C++提倡使用多态

###### 4.7.3、纯虚函数和抽象类

在多态中，通常父类的虚函数的实现是毫无意义的，主要都是调用子类重写的内容，因此可以将虚函数改为纯虚函数

纯虚函数语法：virtual 返回值类型 函数名（参数列表）=0

当类中有了纯虚函数，也称为抽象类

抽象类特点：

- 无法实例化对象

- 子类必须重写抽象类中的纯虚函数，否则也属于抽象类

###### 4.7.4、案例

###### 4.7.5、虚析构和纯虚析构

多态使用时，如果子类有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码

解决方式：将父类中的析构函数改为虚析构或纯虚析构

两者共性：

- 可以解决父类指针释放子类对象
- 都需要有具体的函数实现

虚析构和纯虚析构区别：

如果是纯虚析构，该类属于抽象类，无法实例化对象

```c++
class Animal{
public:
    Animal(){ 
    }
    //利用虚析构可以解决父类指针释放子类对象不干净的问题
    virtual ~Animal(){ 
    }
    //纯虚析构  
    //virtual ~Animal() = 0;
    //代码实现Animal::~Animal(){
	//}
	virtual void speak() = 0;
};
class Cat:public Animal{
public:
    Cat(string name) {
    	m_name = new string(name);
    }
	virtual void speak(){
        cout << "小猫在说话";
    }
    ~Cat(){
        if(m_name != NULL){
            
        }
    }
    string *m_name;
};
void test01(){
    Animal *animal = new Cat;
    animal.speak();
    //父类指针在析构的时候，不会调用子类中的析构函数，导致子类有堆区属性，造成内存泄露
    delete animal;
}
```

总结：

1、虚析构和纯虚析构就是用来解决通过父类指针释放子类对象

2、如果子类中没有堆区数据，可以不写为虚析构或纯虚析构

3、拥有纯虚析构函数的类也属于抽象类

#### 5、文件操作

程序运行时产生的数据都属于临时数据，程序一旦运行结束都会被释放

通过文件可以将数据持久化

C++对文件操作需要包含头文件<fstream>

文件类型分为两种：

1、文本文件：文件以文本的ASCII码形式存储在计算机中

2、二进制文件：文件以文本的二进制形式存储在计算机中，用户一般不能直接读懂他们

操作文件的三大类：

1、ofstream:写操作

2、ifstream：读操作

3、fstream:读写操作

##### 5.1、文本文件

###### 5.1.1、写文件

步骤：

1、包含头文件 #include<fstream>

2、创建流对象 ofstream ofs;

3、打开文件 ofs.open("文件路径"，打开方式);

4、写数据 ofs << "写入的数据"；

5、关闭文件 ofs.close();

![1639017187032](.assets\1639017187032.png)

###### 5.1.2、读文件

步骤：

1、包含头文件 #include<fstream>

2、创建流对象 ofstream ifs;

3、打开文件 ifs.open("文件路径"，打开方式);并且判断是否打开成功

if( !ifs.is_open() ) {cout << "文件打开失败" ，return ；}

4、读数据 4种方式读取

```c++
//第一种
char buf[1024] = {0};
while(ifs >> buf){
	cout << buf;
}
//第二种
char buf[1024] = {0};
while(ifs.getline(buf,sizeof(buf))){
	cout << buf;
}
//第三种
string buf;
while(getline(ifs.buf)){
	cout << buf;
}
//第三种
char c;
while((c = ifs.get()) !=EOF){
	cout << c;
}
```

5、关闭文件 ifs.close();

##### 5.2二进制文件

以二进制的方式对文件进行读写

打开方式要指定为 ios:binary

###### 5.2.1、写文件

主要利用流对象调用成员函数write

函数原型：ostream& write(const char * buffer, int len);

```c++
1、包含头文件
#include<fstream>
class person{
public:
	char m_name[64];
	int m_age;
};
void test01(){
    2、创建输出流对象
	ofstream ofs("person.txt",ios::out |ios:binary);
    3、打开文件
    //ofstream ofs;
   	//ofs.open("person.txt",ios::out |ios:binary);
    person p = {"zz",18};
    4、写文件
    ofs.write((const char *) &p, sizeof(person));
    5、关闭文件
    ofs.close();
}
```

###### 5.2.2、读文件

函数原型：istream& read(char * buffer, int len);

```c++
1、包含头文件
#include<fstream>
class person{
public:
	char m_name[64];
	int m_age;
};
void test01(){
    2、创建输出流对象
	ifstream ofs;
    3、打开文件,判断文件是否打开成功
   	ifs.open("person.txt",ios::in |ios:binary);
    if(!(ifs.is_open())){
        cout << "打开失败";
    }
    person p;
    4、写文件
    ifs.read((char *) &p, sizeof(person));
    5、关闭文件
    ifs.close();
}
```

### 3、C++提高编程

对C++泛型编程和STL技术做详细讲解

#### 1、模板

##### 1.1、模板的概念

模板就是建立通用的模具，提高复用性

模板不可以直接使用，他只是一个框架

模板的通用并不是万能的

##### 1.2、函数模板

C++另一种编程思想称为泛型编程，主要利用的技术就是模板

C++提供两种模板机制：函数模板和类模板

###### 1.2.1、函数模板语法

函数模板作用：建立一个通用函数，其函数返回值和形参类型可以不具体制定，用一个虚拟的类型来代表

语法：

```c++
template<typename T>
函数声明或定义
```

```c
//两个整型交换函数
void swapInt(int &a, int &b){
    int temp = a;
    a = b;
    b = temp;
}
//两个浮点型交换函数
void swapDouble(double &a, double &b){
    double temp = a;
    a = b;
    b = temp;
}
//函数模板
template<typename T>
//声明一个模板，告诉编译器后面代码中紧跟着的T不要报错，T是一个通用数据类型
void swap (T &a, T&b){
    T temp = a;
    a = b;
    b = temp;
}
void test01(){
	int a = 10;
    int b = 20;
  	//两种方式使用函数模板
    //1、自动类型推导
    swap(a, b);
    //2、显示指定类型
    swap<int>(a, b); 
}
```

函数模板关键字template

使用函数模板有两种方式：自动类型推导，显示指定类型

模板的目的是为了提高复用性，将类型参数化

###### 1.2.2、函数模板注意事项

- 自动类型推导，必须推导出一致的数据类型T，才可以使用
- 模板必须确定出T的数据类型，才可以使用

```
template<class T>和<typename T>一样的
```

###### 1.2.3、函数模板案例

- 利用函数模板封装一个排序的函数，可以对不同数据类型数组进行排序
- 排序规则从大到小，排序算法为选择排序
- 分别用char和int数组测试

```c++
//交换算法
template<class T>
void swap (T &a, T&b){
    T temp = a;
    a = b;
    b = temp;
}
//排序算法
template<class T>
void mysort(T arr[], int len){
    for(int i = 0; i < len; i++) {
        int max = i;
        for(int j = i+1; j < len; j++) {
            if(arr[max] < arr[j]) {
                max = j;
            }
        }
        //每次都和最大的交换
        if(max != i) {
            swap(arr[max],arr[i]);
        }
    }
}
//提供打印数组模板
template<class T>
void print(T arr[], int len){
    for(int i = 0; i < len; i++) {
        cout << arr[i] << ' ';
    }
    cout << endl;
}
void test01(){
    char arr[] = "ajodewe";
    int num = sizeof(arr)/sizeof(char);
    mysort(arr,num);
}
```

###### 1.2.4、普通函数与函数模板的区别

普通函数和函数模板的区别：

- 普通函数调用时可以发生自动类型转换（隐式类型转换：默认转换）

- 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换

- 如果利用显示指定类型的方式，可以发生隐式类型转换

  建议使用显示指定类型

###### 1.2.5、普通函数与函数模板的调用规则

调用规则如下：

1、如果函数模板和普通函数都可以实现，优先调用普通函数

2、可以通过空模板参数列表来强调用函数模板

```
myprint<>(a,b);
```

3、函数模板也可以发生重载

4、如果函数模板可以产生更好的匹配，优先调用函数模板

###### 1.2.6、模板的局限性

模板的通用性并不是万能的

```c
template<class T>
void f(T a, T b){
	if(a > b){...}
}
```

如果传入的是类的对象，则不能比较

C++为了解决这种问题，提供模板的重载，可以为特定的类型提供特定的处理方式

```c++
class person{
public:
    person(string name, int age) {
    	m_name = name;
        m_age = age;
    }
    string m_name;
    int m_age;
};
template<class T>
bool mycompare(T &a, T &b){
	if(a == b) return true;
    else return false;
}
//利用具体化的person版本实现代码，具体化优先调用
template<>bool mycompare(person &a, person &b){
	if(p1.m_name == p2.m_name && p1.m_age == p2.m_age) return true;
    else return false;
}
void test01(){
    person p1("tt",10);
    person p2("tt",10);    
    bool mycompare(p1,p2);
}
```

利用具体化的模板，可以解决自定义类型的通用化

学习模板并不是为了写模板，而是能在STL中运用系统提供的模板

##### 1.3、类模板

###### 1.3.1、类模板语法

类模板的作用：建立一个通用类，类中的成员 数据类型可以不具体指定，用一个虚拟的类型来代表

template:声明创建模板

typename:表明后面的符号是一种数据类型，可以用class代替

T ：通用的数据类型，名称可以替换，通常为大写字母

```c++
tempalte<class Nametype, class Agetype>
class person{
public:
    person(Nametype name, Agetype age) {
    	this->m_name = name;
        this->m_age = age;
    }
    Nametype m_name;
    Agetype age;;
};
void test01(){
    person<string, int> p1("ee",16);
}
```

###### 1.3.2、类模板与函数模板区别

1、类模板没有自动类型推导的使用方式

2、类模板在模板参数列表中可以有默认参数

```c
tempalte<class Nametype, class Agetype = int>
```

###### 1.3.3、类模板中成员函数创建时机

类模板中成员函数在调用时创建

普通类的成员函数一开始就创建

###### 1.3.4、类模板对象做函数参数

类模板实例化的对象，像函数传参的方式

一共有三种：

1、指定传入的类型：直接显示对象的数据类型

2、参数模板化：将对象的参数变为模板进行传递

3、整个类模板化：将这个对象类型 模板化进行传递

```c++
template<class T1, class T2>
class person{
public:
	person(T1 name, T2 age) {
		this->m_name = name;
		this->m_age = age;
	}
	void showperson(){
		cout << this->m_name << this->age;
	}
	T1 m_name;
	T2 m_age;
};
//1、指定传入的类型（最常用的）
void print(person<string,int> &p) {
	p.showperson();
}
void test01(){
	person<string,int>p("aa",10);
}
//2、参数模板化
template<class T1, class T2>
void print(person<T1,T2> &p) {
	p.showperson();
}
void test01(){
	person<string,int>p("aa",10);
}
//3、类模板化
template<class T1, class T2>
void print(T &p) {
	p.showperson();
}
void test01(){
	person<string,int>p("aa",10);
}
```

###### 1.3.5、类模板与继承

```c++
template <class T>
class Base{
public:
	T m;
};
class Son:public Base{ //错误，必须知道父类中T类型，才能继承给子类
	
};
//正确方式
class Son:public Base<int>{ 
	
};
//如果想灵活指定父类中T类型，子类也需要类模板
//T2是父类， 子类是T1
tenplate<class T1, class T2>
class Son:public Base<T2>{ 
	T1 ob1;
};
```

###### 1.3.6、类模板成员函数类外实现

```c++
tenplate<class T1, class T2>
class person{ 
public:
    person(T1 name, T2 age); 
    //    this->m_name = name;
    //     this->m_age = age;
    //}
    void showperson();
    //    cout << this->m_name;
    //    cout << this->age;
    //}
    int m_age;
    int m_name;
};
//构造函数类外实现
template<class T1, class T2>
person<T1,T2>::person(T1 name, T2 age){
    this->m_name = name;
    this->m_age = age;
} 
//成员函数类外实现
template<class T1, class T2>
void person<T1,T2>::showperson(){
    cout << this->m_name;
    cout << this->age;
} 
```

###### 1.3.7、类模板分文件编写

类声明 放进.h 类内函数实现放进.cpp

这样会出错 解决办法：将两个文件合并，后缀改为.hpp

###### 1.3.8、类模板与友元

类模板配合友元函数的类内和类外实现

全局函数类内实现：直接在类内声明友元

全局函数类外实现：需要提前让编译器知道全局函数的存在

```c++
tempalte<class T1, class T2>
class person{
    //全局函数类内实现
    friend void printperson(person<T1,T2>p){
        cout << p.m_name << p.m_age;
    }
public:
    person(T1 name, T2 age){
        this->m_name = name;
        this->m_age = age;
    }
private:
	T1 m_name;
	T2 m_age;
};
 //全局函数类外实现，需要先让编译器知道全局函数的存在
```

#### 2、STL初识

##### 2.1、STL诞生

1、重复利用的思想

2、C++的面向对象和泛型编程思想，目的就是复用性的提升

3、数据结构和算法的一套标准

##### 2.2、STL基本概念

- STL：标准模板库

- 广义上分为：容器、算法、迭代器

- 容器和算法之间通过迭代器进行无缝连接

- STL几乎所有代码都采用了模板类或者模板函数

##### 2.3、STL六大组件

容器、算法、迭代器、仿函数、适配器（配接器）、空间配置器

1、容器：各种数据结构：vector list deque set map等存放数据

2、算法：各种常用算法：sort find copy 

3、迭代器：容器与算法之间的胶合剂

4、仿函数：行为类似函数，可作为算法的某种策略

5、适配器：一种修饰容器或者仿函数或迭代器接口的东西

6、空间配置器：负责空间的配置与管理

##### 2.4、STL中容器 算法 迭代器

1、容器:常用的数据结构 数组 链表、树、栈、队列、集合、映射表

分为序列式容器和关联式容器

序列式强调值的排序，序列式容器每个元素均有固定的位置

关联式容器：二叉树结构，各元素之间没有严格的物理顺序关系

2、算法：有限的步骤解决数学或逻辑问题

质变算法：运算过程中会更改区间内的元素内容，拷贝、替换、删除

非质变算法：查找、计数、遍历

3、迭代器

算法要用迭代器访问容器的数

提供一种方法，使之能够依序寻访某个容器所含的各个元素，而又无需暴露该容器的内部实现方式

每个容器都有自己专属的迭代器

类似于指针

常用 双向迭代器和随机访问迭代器

##### 2.5、容器算法迭代器初识

###### 2.5.1、vector

```c++
vector<int>::itertor itbegin = v.begin(); //起始迭代器，指向容器第一个元素
vector<int>::itertor itend = v.end(); //起始迭代器，指向容器最后一个元素的下一个元素
//第一种遍历
while(itbegin != itend) {
	cout << *itbegin;
	itbegin ++;
}
//第二种遍历
for(vector<int>::iterator it = itbegin; it != v.end();it ++) {
	cout << *it;
}
//第三种遍历
void myprint(int val){
    cout << val;
}
for_each(v.begin(),v.end(),myprint);
```

###### 2.5.2、vector存放自定义数据类型

```c++
class person{
public:
	person(string name, int age) {
		this->m_name = name;
		this->age = age;
	}
	string m_name;
	int m_age;
};
//vector存放自定义数据类型
void test01(){
	vector<person> v;
	person p1("aa",10);
	person p2("bb",10);
	v.push_back(p1);
	v.push_back(p2);
	for(vector<person>::iterator it = v.begin(); it != v.end();it ++) {
	cout << (*it).m_name;
	}
}
//vector存放自定义数据类型的指针
void test02(){
	vector<person*> v;
	person p1("aa",10);
	person p2("bb",10);
	v.push_back(&p1);
	v.push_back(&p2);
	for(vector<person*>::iterator it = v.begin(); it != v.end();it ++) {
	cout << (*it)->m_name;
	}
}
```

###### 2.5.3、vector容器嵌套容器

```
vector<vector<int>> v;
vector<int> v1;
```

##### 3.1 string容器

本质：string是C++的字符串，本质上是一个类

string 和 char*区别：

- char* 是一个指针
- string是一个类，类内部封装了char *,管理这个字符串，是一个char ※型的容器

特点：

string管理char*的内存分配，不用担心复制越界和取值越界

##### 3.2、vector容器

可以动态扩展，并不是在原空间之后接续新空间，而是找更大的内存空间，然后将元数据拷贝新空间，释放原空间。

vector的迭代器可以实现随机访问

###### 3.2.3、给vector容器赋值

```
v2 = v1;
v3.assign(v1.begin(),v1.end());
v4.assign(10,100);
```

###### 3.2.4、vector的容器和大小

```
empty();  
capacity();容器容量
size();
resize(int num); 重新指定size
resize(int num, elem); 重新指定size,容器变长则以elem值填充。
```

###### 3.2.5、插入和删除

```
pop_back();删除最后一个元素
insert(pos, elem);向位置pos插入elem
insert(pos,count,elem);向位置pos插入count个elem
erase(pos);
erase(start,end);
clear();//清空
```

###### 3.2.6、vector数据存取

```
at(int idx); 返回idx指向的数据
front();第一个
back();最后一个
```

###### 3.2.8、vector预留空间

减少动态扩展次数

reserve(int len); 容器预留len个元素长度，预留位置不初始化，元素不可访问。

##### 3.3、deque容器

双端数组

```
pop_front push_front pop_bcak push_back
```

![1639637000115](.assets\1639637000115.png)

###### 3.5、stack容器

###### 3.6、queue容器

###### 3.7、list容器

remove(elem); 删除容器内elem的元素

###### 3.8、set/multiset容器

所有元素在插入时被自动排序

属于关联式容器，底层用二叉树实现

s.insert(); set元素不重复multiset可以重复

set查找和统计

```
find(key);查找key是否存在，返回该键的元素的迭代器；若不存在，返回set.end();
count(key);统计key的元素个数  0或1
```

pair对组

```
pair<string,int>p("tt",20);
pair<string,int>p2=make_pair("aa",20);
p.first(); p.second();
```

3.9、map/multimap容器

map中所有元素都是pair   key-value

所有元素根据元素的键值自动排序

属于关联式容器，底层结构是二叉树实现

快速根据key找到value

