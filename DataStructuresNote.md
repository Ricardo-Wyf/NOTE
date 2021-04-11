# 2.数据的艺术

* 数据结构指数据对象中**数据元素之间的关系**
  * 数据元素之间不是独立的
    * 存在特定的关系，这些关系即结构
* **如：数组中各个元素之间存在固定的线性关系**



# 7.数据结构该如何学习？

* 先从概念上形象的**理解**数据元素之间的关系
* 思考这种关系**能够解决什么问题**
* 考虑基于这种关系能够产生哪些算法
* 理解和熟悉最终的算法
* **选择一种熟悉的语言，编码实战**



# 8.泛型编程简介

#### 泛型编程

* 不考虑具体数据类型的编程方式

```c++
void Swap(T& a, T& b)
{
    T t = a;
    b = a;
    a = t;
}
// T泛指任意的数据类型
```

---

#### 函数模板

* 一种特殊的函数可用不同类型进行调用
* 类型可被参数化

```c++
template <typename T>
void Swap(T& a, T& b)
{
    T t = a;
    a = b;
    b = t;
}
```

* `template`关键字用于**声明开始进行泛型编程**
* `typename`关键字用于**声明泛指类型**

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201107113819032.png" alt="image-20201107113819032" style="zoom:80%;" />

**函数模板的使用**

* 自动类型推导调用
* 具体类型显示调用

```c++
int a = 0;
int b = 1;
Swap(a, b);        // 自动推导

float c = 2;
float d = 3;
Swap<float>(c, d); // 显示调用
```

---

#### 类模板

* 一些类主要用于**存储和组织数据元素**，类中数据组织方式和数据元素的**具体类型无关**，如：数组类、链表类、Stack类、Queue类等

```c++
template <typename T>
class Operator
{
    public:
    	T op(T a, T b);
};
```

---

**类模板的应用**

* 只能显示指定具体类型，无法自动推导
* 使用具体类型`<Type>`定义对象

```c++
Operator<int> op1;
Operator<string> op2;
int i = op1.op(1, 2);
string s = op2.op("Ricardo", "Tiffany");
```

* 声明的**泛指类型**`T`可以出现在类模板的任何地方
* 编译器对类模板的处理方式和函数模板相同
  * 从类模板通过具体类型**产生不同的类**
  * 在声明的地方对类模板代码本身进行编译
  * 在使用的地方对参数替换后的代码进行编译（二次编译）

---

**类模板的工程应用**

* 类模板必须在**头文件中定义**
* 类模板**不能分开实现在不同的文件中**
* 类模板**外部定义**的成员函数需要加上**模板`<>`**声明

```c++
template <typename T>
class Operator
{
    public:
    	T add(T a, T b);
		T sub(T a, T b);
		T mul(T a, T b);
		T div(T a, T b);
};

template <typename T>
T Operator<T>::add(T a, T b)
{
	return a + b;
}
//...
```



# 9.智能指针示例

#### 内存泄露

* 动态申请堆空间，用完后不归还
* C++中没有垃圾回收机制
* 指针无法控制所指堆空间的生命周期

#### What we need

* 指针生命周期结束时**主动释放堆空间**
* 一片堆空间**最多只能由一个指针标识**
* **杜绝**指针运算和指针比较

#### 智能指针

* 重载指针特征操作符（<font color='red'>-></font> 和 <font color='red'>*</font>）
* **只能通过成员函数重载**
* 重载函数**不能使用参数**
* 只能定义**一个重载函数**

**WARNING：智能指针只能用来指向堆空间中的对象或者变量**

<font color='red'>**SmartPointer.h**</font>



# 10.C++异常简介

#### 异常处理

* C++内置异常处理的语法元素`try...catch`

  * `try`语句处理正常的代码逻辑
  * `catch`语句处理异常情况

  ```c++
  try
  {
      double r = divide(1, 0);
  }
  catch(...)
  {
      cout << "Divided bt zero..." << endl;
  }
  ```

* 通过`throw`语句抛出异常信息

  ```c++
  double divide(double a, double b)
  {
      const double delta = 0.00000000001;
      double ret = 0;
      
      if (!( (-delta < b) && (b < delta) ))
      {
          ret = a / b;
      }
      else
      {
          throw 0;  // 产生除0异常
      }
      
      return ret;
  }
  ```

* `throw`抛出的异常必须被`catch`处理
  
  * 当前函数**能处理异常**，程序继续执行
  * 当前函数**无法处理异常**，函数**停止执行**，并异常返回

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201111103128658.png" alt="image-20201111103128658" style="zoom:80%;" />

* **同一个`try`语句可以跟上多个`catch`语句**
  
  * `catch`语句可以定义具体处理的异常类型
  * **不同类型的异常**由不同的`catch`语句负责处理
  * **`try`语句中可以抛出任何类型的异常**
  * `catch(...)`用于处理所有类型的异常，**只能位于多个`catch`语句的最后一个**
* 任何异常都只能被捕获（`catch`）一次
  
* 异常处理的匹配规则

   <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201111103918619.png" alt="image-20201111103918619" style="zoom:80%;" />

> 异常处理必须严格匹配，不进行任何的类型转换

* `catch`语句块中可以抛出异常

   <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201111153038239.png" alt="image-20201111153038239" style="zoom:80%;" />



# 11.异常类构建

* 异常的类型可以是**自定义类类型**
* 类类型异常的匹配依旧是**至上而下严格匹配**
* 依旧**适用赋值兼容性原则**
  * 匹配子类异常的`catch`放在上部
  * 匹配父类异常的`catch`放在下部
* 在定义`catch`语句块时**推荐使用引用作为参数，避开拷贝构造提升效率**

---

**现代C++库必然包含充要的异常类族**

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201116174006258.png" alt="image-20201116174006258" style="zoom:80%;" />

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201116174223835.png" alt="image-20201116174223835" style="zoom:80%;" />

**异常类中的接口定义**：

```c++
class Exception
{
protected:
    char* m_message;
    char* m_location;

    //构造函数内部逻辑类似，使用init()来初始化
    void init(const char* message, const char* file, int line);
public:
    Exception(const char* message);
    Exception(const char* file, int line);
    Exception(const char* message, const char* file, int line);

    Exception(const Exception& e);
    Exception& operator = (const Exception& e);

    virtual const char* message() const;
    virtual const char* location() const;

    virtual ~Exception() = 0;
};
```

`strdup`,`strcpy`,`strcat`



# 12.顶层父类的创建

