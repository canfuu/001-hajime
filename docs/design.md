# 001-hajime 设计

> 状态：2026-08-09 与残风敲定。`001-hajime` 是官方能力体系里的**入口 agent cap**——由 `soul` 内核点亮的第一个、全能、自进化的 agent，作为人对话构建整套 agent 世界的入口。

## 一、为什么有 hajime（与 soul 的分工）

上午先做了 `soul`，当时把一个 agent 001 塞在 soul 里。后来定论把它拆开，因为两者定位不同：

| | soul | hajime |
| --- | --- | --- |
| 是什么 | **纯认知内核**（skill：安全 + 自进化） | **入口 agent**（一个具体的、活的 agent） |
| 带 agent 吗 | 否 | 是（`hajime`） |
| requires | `[]` | `[soul]` |
| 类比 | 「灵魂/内核」 | 「装上灵魂后活起来的第一个人」 |
| 谁用它 | 所有 cap 垫底依赖 | 人直接 clone 它、跟它对话 |

**好处**：soul 保持「谁都能垫底的纯内核」，不预设人格；hajime 承载「第一个 agent」的具体身份。要一个能对话的入口就 `clone hajime`（自动带出 soul）。

## 二、命名由来

- `hajime`（日语 はじめ / 初め＝「开始、最初」）：语义正中——**第一个** agent、万物由它而始、你从它**开始**构建一切。
- 与 `soul` 同属「精神/本源」气质家族，并排不违和；也呼应 `001` 的「第一/本体」原意（`001` 已被 CLI 仓占用，hajime 是它的精神延续）。
- 暗合武道口令 “hajime!＝开始！”——发起一切的那一声。
- 它是本体系的一个**专名**（非通用英文词）；词源在此注明一次，`self-evolve` 的官方 cap 约定表里同样登记。

## 三、hajime agent 是什么

一个 `requires: [soul]` 的 cap，声明**一个 agent**（id/name = `hajime`），绑定 soul 带出的 `001-safety` + `001-self-evolve`，全套工具。它在 soul 认知内核之上，叠加三件独有的事：

1. **入口（interface）**：人跟系统对话的唯一门面；先理解澄清再动手，「做不做」留给发起人、「怎么做/够不够」交执行体。
2. **构建 cap（builder）**：核心产出是帮人把想要的能力构建成一个个 cap——判该不该独立成 cap、设计 skill/agent/requires、就地开发 `compile` 自检、`publish` + 源仓 MR 发布；依赖以只读快照消费。
3. **自进化（self-evolving）**：能改自己，按 `001-self-evolve` 把修复回流到承载它的那一层，不踩第二次。

完整 prompt 见 `prompts/hajime.md`。

## 四、`001.yaml`

```yaml
id: hajime
version: 0.1.0
requires:
  - soul
skills:                                   # 顶层列出（compile 从 soul 依赖层解析真身）
  - 001-safety
  - 001-self-evolve
agents:
  - id: hajime
    name: hajime
    system_prompt: ./prompts/hajime.md
    skills: [001-safety, 001-self-evolve]
    model: ultimate
    tools: [Bash, Read, Write, Edit, Glob, Grep]
```

- **skill 真身在 soul，hajime 只引用不重复**：`001-safety`/`001-self-evolve` 的源在 soul cap；hajime 顶层 `skills:` 列出这些名字，compile 通过分层合并从 `.001/caps/soul/skills/` 解析到真身（本地 `skills/` 为空）。agent 的 `skills:` 再从中按名选。
- **role tag**：发布时打 `官方` + `role:interface`。

## 五、开发形态（工作区即 cap）

hajime 遵循「一个目录 = 一个 cap = 一个工作区 = 一个 git 仓」的自相似形态：

```
~/001-dev/                 # 管理目录（人自己的，非 cap）
├── 001-hajime/            # 本 cap 的源仓（在这开发）
│   ├── 001.yaml
│   ├── prompts/hajime.md
│   ├── docs/
│   └── .001/caps/soul/    # 只读依赖快照（不进 git，cap update 重拉）
├── 001-fabric/            # 用 hajime 对话构建出来的
└── …
```

先 `clone hajime → run → 对话` 跑通最小闭环，再用 hajime 逐个构建 fabric→im→access→qca。

## 六、验收

- `001 compile` 通过：`agents=1`，skills 从 soul 依赖解析到（`001-safety`/`001-self-evolve`）。
- `001 run` 后 `agent send` 能对话，回复体现「入口 + 构建 + 自进化 + 守 soul 安全底」的身份。
