# Database Engine

The query processor is important because it helps the database system to simplify and facilitate access to data.

-   查询处理器（Query Processor）可以提供一种抽象，将非过程化/声明式的语言在逻辑层面转化为物理层面的高效操作序列
    
    -   DDL interpreter : 解析 DDL 语句并将定义记录到数据字典中
        
    -   DML compiler： 把 DML 语句转换为查询评估引擎可以理解的低级指令
        
    -   Quer evaluation engine : 查询评估引擎， 负责执行 DML 编译器生成的低级指令
        

## Storage Manager

The storage manager is the component of a database system that provides the interface between the low-level data stored in the database and the application programs andqueries submitted to the system.

存储管理器的组件主要包括：

-   Authorizaation and integrity manager
    
-   Transaction manager
    
-   File manager
    
-   Buffer manager
    

## Transaction Management

-   原子性，一致性，持久性
    
-   程序员负责一致性；数据库负责原子性和持久性(恢复管理器完成)。
    

A **transaction** is a collection of operations that performs a single logical function in a database application.

事务是数据库应用中执行单一逻辑功能的操作集合。

最后，当多个事务并发更新数据库时，即使每个单独事务都是正确的，数据的一致性可能仍然无法保持。

**并发控制管理器 (Concurrency-control manager)** 负责控制并发事务之间的交互，以确保数据库的一致性。

---

# Database and Applicaiton Architecture

目前我们已能清晰呈现数据库系统的各组成部分及其相互连接关系。图1.3展示了运行于集中式服务器上的数据库系统架构，该图系统性地呈现了各类用户与数据库的交互方式，以及数据库引擎各组件间的连接机制。

![pasted_image_c333be4c-acfc-43e5-810a-245442afaf0d.png](file:///Users/wanghaiguang/Library/Application Support/CherryStudio/Data/Files/c333be4c-acfc-43e5-810a-245442afaf0d.png)

后续章节会继续了解关于并行系统架构的内容，我们这里先不做了解。

我们现重点探讨采用数据库作为后端的应用架构。如图1.4所示，这类数据库应用通常分为两到三个模块。早期数据库应用采用双层架构：客户端运行应用程序，通过查询语言指令在服务器端调用数据库系统功能。

![pasted_image_1b260964-1207-4cd0-a06c-d428dcfea57d.png](file:///Users/wanghaiguang/Library/Application Support/CherryStudio/Data/Files/1b260964-1207-4cd0-a06c-d428dcfea57d.png)

# 1.8 Database User and Adminstrators

数据库系统的主要目标是检索数据库中的信息和存储新的信息。使用数据库的人可以分为数据库用户和数据库管理员。

Database Users and User Interface

-   naive user : 傻瓜式操作，视图化
    
-   Application Programmer: 应用软件开发人员是编写应用软件的计算机专业人员，他们可以使用多种工具开发用户界面。
    
-   Sophisticated user :  高级用户无需编写程序即可与系统交互。他们通过数据库查询语言或数据分析软件等工具来构建查询请求。提交查询以探索数据库数据的分析师即属于此类用户。
    

---

Database Administrator

使用数据库管理系统（DBMS）的主要原因之一，是实现对数据及访问这些数据的程序的集中控制。拥有这种系统集中控制权的人员被称为数据库管理员（DBA）。DBA的职能包括：

-   Schema definition
    
-   Storage structure and access-method definition
    
-   Schema and physical-organization modification
    
-   Granting of authorizaiton for data access
    
-   Routine maintenance
    

# History of Database System

这部分就快速过吧，不是很重要

纸带 → 磁带 → 硬盘 → 关系型数据库 → 半结构化数据 → 图数据库

NoSQL : eventual consistency 最终一致性

SaaS
