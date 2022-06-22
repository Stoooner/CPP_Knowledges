# 泛型编程Template
## 1. 函数模板
### 1.1 模板的概念和基本语法
#### 1.1.1 基本概念内容
>**1. 模板的概念：** 模板就是建立通用的模具，大大提高复用性，将类型参数话；
>**2. 函数模板：** C++另一种编程思想称为泛型编程，主要利用的技术就是模板，C++提供两种模板机制：函数模板和类模板，其中函数模板的语法如下：
>- **作用：** 建立一个通用函数，其函数返回值类型和形参类型可以不具体制定，用一个虚拟的类型来代表；
>- **语法：** `template<typename T> 函数声明或者定义`；
>- **解释：** **template --> 声明创建模板；typename --> 表明其后面的符号是一种数据类型，可以用class关键字代替，其中typename是新标准规定的，而class是为了向后兼容才保留的；T --> 通用的数据类型，名称可以替换，通常为大写字母；**
>- **注意：** 1. 自动类型推导，必须推导出一致的数据类型T，才可以使用；2. 模板必须要确定出T的数据类型，才可以使用；

#### 1.1.2 示例代码
```c++
#include <iostream>
using namespace std;
#include <unistd.h>

/*
    本阶段主要针对C++泛型编程和STL技术做详细讲解，讨论C++更深层的使用
    1. 模板
    1.1 模板的概念
        模板就是建立通用的模具，大大提高复用性，将类型参数化
    1.2 函数模板
        C++另一种编程思想称为泛型编程，主要利用的技术就是模板
        C++提供两种模板机制：函数模板和类模板
        1.2.1 函数模板语法
            - 函数模板的作用：建立一个通用函数，其函数返回值类型和形参类型可以不具体制定，用一个虚拟的类型来代表
            - 语法：template<typename T>
                   函数声明或者定义
            - 解释：template --> 声明创建模板
                   typename --> 表明其后面的符号是一种数据类型，可以用class代替，其中typename是新标准规定的，而class是为了向后兼容才保留的
                   T --> 通用的数据类型，名称可以替换，通常为大写字母
            - 注意：
                1. 自动类型推导，必须推导出一致的数据类型T，才可以使用
                2. 模板必须要确定出T的数据类型，才可以使用
*/

// 函数模板
// 两个整型交换的函数，如果要交换例如double时就得重新去写新的函数
void swapInt(int &a, int &b){
    int temp = a;
    a = b;
    b = a;
}

// 函数模板
// ？纯虚函数的参数定死了，模板的方法定死了，两个制约条件相反自然不能代替
template<typename T> // 声明一个模板，告诉编译器后面代码中紧跟着的T不要报错，T是一个通用数据类型
void mySwap(T &a, T &b){
    T temp = a;
    a = b;
    b = temp;
}

// 模板必须要确定出T的数据类型，才可以使用
template<typename T>
void func(){
    cout << "func调用" << endl;
}

void test01(){
    int a = 10;
    int b = 20;
    char c = 'c';

    // func();报错，没有确定T的数据类型
    func<int>(); // 正确，确定了T的数据类型

    // 利用函数模板进行交换
    // 两种方式使用函数模板
    // 1. 自动类型推导：自动类型推导的前提是传入的参数必须能推导出是一致的数据类型T，才可以使用
    mySwap(a, b); //正确案例
    // 错误案例：mySwap(a, c); 推导不出一致的类型T
    cout << "a = " << a << ", b = " << b << endl;
    // 2. 显示指定类型
    mySwap<int>(a, b);
    cout << "a = " << a << ", b = " << b << endl;
}

int main(){
    test01();
    pause();
    return 0;
}
```

### 1.2 普通函数与函数模板的区别
#### 1.2.1 基本概念内容
>**普通函数和函数模板的区别：**
>- 普通函数调用时可以发生自动类型转换(隐式类型转换)；
>- 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换；
>- 如果利用显示指定类型的方式，可以发生隐式类型转换；

#### 1.2.2 示例代码
```c++
#include <iostream>
using namespace std;
#include <unistd.h>

/*
    普通函数和函数模板的区别：
        - 普通函数调用时可以发生自动类型转换(隐式类型转换)
        - 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换
        - 如果利用显示指定类型的方式，可以发生隐式类型转换
*/
// 普通函数
int myAdd01(int a, int b){
    return a+b;
}

template<typename T>
T myAdd02(T a, T b){
    return a + b;
}

void test01(){
    int a = 10;
    int b = 20;
    char c = 'c';
    // 普通函数调用时可以发生自动类型转换(隐式类型转换)
    // 这里将字符型隐式的转换为了整型
    cout << myAdd01(a, c) << endl;

    // 1. 模板中的自动类型推导(函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换)，因此下面这句会报错，因为既可以将a的值转换为字符型，也可以将c的类型转换为整型，因此它无法正确推导出类型
    // cout << myAdd02(a, c) << endl;

    // 2. 显示指定类型(如果利用显示指定类型的方式，可以发生隐式类型转换)
    cout << myAdd02<int>(a, c) << endl;
}

int main(){
    test01();
    pause();
    return 0;
}
```

