# Agent Continuity vs Runtime Infrastructure

## 一个关键认知变化

最开始对 Engram 的理解，更偏向：

- Agent Memory
- Long Context Recall
- Semantic Memory
- Embedding Recall

但在连续几轮讨论后，一个更核心的问题浮现出来：

> LLM 不真正缺“记忆”，缺的是运行时连续性（Runtime Continuity）。

问题已经不是：

```text
如何让 Agent 记住更多内容
```

而是：

```text
如何让 Agent 在长时间运行、中断、重试、上下文压缩后，仍然保持任务连续性。
```

---

# 不要一开始就造 "Temporal for Agents"

一个非常重要的纠偏：

> continuity 应该自然长出 runtime，而不是先设计一个 Agent Runtime Platform。

这是 infra 项目最容易掉进去的陷阱：

```text
个人工作流痛点
-> 想象成行业级基础设施机会
-> 开始设计宏大 Runtime 架构
```

真正合理的路径应该是：

```text
continuity
    -> checkpoint
        -> replay
            -> recovery
                -> tracing
                    -> runtime reliability
```

而不是直接进入：

```text
Temporal for Agents
```

---

# Agent State != Conversation History

这是整个讨论里最重要的认知之一。

现在大量系统默认：

```text
聊天记录 = Agent 状态
```

但真实长任务里并不是。

例如一个任务：

```text
1. 查 MCP 协议
2. 修改 server
3. 更新 README
4. 提交 PR
```

运行 40 分钟后，真正重要的状态可能只有：

```json
{
  "completed_steps": [1,2],
  "current_goal": "update README examples",
  "pending_risk": "schema mismatch",
  "important_files": [
    "server.py",
    "README.md"
  ]
}
```

而不是 20 万 token 对话历史。

但这里有一个致命问题：

> 这个“状态”到底由谁生成？

如果由 LLM 生成：

```text
不可靠的系统
-> 生成 Runtime 状态
-> 再用来做可靠性基础设施
```

会进入递归不可靠。

如果由规则系统生成：

```text
什么状态真正重要？
```

对通用 Agent 很难定义。

这说明：

# “状态提取” 本身就是 Runtime 的核心难题。

---

# Tool Trace 才是第一性数据源

讨论后一个重要收敛：

> conversation / summary / reasoning 都是不可靠层。

真正可信的是：

- tool call
- file change
- retry
- interruption
- token pressure
- execution trace

因为这些是真实发生过的行为。

这和分布式系统很像：

| Agent Runtime | Distributed System |
|---|---|
| Tool Call | RPC |
| Trace | Distributed Trace |
| Replay | WAL Replay |
| Session | Workflow |
| Retry | Retry Policy |
| Context Collapse | State Corruption |

所以 Engram 第一阶段更合理的定位不是：

```text
Memory Layer
```

而是：

# Agent Runtime Recorder

或者：

# Agent Execution Blackbox

像飞机黑匣子一样。

---

# Failure Taxonomy

目前值得优先研究的是：

| Failure Type | 是否可程序检测 |
|---|---|
| Tool State Loss | YES |
| Retry Amnesia | YES |
| False Completion | YES |
| Context Collapse | YES |
| Goal Drift | HARD |
| Memory Pollution | HARD |
| Semantic State Corruption | HARD |

这个分类非常重要。

因为它划清了：

```text
哪些是 Runtime 问题
哪些是模型能力问题
```

很多系统最后失败，就是：

```text
把模型能力问题
误判成基础设施问题
```

---

# Runtime Failure Replay

目前最值得做的，不是：

```text
智能状态恢复
```

而是：

# Runtime Failure Replay

原因：

Replay 是确定性的。

它可以沉淀：

- interruption case
- retry path
- tool trace
- context collapse case
- false completion case

然后真正的 state model 才会逐渐浮现。

否则现在定义“Agent State”，大概率是在脑补。

---

# 一个现实判断

Engram 当前阶段不应该定位成：

```text
Agent Runtime Infrastructure
```

而更应该是：

# Agent Continuity Runtime

或者：

# Long-running Agent Continuity Layer

重点不是“让 Agent 更聪明”。

而是：

```text
让 Agent 在中断、压缩、重试之后，仍然能继续工作。
```

这是更真实、更窄、也更有工程价值的切入点。
