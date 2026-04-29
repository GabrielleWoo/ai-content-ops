<div align="center">

# 🌀 AI Content Ops System

### 飞书故事板 → Seedance 2.0 → 多平台发布包
### Feishu Storyboard → Seedance 2.0 → Multi-Platform Content Pack

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Feishu%20%7C%20Seedance%20%7C%20YouTube%20%7C%20抖音-blue)](.)
[![案例](https://img.shields.io/badge/案例-《回声螺旋》-red)](examples/echo-spiral/)

[English](#english) · [中文](#中文)

</div>

---

## 中文

### 这是什么？

一套**可沉淀、可迭代、可开源、可商业化**的 AI 内容运营操作系统。

输入一个选题，自动产出：

- 🎬 分镜脚本 + Seedance 2.0 提示词（逐镜）
- 📦 三平台发布包（YouTube / 抖音 / X）
- 📖 教程文档（手把手工作流）
- 💾 本地视频归档 + 飞书知识库同步

### 工作流

```
选题输入
  ↓
内容生成（分镜 / 提示词 / 发布包）
  ↓
即梦CLI 批量生成视频（Seedance 2.0）
  ↓
本地归档 + 飞书文档同步
  ↓
多平台发布（YouTube / 抖音 / X）
```

### 工具链

| 工具 | 用途 |
|------|------|
| [飞书 lark-cli](https://github.com/larksuite/lark-cli) | 故事板读取 / 文档创建同步 |
| [即梦 dreamina CLI](https://dreamina.jianying.com) | Seedance 2.0 视频生成 |
| 剪映专业版 | 视频拼接 / 音效 |

### 快速开始

```bash
# 1. 安装依赖
npm install -g @larksuite/cli
# 配置飞书 lark-cli
lark-cli config init

# 2. 读取飞书故事板
lark-cli docs +fetch --api-version v2 --doc [YOUR_DOC_URL] --as user

# 3. 生成视频（Seedance 2.0）
dreamina text2video \
  --model_version seedance2.0 \
  --ratio 16:9 \
  --duration 8 \
  --prompt "your prompt here"

# 4. 查询生成状态
dreamina query_result --submit_id [SUBMIT_ID]

# 5. 下载归档
curl -L [VIDEO_URL] -o ./output/shot_01.mp4
```

### 案例：《回声螺旋》Echo Spiral

> 赛博朋克概念短片 · 60秒 · 8镜 · 1天完成

| 镜号 | 时长 | 描述 |
|------|------|------|
| S1 | 6s | 赤红天空，螺旋机掠过暗色水域 |
| S2 | 5s | 粒子扫描，密码符文叠加 |
| S3 | 8s | 白色螺旋建筑，孤独行者 |
| S4 | 8s | S形走廊，光斑铺地 |
| S5 | 8s | 投影叠影，记忆消散 |
| S6 | 6s | 地板龟裂，红光涌出 |
| S7 | 10s | 超低空城市，光流飞散 |
| S8 | 9s | 红色螺旋光网封印城市 |

📁 [完整案例文件 →](examples/echo-spiral/)
📄 [飞书案例文档 →](https://vqtyhj0mdb9.feishu.cn/docx/WZM2dmQWHoFaIwxQ9gvcR7qlnnD)

### 目录结构

```
.
├── README.md
├── examples/
│   └── echo-spiral/          # 《回声螺旋》完整案例
│       ├── storyboard.png    # 原始故事板
│       ├── content-pack.md   # 内容包（分镜+提示词+发布文案）
│       └── prompts/          # 逐镜 Seedance 提示词
├── skills/
│   └── seedance-content-operator/  # Claude Skill 源码
└── workflow/
    └── tutorial.md           # 完整工作流教程
```

### License

MIT · 欢迎 Fork、Star、提 PR

---

## English

### What is this?

An AI content operations system that turns a topic idea into a complete multi-platform video package in one day.

**Input**: a topic / storyboard  
**Output**: shot scripts + Seedance 2.0 prompts + YouTube / TikTok / X publish packs + Feishu doc sync

### Workflow

```
Topic Input
  ↓
Content Generation (shots / prompts / publish packs)
  ↓
Batch Video Generation via Seedance 2.0 CLI
  ↓
Local Archive + Feishu Sync
  ↓
Publish (YouTube / Douyin / X)
```

### Quick Start

```bash
# Install lark-cli
npm install -g @larksuite/cli
lark-cli config init

# Fetch storyboard from Feishu
lark-cli docs +fetch --api-version v2 --doc [DOC_URL] --as user

# Generate video
dreamina text2video --model_version seedance2.0 --ratio 16:9 --duration 8 --prompt "..."

# Poll status
dreamina query_result --submit_id [ID]
```

### Example: Echo Spiral (《回声螺旋》)

> Cyberpunk concept short film · 60s · 8 shots · Made in 1 day

📁 [Full case files →](examples/echo-spiral/)

### License

MIT · Feel free to fork, star, and contribute
