#Blocks编程要点
[原文地址](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40007502-CH1-SW1)

##简介
Block对象是C级别的语法和运行时特性。它们和标准C函数很类似，但是除了可执行代码外，它们还可能包含了变量自动绑定(栈)或内存托管(堆)。所以一个block维护一个状态集（数据），它们可以在执行的时候用来影响程序行为。 你可以用blocks来编写函数表达式，这些表达式可以作为API使用，或可选的存储，或被多个线程使用。Blocks作为回调特别有用，因为block携带了进行回调所需要的执行代码和执行过程中需要的数据。 Blocks在GCC和Clang里面可用，它附带在Mac OS X v10.6里面的Xcode 开发工具里面。你可以在Mac OS X v10.6及其之后，和iOS 4.0及其之后上面使用blocks。Blocks运行时是开源的，你可以在LLVM’s compiler-rt subproject repository（LLVM的RT编译器的子项目库）里面找到它。Blocks同样作为标准C工作组出现在N1370:Apple’s Extensions to C(该文档同样包括了垃圾回收机制)。因为Objective-C和C++都是从C发展而来，blocks被设计在三种语言上面使用（也包括Objective-C++）。（语法反应了这一目标） 你应该阅读该文档来掌握block对象是什么和如何在C,C++或Objective-C上面使用它们来让你的程序更高效和更易于维护。

###文档结构
该文档包含了以下几个章节:

* “Blocks入门”提供了一个对blocks的快速的和实际的介绍。
* “概念概述”提供了对blocks概念的介绍。
* “声明和创建Blocks”描述如何声明block变量，并实现该blocks。
* “Blocks和变量”介绍了blocks和变量的交互，并定义__block存储类型修饰符。
* “使用Blocks”说明不同的使用模式。

##Blocks入门
以下部分使用实际的例子帮助你开始使用Blocks。

声明和使用一个Block
使用 ^ 操作符来来声明一个block变量和指示block文本的开始。Block本身的主体被{}包含着，如下面的例子那样(通常使用C的 ；符合指示block的结束)：

```
int multiplier = 7;
int (^myBlock)(int) = ^(int num) {
    return num * multiplier;
};
```
该示例的解析如下图:
![alt](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Blocks/Art/blocks.jpg)

注意block可以使用相同作用域范围内定义的变量。
如果你声明一个block作为变量，你可以把它简单的作为一个函数使用：

```
int multiplier = 7;int (^myBlock)(int) = ^(int num) {    return num * multiplier;};printf("%d", myBlock(3));// prints "21"
```
###直接使用Block
在很多情况下，你不需要声明一个block变量；相反你可以简单的写一个内联（inline）的block文本，它需要作为一个参数使用。以下的代码使用qsort_b函数。qsort_b和标准qsort_r函数类似，但是它使用block作为最后一个参数。

```
char *myCharacters[3] = { "TomJohn", "George", "Charles Condomine" };qsort_b(myCharacters, 3, sizeof(char *), ^(const void *l, const void *r) {char *left = *(char **)l;char *right = *(char **)r;return strncmp(left, right, 1);});// myCharacters is now { "Charles Condomine", "George", "TomJohn" }
```
###Cocoa的Blocks
在Cocoa frameworks里面有部分方法使用block作为参数，通常不是执行一个对象的集合操作，就是在操作完成的时候作为回调使用。下面的例子显示了如何通过NSArray的方法sortedArrayUsingComparator:使用block。该方法使用一个参数,即block。为了举例说明，该情况下block被定义为NSComparator的局部变量：

```
NSArray *stringsArray = [NSArray arrayWithObjects:@"string 1",@"String 21",@"string 12",@"String 11",@"String 02", nil];static NSStringCompareOptions comparisonOptions = NSCaseInsensitiveSearch | NSNumericSearch |NSWidthInsensitiveSearch | NSForcedOrderingSearch;NSLocale *currentLocale = [NSLocale currentLocale];
NSComparator finderSortBlock = ^(id string1, id string2) {NSRange string1Range = NSMakeRange(0, [string1 length]);return [string1 compare:string2 options:comparisonOptions range:string1Range locale:currentLocale];};NSArray *finderSortArray = [stringsArray sortedArrayUsingComparator:finderSortBlock];NSLog(@"finderSortArray: %@", finderSortArray);/*Output:finderSortArray: ("string 1","String 02","String 11","string 12","String 21")*/
```
###__block变量
Blocks的最大一个特色就是可以修改相同作用域的变量。你可以使用__block存储类型修饰符来给出信号要修改一个变量。改编“Cocoa的Blocks”所示的例子，你可以使用一个block来计数多少个字符串和block中只读变量currentLocal相同：

