# Tree-Based Method
---


### Uplift-Tree
基于树的方法，仿照标准CART树依据对信息增益的大小不断选择最优的分裂特征和分裂点，从而实现精确分层的过程。其核心思想是通过设计分裂规则，使得分裂后对uplift的预测更准确。

因为存在异质性，这里我们关注的处理效应为**条件平均处理效应**(ConditionalAverageTreatmentEffect)。这里的条件是指，**给定局部特征空间下的平均处理效应。**

即我们希望从协变量中，找到一个最优分裂节点，最大化子节点间处理效应差异，这个差异用分布散度衡量。

分布散度可以度量两个概率分布之间的差异，因此我们可以将treatment组和control组理解为两个(关于Y的)概率分布，以此为分裂依据，若分裂后差异变大，则说明这个分裂有区分能力且有益于描述Treatment对Outcome的影响。

常见的分布散度有**KL 散度 (Kullback-Leibler divergence)**、**欧式距离 (Squared Euclidean distance)** 和**卡方散度(Chi-squared divergence)**，在uplift model中，其具体计算方法如下:

$$
\begin{array}{r}
\text { Kullback }-\text { Leiblerdivergence : } K L\left(P^{T}(Y): P^{C}(Y)\right)=\sum_{y} P^{T}(y) \log \frac{P^{T}(y)}{P^{C}(y)} \\
\text { SquaredEuclideandistance : } E\left(P^{T}(Y): P^{C}(Y)\right)=\sum_{y}\left(P^{T}(y)-P^{C}(y)\right)^{2} \\
C h i-\text { squareddivergence }: \chi^{2}\left(P^{T}(Y): P^{C}(Y)\right)=\sum_{y} \frac{\left(P^{T}(y)-P^{C}(y)\right)^{2}}{P^{C}(y)}
\end{array}
$$

然后，进行分裂点的增益计算，
$$
D_{\text {gain }}(A)=D\left(P^{T}(Y): P^{C}(Y) \mid A\right)-D\left(P^{T}(Y): P^{C}(Y)\right)
$$

其中$P^T,P^C$分别表示treatment组和control组的概率分布，$D(.)$表示对分布散度的衡量，$D(.|A)$表示按照A特征进行分类后的度量，有

$$
D\left(P^{T}(Y): P^{C}(Y) \mid A\right)=\sum_{a} \frac{N(a)}{N} D\left(P^{T}(Y \mid a): P^{C}(Y \mid a)\right)
$$


最后，循环进行上述分裂，直到无增量分裂点，或达到样本量、树深度等限制为止。

**总结**：因为具有异质性，我们采用类似决策树CART的算法将不同类型（异质）的样本划分开，分布散度大，说明Treatment的影响大，Treatment Effect明显，说明划分的更好。**这个划分的过程可以认为是一个控制Confounders的过程，每分裂一次都是通过一个特征将相似的样本分在了同一个分支，叶子结点上的样本集可认为满足CIA假设**。子节点中的预测值为CATE。也就是说，每投入一个样本，模型会找到和其同质的subgroup，并给出CATE。



### Causal Forest
顾名思义，类比RandomForest，CausalForest是指由多个CausalTree模型融合得到的Forest模型，而对于CausalForest，这里可以是任意单Tree-Based方法。相比普通RF，只是换了子树，其他无特殊变化。

$$
\begin{aligned}
\hat{\tau}(x)=& \frac{1}{\left|\left\{i: W_{i}=1, X_{i} \in L\right\}\right|} \sum_{\left\{i: W_{i}=1, X_{i} \in L\right\}}^{Y_{i}} \\
&-\frac{1}{\left|\left\{i: W_{i}=0, X_{i} \in L\right\}\right|} \sum_{\left\{i: W_{i}=0, X_{i} \in L\right\}}^{Y_{i}}
\end{aligned}
$$

基于不同的样本子集训练多个CausalTree，用均值作为最终的结果

$$
\begin{aligned}
&\hat{\tau}(x)=B^{-1} \sum_{b=1}^{B} \hat{\tau}_{b}(x)
\end{aligned}
$$