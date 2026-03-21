[English](./README.md) | **中文**

# Spec-Driven Develop

一个跨平台的 AI Agent 技能，专门为大规模复杂任务自动化"开发前准备"流程。

当你对 AI Agent 说出类似"把这个项目用 Rust 重写"或者"迁移到微服务架构"这样的话时，Agent 会在动手写代码之前，自动执行一套标准化的准备流水线：

1. 深度项目分析
2. 任务分解与规划
3. 进度追踪文档生成
4. 针对每个子任务生成专属 sub-SKILL
5. 以文档驱动的方式进行迭代开发，全程保持进度感知

整个工作流以文档为驱动核心：一个主进度文件充当 Agent 跨对话的"记忆锚点"，让它不管对话切换多少次，都不会丢失当前进展。

## 支持平台

- **Claude Code** -- 以插件形式安装（支持增强的 Agent/命令能力）
- **Codex (OpenAI)** -- 以 Skill 形式安装
- **Cursor** -- 以全局或项目级 Skill 形式安装

## 安装

### Claude Code

添加 Marketplace 并安装插件：

```
/plugin marketplace add zhu1090093659/spec_driven_develop
/plugin install spec-driven-develop@spec-driven-develop
```

安装完成后，执行 `/reload-plugins` 激活。

### Codex CLI

在 Codex 会话中使用内置的 Skill 安装器：

```
$skill-installer install https://github.com/zhu1090093659/spec_driven_develop/tree/main/plugins/spec-driven-develop/skills/spec-driven-develop
```

或者通过 Shell 脚本安装：

```bash
bash <(curl -sL https://raw.githubusercontent.com/zhu1090093659/spec_driven_develop/main/scripts/install-codex.sh)
```

### Cursor

```bash
bash <(curl -sL https://raw.githubusercontent.com/zhu1090093659/spec_driven_develop/main/scripts/install-cursor.sh)
```

也可以克隆仓库后本地安装：

```bash
git clone https://github.com/zhu1090093659/spec_driven_develop.git
bash spec_driven_develop/scripts/install-cursor.sh
```

## 使用方法

### 自动触发

直接向 Agent 描述你的大规模任务即可，Skill 会根据以下关键词自动触发：

- 英文关键词：rewrite、migrate、overhaul、refactor entire project、transform、rebuild in [language]
- 中文关键词：改造、重写、迁移、重构、大规模

### 手动触发（Claude Code）

```
/spec-dev rewrite this Python project in Rust
```

### 跨对话连续性

在跨多轮对话处理长周期任务时，Agent 会在每次新对话开始时读取 `docs/progress/MASTER.md`，恢复上下文并从上次中断的地方继续。

### 清理

当主进度文件中的所有任务都标记为完成后，Agent 会进入清理模式：它会询问你希望保留哪些产出物，然后移除其余的。

## 项目结构

```
spec_driven_develop/
├── .claude-plugin/
│   └── marketplace.json                   # Claude Code Marketplace 目录
├── plugins/spec-driven-develop/           # 独立的 Claude Code 插件
│   ├── .claude-plugin/plugin.json         # 插件清单
│   ├── skills/spec-driven-develop/
│   │   ├── SKILL.md                       # 核心 Skill（全平台通用）
│   │   └── references/doc-templates.md    # 文档模板
│   ├── agents/                            # Claude Code 子 Agent
│   │   ├── project-analyzer.md
│   │   └── task-architect.md
│   └── commands/spec-dev.md               # /spec-dev 斜杠命令
├── scripts/                               # 安装脚本
│   ├── install-cursor.sh
│   ├── install-codex.sh
│   └── install-all.sh
└── LICENSE
```

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=zhu1090093659/spec_driven_develop&type=Date)](https://star-history.com/#zhu1090093659/spec_driven_develop&Date)

## 许可证

MIT
