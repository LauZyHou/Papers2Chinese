# Automata Theory and Model Checking

## 关于原文和本文

论文 DOI：10.1007/978-3-319-10575-8_4

## 更多信息

译者(GitHub)：LauZyHou

主要工具：谷歌翻译

勘误者名单(GitHub)：

## 摘要

We study automata on infinite words and their applications in system specification and verification. We first introduce Büchi automata and survey their closure properties, expressive power, and determinization. We then introduce additional acceptance conditions and the model of alternating automata. We compare the different classes of automata in terms of expressive power and succinctness, and describe decision problems for them. Finally, we describe the automata-theoretic approach to system specification and verification.

我们研究了自动机在无限的单词以及在系统规范和验证中的应用。我们首先介绍了 Büchi 自动机，并考察了它们的闭包性质、表达力和确定性。然后引入了附加的可接受条件和可变的自动机模型。我们比较了不同类型的自动机在表达能力和简洁性方面的差异，并描述了它们的决策问题。最后，我们描述了系统规范和验证的自动化理论方法。

## 4.1 介绍

Finite automata on infinite objects were first introduced in the 1960s. Motivated by decision problems in mathematics and logic, Büchi, McNaughton, and Rabin developed a framework for reasoning about infinite words and infinite trees [6, 52, 61]. The framework has proved to be very powerful. Automata and their tight relation to second-order monadic logics were the key to the solution of several fundamental decision problems in mathematics and logic [62, 74].

无限对象上的有限自动机最初是在 20 世纪 60 年代引入的。在数学和逻辑决策问题的推动下，Büchi，McNaughton 和 Rabin 开发了一个推理无限单词和无限树的框架[6,52,61]。事实证明，该框架非常强大。自动机及其与二阶 monadic 逻辑的紧密关系是解决几个基本问题的关键数学和逻辑中的决策问题[62,74]。

Today, automata on infinite objects are used for specification and verification of nonterminating systems. The idea is simple: when a system is defined with respect to a finite set AP of propositions, each of the system’s states can be associated with a set of propositions that hold in this state. Then, each of the system’s computations induces an infinite word over the alphabet 2AP, and the system itself induces a language of infinite words over this alphabet. This language can be defined by an automaton.
今天，无限对象上的自动机用于非终止系统的规范和验证。这个想法很简单：当一个系统是针对命题的有限集合 AP 定义的时，系统的每个状态都可以与一组保持在这种状态的命题相关联。然后，系统的每个计算都会在字母表 2AP 上产生一个无限的单词，系统本身会在这个字母表中引入无限单词的语言。这种语言可以由自动机定义。

Similarly, a system specification, which describes all the allowed computations, can be viewed as a language of infinite words over 2AP, and can therefore be defined by an automaton. In the automata-theoretic approach to verification, we reduce questions about systems and their specifications to questions about automata. More specifically, questions such as satisfiability of specifications and correctness of systems with respect to their specifications are reduced to questions such as non-emptiness and language containment [48, 77, 79].

类似地，描述所有允许的计算的系统规范，可以被视为超过 2AP 的无限单词的语言，因此可以由自动机定义。在自动机理论的验证方法中，我们将有关系统及其规范的问题规约为关于自动机的问题。更具体地说，诸如规范的可满足性和系统关于其规范的正确性之类的问题被简化为诸如 non-emptiness 和 language containment 之类的问题[48,77,79]。

---

The automata-theoretic approach separates the logical and the combinatorial aspects of reasoning about systems. The translation of specifications to automata handles the logic and shifts all the combinatorial difficulties to automata-theoretic problems, yielding clean and asymptotically optimal algorithms, as well as better understanding of the complexity of the problems. Beyond leading to tight complexity bounds, automata have proven to be very helpful in practice.

自动机方法将系统推理的逻辑和组合方面分开。 规范到自动机的转换处理逻辑并将所有组合困难转移到自动机理论问题，产生清晰和渐近最优的算法，以及更好地理解问题的复杂性。 除了导致严格的复杂性限制之外，自动机已经证明在实践中非常有用。

Automata are the key to techniques such as on-the-fly model checking [11, 21], and they are useful also for modular model checking [41], partial-order model checking [23, 31, 75, 78], model checking of real-time and hybrid systems [26], open systems [1], and infinite-state systems [40, 43]. Automata also serve as expressive specification formalisms [2, 39] and in algorithms for sanity checks [37]. Automata-based methods have been implemented in both academic and industrial automated-verification tools (e.g., COSPAN [24], SPIN [27], ForSpec [72], and NuSMV [9]).

自动机是诸如即时模型检查等技术的关键[11,21]，它们也适用于模块化模型检查[41]，偏序模型检查[23,31,75,78]，实时和混合系统的模型检查[26]，开放系统[1]和无限状态系统[40,43]。 自动机也可用作表达规范形式[2,39]和用于健全性检查的算法[37]。 基于自动机的方法已经在学术和工业自动验证工具中实现（例如，COSPAN [24]，SPIN [27]，ForSpec [72]和 NuSMV [9]）。

This chapter studies automata on infinite words and their applications in system specification and verification. We first introduce Büchi automata, survey their closure properties, expressive power, and determinization. We then introduce additional acceptance conditions and the model of alternating automata. We compare the different classes of automata in terms of expressive power and succinctness, and describe decision problems for them. Finally, we describe the automata-theoretic approach to system specification and verification.

本章研究无限词的自动机及其在系统规范和验证中的应用。 我们首先介绍 Büchi 自动机，调查它们的闭包特性，表现力和确定性。 然后我们引入额外的可接受条件和可改变的自动机的模型。 我们在表达能力和简洁性方面比较不同类别的自动机，并描述它们的决策问题。 最后，我们描述了系统规范和验证的自动机理论方法。
