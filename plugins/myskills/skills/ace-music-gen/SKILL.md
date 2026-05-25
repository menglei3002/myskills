---
name: ace-music-gen
description: ACE-Step 音乐生成工作流。当用户需要生成AI音乐、写歌、创作歌曲时使用。触发词：ACE-Step、生成音乐、AI写歌、创作歌曲、编曲。
---

# ACE-Step 音乐生成

## 前置条件

- `ACEMUSIC_API_KEY` 已在 `.env` 中配置
- 工作目录: `D:\AI\mywork\myshipintest\claude-code-video-toolkit`

## 关键避坑

### 1. 代理问题
ACE-Step 请求会被 Windows 系统代理拦截，必须绕过：
```bash
NO_PROXY="*" no_proxy="*" python tools/music_gen.py ...
```
不加 `NO_PROXY="*"` 会报 `ProxyError: Unable to connect to proxy`。

### 2. Thinking 模式 504 超时
`--thinking`（默认开启）会导致生成时间超过 Cloudflare 60s 网关超时，返回 HTTP 504。
**解决方案**: 加 `--no-thinking`。21s 即可完成 30s 音频。

### 3. Variations 不能批量
`--variations 3` 会导致单次请求过长而超时。
**解决方案**: 分 3 次单独调用，每次 `--variations 1`。

## 抖音搞笑中文歌参数模板

```bash
cd D:/AI/mywork/myshipintest/claude-code-video-toolkit

NO_PROXY="*" no_proxy="*" python tools/music_gen.py \
  --prompt "搞笑流行风 自嘲搞怪 沙雕感十足 轻快节奏 夸张演唱 短视频洗脑神曲" \
  --lyrics "[Chorus]\n歌词行1\n歌词行2\n\n[Verse]\n歌词行3\n歌词行4\n\n[Chorus]\n歌词行5\n歌词行6" \
  --bpm 110 \
  --duration 30 \
  --vocal-language zh \
  --variations 1 \
  --no-thinking \
  --output "D:/AI/mywork/AIMUSIC/output/songs/song_name.mp3"
```

## 参数说明

| 参数 | 推荐值 | 说明 |
|------|--------|------|
| `--prompt` | 搞笑流行风 + 情绪描述 + 编曲 | 支持中文 |
| `--lyrics` | `[Chorus]\n...\n[Verse]\n...` | 用 `\n` 换行 |
| `--bpm` | 100-120 | 搞笑洗脑风偏快 |
| `--duration` | 30 | 短视频最佳长度 |
| `--vocal-language` | zh | 中文演唱 |
| `--no-thinking` | 必加 | 避免网关超时 |
| `--variations` | 1 | 多次调用代替批量 |
| `--output` | 绝对路径 | 输出到 AIMUSIC/output/songs/ |

## 输出

- 30 秒 MP3，48000Hz，128kbps，~470KB
- 生成耗时: ~20-35s（no thinking 模式）
