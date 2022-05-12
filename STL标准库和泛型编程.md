#### C++标准库--体系结构内核分析

泛型编程GP(Generic Programming)：使用模板为主要工具编写程序

根据源代码分析C++ STL的体系结构，而STL就是泛型编程GP最成功的作品。

本课程：以STL为目标深层次的分析泛型编程

level 0: 使用C++标准库

level 1: 认识C++标准库

level 2: 良好使用C++标准库

level 3: 扩充C++标准库

C++标准库

STL，标准模板库： 六大部件都属于标准库

命名空间std的含义：为了解决编程时命名重复的问题，在同一个命名空间中，变量、函数、类等不能重复。

```
//定义命名空间
namespace n1{ int a;}
namespace n2{ int b;}
//使用
n1::a = 8;
n2::a = 8;
//std是标准库函数使用的命名空间
//cin cout 以及一些常用的顺序容器vector array string forward_list
```

##### 第一讲：Test-STL.cpp

###### 1、认识headers、版本、重要资源

标准库是以 header files形式呈现

C++标准库的头文件 不带.h  #include<vector>

新式 C头文件 不带.h #include<cstudio>

###### 2、STL体系结构基础介绍

STL六大部件：容器、算法、迭代器、适配器、分配器、仿函数 

适配器：容器适配器、迭代器适配器、仿函数适配器

<img src=".assets\1641383600246.png" alt="1641383600246" style="zoom:67%;" />

```c++
#include<vector>
#include<algorithm>
#include<functional>
#include<iostream>
using namespace std;
int main() {
    int ia[6] = {27, 210, 12, 33, 45};
    vector<int, allocator<int>> vi(ia, ia+6);
    cout << count_if(vi.begin(), vi.end(), 
                     not1(bind2nd(less<int>(), 40)));
    return 0;
}
```

复杂度：O(1) O(N) O(log2N)

"前闭后开"区间：begin指向容器的首元素，end指向最后一个元素的下一个元素。

![1641384706065](.assets\1641384706065.png)

```c++
//遍历容器
Container<T> c;
Container<T>::iterator ite = c.begin();
for(; ite != c.end(); ++ite) {...}
```

(C++11):range-based for

```c++
for(int i : {2,3,4,6,7,8,32,43}) {
    std::cout << i << std::endl;
}
vector<double> vec;
for(auto elem : vec) {
    cout << elem;
}
for(auto& elem : vec) {
    elem *= 3;
}
```

auto: 自动推导

```
list<string> c;
auto ite = ::find(c.begin(), c.end());
```

###### 3-6、容器之分类与各种测试（一-四）

分类：顺序容器和关联式容器   C++11  无序容器

顺序容器：array vector deque list forward_list

其中C++11新增：array forward_list

关联式容器：标准库中并没有规定set和map用什么实现，但是红黑树是一种很好的底层实现方式，因此编译器都采用红黑树来实现set和map set/multiset 底部实现 ：红黑树（高度平衡二叉树）key

map/multimap底部实现 ：红黑树（高度平衡二叉树）key-value

multi中key可以重复

unordered_set/multiset：hash table哈希表 seperate chaining 分离链接法

unordered_map/multimap：hash table哈希表 seperate chaining

每一个链表也不能太长，避免查询效率太低

<img src=".assets\1641386388016.png" alt="1641386388016" style="zoom:67%;" />

![1641386317276](.assets\1641386317276.png)

```c++
array<long, size> c; //必须指定尺寸
```

为了避免多个测试中变量命名重复，可以将测试程序的每一块都放进一个namespace。

vector   size表示大小 capacity表示容量

forward_list单向链表：只有push_front     list每次只扩充一个节点

slist 非标准库

deque当空间不够时，将会扩充分配一个buffer

<img src=".assets\1641389386556.png" alt="1641389386556" style="zoom:50%;" />

![1641390291317](.assets\1641390291317.png)

![1641390299516](.assets\1641390299516.png)

deque（双端队列）里含有stack栈和queue队列，因此stack和queue被认为是容器适配器

