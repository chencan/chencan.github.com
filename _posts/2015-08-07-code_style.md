---
layout: post
title: "Objective-C style guide"
description: ""
category: 其他
tags: [style, Objective-C, guide]
author: 陈灿
---  



# Objective-C style guide.

## Introduction

这个style guide规范描述了我们iOS开发团队喜欢的Objectiv-C编程习惯。

代码规范的意义，在于提高团队各个成员写的代码的一致性和可读性。一致性能减少工程师编写代码风格的困惑和犹豫；良好的可读性更是能帮助我们浏览其他人代码、以便合作和code review。

**它并不是去定义the right way or the wrong way，而是达成一个需要遵守的共识，it's about agreeing on doing things the same way。**


这个规范可能比其他一般的style guide规范涵盖了更多的方面和细节。

欢迎联系我，给出你关于这个规范的想法。

## Credits

它绝大部分内容，来自[Wonderful Objective-C style guide](https://github.com/markeissler/wonderful-objective-c-style-guide)，并加入了自己的一些偏好。

## Background

此规范也参考了一些苹果官方文档。如果在此规范里找不到你关心的内容，可以尝试在下面的文档里寻找：

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Tools

### VVDocumenter-Xcode
[VVDocumenter-Xcode](https://github.com/onevcat/VVDocumenter-Xcode)是一个帮我们写注释（用于生成文档）的Xcode插件。可以使用[Alcatraz Xcode package manager](http://alcatraz.io)安装后重启Xcode。

### clang-format
1. Download [clang-format 3.5.2](http://llvm.org/releases/3.5.2/clang+llvm-3.5.2-x86_64-apple-darwin.tar.xz)
2. Copy clang-format into folder /usr/local/bin/
3. Copy [pre-commit](https://github.com/chencan/wonderful-objective-c-style-guide/blob/master/pre-commit) into folder .gits/hooks/
4. chmod +x pre-commit
5. Change [.clang-format](https://github.com/chencan/wonderful-objective-c-style-guide/blob/master/clang-format) if you want to change the style.

### Cocoapods

当需要一些“轮子”时，可以先在现有的代码里面找找（Podfile或者Libraray）。没有的话，再用pod search或者github找找，看有没有代码质量高的、大家广泛使用的（watch、star、fork数量多）、比较活跃、比较友好的第三方代码，看看实现原理以及使用方法，是否适合我们的需求。

或可以通过继承，满足我们的需求。如果第三方的代码有bug，或者需要改进，可以自己fork后修改使用（顺便可以给作者发一个pull request）

在你有更好的实现、有时间的前提下，可以自己造更好的“轮子”。

Podfile.lock 有变动表示使用的第三库版本有变化，如果采用新的版本，需要检查新版本的第三方库没有问题。

尽量使用最新版本的第三方库。

但发包前对此文件的修改要多尤其小心。最好发完包后，自己看看新版本有无问题，如果有必要再让测试看看。   


#### Cocoapods命令解释
* pod update: Updating local specs repositories,  更新工程中的第三方库
* pod update --no-repo-update：使用local specs repositories中最新的版本
* pod install: Updating local specs repositories,  安装podfile.lock中规定版本
* pod install --no-repo-update：当podfile.lock中规定的版本，不在local specs repositories中，install会失败  


#### Cocoapods使用规范
* pod update命令，由小组某个成员，开启定时任务：  
  30 12 * * 1 cd /Users/Can/Workspace/zuche;pod update;  
  //用来检查第三方库是否有更新，并升级库。这个是为了定期检查第三方库是否有新版本。  
  //一个小组成员做这件事就可以       
  //发现Podfile.lock变了，表示第三方代码版本升级了，应该了解一下升级的内容，另外上线前不要轻易升级影响功能第三方代码  
* pod install命令，由小组其他成员，开启定时任务：  
  30 12 * * * cd /Users/Can/Workspace/zuche;pod install;  
  //主要用来Updating local specs repositories，防止install失败  
* pull完代码推荐使用pod install，pod install --no-repo-update，后者速度更快可能会报错
* git checkout 老代码，推荐使用pod install --no-repo-update，因为是老代码，一般不会报错
* **Podfile.lock任何情况下不能删除**   
* pod update --no-repo-update，只用在pod install不成功，但需要马上编译成功的情况



## Table of Contents

* [Language](#language)
* [Project Organization](#project-organization)
* [Code Organization](#code-organization)
* [Line Wrapping (Code Width)](#line-wrapping-(code-width))
* [Spacing](#spacing)
* [Comments](#comments)
* [Documentation](#documentation)
	* [Class Documentation](#class-documentation)
	* [Constant Documentation](#constant-documentation)
	* [Property Documentation](#property-documentation)
	* [Class Method Documentation](#class-method-documentation)
	* [Instance Method Documentation](#instance-method-documentation)
* [Naming](#naming)
    * [Naming Conventions for Methods and Variables](#naming-conventions-for-methods-and-variables)
    * [Naming Conventions for Constants and Macros](#naming-conventions-for-constants-and-macros)
    * [Naming Conventions for Enumerated Types](#naming-conventions-for-enumerated-types)
    * [Underscores](#underscores)
* [Methods](#methods)
* [Variables](#variables)
* [Property Attributes](#property-attributes)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Literals](#literals)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Switch Statements and Case Label Blocks](#switch-statements-and-case-label-blocks)
* [Private Properties](#private-properties)
* [Booleans](#booleans)
* [Conditionals](#conditionals)
	* [Conditional Expressions](#conditional-expressions)
    * [Ternary Operator](#ternary-operator)
* [Return Statements](#return-statements)
* [Init Methods](#init-methods)
* [Class Constructor Methods](#class-constructor-methods)
* [CGRect Functions](#cgrect-functions)
* [Error handling](#error-handling)
* [Singletons](#singletons)
* [Line Breaks](#line-breaks)
* [Warnings](#warnings)
* [Other Objective-C Style Guides](#other-objective-c-style-guides)


<h2 id="language">Language</h2>

使用美国英语，而非英国等国家英语。

**Preferred:**

```objc
UIColor *myColor = [UIColor whiteColor];
```

**Not Preferred:**

```objc
UIColor *myColour = [UIColor whiteColor];
```


<h2 id="project-organization">Project Organization</h2>

Xcode工程，采用类似下面的工程文件结构。

```
Your_Project
  |-- AppDelegate.h
  |-- AppDelegate.m
  |-- Images.xcassets
  |-- MainMenu.xib
  |-- Supporting Files
  |-- Models
  |-- Views
  |-- Controllers
  |-- Stores
  |-- Helpers
  |-- ...
```

并且保持对应的物理文件系统路径，方便在finder里面查找，也能保持finder内文件整齐。因此需要加入新的group时，需要先在文件系统里新建一个folder，然后add这个folder。

<h2 id="code-organization">Code Organization</h2>

使用`#pragma mark -`对.m文件里面的Methods进行归类。

```objc
#pragma mark - Class Methods

+ (ClassObject)classWithDefaults;
+ (ClassObject)convertToJSON;
+ (ClassObject)convertToXML;

#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

<h2 id="language">line-wrapping-(code-width)</h2>

合适的规范一般并不需要一行能容纳无限长度的代码，80列的限制反而能提醒你，可能你的代码可读性不太好。

```
Preferences->Text Editing->Page Guide at column:
```

As seen here:
![Xcode Page Guide Pref](http://mix-pub-dist.s3-website-us-west-1.amazonaws.com/objective-c-style-guide/img/pref_page_guide_sm-2.png)

<h2 id="spacing">Spacing</h2>

* 缩进使用4个空格（Xcode默认）。
* 操作符左右留空格。

**Preferred:**
```objc
if (self.changeEable && self.shouldChange)
sum = count1 + count2
```
**Not Preferred:**
```objc
if (self.changeEable&&self.shouldChange)
sum=count1+count2
```

* **方法体的花括号需要在新的一行开启，在新的一行关闭**。而其它花括号(`if`/`else`/`switch`/`while` etc.)，加入一个空格后在行尾开启，在新一行关闭（Xcode默认）。

**Preferred:**

```objc
if (user.isHappy) {
  // Do something
} else {
  // Do something else
}
```

**Not Preferred:**

```objc
if (user.isHappy) 
{
    // Do something
} else 
{
    // Do something else
}
```

 在使用*else*或者*else if*，它们前后的大括号应该是在一行，否则不太好看：
 
**Not Preferred:**

```objc
if (user.isHappy) 
{
  // Do something
} 
else 
{
  // Do something else
}
```
* methods之间只留一个空行。
* 尽量使用auto-synthesis。如果需要使用`@synthesize`，每个property需要新开一行， `@dynamic`也是需要新开一行。
* 当methods里面需要传入block时，不要使用冒号对齐对方式：

**Preferred:**

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**Not Preferred:**

```objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

<h2 id="comments">Comments</h2>

如果需要注释，它只用来解释**为什么**这段代码要这么写，并且是正确的，代码变化时也需要马上更新，不能有误导。

尽可能避免大量使用注释，好的代码应该尽可能是self-documenting。

**Exception: 上面两条不适用于生成文档用的注释.**

<h2 id="documentation">Documentation</h2>

在头文件中的以下内容，需要写注释用来生成文档（可以使用appledoc）。

[VVDocumenter-Xcode](https://github.com/onevcat/VVDocumenter-Xcode)也可以帮我们。

* Classes
* Prototype
* Constants
* Properties
* Class Methods
* Instance Methods

<a id="class-documentation">Class Documentation</a>

```objc
-- **EXAMPLE PENDING** --
```

<a id="constant-documentation">Constant Documentation</a>

```objc
/**
 *  @memberof PDFReaderConfig
 *  Default value for bookmarksEnabled: TRUE
 */
extern const BOOL kPDFReaderDefaultBookmarksEnabled;
```

<a id="property-documentation">Property Documentation</a>

```objc
/**
 *  Enable/disable bookmark support.
 *
 *  @see kPDFReaderDefaultBookmarksEnabled
 */
@property (nonatomic, readwrite, unsafe_unretained, getter=isBookmarksEnabled)
    BOOL bookmarksEnabled;
```

<a id="class-method-documentation">Class Method Documentation</a>

```objc
/**
 * Returns the shared PDFReaderConfig instance, creating it if necessary.
 *
 * @return The shared PDFReaderConfig instance.
 */
+ (instancetype)sharedConfig;
```

<a id="instance-method-documentation">Instance Method Documentation</a>

```objc
/**
 *  Initializes and returns a newly allocated PDFReaderViewController object.
 *
 *  @param object Reference to an initialized PDFReaderDocument
 *
 *  @return Initialized class instance or nil on failure
 *
 *  @throws "<MissingArguments>" When object is nil
 *  @throws "<WrongType>" When object is not a reference to a PDFReaderDocument
 *    object
 *
 *  @remark Designated initializer.
 */
- (instancetype)initWithReaderDocument:(PDFReaderDocument *)object;

/**
 *  Update bookmark state for each page in the PDFReaderDocument's bookmarks 
 *    list.
 */
- (void)updateToolbarBookmarkIcon;
```

<h2 id="naming">Naming</h2>


<h3 id="naming-conventions-for-methods-and-variables">Naming Conventions for Methods and Variables</h3>

详尽的、描述性的方法和变量名，对代码的self-documenting是很有帮助的。命名的清晰性和简洁性都很重要，然而，在Objective-C的世界里，不能为简洁性牺牲清晰性。

**Preferred:**

```objc
UIButton *settingsButton;
NSString *pageTitle = @"My Title";
int pageCounter = 0;
```

**Not Preferred:**

```objc
UIButton *setBut;
NSString *string = @"My Title";
int c = 0;
```

<h3 id="naming-conventions-for-constants-and-macros">Naming Conventions for Constants and Macros</h3>

下面的命名方式第一眼望去可能觉得好复杂，习惯习惯了就好了，别偷懒。。。


	tPRE_Space_Name
	
Where:

| Element | Definition                   | Value       | Example                              |
| :------- | :----------------------------- | :----------- | :------------------------------------ |
| t       | type = constant              | k*          | **k**MIX_MyClass_DefaultTitle        |
|         | type = macro                 | m*          | **m**BUZ_MyClass_doubleIt            |
| PRE     | three-letter prefix          | XXX         | k**XXX**_MyClass_MenuBarHeight       |
| Space   | unique namespace within tPRE | WindowSize  | kXXX\_**MyClass**\_WindowSize        |
| Name    | unique name within tPRE_Space| BorderColor | kZRE_MyClass\_**BorderColor**        |
|         |                              | isIphone    | mZBB_MyClass\_**isIphone**           |

类型(t)元素要么是"k" (for constant)，要么是"m" (for macro)。

PRE元素一般是公司名称前三个字母的大写。

Space元素可以是Library名、类名、app名称或模块名称，采用驼峰命名法并首字母大写。

Name元素是具体名称，**constant**采用驼峰命名法并首字母**大**写，**macro**采用驼峰命名法并首字母**小**写。


**Preferred:**

```objc
static const NSInteger kMIX_PDFReader_DefaultPagingViews = 3;
```

**Not Preferred:**

```objc
static NSTimeInterval const fadetime = 1.7;
```

**Preferred**

```objc
#define mMIX_MacroLib_rgb(r,g,b) [UIColor colorWithRed:r/255.0f green:g/255.0f blue:b/255.0f alpha:1.0f]
```

**Not Preferred**

```objc
#define RGBColor(r,g,b) [UIColor colorWithRed:r/255.0f green:g/255.0f blue:b/255.0f alpha:1.0f]
```

类的成员变量采用驼峰命名法并首字母**小**写。

**Preferred:**

```objc
@property (nonatomic, readwrite, strong) NSString *descriptiveVariableName;

// if/when needed
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not Preferred:**

```objc
id varnm;
...
@property (strong) id varnm;
...
@synthesize varnm;
```

<h3 id="naming-conventions-for-enumerated-types">Naming Conventions for Enumerated Types</h3>

**Enumerated Type names** 用e开头，其他和Constants一样。

**Preferred:**

```objc
// using the NS_ENUM macro (preferred)
typedef NS_ENUM(NSInteger, eMIX_UtilityLib_PlayerStateType) {
  PlayerStateOff,
  PlayerStatePlaying,
  PlayerStatePaused
};
```

**Not Preferred:**

```objc
enum {
    PlayerStateOff,
    PlayerStatePlaying,
    PlayerStatePaused
};

typedef NSInteger PlayerState;
```

<h3 id="underscores">Underscores</h3>

读或写所有properties，instance variables，都应该用`self.`，除了以下三种列外情况:

* Setup and Tear down: init and dealloc
* Overriding accessors (getters/setters)
* Archiving activities: e.g. NSCoding Protocol's encodeWithCoder and initWithCoder

当一个对象还没有初始化好，它处于不稳定的状态，调用getters/setters的副作用，可能会导致异常情况发生，所以应该直接使用`_variableName`。see: [Don’t Message self in Objective-C init (or dealloc)](http://qualitycoding.org/objective-c-init/)。

重载getter/setter来添加自定义的逻辑，必须使用`_variableName`，否则会无限循环。

归档或使用其它持久化方法时，使用`_variableName`可以避免getters/setters的副作用对属性的改变。比如你使用getter读一个属性时，里面的逻辑可能修改了其他属性的值。

还有，local variables不能使用下划线。

<h2 id="methods">Methods</h2>

方法名的-/+符号后面要留一个空格，冒号前面的描述词不能省略，方法里面不要使用and。

**Preferred:**

```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Not Preferred:**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

<h2 id="variables">Variables</h2>

变量名应该尽可能的描述自身的存储信息的内容。`for()`小括弧中的临时变量、或用于计数的变量，可以简单一些。

星号`*`应该紧靠变量名，而不是类型后面，比如：`NSString *text` 而不是 `NSString* text` 或 `NSString * text`。

如果存在qualifier，格式应该为`ClassName * qualifier variableName;`。

为了一致性，类的成员变量应该使用`property`。

**Preferred:**

```objc
@interface MyAppTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

**Not Preferred:**

```objc
@interface MyAppTutorial : NSObject {
  NSString *tutorialName;
}
```

<h2 id="property-attributes">Property Attributes</h2>

property的**所有**属性，应该按照atomicity、accessibility(readonly, readwrite)、storage的顺序明确列出来。

**Preferred:**

```objc
@property (nonatomic, readwrite, weak) IBOutlet UIView *containerView;
@property (nonatomic, readwrite, strong) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
``` 

一个非容器类，如果它具有mutable的相关类，比如NSString与NSMutableString、NSURLRequest与NSMutableURLRequest，在定义类成员变量时，应该使用`copy`而不是`strong`。why？当你定义NSSting为strong时，它可能被赋值为NSMutableString类型对象，这个对象可能会在你不注意的情况下改变值，而你既然把它类型声明为NSSting，这种情况显然不是你需要的。

**Preferred:**

```objc
@property (nonatomic, readwrite, copy) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (nonatomic, readwrite, strong) NSString *tutorialName;
```

<h2 id="dot-notation-syntax">Dot-Notation Syntax</h2>

前面讲Underscores时，说了下点语法的使用。

还有一个限制是，点语法只应该用在访问对象成员变量，而不是去调用对象的方法：

**Preferred:**

```objc
NSInteger arrayCount = self.array.count;
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not Preferred:**

```objc
NSInteger arrayCount = [self.array count];
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

<h2 id="literals">Literals</h2>

尽可能使用语法糖去创建immutable对象。

**Preferred:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**Not Preferred:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

<h2 id="constants">Constants</h2>

因为类型安全的缘故，常量不应该用`#define`去定义，应该声明为全局变量。

**Preferred:**

```objC
// Format: type const constantName = value;
NSString * const kMIX_MyClassShortDateFormat = @"MM/dd/yyyy";

// Macro: #define constantName value
\#define mMIX_MacroLib_isIPad (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPad)
```

**Not Preferred:**

```objc
\#define CompanyName @"MyCompany"
\#define thumbnailHeight 2
```

为了使全局变量能够在其他文件类使用，可以在`.h`中加入：

```objC
extern NSString * const kMIX_MyClass_ShortDateFormat;
```

如果这个全局变量只用在当前类里面，需要在`.m`中去定义，并且前面加上`static`关键字限定作用域。

```objC
static NSString * const kMIX_MyClass_ShortDateFormat = @"MM/dd/yyyy";
```
一个static变量如果定义在函数体里面，它的值能够一直保存，下次调用这个函数时，它的值还是上次调用这个函数后的值。

<h2 id="enumerated-types">Enumerated Types</h2>

使用`NS_ENUM()`去定义枚举，它能够让编译器检查枚举的数据类型。在swift里使用Objecive-C的枚举，也是需要使用`NS_ENUM()`才可以。

**Preferred**

```objc
// using the NS_ENUM macro (preferred)
typedef NS_ENUM(NSInteger, eMIX_UtilityLib_PlayerStateType) {
  PlayerStateOff,
  PlayerStatePlaying,
  PlayerStatePaused
};

// with explicit value assignments
typedef NS_ENUM(NSInteger, eMIX_UtilityLib_PlayerStateType) {
  PlayerStateOff = 0,
  PlayerStatePlaying = 1,
  PlayerStatePaused = 2
};
```

**Not Preferred:**

```objc
// Apple Modern Objective-C style, provides type checking, enum type declared inline...
typedef enum _eMIX_UtilityLib_PlayerStateType : NSUInteger {
    PlayerStateOff,
    PlayerStatePlaying,
    PlayerStatePaused
} eMIX_UtilityLib_PlayerStateType;

// Apple Modern Objective-C style, provides type checking, enum type declared seperately...
enum _eMIX_UtilityLib_PlayerStateType : NSInteger {
  PlayerStateOff,
  PlayerStatePlaying,
  PlayerStatePaused
};
typedef enum _eMIX_UtilityLib_PlayerStateType : NSInteger eMIX_UtilityLib_PlayerStateType;

// Apple legacy style enum (32bit/64bit portability, no formal relationship between type and enum)...
enum {
    PlayerStateOff,
    PlayerStatePlaying,
    PlayerStatePaused
};
typedef NSInteger PlayerState;

// Apple legacy style enum (no 32bit/64bit portability)...
typedef enum {
    PlayerStateOff,
    PlayerStatePlaying,
    PlayerStatePaused
} PlayerState;
// NOTE: Implies...
// typedef int PlayerState;

// Generic C-style enum (no 32bit/64bit portability)...
typedef enum {
    PlayerStateOff,
    PlayerStatePlaying,
    PlayerStatePaused} PlayerState;
```

<h2 id="switch-statements-and-case-label-blocks">Switch Statements and Case Label Blocks</h2>

前面说过，`switich:`后面的大括号不应该新开一行。

`case:`后面一般是不用带大括号的，但如果你需要在里面声明临时变量，编译器要求必须要带大括号。为了一致性和可读性，当case后面的语句多于一行时，带上大括号。


```objc
switch (condition) {
  case 1:
    // ...
    break;
    
  case 2: {
    // ...
    // Multi-line example using braces
    break;
  }
  
  case 3:
    // ...
    break;
  
   default: 
    // ...
    break;
}

```

当多个`case:`需要执行的代码一样时，需要去掉`break`，并注释`fall-through!`：

```objc
switch (condition) {
  case 1:
    // ** fall-through! **
    
  case 2:
    // code executed for values 1 and 2
    break;
    
  default: 
    // ...
    break;
}

```

当使用枚举时，`default`是多余的，不用写。每个枚举值都需要覆盖到，否则会有警告。

```objc
kMIX_LeftMenuTopItemType menuType = kMIX_LeftMenuTopItemMain;

switch (menuType) {
  case kMIX_LeftMenuTopItemMain:
    // ...
    break;
  case kMIX_LeftMenuTopItemShows:
    // ...
    break;
  case kMIX_LeftMenuTopItemSchedule:
    // ...
    break;
}
```


<h2 id="private-properties">Private Properties</h2>

类私有的变量应该声明在class extensions (anonymous categories)中。它可以放在`.m`文件里，或者新建一个 <headerfile>+Private.h文件，给子类或测试用。

**For Example:**

```objc
@interface MyAppDetailViewController ()

@property (nonatomic, readwrite, strong) GADBannerView *googleAdView;
@property (nonatomic, readwrite, strong) ADBannerView *iAdView;
@property (nonatomic, readwrite, strong) UIWebView *adXWebView;

@end
```

<h2 id="image-naming">Image Naming</h2>

图片放入Images.xassets，命名方式是首字母大写的驼峰命名法，并将不同的部分往后靠，在命名中尽量加入模块名、类名进行区分。比如ShortRentCarRentButtonNormal，ShortRentCarRentButtonTouched。

**Examples:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

<h2 id="booleans">Booleans</h2>

Objective-C使用`YES`和`NO`，而不是`true`和`false`（在CoreFoundation，C或C++代码里可以使用）。

nil可以表示`NO`，所以在`if`中，对象的值与`NO`比较是没必要的。

在`if`中不要任何值与`YES`、`NO`比较，`YES`是#define YES (BOOL)1，而`BOOL`是8bit。

**Preferred:**

```objc
if (someObject) {
  // code...
}

if (![anotherObject boolValue]) {
  // code...
}
```

**Not Preferred:**

```objc
if (someObject == nil)
if ([anotherObject boolValue] == NO)
if (isAwesome == YES)     // Never do this.
if (isAwesome == true)    // Never do this.
```

如果一个类的成员变量类型是`BOOL`，并且它可以是个形容词XXX，那么`is`的前缀可以省略，但需要加`getter=isXXX`

```objc
@property (nonatomic, readwrite, unsafe_unretained, getter=isEditable) BOOL editable;
```

参见[Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

<h2 id="conditionals">Conditionals</h2>

为了避免错误、方便以后增加代码、一致性和可读性，`if`后面的大括号应该一直有，大括号在`if`所在行开启。避免的错误包括误将`if`大括号代码块里面的内容弄到`if`外面去了，还有[even more dangerous defect](http://programmers.stackexchange.com/a/16530)


**Preferred:**

```objc
if (!error) {
  return success;
}
```

**Not Preferred:**

```objc
if (!error)
  return success;
```

or

```objc
if (!error) return success;
```

<h3 id="conditional-expressions">Conditional Expressions</h3>

`if`小括号里面的表达式，应该是一个变量，而不是一个函数调用，在`if`小括号里包含太多逻辑不太容易引起人注意。但类似`[anObject boolValue]`这种只会读值，不会有其他逻辑的，是一个例外，但anObject的变量名也需要足够清楚，这种情况单独再声明一个bool型变量不会增加可读性。

**Preferred:**

```objc
BOOL continuousPlayEnabled = [[MediaAppPrefs sharedInstance] continuousPlay];
MediaAppTrack *nextMediaTrack = [MediaAppPlayer nextTrack];
if (continuousPlayEnabled && nextMediaTrack) {
  // play the next song
}
```

**Not Preferred:**

```objc
if ([[MediaAppPrefs sharedInstance] continuousPlay] && [MediaAppPlayer nextTrack]) {
  // play the next song
}
```

<h3 id="ternary-operator">Ternary Operator</h3>

三元操作符号不可滥用，导致可读性降低。

**Preferred:**

```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = (isHorizontal) ? x : y;
```

**Not Preferred:**

```objc
result = a > b ? x = c > d ? c : d : y;

result = isHorizontal ? x : y;
```

<h2 id="return-statements">Return Statements</h2>

不要`return`一个函数调用。

不要在函数中间调用`return`，可以在函数前面检查传参合法性时使用`return`。对于一个比较长的函数，中间有个`return`，读代码的人很难找到。

**Preferred:**

```objc
- (BOOL) playNext
{
  BOOL continuousPlayEnabled = [[MediaAppPrefs sharedInstance] continuousPlay];
  MediaAppTrack *nextMediaTrack = [MediaAppPlayer nextTrack];
  
  return (continuousPlayEnabled && nextMediaTrack);}  
```

**Not Preferred:**

```objc
- (BOOL) playNext
{
  return ([[MediaAppPrefs sharedInstance] continuousPlay] && [MediaAppPlayer nextTrack]);}  
```


<h2 id="init-methods">Init Methods</h2>

初始化方法返回类型应该为`instancetype`，而不是`id`。

```objc
- (instancetype)init
{
  self = [super init];
  if (self) {
    // ...
  }

  return self;
}
```

参见[NSHipster.com](http://nshipster.com/instancetype/)。

另外，应该定义更安全、合理的初始化方法。参见[Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)中Object Creation一章节。

<h2 id="class-constructor-methods">Class Constructor Methods</h2>

类的创建对象便捷方法返回类型应该为`instancetype`，而不是`id`。

```objc
@interface Airplane
+ (instancetype)airplaneWithType:(MyAppAirplaneType)type;
@end
```

参见[NSHipster.com](http://nshipster.com/instancetype/).

<h2 id="cgrect-functions">CGRect Functions</h2>

获取`CGRect`的`x`、`y`、`width`或`height`，应该使用[`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) 而不是直接访问结构体成员。 From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**Not Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

<h2 id="error-handling">Error handling</h2>

参见[Apple's developer documentation](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/ErrorHandling/ErrorHandling.html), 可以向方法传入NSError变量的指针获取异常情况，调用这类方法时，需要对NSError变量进行检查，如果错误需要作出合适的处理：

**Preferred:**

```objc
NSError *error;
BOOL success;
success = [self trySomethingWithError:&error];

if (!success) {
  // Handle Error
  // e.g.
  NSLog(@"Something bad just happened: %@", error);
}
```

**Not Preferred:**

```objc
NSError *error;

if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

<h2 id="singletons">Singletons</h2>

单例的正确创建方法：

```objc
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  // thread-safe init
  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```

This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

另外，还应该在dealloc中添加`abort()`方法:

```objc
- (void)dealloc {

    // implement -dealloc & remove abort() when refactoring for
    // non-singleton use.
    abort();
}
```

因为不应该给单例对象发送`dealloc`消息。参见[StackOverflow thread](http://stackoverflow.com/a/7599682).

<h2 id="line-breaks">Line Breaks</h2>

避免一行太长，应该在合适的地方断行。

**Preferred:**

```objc
self.productsRequest = [[SKProductsRequest alloc] 
  initWithProductIdentifiers:productIdentifiers];
```

**Not Preferred:**

```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```



<h2 id="warnings">Warnings</h2>

在target's Build Settings，中应该开启"Treat Warnings as Errors"，并开启尽可能多的警告。参见[additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings)。如果有些警告你无法避免，可以使用[Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas)。


<h2 id="log">Log</h2>

使用[DDLog](https://github.com/CocoaLumberjack/CocoaLumberjack)。并使用合适的等级打印不同级别的信息。




<h2 id="other-objective-c-style-guides">Other Objective-C Style Guides</h2>

上面的规范可能你不太喜欢，或者没有涉及到你需要的某些方面，可以参考下面的内容:

* [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)
* [New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
