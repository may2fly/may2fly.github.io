---
layout: post
title: Kotlin教程-基础
categories: Android
tags: Android, Kotlin
---

# 1、基本语法

### 包的定义与导入
```kotlin
package my.demo	//包的声明，处于源文件顶部

import kotlin.text.*	//包的导入

// ......

```

### 程序入口
Kotlin 应用程序的入口点是 main 函数。
```kotlin
//无入参的入口函数
fun main() {
    println("Hello world!")
}
//有入参的入口函数
fun main(args: Array<String>) {
    println(args.contentToString())
}
```

### 标准输出
`print/println`将其参数打到标准输出。
```kotlin
 print("Hello ")            //输出其参数
 println("Hello world!")    //输出其参数并添加换行符
```

### 函数

### 变量
定义只读局部变量使用关键字 val 定义。只能为其赋值一次。
```kotlin
fun main() {
    //sampleStart
    val a:Int = 1 //立即赋值
    val b = 2 // 自动推断出 `Int` 类型 val c: Int // 如果没有初始值类型不能省略 c = 3 // 明确赋值
    //sampleEnd
    println("a = $a, b = $b, c = $c")
}
```
可重新赋值的变量使用 var 关键字。
```kotlin
fun main() {
    //sampleStart
    var x = 5 // 自动推断出 `Int` 类型
    x += 1
    //sampleEnd
    println("x = $x")
}  
```

可以在顶层声明变量。
```kotlin
//sampleStart
val PI = 3.14
var x = 0
fun incrementX() {
    x += 1
}
//sampleEnd
fun main() {
    println("x = $x; PI = $PI")
    incrementX()
    println("incrementX()")
    println("x = $x; PI = $PI")
}
```

### 创建类与实例
使用 class 关键字定义类。
```kotlin
    class Shape 
```
类的属性可以在其声明或主体中列出。
```kotlin
class Rectangle(var height: Double, var length: Double) {
    var perimeter = (height + length) * 2
}
```
具有类声明中所列参数的默认构造函数会自动可用。
```kotlin
class Rectangle(var height: Double, var length: Double) {
    var perimeter = (height + length) * 2
}
fun main() {
    //sampleStart
    val rectangle = Rectangle(5.0, 2.0)
    println("The perimeter is ${rectangle.perimeter}")
    //sampleEnd
}
```
类之间继承由冒号( : )声明。默认情况下类都是 final 的;如需让一个类可继承， 请将其标 记为 open 。

```kotlin
open class Shape
class Rectangle(var height: Double, var length: Double): Shape() {
    var perimeter = (height + length) * 2
}
```

### 注释
Kotlin 支持单行(或行末)与多行(块)注释。Kotlin 中的块注释可以嵌套。

```kotlin
// 这是一个行注释
/* 这是一个多行的 块注释。 */

/* 注释从这里开始
/* 包含嵌套的注释 */ 并且在这里结束。 */
```

### 字符串末班
```kotlin
fun main() {
    //sampleStart
    var a = 1
    // 模板中的简单名称: val s1 = "a is $a"
        a=2
    // 模板中的任意表达式:
    val s2 = "${s1.replace("is", "was")}, but now is $a"
    //sampleEnd
    println(s2)
}
```
### 条件表达式

```kotlin
//sampleStart
fun maxOf(a: Int, b: Int): Int {
    if (a > b) {
        return a
    } else {
        return b 
    }
}
//sampleEnd

//if 也可以用作表达式。
fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

### for 循环
```kotlin
fun main() {
    //sampleStart
    val items = listOf("apple", "banana", "kiwifruit")
    for (item in items) {
        println(item)
    }
    //sampleEnd
}
```

```kotlin
fun main() {
    //sampleStart
    val items = listOf("apple", "banana", "kiwifruit")
    for (index in items.indices) {
        println("item at $index is ${items[index]}")
    }
    //sampleEnd
}
```

### while 循环
```kotlin
fun main() {
//sampleStart
    val items = listOf("apple", "banana", "kiwifruit")
    var index = 0
    while (index < items.size) {
        println("item at $index is ${items[index]}")
index++ }
//sampleEnd
}
```

### when 表达式
```kotlin
//sampleStart
fun describe(obj: Any): String =
    when (obj) {
        1       -> "One"
        "Hello" -> "Greeting"
        is Long  -> "Long"
        !is String -> "Not a string"
         else       -> "Unknown"
    }                  

