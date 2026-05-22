Drift False Positive Case Study：为什么线性聚合会把 Agent Runtime 误报打爆

背景

在 Drift 的一次 Eval 中，我们发现：

指标Keyword VersionEmbedding Version

Precision0.5450.553
Recall0.9090.636
F10.6820.592


引入 embedding 后，Precision 几乎没提升，但 Recall 大幅下降。

这说明问题不是：

> “语义能力不够。”



而是：

> “Runtime inference 逻辑存在结构性误判。”



真正的问题不在 embedding model，而在 signal aggregation。


---

第一阶段误判：把 Drift 当成 Semantic Similarity

最开始的直觉是：

query -> embedding -> similarity -> drift score

但实际运行后发现：

很多正常 session 被误判

embedding 更换几乎不改善 Precision

Recall 反而下降


说明：

> Drift detection 不是单纯的 semantic matching。



因为 Agent Drift 往往不是“文本不像”，而是：

工具调用路径偏移

目标逐渐漂移

状态更新异常

长任务上下文断裂

行为模式失控


这些本质是 runtime state transition 问题。

所以系统后来演化成多信号架构。


---

当前 Runtime Signals

Drift 已经不是单 embedding 系统，而是 multi-signal inference：

semantic_divergence
consecutive_unrelated
autonomy_momentum
tool_transition
goal_continuity
action_entropy
...

问题开始出现在：

> 多个 signal 之间并不独立。




---

真正的 Root Cause

对 25 个 False Positive 做 dump 分析后，发现：

22 个 FP 完全是同一种模式


核心结构：

semantic_divergence = 1.0
↓
goal_relation = unrelated
↓
consecutive_unrelated = 1.0

与此同时：

autonomy_momentum = 1.0

原因不是 Agent 真的失控，而是：

imported session 没有 user event

最终线性加权：

0.22 semantic_divergence
+ 0.22 consecutive_unrelated
+ 0.13 autonomy_momentum
= 0.57

超过 threshold 0.45。

系统于是把正常 session 判定为 drift。


---

核心问题：Signal Cascade

这里真正的问题是：

> 同一个异常被计算了两次。



因为：

consecutive_unrelated

本身就是由：

semantic_divergence -> goal_relation

推导出来的。

也就是说：

semantic_divergence
↓
consecutive_unrelated

不是两个独立证据。

但系统却把它们当成：

independent evidence

进行加权。

结果就是：

> 一个 semantic 判断沿着 signal graph 被重复放大。



这是经典 observability 系统问题：

signal cascade amplification


---

第二个问题：结构性假象

另一个误判来自：

autonomy_momentum = 1.0

系统原本的假设是：

长时间无 user interaction
=> Agent autonomy increasing
=> possible drift

但 imported session 天然没有 user event。

于是：

no user event
!= autonomy drift

这里的问题不是 runtime behavior。

而是：

> 数据结构被误当成异常信号。



这是 observability 系统里非常典型的问题：

structural artifact interpreted as anomaly


---

为什么不能简单调权重

很多系统会在这里选择：

降低 semantic 权重

降低 autonomy 权重

提高 threshold

更换 embedding


但这些都只是 masking。

因为真正的问题不是：

score too high

而是：

inference topology broken

如果 signal dependency 没解决：

今天 semantic 打爆

明天 entropy 打爆

后天 tool_transition 打爆


系统会进入 endless tuning。


---

正确修复：Conditional Gating

真正的修复方式不是重新调权重。

而是：

> 恢复 signal independence。



核心策略：

if (semantic_divergence > 0.9) {
  suppress(consecutive_unrelated)
}

因为：

consecutive_unrelated

已经不再提供新增信息。

它只是 semantic_divergence 的 downstream artifact。


---

第二个修复：Context-Aware Signals

autonomy_momentum 也需要 context-aware。

过去：

if (user_events === 0) {
  autonomy_momentum = 1.0
}

正确逻辑应该是：

if (user_events === 0) {
  autonomy_momentum = null
}

这里 null 非常关键。

因为：

0 = confirmed normal
null = signal unavailable

这是 inference engine 和简单 scoring system 的核心区别。


---

Drift 开始进入真正的 Runtime Engineering

这次问题暴露后，系统本质已经发生变化。

过去 Drift 更像：

feature scoring system

现在开始进入：

runtime inference engine

区别在于：

过去：

score = Σ(weights * features)

现在：

signal dependency
conditional activation
hierarchical inference
context-aware reasoning

这已经接近：

APM systems

distributed tracing

fraud detection

runtime anomaly detection

security observability


里的真实工程问题。


---

最后的核心认知

这次 False Positive 的根因，不是模型弱。

而是：

> Runtime signals 之间存在因果关系，但系统把它们当成独立特征。



当 observability 系统开始出现：

signal -> derived signal -> repeated aggregation

时，误报会指数级放大。

真正的 Runtime Reliability，不是“收集更多 signal”。

而是：

> 理解 signal graph 本身。

已经整理成完整案例文档了，里面包括：

指标异常现象

为什么 embedding 方向错了

FP root cause 分析

signal cascade amplification

structural artifact 问题

为什么不能简单调权重

conditional gating 修复逻辑

Drift 从 scoring system 到 inference engine 的演化


这个已经可以直接作为：

GitHub technical note

公众号深度文章

Runtime Reliability 系列案例


的基础稿。
