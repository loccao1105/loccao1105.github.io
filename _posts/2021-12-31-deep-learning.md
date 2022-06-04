---
title: 'Papers for Model-Driven Deep Learning (2021)'
date: 2021-12-31
excerpt: "My reading list for <b>model-driven deep learning</b> papers in 2021."
permalink: /read-list/2021/model-driven-deep-learning/
tags:
  - Deep Learning
  - Model-Driven Method
---

<hr style="height:0px;border:none;border-top:3px solid #555555;" />

My 2021 paper reading list in **model-driven deep learning**. 

<hr style="height:0px;border:none;border-top:3px solid #555555;" />

模型启发的深度学习首先要需要基于问题背景，对任务进行数学建模。然后基于这个数学模型，设计一个合适的优化算法。一般来说，所选择或设计的优化算法是迭代算法。那么，我们可以将这个迭代算法展开为一个固定深度的神经网络，并通过数据驱动，让网络参数得以学习更新，即由模型启发而设计的深度网络。
<div style="text-align: center">
<img src="https://hauliang.github.io/read-list-file/model-driven-deep-learning.jpg" width="600px" height="100px"> 
</div>

<hr style="height:0px;border:none;border-top:3px solid #555555;" />

参考PPT（2021.08.17组会）：[Model-driven deep learning —— differentiable programming](https://hauliang.github.io/read-list-file/Model-driven-deep-learning-differentiable-programming.pdf)。
 

<hr style="height:0px;border:none;border-top:3px solid #555555;" />

# Paper List

* [Learning fast approximations of sparse coding (2010, ICML)](#jump_1)

* [Deep ADMM-Net for compressive sensing MRI (2016, NIPS)](#jump_2)

* [ADMM-CSNet: A deep learning approach for image compressive sensing (2018, TPAMI)](#jump_3)

* [ISTA-Net: Interpretable optimization-inspired deep network for image compressive sensing (2018, CVPR)](#jump_4)

* [FISTA-Net: Learning A fast iterative shrinkage thresholding network for inverse problems in imaging (2021, TMI)](#jump_5)

* [DeepWave: a recurrent neural-network for real-time acoustic imaging (2019, NIPS)](#jump_6)

* [References](#jump_reference)


<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_1"> Learning fast approximations of sparse coding (2010, ICML) </span>

&emsp;&emsp;本文用神经网络来学习稀疏编码问题。稀疏编码问题可以建模为一范数约束下的最小二乘问题，如下：

$$
\boldsymbol{Z}^{*}=\underset{\boldsymbol{Z}}{\operatorname{argmin}} \frac{1}{2}\left\|\boldsymbol{X}-\boldsymbol{W}_{d} \boldsymbol{Z}\right\|_{2}^{2}+\alpha\|\boldsymbol{Z}\|_{1},
$$

其中$\boldsymbol{W}_{d}$表示字典矩阵。对于此优化问题，已有很多算法用于求解它，比如ISTA算法（见参考文献[[7]](#jump_ref7)）。但ISTA算法为迭代算法，需要很多轮迭代才能收敛，计算成本是昂贵的，具体算法步骤如下：
<div style="text-align: center">
<img src="https://hauliang.github.io/read-list-file/ISTA.jpg" width="400px" height="300px"> 
</div>
&emsp;&emsp;作者注意到迭代算法，如ISTA，可以等价为一个循环神经网络，如下：
<div style="text-align: center">
<img src="https://hauliang.github.io/read-list-file/ISTA-block-diagram.jpg" width="400px" height="300px"> 
</div>
&emsp;&emsp;但这个网络是不断循环直至收敛的，这样的网络无法进行训练。那我们如何将ISTA算法转化为一个可训练的神经网络呢？作者提出，我们只需要将上图进行固定轮数的截断，就可以得到一个ISTA版本的神经网络，它等价于执行ISTA算法中的L轮迭代（其中L为网络的深度）。具体来说，本文提出的LISTA网络可以表示为如下形式：
<div style="text-align: center">
<img src="https://hauliang.github.io/read-list-file/LISTA.jpg" width="600px" height="400px"> 
</div>

其中$\boldsymbol{W}_{e}$, $\boldsymbol{S}$和$\theta$都是可学习的参数。至于损失函数，只需要最小化网络输出的稀疏编码与ground truth的稀疏编码即可，具体如下：

$$
\begin{aligned}
&L(\boldsymbol{W})=\frac{1}{P} \sum_{p=1}^{P} L\left(\boldsymbol{W}, \boldsymbol{X}^{p}\right) \text { with } \\
&L\left(\boldsymbol{W}, \boldsymbol{X}^{p}\right)=\frac{1}{2}\left\|\boldsymbol{Z}^{* p}-f_{e}\left(\boldsymbol{W}, \boldsymbol{X}^{p}\right)\right\|_{2}^{2}.
\end{aligned}
$$

&emsp;&emsp;网络可以很好取得较小估计误差和较快的运行时间。部分数值实验结果截取如下：
<div style="text-align: center">
<img src="https://hauliang.github.io/read-list-file/LISTA-result.jpg" width="600px" height="400px"> 
</div>

<hr style="height:0px;border:none;border-top:3px solid #555555;" />


## <span id="jump_2"> Deep ADMM-Net for compressive sensing MRI (2016, NIPS) </span>

&emsp;&emsp;本文用ADMM启发的深度网络来解决MRI压缩感知的问题。作者发现了传统方法和深度学习方法中的问题：

1. 传统方法：最佳变换域以及稀疏约束的选择难以确定，并且难以确定最优参数（如稀疏参数、步长参数等），同时算法需要上百轮迭代才可以收敛；

2. 深度学习方法：CNN计算高效，但感受野有限；DNN感受野深，但计算慢。同时深度学习的黑盒属性让网络难以自动学习变换的稀疏性。

&emsp;&emsp;压缩感知MRI模型可以描述如下：
<div style="text-align: center">
<img src="https://hauliang.github.io/read-list-file/CSMRI-model.jpg" width="600px" height="400px"> 
</div>

而ADMM算法（见参考文献[[8]](#jump_ref8)）是解决这个问题的一个有效方案，ADMM通过迭代优化三个子问题来求解上述优化问题，此即：

$$
\left\{\begin{array}{l}
\boldsymbol{x}^{(n+1)}=\underset{\boldsymbol{x}}{\operatorname{argmin}} \frac{1}{2}\|\boldsymbol{A} \boldsymbol{x}-\boldsymbol{y}\|_{2}^{2}-\sum_{l=1}^{L}\left\langle\boldsymbol{\alpha}_{l}^{(n)}, \mathbf{z}_{l}^{(n)}-\boldsymbol{D}_{l} x\right\rangle+\sum_{l=1}^{L} \frac{\rho_{l}}{2}\left\|\mathbf{z}_{l}^{(n)}-\boldsymbol{D}_{l} \boldsymbol{x}\right\|_{2}^{2}, \\
\boldsymbol{z}^{(n+1)}=\underset{\boldsymbol{z}}{\operatorname{argmin}} \sum_{l=1}^{L} \lambda_{l} g\left(\mathbf{z}_{l}\right)-\sum_{l=1}^{L}\left\langle\boldsymbol{\alpha}_{l}^{(n)}, \mathbf{z}_{l}-\boldsymbol{D}_{l} \boldsymbol{x}^{(n+1)}\right\rangle+\sum_{l=1}^{L} \frac{\rho_{l}}{2}\left\|\mathbf{z}_{l}-\boldsymbol{D}_{l} \boldsymbol{x}^{(n+1)}\right\|_{2}^{2}, \\
\boldsymbol{\alpha}^{(n+1)}=\underset{\boldsymbol{\alpha}}{\operatorname{argmin}} \sum_{l=1}^{L}\left\langle\boldsymbol{\alpha}_{l}, \boldsymbol{D}_{l} \boldsymbol{x}^{(n+1)}-\boldsymbol{z}_{l}^{(n+1)}\right\rangle.
\end{array}\right.
$$

上述子问题可以等价写作：

$$
\left\{\begin{array}{l}
\mathbf{X}^{(\mathbf{n})}: \boldsymbol{x}^{(n)}=\boldsymbol{F}^{T}\left[\boldsymbol{P}^{T} \boldsymbol{P}+\sum_{l=1}^{L} \rho_{l} \boldsymbol{F} \boldsymbol{D}_{l}^{T} \boldsymbol{D}_{l} \boldsymbol{F}^{T}\right]^{-1}\left[\boldsymbol{P}^{T} \boldsymbol{y}+\sum_{l=1}^{L} \rho_{l} \boldsymbol{F} \boldsymbol{D}_{l}^{T}\left(\mathbf{z}_{l}^{(n-1)}-\boldsymbol{\beta}_{l}^{(n-1)}\right)\right], \\
\mathbf{Z}^{(\mathbf{n})}: \mathbf{z}_{l}^{(n)}=S\left(\boldsymbol{D}_{l} x^{(n)}+\boldsymbol{\beta}_{l}^{(n-1)} ; \lambda_{l} / \rho_{l}\right), \\
\mathbf{M}^{(\mathbf{n})}: \boldsymbol{\beta}_{l}^{(n)}=\boldsymbol{\beta}_{l}^{(n-1)}+\eta_{l}\left(\boldsymbol{D}_{l} \boldsymbol{x}^{(n)}-\mathbf{z}_{l}^{(n)}\right).
\end{array}\right.
$$

&emsp;&emsp;注意到ADMM算法是不断迭代上述式子，得到最后的收敛解。那么，我们可以设计L层的深度网络，将算法的迭代步骤用于指导网络的设计。具体来说，ADMM-Net的stage n编队应于ADMM算法的第n轮迭代，网络架构如下：
<div style="text-align: center">
<img src="https://hauliang.github.io/read-list-file/ADMM-Net.jpg" width="700px" height="300px"> 
</div>

&emsp;&emsp;ADMM-Net网络中定义了四种操作，分别为：

1. Reconstruction operation $\boldsymbol{X}^{n}$

&emsp;&emsp;对应于ADMM中的：$\boldsymbol{x}^{(n)}=\boldsymbol{F}^{T}\left[\boldsymbol{P}^{T} \boldsymbol{P}+\sum_{l=1}^{L} \rho_{l} \boldsymbol{F} \boldsymbol{D}_{l}^{T} \boldsymbol{D}_{l} \boldsymbol{F}^{T}\right]^{-1}\left[\boldsymbol{P}^{T} \boldsymbol{y}+\sum_{l=1}^{L} \rho_{l} \boldsymbol{F} \boldsymbol{D}_{l}^{T}\left(\mathbf{z}_{l}^{(n-1)}-\boldsymbol{\beta}_{l}^{(n-1)}\right)\right]$.

2. Convolution operation $\boldsymbol{C}^{n}$ defined by $\left\{\boldsymbol{D}_{l} \boldsymbol{x}^{(n)}\right\}_{l=1}^{L}$

3. Nonlinear transform operation $\boldsymbol{Z}^{n}$ defined by $S(\cdot)$

4. Multiplier update operation 

<hr style="height:0px;border:none;border-top:3px solid #555555;" />


## <span id="jump_3"> ADMM-CSNet: A deep learning approach for image compressive sensing (2018, TPAMI) </span>


<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_4"> ISTA-Net: Interpretable optimization-inspired deep network for image compressive sensing (2018, CVPR) </span>


<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_5"> FISTA-Net: Learning A fast iterative shrinkage thresholding network for inverse problems in imaging (2021, TMI) </span>


<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_6"> DeepWave: a recurrent neural-network for real-time acoustic imaging (2019, NIPS) </span>


<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_reference"> References</span>

[[1] Gregor, Karol and LeCun, Yann. "Learning fast approximations of sparse coding". *Proceedings of the 27th international conference on international conference on machine learning*, 2010.](https://dl.acm.org/doi/abs/10.5555/3104322.3104374)

[[2] Sun, Jian and Li, Huibin and Xu, Zongben and others. "Deep ADMM-Net for compressive sensing MRI". *Advances in neural information processing systems*, 2016.](https://proceedings.neurips.cc/paper/6406-deep-admm-net-for-compressive-sensing-mri)

[[3] Yang, Yan and Sun, Jian and Li, Huibin and Xu, Zongben. "ADMM-CSNet: A deep learning approach for image compressive sensing". *IEEE transactions on pattern analysis and machine intelligence*, 2018.](https://ieeexplore.ieee.org/abstract/document/8550778/)

[[4] Zhang, Jian and Ghanem, Bernard. "ISTA-Net: Interpretable optimization-inspired deep network for image compressive sensing". *Proceedings of the IEEE conference on computer vision and pattern recognition*, 2018.](http://openaccess.thecvf.com/content_cvpr_2018/html/Zhang_ISTA-Net_Interpretable_Optimization-Inspired_CVPR_2018_paper.html)

[[5] Xiang, Jinxi and Dong, Yonggui and Yang, Yunjie. "FISTA-net: Learning a fast iterative shrinkage thresholding network for inverse problems in imaging". *IEEE Transactions on Medical Imaging*, 2021.](https://ieeexplore.ieee.org/abstract/document/9335299/)

[[6] Simeoni, Matthieu and Kashani, Sepand and Hurley, Paul and Vetterli, Martin. "DeepWave: a recurrent neural-network for real-time acoustic imaging". *Advances in Neural Information Processing Systems*, 2019.](https://proceedings.neurips.cc/paper/2019/hash/e9bf14a419d77534105016f5ec122d62-Abstract.html)

<span id="jump_ref7">[[7] Daubechies, Ingrid and Defrise, Michel and De Mol, Christine. "An iterative thresholding algorithm for linear inverse problems with a sparsity constraint".*Communications on Pure and Applied Mathematics: A Journal Issued by the Courant Institute of Mathematical Sciences*, 2019.](https://onlinelibrary.wiley.com/doi/abs/10.1002/cpa.20042?casa_token=NdetDJihuEYAAAAA:ggEgBIc_u5It6u3XUsgrsCh59mhI_R5UjwZslSYQQPYHzsyTaIpbn8YzWr-vGxbQSe7x5OAmCxDZgjT9JA)

<span id="jump_ref8">[[8] Boyd, Stephen and Parikh, Neal and Chu, Eric and Peleato, Borja and Eckstein, Jonathan and others. "Distributed optimization and statistical learning via the alternating direction method of multipliers".*Foundations and Trends{\textregistered} in Machine learning*, 2011.](http://www.nowpublishers.com/article/Details/MAL-016)

<span id="jump_ref9">[[9] Daubechies, Ingrid and Defrise, Michel and De Mol, Christine. "An iterative thresholding algorithm for linear inverse problems with a sparsity constraint".*Communications on Pure and Applied Mathematics: A Journal Issued by the Courant Institute of Mathematical Sciences*, 2019.](https://onlinelibrary.wiley.com/doi/abs/10.1002/cpa.20042?casa_token=NdetDJihuEYAAAAA:ggEgBIc_u5It6u3XUsgrsCh59mhI_R5UjwZslSYQQPYHzsyTaIpbn8YzWr-vGxbQSe7x5OAmCxDZgjT9JA)