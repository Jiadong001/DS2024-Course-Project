# DS 课程项目：基于运营商文本数据的知识库检索 - 比赛

比赛链接：https://www.datafountain.cn/competitions/1045

比赛任务：使用运营商相关的文档构建知识库，根据用户问题检索知识库并返回答案所在的文本块。

- **主办方限定选手应使用向量模型 “bge-large-zh-v1.5” 进行文本向量化。**

- **主办方特别说明**：本赛题意在优化 RAG 中除 “LLM 答案生成及之后部分” 的流程（参考下图蓝框），也就是对 Retrieval 的准确度进行优化，意在**消除大模型的使用对大家的不公平性**。选手应重点关注**文本解析**、**文本切片**、**检索过滤排序**等环节。

    <img src="./assets/RAG-R.jpeg" width=90%>

- 当然，在条件允许的情况下，可以将 “检索到的多条文本 + 问题” 写入 Prompt Template，然后输入到 LLM 中做推理，输出一个更加综合自然的回答。

- 至于微调大模型，个人认为不需要。

比赛关键时间节点，请大家注意：

|2024/11/08（12:00）|截止报名组队|
|:-|:-|
|**2024/11/13（24:00）**|**A 榜提交截止**|
|**2024/11/14**|**B 榜测试集公布（必须在 11月14日 24:00 前下载）**|
|**2024/11/15（00:00-24:00）**|**B 榜提交（必须在一天内提交）**|
|**2024/12/09-12/13**|**B 榜前 5 名线上决赛评审**|
|**2024/12/28-12/29**|**线下评审、颁奖典礼**|


## 比赛解读

> 只是提供给大家参考，大家不要被局限住。可以尝试完全不同的方案。

### 1. 任务流程

主办方提供了一个 **“特别说明”**，还提到 “所有问题的答案都可以在文档原文中找到”，同时评价指标也极大程度降低了 LLM 的影响，所以使用 RAG 中除 “LLM 答案生成及之后部分” 的流程作为 pipeline（参考下图蓝框）。

<center><img src="./assets/RAG-pipline.png" width=90%></center>

基础 Pipeline 如下：

- ① 文档解析：将运营商相关的 PDF 文档解析为文本数据。

- ② 文本切片/分块

- ③ 构建向量数据库：使用向量模型计算出每个文本块的表示向量，并存储到向量数据库中

- ④ 根据问题检索答案：涉及过滤、相似度计算、排序，将检索的文本作为问题的答案。


**文档解析方法(①)、文本分块方法(③)、以及 检索策略(④) 都有较大的探索空间。**

当然，在条件允许的情况下，可以将 “检索到的多条文本 + 问题” 写入 Prompt Template，然后输入到 LLM 中做推理，输出一个更加综合自然的回答。

至于微调大模型，个人认为不需要。


### 2. 对评价指标的理解

<center><img src="./assets/metric.png" width=90%></center>


- 对于每一道题，找对关键词、找对向量，是最重要的，所以使用 LLM 对指标的影响可能不大。

- 注意 answer 的长度不能太长，否则会被惩罚。

- 每道题有对应的难度系数分，针对难度较高题目的一些特点进行改进，是一个提分点。


### 3. 方案 pipeline demo

我用当前比较火的 [``LangChain``](https://github.com/langchain-ai/langchain) 库搭建了一个简单的 pipeline，供大家参考：[demo.ipynb](./demo.ipynb)。

🚀 demo 中还给出了每一步的 **改进建议**，大家没有思路时可以尝试。

在运行 demo 前，需要新建一个 python 环境 或 在已有 python 环境中下载相应 Package：

```commandline
conda create -n rag python=3.9

conda activate rag

conda install jupyter notebook pandas

pip install pdfplumber modelscope langchain langchain-community sentence_transformer faiss-cpu -i https://pypi.tuna.tsinghua.edu.cn/simple
```

硬件方面，最好有一张 **3G** 显存的 GPU 显卡来部署向量模型，用 CPU 应该会较慢。

这个方案的 A 榜得分：（尚未出分）

<center><img src="./assets/A_score.png" width=90%></center>


---


参考阅读：

- [[CSDN] 一文彻底搞定 RAG、知识库、 Llama-3！！](https://blog.csdn.net/xiangxueerfei/article/details/139304577#:~:text=%E6%A3%80%E7%B4%A2%E5%A2%9E%E5%BC%BA%E7%94%9F%E6%88%90%EF%BC%88Ret)
- to be updated
