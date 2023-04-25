---
layout: post
title: Kotlin教程-概念
categories: Android
tags: Android、Kotlin
---

# 类型

## 基本类型
在 Kotlin 中，所有东西都是对象，在这个意义上讲可以在任何变量上调用成员函数与属性。 一些类型可以有特殊的内部表示——例如，数字、字符以及布尔可以在运行时表示为原生类型值。

### 数字
#### 整数类型
Kotlin 提供了一组表示数字的内置类型。 对于整数，有四种不同大小的类型，因此值的范围 也不同:

| 类型 | 大小(比特 数) | 最小值 | 最大值 |
| :------ |:--- | :--- | :--- |
| Byte | 8 | -128 | 127 |
| Short | 16 | -32768 | 32767 |
| Int |32 | -2,147,483,648 (-2<sup>31</sup>) | 2,147,483,647 (2<sup>31</sup> - 1) |
| Long | 64 | -9,223,372,036,854,775,808(-2<sup>63</sup>) | 9,223,372,036,854,775,807(2<sup>63</sup>-1) |

当初始化一个没有显式指定类型的变量时，编译器会自动推断为足以表示该值的最小类型。如果不超过`Int`的表示范围，那么类型是 `Int` 。 如果超过了， 那么类型是 `Long` 。 如需显式指定 `Long `值，请给该值追加后缀 `L` 。 显式指定类型会触发编译器检测该值是否超出指定类型的表示范围。

```Kotlin
val one = 1 // Int
val threeBillion = 3000000000 // Long
val oneLong = 1L // Long
val oneByte: Byte = 1
```
#### 浮点类型
对于实数，Kotlin 提供了浮点类型 `Float` 与 `Double` 类型，`Float`表达单精度，而 `Double` 表达双精度。
这两个类型的大小不同，并为两种不同精度的浮点数提供存储:
| 类型 | 大小(比特数) | 有效数字比特数 | 指数比特数 | 十进制位数 |
| :------ |:--- | :--- | :--- | :--- |
| Float | 32 | 24 | 8 | 6-7 |
| Double | 64 | 53 | 11 | 15-16 |

可以使用带小数部分的数字初始化 Double 与 Float 变量。 小数部分与整数部分之间用句 点( . )分隔 对于以小数初始化的变量，编译器会推断为 Double 类型:

```Kotlin
val pi = 3.14 // Double
// val one: Double = 1 // 错误:类型不匹配 
val oneDouble = 1.0 // Double
```
如需将一个值显式指定为 Float 类型，请添加 f 或 F 后缀。 如果这样的值包含多于 6~7 位十进制数，那么会将其舍入:

```Kotlin
val e = 2.7182818284 // Double
val eFloat = 2.7182818284f // Float，实际值为 2.7182817
```
与一些其他语言不同，Kotlin 中的数字没有隐式拓宽转换。 例如，具有 Double 参数的函数 只能对 Double 值调用，而不能对 Float 、 Int 或者其他数字值调用:
```Kotlin
fun main() {
    fun printDouble(d: Double) { print(d) }

	val i = 1
	val d = 1.0
	val f = 1.0f
	printDouble(d)
//	printDouble(i) // 错误:类型不匹配 
// 	printDouble(f) // 错误:类型不匹配
}
```
#### 数字字面常量
数值常量字面值有以下几种:

* 十进制: 123
	* `Long` 类型用大写`L `标记: 123L
* 十六进制: 0x0F 
* 二进制: 0b00001011

Kotlin 同样支持浮点数的常规表示方法:

* 默认 `double`: 123.5 、 123.5e10
* `Float` 用 f 或者 F 标记: 123.5f

```Kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```

#### 显式数字转换
较小的类型不能隐式转换为较大的类型。 这意味着把`Byte` 型值赋给一个`Int` 变量必须显式转换:

```Kotlin
fun main() {
	//sampleStart
	val b: Byte = 1 // OK, 字面值会静态检测 // val i: Int = b // 错误
	val i1: Int = b.toInt()
	//sampleEnd
}
```
所有数字类型都支持转换为其他类型:

*  `toByte(): Byte`
*  `toShort(): Short`
*  `toInt(): Int`
*  `toLong(): Long`
*  `toFloat(): Float`
* ` toDouble(): Double`

很多情况都不需要显式类型转换，因为类型会从上下文推断出来，而算术运算会有重载做适当转换。

#### 数字运算
Kotlin支持数字运算的标准集: `+ 、 - 、 * 、 / 、 %` 。它们已定义为相应的类成员:
##### 1、整数除法
整数间的除法总是返回整数。会丢弃任何小数部分。

```Kotlin
fun main() {
//sampleStart
    val x = 5 / 2
    //println(x == 2.5) // ERROR: Operator '==' cannot be applied to 'Int' and 'Double'
   
    println(x == 2)
//sampleEnd
}
```
如需返回浮点类型，请将其中的一个参数显式转换为浮点类型:

```Kotlin
fun main() {
//sampleStart
    val x = 5 / 2.toDouble()
    println(x == 2.5)
//sampleEnd
}
```

##### 2、位运算
Kotlin 对整数提供了一组位运算。它们直接使用数字的比特表示在二进制级别进行操作。位运算有可以通过中缀形式调用的函数表示。只能应用于 `Int` 与 `Long`。
完整的位运算列表:

* `shl(bits)`：有符号左移
* `shr(bits) `：有符号右移
* `ushr(bits) `：无符号右移
* `and(bits) `：位与
* `or(bits) `：位或
* `xor(bits) `：位异或
* `inv() `：位非

##### 3、浮点数比较
浮点数操作如下:

* 相等性检测：`a==b `与`a!=b`
* 比较操作符: `a < b `、 `a > b` 、 `a <= b` 、` a >= b` 
* 区间实例以及区间检测: `a..b `、 `x in a..b` 、 `x !in a..b`


###   布尔
`Boolean` 类型表示可以有 `true` 与 `false` 两个值的布尔对象。
`Boolean` 的可空版 `Boolean?` 还有 `null` 值。

布尔值的内置运算有:

* || ——析取(逻辑或) 
* && ——合取(逻辑与) 
* ! ——否定(逻辑非)

|| 与 && 都是惰性(短路)的。

```Kotlin
fun main() {
//sampleStart
    val myTrue: Boolean = true
    val myFalse: Boolean = false
    val boolNull: Boolean? = null
    println(myTrue || myFalse)
    println(myTrue && myFalse)
    println(!myTrue)
//sampleEnd
}
```
###  字符
字符用 `Char` 类型表示。 字符字面值用单引号括起来: `'1'` 。
特殊字符可以以转义反斜杠`\ `开始。 支持这几个转义序列:

* \t ——制表符
* \b ——退格符
* \n ——换行(LF)
* \r ——回车(CR)
* \' ——单引号
* \" ——双引号
* \\ ——反斜杠
* \$ ——美元符

编码其他字符要用 Unicode 转义序列语法: `'\uFF00' `。

```Kotlin
fun main() {
//sampleStart
   val aChar: Char = 'a'
	println(aChar)
	println('\n') // 输出一个额外的换行符 println('\uFF00')
//sampleEnd
}
```
如果字符变量的值是数字，那么可以使用 `digitToInt()` 函数将其显式转换为 `Int` 数字。

