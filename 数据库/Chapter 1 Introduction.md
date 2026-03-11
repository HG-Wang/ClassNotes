The query processor is important because it helps the database system to simplify and facilitate access to data.

-   查询处理器（Query Processor）可以提供一种抽象，将非过程化/声明式的语言在逻辑层面转化为物理层面的高效操作序列
    
    -   DDL interpreter : 解析 DDL 语句并将定义记录到数据字典中
        
    -   DML compiler： 把 DML 语句转换为查询评估引擎可以理解的低级指令
        
    -   Quer evaluation engine : 查询评估引擎， 负责执行 DML 编译器生成的低级指令
        

## Transaction Management

/

-   原子性，一致性，持久性
    

A **transaction** is a collection of operations that performs a single logical function in a database application.

事务是数据库应用中执行单一逻辑功能的操作集合。

## Storage Manager