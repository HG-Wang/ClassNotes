# 章节速览

第二章介绍了关系数据库的基本概念，涵盖关系代数—— 一种构成 SQL 基础的形式化查询语言。SQL 语言是当今使用最广泛的关系查询语言，本章将对其进行深入详尽的阐述。

第三章概述了 SQL 查询语言，包括 SQL 数据定义、查询基本结构、集合运算、聚合函数、嵌套子查询以及数据库修改操作。

第四章进一步详解 SQL 语言，涵盖连接表达式、视图、事务、数据库强制执行的完整性约束，以及控制用户访问与更新操作的授权机制。

第五章探讨 SQL 相关高级主题，包括编程语言调用 SQL、函数、存储过程、触发器、递归查询以及高级聚合功能。

---

# Introduction to the Relational Model

【下面很多内容是直接搬运翻译过来的，没有做整理】

关系模型仍然是商业数据处理应用中的主要数据模型。相较于早期的网络模型或层次模型，关系模型因其简洁性降低了程序员的开发难度，从而确立了主导地位。在其半个多世纪的发展历程中，该模型通过不断融入新特性与功能持续保持领先优势，例如支持复杂数据类型和存储过程的对象关系特性、XML 数据处理能力，以及各类半结构化数据支持工具。关系模型不依赖任何特定底层数据结构的特点，使其在面对包括为大规模数据挖掘设计的现代列式存储在内的新型数据存储方式时，依然保持着持久的生命力。

本章中，我们将首先探讨关系模型的基本原理。关系数据库领域已形成一套完整的理论体系：第六章与第七章将研究有助于关系数据库模式设计的理论要点；第十五章和第十六章将讨论涉及查询高效处理的理论内容；第二十七章则会深入探讨形式化关系语言的相关理论，这已超出本章的基础介绍范畴。

## Structure of Relational Database

Thus, in the relational model the term relation is used to refer to a table, while theterm tuple is used to refer to a row. Similarly, the term attribute refers to a column of atable.

关系：表

tuple 元组 ：行

attribute ： 列

我们使用术语“关系实例”来指代一个关系的特定实例，即包含一组特定行的集合。那么来说的话，一个元组就是一个关系实例

Table 中行的排列是无关紧要的：

-   关系（relation）本质上是一个集合（set），而集合中的元素（即行 / tuple）没有固定的顺序。
    

对于关系的每一个属性，都有一组允许的值： 我们称之为 Domain

对于所有关系 r，其所有属性的域必须是原子的。如果一个域中的元素被视为不可分割的单位，则该域是原子的

\=> 就是说，有 Subpart 不可以当作是元素

**<u>Null value :</u>**

-   表示该值未知或不存在
    
-   空值在访问或更新数据库时会引发诸多问题
    

## Databae Schema

我们必须区分 database schema 和 database instance 之间的区别

-   database schema ： logical design
    
-   database instance : 特定时刻数据状态的快照
    

我们以 department 的 schema 为例：

```sql
department(dept_name,building,budget)
```

在关系模式中使用共有属性是关联不同关系元组的一种方式。

大学中的每门课程可能在不同的学期、甚至在同一学期内开设多次。我们需要一个关系来描述课程的每一次具体开设，即"教学班"(section)。

```sql
section(course_id,sec_id,semester,year,building,room_number,time_slot_id)
```

描述 instructor 和他教授的课之间的关系

```sql
teaches（ID,course_id,sec_id,semester,year)
```

当然了，实际在大学中我们可以构建的关系还可以有很多，比如：

```sql
student(ID,name,dept_name,tot_cred)
advisor(s_id,i_id)
takes(ID,course_id,sec_id,semester,year,grade)
```

---

# Keys

我查了一下，Superkey 有很多种翻译方法，这里我就超键和超码混用了

我们必须有一种方法来指定如何区分给定关系中的元组。这通过其属性来表达。也就是说，元组的属性值必须能够唯一标识该元组。换言之，关系中不允许存在两个元组在所有属性上具有完全相同的值。

（商业数据库系统放宽了对关系必须是集合的要求，允许存在重复元组。这一点将在第 3 章进一步讨论。）

superkey (超码) 是一个或多个属性的集合，这些属性共同作用能够<u>唯一标识</u>关系中的元组。例如，教师关系中的 ID 属性足以区分不同的教师元组，因此 ID 是一个超码。而教师关系中的姓名属性则不是超码，因为多位教师可能拥有相同的姓名。