* 当代软件架构实践中的经验
  * 尽量使用**单重继承**的方式进行系统设计
  * 尽量保持系统中只存在**单一的继承树**
  * 尽量使用**组合关系**代替继承关系

创建`RWLib::Object`：

```c++
class Object
{
public:
    void* operator new (unsigned int size) throw();
    void operator delete (void* p);

    void* operator new[] (unsigned int size) throw();
    void operator delete[] (void* p);

    virtual ~Object() = 0; // 所有子类都能进行动态类型识别
};
```

* **所有数据结构都继承自`Object`类**
* 定义动态内存申请行为，提高代码的移植性



# 13.类族结构的进化

所有类位于单一的继承树

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201117230654573.png" alt="image-20201117230654573" style="zoom:80%;" />

**改进的点：**

* `Exception`类继承自`Object`类
  * 堆空间中创建异常对象失败时，返回NULL指针
* **新增`InvalidOperationException`异常类**
  * 成员函数调用时，如果状态不正确则抛出异常
* `SmartPointer`类继承自`Object`类
  * 堆空间中创建智能指针对象失败时，返回NULL指针



# 14.线性表的本质和操作

#### 线性表的表现形式

* 零个或多个数据元素组成的集合
* 数据元素**在位置上是有序排列的**
* **数据元素的个数是有限的**
* 数据元素的类型必须相同

> 抽象定义：线性表是具有相同类型的N(N>=0)个数组元素的有限序列

#### 线性表（List）的性质

* a0为线性表的第一个元素，只有一个后继
* a(n-1)为线性表的最后一个元素，只有一个前驱
* 除a0和a(n-1)外的其他元素a(i)，既有前驱又有后继
* 直接支持逐项访问和顺序存取

#### 线性表的常用操作

* 将元素插入线性表
* 将元素从线性表中删除
* 获取目标位置处元素的值
* 设置目标位置处元素的值
* 获取线性表的长度
* 清空线性表

**线性表抽象类的创建**：

```c++
template <typename T>
class list : public Object
{
public:
    virtual bool insert(int i, const T& e) = 0;
    virtual bool remove(int i) = 0;
    virtual bool set(int i, const T& e) = 0;
    virtual bool get(int i, const T& e) const = 0;
    virtual int length() const = 0;
    virtual void clear() = 0;
}
```



# 15.线性表的顺序存储结构

**顺序存储的定义**：

线性表的顺序存储结构指的是：用一段地址连续的存储单元依次存储线性表中的数据元素

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201118153053718.png" alt="image-20201118153053718" style="zoom:80%;" />

**设计思路**：

用**一维数组**来实现顺序存储结构

* 存储空间：`T* m_array`
* 当前长度：`int m_length`

```c++
template <typename T>
class SeqList : public List<T>
{
protected:
	T* m_array;
    int m_length;
    //...
};
```

* 顺序存储结构的元素获取操作
  * 判断目标位置是否合法
  * 将目标位置作为数组下标获取元素

```c++
bool SeqList<T>::get(int i, T& e) const
{
    bool ret = ((0 <= i) && (i < m_length));
    if (ret)
    {
        e =  m_array[i];
    }
    
    return ret;
}
```

* 顺序存储结构的元素插入操作
  1. 判断目标位置是否合法
  2. 将目标位置之后的所有元素**后移**一个位置
  3. 将新元素插入目标位置
  4. 线性表长度加1

```c++
bool SeqList<T>::insert(int i, const T& e)
{
    bool ret = ((0 <= i) && (i <= m_length));
    ret = ret && ( (m_length + 1) <= capacity() );
    
    if (ret)
    {
        for (int p=m_length-1; p>=i; p--)
        {
            m_array[p+1] = m_array[p];
        }
        m_array[i] = e;
        m_length++;
    }
    
    return ret;
}
```

* 顺序存储结构的元素删除操作
  1. 判断目标位置是否合法
  2. 将目标位置之后的所有元素**前移**一个位置
  3. 线性表长度减1

```c++
bool SeqList<T>::remove(int i)
{
    bool ret = ((0 <= i) && (i < m_length));
    
    if (ret)
    {
        for (int p=i; p<m_length-1; p++)
        {
            m_array[p] = m_array[p+1];
        }
        m_length--;
    }
    
    return ret;
}
```



# 16.顺序存储结构的抽象实现

#### `SeqList`设计要点

* 抽象类模板，存储空间的位置和大小由子类完成
* 实现顺序存储结构线性表的**关键操作**（增，删，查，等）
* **提供数组操作符**，方便快速获取元素



# 17.StaticList和DynamicList

 ![image-20201118182519793](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201118182519793.png)

#### StaticList设计要点

**类模板**

* 使用**原生数组**作为顺序存储空间
* 使用模板参数决定数组大小

```c++
template <typename T, int N>
class StaticList : public SeqList<T>
{
protected:
    T m_space[N]; // 顺序存储空间，N为模板参数
public:
    StaticList(); // 指定父类成员的具体值
    int capacity() const;
}
```

---

#### DynamicList设计要点

**类模板**

* 申请**连续堆空间**作为顺序存储空间
* **动态设置**顺序存储空间的大小
* **保证重置顺序存储空间时的异常安全性**
  * 函数异常安全的概念
    * 不泄露任何资源
    * 不允许破坏数据
  * 函数异常安全的基本保证
    * 如果异常被抛出
      * 对象内的任何成员仍然能保持有效状态
      * 没有数据的破坏及资源泄露

```c++
template <typename T>
class DynamicList : public SeqList<T>
{
protected:
    int m_capacity; // 顺序存储空间的大小
public:
    DynamicList(int capactiy); // 申请空间
    int capactiy() const;
    /* 重新设置顺序存储空间的大小 */
    void resize(int capacity);
    ~DynamicList(); // 归还空间
};
```

> resize()函数实现需要保证异常安全

```c++
void resize(int capacity)
{
    T* array = new T[capacity];

    if (array != NULL)
    {
        int length = (this->m_length < capacity ? this->m_length : capacity);
        for (int i=0; i<length; i++)
        {
            array[i] = this->m_array[i]; // 保存数据
        }

        T* tmp = this->m_array;    /* 若直接使用delete[] m_array，如果T为类类型，会调用析构函数
        							  若析构函数抛出异常，程序终止，就无法保证对象内的成员有效、该对象										仍然可用，也无法保证保存在新堆空间里的数据是完全正确的。    */

        this->m_array = array;
        this->m_length = length;
        this->m_capacity = capacity;

        delete[] tmp;
    }
    else
    {
        THROW_EXCEPTION(NoEnoughMemoryException, "No enough memory to resize DynamicList!!");
    }
}
```



