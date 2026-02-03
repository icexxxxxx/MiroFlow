# MiroFlow DeepSeek 官方 API 使用手册

本手册详细说明如何使用 MiroFlow 框架与 DeepSeek 官方 API 进行多步骤互联网研究任务。

## 目录

- [快速开始](#快速开始)
- [环境配置](#环境配置)
- [基本使用](#基本使用)
- [高级配置](#高级配置)
- [常见问题](#常见问题)
- [故障排查](#故障排查)

---

## 快速开始

### 1. 前提条件

- Python >= 3.12
- `uv` 包管理器（[安装指南](https://docs.astral.sh/uv/getting-started/)）
- DeepSeek 官方 API 密钥（[获取地址](https://platform.deepseek.com/)）
- Serper 搜索 API 密钥（可选，用于网络搜索功能）
- Jina AI 密钥（可选，用于网页内容提取）

### 2. 项目初始化

```bash
# 克隆你的 fork
git clone https://github.com/your-username/MiroFlow.git
cd MiroFlow

# 同步依赖
uv sync
```

### 3. 配置环境变量

```bash
# 复制模板文件
cp .env.template .env

# 编辑 .env，填入你的 API 密钥
# 最小配置：
# DEEPSEEK_API_KEY="sk-your-api-key-here"
# DEEPSEEK_BASE_URL="https://api.deepseek.com/beta"
# SERPER_API_KEY="your-serper-api-key"
# JINA_API_KEY="your-jina-api-key"

# 使用编辑器编辑
nano .env  # 或 vim, code 等
```

### 4. 运行第一个任务

```bash
# 最简单的示例
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  --task="What is the capital of France?" \
  --task_file_name=""

# 期望输出：
# Task: What is the capital of France?
# Final Answer: Paris
```

---

## 环境配置

### DeepSeek 官方 API 配置

#### 必需环境变量

| 变量 | 说明 | 示例 |
|------|------|------|
| `DEEPSEEK_API_KEY` | DeepSeek API 密钥 | `sk-1234567890abcdef` |
| `DEEPSEEK_BASE_URL` | API 端点 URL | `https://api.deepseek.com/beta` |

#### 可选环境变量

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `SERPER_API_KEY` | 无 | Serper 搜索 API 密钥，启用网络搜索 |
| `JINA_API_KEY` | 无 | Jina AI 密钥，启用网页内容提取 |
| `LOGGER_LEVEL` | `INFO` | 日志级别（DEBUG, INFO, WARNING, ERROR） |

### .env 文件示例

```bash
# ===== 必需配置 =====
# DeepSeek 官方 API
DEEPSEEK_API_KEY="sk-xxxxxxxxxxxxxxxxxxxxxxxx"
DEEPSEEK_BASE_URL="https://api.deepseek.com/beta"

# ===== 搜索工具配置（推荐） =====
# Serper 搜索引擎（https://serper.dev）
SERPER_API_KEY="xxxxxxxxxxxxxxxxxxx"

# Jina AI 网页内容提取（https://jina.ai）
JINA_API_KEY="jina_xxxxxxxxxxxxxxxxxxxxxx"

# ===== 可选配置 =====
# 日志级别
LOGGER_LEVEL="INFO"  # DEBUG 获得更详细的日志

# 任务 ID（用于日志跟踪）
TASK_ID="task_001"

# 数据目录
DATA_DIR="data"

# 搜索结果过滤（可选）
REMOVE_SNIPPETS="false"
REMOVE_KNOWLEDGE_GRAPH="false"
REMOVE_ANSWER_BOX="false"
```

### 获取 API 密钥

#### DeepSeek API 密钥

1. 访问 [DeepSeek API 平台](https://platform.deepseek.com/)
2. 注册账号或登录
3. 进入 API 密钥管理页面
4. 创建新的 API 密钥
5. 复制密钥并保存到 `.env` 文件

#### Serper API 密钥

1. 访问 [Serper.dev](https://serper.dev/)
2. 注册账号
3. 进入控制面板获取 API 密钥
4. 复制到 `.env` 文件

#### Jina API 密钥

1. 访问 [Jina AI](https://jina.ai/)
2. 注册账号
3. 生成 API 密钥
4. 复制到 `.env` 文件

---

## 基本使用

### 命令行基本语法

```bash
uv run main.py trace \
  --config_file_name=CONFIG_NAME \
  --task="YOUR_TASK_DESCRIPTION" \
  [--task_file_name="PATH_TO_FILE"]
```

### 参数说明

| 参数 | 必需 | 说明 | 示例 |
|------|------|------|------|
| `--config_file_name` | ✅ | 配置文件名（不需要 `.yaml` 后缀） | `agent_llm_deepseek_official` |
| `--task` | ✅ | 任务描述/问题 | `"Find current Bitcoin price"` |
| `--task_file_name` | ❌ | 可选的任务数据文件路径 | `"data/sample.xlsx"` |

### 使用示例

#### 示例 1：简单问答

```bash
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  --task="What are the latest advancements in quantum computing?"
```

#### 示例 2：带数据文件的任务

```bash
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  --task="Find the country with the highest GDP in the provided list" \
  --task_file_name="data/countries.xlsx"
```

#### 示例 3：中文任务

```bash
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  --task="2024年全球人工智能发展的主要趋势是什么？"
```

#### 示例 4：复杂研究任务

```bash
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  --task="比较三种主流大语言模型（GPT-4, Claude 3.5, DeepSeek）在代码生成任务上的表现，并列出各自的优缺点"
```

### 输出说明

执行任务后，将在 `logs/` 目录下生成以下文件：

```
logs/
├── task_1.log          # 任务日志文件
├── task_1.json         # 任务结果（JSON 格式）
└── task_1_trace.txt    # 执行轨迹（详细步骤记录）
```

### 日志查看

```bash
# 查看实时日志（适用于长运行任务）
tail -f logs/task_1.log

# 查看完整结果
cat logs/task_1.json | jq .

# 查看执行轨迹
cat logs/task_1_trace.txt
```

---

## 高级配置

### 通过 Hydra 动态覆盖参数

Hydra 允许你在命令行直接覆盖配置参数，无需修改 YAML 文件：

#### 覆盖模型参数

```bash
# 调整温度（控制输出的随机性）
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.temperature=0.5 \
  --task="Generate creative story ideas"

# 调整最大 token 数
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.max_tokens=16000 \
  --task="Write a detailed technical analysis"

# 调整 top_p（核采样）
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.top_p=0.8 \
  --task="Your question here"
```

#### 覆盖 API 配置

```bash
# 动态指定 API 密钥（无需修改 .env）
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.deepseek_api_key="sk-your-api-key" \
  llm.deepseek_base_url="https://api.deepseek.com/beta" \
  --task="Your question"
```

#### 覆盖日志级别

```bash
# 获得详细的调试日志
LOGGER_LEVEL=DEBUG uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  --task="Debug task"
```

#### 覆盖工具配置

```bash
# 禁用某些工具，只使用阅读工具
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  main_agent.tool_config=[tool-reading] \
  --task="Analyze the provided document"
```

### 组合多个参数

```bash
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.temperature=0.7 \
  llm.max_tokens=20000 \
  main_agent.max_turns=30 \
  LOGGER_LEVEL=DEBUG \
  --task="Complex multi-step research task"
```

### 自定义配置文件

如果需要保存常用的参数组合，可以创建新的配置文件：

```bash
# 创建新配置文件
cp config/agent_llm_deepseek_official.yaml config/agent_llm_deepseek_custom.yaml

# 编辑自定义配置
nano config/agent_llm_deepseek_custom.yaml
```

在 `agent_llm_deepseek_custom.yaml` 中，你可以修改：

```yaml
main_agent:
  llm:
    temperature: 0.5        # 自定义温度
    max_tokens: 20000       # 自定义最大 token 数
    top_p: 0.85             # 自定义 top_p
  
  max_turns: 30             # 最大轮数
  max_tool_calls_per_turn: 15  # 每轮最多工具调用数
  
  tool_config:
    - tool-reading
    - tool-searching
```

然后使用新配置运行：

```bash
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_custom \
  --task="Your task"
```

### 性能优化建议

#### 1. 减少 token 使用

```bash
# 限制上下文长度
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.max_context_length=8000 \
  --task="..."
```

#### 2. 减少 API 调用次数

```bash
# 限制最大轮数
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  main_agent.max_turns=10 \
  main_agent.max_tool_calls_per_turn=5 \
  --task="..."
```

#### 3. 加快搜索速度

```bash
# 移除不需要的搜索结果组件
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  main_agent.tool_config=[tool-reading] \
  --task="..."
```

---

## 常见问题

### Q1: 如何知道我的 API 密钥是否有效？

```bash
# 运行一个简单的任务来测试 API
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  --task="Hello, please respond with 'API is working'" \
  LOGGER_LEVEL=DEBUG
```

如果看到正常的响应，说明 API 密钥有效。

### Q2: 如何减少成本？

1. **限制 token 使用**：设置 `llm.max_tokens` 为较小值
2. **减少搜索**：禁用不必要的工具
3. **限制轮数**：设置 `main_agent.max_turns`

```bash
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.max_tokens=8000 \
  main_agent.max_turns=5 \
  main_agent.tool_config=[tool-reading] \
  --task="..."
```

### Q3: 如何处理超长任务？

对于复杂的研究任务，可能需要更多的轮数和 token：

```bash
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.max_tokens=32000 \
  main_agent.max_turns=50 \
  --task="Your complex research question"
```

### Q4: 如何查看详细的执行过程？

```bash
# 启用 DEBUG 日志
LOGGER_LEVEL=DEBUG uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  --task="Your task"

# 然后查看日志
tail -f logs/task_1.log
```

### Q5: 能否同时使用多个 API？

可以，但需要创建不同的配置文件。当前配置专注于 DeepSeek 官方 API。如需切换到其他 API（如 OpenRouter），使用相应的配置文件：

```bash
# 使用 OpenRouter（Claude）
uv run main.py trace \
  --config_file_name=agent_llm_claude37sonnet \
  --task="..."

# 使用 OpenRouter（DeepSeek）
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_openrouter \
  --task="..."

# 使用官方 DeepSeek API
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  --task="..."
```

---

## 故障排查

### 问题 1: `DEEPSEEK_API_KEY not found`

**原因**：环境变量未正确设置

**解决方案**：
```bash
# 确认 .env 文件存在并包含正确的密钥
cat .env | grep DEEPSEEK_API_KEY

# 手动设置环境变量（临时）
export DEEPSEEK_API_KEY="your-api-key"
uv run main.py trace --config_file_name=agent_llm_deepseek_official --task="test"
```

### 问题 2: `Invalid API Key`

**原因**：API 密钥无效或已过期

**解决方案**：
1. 访问 [DeepSeek 平台](https://platform.deepseek.com/) 验证密钥
2. 重新生成新的 API 密钥
3. 更新 `.env` 文件

### 问题 3: `Connection timeout`

**原因**：网络问题或 API 端点不可达

**解决方案**：
```bash
# 检查网络连接
ping api.deepseek.com

# 确认 API 端点 URL 正确
cat .env | grep DEEPSEEK_BASE_URL

# 检查防火墙设置
```

### 问题 4: `Context length exceeded`

**原因**：输入文本过长，超过模型限制

**解决方案**：
```bash
# 减小最大 token 数或简化任务
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.max_context_length=16000 \
  --task="Simplified task"
```

### 问题 5: 日志中有 `Tool call failed`

**原因**：搜索或网页提取工具失败

**解决方案**：
1. 检查 Serper 或 Jina API 密钥是否有效
2. 验证搜索关键词是否合理
3. 尝试禁用问题工具：
```bash
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  main_agent.tool_config=[tool-reading] \
  --task="Your task without search"
```

### 问题 6: 获取日志中的更多信息

```bash
# 最详细的日志
LOGGER_LEVEL=DEBUG uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  --task="test" 2>&1 | tee debug.log

# 查看完整输出
cat logs/task_1.log | head -100  # 查看前 100 行
cat logs/task_1.log | tail -50   # 查看最后 50 行
```

---

## 最佳实践

### 1. 版本控制

保持代码库最新：

```bash
# 定期同步上游
git fetch upstream
git rebase upstream/main

# 合并上游更新
git merge upstream/main
```

### 2. 任务日志管理

```bash
# 保存重要任务的日志
mkdir -p logs/archived
cp logs/task_1.log logs/archived/task_1_$(date +%Y%m%d_%H%M%S).log

# 清理旧日志（可选）
find logs/ -name "*.log" -mtime +7 -delete
```

### 3. 成本监控

定期检查 API 使用情况：

```bash
# 统计 token 使用（在日志中查找）
grep "Total Input:" logs/task_*.log | awk '{sum+=$NF} END {print "Total tokens:", sum}'
```

### 4. 配置备份

```bash
# 备份当前配置
cp .env .env.backup
cp config/agent_llm_deepseek_official.yaml config/agent_llm_deepseek_official.yaml.backup
```

---

## 获取帮助

- 📖 [项目 README](README.md)
- 🔧 [环境变量完整清单](env.md)
- 🐛 [GitHub Issues](https://github.com/MiroMindAI/MiroFlow/issues)
- 💬 [Discord 社区](https://discord.com/invite/GPqEnkzQZd)

---

## 更新日志

### v1.0.0 (2026-02-03)
- ✅ 初始版本发布
- ✅ DeepSeek 官方 API 支持
- ✅ 完整的文档和示例

---

**最后更新**：2026-02-03  
**维护者**：MiroMind AI 团队
