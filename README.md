# OpenClaw (小龙虾) 完整培训 Wiki

> 📘 **企业级 AI 助手培训体系** — 从安装部署到生产级自动化工作流，一站式掌握 OpenClaw 全部能力

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Docs](https://img.shields.io/badge/Docs-10_Tutorials-green.svg)](安装配置)
[![Stages](https://img.shields.io/badge/Training-5_Stages-orange.svg)](实战场景)
[![Scenarios](https://img.shields.io/badge/Scenarios-6_RealWorld-red.svg)](实战场景)

---

## 🎯 项目简介

**OpenClaw（小龙虾）** 是一款运行在本地的 AI 助手，支持技能管理、自动化工作流、多 Agent 协作、平台集成等功能。

本 Wiki 是一套**完整的企业培训体系**，按照"从入门到精通"的学习路径组织，涵盖 **5 大培训阶段、10 篇详细教程、约 10 万字内容、6 个企业级实战场景**，帮助团队成员快速掌握 OpenClaw 的全部能力。

### 核心能力

| 能力 | 说明 | 示例 |
|------|------|------|
| 🤖 **智能代理** | Agent 具备感知/思考/执行/记忆能力 | 理解需求、编写代码、执行测试 |
| 🛠️ **技能系统** | Skill 技能包扩展专业能力 | 数据分析、PPT 生成、爬虫 |
| ⏰ **自动化工作流** | Cron/Heartbeat/Follow-up 三种调度机制 | 每日晨报、定时监控、链式任务 |
| 👥 **多 Agent 协作** | Manager/Researcher/Writer/Reviewer 角色分工 | 代码审查、新闻流水线 |
| 🔗 **平台集成** | 接入企业微信/飞书/钉钉/Telegram/Discord | 手机随时对话、群自动推送 |
| 🧠 **本地模型** | QMD 组件支持 Ollama/llama.cpp 本地部署 | 数据不出机、零 API 费用 |

---

## 📚 目录结构

```
openclaw-start/
├── README.md                          # 项目总览（本文件）
├── 安装配置/                          # 📘 第一阶段：安装与基础概念（1 天）
│   ├── 00-安装与首次使用教程.md       #    系统要求、三种安装方式、Gateway 启动
│   ├── 01-核心概念入门教程.md         #    Agent/Session/Skill/Cron/Team 关系
│   └── 02-QMD-本地模型教程.md         #    Ollama/llama.cpp 部署、RAG、硬件选型
├── 技能管理/                          # 📘 第二阶段：技能管理（1-2 天）
│   └── 00-技能管理教程.md             #    SKILL.md 格式、ClawHub 安装、自定义开发
├── 自动化工作流/                      # 📘 第三阶段：自动化工作流（2 天）
│   ├── 00-自动化工作流教程.md         #    Cron/Heartbeat/Follow-up 三种机制
│   └── 01-多Agent-协作教程.md         #    Manager/Researcher/Writer/Reviewer 架构
├── 平台集成/                          # 📘 第四阶段：平台集成与团队协作（1 天）
│   └── 00-平台集成教程.md             #    企微/飞书/钉钉/Telegram/Discord 接入
├── 实战场景/                          # 📘 综合实战：6 个企业级场景（6-8 小时）
│   └── 00-实战场景教程.md             #    舆情监控/竞品分析/客户关系/营销/晨报/会议
└── 故障排查/                          # 📘 第五阶段：生产级运维（1 天）
    └── 00-故障排查手册.md             #    系统化排查流程、常见报错解决方案
```

### 模块统计

| 模块 | 文件数 | 大小 | 培训阶段 | 预计用时 |
|------|--------|------|---------|---------|
| 安装配置 | 3 篇 | ~41 KB | 第一阶段 | 1 天 |
| 技能管理 | 1 篇 | ~11 KB | 第二阶段 | 1-2 天 |
| 自动化工作流 | 2 篇 | ~30 KB | 第三阶段 | 2 天 |
| 平台集成 | 1 篇 | ~11 KB | 第四阶段 | 1 天 |
| 实战场景 | 1 篇 | ~10 KB | 综合实战 | 6-8 小时 |
| 故障排查 | 1 篇 | ~10 KB | 第五阶段 | 1 天 |
| **合计** | **10 篇** | **~113 KB** | **5 阶段** | **约 1 周** |

---

## 🗺️ 学习路径

### 路径 A：快速上手（适合新手，3-4 天）

```
安装配置 (00→01) → 技能管理 → 实战场景 → 开始工作
```

**目标**：能独立安装、配置技能、完成基础自动化任务

### 路径 B：系统学习（适合团队培训，1 周）

```
安装配置 (00→01→02) → 技能管理 → 自动化工作流 (00→01) → 平台集成 → 实战场景 → 故障排查
```

**目标**：全面掌握 OpenClaw 所有能力，能设计复杂工作流

### 路径 C：运维部署（适合运维/DevOps，2-3 天）

```
安装配置 (00→02) → 自动化工作流 → 平台集成 → 故障排查
```

**目标**：能完成生产环境部署、监控、问题排查

---

## 📖 核心内容速览

### 第一阶段：安装与基础概念

| 教程 | 核心内容 | 关键知识点 |
|------|---------|-----------|
| 00-安装与首次使用 | 系统要求、三种安装方式、首次配置 | npm/pnpm/国内镜像、API Key 配置、Gateway 启动 |
| 01-核心概念入门 | Agent/Session/Skill/Cron/Team 关系 | Agent 三种模式（Craft/Plan/Ask）、架构数据流 |
| 02-QMD-本地模型 | 本地模型部署与 RAG | Ollama/llama.cpp、models.json 配置、硬件选型 |

### 第二阶段：技能管理

| 教程 | 核心内容 | 关键知识点 |
|------|---------|-----------|
| 00-技能管理 | Skill 系统详解 | SKILL.md 标准格式、ClawHub 安装、自定义开发、调试测试 |

### 第三阶段：自动化工作流

| 教程 | 核心内容 | 关键知识点 |
|------|---------|-----------|
| 00-自动化工作流 | Cron/Heartbeat/Follow-up | 三种机制对比、Cron 表达式、复杂工作流组合 |
| 01-多 Agent 协作 | 多 Agent 架构 | Manager 调度、角色分工、通信方式、代码审查案例 |

### 第四阶段：平台集成

| 教程 | 核心内容 | 关键知识点 |
|------|---------|-----------|
| 00-平台集成 | 多平台接入 | 企微/飞书/钉钉/Telegram/Discord、Webhook、并发管理 |

### 综合实战：6 个企业级场景

| 场景 | 业务价值 | 核心技能 | 难度 |
|------|---------|---------|------|
| 1. 舆情监控自动化 | 品牌/行业实时监测 | Cron、Web 搜索、企微通知 | ⭐⭐ |
| 2. 竞品分析 | 自动化竞品追踪 | 多 Agent 协作、知识库 | ⭐⭐⭐ |
| 3. 客户关系管理 | 自动跟进 & 提醒 | 平台接入、记忆系统 | ⭐⭐ |
| 4. 营销内容生成 | 多平台内容分发 | 多 Agent、Cron | ⭐⭐⭐ |
| 5. 每日晨报 | 自动汇总关键信息 | Cron、数据聚合 | ⭐⭐ |
| 6. 会议自动化 | 纪要 & 待办 & 跟进 | 技能调用、任务管理 | ⭐⭐⭐ |

### 第五阶段：故障排查

| 教程 | 核心内容 | 覆盖场景 |
|------|---------|---------|
| 00-故障排查手册 | 系统化排查流程 | 安装失败、Gateway 问题、模型连接、Skill/Cron 异常、平台接入 |

**快速诊断流程**：
```
OpenClaw 出问题了？
  ↓
1. 看日志 → openclaw logs 或 tail -f ~/.openclaw/gateway/gateway.log
  ↓
2. 检查 Gateway 状态 → openclaw gateway status
  ↓
3. 检查模型连接 → curl 测试 API
  ↓
4. 检查 Skill/Cron 配置 → openclaw skills list / openclaw cron list
  ↓
5. 搜本章 → 有没有匹配的报错？
  ↓
6. 按方案解决 → 验证问题是否修复
```

---

## 💡 特色亮点

### 🎯 实战导向
- **6 个企业级场景**：舆情监控、竞品分析、客户关系、营销自动化、每日晨报、会议自动化
- **完整工作流设计**：每个场景都包含从需求分析到实施落地的完整步骤
- **真实业务价值**：不是玩具示例，而是可以直接用于生产的解决方案

### 📚 体系化培训
- **5 大培训阶段**：从安装到运维，循序渐进
- **10 篇详细教程**：约 10 万字，覆盖所有核心能力
- **明确学习目标**：每篇教程都有清晰的学习目标清单

### 🏢 企业级能力
- **多平台集成**：企业微信/飞书/钉钉/Telegram/Discord，满足各种企业环境
- **多 Agent 协作**：Manager/Researcher/Writer/Reviewer 角色分工，适合团队协同
- **生产级运维**：系统化故障排查流程，覆盖所有常见报错场景

### 🧠 灵活部署
- **云端模型**：支持百炼/智谱/OpenAI 等主流 API
- **本地模型**：QMD 组件支持 Ollama/llama.cpp，数据不出机
- **混合部署**：云端 + 本地混合使用，平衡成本与隐私

---

## 🚀 快速开始

### 前置要求

- **操作系统**：macOS / Linux / Windows (WSL2)
- **Node.js**：v20+ (推荐使用 nvm 管理)
- **内存**：至少 8GB RAM
- **磁盘空间**：至少 2GB 可用空间
- **API Key**：一个可用的模型 API Key（百炼/智谱/OpenAI 等）

### 安装步骤

```bash
# 方式 A：使用 npm（推荐）
npm install -g openclaw

# 方式 B：使用 pnpm
pnpm add -g openclaw

# 方式 C：国内镜像加速
npm install -g openclaw --registry=https://registry.npmmirror.com
```

### 首次使用

```bash
# 启动 Gateway
openclaw gateway start

# 访问 Web 界面
openclaw dashboard

# 或直接在终端使用
openclaw chat
```

### 克隆本 Wiki

```bash
git clone https://github.com/zhushuiqing/openclaw-start.git
cd openclaw-start
```

---

## 🤝 贡献指南

本 Wiki 是一套开放的企业培训体系，欢迎贡献！

### 内容规范

1. **教程结构**：每篇教程应包含学习目标、核心概念、实施步骤、常见问题
2. **代码示例**：所有代码示例必须经过测试验证
3. **截图/图表**：复杂流程应配有架构图或流程图
4. **语言风格**：简洁明了，避免冗长描述，多用表格和列表

### 提交 PR

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 提交 Pull Request

---

## 📊 项目统计

| 指标 | 数值 |
|------|------|
| 教程文档 | 10 篇 |
| 总字数 | 约 10 万字 |
| 培训阶段 | 5 个 |
| 实战场景 | 6 个 |
| 支持平台 | 企微/飞书/钉钉/Telegram/Discord |
| 支持模型 | 百炼/智谱/OpenAI/Ollama/llama.cpp |
| 文档语言 | 简体中文 |

---

## 🔗 相关资源

- **官网**：https://copilot.tencent.com/work/
- **技能商店**：内置 Skills 商店一键安装
- **ClawHub**：社区技能市场，发现更多能力
- **问题反馈**：[GitHub Issues](https://github.com/zhushuiqing/openclaw-start/issues)

---

## 📄 License

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

---

> 🦞 **OpenClaw（小龙虾）** — 本地运行、技能扩展、自动化工作流、多 Agent 协作，你的企业级 AI 助手
