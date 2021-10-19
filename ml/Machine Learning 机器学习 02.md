# Machine Learning 机器学习 02



## Model performance assessment 模型性能评估 

在ML中，有许多指标去评判模型的性能



### Metrics in classification 分类模型的指标

- **confussion matrix** *混淆矩阵*
- **precision** *精确率*
- **recall** *召回率*
- **accuracy** *准确性*



#### Confussion matrix

> 一种 NxN 表格，用于总结 `分类模型`的预测效果；即标签和模型预测的分类之间的关联。
>
> 在混淆矩阵中，一个轴表示模型预测的标签，另一个轴表示实际标签。N 表示类别个数。在 `二元分类` 问题中，N=2。

例如：

|                  | spam(predicted) | not-spam(predicted) |
| ---------------- | --------------- | ------------------- |
| spam(actual)     | 23 (TP)         | 1 (FN)              |
| not-spam(actual) | 12 (FP)         | 556 (TN)            |

*P (Postive）：spam*

*N (Negative) : Not-spam*

上述的混淆矩阵表示：

- 在24个垃圾邮件的样本中，该模型**正确预测**23个垃圾样本（正确且为正例 TP），1个**预测错误**认为是非垃圾邮件（错误且为负例 NP）
- 在568个正常邮件的样本中，该模型**正确预测**556个是正常邮件（TN）, 12个**预测错误**认为是垃圾邮件（FP）



**Python代码**

```python
from sklearn.metrics import confusion_matrix
from random import choice

label = ['spam','not-spam']

# actual: ['spam', 'not-spam', 'not-spam', 'spam', 'not-spam', 'not-spam', 'spam', 'not-spam', 'spam', 'not-spam'] 
actual = [ choice(label)  for i in range(10)]
# pred: ['not-spam', 'not-spam', 'spam', 'spam', 'spam', 'not-spam', 'not-spam', 'spam', 'spam', 'spam']
pred = [  choice(label)  for i in range(10)]

confusion_matrix(actual,pred,labels=label)
"""
array([[2, 2],
       [4, 2]])
"""
```

#### Precision

模型在**`预测为正类时`** **( TP+FP )** 正确的频率
$$
precision = \frac {TP} {TP + FP}
$$
例如，根据上述混淆矩阵，可以得出
$$
precision = \frac {TP} {TP + FP} = \frac {23} {23+12} ≈ 0.657
$$



**Python实现**

```python
from sklearn.metrics import precision_score

actual = ['spam', 'not-spam', 'not-spam', 'spam', 'not-spam', 'not-spam', 'spam', 'not-spam', 'spam', 'not-spam']
pred = ['not-spam', 'not-spam', 'spam', 'spam', 'spam', 'not-spam', 'not-spam', 'spam', 'spam', 'spam']

precision_score(actual, pred, pos_label= 'spam')

"""
0.3333333333333333
"""
```



#### Recall 

模型在**`所有正类中` ( TP + FN )**，预测为 **正例且正确 ( TP )** 的频率
$$
recall = \frac { TP } { TP + FN }
$$
例如，根据上述混淆矩阵，可以得出
$$
recall = \frac {TP} {TP + FN} = \frac {23} {23 + 1} ≈ 0.958
$$

**Python实现**

````python
from sklearn.metrics import recall_score

actual = ['spam', 'not-spam', 'not-spam', 'spam', 'not-spam', 'not-spam', 'spam', 'not-spam', 'spam', 'not-spam']
pred = ['not-spam', 'not-spam', 'spam', 'spam', 'spam', 'not-spam', 'not-spam', 'spam', 'spam', 'spam']

recall_score(actual, pred, pos_label= 'spam')

"""
0.5
"""
````



> #### 关于 Precision 和 Recall
>
> 1. `precision` 和 `recall` 的分子都是 TP, 即正确识别出的目标类别。也就是说，这两个指标都是用在目标类别上的，并围绕着正确识别出的目标类别转的。
> 2. `precision` 强调的是**在预测为Positive的所有数据中** (TP+FP), 有多少本来是postive的 (TP)
> 3. `recall` 强调的是**事实上本身就是Postive的所有数据中** (TP+FN), 有多少本来是postive(TP)
> 4. 当**False Negative (FN)的成本代价很高时**，我们希望能尽量避免产生FN，这时候我们要**着重考虑提高Recall指标**
> 5. 当**False Postive (FP)的成本代价很高时**，**尽可能提高Precision值**，哪怕牺牲一部分recall



#### Accuracy

模型在**`所有类中` ( TP + TN + FP + FN )**，**`预测正确的类` (TP + TN)** 占比
$$
accuracy = \frac {TP + TN} {TP + TN + FP + FN}
$$


例如，根据上述混淆矩阵，可以得出
$$
accuracy = \frac {TP + TN} {TP + TN + FP + FN} = \frac {23 + 556}{ 23+1+556+12} ≈ 0.978
$$
**Python实现**

```python
from sklearn.metrics import accuracy_score

actual = ['spam', 'not-spam', 'not-spam', 'spam', 'not-spam', 'not-spam', 'spam', 'not-spam', 'spam', 'not-spam']
pred = ['not-spam', 'not-spam', 'spam', 'spam', 'spam', 'not-spam', 'not-spam', 'spam', 'spam', 'spam']

accuracy_score(actual, pred)

"""
0.4
"""
```



> 1. `accuracy` **不管分类目标**，只用于计算**每一个分类正确的数量**与总数量的百分比
> 2. **对于不平衡数据集而言, accuracy并不是一个好指标。**会给人一种分类很好的错觉，但其实分类器或许并没有作用



### Metrics in regression 回归模型中的指标

- root mean squared error (**RMSE**) *均方根误差*
- mean absolute error (**MAE**) *平均绝对误差*



#### RMSE

$$
RMSE = \sqrt{\frac {1}{n} \sum_{i=1}^n {(\hat{y_i} - y_i)^2}}
$$

这个是开根号的MSE, **当预测值与真实值完全吻合时等于0**。例如RMSE = 10, 可以认为是**回归效果**相比真实值**平均相差**10

**Python代码**

```python
import numpy as np
from sklearn.metrics import mean_squared_error

# assume that y_true is the truth dataset, y_pred is the prediction dataset
np.sqrt(mean_squared_error(y_true, y_pred))
```



#### MAE

$$
MAE = \frac {1}{n} \sum_{i=1}^n {\abs {\hat{y_i}-y_i}}
$$

范围为 [0, +∞]，**当MAE = 0, 表示预测值与真实值吻合**；同样，如果误差越大，该值越大

**Python代码**

```python
import numpy as np
from sklearn.metrics import mean_absolute_error

# assume that y_true is the truth dataset, y_pred is the prediction dataset
mean_absolute_error(y_true, y_pred)
```











# 参考

1. https://zhuanlan.zhihu.com/p/37246394
2. https://zhuanlan.zhihu.com/p/147663370
3. https://ailearning.apachecn.org/#/docs/ml/1

