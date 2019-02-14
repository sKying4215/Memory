
```C++
//  Created by 石迎 on 2019/2/14.
//  Copyright © 2019年 石迎. All rights reserved.
//

#include <iostream>
using namespace std;

class person{
public:
    person(){
        cout<<"基类构造函数运行中.....\n";
    }
    ~person(){
        cout<<"基类析构函数运行中.....\n";
    }
};
class DS:public person{
public:
    DS(){
        cout<<"派生类构造函数运行中.....\n";
    }
    ~DS(){
        cout<<"派生类析构函数运行中.....\n";
    }
};

int main(int argc, const char * argv[]) {
    DS p;
    return 0;
}
输出：
基类构造函数运行中.....
派生类构造函数运行中.....
派生类析构函数运行中.....
基类析构函数运行中.....
实例化派生类DS一个对象p 先调用基类构造-派生类构造-派生类析构-基类析构。没有问题 

*********假设new了。却没delete。******
int main(int argc, const char * argv[]) {
    DS *p = new DS();
    return 0;
}
输出：
基类构造函数运行中.....
派生类构造函数运行中.....
此时会造成内存泄漏，因为new了  但是没有delete 程序在执行的过程中不会调用析构函数，直到程序结束才会释放，所以造成了泄漏。
int main(int argc, const char * argv[]) {
    DS *p = new DS();
    delete p;
    return 0;
}
输出：
基类构造函数运行中.....
派生类构造函数运行中.....
派生类析构函数运行中.....
基类析构函数运行中.....

此时正常。

***********基类指针仅仅指向了派生类继承自己的部分，只释放了基类，派生类析构未调用，此时同样会造成内存泄漏************
int main(int argc, const char * argv[]) {
    person *p = new DS();
    delete p;
    return 0;
}
输出：
基类构造函数运行中.....
派生类构造函数运行中.....
基类析构函数运行中.....
解决方案：
在基类的析构函数增加virtual,此时在基类指针调用析构时候，因为析构函数是虚函数，所以会先调用派生类的析构，再去调用基类析构。
class person{
public:
    person(){
        cout<<"基类构造函数运行中.....\n";
    }
    virtual ~person(){
        cout<<"基类析构函数运行中.....\n";
    }
};
输出：
基类构造函数运行中.....
派生类构造函数运行中.....
派生类析构函数运行中.....
基类析构函数运行中.....

****************深拷贝  浅拷贝**************
    
