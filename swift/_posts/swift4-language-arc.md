---
title: swift4-0起步系列-0x0x-自动引用计数(ARC)
date: 2018-08-01 11:03:48
tags: 
- swift
- ios
- arc
categories: 
- IOS-Swift4
---

😈：你和高手之间只隔着1个内存优化。摘要：本文着重讲解Swift的ARC和内存管理，ARC是什么，MRC是什么，运行原理以及内存管理balabala...，但是要理解ARC，需要有些内存知识进行铺垫，本文也会捎带

<!-- more -->

# 公元前
 *简单对内存知识进行介绍*

 ## 5大内存区域
 > <font size=2>· 栈区
 · 堆区
 · 全局区
 · 常量区
 · 代码区
 · 5大内存区域之外还有，自由存储区（也称之5大区域之外区）</font>

 - <font size=2>栈区</font>
 > <font size=2>· 创建临时变量时由编译器自动分配，在不需要的时候自动清除的**<font color=FF7F50>变量的存储区</font>**
 · 里面的变量通常是<font color=FF7F50>局部变量、函数参数</font>等。
 · 在一个进程中，位于用户虚拟地址空间顶部的是用户栈，编译器用它来实现函数的调用。
 · 和堆一样，用户栈在程序执行期间可以动态地扩展和收缩
 </font>
 ```
  func test(name: String)) {
    var test = 1;   // test是局部变量，存在栈区，name是参数，存在栈中
  }
 ```


 - <font size=2>堆区</font>
 > <font size=2>· 由 <font color=FF7F50>new</font> alloc 创建的对象所分配的内存块
 · 系统不会主动释放，需要开发者去告诉系统什么时候释放这块内存（引用计数为0）
 · 堆可以动态地扩展和收缩
 </font>
 ```
 let test1 = ClassA()   // 使用new创建出的对象存储在堆区
 ```


 - <font size=2>全局/静态存储区</font>
 
 > <font size=2>· 全局变量和静态变量被分配到同1块内存中</font>


 - <font size=2>常量区</font>
 
 > <font size=2>· 存放的是常量，不允许修改 </font> 


 - <font size=2>代码区</font> 
 
 > <font size=2>· 存放函数的二进制代码 </font> 


 - <font size=2>自由存储区</font>
 
 > <font size=2>点 由 malloc 等分配的内存块
 · 和堆是十分相似的
 · 用 free 来结束自己的生命的 </font>

 ## 属性标识符
  ### @property、@synthesize、@dynamic

   - <font size=2>@synthesize</font>
   
   > <font size=2>在对象属性使用@synthesize声明的时候编译器会自动为该属性按照固有规则生成相应的getter setter方法</font>
   
   - <font size=2>@dynamic</font>

   > <font size=2>与@synthesize相反，使用@dynamic声明时相当于告诉编译器getter setter方法由用户自己**手动生成**</font>
 
   - <font size=2>@property</font>  
  
   > <font size=2>· @property声明的作用区域在@interface内部。 
   · 它会告诉编译器自动生成getter setter方法。也允许用户手动生成getter setter中的一个方法
   · 用@property声明的属性不能手动同时写getter setter方法，否则编译器会报错</font>

  ### nonatomic与atomic

   - <font size=2>nonatomic</font>
   
   > <font size=2>· 在调用用nonatomic声明的对象属性时是非线程安全性的 
   · <font color=FF7F50>非线程安全</font>采用的nonatomic，不同操作可以同时执行，而不需要等前面的操作完成后在进行下一步操作。
   · 非原子性的执行效率更高不会阻塞线程 </font>

   - <font size=2>atomic</font>
   
   > <font size=2>· atomic是具有线程安全性的 </font>
   · get/set方法中加入线程操作。每当对用atomic声明的对象属性操作时，会根据操作加入线程的顺序一步一步完成操作
   · 执行效率较低

  ### strong、weak、retain、assgin、copy、unsafe_unretained
  
   - <font size=2>retain：释放旧对象</font>
   
   > <font size=2>会对输入对象的引用计数+1，将输入对象的值赋值于旧对象，只能用户声明OC对象</font>
   
   - <font size=2>strong：强引用</font>

   > <font size=2><font color=FF7F50>ARC特有</font>
   在MRC时代没有，相当于retain。由于MRC时代是靠引用计数器来管理对象什么时候被销毁所以用retain，而ARC时代管理对象的销毁是有系统自动判断，判断的依据就是该对象是否有强引用对象。如果对象没有被任何地方强引用就会被销毁。所以在ARC时代基本都用的strong来声明代替了retain。只能用于声明OC对象(ARC特有)
   </font>

   - <font size=2>assgin：简单赋值操作</font>

   > <font size=2>不会更改引用计数，用于基本的数据类型声明 </font>

   - <font size=2>weak：弱引用</font>

   > <font size=2>· <font color=FF7F50>ARC特有。</font> 表示该属性是一种"非拥有关系"
   对于 weak 对象会放入一个 hash 表中。 用 weak 指向的对象内存地址作为 key，当此对象的引用计数为0的时候会 dealloc，假如 weak 指向的对象内存地址是a，那么就会以a为键， 在这个 weak 表中搜索，找到所有以a为键的 weak 对象，从而设置为 nil。 </font>

   - <font size=2>copy</font>
    
    - <font size=2>浅拷贝</font>
    
    > <font size=2>如果对象是一个不可变对象
    例如NSArray NSString 等，那么copy等同于retain、strong，引用计数+1</font>

    - <font size=2>深拷贝</font> 
    
    > <font size=2>对象是一个可变对象
    例如:NSMutableArray,NSMutableString等，它会在内存中重新开辟了一个新的内存空间，存储新的对象,和原来的对象是两个不同的地址</font>

    
 ## MRC与ARC区别

  ### MRC手动内存管理
  
   #### 引用计数器
   > <font size=2>在MRC时代，系统判定一个对象是否销毁是根据这个对象的引用计数器来判断的
   1.每个对象被创建时引用计数都为1
   2.每当对象被其他指针引用时，需要手动使用[obj retain]; 自己持有对象，让该对象引用计数+1。
   3.当指针变量不在使用这个对象的时候，需要手动释放[obj release];这个对象。 让其的引用计数-1.
   4.当一个对象的引用计数为0的时候，系统就会销毁这个对象 
   在MRC模式下必须**遵循**: <font color=FF7F50>谁创建，谁释放，谁引用，谁管理</font> </font>


  ### ARC自动内存管理
  *自动管理机制 —— 自动引用计数（ARC）* 
  > <font size=2>特性：
  &emsp;它不是垃圾回收机制而是编译器的一种特性
  机制：
  &emsp;ARC管理机制与MRC手动机制差不多，只是不再需要手动调用retain、release、autorelease；当你使用ARC时，编译器会在在适当位置插入release和autorelease</font>

  ### autoreleasepool自动释放池
  > <font size=2>自动释放池始于MRC时代，主要是用于 自动 对 释放池内 对象 进行引用计数-1的操作，即自动执行release方法。
  · MRC下，autoreleasepool必须在代码块内部手动为对象调用autorelease把对象加入到的自动释放池，系统会自动在代码块结束后，对加入自动释放池中的对象发送一个release消息。无需手动调用release
  · 在ARC下不要手动的为@autoreleasepool代码块内部对象添加autorelease，ARC下自动的把@autoreleasepool代码块中创建的对象加入了自动释放池中
  </font>