sort find 标准库有函数，某些容器本身也有这些函数

容器本身的函数更加快速

以下为非顺序式容器，没有iterator

multiset 

multimap

###### 7、分配器之测试

容器的背后需要一个分配器来支持他对内存的使用

一般有默认的分配器

##### 第二讲

###### 8、源代码之分布

###### 9、OOP（面向对象编程）vs GP(泛型编程)

OOP将data和method关联，数据和操作都放在类里面

GP将data和method分开，数据放在类里（ 容器），操作放在全局函数（算法），两者之间用迭代器链接。

::sort( c.begin(), c.end() );

GP优点：容器和算法分开来，用iterator连通。算法通过迭代器确定操作范围，并通过迭代器取出容器里的元素。

sort内部使用随机访问迭代器，list不是随机访问的，不支持

所有算法，内部设计元素本身的操作，基本就是比大小。

###### 10、操作符重载和模板

模板函数的抽象意义:实现代码的重用。

STL标准模板库通过traits技术实现软件的复用。

```c++
class IntArray
{
private:
    int a[10];
public:
    IntArray()
    {
        for(int i=0;i<10;i++)
        {
            a[i]=i+1;
        }
    }
    //该函数输出所有元素的和与times的乘积
    int Calculate(int times)
    {
        int sum=0;
        for(int i=0;i<10;i++)
        {
            sum+=a[i];
        }
        return sum * times;
    }
};

class FloatArray
{
private:
    float a[10];
public:
    FloatArray()
    {
        for(int i=0;i<10;i++)
        {
            a[i]=i+1;
        }
    }
    //该函数输出所有元素的和与times的乘积
    float Calculate(float times)
    {
        int sum=0;
        for(int i=0;i<10;i++)
        {
            sum+=a[i];
        }
        return sum * times;
    }
};

//使用traits技术
//1、定义基本模板类
template<class T>
class NumTraits
{
};

//2、模板特化
template<>
class NumTraits<IntArray>
{
public:
    typedef int inputType;
    typedef int resultType;
};

//模板特化
template<>
class NumTraits<FloatArray>
{
public:
    typedef float inputType;
    typedef float resultType;
};

//3、统一模板调用类编写
template<class T>
class Array
{
public:
    typedef typename NumTraits<T>::resultType result;
    typedef typename NumTraits<T>::inputType input;

    result Calculate(T &t,input times)
    {
        return t.Calculate(times);
    }
};

int main()
{
  IntArray a;
  FloatArray f;
  Array<IntArray> A1;
  Array<FloatArray> A2;
  cout<<"整形数组和的 2 倍是："<<A1.Calculate(a,2)<<endl;
  cout<<"浮点型数组和的 2.2倍是: "<<A2.Calculate(f,2.1)<<endl;
  return 0;
}
```

操作符重载与模板

```c++
#include <iostream>
#include <string.h>
using namespace std;

class Student
{
private:
    char name[20];
    int grade;
public:
    Student(char name[],int grade)
    {
        strcpy(this->name,name);
        this->grade=grade;
    }
    bool operator>(const int &value)const
    {
        return this->grade>value;
    }
};

template <class U,class V>
bool Grater(U const &u,V const &v)
{
    return u>v;
}

int main()
{
    Student s("Raito",100);
    cout<<Grater(s,99)<<endl;
    return 0;
}
```

模板特化、模板泛化

```c++
//泛化
template <class Key> struct hash {  };
//特化 已经把Key绑定为char,因而template<>
template<> struct hash<char>{ };
```

```c++
//泛化
template<class T, class Alloc = alloc>
class vector {  };
//偏特化（个数偏），两个模板参数绑定了其中一个
template<class Alloc>
class vector<bool, Alloc> { };
```

```c++
//泛化
template <class Iterator>
struct iterator_traits { ... };
//偏特化（范围偏），将type特化为指针
template <class T>
struct iterator_traits<T*> {...};
```

###### 11、分配器allocators

VC版本：allocator

