[TOC]
#C++知识
- 当使用默认形参和函数重载时要避免出现二义性；
- 友元关系不能传递，也不能继承；
### 1.private ，public， protect
#### a. C++中的继承方式(派生访问说明符)： 
public、private、protected 三种,class是默认是private继承，struct默认public
- public（公有继承）：继承时保持基类中各成员属性不变。基类中private成员被隐藏，派生类函数和对象只能通过基类的公有成员函数访问。派生类的成员只能访问基类中的public/proted成员，而不能访问private成员；派生类的对象只能访问基类中的public成员。 
- private（私有继承）：继承时基类中各成员属性均变为private，并且基类中private成员被隐藏。派生类的成员也只能访问基类中的public/protected成员，而不能访问private成员；派生类的对象不能访问基类中的任何的成员。  
- protected（保护性继承）：继承时基类中public 和 protected 成员属性均变为protected，private 仍为private。基类中private成员被隐藏。派生类的成员只能访问基类中的public/protected成员，而不能访问private成员；派生类的对象不能访问基类中的任何的成员。

不管是什么继承，派生类的对象和成员函数都不能直接访问基类的私有成员，只能通过基类的公有函数和成员访问。**继承方式说明符是为了限制派生类用户（即派生类对象）对基类成员的访问权限。**
#### b. 类的数据成员类型(基类中成员访问说明符)
- public：类的对象和成员均可以访问，public继承的派生类对象和成员均可以访问
- private：只能通过类内的成员进行访问，派生类的对象和成员均不可以访问
- protected：派生类成员或者友元是可以访问的，但是对于类的用户(对象)来说是不可访问的。**派生类的成员和友元只能访问派生类对象中的基类部分的受保护的成员，对于普通的基类对象中的保护成员不具有特殊的访问权限**
例子：
  ```
  class Base
   {
     protected:   
           int prot_mem;     
                 
   }
  class Sneaky :public Base
  {
  friend void clobber(Sneaky &);
  friend void clobber(Base &);
  int j;
  }
   friend void clobber(Sneaky &s)  //正确
   {
      s.j=s.prot_mem=0;
   }
      friend void clobber(Base &b)  //错误
   {
     b.prot_mem=0;
   }
  ```

只有在派生类中protected和private才有区别，对于自己的类的成员，他俩之间没有任何区别。
### 2.继承中构造、析构和拷贝的调用规则
数据抽象  继承   和动态绑定
- 在对象之间不存在类型转换
-  类中的静态成员在整个继承体系中只存在该成员的唯一定义 
- 在类名字后面加上final，可以防止继承的发生，例如：
 ```
   class NoDerived final  {/*    */}  //该类不能被其他类继承
 ```
  
 
### 3.静态类型和动态类型
1.  一个对象（变量）的静态类型就是其声明类型，如表达式int a中的int就是对象a的声明类型，即静态类型；而一个对象（变量）的动态类型就是指程序执行过程中对象（指针或引用）实际所指对象的类型。表达式或对象的静态类型在编译时已知，动态类型则在程序运行时才可以确定。
2. 只有当我们的表达式使用的是指针或引用时对象的静态类型和动态类型才有可能不同，其他都相同。
3. 而且就算一个对象的静态类型和动态类型不相等，其还不一定能享受到动态类型所带来的方便（即动态绑定），因为我们知道，动态绑定的前提条件是继承体系中的子类继承并重写了基类的虚函数，而且只有通过指针或引用调用虚函数时才会发生。


