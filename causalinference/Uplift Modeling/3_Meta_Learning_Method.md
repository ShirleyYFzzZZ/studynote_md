# Meta-Learning Method
---

Meta-Learning方法是指基于Meta-Learner进行Uplift预估，其中Meta-Learner可以是任意的既有预测算法，如LR、SVM、RF、GBDT等。根据Meta-Learner的组合不同，通常分为：S-Learner、T-Learner、X-Learner、R-Learner。

【优点】：利用了既有预测算法的预测能力，方便易实现。

【缺点】：不直接建模uplift，效果打折扣。


### S-learner

“S-”是single的意思，是指使用一个预测模型完成uplift估计。具体步骤包括:

- Step1：基于变量X和干预W训练预测模型
$$
\mu(x, w):=\mathbb{E}\left[Y^{o b s} \mid X=x, W=w\right]
$$

- Step2：分别估计干预和不干预时的得分，差值即为增量
$$
\hat{\tau}_{S}(x)=\hat{\mu}(x, 1)-\hat{\mu}(x, 0)
$$

**评述：**
【优点】S-Learner简单直观、直接使用既有预测算法；预测仅依赖一个模型，避免了多模型的误差累积；更多的数据和特征工程对预测准确率有利。

【缺点】但是该方法不直接建模uplift；且需要额外进行特征工程工作(由于模型拟合的是Y，所以若W直接作为一个特征放进去，可能由于对Y的预测能力不足而未充分利用)。

【应用】在因果推断未受关注之前，诸如”优惠券“发放的问题常用该方法，直接建模”对什么人，发放什么面额券，是否会下单“，预测阶段则对User和Coupon交叉组合后进行预测，得到(User,Coupon)组合的下单率，然后再依据预算、ROI或其他约束进行MCKP求解。

### T-learner

”T-“是Two的意思，是指用两个模型分别建模干预、不干预的情况，取差值作为uplift。具体步骤：

- Step1：对treatment组数据和control组数据分别训练预测模型
$$
\begin{aligned}
&\mu_{0}(x)=\mathbb{E}[Y(0) \mid X=x] \\
&\mu_{1}(x)=\mathbb{E}[Y(1) \mid X=x]
\end{aligned}
$$

- Step2：两个模型分别打分
$$
\hat{\tau}_{T}(x)=\hat{\mu}_1(x)-\hat{\mu}_0(x)
$$

**评述：**
【优点】T-Learner一样简单直观、直接使用既有预测算法；将不同的数据集中的增量效果转换为模型间的差异，不需要太多的特征工程工作；当有随机试验的数据时该方法作为baseline很方便。

【缺点】该方法存在双模型误差累积问题；同时当数据差异过大时(如数据量、采样偏差等)，对准确率影响较大。

### X-learner

”X-“表示交叉的意思，该方法主要解决T-Learner对不同Treatment组合Control组间数据量差异过大情况表现不佳的问题。具体步骤：

- Step1：对treatment组数据和control组数据分别训练预测模型
$$
\begin{aligned}
&\mu_{0}(x)=\mathbb{E}[Y(0) \mid X=x] \\
&\mu_{1}(x)=\mathbb{E}[Y(1) \mid X=x]
\end{aligned}
$$

- Step2：计算一组uplift的近似表示的数据集，用treatment组模型预测control组数据，control组模型预测treatment组数据，分别做与Y的差值得到增量的近似

$$
\begin{aligned}
&\tilde{D}_{i}^{1}:=Y_{i}^{1}-\hat{\mu}_{0}\left(X_{i}^{1}\right) \\
&\tilde{D}_{i}^{0}:=\hat{\mu}_{1}\left(X_{i}^{0}\right)-Y_{i}^{0}
\end{aligned}
$$

然后，以此为目标再训练两个预测模型，拟合uplift：

$$
\hat{\tau}(x)=g(x)\hat{\tau}_0(x)-(1-g(x))\hat{\tau}_1(x)
$$

评述：
X-Learner在T-Learner基础上，利用了全量的数据进行预测，主要解决Treatment组间数据量差异较大的情况。但流程相对复杂、计算成本较高，有时还会由于多模型误差累积等问题效果不佳。

另外，不论是分类问题还是回归问题，在${\tau}(x)$步骤时，都需要使用回归模型来拟合。

理解：对Step3中，若W=1的比例极小，则$g(x)$极小，则$\hat{\tau}_1(x)$的权重更大，即更倾向于使用$\tilde{D}_{i}^{1}$的结果，也即倾向于control组数据训练的模型。