分配内存时allocate会调用operator new() ，而operator new()会调用 malloc()

回收内存时deallocate会调用operator delete() 

malloc()分配的内存 除了 需要的内存，还有一些额外的开销overload

<img src=".assets\1641563896925.png" alt="1641563896925" style="zoom:50%;" />

本身需要的内存小，产生的额外开销就大；本身需要的内存大，产生的额外开销就小一点。  

```c++
//分配512ints,allocator<int>()是分配的临时对象
int *p = allocator<int>().allocate(512, (int*)());
allocate<int>().deallocate(p, 512);
```

GCC2.9版本alloc：

和VC一样，但是其容器并不使用GCC里面的分配器，其优点是减少了层层传递产生的额外开销。

<img src=".assets\1641564058111.png" alt="1641564058111" style="zoom:50%;" />

GCC内部类似与如上图的内存条，所有容器都将从此处获得内存。将容器所需要的内存转为8的倍数（例如需要62，则转为64）64在上图中对应#7（从0开始）的链表块。如果该链表块满足需要的内存就直接分配，如果不满足则会调用malloc()向操作系统请求内存，再分为块，分好的块之间用链表连接，再用块进行分配。malloc()过程会产生overload,相比VC这个过程并不会产生大量额外的开销。

GCC4.9版本又改用了allocator，回到最初的allocator。

但是标准库中 _pool_alloc就是2.9版本的alloc

```c++
vector<string, _gnu_exx::_pool_alloc<string>>vec;
```

###### 12、容器之间的实现关系与分类

  图中以缩进表达复合composition关系(并不是继承)

set里面有一个红黑树

<img src=".assets\1641565407930.png" alt="1641565407930" style="zoom:80%;" />

![1641565429108](.assets\1641565429108.png)

###### 13、深度探索list（上）

1、list数据结构 2、迭代器的定义 3、list操作方法的实现

<img src=".assets\1641995838414.png" alt="1641995838414" style="zoom:80%;" />

```c++
//list源码 GCC2.9版本
//容器：typedef + opreator运算符重载
//链表list的iterator设计成class
//所有容器除了vector和array都要将iterator写成class形式
template<class T, class Alloc = alloc>
class list { //list类
protected:
    typedef _list_node<T> list_node;
public:
    typedef list_node* link_type;
    typedef _list_iterator<T,T&,T*> iterator;
protected:
    link_type node;
};

template<class T>
struct _list_node { //节点类型
    typedef void* void_pointer;
    void_pointer prev;
    void_pointer next;
    T data;
}; 

template<class T, class Ref, class Ptr>
struct _list_iterator {
    //定义类型
    typedef T value_type;
    typedef Ptr pointer;
    typedef Ref reference;
    //重载运算符
    reference operator*() const { return (*node).data; }
    pointer operator->() const { return &(operator*()); }
    self& operator++()  //前置++
    {
        node = (link_type)((*node).next);
        return *this;
    }
    self operator++(int)//后置++ 
        //(i++)++;是错误的，不能返回引用
    {
        self tmp = *this;  //记起原值
        //其实是唤起拷贝构造函数，用以创建临时变量tmp，并以*this为初值
        //不会唤起operator*，因为*this已经被解释为构造函数的参数
//唤起_list_iterator(const iterator& x) : node(x.node) {}
//而不是调用 reference operator*() const { return (*node).data; }
        ++*this; //执行操作 
        return tmp; //返回原值
    }
};
```

###### 14、下

```c++
//list 4.9
template<typename _Tp, typename _Alloc = std::allocator<_Tp>>
class list :: protected _List_base<_Tp, _Alloc> {
public:
    public:typedef _List_iterator<_Tp> iterator;
};

template<typename T>
struct _List_node_base { //list节点的基础实现
    _List_node_base* _M_prev; //定义前驱节点
    _List_node_base* _M_next; //定义后继节点
};
template<typename _Tp> //类模板
struct _List_node : public _List_node_base{
    _Tp _M_data; //节点存储的数据
};
//首先定义了List的基类，实现前后指针的定义，然后继承_List_node类，定义存储的节点的value。
template<typename _Tp>
struct _List_iterator {
    typedef _Tp* pointer;
    typedef _Tp& reference;
};
```

