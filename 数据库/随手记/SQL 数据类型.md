>  该文档由 Claude 4.6 生成， 仅供参考

SQL 中的数据类型主要分为以下几大类：

---

## 1. 数值类型（Numeric）

| 类型                              | 说明         | 示例                            |
| ------------------------------- | ---------- | ----------------------------- |
| `TINYINT`                       | 极小整数（1 字节） | 0 ~ 255                       |
| `SMALLINT`                      | 小整数（2 字节）  | -32,768 ~ 32,767              |
| `INT / INTEGER`                 | 标准整数（4 字节） | ±21 亿                         |
| `BIGINT`                        | 大整数（8 字节）  | 非常大的整数                        |
| `DECIMAL(p,s)` / `NUMERIC(p,s)` | 精确小数（定点数）  | `DECIMAL(10,2)` → 99999999.99 |
| `FLOAT` / `REAL`                | 浮点数（近似值）   | 科学计算用                         |
| `DOUBLE`                        | 双精度浮点数     | 更高精度的近似值                      |

> 💡 **金额/货币** 用 `DECIMAL`，避免用 `FLOAT`（有精度丢失问题）。

---

## 2. 字符串类型（String）

| 类型 | 说明 | 示例 |
|------|------|------|
| `CHAR(n)` | **定长**字符串，不足补空格 | `CHAR(10)` 固定占 10 字符 |
| `VARCHAR(n)` | **变长**字符串，按实际长度存储 | `VARCHAR(255)` 最多 255 字符 |
| `TEXT` | 长文本（大段文字） | 文章内容、评论 |
| `BLOB` | 二进制大对象 | 图片、文件（不推荐） |

> 💡 **常用选择**：短字段用 `VARCHAR`，长文本用 `TEXT`，固定长度（如国家代码）用 `CHAR`。

---

## 3. 日期和时间类型（Date/Time）

| 类型 | 格式 | 示例 |
|------|------|------|
| `DATE` | 日期 | `2025-01-15` |
| `TIME` | 时间 | `14:30:00` |
| `DATETIME` | 日期+时间 | `2025-01-15 14:30:00` |
| `TIMESTAMP` | 时间戳（自动更新） | 常用于记录修改时间 |
| `YEAR` | 年份 | `2025` |

> 💡 `TIMESTAMP` 会自动转换时区，`DATETIME` 不会。

---

## 4. 布尔类型（Boolean）

| 类型 | 说明 |
|------|------|
| `BOOLEAN` / `BOOL` | `TRUE` / `FALSE`（MySQL 中本质是 `TINYINT(1)`） |

---

## 5. 枚举与集合（MySQL 特有）

```sql
-- 只能选其中一个值
gender ENUM('male', 'female', 'other')

-- 可以选多个值
hobbies SET('reading', 'coding', 'gaming')
```

---

## 6. JSON 类型（现代数据库支持）

```sql
-- MySQL 5.7+ / PostgreSQL
info JSON  -- 存储 JSON 文档
```

---

## 快速选型指南

```
整数      → INT（通用）、BIGINT（大范围）
小数/金额 → DECIMAL(p,s)
短文本    → VARCHAR(n)
长文本    → TEXT
日期时间  → DATETIME / TIMESTAMP
是否标记  → BOOLEAN
主键ID    → INT AUTO_INCREMENT 或 BIGINT
```

## 建表示例

```sql
CREATE TABLE users (
    id          INT PRIMARY KEY AUTO_INCREMENT,  -- 整数主键
    username    VARCHAR(50)   NOT NULL,           -- 变长字符串
    email       VARCHAR(100)  UNIQUE,             -- 唯一约束
    balance     DECIMAL(10,2) DEFAULT 0.00,       -- 精确小数
    is_active   BOOLEAN       DEFAULT TRUE,       -- 布尔
    bio         TEXT,                              -- 长文本
    created_at  TIMESTAMP     DEFAULT CURRENT_TIMESTAMP  -- 时间戳
);
```

---

> ⚠️ **注意**：不同数据库（MySQL、PostgreSQL、SQL Server、Oracle）的类型名称和支持程度略有差异，以上以 **MySQL** 为主要参考。