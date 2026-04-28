# 📘 QMD 本地模型教程

> **培训阶段**：第三阶段 - 自动化工作流（2 天）
> **子模块**：QMD 使用 — 本地模型部署
> **预计用时**：2-3 小时

---

## 📋 学习目标

完成本教程后，你将能够：
- ✅ 理解 QMD 在 OpenClaw 架构中的定位与作用
- ✅ 独立部署 Ollama / llama.cpp 本地推理服务
- ✅ 正确配置 `models.json` 接入本地模型
- ✅ 根据硬件条件选择合适的模型尺寸
- ✅ 排查本地模型连接与性能问题
- ✅ 对比云端模型与本地模型的适用场景

---

## 一、QMD 与本地模型架构概览

### 1.1 什么是 QMD？

QMD（Query Model Dispatcher）是 OpenClaw 的语义检索与本地模型调度组件。它在自动化工作流中扮演以下角色：

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│   OpenClaw   │────▶│   QMD 调度    │────▶│  本地 LLM    │
│   Gateway    │     │              │     │ (Ollama等)   │
└─────────────┘     └──────────────┘     └─────────────┘
                           │
                    ┌──────┴──────┐
                    │  语义检索库   │
                    │  (向量索引)   │
                    └─────────────┘
```

**核心功能**：
- 将 Agent 请求路由到本地模型端点
- 管理模型上下文窗口与 Token 限制
- 配合向量检索实现 RAG（检索增强生成）

### 1.2 本地模型 vs 云端模型对比

| 维度 | 云端模型 | 本地模型 |
|------|---------|---------|
| 费用 | 按 token 计费 | 一次性硬件成本 |
| 隐私 | 数据传到外部 | 完全本地，数据不出机 |
| 速度 | 受网络延迟影响 | 取决于硬件（GPU/内存） |
| 质量 | 最强（云端大模型） | 中等（7B-70B 量化模型） |
| 离线 | 需要网络 | 可以完全离线运行 |
| 适合场景 | 高质量回答、复杂推理 | 隐私敏感、频繁调用、低成本 |

---

## 二、详细配置指南

### 2.1 方案 A：使用 Ollama（推荐）

#### 步骤 1：安装 Ollama

```bash
# macOS
brew install ollama

# Linux
curl -fsSL https://ollama.com/install.sh | sh

# 验证安装
ollama --version
```

#### 步骤 2：下载模型

```bash
# 根据硬件选择模型尺寸
ollama pull qwen2.5:7b        # 8GB+ 内存
ollama pull qwen2.5:14b       # 16GB+ 内存
ollama pull qwen2.5:32b       # 32GB+ 内存
ollama pull phi3:mini         # 4GB+ 内存（最低配置）
```

#### 步骤 3：启动 Ollama 服务

```bash
# 前台启动（调试用）
ollama serve

# 后台启动（生产用）
nohup ollama serve > /tmp/ollama.log 2>&1 &
```

#### 步骤 4：配置 OpenClaw models.json

编辑 `~/.openclaw/agents/main/agent/models.json`：

```json
{
  "providers": {
    "local-ollama": {
      "baseUrl": "http://localhost:11434/v1",
      "api": "openai-completions",
      "models": [
        {
          "id": "qwen2.5:7b",
          "name": "Qwen 2.5 7B",
          "contextWindow": 32768,
          "maxTokens": 4096,
          "temperature": 0.7
        }
      ]
    }
  }
}
```

**参数说明**：

| 参数 | 说明 | 推荐值 |
|------|------|--------|
| `baseUrl` | Ollama API 端点 | `http://localhost:11434/v1` |
| `api` | API 兼容格式 | `openai-completions` |
| `contextWindow` | 上下文窗口大小 | 7B: 32768, 14B: 32768, 32B: 32768 |
| `maxTokens` | 最大输出 Token 数 | 4096 |
| `temperature` | 生成随机性 | 0.3-0.7 |

#### 步骤 5：验证连接