###### 15、迭代器的设计原则和Traits的作用和设计

- typedef 为一种数据类型起一个别名

1、在结构体或类中使用typedef嵌套定义数据类型

2、在结构体或类中定义的类型可以通过结构体名：：类型名 的方式使用

- struct定义空结构体的作用：

作为函数参数类型，在函数重载的时候进行区分。

```c++
struct type1{};//定义两个空结构体类型，里面什么也没有
struct type2{};
void showType(type1)//重载函数1：参数类型为结构类型type1
{
    cout<<"show type1"<<endl;
}
void showType(type2)//重载函数2：参数类型为结构类型type2
{
    cout<<"show type2"<<endl;
}
int main()
{
    type1 Type1;//定义一个type1类型对象Type1，虽然是空的，但是是Type1是对象，不是类型
    type2 Type2;//type1()为一个对象，相当于条用了自己的默认构造函数，生成一个对象
    showType(Type1);//或showType(type1());  因为传入的实参类型为type1，所以调用重载函数1
    showType(Type2);//实参类型为type2，所以调用重载函数2
    return 0;
}
输出结果如下：
show type1
show type2
```

- Traits 一种人为设计的萃取机

针对iterator设计的iterator traits（萃取iterator特征）

迭代器是容器和算法的桥梁，容器让iterator确认要处理的范围

迭代器必须定义以下5种typedef以便回答算法的提问

迭代器的相关5种类型：

1 category分类 

2 difference_type距离 

 3 value_type值 (前三种出现在标准库中)  

4 reference  

5 pointer （未在标准库中被使用）

traits技术可以获得一个类型的相关信息。

```c++
//假如有一个泛型的迭代器类
template <typename T>
class myIterator
{
	...
};
//使用myIterator时，怎么才能知道它指向的元素类型？加入一个内嵌类型
template <typename T>
class myIterator
{
	typedef T value_type;
	...
};
//当使用myIterator类时，可以通过myIterator::value_type获得对应的myIterator类型。
//设计一个算法使用以上信息
template <typename T>
typename myIterator<T>::value_type Foo(myIterator<T> i)
{
	...
}
//定义了上面的函数模板之后，我们就可以以如下方式来使用上面的函数：
int a=Foo(iter1);//iter1类型为myIterator<int> 类型
double b=Foo(iter2);//iter2类型为myIterator<double> 类型
//这里返回值和参数类型是一致的，为什么要用typedef?也可以直接用模板实现
//这是因为上面的函数的参数类型只是myIterator，换成别的迭代器将不再适用
//如何解决？修改Foo函数使之适合于所有迭代器类型
template <typename I>
typename I::value_type Foo(I i)
{
	...
}
//这里数据类型T可以通过myIterator提取，但是换成iterator迭代器将无法提取数据类型T，因此通过typedef定义内嵌数据类型，以便可以提取出来。

```

```c++
//iterator必须提供的5种 关联类型
template<class T, class Ref, class Ptr>
struct _list_iterator
{
    typedef bidirectional_iterator_tag iterator_category;
    typedef T   value_type;
    typedef Ptr pointer;
    typedef Ref reference;
    typedef ptrdiff_t difference_type;
};
```

```c++
template<typename I>
inline void algorithm(I first, I last)
{
    I::iterator_category;
    I::value_type;
    I::pointer;
    I::reference;
    I::difference_type;
};
```

![1641824184154](.assets\1641824184154.png)

以上并不需要用到traits

如果iterator并不是class，而是native pointer,这时候就需要traits，加一个中间层。 

traits萃取机必须有能力区分它所获得的iterator是class还是native pointer,利用partial specialization可达到目标。

![1649643888887](.assets\1649643888887.png)

