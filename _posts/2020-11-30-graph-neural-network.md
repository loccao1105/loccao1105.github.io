---
title: 'Summary for Graph Neural Network (GNN)'
excerpt: "My reading list for <b>graph neural network</b> papers."
permalink: /read-list/graph-neural-network/
tags:
  - Graph Embedding
  - Link Prediction
---


<hr style="height:0px;border:none;border-top:3px solid #555555;" />

My paper reading list in graph neural network (GNN), with special emphasis on **graph embedding**, **alignment** and **link prediction**. 

<hr style="height:0px;border:none;border-top:3px solid #555555;" />

# Paper List

* [node2vec: Scalable feature learning for networks (2016, KDD)](#jump_1)

* [Link prediction via subgraph embedding-based convex matrix completion (2018, AAAI)](#jump_2)

* [Weisfeiler-lehman neural machine for link prediction (2017, KDD)](#jump_3)

* [Link prediction based on graph neural networks (2018, NIPS)](#jump_4)

* [Understanding negative sampling in graph representation learning (2020, KDD)](#jump_5)

* [Bursting the filter bubble: Fairness-aware network link prediction (2020, AAAI)](#jump_6)

* [References](#jump_reference)


<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_1"> node2vec: Scalable feature learning for networks (2016, KDD) </span>

&emsp;Graph embedding via **representation learning** (node -> vector).

&emsp;This paper proposed a flexible method to learn domain information, in which different types of domains can be learned through parameter adjustment, and thus a smooth interpolation may be implemented between depth first search and breadth first search.

&emsp;Specifically, the **second-order Markov random walk** may be introduced to obtain the **sequence of node random walk**, which may then **be treated as a special sentence**. Thus, the **Skip-Gram model** under the negative sampling strategy may be used to learn the node representation.

&emsp;After the node representation is obtained, the feature representation of its edge may then be obtained by operating the transformation, which can be used to link prediction by using a classification model.


<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_2"> Link prediction via subgraph embedding-based convex matrix completion (2018, AAAI)</span>

&emsp;Link prediction via **representation learning** (better for heterogeneous graphs).

&emsp;The main idea of this paper is to **introduce sub-graphs for node embedding**. Specifically, **sub-graph embedding** and **convex matrix completion** is introduced, and SVD decomposition may be used to replace the Skip-Gram model under negative sampling (supported by theoretical proof). The steps are following:

&emsp;1) **Retrieve sub-graphs**: The sub-graphs with different depths of each vertex may be searched by using the breadth first search. Then, the **sub-graphs can be regarded as new vertices**, and their "context" can be defined. Thus, a new graph $G$ can be constructed, with sub-graphs as vertices and edges formed by whether the two sub-graphs are contexts.

&emsp;2) **Calculate the $PPMI$ matrix**: The $PPMI$ matrix of graph $G$ is defined as $M=(A+A^2)/2$.

&emsp;3) **Matrix completion**: **Learning the low-rank representation of $M$** can remove the influence of the white noise, and thus the soft threshold algorithm may be introduced to complete the matrix $M$, obtaining the matrix $W$.

&emsp;4) **Sub-graph embedding**: After obtaining $W$, it can be regarded as the embedding matrix of the sub-graphs, and **its rows can be used to represent the corresponding sub-graphs**. Then, the sub-graph vectors of different depths of a vertex may be spliced, forming the embedding of this vertex.

&emsp;5) **Link prediction**: The **cosine similarity** between the two vetrice vectors is used to classification -- whether the link is exist.


<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_3"> Weisfeiler-lehman neural machine for link prediction (2017, KDD)</span>

&emsp;Link prediction via **first or high-order structural information**.

&emsp;Traditional methods, such as the neighbor-based method, use the **first or high-order information of the graph to judge the existence of links**. In this paper, the authors want to **learn these features automatically** by network without artificial definitions. The steps are following:

&emsp;1) **A closed sub-graph of each pair of links** is defined. Each sub-graph has fixed $K$ (a hyperparameter) nodes.

&emsp;2) The proposed **palette-WL algorithm** may be used to **sort the nodes** of the sub-graph, which ensures that the vertex sorting method is consistent in different sub-graphs and similar nodes have the same ordering.

