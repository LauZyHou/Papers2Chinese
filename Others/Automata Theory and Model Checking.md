# Automata Theory and Model Checking

> 译文仓库：https://github.com/LauZyHou/Papers2Chinese
> 库内编号：2

## 关于原文和本文

论文 DOI：10.1007/978-3-319-10575-8_4

## 更多信息

译者(GitHub)：LauZyHou

主要工具：谷歌翻译

勘误者名单(GitHub)：

## 摘要

We study automata on infinite words and their applications in system specification and verification. We first introduce Büchi automata and survey their closure properties, expressive power, and determinization. We then introduce additional acceptance conditions and the model of alternating automata. We compare the different classes of automata in terms of expressive power and succinctness, and describe decision problems for them. Finally, we describe the automata-theoretic approach to system specification and verification.

我们研究了自动机在无限字以及在系统规约和验证中的应用。我们首先介绍了 Büchi 自动机，并考察了它们的闭包性质、表达力和确定性。紧接着引入了额外的可接受条件和交互的自动机模型。我们比较了不同类型的自动机在表达能力和简洁性方面的差异，并描述了它们的决策问题。最后，我们描述了系统规约和验证的自动机理论方法。

## 4.1 介绍

> Introduction

Finite automata on infinite objects were first introduced in the 1960s. Motivated by decision problems in mathematics and logic, Büchi, McNaughton, and Rabin developed a framework for reasoning about infinite words and infinite trees [6, 52, 61]. The framework has proved to be very powerful. Automata and their tight relation to second-order monadic logics were the key to the solution of several fundamental decision problems in mathematics and logic [62, 74].

无限对象上的有限自动机最初是在 20 世纪 60 年代引入的。在数学和逻辑决策问题的推动下，Büchi，McNaughton 和 Rabin 开发了一个推理无限单词和无限树的框架[6,52,61]。事实证明，该框架非常强大。自动机及其与一元二阶逻辑的紧密关系是解决几个数学和逻辑上的基本决策问题的关键[62,74]。

Today, automata on infinite objects are used for specification and verification of nonterminating systems. The idea is simple: when a system is defined with respect to a finite set AP of propositions, each of the system’s states can be associated with a set of propositions that hold in this state. Then, each of the system’s computations induces an infinite word over the alphabet 2AP, and the system itself induces a language of infinite words over this alphabet. This language can be defined by an automaton.
今天，无限对象上的自动机用于非终止系统的规约和验证。这个想法很简单：当一个系统是针对命题的有限集合 AP 定义的时，系统的每个状态都可以与一组在这种状态满足的命题相关联。然后，系统的每个执行都会在字母表 $2^{AP}$ 上产生一个无限字，系统本身引入了这个无限字上的语言。这种语言可以用自动机定义。

Similarly, a system specification, which describes all the allowed computations, can be viewed as a language of infinite words over 2AP, and can therefore be defined by an automaton. In the automata-theoretic approach to verification, we reduce questions about systems and their specifications to questions about automata. More specifically, questions such as satisfiability of specifications and correctness of systems with respect to their specifications are reduced to questions such as non-emptiness and language containment [48, 77, 79].

类似地，描述所有允许的执行的系统规约，可以被视为在 $2^{AP}$ 上的无限字语言，因此可以用自动机定义。在自动机理论的验证方法中，我们将有关系统及其性质规约的问题转化为关于自动机的问题。更具体地说，诸如可满足性规约和系统正确性规约之类的问题被转化为诸如"集合非空"和"语言包含"之类的问题[48,77,79]。

---

The automata-theoretic approach separates the logical and the combinatorial aspects of reasoning about systems. The translation of specifications to automata handles the logic and shifts all the combinatorial difficulties to automata-theoretic problems, yielding clean and asymptotically optimal algorithms, as well as better understanding of the complexity of the problems. Beyond leading to tight complexity bounds, automata have proven to be very helpful in practice.

自动机理论的方法将系统推理的逻辑和组合方面分开。 规约到自动机的转换处理了逻辑并将所有组合困难转移到自动机理论问题，产生清晰和渐近最优的算法，并能更好地理解问题的复杂性。 除了导致严格的复杂性限制之外，自动机已经在实践中被证实非常有用。

Automata are the key to techniques such as on-the-fly model checking [11, 21], and they are useful also for modular model checking [41], partial-order model checking [23, 31, 75, 78], model checking of real-time and hybrid systems [26], open systems [1], and infinite-state systems [40, 43]. Automata also serve as expressive specification formalisms [2, 39] and in algorithms for sanity checks [37]. Automata-based methods have been implemented in both academic and industrial automated-verification tools (e.g., COSPAN [24], SPIN [27], ForSpec [72], and NuSMV [9]).

自动机是诸如运行时模型检查等技术的关键[11,21]，它们也适用于模块化模型检查[41]，偏序模型检查[23,31,75,78]，实时和混合系统的模型检查[26]，开放系统[1]和无限状态系统[40,43]。 自动机也用于表达规约形式[2,39]以及用于健全性检查的算法[37]。 基于自动机的方法已经在学术和工业界的自动验证工具中实现（例如，COSPAN [24]，SPIN [27]，ForSpec [72]和 NuSMV [9]）。

This chapter studies automata on infinite words and their applications in system specification and verification. We first introduce Büchi automata, survey their closure properties, expressive power, and determinization. We then introduce additional acceptance conditions and the model of alternating automata. We compare the different classes of automata in terms of expressive power and succinctness, and describe decision problems for them. Finally, we describe the automata-theoretic approach to system specification and verification.

本章研究无限字上的自动机及其在系统规约和验证中的应用。 我们首先介绍 Büchi 自动机，研究它们的闭包特性，表达能力和确定性。 然后我们引入额外的可接受条件和交互的自动机的模型。 我们在表达能力和简洁性方面比较不同类别的自动机，并描述它们的决策问题。 最后，我们描述了系统规约和验证的自动机理论方法。

## 4.2 无限字上的非确定性 Büchi 自动机

> Nondeterministic Büchi Automata on Infinite Words

### 4.2.1 定义

> Definitions

For a finite alphabet Σ, an infinite word w = σ1 · σ2 · σ3 · · · is an infinite sequence of letters from Σ.We use Σω to denote the set of all infinite words over the alphabet Σ. A language L ⊆ Σω is a set of words. We sometimes refer also to finite words, and to languages L ⊆ Σ∗ of finite words over Σ. A prefix of w = σ1 ·σ2 · · · is a (possibly empty) finite word σ1 · σ2 · σ3 · · · σi, for some i ≥ 0. A suffix of w is an infinite word σi · σi+1 · · ·, for some i ≥ 1. A property of a system with a set AP of atomic propositions can be viewed as a language over the alphabet $2^{AP}$. We have seen in Chap. 2 that languages over this alphabet can be defined by linear temporal-logic (LTL, for short) formulas. Another way to define languages is by automata.

对一个有限的字母表$\Sigma$而言，无限字$w =\sigma_1 \cdot \sigma_2 \cdot \sigma_3··$是取自$\Sigma$的无限字母序列。我们使用$\Sigma^\omega$表示字母表$\Sigma$上的所有无限字的集合。 语言$L \subseteq \Sigma^\omega$是一组无限字。 我们有时也会提到有限字，以及$\Sigma$上有限字的语言$L \subseteq \Sigma^*$。对于$i \geqslant 0$，$w = \sigma_1 \cdot \sigma_2 \cdot ...$的前缀是（可能是空）有限字$\sigma_1 \cdot \sigma_2 \cdot \sigma_3 \cdot ... \cdot \sigma_i$。对于$i \geqslant 1$，$w$的后缀是无限字$\sigma_i \cdot \sigma_{i+1} \cdot ...$。原子命题集合 AP 上的系统的性质可以被视为字母表$2^{AP}$上的语言。 我们在 Chap 2 中看到过，该字母表中的语言可以用线性时态逻辑（简称 LTL）公式定义。 定义语言的另一种方法是使用自动机。