```bash
# 测试 Ollama API
curl -X POST "http://localhost:11434/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen2.5:7b",
    "messages": [{"role": "user", "content": "你好，请用一句话介绍你自己"}]
  }'

# 预期输出：包含 model 回复的 JSON
```

---

### 2.2 方案 B：使用 llama.cpp

#### 步骤 1：安装 llama.cpp

```bash
# macOS
brew install llama.cpp

# 或从源码编译
git clone https://github.com/ggerganov/llama.cpp.git
cd llama.cpp && make -j$(nproc)
```

#### 步骤 2：下载 GGUF 格式模型

```bash
# 使用 huggingface-cli 下载
pip install huggingface-hub
huggingface-cli download Qwen/Qwen2.5-7B-Instruct-GGUF \
  --include "*.gguf" \
  --local-dir ./models
```

#### 步骤 3：启动 llama-server

```bash
llama-server \
  -m ./models/qwen2.5-7b-instruct-q4_k_m.gguf \
  --host 0.0.0.0 \
  --port 8080 \
  --ctx-size 32768 \
  --n-gpu-layers 99
```

**参数说明**：

| 参数 | 说明 |
|------|------|
| `-m` | 模型文件路径 |
| `--host` | 监听地址 |
| `--port` | 监听端口 |
| `--ctx-size` | 上下文大小 |
| `--n-gpu-layers` | GPU 卸载层数（99=全部） |

#### 步骤 4：配置 OpenClaw

```json
{
  "providers": {
    "local-llama": {
      "baseUrl": "http://localhost:8080/v1",
      "api": "openai-completions",
      "models": [
        {
          "id": "qwen2.5-7b",
          "name": "Qwen 2.5 7B (GGUF)",
          "contextWindow": 32768,
          "maxTokens": 4096
        }
      ]
    }
  }
}
```

---

### 2.3 模型选型指南

| 场景 | 推荐模型 | 最低内存 | 推理速度 |
|------|---------|---------|---------|
| 简单对话 / 客服 | qwen2.5:7b | 8GB | 快 |
| 代码生成 | qwen2.5:14b | 16GB | 中 |
| 复杂推理 / 分析 | qwen2.5:32b | 32GB | 慢 |
| 最低配置设备 | phi3:mini (3.8B) | 4GB | 快 |
| 英文为主 | llama3.1:8b | 8GB | 快 |

---

## 三、本地模型在自动化工作流中的应用

### 3.1 Cron 定时任务 + 本地模型

```json
{
  "name": "daily-news-local",
  "schedule": {
    "kind": "cron",
    "expr": "0 9 * * *",
    "tz": "Asia/Shanghai"
  },
  "payload": {
    "kind": "agentTurn",
    "message": "使用本地模型生成今日新闻摘要"
  },
  "sessionTarget": "isolated"
}
```

### 3.2 成本对比示例

| 任务 | 云端模型（月） | 本地模型（月） |
|------|--------------|--------------|
| 每日新闻简报（30次） | ¥15 | ¥0（电费约¥2） |
| 邮件检查（1440次） | ¥200 | ¥0（电费约¥10） |
| 代码审查（500次） | ¥50 | ¥0（电费约¥5） |

---

## 🎯 实操练习

### 练习 1：部署 Ollama 并运行 Qwen2.5

```bash
# 1. 安装 Ollama
brew install ollama

# 2. 拉取模型
ollama pull qwen2.5:7b

# 3. 启动服务
ollama serve &

# 4. 测试 API
curl -X POST http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"qwen2.5:7b","messages":[{"role":"user","content":"1+1=?"}]}'

# 5. 配置 OpenClaw models.json 并重启 Gateway
openclaw gateway restart
```

### 练习 2：创建本地模型 Cron 任务

1. 在 OpenClaw 中创建一个每小时执行的 Cron 任务
2. 任务内容：检查系统负载并生成简短报告
3. 使用本地模型生成回复
4. 验证任务是否正常执行