### 1.3 普通函数与函数模板的调用规则
#### 1.3.1 基本概念内容
>**普通函数与函数模板的调用规则：**
>- 如果函数模板和普通函数都可以调用，优先调用普通函数；
>- 可以通过空模板参数列表来强制调用函数模板；
>- 函数模板也可以发生重载；
>- 如果函数模板可以产生更好的匹配，优先调用函数模板；
>- 因此既然提供了函数模板了，那么最好就不要提供普通函数了，否则容易产生二义性；

#### 1.3.2 示例代码
```c++
#include <iostream>
using namespace std;
#include <unistd.h>

/*
    普通函数与函数模板的调用规则：
        - 如果函数模板和普通函数都可以调用，优先调用普通函数；
        - 可以通过空模板参数列表来强制调用函数模板；
        - 函数模板也可以发生重载；
        - 如果函数模板可以产生更好的匹配，优先调用函数模板
        - 因此既然提供了函数模板了，那么最好就不要提供普通函数了，否则容易产生二义性；
*/

void myprint(int a, int b){
    cout << "调用的是普通函数" << endl;
}

template<typename T>
void myprint(T a, T b){
    cout << "调用的是模板" << endl;
}

template<typename T>
void myprint(T a, T b, T c){
    cout << "调用的是重载了的模板" << endl;
}

void test01(){
    int a = 10;
    int b = 20;
    int c = 30;

    // 1. 如果函数模板和普通函数都可以实现，优先调用普通函数；
    // myprint(10, 20); 

    // 2. 可以通过空模板参数列表来强制调用函数模板；
    // myprint<>(a, b);

    // 3. 函数模板也可以发生重载
    // myprint(a, b, c);

    // 4. 如果函数模板可以产生更好的匹配，优先调用函数模板
    char c1 = 'a';
    char c2 = 'b';
    // 对于这样的调用，调用普通函数或者模板都能实现，因为普通函数调用时可以发生自动类型转换(隐式类型转换)
    // 但是实际结果是调用的模板，是因为：调用普通函数的话还要对数据做一次转换
    // 但是对于模板而言的话直接将T类型推导为char即可
    myprint(c1, c2);

}

int main(){
    test01();
    pause();
    return 0;
}
```


### 1.4 模板的局限性
#### 1.4.1 基本概念内容
>**模板的局限性：**
>- 模板的通用性并不是万能的， 针对诸如自定义数据类型模板无法正常运行的情况，C++提供了模板的重载，可以为这些特定的类型提供具体化的模板。学习模板并不是为了写模板，而是为了在STL(Standard Template Library)中能够运用系统提供的模板；

#### 1.4.2 示例代码
```c++
#include <iostream>
using namespace std;
#include <unistd.h>
#include <string>

/*
    模板的局限性：模板的通用性并不是万能的
    针对诸如自定义数据类型模板无法正常运行的情况，C++提供了模板的重载，可以为
    这些特定的类型提供具体话的模板
    学习模板并不是为了写模板，而是为了在STL(Standard Template Library)中能够运用系统提供的模板
*/

class Person{
public:
    Person(string name, int age){
        this->m_Name = name;
        this->m_Age = age;
    }
    // 姓名
    string m_Name;
    // 年龄
    int m_Age;
};

// 对比两个数据是否相等的函数
template<typename T>
bool myCompare(T &a, T &b){
    if (a == b){
        return true;
    }else{
        return false;
    }
}

// 利用具体化的Person的版本实现代码，具体化会被优先调用
// 如果不加template<>则变成了普通函数优先调用
// 加上template<>使得这个函数变成了模板的重载版本
// 如果写普通函数只能处理Person，这个函数模板可以处理包含Person在内的所有数据类型
// 写这个Person不是专门处理Person，是对已有模板bool myCompare(T &a, T &b)进行优化，加入Person的比较
template<> bool myCompare(Person &p1, Person &p2){
    if (p1.m_Name == p2.m_Name && p1.m_Age == p2.m_Age){
        return true;
    }else{
        return false;
    }
}

void test01(){
    int a = 10;
    int b = 20;
    bool ret = myCompare(a, b);
    if (ret){
        cout << "a == b " << endl;
    }else{
        cout << "a != b " << endl;
    }
}

void test02(){
    Person p1("Tom", 10);
    Person p2("Tom", 10);
    bool ret = myCompare(p1, p2);
    if (ret){
        cout << "p1 == p2 " << endl;
    }else{
        cout << "p1 != p2 " << endl;
    }
}

int main(){
    // test01();
    test02();
    pause();
    return 0;
}
```


## 2. 类模板
### 2.1 类模板的基本语法
>pass

### 2.2 类模板与函数模板的区别
>pass

### 2.3 类模板中成员函数创建时机
>pass

### 2.4 类模板函数对象做函数参数
>pass

### 2.5 类模板与继承
>pass

### 2.6 类模板成员函数类外实现
>pass

### 2.7 类模板分文件编写
>pass

### 2.8 类模板与友元
>pass