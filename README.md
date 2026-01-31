# podcastfy-skill

A skill that generates podcast-style MP3 from one or more URLs using the open-source **Podcastfy** project.

It provides a wrapper script that:

- Creates/uses a local Python venv
- Installs/updates `podcastfy`
- Supports multiple LLMs: **Claude (Opus 4.5/Sonnet 4)**, **Gemini**, **OpenAI**
- Uses **Edge TTS** for speech synthesis (no paid TTS API required)

> Note: This repository intentionally does **not** include any generated audio/transcripts or any secrets.

---

## Features

- Generate a single MP3 from a single article URL
- Generate one MP3 from multiple URLs (e.g., 3 articles → 1 combined episode)
- Support for multiple languages: English, Chinese, Bilingual (English + Chinese translation)
- Automatic MP3 validation and fallback re-synthesis
- Safer defaults: `.env`, `.venv`, and `output/` are ignored

---

## Requirements

- Linux/macOS (tested on both)
- Python 3.10+ (tested with Python 3.12)
- `ffmpeg`
- API key for your chosen LLM provider

Install ffmpeg:

```bash
# macOS
brew install ffmpeg

# Ubuntu/Debian
sudo apt-get update && sudo apt-get install -y ffmpeg
```

---

## Setup

### 1) Create a `.env`

Create a `.env` file in the skill directory. Choose one of the following configurations:

**Option A: Claude (Recommended)**

```bash
ANTHROPIC_API_KEY=your_anthropic_api_key
PODCASTFY_API_KEY_LABEL=ANTHROPIC_API_KEY
PODCASTFY_LLM_MODEL=claude-opus-4-5-20251101
```

**Option B: Claude with custom proxy**

```bash
ANTHROPIC_BASE_URL=https://your-proxy.example.com/api
ANTHROPIC_AUTH_TOKEN=your_token_here
PODCASTFY_API_KEY_LABEL=ANTHROPIC_AUTH_TOKEN
PODCASTFY_LLM_MODEL=claude-opus-4-5-20251101
```

**Option C: Gemini**

```bash
GEMINI_API_KEY=your_gemini_api_key
PODCASTFY_LLM_MODEL=gemini-1.5-flash
```

**Optional: Language & Voice settings**

```bash
# Language options: English (default), Chinese, bilingual
PODCASTFY_LANGUAGE=English

# Custom TTS voices (Edge TTS)
PODCASTFY_EDGE_VOICE_Q=en-US-JennyNeural
PODCASTFY_EDGE_VOICE_A=en-US-EricNeural
```

### 2) Run

Single URL:

```bash
./scripts/podcastfy_generate.py --url "https://example.com/article"
```

Multiple URLs:

```bash
./scripts/podcastfy_generate.py \
  --url "https://example.com/a" \
  --url "https://example.com/b" \
  --url "https://example.com/c"
```

Long-form content:

```bash
./scripts/podcastfy_generate.py --url "https://example.com/article" --longform
```

---

## Output

On success, the script prints the generated MP3 path.

Typical output locations:

- `output/audio/*.mp3`
- `output/transcripts/*.txt`

---

## Supported Models

| Provider | Model ID | Notes |
|----------|----------|-------|
| Claude | `claude-opus-4-5-20251101` | Latest, highest quality |
| Claude | `claude-sonnet-4-20250514` | Fast, cost-effective |
| Claude | `claude-opus-4-20250514` | Powerful |
| Gemini | `gemini-1.5-flash` | Default for Gemini |
| Gemini | `gemini-2.5-flash` | Latest Gemini |

---

## Notes / Troubleshooting

- If Gemini returns `429` with `limit: 0`, your project/key has **no usable quota**. Check your Gemini API rate-limit page.
- Some websites keep connections open and can cause Playwright timeouts; the wrapper is tuned to be tolerant, but some pages may still fail.
- The `.venv` folder (~1.2GB) is auto-generated on first run and can be safely deleted.

---

## License

MIT

---

# 中文说明

