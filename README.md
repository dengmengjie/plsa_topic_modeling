# PLSA implementation with wordcloud visualization

## PLSA

论文：[Probabilistic Latent Semantic Indexing](https://dl.acm.org/doi/10.1145/3130348.3130370)



#### 介绍

**PLSA** (Probabiistic Latent Semantic Analysis)，概率潜在语义分析；也叫PLSI (Probabilistic Latent Semantic Indexing)，概率潜在语义索引。是一种基于似然函数提出的生成模型，常用于文档索引、主题模型等任务。

在主题模型中，PLSA可以在无监督语料中建模主题，为每篇文档标注主题，并获得每个主题中的词汇分布。

符号：$d$->文档，$z$->主题，$w$->单词，$n_d$>文档数，$n(d,w)$->文档d中单词w的出现次数。

PLSA基于这样的一个假设：

* 每篇文档有一个**主题分布$p(z|d)$**，文档由这个主题分布生成。例如，一篇文档包含60%的”体育“主题，”30“的”娱乐“主题，”10%“的”新闻“主题。
* 每个主题有一个**词汇分布$p(w|z)$**，主题按照词汇分布生成文档。例如，“体育”主题由0.4概率的“比赛”，0.3概率的“俱乐部”，0.2概率的“球队”，0.1概率的“球员”。

PLSA的假设有两个特点：

* 基于词袋模型，即不考虑词之间的顺序，而只考虑词出现的次数。
* 是一种混合模型，把每个主题看作一个模型（即$p(w|z)$），每篇文档是由多个主题模型混合而成的。

基于这个假设，一篇文档$d$的生成过程是这样的：

* 随机确定总词数$N_d$。
* 每一个词按照以下方式生成：根据主题分布$p(z|d)$生成一个主题z，根据词汇分布$p(w|z)$生成一个单词。
* $N_d$个单词构成了这一篇文档。





#### 算法

最大化似然估计（MLE, Maximum Likelihood Estimation）：

$$
\begin{align*}
& \max \mathcal{Likelihood} = \prod_{d'} \prod_{w'} p(d',w') \\
& \max \mathcal{Likelihood} = \prod_d \prod_w p(d,w)^{n(d,w)} \\
& \max \mathcal{L} = \log \mathcal{Likelihood} = \sum_d\sum_w n(d,w) \log p(d,w) \\
\end{align*}
$$

用EM算法解决MLE：

$$
\begin{align*}
& E-step: \\
& p(z|d,w) = \frac{p(w|z)p(z|d)}{\sum_{z'}p(w|z')p(z'|d)} \\
& \\
& M-step: \\
& p(w|z) = \frac{\sum_d n(d,w) p(z|d, w)}{\sum_{w'} \sum_{d'} n(d', w') p(z | d', w')} \\
& p(z|d) = \frac{\sum_w n(d,w) p(z|d, w)}{n_d} \\
\end{align*}
$$


### 数据集

`train.txt`选取[THUCNews](http://thuctc.thunlp.org/)新闻数据集下“体育”、“科技”、“财经”、“房产”、“教育”各类目下前400篇文档构建成2000篇文档的小数据集，无标注。

[THUCNews参考资料](https://www.jianshu.com/p/687ecdd3f840)