A nondeterministic finite automaton is a tuple A = Σ,Q,Q0, δ,α, where Σ is a finite non-empty alphabet, Q is a finite non-empty set of states, Q0 ⊆ Q is a non-empty set of initial states, δ :Q×Σ →2Q is a transition function, and α is an acceptance condition, to be defined below.

一个非确定性有限自动机可表示为元组$\mathcal{A} = \langle \Sigma, Q, Q_0, \delta, \alpha \rangle $，其中$\Sigma$是有限非空字母表，$Q$是有限非空状态集，$Q_0 \subseteq Q$是非空的初始状态集合，$\delta : Q \times \Sigma \to 2^Q$是转移函数，$\alpha$是可接受条件，将在后面定义。

Intuitively, when the automaton A runs on an input word over Σ, it starts in one of the initial states, and it proceeds along the word according to the transition function. Thus, δ(q,σ) is the set of states that A can move into when it is in state q and it reads the letter σ . Note that the automaton may be nondeterministic, since it may have several initial states and the transition function may specify several possible transitions for each state and letter. The automaton A is deterministic if |Q0| = 1 and |δ(q,σ)| = 1 for all states q ∈ Q and symbols σ ∈ Σ. Specifying deterministic automata, we sometimes describe the single initial state or destination state, rather than a singleton set.

直观地，当自动机$A$在取自$\Sigma$中的输入字上运行时，它从某个初始状态开始，并且沿着该字依据转移函数继续执行。 因此，$\delta (q,\sigma)$是$A$处于状态$q$时读到字母$\sigma$可以转移到的状态的集合。 注意，自动机可能是不确定的，因为它可能具有若干个初始状态，并且转移函数可以为每个状态和字母指定若干个可能的转移。 如果对所有状态$q \in Q$和符号$\sigma \in \Sigma$有$|Q_0|=1$并且$|\delta (q,\sigma)|=1$，则称自动机$A$是确定的。 指定确定性自动机，我们有时会描述单个初始状态或目标状态，而不是单元素集合。

Formally, a run r of A on a finite word w = σ1 · σ2 · · · σn ∈ Σ∗ is a sequence r = q0, q1, . . . , qn of n + 1 states in Q such that q0 ∈Q0, and for all 0 ≤ i < n we have qi+1 ∈ δ(qi,σi+1). Note that a nondeterministic automaton may have several runs on a given input word. In contrast, a deterministic automaton has exactly one run on a given input word. When the input word is infinite, thus w = σ1 · σ2 · σ3 · · · ∈ Σω, then a run of A on it is an infinite sequence of states r = q0, q1, q2, . . . such that q0 ∈ Q0, and for all i ≥ 0, we have qi+1 ∈ δ(qi,σi+1). For an infinite run r, let inf (r) = {q : qi = q for infinitely many i’s }. Thus, inf (r) is the set of states that r visits infinitely often.