```
NSArray *stringsArray = @[ @"string 1",
                          @"String 21", // <-
                          @"string 12",
                          @"String 11",
                          @"Strîng 21", // <-
                          @"Striñg 21", // <-
                          @"String 02" ];
 
NSLocale *currentLocale = [NSLocale currentLocale];
__block NSUInteger orderedSameCount = 0;
 
NSArray *diacriticInsensitiveSortArray = [stringsArray sortedArrayUsingComparator:^(id string1, id string2) {
 
    NSRange string1Range = NSMakeRange(0, [string1 length]);
    NSComparisonResult comparisonResult = [string1 compare:string2 options:NSDiacriticInsensitiveSearch range:string1Range locale:currentLocale];
 
    if (comparisonResult == NSOrderedSame) {
        orderedSameCount++;
    }
    return comparisonResult;
}];
 
NSLog(@"diacriticInsensitiveSortArray: %@", diacriticInsensitiveSortArray);
NSLog(@"orderedSameCount: %d", orderedSameCount);
 
/*
Output:
 
diacriticInsensitiveSortArray: (
    "String 02",
    "string 1",
    "String 11",
    "string 12",
    "String 21",
    "Str\U00eeng 21",
    "Stri\U00f1g 21"
)
orderedSameCount: 2
*/
```
这会在“Blocks和变量”部分里面有更多的讨论。

##概念概述
Block对象提供了一个使用C语言和C派生语言（如Objective-C和C++）来创建表达式作为一个特别(ad hoc)的函数。在其他语言和环境中，一个block对象有时候被成为“闭包(closure)”。在这里，它们通常被口语化为”块(blocks)”，除非在某些范围它们容易和标准C表达式的块代码混淆。
Block功能一个block就是一个匿名的内联代码集合体：
* 和函数一样拥有参数类型* 有推断和声明的返回类型* 可以捕获它的声明所在相同作用域的状态
* 可以选择性修改作用域的状态* 可以和其他定义在相同作用域范围的blocks进行共享更改* 在相同作用域范围（栈帧）被销毁后持续共享和更改相同作用域范围(栈帧)的状态
你可以拷贝一个block,甚至可以把它作为可执行路径传递给其他线程（或者在自己的线程内传递给run loop）。编译器和运行时会在整个block生命周期里面为所有block引用变量保留一个副本。尽管blocks在纯C和C++上面可用，但是一个block也同样是一个Objective-C的对象。
###用处
Blocks通常代表一个很小、自包的代码片段。因此它们作为封装的工作单元在并发执行，或在一个集合项上，或当其他操作完成时的回调的时候非常实用。 Blocks作为传统回调函数的一个实用的替代办法，有以下两个原因:1. 它们可以让你在调用的地方编写代码实现后面将要执行的操作。 因此Blocks通常作为框架方法的参数。2. 它们允许你访问局部变量。而不是需要使用一个你想要执行操作时集成所有上下文的信息的数据结构来进行回调，你可以直接简单的访问局部变量。
##声明和创建Blocks 
###声明一个block的引用
    Block变量拥有blocks的引用。你可以使用和声明函数指针类似的语法来声明它们，除了它们使用 ^ 修饰符来替代 * 修饰符。Block类型可以完全操作其他C系统类型。以下都是合法的block声明:

```
void (^blockReturningVoidWithVoidArgument)(void);int (^blockReturningIntWithIntAndCharArguments)(int, char);void (^arrayOfTenBlocksReturningVoidWithIntArgument[10])(int);
```
Blocks还支持可变参数（...）。一个没有使用任何参数的block必须在参数列表上面用void标明。Blocks被设计为类型安全的，它通过给编译器完整的元数据来合法使用blocks、传递到blocks的参数和分配的返回值。你可以把一个block引用强制转换为任意类型的指针，反之亦然。但是你不能通过修饰符 * 来解引用一个block,因此一个block的大小是无法在编译的时候计算的。 你同样可以创建blocks的类型。当你在多个地方使用同一个给定的签名的block时，这通常被认为是最佳的办法。