# 18.顺序存储线性表的分析

#### 效率分析

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201119170345974.png" alt="image-20201119170345974" style="zoom:80%;" />

**若T所代表的类型不同，即使有相同的时间复杂度，实际执行时间可能大相径庭。**

> 比如：int 和 string

---

 ![image-20201119171929963](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201119171929963.png)

 ![image-20201119172004328](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201119172004328.png)

**通过对象赋值和拷贝构造，会使得`m_array`指向同一片内存区域，这会带来隐患。**

<font color='red'>**所以，对于容器类型的类，可以考虑禁用拷贝构造和赋值操作**</font>

---

 ![image-20201119182649113](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201119182649113.png)

**线性表必须先插入元素，才能使用操作符[] 访问元素**，提供数组操作符重载是为了快捷方便获取目标位置的数据元素，使用形式上类似数组，但本质不同，不能代替数组。



# 19、20.数组类的创建

 ![image-20201120094231381](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201120094231381.png)

创建数组类**代替原生数组**的使用

* 数组类包含长度信息
* 数组类能够主动发现越界访问

#### **`Array`设计要点**：

* 抽象类模板，**存储空间的位置和大小由子类完成**
* **重载数组操作符**，判断访问下标是否合法
* 提供数组长度的**抽象访问函数**
* **提供数组的对象间的复制操作**

```c++
template <typename T>
class Array : public Object
{
protected:
    T* m_array;
public:
    virtual bool set(int i, const T& e);
	virtual bool get(int i, T& e) const;

    T& operator [] (int i);
	T operator [] (int i) const;

    virtual int length() = 0;
};
```

---

#### **`StaticArray`设计要点**

**类模板**

* 封装**原生数组**
* 使用模板参数决定数组大小
* 实现函数返回数组的长度
* **拷贝构造**和**赋值操作**

```c++
template <typename T, int N>
class StaticArray : public Array<T>
{
protected:
    T m_space[N];
public:
    StaticArray();

    StaticArray(const StaticArray<T, N>& obj);
    StaticArray<T, N>& operator = (const StaticArray<T, N>& obj);

    int length()
    {
        return N;
    }
};
```

---

#### **`DynamicArray`设计要点**

**类模板：**

* **动态确定**内部数组空间大小
* 实现函数返回数组长度
* **拷贝构造**和**赋值操作**

```c++
template <typename T>
class DynamicArray : public Array<T>
{
protected:
    int m_length;
public:
    DynamicArray(int length);

    DynamicArray(const DynamicArray<T>& obj);
    DynamicArray<T>& operator = (const DynamicArray<T>& obj);

    int length() const;
    void resize(int length);

    ~DynamicArray();
};
```

---

#### 重复代码逻辑的抽象

* **init**
  * 对象构造时的初始化操作
* **copy**
  * 在堆空间中申请新的内存，传入被赋值的数组指针，并执行拷贝操作
* **update**
  * 将指定（赋值后）的堆空间指针赋值给成员变量`m_array`，作为内部存储数组使用



# 21.线性表的链式存储结构

#### 链式存储

定义：**为了表示每个数据元素与其直接后继元素之间的逻辑关系，数据元素除了存储本身的信息外，还需要存储其直接后继的信息。**

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201121100142685.png" alt="image-20201121100142685"  />

**逻辑结构**：

* 基于链式存储结构的线性表中，每个结点都包含**数据域**和**指针域**
  * 数据域：存储数据元素本身
  * 指针域：存储相邻结点的地址

 ![image-20201121100404122](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201121100404122.png)

---

#### 链表中的基本概念

* **头结点**
  * 链表中的辅助结点，辅助数据元素的定位，方便插入和删除操作；**头结点不存储实际的数据元素，只包含指向第一个数据元素的指针**
* **数据结点**
  * 链表中代表数据元素的结点，表现形式为：（数据元素，地址）
* **尾结点**
  * 链表中的最后一个数据结点，包含的地址信息为空

**单链表中的结点定义**：

 ![image-20201121100907142](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201121100907142.png)

---

#### 链表插入和删除

* 在目标位置处**插入**数据元素

  1. **从头结点开始**，通过`current`指针**定位到目标位置**

  2. 从堆空间申请新的`Node`结点

  3. 执行操作：

     ```c++
     node->value = e;
     node->next = current->next;
     current->next = node;
     ```

* 在目标位置处**删除**数据元素

  1. **从头结点开始**，通过`current`指针**定位到目标位置**

  2. **使用`toDel`指针指向需要被删除的结点**

  3. 执行操作：

     ```c++
     toDel = current->next;
     current->next = toDel->next;
     delete toDel;
     ```



# 22.单链表的具体实现

#### LinkList设计要点

* 类模板，**通过头结点访问后继结点**
* 定义内部结点类型`Node`，用于描述数据域和指针域
* 实现线性表的**关键操作**（增、删、查、等）

```c++
template <typename T>
class LinkList : public List<T>
{
protected:
    struct Node : public Object
    {
		T value;
        Node* next;
    };
    
    Node m_header;//////
    int m_length;///////
public:
    LinkList();
    //...
};
```

#### 头结点的隐患和代码优化

 ![image-20201122163043649](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201122163043649.png)

`LinkList<Test> list`，定义成员变量`m_header`时，会调用`Test`的构造函数，从而抛出异常。

**优化**：

```c++
mutable struct : public Object
{
    char reserved[sizeof(T)];
    Node* next;
} m_header;
// 根据Node内部结构定义m_header，避免构造函数的调用
```



# 23.顺序表和单链表的对比分析

#### 如何判断某个数据元素是否存在于线性表中？

* `find`
  * 可为线性表（List）增加一个查找操作
  * `int find(const T& e) const;`
    * 参数：待查找的数据元素
    * 返回值：
      * 大于=0：数据元素在线性表中**第一次出现的位置**
      * -1：数据元素**不存在**

```c++
// 查找示例
LinkList<int> list;

for (int i=0; i<5; i++)
{
    list.insert(0, i);
}

cout << list.find(3) << endl; // ==> 1

```

* 线性表中元素的查找依赖于**相等比较操作符**（==），需要对其重载

```c++
bool Object::operator == (const Object& obj)
{
    return (this == &obj);
}

bool Object::operator != (const Object& obj)
{
    return (this != &obj);
}
```

