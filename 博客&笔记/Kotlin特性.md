# Kotlin特性
## 空安全
* `lateinit` 延迟初始化
* `?.` 「safe call」
* `!!.` 「non-null asserted call」
* `var` variable的缩写，声明变量
* `val` value的缩写，声明只读变量

## 语法
Java 和 Kotlin 中关于类的声明主要关注以下几个方面：

1. 类的可见性和开放性
2. 构造方法
3. 继承
4. override 函数

* get()和set()写法：

```
class Test {
    var name = "baize"
        get() {
            return "$field nb"
        }
        set(value) {
            field = "cate$value"
        }
}
```
* `constructor` 构造函数关键字

```
class MainActivity constructor() : AppCompatActivity() {
    //这种写法也可以
    constructor() {
    }               👆
}
```

* `override` 的不同
	* Kotlin 里的 override 变成了关键字。
	* Kotlin 省略了 `protected` 关键字，Kotlin 里的 `override` 函数的可见性是继承自父类的。
	* Kotlin里的类默认无法继承，如果需要继承需要在类前加`open`关键字。注意 `open` 没有父类到子类的遗传性。
	* `override` 是有遗传性的，如果要关闭`overide`的遗传性：
	
	```
	open class MainActivity : AppCompatActivity() {
	    // 👇加了 final 关键字，作用和 Java 里面一样，关闭了 override 的遗传性
	    final override fun onCreate(savedInstanceState: Bundle?) {
	        ...
	    }
	}
	```
	
## Kotlin中不java的写法

* `constructor` 声明Kotlin构造器
* `init` 初始化块	要使用`init`修饰(初始化代码块会先于构造器执行)
* 被`val`修饰的常量，也可以通过重写`get()`方法来动态返回它的值，这点与java被final修饰的常量不同
* `object` 关键字：

	`object` 是一个关键字，同`class`一样可以用来修饰一个类
	
	```
	object Sample {
	    val name = "A name"
	}
	```
	
	它的含义是：创建一个类，并且创建一个这个类的对象。`object`的意思就是：对象
	
	（在Java中复杂写出的单例，用`object`关键字就能实现）
	> 这种通过 object 实现的单例是一个饿汉式的单例，并且实现了线程安全。
	
	用`object`修饰的对象中变量和函数都是静态的，可以直接通过类名进行访问。
	
* `companion object` 关键字
	
	声明类的伴生对象，放在`compation object`中的函数对象
	
	> Java 中的 Object 在 Kotlin 中变成了 Any，和 Object 作用一样：作为所有类的基类。

	在Kotlin中，静态变量和静态方法这两个概念被去除了。
	
* 「top-level declaration 顶层声明」
	
	```
	package com.hencoder.plus
	// 👇 属于 package，不在 class/object 内
	fun topLevelFuncion() {
	}
	```
	顶级函数属于包,不属于任何 `class`
	
* `const` 声明常量关键字
	* Kotlin 的常量必须声明在对象（包括伴生对象）或者「top-level 顶层」中，因为常量是静态的。
	* Kotlin 中只有基本类型和 String 类型可以声明成常量。

### 数组

	声明一个String数组：
	
	```
	Java:
	String[] strs = {"a","b","c"};
	
	Kotlin:
	var strs :Array<String>  = arrayOf("a","b","c")
	
	访问元素：
	strs[0]
	```
	
	Kotlin数组由于在语言层使用泛型实现，会失去协变(covariance)的特性（子类数组对象不能赋给父类数组变量）
	
	```
	val anys: Array<Any> = strs // compile-error: Type mismatch
	
	Object[] objs = strs; // success java中是可以的
	```

### 集合

	三种集合类型：
	* List 以固定顺序存储一组元素，元素可以重复。 Kotlin 中的 List 多了一个特性：**支持 covariant（协变）**	
	* Set 存储一组互不相等的元素，通常没有固定顺序。
	* Map 存储 键-值 对的数据集合，键互不相等，但不同的键可以对应相同的值。

	Kotlin声明一个集合：

	```
	val strList = listOf("a", "b", "c")
	```
	
	集合和数组的选择，在一些性能需求比较苛刻的场景，并且元素类型是基本类型时，用数组好一点。不过这里要注意一点，Kotlin 中要用专门的基本类型数组类 (IntArray FloatArray LongArray) 才可以免于装箱。也就是说元素不是基本类型时，相比 `Array`，用 `List` 更方便些。
	
