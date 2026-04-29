# AI内容运营工作流 · 完整教程

> 从选题到发布，1天内完成一部AI概念短片的全流程指南

---

## 准备工作

### 安装依赖

```bash
# 安装飞书 lark-cli
npm install -g @larksuite/cli

# 初始化飞书配置
lark-cli config init

# 验证即梦 CLI
dreamina --version
```

### 目录结构规范

```
your-project/
├── README.md
├── examples/
│   └── [项目名]/
│       ├── storyboard.png      # 故事板截图
│       ├── content-pack.md     # 内容包
│       └── videos/             # 生成视频归档
└── workflow/
    └── tutorial.md
```

---

## Step 1：写故事板（飞书）

在飞书文档中按以下结构写分镜：

| 镜号 | 时长 | 景别 | 画面描述 | 镜头运动 | 情绪/节奏 |
|------|------|------|----------|----------|-----------|
| S1 | 6s | 远景 | ... | 推进+仰拍 | 震撼开场 |

**建议：**
- 8-12 镜适合 1 分钟短片
- 每镜时长 4-12 秒（Seedance 支持范围）
- 提前规划色彩基调和镜头节奏

---

## Step 2：读取飞书故事板

```bash
# 搜索文档
lark-cli docs +search --query "你的项目名" --as user

# 读取文档内容
lark-cli docs +fetch --api-version v2 --doc [URL或token] --as user
```

---

## Step 3：生成 Seedance 提示词

将每个镜头的中文描述翻译为英文提示词，加入：
- **色彩词汇**：crimson, teal, amber, cold white...
- **光线描述**：volumetric light, rim light, ambient glow...
- **镜头运动**：slow push-in, tilt-down, tracking shot, orbital...
- **风格标签**：cinematic, film grain, dreamlike...

**模板：**
```
[景别] of [主体描述], [环境描述],
[核心动作], [色彩/光线],
[镜头运动], [风格标签]
```

---

## Step 4：逐镜生成视频

```bash
# 提交生成任务
dreamina text2video \
  --model_version seedance2.0 \
  --ratio 16:9 \
  --duration 8 \
  --prompt "Wide shot of lone figure walking through vast white spiral architecture..."

# 记录返回的 submit_id
# {"submit_id": "xxxx-xxxx-xxxx", "gen_status": "querying", "credit_count": 24}
```

**注意：**
- 并发限制为1，需串行提交（等上一个完成后再提交下一个）
- 队列等待约 5-15 分钟（视服务器负载）
- `seedance2.0` 消耗约 15-24 积分/镜

---

## Step 5：轮询状态 + 下载

```bash
# 查询状态
dreamina query_result --submit_id "xxxx-xxxx-xxxx"

# 成功后取 video_url 下载
curl -L "[video_url]" -o "./videos/S1_场景名.mp4"
```

**自动化轮询脚本：**
```bash
poll_and_download() {
  local sid=$1 outfile=$2
  while true; do
    result=$(dreamina query_result --submit_id "$sid" 2>&1)
    gst=$(echo "$result" | python3 -c "import sys,json; print(json.load(sys.stdin).get('gen_status',''))" 2>/dev/null)
    if [ "$gst" = "success" ]; then
      url=$(echo "$result" | python3 -c "import sys,json; print(json.load(sys.stdin)['result_json']['videos'][0]['video_url'])" 2>/dev/null)
      curl -sL "$url" -o "$outfile" && echo "✅ 下载完成: $outfile"
      break
    elif [ "$gst" = "fail" ]; then
      echo "❌ 生成失败"
      break
    fi
    sleep 20
  done
}

poll_and_download "your-submit-id" "./videos/S1.mp4"
```

---

## Step 6：剪映拼接

1. 导入所有 mp4 到剪映时间轴
2. 按分镜顺序排列
3. 添加背景音乐（建议：氛围感电子/管弦乐）
4. 加字幕/标题卡（可选）
5. 导出：1080p + H.264

---

## Step 7：创建飞书案例文档

```bash
lark-cli docs +create --api-version v2 --as user \
  --content '<title>项目名 · AI内容运营包</title><p>...</p>'
```

---

## Step 8：多平台发布

| 平台 | 格式 | 钩子策略 |
|------|------|----------|
| YouTube | 16:9, 标题含问句 | 前3秒悬念/视觉冲击 |
| 抖音 | 9:16 竖屏剪辑 | 钩子文案在封面显示 |
| X | 横版原片 + 工作流复盘 | 开源角度切入 |

---

## 常见问题

**Q: ExceedConcurrencyLimit 错误**  
A: 同时只能有1个任务在队列中，等上一个完成后再提交。

**Q: 队列位置很高（>5000）等太久**  
A: 正常现象，高峰期约等待 10-20 分钟。可以用 poll 脚本自动等待。

**Q: 视频质量不满意**  
A: 丰富提示词中的光线/风格描述，加入具体的色彩词和镜头运动词。

**Q: 想用 9:16 竖版**  
A: 将 `--ratio 16:9` 改为 `--ratio 9:16` 即可。

---

## 时间预估

| 步骤 | 时间 |
|------|------|
| 写故事板 | 30-60 分钟 |
| 生成提示词 | 15-30 分钟 |
| 视频生成（8镜） | 1.5-3 小时（队列等待） |
| 剪映拼接 | 30-60 分钟 |
| 飞书同步 + 发布包准备 | 15-30 分钟 |
| **全流程总计** | **3-5 小时** |