```c++
template<class I>   //I是class iterator
struct iterator_traits {
    typedef typename I::value_type value_type;
};
//两个 偏特化
template<class T>  //I是pointer to T
struct iterator_traits<T*> {
    typedef T value_type;
};
template<class T> //I是pointer to const T
struct iterator_traits<const T*> {
    typedef T value_type;//这里不加const，加了没用
    //声明一个无法被赋值的变量没有用
};
//当需要知道I的value_type时，可以这么写：
template<typename I, ...>
void algorithm(...) {
    typename iterator_traits<I>::value_type v1;
}
//不能直接问，将iterator放在traits里面，问traits
```

总结：traits技术，提取不同类的特性以便于做统一处理。将代码中因类型不同而发生变化的片段拖出来，用统一的接口包装。这个接口可以包括C++类所用的任何东西：内嵌类型、成员函数、成员变量。作为客户的模板代码，可以通过traits模板类所公开的接口来间接访问。

###### 16、vector深度探索

动态数组  vector容量双倍增长

容器：前闭后开区间

内存分配 销毁 再分配

<img src=".assets\1641992949754.png" alt="1641992949754" style="zoom: 50%;" />

```c++
template <class T, class Alloc = alloc>
    //默认分配器alloc
class vector {
public:
    typedef T  value_type;
    typedef value_type* iterator;
    typedef value_type& reference;
    typedef size_t size_type;
protected:
    iterator start;
    iterator finish;
    iterator end_of_storeage;
public:
    iterator begin()  { return start; }
    iterator end()    { return end;   }
    size_type size() const 
    {
       return size_type(end() - begin());
    }
    size_type capacity() const 
    {
       return size_type(end_of_storeage-begin());
    }
    bool empty() const {return begin() == end();}
    reference operator[] (size_type n)
    {
        return *(begin() + n);
    }
    reference front() { return *begin(); }
    reference back()  { return *(end() - 1);}
};
```

```c++
//vector如何填充元素，扩充二倍capacity？
void push_back(const T& x) {
    if (finish != end_of_storeage) {//尚有空间
        construct(finish, x); //全局函数
        ++finish;  //调整水位高度
    }
    else { //已无可用空间
        insert_aux(end(), x);
    }
}

template <class T, class Alloc>
void vector<T, Alloc>::insert_aux(iterator position, const T& x) {
    if(finish != end_of_storeage) {//尚有空间
    //在可用空间起始处建立一个元素，并以vector最后一个元素为其初值
        construct(finish, *(finish - 1));
        ++finish;
        T x_copy = x;
        copy_backward(position, finish-2, finish-1);
        *position = x_copy;
    }
    else {
        const size_type old_size = size();
        const size_type len = old_size!=0 ? 2*old_size : 1;
        //分配原则：如果原大小为0，分配1；如果原大小不为0，则分配原来的两倍
        iterator new_start = data_allocator::allocate(len);//分配器分配空间
        iterator new_finish = new_start;
        try {
            //将原vector内容拷贝到新的vector
            //为新元素设初值x
            //调整水位
            //拷贝安插点处的原内容
        }
        catch(...){
            ...
        }
        //解放并释放原vector
        destoroy(begin(), end());
        deallocate();
        //调整迭代器，指向新的vector
        start = new_start;
		finish = new_finish;
		end_of_storeage = new_start + len;
    }
}
```

vector's iterator

```c++
template <class T, class Alloc = alloc>
class vector {
public:
    typedef T  value_type;
    typedef value_type* iterator;  //T*
    ...
};
//取vector的迭代器
vector<int> vec;
vector<int>::iterator ite = vec.begin();
iterator_traits<ite>::iterator_category
iterator_traits<ite>::difference_type
iterator_traits<ite>::value_type
```

![1649644892643](.assets\1649644892643.png)

###### 17、array、forward_list深度探索

