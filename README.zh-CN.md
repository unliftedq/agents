# Agents

<p align="center"><a href="README.md">English</a> · <strong>简体中文</strong></p>

一组由 **[kman](https://github.com/unliftedq/kman)** 管理的、可移植的领域智能体。每个智能体都把自己的人格、skills、工具和权限放在独立目录里，让任务被交给合适的专家，而不是塞进一个越来越臃肿的全能助手。

## 为什么需要独立智能体

传统方式通常把所有 skill 放进同一个共享空间。一个具体任务可能只需要其中几个 skill，却不得不携带几十个无关能力：做视频的智能体拖着写代码的 skill，写代码的智能体也拖着做 PPT 的 skill。它们占用上下文、争抢注意力、抬高成本，还会让模型更容易选错工具或拿错方法。

kman 的出发点很简单：**任务需要的不是“挂满所有能力的助手”，而是“正好懂这个领域的专家”。**

- 视频制作应该交给动效和渲染专家。
- 工程任务应该交给熟悉开发流程、调试和测试的专家。
- 设计和前端打磨应该交给真正关注视觉、交互和体验的专家。

当工作跨越多个领域时，也不应该把所有能力重新塞回一个巨型智能体。更好的做法是先编排，再分派：由 orchestrator 把需求拆到合适粒度，再把每一块路由给对应专家，最后汇总结果。

## kman 智能体的边界

许多工具里的“agent”更像是一个 profile：换一段系统提示词、换一个名字，但底层仍共用同一套 skills、工具和权限。kman 关注的是更硬的边界：

| profile 式“agent” | kman 智能体 |
|-------------------|-------------|
| 所有 profile 共用一个 skill 空间 | 每个智能体只装载自己的领域 skill |
| 工具对所有 profile 全局开放 | 工具按智能体绑定和暴露 |
| 权限没有真正分层 | 权限可以按智能体单独定义 |
| 主要靠系统提示词区分 | 由人格、skills、工具和权限共同定义 |
| 任务携带大量无关上下文 | 任务面对一位聚焦专家 |

最终得到的不是“带插件的助手”，而是一组彼此隔离、可组合、可版本化的领域专家。

## 目录结构

每个智能体都是一个完整、可移植的文件夹：

```text
<agent>/
  agent.toml      # 名称、描述、运行时、soul 引用和默认配置
  soul.md         # 智能体的人格 / 系统提示词
  mcp.json        # MCP 服务器配置（如有）
  skills/         # 当前智能体自己的领域技能，每个技能包含 SKILL.md
  hooks/          # 当前智能体自己的 hooks
  scripts/        # 当前智能体自己的辅助脚本
```

核心文件的职责：

- **`agent.toml`** 定义智能体是谁、什么时候使用、默认运行时、权限模式和 soul 文件位置。
- **`soul.md`** 是智能体的人格和系统提示词。
- **`skills/`** 存放只属于当前智能体的领域知识、参考资料、模板和脚本。
- **`mcp.json`** 用于声明该智能体需要连接的 MCP 服务器。

## 新增智能体

创建新智能体时，先把领域切清楚。领域越明确，智能体越容易保持专注。

1. 确定智能体的领域和使用边界。
2. 创建以智能体命名的目录。
3. 添加 `agent.toml` manifest。
4. 编写 `soul.md`，定义人格、工作方式和约束。
5. 添加 `skills/`，只放这个领域真正需要的技能。
6. 如有需要，添加 `mcp.json`、`hooks/` 和 `scripts/`。

也可以通过 CLI 创建：

```bash
kman agent create "some-agent" \
  --runtime copilot-cli \
  --description "description of the agent" \
  --soul "soul of the agent"
```

## 使用智能体

把一次性任务交给某个智能体：

```bash
kman -a superpowers run --task "fix the failing tests"
```

进入交互模式：

```bash
kman -a superpowers chat
```

了解更多关于 kman 的信息：**[unliftedq/kman](https://github.com/unliftedq/kman)**。