### 练习 3：性能基准测试

```bash
# 记录不同模型的推理速度
time curl -X POST http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"qwen2.5:7b","messages":[{"role":"user","content":"写一篇200字的文章"}]}'
```

---

## ✅ 考核验证清单

| # | 考核点 | 验证方法 | 状态 |
|---|--------|---------|------|
| 1 | 成功安装并启动 Ollama | `ollama list` 显示已下载模型 | ☐ |
| 2 | models.json 配置正确 | `openclaw gateway status` 无报错 | ☐ |
| 3 | 本地模型能正常响应 | curl 测试返回有效回复 | ☐ |
| 4 | Cron 任务使用本地模型执行 | 日志显示模型为 local-ollama | ☐ |
| 5 | 能根据硬件选择合适模型 | 说出 7B/14B/32B 的内存要求 | ☐ |

---

## 🔍 常见问题排查（Q&A）

### Q1: Ollama 启动后连接被拒绝？

**A:** 检查 Ollama 是否正在运行：
```bash
# 检查进程
ps aux | grep ollama

# 检查端口
lsof -i :11434

# 如果端口被占用
kill -9 <PID>
ollama serve
```

### Q2: 本地模型回复速度很慢？

**A:** 可能原因及解决方案：
- **CPU 推理**：考虑使用 GPU 加速，设置 `--n-gpu-layers 99`
- **模型过大**：换用更小的模型（7B → phi3-mini）
- **量化格式**：使用 Q4_K_M 量化而非 Q8_0
- **内存不足**：关闭其他占用内存的应用

### Q3: models.json 配置后 Gateway 启动失败？

**A:** 检查 JSON 格式：
```bash
cat ~/.openclaw/agents/main/agent/models.json | jq .
# 如果 jq 报错，修复 JSON 语法（逗号、引号、括号）
```

### Q4: 本地模型回复质量不如云端？

**A:** 这是正常现象。本地模型（7B-70B）在复杂推理上弱于云端大模型。建议：
- 简单任务用本地模型
- 复杂任务用云端模型
- 混合配置：`models.json` 中同时配置多个 provider

### Q5: 如何查看 Ollama 日志？

**A:**
```bash
# macOS (Homebrew)
brew services logs ollama

# Linux (systemd)
journalctl -u ollama -f

# 直接查看日志文件
cat /tmp/ollama.log
```

---

## 🎯 实操练习

### 练习 1：部署 Ollama 并运行 Qwen2.5（45 分钟）

**目标**：安装 Ollama 并部署 Qwen2.5 模型。

**步骤**：
1. 安装 Ollama
2. 下载 qwen2.5:7b 模型
3. 启动 Ollama 服务
4. 测试 API 连接
5. 配置 OpenClaw models.json

**验收标准**：Ollama 正常运行，OpenClaw 能调用本地模型。

---

### 练习 2：创建本地模型 Cron 任务（30 分钟）

**目标**：配置使用本地模型的定时任务。

**步骤**：
1. 创建每小时执行的 Cron 任务
2. 配置使用 local-ollama 模型
3. 测试任务执行
4. 查看日志确认

**验收标准**：Cron 任务能使用本地模型正常执行。

---

### 练习 3：性能基准测试（30 分钟）

**目标**：测试本地模型性能并优化。

**步骤**：
1. 测试推理速度
2. 对比不同模型性能
3. 启用 GPU 加速（如有）
4. 记录测试结果

**验收标准**：能完成性能测试，理解优化方法。

---

### 练习 4：配置混合模型策略（30 分钟）

**目标**：配置云端 + 本地模型混合策略。

**步骤**：
1. 配置主模型为云端模型
2. 配置备用模型为本地模型
3. 测试故障转移
4. 验证自动切换

**验收标准**：故障转移配置正确，能自动切换。

---

## ✅ 考核验证清单