```c++
//TR1版本
template<typename _Tp, std::size_t _Nm>
struct array {
    typedef _Tp  value_type;
    typedef _Tp* pointer;
     typedef value_type* iterator;
    value_type _M_instance[_Nm ? _Nm : 1];
    //_Nm是0时就变成1，避免array长度为1
    iterator begin() {
        return iterator(&_M_instance[0]);
    }
    iterator end() {
        return iterator(&_M_instance[_Nm]);
    }
    ...
};
array<int, 10>myArray;
auto ite = myArray.begin();
ite += 3;
cout << *ite;
```

###### 18、deque、queue、stack深度探索上

![1649646970343](.assets\1649646970343.png)

分段连续 分段然后串接 每段是一个buffer

map是一个vector，作为控制中心，控制中心用完之后，会扩充到原来容量的2倍，旧的数据copy到新的map的中段

缓冲区buffer用完之后，map会扩充一个新的缓冲区

node指向map控制中心

如果iterator走到了边界，就需要一个新的buffer

```c++
template <class T, class Alloc = alloc, 
	size_t BufSiz = 0>
class deque {
public:
    typedef T value_type;
    typedef _deque_iterator<T,T&,T*,BufSiz> iterator;
protected:
    typedef pointer* map_pointer; //T**
protected:
    iterator start;
    iterator finish;
    map_pointer map;
    suze_type map_size;
public:
    iterator begin() {return start;}
    iterator end() {return finish;}
    size_type size() const {return finish - start;}
}

template <class T, class Ref, class Ptr, size_t BufSiz>
//第三个模板参数就是缓冲区大小参数，默认是0表示缓冲区大小为512字节。
struct _deque_iterator {
    typedef random_access_iterator_tag iterator_category;
    typedef T value_type;
    typedef Ptr pointer;
    typedef Ref reference;
    typedef Size_t size_type;
    typedef ptrdiff_t difference_type;
    typedef T** map_pointer;
    typedef _deque_iterator self;
    
    T* cur;
    T* first;
    T* last;
    map_pointer node;
}

//在position处安插一个元素，其值为x
iterator insert(iterator position,const value_tyoe& x)
    if(position.cur == start.cur) {
        push_front(x);
        return start;
    }
	else if(position.cur == finish.cur) {
        push_back(x);
        iterator tmp = finish;
        --tmp;
        return tmp;
    }
else {
    return insert_aux(position, x);
}
```

deque如何模拟连续空间？ 依靠deque iterators

```c++
reference operator[](size_type n) {
    return start[difference_type(n)];
}
reference front() {return *start;}
reference back() {
    iterator tmp = finish;
    --tmp;
    return *tmp;
}
size_type size() const
{
    return finish - start;
}
bool empty() const {
    return finish == start; 
}
```

总结：

vector动态扩容机制：当需要扩容时需要分配一块更大的空间，并将原始空间的元素一个个移动到新的空间。

dueue是一个双端开口的数组，没有容量的概念，可以随时在原空间基础上扩容。

缺点：deque的迭代器并不像vector一样可以实现随机访问。

dueue的空间是分段连续的，连续是假象，分段是实质。通过一个中控器控制地址的分配，当中控器用完之后，依照vector的内存扩展延伸至2倍空间，将旧的内容拷贝到新map的中间位置。map中控器分配的每一段空间（一个缓冲区）是连续的，而每一段分布在不同的内存空间。这种拷贝只是地址的拷贝

一开始分配一个map,从map的中间开始存放需要的缓冲区地址，当dueue尾部存放元素超过当前大小，就分配一个新的缓冲区将地址存放在map的后一个位置。前插则往前存放缓冲区地址。

（此Map只是一个中控器，并不是STL中的map）

deque的迭代器：每一个迭代器管理map的一个node，也就是一个buffer。迭代器的三个重要指针：cur first last.node的当前元素，头和尾，整个迭代器维护整体连续的假象，实现所有随机访问迭代器的所有接口。

容器queue：只能从队首删除元素，从队尾插入元素；deque双端队列可以在队头队尾进行入队出队操作;stack只从一段进出。

如何在deque的基础上实现queue和stack?只要将deque的一部分功能关闭就可以实现后面两种容器，将deque作为底层容器，把事情交给deque去做。因此queue和stack也称为容器适配器。

