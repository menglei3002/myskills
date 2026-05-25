---
name: ffmpeg-video-compose
description: FFmpeg 合成短视频工作流。当用户需要将图片序列、音频、ASS字幕、数字人overlay合成为视频时使用。触发词：FFmpeg、视频合成、字幕烧录、图片转视频、数字人视频。
---

# FFmpeg 短视频合成

## 为什么用 FFmpeg 而不是剪映 API

capcut-mate 创建的草稿使用加密格式 (`draft_content.json`)，推送到本地剪映后可能出现图片不显示、音画不同步、数字人 overlay 消失等问题。**最终合成直接用 FFmpeg，可靠且可复现。**

## 完整合成命令

```bash
FFMPEG="D:/AI/mp3zhuanhuan/dist/ffmpeg.exe"

"$FFMPEG" -y \
  -f concat -safe 0 -i concat_list.txt \
  -i audio.mp3 \
  -i digital_human.png \
  -filter_complex \
    "[0:v]scale=1080:1920,fps=30[bg];\
     [2:v]scale=280:-1[lum];\
     [bg][lum]overlay=W-w-20:H-h-20[v1];\
     [v1]ass='subtitles.ass'[out]" \
  -map "[out]" -map 1:a \
  -c:v libx264 -preset fast -crf 22 \
  -c:a aac -b:a 128k \
  -pix_fmt yuv420p \
  -t 30.024 \
  output.mp4
```

## 滤镜链

| 步骤 | 滤镜 | 作用 |
|------|------|------|
| 1 | `[0:v]scale=1080:1920,fps=30[bg]` | 背景图片序列缩放到竖屏 |
| 2 | `[2:v]scale=280:-1[lum]` | 数字人缩放到 280px 宽 |
| 3 | `[bg][lum]overlay=W-w-20:H-h-20[v1]` | 数字人叠加到右下角 |
| 4 | `[v1]ass='subtitles.ass'[out]` | 烧录 ASS 字幕 |

## Concat 文件格式

```
file 'D:/path/to/image_00.png'
duration 3.5
file 'D:/path/to/image_01.png'
duration 3.5
...
```

- 最后一张也需要 `duration`
- `-safe 0` 允许绝对路径

## ASS 字幕模板

```ini
[Script Info]
Title: 视频标题
ScriptType: v4.00+
PlayResX: 1080
PlayResY: 1920
WrapStyle: 2

[V4+ Styles]
Format: Name, Fontname, Fontsize, PrimaryColour, SecondaryColour, OutlineColour, BackColour, Bold, Italic, Underline, StrikeOut, ScaleX, ScaleY, Spacing, Angle, BorderStyle, Outline, Shadow, Alignment, MarginL, MarginR, MarginV, Encoding
Style: Default,Microsoft YaHei,48,&H00FFFFFF,&H000000FF,&H00000000,&H80000000,-1,0,0,0,100,100,0,0,1,4,1,2,80,80,200,1

[Events]
Format: Layer, Start, End, Style, Name, MarginL, MarginR, MarginV, Effect, Text
Dialogue: 0,0:00:00.00,0:00:03.50,Default,,0,0,0,,歌词第一句
Dialogue: 0,0:00:03.50,0:00:07.00,Default,,0,0,0,,歌词第二句
```

- `Microsoft YaHei` = 微软雅黑，中文兼容好
- `Bold=-1` = 粗体，`Outline=4` = 描边宽度
- 时间戳精确到 10ms

## 音频时长对齐

AI 生成的音频时长可能不是精确整数。用 ffprobe 获取精确值：

```bash
ffprobe -v quiet -print_format json -show_format audio.mp3
# 取 format.duration
```

FFmpeg 的 `-t` 参数用精确秒数截断视频到音频长度。

## FFmpeg 版本要求

**不能用剪映自带的 FFmpeg** — 缺少 libass/libx264/overlay 等关键库。

正确版本: `D:/AI/mp3zhuanhuan/dist/ffmpeg.exe` (gyan.dev 完整编译版)
