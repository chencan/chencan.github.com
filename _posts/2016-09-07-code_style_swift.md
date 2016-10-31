---
layout: post
title: "Swift style guide"
description: ""
category: 其他
tags: [style, Swift, guide]
author: 陈灿
---  


# Swift style guide.

## Introduction

之前有一个[Objective-C style guide.](https://github.com/chencan/wonderful-objective-c-style-guide#project-organization)，这一篇是针对swift的一个补充。

## Credits

[API Design Guidelines](https://swift.org/documentation/api-design-guidelines)是苹果专门针对API的一个规范，本规范涉及到一些相关的内容，大都是保持和苹果的规范一致。

它绝大部分内容，来自[The Official raywenderlich.com Swift Style Guide.](https://github.com/raywenderlich/swift-style-guide)，并加入了自己的一些偏好。  

同时，Swift语言在快速的发展中，这个规范也会随着Swift的发展、以及我们对Swift更多的使用和了解，持续地进行修改和完善。



## Table of Contents
* [Naming](#naming)
  * [Protocols](#protocols)
  * [Enumerations](#enumerations)
  * [Generics](#generics)
  * [Class Prefixes](#class-prefixes)
  * [Language](#language)
* [Code Organization](#code-organization)
  * [Protocol Conformance](#protocol-conformance)
  * [Unused Code](#unused-code)
  * [Minimal Imports](#minimal-imports)
* [Spacing](#spacing)
* [Comments](#comments)
* [Classes and Structures](#classes-and-structures)
  * [Use of Self](#use-of-self)
  * [Protocol Conformance](#protocol-conformance)
  * [Computed Properties](#computed-properties)
  * [Final](#final)
* [Closure Expressions](#closure-expressions)
* [Types](#types)
  * [Constants](#constants)
  * [Optionals](#optionals)
  * [Struct Initializers](#struct-initializers)
  * [Lazy Initialization](#lazy-initialization)
  * [Type Inference](#type-inference)
  * [Syntactic Sugar](#syntactic-sugar)
* [Functions vs Methods](#functions-vs-methods)
* [Memory Management](#memory-management)
* [Access Control](#access-control)
* [Golden Path](#golden-path)
  * [Failing Guards](#failing-guards)
* [Parentheses](#parentheses)
* [Other Swift Style Guides](#other-swift-style-guides)







<h2 id="naming"> Naming </h2>

classes, structures, enumerations 和 protocols采用首字母**大**写的驼峰命名法，method names and variables采用首字母**小**写的驼峰命名法。

**Preferred:**

```swift
private let maximumWidgetCount = 100

class WidgetContainer {
  var widgetButton: UIButton
  let widgetHeightPercentage = 0.85
}
```

**Not Preferred:**

```swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
  var wBut: UIButton
  let wHeightPct = 0.85
}
```

缩写应该避免，但如URL、ID这种常见的缩写可以使用。  

[API Design Guidelines](https://swift.org/documentation/api-design-guidelines/#follow-case-conventions)中提到，如果使用这些缩写，字母应该全为大写或者小写。Examples:

**Preferred**

```swift
let urlString: URLString
let userID: UserID
```

**Not Preferred**

```swift
let uRLString: UrlString
let userId: UserId
```

使用argument label，替代注释，使代码self-documenting，可读性更强。



```swift
func convertPointAt(column: Int, row: Int) -> CGPoint
func timedAction(afterDelay delay: NSTimeInterval, perform action: SKAction) -> SKAction!

// would be called like this:
convertPointAt(column: 42, row: 13)
timedAction(afterDelay: 1.0, perform: someOtherAction)
```

### Protocols

按照苹果的API Design Guidelines，Protocols名字可以使用名词来描述这个Protocols的内容，比如`Collection`, `WidgetFactory`。  

或以-ing、-able结尾来描述Protocols实现的一些功能，比如: `Equatable`, `Resizing`。

### Enumerations

按照苹果的API Design Guidelines，枚举值使用小写开头的驼峰命名法，也就是lowerCamelCase。


```swift
enum Shape {
  case rectangle
  case square
  case rightTriangle
  case equilateralTriangle
}
```


### Class Prefixes

在Swift里面，每一个module都是一个namesapce。而在ObjC里没有namespace的概念，只是在每个类名前面添加前缀，比如NSArray。  

当不同的module有同名的类名时，需要指明module name。

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```


### Generics
 
范型的类型名，应该是描述性的名词、upperCamelCase。如果不能起一个有意义的关系或角色名称，可以使用`T`、`U`或`V`。
 
**Preferred:**

```swift
struct Stack<Element> { ... }
func writeTo<Target: OutputStream>(inout target: Target)
func max<T: Comparable>(x: T, _ y: T) -> T
```

**Not Preferred:**

```swift
struct Stack<T> { ... }
func writeTo<target: OutputStream>(inout t: target)
func max<Thing: Comparable>(x: Thing, _ y: Thing) -> Thing
```

### Language

使用美国英语。

**Preferred:**

```swift
let color = "red"
```

**Not Preferred:**

```swift
let colour = "red"
```

## Code Organization


多使用extensions来组织代码。每个  extensions使用`// MARK: -`来分隔开。

### Protocol Conformance
 
当一个类遵守一个协议时，使用extensions来组织代码。

**Preferred:**

```swift
class MyViewcontroller: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewcontroller: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

**Not Preferred:**

```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

### Unused Code

能删除的代码，都删掉。

**Not Preferred:**

```swift
override func didReceiveMemoryWarning() {
   super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
   // #warning Incomplete implementation, return the number of sections
   return 1
}

override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}

```

**Preferred:**

```swift
override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```

### Minimal Imports

尽可能减少依赖和imports。


## Spacing


* 缩进使用2个空格:
* **方法体的花括号需要在新的一行开启，在新的一行关闭**。而其它花括号(`if`/`else`/`switch`/`while` etc.)，加入一个空格后在行尾开启，在新一行关闭（Xcode默认）。  
* 提示：⌘A选中代码后使用Control-I (或者菜单Editor\Structure\Re-Indent)来调整缩进.

**Preferred:**

```swift
if user.isHappy {
  // Do something
} else {
  // Do something else
}
```

**Not Preferred:**

```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```

* methods之间只留一个空行。methods内部，使用空行来分隔不同功能的代码，为不同的section。一个method内部的section不宜太多，否则应该考虑分拆。  
* 冒号左边没有空格，右边有一个空格。**Exception：`? :` 和 `[:]`**。

**Preferred:**

```swift
class TestDatabase: Database {
  var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

**Not Preferred:**

```swift
class TestDatabase : Database {
  var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```

## Comments

尽可能避免大量使用注释，好的代码应该尽可能是self-documenting。

如果需要注释，它只用来解释**为什么**这段代码要这么写，而不是解释代码的逻辑。  
并且是正确的，代码变化时也需要马上更新，不能有误导。


**Exception: 上面两条不适用于生成文档用的注释.**

## Classes and Structures

### Which one to use?

Structs是[value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144)。  

而Classes是[reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). 

有些时候，只需要Structs就够了。但有些Class，由于历史原因，被弄成类，如`NSDate`、`NSSet`等。

### Example definition

下面是一个比较规范的Class定义：

```swift
class Circle: Shape {
  var x: Int, y: Int
  var radius: Double
  var diameter: Double {
    get {
      return radius * 2
    }
    set {
      radius = newValue / 2
    }
  }

  init(x: Int, y: Int, radius: Double) {
    self.x = x
    self.y = y
    self.radius = radius
  }

  convenience init(x: Int, y: Int, diameter: Double) {
    self.init(x: x, y: y, radius: diameter / 2)
  }

  func describe() -> String {
    return "I am a circle at \(centerString()) with an area of \(computeArea())"
  }

  override func computeArea() -> Double {
    return M_PI * radius * radius
  }

  private func centerString() -> String {
    return "(\(x),\(y))"
  }
}
```

上面的例子，给我们演示了这些规范：

 + 冒号在用于指明类型时，左边没有空格，右边有一个空格。
 + 当一组变量、常量有关联时，定义在一行里。
 + 函数修饰符`internal`是缺省值，可以省略。重载一个函数时，访问修饰符也可以省略掉。


### Use of Self

避免使用self来访问属性。除非需要区分函数参数和属性。

```swift
class BoardLocation {
  let row: Int, column: Int

  init(row: Int, column: Int) {
    self.row = row
    self.column = column
    
    let closure = {
      print(self.row)
    }
  }
}
```

### Computed Properties

Computed property一般是只读，同时省略get clause。get clause只是当set clause存在时才需要写。


**Preferred:**

```swift
var diameter: Double {
  return radius * 2
}
```

**Not Preferred:**

```swift
var diameter: Double {
  get {
    return radius * 2
  }
}
```

### Final

当一个类不想被继承时，使用`final`关键字。Example:

```swift
// Turn any generic type into a reference type using this Box class.
final class Box<T> {
  let value: T 
  init(_ value: T) {
    self.value = value
  }
}
```

## Closure Expressions


方法的参数列表最后一参数类型为闭包时，可以使用尾闭包。但只在只存在一个闭包参数时才使用尾闭包。

**Preferred:**

```swift
UIView.animateWithDuration(1.0) {
  self.myView.alpha = 0
}

UIView.animateWithDuration(1.0,
  animations: {
    self.myView.alpha = 0
  },
  completion: { finished in
    self.myView.removeFromSuperview()
  }
)
```

**Not Preferred:**

```swift
UIView.animateWithDuration(1.0, animations: {
  self.myView.alpha = 0
})

UIView.animateWithDuration(1.0,
  animations: {
    self.myView.alpha = 0
  }) { f in
    self.myView.removeFromSuperview()
}
```

只有一个表达式的、用来返回值的闭包，可以省略return。

```swift
attendeeList.sort { a, b in
  a > b
}
```


## Types

尽可能使用Swift原生类型，而不是使用ObjC的NS类型。

**Preferred:**

```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**Not Preferred:**

```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

但是有些情况例外，比如在写Sprite Kit代码时，用`CGFloat`可以减少转换。

### Constants

尽可能使用let，只有在需要使用变量的时候使用var。

**Tip:** 有一个办法能达到上面的目的，就是自己只写let，让编译器帮你确定哪些需要改成var。

使用`static let`定义类常量，而不是实例常量，或者全局常量。

**Preferred:**

```swift
enum Math {
  static let e  = 2.718281828459045235360287
  static let pi = 3.141592653589793238462643
}

radius * Math.pi * 2 // circumference

```
**Note:** 使用枚举定义常量的好处是让常量定义在特定的命名空间。

**Not Preferred:**

```swift
let e  = 2.718281828459045235360287  // pollutes global namespace
let pi = 3.141592653589793238462643

radius * pi * 2 // is pi instance data or a global constant?
```

### Optionals

当访问一个optional value前，需要访问多个optional value时，使用optional chaining：

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

访问一个optional value后，需要执行多个操作，可以使用optional binding：

```swift
if let textContainer = self.textContainer {
  // do many things with textContainer
}
```

给optional变量命名时，不要使用类似`optionalString`或`maybeView`这样的方式，因为optional-ness已经在类型声明中体现。

相对应的，使用unwrapped value时，避免使用unwrappedView`或`actualLabel`，使用optional变量名就可以了。

**Preferred:**

```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, volume = volume {
  // do something with unwrapped subview and volume
}
```

**Not Preferred:**

```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
  if let realVolume = volume {
    // do something with unwrappedSubview and realVolume
  }
}
```

### Struct Initializers

使用Swfit原生的struct initializers，而不是遗留的CGGeometry constructors。

**Preferred:**

```swift
let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)
let centerPoint = CGPoint(x: 96, y: 42)
```

**Not Preferred:**

```swift
let bounds = CGRectMake(40, 20, 120, 80)
let centerPoint = CGPointMake(96, 42)
```

同样的，使用struct-scope constants `CGRect.infinite`， `CGRect.null`， etc， 而不是global constants `CGRectInfinite`, `CGRectNull`, etc。



### Type Inference


对于适用于类型推断的地方，使用类型推断。

**Preferred:**

```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

**Not Preferred:**

```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
```


### Syntactic Sugar

尽可能使用语法糖。

**Preferred:**

```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Not Preferred:**

```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## Functions vs Methods

不依附于任何class或type的函数被称为free function，应该尽量避免使用，因为不太好找到这个方法。

**Preferred**

```swift
let sorted = items.mergeSort()  // easily discoverable
rocket.launch()  // clearly acts on the model
```

**Not Preferred**

```swift
let sorted = mergeSort(items)  // hard to discover
launch(&rocket)
```

**Free Function Exceptions**

```swift
let tuples = zip(a, b)  // feels natural as a free function (symmetry)
let value = max(x,y,z)  // another free function that feels natural
```

## Memory Management

用下面prefered的方式，避免循环引用。

**Preferred**

```swift
resource.request().onComplete { [weak self] response in
  guard let strongSelf = self else { return }
  let model = strongSelf.updateModel(response)
  strongSelf.updateUI(model)
}
```

**Not Preferred**

```swift
// might crash if self is released before response returns
resource.request().onComplete { [unowned self] response in
  let model = self.updateModel(response)
  self.updateUI(model)
}
```

**Not Preferred**

```swift
// deallocate could happen between updating the model and updating UI
resource.request().onComplete { [weak self] response in
  let model = self?.updateModel(response)
  self?.updateUI(model)
}
```

## Access Control

访问修饰符应该放在靠前的位置，前面只能有`static`、`@IBAction` 和 `@IBOutlet`。

**Preferred:**

```swift
class TimeMachine {  
  private dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```

**Not Preferred:**

```swift
class TimeMachine {  
  lazy dynamic private var fluxCapacitor = FluxCapacitor()
}
```

## Golden Path

嵌套的`if`会让代码的缩进层次不齐（整齐的缩进被称作Golden Path），会让代码可读性变差，使用`guard`来做函数输入合法性检查，可以减少if嵌套。

**Preferred:**

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  guard let context = context else { throw FFTError.noContext }
  guard let inputData = inputData else { throw FFTError.noInputData }
    
  // use context and input to compute the frequencies
    
  return frequencies
}
```

**Not Preferred:**

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

  if let context = context {
    if let inputData = inputData {
      // use context and input to compute the frequencies

      return frequencies
    }
    else {
      throw FFTError.noInputData
    }
  }
  else {
    throw FFTError.noContext
  }
}
```

**Preferred:**

```swift
guard let number1 = number1, number2 = number2, number3 = number3 else { fatalError("impossible") }
// do something with numbers
```

**Not Preferred:**

```swift
if let number1 = number1 {
  if let number2 = number2 {
    if let number3 = number3 {
      // do something with numbers
    }
    else {
      fatalError("impossible")
    }
  }
  else {
    fatalError("impossible")
  }
}
else {
  fatalError("impossible")
}
```

### Failing Guards

Guard检查失败执行的语句应该退出当前方法，并且应该只有一条语句，如`return`, `throw`, `break`, `continue`, and `fatalError()`。如果需要多行语句，考虑使用`defer`。


## Parentheses

保住条件语句的圆括号应该省略。

**Preferred:**
```swift
if name == "Hello" {
  print("World")
}
```

**Not Preferred:**
```swift
if (name == "Hello") {
  print("World")
}
```



<h2 id="other-swift-style-guides">Other Swift Style Guides</h2>

上面的规范可能你不太喜欢，或者没有涉及到你需要的某些方面，可以参考下面的内容:

* [GitHub](https://github.com/github/swift-style-guide)

