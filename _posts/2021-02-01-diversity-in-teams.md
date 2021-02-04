---
layout: post
title: "团队的多样性会影响工作效率吗？"
subtitle: '以软件开发团队为例'
author: "Xiaozhe Yao"
header-style: text
tags:
  - Others
---

# 平权运动

自20世纪起，全世界的女性、少数族裔、性少数群体等掀起了平权运动。这其中有我们熟知的、马丁路德金博士的“我有一个梦想”的宣言，有社会主义的中国里“妇女能顶半边天”的口号，有“Black Lives Matter”的运动，也有“Be Proud”的 LGBT 群体的抗争。

![190.jpg](https://i.loli.net/2021/02/05/dm2BsXpvIPnJ4to.jpg)

这些一次次的运动逐渐让平权意识成为了社会的主流 —— 即使现在还不是，迟早有一天也会是。在这些运动、口号中，潜藏着人人生而平等，生而自由的潜台词。这样的潜台词早在 1776 年的《独立宣言》中就被提出，但为何时至今日，人们仍然在为“人人生而平等”这一理念而奋斗？

# 潜规则

歧视无处不在，即使在瑞士这样号称“高度发展，高度平等”的社会。这种歧视并不会表现在表面上，而成为了一种潜意识。通常大家不会把这种歧视叫做歧视，而是“潜规则”。

最明显的一个例子便是，招聘时会“潜意识”里优先选择男性。这甚至被很多人认为是“合理”的，因为男性不会因为生育而要求产假，从而可以“提高工作效率”。其余的例子还有很多，例如一些工作会更不倾向于招聘有色人种，因为担心会对客户造成心理负担。我必须检讨，在很多年前我也曾因为打车遇到黑人司机而有潜意识里“提心吊胆”的感觉。

这种潜意识里的歧视要比明面上的歧视更难避免：人们并不会告诉你，不招聘你的原因仅仅是因为有一个男性候选者，而他不会要求产假。也不会告诉你不招聘你的原因是有一位白人候选者：这种潜意识的歧视已经深深扎根在脑海里，让人们认为只要招聘女性或是少数群体便一定带来负面作用。人们把它放在脑海里，表面上不去触碰它，实则在不断加强这种固有印象。

这种潜意识的歧视甚至已经出现在了计算机算法中。例如亚马逊公司的招聘算法[被指责](https://www.reuters.com/article/us-amazon-com-jobs-automation-insight-idUSKCN1MK08G)

> Amazon.com Inc's machine-learning specialists uncovered a big problem: their new recruiting engine did not like women.
>
> 亚马逊的机器学习专家爆出一个大问题：该公司新的招聘工具不喜欢女性。

人们把对多样性的追求认为是一种“政治正确”。恶意地想的话，人们认为保持多样性，性别平衡是一种“正确而无用”的事，甚至有可能对公司造成负面影响。而“政治正确”并不能打消人们这种“正确而无用”的评价，因为它确实没有包含更多有用的信息。

这也是我们认为，对平等的追求时至今日仍然“前路漫漫”的主要原因：没有明显的好处。

# 一项调查

试图从理论上证明多样性有好处是很难的，甚至感到无从下手。我们只能从一些小例子中管中窥豹，进行一些粗浅的统计学分析。这也是我今天试图介绍的一项工作：Chen, Yingying, Weijie Niu and Xiaozhe Yao. Diversity in Open Source Software Community and its Impact on Software Quality (2019). 

在这项工作中，我们试图从一个软件开发者的角度，从统计的角度证明：从软件开发的角度，招聘一个让你的团队更多样的候选者，要利大于弊。例如：女性开发者、出生于不同国家的开发者、经验不同的开发者。更具体地来说，我们在开源社区分析了 857 个较为优秀、受到较多关注的软件项目，并分析了它们的贡献者的来源和相应的代码质量/效率。我们使用不同的尺度来衡量多样性，例如 Blau Index。

我们以性别多样性和软件质量的为例来说明：

![](https://i.loli.net/2021/02/05/3p6tsCM7SUf1lKW.png)

从上图中，我们可以比较清除地看到：尽管很多项目都是由同一性别，甚至是同一个人做出的，但更多样化的团队往往会带来更高质量的软件项目。两者是一个正相关关系。

这背后可能有很多原因，例如女性开发者和男性开发者的视角可能有所不同，对项目的关注点也不会完全一样。通常认为这种不同可能会导致沟通失调，从而拖慢进度，导致质量下降。但我们可以通过调查发现，恰恰相反，这种不同使得总的质量升高，缺陷的数目下降。我们完全认可这种不同可能导致争执甚至对抗，但是对于工作来说，它仍是利大于弊的。

# 批评声音和改进空间

我们收到了很多的评论和批评。相比于罗列这些评论，我打算讨论一些我自己的想法和过程中的缺点：

* 我们的数据量太少太少了。我们只分析了 857 个单一语言（Go）的项目，因为这个语言我熟悉，且我了解它适合团队开发。这使得图中很多项目的质量都被过高地估计了。同时，我们本以为使用该语言，且较受欢迎、比较活跃的项目会比较多，可实际上的数目（857）远远低于我们的估计（2000+）。
* 我们使用的分析工具并不能支持跨语言的分析，因此我们只能局限我们自己在单一语言。
* 我们只分析了软件开发团队和开源软件项目。这在一定程度上可以反映现状，但并不是一个全景式的分析。具体的情况很有可能根据国家、地区有所不同。实际上，我认为这种现象很有可能也会出现在其他的领域和行业，这种调查在其他行业也会是大有裨益的。但圄于时间和专业性，这些工作还是需要更专业的人去做。
* 这不是一个关于“政治正确”的分析。我们在一开始的目的是提供“政治正确”的补充，即从另一个角度的观察：多样性有实质性的帮助。我们收到了一些关于这篇文章“政治不正确”的评价，例如：“你这个调查的时候居然不允许第三种性别，这不正确”。谢谢你的建议，但我认为，仅允许第三种性别也是一种政治不正确，因为还可以有：“我虽然是个男性，但是我觉得我是个女性”等等种类。这会偏移我们的分析重心。

另外我们也收到了很多积极的评价和建议，例如：

![image-20210204215755168](https://i.loli.net/2021/02/05/WIFtJAVLMn1icwC.png)

![image-20210204215735864](https://i.loli.net/2021/02/05/cWjulmUzDqKS8AY.png)

在此一并对他们表示感谢。