* 对于具体的类类型，根据需求不同定义自己的**相等比较操作符**

```c++
class Test : public Object
{
protected:
    int i;
public:
    Test(int v = 0)
    {
        i = v;
    }

    bool operator == (const Test& obj)
    {
        return (i == obj.i);
    }
};
```

---

#### 顺序表和链表效率分析

* 插入和删除
  * 顺序表：**涉及大量数据对象的复制操作**
  * 单链表：只涉及指针操作，**效率与数据对象无关**
* 数据访问
  * 顺序表：**随机访问**，可直接定位数据对象
  * 单链表：**顺序访问**，必须从头访问数据对象，**无法直接定位**

**工程开发中的选择：**

* 顺序表：
  * **数据元素的类型相对简单**，不涉及深拷贝
  * **数据元素相对稳定**，访问操作远多于插入和删除操作
* 单链表：
  * **数据元素的类型相对复杂**，复制操作相对耗时
  * 数据元素不稳定，**需要经常插入和删除**，访问操作较少



# 24.单链表的遍历与优化

**当前单链表的遍历方法：**

```c++
LinkList<int> list;

for (int i=0; i<5; i++) // O(n)
{
    list.insert(0, i);
}

for (int i=0; i<list.length(); i++) // O(n2)
{
    cout << list.get(i) << endl;
}
```

#### **新需求：在线性时间内完成单链表的遍历**

**设计思路（游标）：**

* 在单链表的内部**定义一个游标**（`Node* m_current`）
* 遍历开始前**将游标指向位置为0的数据元素**
* 获取游标指向的数据元素
* 通过结点中的`next`指针**移动游标**

 ![image-20201123201241692](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201123201241692.png)

```c++
bool move(int i, int step = 1);
bool next();
T current();
bool end();
```

#### 封装优化

单链表**内部**的一次封装

```c++
virtual Node* create()
{
    return new Node();
}

virtual void destroy(Node* pn)
{
    delete pn;
}
```



# 25.静态单链表的实现

**单链表的缺陷**

* 触发条件
  * 长时间使用单链表对象频繁增加和删除数据元素
* 可能结果
  * 堆空间产生大量的内存碎片，导致系统运行缓慢

#### **新的线性表**

设计思路：

**在“单链表”的内部增加一片预留的空间，所有的Node对象都在这片空间中动态创建和动态销毁**

 ![image-20201124095738349](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201124095738349.png)

 ![image-20201124095754361](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201124095754361.png)

#### 静态单链表的实现思路

* 通过模板定义静态单链表类（`StaticLinkList`)
* 在类中定义**固定大小的空间**（`unsigned char []`)，**在预留的空间中创建结点对象**
* 重写`create`和`destroy`函数，**改变内存的分配和归还方式**
* 在`Node`类中重载`operator new`，用于在指定内存上创建对象

 ![image-20201124193947709](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201124193947709.png)

> 静态单链表适合于频繁增删数据元素的场合（最大元素个数固定）



# 26.典型问题分析（Bugfix）

#### 问题1.创建异常对象时的空指针问题

 ![image-20201125101459274](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201125101459274.png)

**解决方案：应对空指针的情况进行处理**

```c++
 m_message = (m_message ? strdup(message) : NULL);
```



#### 问题2.LinkList中的数据元素删除

```c++
LinkList<Test> list;
Test t0(0), t1(1), t2(2);

try
{
	list.insert(t0);
    list.insert(t1);////////////////
    list.insert(t2);
    
    list.remove(1);
}
catch(...)
{
    cout << list.length() << endl; // Expected is 2
}
```

**解决方案：交换`m_length--`和`destroy`的逻辑顺序（在VS下实验），最后再调用`destroy`**



#### 问题3.LinkList中遍历操作与删除操作的混合使用

```c++
LinkList<int> list;

for (int i=0; i<5; i++)
{
    list.insert(i);
}

for (list.move(0); !list.end(); list.next())
{
	if (list.current() == 3)
    {
		list.remove(list.find(list.current())); // 删除
        cout << list.current() << endl; // 删除成功后，list.current()返回什么值
    }
}

for (int i=0; i<list.length(); i++)
{
    cout << list.get(i) << endl;
}
```

**解决方案：使`m_current`指向下一个数据元素**

```c++
bool remove(int i)--->
if (m_current == toDel)
{
    m_current = toDel->next;
}
```



#### 问题4.StaticLinkList中数据元素删除时的效率问题

```c++
void destroy(Node* pn)
{
    SNode* space = reinterpret_cast<SNode*>(m_space);
    SNode* psn = dynamic_cast<SNode*>(pn);

    for (int i=0; i<N; i++)
    {
        if (psn == (space + i)) //////////////////
        {					 	// 空间归还，对象析构
            m_used[i] = 0;		// 即可跳出循环
            psn->~SNode();		//
        }						//////////////////
    }

}
```

**解决方案：`break`**

```c++
if (psn == (space + i)) //////////////////
{					 	// 空间归还，对象析构
    m_used[i] = 0;		// 即可跳出循环
    psn->~SNode();		//
    break;
}
```



#### 问题5.StaticLinkList是否需要提供析构函数？

由于`StaticLinkList`继承`LinkList`，如果使用默认的析构函数，会调用`LinkList`的析构函数：

```c++
~LinkList()
{
    clear();// 为析构函数里的虚函数，只会调用当前类的虚函数
}

void clear()
{
    while (m_header.next)
    {
        Node* toDel = m_header.next;
        m_header.next = toDel->next;
  
        m_length--;

        destroy(toDel); // 为析构函数里的虚函数，只会调用当前类的虚函数
    }
}

virtual void destroy(Node* pn)
{
    delete pn;
}
```

> 子类StaticLinkList里的数据元素被销毁时，调用的是delete，这是错误的，故需要定义自己的析构函数

**解决方案：定义`StaticLinkList`自己的析构函数**

```c++
~StaticLinkList()
{
    this->clear();
}
```



#### RWLib是否有必要增加多维数组类？

**多维数组的本质：数组的数组！**

```c++
#include <iostream>
#include "DynamicArray.h"

using namespace std;
using namespace RWLib;

int main()
{
    DynamicArray< DynamicArray<int> > list(3);

    for (int i=0; i<list.length(); i++)
    {
        list[i].resize(i + 1);
    }

    for (int i=0; i<list.length(); i++)
    {
        for (int j=0; j<list[i].length(); j++)
        {
            list[i][j] = i * j;
        }
    }

    for (int i=0; i<list.length(); i++)
    {
        for (int j=0; j<list[i].length(); j++)
        {
            cout << list[i][j] << " ";
        }

        cout << endl;
    }
    return 0;
}

```



