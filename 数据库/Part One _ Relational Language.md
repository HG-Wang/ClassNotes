# 章节速览

第二章介绍了关系数据库的基本概念，涵盖关系代数—— 一种构成SQL基础的形式化查询语言。SQL语言是当今使用最广泛的关系查询语言，本章将对其进行深入详尽的阐述。

第三章概述了SQL查询语言，包括SQL数据定义、查询基本结构、集合运算、聚合函数、嵌套子查询以及数据库修改操作。

第四章进一步详解SQL语言，涵盖连接表达式、视图、事务、数据库强制执行的完整性约束，以及控制用户访问与更新操作的授权机制。

第五章探讨SQL相关高级主题，包括编程语言调用SQL、函数、存储过程、触发器、递归查询以及高级聚合功能。

---

# Introduction to the Relational Model

【下面很多内容是直接搬运翻译过来的，没有做整理】

关系模型仍然是商业数据处理应用中的主要数据模型。相较于早期的网络模型或层次模型，关系模型因其简洁性降低了程序员的开发难度，从而确立了主导地位。在其半个多世纪的发展历程中，该模型通过不断融入新特性与功能持续保持领先优势，例如支持复杂数据类型和存储过程的对象关系特性、XML数据处理能力，以及各类半结构化数据支持工具。关系模型不依赖任何特定底层数据结构的特点，使其在面对包括为大规模数据挖掘设计的现代列式存储在内的新型数据存储方式时，依然保持着持久的生命力。

本章中，我们将首先探讨关系模型的基本原理。关系数据库领域已形成一套完整的理论体系：第六章与第七章将研究有助于关系数据库模式设计的理论要点；第十五章和第十六章将讨论涉及查询高效处理的理论内容；第二十七章则会深入探讨形式化关系语言的相关理论，这已超出本章的基础介绍范畴。

## Structure of Relational Database

Thus, in the relational model the term relation is used to refer to a table, while theterm tuple is used to refer to a row. Similarly, the term attribute refers to a column of atable.

关系：表

tuple 元组 ：行

attribute ： 列

我们使用术语“关系实例”来指代一个关系的特定实例，即包含一组特定行的集合。那么来说的话，一个元组就是一个关系实例

Table中行的排列是无关紧要的：

-   关系（relation）本质上是一个集合（set），而集合中的元素（即行 / tuple）没有固定的顺序。
    

对于关系的每一个属性，都有一组允许的值： 我们称之为 Domain

对于所有关系r，其所有属性的域必须是原子的。如果一个域中的元素被视为不可分割的单位，则该域是原子的

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

（商业数据库系统放宽了对关系必须是集合的要求，允许存在重复元组。这一点将在第3章进一步讨论。）

superkey (超码) 是一个或多个属性的集合，这些属性共同作用能够<u>唯一标识</u>关系中的元组。例如，教师关系中的ID属性足以区分不同的教师元组，因此ID是一个超码。而教师关系中的姓名属性则不是超码，因为多位教师可能拥有相同的姓名。

![pasted_image_06e8cd13-030c-450c-99c2-f92ec56edc01.png](file:///Users/wanghaiguang/Library/Application Support/CherryStudio/Data/Files/06e8cd13-030c-450c-99c2-f92ec56edc01.png)

也就是说不同record 的 superkey 不一样

如果K是超码，那么K的任何超集也是超码。

我们通常关注的是那些没有真子集能成为超码的超码，这类最小超码被称为候选码。(Candidate keys)

-   可能存在多个不同的属性集都可以作为候选码。
    

我们将使用主键（Primary key）这一术语，指代由数据库设计者选定、用于标识关系中元组的主要候选码。

键（无论是主键、候选键还是超键）是整个关系的属性，而非单个元组的属性。关系中任意两个元组禁止在键属性上同时具有相同的值。键的指定代表了所建模的现实企业中的一种约束，<u>因此主键也被称为主键约束。</u>

关系模式中的主键属性会列在其他属性之前；例如，部门关系中的系名属性被列在首位，因为它是主键。主键属性通常也会加下划线标注。

![pasted_image_ac81d2c0-a129-4079-8c4a-36c93e14e0b7.png](file:///Users/wanghaiguang/Library/Application Support/CherryStudio/Data/Files/ac81d2c0-a129-4079-8c4a-36c93e14e0b7.png)

选择主键时应确保其属性值极少或从不发生更改。例如，个人的地址字段不应作为主键的一部分，因为地址很可能变动。

![pasted_image_402c0852-c8e0-4bd3-bfbf-8d7cd206e375.png](file:///Users/wanghaiguang/Library/Application Support/CherryStudio/Data/Files/402c0852-c8e0-4bd3-bfbf-8d7cd206e375.png)

外键约束规定了从关系 r1 的属性集 A 到关系 r2 的主键 B 的引用关系

属性集 A 被称为来自 r1 并引用 r2 的外键。关系 r1 也被称为该外键约束的参照关系，而 r2 则被称为被参照关系。

注意一下： 被引用的属性在所在的被引用关系里面，应该要是主键（Primary key)

而更一般的情况——参照完整性约束——放宽了要求，允许被引用的属性不必构成被引用关系的主键。

-   这里回顾一下两种完整性约束
    
    -   实体完整性约束：每个表必须要有一个 Primary Key ，而且必须唯一且非空
        
    -   参照完整性约束： 这是一个广义的、理论上的概念。它指的是：关系A（参照关系）中的某个属性（或属性组）的值，必须在关系B（被参照关系）的某个属性（或属性组）中存在。
        
    -   外键约束：这是数据库系统实际实现的、狭义且具体的一种参照完整性约束。+  B中被参照的属性必须是一个主键（或至少具有唯一约束）。
        

需要注意的是，`time_slot` 虽然构成了 `time_slot` 关系主键的一部分，但并未单独形成该关系的主键。因此，我们无法使用外键约束来强制执行上述约束。实际上，**外键约束是参照完整性约束的一种特殊情况**——其要求被引用的属性必须构成被参照关系的主键。当今的数据库系统通常支持外键约束，但**并不支持被引用属性非主键的通用参照完整性约束**。

# 