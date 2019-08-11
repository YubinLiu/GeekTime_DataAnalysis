### 决策树：CART
Classification And Regression Tree：分类回归树。

#### 分类树和回归树
* 分类树：处理离散数据，数据种类有限的数据，输出的是样本的类别。
* 回归树：对连续型的数值进行预测。

#### CART 分类树
* 属性选择指标：基尼系数。
  * 基尼系数：反应了样本的不确定度。越小，样本之间差异性小，不确定程度低。
  * t 为节点，p(Ck|t) 表示节点 t 属于 Ck 的概率。\
  ![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/基尼系数_公式.png)
  * 例
  ![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/基尼系数_例.jpg)
    * GINI(D1) = 0
    * GINI(D2) = 0.5
    * GINI(D, A) = D1/D * GINI(D1) + D2/D * GINI(D2)

#### CART 回归树
* 样本的离散程度
  * 差值的绝对值：|x - μ|，x 为样本的个体，μ 为均值。
  * 方差：s = 1/n * ∑(x - μ)²
  * 对应两种目标函数最优化的标准，最小绝对偏差(LAD)，最小二乘偏差(LSD)

#### CART 决策树的剪枝
* CCP(cost-complexity prune)，代价复杂度，一种后剪枝的方法，用到的指标是：节点的表面误差率增益值。\
  ![](https://github.com/YubinLiu/GeekTime_DataAnalysis/blob/master/img/CCP_误差指标.png)
  * Tt：以 t 为根节点的子树
  * C(Tt)：节点 t 的子树没被剪时子树 Tt 的误差
  * C(t)：节点 t 的子树被剪枝后节点 t 的误差
  * |Tt|：子树 Tt 的叶子数，剪枝后，T 的叶子数减少了 |Tt| - 1
* 节点的表面误差率增益值 等于 节点 t 的子树被剪枝后的误差变化 除以 剪掉的叶子数量
* 寻找最小 α 值对应的节点，剪掉，生成第一个子树。重复上述过程，继续剪枝，知道最后只剩根节点，即为最后一个子树。
