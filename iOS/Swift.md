# Swift Apprentice

*Until Swift 5.0*

### Swift Basics

##### Expreesions, Variables, Constants

* 算数运算的两边要不就都加空格或者都不加空格 `2 + 6` or `2+6`, 不能`2+ 6`（会报错）
* 不可变量(Constants) `let` 赋值后不能改变。`let number: Int = 10` (类似c++ `const int number = 10;`)
* 变量(Variables)`var`赋值后可以改变。`var number:Int = 10_0000`(类似c++ `int number = 100000;`)
* 没有自增自减运算符`++` `--`，用`+=` `-=`代替

##### Types & Operations

* 其他语言中，类型隐式转换常常引出bug， swift不能隐式类型转换（编译器报错）,一定需要显式转换。如`integer为Int，decimal为Double` , `integer = Int(decimal)`(类似c++ `integer = (int)decimal`) 。算术运算同理需要先显式转换再算`let ans: Double = Double(integer) * decimal`

* 类型推断(Type inference),整数默认为`Int`，小数默认为`Double` :`let typeInferedInt = 42 /* Type = Int */`， `let typeInferedDouble = 3.14159 /* Type = Double */`；`type(of: a)`来验证a是什么类型

* `as` 也可以进行类型转换 。这三者声明等价`let a = 3 as Double`, `let a: Double = 3`, `let a = Double(3)`。这个3是`Literal value`在程序用到它的时候才会进行转换，单个字3是不代表任何类型的。带小数点的数字是不能代表整数的，直接使用`let a = 3.0`最好（a默认为Double）(double literal)

* Swift中String和Character都是使用双引号`""`。 `let characterA: Character = "a"`, `let stringDoy: String = "Dog"`。 可以写成(string literal)类型推断形式`let stringDog = "Dog"`。PS: Character是没有character literal，只能显示定义。`let s = "a"`（s为长度为1的String类型）

* 字符串声明成var才能改变，let不能改变（但let可以添加到var的变量中去）。 用 `+` 或者 `+=` 链接两个字符串。跟机制Java（是一个immutable的变量，都是拷贝）。String要增加Character类型的变量需要把Character类型变量强制转换成String再拼接。跟前面讲的Swift不能隐式转换类似

* 插入变量值(Interpolation): `\()`。`message = "Hello my name is \(name)!" // "Hello my name is Matt!”`； 可以使用`"""  """`前后3个双引号输出多行的值。

* `Tuple`是可以让一个以上的类型绑定起来，可以没有名字`let pair:(Int, Double) = (2, 3.0)`。也是可以类型推断省去后面的声明。可以通过`.`语法获取对应的值,下标从0开始`let f = pair.0`, `let s = pair.1`。 还可以对每个数命名通过`.名字`获取对应的值。`let coordinates = (x: 2, y: 3)` -> `let x1 = coordinates.x`拿到x

* Tuple技巧：1. 直接接住赋值。 2.用下划线(underscore)`_`忽略不要的元素

  ```swift
  let coordinates3D = (x: 2, y: 3, z: 1)
  let (x3, y3, z3) = coordinates3D  // x3, y3, z3不用声明直接用
  // 等价于下面：
  //let x3 = coordinates3D.x
  //let y3 = coordinates3D.y
  //let z3 = coordinates3D.z
  ```

  ```swft
  let (x4, y4, _) = coordinates3D  // 最后一维被忽略
  ```

* `typealias`别名：给一个类型起一个别名(类似c++ typedef)。`typealias Coordinate = (Int, Int)` ;  `let xy: Coordinates = (2, 4)` 。（同c++ `typedef pair<int, int> Coordinate`)
* Swift的类型(type)是通过协议(Protocols)组织起来的，他们描述了这些类型的公共的操作

##### Basic Control Flow

* `Bool && || ! `都跟c++和Java相同
* `if `的书写方式`if 2 > 1 { }` (相较于c不用写if后面的括号了)，`else if` `while`同理；`let min = a < b ? a : b`三目运算符也可以使用；`repeat...while`(相当于c++的`do...while`)；`break continue`

##### Advance Control Flow

