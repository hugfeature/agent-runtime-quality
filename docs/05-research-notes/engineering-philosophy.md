# Engineering Philosophy

> Runtime Reliability is the next infrastructure layer of AI Engineering.

---

# 为什么这个方向重要

2024 年大家讨论的是：

- Prompt
- RAG
- Agent Workflow
- Multi-Agent

2025 年以后，问题开始变化。

越来越多团队发现：

```text
Agent 最大的问题不是能力。
而是稳定性。
```

很多 Demo 可以跑。

但上线以后会出现：

- 长任务中断
- Tool 调用漂移
- Planner 死循环
- Session 丢失
- Memory 污染
- Runtime 状态错乱
- Eval 与线上行为脱节

这些问题本质上已经不是 Prompt 问题。

它们是：

```text
Runtime System Problem
```

---

# 一个核心判断

未来 AI Agent 更像：

```text
Distributed Runtime System
```

而不是聊天机器人。

这意味着：

| 传统 AI 关注 | Runtime 时代关注 |
|---|---|
| Prompt | Runtime State |
| Accuracy | Reliability |
| Benchmark | Recovery |
| Offline Eval | Online Feedback Loop |
| Chat | Long-running Task |

---

# 为什么 Replay 很重要

传统软件：

Bug 出现后可以 Debug。

今天很多 Agent：

```text
失败以后无法复现。
```

这会导致：

- 无法定位原因
- 无法做回归
- 无法做稳定性治理
- 无法建立 Reliability 基线

所以：

```text
Replay 是 Runtime Reliability 的基础设施。
```

---

# 为什么 Observability 很重要

很多 Agent 今天属于：

```text
黑盒自动化
```

只能看到：

- 最终结果

但看不到：

- Planner Route
- Tool Decision
- Memory Mutation
- Runtime Drift
- Retry Path
- Failure Transition

没有 Observability：

就没有 Reliability。

---

# 为什么 Eval 要升级

传统 Eval：

```text
离线跑分
```

问题：

线上失败通常不在 Dataset 里。

未来更重要的是：

```text
Runtime Feedback Eval
```

即：

```text
线上失败
→ 自动生成 Eval
→ Replay
→ Regression Detection
→ Reliability 提升
```

---

# Runtime Quality 的真实目标

不是追求：

```text
Agent 永远不失败
```

而是：

```text
失败可观测
失败可复现
失败可恢复
失败可治理
```

这是两个完全不同的工程思路。

---

# 对测试工程的影响

未来 AI 测试会逐渐变化：

过去：

```text
验证功能是否正确
```

未来：

```text
约束 Runtime 行为是否稳定
```

测试工程会逐渐进入：

- Runtime Observability
- Eval Infra
- Recovery Validation
- Runtime Governance
- Reliability Engineering

---

# 当前项目的长期方向

这个项目不会做成：

- Prompt Playground
- Demo Agent
- Benchmark 排行榜

长期方向是：

```text
AI Agent Runtime Reliability Infrastructure
```

包括：

- Runtime Trace
- Replay
- Failure Taxonomy
- Online Eval
- Runtime Recovery
- Reliability Scoring
- Runtime Governance

---

# 一个现实问题

大部分团队今天还没有意识到：

```text
AI 系统已经开始进入“运维时代”。
```

后面真正稀缺的，不一定是最会调 Prompt 的人。

而是：

```text
最懂 Runtime Failure 的人。
```