### 4.虚函数
- 任何构造函数之外的非静态函数都可以是虚函数，关键字virtual只能出现在类内部的声明语句之前而不能用于类外部的函数定义。
- 如果某个函数没声明为virtual，则其解析过程发生在 **运行** 时而非编译时。
- 如果对虚函数的某次调用使用了默认实参，则该实参值由本次调用的静态类型决定。
- 可以使用关键字override来说明派生类中某个函数明确覆盖掉基类中某个函数，如果该函数并没有覆盖掉的话，编译器将报错。
- 可以使用关键字final 明确指出基类中某个函数不允许后续的其他类覆盖。
### 5.构造函数与拷贝控制
#### 5.1虚析构函数
当我们delete一个动态分配的对象的指针时将执行析构函数，如果该指针指向继承体系中的某个类型，则有可能出现指针的静态类型与被删除对象的动态类型不符的情况。例如：
```
class Quote
{
//如果我们删除的是一个指向派生类对象的基类指针，则需要虚析构函数
virtual  ~Quote（）=default；   //动态绑定析构函数
}
```
和其他虚函数一样，析构函数的虚属性也会被继承。无论Quote的派生类使用合成的析构函数还是定义自己的析构函数，都将是虚析构函数。
```
Quote *itemP =new Quote； //静态类型和动态类型一致
delete itemP；            //调用Quote的析构函数
itemP==new Bulk_quote；   //静态类型和动态类型不一致
delete itemP；            //调用Bklk_quote的析构函数
```
我们可以有以下结论：如果基类的析构函数不是虚函数，则delete一个指向派生类对象的基类指针将产生未定义的行为。
另外，当一个类的析构函数是虚函数时，即使它=通过=default的形式使用了合成的版本，编译器也不会为这个类合成移动操作。

#Effective  C++ 笔记

**说明：**写这个笔记主要是记录自己在看Effective C++和primer时候自己不了解的东西


###1.条款05  了解C++默认编写并调用哪些函数
如果打算在一个“内含reference  或者 const 的成员” 的class内支持赋值操作，必须自己定义copy  assignment 操作符，因为：`C++不允许“让 reference 改指不同的对象",也不允许改变const类型的变量的值`。

###2.条款06 若不想使用编译器自动生成的函数，就应该明确拒绝(定义一个不能被拷贝的类)
当我们想让某个类的对象只有唯一一个的时候，可以把拷贝构造函数和拷贝赋值操作符生命为private，并且不去定义其实现，这样的话当我们尝试复制某个类的对象时，编译器就会报错。如果是在member函数或者friend函数之内尝试复制某个对象时，连接器会报错。典型的例子就是标准程序库实现代码中的ios_base,basic_ios和sentry等。
另外，还有一个方法就是，设计一个base class，把base class 的拷贝构造和拷贝赋值声明为private，然后让不可复制的类继承这个基类即可。
- 为驳回编译器自动提供的机能，可将相应的成员函数声明为private并且不予实现。使用像Uncopyable这样的base class 也是一种做法。
###3. 条款33  避免遮掩继承而来的名称
- derived class内的名称会遮掩base class内的名称。在public继承下，从来没有人希望如此。
- 为了让被遮掩的名称再见天日，可使用 using 声明式(或作用域运算符)或转交函数。
例子：
 ```
  class Base
   {
      private: 
           int  x;
       public:
            virtual void mf1()=0;
            virtual void mf1(int);
            virtual void mf2();
            void mf3();
            void mf3(double);
            .......
   }
   class Derived : public Base
   {
   public：
         //using  Base::mf1;       //using 声明式 ，当有这两句话时下面的测试程序均正确。此时的mf1在Derived中是公有的
         //using Base::mf3;
         virtual void mf1();
         void mf3();
         void mf4();
          .......
   }
 //使用的时候的代码如下：
   Derived d;
   int x;
   ...
   d.mf1();         // 没问题,调用Derived::mf1;
   d.mf1(x);        //错误！因为Derived::mf1遮掩了Base::mf1
   d.mf2();         //正确 ，调用Base::mf2;   
   d.mf3();         //正确 ，调用Derived::mf3;
   d.mf3(x);        //错误，Derived::mf3遮掩了Base::mf3
   
 ```
转交函数的用法在这里不再写了，需要的时候可以查看Effective C++ 条款33，或者Primer的P547

 
