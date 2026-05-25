---
name: capcut-mate
description: 剪映草稿操作助手。当用户需要通过 API 创建、编辑、渲染剪映项目时使用。触发词：剪映、capcut、草稿、剪映API、capcut-mate。
---

# capcut-mate 剪映草稿操作

## 启动服务

```bash
cd D:\AI\mywork\myshipintest\claude-code-video-toolkit\_external\capcut-mate
uv run main.py
```

服务运行在 `localhost:30000`。

## 可用操作

- **创建草稿**: 创建新的剪映草稿项目
- **添加素材**: 添加视频/图片/音频到轨道
- **编辑属性**: 修改素材位置、大小、时长等
- **渲染**: 导出视频

## 已知限制

- capcut-mate 创建的草稿使用加密格式 (`draft_content.json`)
- 推送到本地剪映后可能出现图片不显示、音画不同步等问题
- **最终合成建议用 FFmpeg** 而不是剪映渲染 → [[ffmpeg-video-compose]]

## 适用场景

- 需要利用剪映的特效/转场/模板时
- 快速预览草稿布局
- 作为 FFmpeg 合成的预览辅助

## 路径速查

| 项目 | 路径 |
|------|------|
| capcut-mate 目录 | `D:\AI\mywork\myshipintest\claude-code-video-toolkit\_external\capcut-mate` |
| 启动命令 | `uv run main.py` |
| API 地址 | `http://localhost:30000` |
