# AI Cognitive Reliability Training

最近几个月，我发现自己越来越习惯：

- AI 拉资料
- AI 做总结
- AI 给方案
- 我只看最终结果

短期效率极高。

但慢慢开始出现一个危险信号：

> 我越来越少接触原始信息了。

尤其做：

- Agent Runtime
- Eval
- Reliability
- AI 测试
- Infra

这个问题会非常危险。

因为未来真正值钱的能力，不是“会使用 AI”。

而是：

知道 AI 哪里在胡说、遗漏、偷换问题。

---

# 我的目标

不是减少 AI 使用。

而是：

> 在高频 AI 协作下，依然保持工程判断力。

我现在开始把“认知可靠性”当成一个长期训练项目。

---

# Daily Training

## 1. Raw Signal Training

规则：

AI summary 后，必须亲自看一次原始信息。

内容包括：

- GitHub issue
- log
- benchmark
- PR discussion
- paper abstract
- failure case
- system prompt

重点不是阅读量。

重点是训练：

- 异常感
- 边界意识
- 上下文理解
- 对细节的敏感度

### 记录模板

```md
## Raw Signal

AI Summary:
-

Original Context:
-

AI Missed:
-

My Judgment:
-
```

---

## 2. Prediction First

训练规则：

先自己下注，再问 AI。

例如：

- Bug root cause
- Agent failure reason
- Architecture choice
- Performance bottleneck

要求：

至少先写：

- 3 个可能性
- 1 个优先判断

然后再让 AI challenge。

### 记录模板

```md
## Prediction First

Problem:
-

My Predictions:
1.
2.
3.

Priority Judgment:
-

AI Challenge:
-

Final Result:
-

What I Missed:
-
```

---

## 3. No-AI Debug

规则：

遇到问题：

至少先自己排查 15 分钟。

不允许：

- 直接把错误贴给 AI
- 直接问“怎么修”
- 直接要最终代码

训练目标：

- log 阅读能力
- 系统隔离能力
- Runtime 故障定位能力
- failure path 思维

---

# Weekly Training

## 1. Handwritten Architecture

每周手写一个系统：

- Agent Runtime
- Memory Pipeline
- Eval Infra
- Tool Calling Chain
- Prompt Injection Defense

要求：

第一版不使用 AI。

完成后：

再让 AI challenge。

重点不是画得对。

重点是：

- 是否有状态流转意识
- 是否考虑 failure path
- 是否考虑边界条件

---

## 2. Incident Review

每周复盘一个 AI failure：

- hallucination
- context drift
- memory pollution
- eval false positive
- tool failure

### 模板

```md
# Incident

## Symptom

## Impact

## Root Cause

## Earliest Signal

## Why It Was Missed

## How To Prevent Again

## What AI Summary Would Hide
```

---

# Weekly Reflection Template

建议：

每周一篇记录。

路径：

```text
/docs/training-log/2026-05-week2.md
```

模板：

```md
# Weekly AI Cognitive Training

## Where I Over-Relied On AI

-

---

## Biggest Fake Understanding Moment

-

---

## What I Found From Original Context

-

---

## Signals AI Missed

-

---

## One Correct Independent Judgment

-

---

## What To Improve Next Week

-
```

---

# 核心变化

以前：

```text
AI -> 给答案 -> 我批准
```

现在：

```text
我 -> 做判断 -> AI challenge
```

这个位置必须调回来。

---

# 长期目标

未来 Runtime / Reliability / Eval 方向真正稀缺的人：

不是最会调用 AI 的人。

而是：

- 保持工程直觉的人
- 保持异常感的人
- 能发现“不对劲”的人
- 能在复杂系统里保持判断力的人

这些能力，不能外包给 AI。
