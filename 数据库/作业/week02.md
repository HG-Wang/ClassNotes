# 第2周作业：关系代数与数学证明

**场景：校园教务系统"极限抢课"**

---

## 背景与数据

校园教务系统包含以下关系表：

| 表名 | 属性 | 说明 |
|------|------|------|
| `Student(sid, name, credit)` | 学号、姓名、学分 | — |
| `Course(cid, cname, teacher, is_hardcore)` | 课程号、课名、教师、是否硬核 | `is_hardcore` 为布尔值 |
| `SelectRecord(sid, cid, status)` | 学号、课程号、选课状态 | `status` ∈ {'Success', 'Waiting', 'Failed'} |

---

## 题1：关系代数精确追踪

**任务：** 手工作答以下关系代数表达式，给出结果表。

$$\pi_{name}\!\left(\sigma_{status=\text{'Success'}}\!\left(Student \bowtie SelectRecord\right)\right)$$

**要求：**
1. 列出自然连接 $Student \bowtie SelectRecord$ 的完整中间表（展示所有属性列）。
2. 在中间表上施加选择条件 $\sigma_{status=\text{'Success'}}$，保留满足条件的行。
3. 最终对 $name$ 属性投影，给出"抢课成功"学生名单。
4. 用文字说明：若同一学生同时出现在多门成功记录中，投影后结果集如何处理重复元组（关系模型的集合语义）。

---

## 题2：除法算子（Division）硬核推演

**背景：** 四位教授 `T = {'张捕头', '李捕头', '王捕头', '赵捕头'}` 被戏称为"四大名捕"，挂科率极高。
**任务：** 找出选修了某位"名捕"教授开设的**所有**硬核课程（`is_hardcore = true`）的"头铁"学生 `sid`。

**Part A — 构造除法操作数：**

设：
$$R = \pi_{sid,\, cid}\!\left(\sigma_{status=\text{'Success'}}(SelectRecord)\right)$$
$$S = \pi_{cid}\!\left(\sigma_{is\_hardcore=\text{true} \;\wedge\; teacher=T_0}(Course)\right)$$

其中 $T_0$ 为某位指定"名捕"教师。直接写出 $R \div S$ 的语义定义。

**Part B — 基础算子展开：**

利用**投影、笛卡尔积、差**，写出等价于 $R \div S$ 的完整展开式：

$$R \div S \;=\; \pi_{sid}(R) \;-\; \pi_{sid}\!\left(\left(\pi_{sid}(R) \times S\right) - R\right)$$

逐步解释每个子表达式的含义：
- $\pi_{sid}(R) \times S$：所有学生与所有硬核课程的"应修"组合
- $(\pi_{sid}(R) \times S) - R$：学生**缺选**的课程组合
- 最外层差：排除存在"漏选"的学生

**Part C — 数据验证：**

用自拟的 3 行 $R$、2 行 $S$ 小数据集，手工验证展开式与直觉结果一致。

---

## 题3：等价性数学证明——下推选择（Push-down Selection）

**背景：** 抢课高峰期，优化器面临选择：先过滤特定课程再 Join，还是先 Join 再过滤？
**任务：** 证明以下两个表达式在关系代数上严格等价。

$$\sigma_{\theta}(R \bowtie S) \;\equiv\; \sigma_{\theta}(R) \bowtie S \quad \text{（当 } \theta \text{ 仅涉及 } R \text{ 的属性时）}$$

**证明步骤提示：**
1. 展开自然连接的定义：$R \bowtie S = \pi_{attrs}(\sigma_{join\_cond}(R \times S))$。
2. 利用选择对笛卡尔积的分配律：$\sigma_{\theta \wedge \phi}(R \times S) = \sigma_{\theta}(R) \times \sigma_{\phi}(S)$（当 $\theta$ 仅含 $R$ 属性，$\phi$ 仅含 $S$ 属性时）。
3. 推导等式两端均可化简到同一表达式。
4. 结合本场景定性分析：若 $\sigma$ 过滤出选课成功的记录（占总记录的 5%），下推后中间关系规模缩减了多少倍？为什么这对内存和 I/O 都至关重要？
