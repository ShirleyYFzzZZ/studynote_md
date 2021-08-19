# Framework
---

### 因果效应估计框架（观测数据下）

1. **D.B Rubin的Rubin Causal Model (RCM) -- 常用理论**
- **基于Potential Outcome Framework**，更加简单直观，统计和社科用的多。它设想与观测相悖情况，是一种**反事实因果**，被称为Experimental causality（但其一般回答干预层的问题）。因果分析步骤主要有 1. 定义问题构建粗粒化因果图 2. Do-Calculus（干预）基于概率计算效应

2. **基于Judea Pearl的结构因果模型(SCM)**
- Pearl提出**小图灵测试**是实现真正智能的必要条件（机器如何迅速访问必要信息、正确回答问题，输出因果知识）。并提出因果推理引擎，以假设（图模型）、数据和Query输入，输出Estimand（基于do-calculus判断query是否可识别）、Estimate（概率估计）和Fit Indices（评估）。
- 其中do-calculus是判断因果问题是否可解的前提，原理就是贝叶斯网络中D-seperation（图分离与概率独立等价条件，参考PRML）
- **一般回答反事实问题需要SCM模型**，由图模型（表示因果知识）、反事实和干预逻辑（形式化问题）和结构方程（链接因果知识【图模型】和因果问题【反事实和干预逻辑】的语义）组成。一般步骤为 1. abduction（基于现有事实分布【先验】p(u|e)更新图概率p(u)） 2. action（基于结构方程更新x） 3. prediction（预测反事实）

**统计估计的主要困难是数据缺失，如何去除数据产生的偏差（Debias）是核心主题。Pearl提出解决混杂偏差、选择偏差和迁移学习方法（数据本身特点导致）**
