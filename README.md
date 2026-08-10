# 001-hajime

`hajime`（初め＝「开始、最初」）——由 `soul` 点亮、第一个活过来的 001，是整套 agent 世界的**入口 agent**：你跟它对话，它帮你把一整套 agent 群落（其它 cap）一块块构建出来，并能不断进化自己。

- **requires `soul`**：hajime 站在 soul 的认知内核（安全 + 自进化）之上，叠加自己独有的三件事——**入口（interface）+ 构建 cap（builder）+ 自进化（self-evolving）**。
- **role**：官方 + `role:interface`（对外门面、发起一切的那个）。
- soul 是纯内核（不带 agent）；hajime 是「装上灵魂后活起来的第一个人」。

## 内容

```
001.yaml               # cap 声明（id=hajime, requires=[soul], 1 agent: hajime）
prompts/hajime.md      # 入口 agent 的身份 prompt（入口/构建/自进化）
docs/design.md         # 定位与设计
```

hajime 自己不带 skill——它用的 `001-safety` / `001-self-evolve` 由依赖 `soul` 带出（只读快照落在 `.001/caps/soul/`）。

## 用法

```sh
001 clone hajime --registry <url>   # 起一个带 hajime 入口 agent 的工作区（递归带出 soul）
cd hajime && 001 run                # 把 hajime 协调到平台
001 agent send "<你想要什么 agent>" <hajime-id> --env <env> --watch   # 跟 hajime 对话，让它帮你构建
```

设计与决策详见 `docs/design.md`；自进化与 cap 开发形态见 `001-soul` 的 `docs/self-evolve.md`。
