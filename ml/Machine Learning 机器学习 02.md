# Machine Learning 机器学习 02



## Model performance assessment 模型性能评估 

在ML中，有许多指标去评判模型的性能



### Metrics in classification 分类指标

- **confussion matrix** 混淆矩阵
- **precision** 精确率
- **recall** 召回率
- **accuracy** 准确性



#### Confussion matrix

> 一种 NxN 表格，用于总结 `分类模型`的预测效果；即标签和模型预测的分类之间的关联。
>
> 在混淆矩阵中，一个轴表示模型预测的标签，另一个轴表示实际标签。N 表示类别个数。在 `二元分类` 问题中，N=2。

例如：

|                  | spam(predicted) | not-spam(predicted) |
| ---------------- | --------------- | ------------------- |
| spam(actual)     | 23 (TP)         | 1 (FN)              |
| not-spam(actual) | 12 (FP)         | 556 (TN)            |

P (Postive）：spam

N (Negative) : Not-spam

上述的混淆矩阵表示：

- 在24个垃圾邮件的样本中，该模型**正确预测**23个垃圾样本（正确且为正例 TP），1个**预测错误**认为是非垃圾邮件（错误且为负例 NP）
- 在568个正常邮件的样本中，该模型**正确预测**556个是正常邮件（TN）, 12个**预测错误**认为是垃圾邮件（FP）



#### Precision

模型在 `预测` **正类**时正确的频率
$$
precision = \frac {TP} {TP + FP}
$$
例如，根据上述混淆矩阵，可以得出
$$
precision = \frac {TP} {TP + FP} = \frac {23} {23+12} ≈ 0.657
$$

#### Recall 

模型在 `所有正类中`，预测正例正确正确的频率
$$
recall = \frac { TP } { TP + FN }
$$
例如，根据上述混淆矩阵，可以得出
$$
recall = \frac {TP} {TP + FN} = \frac {23} {23 + 1} ≈ 0.958
$$

#### Accuracy

模型在所有正类反类中，预测正确的占比
$$
accuray = \frac {TP + TN} {TP + TN + FP + FN}
$$