# 自动引用计数
 ## ARC的工作机制
  <font size=2>**从创建实例说起**</font>
  
  > <font size=2>1）每次你创建一个类的实例，ARC 会分配一大块内存来存储这个实例的信息（保存实例的类型信息，以及该实例所有存储属性值的信息）。
  2）当实例不需要时，ARC 会释放该实例所占用的内存
  3）确保使用中的实例不会消失，ARC 会跟踪和计算当前实例被多少属性，常量和变量所引用。只要存在对该类实例的引用，ARC 将不会释放该实例
  4）如何确保不消失？将实例分配给属性，常量或变量，它们都会创建该实例的<font color=FF7F50>强引用</font>，只要有强引用，实例不允许被销毁
  🌰 </font>
  ```
    // 定义类
    class Person {
        let name: String            // 私有属性
        init(name: String) {        // 初始化
            self.name = name
            print("\(name) is being initialized")
        }
        deinit {                    // 销毁时调用
            print("\(name) is being deinitialized")
        }
    }
    // 定义变量，未初始化
    var reference1: Person?
    var reference2: Person?
    var reference3: Person?
    // 创建实例，产生1个强引用
    reference1 = Person(name: "John Appleseed")
    // 将同个Person实例分配给2个变量，又产生2个强引用
    reference2 = reference1
    reference3 = reference1
    // 赋空，断开2个强引用，仍存在1个，Person 实例不会被销毁
    reference1 = nil
    reference2 = nil
    // 直到最后1个强引用断开， ARC 销毁 Person实例
    reference3 = nil
  ```


 ## 类实例之间的循环强引用
 
 > <font size=2>· 循环强引用：2个类实例彼此持有对方的强引用，因而每个实例都让对方1直存在
 · 可能造成循环引用
 🌰 </font>
 ```
    class Person {
        let name: String
        init(name: String) { self.name = name }
        var apartment: Apartment?                               // 可选的公寓
        deinit { print("\(name) is being deinitialized") }
    }
    class Apartment {
        let unit: String
        init(unit: String) { self.unit = unit }
        var tenant: Person?                                     // 可选的户主
        deinit { print("Apartment \(unit) is being deinitialized") }
    }
    // 各自创建实例
    var john: Person?
    var unit4A: Apartment?
    john = Person(name: "John Appleseed")
    unit4A = Apartment(unit: "4A")
    // 造成循环引用
    john!.apartment = unit4A
    unit4A!.tenant = john
    // 即便赋空，没有任何一个反初始化器被调用。
    // 循环强引用会一直阻止 Person 和 Apartment 类实例的释放，这就在你的应用程序中造成了内存泄漏
    john = nil
    unit4A = nil
  ```



 ## 解决实例之间的循环强引用

 <font size=2>需要允许循环引用中的1个实例引用另外1个实例而不保持强引用，这样实例能够互相引用而不产生循环强引用
 2种方法解决：
 · 弱引用（weak reference）
 · 无主引用（unowned reference）
 对于生命周期中会变为 nil 的实例使用弱引用，
 相反，对于初始化赋值后再也不会被赋值为 nil 的实例，使用无主引用 </font>

 - <font size=2>弱引用</font>

 > <font size=2>·ARC 会在被引用的实例被释放是自动地设置弱引用为 nil 。由于弱引用需要允许它们的值为 nil ，它们一定得是可选类型 
 · 关键字：weak </font>
 ```
    class Person {
        weak var apartment: Apartment?
    }
    class Apartment {
        ...
        weak var tenant: Person?
        ...
    }
    var john: Person?
    var unit4A: Apartment?
    john = Person(name: "John Appleseed")
    unit4A = Apartment(unit: "4A")
    // 设置属性
    john!.apartment = unit4A
    unit4A!.tenant = john
    // 释放
    john = nil
    unit4A = nil
 ```
 
 - <font size=2>无主引用</font>

 > <font size=2>· 无主引用假定是永远有值的。因此，无主引用总是被定义为非可选类型。
 · 关键字：unowned 
 🌰 </font>
 ```
    class Customer {
        let name: String
        var card: CreditCard?
        init(name: String) {
            self.name = name
        }
        deinit { print("\(name) is being deinitialized") }
    }
    class CreditCard {
        let number: UInt64
        unowned let customer: Customer   // 信用卡总是关联着一个客户，因此将 customer 属性定义为无主引用
        init(number: UInt64, customer: Customer) {
            self.number = number
            self.customer = customer
        }
        deinit { print("Card #\(number) is being deinitialized") }
    }
    // 创建实例
    var john: Customer?
    john = Customer(name: "John Appleseed")
    john!.card = CreditCard(number: 1234_5678_9012_3456， customer: john!)
    // 释放
    john = nil  // Customer 实例和 CreditCard 都将被释放
 ```

 
 
 ## 无主引用和隐式展开的可选属性
 <font size=2>场景：
 classA，classB分别包含对方不能为空的对象属性，且classA初始化器调用classB的初始化
 此时需要：classB使用无主属性，classA使用隐式展开的可选属性 
 🌰 </font>
 ```
    class Country {
        let name: String
        var capitalCity: City!                          // 隐式展开，国家必须有1个首都城市（有1个默认nil，但无需展开即可使用），
        init(name: String, capitalName: String) {
            self.name = name
            // 只有 Country 的实例完全初始化完后， Country 的初始化器才能把 self 传给 City 的初始化器
            self.capitalCity = City(name: capitalName, country: self)
        }
        deinit { print("Country #\(name) is being deinitialized") }
    }
    class City {
        let name: String
        unowned let country: Country                    // 无主引用，初始化的时候传入
        init(name: String, country: Country) {
            self.name = name
            self.country = country
        }
        deinit { print("City #\(name) is being deinitialized") }
    }
    var country:Country?
    country = Country(name: "Canada", capitalName: "Ottawa")
    print("\(country!.name)'s capital city is called \(country!.capitalCity.name)")
    country = nil
 ```

 
 
 ## 闭包的循环强引用


   

