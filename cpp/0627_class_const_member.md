# 不允许类内初始化非常量静态成员

* 代码说明：class里member想要**用static并付初值**必须用常量声明式（const）

```c++
class TT
{
    private:
        int a = 5;  //可以，类内一般member
        const int b = 5;    //可以，类内const member
        //static int c = 5;   /*错误，报错：error: ISO C++ forbids 
        //                    in-class initialization of non-const static member 'TT::c'*/
        const static int d = 5; //可以，静态member用const限定
        static int e;   //可以，只是声明
    public:
        int prte(){return e;}
};
int TT::e = 5;//使用的时候再赋值
```

* 为啥要这样干？

  给class一个专属常量（将常量的作用域限定在class内）并保证此常量最多只有一份实体

  

* 为什么不允许？

  最通俗的解释：静态成员属于整个类，而不属于某个对象，如果在类内初始化，会导致每个对象都包含该静态成员，这是矛盾的。

  理解：一个.h头文件通常被多个翻译单元（简单理解为.cpp）所include。c++在链接时要求每个对象都有唯一的定义。然而静态成员不单独属与某个对象，破坏了这个规则

  > 延申：关于linker：
  >
  > 看这篇博文：帮 C/C++ 程序员彻底了解链接器
  >
  > https://www.lurklurk.org/linkers/linkers.html
  > https://blog.csdn.net/oncealong/article/details/50384538

* const 的作用：

  c:

  const修饰的全局变量，在常量区（.rodata段）分配内存空间，不能通过变量地址来修改值；const修饰的局部变量在栈区分配内存空间，可以通过变量地址来修改值；

  

  c++:

  局不局部都不能修改，有种情况不会报修改read_only错
  
  ```c++
  int main()
  {
      const int a = 100;
      int p  = (int )&a;
      p  = 200;
      cout<<a<<endl;//100
      return 0;
  }
  ```
  
  理解：c++里const修饰后就是个常量了



* 影响范围：整数类型

  Types bool, char, wchar_t, and the signed and unsigned integer types 

