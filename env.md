# MiroFlow 环境变量完整清单

本文档列出项目中所有被读取的环境变量。

## 核心 & 日志配置

| 环境变量       | 默认值 | 说明                                                         |
| -------------- | ------ | ------------------------------------------------------------ |
| `LOGGER_LEVEL` | `INFO` | 日志级别（DEBUG、INFO、WARNING、ERROR 等），影响全局日志输出 |
| `TASK_ID`      | `"0"`  | 任务标识符，用于日志跟踪和任务识别                           |

## LLM API 密钥 & 基础 URL

| 环境变量             | 默认值                      | 说明                                               |
| -------------------- | --------------------------- | -------------------------------------------------- |
| `OPENROUTER_API_KEY` | **必需**                    | OpenRouter API 密钥，用于访问多个模型（主入口）    |
| `OPENAI_API_KEY`     | `""`                        | OpenAI API 密钥，用于 GPT 模型和 Audio/Vision 功能 |
| `OPENAI_BASE_URL`    | `https://api.openai.com/v1` | OpenAI API 端点，可用于自定义或兼容的 OpenAI 实现  |
| `ANTHROPIC_API_KEY`  | `""`                        | Anthropic API 密钥，用于 Claude 模型               |
| `ANTHROPIC_BASE_URL` | `https://api.anthropic.com` | Anthropic API 端点                                 |
| `GEMINI_API_KEY`     | `""`                        | Google Gemini API 密钥，用于 Gemini 视觉模型       |
| `DEEPSEEK_API_KEY` | `""`                        | DeepSeek 官方 API 密钥，用于 DeepSeek 主代理       |
| `DEEPSEEK_BASE_URL` | `https://api.deepseek.com/beta` | DeepSeek 官方 API 端点                             |

## 主代理模型选择

| 环境变量               | 默认值                                | 说明                                                |
| ---------------------- | ------------------------------------- | --------------------------------------------------- |
| `OPENAI_MODEL_NAME`    | `gpt-4o` (vision)<br>`o3` (reasoning) | 指定 OpenAI 视觉任务和推理任务使用的模型            |
| `ANTHROPIC_MODEL_NAME` | `claude-3-7-sonnet-20250219`          | 指定 Anthropic 推理和视觉任务使用的 Claude 模型版本 |
| `GEMINI_MODEL_NAME`    | `gemini-2.5-pro`                      | 指定 Google Gemini 视觉任务使用的模型               |

## 音频处理模型

| 环境变量                          | 默认值                 | 说明                                           |
| --------------------------------- | ---------------------- | ---------------------------------------------- |
| `OPENAI_TRANSCRIPTION_MODEL_NAME` | `gpt-4o-transcribe`    | OpenAI 音频转录模型                            |
| `OPENAI_AUDIO_MODEL_NAME`         | `gpt-4o-audio-preview` | OpenAI 音频生成/处理模型                       |
| `WHISPER_MODEL_NAME`              | 无                     | Whisper 音频转录模型（若未设置则不使用此服务） |

## 自定义/扩展模型（OS 服务）

| 环境变量               | 默认值 | 说明                                                     |
| ---------------------- | ------ | -------------------------------------------------------- |
| `VISION_MODEL_NAME`    | 无     | 自定义视觉模型（通过 OS 服务），若设置则覆盖默认视觉模型 |
| `VISION_API_KEY`       | 无     | 自定义视觉模型的 API 密钥                                |
| `VISION_BASE_URL`      | 无     | 自定义视觉模型的 API 端点                                |
| `REASONING_MODEL_NAME` | 无     | 自定义推理模型（通过 OS 服务），若设置则覆盖默认推理模型 |
| `REASONING_API_KEY`    | 无     | 自定义推理模型的 API 密钥                                |
| `REASONING_BASE_URL`   | 无     | 自定义推理模型的 API 端点                                |

## 搜索 & 网页内容检索工具

| 环境变量          | 默认值                      | 说明                               |
| ----------------- | --------------------------- | ---------------------------------- |
| `SERPER_API_KEY`  | `""`                        | Serper 搜索引擎 API 密钥           |
| `SERPER_BASE_URL` | `https://google.serper.dev` | Serper API 端点                    |
| `JINA_API_KEY`    | `""`                        | Jina AI 网页阅读/内容提取 API 密钥 |
| `JINA_BASE_URL`   | `https://r.jina.ai`         | Jina API 端点                      |

## 搜索结果过滤选项