* 可变集合/不可变集合

	Kotlin 中集合分为两种类型：只读的和可变的。这里的只读有两层意思：
	
	* 集合的 size 不可变
	* 集合中的元素值不可变
	
	以下是三种集合类型创建不可变和可变实例的例子：
	
	* `listOf()` 创建不可变的 List，`mutableListOf()` 创建可变的 List。
	* `setOf()` 创建不可变的 Set，`mutableSetOf()` 创建可变的 Set。
	* `mapOf()` 创建不可变的 Map，`mutableMapOf()` 创建可变的 Map。

	不可变集合可以通过`toMutable*()`方法返回的是一个新建的集合，原有的集合还是不可变的，所以只能对函数返回的集合修改。
	
* 可见行修饰符
	* `public`：公开，可见性最大，哪里都可以引用。
	* `private`：私有，可见性最小，根据声明位置不同可分为类中可见和文件中可见。且与Java不同，外部类不可以访问内部类的 `private` 变量
	* `protected`：保护，相当于 private + 子类可见。
	* `internal`：内部，仅对 module 内可见。在写一个 library module 时非常有用

## Kotlin更方便的

### `constructor` 构造器

Kotlin构造器分为主构造器「primary constructor」和次构造器。次构造器则是写在类中的常用写法，主构造器写法如下：

```
class User constructor(name: String) {
    // 👇 这里与构造器中的 name 是同一个
    var name: String = name
}
```

- 在 Kotlin 中一个类最多只能有 1 个主构造器（也可以没有），而次构造器是没有个数限制。
- 主构造器中的参数除了可以在类的属性中使用，还可以在 init 代码块中使用。
- 主构造器没有函数体，init 代码块就充当了主构造器代码体的功能。
- 如果类中有主构造器，那么其他的次构造器都需要通过 this 关键字调用主构造器。

```
class User constructor(var name: String) {
                                   // 👇  👇 直接调用主构造器
    constructor(name: String, id: Int) : this(name) {
    }
                                                // 👇 通过上一个次构造器，间接调用主构造器
    constructor(name: String, id: Int, age: Int) : this(name, id) {
    }
}
```
- 主构造器的`constructor`关键字通常可以省略，但有些场景，`constructor` 是不可以省略的，例如在主构造器上使用「可见性修饰符」或者「注解」

### 函数简化
* 使用 = 连接返回值

```
fun area(width: Int, height: Int): Int {
    return width * height
}
//简化后：
fun area(width: Int, height: Int): Int = width * height
```

* 参数默认值

```
fun sayHi(name: String = "world") = println("Hi " + name)
调用👇：
sayHi() //使用了默认值 "world"
sayHi(“baize”)
```

* 命名参数
在调用函数时，显式地指定参数的名称，这就是「命名参数」。

```
fun sayHi(name: String = "world", age: Int) {
    ...
}
      👇   
sayHi(age = 21) //命名参数
```

与命名参数相对的一个概念被称为「位置参数」，也就是按位置顺序进行参数填写。如果混用位置参数与命名参数，那么所有的位置参数都应该放在第一个命名参数之前：

```
fun sayHi(name: String = "world", age: Int) {
    ...
}
​
sayHi(name = "wo", 21) // 👈 IDE 会报错，Mixing named and positioned arguments is not allowed
sayHi("wo", age = 21) // 👈 这是正确的写法
```

### 本地函数（嵌套函数）

函数中再嵌套一个函数，叫做嵌套函数。嵌套函数中可以访问在它外部的所有变量或常量.

使用一个嵌套函数简化登陆验证逻辑：

