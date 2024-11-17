# Transformer模型

意义：长距离依赖关系处理和并行计算

https://jalammar.github.io/illustrated-transformer/

## Attention原理

在原始的RNN中，Encoder 循环接收上一层的输出以及新的输入进行运算，得到最后的 **hidden states**，就是我们的上下文，将其放入 Decoder 中解码，但是这里存在的问题是处理长难句变得有挑战性，因为当上下文较长的时候，远距离的 hidden states 可能在运算中被忽视了。

Sequence2Sequence 的 Encoder-Decoder 中，所有的 Encoder 层输出的 hidden states 都会传给 Decoder。

Attention Decoder 在产生输出之前执行额外的步骤。为了更加关注对应位置的输入，Decoder 执行以下操作：

1. 查看它收到的 Endcoder hidden states - 每个编码器隐藏状态与输入句子中的某个单词最相关
2. 给每个隐藏状态一个分数
3. 将每个隐藏状态乘以其 softmax 分数，从而放大高分 hidden states，并忽略低分 hidden states

推荐可视化博客：https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/



## 计算隐藏状态分数

**Query**（**查询**）：Query是一个特征向量，描述我们在序列中寻找什么，即我们可能想要注意什么。

**Keys：**每个输入元素有一个**键**，它也是一个特征向量。该特征向量粗略地描述了该元素“提供”什么，或者它何时可能很重要。键的设计应该使得我们可以根据Query来识别我们想要关注的元素。例如，键通常由编码器产生，代表输入序列中每个词或短语的抽象表示。

**Values：**每个输入元素，我们还有一个**值**向量。这个向量就是我们想要平均的向量。它与键一起被编码，但用途不同。

**Score function：**评分函数，为了对想要关注的元素进行评分，我们需要指定一个评分函数`f`该函数将查询和键作为输入，并输出查询-键对的得分/注意力权重。它通常通过简单的[相似性度量](https://www.zhihu.com/search?q=相似性度量&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A3254012065})来实现，例如[点积](https://www.zhihu.com/search?q=点积&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A3254012065})或MLP。
$$
\alpha_i=\frac{\exp \left(f_{\text {attn }}\left(\text { key }_i, \text { query }\right)\right)}{\sum_j \exp \left(f_{\text {attn }}\left(\text { key }_j, \text { query }\right)\right)}, \quad \text { out }=\sum_i \alpha_i \cdot \text { value }_i
$$
![img](https://pic1.zhimg.com/80/v2-5da9123d899c768f8dacad601c8872c3_720w.webp?source=2c26e567)

**如何生成**：在实际的深度学习模型中，特别是在使用Transformer架构的模型中，key、value和query通常通过对输入数据的不同表示进行线性变换得到。具体来说，对于一个给定的输入向量，模型会使用不同的权重矩阵分别生成key、value和query。这些权重矩阵是模型训练过程中学习得到的，能够将输入数据映射到最适合当前任务的key、value和query表示上。

**匹配过程**：一旦key、value和query被确定，模型将使用一个函数（通常是点积或者某种形式的加权和）来计算每个query与所有key之间的匹配程度。这个匹配程度通常通过softmax函数转换为权重，表示每个value对于回答query的重要性。最后，根据这些权重，模型将所有的value进行加权求和，得到最终的输出。



## 词嵌入（Embedding）和位置编码（Positional Encoding）

#### **词嵌入（Embedding）**
词嵌入是深度学习模型将离散的文本数据（如单词或token）转换为连续的数值向量的过程。这些数值向量表示单词的语义关系，是深度学习模型理解语言的基础。

1. **作用**：
   - **将离散数据映射为向量**：Embedding层将每个词映射为固定维度的向量，使得模型能够在高维空间中对语言进行计算。
   - **捕获语义信息**：Embedding通过学习使得语义相近的词向量在空间中更接近。例如，“king”和“queen”的词向量可能在性别维度上有所区分，但在其他维度上保持相似。

2. **实现**：
   - **嵌入矩阵**：将词表中的每个单词映射为一个固定大小的向量。假设词表大小为$V$，每个向量的维度为 $d_{model}$ ，嵌入矩阵的大小就是$V \times d_{model}$。
   - **训练学习**：Embedding层通常初始化为随机值，然后通过反向传播在训练过程中优化。

3. **结合位置编码**：
   - 词嵌入仅包含单词的语义信息，不能直接反映单词在序列中的位置，因此需要位置编码补充序列的顺序信息。

---

#### **位置编码（Positional Encoding）**
Transformer模型摒弃了传统的循环神经网络（RNN）和卷积神经网络（CNN），其架构中没有显式的序列处理能力。因此，为了捕获序列中单词的位置信息，引入了位置编码。

1. **作用**：
   - **引入序列位置信息**：通过位置编码，让模型能够感知输入序列中单词的顺序。
   - **增强上下文理解**：在捕获全局信息时，通过叠加位置编码，模型能够将每个单词的语义与其在序列中的位置结合。

2. **实现方式**：
   - **固定位置编码（Sinusoidal Positional Encoding）**：
     - 使用正弦和余弦函数生成位置向量。
     - 对于位置$pos$和维度$i$，位置编码公式为：
       $$
       PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d_{model}}}\right)
       $$
       
       $$
       PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d_{model}}}\right)
       $$
     - 这种方式确保了不同位置编码之间的规律性，模型可以学习到序列中位置的远近关系。
     
   - **可学习的位置嵌入（Learned Positional Embedding）**：
     - 将每个位置初始化为一个可学习的向量。
     - 与词嵌入类似，位置嵌入作为模型参数，在训练中被更新。
     - 这种方法在BERT、GPT等预训练语言模型中被广泛使用。
   
