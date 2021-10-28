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

熵是对正在处理的信息中随机性的一种度量。**熵越高，从该信息得出结论就越难。**



概率与熵 Probability and entropy

- 高概率 -> 低熵
- 低概率 -> 高熵 (信息量越大)

$$
H(t) = - \sum {(P(t=i) \times{log_s (P(t=i))})}
$$

***一个事情它的随机性越大就越难预测***

> 例如 ： 对于52张扑克牌来说，熵是多少？
> $$
> P(card = i) =\frac {1} {52}=  0.019 \\
> 所以, H(card) = -\sum_{i=1}^{52}{(P(card=i) \times{log_s (P(card=i))})} \\
> = -\sum_{i=1}^{52}{0.019 \times{log_2 (0.019)})} \\
> = -\sum_{i=1}^{52}{-0.1096} \\
> = 5.700 \space bits
> $$







## 决策树学习的 3 个步骤

特征选择



决策树生成



决策树剪枝



## 参考

1. https://ljalphabeta.gitbooks.io/python-/content/tree.html
2. https://ailearning.apachecn.org/#/docs/ml/3
3. https://easyai.tech/ai-definition/decision-tree/
4. https://time.geekbang.org/column/article/12258?utm_term=pc_interstitial_1269
5. https://www.jiqizhixin.com/articles/2020-06-09
6. http://www.woshipm.com/pmd/3993547.html