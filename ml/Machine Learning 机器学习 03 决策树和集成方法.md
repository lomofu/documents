# Machine Learning 机器学习 03 决策树和集成方法

> 树模型（tree-based model）遵循 “分而治之”的思路，以递归方式将特征空间划分为若干个矩形的区间，再在每一个区间上你和一个简单的模型。而在分类问题中，这个简单模型即是类别的标签选择。决策树( Decision Tree) 算法是一种**基本的分类与回归方法**，如果在意模型的可解释性，决策树分类器绝对是最好的选择。我们可以把它理解成基于一系列问题对数据做出的分割选择。



例：一个邮件分类系统其决策树形式

<img src="http://image.woshipm.com/wp-files/2020/06/hNl8wKqMsdsAWrjkJM6u.png" alt="img" style="zoom:67%;" />

## Definition 定义

### 一个决策树的组成

- 根节点 root node / starting node 【包含样本的全集】
- 内部节点 internal nodes 【对应特征属性测试】
- 叶结点 leaf node / terminating nodes 【代表决策的结果】

> - **非叶子节点 non-leaf nodes** ( 内部节点 internal nodes 或者 根节点 root node ) 表示**特征或者属性 (features**)
>
> - **叶节点 leaf nodes** 表示一个类 (labels)



### 使用决策树对需要测试的实例进行分类

1. 从根节点开始，对实例某一个特征进行测试，根据测试得到的结果，将实例分配到其对应的子节点
2. 这时，每一个子节点都对应该特征的一个取值
3. 如此**递归**对实例进行测试分配
4. 直至达到叶结点，最后将其分配到叶结点的类中

这是一种基于 if-then-else 规则的有监督学习算法，决策树的这些规则是通过训练得到的，而不是人工制定的。





## 熵 Entropy

熵是对正在处理的信息中随机性的一种度量（即随机变量不确定性的度量）。**熵越高，从该信息得出结论就越难。**



**概率与熵 Probability and entropy**

- 高概率 -> 低熵
- 低概率 -> 高熵 (信息量越大)



**公式**

假如一个随机变量X的取值是 $X = \{{x_1,x_2,x_3, ..., x_n}\} $, 每一种概率的分别是 ${\{ p_1,p_2,p_3,..., p_n \}}$，那么X的熵定义为
$$
H(X) = - \sum_i^n {(P(X=i) \times{log_2 (P(X=i))})}
$$

***一个事情它的随机性越大就越难预测***



> 例如 ： 对于52张扑克牌来说，熵是多少？
> $$
> P(card = i) =\frac {1} {52}=  0.019 \\
> H(card) = -\sum_{i=1}^{52}{(P(card=i) \times{log_s (P(card=i))})} \\
>  = -\sum_{i=1}^{52}{0.019 \times{log_2 (0.019)})} \\
>  = -\sum_{i=1}^{52}{-0.1096} \\
>  = 5.700 \space bits
> $$

**当Entropy最大为1的时候，是分类效果最差的状态，当它最小为0的时候，是完全分类的状态。因为熵等于零是理想状态，一般实际情况下，熵介于0和1之间。**



## 条件熵 Conditional Entropy



<img src="img/03/image-20211028153445919.png" alt="image-20211028153445919" style="zoom:67%;" />



$H(D|A) $ ：使用某个特**征A划**分**数据集D**
$$
H(D | A) = - \sum_{a \in A} {p(x)} \sum_{ d \in D} {p(d|a)} \ log \ p(d|a)
$$

> 将 Supsicious words、Unknown sender、Contains images 代表为 {A, B, C}
>
> 根据题意，得出**D=6**，**A=Suspicious words**
> $$
> H(D|A)  = - \sum_{a \in A} {p(a)} \sum_{ d \in D} {p(d|a)} \ log \ p(d|a) \\
>  = - [(\frac {3}{6} \times (\frac {3}{3} \times log_2(\frac{3}{3}) + \frac {0}{3} \times log_2(\frac {0}{3}))) + \frac{3}{6} \times (\frac{3}{3} \times log_2(\frac {3}{3}) + \frac{0}{3} \times log_2(\frac{0}{3}))] \\
>  = 0 bits
> $$





## 信息增益 / 互信息 Information Gain

信息增益表示在添加信息后能减少多少不确定性。

**定义**

特征A对数据集D的信息增益 $IG(D,A)$ 定义为集合D的**经验熵H(D)**与特征A给定的条件下D的**经验熵H(D|A)**之差
$$
IG(D,A) = H(D) - H(D|A)
$$