4. **位置编码与嵌入的结合**：
   - 在输入到模型前，将词嵌入和位置编码直接相加：
     $$
     \text{input} = \text{embedding} +\text{positional\_encoding}
     $$
     
   - 这种简单的加法操作将语义信息与位置信息结合，使得模型同时理解词语的意义和其在句子中的位置。

```python
import torch
from torch import nn

class PositionalEncoding(nn.Module):
    def __init__(self, d_model, max_len, device):
        """
        :param d_model: dimension of model
        :param max_len: max sequence length
        :param device: hardware device setting
        """
        super(PositionalEncoding, self).__init__()

        # 和输入尺寸一样
        self.encoding = torch.zeros(max_len, d_model, device=device)
        self.encoding.requires_grad = False

        pos = torch.arange(0, max_len, device=device)
        pos = pos.float().unsqueeze(dim=1)
        # 1D => 2D 代表词在句子中的位置

        _2i = torch.arange(0, d_model, step=2, device=device).float()
        # 'i' means index of d_model (e.g. embedding size = 50, 'i' = [0,50])
        # "step=2" means 'i' multiplied with two (same with 2 * i)

        self.encoding[:, 0::2] = torch.sin(pos / (10000 ** (_2i / d_model)))
        self.encoding[:, 1::2] = torch.cos(pos / (10000 ** (_2i / d_model)))
        # compute positional encoding to consider positional information of words

    def forward(self, x):
        # self.encoding
        # [max_len = 512, d_model = 512]
        batch_size, seq_len = x.size()
        # [batch_size = 128, seq_len = 30]
        return self.encoding[:seq_len, :]
        # [seq_len = 30, d_model = 512]
        # it will add with tok_emb : [128, 30, 512]
```

## self attention

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/gYUsOT36vfpYH1yUdqoQEJW1xtumvSKibLt7OlLBv0j6xRLK2ib1qdb1HRIuz2VEMwSb1hg2ZSgPlgfKPNMvvIeA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)





## 多头注意力机制





# 参考

[Self-Attention & Transformer完全指南：像Transformer的创作者一样思考][https://zhuanlan.zhihu.com/p/630356292]

