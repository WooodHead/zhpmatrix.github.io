---
layout: post
title: "[NLG]基于知识图谱的主动聊天-数据篇"
excerpt: "关于2019语言与智能技术竞赛-知识驱动对话任务的讨论"
date: 2019-09-06 17:00:00
mathjax: true
---

**前言：**在比赛刚开始的时候，博主还是一个初入NLP领域的菜鸟，正值初次接触生成课题，于是基于PyTorch官方的Chatbot Tutorial实现了一个该任务的[baseline](https://github.com/zhpmatrix/lic2019-competition)，收获了一些star和fork。最近一直在做抽取相关的工作，对生成也有了一些新的理解，而该任务正好结合了二者，因此想重新思考一下该任务，并尝试做一些新的工作。围绕该任务的探讨也将以系列博客的方式呈现。博客尽可能从“第一性原理“的角度出发，希望尽可能用一种新手的观点呈现。

关于比赛形式的具体介绍可以看[官方地址](http://lic2019.ccf.org.cn/talk)，这篇博客不做过多阐述，只讨论一些需要思考的地方。

第一：为什么要在对话中融入知识图谱的信息？

个人认为是为模仿人类聊天时候的过程，话题的转移是基于对话双方的知识体系。所谓“对牛弹琴”，假设这头牛懂琴，相必也有“伯牙子期”之情吧。因此，知识图谱作为结构化知识的表达，引入对话过程几乎是一个必然的路子。所谓“有知识，有学识”的AI，在技术上的实现需要依赖知识图谱。另外一方面看，这也算是知识图谱在应用层的一个用处。

第二：融入知识图谱的对话系统怎么做？

这里从整体上建立一个认识。NLG的三大经典任务包括文本摘要，机器翻译和对话生成。当然，针对每种任务，大家都研究了很长一段时间，具体的建模方式也不尽相同。这里关注基于seq2seq的方式，也就是生成的方式。对于前两个任务，需要平行语料。输入和输出分别是单文本语句。对话的任务设定是给定人类的输入，得到机器的输出。在对话过程中，会涉及到多轮对话，这是一个过程，会形成对话历史。当只考虑最近一次人类的输出，不考虑之前人类和机器的对话历史时，在建模上和前两个任务相似。但是对话的独特性之一在于，对话历史对于机器输出的重要性影响，因此，对对话历史建模应该非常重要。这里，输入时多个语句（机器和人的对话历史），输出时单个语句（机器输出）。当融入知识图谱信息时，输入多了图谱的信息。图谱是一种基于图的数据结构，语句是基于数组的数据结构，因此需要对二者进行好的信息融合。这只是一种最朴素的观点，具体的做法后续可能会展开。

第三：图谱怎么用？

关于知识图谱本身的研究就是一件非常有意思的事情。从自己的观察来看，目前对图谱的研究多数在于对图谱本身的讨论，包括工程层面上的存储，计算，推理等，模型层面上的融合，表达，消歧，链指等，在应用层面上的研究正在进展中，特别是用于生成的研究似乎成果不多。也就是说，现在正处在一个建立知识图谱的过程中，对应用的研究似乎还没有大规模展开。从这个角度来讲，百度这个比赛是很有意义的。在任务中，对图谱的表达是SPO三元组，其实还有其他的表达方式，不过这是相对简洁，讨论相对较多的一种表示。

第四：评估方式？

目前生成的工作评估多数需要机器自动评估和人工评估相结合。自动评估指标保证不要太差，人工评估比较谁比较好，二者相结合可以确定一个生成模型的上下界，因此是目前可行的一种方式。希望有一天用机器替代上界的确定，不过在可预见的时期内，是一件比较难的事情。

**总结：**

数据篇没有讨论很多的数据，但是从整体上对所要研究的事情建立一个直观的印象。自己后续会关注比较多的生成方式，但是检索式仍旧是不可忽略的一支。因此，这里有了最简单的建模方式，基于seq2seq，需要在encoder端融入图谱的信息，或者可以理解为一个多输入的seq2seq，其余模型上的对应改变可以在之前的一些博客中找到。那么，接下来就可以考虑具体如何建模了。下一篇博文会梳理官方提供的两种建模方式，分别是生成式和检索式模型。