```
fun login(user: String, password: String, illegalStr: String) {
    // 验证 user 是否为空
    if (user.isEmpty()) {
        throw IllegalArgumentException(illegalStr)
    }
    // 验证 password 是否为空
    if (password.isEmpty()) {
        throw IllegalArgumentException(illegalStr)
    }
}
//使用嵌套函数后：
fun login(user: String, password: String, illegalStr: String) {
    fun validate(value: String) {
        if (value.isEmpty()) {
            throw IllegalArgumentException(illegalStr)
        }
    }
    validate(user)
    validate(password)
}
```

### 模版字符串

```
val name = "world"
println("Hi $name") //$ 后可加单个参数
println("Hi ${name.length}") //$ 后加一个表达式需要用{}
```

字符串模板还支持转义字符.

#### raw string (原生字符串)

原声字符串可以省略转义字符，用法是通过`""""`将字符串扩起来：

特点：

- `\n` 并不会被转义
- 最后输出的内容与写的内容完全一致，包括实际的换行
- `$` 符号引用变量仍然生效


```
val name = "world"
val myName = "kotlin"
原声字符串：👇
val text = """
      Hi $name!
    My name is $myName.\n
"""
println(text)
//输出：
    Hi world!
My name is kotlin.\n

```

#### trimMargin() 函数

上面输出的对齐方式不太优雅，原生字符串还可以通过 trimMargin() 函数去除每行前面的空格：

```
val text = """
     👇 
      |Hi world!
    |My name is kotlin.
""".trimMargin()
println(text)
```

输出结果：
	
```
Hi world!
My name is kotlin.
```


- `|` 符号为默认的边界前缀，前面只能有空格，否则不会生效
- 输出时 `|` 符号以及它前面的空格都会被删除
- 边界前缀还可以使用其他字符，比如 `trimMargin("/")`，只不过上方的代码使用的是参数默认值的调用方式


### 数组与集合的操作符

首先声明一个数组与一个集合，然后再介绍操作符：

```
val intArray = intArrayOf(1, 2, 3)
val strList = listOf("a","b","c")
```

- `forEach`:遍历每一个元素

	```
	// 👇 lambda 表达式，i 表示数组的每个元素
	intArray.forEach { i ->
	    print(i + " ")
	}
	// 输出：1 2 3 
	```

- `filter`：对每个元素进行过滤操作，如果 lambda 表达式中的条件成立则留下该元素，否则剔除，最终生成新的**集合**

	```
	//👇 注意，这里变成了 List
	val newList: List = intArray.filter { i ->
	    i != 1 // 👈 过滤掉数组中等于 1 的元素
	}
	//[1, 2, 3] -> [2,3]
	```

- `map`:遍历每个元素并执行给定表达式，最终形成新的集合

	```
	val newList: List = intArray.map { i ->
	    i + 1 // 👈 每个元素加 1
	}
	//[1, 2, 3] -> {2, 3, 4}
	```

- flatMap：遍历每个元素，并为每个元素创建新的集合，最后合并到一个集合中

	```
	intArray.flatMap { i ->
	    listOf("${i + 1}", "a") // 👈 生成新集合
	}
	//[1, 2, 3] -> {"2", "a" , "3", "a", "4", "a"}
	```

### `Range` ：区间

实现类有：`IntRange`，`CharRange`,`LongRange`

```
val range: IntRange = 0..1000 //表示从 0 到 1000 的范围，包括 1000
```

如果定义半区间，可以使用`until`关键字来去除边界：

```
val range: IntRange = 0 until 1000 //表示从 0 到 1000，但不包括 1000
```

可以使用 `in` 关键字与 `for` 循环结合使用，挨个遍历 `range` 中的值:

```
val range = 0..1000
for (i in range) {
    print("$i, ") // 0,1,2,3,4,...1000
}
```

- 还可以用中缀表达式`step`设置步长：

```
for (i in range step 2) {
    print("$i, ") //步长为 2，输出：0, 2, 4, 6, 8, 10,....1000,
}
```

中缀表达是`downTo `表示递减，递减没有半开区间用法：

```
for (i in 4 downTo 1) {
    print("$i, ") //输出：4, 3, 2, 1, 
}
```

### Sequence：惰性集合操作