* 可数范围(Countable range)：可以定义一个范围`a...b`相当于`[a, b]`, `a..<b`相当于`[a, b)`；生成[0, 6]随机数(random interlude) `Int.random(in: 1...6)`
* For-Loops跟c++和Java有所区别：格式`for <constant> in <countable range> { }`； `for i in 0..<n {}`(类似c++`for (int i = 0; i < n; i++)`)； 还可以省略迭代变量`for _ in 1...n {}`； 
* 关键字`where`可以跟`for`一起使用，在where后为true的时候才会进入循环体：`for i in 1...count where i % 2 == 1 { }`只有i是奇数才会执行（等价于 `for i in 1...count { if i % 2 == 1 {...} }`）
* `switch` : 语法糖较多，用到时可查看官方文档

##### Funtions

* 函数：`func [函数名](<参数1额外名字_> [参数1名]: [类型], <参数2额外名字_> [参数2名]: [类型] = <默认值>, ...) -> <返回值类型> { }`。(`[]`必写， `<>`选写)。 参数的额外名字若没有可以写成下跨线`_`，这样在函数调用的时候不用写参数名字. 函数只有一句话的时候不用写return

  ```swift
  func multiplyAndDivide(_ number: Int, by factor: Int) -> (product: Int, quotient: Int) {
    return (number * factor, number / factor)
  }
  let results = multiplyAndDivide(4, by: 2)   // 下划线_不用添加参数名字， 没有添加的需要写别名
  let product = results.product
  let quotient = results.quotient
  ```

* 函数参数是值传递(Pass-by-value)，跟Java一样。 另外可以使用一种copy-in copy-out，使用关键字inout ,在参数类型前面添加关键字`inout`可以改变传入的参数，并在调用时参数前添加`&`。（跟c++引用不一样，swift原理是把该值复制函数，在复制出来给调用这个函数的参数）

  ```swift
  func incrementAndPrint(_ value: inout Int) {
    value += 1
    print(value)
  }
  var value = 5
  incrementAndPrint(&value)
  ```

* 可以重载函数(overloading)，函数的函数名字也可一样，参数个数不一样，**返回值也可以不一样**（c++是不能根据返回值不一样重载的）。返回值不一样的时候一定要显示明确类型才可以接受返回值

  ```swift
  // getValue是重载函数，一个返回Int，一个返回String
  let valueInt: Int = getValue()
  let valueString: String = getValue()
  ```

* 函数也可以作为变量赋值给其他变量，并且可以作为参数传递到函数内部

* `Never`作为返回值，不是代表函数返回空，而是在里面不返回了，陷入永久的循环。这是现代移动应用用户跟屏幕交互的重要实现方式，类似runloop。而且类似`fatalError("reason to terminate")`这个函数终止了就是`Never`，陷入永循环，不会退出.

* 自动生成注释`opt + command + /`

##### Optionals

* Swift中是类型安全的语言，若声明了一个变量Int，这个Int保证是一个合法的整数。

* `变量名:类型?`，这称之为可选变量Optional Variables。swift若要表示一个值可能存在也可能不存在，需要声明的时候加上`?`， `var errorCode: Int?` errorCode可选变量可能是一个Int也可以是一个`nil`。而Objc中的nil只是系统用一个0表示一个哨兵（sentinel value），很多值有可能不一定是合法的。`nil`代表空缺值(absence of value)

* 在使用可选变量(Optionals)的时候一定拆箱(Unwrapping)要跟`!`来使用， `errorCode!`。用箱子来理解，Optional Variables相当于这个变量是一个箱子，里面可能是他`本身的类型`，也可能是`nil`. 需要使用拆箱才能获取对应的值. 拆箱(unwrapping)处理Optionals变量有两种方法,1. 可选绑定(Optional binding); 2. Nil合并(Nil coalescing). 下面对两种方法进行展开:

* 可选绑定(Optional binding), 是Swift中常用的语法,且安全的验证可选变量(Optional Variables)是否不为nil, `let 变量名 = 可选变量`**若为true,则可选变量不为nil** ; `if let 变量 = 可选变量 { //不为nil的情况 } else { //为nil }`. 不用使用判断一个是否为nil.  

  ```swift
  // 常用写法
  if let authorName = authorName, let authorAge = authorAge {  // ,号可以分隔很多条件 是 &&
    print("The author is \(authorName) who is \(authorAge) years old.")
  } else {
    print("No author or no age.")
  }
  
  // 可以这样写,但不推荐,因为前者比较好
  if authorName != nil {
    print("Author is \(authorName!)")  // authorName! 需要使用'!', 而前者是不需要的
  } else {
    print("No author.")
  }
  ```

