# SysML Models and Model Transformation for Security

## 关于原文和本文

论文原文：[下载](https://perso.telecom-paristech.fr/apvrille/docs/Modelswards2016_Lugou_paper.pdf)

避免迷惑，Safety和Security作为表特定含义的词，在本文的某些地方出现时原样保留。

## 更多信息

译者(GitHub)：LauZyHou

辅助工具：网易有道词典

勘误者名单(GitHub)：

## 关键词

SysML-Sec, Security, Model-driven engineering, Model transformation, ProVerif, TTool

## 摘要

嵌入式系统的安全漏洞已经成为网络犯罪分子的有价值目标。引入SysML-Sec是为了在这些系统的开发过程中来保证其安全性。然而，在这些阶段评估对攻击的抵抗能力需要有效地捕获系统的行为，并从这些行为中对安全性属性进行形式化证明。因此，本文提出(i)新的SysML模块和状态机图，以更好地捕获Security特征，(ii)模型到ProVerif的转换。ProVerif是第一个发布用于安全协议的形式化分析的工具包，但它可以更广泛地用于评估保密性(Confidentially)和真实性(Authenticity)属性。本文用一个复杂的非对称密钥分发协议证明了该方法的有效性。

---

# 1 介绍

连接嵌入式系统和物联网系统的比例越来越高，为网络犯罪分子提供了更多的攻击机会。仅举几个过去攻击此类连接系统的例子，我们可以谈及ADSL路由器(Assolini, 2012)、移动电话&智能手机(Maslennikov, 2010)、航空电子或汽车系统(Hoppe等，2011)和智能对象，例如Fitbit最近暴露的漏洞(Apvrille, 2015)。医疗器械的安全漏洞甚至已经被披露，例如Hospira Symbiq加药泵(ICS-CERT, 2015)。这种攻击还针对工业系统，这些系统的传感器越来越多地与易受攻击的信息系统相连接，Stuxnet、Flame和Duqu (Maynor, 2006)攻击就证明了这一点。这些系统的可靠性有不同的目标，例如恐怖分子和勒索软件。

> 译者注："加药泵"(drug pump)，或叫"给药泵"、"注药泵"是一种医疗器械。

在代码大小、分布以及异构等其他方面引起的系统复杂性是一个主要的风险因素。要更好地考虑这些系统的风险，一个解决方案是在开发周期中考虑它们的所有约束，包括安全性。我们之前介绍了SysML-Sec环境来处理此类复杂系统的设计，包括Safety、性能和Security(Apvrille和Roudier, 2015)。SysML-Sec从需求和可能的攻击着手处理系统开发，顾及软件/硬件的划分。在划分阶段之后，SysML-Sec还支持软件组件的设计，同样考虑到了Safety和Security。TTool是支撑SysML-Sec的一款免费且开源的工具(Apvrille, 2003)。

> 译者注：heterogeneity多相性，这里据语境翻译成了异构性。

TTool依赖于UPPAAL进行Safety的证明，并依赖于ProVerif从SysML-Sec框图执行Security的证明。然而SysML-Sec有些很严重的限制，在建模方面(即对Security特征建模)以及在证明方面(模型到proverif的转换中的限制)。本文使用新的建模和转换方法解决了其中的一些限制。特别是，我们已经完全形式化并实现了模型转换。

下一节给出了有关Security的建模和验证环境方面的相关工作。第三节介绍了SysML-Sec。第4节关注建模扩展以及模型到proverif的转换。第5节介绍了TTool的更新版本和验证特性。最后，第六部分对全文进行了总结。

# 2 相关工作

在设计软件组件时，评估Security属性主要依赖于形式化方法。例如，(Toussaint, 1993)提出用概率分析方法验证加密协议。协议被表示为树，树的节点捕获知识，边被分配转移概率。尽管这些树可能包含恶意代理，以便对攻击和威胁进行建模，但仍然没有显式地表示Security属性。此外，对于威胁分析，攻击应该明确表示，并手动解决。(Trcek和Blazic, 1995)为实现Security目标定义了一组形式上的的基本Security服务。在这种方法中，对Security属性的分析强烈依赖于设计者的经验。此外，威胁评估并不容易实现。

在更多最近的研究中，时态逻辑语言被用于表示安全属性。(Drouineaud et al., 2004)将一阶线性时态逻辑(LTL)引入到Isabelle/HOL定理证明程序中，从而使对系统及其Security属性建模成为可能，但不幸的是，这会产生不易复用的特定模型。(Mana and Pujol, 2008) 混合了形式化的以及非形式化的Security属性，但整个验证过程并非完全自动化，需要专门的技能。类似地，软件架构建模(SAM)框架(Ali et al., 2009)旨在填补非形式化的Security需求与其形式化表示和验证之间的鸿沟。SAM使用形式化和非形式化的Security技术来完成目标的定义和缺陷的缓解。因此，依赖于符号模型验证器(SMV)，可以在LTL模型上验证liveness和deadlock-freedom properties。即使SAM依赖于一个成熟的工具包(SMV)，然后处理一个威胁模型，"从Security属性到证明"的过程也不是自动的。

> 译者注：这里deadlock-freedom可以翻译为无死锁性。

UMLsec (Jurjens, 2007)是一个建模框架，旨在定义软件组件及其组成在UML框架中的Security属性。它还提供了一个相当完整的框架，用于处理模型驱动的安全软件工程的各个阶段，从Security需求的规格说明书到测试，包括关于软件组件的组成的基于逻辑的形式化验证。

最近，(Shen et al, 2014)开发了一个扩展的UML模型，该模型扩展了用于安全协议验证的UML序列图。他们的方法还包括将模型转换为ProVerif以验证保密性和correspondance。然而，我们的工作包括状态图，以用于为更广泛的协议建模。基本序列图可以只对单个执行过程进行建模，而状态图可以对包含if语句和循环的协议进行建模。此外，我们的过程包括验证弱和强的真实性(authenticity)。

> 译者注：correspondance不知道如何翻译。

关于我们以前发表的内容，我们提出了一种更好的模型状况(例如循环)及其模型到ProVerif转换的方法，考虑到了ProVerif的功能和限制。因此，我们设法限制Security属性证明失败的那些case，而不影响SysML-Sec图的Safety证明的功能。

