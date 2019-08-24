### SVM
* Support Vector Machine —— 支持向量机
* 分类方法，有监督的学习模型

#### SVM 工作原理
* 例：将二维平面转为三维空间，分割曲线变为平面，该平面称为超平面。\
![](https://github.com/YubinLiu/GeekTime_DataAnalysis/tree/master/img/二维平面曲线分割.jpg)
![](https://github.com/YubinLiu/GeekTime_DataAnalysis/tree/master/img/三维空间平面分割.jpg)
* SVM 计算的过程是帮我们找到超平面的过程，超平面即为 SVM 分类器。

#### 分类间隔
* 找到两个极限位置，决策面 A 和 B，即越过该位置就会产生分类错误。
* 极限位置到最优决策面 C 的距离，为分类间隔，margin。
* 拥有最大间隔（max margin）的决策面就是 SVM 要找的最优解。\
![](https://github.com/YubinLiu/GeekTime_DataAnalysis/tree/master/img/最大间隔.jpg)

#### 点到超平面的距离公式
* 超平面的数学表达式 \
![](https://github.com/YubinLiu/GeekTime_DataAnalysis/tree/master/img/平面数学表达式.png)
  * w 是法向量（垂直于平面的直线所表示的向量，决定超平面的方向），x 是函数变量。
* di 代表点到超平面 wxi + b = 0 的欧氏距离，求 di 最小值。\
![](https://github.com/YubinLiu/GeekTime_DataAnalysis/tree/master/img/欧氏距离.png)
  * ||w|| 为超平面的范数

#### 最大间隔的优化模型
凸优化问题，求出最优的 w 和 b。

#### 硬间隔、软间隔和非线性 SVM
* 硬间隔：完全分类准确，不能存在分类错误的情况。
* 软间隔：允许一定量的样本分类错误。
* 非线性 SVM
  * 核函数：将样本从原始空间映射到一个更高维的特质空间中，使得样本在新的空间中线性可分。
  * 常见的核函数：线性核、多项式核、高斯核、拉普拉斯核、sigmoid 核，或它们的组合。

#### 使用 SVM 解决多分类问题
SVM 本身是一个二值分类器，可组合起来形成一个多分类器。
1. 一对多：将物体分为 A B C D 四类，将其中的一类作为分类 1，其余的作为分类 2。
  * 样本 A 为正集，B C D 为负集。
  * 样本 B 为正集，A C D 为负集。
  * 样本 C 为正集，A B D 为负集。
  * 样本 D 为正集，A B C 为负集。
* *针对 K 个分类需要 K 个分类器，分类速度快，训练速度慢。*
* *每个分类器都需要对全部样本进行训练，负样本数量远大于正样本数量，造成样本不对称的情况。*
* *增加新的分类，如 K+1 类，需重新构造*

2. 一对一：在任意两个样本之间构建 SVM，对 K 类样本，有 C(K,2) 类分类器。如有 A B C 三个分类。
  * 分类器 1：A B。
  * 分类器 2：A C。
  * 分类器 3：B C。
* *当一个未知样本进行分类时，每个分类器都有一个分类结果，即为 1 票，最终得票最多的是整个未知样本的类别。*
* *新增一类，不需要重新训练所有的 SVM*
* *分类器个数与 K 的平方成正比，K 较大，测试和训练时间较慢。*
