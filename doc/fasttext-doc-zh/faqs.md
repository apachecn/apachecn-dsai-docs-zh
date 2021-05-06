# 常问问题

## 什么是 fastText? 有教程吗?

FastText 是一个文本分类与表示的库. 它将文本转化为能用于任何语言相关任务的连续向量. 有一些教程可供使用.

## 为什么我的 fastText 模型那么大?

FastText 使用散列表来表示单词或字符 ngram(n元模型). 散列表的大小直接影响模型的大小. 要减小模型的大小, 可以使用 '-hash' 选项, 例如一个很好的值是20000. 另一个大大影响模型大小的选项是矢量的大小 (-dim) . 可以减少此维度以节省空间, 但这可能会显著影响性能. 如果仍然产生太大的模型，可以使用量化选项进一步减小训练模型的大小. 
```bash
./fasttext quantize -output model
``` 

## 表示单词短语而不是单词的最佳方法是什么?

目前, 表示单词短语或句子的最佳方法是将单词向量的单词做成词袋. 此外, 对于诸如“纽约”这样的短语, 预处理数据以使其成为单个标记 "New_York" 可以提供极大的帮助。

## 为什么 fastText 对未知词也产生向量?

FastText 词表示的一个关键特征就是它能对任何词产生词向量, 即使是自制词. 
事实上, fastText 词向量是由包含在其中的字符字串构成的. 
这甚至允许为拼写错误的单词或拼接单词创建词向量. 

## 为什么分层 softmax 的效果比完全 softmax 效果要略差一些? 

分层 softmax 是完全 softmax 损失函数的一种近似, 它能够在大量类的数据上高效训练. 这通常会损失一些精确度. 
还要注意, 这个损失函数是针对某些类比其他类出现得更为频繁的类别不均衡情况的. 如果你的数据集中各类的样本均衡, 那么值得尝试一下负采样损失 (-loss ns -neg 100). 
然而, 负采样在测试时仍然会非常慢, 因为会计算完全 softmax. 

## 我们可以在 GPU 上运行 fastText 程序吗?

FastText 由于可访问性只工作于 CPU. 就是说, fastText 已经可以在能运行于 GPU 的 caffe2 库中实现.

## 我能用 python 语言使用 fastText 吗? 或者其他语言?

Github 上几乎没有非官方的 python 或者 lua 包装器. 

## 我能用 fastText 处理连续数据吗?

FastText适用于离散标记, 因此不能直接用于连续标记. 但是, 可以将连续标记离散化以对其使用fastText, 例如将值四舍五入为特定数字 ("12.3" 变为 "12"). 

## 词典中一些错误拼写的词. 我们应该提升文本规范化吗?

如果这些词出现频率不高, 无须理会.

## 我遇到了 NaN, 为什么会这样呢?

你出现这个情况可能是因为学习率太高. 尝试减小学习率直到看不到这个错误. 

## 我的编译器 / 体系结构无法构建 fastText. 我该怎么办?

尝试新版本的编译器. 我们试图保持与老版本 gcc 和很多平台的兼容性, 然后有时候保持后端兼容变得非常难. 
一般来说, 附带 LTS 版本的主要 linux 发行版的编译器和工具链应该都是没问题的. 遇到任何情况, 都可以创建一个你的编译器版本和体系结构的 issue(问题, 指在github上提出), 我们将尽力实现兼容性. 