这是一个可以把一个或多个网页链接生成「播客风格」**MP3** 的工具。

该仓库提供一个包装脚本，主要功能：

- 自动创建/使用 Python 虚拟环境
- 安装/更新 `podcastfy`
- 支持多种 LLM：**Claude (Opus 4.5/Sonnet 4)**、**Gemini**、**OpenAI**
- 使用 **Edge TTS** 合成语音（无需付费 TTS API）

> 注意：本仓库不会提交任何密钥、`.env`、虚拟环境、以及生成的音频/转写文件。

---

## 功能

- 单链接生成一条 MP3
- 多链接合并生成一条 MP3（例如 3 篇文章合成 1 期播客）
- 支持多语言：英文、中文、双语（英文 + 中文翻译）
- 自动验证 MP3 有效性，失败时自动重新合成
- 默认安全：忽略 `.env` / `.venv` / `output/`

---

## 环境要求

- Linux/macOS（均已测试）
- Python 3.10+（测试环境为 Python 3.12）
- `ffmpeg`
- LLM 提供商的 API Key

安装 ffmpeg：

```bash
# macOS
brew install ffmpeg

# Ubuntu/Debian
sudo apt-get update && sudo apt-get install -y ffmpeg
```

---

## 配置与运行

### 1）创建 `.env`

在项目目录创建 `.env`，选择以下配置之一：

**选项 A：Claude（推荐）**

```bash
ANTHROPIC_API_KEY=你的API密钥
PODCASTFY_API_KEY_LABEL=ANTHROPIC_API_KEY
PODCASTFY_LLM_MODEL=claude-opus-4-5-20251101
```

**选项 B：Claude + 自定义代理**

```bash
ANTHROPIC_BASE_URL=https://your-proxy.example.com/api
ANTHROPIC_AUTH_TOKEN=你的Token
PODCASTFY_API_KEY_LABEL=ANTHROPIC_AUTH_TOKEN
PODCASTFY_LLM_MODEL=claude-opus-4-5-20251101
```

**选项 C：Gemini**

```bash
GEMINI_API_KEY=你的Gemini密钥
PODCASTFY_LLM_MODEL=gemini-1.5-flash
```

**可选：语言和声音设置**

```bash
# 语言选项：English（默认）、Chinese、bilingual（双语）
PODCASTFY_LANGUAGE=Chinese

# 自定义 TTS 声音
PODCASTFY_EDGE_VOICE_Q=zh-CN-XiaoxiaoNeural
PODCASTFY_EDGE_VOICE_A=zh-CN-YunxiNeural
```

### 2）运行

单链接：

```bash
./scripts/podcastfy_generate.py --url "https://example.com/article"
```

多链接：

```bash
./scripts/podcastfy_generate.py \
  --url "https://example.com/a" \
  --url "https://example.com/b" \
  --url "https://example.com/c"
```

长文内容：

```bash
./scripts/podcastfy_generate.py --url "https://example.com/article" --longform
```

---

## 输出

成功后脚本会打印 MP3 输出路径，通常在：

- `output/audio/*.mp3`
- `output/transcripts/*.txt`

---

## 支持的模型

| 提供商 | 模型 ID | 说明 |
|--------|---------|------|
| Claude | `claude-opus-4-5-20251101` | 最新，质量最高 |
| Claude | `claude-sonnet-4-20250514` | 快速，性价比高 |
| Claude | `claude-opus-4-20250514` | 强大可靠 |
| Gemini | `gemini-1.5-flash` | Gemini 默认 |
| Gemini | `gemini-2.5-flash` | 最新 Gemini |

---

## 常见问题

- 如果 Gemini 报错 `429` 且包含 `limit: 0`，通常是因为该项目/Key 当前没有可用配额。请到 Gemini API 的 rate limit 页面确认配额状态。
- 某些网站会让 Playwright 一直等待，可能导致抓取超时；此仓库已做一定容错，但仍可能遇到个别页面失败。
- `.venv` 文件夹（约 1.2GB）会在首次运行时自动生成，可以安全删除。