###  字符串
Kotlin 中字符串用 `String` 类型表示。 通常，字符串值是双引号( " )中的字符序列:

```Kotlin
val str = "abcd 123"
```

字符串的元素——字符可以使用索引运算符访问: s[i] 。 可以使用 for 循环遍历这些字符:

```Kotlin
val str = "abcd 123"
fun main() {
	val str = "abcd"
//sampleStart
	for (c in str) {
		println(c) 
	}
//sampleEnd
}
```
字符串是不可变的。 一旦初始化了一个字符串，就不能改变它的值或者给它赋新值。 所有转 换字符串的操作都以一个新的`String` 对象来返回结果，而保持原始字符串不变:

```Kotlin
fun main() {
//sampleStart
	val str = "abcd"
	println(str.uppercase()) // 创建并输出一个新的 String 对象 println(str) // 原始字符串保持不变
//sampleEnd
}
```
如需连接字符串，可以用 + 操作符。这也适用于连接字符串与其他类型的值， 只要表达式 中的第一个元素是字符串:

#### 字符串字面值
Kotlin 有两种类型的字符串字面值:

* 转义字符串：转义字符串可以包含转义字符。采用传统的反斜杠( \ )方式。
*  原始字符串：原始字符串可以包含换行以及任意文本。 它使用三个引号( """ )分界符括起来，内部没有转义并且可以包含换行以及任何其他字符:
*  
```Kotlin
val s = "Hello, world!\n" //转义字符串

//原始字符串
val text = """
    for (c in "foo")
"""
```
如需删掉原始字符串中的前导空格，请使用 `trimMargin() `函数。默认以竖线` | `作为边界前缀，但你可以选择其他字符并作为参数传入，比如 trimMargin(">") 。

#### 字符串模板
字符串字面值可以包含模板表达式——一些小段代码，会求值并把结果合并到字符串中。 模 板表达式以美元符( $ )开头，要么由一个的名称构成:

```Kotlin
fun main() {
//sampleStart
	val i = 10
	println("i = $i") // 输出“i = 10” 
	//sampleEnd
}
```
要么是用花括号括起来的表达式:
```Kotlin
fun main() {
//sampleStart
	val s = "abc"
	println("$s.length is ${s.length}") // 输出 "abc.length is 3" 
	//sampleEnd
}
```
在原始字符串及转义字符串中都可以使用模板。

###  数组
数组在 Kotlin 中使用 Array 类来表示。它定义了 `get()` 与` set ()` 函数(按照运算符重载约定这会转变为 [] )与 size 属性及其他有用的成员函数:

```Kotlin
class Array<T> private constructor() {
    val size: Int
    operator fun get(index: Int): T
    operator fun set(index: Int, value: T): Unit
    operator fun iterator(): Iterator<T>
// ...
}
```
可以使用函数 arrayOf() 来创建一个数组并传递元素值给它，这样 arrayOf(1, 2, 3) 创建了 array [1, 2, 3] 。 或者，函数 arrayOfNulls() 可以用于创建一个指定大小的、所有元素都为空的数组。

用接受数组大小以及一个函数参数的 Array 构造函数，用作参数的函数能够返 回给定索引的元素:

```Kotlin
fun main() {
//sampleStart
	// 创建一个 Array<String> 初始化为 ["0", "1", "4", "9", "16"] 
	val asc = Array(5) { i -> (i * i).toString() }
	asc.forEach { println(it) }
//sampleEnd
}
```
[] 运算符代表调用成员函数 get() 与 set() 。

Kotlin 也有无装箱开销的类来表示原生类型数组: ByteArray 、 ShortArray 、 IntArray 等 等。这些类与 Array 并没有继承关系，但是它们有同样的方法属性集。它们也都有相应的工 厂方法:

```Kotlin
val x: IntArray = intArrayOf(1, 2, 3)
x[0] = x[1] + x[2]

// 大小为 5、值为 [0, 0, 0, 0, 0] 的整型数组
 val arr = IntArray(5)
 
// 用常量初始化数组中的值的示例
// 大小为 5、值为 [42, 42, 42, 42, 42] 的整型数组 
val arr = IntArray(5) { 42 }

// 使用 lambda 表达式初始化数组中的值的示例
// 大小为 5、值为 [0, 1, 2, 3, 4] 的整型数组(值初始化为其索引值) 
var arr = IntArray(5) { it * 1 }
```
###  无符号整型
除了整数类型，对于无符号整数，Kotlin 还提供了以下类型:

* UByte :无符号8比特整数，范围是0到255 
* UShort :无符号16比特整数，范围是0到65535 
* UInt :无符号32比特整数，范围是0到2^32-1 
* ULong :无符号64比特整数，范围是0到2^64-1

与原生类型相同，每个无符号类型都有表示相应类型数组的类型:

* UByteArray :无符号字节数组 
* UShortArray :无符号短整型数组 
* UIntArray :无符号整型数组 
* ULongArray :无符号长整型数组

## 类型检测与类型转换

### is 与 !is 操作符
使用 is 操作符或其否定形式 !is 在运行时检测对象是否符合给定类型:

```Kotlin
if (obj is String) {
    print(obj.length)
}

if (obj !is String) { // 与 !(obj is String) 相同 
	print("Not a String")
} else {
    print(obj.length)
}
```

### 智能转换
大多数场景都不需要在Kotlin中使用显式转换操作符，因为编译器跟踪不可变值的 is -检测
以及显式转换，并在必要时自动插入(安全的)转换:

```Kotlin
fun demo(x: Any) {
    if (x is String) {
		print(x.length) // x 自动转换为字符串 
	}
}
```
编译器足够聪明，能够知道如果反向检测导致返回那么该转换是安全的:

```Kotlin
if (x !is String) return 

print(x.length) // x 自动转换为字符串
```
或者转换在 && 或 || 的右侧，而相应的(正常或否定)检测在左侧:

```Kotlin
// `||` 右侧的 x 自动转换为 String
if (x !is String || x.length == 0) return

// `&&` 右侧的 x 自动转换为 String
if (x is String && x.length > 0) {
	print(x.length) // x 自动转换为 String
}
```
当编译器能保证变量在检测和使用之间不可改变时，智能转换才有效。智能转换适用于以下情形:

* val 局部变量
* val 属性
* var 局部变量
* var 属性

### “不安全的”转换操作符
如果转换是不可能的，转换操作符会抛出一个异常。因此，称为不安全的。 Kotlin 中的不安全转换使用中缀操作符 as 。

```Kotlin
val x: String = y as String
```

请注意， null 不能转换为 String 因该类型不是可空的。 如果 y 为空，上面的代码会抛 出一个异常。 为了让这样的代码用于可空值，请在类型转换的右侧使用可空类型:

```Kotlin
val x: String = y as String?
```

### “安全的”(可空)转换操作符
为了避免异常，可以使用安全转换操作符 as? ，它可以在失败时返回 null :

```Kotlin
val x: String? = y as? String
```

尽管事实上 as? 的右边是一个非空类型的 String ，但是其转换的结果是可空的。

# 控制流程

## 条件与循环
### If 表达式
在 Kotlin 中， `if` 是一个表达式:它会返回一个值。 因此就不需要三元运算符( 条件 ? 然后 : 否则 )，因为普通的 if 就能胜任这个角色。

```Kotlin
fun main() {
  val a = 2
  val b = 3
  
  //sampleStart
  var max = a
  if (a < b) max = b
  
// With else
  if (a > b) {
    max = a
 } else { 
   max = b
}

// 作为表达式
max = if (a > b) a else b

  // You can also use `else if` in expressions:
  val maxLimit = 1
  val maxOrLimit = if (maxLimit > a) maxLimit else if (a > b) a else b
  
//sampleEnd
  println("max is $max")
  println("maxOrLimit is $maxOrLimit")
}
```
if 表达式的分支可以是代码块，这种情况最后的表达式作为该块的值:

```Kotlin
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
 }
```
### When 表达式
`When`定义具有多个分支的条件表达式，在类C语言中类似于`switch`语句。

```Kotlin
when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> {
        print("x is neither 1 nor 2")
    }
}
```
when 将它的参数与所有的分支条件顺序比较，直到某个分支满足条件。
when 既可以作为表达式使用也可以作为语句使用。如果它被当做表达式， 第一个符合条件 的分支的值就是整个表达式的值，如果当做语句使用，则忽略个别分支的值。类似于 if ， 每一个分支可以是一个代码块，它的值是块中最后的表达式的值。

如果其他分支都不满足条件将会求值 else 分支。 如果 when 作为一个表达式使用，那么必 须有 else 分支， 除非编译器能够检测出所有的可能情况都已经覆盖了。

可以检测一个值在( in )或者不在( !in )一个区间或者集合中:

```Kotlin
when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}
```
`when` 也可以用来取代 `if - else if` 链。 如果不提供参数，所有的分支条件都是简单的布 尔表达式，而当一个分支的条件为真时则执行该分支:

```Kotlin
when {
    x.isOdd() -> print("x is odd")
    y.isEven() -> print("y is even")
    else -> print("x+y is odd")
}
```
可以使用以下语法将 when 的主语(subject，译注:指 when 所判断的表达式)捕获到变量 中:

```Kotlin
fun Request.getBody() =
    when (val response = executeRequest()) {
        is Success -> response.body
        is HttpError -> throw HttpException(response.status)
    }
```
在 when 主语中引入的变量的作用域仅限于 when 主体。

### For 循环
for 循环可以对任何提供迭代器(iterator)的对象进行遍历， for 的语法如下所示:

```Kotlin
for (item in collection) print(item)

//for 循环体可以是一个代码块。
for (item: Int in ints) {
    // ......
}
```
for 可以循环遍历任何提供了迭代器的对象。这意味着:

* 有一个成员函数或者扩展函数 iterator() 返回 Iterator<> : 
	* 有一个成员函数或者扩展函数 next() 
	* 有一个成员函数或者扩展函数 hasNext() 返回 Boolean 。

这三个函数都需要标记为 operator 。

如需在数字区间上迭代，请使用区间表达式:

```Kotlin
fun main() {
//sampleStart
    for (i in 1..3) {
        println(i)
    }
    
    for (i in 6 downTo 0 step 2) {
    	   println(i) 
    }
//sampleEnd
}
```
对区间或者数组的 for 循环会被编译为并不创建迭代器的基于索引的循环。

遍历一个数组或者一个 list：

```Kotlin
fun main() {
	val array = arrayOf("a", "b", "c")
	
//通过索引
      for (i in array.indices) {
          println(array[i])
	}
	
//通过库函数withIndex 
    for ((index, value) in array.withIndex()) {
        println("the element at $index is $value")
    }
}
```

### while 循环
`while`和`do-while`循环在满足条件时持续执行它们的循环体。它们之间的区别是状态检查时间: 
* `while`检查条件，如果满足条件，则执行语句体，然后返回条件检查。 
* `do-while`执行语句体，然后检查条件。如果满足，循环重复。因此，无论条件如何，do-while函数体至少执行一次。

```Kotlin
while (x > 0) {
    x--
}

do {
  val y = retrieveData()
} while (y != null) // y 在此处可见
```
在循环中 Kotlin 支持传统的 break 与 continue 操作符。

## 返回与跳转
Kotlin 有三种结构化跳转表达式:

* return 默认从最直接包围它的函数或者匿名函数返回。
* break 终止最直接包围它的循环。
* continue 继续下一次最直接包围它的循环。

所有这些表达式都可以用作更大表达式的一部分:

```Kotlin
val s = person.name ?: return
```

在 Kotlin 中任何表达式都可以用标签来标记。 标签的格式为标识符后跟`@ `符号， 要为一个表达式加标签，我们只要在其前加标签即可。
可以用标签限定 break 或者 continue :
```Kotlin
loop@ for (i in 1..100) {
    for (j in 1..100) {
        if (......) break@loop
    }
}
```
标签限定的 `break` 跳转到刚好位于该标签指定的循环后面的执行点。 `continue` 继续标签指定的循环的下一次迭代。

##  异常
Kotlin 中所有异常类继承自 Throwable 类。 每个异常都有消息、堆栈回溯信息以及可选的原因。
使用 throw 表达式来抛出异常:

```Kotlin
fun main() {
//sampleStart
    throw Exception("Hi There!")
//sampleEnd
}
```
使用 try ...... catch 表达式来捕获异常:

```Kotlin
try {
	// 一些代码
} catch (e: SomeException) { 
	// 处理程序
} finally {
	// 可选的 finally 块
}
```
可以有零到多个 catch 块， finally 块可以省略。 但是 catch 与 finally 块至少需有一 个。

try 是一个表达式，意味着它可以有一个返回值:

```Kotlin
val a: Int? = try { input.toInt() } catch (e: NumberFormatException) { null }
```
try -表达式的返回值是 try 块中的最后一个表达式或者是(所有) catch 块中的最后一个 表达式。 finally 块中的内容不会影响表达式的结果。


# 包与导入
源文件通常以包声明开头:

```Kotlin
package org.example

fun printMessage() { /*......*/ }
class Message { /*......*/ }

// ......
```

源文件所有内容(无论是类还是函数)都包含在该包内。 所以上例中` printMessage()` 的全名是` org.example.printMessage` ， 而 `Message` 的全名是`org.example.Message`。
如果没有指明包，该文件的内容属于无名字的默认包.

## 默认导入
有多个包会默认导入到每个 Kotlin 文件中:

* kotlin.* 
* kotlin.annotation.* 
* kotlin.collections.* 
* kotlin.comparisons.* 
* kotlin.io.* 
* kotlin.ranges.* 
* kotlin.sequences.* 
* kotlin.text.*

根据目标平台还会导入额外的包:

* JVM:
	* java.lang.*
	* kotlin.jvm.*
* JS:
	* kotlin.js.*

## 导入
除了默认导入之外，每个文件可以包含它自己的导入( import )指令。

1、导入一个单个名称: 

```Kotlin
import org.example.Message // 现在 Message 可以不用限定符访问
```

2、可以导入一个作用域下的所有内容:包、类、对象等:

```Kotlin
import org.example.* // “org.example”中的一切都可访问
```

3、如果出现名字冲突，使用 as 关键字在本地重命名冲突项来消歧义:

```Kotlin
import org.example.Message // Message 可访问
import org.test.Message as TestMessage // TestMessage 代表“org.test.Message”
```

# 类与对象
## 类
Kotlin 中使用关键字 class 声明类

```Kotlin
class Person { /*......*/ }
```
类声明由类名、类头(指定其类型参数、主构造函数等)以及由花括号包围的类体构成。类 头与类体都是可选的; 如果一个类没有类体，可以省略花括号。

```Kotlin
class Empty
```

### 构造函数
在 Kotlin 中的一个类可以有一个主构造函数以及一个或多个次构造函数。主构造函数是类头 的一部分:它跟在类名与可选的类型参数后。

```Kotlin
class Person constructor(firstName: String) { /*......*/ }
```
如果主构造函数没有任何注解或者可见性修饰符，可以省略这个 constructor 关键字。

```Kotlin
class Person(firstName: String) { /*......*/ }
```
主构造函数不能包含任何的代码。初始化的代码可以放到以 init 关键字作为前缀的初始化 块(initializer blocks)中。
在实例初始化期间，初始化块按照它们出现在类体中的顺序执行，与属性初始化器交织在一起:


```Kotlin
//sampleStart
class InitOrderDemo(name: String) {
    val firstProperty = "First property: $name".also(::println)
    
    init {
        println("First initializer block that prints $name")
    }
	
    val secondProperty = "Second property: ${name.length}".also(::println)
	
    init {
        println("Second initializer block that prints ${name.length}")
     }
}
//sampleEnd

fun main() {
    InitOrderDemo("hello")
}
```
主构造的参数可以在初始化块中使用。它们也可以在类体内声明的属性初始化器中使用:

```Kotlin
class Customer(name: String) {
    val customerKey = name.uppercase()
}

//声明属性并从主构造函数初始化它们:
class Person(val firstName: String, val lastName: String, var age: Int)

//声明也可以包括类属性的默认值:
class Person(val firstName: String, val lastName: String, var isEmployed: Boolean = tr ue)
```

如果构造函数有注解或可见性修饰符，这个 constructor 关键字是必需的，并且这些修饰符
在它前面:

```Kotlin
class Customer public @Inject constructor(name: String) { /*......*/ }
```
### 次构造函数
类也可以声明前缀有 constructor 的次构造函数:

```Kotlin
class Person(val pets: MutableList<Pet> = mutableListOf())
class Pet {
    constructor(owner: Person) {
        owner.pets.add(this) // adds this pet to the list of its owner's pets
    }
}
```
如果类有一个主构造函数，每个次构造函数需要委托给主构造函数， 可以直接委托或者通过别的次构造函数间接委托。委托到同一个类的另一个构造函数用 this 关键字即可:

```Kotlin
class Person(val name: String) {
    val children: MutableList<Person> = mutableListOf()
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
}
```
初始化块中的代码实际上会成为主构造函数的一部分。对主构造函数的委托发生在访问次构造函数的第一条语句时，因此所有初始化块与属性初始化器中的代码都会在次构造函数体之前执行。
即使该类没有主构造函数，这种委托仍会隐式发生，并且仍会执行初始化块:

```Kotlin
//sampleStart
class Constructors {
    init {
        println("Init block")
    }
    
    constructor(i: Int) {
        println("Constructor $i")
} }
//sampleEnd

fun main() {
    Constructors(1)
}
```
如果一个非抽象类没有声明任何(主或次)构造函数，它会有一个生成的不带参数的主构造 函数。构造函数的可见性是 public。
如果你不希望你的类有一个公有构造函数，那么声明一个带有非默认可见性的空的主构造函数:

```Kotlin
class DontCreateMe private constructor() { /*......*/ }
```
### 创建类的实例
创建一个类的实例，只需像普通函数一样调用构造函数:

```Kotlin
val invoice = Invoice()
val customer = Customer("Joe Smith")
```
### 类成员
类可以包含:

*   构造函数与初始化块
*   函数
*   属性
*   嵌套类与内部类
*   对象声明
类可以彼此派生并形成继承层次结构。

### 抽象类
类以及其中的某些或全部成员可以声明为 `abstract`。抽象成员在本类中可以不用实现。并不需要用 `open` 标注抽象类或者函数。

```Kotlin
abstract class Polygon {
    abstract fun draw()
}

class Rectangle : Polygon() {
     override fun draw() {
     // draw the rectangle
	}
}
```
可以用一个抽象成员覆盖一个非抽象的开放成员。

```Kotlin
open class Polygon {
    open fun draw() {
} }

// some default polygon drawing method
abstract class WildShape : Polygon() {
    // Classes that inherit WildShape need to provide their own
    // draw method instead of using the default on Polygon
    abstract override fun draw()
}
```
### 伴生对象
如果你需要写一个可以无需用一个类的实例来调用、但需要访问类内部的函数(例如，工厂方法)，你可以把它写成该类内对象声明中的一员。

更具体地讲，如果在你的类内声明了一个伴生对象， 你就可以访问其成员，只是以类名作为限定符。

## 继承
在Kotlin中所有类都有一个共同的超类 Any ，对于没有超类型声明的类它是默认超类:

```Kotlin
class Example // 从 Any 隐式继承
```
Any 有三个方法: `equals()` 、 `hashCode() `与 `toString()` 。因此，为所有 Kotlin 类都定义了这些方法。
默认情况下，Kotlin 类是最终(`final`)的——它们不能被继承。 要使一个类可继承，请用 open 关键字标记它:

```Kotlin
open class Base // 该类开放继承
```
如需声明一个显式的超类型，请在类头中把超类型放到冒号之后:

```Kotlin
open class Base(p: Int)

class Derived(p: Int) : Base(p)
```
如果派生类有一个主构造函数，其基类可以(并且必须)根据其参数在该主构造函数中初始化。

如果派生类没有主构造函数，那么每个次构造函数必须使用 super 关键字初始化其基类型， 或委托给另一个做到这点的构造函数。 请注意，在这种情况下，不同的次构造函数可以调用 基类型的不同的构造函数:

```Kotlin
class MyView : View {
    constructor(ctx: Context) : super(ctx)
    constructor(ctx: Context, attrs: AttributeSet) : super(ctx, attrs)
}
```

### 覆盖方法
Kotlin 对于可覆盖的成员以及覆盖后的成员需要显式修饰符:

```Kotlin
open class Shape {
    open fun draw() { /*......*/ }
    fun fill() { /*......*/ }
}

class Circle() : Shape() {
    override fun draw() { /*......*/ }
}
```
`Circle.draw() `函数上必须加上 `override` 修饰符。如果函数没有标注 `open` 如 Shape.fill() ，那么子类中不允许定义相同签名的函数 。将 `open` 修饰符添加到` final `类(即没有 open 的类) 的成员上不起作用。 标记为 `override` 的成员本身是开放的，因此可以在子类中覆盖。如果你想禁止再次覆盖，
使用 final 关键字:

```Kotlin
open class Rectangle() : Shape() {
    final override fun draw() { /*......*/ }
}
```
### 覆盖属性
属性与方法的覆盖机制相同。在超类中声明然后在派生类中重新声明的属性必须以 override 开头，并且它们必须具有兼容的类型。 每个声明的属性可以由具有初始化器的属性或者具有
get 方法的属性覆盖:

```Kotlin
open class Shape {
    open val vertexCount: Int = 0
}

class Rectangle : Shape() {
    override val vertexCount = 4
}
```
也可以用一个 `var` 属性覆盖一个 `val` 属性，但反之则不行。 因为一个 `val` 属性本质上声明了一个 `get` 方法， 而将其覆盖为 `var` 只是在子类中额外声明一个 `set` 方法。
可以在主构造函数中使用 override 关键字作为属性声明的一部分:

```Kotlin
interface Shape {
    val vertexCount: Int
}

class Rectangle(override val vertexCount: Int = 4) : Shape // 总是有 4 个顶点

class Polygon : Shape {
	override var vertexCount: Int = 0 // 以后可以设置为任何数
}
```
### 派生类初始化顺序
在构造派生类的新实例的过程中，第一步完成其基类的初始化 ，这意味着它发生在派生类的初始化逻辑运行之前。

```Kotlin
//sampleStart
open class Base(val name: String) {
    init { println("Initializing a base class") }
    open val size: Int =
        name.length.also { println("Initializing size in the base class: $it") }
}

class Derived(
    name: String,
val lastName: String,
) : Base(name.replaceFirstChar { it.uppercase() }.also { println("Argument for the bas e class: $it") }) {
	  init { println("Initializing a derived class") }
    
	override val size: Int =
		(super.size + lastName.length).also { println("Initializing size in the derived class: $it") }
}

//sampleEnd
fun main() {
    println("Constructing the derived class(\"hello\", \"world\")")
    
    Derived("hello", "world")
}
```
基类构造函数执行时，派生类中声明或覆盖的属性都还没有初始化。在基类初始化逻辑中(直接或者通过另一个覆盖的 open 成员的实现间接)使用任何一个这种属性，都可能导致不正确的行为或运行时故障。 设计一个基类时，应该避免在构造函数、属性初始化器或者 init 块中使用 open 成员。

### 调用超类实现
派生类中的代码可以使用 super 关键字调用其超类的函数与属性访问器的实现:

```Kotlin
open class Rectangle {
    open fun draw() { println("Drawing a rectangle") }
    val borderColor: String get() = "black"
}

class FilledRectangle : Rectangle() {
    override fun draw() {
        super.draw()
        println("Filling the rectangle")
    }
    
    val fillColor: String get() = super.borderColor
}
```
在一个内部类中访问外部类的超类，可以使用由外部类名限定的 `super` 关键字来实 现: `super@Outer `:
```Kotlin
open class Rectangle {
    open fun draw() { println("Drawing a rectangle") }
    val borderColor: String get() = "black"
}

//sampleStart
class FilledRectangle: Rectangle() {
    override fun draw() {
        val filler = Filler()
        filler.drawAndFill()
    }
    
    inner class Filler {
        fun fill() { println("Filling") }
        fun drawAndFill() {
			super@FilledRectangle.draw() // 调用 Rectangle 的 draw() 实现
			fill()
			println("Drawn a filled rectangle with color ${super@FilledRectangle.borderColor}") // 使用 Rectangle 所实现的 borderColor 的 get() }
		}
  }
//sampleEnd

fun main() {
    val fr = FilledRectangle()
    fr.draw()
}
```

### 覆盖规则
在 Kotlin 中，实现继承由下述规则规定:如果一个类从它的直接超类继承相同成员的多个实现， 它必须覆盖这个成员并提供其自己的实现(也许用继承来的其中之一)。
如需表示采用从哪个超类型继承的实现，请使用由尖括号中超类型名限定的 `super` ，如 `super<Base> `:

```Kotlin
open class Rectangle {
    open fun draw() { /* ...... */ }
}

interface Polygon {
	fun draw() { /* ...... */ } // 接口成员默认就是“open”的
}

class Square() : Rectangle(), Polygon { 
	// 编译器要求覆盖 draw():
	override fun draw() {
		super<Rectangle>.draw() // 调用 Rectangle.draw()
		super<Polygon>.draw() // 调用 Polygon.draw() 
	}
}
```
可以同时继承 Rectangle 与 Polygon ， 但是二者都有各自的 draw() 实现，所以必须在 Square 中覆盖 draw() ， 并为其提供一个单独的实现以消除歧义。

## 属性
### 声明属性
Kotlin 类中的属性既可以用关键字 `var` 声明为可变的， 也可以用关键字 `val` 声明为只读的。

```Kotlin
class Address {
    var name: String = "Holmes, Sherlock"
    var street: String = "Baker"
    var city: String = "London"
    var state: String? = null
    var zip: String = "123456"
}

//使用一个属性，以其名称引用它即可:
fun copyAddress(address: Address): Address {
	val result = Address() // Kotlin 中没有“new”关键字 
	result.name = address.name // 将调用访问器 
	result.street = address.street
	// ......
	return result
}
```
### Getter 与 Setter
声明一个属性的完整语法如下:

```Kotlin
var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
```
其初始器(initializer)、getter 和 setter 都是可选的。属性类型如果可以从初始器， 或其 getter 的返回值(如下文所示)中推断出来，也可以省略:

```Kotlin
var initialized = 1 // 类型 Int、默认 getter 和 setter
// var allByDefault // 错误:需要显式初始化器，隐含默认 getter 和 setter
```
一个只读属性的语法和一个可变的属性的语法有两方面的不同: 

1. 只读属性的用 val 而不 是 var 声明 
1. 只读属性不允许 setter

```Kotlin
val simple: Int? // 类型 Int、默认 getter、必须在构造函数中初始化 
val inferredType = 1 // 类型 Int 、默认 getter
```
可以为属性定义自定义的访问器。如果定义了一个自定义的 getter，那么每次访问该属性时都 会调用它 (这让可以实现计算出的属性)。以下是一个自定义 getter 的示例:
```Kotlin
//sampleStart
class Rectangle(val width: Int, val height: Int) {
	  val area: Int // property type is optional since it can be inferred from the getter's return type
        get() = this.width * this.height
}
//sampleEnd

fun main() {
	val rectangle = Rectangle(3, 4)
	println("Width=${rectangle.width}, height=${rectangle.height}, area=${rectangle.area}") 
}

//如果可以从 getter 推断出属性类型，则可以省略它:
val area get() = this.width * this.height
```

如果定义了一个自定义的 setter，那么每次给属性赋值时都会调用它, except its initialization. 一 个自定义的 setter 如下所示:

```Kotlin
var stringRepresentation: String
    get() = this.toString()
    set(value) {
       	setDataFromString(value) // 解析字符串并赋值给其他属性 
  }
```
如果需要改变对一个访问器进行注解或者改变其可见性，但是不希望改变默认的实现， 那么 可以定义访问器而不定义其实现:


```Kotlin
var setterVisibility: String = "abc"
	private set // 此 setter 是私有的并且有默认实现

var setterWithAnnotation: Any? = null 
	@Inject set // 用 Inject 注解此 setter
```

### 幕后字段
在 Kotlin 中，字段仅作为属性的一部分在内存中保存其值时使用。字段不能直接声明。 然 而，当一个属性需要一个幕后字段时，Kotlin 会自动提供。这个幕后字段可以使用 field 标 识符在访问器中引用:

```Kotlin
var counter = 0 // 这个初始器直接为幕后字段赋值 
	set(value) {
        if (value >= 0)
            field = value
            // counter = value // ERROR StackOverflow: Using actual name 'counter' would make setter recursive
}
```
field 标识符只能用在属性的访问器内。

如果属性至少一个访问器使用默认实现， 或者自定义访问器通过为该属性生成一个幕后字段。

### 幕后属性
如果你的需求不符合这套隐式的幕后字段方案， 那么总可以使用 幕后属性(backing property):

```Kotlin
private var _table: Map<String, Int>? = null
public val table: Map<String, Int>
    get() {
        if (_table == null) {
		_table = HashMap() // 类型参数已推断出 
	}
      return _table ?: throw AssertionError("Set to null by another thread")
  }
```
### 编译期常量
如果只读属性的值在编译期是已知的，那么可以使用 const 修饰符将其标记为编译期常量。 这种属性需要满足以下要求:

* 必须位于顶层或者是 object 声明 或伴生对象的一个成员 
* 必须以 String 或原生类型值初始化
* 不能有自定义 getter

### 延迟初始化属性与变量
一般地，属性声明为非空类型必须在构造函数中初始化。在类体中引用该属性时避免空检测，可以使用`lateinit`修饰符标记该属性:

```Kotlin
public class MyTest {
    lateinit var subject: TestSubject
    
	@SetUp fun setup() {
	     subject = TestSubject()
	}

	@Test fun test() {
	 subject.method() // 直接解引用
	} 
}
```
该修饰符只能用于在类体中的属性，也用于顶层属性与局部变量。 该属性或变量必须为非空类 型，并且不能是原生类型。

### 检测一个 lateinit var 是否已初始化
要检测一个 lateinit var 是否已经初始化过，请在该属性的引用上使用 .isInitialized :

```Kotlin
if (foo::bar.isInitialized) {
    println(foo.bar)
}
```
检测仅对可词法级访问的属性可用，当声明位于同一个类型内、位于其中一个外围类型中或者位于相同文件的顶层的属性时。


## 接口 
Kotlin 的接口可以既包含抽象方法的声明也包含实现。与抽象类不同的是，接口无法保存状 态。它可以有属性但必须声明为抽象或提供访问器实现。

使用关键字 interface 来定义接口:

```Kotlin
interface MyInterface {
    fun bar()
    
	fun foo() {
		// 可选的方法体
	}
}
```

### 实现接口
一个类或者对象可以实现一个或多个接口:

```Kotlin
class Child : MyInterface {
    override fun bar() {
		// 方法体 
	}
}
```

### 接口中的属性
可以在接口中定义属性。在接口中声明的属性要么是抽象的，要么提供访问器的实现。在接口中声明的属性不能有幕后字段(backing field)，因此接口中声明的访问器不能引用它们:

```Kotlin
interface MyInterface { 
	val prop: Int // 抽象的
	
	val propertyWithImplementation: String
        get() = "foo"
        
    fun foo() {
        print(prop)
	}
}

class Child : MyInterface {
    override val prop: Int = 29
}
```
### 接口继承
一个接口可以从其他接口派生，意味着既能提供基类型成员的实现也能声明新的函数与属性。很自然地，实现这样接口的类只需定义所缺少的实现:

```Kotlin
interface Named {
    val name: String
}

interface Person : Named {
    val firstName: String
    val lastName: String
    override val name: String get() = "$firstName $lastName"
}

data class Employee( // 不必实现“name”
    override val firstName: String,
    override val lastName: String,
    val position: Position
) : Person
```
### 解决覆盖冲突
实现多个接口时，可能会遇到同一方法继承多个实现的问题。

## 函数式(SAM)接口 

只有一个抽象方法的接口称为函数式接口或单一抽象方法(SAM)接口。函数式接口可以有多个非抽象成员，但只能有一个抽象成员。
可以用 fun 修饰符在 Kotlin 中声明一个函数式接口。

```Kotlin
fun interface KRunnable {
   fun invoke()
}
```

### SAM 转换
使用 lambda 表达式可以替代手动创建实现函数式接口的类。通过 SAM 转换，KotlinKotlin 可以将其签名与接口的单个抽象方法的签名匹配的任何 lambda 表达式转换为实现该接口的类的实例。

### 函数式接口与类型别名比较
函数式接口和类型别名用途并不相同。类型别名只是现有类型的名称——它们不会创建新的类型，而函数式接口却会创建新类型。

类型别名只能有一个成员，而函数式接口可以有多个非抽象成员以及一个抽象成员。函数式接口还可以实现以及继承其他接口。

## 可见性修饰符
在 Kotlin 中有这四个可见性修饰符：private、 protected、 internal 和 public，默认可见性是 public。

1、包
函数、属性和类、对象和接口可以在顶层声明，即直接在包内。

```Kotlin
// 文件名：example.kt
package foo

fun baz() { …… }
class Bar { …… }
```

* 如果你不指定任何可见性修饰符，默认为 public，这意味着你的声明将随处可见；
* 如果你声明为 private，它只会在声明它的文件内可见；
* 如果你声明为 internal，它会在相同模块内随处可见；
* protected 不适用于顶层声明。

2、类和接口
对于类内部声明的成员：

* private 意味着只在这个类内部（包含其所有成员）可见；
* protected—— 和 private一样 + 在子类中可见。
* internal —— 能见到类声明的 本模块内 的任何客户端都可见其 internal 成员；
* public —— 能见到类声明的任何客户端都可见其 public 成员。

在 Kotlin 中，外部类不能访问内部类的 private 成员。

3、局部声明
局部变量、函数和类不能有可见性修饰符。

4、模块
可见性修饰符 internal 意味着该成员只在相同模块内可见。更具体地说， 一个模块是编译在一起的一套 Kotlin 文件：

* 一个 IntelliJ IDEA 模块；
* 一个 Maven 项目；
* 一个 Gradle 源集（例外是 test 源集可以访问 main 的 internal 声明）；
* 一次 <kotlinc> Ant 任务执行所编译的一套文件。

## 扩展
Kotlin 能够扩展一个类的新功能而无需继承该类或者使用像装饰者这样的设计模式。 这通过叫做 扩展 的特殊声明完成。

### 扩展函数
声明一个扩展函数，我们需要用一个接收者类型也就是被扩展的类型来作为他的前缀。 下面代码为 MutableList<Int> 添加一个swap 函数：

```Kotlin
fun MutableList<Int>.swap(index1: Int, index2: Int) {
    val tmp = this[index1] // “this”对应该列表
    this[index1] = this[index2]
    this[index2] = tmp
}
```
### 扩展是静态解析的
扩展不能真正的修改他们所扩展的类。通过定义一个扩展，你并没有在一个类中插入新成员， 仅仅是可以通过该类型的变量用点表达式去调用这个新函数。
扩展函数是静态分发的，即他们不是根据接收者类型的虚方法。 这意味着调用的扩展函数是由函数调用所在的表达式的类型来决定的， 而不是由表达式运行时求值结果决定的。

### 扩展的作用域
大多数时候我们在顶层定义扩展——直接在包里：

```Kotlin
package org.example.declarations
 
fun List<String>.getLongestString() { /*……*/}
```
要使用所定义包之外的一个扩展，我们需要在调用方导入它：

```Kotlin
package org.example.usage

import org.example.declarations.getLongestString

fun main() {
    val list = listOf("red", "green", "blue")
    list.getLongestString()
}
```

## 数据类
创建一些只保存数据的类， 在这些类中，一些标准功能以及一些工具函数往往是由数据机械推导而来的。在 Kotlin中，这叫做数据类并以`data` 标记:

```Kotlin
data class User(val name: String, val age: Int)
```
编译器自动从主构造函数中声明的所有属性导出以下成员:

* equals()/hashCode() 对；
* toString() 格式是 "User(name=John, age=42)"
* componentN() 函数 按声明顺序对应于所有属性。 
* copy() 函数

数据类必须满足以下要求：

* 主构造函数需要至少有一个参数；
* 主构造函数的所有参数需要标记为 `val` 或 `var`；
* 数据类不能是抽象、开放、密封或者内部的；
* 数据类只能实现接口


### 在类体中声明的属性
 
 如需在生成的实现中排除一个属性，请将其声明在类体中：
 
```Kotlin
data class Person(val name: String) {
    var age: Int = 0
}
```
在 `toString()`、 `equals()`、 `hashCode()` 以及 `copy() `的实现中只会用到 `name` 属性。

## 密封类 
密封类用来表示受限的类继承结构：当一个值为有限几种的类型、而不能有任何其他类型时。要声明一个密封类，需要在类名前面添加 `sealed`修饰符。虽然密封类也可以有子类，但是所有子类都必须在与密封类自身相同的文件中声明。

```Kotlin
sealed class Expr
data class Const(val number: Double) : Expr()
data class Sum(val e1: Expr, val e2: Expr) : Expr()
object NotANumber : Expr()
```

## 泛型:in、out、where 
泛型（Generics）其实就是把类型参数化，真正的名字叫做类型参数。

```Kotlin
class Box<T>(t: T) {
    var value = t
}
```
型变注解：

* `out`：协变，生产者。协变类型参数只能用作输出
* `in`：逆变，消费者。逆变类型参数只能用作输入

```
//Kotlin使用处协变
fun sumOfList(list: List<out Number>)

//Java上界通配符
void sumOfList(List<? extends Number> list)

//Kotlin使用处逆变
fun addNumbers(list: List<in Int>)

//Java下界通配符
void addNumbers(List<? super Integer> list)
```

Kotlin 为泛型声明用法执行的类型安全检测在编译期进行。 运行时泛型类型的实例不保留关 于其类型实参的任何信息。 其类型信息称为被擦除。

## 嵌套类与内部类
类可以嵌套在其他类中，所有类与接口的组合都是可能的:可以将接口嵌套在类中、将类嵌套在接口中、将接口嵌套在接口中。

```Kotlin
interface OuterInterface {
    class InnerClass
    interface InnerInterface
}

class OuterClass {
    class InnerClass
    interface InnerInterface
}
```
标记为 `inner` 的嵌套类能够访问其外部类的成员。内部类会带有一个对外部类的对象的引用:

```Kotlin
class Outer {
    private val bar: Int = 1
    
    inner class Inner {
        fun foo() = bar
    }
}

val demo = Outer().Inner().foo() // == 1
```

## 枚举类
枚举类的最基本的应用场景是实现类型安全的枚举。

```Kotlin
enum class Direction {
    NORTH, SOUTH, WEST, EAST
}
```
每个枚举常量都是一个对象。枚举常量以逗号分隔。因为每一个枚举都是枚举类的实例，所以可以这样初始化:

```Kotlin
enum class Color(val rgb: Int) {
    RED(0xFF0000),
    GREEN(0x00FF00),
    BLUE(0x0000FF)
}
```
枚举常量可以声明其带有相应方法以及覆盖了基类方法的自身匿名类。如果枚举类定义任何成员，那么使用分号将成员定义与常量定义分隔开。

```Kotlin
enum class ProtocolState {
    WAITING {
        override fun signal() = TALKING
    },
    TALKING {
        override fun signal() = WAITING
};
    abstract fun signal(): ProtocolState
}
```

枚举类可有以下用法：
1. 在枚举类中实现接口，为所有条目提供统一的接口成员实现。
2. 使用枚举常量，列出定义的枚举常量以及通过名称获取枚举常量。每个枚举常量都具有在枚举类声明中获取其名称与(自0起的)位置的属性。

## 内联类
内联类必须含有唯一的一个属性在主构造函数中初始化。在运行时， 将使用这个唯一属性来表示内联类的实例。

内联类允许去继承接口，内联类不能继承其他的类而且始终是 `final` 的。

```
interface Printable {
    fun prettyPrint(): String
}
@JvmInline
value class Name(val s: String) : Printable {
    override fun prettyPrint(): String = "Let's $s!"
}

fun main() {
	val name = Name("Kotlin")
	println(name.prettyPrint()) // 仍然会作为一个静态方法被调用
}
```

##   委托
```
interface Base {
    val message: String
    fun print()
}
class BaseImpl(val x: Int) : Base {
    override val message = "BaseImpl: x = $x"
    override fun print() { println(message) }
}

class Derived(b: Base) : Base by b {
	// 在 b 的 `print` 实现中不会访问到这个属性 
	override val message = "Message of Derived"	//覆盖委托
}

fun main() {
    val b = BaseImpl(10)
    val derived = Derived(b)
    derived.print()
    println(derived.message)
}
```

##   属性委托
Kotlin 支持委托属性，语法是: `val/var <属性名>: <类型> by <表达式> `。

```Kotlin
class Example {
    var p: String by Delegate()
}

class Delegate {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): String {
        return "$thisRef, thank you for delegating '${property.name}' to me!"
    }
    
    operator fun setValue(thisRef: Any?, property: KProperty<*>, value: String) { 
	   println("$value has been assigned to '${property.name}' in $thisRef.")
    } 
}
```
当从委托到一个 Delegate 实例的 p 读取时，将调用 Delegate 中的 getValue() 函数。 它的第一个参数是读出 p 的对象、第二个参数保存了对 p 自身的描述。

标准委托：

* 延迟属性 Lazy properties：`lazy()`接受一个 lambda 并返回一个 Lazy <T> 实例的函数，返回的实例可以作为实现延 迟属性的委托。
* 可观察属性 Observable properties：`Delegates.observable()` 接受两个参数:初始值与修改时处理程序(handler)。

##   类型别名
类型别名为现有类型提供替代名称。类型别名不会引入新类型，有助于缩短较长的泛型类型。

```Kotlin
typealias NodeSet = Set<Network.Node>

typealias FileTable<K> = MutableMap<K, MutableList<File>>
```

# 函数
Kotlin 函数使用 `fun` 关键字声明:

```Kotlin
fun double(x: Int): Int {
    return 2 * x
}
```
函数参数使用 `Pascal` 表示法定义——`name: type`。参数用逗号隔开，每个参数必须有显式类 型:

```Kotlin
fun powerOf(number: Int, exponent: Int): Int { /*......*/ }

//函数参数可以有默认值，当省略相应的参数时使用默认值。
fun read(
    b: ByteArray,
    off: Int = 0,
    len: Int = b.size,
) { /*......*/ }
```

### 1、单表达式函数

当函数返回单个表达式时，可以省略花括号并且在 = 符号之后指定代码体即可:

```Kotlin
fun double(x: Int): Int = x * 2

//当返回值类型可由编译器推断时，显式声明返回类型是可选的:
fun double(x: Int) = x * 2
```

### 2、可变数量的参数(varargs)
函数的参数(通常是最后一个)可以用 vararg 修饰符标记:

```Kotlin
fun <T> asList(vararg ts: T): List<T> {
    val result = ArrayList<T>()
    for (t in ts) // ts is an Array
        result.add(t)
    return result
}
```
只有一个参数可以标注为 `vararg`。当调用 `vararg` -函数时，可以逐个传参

### 3、函数作用域
* 局部函数：一个函数在另一个函数内部，局部函数可以访问外部函数(闭包)的局部变量。

```Kotlin
fun dfs(graph: Graph) {
    val visited = HashSet<Vertex>()
    fun dfs(current: Vertex) {
        if (!visited.add(current)) return
        for (v in current.neighbors)
dfs(v)
}
    dfs(graph.vertices[0])
}
```
* 成员函数：在类或对象内部定义的函数。成员函数以点表示法调用.

```Kotlin
class Sample {
    fun foo() { print("Foo") }
}
```
* 泛型函数：有泛型参数，通过在函数名前使用尖括号指定

```Kotlin
fun <T> singletonList(item: T): List<T> { /*......*/ }
```

## 高阶函数与 lambda 表达式
高阶函数是将函数用作参数或返回值的函数。

### 1、函数类型实例化方法：

* 使用函数字面值的代码块
	* lambda表达式：{ a, b -> a + b } ,
	* 匿名函数：fun(s: String): Int { return s.toIntOrNull() ?: 0 }
* 使用已有声明的可调用引用:
	* 顶层、局部、成员、扩展函数: ::isOdd 、 String::toInt 
	* 顶层、成员、扩展属性: List<Int>::size ，
	* 构造函数: ::Regex
* 使用实现函数类型接口的自定义类的实例:

### 2、Lambda 表达式与匿名函数

lambda 表达式与匿名函数是函数字面值，函数字面值即没有声明而是立即做为表达式传递的 函数。

```Kotlin
max(strings, { a, b -> a.length < b.length })

//函数 max 是一个高阶函数，因为它接受一个函数作为第二个参数。 其第二个参数是一个表达式，它本身是一个函数，称为函数字面值，它等价于
fun compare(a: String, b: String): Boolean = a.length < b.length
```
Lambda 表达式的完整语法形式如下:

```Kotlin
val sum: (Int, Int) -> Int = { x: Int, y: Int -> x + y }
```
* lambda 表达式总是括在花括号中。完整语法形式的参数声明放在花括号内，并有可选的类型标注。
* 函数体跟在一个` ->` 之后。
* 如果推断出的该lambda的返回类型不是 Unit ，那么该lambda主体中的最后一个(或可 能是单个)表达式会视为返回值。

按照 Kotlin 惯例，如果函数的最后一个参数是函数，那么作为相应参数传入的 lambda 表达式可以放在圆括号之外，称为拖尾 lambda 表达式。

一个 lambda 表达式只有一个参数很常见。该参数会隐式声明为 `it` 。

```Kotlin
ints.filter { it > 0 } // 这个字面值是“(it: Int) -> Boolean”类型的
```

可以使用限定的返回语法从 lambda 显式返回一个值。 否则，将隐式返回最后一个表达式的值。

```Kotlin
ints.filter {
    val shouldFilter = it > 0
    shouldFilter
}

ints.filter {
    val shouldFilter = it > 0
    return@filter shouldFilter
}

//如果 lambda 表达式的参数未使用，那么可以用下划线取代其名称:
map.forEach { (_, value) -> println("$value!") }
```

## 内联函数
高阶函数在运行时带来效率损失:每一个函数都是一个对象，并且会捕获一个闭包。闭包那些在函数体内会访问到的变量的作用域。 内存分配(对于函数对象和类)和虚拟 调用会引入运行时间开销。

在许多情况下通过内联化 lambda 表达式可以消除这类的开销。
`inline` 修饰符影响函数本身和传给它的 lambda 表达式:所有这些都将内联到调用处。

```Kotlin
inline fun <T> lock(lock: Lock, body: () -> T): T { ...... }
```

## 操作符重载
在 Kotlin 中可以为类型提供预定义的一组操作符的自定义实现。这些操作符具有预定义的符 号表示(如 + 或 * )与优先级。为了实现这样的操作符，需要为相应的类型提供一个指定 名称的成员函数或扩展函数。

要重载操作符, 要对相应的函数使用 operator 修饰符。如果在子类中重载操作符, 可以省略 operator:
```Kotlin
interface IndexedContainer {
    operator fun get(index: Int)
}
```

| 表达式 | 对应操作 | 
| :------ |:--- | 
| +a | a.unaryPlus() |
| -a | a.unaryMinus() | 
| !a | a.not()| 
|a++  | a.inc()| 
| a-- | a.dec() | 
|a + b |a.plus(b)|
|a - b|	a.minus(b)|
|a * b|	a.times(b)|
|a / b|	a.div(b)|
|a % b|	a.rem(b)|
|a..b|	a.rangeTo(b)|
|a in b|	b.contains(a)|
|a !in b|	!b.contains(a)|
|a += b|	a.plusAssign(b)|
|a -= b|	a.minusAssign(b)|
|a *= b|	a.timesAssign(b)|
|a /= b|	a.divAssign(b)|
|a %= b|	a.remAssign(b)|
|a == b |a?.equals(b) ?: (b === null)|
|a != b|	!(a?.equals(b) ?: (b === null))|

# 类型安全的构建器
通过使用命名得当的函数作为构建器，结合带有接收者的函数字面值， 可以在 Kotlin 中创建 类型安全、静态类型的构建器。
类型安全的构建器可以创建基于 Kotlin 的适用于采用半声明方式构建复杂层次数据结构领域 专用语言(DSL)。

* 使用 Kotlin 代码生成标记语言，例如 HTML 或 XML 
* 为 Web 服务器配置路由:Ktor

```Kotlin
import com.example.html.* // 参见下文声明
fun result() =
    html {
        head {
            title {+"XML encoding with Kotlin"}
}
body {
            h1 {+"XML encoding with Kotlin"}
            p  {+"this format can be used as an alternative markup to XML"}
// 一个具有属性和文本内容的元素
a(href = "https://kotlinlang.org") {+"Kotlin"}
// 混合的内容 p{
                +"This is some"
                b {+"mixed"}
                +"text. For more see the"
                a(href = "https://kotlinlang.org") {+"Kotlin"}
                +"project"
            }
            p {+"some text"}
// 以下代码生成的内容 p{
} }
}
```

# 空安全
1、可空类型与非空类型
Kotlin 的类型系统旨在消除来自代码空引用的危险。许多编程语言(包括 Java)中最常见的陷阱之一，就是访问空引用的成员会导致空引用异常。

Kotlin 中 NPE 的可能的原因只可能是:

* 显式调用 throw NullPointerException() 。 
* 使用了下文描述的 !! 操作符。 
* 数据在初始化时不一致，例如当:
	* 传递一个在构造函数中出现的未初始化的 this 并用于其他地方(“泄漏 this ”)。
	*   超类的构造函数调用一个开放成员，该成员在派生中类的实现使用了未初始化的状态。 
* Java 互操作:
	* 企图访问平台类型的 null 引用的成员;
	* 用于 Java 互操作的泛型类型的可空性问题，例如一段 Java 代码可能会向 Kotlin 的
	* MutableList<String> 中加入 null ，就需要一个 MutableList<String?> 才能处 理。
	* 由外部 Java 代码引发的其他问题。

在 Kotlin 中，类型系统区分一个引用可以容纳 null (可空引用)还是不能容纳(非空引 用)。如果要允许为空，可以声明一个变量为可空字符串(写作 String? ):

```Kotlin
fun main() {
	//sampleStart
	var b: String? = "abc" // 可以设置为空 
	b = null // ok
	print(b)
	//sampleEnd
}
```
在条件中检测 null

```Kotlin
//1、显式检测
val l = if (b != null) b.length else -1
```
访问可空变量的属性的第二种选择是使用安全调用操作符` ?. `:

```Kotlin
fun main() {
//sampleStart
val a = "Kotlin"
val b: String? = null println(b?.length) println(a?.length) // 无需安全调用
//sampleEnd
}

//安全调用在链式调用中很有用。如果任意一个属性(环节)为 null ，这个链式调用就会返回 null 。如：
bob?.department?.head?.name

//如果要只对非空值执行某个操作，安全调用操作符可以与 let 一起使用:
	val listWithNulls: List<String?> = listOf("Kotlin", null)
    for (item in listWithNulls) {
		item?.let { println(it) } // 输出 Kotlin 并忽略 null
}

```


```Kotlin
//Elvis 操作符：如果 ?: 左侧表达式不是 null ，Elvis 操作符就返回其左侧表达式，否则返回右侧表达式。
val l = b?.length ?: -1

// !! 操作符：非空断言运算符( !! )将任何值转换为非空类型，若 该值为 null 则抛出异常。
val l = b!!.length
```
如果对象不是目标类型，那么常规类型转换可能会导致 ClassCastException 。另一个选择是
使用安全的类型转换，如果尝试转换不成功则返回 null :

# 相等性
Kotlin 中有两类相等性:
* 结构相等( == ——用 equals() 检测); 
* 引用相等( === ——两个引用指向同一对象)

## 1、结构相等
结构相等由 == 以及其否定形式 != 操作判断。  按照约定，像 a == b 这样的表达式会翻译 成:

```Kotlin
a?.equals(b) ?: (b === null)
```

## 2、引用相等
引用相等由 === (以及其否定形式 !== )操作判断。 a === b 当且仅当 a 与 b 指向同 一个对象时求值为 true。对于运行时以原生类型表示的值 (例如 Int )， === 相等检测等 价于 == 检测。

# this表达式
表示当前的 接收者 可使用 this 表达式:

* 在类的成员中， this 指的是该类的当前对象。 
* 在扩展函数或者带有接收者的函数字面值中， this 表示在点左侧传递的 接收者 参数。

如果 this 没有限定符，它指的是最内层的包含它的作用域。要引用其他作用域中的 this ，请使用标签限定符:

## 限定的 this
要访问来自外部作用域的 this 使用 this@label ， 其中 @label 是一个代指 this 来源的标签:

```Kotlin
class A { // 隐式标签 @A
	inner class B { // 隐式标签 @B
		fun Int.foo() { // 隐式标签 @foo 
			val a = this@A // A 的 this 
			val b = this@B // B 的 this
			
			val c = this // foo() 的接收者，一个 Int
			val c1 = this@foo // foo() 的接收者，一个 Int
			
			val funLit = lambda@ fun String.() {
				val d = this // funLit 的接收者，一个 String
			}
			
			val funLit2 = { s: String ->
				// foo() 的接收者，因为它包含的 lambda 表达式 
				// 没有任何接收者
				val d1 = this
			} 
		}
	} 
}

```
## Implicit this
当对 `this` 调用成员函数时，可以省略 `this.` 部分。 但是如果有一个同名的非成员函数时，请谨慎使用，因为在某些情况下会调用同名的非成员:

```Kotlin
fun main() {
  //sampleStart
  fun printLine() { println("Top-level function") }
  
  class A {
    fun printLine() { println("Member function") }
    
    fun invokePrintLine(omitThis: Boolean = false)  {
      if (omitThis) printLine()
      else this.printLine()
	 } 
  }
	
  A().invokePrintLine() // Member function
  A().invokePrintLine(omitThis = true) // Top-level function
  
//sampleEnd()
}
```


# 异步程序设计技术
应用程序被阻塞的解决途径：
* 线程
* 回调
* Future、 Promise 及其他 
* 反应式扩展
* 协程

## 线程
线程是最常见的避免应用程序阻塞的方法。

使用线程技术的缺点：

* 线程并非廉价的。线程需要昂贵的上下文切换。
* 线程不是无限的。可被启动的线程数受底层操作系统的限制。在服务器端应用程序中， 这可能会导致严重的瓶颈。
* 线程并不总是可用。在一些平台中，比如 JavaScript 甚至不支持线程。
* 线程不容易使用。线程的 Debug，避免竞争条件是我们在多线程编程中遇到的常见问题。

## 回调
使用回调，其想法是将一个函数作为参数传递给另一个函数，并在处理完成后调用此函数。

```Kotlin
fun postItem(item: Item) {
    preparePostAsync { token ->
        submitPostAsync(token, item) { post ->
            processPost(post)
	}
   }
}

fun preparePostAsync(callback: (Token) -> Unit) {
	 // 发起请求并立即返回
	// 设置稍后调用的回调
}
```
缺点：

* 回调嵌套的难度。通常被用作回调的函数，经常最终需要自己的回调。这导致了一系列回调嵌套并导致出现难以理解的代码。
* 错误处理很复杂。嵌套模型使错误处理和传播变得更加复杂。

## Future、 Promise 及其他 
futures 或 promises 背后的想法(这也可能会根据语言/平台而有不同的术语)，是当我们发起 调用的时候，我们承诺在某些时候它将返回一个名为 Promise 的可被操作的对象。

```Kotlin
fun postItem(item: Item) {
    preparePostAsync()
		.thenCompose { token ->
 	  		submitPostAsync(token, item)
	}
		.thenAccept { post ->
   			 processPost(post)
	}

}

fun preparePostAsync(): Promise<Token> {
	// 发起请求并当稍后的请求完成时返回一个 promise 
	return promise
}
```
这种方法需要对我们的编程方式进行一系列更改，尤其是:

* 不同的编程模型。与回调类似，编程模型从自上而下的命令式方法转变为具有链式调用 的组合模型。传统的编程结构例如循环，异常处理，等等。通常在此模型中不再有效。
*  不同的 API。通常这需要学习完整的新 API 诸如 thenCompose 或 thenAccept ，这也可 能因平台而异。 
*  具体的返回值类型。返回类型远离我们需要的实际数据，而是返回一个必须被内省的新 类型“Promise”。
*  异常处理会很复杂。错误的传播和链接并不总是直截了当的。

## 反应式扩展
反应式扩展(Rx) 背后的想法是走向所谓的“可观察流”，我们现在将数据视为流(无限量的数据)，并且可 以观察到这些流。

```
“一切都是流，并且它是可被观察的”
```
这意味着处理问题的方式不同，并且在编写同步代码时从我们使用的方式发生了相当大的转 变。

## 协程
Kotlin 编写异步代码的方式是使用协程，这是一种计算可被挂起的想法。即一种函数可以在某 个时刻暂停执行并稍后恢复的想法。

协程的一个好处是，当涉及到开发人员时，编写非阻塞代码与编写阻塞代码基本相同。编程模型本身并没有真正改变。


```Kotlin
fun postItem(item: Item) {
    launch {
        val token = preparePost()
        val post = submitPost(token, item)
        processPost(post)
	}
}

suspend fun preparePost(): Token {
	// 发起请求并挂起该协程
	return suspendCoroutine { /* ... */ }
}
```

`preparePost` 就是所谓的可挂起的函数 ，因此它含有 `suspend` 前缀。

# 注解
注解是将元数据附加到代码的方法。要声明注解，请将 annotation 修饰符放在类的前面:

```Kotlin
annotation class Fancy
```
注解的附加属性可以通过用元注解标注注解类来指定:

* `@Target` 指定可以用该注解标注的元素的可能的类型(类、函数、属性与表达式);
* `@Retention` 指定该注解是否存储在编译后的 class 文件中，以及它在运行时能否通过反射可见 (默认都是 true);
* `@Repeatable` 允许在单个元素上多次使用相同的该注解;
* `@MustBeDocumented` 指定该注解是公有 API 的一部分，并且应该包含在生成的 API 文档 中显示的类或方法的签名中。

```Kotlin
@Target(AnnotationTarget.CLASS, AnnotationTarget.FUNCTION, 
	AnnotationTarget.TYPE_PARAMETER, AnnotationTarget.VALUE_PARAMETER, 
	AnnotationTarget.EXPRESSION)
@Retention(AnnotationRetention.SOURCE)
@MustBeDocumented
annotation class Fancy

//用法
@Fancy class Foo {
    @Fancy fun baz(@Fancy foo: Int): Int {
        return (@Fancy 1)
    }
}
```

## Java 注解
Java 注解与 Kotlin 100% 兼容:

```Kotlin
import org.junit.Test
import org.junit.Assert.*
import org.junit.Rule
import org.junit.rules.*

class Tests {
	// 将 @Rule 注解应用于属性 getter
	@get:Rule val tempFolder = TemporaryFolder()
	
	@Test fun simple() {
        val f = tempFolder.newFile()
        assertEquals(42, getTheAnswer())
	}
}
```
Java 编写的注解没有定义参数顺序，所以不能使用常规函数调用语法来传递参数。

# 反射
反射是这样的一组语言和库功能，让你可以在运行时自省你的程序的结构。 Kotlin 中的函数 和属性是一等公民，而对其自省(即在运行时获悉一个属性或函数的名称或类型)能力是使 用函数式或反应式风格时所必需的。

## JVM依赖
在 JVM 平台上, Kotlin 编译器包含了使用反射功能所需要的运行时组件, 它是一个单独的 JAR 文件 kotlin-reflect.jar。

在 Gradle 或 Maven 项目中, 如果需要使用反射, 需要添加 kotlin-reflect 的依赖项:

```
//Kotlin
dependencies { 
	implementation("org.jetbrains.kotlin:kotlin-reflect:1.8.10")
}

//Groovy
dependencies {
    implementation "org.jetbrains.kotlin:kotlin-reflect:1.8.20"
}

//Maven
<dependencies>
  <dependency>
      <groupId>org.jetbrains.kotlin</groupId>
      <artifactId>kotlin-reflect</artifactId>
  </dependency>
</dependencies>
```

## 类引用(Class Reference)
最基本的反射功能就是获取一个 Kotlin 类的运行时引用. 要得到一个静态的已知的 Kotlin 类的引用, 可以使用 类字面值(class literal) 语法:


```Kotlin
val c = MyClass::class

//::class 语法同样可以用于取得某个对象实例的类的引用:
val widget: Widget = ...
assert(widget is GoodWidget) { "Bad widget: ${widget::class.qualifiedName}" }
```

## 可调用引用
函数、属性以及构造函数的引用可以用于调用或者用作函数类型的实例。

所有可调用的引用的共同的超类是 KCallable<out R>, 这里的 R 是返回值的类型. 对于属性来说就是属性类型, 对构造器来说就是它创建出来的类的类型.

### 函数引用(Function Reference)
函数引用的类型属于 KFunction<out R> 的子类之一, 具体是哪个由函数的参数个数决定。

### 属性引用(Property Reference)
在 Kotlin 中, 可以将属性作为一等对象来访问, 方法是使用 :: 操作符:

### 与 Java 反射功能的互操作性
在 Java 平台上, Kotlin 的标准库包含了针对反射类的扩展函数, 这些反射类提供了与 Java 反射对象的相互转换功能。