# 27、28.再论智能指针

**思考：使用智能指针（SmartPointer)替换单链表（LinkList）中的原生指针是否可行？**

**ANSWER**：由于`SmartPointer`的设计是**一片堆空间最多只能由一个指针标识**，但在`insert`函数里有两个指针指向同一片堆空间的情况，故`SmartPointer`无法替换单链表中的原生指针。

---

 ![image-20201128152728498](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201128152728498.png)

#### 新的设计方案

* **Pointer** 是智能指针的抽象父类（模板）
  * 纯虚析构函数`virtual ~Pointer() = 0`
  * 重载`operator -> ()`
  * 重载`operator * ()`

```c++
template <typename T>
class Pointer : public Object
{
protected:
	T* m_pointer;
public:
    Pointer(T* p = NULL);
    T* operator -> ();
    T& operator * ();
    bool isNUll();
    T* get();
};
```

#### 实现SharedPointer

**`SharedPointer`设计要点**

**类模板：**

通过**计数机制（ref）**标识堆内存

* 堆内存被指向时：ref + +
* 指针被置空时：ref - -
* ref == 0时：释放堆内存

 ![image-20201129202346711](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201129202346711.png)

```c++
template <typename T>
class SharedPointer : public Pointer<T>
{
protected:
    int* m_ref; // 计数机制成员指针
public:
    SharedPointer(T* p = NULL);
    SharedPointer(const SharedPointer<T>& obj);
    
    SharedPointer& operator = (const SharedPointer<T>& obj);
    
    void clear(); // 将当前指针置为空
    
    ~SharedPointer();
};
// 由于SharedPointer支持多个对象同时指向一片堆空间，必须支持比较操作
template <typename T>
bool operator == (const SharedPointer<T>& l, const SharedPointer<T>& r)
{
    return (l.m_pointer == r.m_pointer);
}

template <typename T>
bool operator != (const SharedPointer<T>& l, const SharedPointer<T>& r)
{
    return !(l == r);
}
```

#### 智能指针的使用军规

* 只能用来**指向堆空间中的单个变量（对象）**
* 不同类型的智能指针对象**不能混合使用**
* 不要使用`delete`释放智能指针指向的堆空间



# 29.循环链表的实现

#### 循环链表

**概念上：**

* 任意数据元素都有一个前驱和一个后继
*  所有的数据元素的关系构成一个**逻辑上的环**

**实现上：**

* 循环链表是**一种特殊的单链表**
* 尾结点的指针域保存了首结点的地址

 ![image-20201201230902800](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201201230902800.png)

**实现思路：**

* 通过模板定义`CircleList`类，继承自`LinkList`类
* 定义内部函数`last_to_first()`，**用于将单链表首尾相连**
* **特殊处理：**首元素的**插入操作和删除操作**
* **重新实现：**清空操作和遍历操作

**实现要点：**

* **插入位置为0时**
  * 头结点和尾结点均指向新结点
  * 新结点成为首结点插入链表
* **删除位置为0时**
  * 头结点和尾结点指向位置为1的结点
  * 安全销毁首结点

#### 循环链表的应用

约瑟夫环问题

```c++
void Josephus(int all, int s, int n)
{
    CircleList<int> cl;
    for (int i=1; i<=all; i++)
    {
        cl.insert(i);
    }

    cl.move(s-1, n-1);

    while (cl.length() > 0)
    {
        cl.next();

        cout << cl.current() << endl;

        cl.remove(cl.find(cl.current()));
    }
}

int main()
{
    Josephus(41, 1, 3);

    return 0;
}
```



# 30.双向链表的实现

#### 单链表的缺陷

* **单向性**
  * 只能从头结点开始高效访问链表中的数据元素
* **缺陷**
  * 需要**逆向访问**单链表中的数据元素时将**极其低效**

```c++
int main()
{
    LinkList<int> l;
    
    for (int i=0; i<5; i++)
    {
        l.insert(0, i);
    }
    
    for (int i=l.length()-1; i>=0; i--) // O(n2)
    {
        cout << l.get(i) << endl;
    }
    
    return 0;
}
```

#### 双向链表

 ![image-20201204171014884](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201204171014884.png)

 ![image-20201204171023123](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201204171023123.png)

```c++
template <typename T>
class DualLinkList : public List<T>
{
protected:
    struct Node : public Object
    {
        T value;
        Node* next;
        Node* pre;
    };

    mutable struct : public Object
    {
        char reserved[sizeof(T)];
        Node* next;
        Node* pre;
    } m_header;
    //...
};
```

**WARNING：** **首结点的pre为NULL**（当current为头结点时）；**next不为空时**才能有`next->pre = node`操作。



# 31.老生常谈的两个宏（Linux）

```c
#define offsetof(type, member) ((size_t)&((type*)0)->member)
```

#### offsetof

**用于计算`type`结构中`member`成员的偏移位置**

* 编译器清楚的知道结构体成员变量的偏移位置
* 通过**结构体变量首地址**与**偏移量**定位成员变量

```c++
struct ST
{
    int i;
    int j;
    char c;
};

void func(struct ST* pst)
{
    int* pi = &(pst->i); // (unsigned int)&s + 0
    int* pj = &(pst->j); // (unsigned int)&s + 4
    char* pc = &(pst->c);// (unsigned int)&s + 8

    printf("pst = %p\n", pst);
    printf("pi = %p\n", pi);
    printf("pj = %p\n", pj);
    printf("pc = %p\n", pc);
}
```

#### container_of

```c
#define container_of(ptr, type, member) ({         \
    const typeof(((type*)0)->member)* mptr = (ptr);  \  /*类型安全检查*/
    (type*)((char*)mptr - offsetof(type, member));    }) 
```

* `( {  } )`是GNU C编译器的语法扩展
* 与逗号表达式类似，结果为最后一个语句的值

 ![image-20201205153824623](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201205153824623.png)

#### typeof

* 是GNU C编译器特有的关键字
* 只在编译期有效，**用于得到变量的类型**

```c
int i = 100;
typeof(i) j = i;
const typeof(j)* p = &j;

printf("sizeof(j) = %d\n", sizeof(j));
printf("j = %d\n", j);
printf("*p = %d\n", *p);
```

 ![image-20201205154300696](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201205154300696.png)