fun main() {
    println(describe(1))
    println(describe("Hello"))
    println(describe(1000L))
    println(describe(2))
    println(describe("other"))
}
```

### 使用区间(range)
使用 in 操作符来检测某个数字是否在指定区间内。
```kotlin
fun main() {
    //sampleStart
    val x = 10
    val y = 9
    if (x in 1..y+1) {
        println("fits in range")
    }
    //sampleEnd
}
```
检测某个数字是否在指定区间外。
```kotlin
fun main() {
    //sampleStart
    val list = listOf("a", "b", "c")
    if (-1 !in 0..list.lastIndex) {
        println("-1 is out of range")
    }
    if (list.size !in list.indices) {
        println("list size is out of valid list indices range, too")
    }
    //sampleEnd
}
```
区间迭代。
```kotlin
fun main() {
//sampleStart
    for (x in 1..5) {
        print(x)
}
//sampleEnd
}
```

或数列迭代
```kotlin
fun main() {
    //sampleStart
    for (x in 1..10 step 2) {
        print(x) 
    }
    println()
    for (x in 9 downTo 0 step 3) {
        print(x) 
    }
    //sampleEnd
}
```

### 集合
对集合进行迭代。

```kotlin
fun main() {
    val items = listOf("apple", "banana", "kiwifruit")
    //sampleStart
    for (item in items) {
        println(item)
    }
    //sampleEnd
}
```
使用 in 操作符来判断集合内是否包含某实例。
```kotlin
fun main() {
    val items = setOf("apple", "banana", "kiwifruit")
    //sampleStart
    when {
        "orange" in items -> println("juicy")
        "apple" in items -> println("apple is fine too")
    }
    //sampleEnd
}
```

使用 lambda 表达式来过滤(filter)与映射(map)集合:
```kotlin
fun main() {
    //sampleStart
    val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
    fruits
      .filter { it.startsWith("a") }
      .sortedBy { it }
      .map { it.uppercase() }
      .forEach { println(it) }
      //sampleEnd
}
```
### 空值与空检测
当可能用 null 值时，必须将引用显式标记为可空。可空类型名称以问号( ? )结尾。
如果 str 的内容不是数字返回 null :
```kotlin
fun parseInt(str: String): Int? {
    // ......
}
```

使用返回可空值的函数:
```kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}

//sampleStart
fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

    // 直接使用 `x * y` 会导致编译错误，因为它们可能为 null if (x != null && y != null) {
    // 在空检测后，x 与 y 会自动转换为非空值(non-nullable)
        println(x * y)
    }
    else {
        println("'$arg1' or '$arg2' is not a number")
    } 
}
//sampleEnd

fun main() {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("a", "b")
}
```

或者
```kotlin
fun parseInt(str: String): Int? {
    return str.toIntOrNull()
}

fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

//sampleStart
    // ......
    if (x == null) {
        println("Wrong number format in arg1: '$arg1'")
        return
    }   
    if (y == null) {
        println("Wrong number format in arg2: '$arg2'")
        return
    }
    // 在空检测后，x 与 y 会自动转换为非空值
    println(x * y)
//sampleEnd
}

