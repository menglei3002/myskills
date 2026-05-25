---
name: aimusic-pipeline
description: AI 音乐短视频完整流水线 — 从选题到发布的端到端方案。当用户提到 AI音乐、搞笑短视频、洗脑神曲、AI写歌、短视频制作流水线时使用此技能。
---

# AI 音乐短视频完整流水线

## 6 步流水线

```
选题 → 歌词 → 生成音乐 → 制作视频 → 发布 → 数据追踪
```

## Step 1: 选题

- 搜索抖音/B站当前热门搞笑话题
- 生成 10 个选题，按传播力排序
- **暂停让用户选 1 个**
- 公式：**正常开头 → 突然反转 → 重复洗脑**

## Step 2: 歌词

- 15 秒爆点优先，再扩展 30 秒
- 每句 ≤12 字，副歌 4 句 + 主歌 4 句
- 结构: `[Chorus] → [Verse] → [Chorus]`
- Hook 句必须重复出现
- **暂停让用户确认**

## Step 3: 生成音乐 → [[ace-music-gen]]

使用 ACE-Step 生成 3 个版本：

```bash
cd D:/AI/mywork/myshipintest/claude-code-video-toolkit
NO_PROXY="*" no_proxy="*" python tools/music_gen.py \
  --prompt "搞笑流行风 自嘲搞怪 沙雕感十足 轻快节奏 夸张演唱 短视频洗脑神曲" \
  --lyrics "[Chorus]\n...\n\n[Verse]\n...\n\n[Chorus]\n..." \
  --bpm 110 --duration 30 --vocal-language zh \
  --variations 1 --no-thinking \
  --output "D:/AI/mywork/AIMUSIC/output/songs/song_v1.mp3"
# 重复 3 次生成 v1/v2/v3
```

- **暂停让用户选最佳版本**

## Step 4: 制作视频

### 4a. 生成表情包背景
用 Pillow 按歌词生成 10 张 1080×1920 彩色 meme 卡片：
```python
from PIL import Image, ImageDraw, ImageFont
# 每句歌词一张图，切换背景色和 emoji
# 字体: C:/Windows/Fonts/simhei.ttf, 120px
```

### 4b. 生成字幕 ASS 文件 → [[ffmpeg-video-compose]]
- 音频实际时长用 ffprobe 获取
- 副歌每句 3.5s，主歌每句 2.5s
- 字体: Microsoft YaHei, 48px, 粗体白字黑边

### 4c. FFmpeg 合成 → [[ffmpeg-video-compose]]

```bash
FFMPEG="D:/AI/mp3zhuanhuan/dist/ffmpeg.exe"
"$FFMPEG" -y \
  -f concat -safe 0 -i concat_list.txt \
  -i audio.mp3 -i digital_human.png \
  -filter_complex \
    "[0:v]scale=1080:1920,fps=30[bg];\
     [2:v]scale=280:-1[lum];\
     [bg][lum]overlay=W-w-20:H-h-20[v1];\
     [v1]ass='subtitles.ass'[out]" \
  -map "[out]" -map 1:a \
  -c:v libx264 -preset fast -crf 22 \
  -c:a aac -b:a 128k -pix_fmt yuv420p \
  -t <精确秒数> output.mp4
```

### 可选：capcut-mate 剪映草稿 → [[capcut-mate]]
```bash
cd D:\AI\mywork\myshipintest\claude-code-video-toolkit\_external\capcut-mate
uv run main.py  # 启动服务器 localhost:30000
```
然后通过 API 创建草稿 → 添加素材 → 渲染。但最终合成建议用 FFmpeg。

## Step 5-6: 发布与追踪

- 发布平台: 抖音 + B 站
- 策略: v2 表情包版先发测试
- 根据数据反馈优化下一首歌

## 关键文件路径

| 用途 | 路径 |
|------|------|
| 项目根目录 | `D:\AI\mywork\AIMUSIC` |
| 音频输出 | `output/songs/` |
| 视频输出 | `output/videos/` |
| 表情包素材 | `output/memes/` |
| 字幕文件 | `output/subtitles_v2.ass` |
| concat 列表 | `output/concat_list.txt` |
| 项目状态 | `output/project.md` |
| ACE-Step CLI | `D:\AI\mywork\myshipintest\claude-code-video-toolkit\tools\music_gen.py` |
| FFmpeg (完整版) | `D:\AI\mp3zhuanhuan\dist\ffmpeg.exe` |
| capcut-mate | `D:\AI\mywork\myshipintest\claude-code-video-toolkit\_external\capcut-mate` |