```
typedef float (^MyBlockType)(float, float);MyBlockType myFirstBlock = // ... ;MyBlockType mySecondBlock = // ... ;
```
###创建一个block
你可以使用 ^ 修饰符来标识一个block表达式的开始。它通常后面跟着一个被 () 包含起来的参数列表。Block的主体一般被包含在 {} 里面。下面的示例定义了一个简单的block，并把它赋值给前面声明的变量(oneFrom)。这里block使用一个标准C的结束符 ; 来结束。

```
float (^oneFrom)(float);
 
oneFrom = ^(float aFloat) {
    float result = aFloat - 1.0;
    return result;
};
```
如果你没有显式的给block表达式声明一个返回值，它会自动的从block内容推断出来。如果返回值是推断的，而且参数列表也是void，那么你同样可以省略参数列表的void。如果或者当出现多个返回状态的时候，它们必须是完全匹配的（如果有必要可以使用强制转换）。
###全局blocks在文件级别，你可以把block作为全局标示符：

```
#import <stdio.h>
 
int GlobalInt = 0;
int (^getGlobalInt)(void) = ^{ return GlobalInt; };
```

##Blocks和变量本文描述blocks和变量之间的交互，包括内存管理。###变量类型在block的主体代码里面，变量可以被使用五种方法来处理。 你可以引用三种标准类型的变量，就像你在函数里面引用那样:

* 全局变量，包括静态局部变量。* 全局函数（在技术上而言这不是变量）。* 封闭范围内的局部变量和参数。

Blocks同样支持其他两种类型的变量:1. 在函数级别是__block变量。这些在block里面是可变的(和封闭范围)，并任何引用block的都被保存一份副本到堆里面。2. 引入const。3. 最后，在实现方法里面，blocks也许会引用Objective-C的实例变量。参阅“对象和Block变量”部分。 在block里面使用变量遵循以下规则:1. 全局变量可访问，包括在相同作用域范围内的静态变量。2. 传递给block的参数可访问（和函数的参数一样）。3. 程序里面属于同一作用域范围的堆（非静态的）变量作为const变量(即只读)。它们的值在程序里面的block表达式内使用。在嵌套block里面，该值在最近的封闭范围内被捕获。4. 属于同一作用域范围内并被__block存储修饰符标识的变量作为引用传递因此是可变的。5. 属于同一作用域范围内block的变量，就和函数的局部变量操作一样。 每次调用block都提供了变量的一个拷贝。这些变量可以作为const来使用，或在block封闭范围内作为引用变量。

下面的例子演示了使用本地非静态变量:

```
int x = 123;
 
void (^printXAndY)(int) = ^(int y) {
 
    printf("%d %d\n", x, y);
};
 
printXAndY(456); // prints: 123 456
```
正如上面提到的，在block内试图给x赋一个新值会导致错误发生:

```
int x = 123;
 
void (^printXAndY)(int) = ^(int y) {
 
    x = x + y; // error
    printf("%d %d\n", x, y);
};
```
为了可以在block内修改一个变量，你需要使用__block存储类型修饰符来标识该变量。参阅“__block存储类型”部分。
###__block存储类型
你可以指定引入一个变量为可更改的，即读-写的，通过应用__block存储类型修饰符。局部变量的__block的存储和register、auto、static等存储类型相似，但它们之间不兼容。 __block变量保存在变量共享的作用域范围内，所有的blocks和block副本都声明或创建在和变量的作用域相同范围内。所以，如果任何blocks副本声明在栈内并未超出栈的结束时，该存储会让栈帧免于被破坏（比如封装为以后执行）。同一作用域范围内给定的多个block可以同时使用一个共享变量。 作为一种优化，block存储在栈上面，就像blocks本身一样。如果使用Block_copy拷贝了block的一个副本（或者在Objective-C里面给block发送了一条copy消息），变量会被拷贝到堆上面。所以一个__block变量的地址可以随时间推移而被更改。