fun main() {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("99", "b")
}
```

### 类型检测与自动类型转换
`is`操作符检测一个表达式是否某类型的一个实例。 如果一个不可变的局部变量或属性已经 判断出为某类型，那么检测后的分支中可以直接当作该类型使用，无需显式转换:

```kotlin
//sampleStart
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
        // `obj` 在该条件分支内自动转换成 `String`
        return obj.length
    }
    // 在离开类型检测分支后，`obj` 仍然是 `Any` 类型
    return null 
}
//sampleEnd
fun main() {
    fun printLength(obj: Any) {
println("Getting the length of '$obj'. Result: ${getStringLength(obj) ?: "Erro
r: The object is not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}
```
或者
```kotlin
fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null
    // `obj` 在这一分支自动转换为 `String`
    return obj.length
}

fun getStringLength(obj: Any): Int? {
    // `obj` 在 `&&` 右边自动转换成 `String` 类型 
    if (obj is String && obj.length > 0) {
        return obj.length
    }
    return null 
}
```


# 2、习惯用法
### 创建 DTO(POJO/POCO)

```kotlin
data class Customer(val name: String, val email: String)
```

会为 Customer 类提供以下功能:
* 所有属性的 getter (对于 var 定义的还有 setter) equals()
* hashCode()
* toString()
* copy()
* 所有属性的 component1() 、 component2() ......等等(参见数据类)

### 函数的默认参数
```kotlin
fun foo(a: Int = 0, b: String = "") { ...... }
```

### 过滤 list
```kotlin
val positives = list.filter { x -> x > 0 }
```
或者可以更短:
```kotlin
val positives = list.filter { it > 0 }
```

### 检测元素是否存在于集合中
```kotlin
if ("john@example.com" in emailsList) { ...... }    //在集合中判断

if ("jane@example.com" !in emailsList) { ...... }   //不在集合中判断
```

### 字符串内插
```kotlin
println("Name $name")
```

### 类型判断
```kotlin
when (x) {
    is Foo -> ......
    is Bar -> ......
    else -> ...... }
```

### 只读 list
```kotlin
val list = listOf("a", "b", "c")
```

### 只读 map
```kotlin
val map = mapOf("a" to 1, "b" to 2, "c" to 3)
```

### 访问 map 条目
```kotlin
println(map["key"])
map["key"] = value
```
### 遍历 map 或者 pair 型 list
```kotlin
for ((k, v) in map) {
    println("$k -> $v")
}
```
k 与 v 可以是任何适宜的名称，例如 name 与 age 。

### 区间迭代
```kotlin
for (i in 1..100) { ...... } // 闭区间:包含 100
for (i in 1 until 100) { ...... } // 半开区间:不包含 100 
for (x in 2..10 step 2) { ...... }
for (x in 10 downTo 1) { ...... }
(1..10).forEach { ...... }
```

### 延迟属性
```kotlin
val p: String by lazy { // 该值仅在首次访问时计算 
// 计算该字符串
}
```

### 扩展函数
```kotlin
fun String.spaceToCamelCase() { ...... }

"Convert this to camelcase".spaceToCamelCase()
```

### 创建单例
```kotlin
object Resource {
    val name = "Name"
}
```

### 实例化一个抽象类
```kotlin
abstract class MyAbstractClass {
    abstract fun doSomething()
    abstract fun sleep()
}

fun main() {
    val myObject = object : MyAbstractClass() {
        override fun doSomething() {
            // ......
        }
        override fun sleep() {
            // ......
        }
    }
    myObject.doSomething()
}
```

### if-not-null 缩写
```kotlin
val files = File("Test").listFiles()

println(files?.size) // 如果 files 不是 null，那么输出其大小(size)
```

### if-not-null-else 缩写
```kotlin
val files = File("Test").listFiles()

println(files?.size ?: "empty") // 如果 files 为 null，那么输出“empty”

// To calculate the fallback value in a code block, use `run`
val filesSize = files?.size ?: run {
    return someSize
}
println(filesSize)
```

### if null 执行一个语句
```kotlin
val values = ......
val email = values["email"] ?: throw IllegalStateException("Email is missing!")
```

### 在可能会空的集合中取第一元素
```kotlin
val emails = ...... // 可能会是空集合
val mainEmail = emails.firstOrNull() ?: ""
```

### if not null 执行代码
```kotlin
val value = ......

value?.let {
    ...... // 如果非空会执行这个代码块
}
```

### 映射可空值(如果非空的话)
```kotlin
val value = ......
val mapped = value?.let { transformValue(it) } ?: defaultValue // 如果该值或其转换结果为空，那么返回 defaultValue。
```

### 返回 when 表达式
```kotlin
fun transform(color: String): Int {
    return when (color) {
        "Red" -> 0
        "Green" -> 1
        "Blue" -> 2
        else -> throw IllegalArgumentException("Invalid color param value")
    }
}
```

### try-catch 表达式
```kotlin
fun test() {
    val result = try {
        count()
    } catch (e: ArithmeticException) {
        throw IllegalStateException(e)
    }

    // 使用 result 
}
```

### if 表达式
```kotlin
val y = if (x == 1) {
    "one"
} else if (x == 2) {
    "two"
} else {
    "other"
}
```

### 返回类型为 Unit 的方法的构建器风格用法
```kotlin
fun arrayOfMinusOnes(size: Int): IntArray {
    return IntArray(size).apply { fill(-1) }
}
```

### 单表达式函数
```kotlin
fun theAnswer() = 42
//等价于
fun theAnswer(): Int {
    return 42
}
```
单表达式函数与其它惯用法一起使用能简化代码，例如和 when 表达式一起使用:
```kotlin
fun transform(color: String): Int = when (color) {
    "Red" -> 0
    "Green" -> 1
    "Blue" -> 2
    else -> throw IllegalArgumentException("Invalid color param value")
}
```

### 对一个对象实例调用多个方法 (with)
```kotlin
class Turtle {
    fun penDown()
    fun penUp()
    fun turn(degrees: Double)
    fun forward(pixels: Double)
}

val myTurtle = Turtle()
with(myTurtle) { // 画一个 100 像素的正方形
    penDown()
    for (i in 1..4) {
        forward(100.0)
        turn(90.0) 
    }

    penUp() 
}
```

### 配置对象的属性(apply)
```kotlin
val myRectangle = Rectangle().apply {
    length = 4
    breadth = 5
    color = 0xFAFAFA
}
```
这对于配置未出现在对象构造函数中的属性非常有用。

### 需要泛型信息的泛型函数
```kotlin
//  public final class Gson {
//      ......
// public <T> T fromJson(JsonElement json, Class<T> classOfT) throws JsonSyntaxExc eption {
//      ......
inline fun <reified T: Any> Gson.fromJson(json: JsonElement): T = this.fromJson(json, T::class.java)
```

### 可空布尔
```kotlin
val b: Boolean? = ......
if (b == true) {
    ......
} else {
    // `b` 是 false 或者 null 
}
```

### 交换两个变量
```kotlin
var a = 1
var b = 2
a = b.also { b = a }
```

### 将代码标记为不完整(TODO)
Kotlin 的标准库有一个 TODO() 函数，该函数总是抛出一个 NotImplementedError 。 其返回类
型为 Nothing ，因此无论预期类型是什么都可以使用它。还有一个接受原因参数的重载:
```kotlin
fun calcTaxes(): BigDecimal = TODO("Waiting for feedback from accounting")
```
IntelliJ IDEA 的 kotlin 插件理解 TODO() 的语言，并且会自动在 TODO 工具窗口中添加代码指 示。

# 3、编码规范
众所周知且易于遵循的编码规范对于任何编程语言都至关重要。

### 目录结构
在纯 Kotlin 项目中，推荐的目录结构遵循省略了公共根包的包结构。

### 源文件名称
如果 Kotlin 文件包含单个类或接口(以及可能相关的顶层声明)，那么文件名应该与该类的 名称相同，并追加 .kt 扩展名。 这适用于所有类型的类和接口。 如果文件包含多个类或只 包含顶层声明， 那么选择一个描述该文件所包含内容的名称，并以此命名该文件。 使用首字 母大写的驼峰风格(也称为Pascal风格)，

文件的名称应该描述文件中代码的作用。因此，应避免在文件名中使用诸如 Util 之类的无意义词语。

### 源文件组织
鼓励多个声明(类、顶级函数或者属性)放在同一个 Kotlin 源文件中， 只要这些声明在语义上彼此紧密关联，并且文件保持合理大小 (不超过几百行)。

特别是在为类定义与类的所有客户都相关的扩展函数时， 请将它们放在与类自身相同的地 方。而在定义仅对指定客户有意义的扩展函数时，请将它们放在紧挨该客户代码之后。避免 只是为了保存 某个类的所有扩展函数而创建文件。

### 类布局
一个类的内容应按以下顺序排列:
1. 属性声明与初始化块 
2. 次构造函数
3. 方法声明
4. 伴生对象
不要按字母顺序或者可见性对方法声明排序，也不要将常规方法与扩展方法分开。而是要把相关的东西放在一起，这样从上到下阅读类的人就能够跟进所发生事情的逻辑。选择一个顺序(高级别优先，或者相反)并坚持下去。

将嵌套类放在紧挨使用这些类的代码之后。如果打算在外部使用嵌套类，而且类中并没有引用这些类，那么把它们放到末尾，在伴生对象之后。

### 接口实现布局
在实现一个接口时，实现成员的顺序应该与该接口的成员顺序相同(如果需要， 还要插入用 于实现的额外的私有方法)。

### 重载布局
在类中总是将重载放在一起。

### 命名规则
在 Kotlin 中，包名与类名的命名规则非常简单:
* 包的名称总是小写且不使用下划线( org.example.project )。 通常不鼓励使用多个词的 名称，但是如果确实需要使用多个词，可以将它们连接在一起或使用驼峰风格
( org.example.myProject )。
* 类与对象的名称以大写字母开头并使用驼峰风格:
```kotlin
open class DeclarationProcessor { /*......*/ }

object EmptyDeclarationProcessor : DeclarationProcessor() { /*......*/ }
```

### 函数名
函数、属性与局部变量的名称以小写字母开头、使用驼峰风格而不使用下划线:
```kotlin
fun processDeclarations() { /*......*/ }

var declarationCount = 1
```
例外:用于创建类实例的工厂函数可以与抽象返回类型具有相同的名称:
```kotlin
interface Foo { /*......*/ }

class FooImpl : Foo { /*......*/ }

fun Foo(): Foo { return FooImpl() }
```

### 测试方法的名称
当且仅当在测试中，可以使用反引号括起来的带空格的方法名。 请注意，Android 运行时目前 不支持这样的方法名。测试代码中也允许方法名使用下划线。
```kotlin
class MyTestCase {
     @Test fun `ensure everything works`() { /*......*/ }
}
```

### 属性名
常量名称(标有 const 的属性，或者保存不可变数据的没有自定义 get 函数的顶层/对象 val 属性)应该使用大写、下划线分隔的 (screaming snake case) 名称:
```kotlin
const val MAX_COUNT = 8
val USER_NAME_FIELD = "UserName"
```
保存带有行为的对象或者可变数据的顶层/对象属性的名称应该使用驼峰风格名称:
```kotlin
val mutableCollection: MutableSet<String> = HashSet()
```
保存单例对象引用的属性的名称可以使用与 object 声明相同的命名风格:
```kotlin
val PersonComparator: Comparator<Person> = /*......*/
```
对于枚举常量，可以使用大写、下划线分隔的名称 (screaming snake case) ( enum class Color { RED, GREEN } )也可使用首字母大写的常规驼峰名称，具体取决于用途。

### 幕后属性的名称
如果一个类有两个概念上相同的属性，一个是公共 API 的一部分，另一个是实现细节，那么 使用下划线作为私有属性名称的前缀:
```kotlin
class C {
    private val _elementList = mutableListOf<Element>()
    val elementList: List<Element>
         get() = _elementList
}
```

### 选择好名称
类的名称通常是用来解释类是什么的名词或者名词短语: List 、 PersonReader 。

方法的名称通常是动词或动词短语，说明该方法做什么: close 、 readPersons 。 修改对象 或者返回一个新对象的名称也应遵循建议。例如 sort 是对一个集合就地排序，而 sorted 是返回一个排序后的集合副本。

名称应该表明实体的目的是什么，所以最好避免在名称中使用无意义的单词 ( Manager 、 Wrapper )。

当使用首字母缩写作为名称的一部分时，如果缩写由两个字母组成，就将其大写 ( IOStream ); 而如果缩写更长一些，就只大写其首字母( XmlFormatter 、HttpInputStream )。


### 格式化
#### 缩进
使用 4 个空格缩进。不要使用 tab。 

对于花括号，将左花括号放在结构起始处的行尾，而将右花括号放在与左括结构横向对齐的
单独一行。

```kotlin
if (elements != null) {
    for (element in elements) {
        // ......
    } 
}
```
#### 横向空白
* 在二元操作符左右留空格( a + b )。例外:不要在“range to”操作符( 0..i )左右留 空格。
* 不要在一元运算符左右留空格( a++ )。
* 在控制流关键字( if 、 when 、 for 以及 while )与相应的左括号之间留空格。
* 不要在主构造函数声明、方法声明或者方法调用的左括号之前留空格

```kotlin
class A(val x: Int)

fun foo(x: Int) { ...... }

fun bar() {
    foo(1)
}
```
* 绝不在`(`、`[` 之后或者``]`、`)` 之前留空格
* 绝不在`.`或者 `?.`左右留空格:`foo.bar().filter { it > 2 }.joinToString() , foo?.bar()`
* 在 `//` 之后留一个空格: `// 这是一条注释 `
* 不要在用于指定类型参数的尖括号前后留空格: `class Map<K, V> { ...... }`
* 不要在 `:: `前后留空格:`Foo::class 、 String::length`
* 不要在用于标记可空类型的 `?` 前留空格:` String?`

作为一般规则，避免任何类型的水平对齐。将标识符重命名为不同长度的名称不应该影响声明或者任何用法的格式。

#### 冒号
在以下场景中的 `: `之前留一个空格:
* 当它用于分隔类型与超类型时 
* 当委托给一个超类的构造函数或者同一类的另一个构造函数时 
* 在 object 关键字之后

而当分隔声明与其类型时，不要在 `: `之前留空格。 在 `: `之后总要留一个空格。

```kotlin
abstract class Foo<out T : Any> : IFoo {
    abstract fun foo(a: Int): T
}
class FooImpl : Foo() {
    constructor(x: String) : this(x) { /*......*/ }
    val x = object : IFoo { /*......*/ }
}
```

#### 类头
具有少数主构造函数参数的类可以写成一行:

```kotlin
class Person(id: Int, name: String)
```
具有较长类头的类应该格式化，以使每个主构造函数参数都在带有缩进的独立的行中。 另 外，右括号应该位于一个新行上。如果使用了继承，那么超类的构造函数调用或者所实现接 口的列表应该与右括号位于同一行:
```kotlin
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name) { /*......*/ }
```
对于多个接口，应该将超类构造函数调用放在首位，然后将每个接口应放在不同的行中:
```kotlin
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name),
    KotlinMaker { /*......*/ }
```
对于具有很长超类型列表的类，在冒号后面换行，并横向对齐所有超类型名:
```kotlin
class MyFavouriteVeryLongClassHolder :
    MyLongHolder<MyFavouriteVeryLongClass>(),
    SomeOtherInterface,
    AndAnotherOne {
    fun foo() { /*......*/ }
}
```
为了将类头与类体分隔清楚，当类头很长时，可以在类头后放一空行 (如上例所示)或者将 左花括号放在独立行上:
```kotlin
class MyFavouriteVeryLongClassHolder :
    MyLongHolder<MyFavouriteVeryLongClass>(),
    SomeOtherInterface,
    AndAnotherOne
{
    fun foo() { /*......*/ }
}
```
构造函数参数使用常规缩进(4 个空格)。这确保了在主构造函数中声明的属性与 在类体中 声明的属性具有相同的缩进。

#### 修饰符
如果一个声明有多个修饰符，请始终按照以下顺序安放:
```kotlin
public / protected / private / internal
expect / actual
final / open / abstract / sealed / const
external
override
lateinit
tailrec
vararg
suspend
inner
enum / annotation / fun // 在 `fun interface` 中是修饰符 companion
inline/ value
infix
operator
data
```
将所有注解放在修饰符前:
```kotlin
@Named("Foo")
private val foo: Foo
```
除非你在编写库，否则请省略多余的修饰符(例如 public )。

#### 注解
通常将注解放在单独的行上，在它们所依附的声明之前，并使用相同的缩进:
```kotlin
@Target(AnnotationTarget.PROPERTY)
annotation class JsonExclude
```
无参数的注解可以放在同一行:
```kotlin
@JsonExclude @JvmField
var x: String
```
无参数的单个注解可以与相应的声明放在同一行:
```kotlin
@Test fun foo() { /*......*/ }
```

#### 文件注解
文件注解位于文件注释(如果有的话)之后、 package语句之前， 并且用一个空白行与package 分开(为了强调其针对文件而不是包)。

```kotlin
/** 授权许可、版权以及任何其他内容 */
@file:JvmName("FooBar")

package foo.bar
```
#### 函数
如果函数签名不适合单行，请使用以下语法:
```kotlin
fun longMethodName(
    argument: ArgumentType = defaultValue,
    argument2: AnotherArgumentType,
): ReturnType { 
    // 函数体
}
```
函数参数使用常规缩进(4 个空格)。有助于确保与构造函数参数一致 对于由单个表达式构成的函数体，优先使用表达式形式。


#### 表达式函数体格式化
如果函数的表达式函数体与函数声明不适合放在同一行，那么将`=`留在第一， 并将表达式函数体缩进 4 个空格。
```kotlin
fun f(x: String, y: String, z: String) = 
    veryLongFunctionCallWithManyWords(andLongParametersToo(), x, y, z)
```

#### 属性格式化
对于非常简单的只读属性，请考虑单行格式:
```kotlin
val isEmpty: Boolean get() = size == 0
```
对于更复杂的属性，总是将 get 与 set 关键字放在不同的行上:
```kotlin
val foo: String
    get() { /*......*/ }
```
对于具有初始化器的属性，如果初始化器很长，那么在 = 号后增加一个换行并将初始化器缩 进四个空格:
```kotlin
private val defaultCharset: Charset? = 
    EncodingRegistry.getInstance().getDefaultCharsetForPropertiesFiles(file)
```

#### 控制流语句
如果 if 或 when 语句的条件有多行，那么在语句体外边总是使用大括号。 将该条件的每个 后续行相对于条件语句起始处缩进 4 个空格。 将该条件的右圆括号与左花括号放在单独一 行:
```kotlin
if (!component.isSyncing &&
    !hasAnyKotlinRuntimeInScope(module)
){
    return createKotlinNotConfiguredPanel(module)
}
```
这有助于对齐条件与语句体。
将 `else`、 `catch` 、 `finally` 关键字以及 `do-while` 循环的 `while` 关键字与之前的花括号 放在相同的行上:
```kotlin
if (condition) { 
    // 主体
} else {
    // else 部分
}

try {
    // 主体
} finally { 
    // 清理
}
```
在 when 语句中，如果一个分支不止一行，可以考虑用空行将其与相邻的分支块分开:
```kotlin
private fun parsePropertyValue(propName: String, token: Token) {
    when (token) {
        is Token.ValueToken ->
            callback.visitValue(propName, token.value)
        Token.LBRACE -> { // ......
        } 
    }
}
```
将短分支放在与条件相同的行上，无需花括号。
```kotlin
when (foo) {
    true -> bar() // 良好 
    false -> { baz() } // 不良
}
```

#### 方法调用格式化
在较长参数列表的左括号后添加一个换行符。按 4 个空格缩进参数。 将密切相关的多个参数 分在同一行。
```kotlin
drawSquare(
    x = 10, y = 10,
    width = 100, height = 100,
    fill = true 
)  
```
在分隔参数名与值的`=`左右留空格。

### 链式调用换行
当对链式调用换行时，将` . `字符或者` ?. `操作符放在下一行，并带有单倍缩进:
```kotlin
val anchor = owner
    ?.firstChild!!
    .siblings(forward = true)
    .dropWhile { it is PsiComment || it is PsiWhiteSpace }
```
调用链的第一个调用通常在换行之前，当然如果能让代码更有意义也可以忽略这点。

#### Lambda 表达式
在 lambda 表达式中，应该在花括号左右以及分隔参数与代码体的箭头左右留空格。 如果一个调用接受单个 lambda 表达式，尽可能将其放在圆括号外边传入。
```kotlin
list.filter { it > 10 }
```
如果为 lambda 表达式分配一个标签，那么不要在该标签与左花括号之间留空格:
```kotlin
fun foo() {
    ints.forEach lit@{
    // ......
    }
}
```
在多行的 lambda 表达式中声明参数名时，将参数名放在第一行，后跟箭头与换行符:
```kotlin
appendCommaSeparated(properties) { prop ->
    val propertyValue = prop.get(obj)  // ......
}
```
如果参数列表太长而无法放在一行上，请将箭头放在单独一行:
```kotlin
foo {
   context: Context,
   environment: Env
   ->
   context.configureEnv(environment)
}
```

### 文档注释
对于较长的文档注释，将开头 /** 放在一个独立行中，并且后续每行都以星号开头:
```kotlin
/**
* 这是一条多行 
* 文档注释。 
*/
```
简短注释可以放在一行内:
```kotlin
/** 这是一条简短文档注释。 */
```
通常，避免使用 `@param` 与 `@return` 标记。而是将参数与返回值的描述直接合并到文档注释 中，并在提到参数的任何地方加上参数链接。 只有当需要不适合放进主文本流程的冗长描述 时才应使用 `@param` 与 `@return` 。
```kotlin
// 避免这样:
/**
 * Returns the absolute value of the given number.
 * @param number The number to return the absolute value for.
 * @return The absolute value.
 */
fun abs(number: Int): Int { /*......*/ }
// 而要这样:
/**
 * Returns the absolute value of the given [number].
 */
fun abs(number: Int): Int { /*......*/ }
```
### 避免重复结构
一般来说，如果 Kotlin 中的某种语法结构是可选的并且被 IDE 高亮为冗余的，那么应该在代 码中省略之。为了清楚起见，不要在代码中保留不必要的语法元素 。

#### Unit 返回类型
如果函数返回 Unit，那么应该省略返回类型:
```kotlin
fun foo() { // 这里省略了“: Unit” }
```
#### 分号
尽可能省略分号。

#### 字符串模版
将简单变量传入到字符串模版中时不要使用花括号。只有用到更长表达式时才使用花括号。
```kotlin
println("$name has ${children.size} children")
```

### 语言特性的惯用法
#### 不可变性
优先使用不可变(而不是可变)数据。初始化后未修改的局部变量与属性，总是将其声明为 `val` 而不是 `var` 。
总是使用不可变集合接口`( Collection , List , Set , Map )`来声明无需改变的集合。使用 工厂函数创建集合实例时，尽可能选用返回不可变集合类型的函数:
```kotlin
/ 不良:使用可变集合类型作为无需改变的值
fun validateValue(actualValue: String, allowedValues: HashSet<String>) { ...... }

// 良好:使用不可变集合类型
fun validateValue(actualValue: String, allowedValues: Set<String>) { ...... }

// 不良:arrayListOf() 返回 ArrayList<T>，这是一个可变集合类型
val allowedValues = arrayListOf("a", "b", "c")

// 良好:listOf() 返回 List<T>
val allowedValues = listOf("a", "b", "c")
```
#### 默认参数值
优先声明带有默认参数的函数而不是声明重载函数。
```kotlin
// 不良
fun foo() = foo("a")
fun foo(a: String) { /*......*/ }

// 良好
fun foo(a: String = "a") { /*......*/ }
```
#### 类型别名
如果有一个在代码库中多次用到的函数类型或者带有类型参数的类型，那么最好为它定义一个类型别名:
```kotlin
typealias MouseClickHandler = (Any, MouseEvent) -> Unit
typealias PersonIndex = Map<String, Person>
```
如果使用一个 `private` 或者 `internal` 的类型别名来避免名称冲突，请优先使用包与导入中提到的`import ...... as ...... `

##### Lambda 表达式参数
在简短、非嵌套的 lambda 表达式中建议使用 it 用法而不是显式声明参数。而在有参数的嵌套 lambda 表达式中，始终显式声明参数。

##### 在 lambda 表达式中返回
避免在 `lambda` 表达式中使用多个返回到标签。请考虑重新组织这样的 `lambda` 表达式使其只有 单一退出点。 如果这无法做到或者不够清晰，请考虑将 `lambda` 表达式转换为匿名函数。

不要在 `lambda` 表达式的最后一条语句中使用返回到标签。

##### 具名参数
当一个方法接受多个相同的原生类型参数或者多个 `Boolean` 类型参数时，请使用具名参数语法， 除非在上下文中的所有参数的含义都已绝对清楚。
```kotlin
drawSquare(x = 10, y = 10, width = 100, height = 100, fill = true)
```
##### 条件语句
优先使用 `try` 、 `if` 与 `when` 的表达形式。
```kotlin
return if (x) foo() else bar()

return when(x) {
    0 -> "zero"
    else -> "nonzero"
}
```
优先选用上述代码而不是:
```kotlin
if (x)
    return foo()
else
    return bar()

when(x) {
    0 -> return "zero"
    else -> return "nonzero"
}
```

##### if 还是 when
二元条件优先使用 `if` 而不是 `when` 。 例如，使用 `if` 的这种语法:
```kotlin
if (x == null) ...... else ......
```
而不是使用 when 的这种:
```kotlin
when (x) {
    null -> // ......
    else -> // ...... 
}
```
如果有三个或多个选项时优先使用 `when` 。

##### 在条件中的可空的布尔值
如果需要在条件语句中用到可空的 Boolean , 使用 if (value == true) 或 if (value == false) 检测。

##### 循环
优先使用高阶函数( `filter` 、 `map` 等)而不是循环。例外: forEach (优先使用常规的 for 循环， 除非 forEach 的接收者是可空的或者 forEach 用做长调用链的一部分。) 

当在使用多个高阶函数的复杂表达式与循环之间进行选择时，请了解每种情况下所执行操作的开销并且记得考虑性能因素。

##### 区间上循环
使用 until 函数在一个开区间上循环:
```kotlin
for(iin0..n-1){/*......*/} //不良

for(iin0untiln){/*......*/} //良好
```
##### 字符串
优先使用字符串模板而不是字符串拼接。

优先使用多行字符串而不是将 \n 转义序列嵌入到常规字符串字面值中。

如需在多行字符串中维护缩进，当生成的字符串不需要任何内部缩进时使用 trimIndent ，而 需要内部缩进时使用 trimMargin :

```kotlin
fun main() {
//sampleStart
   println("""
    Not
    trimmed
    text
    """
)
   println("""
    Trimmed
    text
    """.trimIndent()
)

    println()
   val a = """Trimmed to margin text:
|if(a > 1) {
|    return a
|}""".trimMargin()

    println(a)
//sampleEnd
}
```

##### 函数还是属性
在某些情况下，不带参数的函数可与只读属性互换。 虽然语义相似，但是在某种程度上有一 些风格上的约定。

底层算法优先使用属性而不是函数:
* 不会抛异常
* 计算开销小(或者在首次运行时缓存)
* 如果对象状态没有改变，那么多次调用都会返回相同结果


#### 扩展函数
放手去用扩展函数。每当你有一个主要用于某个对象的函数时，可以考虑使其成为一个以该 对象为接收者的扩展函数。为了尽量减少 API 污染，尽可能地限制扩展函数的可见性。根据 需要，使用局部扩展函数、成员扩展函数或者具有私有可视性的顶层扩展函数。

#### 中缀函数
一个函数只有用于两个角色类似的对象时才将其声明为中缀( infix )函数。良好示例如:and 、 to 、zip 。不良示例如:add 。 如果一个方法会改动其接收者，那么不要声明为中缀( infix )形式。

#### 工厂函数
如果为一个类声明一个工厂函数，那么不要让它与类自身同名。优先使用独特的名称， 该名 称能表明为何该工厂函数的行为与众不同。只有当确实没有特殊的语义时， 才可以使用与该 类相同的名称。
```kotlin
class Point(val x: Double, val y: Double) {
    companion object {
        fun fromPolar(angle: Double, radius: Double) = Point(......)
    } 
}
```
如果一个对象有多个重载的构造函数，它们并非调用不同的超类构造函数，并且不能简化为具有默认参数值的单个构造函数，那么优先用工厂函数取代这些重载的构造函数。

#### 平台类型
返回平台类型表达式的公有函数/方法必须显式声明其 Kotlin 类型:
```kotlin
fun apiCall(): String = MyJavaApi.getProperty("name")
```
任何使用平台类型表达式初始化的属性(包级别或类级别)必须显式声明其 Kotlin 类型:
```kotlin
class Person {
    val name: String = MyJavaApi.getProperty("name")
}
```
使用平台类型表达式初始化的局部值可以有也可以没有类型声明:
```kotlin
fun main() {
    val name = MyJavaApi.getProperty("name")
    println(name)
}
```

#### 作用域函数 apply/with/run/also/let
Kotlin 提供了一系列用来在给定对象上下文中执行代码块的函数: `let` 、 `run `、`with` 、`apply` 以及 `also` 。

### 库的编码规范
在编写库时，建议遵循一组额外的规则以确保 API 的稳定性:
* 总是显式指定成员的可见性(以避免将声明意外暴露为公有 API ) 
* 总是显式指定函数返回类型以及属性类型(以避免当实现改变时意外更改返回类型) 
* 为所有公有成员提供 KDoc 注释，不需要任何新文档的覆盖成员除外 (以支持为该库生 成文档)
