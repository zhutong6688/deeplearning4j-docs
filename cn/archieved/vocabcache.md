---
title: 词汇缓存的工作原理
layout: cn-default
---

# 词汇缓存的工作原理

词汇缓存即词汇表缓存，是Deeplearning4j用于处理TF-IDF、词向量和某些信息提取方法等自然语言分析任务的一种通用机制。词汇缓存的作用是充当文本向量化的一站式存储组件，保存词袋和词向量等模型通常需要使用的一些信息。

词汇缓存用一个倒排索引来存储词例、词频概率、逆文档概率和词在文档中的出现次数，其参考实现为InMemoryLookupCache。

为了在对文本和索引词例进行迭代时使用词汇缓存，您需要明确词例是否应当纳入词汇表中。一般的标准是词例在语料库中出现次数是否超过预设频率。如词例的出现频率低于预设值，则不把该词例纳入词汇表，而是仅仅作为词例处理。 

我们也跟踪词例。要跟踪一个词例，请这样操作：

        addToken(new VocabWord(1.0,"myword"));

如需将一个词纳入词汇表，请这样操作：

        addWordToIndex(0, Word2Vec.UNK);
        putVocabWord(Word2Vec.UNK);

首先将词添加至索引，设置索引值。然后声明这个词是词汇表中的词。（声明某个词被纳入词汇表时，该方法会从索引中调出这个词。）