使用__block的变量有两个限制：它们不能是可变长的数组，并且它们不能是包含有C99可变长度的数组变量的数据结构。 以下举例说明了如何使用__block变量：

```
__block int x = 123; //  x lives in block storage
 
void (^printXAndY)(int) = ^(int y) {
 
    x = x + y;
    printf("%d %d\n", x, y);
};
printXAndY(456); // prints: 579 456
// x is now 579
```
下面的例子显示了blocks和其他几个类型变量间的交互:

```
extern NSInteger CounterGlobal;
static NSInteger CounterStatic;
 
{
    NSInteger localCounter = 42;
    __block char localCharacter;
 
    void (^aBlock)(void) = ^(void) {
        ++CounterGlobal;
        ++CounterStatic;
        CounterGlobal = localCounter; // localCounter fixed at block creation
        localCharacter = 'a'; // sets localCharacter in enclosing scope
    };
 
    ++localCounter; // unseen by the block
    localCharacter = 'b';
 
    aBlock(); // execute the block
    // localCharacter now 'a'
}

```
###对象和Block变量
Block提供了支持Objective-C和Objective-C++的对象，和其他blocks的变量。
####Objective-C对象
当Block被拷贝时，在block中使用对象会创建对象的强引用， 如果你在实现方法的时候使用了block:* 如果你通过引用来访问一个实例变量，self会被强引用。* 如果你通过值来访问一个实例变量，那么变量会被强引用。
下面举例说明两个方式的不同:

```
dispatch_async(queue, ^{
    // instanceVariable is used by reference, a strong reference is made to self
    doSomethingWithObject(instanceVariable);
});
 
 
id localVariable = instanceVariable;
dispatch_async(queue, ^{
    /*
      localVariable is used by value, a strong reference is made to localVariable
      (and not to self).
    */
    doSomethingWithObject(localVariable);
});
```
To override this behavior for a particular object variable, you can mark it with the __block storage type modifier.

要覆盖特定对象变量的此行为，可以使用__block存储类型修饰符标记该行为。
####C++对象
通常你可以在block内使用C++的对象。在成员函数里面，通过隐式的导入this指针引用成员变量和函数，结果会很微妙。有两个条件可以让block被拷贝:

* 如果你拥有__block存储的类，它本来是一个基于栈的C++对象，那么通常会使用copy的构造函数。* 如果你在block里面使用任何其他C++基于栈的对象，它必须包含一个const copy的构造函数。该C++对象使用该构造函数来拷贝。

####Blocks
当你拷贝一个block时，任何在该block里面对其他blocks的引用都会在需要的时候被拷贝，即拷贝整个目录树(从顶部开始)。如果你有block变量并在该block里面引用其他的block，那么那个其他的block会被拷贝一份。 

##使用Blocks
###调用一个Block
如果你声明了一个block作为变量，你可以把它作为一个函数来使用，如下面的两个例子所示:

```
int (^oneFrom)(int) = ^(int anInt) {
    return anInt - 1;
};
 
printf("1 from 10 is %d", oneFrom(10));
// Prints "1 from 10 is 9"
 
float (^distanceTraveled)(float, float, float) =
                         ^(float startingSpeed, float acceleration, float time) {
 
    float distance = (startingSpeed * time) + (0.5 * acceleration * time * time);
    return distance;
};
 
float howFar = distanceTraveled(0.0, 9.8, 1.0);
// howFar = 4.9

```
然而你通常会把block作为参数传递给一个函数或方法。在这种情况下，你通过需要创建一个”内联（inline）”的block。
###使用Block作为函数的参数
你可以把一个block作为函数的参数就像其他任何参数那样。然而在很多情况下，你不需要声明blocks;相反你只要简单在需要它们作为参数的地方内联实现它们。下面的例子使用qsort_b函数。qsort_b和标准qsort_r函数类似，但是它最后一个参数用block.