| 环境变量                 | 默认值  | 说明                                       |
| ------------------------ | ------- | ------------------------------------------ |
| `REMOVE_SNIPPETS`        | `false` | 是否从搜索结果中移除摘要片段（true/false） |
| `REMOVE_KNOWLEDGE_GRAPH` | `false` | 是否从搜索结果中移除知识图谱（true/false） |
| `REMOVE_ANSWER_BOX`      | `false` | 是否从搜索结果中移除答案框（true/false）   |

## 代码执行 & 沙箱环境

| 环境变量              | 默认值            | 说明                                                          |
| --------------------- | ----------------- | ------------------------------------------------------------- |
| `E2B_API_KEY`         | 无                | E2B 云代码执行平台 API 密钥，用于在隔离沙箱中执行 Python 代码 |
| `DEFAULT_TEMPLATE_ID` | `all_pip_apt_pkg` | E2B 沙箱模板 ID，指定预装的包环境                             |
| `DEFAULT_TIMEOUT`     | `1800`            | E2B 代码执行超时时间（秒），默认 30 分钟                      |
| `LOGS_DIR`            | 无                | 基准测试日志存储目录路径                                      |

## 功能开关

| 环境变量               | 默认值  | 说明                                   |
| ---------------------- | ------- | -------------------------------------- |
| `ENABLE_CLAUDE_VISION` | `false` | 是否启用 Claude 视觉能力（true/false） |
| `ENABLE_OPENAI_VISION` | `false` | 是否启用 OpenAI 视觉能力（true/false） |

## GitHub Actions / CI-CD

| 环境变量              | 默认值   | 说明                                                      |
| --------------------- | -------- | --------------------------------------------------------- |
| `GITHUB_STEP_SUMMARY` | 自动设置 | GitHub Actions 步骤总结输出文件路径（由 GitHub 自动设置） |

---

## 使用建议

### 最小化快速启动

仅需设置：

```bash
OPENROUTER_API_KEY="你的 OpenRouter API 密钥"
```

### 完整配置示例

**使用 OpenRouter（OpenRouter 代理多个模型）**：
```bash
# 核心 API
OPENROUTER_API_KEY="sk-or-v1-xxxxxxxx99b"
OPENAI_API_KEY="sk-xxxxxxxx"
ANTHROPIC_API_KEY="sk-ant-xxxxxxxx"

# 搜索工具
SERPER_API_KEY="xxxxxxxxxxxx"
JINA_API_KEY="xxxxxxxxxxxx"

# 代码执行
E2B_API_KEY="xxxxxxxxxxxx"

# 日志
LOGGER_LEVEL="DEBUG"

# 可选：功能开关
ENABLE_CLAUDE_VISION="true"
ENABLE_OPENAI_VISION="true"
```

**使用 DeepSeek 官方 API**：
```bash
# DeepSeek 官方 API
DEEPSEEK_API_KEY="sk-xxxxxxxx"
DEEPSEEK_BASE_URL="https://api.deepseek.com/beta"

# 搜索工具（仍需配置）
SERPER_API_KEY="xxxxxxxxxxxx"
JINA_API_KEY="xxxxxxxxxxxx"

# 日志
LOGGER_LEVEL="DEBUG"
```

### 配置位置

- `.env.template` - 模板文件（复制后重命名为 `.env` 并填入密钥）
- `.env` - 实际配置文件（运行前需创建）

### 快速启动命令

**使用 OpenRouter 配置**：
```bash
cp .env.template .env
# 编辑 .env 并填入 OPENROUTER_API_KEY
uv run main.py trace --config_file_name=agent_quickstart_reading --task="..." --task_file_name="..."
```

**使用 DeepSeek 官方 API**：
```bash
# 编辑 .env 并填入 DEEPSEEK_API_KEY 和 DEEPSEEK_BASE_URL
uv run main.py trace --config_file_name=agent_llm_deepseek_official --task="..." --task_file_name="..."
```

### 通过 Hydra 覆盖配置

除了环境变量，许多配置也可通过 Hydra 命令行参数覆盖：

```bash
# 示例：使用 DeepSeek 官方 API
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.model_name=deepseek-chat \
  llm.temperature=0.3 \
  LOGGER_LEVEL=DEBUG \
  --task="你的问题"

# 示例：动态指定 DeepSeek API 密钥和 URL
uv run main.py trace \
  --config_file_name=agent_llm_deepseek_official \
  llm.deepseek_api_key="sk-xxxx" \
  llm.deepseek_base_url="https://api.deepseek.com/beta" \
  --task="你的问题"
```
