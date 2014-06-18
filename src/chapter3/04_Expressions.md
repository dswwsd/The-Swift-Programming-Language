#表达式

在swift中有四种表达式：前缀表达式，二元表达式，主表达式和后缀表达式。对表达式求值可以得到一个返回值、或完成某些逻辑运算，或同时完成这两件事。

前缀和二元表达式可以让我们在更短小的表达式中运算符。主表达式从概念上来看是最简单短小的表达式，同时又提供了一种求值的途径。后缀表达式，与前缀表达式和二元表达式相似，都可以让你建立更为复杂的表达方式，比如函数调用和成员访问。我们将在本节中详细解释每种表达式。

>表达式的语法
>
>

##前缀表达式

前缀表达式由一个任意前缀运算符接表达式构成。前缀表达式只接受一个参数。
Swift标准库提供了如下前缀运算符：

* ++ 自增
* -- 自减
* ! 逻辑否
* ~ 按位否
* \+ 一元加
* \- 一元负

关于这些运算符的信息，请参见：“Basic Operators and Advanced Operators.”两节。

作为对上述运算符的补充，你可以在作为某个函数参数的变量名前使用‘&’运算符。更多信息和实例，参见：In-Out Parameters一节。

>前缀表达式语法
>
>

##二元表达式

二元表达式由中缀二元运算符和“左边参数”与“右边参数”组成。形式如下：
left-hand argument operator right-hand argument

Swift标准库提供了如下的二元运算符：

* 指数（无结合性，优先级160）
    * << 按位左移
    * \>> 按位右移
* 乘法（左结合，优先级150）
	* \* 乘
	* / 除
	* % 求余
	* &* 乘，ignoring overflow
	* &/ 除，ignoring overflow
	* &% 求余，ignoring overflow
	* & 按位与
* 加法（左结合，优先级140）
	* \+ 加 
	* \- 减
	* &+ 加，with overflow
	* &- 减，with overflow
	* | 按位或
	* ^ 按位异或
* 值域（无结合性，优先级135）
	* .. 半闭值域
	* ... 闭值域
* 类型转换（无结合性，优先级132）
	* is 类型检查
	* as 类型转换
* 比较（无结合性，优先级130）
	* < 小于
	* <= 小于等于
	* \> 大于
	* \>= 大于等于
	* == 等于
	* != 不等于
	* === 恒等于
	* !== 不恒等
	* ~= 模式匹配
* 合取性(conjunctive)（左结合，优先级120）
	* && 逻辑与 
* 析取性(disjunction)（左结合，优先级110）
	* || 逻辑或
* 三元条件（右结合，优先级100）
	* ?: 三元条件
* 赋值（右结合，优先级90）
	* = 赋值
	* *= 乘等
	* /= 除等
	* %= 余等
	* += 加等
	* -= 减等
	* <<= 左移等
	* \>>= 右移等
	* &= 按位与等
	* ^= 按位异或等
	* |= 按位非等
	* &&= 逻辑与等
	* ||= 逻辑或等

关于这些运算符的信息，请参见：Basic Operators and Advanced Operators两节。


*注解*
	
	在解析时，由二元操作符构成的表达式被表达为一个简单列表。这个列表按照运算符的优先级被转换成树（tree），比如“2 + 3 * 5”首先被认为是'2'、'+'、'3'、'+'、'5'这5个元素，随后被转换成树(2 + (3 * 5))。

> 二元表达式语法
>
>


###赋值运算符
赋值运算符会给某个给定的表达式赋值。
> expression = value

上面语句的意思就是计算`value`的值并赋给`expression`。如果`expression`是个元组，那么`value`必须是含有同等数量元素的元祖。（嵌套元祖也是可以直接赋值滴。）元祖赋值会把`value`中相对应的部分分别赋给`expression`。例如：
> (a, _, (b, c)) = ("test", 9.45, (12, 3))

> // a is "test", b is 12, c is 3, and 9.45 is ignored

赋值运算符不返回任何值。
> 赋值运算符的语法

###三元条件运算符
三元条件运算符是根据一个条件判断来求值。形式如下：
> condition ? expression used if true : expression used if false

上面例子中，当condition的值为true时，那么将对第一个表达式求值并返回。否则将对第二个表达式求值并返回。期间没有用到的那个表达式将不会被调用。

更多三元条件运算符的例子，请参见Ternary Conditional Operator。

> 条件运算符语法：
> 
> conditional-operator → ?expression:

###类型转换运算符
类型转换运算符有两种：as运算符合is运算符。形式如下：
> expression as type

> expression as? type

> expression is type

上面例子中，as 运算符会把目标表达式`expression`转换成指定的类型`type`。过程如下：

