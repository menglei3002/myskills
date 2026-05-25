# myskills — SuperPowers Skills

个人 AI 工具技能集，可全局安装到 Claude Code 中使用。

## 技能列表

| 技能 | 用途 | 触发词 |
|------|------|--------|
| `aimusic-pipeline` | AI 音乐短视频完整流水线：选题→歌词→生成→视频→发布 | AI音乐, 搞笑短视频, 洗脑神曲 |
| `ffmpeg-video-compose` | FFmpeg 合成短视频：图片序列+音频+ASS字幕+数字人overlay | FFmpeg, 视频合成, 字幕烧录 |
| `ace-music-gen` | ACE-Step 音乐生成：参数模板与避坑指南 | ACE-Step, 生成音乐, 写歌 |
| `capcut-mate` | 剪映草稿创建与操作：通过 API 创建/编辑/渲染剪映项目 | 剪映, capcut, 草稿 |

## 安装

```bash
# 在 Claude Code 中添加 marketplace
claude plugins marketplace add myskills D:/AI/mywork/myskill

# 或直接复制到全局 skills 目录
cp -r plugins/myskills/skills/* ~/.claude/skills/
```

## 目录结构

```
myskill/
├── .claude-plugin/marketplace.json   # 市场定义
├── plugins/myskills/                 # 主插件
│   ├── .claude-plugin/plugin.json
│   └── skills/                       # 技能文件
│       ├── aimusic-pipeline/
│       ├── ffmpeg-video-compose/
│       ├── ace-music-gen/
│       └── capcut-mate/
└── README.md
```
