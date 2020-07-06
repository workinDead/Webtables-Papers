*Herzig, J., Nowak, P. K., Müller, T., Piccinno, F., & Eisenschlos, J. M. (2020). TAPAS: Weakly Supervised Table Parsing via Pre-training. arXiv preprint arXiv:2004.02349.*

## Resource

- [论文地址](https://arxiv.org/pdf/2004.02349.pdf)
- [代码和模型](https://github.com/google-research/tapas)
  - SQA数据集上的预测[[Colab](https://colab.research.google.com/drive/11eUs7vHT-tzSd2TBmnaom1SXoy7DvmxO?usp=sharing)]

##  Abstract

Answering natural language questions over tables is usually seen as a semantic parsing task. To alleviate the collection cost of full logical forms, one popular approach focuses on weak supervision consisting of denotations instead of logical forms. However, training semantic parsers from weak supervision poses difficulties, and in addition, the generated logical forms are only used as an intermediate step prior to retrieving the denotation. In this paper, we present TAPAS, an approach to question answering over tables without generating logical forms. TAPAS trains from weak supervision, and predicts the denotation by selecting table cells and optionally applying a corresponding aggregation operator to such selection. TAPAS extends BERT’s architecture to encode tables as input, initializes from an effective joint pre-training of text segments and tables crawled from Wikipedia, and is trained end-to-end. We experiment with three different semantic parsing datasets, and find that TAPAS outperforms or rivals semantic parsing models by improving state-of-the-art accuracy on SQA from 55.1 to 67.2 and performing on par with the state-of-the-art on WIKISQL and WIKITQ, but with a simpler model architecture. We additionally find that transfer learning, which is trivial in our setting, from WIKISQL to WIKITQ, yields 48.7 accuracy, 4.2 points above the state-of-the-art.

## Background

世上许多信息都是以表格形式存储的，这些表格见诸于网络、数据库或文件中。它们包括消费产品的技术规格、金融和国家发展统计数据、体育赛事结果等等。目前，要想找到问题的答案，人们仍需以人工方式查找这些表格，或使用能提供特定问题（比如关于体育赛事结果的问题）的答案的服务。如果可通过自然语言来查询这些信息，那么取用这些信息会容易很多。

![A table with corresponding example questions](http://ww1.sinaimg.cn/large/74c6a0b5gy1ggh0twg1xnj20hs067jrv.jpg)

针对这一问题，近来的很多方法采用了传统的语义解析方案，即将自然语言问题转译成一个类 SQL 的数据库查询，其在数据库上执行后可提供答案。如图示中第三个问题，「How many world champions are there with only one reign」会被映射到这样一个查询：select count(*) where column("No. of reigns") == 1，执行该查询后即可得到答案。为了得到句法和语义上有效的查询，转化成类SQL的这种方法所需的工程量大，而且仅适用于与特定表格（如体育赛事结果）有关的问题，难以扩展应用于任意问题。

## Research Questions

解决单表（限小表）的单轮或多轮问答问题（限简单聚合操作：SUM, AVERAGE, COUNT）

## Data

- [预训练数据](https://github.com/google-research/tapas/blob/master/PRETRAIN_DATA.md)

百万级[英文维基百科](https://en.wikipedia.org/wiki/Wikipedia)表格(6.2M)：3.3M [Infobox](https://en.wikipedia.org/wiki/Help:Infobox) (实体型表格) + 2.9M [WikiTable](https://en.wikipedia.org/wiki/ARY_Film_Award_for_Best_Dialogue) (限有表头的横向关系型表格*)；

及相关正文部分(21.3M)：page title, description, table caption, section title, section text.[Checkpoints](https://github.com/google-research/tapas#data)

<img src="http://ww1.sinaimg.cn/large/74c6a0b5gy1ggh2hw57o4j214q1oh4qp.jpg" alt="wiki-example" style="zoom: 33%;" />

- 微调数据

  [WIKISQL](https://github.com/salesforce/WikiSQL), [WIKITQ](https://github.com/ppasupat/WikiTableQuestions), [SQA](https://www.microsoft.com/en-us/download/details.aspx?id=54253)

<img src="http://ww1.sinaimg.cn/large/74c6a0b5gy1ggh2mkz0awj20oe0bgab2.jpg" alt="dataset statistics" style="zoom:50%;" />

## Method/Models

**TAPAS: 扩展型的BERT架构可对问题与表格结构进行联合编码，最终得到的模型可直接指向问题答案**

对于问题列表中第二个问题「Average time as champion for top 2 wrestlers」，这种扩展型 BERT 模型使用特定的嵌入来编码表格结构，并且能在逐行编码表格内容的同时联合编码问题。

对于基于 transformer 的 BERT 模型，核心扩展思路是新增了用于编码结构化输入的额外嵌入。这依赖于为列索引、行索引和一个特别的排序索引（表示数值列中元素的顺序）所学习的嵌入。下图展示了这些嵌入聚合成输入的方式以及馈送入 transformer 网络层的方式。

下图展示了编码问题的方式，并在左边给出了一张小表格。每个单元格 token 都有一个指示其行、列和在列中的数值排序的特殊嵌入。

![Encoding of the question and a simple table using the special embeddings of TAPAS](http://ww1.sinaimg.cn/large/74c6a0b5gy1ggh34a3fv5j20hs04imxs.jpg)

*BERT 层输入：每个输入 token 都被表示成其词、绝对位置、句段（无论是属于问题还是表）、列和行以及数值排序的嵌入之和*



该模型有两个输出：1）一个分数，用于表示每个表格单元格的内容属于答案一部分的概率；2）一个聚合操作，用于表示是否应用操作以及应用哪些操作来将各个单元格的内容聚合成最终答案。下图展示了对于问题「Average time as champion for top 2 wrestlers」，该模型有较高的概率选择 Combined days 列的前两个单元格以及使用 AVERAGE 操作。

![TAPAS model with example model outputs for the question](http://ww1.sinaimg.cn/large/74c6a0b5gy1ggh3g4efj1j20hs0a3q3e.jpg)

*模型示意图：BERT 层同时编码问题和表格。该模型会输出每个聚合操作的概率以及每个表格单元格的选择概率。对于问题「Average time as champion for top 2 wrestlers」，该模型以较高的概率选择了 AVERAGE 操作以及数值为 3749 和 3103 的两个单元格*



**预训练过程**

预训练过程类似于 BERT 在文本上的训练方法，其训练数据是从在本文Data部分提到的英语维基百科提取的 620 万组表格 - 文本数据对。在预训练过程中，模型的学习目标是恢复表格和文本中被掩码替换的词。通过实验发现，该模型在这项任务上的准确度相对较高——对于训练过程中未曾见过的表格，该模型能够正确恢复 71.4% 的被掩盖 token。

**微调过程**

在微调过程中，模型的目标是学习如何基于表格回答问题。这可以通过强监督方法实现，也可使用弱监督方法。如果使用强监督方法，则对于给定表格和问题，必须先提供所要选择的单元格和聚合操作（比如求和或计数），但这个过程非常耗时耗力。因此本文采取的是更常见的方法，即使用弱监督方法进行训练，此时仅需提供正确答案即可（比如对于以上示例，正确答案是 3426）。

在弱监督情况下，模型需要自己尝试寻找能得到接近正确答案的聚合操作和单元格。这个过程需要在所有可能的聚合决策上计算期望，并将其与真实结果进行比较。弱监督方法更加有利，因为它让非专家也能提供训练模型所需的数据，而且消耗的时间也比强监督方法少。

## Results

在 SQA、WikiTableQuestions (WTQ) 和 WikiSQL 这三个数据集上进行了实验验证，并对比了在解析表格数据任务中表现最佳的三种其它方法。其中，在 WikiSQL 上对比的模型为 Min et al (2019)，在 WTQ 上对比的模型为 Wang et al. (2019)，在 SQA 上对比的模型为 Mueller et al., (2019)

对于所有数据集，报告的结果都是弱监督训练设置下在测试集上的答案准确度。对于 SQA 和 WikiSQL，使用了基于维基百科数据得到的预训练模型作为基础模型；而对于 WTQ，他们发现在 SQA 数据上再进行预训练会更有利。谷歌新方法的表现优于之前最佳水平——在 SQA 上超过之前最佳方法 12 个百分点，在 WTQ 上超过之前最佳方法 4 个百分点，在 WikiSQL 上与之前最佳方法表现相近。

![image](http://ww1.sinaimg.cn/large/74c6a0b5gy1ggh3zewo0mj20b406wgll.jpg)

*弱监督设置下，模型在三个学术级表格问答数据集上的测试答案准确度*



## Contribution

1. 模型架构较为简单
2. 模型拓展性强：可解决多种问题如涉及聚合操作的问题或者多轮对话

## Limitations

1. 解决单表问答：模型本身对编码长度的限制，导致过大的表格无法被有效编码，因此如果要处理大表，可能会需要大量的数据预处理
2. 无法解决涉及多次聚合操作的问答：例如同时涉及AVERAGE和COUNT，「Number of actors with an average rating higher than 4」 