* 如果转换为`type`类型保证能够成功，那么目标表达式`expression`就会返回指定类型的实例。一个显而易见的例子就是子类类型转换成父类类型。
* 如果转换失败，则抛出编译错误。
* 如果不是以上两种情况，那么目标表达式`expression`就会被转换成指定类型的optional。在运行时，如果转换成功，那么`expression`会返回一个optional的包装类型；否则返回nil。举个例子：

> class SomeSuperType {}
> 
> class SomeType: SomeSuperType {}
> 
> class SomeChildType: SomeType {}
> 
> let s = SomeType()

> let x = s as SomeSuperType  // known to succeed; type is SomeSuperType
> 
> let y = s as Int            // known to fail; compile-time error
> 
> let z = s as SomeChildType  // might fail at runtime; type is SomeChildType?

使用as指定类型与使用类型注释效果相同，看下面这个例子：
let y1 = x as SomeType  // Type information from 'as'
let y2: SomeType = x    // Type information from an annotation


is运算符会在运行时检查`expression`是否指定了类型。成功会返回true, 否则 false。上述检查在编译时不可用，下面例子就是无效的：

> "hello" is String
> 
  "hello" is Int

关于类型转换的更多内容和例子，请参见： Type Casting.

> 类型转换的语法
> type-casting-operator → is­ type­| as­?(opt)­ type

##主表达式
主表达式是种最基本的表达式。它可以单独使用，也可以和其他表达式组合使用。
> 主表达式的语法

primary-expression → identifiergeneric-argument-clauseopt
‌ primary-expression → literal-expression
‌ primary-expression → self-expression
‌ primary-expression → superclass-expression
‌ primary-expression → closure-expression
‌ primary-expression → parenthesized-expression
‌ primary-expression → implicit-member-expression
‌ primary-expression → wildcard-expression


###字面量表达式
一个字面量表达式可以由普通文本（比如一个字符串或者一个数字）、一个数组或字典字面量或以下指定的文本组成：

Literal      | Type    | Value
------------ | --------| ------------
\__FILE__    | String  | 当前文件的文件名
\__LINE__    | Int     | 当前行数
\__COLUMN__  | Int     | 当前列数
\__FUNCTION__| String  | 当前声明的名字

在函数中，__FUNCTION__的值为当前函数的名字。在方法中，它的值为当前方法的名字。在某个属性的getter/setter中为这个属性的名字。在像init和subscript这样的特殊成员中，它的值为那个关键字的名字。在文件顶层（at the top level of a file）则是当前模块的名字。

数组字面量是有序的值的集合。形式如下：
> [value 1, value 2, ...]

数组中的最后一个表达式后面可以接一个逗号。一对空的中括号（[]）组成了一个空的数组字面量。数组字面量的类型是T[]，这个T就是数组中表达式的类型。如果数组含多个类型的表达式，T则是他们的最近公共父类型（closest common supertype）。

字典字面量是无序键值对的集合。形式如下：
> [key 1: value 1, key 2: value 2, ...]

字典中的最后一个表达式后面也可以接一个逗号。一对空的中括号包含一个冒号（[:]）组成了一个空的字典字面量。字典字面量的类型是Dictionary<KeyType, ValueType>，这里KeyType、ValueType就是key和value的类型。如果包含多个类型，则分别取他们相应的最近的公共父类型（closest common supertype）。

> 字面量表达式的语法


###self表达式

self表达式是对当前类型或当前实例的直接引用。形式如下：
> self
> 
> self.member name
> 
> self[subscript index]
> 
> self(initializer arguments)
> 
> self.init(initializer arguments)

如果在初始设定式、子脚本、实例方法中，self为当前类型实例的引用。在静态方法和类方法中，self为当前类型的引用。

self表达式被用于在访问成员变量时指定作用域，当作用域中有重名变量时，它还用来消除歧义（例如函数的参数）。例如：

class SomeClass {
    var greeting: String
    init(greeting: String) {
        self.greeting = greeting
    }
}

在派生方法中，你可以把一个那个类型的新实例赋值给self。例如：

