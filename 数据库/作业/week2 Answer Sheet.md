10235101405 王海光
## 第一题： 关系代数精确追踪

1. 列出自然连接 $Student \bowtie SelectRecord$ 的完整中间表（展示所有属性列）。
![](http://cdn.sealight.site/notes/202603151227822.png)

2. 在中间表上施加选择条件 $\sigma_{status=\text{'Success'}}$，保留满足条件的行
![](http://cdn.sealight.site/notes/202603151228173.png)

3. 最终对 $name$ 属性投影，给出"抢课成功"学生名单。
![](http://cdn.sealight.site/notes/202603151241286.png)

若同一学生同时出现在多门成功记录中，投影后结果集将消除重复元组。根据关系模型，Relation 是 the set of tuples ， 要求不能有一样的 tuple 出现，我们在投影消除一些 attributes 之后，可能会产生一样的 tuples，这时候就需要去重。

---

## 第二题：除法算子（Division）硬核推演

**Part A — 构造除法操作数：**
语义定义：$R \div S = \{ sid \mid \forall\, cid \in S,\ (sid, cid) \in R \}$
即成功选修了指定老师 T₀ 的所有硬核课程的学生

**Part B — 基础算子展开：**
$R \div S \;=\; \pi_{sid}(R) \;-\; \pi_{sid}\!\left(\left(\pi_{sid}(R) \times S\right) - R\right)$

**Part C — 数据验证：**
![](http://cdn.sealight.site/notes/Pasted%20image%2020260315121927.png)
手工验证展开式与直觉结果一致。
（这里其实答案不太对，没有做去重，不过我也懒得改了...）

---

## 第三题： 等价性数学证明——下推选择（Push-down Selection）

$$
\begin{align}
\sigma_{\theta}(R \bowtie S) &=\sigma_{\theta}(\pi_{attrs}(\sigma_{join\_cond}(R \times S))) \\
&=\pi_{attrs}(\sigma_{\theta\land \;{join\_cond}}(R \times S)) \tag{1}
\end{align}
$$
由于 （$\theta$ 仅涉及 R 的属性），所以：
$$
\sigma_\theta(R \times S) = \sigma_\theta(R) \times S
$$
代入（1）式有：
$$
\sigma_{\theta}(R \bowtie S)=\pi_{attrs}(\sigma_{{join\_cond}}(\sigma_\theta (R )\times S))
$$
再根据自然连接的定义，即有：
$$
\sigma_{\theta}(R \bowtie S) \;\equiv\; \sigma_{\theta}(R) \bowtie S
$$

对于第四小问，我们根据常识可以推定：选课记录表 R 一般是很大的，而课程表 S 是较小的。
如果我们不做下推，即先进行 $R\times S$ 笛卡尔积的结果会很大，可能会爆内存。而下推之后，先 $\sigma_\theta(R)$ 将规模缩至 0.05 × |R|，再与 S 做连接 → 中间关系规模大约是 0.05 × |R| × |S|。  缩减倍数 = 20 倍（1 / 0.05）。
抢课高峰期数据库并发量极高，20 倍中间结果意味着临时文件/缓冲区体积减少 80%，避免了大量磁盘 I/O（排序、外存连接算法的写盘开销），避免内存占用过大的问题，所以对内存和 I/O 都至关重要。

![](http://cdn.sealight.site/notes/202603161430214.png)