# 32.Linux内核链表剖析

* Linux内核链表的位置及依赖
  * 位置
    * {linux-2.6.39} \\ \ include\linux\list.h
  * 依赖
    * #include <linux/types.h>
    * #include <linux/stddef.h>
    * #include <linux/poison.h>
    * #include <linux/prefetch.h>

* 移植时的注意事项
  * 清除文件间的依赖
    * 剥离依赖文件中与链表实现相关的代码
  * 清除平台相关代码（GNU C）
    * ( {  } )
    * typeof
    * __builtin_prefetch
    * static inline

#### Linux内核链表的实现

* 带头节点的**双向循环链表**，且头节点为表中成员
* 头节点的**next**指针指向**首节点**
* 头节点的**prev**指针指向**尾结点**

 ![image-20201207195201022](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201207195201022.png)

**Linux内核链表的节点定义**

```c
struct list_head 
{
    struct list_head *next, *prev;
};
```

**使用Linux内核链表时需要自定义链表节点**

* 将`struct list_head`作为节点结构体的第一个成员或最后一个成员
* `struct list_head`作为最后一个成员时，**遍历时需要使用`list_entry`宏**
* `list_entry`的定义中使用了`container_of`宏

```c
struct Node
{
	struct list_head head;
    Type1 value1;
    Type2 value2;
	//...
};

struct Node
{
    Type1 value1;
    Type2 value2;
    struct list_head head;
	//...
};
////////////////////////////////////////////////////////////////////////////////////////
#define list_entry(ptr, type, member) \
    container_of(ptr, type, member)

for(i=0; i<5; i++)
{
    struct Node* n = (struct Node*)malloc(sizeof(struct Node));

    n->value = i;

    list_add(&n->head, list);
}

list_for_each(slider, list)
{
    printf("%d\n", list_entry(slider, struct Node, head)->value);
}
```

**Linux内核链表的创建及初始化**

```c
struct Node
{
    struct list_head head;
    int value;
};

static void INIT_LIST_HEAD(struct list_head *list)
{
    list->next = list;
    list->prev = list;
}

int main()
{
    struct Node l = {0};
    struct list_head* list = (struct list_head*)&l;
    
    INIT_LIST_HEAD(list);
    //...
}
```

**Linux内核链表的插入操作**

* 在链表头部插入：`list_add(new, head)`
* 在链表尾部插入：`list_add_tail(new, head)`

```c
static void __list_add(struct list_head *new,
                  struct list_head *prev,
                  struct list_head *next)
{
    next->prev = new;
    new->next = next;
    new->prev = prev;
    prev->next = new;
}

static void list_add(struct list_head *new, struct list_head *head)
{
    __list_add(new, head, head->next);
}

static void list_add_tail(struct list_head *new, struct list_head *head)
{
    __list_add(new, head->prev, head);
}
```

**Linux内核链表的删除操作**

```c
#define LIST_POISON1 NULL
#define LIST_POISON2 NULL

static void __list_del(struct list_head * prev, struct list_head * next)
{
    next->prev = prev;
    prev->next = next;
}

static void __list_del_entry(struct list_head *entry)
{
    __list_del(entry->prev, entry->next);
}

static void list_del(struct list_head *entry)
{
    __list_del(entry->prev, entry->next);
    entry->next = LIST_POISON1;
    entry->prev = LIST_POISON2;
}
```

**Linux内核链表的遍历**

* 正向遍历：`list_for_each(pos, head)`
* 逆向遍历：`list_for_each_pre(pos, head)`

```c
#define list_for_each(pos, head) \
    for (pos = (head)->next; prefetch(pos->next), pos != (head); \
            pos = pos->next)

#define list_for_each_prev(pos, head) \
    for (pos = (head)->prev; prefetch(pos->prev), pos != (head); \
            pos = pos->prev)
```



# 33.双向循环链表的实现

 ![image-20201210100006687](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201210100006687.png)

#### 设计思路

**数据节点之间在逻辑上构成双向循环链表，头节点仅用于节点的定位。**

 ![image-20201210100149669](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201210100149669.png)

#### 实现思路

* 通过模板定义`DualCircleList`类，继承自`DualLinkList`类
* 在`DualLinkList`内部**使用Linux内核链表进行实现**
* 使用`struct list_head`定义`DualCircleList`的头节点
* **特殊处理**：**循环遍历时忽略头节点**

#### 实现要点

* 通过`list_head`进行目标节点定位（position(i)）
* **通过`list_entry`将`list_head`指针转换为目标节点指针**
* 通过`list_for_each`实现`int find(const T& e)`函数
* 遍历函数中的`next()`和`pre()`需要考虑跳过头节点



# 34、35.栈的概念及实现

#### 栈的定义

* 栈是一种**特殊的线性表**
* **栈仅能在线性表的一端进行操作**
  * 栈顶（Top）：**允许操作的一端**
  * 栈底（Bottom）：不允许操作的一端

#### 栈的特性

* **后进先出（Last In First Out）**

 ![image-20201211095608341](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201211095608341.png)

#### 栈的操作

* 创建栈（`Stack()`）
* 销毁栈（`~Stack()`）
* 清空栈（`clear()`）
* 进栈（`push()`）
* 出栈（`pop()`）
* 获取栈顶元素（`top()`）
* 获取栈的大小（`size()`）

#### 栈的实现

 ![image-20201211095901760](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201211095901760.png)

```c++
template <typename T>
class Stack : public Object
{
public:
    virtual void push(const T& e) = 0;
    virtual void pop() = 0;
    virtual T top() const = 0;
    virtual void clear() = 0;
    virtual int size() const = 0;
};
```

---

**栈的顺序实现**

 ![image-20201211100203367](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201211100203367.png)

##### StaticStack设计要点

* 类模板
  * 使用**原生数组**作为栈的存储空间
  * 使用模板参数决定栈的**最大容量**

```c++
template <typename T, int N>
class StaticStack : public Stack<T>
{
protected:
    T m_space[N];   // 栈存储空间，N为模板参数
    int m_top;		// 栈顶标识
    int m_size;		// 当前栈的大小
public:
    StaticStack();  // 初始化成员变量
    int capacity() const;
    //...
};
```

---

**链式栈的存储实现**

 ![image-20201211110931090](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201211110931090.png)

##### 链式栈的设计要点

* 类模板，**抽象父类Stack的直接子类**
* 在内部**组合使用LinkList类**，实现栈的链式存储
* **只在单链表成员对象的头部进行操作**

 ![image-20201211120033192](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201211120033192.png)