struct Point {
    var x = 0.0, y = 0.0
    mutating func moveByX(deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}

> self表达式的语法


###超类表达式

超类表达式可以让我们在某个class中访问它的超类，形式如下：

>super.`member name`
>
super[`subscript index`]
>
super.init（`initializer arguments`）

第一种形式用来访问超类的某个成员。第二种形式用来访问超类的子脚本实现（subscript implementation）。第三种用来访问该超类的初始设定式。
子类可以利用超类的实现，并通过使用超类表达式来实现它们的成员、子脚本和初始值。

> 超类表达式的语法

###闭包表达式

闭包表达式可以创建一个闭包，就好像其他语言中的lambda或者匿名函数。与函数声明相同，闭包包含了要执行的语句，接收作用域中的变量。形式如下：

{ (parameters) -> return type in
    statements
}

闭包的参数声明跟函数的参数一样，具体参见Function Declaration。

为了写起来更加简洁，闭包还有几种特殊形式：

* 闭包可以省略参数和返回值的类型。如果省略了参数和参数类型，语句前面的in也要省略。如果省略的类型不能被自动甄别，那么就会抛出编译错误。
* 闭包可以省略参数名。如果省略了它们则会隐式地命名为“$+它们的顺序号”：`$0`,`$1`,`$2`等。
* 如果闭包中只包含一个表达式，那么该表达式就会自动成为该闭包的返回值。同时表达式的内容在进行类型推断的时候也会参考周围的表达式。

以下几个表达式是等价的：

myFunction {
    (x: Int, y: Int) -> Int in
    return x + y
}
 
myFunction {
    (x, y) in
    return x + y
}
 
myFunction { return $0 + $1 }
 
myFunction { $0 + $1 }

关于将闭包作为函数参数的更多内容，请参见：Function Call Expression。

可以通过使用捕获列表从周围作用域中捕获值来显式指定给闭包表达式。捕获列表是以在参数列表前使用中括号加逗号分隔的形式组成的。一旦使用了参数列表，即使省略了参数名，参数类型和返回值类型，也要使用in关键字。
捕获列表中的每一项都要标记为weak或unowned来捕获弱引用和无主引用。

myFunction { print（self.title） }                    // strong capture
myFunction { [weak self] in print（self!.title） }    // weak capture
myFunction { [unowned self] in print（self.title） }  // unowned capture

在捕获列表中，你能绑定任意表达式给一个命名值。表达式在闭包形成时被根据指定强度计算并捕获。

// Weak capture of "self.parent" as "parent"
myFunction { [weak parent = self.parent] in print(parent!.title) }

关于闭包表达式更多信息和例子，请参见 Closure Expressions。

> 闭包表达式的语法


###隐式成员表达式
在可以判断隐藏类型的上下文中，隐式成员表达式是访问某个类型的成员的简便写法，例如在枚举或类方法中。形式如下：
>.member name

例子：

“var x = MyEnumeration.SomeValue
x = .AnotherValue”

> 隐式成员表达式的语法：


###圆括号表达式
圆括号表达式由中括号包裹的一组逗号分隔的子表达式列表组成。每个子表达式前面可以有一个可选标识符，由`:`隔开。形式如下：
（identifier 1: expression 1, identifier 2: expression 2, ...）

圆括号表达式可以用来创建元祖或给函数传参。如果圆括号表达式中只有一个值，那么这个表达式的类型则与此值相同，例如表达式`(1)`的类型为`Int`而不是`(Int)`。
> 圆括号表达式的语法

###通配符表达式
通配符表达式用来在赋值的时候显式地忽略某个值。比如下面的赋值语句中，`10`被传递给`x`，`20`则被忽略。

> (x, _) = (10, 20)
// x is 10, 20 is ignored



> 通配符表达式的语法

##后缀表达式
后缀表达式由一个表达式后面加上一个后缀操作符或其他后缀语法组成。单纯从语法上讲，每个主表达式也是一个后缀表达式。
swift标准库提供了如下后缀操作符：
 
* ++ 自增
* -- 自减

关于这些表达式行为的更多信息，请参见“Basic Operators and Advanced Operators.”

> 后缀表达式的语法


###函数调用表达式

函数调用表达式由函数名后加圆括号组成，圆括号里面为逗号分隔的函数参数列表。形式如下：
> function name(argument value 1, argument value 2)

上面的`function name`可以是任何返回值为函数类型的表达式。
如果函数声明中包含了参数名，那么函数调用时必须在参数值前加上参数名，并用分号隔开。这种函数调用表达式形式如下：
> function name(argument name 1: argument value 1, argument name 2: argument value 2)

函数调用表达式可以包含一个由紧跟在圆括号后面由一个闭包表达式组成的尾随闭包（`trailing closure`）。追加在圆括号后面的尾随闭包会被当做函数的参数。下面两种写法均可：
> // someFunction takes an integer and a closure as its arguments
someFunction(x, {$0 == 13})
someFunction(x) {$0 == 13}

如果这个闭包是函数的唯一参数，那么圆括号可以省略。

> // someFunction takes a closure as its only argument
myData.someMethod() {$0 == 13}
myData.someMethod {$0 == 13}

> 函数调用表达式语法



###初始化函数表达式
初始化函数表达式用来给类型初始化。形式如下：
> expression.init（initializer arguments）

你可以在函数调用表达式中使用初始化函数表达式来初始化一个类型的实例。初始化函数不像函数，它不能有返回值，例如：
> var x = SomeClass.someClassFunction // ok

> var y = SomeClass.init              // error

初始化函数表达式还可以用来完成对父类初始化函数的代理。

> class SomeSubClass: SomeSuperClass {
    init() {
        // subclass initialization goes here
        super.init()
    }
}

> 初始化函数表达式的语法

###显式成员表达式


###后缀self表达式

###动态类型表达式

###子脚本表达式

###强取值表达式


###可选链式表达式