| 序号 | 考核点 | 验证方法 | 合格标准 |
|------|--------|---------|---------|
| 1 | Ollama 部署 | 安装并启动 | Ollama 正常运行 |
| 2 | 模型配置 | 配置 models.json | 配置正确，能调用 |
| 3 | 本地模型测试 | curl 测试 API | 返回有效回复 |
| 4 | Cron 任务 | 创建定时任务 | 使用本地模型执行 |
| 5 | 性能测试 | 推理速度测试 | 理解优化方法 |
| 6 | 故障转移 | 配置主备模型 | 能自动切换 |
| 7 | 混合策略 | 云端 + 本地配置 | 策略配置正确 |

---

## 🔍 常见问题排查

### Q1: Ollama 启动后连接被拒绝？

**A:** 检查 Ollama 是否正在运行：
```bash
# 检查进程
ps aux | grep ollama

# 检查端口
lsof -i :11434

# 如果端口被占用
kill -9 <PID>
ollama serve
```

---

### Q2: 本地模型回复速度很慢？

**A:** 可能原因及解决方案：
- **CPU 推理**：考虑使用 GPU 加速，设置 `--n-gpu-layers 99`
- **模型过大**：换用更小的模型（7B → phi3-mini）
- **量化格式**：使用 Q4_K_M 量化而非 Q8_0
- **内存不足**：关闭其他占用内存的应用

---

### Q3: models.json 配置后 Gateway 启动失败？

**A:** 检查 JSON 格式：
```bash
cat ~/.openclaw/agents/main/agent/models.json | jq .
# 如果 jq 报错，修复 JSON 语法（逗号、引号、括号）
```

---

### Q4: 本地模型回复质量不如云端？

**A:** 这是正常现象。本地模型（7B-70B）在复杂推理上弱于云端大模型。建议：
- 简单任务用本地模型
- 复杂任务用云端模型
- 混合配置：`models.json` 中同时配置多个 provider

---

### Q5: 如何查看 Ollama 日志？

**A:**
```bash
# macOS (Homebrew)
brew services logs ollama

# Linux (systemd)
journalctl -u ollama -f

# 直接查看日志文件
cat /tmp/ollama.log
```

---

### Q6: 如何更新本地模型？

**A:**
```bash
# 拉取最新版本
ollama pull qwen2.5:7b

# 查看已安装模型
ollama list

# 删除旧版本
ollama rm qwen2.5:7b
```

---

### Q7: 如何备份本地模型？

**A:**
```bash
# 备份模型文件
cp -r ~/.ollama/models ~/ollama-models-backup/

# 恢复模型
cp -r ~/ollama-models-backup/models ~/.ollama/
```

---

## 📝 培训记录表

| 项目 | 内容 |
|------|------|
| **培训阶段** | 第三阶段：自动化工作流 |
| **培训模块** | QMD 本地模型教程 |
| **培训时长** | 2-3 小时 |
| **培训日期** | ____年____月____日 |
| **培训讲师** | ____________________ |
| **学员姓名** | ____________________ |

### 学习进度记录

| 时间 | 学习内容 | 完成状态 | 讲师签字 |
|------|---------|---------|---------|
| 第 1 小时 | Ollama 部署 + 模型配置 | ☐ 未完成 ☐ 已完成 | ________ |
| 第 2 小时 | Cron 任务 + 性能测试 | ☐ 未完成 ☐ 已完成 | ________ |
| 第 3 小时 | 混合策略 + 故障转移 | ☐ 未完成 ☐ 已完成 | ________ |

### 考核结果

| 考核项 | 得分 | 备注 |
|--------|------|------|
| 实操练习 1 | ____/25 | |
| 实操练习 2 | ____/25 | |
| 实操练习 3 | ____/25 | |
| 实操练习 4 | ____/25 | |
| **总分** | **____/100** | |

### 签字确认

| 角色 | 签字 | 日期 |
|------|------|------|
| 学员 | ____________________ | ____年____月____日 |
| 讲师 | ____________________ | ____年____月____日 |
| 主管 | ____________________ | ____年____月____日 |
