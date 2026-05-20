Engram 讨论总结

最重要的收敛

Engram 最开始容易被理解成：

Agent Memory

长期记忆

Context Persistence

Recall System


但讨论后，方向已经明显收缩。

现在更准确的定位是：

> Durable Execution Runtime for Long-Running Agents



或者：

> Recoverable Working Session for Coding Agents



核心问题不是：

Agent 记住了什么

而是：

Agent 中断后，如何继续当前工作。


---

为什么会收缩

中间有一段讨论，方向曾经开始膨胀：

trust graph

provenance system

confidence scoring

runtime verification

truth infrastructure


后来发现：

> 这会无限递归。



因为：

谁验证 verifier？

最终会掉进：

infinite trust recursion

这不是小团队第一阶段能解决的问题。


---

最关键的工程化判断

Engram 不应该解决：

世界状态是否真实

Engram 应该解决：

runtime execution continuity

也就是：

当前执行到哪

哪些 action 已发生

哪些 side effect 已提交

哪些步骤已完成

哪些 decision pending

recovery 后如何继续工作


这是一个非常大的 scope reduction。


---

核心 distinction

讨论里一个关键转折：

不要保存：

Agent 想了什么

比如：

chain of thought

hidden reasoning

latent beliefs

internal cognition

speculative plans


这些：

不稳定

replay 不可靠

模型升级后容易失效

容易 drift



---

应该保存：

Agent 真正做了什么

比如：

executed commands

modified files

tool outputs

runtime artifacts

side effects

observations


这是：

可观察

可 replay

更 durable



---

一个重要 distinction

后面又进一步收敛：

Observation ≠ Interpretation

这个非常关键。


---

可 durable 的 observation

例如：

{
  "observation": "inactive timestamp = future value"
}

这是：

runtime artifact

可观察

可验证



---

不应该 blind trust 的 interpretation

例如：

{
  "hypothesis": "future timestamp causes inactive rendering"
}

这是：

推断

reasoning result

可能错误


恢复后应该重新评估。


---

两类 Continuity 的区别

讨论里出现了一个非常重要的拆分。


---

场景1：Repo / Docs 演进

例如：

README 重构

taxonomy 调整

architecture repositioning

docs restructuring


这里真正丢失的是：

shared design consensus

也就是：

哪些决策已经做过

为什么这么做

哪些边界不能回退


这更像：

Architectural Continuity


---

场景2：长链 Bug 修复

例如：

timeline bug 修复

replay mismatch 排查

migration/debugging


这里真正丢失的是：

search tree state

包括：

哪条路试过

为什么 abandon

当前 hypothesis

当前 reasoning branch


这更像：

Exploration Continuity

两者 durable 的 state 完全不同。


---

第一阶段最真实的 workload

讨论最终收敛：

最适合 Engram v1 的真实场景：

Long-Running Coding Agent

原因：

已有真实 pain

session 中断高频

recovery 成本真实存在

side effects 可观察

task boundary 清晰

execution log 天然存在


最关键的是：

> 这是你自己真实会用的东西。




---

一个非常关键的判断

真正的 runtime intuition 来自：

“我不想重新跑这一小时。”

而不是：

“这个 architecture 很优雅。”

这个成为整个讨论的重要转折点。


---

Engram v1 最合理的范围

应该做：

task continuity

execution boundary persistence

checkpoint

resumable execution

modified file tracking

command history

pending decisions

abandoned paths

observations persistence



---

不应该现在做：

universal memory

autonomous truth systems

generalized trust infrastructure

cognition persistence

full provenance graph

confidence scoring systems


这些会快速无限膨胀。


---

一个非常重要的实践建议

讨论最后收敛出：

不要先做系统。

先做：

human-written session.json

例如：

{
  "task": "fix inactive timestamp issue",
  "phase": "timeline replay validation",
  "modified_files": [
    "timeline.ts",
    "replay.ts"
  ],
  "observations": [
    "inactive timestamps are future values"
  ],
  "abandoned_paths": [
    "frontend render issue hypothesis"
  ],
  "pending_questions": [
    "should replay normalize timestamps?"
  ]
}

然后：

真正中断一次

第二天恢复

看哪些信息有用

看哪些 state 不够

看 recovery 真正缺什么


这个过程比继续抽象 architecture 更重要。


---

最后的定位（当前阶段）

Engram 现在最合理的定义：

> Recoverable Working Session for Long-Running Coding Agents



或者：

> Durable Execution Runtime for Coding Agents



它不是：

AI Memory

Agent Consciousness Persistence

Long-term Recall System


而是：

> Runtime Continuity System for Interrupted Autonomous Workflows.