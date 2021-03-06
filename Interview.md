[TOC]

## Java

### 面向对象的特征有哪些方面？

**抽象**

将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象两方面。

抽象只关注对象有哪些属性和行为，并不关注这些行为的细节是什么。

**继承**

从已有类得到继承信息，转而创建新类的过程。

提供继承信息的类被称为父类，得到继承信息的类被称为子类。

**封装**

把数据和操作数据的方法绑定起来，从而对数据的访问只能通过已定义的接口。

面向对象的本质是将现实世界描绘成一系列完全自治且封闭的对象。

在类中编写的方法就是对实现细节的一种封装，编写一个类就是对数据和数据操作的封装。

可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口。

**多态性**

允许不同子类型的对象对同一消息作出不同的响应。简单地说，就是用同样的对象调用同样的方法但是做了不同的事情。

多态性分为编译时的多态性和运行时的多态性。

方法重载实现的是编译时的多态性（也称为前绑定），方法重写实现的是运行时的多态性（也称为后绑定）。

运行时的多态性是面向对象最精髓的东西，需要做两件事：方法重写（子类继承父类并重写父类中已有的或抽象的方法），对象造型（用父类型引用引用子类型对象，这样，同样的引用调用同样的方法就会根据子类对象的不同而表现出不同的行为）



### 访问修饰符 public、private、protected 以及不写（默认）时的区别？

类的成员不写访问修饰时，默认为 default。对于同一个包中的其他类相当于公开（public），不是同一个包中的其他类相当于私有（private）。

受保护（protected）对子类相当于公开，对不是同一包中的没有父子关系的类相当于私有。

外部类的修饰符只能是 public 或默认，类的成员（包括内部类）的修饰符可以是以上四种。



### String是最基本的数据类型吗？

不是。

基本数据类型只有 8 个：byte、short、int、long、float、double、char、boolean。

除了基本类型，剩下的都是引用类型，枚举类型也是引用类型。



### Java 有没有 goto？

goto 是 Java 中的保留字，在目前版本的 Java 中没有使用。

James Gosling（Java 之父）编写的《The Java Programming Language》的附录中给出了一个Java关键字列表，其中有 goto 和 const，但是这两个是目前无法使用的关键字，因此有些地方将其称之为保留字。其实保留字这个词应该有更广泛的意义，因为熟悉 C 语言的程序员都知道，在系统类库中使用过的有特殊意义的单词或单词的组合都被视为保留字。



### int 和  Integer 有什么区别？

Java 是一个近乎纯洁的面向对象编程语言，但是为了编程的方便，还是引入了基本数据类型。但是为了能够将这些基本数据类型当成对象操作，Java 为每一个基本数据类型都引入了对应的包装类型。int 的包装类就是 Integer，从 Java 5 开始引入了自动装箱/拆箱机制，使得两者可以相互转换。



### & 和 && 的区别？

& 运算符有两种用法：（1）按位与；（2）逻辑与。

&& 运算符是短路与运算。

逻辑与和短路与的差别是非常巨大的，虽然两者都要求运算符左右两端的布尔值都是 true，整个表达式的值才是 true。&& 之所以称为短路运算是因为，如果 && 左边的表达式的值是 false，右边的表达式会被直接短路掉，不会进行运算，

逻辑或运算符（|）和短路或运算符（||）的差别也是如此。



### 解释内存中的栈、堆和静态区的用法

通常，定义基本数据类型的变量、对象的引用以及函数调用的现场保存都使用内存中的栈空间。通过 new 关键字和构造器创建的对象放在堆空间。程序中的字面量，如直接书写的100、“hello” 和常量都放在静态区中。

栈空间操作起来最快，但是栈很小。通常大量的对象都放在堆空间，理论上整个内存没有被其他进程使用的空间甚至硬盘上的虚拟内存都可以被当成堆空间来使用。



### 数组有没有 length() 方法？String 有没有 length() 方法？

数组没有 length() 方法，有 length 属性。String 有 length() 方法。



### 在 Java 中，如何跳出当前的多重嵌套循环？

在最外层循环前加一个标记如 A，然后用 break A，可以跳出多重循环。



### 是否可以继承 String 类？

String 类是 final 类，不可以被继承。