#### 栈的应用实践

* 符号匹配问题
  * C语言中有一些成对匹配出现的符号
    * 括号：（），[ ]，{ }，< >
    * 引号：' '，" "

**算法思路：**

* **从第一个字符开始扫描**
  * 当遇见普通字符时忽略
  * 当遇见左符号时压入栈中
  * 当遇见右符号时弹出栈顶符号，并进行匹配
* **结束**
  * 成功：所有字符扫描完毕，且栈为空
  * 失败：匹配失败或所有字符扫描完毕但栈非空

> 栈非常适合于需要”就近匹配“的场合



# 36、37.队列的概念及实现

**队列是一种特殊的线性表**

队列**仅能在线性表的两端**进行操作

* 队头（Front）：取出数据元素的一端
* 队尾（Rear）：插入数据元素的一端

#### 队列的特性

* 先进先出（First In First Out）

 ![image-20201214155029724](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214155029724.png)

#### 队列的操作

* 创建队列（`Queue()`）
* 销毁队列（`~Queue()`）
* 清空队列（`clear()`）
* 进队列（`add()`）
* 出队列（`remove()`）
* 获取队列头元素（`front()`）
* 获取队列的长度（`length()`）

#### 队列的实现

 ![image-20201214155301515](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214155301515.png)

```c++
template <typename T>
class Queue : public Object
{
public:
    virtual void add(const T& e) = 0;
    virtual void remove() = 0;
    virtual T front() const = 0;
    virtual void clear() = 0;
    virtual int length() const = 0;
};
```

**队列的顺序实现**

 ![image-20201214155357320](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214155357320.png)

##### StaticQueue设计要点

* 类模板
  * 使用**原生数组**作为队列的存储空间
  * 使用模板参数决定队列的**最大容量**

```c++
template <typename T, int N>
class StaticQueue : public Stack<T>
{
protected:
    T m_space[N];   // 队列存储空间，N为模板参数
    int m_front;	// 队头标识
    int m_rear;		// 队尾标识
    int m_length;	// 当前队列的长度
public:
    StaticQueue();  // 初始化成员变量
    int capacity() const;
    //...
};
```

**实现要点（循环计数法）**

* **关键操作**
  * 进队列：`m_space[m_rear] = e; m_rear = (m_rear + 1) % N;`
  * 出队列：`m_front = (m_front + 1) % N;`
* **队列的状态**
  * 队空：`(m_length == 0) && (m_front == m_rear)`
  * 队满：`(m_length == N) && (m_front == m_rear)`

---

**队列的链式存储实现**

 ![image-20201214162316894](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214162316894.png)

##### 链式队列的设计要点

* 类模板，**抽象父类Queue的直接子类**
* 在内部**使用链式结构**实现元素的存储
* 只在链表的**头部**和**尾部**进行操作

 ![image-20201214162924686](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214162924686.png)

> 因为使用LinkList类模板中的`insert`，时间复杂度为O(n)，为了优化进队列的效率，对队列链式存储实现进行优化，优化后入队和出队操作可以在常量时间内完成

 ![image-20201214195916493](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214195916493.png)

**利用LinuxList来进行优化**

 ![image-20201214195954952](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201214195954952.png)



# 38.两个有趣的问题

#### 用栈实现队列

* **用”后进先出“的特性实现”先进先出“的特性**

**解决方案设计**

 ![image-20201215215100526](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201215215100526.png)

**实现思路：**

准备两个栈用于实现队列：`stack_in`和`stack_out`

* 当有**新元素入队**时：将其压入`stack_in`

* 当需要**出队**时：

  **stack_out.size() == 0：**

  * 将`stack_in`中的元素逐一弹出并压入`stack_out`
  * 将`stack_out`的栈顶元素弹出

  stack_out.size() > 0：

  * 将`stack_out`的栈顶元素弹出

#### 用队列实现栈

* **用队列”先进先出“的特性实现”后进先出“的特性**

**解决方案设计**

 ![image-20201218103640918](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201218103640918.png)

**实现思路：**

准备两个队列用于实现栈：`queue_1[in]`和`queue_2[out]`

* 当有**新元素入栈**时：将其加入队列[in]
* 当需要**出栈**时：
  * 将队列[in]中的 n-1 个元素出队列，并进入队列[out]中
  * 将队列[in]中的最后一个元素出队列（出栈）
  * 交换两个队列的角色：`queue_1[out]`和`queue_2[in]`



# 39、40.字符串类的创建

#### 遗留问题

* C语言**不支持**真正意义上的**字符串**
* C语言用**字符数组**和**一组函数**实现字符串操作
* C语言**不支持**自定义类型，因此**无法获得字符串类型**
* 从C到C++的进化过程引入了**自定义类型**
* 在C++中可以**通过类完成字符串类型的定义**

#### 设计与实现

 ![image-20201218104221733](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201218104221733.png)

**字符串类实现**

```c++
class String : public Object
{
protected:
    char* m_str;
    int m_length;

    void init(const char* s);
public:
    String();
    String(const char* s);
    String(const String& s);
    int length() const;
    const char* str() const;
	
    /* 比较操作符重载函数 */
	/* 加法操作符重载函数 */
	/* 赋值操作符重载函数 */
    
    ~String();
};
```

**实现注意事项**

* 无缝实现**String**对象和**char***字符串的互操作
* 操作符重载函数需要考虑**是否支持const版本**
* 通过C语言中的字符串函数实现String的成员函数

#### 字符串类中的常用成员函数

 ![image-20201218161734433](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201218161734433.png)

* **重载数组访问操作符[]**
  * `char& operator [] (int i);`
  * `char operator  [] (int i) const;`
* 注意事项
  * 当`i`取值不合法时，抛出异常
    * 合法范围：`( 0 <= i ) && ( i  < m_length )`

* **判断是否以指定字符串开始或结束**

  ```c++
  bool startWith(const char* s) const;
  bool startWith(const String& s) const;
  
  bool endOf(const char* s) const;
  bool endOf(const String& s) const;
  ```

 ![image-20201218162434733](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201218162434733.png)

* **在指定位置处插入字符串**
  * `String& insert(int i, const char* s);`
  * `String& insert(int i, const String& s);`

 ![image-20201218162757182](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201218162757182.png)

* **去掉字符串两端的空白字符**
  * `String& trim();`

 ![image-20201218162854617](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201218162854617.png)