* `guard`关键字跟if作用一样,但是在语义上有提前退出的作用,推荐使用. 理解为,下执行下面一段代码的前面保证这个可选变量是存在的(即不是nil), 才往下执行, 通常若是nil会直接return.  (类似c++, assert在代码前面做断言)

  ```swift
  func guardMyCastle(name: String?) {
    guard let castleName = name else {
      print("No castle!")
      return
    }
  
    // At this point, `castleName` is a non-optional String
  
    print("Your castle called \(castleName) was guarded!")
  }
  ```

* `var 变量 = 可选变量 ?? 默认值`, `??` 是另一种拆箱方法成为Nil coalescing. 在可选变量(Optional Variables)是nil的时候给定一个默认值. `var mustHaveResult = optionalInt ?? 0`, 这样获得的变量`mustHaveResult`一定会有合法的值. 

### Collection Types

##### Array, Dictionaries, Sets

* **Array**. 跟其他语言类似. 不可变数组的声明为`let`, 可变数组声明成`var`.  声明方式和作为函数参数传递 `数组名: [数组类型]`

  ```swift
  /// 数组创建
  let evenNumbers = [2, 4, 6, 8]   // 类型推断创建数组
  var players: [String] = []   // 显式创建数组
  
  /// 访问成员属性 property
  evenNumbers[1]
  evenNumbers.isEmpty   // c++ empty() 和 java isEmpty() 是方法而不是成员属性
  players.count
  // last属性返回的是一个Optional Variable, 可能是nil. 把Any可以代表任何类型的东西,编译器不报错
  print(evenNumbers.last as Any)  // > Optional(8)
  
  /// 成员方法
  let minV = evennumbers.min()
  
  // 遍历 for-each
  for player in player {  }
  // 根据下标遍历 for
  for (index, player) in players.enumerated() {  }
  // c++ 式的写法, 不推荐这样写 可以用上面的2中
  for i in 0..<players.count { print(players[i]) }  
  ```

  常用方法: `+=` `append, `, `insert`, `removeLast`

* **Dictionaries**:  不可变dic的声明为`let`, 可变数组声明成`var`. 声明方式和作为函数参数传递`字典名字:[key类型: value类型]`

  ```swift
  /// Dictionary创建
  var namesAndScores = ["Anna": 2, "Brian": 2, "Craig": 8, "Donna": 6]  // 类型推断初始化
  namesAndScores = [:]  // !!!只有在已经初始化过的dic 才可以这样初始化, 因为已经知道对应的类型了
  
  var pairs: [String: Int] = [:]  // 正常初始化是要声明类型的
  
  /// 访问成员属性
  namesAndScores["Greg"] // 若不在字典中则返回nil
  namesAndScores.isEmpty // false
  namesAndScores.count // 4
  
  // 更新或添加方法 2种
  bobData.updateValue("CA", forKey: "state")
  bobData["city"] = "San Francisco"   // 个人喜欢第二种,因为跟c++比较像
  
  // 删除 2种方法, 两种方法等价
  bobData.removeValue(forKey: "state")  // 个人喜欢这一种,语义比较明确
  bobData["city"] = nil  // !! 把value置成nil的话key也会在字典中删除
  ```

* **Sets**: 不可变dic的声明为`let`, 可变数组声明成`var`. 声明方式和作为函数参数传递`SetName: Set<Type>`

  ```swift
  /// Set创建
  let setOne: Set<Int> = [1, 2] 
  var someSet = Set([1, 2, 3, 1])  // Set literals 生成列表式创建
  
  let someArray = [1, 2, 3, 1]  // 通过数组创建
  var explicitSet: Set<Int> = [1, 2, 3, 1]
  
  /// 判断是否在Set中
  someSet.contains(1)  // > true
  
  /// 添加
  someSet.insert(5)
  
  /// 删除
  someSet.remove(1)
  ```

##### Collection Iteration with Closures 