### 当一个对象被当作参数传递到一个方法后，此方法可改变这个对象的属性，并可返回变化后的结果，那么这里到底是值传递还是引用传递？

值传递。

Java 的方法调用只支持参数的值传递。当一个对象实例作为一个参数被传递到方法中时，参数的值就是对该对象的引用。对象的属性可以在被调用过程中被改变，但对对象引用的改变是不会影响到调用者的。



### 重载和重写的区别。重载的方法能否根据返回类型进行区分？

方法的重载和重写都是实现多态的方式，区别在于前者实现的是编译时的多态性，而后者实现的是运行时的多态性。重载发生在一个类中，同名的方法如果有不同的参数列表（参数类型不同、参数个数不同或两者都不同）则视为重载。重写发生在父类与子类之间，要求子类被重写方法与父类被重写方法有相同的返回类型，比父类被重写方法更好访问，不能比父类被重写方法声明更多的异常。

重载对返回类型没有特殊的要求。



### 观察者模式

观察者模式是对象的行为模式。

它定义了一种一对多的依赖关系，让多个观察者同时监听某一主题对象。当主题对象状态发生变化，会通知所有的观察者对象，使它们能够自动更新自己。

**优点**：观察者模式在被观察者和观察者之间建立了一个抽象的耦合。被观察者角色所知道的只是一个具体观察者列表，每一个具体观察者都符合一个抽象观察者的接口。由于被观察者和观察者没有紧密地耦合在一起，因此，它们可以属于不同的抽象化层次。

观察者模式支持广播通讯，被观察者会向所有的登记的观察者发出通知。

**缺点：**如果一个被观察者对象有很多直接和间接的观察者，将所有的观察者都通知到会花费很多时间。

如果在被观察者间有循环依赖的话，会触发它们之间的循环调用，导致系统崩溃。

虽然观察者模式可以随时使观察者知道所观察的对象发生了变化，但是没有相应的机制使观察者知道所观察的对象是怎么发生变化的。



### 工厂模式

定义一个用于创建对象的接口，让子类决定实例化哪一个类，即工厂模式使一个类的实例化延迟到其子类。

**优点：**具有良好的封装性，代码结构清晰，利于扩展。在增加产品类的情况下，只需要适当地修改或扩展具体的工厂类。

可以屏蔽产品类。产品类的实现如何变化，调用者都不用关心，只需要了解产品的接口。只要接口保持不变，系统的上层模块就不需要发生变化。







## C++

### 指针（*）、引用（&）、解引用（\*）、取地址（&）的概念和区别

指针指向一块内存，保存的是内存的地址；引用是变量的别名，本质是引用该变量的地址；解引用是取指针指向的地址的内容；取地址是获得变量在内存中的地址。

引用使用无需解引用，指针需解引用。

引用不能为空，指针可以为空。

引用在定义时被初始化一次，之后不可变；指针指向的值和本身的值是可变的，也就是说指针只是一块地址，地址里的东西可变。

程序会给指针变量分配内存区域，而引用不需要分配内存区域。



### struct 和 class 的区别，struct 和 union 的区别

struct 和 class 都是声明类的关键字，区别是：默认情况下，struct 的成员变量是公共的，class 的成员变量是私有的；struct 保证成员按照声明顺序在内存中存储，class 不能保证；对于继承来说，class 默认是 private 继承，struct 默认是 public 继承。

struct 和 union 的区别是：struct 和 union 都由多个不同的数据类型成员组成，但union 类型的变量，所有成员变量共享一块内存，该内存的大小由这些成员变量中长度最大的一个决定，struct 中成员变量内存都是独立的；union 分配的内存是连续的，而 struct 不能保证分配的内存是连续的。



### 如何引用一个已经定义过的全局变量，区别是什么？

如果在同一个文件中，直接引用即可；如果不在同一个文件，有两种方式：直接引用头文件或用 extern 关键字重新声明一下。



### 全局变量可不可以定义在可被多个 .c 文件包含的头文件中？

全局变量的作用域是整个源程序，可以声明多次，但是只能定义一次。

在实际的编程中，一般很少在头文件中定义全局变量，因为多次引用可能重定义。



