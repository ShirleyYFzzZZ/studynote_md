

**1.定义目标函数**
XGBoost的目标函数由训练损失和正则化项两部分组成，拟合第K棵树时，目标函数如下：
$$
Obj=\sum_{i=1}^{n} l\left(y_{i}, \hat{y}_{i}\right)+\sum_{k=1}^{K} \Omega\left(f_{k}\right)
$$

其中$\sum_{i=1}^{n} l\left(y_{i}, \hat{y}_{i}\right)$为第K棵树的预测值和真实值之间的loss，


$$
\begin{aligned}
O b j^{(t)} &=\sum_{i=1}^{n} l\left(y_{i}, \hat{y}_{i}^{(t)}\right)+\sum_{i=1}^{t} \Omega\left(f_{i}\right) \\
&=\sum_{i=1}^{n} l\left(y_{i}, \hat{y}_{i}^{(t-1)}+f_{t}\left(x_{i}\right)\right)+\Omega\left(f_{t}\right)
\end{aligned}
$$