> 根据条件熵中给定的例子,将 Supsicious words、Unknown sender、Contains images 代表为 {A, B, C}
>
> 根据题意，得出**D=6**，A=Suspicious words, 通过计算可以得到 
> $$
> H(D) = -\sum_{i \in \{ 'spam' ,\ 'ham' \}} (P(i) \times log_2 {P(i)}) \\
> = - ((P(i='spam') \times log_2(P(i='spam'))) + P(i='ham' \times log_2(P(i='ham')))) \\
> = - ((\frac {3}{6} \times log_2(\frac{3}{6})) +(\frac {3}{6} \times log_2(\frac{3}{6}))　\\
> ＝　1 bit
> $$
> 
> $$
> H(D|A) = - \sum_{a \in A} {p(a)} \sum_{ d \in D} {p(d|a)} \ log \ p(d|a) \\
> = - [(\frac {3}{6} \times (\frac{3}{3} \times log_2(\frac {3}{3})+ (\frac{0}{3}) \times log_2(\frac{0}{3})) + \frac{3}{6} \times (\frac{3}{3} \times log_2(\frac{3}{3})+\frac{0}{3} \times log_2(\frac{0}{3}))]
> = 0 bits
> $$
> 因此
> $$
> IG(D|A) = 1	bits - 0 bits = 1 bits
> $$



## 处理连续性特征 Handling continuous features

### 将连续性特征转换为布尔特征

- 对于连续性特征，我们可以定义一个 `阈值 Threshold` 把它们变成布尔特征
- `阈值 Threshold` 用来对连续性特征值的实例进行划分

### 排序实例 Sorting the instances

- 根据连续性特征的值将数据集中的实例进行排序
- 在排序中，具有不同分类的相邻则被选为可能的阈值点
- 最优的阈值点可以通过计算每一个分类过渡边界的 `信息增益 Information Gain` 并选择最高 `信息增益 Information Gain`的边界作为阈值



### 将特征作为分类特征 Treat the feature as a categorial feature

- 一旦设置了阈值，动态创建的新布尔特征可以与其他分类特征竞争，作为该节点的分割特征
- 在树的生成过程中，这一过程可以重复在每个节点中进行

 

### 例子 Example

给定数据集如下：

![image-20211030224312486](img/03/image-20211030224312486.png)

1. Sorting the instances

根据连续型特征 **`Elevation`**排序，得到

![image-20211030225027185](img/03/image-20211030225027185.png)



2. Thresholds and partitions

例如根据阈值 ≥750, ≥1 350, ≥2 250 和 ≥4 175.

以≥750为例子

可以分为：

| Threshold | Part                   |
| --------- | ---------------------- |
| ≥750      | d2,                    |
| ≥750      | d4, d3, d7, d1. d5, d6 |

> $$
> H(D) = - (\sum_{l \in \{ 'riparian','chapparal', 'conifer'\}} P(i=l) \times log_2(P(i=l))) \\
> = - (\frac {2}{7} \times log_2(\frac {2}{7})+ \frac {3}{7} \times log_2(\frac {3}{7}) + \frac {2}{7} \times log_2(\frac {2}{7})) \\
> ≈ 1.55665
> $$
>
> $$
> H(D | threshold ≥750) = \sum_{a \in {threeshold ≥750}} P(i=a) \sum_{d \in \{'riparian','chapparal', 'conifer'\}} P(d|a)*log_2(P(d|a)) \\ = - (\frac {1}{7} \times ((\frac{1}{1} \times log_2(\frac{1}{1})+\frac {0}{1} \times log_2(\frac {0}{1})+\frac {0}{1} \times log_2(\frac {0}{1}))+ \\
> \frac{6}{7} \times ((\frac{3}{6} \times log_2(\frac{3}{6})+\frac{1}{6} \times log_2(\frac{1}{6})+\frac{2}{6} \times log_2(\frac {2}{6}))) \\
> ≈ 1.25069
> $$
>
> $$
> IG(D|threshold ≥750) = H(D) - H(D|threshold ≥750) = 0.3060
> $$

以此类推，可以得到

![image-20211030233505648](img/03/image-20211030233505648.png)



## Growing a tree决策树生成

### ID3 Algorithm (Iterative Dichotomiser 3)

它利用**信息增益 `Information Gain`**来选取特征。 创建一个多路树，找出每个节点（即以贪心的方式）分类特征，这会产生分类目标的最大信息增益。决策树发张大最大的尺寸，通常利用剪枝来提高数对未知数据的泛化能力。

### C4.5 Algorithm

是ID3的改进版，不直接使用信息增益，而是引入了”信息增益“指标作为特征的选择依据。

### CARD (Classification and Regression Trees)

此算法可以用与分类与回归两种问题。**CART 算法使用了基尼系数取代信息熵模型。**

*scikit-learn 使用 CART 算法的优化版本。*

### 三者比较

| 算法 | 支持模型   | 树结构 | 特征选择                  | 连续值处理 | 缺失值处理 | 剪枝 |
| ---- | ---------- | ------ | ------------------------- | ---------- | ---------- | ---- |
| ID3  | 分类       | 多叉树 | 信息增益 Information Gain | No         | No         | No   |
| C4.5 | 分类       | 多叉树 | 信息增益比                | Yes        | Yes        | Yes  |
| CART | 分类，回归 | 二叉树 | 基尼指数，均方差          | Yes        | Yes        | Yes  |



## Regression trees 回归树











# 参考

1. https://ljalphabeta.gitbooks.io/python-/content/tree.html
2. https://ailearning.apachecn.org/#/docs/ml/3
3. https://easyai.tech/ai-definition/decision-tree/
4. https://time.geekbang.org/column/article/12258?utm_term=pc_interstitial_1269
5. https://www.jiqizhixin.com/articles/2020-06-09
6. http://www.woshipm.com/pmd/3993547.html
7. https://blog.csdn.net/zhangyingjie09/article/details/85639641
8. https://cloud.tencent.com/developer/article/1475554
9. https://zhuanlan.zhihu.com/p/82054400