形式上，$A$中的有限字$w = \sigma_1 \cdot \sigma_2 \cdot ... \cdot \sigma_n \in \Sigma^*$上的执行$r$是序列$r = q_0, q_1,...,q_n$，这取自$Q$中的$n+1$个状态，其中$q_0 \in Q_0$，并且对于所有$0 \leqslant i < n$，我们有$q_{i+1} \in \delta (q_i, \sigma_{i+1})$。 请注意，非确定性自动机可能在给定输入字上有多种执行。 相反，确定性自动机在给定输入字上只有一种执行。 当输入字是无穷的时，也即$w=\sigma_1 \cdot \sigma_2 \cdot \sigma_3 \cdot ... \in \Sigma^{\omega}$，在$A$上对应的运行是无穷的状态序列$r=q_0,q_1,q_2,...$，其中$q_0 \in Q_0$，并且对于所有$i \geqslant 0$，我们有$q_{i+1} \in \delta (q_i, \sigma_{i+1})$。 对于无穷的运行$r$，记$inf(r)=\{q:q_i=q \ for \ infinity \ many \ q's\}$。 即是说，$inf(r)$是$r$无限经常次访问的状态集的集合。

The acceptance condition α determines which runs are “good”. For automata on finite words, α ⊆ Q and a run r is accepting if qn ∈ α. For automata on infinite words, one can consider several acceptance conditions. Let us start with the Büchi acceptance condition [6]. There, α ⊆ Q, and a run r is accepting if it visits some state in α infinitely often. Formally, r is accepting iff inf (r) ∩ α = ∅. A run that is not accepting is rejecting. A word w is accepted by an automaton A if there is an accepting run of A on w. The language recognized by A , denoted L(A ), is the set of words that A accepts. We sometimes refer to L(A ) also as the language of A .

可接受条件$\alpha$确定哪些运行是“好”的。 对于有限字上的自动机，如果$q_n \in \alpha$，则$\alpha \subseteq Q$和$run \ r$将被接受。 对于无限字上的自动机，可以考虑若干可接受条件。 让我们从 Büchi 可接受条件[6]开始。 在那里，$\alpha \subseteq Q$，一个运行$r$是可接受的，仅当它能无限经常次地访问$\alpha$中的某个状态。 形式上，$r$是可接受的，当且仅当$inf(r) \cap \alpha \neq \phi$。 一个运行如果不是可接受的，那就是拒绝。 一个字$w$被一个自动机$A$接受，仅当存在$A$的识别$w$的可接受运行。 $A$所识别的语言，表示为$\mathcal{L(A)}$，是$A$接受的字的集合。 我们有时也将$\mathcal{L(A)}$称为 A 的语言。

We use NBW and DBW to abbreviate nondeterministic and deterministic Büchi automata, respectively.1 For a class γ of automata (so far, we have introduced γ ∈ {NBW,DBW}), we say that a language L ⊆ Σω is γ -recognizable iff there is an automaton in the class γ that recognizes L. A language is ω-regular iff it is NBWrecognizable.

我们使用 NBW 和 DBW 分别作为非确定性和确定性的 Büchi 自动机的缩写。对于自动机的类别$\gamma$（到目前为止，我们已经引入了$\gamma \in \{NBW,DBW\}$）。称$L \subseteq \Sigma^{\omega}$是$\gamma$-可识别的，当且仅当在类$\gamma$中存在识别语言$L$的自动机。称一个语言是$ω$-正则的，当且仅当它是 NBW-可识别的。

Example 1 Consider the DBW A1 appearing in Fig. 1. When we draw automata, states are denoted by circles. Directed edges between states are labeled with letters and describe the transitions. Initial states (q0, in the ﬁgure) have an edge entering them with no source, and accepting states (q1, in the ﬁgure) are identiﬁed by double circles. The DBW moves to the accepting state whenever it reads the letter a, and it moves to the non-accepting state whenever it reads the letter b. Accordingly, the singlerun r onawordw visits the accepting state inﬁnitely often iff w has inﬁnitely many a’s. Hence, L (A1)={w:w has inﬁnitely many a’s}.

**例1** 考察图1中出现的DBW $A_1$。当我们绘制自动机时，状态用圆圈表示。状态之间的有向边用字母标记以描述转移。初始状态(图中为$q_0$)有一条不带源点的边，可接受状态(图中为$q_1$)由双圆圈标识。当DBW读到字母a时，就会转移到可接受状态，当它读到字母b时，就会移动到不可接受状态。因此，$\mathcal{L(A_1)}=\{w:w \ has \ infinity \ many \ a's\}$。

<center>
<img src='../_imgs/2/1.png'>

图1：DBW$\{w:w \ has \ infinity \ many \ a's\}$
</center>

Example 2 Consider the NBW A2 appearing in Fig. 2. The automaton is nondeterministic, and in order for a run to be accepting it has to eventually move to the accepting state, where it has to stay forever while reading b. Note that if A2 reads a from the accepting state it gets stuck. Accordingly, A2 has an accepting run on a wordw iff w has a position from which an inﬁnite tail of b’s starts. Hence, L (A2)={w:w has only ﬁnitely many a’s}.

**例2** 考虑图2中出现的NBW $A_2$。这个自动机是非确定性的，为了让运行是可接受的，它最终必须转移到可接受状态，在读取b时它必须永远停留在那里。注意，如果$A_2$从接受状态读取a，它就会被卡住。因此，A2在字w上有一个可接受的运行，当且仅当w从某一位置开始有无限长的b组成的的尾部。因此，$\mathcal{L(A2)}=\{w:w \ has \ only \ finity \ many \ a's\}$。

<center>
<img src='../_imgs/2/2.png'>

图2：DBW$\{w:w \ has \ only \ finity \ many \ a's\}$
</center>

Consider a directed graph G=V,E.Astrongly connected set of G (SCS) is aset C⊆V of vertices such that for every two vertices v,v∈C, there is a path fromv to v. An SCSC is maximal if it cannot be extended to a larger SCS. Formally, for every nonempty C⊆V \C, we have that C∪C is not an SCS. The maximal strongly connected sets are also termed strongly connected components (SCC). An automaton A =Σ,Q,Q0,δ,αinduces a directed graph GA =Q,Ein which q,q∈E iffthereisaletter σ suchthat q∈δ(q,σ).When we talk about the SCSs and SCCs of A , we refer to those of GA . Consider a run r of an automaton A . It is not hard to see that the set inf(r) is an SCS. Indeed, since every two states q and q in inf(r) are visited inﬁnitely often, the state q must be reachable from q.

考虑一个有向图$G=\langle V,E \rangle$。$G$的强连通集(SCS)是顶点集$C\subseteq V$，对于其中每两个顶点$v,v \in C$，都有一条从$v$到$v'$的路径。 如果不能将某个$SCS$扩展到更大的SCS，则称它是最大强连通集。 形式上，对于每个非空集合$C \in V \setminus C$，都有$C \cup C'$都不再是SCS。 最大强连接集也称为强连通分量(SCC)。 一个自动机$A=\langle\Sigma,Q,Q_0,\delta,\alpha\rangle$对应着一个有向图$G_A=Q,E$，其中$\langle q,q' \rangle \in E$仅当存在字母$\sigma$，使得$q \in \delta(q',\sigma)$。 当我们谈论$A$的SCS和SCC时，我们指的就是有向图$G_A$。 考虑自动机$A$的运行$r$。 不难看出集合$inf(r)$是一个SCS。 实际上，由于$inf(r)$中的任意两个状态$q$和$q'$都可以被无限经常次访问，因此状态$q'$一定可以从$q$到达。

### 4.2.2 封闭性

> Closure Properties

Automata on ﬁnite words are closed under union, intersection, and complementation. In this section we study closure properties for nondeterministic Büchi automata.

有限字上的自动机对并、交、补运算是封闭的。 在本节中，我们学习非确定性Büchi自动机中的封闭性。

#### 4.2.2.1 并和交下的封闭性

>  Closure Under Union and Intersection

We start with closure under union, where the construction that works for nondeterministic automata on ﬁnite words, namely putting the two automata “one next to the other”, works also for nondeterministic Büchi automata. Formally, we have the following.

我们从并运算之下的封闭性开始，在这种情况下，对有限字进行非确定性自动机的构造（即将两个自动机“一个接着另一个”）也适用于非确定性Büchi自动机。 形式上，有如下陈述。

Theorem 1 ([8]) Let A1 and A2 be NBWs with n1 and n2 states, respectively. There is an NBW A such that L (A ) =L (A1)∪L (A2) and A has n1 +n2 states.

**定理1** 令$A_1$和$A_2$分别是有$n_1$和$n_2$个状态的NBW。 存在一个NBW $A$，使得$\mathcal{L(A)} = \mathcal{L(A1)} \cup \mathcal{L(A2)}$且$A$具有$n_1+n_2$个状态。

Proof Let A1 =Σ,Q1,Q0 1,δ1,α1 and A2 =Σ,Q2,Q0 2,δ2,α2. We assume,without loss of generality, that Q1 and Q2 are disjoint. Since nondeterministic automata may have several initial states, we can deﬁne A as the NBW obtained by taking the union of A1 and A2. Thus, A =Σ,Q1∪Q2,Q0 1∪Q0 2,δ,α1∪α2,where for every state q ∈Q1 ∪Q2, we have thatδ(q,σ)=δi(q,σ), for the indexi ∈{1,2}suchthat q ∈Qi.It is easy to see that for every word w∈Σω,the NBW A has an accepting run on w iff at least one of the NBWs A1 and A2 has an accepting run on w.

证明，设$A_1=\langle\Sigma,Q_1,Q^0_1,\delta_1,\alpha_1\rangle$以及$A_2=\langle\Sigma,Q_2,Q^0_2,\delta_2,\alpha_2\rangle$。 在不失一般性的前提下，我们假设$Q_1$和$Q_2$是不交的。 由于非确定性自动机可以具有若干个初始状态，因此我们可以将$A$定义为通过将$A_1$和$A_2$做并运算得到的NBW。 因此，$A=\langle \Sigma,Q_1 \cup Q_2,Q^0_1 \cup Q^0_2,\delta,\alpha_1\cup\alpha_2 \rangle$，其中对于每个状态$q \in Q_1 \cup Q_2$，我们有$\delta(q,\sigma)=\delta_i(q,\sigma)$，索引$i\in\{1,2\}$使得$q\in Q_i$。很容易看到，对于每个字$w \in \Sigma^\omega$，NBW $A$在$w$上有一个可接受的运行，当且仅当NBW $A_1$和$A_2$中至少有一个在$w$上有一个可接受的运行。

We proceed to closure under intersection. For the case of ﬁnite words, one proves closure under intersection by constructing, given A1 and A2, a “productautomaton” that has Q1×Q2 as its state space and simulates the runs of both A1 and A2 on the input words. A word is then accepted by both A1 and A2 iff the product automaton has a run that leads to a state in α1×α2. As the example below demonstrates, this construction does not work for Büchi automata.

接下来我们看看在交运算下的封闭性。对于有限字下的自动机，可以这样来证明交运算的封闭性，给定$A_1$和$A_2$的情况下构造一个以$Q1×Q2$作为其状态空间的“乘积自动机”，并在输入字上模拟$A_1$和$A_2$的运行，来证明交运算下的封闭性。 $A_1$和$A_2$都接受一个字当且仅当乘积自动机运行到$\alpha_1 \times \alpha_2$的状态。 如下例所示，此构造不适用于Büchi自动机。

Example 3 Consider the two DBWs A1 and A2 on the left of Fig. 3. The product automaton A1×A2 is shown on the right. Clearly, L (A1)=L (A2)={aω}, but L (A1×A2)=∅

**例3** 考虑图3左侧的两个DBW $A_1$和$A_2$。乘积自动机$A_1 \times A_2$显示在右侧。 显然，$\mathcal{L(A_1)}=
\mathcal{L(A_2)}=\{a^\omega\}$，但是$\mathcal{L(A_1\times A_2)}=\phi$。

<center>
<img src='../_imgs/2/3.png'>

图3：两个接受语言$\{a^\omega\}$的Büchi自动机，以及它们为空的乘积
</center>

As demonstrated in Example 3, the problem with the product automaton is that the deﬁnition of the set of accepting states to be α1×α2 forces the accepting runs of A1 and A2 to visit α1 and α2 simultaneously. This requirement is too strong, as an input word may still be accepted by both A1 and A2, but the accepting runs on it visit α1 and α2 in different positions. As we show below, the product automaton is a good basis for proving closure under intersection, but one needs to take two copies of it: one that waits for visits of runs of A1 to α1 (and moves to the second copy when such a visit is detected) and one that waits for visits of runs of A2 to α2 (and returns to the ﬁrst copy when such a visit is detected). The acceptance condition then requires the run to alternate between the two copies inﬁnitely often, which is possible exactly when both the run of A1 visits α1 inﬁnitely often, and the run of A2 visits α2 inﬁnitely often. Note that A2 may visit α2 when the run is in the ﬁrst copy, in which case the visit to α2 is ignored, and in fact this may happen inﬁnitely many times. Still, if there are inﬁnitely many visits to α1 and α2, then eventually the run moves to the second copy, where it eventually comes across a visit to α2 that is not ignored. Formally, we have the following.

如例3所示，乘积自动机的问题在于，将一组接受状态定义为$\alpha_1 \times \alpha_2$会迫使$A_1$和$A_2$的可接受运行同时访问$\alpha_1$和$\alpha_2$。这个要求过强了，因为输入的单词可能仍然被$A_1$和$A_2$都接受，但是接受它的运行在不同的位置访问了$\alpha_1$和$\alpha_2$。正如我们接下来要看到的，乘积自动机是证明在交运算下封闭的良好基础，但是一个乘积需要复制两个副本：一个等待在$A_1$上访问$\alpha_1$的运行（并在检测到这种访问时，移至第二个副本）和一个等待$A_2$上访问$\alpha_2$的运行（并在检测到这种访问时，返回到第一个副本）。然后，接受条件要求可接受运行在两个副本之间无限经常次交替，这正是在说$A_1$无限经常次访问$\alpha_1$，并且$A_2$无限经常次访问$\alpha_2$。请注意，当运行在第一个副本中时，$A_2$可能会访问$\alpha_2$，在这种情况下，对$\alpha_2$的访问将被忽略，实际上，这可能会无限次发生。但是，假设无限经常此访问了$\alpha_1$和$\alpha_2$，最终运行移动到了第二个副本，在该副本中最终会遇到对$\alpha_2$的访问，这是不可忽略的。形式上，有如下陈述。

Theorem 2 ([8]) Let A1 and A2 be NBWs with n1 and n2 states, respectively. There is an NBW A such that L (A )=L (A1)∩L(A2) and A has 2n1n2 states. 

**定理2** 令$A_1$和$A_2$分别是状态数目为$n_1$和$n_2$的NBW。 存在一个NBW $A$，使得$\mathcal{L(A)}=\mathcal{L(A_1)} \cap \mathcal{L(A_2)}$，且$A$具有$2 n_1 n_2$个状态。

Proof Let A1 =Σ,Q1,Q0 1,δ1,α1 and A2 =Σ,Q2,Q0 2,δ2,α2. We deﬁne A =Σ,Q,Q0,δ,α, where

证明，设$A_1=\langle\Sigma,Q_1,Q^0_1,\delta_1,\alpha_1\rangle$以及$A_2=\langle\Sigma,Q_2,Q^0_2,\delta_2,\alpha_2\rangle$。 定义$A=\langle\Sigma,Q,Q^0,\delta,\alpha\rangle$，其中

Q = Q1 ×Q2 ×{ 1,2}. That is, the state space consists of two copies of theproduct automaton.

- $Q=Q_1 \times Q_2 \times \{1,2\}$。 也就是说，状态空间由乘积自动机的两个副本组成。

Q0 =Q0 1×Q0 2×{1}. That is, the initial states are tripless1,s2,1 such that s1and s2 are initial in A1 and A2, respectively. The run starts in the ﬁrst copy. 

- $Q^0=Q^0_1 \times Q^0_2 \times \{1\}$。 即是说，初始状态是三元组$\langle s_1,s_2,1 \rangle$，其中$s_1$和$s_2$分别在$A_1$和$A_2$中是初始状态。 运行从第一个副本中开始。

For all q1 ∈Q1, q2 ∈Q2, c ∈{ 1,2}, and σ ∈Σ, we deﬁne δ(s1,s2,c,σ)= δ1(s1,σ)×δ2(s2,σ)×{next(s1,s2,c)}, where

- 对于所有$q_1 \in Q_1$，$q_2 \in Q_2$，$c \in \{1,2\}$，以及$\sigma \in \Sigma$，我们定义$\delta(\langle s_1,s_2,c \rangle,\sigma)=\delta_1(s_1,\sigma) \times \delta_2(s_2,\sigma) \times \{next(s_1,s_2,c)\}$，其中

next(s1,s2,c)= 1 if ( c=1 and s1 / ∈α1) or ( c=2 and s2∈α2),2 if ( c=1 and s1∈α1) or ( c=2 and s2 / ∈α2). 

$$
\begin{aligned}
next(s_1,s_2,c)=
    \left[
        \begin{array}{lr}
            1 \ \ \ if(c=1 \ and \ s_1 \notin \alpha_1) \ or \ 
            (c=2 \ and \ s_2 \in \alpha_2), \\
            2 \ \ \ if(c=1 \ and \ s_1 \in \alpha_1) \ or \ 
            (c=2 \ and \ s_2 \notin \alpha_2).
        \end{array}
    \right.
\end{aligned}
$$

That is, A proceeds according to the product automaton, and it moves from the ﬁrst copy to the second copy when s1∈α1, and from the second copy to the ﬁrst copy when s2∈α2. In all other cases it stays in the current copy.

也就是说，$A$根据乘积自动机运行，当$s_1 \in \alpha_1$时，它从第一副本移动到第二副本，而当$s_2 \in \alpha_2$时，它从第二副本移动到第一副本。 在所有其他情况下，它将保留在当前副本中。

α =α1 ×Q2 ×{1}. That is, a run of A is accepting if it visits inﬁnitely many states in the ﬁrst copy in which the Q1-component is in α1. Note that after such a visit, A moves to the second copy, from which it returns to the ﬁrst copy after visiting a state in which the Q2-component is in α2. Accordingly, there must be a visit to a state in which the Q2-component is in α2 between every two successive visits to states in α. This is why a run visits α inﬁnitely often iff its Q1-component visits α1 inﬁnitely often and its Q2-component visits α2 inﬁnitely often.

$\alpha=\alpha_1 \times Q_2 \times \{1\}$。 也就是说，如果$A$遍历了第一个副本中的无穷多个状态，保证$Q_1$分量处在$α_1$中，则$A$是可接受运行。 请注意，在这样的访问之后，$A$将移动至第二个副本，在访问了$Q_2$分量处于$\alpha_2$中的状态后，它将从第二个副本返回第一个副本。 因此，必须在每两次连续访问$\alpha$中的状态之间访问$Q_2$分量处于$\alpha_2$中的状态。 这就是为什么运行无限经常此访问$\alpha$，当且仅当其$Q_1$成分无限经常次$\alpha_1$以及$Q_2$成分无限经常次访问$\alpha_2$。

Note that the product construction retains determinism; i.e., starting with deterministic A1 and A2, the product A is deterministic. Thus, DBWs are also closed under intersection. Also, while the union construction we have described does not retain determinism, DBWs are closed also under union. Indeed, if we take the product construction (one copy of it is sufﬁcient), which retains determinism, and deﬁne the set of accepting states to be (α1×Q2)∪(Q1×α2),we get a DBW for the union. Note, however, that unlike the n1+n2 blow-up in Theorem 1, the blow-up now is n1n2.

请注意，构造出的乘积保留确定性。 即，从确定性的$A_1$和$A_2$开始，乘积$A$是确定性的。 因此，DBW对交运算也是封闭的。 同样，尽管我们描述的并运算并没有保持确定性，但是DBW也在并下封闭。 的确，如果我们采用保留确定性的乘积结构（一个副本足够），并且将接受状态的集合定义为$(\alpha_1 \times Q_2) \cup (Q_1 \times α_2)$，我们将得到并后的的DBW。 但是请注意，与定理1中的$n_1+n_2$不同，现在是$n_1 n_2$。

#### 4.2.2.2 补运算下的封闭性

> Closure Under Complementation

For deterministic automata on ﬁnite words, complementation is easy: the single run is rejecting iff its last state is not accepting, thus complementing a deterministic automaton can proceed by dualizing its acceptance condition: for an automaton with state space Q and set α of accepting states, the dual acceptance condition is ˜ α=Q\α, and it is easy to see that dualizing the acceptance condition of a deterministic automaton on ﬁnite words results in a deterministic automaton for the complement language. It is also easy to see that such a simple dualization does not work for DBWs. Indeed, a run of a Büchi automaton is rejecting iff it visits α only ﬁnitely often, which is different from requiring it to visit ˜ α inﬁnitely often. As a concrete example, consider the DBW A1 from Fig. 1. Recall that L(A1)={w:w has inﬁnitely many a’s}. An attempt to complement it by deﬁning the set of accepting states to be {q0} results in a DBW whose language is {w:w has inﬁnitely many b’s}, which does not complement L (A1). For example, the word (a·b)ω belongs to both languages. In this section we study the complementation problem for Büchi automata. We start with deterministic automata and show that while dualization does not work, their complementation is quite simple, but results in a nondeterministic automaton. We then move on to nondeterministic automata, and describe a complementation procedure for them.

对于有限字的确定性自动机，补运算很容易：运行是拒绝的，当且仅当其最后一个状态不是可接受状态，因此可以通过对可接受条件进行反转来对确定性自动机进行补运算：对于具有状态空间$Q$并设置接受状态为$\alpha$的自动机，对偶的可接受条件为$\tilde{a}= Q \setminus \alpha$，并且很容易看出，将确定性自动机对有限词的接受条件反转就能得到补的语言的确定性自动机。还容易看到，这种简单的取反操作不适用于DBW。的确，如果Büchi自动机的一个运行仅有限次经常访问$\alpha$，则它是被拒绝的，这不同于要求它无限经常次访问$\tilde{a}$。作为一个具体的例子，请考察图1中的DBW $A_1$。回想一下，$\mathcal{L(A_1)}=\{w:w \ has \ inﬁnitely \ many \ a’s\}$。尝试通过将可接受状态集定义为$\{q_0\}$来对其求补，这将导致DBW的语言为$\{w:w \ has \ inﬁnitely \ many \ b’s\}$，并不能得到$\mathcal{L(A_1)}$的补。例如，字$(a·b)^{\omega}$同属于两种语言。在本节中，我们研究Büchi自动机的补运算问题。我们从确定性自动机开始，并表明虽然对偶化不起作用，但它们的补运算是相当简单的，但是会产生非确定性自动机。然后，我们继续考察非确定性自动机，并描述它们的补运算。

Theorem 3 ([47]) Let A be a DBW withn states. There is an NBW A such that L (A)=Σω \L (A ), and A has at most 2n states.

**定理3** 设$A$为具有$n$个状态的DBW。 有一个NBW $A'$，使得$\mathcal{L(A)}=\Sigma^{\omega} \setminus \mathcal{L(A)}$，并且$A$最多具有$2n$个状态。

Proof Let A =Σ,Q,q0,δ,α. The NBWA should accept exactly all words w for which the single run of A on w visits α only ﬁnitely often. It does so by guessing a position from which no more visits of A to α take place. For that, A consists of two copies of A : one that includes all the states and transitions of A , and one that excludes the accepting states of A , and to which A moves when it guesses that no more states in α are going to be visited. All the states in the second copy are accepting. Formally, A=Σ,Q,Q  0,δ,α, where

证明，设$A=\langle \Sigma,Q,q_0,\delta,\alpha \rangle$。NBW $A'$应该完全接受$A$有限经常次访问$\alpha$的那些字$w$。可以推测出一个位置，从这个位置往后$A$不再访问$\alpha$。 为此，$A'$由$A$的两个副本组成：一个副本包含$A$的所有状态和转移，一个副本排除$A$的可接受状态，并且当$A'$推测$\alpha$中没有更多的状态时，$A'$将移动到该副本。第二个副本中的所有状态都是可接受的。 形式上，$A'=\langle\Sigma,Q',Q_0',\delta',\alpha' \rangle$，其中

- $Q'=(Q \times \{0\}) \cup ((Q \setminus \alpha) \times \{1\})$.
- $Q_0'=\{\langle q_0,0 \rangle \}$.  
- 对任意的$q \in Q$,$c \in \{0,1\}$以及$\sigma \in \Sigma$满足$\delta(q,\sigma)=q'$，我们有
$$
\begin{aligned}
\delta'(\langle q,c\rangle,\sigma)=
    \left[
        \begin{array}{lr}
            \{\langle q',0 \rangle , \langle q',1 \rangle\}
            & if \ c=0 \ and \ q' \notin a, \\
            \{\langle q',0 \rangle\}
            & if \ c=0 \ and \ q' \in a, \\
            \{\langle q',1 \rangle\}
            & if \ c=1 \ and \ q' \notin a, \\
            \varnothing
            & if \ c=1 \ and \ q' \in a.
        \end{array}
    \right.
\end{aligned}
$$

- $\alpha'=(Q \setminus \alpha) \times \{1\}$.

Thus, A can stay in the ﬁrst copy forever, but in order for a run of A to be accepting, it must eventually move to the second copy, from where it cannot go back to the ﬁrst copy and must avoid states in α.

因此，$A'$可以永远停留在第一个副本中，但是要使$A'$的一个运行被接受，它必须最终移至第二个副本，在那里不能返回第一个副本，并且必须避开$\alpha$中的状态 。

The construction described in the proof of Theorem 3 can be appliedalso to nondeterministic automata. Since, however, A accepts a word w iff there exists a run of A on w that visits α only ﬁnitely often, whereas a complementing automaton should accept a word w iff all the runs of A on w visit α only ﬁnitely often, the construction has a one-sided error when applied to nondeterministic automata. This is not surprising, as the same difﬁculty exists when we complement nondeterministic automata on ﬁnite words. By restricting attention to deterministic automata, we guarantee that the existential and universal quantiﬁcation on the runs of A coincide. 

定理3的证明中描述的构造也可以应用于非确定性自动机。 但是，$A'$接受字$w$，当且仅当$A$上存在一个和$w$呼应的运行仅有限经常次访问$\alpha$，而互补的自动机应接受字$w$，当且仅当$A$上所有和$w$呼应的运行仅有限经常次访问$\alpha$，所以，当将结构应用于非确定性自动机时，该结构存在一个单方面错误。 这不足为奇，因为当我们对有限字上的非确定性自动机求补时，存在着同样的困难。 通过将注意力集中在确定性自动机上，我们保证$A$的运行上存在量词和全称量词一致。

We now turn to consider complementation for nondeterministic Büchi automata. In the case of ﬁnite words, one ﬁrst determinizes the automaton and then complements the result. An attempt to follow a similar plan for NBWs, namely at translation to a DBW and then an application of Theorem 3, does not work: as we shall see in Sect. 4.2.3, DBWs are strictly less expressive than NBWs, thus not all NBWs can be determinized. Nevertheless, NBWs are closed under complementation.

现在，我们考虑对非确定性Büchi自动机的补运算。 在有限字的情况下，首先确定自动机，然后对结果求补。 尝试在NBW上用类似的方式，即先翻译DBW，然后再应用定理3，但这是行不通的：正如我们将在第4.2.3节中看到的那样，DBW的表达能力严格不如NBW，因此并非所有NBW都可以转换成确定性的。 尽管如此，NBW在补运算下仍是封闭的。

Efforts to develop a complementation construction for NBWs started in the early 1960s, motivated by decision problems for second-order logics. Büchi introduced a complementation construction that involved a complicated Ramsey-based combinatorial argument and a doubly-exponential blow-up in the state space [6]. Thus, complementing an NBW with n states resulted in an NBW with 22O(n) states.  In [70], Sistla et al. suggested an improved implementation of Büchi’s construction, with only 2O(n2) states, which is still not optimal.Only in [64], Safra introduced a determinization construction that involves an acceptance condition that is stronger than Büchi, and used it in order to present a 2O(nlogn) complementation construction,matching the known lower bound[54].The use of complementation in practice has led to a resurgent interest in the exact blow-up that complementation involves and the feasibility of the complementation construction (e.g., issues like whether the construction can be implemented symbolically, whether it is amenable to optimizations or heuristics—these are all important criteria that complementation constructions that involve determinization do not satisfy).In [33], Klarlund introduced an optimal complementation construction that avoids determinization. Rather, the states of the complementing automaton utilize progress measures—a generic concept for quantifying how each step of a system contributes to bringing a computation closer to its speciﬁcation.In [44], Kupferman and Vardi used ranks, which are similar to progress measures, in a complementation construction that goes via intermediate alternating co-Büchi automata. Below we describe the construction of [44] circumventing the intermediate alternating automata. 

在二阶逻辑的决策问题的推动下，于1960年代初开始为NBW开发补结构。 Büchi提出了一种补结构，其中涉及到基于Ramsey的复杂组合论证和状态空间中的双指数爆炸[6]。 因此，求具有$n$个状态的NBW的补会得到具有$2^{2^{O(n)}}$个状态的NBW。在[70]中，Sistla等人。 建议对Büchi的结构进行改进的实现，只有$2^{O(n^2)}$个状态，不过这仍然不是最佳的。只有在[64]中，Safra引入了一个确定性结构，它包括了比Büchi更强的可接受条件，并用它来表示$2^{O(n \ \text{log}n)}$的补结构，这与已知的下界相匹配[54]。在实践中使用补运算已引起人们对补运算所涉及的确切的状态爆炸和补的构建可行性的兴趣（例如，诸如是否可以象征性地实现构造，是否适合优化或启发式等问题，这些都是涉及确定性的补构造不满足的重要标准）。在[33]中，Klarlund引入了避免确定性的最佳补结构。 相反，补自动机的状态利用progress measure——一种用于量化系统每个步骤如何有助于使计算更接近其规约的通用概念。在[44]中，Kupferman和Vardi在通过intermediate alternating co-Büchi自动机进行补的构造中使用了类似于progress measure的等级。 下面我们描述绕过intermediate alternating的自动机[44]的构造。

Let A =Σ,Q,Q0,δ,αbe an NBW with n states. Let w=σ1·σ2·σ3···be a word in Σω. We deﬁne an inﬁnite DAG G that embodies all the possible runs of A on w. Formally, G=V,E, where

令$A=\langle \Sigma,Q,Q_0,\delta,\alpha \rangle$是具有n个状态的NBW。 令$w=\sigma_1 \cdot \sigma_2 \cdot \sigma_3···$作为$\Sigma^\omega$中的一个单词。 我们定义了一个无限的DAG $G$，它体现了$A$在$w$上的所有可能运行。 形式上，$G=\langle V,E \rangle$，其中

- $V \subseteq Q \times \mathbb{N}$是并集$\bigcup_{l\geq 0}(Q_l \times \{l\})$，其中对所有的$l \geq 0$，有$Q_{l+1}=\bigcup_{q \in Q_1} \delta(q,\sigma_{l+1})$。

- $E \in \bigcup_{l \geq 0}(Q_l \times \{l\}) \times (Q_{l+1} \times \{l+1\})$，对所有的$l \geq 0$，我们有$E(\langle q,l \rangle , \langle q' , l+1 \rangle)$当且仅当$q' \in \delta(q,\sigma_{l+1})$。

We refer to G as the run DAG of A on w. We say that a vertexq,lis a successor of a vertex q,l iff E(q,l,q,l). We say that q,l is reachable from q,l iff there exists a sequence q0,l0,q1,l1,q2,l2,...of successive vertices such that q,l=q0,l0, and there exists i ≥0 such that q,l=qi,l i. We say thata vertex q,l is an α-vertex iff q ∈α. Finally, we say that G is an accepting runDAG if G has a path with inﬁnitely many α-vertices. Otherwise, we say that G is rejecting. It is easy to see that A accepts w iff G is accepting.

我们称$G$为$A$在$w$上的运行DAG。 我们说，顶点$\langle q',l' \rangle$是顶点$\langle q,l \rangle$的后继，当且仅当$E(\langle q,l \rangle , \langle q',l' \rangle)$。 我们称$\langle q',l' \rangle$是从$\langle q,l \rangle$可达的，当且仅当存在连续的顶点序列$\langle q_0,l_0 \rangle , \langle q_1,l_1 \rangle , \langle q_2,l_2 \rangle , ...$使得$\langle q,l \rangle = \langle q_0,l_0 \rangle$，并且存在$i \geq 0$使得$\langle q',l' \rangle = \langle q_i,l_i \rangle$。 我们说一个顶点$\langle q,l \rangle$是一个$\alpha$顶点，当且仅当$q \in \alpha$。 最后，我们说，如果$G$的路径具有无限多的$\alpha$顶点，则$G$是可接受的运行DAG。 否则，称$G$是拒绝的。 很容易发现，$A$可接受$w$当且仅当$G$是可接受的。

For k ∈ N, let[k] denote the set {0,1,...,k}.Aranking for G is a function f :V →[2n]that satisﬁes the following two conditions: 

对于$k \in \mathcal{N}$，令$[k]$表示集合$\{0,1,...,k\}$。$G$的阶是函数$f:V \to [2n]$，它满足以下两个条件：

1. 对所有顶点$\langle q,l \rangle \in V$，如果$f(\langle q,l \rangle)$是奇数个，那么$q \notin a$。
2. 对所有边$\langle\langle q,l \rangle, \langle q',l' \rangle\rangle \in E$，有$f(\langle q',l' \rangle) \leq f(\langle q,l \rangle)$。

Thus, a ranking associates with each vertex in G a rank in [2n] so that the ranks along paths decrease monotonically, and α-vertices get only even ranks. Note that each path in G eventually gets trapped in some rank. We say that the ranking f is an odd ranking if all the paths of G eventually get trapped in an odd rank. Formally, f is odd iff for all paths q0,0,q1,1,q2,2,...in G, there is j ≥0 such that f(qj,j) is odd, and for all i≥1, we have f(qj+i,j+i)=f(qj,j). 

因此，阶与$[2n]$中的$G$中的每个顶点相关联，从而沿着路径的阶是单调减少的，而$\alpha$顶点一定是偶数阶。 请注意，$G$中的每条路径最终都会进入某个阶。 我们说，如果$G$的所有路径最终都进入一个奇数阶中，则该阶$f$是一个奇数阶。 形式上，对于$G$中的所有路径$\langle q_0,0 \rangle, \langle q_1,1 \rangle, \langle q_2,2 \rangle ,...$，$f$是奇的当且仅当$j\geq 0$使得$f(\langle q_j,j \rangle$是奇的，对于所有$i \geq 1$，我们有$f(\langle q_{j+i},j+i \rangle)= f(\langle q_j,j \rangle)$。

We are going to prove that G is rejecting iff it has an odd ranking. The difﬁcult direction is to show that if G is rejecting, then it has an odd ranking. Below we make some observations on rejecting run DAGs that help us with this direction. We say that a vertexq,lis ﬁnite in a DAG G⊆G iff only ﬁnitely many vertices in G are reachable from q,l. The vertex q,l is α-free in G iff all the vertices in G that are reachable from q,l are not α-vertices. Note that, in particular, an α-free vertex is not an α-vertex. We deﬁne an inﬁnite sequence G0 ⊇G1 ⊇G2 ⊇...of DAGs inductively as follows.

我们将证明$G$是被拒绝的，前提是它的阶是奇数的。 困难的方式是展示如果$G$被拒绝，那么它的阶就是奇数的。 下面我们对拒绝运行DAG进行一些观察，这有助于我们朝这个方向推进。 我们说顶点$\langle q,l \rangle$在DAG $G' \subseteq G$中是有限的，当且仅当从$\langle q,l \rangle$只能到达有限个$G'$中的顶点。$G'$中的顶点$\langle q,l \rangle$是$\alpha-free$的，当且仅当从$\langle q,l \rangle$能到达的$G'$中的那些顶点全部都是$\alpha$顶点。 注意，特别地，$\alpha-free$顶点不是$\alpha$顶点。 我们通过归纳法定义DAG的无限序列$G_0 \supseteq G_1 \supseteq G_2 \supseteq ...$如下

- $G_0=G$. 
- 对$i \geq 0$，我们有$G_{2i+1}=G_{2i} \setminus \{\langle q,l \rangle:\langle q,l \rangle$在$G2i$中是有限的$\}$。
- 对$i\geq 0$，我们有$G_{2i+2}=G_{2i+1} \setminus \{\langle q,l \rangle:\langle q,l\rangle$在$G_{2i+1}$中是$\alpha-free$的$\}$。

Lemma 1 If G is rejecting, then for every i ≥0, there exists li such that for all l≥li, there are at most n−i vertices of the formq,lin G2i.

**引理1** 如果$G$是拒绝的，则对于每一个$i \geq 0$，都存在一个$l_i$，这样对于所有$l \geq l_i$，$G_{2i}$中最多有$n-i$个$\langle q,l \rangle$形式的顶点。

Proof We prove the lemma by an inductionon i. The case where  i=0 follows from the deﬁnition of G0 =G. Indeed, in G all levels l ≥0 have at mostn vertices of the form q,l. Assume that the lemma’s requirement holds for i; we prove it for i +1. Consider the DAG G2i. We distinguish between two cases. First, if G2i is ﬁnite,then G2i+1 isempty, G2i+2 is emptyas well, and weare done. Otherwise, we claim that there must be some α-free vertex in G2i+1.

**证明** 我们通过归纳法证明引理。 从$G_0=G$的定义得出$i=0$的情况。 实际上，在$G$中，所有级别$l \leq 0$最多具有$n$个$\langle q,l \rangle$形式的顶点。 假设引理的要求适用于$i$；我们证明$i+1$。 考虑DAG $G_{2i}$。 我们区分两种情况。 首先，如果$G_{2i}$是有限的，那么$G_{2i+1}$是空的，$G_{2i+2}$也为空，并且已经完成。 否则，我们说$G_{2i+1}$中必须有一些$\alpha-free$的顶点。

 To see this, assume, by way of contradiction, that G2i is inﬁnite and no vertex in G2i+1 is α-free. Since G2i is inﬁnite, G2i+1 is also inﬁnite. Also, each vertex in G2i+1 has at least one successor. Consider some vertex q0,l0 in G2i+1. Since, by the assumption, it is not α-free, there exists an α-vertex q 0,l 0 reachable from q0,l0.

 为了得到这一点，通过导出矛盾的方式，假设$G_{2i}$是无限的，并且$G_{2i+1}$中没有顶点是$\alpha-free$的。 由于$G_{2i}$是无限的，因此$G_{2i+1}$也是无限的。 同样，$G_{2i+1}$中的每个顶点至少具有一个后继。 考虑$G_{2i+1}$中的某个顶点$\langle q_0,l_0 \rangle$。 至此，根据假设，它不是$\alpha-free$的，因此存在一个从$\langle q_0,l_0 \rangle$可达的$\alpha$顶点$\langle q'_0,l'_0\rangle$。

 Letq1,l1 be a successorof q .By the assumption, q1,l1 is also not α-free. Hence, there exists an α-vertex q 1,l 1reachable fromq1,l1. Letq2,l2be a successor ofq 1,j 1. By theassumption, q2,l2 is also not α-free. Thus, we can continue similarly and construct an inﬁnite sequence of verticesqj,lj,q j,l jsuch that for all j, the vertex q j,l j is an α-vertex reachable from qj,lj, and qj+1,lj+1 is a successor of q j,l j. Such a sequence, however, corresponds to a path in G with inﬁnitely many α-vertices, contradicting the assumption that G is rejecting.
 
 设$\langle q_1,l_1 \rangle$为$\langle q'_0,l'_0 \rangle$的后继。根据假设，$q_1,l_1$也不是$\alpha-free$的。 因此，存在可从$\langle q_1,l_1 \rangle$到达的$\alpha$顶点$\langle q_1',l_1' \rangle$。 令$\langle q_2,l_2 \rangle$是$\langle q_1',l_1' \rangle$的后继。 根据假设，$\langle q_2,l_2 \rangle$也不是$\alpha-free$的。 因此，我们可以类似地继续并构造顶点$\langle q_j,l_j \rangle$，$\langle q_j',l_j' \rangle$的无限序列，使得对于所有$j$而言，顶点$\langle q_j',l_j' \rangle$是一个从$\langle q_j,l_j \rangle$可达的$\alpha$顶点，并且$\langle q_{j+1},l_{j+1} \rangle$是$\langle q_j',l_j' \rangle$的后继。 但是，这样的序列对应于$G$中具有无限多个$\alpha$顶点的路径，这与“$G$是拒绝的”这个假设相矛盾。

 So, letq,lbe an α-free vertex in G2i+1. We claim that taking li+1=max{l,li} satisﬁes the requirement of the lemma. That is, we claim that for all j ≥max{l,li}, there are at most n−(i +1) vertices of the form q,j in G2i+2. Since q,l is in G2i+1, it is not ﬁnite in G2i. Thus, there are inﬁnitely many vertices in G2i that are reachable fromq,l. Hence, by König’s Lemma, G2i contains an inﬁnite path q,l,q1,l+1,q2,l+2,....

因此，令$\langle q,l \rangle$为$G_{2i+1}$中$\alpha-free$的顶点。 我们称取$l_{i+1}=max\{l,l_i\}$满足引理的要求。 也就是说，我们称对于所有$j \geq max\{l，l_i\}$，在$G_{2i+2}$中至多有$n-(i+1)$个$\langle q,j \rangle$形式的顶点。 由于$\langle q,l \rangle$在$G_{2i+1}$中，因此在$G_{2i}$中不是有限的。 因此，在$G_{2i}$中无限多的顶点可以从$\langle q,l \rangle$到达。 因此，根据柯尼希（König）的引理，$G_{2i}$包含无限路径$\langle q,l \rangle ,\langle q_1,l+1 \rangle,\langle q_2,l+2 \rangle,...$


For all k ≥1, the vertex qk,l+k has inﬁnitely many vertices reachable from it in G2i and thus, it is not ﬁnite in G2i. Therefore, the path q,l,q1,l+1,q2,l+2,...exists also in G2i+1. Recall that q,l is α-free. Hence, being reachable from q,l, all the vertices qk,l+k in the path are α-free as well. Therefore, they are not in G2i+2. It follows that for all j ≥l, the number of vertices of the formq,jin G2i+2 is strictly smaller than their number in G2i. Hence, by the induction hypothesis, we are done.

对于所有$k \geq 1$，顶点$\langle q_k,l+k \rangle$在$G_{2i}$中可无限地到达其顶点，因此在$G_{2i}$中不是有限的。 因此，路径$\langle q,l \rangle ,\langle q_1,l+1 \rangle,\langle q_2,l+2 \rangle,...$也存在于$G_{2i+1}$中。 回想一下$\langle q,l \rangle ,\langle q_1,l+1 \rangle,\langle q_2,l+2 \rangle,...$是$\alpha-free$的。 因此，从$\langle q,l \rangle$可以到达，路径中的所有顶点$\langle q_k,l+k \rangle$也是$\alpha-free$的。 因此，它们不在G_{2i+2}中。 因此，对于所有$j \geq l$，$G_{2i+2}$中形式为$\langle q,j \rangle$的顶点的数量严格小于$G_{2i}$中的顶点的数量。 因此，通过归纳假设，我们完成了证明。

Note that, in particular, by Lemma 1, ifG is rejecting then G2n is ﬁnite. Hence the following corollary.

请注意，尤其是引理1，如果$G$是拒绝的，则$G_{2n}$是有限的。 因此，有以下推论。

Corollary 1 If G is rejecting then G2n+1 is empty.

**推论1** 如果$G$是拒绝的，则$G_{2n+1}$为空。

We can now prove the main lemma required for complementation , which reduces the fact that all the runs of A on w are rejecting to the existence of an odd ranking for the run DAG of A on w.

现在，我们可以证明互补所需要的主要引理，这减少了$A$对$w$的所有运行都拒绝存在$A$对$w$的DAG运行的阶是奇数的这一事实。

Lemma 2 An NBW A rejects a word w iff there is an odd ranking for the run DAG of A on w.

**引理2** NBW $A$拒绝字$w$当且仅当NBW对$A$的运行DAG在$w$上的阶为奇数。

Proof Let G betherun DAG ofA on w.We ﬁrst claim that if there is an odd ranking for G, then A rejects w. To see this, recall that in an odd ranking, every path in G eventually gets trapped in an odd rank. Hence, as α-vertices get only even ranks, it follows that all the paths of G,and thus all the possible runs of A on w, visit α only ﬁnitely often.

证明让$G$成为$w$上A的DAG。我们首先宣称，如果$G$的排名为奇数，则$A$拒绝$w$。 为此，请记住，在奇数阶中，$G$中的每条路径最终都会陷入一个奇数阶。 因此，由于$\alpha$顶点仅获得偶数阶，因此可以得出，$G$的所有路径以及$A$在$w$上的所有可能运行都仅有限经常次访问$\alpha$。

Assume now that A rejects w. We describe an odd ranking for G. Recall that if A rejects w, then G is rejecting and thus, by Corollary 1, each vertexq,lin G is removed from Gj, for some 0≤j ≤2n. Thus, there is 0≤i ≤n such thatq,lis ﬁnite in G2i or α-free in G2i+1. Given a vertexq,l, we deﬁne the rank ofq,l, denoted f(q,l), as follows.

现在假设$A$拒绝$w$。 我们描述了$G$的奇数阶。回想一下，如果$A$拒绝$w$，则$G$是拒绝的，因此，根据推论1，$G$中的每个顶点$\langle q,l \rangle$对于$0 \leq j \leq 2n$均从$G_j$中删除。 因此，存在$0 \leq i \leq n$，使得$\langle q,l \rangle$在$G_{2i}$中是有限的，在$G_{2i+1}$中是$\alpha-free$的。 给定顶点$\langle q,l \rangle$，我们定义$\langle q,l \rangle$的阶，表示为$f(q,l)$，如下所示。

$$
\begin{aligned}
f(q,l)=
    \left[
        \begin{array}{lr}
            2i
            & if \ \langle q,l \rangle \ is \ finite \ in \ G_{2i}. \\
            2i+1
            & if \ \langle q,l \rangle \ is \ \alpha-free \ in \ G_{2i+1}.
        \end{array}
    \right.
\end{aligned}
$$

We claim that f is an odd ranking for G. First, by Lemma 1, the subgraph G2n is ﬁnite. Hence, the maximal rank that a vertex can get is 2n. Also, since an α-free vertex cannot be an α-vertex and f(q,l) is odd only for α-free q,l, the ﬁrst condition for f being a ranking holds.


我们认为f是G的奇数阶。首先，根据引理1，子图$G_{2n}$是有限的。 因此，顶点可获得的最大阶为$2n$。 同样，由于$\alpha-free$的顶点不能是$\alpha$顶点，并且$f(\langle q,l \rangle)$仅对$\alpha-free$的$\langle q,l \rangle$是奇数阶，因此$f$关于阶的第一个条件成立。

 We proceed to the second condition. We ﬁrst argue (and a proof proceeds easily by an induction on i) that for every vertex q,l in G and rank i ∈[2n], ifq,l / ∈Gi, then f(q,l) i. Now, we prove thatfor every two vertices q,l and q,l in G, ifq,l is reachable from q,l (inparticular,if q,l,q,l∈E),then f(q,l)≤f(q,l).

 我们进入第二个条件。 我们首先讨论（对$i$使用归纳法很容易得到证明）的是，对于$G$中每个顶点$\langle q,l \rangle$和秩$i \in [2n]$，如果$\langle q,l \rangle \notin G_i$，则$f(q,l) < i$。 现在，我们证明对于$G$中的每两个顶点$\langle q,l \rangle$和$\langle q',l' \rangle$，如果$\langle q',l' \rangle$可以从$\langle q,l \rangle$到达（特别是，如果$\langle \langle q,l \rangle,\langle q,l \rangle \rangle \in E$），则$f(q',l') \leq f(q,l)$。

 Assumethat f(q,l)=i. We distinguish between two cases. If i is even, in which case q,l is ﬁnite in Gi, then eitherq,lis not in Gi, in which case, by the above claim, its rank is at most i−1,orq,lis in Gi, in which case, being reachable fromq,l, it must be ﬁnite in Gi and have rank i.
 
 
 假设$f(q,l)=i$。我们区分两种情况。 如果$i$是偶数，则$\langle q,l \rangle$在$G_i$中是有限的，若$\langle q',l' \rangle$不在$G_i$中，在这种情况下，根据上述主张，其阶最多为$i-1$。 若$\langle q',l' \rangle$在$G_i$中，在这种情况下，可以从$\langle q,l \rangle$到达，它必须在$G_i$中是有限的，并且阶为$i$。

  If i is odd, in which case q,l is α-free in Gi, then either q,l is not in Gi, in which case, by the above claim, its rank is at most i−1, or q,l is in Gi, in which case, being reachable from q,l, it must byα-free in Gi and have rank i. 

  如果$i$是奇数，则$\langle q,l \rangle$在$G_i$中是$\alpha-free$的，则$\langle q',l' \rangle$不在Gi中，在这种情况下，根据上述声明，其阶最多为$i-1$ ，或者$\langle q',l' \rangle$在Gi中，在这种情况下，可以从$\langle q,l \rangle$到达，它在$G_i$中必须不含$\alpha$，并且阶为$i$。