* **闭包(closure)**, 地位等同于Objc的Block.  直观理解就是没有名字的函数. 可以作为函数参数 `closureName:(Type1, Type2,...) -> Type`, 用`{ }`括起来 . **简写的时候`$0` `$1`代表的是第几个参数, 从0开始数.**

  ```swift
  // multiplyClosure: (Int, Int) -> Int
  var multiplyClosure = { (a: Int, b: Int) -> Int in
    return a * b
  }
  let result = multiplyClosure(4, 2)
  
  // 简写
  multiplyClosure = { (a, b) -> in a * b}  // 缩掉声明
  multiplyClosure = {$0 * $1}   // 缩掉参数
  ```

  例子:

  ```swift
  // 定义一个函数, 参数operation传入一个函数
  func operateOnNumbers(_ a: Int, _ b: Int, operation: (Int, Int) -> Int) -> Int {
    let result = operation(a, b)  // 调用operation函数
    //print(result)
    return result
  }
  
  // 通过传入函数
  func addFunction(_ a: Int, _ b: Int) -> Int {
    a + b
  }
  operateOnNumbers(4, 2, operation: addFunction)
  
  // 传入闭包的4种写法 (从复杂到简洁) 都非常常用
  operateOnNumbers(4, 2, operation: { (a: Int, b: Int) -> Int in
    return a + b
  })
  operateOnNumbers(4, 2, operation: { $0 + $1 })
  operateOnNumbers(4, 2, operation: +)
  operateOnNumbers(4, 2) {  // 通过括号来匹配对应的闭包参数
    $0 + $1
  }
  ```

* 闭包返回空值的时候不能像写函数一样不写返回值, 要写**Void**, 要写成`let voidClosure: () -> Void = { }` ,这个闭包没有参数,也没有返回值.  PS: **Void**在系统有别名**()**`typealias Void = ()`. 所以上述表达式也可以写成`let voidClosure: () -> () = { }`

* 闭包可以访问自己所在括号外部和内部中的变量和常量(用c语言的说法可以捕获跟这个闭包在相同作用域中的变量), 而且可以改变这些变量. (在Objc中block也可以捕获外部的变量,但是是副本,要改变外部的变量需要加`__block`声明)

  ```swift
  var counter = 0
  let incrementCounter = {  
    counter += 1   // 可以捕获(capture)这个counter变量是因为这个couter变量跟这个闭包在同一个作用域
  }
  incrementCounter()
  incrementCounter()
  // counter == 2
  ```

  实现每次调用countingClosure函数,该函数的返回值都会加1 (第一次调用返回1)

  ```swift
  func countingClosure() -> () -> Int {
    var counter = 0
    let incrementCounter: () -> Int = {
      counter += 1
      return counter
    }
    return incrementCounter
  }
  
  let counter1 = countingClosure()  // 将函数赋值
  let counter2 = countingClosure()  // 将函数赋值
  // 二者相互独立不影响
  counter1() // 1
  counter2() // 1
  counter1() // 2
  counter1() // 3
  counter2() // 2
  ```

* 常用通过闭包实现排序, 类似Java和C++的Lambda表达式. 注意是大括号 `{}`

  ```swift
  let names = ["ZZZZZZ", "BB", "A", "CCCC", "EEEEE"]
  names.sorted()   // sorted() 函数是返回一个排好序的copy. 因为name是let不可变的所以不能使用sort()
  
  let sortedByLength: [String] = names.sorted {
      $0.count > $1.count   // 从打到小排序  跟c++类似一样.  注意是大括号
  }
  ```

* 闭包在函数式编程(functional)的应用`filter`, `map` `reduce`等等, 并且可以通过函数式编程实现懒容器(Lazy collections)

##### Strings ???? 



### Building Your Own Types

##### Structures

* 结构体赋值是copy-on-assignment, 是copy的方式. 是value types. 而类是引用类型(reference type)
* 结构体可以**遵守协议**, **实现方法**, 但是**不能继承**
* 结构体一定需要初试化(可以不用构造方法`init`, 像函数一样指定名字就行), 但也可以在结构体中给定默认值, 可以用`.`来访问成员(属性, 方法)
* 其实Int, Double, String, Bool, Array, Dictionary等等都是一个swift语言内部实现的结构体, 并遵循了一些协议. 
* 结构体可以遵从协议. 如协议中的`CustonStringConvertiable`中有一个方法`description`类似Java中的重写toString方法.  `struct a: CustonStringConvertiable` 这个结构体就遵循了某种协议