# 41.KMP子串查找算法

#### 基础知识

* **匹配失败时的右移位数与子串本身相关**，与目标串无关
* **移动位数** = 已匹配的字符数 - **对应的部分匹配值**
* 任意子串都存在一个唯一的部分匹配表

 ![image-20201221114011400](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201221114011400.png)

* **前缀**
  * 除了**最后一个字符以外**，一个字符串的全部头部组合
* **后缀**
  * 除了**第一个字符以外**，一个字符串的全部尾部组合
* **部分匹配值**
  * **前缀和后缀最长共有元素的长度**

 ![image-20201221114225838](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201221114225838.png)

#### PMT编程实现关键

**ll ：longest length（前后缀交集元素的最长长度）**

* PMT[0] = 0
* 从2个字符开始**递推**（从下标为 1 的字符开始递推）
* 当前可选**ll**值为0时，直接比对首尾元素，比对成功**ll** + 1
* 将以**ll**值为下标的值作为**扩展字符**与**尾部扩展字符**比对，**比对成功ll + 1**
* 若**比对失败**，**ll = PMT[ll - 1]**，继续以**ll**个种子进行扩展比对，直到**ll**为0

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201221113849930.png" alt="image-20201221113849930" style="zoom: 50%;" />

```C++
int* make_pmt(const char* p)
{
    int plen = strlen(p);
    int ll = 0;
    int* pmt = static_cast<int*>(malloc(sizeof(int) * plen));

    if(pmt)
    {
        pmt[0] = 0;

        for(int i=1; i<plen; i++)
        {
            while( (ll > 0) && (p[ll] != p[i]) )
            {
                ll = pmt[ll - 1];
            }

            if(p[ll] == p[i])
            {
                ll++;
            }

            pmt[i] = ll;
        }
    }

    return pmt;
}
```

#### KMP

 ![image-20201221205801357](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201221205801357.png)

<font color=#FF0000 >因为，s[i] != p[j] **—>** 查表，LL = PMT[j - 1] **—>** p右移，i的值不变，j的值改变，**j回溯，j = PMT[j - 1]**</font>

```c++
int kmp(const char* s, const char* p)
{
    int ret = -1;
    int slen = strlen(s);
    int plen = strlen(p);
    int* pmt = make_pmt(p);

    if( (pmt) && (plen <= slen) && (plen > 0) )
    {
        for(int i=0, j=0; i<slen; i++)
        {
            while( (j > 0) && (s[i] != p[j]) )
            {
                j = pmt[j - 1];
            }

            if(s[i] == p[j])
            {
                j++;
            }

            if(j == plen)
            {
                ret = i - plen + 1;
                break;
            }
        }
    }

    free(pmt);

    return ret;
}
```



# 42.KMP算法的应用

#### 字符串类中的新功能

| 成员函数       | 功能描述                  |
| -------------- | :------------------------ |
| indexOf(s)     | 查找子串s在字符串中的位置 |
| remove(s)      | 将字符串中的子串s删除     |
| operator - (s) | 定义字符串减法            |
| replace(s, t)  | 将字符串中的子串s替换为t  |
| sub(i, len)    | 从字符串中创建子串        |

* **子串查找(KMP)**

  ```c++
  int indexOf(const char* s) const;
  int indexOf(const String& s) const;
  ```

* **在字符串中将指定的子串删除**

  ```c++
  String& remove(int i, int len);
  String& remove(const char* s);
  String& remove(const String& s);
  ```

  1. **根据KMP在目标字符串中查找子串的位置**
  2. **通过子串位置和子串长度进行删除**

* **字符串的减法操作定义（operator - ）**

  * 使用remove实现字符串间的减法操作
    * 字符串自身不被修改
    * 返回产生的新串

  ```c++
  String s1 = "abcde";
  String s2 = s1 - "bcd";
  
  cout << s1.str() << endl; // abcde
  cout << s2.str() << endl; // ae
  ```

* **字符串中的子串替换**

  ```c++
  String& replace(const char* t, const char* s);
  String& replace(const String& t, const char* s);
  String& replace(const char* t, const String& s);
  String& replace(const String& t, const String& s);
  ```

* **从字符串中创建子串**

  ```c++
  String sub(int i, int len) const
  ```

  * 以**i**为起点提取长度为len的子串
  * **子串提取不会改变字符串本身的状态**

  ```c++
  String s1 = "abcde";
  String s2 = s1.sub(1, 3);
  
  cout << s1.str() << endl; // abcde
  cout << s2.str() << endl; // bcd
  ```



# 43.递归的思想与应用（上）

递归是一种数学上**分而自治**的思想

* **将原问题分解为规模较小的问题进行处理（建立递归模型）**
  * 分解后的问题与原问题的类型完全相同，但规模较小
  * 通过小规模问题的解，能够轻易求得原问题的解
* **问题的分解是有限的（递归不能无限进行）**
  * 当边界条件不满足时，分解问题（递归继续进行）
  * 当边界条件满足时，直接求解（递归结束）

**递归一般模型**：

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210104160951068.png" alt="image-20210104160951068" style="zoom:80%;" />

#### 递归在程序设计中的应用

递归函数

* 函数体中存在**自我调用的函数**
* 递归函数**必须有递归出口（边界条件）**
* 函数的**无限递归将导致程序崩溃**

#### 递归思想的应用

1.求解`Sum(n) = 1 + 2 + 3 + ... + n`

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210104161338072.png" alt="image-20210104161338072" style="zoom:80%;" />

2.斐波拉契数列

数列自身递归定义：1，1，2，3，5，8，13，21，...

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210104161452321.png" alt="image-20210104161452321" style="zoom:80%;" />

3.递归方法编写函数求字符串长度

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210104161527360.png" alt="image-20210104161527360" style="zoom:80%;" />



# 43.递归的思想与应用（中）

#### 单向链表的转置

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210104163923095.png" alt="image-20210104163923095" style="zoom:80%;" />

#### 单向排序链表的合并

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210104164052806.png" alt="image-20210104164052806" style="zoom:80%;" />

#### 汉诺塔

* 将n-1个木块借助C柱由A柱移动到B柱
* 将最底层的唯一木块直接移动到C柱
* 将n-1个木块借助A柱由B柱移动到C柱

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210104164410128.png" alt="image-20210104164410128" style="zoom:80%;" />

#### 全排列

 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210104164444870.png" alt="image-20210104164444870" style="zoom:80%;" />



# 45.递归的思想与应用（下）

