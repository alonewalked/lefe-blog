---
title: swift4-0起步系列-0x01-基础(2)
date: 2018-07-31 11:03:48
tags: 
- swift
- ios
categories: 
- IOS-Swift4
---

☔️：与其焦虑成疾，不如静心学习
摘要：本文继续记录swift语言基础，涉及控制流，晦涩难懂的闭包等


# 基础内容（2）

 ## 控制流
   ### For-in 
   > <font size=2>** · 循环来遍历序列 **</font>
   ```
    let names = ["Anna", "Alex", "Brian", "Jack"]
    for name in names {
        print("Hello, \(name)!")
    }
   ```

   > <font size=2>** · 遍历键值对 **
   注意：Dictionary是无序的，返回遍历顺序不是插入顺序</font>
   ```
    let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
    for (key, value) in numberOfLegs {
        print("\(key)s have \(value) legs")
    }
   ```

   > <font size=2>** · 遍历数字区间 **
   使用闭区间运算符 ( ... )，或者半开区间（ ..< )</font>
   ```
    for index in 1...5 {
        print("\(index) times 5 is \(index * 5)")
    }
    let minutes = 60
    for tickMark in 0..<minutes {
        // render the tick mark each minute (60 times)
    }
   ```

   > <font size=2>** · 跳过某些关键帧 **
   使用 stride(from:to:by:) 函数来跳过不想要的标记
   使用 stride(from:through:by:)，应用于闭区间 </font>
   ```
    let minuteInterval = 5
    for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
        // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55)
    }
    for tickMark in stride(from: 0, through: minutes, by: minuteInterval) {
        // render the tick mark every 5 minutes (0, 5, 10, 15 ... 45, 50, 55, 60)
    }
   ```


   
   ### While 循环
   
   <font size=2>** 循环执行一个合集的语句直到条件变成false **
   配合 break 和 continue
   2种 while 循环: </font>
   
   > <font size=2> · while ：条件计算时机 —— 在每次循环开始时 </font>
   ```
    while condition {
        statements
    }
   ```
   > <font size=2>· repeat-while： 条件计算时机 —— 在每次循环结束的时 </font>
   ```
    repeat {
        statements
    } while condition
   ```
  
 
 
 ## 条件语句

  ### If
  <font size=2> if - else if - else </font>

  ### Switch
  
  <font size=2>注意：**break 在 Swift 里不是必须的**
  整个 switch 语句会在匹配到第一个 switch 情况执行完毕之后退出，不再需要显式的 break 语句
  如果需要贯穿，末尾加入<font color=#FF7F50>fallthrough</font></font>
  ```
   switch [consition] {
        case value 1:
            respond to value 1
        case value 2, value 3:
            respond to value 2 or 3
        default:
            otherwise, do something else
    }
  ```

  <font size=2>**区间匹配**</font>
  > <font size=2>可以在1个区间中匹配</font>
  ```
    let count = 62
    var text: String
    switch count {
    case 0:
        text = "no"
    case 1..<5:
        text = "a few"
    default:
        text = "many"
    }
  ```

  <font size=2>**元组匹配**</font>
  > <font size=2>每个元组中的元素都可以与不同的值或者区间进行匹配</font >
  ```
    let somePoint = (1, 1)
    switch somePoint {
    case (0, 0):
        print("(0, 0) is at the origin")
    case (_, 0):
        print("(\(somePoint.0), 0) is on the x-axis")
    case (0, _):
        print("(0, \(somePoint.1)) is on the y-axis")
    case (-2...2, -2...2):
        print("(\(somePoint.0), \(somePoint.1)) is inside the box")
    default:
        print("(\(somePoint.0), \(somePoint.1)) is outside of the box")
    }
  ```

  <font size=2>**值绑定**</font>
  > <font size=2>可以将匹配到的值临时绑定为1个常量或者变量</font>
  ```
    let p = (0, 1)
        switch p {
        case (let x, 0):
            print("on the x-axis with an x value of \(x)")
        case (0, let y):
            print("on the y-axis with a y value of \(y)")      // 匹配到元组第1个值，进入case，并把元组第2个值赋给y
        case let (x, y):
            print("somewhere else at (\(x), \(y))")
    }
  ```

  <font size=2>**where**</font>
  > <font size=2>可以使用 where 分句来检查额外的情况</font>
  ```
    let wp = (1, -1)
    switch wp {
    case let (x, y) where x == y:
        print("(\(x), \(y)) is on the line x == y")
    case let (x, y) where x == -y:                     // 匹配到where条件，进入case
        print("(\(x), \(y)) is on the line x == -y")
    case let (x, y):
        print("(\(x), \(y)) is just some arbitrary point")
    }
  ```

  <font size=2>**复合情况**</font>
  > <font size=2>支持case 后写多个模式来复合，可换行</font>
  ```
    let c: Character = "e"
    switch wp {
    switch c {
    case "a", "e", "i", "o", "u":
        print("\(c) is a vowel")
    case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
        "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
        print("\(c) is a consonant")
    default:
        print("\(c) is not a vowel or a consonant")
    }
  ```

  <font size=2>**给语句打标签**</font>
  > <font size=2>当while，switch嵌套时，添加标签，可以指定被标记的语句break或continue</font>
  ```
    label name: while condition {
        statements
    }
    // demo:
    gameLoop: while square != finalSquare {
        diceRoll += 1
        if diceRoll == 7 { diceRoll = 1 }
        switch square + diceRoll {
        case finalSquare:
            // diceRoll will move us to the final square, so the game is over
            break gameLoop
        case let newSquare where newSquare > finalSquare:
            // diceRoll will move us beyond the final square, so roll again
            continue gameLoop
        default:
            // this is a valid move, so find out its effect
            square += diceRoll
            square += board[square]
        }
    }
  ```


 ## 函数
  ### 定义和调用函数
  > <font size=2>· 形参表
  参数名: 参数类型
  · 返回值
  -> 返回类型 </font>
  ```
    // 定义
    func greet(person: String) -> String {
        let greeting = "Hello, " + person + "!"
        return greeting
    }
    // 调用
    print(greet(person: "Anna"))
  ```
 

  <font size=2>· **多返回值的函数** </font>
  > <font size=2>函数返回多个值作为一个复合的返回值，你可以使用元组类型作为返回类型</font>
  ```
    func minMax(array: [Int]) -> (min: Int, max: Int) {
        var currentMin = array[0]
        var currentMax = array[0]
        for value in array[1..<array.count] {
            if value < currentMin {
                currentMin = value
            } else if value > currentMax {
                currentMax = value
            }
        }
        return (currentMin, currentMax)
    }
    // 调用
    let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
    print("min is \(bounds.min) and max is \(bounds.max)")
  ```
 

  <font size=2>· **可选元组返回类型** </font>
  > <font size=2>如果元组在函数的返回类型中有可能空，你可以用一个可选元组返回类型来说明整个元组的可能是 nil
  对于可选元组类型，整个元组是可选的，而不仅仅是元组里边的单个值</font>
  ```
    func minMax(array: [Int]) -> (min: Int, max: Int)? {
        if array.isEmpty { return nil }
        var currentMin = array[0]
        var currentMax = array[0]
        for value in array[1..<array.count] {
            if value < currentMin {
                currentMin = value
            } else if value > currentMax {
                currentMax = value
            }
        }
        return (currentMin, currentMax)
    }
    // 调用，只处理有返回值的情况
    if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
        print("min is \(bounds.min) and max is \(bounds.max)")
    }
  ```
 

  <font size=2>· **省略实际参数标签** </font>
  > <font size=2>使用下划线（_）省略 实际参数名</font>
  ```
  func someFunction(_ a: Int, b: Int) {
        // In the function body, firstParameterName and secondParameterName
        // refer to the argument values for the first and second parameters.
    }
    someFunction(1, b: 2)
  ```
 

  <font size=2>· **可变形参** </font>
  > <font size=2>形参名:形参类型 = 默认值</font>
  ```
    func someFunction(a: Int = 12) {
        // In the function body, if no arguments are passed to the function
        // call, the value of [a] is 12.
    }
    someFunction(a: 6) // a is 6
    someFunction() // a is 12
  ```
 

  <font size=2>· **可变形参** </font>
  ><font size=2>形参表只能存在1个可变形参，可变参数的位置任意</font>
  ```
    func add(numbers: Int..., fixed: Int) -> Int {
        var totle = 0;
        for num in numbers {
            totle += num;
        }
        return totle + fixed;
    }
    // 调用
    add(numbers: 1,2,3,4, fixed:2)
  ```
 

  <font size=2>· **输入输出形式参数** </font>
  
  > <font size=2>输出形参使用关键字 inout
  让参数可以被函数修改，传参时加上&符 </font>
  ```
    func swap(_ a: inout Int, _ b: inout Int) {     // 输出形式参数
        let temporaryA = a
        a = b
        b = temporaryA
    }
    var test1 = 3
    var test2 = 107
    swapTwoInts(&test1, &test2)
  ```
 

  ### 函数类型
  > <font size=2>每1个函数都有1个特定的函数类型，它由形式参数类型，返回类型组成 
  (Int, Int) -> Int
  () -> Void </font>

   #### 使用函数类型
   
   > <font size=2>· 定义1个函数类型变量，指向某个对应的函数，可直接调用或者作为形参，返回值
   · 可以实现柯里化</font>
   ```
    func addTwoInts(_ a: Int, _ b: Int) -> Int {
        return a + b
    }
    // 函数类型变量，指向某个对应的函数
    var mathFunction: (Int, Int) -> Int = addTwoInts
    // 1）可直接调用
    mathFunction(2, 3)

    // 2）作为形参类型
    func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
        print("Result: \(mathFunction(a, b))")
    }
    printMathResult(addTwoInts, 3, 5)

    // 3）函数类型作为返回类型
    func stepForward(_ input: Int) -> Int {
        return input + 1
    }
    func stepBackward(_ input: Int) -> Int {
        return input - 1
    }
    func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
        return backwards ? stepBackward : stepForward
    }
    // chooseStepFunction(backwards: true) 返回1个函数
    print("moveNearerToZero is: \(chooseStepFunction(backwards: true)(3))")
   ```
 

  ### 内嵌函数
  > <font size=2>函数包含在某函数中，并做为返回值返回</font>
  ```
    func chooseStepFunction(backward: Bool) -> (Int) -> Int {
        func stepForward(input: Int) -> Int { return input + 1 }
        func stepBackward(input: Int) -> Int { return input - 1 }
        return backward ? stepBackward : stepForward
    }
  ```
 

 ## 闭包

 > <font size=2>闭包是可以在你的代码中被传递和引用的功能性独立模块
 · 闭包能够捕获和存储定义在其上下文中的任何常量和变量的引用，这也就是所谓的闭合并包裹那些常量和变量，因此被称为“闭包”，Swift 能够为你处理所有关于捕获的内存管理的操作。</font>

 - <font size=2>闭包符合3种形式 </font>

  - <font size=2>全局函数是一个有名字但不会捕获任何值的闭包；</font>
  - <font size=2>内嵌函数是一个有名字且能从其上层函数捕获值的闭包；</font>
  - <font size=2>闭包表达式是一个轻量级语法所写的可以捕获其上下文中常量或变量值的没有名字的闭包。 </font>

 - <font size=2>闭包使代码更简洁 </font>
 
  - <font size=2>利用上下文推断形式参数和返回值的类型；</font>
  - <font size=2>单表达式的闭包可以隐式返回；</font>
  - <font size=2>简写实际参数名；</font>
  - <font size=2>尾随闭包语法。</font>



  ### 闭包表达式
  
  > <font size=2>闭包表达式是一种在简短行内就能写完闭包的语法</font>
  ```
  // 语法：
  { (parameters) -> (return type) in
    statements
  }
  ```

  <font size=2>**· Sorted 方法**</font>
  
  > <font size=2>sorted(by:) 方法接收一个接收两个与数组内容相同类型的实际参数的闭包，然后返回一个 Bool</font>
  ```
    let names = ["Chris","Alex","Ewa","Barry","Daniella"]
    // 1）使用普通函数
    func backward(_ s1: String, _ s2: String) -> Bool {
        return s1 > s2
    }
    var reversedNames = names.sorted(by: backward)

    // 2）闭包表达式语法
    reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
        return s1 > s2
    })

    // 3）简写，类型可推断出来
    reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
  ```


  