### 对于一个频繁使用的短小函数，在 C 语言中应用什么实现，在 C++ 中呢？

C 用宏定义，C++ 用 inline。



### main 函数执行以前，会执行什么代码？

全局对象的构造函数会在 main 函数之前执行。比如 int a; 初始化为 0。



### 局部变量能否和全局变量重名？

可以，但是局部变量会屏蔽全局变量。如果要使用全局变量，需要域作用符 “::”。



### 描述内存分配方式及它们的区别？

从静态存储区域分配。该存储区域在程序编译的时候就已经分配好了，这块内存在程序的整个运行期间都存在。例如全局变量，static 变量。

在栈上创建。执行函数时，局部变量存储在该区域，执行结束时会释放该存储空间。

从堆上分配，也称为动态内存分配。例如，程序在运行的时候用 malloc 或 new 申请内存，程序员自己负责用 free 或 delete 释放内存。动态内存的生存期由程序员决定，使用非常灵活。



### 类的成员函数重载、覆盖和隐藏的概念和区别？

重载是指在同一个作用域内，有几个同名的函数，但是参数列表的个数和类型不同。

覆盖是指派生类函数覆盖基类函数，函数名、参数类型和返回值类型一模一样。派生类的对象会调用子类中的覆盖版本，覆盖父类中的函数版本。

隐藏是指派生类的函数屏蔽了与其同名的基类函数。

覆盖和重载的区别：函数是否处在不同的作用域，参数列表是否一样，基类函数是否有 virtual 关键字。

隐藏和覆盖的区别：派生类的函数与基类的函数同名，但是参数不同。此时，不论有无 virtual 关键字，基类的函数将被隐藏；派生类的函数与基类的函数同名，参数也相同，但是基类函数没有 virtual 关键字，此时，基类的函数被隐藏，如果有 virtual，就是覆盖。



### static 关键字

编译器为局部变量在栈上分配空间，但是函数执行结束时会释放掉。所以，static 可以解决函数执行完后变量值被释放的问题，static 变量存储在静态存储区。

缺点：不安全，因为作用域是全局的。

优点：因为静态变量是所有对象公有的，可以节省内存，提高时间效率。

静态数据成员使用方式：<类名>::<静态成员名>

作用：函数体内 static 变量的作用范围为该函数体，不同于 auto 变量，该变量的内存只被分配一次，因此其值在下次调用时仍维持上次的结果；在模块内的 static 全局变量可以被模块内函数访问，但不能被模块外其他函数访问；在模块内的 static 函数只可被这一模块内的其他函数调用，其使用范围被限制在声明它的模块内；在类中的static 成员变量属于整个类所拥有，对类的所有对象有一份拷贝；在类中的 static 成员函数属于整个类所拥有，这个函数不接收 this 指针，因而只能访问类的 static 成员变量。



### const 和 #define 的概念和优缺点？

const 用来定义常量、修饰函数参数、修饰函数返回值，可以避免被更改，提高程序的而健壮性。

define 是宏定义，在编译的时候会进行替换，可以避免没有意义的数字或字符串，便于程序的阅读。

区别：const 定义的数据有数据类型，而宏常量没有数据类型，编译器可以对 const 常量进行类型检查，而对宏定义只进行字符替换，没有类型安全检查，所以可能出错。

const 关键字的作用：欲阻止一个变量被改变，可以使用，在定义该 const 变量时，通常需要对它进行初始化，因为以后就没有机会再去改变它了；对指针来说，可以指定指针本身为 const，也可以指定指针所指的数据为 const，或者两者同时指定为 const；在一个函数声明中，const 可以修饰形参，表明它是一个输入参数，在函数内部不能改变其值；对于类的成员函数，若指定其为 const 类型，则表明是一个常函数，不能修改类的成员变量；对于类的成员函数，有时必须指定其返回值为 const 类型，从而使得其返回值不为 “左值”。



### 堆栈溢出的原因？

没有回收垃圾资源。

栈溢出：一般都是由越界访问导致的。例如局部变量数组越界访问或函数内部局部变量使用过多，超出了操作系统为该进程分配的栈大小。

堆溢出：由于堆是用户申请的，所以溢出的原因可能是程序员申请了资源但是忘记释放。



