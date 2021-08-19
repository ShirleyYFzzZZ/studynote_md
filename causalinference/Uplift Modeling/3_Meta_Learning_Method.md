# Meta-Learning Method
---

Meta-Learning方法是指基于Meta-Learner进行Uplift预估，其中Meta-Learner可以是任意的既有预测算法，如LR、SVM、RF、GBDT等。根据Meta-Learner的组合不同，通常分为：S-Learner、T-Learner、X-Learner、R-Learner。

【优点】：利用了既有预测算法的预测能力，方便易实现。

【缺点】：不直接建模uplift，效果打折扣。