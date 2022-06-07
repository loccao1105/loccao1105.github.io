---
title: 'Papers for Statistics & Optimization (2021)'
date: 2021-12-31
excerpt: "My reading list for <b>statistics & optimization</b> papers in 2021."
permalink: /read-list/2021/statistics-optimization/
tags:
  - Bayesian Method
  - Sparse Optimization
  - General LASSO
---

<hr style="height:0px;border:none;border-top:3px solid #555555;" />

My 2021 paper reading list in statistics & optimization, with special emphasis on **Bayesian method**, **sparse optimization** and **general LASSO problem**. 

<hr style="height:0px;border:none;border-top:3px solid #555555;" />

# Paper List

* [An augmented ADMM algorithm with application to the generalized lasso problem (2017, Journal of Computational and Graphical Statistics)](#jump_1)

* [The solution path of the generalized lasso (2011, The Annals of Statistics)](#jump_2)

* [Sparse Bayesian learning and the relevance vector machine (2001, Journal of machine learning research)](#jump_3)

* [Analysis of sparse Bayesian learning (2002, NIPS)](#jump_4)

* [Fast marginal likelihood maximisation for sparse Bayesian models (2003, International workshop on artificial intelligence and statistics)](#jump_5)

* [References](#jump_reference)


<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_1"> An augmented ADMM algorithm with application to the generalized lasso problem (2017, Journal of Computational and Graphical Statistics) </span>

&emsp;&emsp;本文是对经典ADMM方法的改进方法。具体来说，本文提出了新的变罚形式来加速ADMM算法，可以在保持相同收敛速度的同时降低计算量。提出的aug-ADMM可以解决广义LASSO问题，即

$$
\underset{\beta \in \mathbb{R}^{p}}{\operatorname{minimize}}(1 / 2) \cdot\|y-X \beta\|_{2}^{2}+\lambda\|A \beta\|_{1}.
$$

### **Preliminary:**

&emsp;&emsp;考虑如下形式的优化问题：

$$
\operatorname{minimize}_{\theta \in \mathbb{R}^{p}} f(\theta)+g(A \theta),
$$

其中$f(\cdot)$和$g(\cdot)$都是“简单”的凸函数，且$A\in \mathbb{R}^{m\times p}$。标准的ADMM算法首先通过引入等价约束$A \theta=\gamma$来解耦上述优化问题中的两个函数，可得到一个等价的联合优化问题，为：

$$
\operatorname{minimize}_{\theta \in \mathbb{R}^{p}, \gamma \in \mathbb{R}^{m}} f(\theta)+g(\gamma) \quad \text { subject to } A \theta-\gamma=0.
$$

&emsp;&emsp;通过分裂算子、交替优化变量，ADMM将原联合优化问题化为若干子问题，并对原变量$(\theta,\gamma)$和对应的对偶变量$\alpha$交替优化更新，得到如下更新公式：

$$
\begin{aligned}
\theta^{k+1} &=\underset{\theta \in \mathbb{R}^{p}}{\arg \min }\left(f(\theta)+\frac{\rho}{2}\left\|A \theta-\gamma^{k}+\rho^{-1} \alpha^{k}\right\|_{2}^{2}\right), \\
\gamma^{k+1} &=\underset{\gamma \in \mathbb{R}^{m}}{\arg \min }\left(g(\gamma)+\frac{\rho}{2}\left\|A \theta^{k+1}-\gamma+\rho^{-1} \alpha^{k}\right\|_{2}^{2}\right), \\
\alpha^{k+1} &=\alpha^{k}+\rho\left(A \theta^{k+1}-\gamma^{k+1}\right) .
\end{aligned}
$$

其中，$\rho > 0$表示惩罚参数，该迭代过程的收敛速率为$O(1/k)$。

### **aug-ADMM:**

&emsp;&emsp;然而，上述ADMM的$\theta$-更新过程偶尔会难以计算，特别是当$A$是非对角阵时。为了克服这个困难，作者提出引入“增广”变量$(\gamma, \tilde{\gamma})$，其中






<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_2"> The solution path of the generalized lasso (2011, The Annals of Statistics) </span>



<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_3"> Sparse Bayesian learning and the relevance vector machine (2001, Journal of machine learning research) </span>



<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_4"> Analysis of sparse Bayesian learning (2002, NIPS) </span>



<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_5"> Fast marginal likelihood maximisation for sparse Bayesian models (2003, International workshop on artificial intelligence and statistics) </span>



<hr style="height:0px;border:none;border-top:3px solid #555555;" />


## <span id="jump_reference"> References</span>

[[1] Zhu, Yunzhang. "An augmented ADMM algorithm with application to the generalized lasso problem". *Journal of Computational and Graphical Statistics*, 2017.](https://www.tandfonline.com/doi/abs/10.1080/10618600.2015.1114491?casa_token=DE1-adlG2v8AAAAA:_KATDTd4o-sM59EiZ9BfurRXKUyoegiG-utOAAFTSAat-iKlyNWgnJR9YNTN7SKDXC8bIZ0ffFowQA)

[[2] Tibshirani, Ryan J and Taylor, Jonathan. "The solution path of the generalized lasso". *The annals of statistics*, 2011.](https://projecteuclid.org/journals/annals-of-statistics/volume-39/issue-3/The-solution-path-of-the-generalized-lasso/10.1214/11-AOS878.short)

[[3] Tipping, Michael E. "Sparse Bayesian learning and the relevance vector machine". *Journal of machine learning research*, 2001.](https://www.jmlr.org/papers/volume1/tipping01a/tipping01a.pdf?ref=https://githubhelp.com)

[[4] Tipping, ACFME and Faul, A. "Analysis of sparse Bayesian learning". *Advances in neural information processing systems*, 2002.](https://books.google.com/books?hl=zh-CN&lr=&id=PGrlRWV5-v0C&oi=fnd&pg=PA383&dq=Analysis+of+Sparse+Bayesian+Learning&ots=ax8N-zCLdo&sig=4nvf0mpcmPI21iujwGuLANE4014)

[[5] Tipping, Michael E and Faul, Anita C. "Fast marginal likelihood maximisation for sparse Bayesian models". *International workshop on artificial intelligence and statistics*, 2003.](http://proceedings.mlr.press/r4/tipping03a.html)