![pasted_image_06 e 8 cd 13-030 c-450 c-99 c 2-f 92 ec 56 edc 01.png](file:///Users/wanghaiguang/Library/Application Support/CherryStudio/Data/Files/06 e 8 cd 13-030 c-450 c-99 c 2-f 92 ec 56 edc 01.png)

也就是说不同 record 的 superkey 不一样

如果 K 是超码，那么 K 的任何超集也是超码。

我们通常关注的是那些没有真子集能成为超码的超码，这类最小超码被称为候选码。(Candidate keys)

-   可能存在多个不同的属性集都可以作为候选码。
    

我们将使用主键（Primary key）这一术语，指代由数据库设计者选定、用于标识关系中元组的主要候选码。

键（无论是主键、候选键还是超键）是整个关系的属性，而非单个元组的属性。关系中任意两个元组禁止在键属性上同时具有相同的值。键的指定代表了所建模的现实企业中的一种约束，<u>因此主键也被称为主键约束。</u>

关系模式中的主键属性会列在其他属性之前；例如，部门关系中的系名属性被列在首位，因为它是主键。主键属性通常也会加下划线标注。

![pasted_image_ac 81 d 2 c 0-a 129-4079-8 c 4 a-36 c 93 e 14 e 0 b 7.png](file:///Users/wanghaiguang/Library/Application Support/CherryStudio/Data/Files/ac 81 d 2 c 0-a 129-4079-8 c 4 a-36 c 93 e 14 e 0 b 7.png)

选择主键时应确保其属性值极少或从不发生更改。例如，个人的地址字段不应作为主键的一部分，因为地址很可能变动。

![pasted_image_402 c 0852-c 8 e 0-4 bd 3-bfbf-8 d 7 cd 206 e 375.png](file:///Users/wanghaiguang/Library/Application Support/CherryStudio/Data/Files/402 c 0852-c 8 e 0-4 bd 3-bfbf-8 d 7 cd 206 e 375.png)

外键约束规定了从关系 r 1 的属性集 A 到关系 r 2 的主键 B 的引用关系

属性集 A 被称为来自 r 1 并引用 r 2 的外键。关系 r 1 也被称为该外键约束的参照关系，而 r 2 则被称为被参照关系。

注意一下： 被引用的属性在所在的被引用关系里面，应该要是主键（Primary key)

而更一般的情况——参照完整性约束——放宽了要求，允许被引用的属性不必构成被引用关系的主键。

-   这里回顾一下两种完整性约束
    
    -   实体完整性约束：每个表必须要有一个 Primary Key ，而且必须唯一且非空
        
    -   参照完整性约束： 这是一个广义的、理论上的概念。它指的是：关系 A（参照关系）中的某个属性（或属性组）的值，必须在关系 B（被参照关系）的某个属性（或属性组）中存在。
        
    -   外键约束：这是数据库系统实际实现的、狭义且具体的一种参照完整性约束。+  B 中被参照的属性必须是一个主键（或至少具有唯一约束）。
        

需要注意的是，`time_slot` 虽然构成了 `time_slot` 关系主键的一部分，但并未单独形成该关系的主键。因此，我们无法使用外键约束来强制执行上述约束。实际上，**外键约束是参照完整性约束的一种特殊情况**——其要求被引用的属性必须构成被参照关系的主键。当今的数据库系统通常支持外键约束，但**并不支持被引用属性非主键的通用参照完整性约束**。

# Schema Diagrams 模式图

- 每个关系显示为一个方框 ， 属性列在方框内部。 主键属性有下划线。外键约束用箭头形式呈现。从参照关系的外键属性指向被参照关系的主键。如果是非外键约束的参照完整性约束，我们用双箭头表示。

![](http://cdn.sealight.site/notes/202603131733200.png)

# 2.5 Relational Query Language

查询语言是一种用户用以从数据库中请求信息的语言。
分类：
- 命令式   imperative
	- 有状态变量
- 函数式   functional 
	- 不更新程序状态
- 声明式   declarative
	- 数学逻辑
存在多种纯粹的查询语言：
- 关系代数 ， 我们在下一节里面详述。他属于函数式查询语言，构成了 SQL 查询语言的理论基础
- 元组关系演算（tuple relational calculus）和域关系演算（domain relational calculus）则属于声明式查询语言。

>  These query languages are terse and formal, lacking the “syntactic sugar” of commerciallanguages, but they illustrate the fundamental techniques for extracting data from thedatabase. 简洁而正式，没什么语法糖，但是是理论基础

我们实际使用的语言，像是 SQL ，其实是三者的混合。

# 2.6 The Relational Algebra 关系代数

关系代数一系列这样的操作： 输入一个或者两个 Relation, 然后输出一个新的 Relation
分成：
- 一元操作（**unary** operation）: 顾名思义，就是接受一个 Relation 作为输入。包括 select, project, and rename 等操作
- 二元操作（**Binary** operation）: 接受两个 Relation 作为输入，比如 union, Cartesian product, and set diﬀerence

我们之前了解过，Relation 是 tuple 的 set，也就是说每个 tuple 是不能够有重复的，所以 we require that **duplicates be eliminated**, 消除重复。【数学定义】。事实上的商用数据库是允许有重复的。

---
## 2.6.1 The Select Operation

`SELECT` operation 选取满足给定谓词的元组。我们使用小写希腊字母西格玛（σ）来表示选择操作。谓词作为σ的**下标**出现，而参数关系 Relation 则放在σ后面的括号内。因此，要选取教师关系中属于“物理”系的教师元组，我们可以这样表示：
$$σ_{dept\_name = "Physics" }( instructor )$$
We can ﬁnd all instructors with salary greater than $90,000 by writing:
$$
σ_{salary > 90000}( instructor )
$$
通常来说，我们可以用 $> \; < \; \ge \; \le \; \ne \; =$ 等一系列符号来描述我们的谓词
我们还可以把不同的谓词用下面的与或非符号组成更大的谓词：
- and (∧), or (∨), and not (¬)
比如如果我们想要找在物理系且工资大于 $90000 的 instructor，那么就有：
$$
\sigma_{dept\_name = "Physics" \land \;salary \;>90000 }
$$
谓词也可以是两个 attribute 之间的比较。比如我们想要找到部门名和建筑名一样的部门，就可以用下面的 operation：
$$
\sigma_{dept\_name = building}(department)
$$
---

## 2.6.2 The Project Operation

如果我们只想要 Instructor 的 salary，id 和 name，而不想要 dept 字段呢？那么我们就要用 project 操作了。

**project** 也是一个一元操作，输入一个关系，输出去掉其中一些 attribute 之后的关系。由于我们强调过，Relation 是不会保留相同的 tuple 的，如果说去掉某些 attribute 之后导致两个 tuple 一样了，是要**去重**的。

projection 用希腊字母大π表示： $\Pi$ , 我们把想要保留的字段/属性作为**下标**
$$\Pi_{I D, \text { name, salary }}(instructor)$$
投影运算符（Project Operator）的进阶用法：
Π_L(E), L 里面**允许写表达式**（比如四则运算、函数等）。系统会根据表达式**现场计算出新的一列**，然后把这列也包含在结果里。
$$\Pi_{I D, \text { name,salary } /12}(instructor)$$
比如这里就把年薪变成了月薪

## 2.6.3 Composition of Relational Operations

relational operation 的结果本身也是一个 relation , 所以我们可以对它做复合运算。比如 “Find the names of all instructors in the Physics department.”
$$
Π_{name}（σ_{dept\_name="Physics"}（instructor））
$$
> relational-algebra operations can be composed together into a
> relational-algebra expression.

## 2.6.4 The Cartesian-Product Operation

笛卡尔积，用 $\times$ 表示，允许我们把两个 Relation 的信息结合起来。$r_1$ 和 $r_2$ 的笛卡尔积写作 $r_1 \times r_2$ 。 和算术运算里的笛卡尔积不太一样，它不会产生一个 Pair，而是一个单独的 tuple。如图所示：
![](http://cdn.sealight.site/notes/202603141045899.png)

由于两个 Relation 中有的 attribute 可能会充满，所以我们要标注一下来源：
> (instructor.ID, instructor.name, instructor.dept name, instructor.salary,teaches.ID, teaches.course id, teaches.sec id, teaches.semester, teaches.year)

这样我们就可以区分  instructor.ID 和 teaches.ID 了。当然，如果有的属性只在两个 Relation 的其中一个里面，我们就可以把前缀去掉啦。
>  (instructor.ID, name, dept name, salary,teaches.ID, course id, sec id, semester, year)

作为笛卡尔积运算参数的关系必须具有不同的名称。这一要求在某些情况下会引发问题，例如当需要计算一个关系与其自身的笛卡尔积时，或者 Input 是表达式结果的时候。所以我们需要继续学习重命名操作。
既然我们已经知道关系模式 r = instructor × teaches，那么 r 中包含哪些元组呢？你可能已经猜到，我们通过从 instructor 关系（图 2.1）和 teaches 关系（图 2.7）中**各取一个元组，构成所有可能的元组对**，从而形成 r 中的元组。因此，r 是一个很大的关系，从图 2.12 中你可以看到这一点——该图仅展示了构成 r 的部分元组。

## 2.6.5 The Join Operation

假设我们想要查找所有教师的信息，以及他们教授过的所有课程的课程 ID。我们需要结合教师关系和授课关系中的信息来计算所需结果。教师关系和授课关系的笛卡尔积确实能将这两个关系的信息合并，但遗憾的是，笛卡尔积会将每位教师与每门已开设的课程关联起来，无论该教师是否教授过该课程。
由于笛卡尔积操作会将教师表中的每个元组与授课表中的每个元组进行组合，因此我们知道，如果一位教师教授过某门课程（如授课关系中所记录），那么在教师表与授课表的笛卡尔积结果中，必然存在某个元组包含该教师的姓名，并且满足教师表中的 ID 等于授课表中的 ID。因此，如果我们写出以下表达式：
$$\sigma_{instructor.ID = teaches.ID}（instructor \times teaches）$$
我们就可以得到想要的结果。
![](http://cdn.sealight.site/notes/202603141334572.png)
值得注意的是，此表达式会导致教师 ID 的重复。通过添加投影操作消除 teaches.ID 列，即可轻松解决此问题。连接运算（**join**）使我们能够将选择和笛卡尔积组合为单一操作。
Consider relations $r(R)$ and $s(S)$， and let  $\theta$  be a predicate on attributes in the schema $R \cup S$ . The **join** operation $r \bowtie_\theta s$ is defined as follows：
$$
r \bowtie_\theta s = \sigma_\theta（r \times s）
$$
Thus, $\sigma_{\text{instructor.ID=teaches.ID}}（instructor \times teaches）$ can equivalently be written as $instructor \bowtie_{\text{instructor.ID=teaches.ID}} teaches.$ 


还有一个概念叫做自然连接，教材里面好像没有讲：
**自然连接（Natural Join）** 是关系数据库中的一种特殊连接操作。它的核心特点是 **自动基于两个表中所有同名列进行等值连接，并且结果中会消除重复的列**。
## 2.6.6 Set Operations

考虑一个查询：找出所有在 2017 年秋季学期、2018 年春季学期或两个学期都开设的课程集合。要找出所有在 2017 年秋季学期开设的课程，我们可以这样写：
$$\Pi_{course\_id}（\sigma_{semester="\text{Fall"}\;\land \;year=2017}（section））$$
要找出所有在 2018 年春季学期开设的课程，我们可以这样写：
$$\Pi_{course\_id}（\sigma_{semester="\text{Spring"}\;\land \;year=2018}（section））$$
为了回答查询，我们需要这两个集合的**并集**；也就是说，我们需要出现在这两个关系中任意一个或同时出现的所有课程 ID。我们通过二元操作并集来找到这些数据，如同集合论中一样，**用符号∪表示**。因此所需的表达式是：
![](http://cdn.sealight.site/notes/202603141813646.png)

结果中有八个元组，尽管 2017 年秋季学期开设了三门不同的课程，而 2018 年春季学期开设了六门不同的课程。由于关系是集合，重复的值（例如 CS-101，在两个学期都有开设）会被替换为单一出现。注意，在我们的例子中，我们对两个集合进行了并集操作，这两个集合都由课程 ID 值组成。一般来说，要使并集操作有意义：
1. 我们必须确保进行并集操作的输入关系具有**相同数量的属性**；关系的属性数量称为其元数。
2. 当属性具有关联类型时，两个输入关系的**第 i 个属性的类型必须相同**，对于每个 i 而言。
这类关系被称为兼容关系。例如，对 instructor（教师）关系和 section（课程段）关系进行并集操作是没有意义的，因为它们具有不同数量的属性。尽管 instructor 关系和 student（学生）关系都具有元数 4，但它们的第 4 个属性，即 salary（工资）和 tot_cred（总学分），属于两种不同的类型。在大多数情况下，对这两个属性进行并集操作是没有意义的。

本质就是**拼接后去重**

**交操作，记作 ∩**，允许我们找出同时存在于两个输入关系中的元组。表达式 r ∩ s 产生一个关系，包含那些既在 r 中又在 s 中的元组。与并操作类似，我们必须确保在兼容的关系之间进行交操作。
假设我们希望找出在 **2017 年秋季** 和 **2018 年春季** 两个学期都开设的所有课程的集合。  
利用集合的交运算，我们可以这样写：
![](http://cdn.sealight.site/notes/202603141813450.png)

差集操作，记作 **−**，允许我们找出在一个关系中存在但在另一个关系中不存在的元组。表达式 **r − s** 生成一个关系，包含那些在 r 中但不在 s 中的元组。

我们可以通过以下表达式找出在 **2017 年秋季学期** 开设但 **不在 2018 年春季学期** 开设的所有课程：

![](http://cdn.sealight.site/notes/202603141824813.png)

和 union 一样，我们也希望 set differences 的两个关系应该是相容的。

## 2.6.7 The Assignment Operation

有时，通过将关系代数表达式的部分分配给临时关系变量来书写会很方便。赋值操作，记作 **←**，其工作方式类似于编程语言中的赋值。  
为了说明这个操作，考虑我们之前见过的查询：找出在 **2017 年秋季** 和 **2018 年春季** 都开设的课程。  
我们可以这样写：
![](http://cdn.sealight.site/notes/202603141915210.png)

上面的最后一行显示了查询结果。前两行将查询结果赋值给一个临时关系。  
赋值操作本身并不会向用户显示任何关系；相反，**←** 右侧表达式的结果会被赋给 **←** 左侧的关系变量。这个关系变量可以在后续表达式中使用。

通过赋值操作，查询可以写成一个顺序程序，由一系列赋值语句和一个表达式组成，该表达式的值会作为查询结果显示出来。  
对于关系代数查询，赋值必须总是针对临时关系变量；对永久关系的赋值则构成数据库修改。

需要注意的是，赋值操作并没有为代数提供额外的表达能力，但它是一种表达复杂查询的便捷方式。

## 2.6.8 The Rename Operation

与数据库中的关系不同，关系代数表达式的结果没有我们可以用来引用它们的名称。在某些情况下，为它们命名是有用的；重命名操作符，用小写希腊字母 rho（ρ）表示，允许我们这样做。给定一个关系代数表达式 E，表达式
$$
\rho_{x}（E）
$$
以 x 的名字返回表达式 E 的结果
关系 r 本身被视为一个（平凡的）关系代数表达式。因此，我们也可以对关系 r 应用重命名操作，使其获得一个新的名称。某些查询中需要多次使用同一个关系；在这种情况下，重命名操作可用于为同一关系的不同出现赋予唯一的名称。（我们之前不是在笛卡尔积的 attributes 里面提到过吗，为了避免前缀一样，就可以重命名）

重命名操作的第二种形式如下：  
假设关系代数表达式 \( E \) 的元数为 \( n \)。那么，表达式  
$$\rho_{x(A_1, A_2, \dots, A_n)}(E)$$
返回表达式  E  的结果，并将其命名为  x ，同时将属性重命名为 $A_1, A_2, \dots, A_n$ 。这种形式的**重命名操作**可用于为涉及属性表达式的关系代数运算结果中的属性命名。

## 2.6.9 Equivalent Queries

ﬁnds information about courses taught by instructors in the Physics department:
$$\sigma_{dept\_name="Physics"}(instructor \bowtie_{instructor.ID=teaches.ID} teaches)
$$
也可以有另外一种写法：
$$(\sigma_{dept\_name="Physics"}(instructor)) \bowtie_{instructor.ID=teaches.ID} teaches$$
二者的差异就在于，在第一个查询中，将系别限制为物理系的选择操作是在完成教师表和授课表的连接之后进行的；而在第二个查询中，对系别限制为物理系的选择操作先作用于教师表，随后才执行连接操作。
尽管这两个查询并非完全相同，但它们实际上是等价的；也就是说，在任何数据库上它们都会给出相同的结果。数据库系统中的查询优化器通常会分析表达式所计算的结果，并寻找一种高效的计算方式，而不是严格遵循查询中指定的步骤序列。

# 扩展运算

>  做作业的时候才发现还有扩展运算没有复习到，今天晚上把扩展也整理完吧。

之前介绍了五种基本运算，但是对于一些查询来说，使用基本运算可能过于繁琐。
## Outer Join (外连接)
![](http://cdn.sealight.site/notes/202603151031324.png)

![](http://cdn.sealight.site/notes/202603151032682.png)

## 除运算
![](http://cdn.sealight.site/notes/202603151035248.png)

![](http://cdn.sealight.site/notes/202603151037735.png)

![](http://cdn.sealight.site/notes/202603151039396.png)