&emsp;3) Extract the **upper triangular of the adjacency matrix** of each sub-graph. Note that here is a relaxed adjacency matrix, not all 0 or 1, but relax some 0 parts to part of the distance. Also, remove the adjacent part of the two points corresponding to the link to be predicted.

&emsp;4) **Straighten the extracted upper triangle**, and put it into the neural network to train the classifier.

<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_4"> Link prediction based on graph neural networks (2018, NIPS)</span>

&emsp;Link prediction via **first or high-order structural information**.

&emsp;This work builds upon the previours paper ([*Weisfeiler-lehman neural machine for link prediction, 2017, KDD*](#jump_3)). The weakness of the previous paper: the sub-graph is restricted to have $K$ nodes, which may loss the ability to catch structural information; the embedding  is learned from adjacency matrix, which is unable to exploit node feature assistance. This paper has made some enhancement, mainly:

&emsp;1) **GNN** is introduced to replace the fully connected neural network; 2) The embedding is learned by **exploiting the sub-graph structure and node features**.

&emsp;Link prediction is implemented by **learning local sub-graphs combined with node embedding information**. In this paper, each local sub-graph is considered corresponds to a pair of nodes.

&emsp;First, it is proved theoretically that local sub-graphs, which contain rich information to predict links, can **approach to high-order features** with small errors. The structural node label information can be integrate by using the dual-radius node labeling method, and then **GNN** is introduced to **learn node features** implicitly or implicitly. In the training of each local sub-graph GNN, the negative sample strategy may be adopted, and some non-existing edges are also added for robust training. Finally, **GNN** is employed to **perform classification training**, and thus implements the link prediction. The specific detail of graph classification process can be found in [[5]](#jump_ref5) (*An end-to-end deep learning architecture for graph classification, 2018, AAAI*).

<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_5"> Understanding negative sampling in graph representation learning (2020, KDD)</span>

&emsp;Link prediction via a **new negative sampling strategy**.

&emsp;The importance of negative sampling is discussed from **target** and **risk**. The authors prove that the **positive and negative distributions have the same influence** to the optimization procedure, and the squared deviation loss may be determined by the minimum value of the probability of the two distributions. Then, this paper concludes that the **negative sampling distribution should be sublinearly related to the positive**, and thus proposes a new negative sampling strategy:

&emsp;1) Employ the **self-contrastive approximation** to replace the negative sampling distribution with the **inner product of the encoder**.

&emsp;2) Employ the **Metropolis-Hastings (MH)** algorithm to obtain a sequence of random samples from a non-standardized distribution.

&emsp;3) Aimed at the long iteration time in the MH algorithm, the characteristics, i.e., the **adjacent nodes sharing similar positive distribution**, may be used to improve the negative sampling process of the Markov chain. 

&emsp;4) The proposed negative sampling strategy can be used to replace the other negative sampling strategies to improve the performance.



<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_6"> Bursting the filter bubble: Fairness-aware network link prediction (2020, AAAI)</span>

&emsp;Generate more heterogeneous links **via network fair embedding**.

&emsp;Existing algorithms promote the generation of links, which may cause **a filter bubble effect**, i.e., the users become more and more isolated in social networks and are limited to similar communities.From the perspective of fairness, it is unfair to favor certain types of links. This paper thus considers **fairness from the perspective of promoting links**.

&emsp;the impact of attributes on links empirically. Then, **a fairness criterion** for this problem, termed **modred**, is proposed. Finally, a fairness-aware link prediction framework is proposed, which combines **adversarial** and **supervised learning**. The specific steps are following:

&emsp;1) Employ a generator to **generate node representations** [[8]](#jump_ref8) (*Deepwalk: Online learning of social representations, 2014, KDD*).

&emsp;2) Employ a discriminator to **distinguish the intra-group and inter-group pairs**. Note that a intra-group pair indicates that the two nodes forming the link are from similar communities.

&emsp;3) Link prediction network. The prediction network is obtained by **splicing two node embedding vectors** into **a fully linked neural network** for supervised training.




<hr style="height:0px;border:none;border-top:3px solid #555555;" />

## <span id="jump_reference"> References</span>

[[1] Grover, Aditya and Leskovec, Jure. "node2vec: Scalable feature learning for networks". *Proceedings of the 22nd ACM SIGKDD international conference on Knowledge discovery and data mining*, 2016.](https://dl.acm.org/doi/abs/10.1145/2939672.2939754?casa_token=YaQ6zJHC0tEAAAAA:TyGhi4mFoIITlQkON047aJq7s0p-u5eUDt0BXzcE5U8DY_B_mD0LLUjWlYX_C5UnqeNYgwlsFRuVbw)

[[2] Cao, Zhu and Wang, Linlin and De Melo, Gerard. "Link prediction via subgraph embedding-based convex matrix completion". *Proceedings of the AAAI Conference on Artificial Intelligence*, 2018.](https://ojs.aaai.org/index.php/AAAI/article/view/11655)

[[3] Zhang, Muhan and Chen, Yixin. "Weisfeiler-lehman neural machine for link prediction". *Proceedings of the 23rd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining*, 2017.](https://dl.acm.org/doi/abs/10.1145/3097983.3097996)

[[4] Zhang, Muhan and Chen, Yixin. "Link prediction based on graph neural networks". *Advances in Neural Information Processing Systems*, 2018.](https://proceedings.neurips.cc/paper/2018/file/53f0d7c537d99b3824f0f99d62ea2428-Paper.pdf)

<span id="jump_ref5"> [[5] Zhang, Muhan and Cui, Zhicheng and Neumann, Marion and Chen, Yixin. "An end-to-end deep learning architecture for graph classification". *Thirty-Second AAAI Conference on Artificial Intelligence*, 2018.](https://www.findshine.com/me/downloads/papers/AAAI2018-DGCNN.pdf)</span>

[[6] Yang, Zhen and Ding, Ming and Zhou, Chang and Yang, Hongxia and Zhou, Jingren and Tang, Jie. "Understanding negative sampling in graph representation learning". *Proceedings of the 26th ACM SIGKDD International Conference on Knowledge Discovery \& Data Mining*, 2020.](https://dl.acm.org/doi/abs/10.1145/3394486.3403218?casa_token=iaq_bZ8FDMQAAAAA:78WO0m9SeU1dVAHjS8w0gQZfRfBl3eaQbxvLK9bQo4RczuMgTs65UNiF7sPk6s8AnHjPjsP3XTgmqA)

[[7] Masrour, Farzan and Wilson, Tyler and Yan, Heng and Tan, Pang-Ning and Esfahanian, Abdol. "Bursting the filter bubble: Fairness-aware network link prediction". *Proceedings of the AAAI Conference on Artificial Intelligence*, 2020.](https://ojs.aaai.org/index.php/AAAI/article/view/5429)

<span id="jump_ref8">[[8] Perozzi, Bryan and Al-Rfou, Rami and Skiena, Steven. "Deepwalk: Online learning of social representations". *Proceedings of the 20th ACM SIGKDD international conference on Knowledge discovery and data mining*, 2014.](https://dl.acm.org/doi/abs/10.1145/2623330.2623732?casa_token=k6_u02birAAAAAAA:WqyqWziZOFmOVmvlX7_xcSRuIG3fT98M6FRzMsSuSZn1XkxVwVFA9-ixem4s9VFy3Xhj3CTI2iaEWQ)</span>