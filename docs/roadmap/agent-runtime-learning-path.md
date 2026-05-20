# Agent Runtime Learning Path

> 从 AI 测试 / AI 应用工程，进入 Agent Runtime / Reliability / Eval Infra。

这不是一条“学 Prompt”的路线。

这是：

> 从 AI Application 进入 AI Runtime Systems 的路线。

---

# 一个关键判断

未来最值钱的问题，正在从：

- 模型够不够聪明

转向：

- Agent 能不能长期稳定运行

所以真正重要的能力会逐渐变成：

- Runtime
- Reliability
- Eval
- Observability
- Recovery
- Multi-Agent Coordination

而不是单纯 Prompt Engineering。

---

# 学习路线总览

```text
LLM Fundamentals
    ↓
Workflow Agents
    ↓
Multi-Agent Systems
    ↓
Runtime State Management
    ↓
Reliability Engineering
    ↓
Replay / Recovery Systems
    ↓
Runtime Observability
    ↓
Eval Infrastructure
    ↓
Autonomous Runtime Architecture
```

---

# Stage 1 — Agent Fundamentals

目标：

理解 Agent 不等于 ChatBot。

重点：

- Tool Calling
- Workflow Orchestration
- Planning vs Execution
- Memory vs State
- Structured Output
- Context Window Limits

建议关注：

- OpenAI Agents
- Anthropic Computer Use
- LangGraph
- CrewAI
- AutoGen

这一阶段不要沉迷：

- Prompt 技巧
- benchmark 排名
- roleplay agent

重点是：

> Agent 如何执行任务。

---

# Stage 2 — Multi-Agent Systems

目标：

理解为什么单 Agent 会撞墙。

重点：

- planner / executor / critic 模式
- agent coordination
- delegation
- agent hierarchy
- shared state
- agent communication

关键问题：

- 多 Agent 如何共享状态？
- 如何避免 state divergence？
- 如何避免 recursive planning？
- 如何做 agent governance？

建议阅读方向：

- actor model
- distributed workflow systems
- event-driven architecture

因为：

> Multi-Agent 本质越来越像分布式系统。

---

# Stage 3 — Runtime Systems

这是整个路线真正的核心。

目标：

理解 Agent 真正困难的不是推理。

而是：

> Runtime Continuity。

重点学习：

- runtime state
- checkpoints
- replay systems
- recovery pipeline
- event sourcing
- resumable execution
- runtime durability
- workflow interruption

关键问题：

- Agent 中断后如何恢复？
- 如何 replay failed trajectory？
- 如何恢复长期任务？
- 如何做 deterministic replay？
- 如何做 runtime persistence？

建议重点研究：

- Temporal
- Durable Execution
- Event Sourcing
- Workflow Engine

这是 Agent Runtime 最接近传统 Infra 的部分。

---

# Stage 4 — Reliability Engineering

目标：

理解为什么 Agent Demo 不等于 Production。

重点：

- retry storms
- timeout handling
- idempotency
- state corruption
- memory poisoning
- partial failure
- recovery inconsistency
- cascading failure

关键问题：

- 为什么 Agent 会 drift？
- 为什么 recovery 后状态会错乱？
- 为什么长 session 后开始失控？
- 为什么 replay 后结果不同？

这里会越来越像：

- SRE
- distributed systems reliability
- fault tolerance

---

# Stage 5 — Runtime Observability

目标：

让 Agent Runtime 可调试。

重点：

- runtime traces
- trajectory logging
- state transitions
- replay visualization
- runtime metrics
- execution graph
- failure reconstruction

关键问题：

- Agent 为什么失败？
- 哪一步开始 drift？
- 哪个 tool call 导致污染？
- recovery 为什么失效？

建议研究：

- OpenTelemetry
- tracing systems
- observability pipeline
- distributed tracing

因为未来 Agent Runtime：

> 会越来越像复杂分布式系统。

---

# Stage 6 — Eval Infrastructure

目标：

建立 Runtime 级别 Eval。

不是只评测模型。

而是评测：

- continuity
- recoverability
- replayability
- drift resistance
- multi-agent stability
- governance quality

重点：

- runtime fixtures
- long-session eval
- replay benchmark
- synthetic failure injection
- trajectory grading
- runtime scoring

这是未来很重要的一层。

因为：

> Agent 真正的问题发生在 Runtime，而不是单轮输出。

---

# 一个重要认知

不要把自己定位成：

```text
AI 应用开发
```

而是：

```text
AI Runtime Infrastructure
```

这两个方向完全不同。

前者更偏：

- UI
- workflow assembly
- product iteration

后者更偏：

- runtime systems
- reliability
- orchestration
- distributed execution
- recovery semantics
- observability

未来真正稀缺的，很可能是后者。

---

# 适合长期深耕的方向

结合当前 Agent 发展趋势，长期值得投入的方向：

- Runtime Reliability
- Runtime Eval
- Multi-Agent Coordination
- Agent Observability
- Replay Systems
- Recovery Infrastructure
- Runtime Governance
- Human Takeover Systems
- Runtime Drift Detection

这些方向现在还非常早。

但会越来越重要。

---

# 最后

过去几年：

> AI 在追求“更聪明”。

接下来几年：

> 行业会开始追求“更稳定”。

而 Runtime、Reliability、Eval，都会逐渐从边缘问题，变成 Agent 时代的核心基础设施。
