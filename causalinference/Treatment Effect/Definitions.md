# Definitions
---

**treatment**，就是我们感兴趣的那个因，比如我们研究给用户发优惠券对购买转化率的影响，那么不发券（W=0），发券（W=1）。

通常用$W$代表策略（处理/Treament）

### 因果效应估计指标

在总体水平上估计treatment（W）的处理效应，Treatment Effect为**ATE(average treatment effect)**，ATE is the expectation of ITE over the whole population i=1,...,n

$$
\mathrm{ATE}=\mathbb{E}[\mathrm{Y}(W=1)-\mathrm{Y}(W=0) ]
$$

对个体individual而言，估计每个人/用户的处理效应，Treatment Effect为**ITE (Individual treatment effect)**

$$
\operatorname{ITE}_{i}=Y_{i}(W=1)-Y_{i}(W=0)
$$


介于两者中间的，对一个子群体，处理效应即为**CATE(conditional average treatment effect)**


$$
\mathrm{CATE}=\mathbb{E}[\mathrm{Y}(W=1) \mid X=x]-\mathbb{E}[\mathrm{Y}(W=0) \mid X=x]
$$

ITE其实就是CATE的变种，只不过将subpopulation缩小到了一个人。


### 对应场景
一般按照我们的目标角度和层次选择指标和估计目标。

#### 估计ATE
估计ATE的作用是做一些宏观的决策，或者对于整体population是否施加treatment做一些决策。举个例子，我们想要评估打疫苗对病变的效果，我们要评估一个overall的疫苗效应，这个时候我们去预估ATE就够了。


#### 估计ITE（CATE）
当整个population是**heterogeneous**的时候，即人群有异质性的时候，ATE可能会误导结论。举个例子，我们衡量大众点评评分对餐馆的销量影响的时候，ATE可能会误导，因为大城市的餐馆可能会更多被大众点评影响，小城市或农村可能影响更小。这时候其实我们要评估的每一个subpopulation的ATE，也即CATE（或者细粒度到每个individual的ITE）。那么我们怎么去定义各个subpopulation呢？就是靠除了treatment t之外的其他特征X，每一组X的取值就代表了一个subpopulation。

Overall，互联网业界主要以处理**CATE**为主，因为用户具有异质性heterogeneous，我们希望关注个体效果的提升或处理效果。而一些人文社科医学研究，研究某些政策、治疗方法，想评估是否对人群总体有效或效果是多少，所以关注的是**ATE**。