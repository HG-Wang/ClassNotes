#Dart
# Data Types and Variables
## A Brief Introduction to Objects

对象无处不在。从我们吃的食物到养着的宠物，万物皆是对象。
每个对象都具有**特征**和**行为** 。例如，人具有姓名、年龄和身高等特征。人也可以执行走路、吃饭和睡觉等行为。这些特征和行为共同定义了一个人的身份。

![](http://cdn.sealight.site/notes/202603161951219.png)

同样地，Dart 中的一切都是对象。编程语言中的对象也具有被称为属性（properties）的特征，并且它们也能执行被称为方法（methods）的行为。属性代表对象所知道的内容，而方法则代表对象能够执行的操作。

## Variables

变量概念总结（类比书架制作）：

1. 变量就像带标签的盒子
    - 如同制作书架时，将木板、钉子、工具分类放入不同盒子并贴上标签，变量是计算机中存储数据的“盒子”，每个变量有唯一的名字（标识符）。

2. 变量用于分类与快速存取
    - 盒子分装不同材料（如宽木板、窄木板），变量分装不同类型的数据（如数字、文字），方便程序按需调用。

3. 变量的三要素
    - 声明变量 = 准备一个盒子
    - 命名变量 = 给盒子贴唯一标签
    - 赋值与类型 = 规定盒子里放什么（数据类型）并放入内容（初始值）。

4. 核心作用
    - 让程序有条理地存储和访问数据，提高代码可读性和效率。

![](http://cdn.sealight.site/notes/202603162116630.png)

### Declaring a variable in Dart

```dart
main() {
  int myFirstDartVariable = 5;
}
```

在 Dart 中，命名变量时采用小驼峰命名法是一种风格约定。换句话说，你应将每个单词的首字母大写（第一个单词除外），且不使用任何分隔符，例如：lowerCamelCase。
其实和 C/C++的定义方法是一样的。

### Print Variables

下面的语句把变量打印出来
```dart
print(variableName)
```


# Data Type

Dart 的数据类型如下：
![](http://cdn.sealight.site/notes/202603171814279.png)

Numbers, Strings, Booleans, Lists, Sets, Maps, Runes, Symbols

本章将重点讲解数字 Number、字符串 String 和布尔值 Booleans 。列表、集合和映射将在另一章中讨论。游标和符号超出了本课程的范围。

数值类型大概可以分成两类：
- 引用类型
- 值类型
值类型提供的信息是值本身。对于引用类型，其提供的信息是指向某个对象的引用，即对象存储位置的内存地址

在大多数语言中，基本数据类型是值类型，**但在 Dart 中，所有数据类型都是对象。这意味着即使是基本数据类型也是引用类型**。因此，可以说在 Dart 中，变量专门存储引用并指向对象。

### Dafault Value
未初始化的变量的初始值为 `null` 。即使是数值类型的变量，初始值也是 `null` ，因为数字——就像 Dart 中的其他所有内容一样——都是对象。 `null` 仅仅意味着该变量没有引用任何对象；它什么也没有引用。

在下面的代码片段中，我们创建了一个变量 `notInitialized` ，但未对其进行初始化。当我们尝试打印变量 `notInitialized` 的值时，输出结果为 `null` 。

```dart
main() {
    int notInitialized;
    print(notInitialized);
} 
```

## Literals

Literals: 字面量被定义为以事物最常用和基本的方式来表达其含义。将其映射到计算机编程中，字面量是指在源代码中直接出现的固定值。例如， `Hello World` 、 `5` 和 `A` 都是字面量。

# Number

## The `num` Type

如果我们想要一个具有数值类型的变量，我们可以使用 `num` 数据类型来声明它。基本语法如下：
```dart
main() {
  num firstNumber = 5;
  num secondNumber = 5.1;
  num thirdNumber = firstNumber;

  // Driver Code
  print(firstNumber);
  print(secondNumber);
  print(thirdNumber);
}
```

Dart 的 num 还可以继续分成两个子类型：
- integers (`int`)
- doubles (`double`)

## integers

在下面的代码片段中，我们声明了两个 `int` 类型的变量。第一个变量是一个简单的整数，而第二个变量是一个十六进制数。

```dart
main() {
  int simpleInteger = 1;
  int hex = 0xDA34F;
  int integer = simpleInteger;

  // Driver Code
  print(simpleInteger); 
  print(hex);
  print(integer);
}
```

![](http://cdn.sealight.site/notes/202603171915730.png)

## Doubles

在下面的代码片段中，我们声明了两个 `double` 类型的变量。第一个变量是一个普通的 double 值，而第二个变量使用了指数表示法。

```dart
main() {
  double simpleDouble = 1.1;
  double exponents = 1.42e5;

  // Driver Code
  print(simpleDouble);
  print(exponents);
}
```

截至 **Dart 2.1**，整数字面量在必要时会自动转换为双精度浮点数。当您运行下面的代码片段时，显示的输出将是 `1.0` 而不是 `1`。

![](http://cdn.sealight.site/notes/202603171919401.png)

---

# Strings

Dart 字符串是 **UTF-16** 代码单元的序列。**UTF** 代表 _Unicode 转换格式_ 。[Unicode](https://unicode-table.com/en/#0041) 是一套字符集，其中每个字符都是唯一的代码单元
>  这个是借鉴 Java/JS 的，他们底层也是 UTF-16

## String literals

从表面来看，字符串字面量只是被引号（单引号和双引号都 OK）包裹的文本。让我们看看在 Dart 中创建字符串的各种方法。

```dart
main() {
  // Single Quotes
  print('Using single quotes');

  // Double Quotes
  print("Using double quotes");

  // Single quotes with escape character \
  print('It\'s possible with an escape character');

  // Double quotes
  print("It's better without an escape character");
}
```

上面代码片段中，我们只是用多种方法打印字符串。所有这些方法都相当直接，除了可能是**第 9 行** 。在**第 9 行**中，我们使用单引号来创建字符串，但字符串本身在单词 `It's` 中包含一个单引号。如果我们原样打印该字符串 `'It's possible with an escape character'` ，将会出错，因为编译器会将单词 `It's` 中的单引号视为字符串的结束引号。为了解决这个问题，我们可以使用转义字符（`\`），它告诉编译器“跳过”单引号的内置功能，并将其作为字符串的一部分进行计算。

## Defining a string variable

如果我们想要定义一个存储 `String` 类型值的变量，我们将使用以下语法：
```dart
main() {
    String s1 = "A String";
    print(s1);
} 
```

# String Interpolation 字符串插值

## String Concatenation 字符串拼接

两个或多个字符串的连接是通过 `+` 运算符完成的
```dart
main() {
  String s1 = "First half of the string. ";
  String s2 = "Second half of the string";
  print(s1 + s2);
}
```

上面的代码相当简单。在第 **4 行** ，我们将第一个字符串 `s1` 和第二个字符串 `s2` 连接起来，然后打印连接后的字符串。
这个和 C++是类似的，详见  [[C++ 字符串拼接]]

## String interpolation

**字符串插值**是指通过嵌入表达式来创建新字符串或修改现有字符串的能力。表达式会被求值，生成的结果值随后转换为字符串，并与外层字符串进行拼接。插值是 Dart 语言中比字符串拼接更简洁、更高效的替代方案。不过，字符串插值比简单的拼接复杂得多，这也使其具备更大的灵活性。

### Syntax

字符串中未转义的 `$` 字符表示插值表达式的开始。`$` 符号后可以跟一个不包含 `$` 字符的单一 _标识符 id_
```dart
print ("optionalstring $variableIndentifies optional string");
```

`＄` 符号后也可以跟一个由大括号定界的表达式。

```dart
print("optional string ${expression} optional string");
```

![](http://cdn.sealight.site/notes/202603172000806.png)

## Mupltiple Line

您也可以使用相邻的字符串字面量来拼接字符串。请注意我们同时使用了双引号（`"`）和单引号（`'`）。

![](http://cdn.sealight.site/notes/202603172002906.png)

您可以在 Dart 中创建多行字符串。只需按期望打印的方式编写字符串，并用三个双引号或三个单引号将其括起来即可。
>  这个也是和 C++ 里面一样的。

```dart
main() {
  var multilineString = """This is a 
multiline string
consisting of 
multiple lines""";

  print(multilineString);
}
```

# Booleans
Dart 的 `bool` 类型表示布尔值。只有两个对象属于 `bool` 类型，即布尔字面量 `true` 和 `false`。
```dart
main() {
  bool b1 = true;
  print(b1);
}
```

没什么好说的，很简单

---



#英语 
- planks of wood → 木板