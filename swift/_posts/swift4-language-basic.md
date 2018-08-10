---
title: swift4-0起步系列-0x01-基础
date: 2018-07-31 11:03:48
tags: 
- swift
- ios
categories: 
- IOS-Swift4
---

💪：无折腾不欢，作不作都得死，本系列记录从0开始被swift虐的过程~
摘要：基础的套路就是从1个语言的常量(constant)、变量(variable)、类型(type)和元组(tuples)，学习怎么声明他们、命名他们和改变他们。也会学习关于类型推断(type inference)——Swift最重要的特色之一

<!-- more -->

# 基础内容
  ## 变量 & 常量
  - <font size=2>如何声明？</font>
    · <font color=FF7F50 >let</font> 来声明常量
    · <font color=FF7F50 >var</font> 来声明变量
    · 1行可声明多个变量，使用<font color=FF7F50 >逗号</font>分隔

  - <font size=2>类型标注</font>

    > <font size=2>* **格式**：变量名:类型名*
    var welcomeMessage: String
    var red, green, blue: Double </font>

  - <font size=2>如何命名？</font>
    
    > <font size=2>· *名字<font color=A52A2A>不能</font>包含空白字符、数学符号、箭头、保留的（或者无效的）Unicode 码位、连线和制表符。也不能以数字开头*
    · 如果你需要使用保留的关键字来给常量或变量命名，**可以使用反引号（ ` ）**包围它来作为名称
    let 哈 = "哈哈哈" </font>



  ## 基本类型
   
   ### 整型
   > <font size=2>· 8，16，32 和 64 位编码的有符号和无符号整数
   · 可以选择性地使用下划线来让大数字看起来更可读。
   · var a: Int = 1_000_000
   </font>


   #### 整数范围
   > <font size=2>min 和 max 属性
   · let minValue = UInt8.min
   · let maxValue = UInt8.max </font>

   #### 类型
   - <font size=2>Int</font>
   - <font size=2>UInt</font>

   
   ### 浮点数
   - <font size=2>2种类型</font>
    - <font size=2><font color=FF7F50 >Double </font> 代表 64 位的浮点数。</font> 
    - <font size=2><font color=FF7F50 >Float </font> 代表 32 位的浮点数。</font> 

    > <font size=2>Double精度 15 位，Float精度 6 位, 推荐使用Double</font>


   ### 布尔值
   - <font size=2>Bool</font>
   > <font size=2>true, false</font>
   ```
    if turnipsAreDelicious {
        print("Mmm, tasty turnips!")
    } else {
        print("Eww, turnips are horrible.")
    }
   ```

   ### 元组
   - <font size=2>元组把多个值合并成单1的复合型的值。</font>
   <font size=2>*元组内的值可以是任何类型，而且可以不必是同类型*</font>
   > <font size=2> 元组在临时的值组合中很有用，但是它们不适合创建复杂的数据结构

   ```
    // 描述了 HTTP 状态代码 的元组
    let http404Error = (404, "Not Found")
    // 将1个元组的内容分解成单独的常量或变量, 不需要的可用下划线_代替
    let (statusCode, _) = http404Error
    print("The status code is \(statusCode)");
    // 利用从零开始的索引数字访问元组中的单独元素
    print("The status code is \(http404Error.0)")
    // 定义的时候，可命名元素，
    let http200Status = (statusCode: 200, description: "OK")
    print("The status code is \(http200Status.statusCode)")

    
   ```
   </font> 


  ## 类型安全和类型推断
  
   - <font size=2>Swift 是类型安全的</font>
   · <font size=2>编译代码的时候会进行类型检查</font> 

   - <font size=2>类型推断</font>
   · <font size=2>通过检查你给变量赋的值，类型推断能够在编译阶段自动的推断出值的类型，减少类型声明</font>


  ## 数值型字面量
  
  - <font size=2>整型</font>
   - <font size=2>十进制数，没有前缀</font>
   - <font size=2>二进制数，前缀是 0b</font>
   - <font size=2>八进制数，前缀是 0o</font>
   - <font size=2>十六进制数，前缀是 0x</font> 


  ## 数值类型转换

   ### 整数转换
   
   > <font size=2>每个数值类型可存储的值的<font color=FF7F50>范围</font>不同，你必须根据不同的情况进行数值类型的转换</font>
   ```
   let twoThousand: UInt16 = 2_000
   let one: UInt8 = 1
   let twoThousandAndOne = twoThousand + UInt16(one)
   ```


   ### 整数和浮点数转换
   
   > <font size=2>整数和浮点数类型的转换必须<font color=FF7F50>显式</font>地指定类型</font>
   ```
   let three = 3
   let pointOneFourOneFiveNine = 0.14159
   let pi = Double(three) + pointOneFourOneFiveNine
   ...
   let integerPi = Int(pi)  // 数值会被截断
   ```

  ## 类型别名
  > <font size=2>用 <font color=FF7F50>typealias</font> 关键字定义类型别名</font>
  ```
  typealias AudioSample = UInt16
  ```


  ## 可选项
  > <font size=2>· 用可以利用<font color=FF7F50>可选项</font>处理空值的情况
  · 可选的 Int 写做 Int? ，而不是 Int 。问号明确了它储存的值是一个可选项，意思就是说它可能包含某些 Int 值，或者可能根本不包含值</font>*

   ### nil
   > <font size=2>· nil表示值为空，可以使用它设置到没有值的情况
   · nil 不能用于非可选的常量或者变量
   · nil 在Objective-C和Swift区别：
   &emsp;· 在 Objective-C 中 nil 是一个指向不存在对象的指针
   &emsp;· 在 Swift中， nil 不是指针，他是值缺失的一种特殊类型，任何类型的可选项都可以设置成 nil 而不仅仅是对象类型
   · if判断条件中，nil视为false
   ```
    var serverResponseCode: Int? = 404
    serverResponseCode = nil
    var surveyAnswer: String?   // 没有提供一个默认值，变量会被自动设置成 nil
   ```
   </font>

  ## 隐式展开可选项
  > <font size=2>可选项被定义为<font color=FF7F50>隐式展开可选项</font>，是在声明的类型后边添加一个叹号（ String! ）而非问号（  String? ）</font>
  ```
  let ageInteger: Int? = 23
  print(ageInteger)
  print(ageInteger + 1)     // error: value of optional type 'Int?' not unwrapped; did you mean to use'!' or '?'?（错误：可选值类型 'Int?' 的值没有被拆包；你是不是想使用 '!' 或 '?'？）
  ```

   ### 强制拆包
   > <font size=2>错误信息给了解决方案的指示：它告诉你可选值"没有被拆包"，需要声明如下，
   ```
    authorName = nil
    var unwrappedAuthorName = authorName!
    print("作者是\(unwrappedAuthorName)")    
    // fatal error: unexpectedly found nil while unwrapping an Optional value（致命错误：未料到地，拆包一个可选值的时候发现是 nil）
    // 发生了这个错误是因为在你尝试拆包变量的时候它没有包含任何值
    // 需要判断
    if authorName != nil {
        var unwrappedAuthorName = authorName!
        print("作者是\(unwrappedAuthorName)")
    } else {
        print("没有作者。")
    }
   ```
   </font>

   ### nil 合并
   > <font size=2>当你想要取出可选值里的值，不论值是什么，就用它—如果是 nil，就会使用一个默认值。这叫做 nil 合并（coalescing）</font>
   ```
    // 使用(??)进行nil合并
    var optionalInt: Int? = nil
    var r1: Int = optionalInt ?? 0
    print("nil ?? is: \(r1)");
   ```

   ### nil总结
   <font size=2>nil 表示值的缺失。</font>
   <font size=2>非可选值变量和常量一定会有一个非 nil 值。</font>
   <font size=2>可选值（Optional）变量和常量就像盒子，可以包含一个值或是空的（nil）。</font>
   <font size=2>要处理可选值里的值，首先必须从可选值里拆包。</font>
   <font size=2>拆包可选值最安全的方式是使用 if let 绑定（binding）或 nil 合并。避免强制拆包（forced unwrapping），它可能会导致一个运行时错误。</font>

   ## 错误处理机制
   ```
    func makeASandwich() throws {
        // ...
    }
    
    do {
        try makeASandwich()
        eatASandwich()
    } catch Error.OutOfCleanDishes {
        washDishes()
    } catch Error.MissingIngredients(let ingredients) {
        buyGroceries(ingredients)
    }
   ```


<br />

# 基本运算

 ## 赋值运算符
   ><font size=2>元组赋值</font>
   ```
   let (x, y) = (1, 2)
   // x 等于 1, 同时 y 等于 2
   ```


   ><font size=2>Swift 的赋值符号自身不会返回值 </font>  
   ```
   if x = y {
       // 这是不合法的, 因为 x = y 并不会返回任何值。
   }
   ```
 

 ## 比较元组

   > <font size=2>元组以从左到右的顺序比较大小，一次一个值，直到找到两个不相等的值为止。如果所有的值都是相等的，那么就认为元组本身是相等的 </font>
   ``` 
   (1, "zebra") < (2, "apple")   // true 1<2
   (3, "apple") < (3, "bird")    // true apple<bird
   (4, "dog") == (4, "dog")      // true
   ```


 ## 区间运算符
   ### 闭区间运算符
   > <font size=2>闭区间运算符（ a...b ）定义了从 a 到 b 的一组范围，并且包含 a 和 b 。 a 的值不能大于 b 。</font>
   ```
    for index in 1...5 {
        print("\(index) times 5 is \(index * 5)")
    }
   ```

   ### 半开区间运算符
   > <font size=2>· 半开区间运算符（ a..<b ）定义了从 a 到 b 但不包括 b 的区间
   · 半开区间在遍历基于零开始序列比如说数组的时候非常有用，它从零开始遍历到数组长度（但是不包含）</font>
   ```
    let names = ["Anna", "Alex", "Brian", "Jack"]
    let count = names.count
    for i in 0..<count {
        print("Person \(i + 1) is called \(names[i])")
    }
   ```

   ### 单侧区间
   > <font size=2>闭区间有另外一种形式来让区间朝一个方向尽可能的远，可以省略区间运算符一侧的值</font>
   ```
    for name in names[2...] {
        print(name) // Brian // Jack
    }
    
    for name in names[...2] {
        print(name) // Anna // Alex // Brian
    }
    
    for name in names[..<2] {
        print(name) // Anna // Alex
    }

   ```


# 字符串和字符
 ## 字符串类型 
 - <font size=2>String</font>
 - <font size=2>Character</font>
 
 <font size=2>· String的内容可以通过各种方法来访问到，包括作为 Character值的集合
 · 快速操作Unicode
 </font>
  
 - <font size=2>字符串是值类型</font>
 
 > <font size=2>每一次赋值和传递，现存的 String值都会被复制一次，传递走的是拷贝而不是原本 </font>


 ## 字符串字面量
 - <font size=2>双引号("")表示</font>
 - <font size=2>多行的字符串，使用3个双引号(""")</font>
 ```
 let quotation = """
  The White Rabbit put on his spectacles.  "Where shall I begin,
  please your Majesty?" he asked.
 
  "Begin at the beginning," the King said gravely, "and go on
  till you come to the end; then stop."
 """
 // 注意其中连续空白符会当成空格处理
 ```
 
 <img src="../../../../images/swift-page1-1.png" width=500 />


 - <font size=2>初始化一个空字符串</font>
  ```
  var emptyString = ""               // empty string literal
  var anotherEmptyString = String()  // initializer syntax
  ```

 

 ## 操作字符

 - <font size=2>初始化字符串</font>
  ```
  // 使用Character创建
  let catCharacters: [Character] = ["C", "a", "t", "!", "?"];
  let catString = String(catCharacters);
  ```

 - <font size=2>连接字符串和字符</font>
  - <font size=2> + </font>
  - <font size=2> += </font>
  - <font size=2> append() </font>
  > <font size =2>可以给一个 String变量的末尾追加 Character值</font>
  ```
  let exclamationMark: Character = "!"
  welcome.append(exclamationMark)
  ```
  - <font size=2>字符串插值</font>
  > <font size =2>· 混合常量、变量、字面量和表达式的字符串字面量构造出新的String值的方法
  · 字符串字面量的元素都要被一对圆括号包裹，然后使用反斜杠前缀 </font>
  ```
  let multiplier = 3
  let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
  ```
  
  - <font size=2>字符统计</font>
  > <font size=2>使用字符串的 count属性</font>
  > <font size=2><font color=FF7F50>注意：</font> Swift3，获取字符数count属性在characters下</font>
  ```
  var test1: String = "beijing"
  print("test1 has \(test1.count) characters")
  // swift3：test1.characters.count
  ```

  - <font size =2>字符串索引</font>

  > <font size=2>遵循 Indexable 协议的类型中使用 startIndex 和 endIndex 属性以及 index(before:) ， index(after:) 和 index(_:offsetBy:) 方法。这包括这里使用的 String，还有集合类型比如 Array， Dictionary 和 Set </font>

   - <font size =2>String.Index，它相当于每个 Character在字符串中的位置</font>
   - <font size =2>String.startIndex，String.endIndex </font>
   - <font size =2>index(before:),index(after:)：表示给定索引的前后索引值</font>
   - <font size =2>index(_:offsetBy:)：表示给定索引偏移后的值</font>
    ```
    let greeting = "Guten Tag!"
    greeting[greeting.startIndex]   // G
    greeting[greeting.index(before: greeting.endIndex)]     // !
    greeting[greeting.index(after: greeting.startIndex)]    // u
    let index = greeting.index(greeting.startIndex, offsetBy: 7)
    greeting[index]    // a
    ```

   - <font size =2>indices属性来访问字符串中每个字符的索引</font>
   ```
   for index in greeting.indices {
     print("\(greeting[index]) ", terminator: "")
   }
   ```

  - <font size =2>插入和删除 </font>
   
   - <font size =2>insert(_:at:) —— 插入1个字符</font>
   - <font size =2>insert(contentsOf:at:) —— 插入多个字符，swift4，contentsOf传入字符串，swift3里，contentsOf传入字符串的characters属性</font>
   ```
    var welcome = "hello"
    welcome.insert("!", at: welcome.endIndex)
    welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
    // swift3, welcome.insert(contentsOf: " there".characters, at: welcome.index(before: welcome.endIndex))
    print("insert test: " + welcome)
   ```

   - <font size =2>remove(at:) —— 删除1个字符</font>
   - <font size =2>removeSubrange(_:) —— 删除多个字符（删除范围）</font>
   ```
    welcome.remove(at: welcome.index(before: welcome.endIndex))
    print("remove test: " + welcome)
    // 移除多个
    let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
    welcome.removeSubrange(range)
    print("remove test range of: " + welcome)
   ```

   - <font size =2>replaceSubrange(_:) —— 替换多个字符（替换范围）</font> 
   ```
    //替换
    welcome.replaceSubrange(range, with: " hahah")
   ```

  - <font size =2>子字符串</font>

  > <font size=2>· 当你获得了一个字符串的子字符串——比如说，使用下标或者类似 prefix(_:) 的方法——结果是一个<font color=FF7F50>Substring</font>的实例，不是另外一个字符串
  · 字符串与子字符串的不同之处在于，作为性能上的优化，子字符串可以重用一部分用来保存原字符串的内存，或者是用来保存其他子字符串的内存。（字符串也拥有类似的优化，但是如果两个字符串使用相同的内存，他们就是等价的。）
  </font>
  ```
  // swift4
  let greeting = "Hello, world!"
  let index = greeting.index(of: ",") ?? greeting.endIndex
  let beginning = greeting[..<index]    // beginning is "Hello"
  let newString = String(beginning)
  
  // swift3
  var greeting = "Hello, world!";
  let startInt = greeting.index(greeting.startIndex, offsetBy: 0)
  let endInt = greeting.index(greeting.startIndex, offsetBy: 5)
  var beginning = greeting[startInt..<endInt]
  print("substring: " + beginning);
  greeting.insert(contentsOf: " test".characters, at: startInt)
  print("substring: \(beginning) , new string: \(greeting)");
  ```

  - <font size =2>字符串比较</font>
   - <font size =2>3种文本值比较：</font>
    - <font size =2>字符串和字符相等性 </font>
      &emsp;·<font size =2>字符串和字符相等使用“等于”运算符 (==) 和“不等”运算符 (!=)进行检查 </font>
    - <font size =2>前缀相等性 </font>
      &emsp;·<font size =2>hasPrefix(_:)</font>
    - <font size =2>后缀相等性 </font>
      &emsp;·<font size =2>hasSuffix(_:)</font>

  > <font size=2>hasPrefix(_:)和 hasSuffix(_:)方法只对字符串当中的每一个扩展字形集群之间进行了一个逐字符的规范化相等比较。</font>

 ## Unicode 标量
  > <font size=2>· 原生 String 类型建立于 Unicode 标量值之上
  · 一个 Unicode 标量是一个为字符或者修饰符创建的独一无二的<font color=FF7F50>21</font>位数字
  · Unicode 标量码位位于 U+0000到 U+D7FF或者 U+E000到 U+10FFFF之间。Unicode 标量码位不包括从 U+D800到 U+DFFF的16位码元码位。</font>
  
  - <font size=2>特殊字符</font>
   
   - <font size=2>转义特殊字符 \0 (空字符)， \\ (反斜杠)， \t (水平制表符)， \n (换行符)， \r(回车符)， \" (双引号) 以及 \' (单引号)；</font>
   - <font size=2>任意的 Unicode 标量，写作 \u{n}，里边的 n是一个 1-8 个与合法 Unicode 码位相等的16进制数字。</font>
   ```
   let blackHeart = "\u{2665}" // ♥
   ```


 ## 集合类型

 > <font size=2>· 用来储存值的集合
 · 明确能储存的值的类型以及它们能储存的键
 · 集合可变性 </font>
 
  ### 3种类型
   - <font size=2>数组 </font>
   &emsp;<font size=2>数组是有序的值的集合 </font>
   - <font size=2>合集 </font>
   &emsp;<font size=2>唯一值的无序集合 </font>
   - <font size=2>字典 </font>
   &emsp;<font size=2>无序的键值对集合 </font>

  ### 数组
  > · <font size=2>有序
  · 类型相同
  · Swift 的 Array类型被桥接到了基础框架的 NSArray类上 </font>
  
  - <font size=2>数组类型简写语法</font>
    - <font size=2>Array<Element></font>
    - <font size=2>[Element]</font>

  - <font size=2>基本用法</font>
   -  <font size=2>创建空数组</font>
   ```
   var someInts = [Int]()
   print("someInts is of type [Int] with \(someInts.count) items.")
   ```
   
   - <font size=2>对已有类型的数组，添加，置空</font>
   ```
   someInts.append(3)   // 添加
   someInts = []        // 置空
   ```

   - <font size=2>默认值创建数组</font>

   > <font size=2>Array类型提供了初始化器来创建确定大小且元素都设定为相同默认值的数组</font>
   ```
   var a1 = Array(repeating: 0.0, count: 3)
   // repeating表示默认值， count表示数组长度
   ```
   
   - <font size=2>通过连接2个数组来创建数组</font>

   > <font size=2>2个兼容类型的现存数组用加运算符（+），创建新数组</font>
   ```
   var a2 = Array(repeating: 2.5, count: 3)
   var a3 = a1 + a2     // 合并数组[0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
   ```

   - <font size=2>使用数组字面量创建数组</font>
   ```
   var a4: [String] = ["Eggs", "Milk"]
   ```

   - <font size=2>获取数组个数及元素</font>
    
     - <font size=2>Array.count: 获取数组长度</font>
     - <font size=2>Array[index]: 访问元素</font>
     - <font size=2>Array.isEmpty: 判断是否是空数组(count==0)</font>
     - <font size=2>Array.append(_:): 添加元素至末尾</font>
     - <font size=2>加赋值运算符(+=)，添加1个或多个元素至末尾</font>
     - <font size=2>Array[index1...index2] = ["new1", "new2"]: 替换数组内某索引的元素</font>
     - <font size=2>Array.insert(_:at:): 指定位置插入元素</font>
     - <font size=2>Array.remove(at:): 指定位置删除元素，返回被删除元素</font>
     - <font size=2>Array.insertLast(): 删除最后1个元素</font>
     - <font size=2>for-in遍历一个数组</font>
     - <font size=2>Array.enumerated()返回数组中每个元素的元组, 可以获得索引值</font>
   ```
   // 获取数组长度
   var a1 = ["test1", "test2"];
   print("get array count: \(a1.count) items; get array index 0: \(a1[0])");
   // 扩展数组
   a1+=["test3", "test4", "test5"]
   print("get array count: \(a1.count) items; get a1: \(a1.joined(separator: ","))");
   // 替换数组元素
   a1[2...3] = ["new1", "new2"]
   print("replace a1: \(a1.joined(separator: ","))")
   a1.insert("insert1", at: 3)
   print("insert after a1: \(a1.joined(separator: ","))");
   var item = a1.remove(at: 3);
   print("removed: \(item) , and after a1: \(a1.joined(separator: ","))");
   // 数组遍历
   for item in a1 {
       print("item is \(item)")
   }
   for (index, value) in a1.enumerated() {
       print("a1's \(index): \(value)");
   }
   ```

  ### 合集
  > · <font size=2>不重复且值无序
  · 类型相同
  · Swift 的 Set类型桥接到了基础框架的 NSSet 
  · 值必须是可哈希的（基础类型： String, Int, Double, 和 Bool 均可， 自定义类型需遵循Hashable协议，Hashable协议遵循Equatable）</font>
  * <font size=2> 哈希值是Int值且所有的对比起来相等的对象都相同，比如 a == b，它遵循 a.hashValue == b.hashValue </font>


  - <font size=2>合集基本语法</font>
 
   - <font size=2>Set<Element>，这里的 Element是合集要储存的类型</font>


  - <font size=2>合集基本使用</font>
  
   - <font size=2>创建并初始化1个空合集</font>
   ```
   var set1 = Set<Character>()
   print("set has \(set1.count)");
   ```

   - <font size=2>使用数组字面量创建合集</font>
   ```
   var set2 = ["test1","test2","test3"];
   print("set2 has \(set2.count), with: \(set2.joined(separator: ","))");
   ```



  - <font size=2>基本使用</font>
  
   - <font size=2>Set.count：获取总个数</font>
   - <font size=2>Set.isEmpty：是否为空</font>
   - <font size=2>Set.insert(_:)：添加元素</font>
   - <font size=2>Set.remove(_:)：移除元素，返回移除的元素值</font>
   - <font size=2>Set.contains(_:)：是否含有某元素</font> 
   - <font size=2>for-in进行遍历</font>

   
  - <font size=2>基本合集操作</font>
   
   <img src="../../../../images/swift-page1-2.png" width=500 />

   - <font size=2>Set.intersection(_:)：创建一个只包含两个合集共有值的新合集；</font>
   - <font size=2>Set.symmetricDifference(_:)：创建一个只包含两个合集各自有的非共有值的新合集；</font>
   - <font size=2>Set.union(_:)：创建一个包含两个合集所有值的新合集；</font>
   - <font size=2>Set.subtracting(_:)：创建一个两个合集当中不包含某个合集值的新合集。 </font>


  - <font size=2>合集成员关系和相等性</font>
   
   - <font size=2>（==）：判断所有元素是否相等；</font>
   - <font size=2>Set.isSubset(of:)：确定一个合集的所有值是被某合集包含；</font>
   - <font size=2>Set.isSuperset(of:)：确定一个合集是否包含某个合集的所有值；</font>
   - <font size=2>Set.isStrictSubset(of:)，或 Set.isStrictSuperset(of:)：确定是个合集是否为某一个合集的子集或者超集，但并不相等 </font>
   - <font size=2>Set.isDisjoint(of:)：判断两个合集是否拥有完全不同的值；</font>


  ### 字典