```c++
template <class T, class Sequence == deque <T>>
class queue {
....
public:
	typedef typename Sequence::value type value type;
	typedef typename Sequence:size_ type size_ type;
	typedef typename Sequence::reference reference;
	typedef typename Sequence:const reference const reference;
protected:
	Sequence c; //底層容器
public:
    bool emptyO const { return c.emptyO; }
    size_ type size() const { return c.size(); }
    reference front() { return cfront(); }
    const reference front() const { return c.front(); }
    reference back() { return c.back(); } 
    const reference back( const { return c.back(; }
    void push(const value_ type& x) { c.push_ back(x); }
    void pop() { c.pop_ front0; }
```

stack和queue都不允许遍历，也不提供迭代器。可以选择list和deque作为底层结构。stack可以选择vector做底层结构，queue不可以选择vector做底层结构。都不可以选择set或map作为底层结构。原因是调用不到底层结构容器中的函数。

以下为关联式容器

###### 20、RB-tree深度探索

 rb-tree提供遍历及迭代器，按照++ite遍历便可以获得排序状态。

不应使用迭代器改变元素的值。可以这样做，但是红黑树要为set和map提供服务，作为他们的底层结构。map允许改变value,key不允许改变。

rb-tree提供两种insertion操作：insert_unique()和insert_equal()

前者表示key不能重复，后者可以重复

![1649671363638](.assets\1649671363638.png)

```c++
template<class Key,
		class Value,
		class KeyOfValue,
		class Compare,
		class Alloc = alloc>
class rb_tree {
protected:
    typedef _rb_tree_node<Value> rb_tree_node;
    ...
public:
    typedef rb_tree_node* link_type;
    ...
protected:
    size_type node_count;  //rb_tree大小（节点数量）
    link_type header;
    Compare key_compare; //key大小比较准则
    ...
} 
//使用红黑树 key|data合成value
rb_tree<int,
		int,
		identity<int>, //identity函数实现 key就是value
		less<int>,
		alloc>
myTree;
```

######  21、set、multiset深度探索

以rb_tree为底层结构，value和key合一，value就是key

提供迭代器遍历，不能使用迭代器改变元素值。set/multiset的iterator是其底部的RB tree的const-iterator

![1649676272540](.assets\1649676272540.png)

set的操作也来自于底层红黑树的操作，也可以看作一个容器适配器。

```c++
template <class Key,
    	class Compare = less<Key>,
   		class Alloc = alloc>
class set {
public:
	// typedefs:
    typedef Key key_type;
    typedef Key value_type;
    typedef Compare key_compare;
    typedef Compare value_compare;
private:
    typedef rb_tree<key_type, value_type,
    identity<value_type>,key_compare,Alloc> rep_type;
    rep_type t;
public:
    typedef typename rep_type: :const_iterator iterator;
    ...
}
```

什么时候用set？需要随时插入元素，随时对元素快速查找，又要按照某种顺序对元素进行遍历

什么时候用multiset？key可以重复存在。

 multiset和set的成员函数声明是基本一样的，set::insert(key)的返回值是一个pair<iterator, bool>，其中pair中的bool成员表明了key被插入之前，set中是否已存在相同的key；也就是说，如果set中已经存在相同key的元素，那么插入操作是会失败的，新的元素不会被插进去；而multiset::insert的返回值只是一个iterator，插入操作总是会成功的。

###### 22、map、multimap深度探索

map的key必须独一无二，multimap的key可以重复

![1649679122484](.assets\1649679122484.png)

###### 23、hashtable深度探索

 hashTable的主干部分使用vector来实现的，vector上的每一个元素被称为bucket，即桶，而每一个bucket都指向一个list。这里的list并不是 stl 中的list，而是自行维护的 hashTable node。 

![1649681229236](.assets\1649681229236.png)

55%53 = 2    2%53 = 2    108%53 = 2

如果元素个数超过了buckets的大小，那么将buckets的容量扩升到两倍附近的质数  53就扩充到97，重新计算再放进去



