# Overview of Uplift Modeling
---
### Overview
有多种方法可以评估**ACE(平均因果效应)**或**ITE(个体处理效应)**，这里我们关注的是ITE估计，即利用Uplift Modeling的方法，为每个样本(如用户)**分别估计干预/不干预时的结果**，得到**treatment(干预)**相对**control(不干预)**的**uplift(增量)值**。

增量建模有多种分类方法，既可以按照对增量的直接建模和间接建模分，也可以依据实现方式分为Meta-Learning和Tree-Based等。

各类方法大体上可以分为4种来介绍：
- [The Class Transformation Method](./2_The_Class_Transformation_Method.md)
- [Meta-Learning Method](./3_Meta_Learning_Method.md)
- [Tree-Based Method](./4_Tree_Based_Method.md)
- [NN-Based Method](./5_NN_Based_Method.md)


### Uplifting增益模型的表示
假设有$N$个用户，$Y_i(W=1)$表示我们对用户$i$干预($W=1$)后的结果，比如给用户$i$发放优惠券后（干预）用户下单（结果），$Y_i(0)$表示没有对用户干预的情况下用户的输出结果，比如没有给用户$i$发放优惠券（干预），用户下单（结果）。用户$i$的因果效应的计算如下：

$$
\operatorname{ITE}_{i}: \tau_i=Y_{i}(W=1)-Y_{i}(W=0)
$$

增益模型的目标就是最大化$\tau_i$，这是一个增量，即有干预策略相对于无干预策略的提升，简单讲就是干预前后结果的差值。**实际使用时会取所有用户的因果效应期望的估计值来衡量整个用户群的效果**，称为条件平均因果效应（Conditional Average Treatment Effect, CATE）:
$$
\operatorname{CATE}: \tau\left(X_{i}\right)=E\left[Y_{i}(W=1) \mid X_{i}\right]-E\left[Y_{i}(W=0) \mid X_{i}\right]
$$

上式中$X_i$是用户$i$的特征，所谓的conditional指基于用户特征。

上式是理想的uplift计算形式，实际上，对用户$i$我们不可能同时观察到使用**策略（treatment）** 和 **未使用策略（control）** 的输出结果，即不可能同时得到$Y_i(W=1)$和$Y_i(W=0)$，因为对某个用户，我们要么发优惠券，要么不发，只可能观察到一个结果。



### 参考文献

[1] Causal Inference and Uplift Modeling A review of the literature

[2] Metalearners for estimating heterogeneous treatment effects using machine learning

[3] Quasi-Oracle Estimation of Heterogeneous Treatment Effects

[4] Adapting Neural Networks for the Estimation of Treatment Effects

[5] Decision trees for uplift modeling with single and multiple treatments

[6] Estimation and Inference of Heterogeneous Treatment Effects using Random Forests

[7] Uplift Modeling with Multiple Treatments and General Response Types

[8] Causal ML