```
char *myCharacters[3] = { "TomJohn", "George", "Charles Condomine" };qsort_b(myCharacters, 3, sizeof(char *), ^(const void *l, const void *r) {char *left = *(char **)l;char *right = *(char **)r;return strncmp(left, right, 1);});// Block implementation ends at "}"// myCharacters is now { "Charles Condomine", "George", "TomJohn" }
```
注意函数参数列表包含了一个block。 下一个例子显示了如何在dispatch_apply函数里面使用block。dispatch_apply声明如下:

```
void dispatch_apply(size_t iterations, dispatch_queue_t queue, void (^block)(size_t));
```
该函数提交一个block给批处理队列来多次调用。它需要三个参数；第一个指定迭代器的数量；第二个指定一个要提交block的队列；第三个是block它本身，它自己需要一个参数（当前迭代器的下标）。 你可以使用dispatch_apply来简单的打印出迭代器的下标，如下:

```
#include <dispatch/dispatch.h>
size_t count = 10;
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
 
dispatch_apply(count, queue, ^(size_t i) {
    printf("%u\n", i);
});

```
###使用Block作为方法的参数
Cocoa提供了一系列使用block的方法。你可以把一个block作为方法的参数就像其他参数那样。 下面的例子确定数组前面五个元素中第一个出现在给定的过滤器集中任何一个的下标。

```
NSArray *array = @[@"A", @"B", @"C", @"A", @"B", @"Z", @"G", @"are", @"Q"];
NSSet *filterSet = [NSSet setWithObjects: @"A", @"Z", @"Q", nil];
 
BOOL (^test)(id obj, NSUInteger idx, BOOL *stop);
 
test = ^(id obj, NSUInteger idx, BOOL *stop) {
 
    if (idx < 5) {
        if ([filterSet containsObject: obj]) {
            return YES;
        }
    }
    return NO;
};
 
NSIndexSet *indexes = [array indexesOfObjectsPassingTest:test];
 
NSLog(@"indexes: %@", indexes);
 
/*
Output:
indexes: <NSIndexSet: 0x10236f0>[number of indexes: 2 (in 2 ranges), indexes: (0 3)]
*/
```
下面的例子确定一个NSSet是否包含一个由局部变量指定的单词，并且如果条件成立把另外一个局部变量(found)设置为YES（并停止搜索）。注意到found同时被声明为__block变量，并且该block是内联定义的:

```
__block BOOL found = NO;
NSSet *aSet = [NSSet setWithObjects: @"Alpha", @"Beta", @"Gamma", @"X", nil];
NSString *string = @"gamma";
 
[aSet enumerateObjectsUsingBlock:^(id obj, BOOL *stop) {
    if ([obj localizedCaseInsensitiveCompare:string] == NSOrderedSame) {
        *stop = YES;
        found = YES;
    }
}];
 
// At this point, found == YES

```
###拷贝Blocks
通常，你不需要copy(或retain)一个block.在你希望block在它被声明的作用域被销毁后继续使用的话，你子需要做一份拷贝。拷贝会把block移到堆里面。 你可以使用C函数来copy和release一个block：

```
Block_copy();
Block_release();

```
为了避免内存泄露，你必须总是平衡Block_copy()和Block_release()。
###需要避免的模式
一个block的文本（通常是 ^ {...}）是一个代表block的本地栈数据结构地址。因此该本地栈数据结构的作用范围是封闭的复合状态，所以你应该避免下面例子显示的模式:

```
void dontDoThis() {
    void (^blockArray[3])(void);  // an array of 3 block references
 
    for (int i = 0; i < 3; ++i) {
        blockArray[i] = ^{ printf("hello, %d\n", i); };
        // WRONG: The block literal scope is the "for" loop.
    }
}
 
void dontDoThisEither() {
    void (^block)(void);
 
    int i = random():
    if (i > 1000) {
        block = ^{ printf("got i at: %d\n", i); };
        // WRONG: The block literal scope is the "then" clause.
    }
    // ...
}

```
###调试
你可以在blocks里面设置断点并单步调试。你可以在一个GDB的对话里面使用invoke-block来调用一个block。如下面的例子所示:

```
$ invoke-block myBlock 10 20
```
如果你想传递一个C字符串，你必须用引号括这它。例如，为了传递this string给doSomethingWithString的block，你可以类似下面这样写:

```
$ invoke-block doSomethingWithString "\"this string\""
```