### 刷新缓冲区方式？

换行刷新缓冲区：printf("\n");

程序结束刷新缓冲区：return 0;



### 类和对象的两个基本概念？

类：用来描述一组具有相似属性的东西的数据结构。类中有数据成员的声明和定义，有成员函数的实现代码。

对象就是类的实例化。



### 介绍一下 STL，详细说明 STL 如何实现 vector

STL 是标准模板库，由容器算法迭代器组成。

STL 优点：很方便地对数据进行排序（调用 sort()）；调试程序时更加安全和方便；跨平台的，在 linux 下也可以使用。

vector 实际上就是一个动态数组，会根据数据的增加，动态地增加数组空间。



### 变量的声明和定义有什么区别？

变量的声明是告知编译器有某个类型的变量，但不会为其分配内存。

定义会分配内存。



### 简述 #define、#endif 和 #ifndef 的作用

这三个命令一般是为了避免头文件被重复引用。

\#ifndef CH_H	//如果没有引用ch.h

\#define			//引用ch.h

\#endif			//否则不需要引用



### 引用与指针有什么区别？

引用必须被初始化，指针不必；引用初始化以后不能被改变，指针可以改变所指的对象；不存在指向空值的引用，但是存在指向空值的指针。



### C++继承机制

public：类本身、派生类和其他类均可访问；

protected：类本身和派生类均可访问，其他类不能访问；

private（默认）：类本身，派生类和其他类不能访问。



### 头文件的作用是什么？

头文件用于保存程序的声明；通过头文件可以调用库函数，有些代码不能向用户公布，只要向用户提供头文件和二进制的库即可，用户只需要按照头文件中的接口声明来调用库功能，编译器会从库中提取相应的代码；如果某个接口被实现或被使用时，其方式与头文件中的声明不一致，编译器就会指出错误，这一规则能大大减轻程序员调试及改错的负担。



### system("pause") 的作用？

调用 DOS 的命令，按任意键继续，省去了使用 getchar()。区别是一个属于系统命令，一个属于 C++ 标准函数库。



### 析构函数和虚函数的用法和作用？

析构函数是类成员函数，在类对象生命期结束的时候，由系统自动调用，释放在构造函数中分配的资源。

虚函数是为了实现多态。

含有纯虚函数的类称为抽象类，不能实例化对象，主要用作接口类。

析构函数的特点：函数名称固定，~类名()；没有返回类型，没有参数；不可以重载，一般由系统自动调用。



### new、delete、malloc 和 free 的关系

new 和 delete 是一组，new 调用构造函数来实例化对象，delete 调用析构函数释放对象申请的资源。

malloc 和 free 是一组，malloc 用来申请内存，free 用来释放内存，但是申请和释放的对象只能是内部数据类型。

区别：malloc 和 free 是 C++/C 语言的标准库函数，new/delete 是 C++ 的运算符；malloc/free 只能操作内部数据类型。



### delete 与 delete[] 的区别

delete 只会调用一次析构函数，delete[] 会调用每一个成员的析构函数；delete 与 new 配套，delete[] 与 new[] 配套。



### 继承优缺点

优点：可以方便地改变父类的实现；可以实现多态；子类可以继承父类的方法和属性。

缺点：破坏封装；子类和父类可能存在耦合；子类不能改变父类的接口。



### C 和 C++ 有什么不同？

C 是面向过程的，即更偏向逻辑设计；C++ 是面向对象的，偏向类的设计。

C 适合要求代码体积小、效率高的场合，如嵌入式。



### 析构函数的调用次序，子类析构时要调用父类的析构函数吗？

析构函数调用的次序是：先派生类的析构函数，然后基类的析构函数。也就是在基类析构函数调用的时候，派生类的信息已经全部销毁了。

定义一个对象时，先调用基类的构造函数，然后调用派生类的构造函数。



### 什么是 “野指针”？

野指针指向一个已删除的对象或无意义的地址。

与空指针不同，野指针无法通过简单地判断是否为 NULL 避免，只能通过养成良好的编程习惯来尽力避免。

造成的主要原因：指针变量没有被初始化，或者被 free 或 delete 后，没有设置为 MULL。



### 常量指针和指针常量的区别？