```
val sequence = sequenceOf(1, 2, 3, 4)
val result: List = sequence
    .map { i ->
        println("Map $i")
        i * 2 
    }
    .filter { i ->
        println("Filter $i")
        i % 3  == 0 
    }
👇
println(result.first()) // 👈 只取集合的第一个元素
```
- 惰性的概念就是在printIn()前，代码运行时不会执行，只是定义了一个流程，只有`result`被使用到时才会执行。
- `println(result.first())`执行流程：
	- 取出元素 1 -> map 为 2 -> filter 判断 2 是否能被 3 整除
	- 取出元素 2 -> map 为 4 -> filter 判断 4 是否能被 3 整除
	- 取出元素 3 -> map 为 6 -> filter 判断 4 是否能被 3 整除 满足条件，返回6并跳出循环

而 `List` 是没有惰性的特性的：

```
val list = listOf(1, 2, 3, 4)
val result: List = list
    .map { i ->
        println("Map $i")
        i * 2 
    }
    .filter { i ->
        println("Filter $i")
        i % 3  == 0 
    }
👇
println(result.first()) // 👈 只取集合的第一个元素
```
- 声明之后立即执行
- 数据处理流程如下：
	- {1, 2, 3, 4} -> {2, 4, 6, 8}
	- 遍历判断是否能被 3 整除

Sequence 这种类似懒加载的实现有下面这些优点：

- 一旦满足条件，就可以跳过后续不必要的循环。
- 像 List 这种实现 Iterable 接口的集合类，每调用一次函数就会生成一个新的 Iterable，下一个函数再基于新的 Iterable 执行，每次函数调用产生的临时 Iterable 会导致额外的内存消耗，而 Sequence 在整个流程中只有一个。

Sequence 这种数据类型可以在数据量比较大或者数据量未知的时候，作为流式处理的解决方案。

### 条件控制

#### if/else
Kotlin 中弃用了三元运算符了但是可以使用`if/else`代替：
```
val max = if (a > b) {
    println("max:a")
    a // 👈 返回 a
} else {
    println("max:b")
    b // 👈 返回 b
}
```

#### when

`when`替代了`switch`，并且功能更强大。与java不同点是：

- 省略了`case`和`break`，因为 Kotlin 自动为每个分支加上了 break 的功能
- Java 中的默认分支使用的是 `default` 关键字，Kotlin 中使用的是 `else`

```
when (x) {
    1, 2 -> print("x == 1 or x == 2")
    else -> print("else")
}
```

- `when`的分支判断条件还可以为表达式，如,

	用`in`判断区间:
	
	```
	when (x) {
	    in 1..10 -> print("x 在区间 1..10 中")
	    in listOf(1,2) -> print("x 在集合中")
	    else -> print("不在任何区间上")
	}
	```
	
	is 进行特定类型的检测：
	
	```
	val isString = when(x) {
	    is String -> true
	    else -> false
	}
	```
	
	还可以省略 when 后面的参数，每一个分支条件都可以是一个布尔表达式：
	
	```
	when {
	    str1.contains("a") -> print("字符串 str1 包含 a")
	    str2.length == 3 -> print("字符串 str2 的长度为 3")
	}
	```
	
### for循环
Kotlin使用`in`关键字实现循环：

```
val array = intArrayOf(1, 2, 3, 4)
	for (item in array){
}

for (i in 0..10) {
    println(i)
}
```

### try-catch

Kotlin 中 try-catch 语句也可以是一个表达式，允许代码块的最后一行作为返回值：

```
val a: Int? = try { parseInt(input) } catch (e: NumberFormatException) { null }
```

### Elvis操作符: `?:`

```
val str: String? = "Hello"
val length: Int = str?.length ?: -1
```

### `==` 和 `===`
- `==` ：可以对基本数据类型以及 String 等类型进行内容比较，相当于 Java 中的 equals
- `===` ：对引用的内存地址进行比较，相当于 Java 中的 ==

## 泛型

java的泛型擦除，就是泛型信息只存在于代码编译阶段，在进入 JVM 之前，与泛型相关的信息会被擦除掉。换句话说，在虚拟机中泛型类和普通类没有什么区别。