##### Properties

* **默认值(Default value)**, 结构体属性, 可以设置默认值, 初试化的时候不用传值

* !! 什么都不加声明属性默认为**存储属性(stored properties)**, 还有一种叫**计算属性(Computed properties)**.  存储属性(stored properties)使用的是属性观察者`willSet & didSet`, 而计算属性(Computed properties)是使用的`getter & setter`. 二者尽管语法很像, 但是是完全不同的概念.

* **计算属性(Computed properties)**, 改属性是不用初试化的, 是根据其他属性计算得来的, 可以理解为跟其他指定属性**保持一种关系约束**. (因为结构体的其他值是一定要初始化的, 所以该属性声明后一定存在值);  `getter & setter`**, getter是根据其他属性计算得到, 而setter是通过用户给具体的值给这个计算属性, 然后其他被绑定的值就可以获得改变.**

  ```swift
  /// 版本一: read-only computed property
  struct TV {
    var height: Double
    var width: Double  // stored properties
      
    // Computed properties
    var diagonal: Int {
        let result = (height * height + width * width).squareRoot().rounded()
        return Int(result)
    }
  }
  
  // 版本二: getter & setter  read-write computed property
  struct TV {
    var height: Double
    var width: Double
    
    var diagonal: Int {
      get {   // 根据height和width计算得到
        let result = (height * height + width * width).squareRoot().rounded()
        return Int(result)
      }
      set {  // 通过该值改变height和width
        let ratioWidth = 16.0
        let ratioHeight = 9.0
        let ratioDiagonal = (ratioWidth * ratioWidth + ratioHeight * ratioHeight).squareRoot()
        height = Double(newValue) * ratioHeight / ratioDiagonal  // height值被改变
        width = height * ratioWidth / ratioHeight   // width的值被改变
      }
    }
  }
  ```

* **属性观察者(Property Observers) :** `willSet`和`didSet` 跟`getter`和`setter`很像,. 只能观察**存储属性(stored properties)**. 类似于**触发器**。用来监视属性的除初始化之外的属性值变化，当属性值发生改变时可以对此作出响应:

  1. 不仅可以在属性值改变后触发didSet，也可以在属性值改变前触发willSet
  2. 给属性添加观察者必须要声明清楚属性类型，否则编译器报错
  3. willSet可以带一个newName的参数(`willSet(name) { }`)，没有的话，该参数默认命名为newValue
  4. didSet可以带一个oldName的参数，表示旧的属性，不带的话默认命名为oldValue
  5. 属性初始化时，willSet和didSet不会调用。**只有在初始化上下文之外**，当设置属性值时才会调用
  6. 即使是设置的值和原来值相同，willSet和didSet也会被调用

  ```swift
  class People {
      var nickName: String = ""
      
      //带属性监视器的普通属性
      var age:Int = 0 {
          //我们需要在age属性变化前做点什么
          willSet {
              print("Will set an new value \(newValue) to age")
          }
          //我们需要在age属性发生变化后，更新一下nickName这个属性
          didSet {
              print("age filed changed form \(oldValue) to \(age)")
              if age<10
              {
                  nickName = "Little"
              }else
              {
                  nickName = "Big"
              }
          }
      }
  }
  
  me.age = 30
  // Will set an new value 30 to age
  // age filed changed form 0 to 30 
  ```

* **类型属性(Type properties)**, (类似Java和c++的类属性, 就是通过`类名.属性名`访问) . 使用 `static` 关键字 : `static var highestLevel = 1`. 声明了这个结构体属性, 通过`结构体名.属性名访问`. 它不属于具体某一个实例,而是属于整个结构体的. 在结构体内部访问static变量要使用`Self.变量` (Self S大写) 普通属性小写

* **懒属性 (lazy stored property)**`lazy`关键字, 声明属性`lazy ` . `lazy var 变量名 = { 计算的值 }()` 跟计算属性不同, 他是在一开始初始化的时候不加载, 等什么时候使用的时候才会加载. 就是**懒加载**. **只加载一次.** 通常需要重写init构造函数, 不要初始化这个变量 *PS: 注意, 懒加载要一定用`var`声明, 原因: 懒属性天生就是变量, 一开始初始化时候没有加载, 但是用到它的时候才加载. 尽管只加载一次, 但也是用var声明*