### `? extend`：上界通配符（生产者 Producer）
上界通配符可以使Java泛型具有`协变性(Covariance)`。由于`extends`限制了通配符`?`的父类型，所以称为「上界」

```
List<Button> buttons = new ArrayList<Button>();
List<? extends TextView> textViews = buttons; //合法
```
使用了上界通配符之后，`List` 的使用：

```
List<? extends TextView> textViews = new ArrayList<Button>();
TextView textView = textViews.get(0); // 👈 get 可以
textViews.add(textView); //add 会报错，no suitable method found for add(TextView)
```

因为确定了泛型的上界，所以`get`出的对象一定是`TextView`的子类型，根据多态特性，是可以直接赋值给TextView 的。

对于是`add`操作，由于`<? extends TextView>`属于未知类型，可能是`List<Button>`,也可能是`List<TextView>`，如果是前者那么肯定是不可以的。实际情况是编译器无法确定到底属于哪一种，无法继续执行下去，就报错了。

顺便提一下，不要上界通配符直接使用`?`也是不行的，因为`List<?>`其实是`List<? extend Object>`的简写。与上面一样，编译器无法确定`？`的类型，只能get到`Object`类型对象。

由于`add`这个限制，被上界通配符修饰的List只能向外界提供数据，所以叫生产者。上界通配符对应Kotin的`out`关键字。

### `? super`：下界通配符（消费者 Consumer）
`? super`叫做下界通配符，可以使Java泛型具有`逆变性Contravariance`。由于`super`限制了通配符`?`的子类型，所以称作「下界」

`super`限制范围有限制类型的本身，直接父类和间接父类。

使用了下界通配符之后，List的读取：

```
List<? super Button> buttons = new ArrayList<TextView>();
Object object = buttons.get(0); // 👈 get 出来的是 Object 类型
Button button = ...
buttons.add(button); // 👈 add 操作是可以的
```

由于不知道`?`的具体类型，但是Java中所有类型都是继承`Object`的，所以get出的类型是`Object`。一般没有什么实际的使用场景。

Button对象一定是这个位置类型的子类，根据多态特性，`add`是合法的。通常也只拿它来添加数据，也就是消费已有的 `List<? super Button>`，往里面添加 `Button`,因此这种泛型类型声明称之为「消费者 Consumer」。

### 小结：Java泛型本身是不支持协变和逆变的。

- 可以用上界通配符`? extends`来让泛型支持协变，但是「只能读取不能修改」，`remove()`及`clear()`是可以的。
- 使用泛型下界通配符`? super`来让泛型支持逆变，但是「只能修改不能读取」。这里指的是不能按泛型类型读取，如果按`Object`读取，然后向下强转也是可以的。

### PECS
什么使用extends，什么时候使用super。遵循PECS原则：producer-extends, consumer-super。
说白就是extends是限制数据来源的（生产者）而super是限制数据流入的（消费者）。

### Kotlin中的`out`和`in`
和Java一样，Kotlin中泛型本身也是不可变的。
- 使用`out`来支持协变，等同于Java中的上界通配符`? extends`
- 使用`in`类支持逆变，等同于Java中的下界通配符`? super`

`out`表示这个变量或者参数只用来输出，不能用来输入；`in`表示这个变量或者参数只能用来输入，不能用来输出。

```
//生产者：
class Producer<out T> {
    fun produce(): T {
        ...
    }
}
val producer: Producer<TextView> = Producer<Button>() // 👈 这里不写 out 也不会报错
val producer: Producer<out TextView> = Producer<Button>() // 👈 out 可以但没必要

//消费者：
class Consumer<in T> {
    fun consume(t: T) {
        ...
    }
}
val consumer: Consumer<Button> = Consumer<TextView>() // 👈 这里不写 in 也不会报错
val consumer: Consumer<in Button> = Consumer<TextView>() // 👈 in 可以但没必要
```

### `*`号
`*`号等价于`out Any`,也与Java中单个`?`等效。

和 Java 不同的地方是，如果你的类型定义里已经有了 out 或者 in，那这个限制在变量声明时也依然在，不会被 * 号去掉。

比如你的类型定义里是 out T : Number 的，那它加上 <*> 之后的效果就不是 out Any，而是 out Number。

### `where` 关键字

where只是把Java中的老功能换了新写法。

Java声明类或方法的时候，可以使用`extends`来设置边界，将泛型类型参数限制为某个类型的子集：

```
//           👇  T 的类型必须是 Animal 的子类型
class Monster<T extends Animal>{
}

//Kotin写法:
class Monster<T : Animal>
```

注意这个和前面讲的声明变量时的泛型类型声明是不同的东西.

这个边界可以设置成多个，Java中用`&`连接，Kotlin用`where`

```
//           👇  T 的类型必须同时是 Animal 和 Food 的子类型
class Monster<T extends Animal & Food>{ 
}

Kotlin写法，用where：
class Monster<T> where T:Animal,T:Food
```

Kotlin 里 where 这样的写法可读性更符合英文里的语法，尤其是如果 Monster 本身还有继承的时候：

```
class Monster<T> : MonsterParent<T>
    where T : Animal
```

### `reified`关键字

由于 Java 中的泛型存在类型擦除的情况，任何在运行时需要知道泛型确切类型信息的操作都没法用了。
比如不能检查一个对象是否为泛型类型 T 的实例。

```
<T> void printIfTypeMatch(Object item) {
    if (item instanceof T) { // 👈 IDE 会提示错误，illegal generic type for instanceof
        System.out.println(item);
    }
}
```

在Kotlin可以使用关键字 `reified` 配合 `inline` 来解决：

```
inline fun <reified T> printIfTypeMatch(item: Any) {
    if (item is T) { // 👈 这里就不会在提示错误了
        println(item)
    }
}
```

### Kotlin与Java泛型不一致的地方

1. Java中的数组是支持协变的，而Kotlin中的数组不支持协变。

	这是因为在 Kotlin 中数组是用 `Array`	 类来表示的，这个 `Array` 类使用泛型就和集合类一样，所以不支持协变。
	
2. Java 中的 `List` 接口不支持协变，而 Kotlin 中的 `List` 接口支持协变。
	Java 中的 List 不支持协变，需要使用泛型通配符来解决。
	在Kotlin中，`MutableList`接口才相当于Java中的`List`。Kotlin中的List实现了只读操作，所以不会有类型安全上的问题，自然支持协变。
	
看一下Kotlin中`List`接口定义:
	
```
public interface List<out E> : Collection<E> {
```

### 练习题
1. 实现一个 fill 函数，传入一个 Array 和一个对象，将对象填充到 Array 中，要求 Array 参数的泛型支持逆变（假设 Array size 为 1）。

```
fun <T> fill(arr: Array<in T>, t: T) {
	arr[0] = t
}
调用：
fill(arrayOf("a"), "b")
```

2. 实现一个 copy 函数，传入两个 Array 参数，将一个 Array 中的元素复制到另外个 Array 中，要求 Array 参数的泛型分别支持协变和逆变。（提示：Kotlin 中的 for 循环如果要用索引，需要使用 Array.indices）

```
fun <E> copy(src: Array<out E>, dest: Array<in E>) {
	for (index in src.indices) {
		dest[index] = src[index]
	}
}
调用：
copy(arrayOf("a", "b"), arrayOf("c","d"))
```

类似Java中Collections的copy方法：

```
// Collections.java
public static <T> void copy(List<? super T> dest, List<? extends T> src) {
    int srcSize = src.size();
    if (srcSize > dest.size())
        throw new IndexOutOfBoundsException("Source does not fit in dest");

    if (srcSize < COPY_THRESHOLD ||
        (src instanceof RandomAccess && dest instanceof RandomAccess)) {
        for (int i=0; i<srcSize; i++)
            dest.set(i, src.get(i));
    } else {
        ListIterator<? super T> di=dest.listIterator();
        ListIterator<? extends T> si=src.listIterator();
        for (int i=0; i<srcSize; i++) {
            di.next();
            di.set(si.next());
        }
    }
}
```

## 协程

在Kotlin中有这样一个语法糖，当函数最后一个参数是lambda表达式时，可以将lambda表达式写在括号外。设就是它的闭包原则。






