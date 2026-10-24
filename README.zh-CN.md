# remotion-science-video（中文说明）

> English version: [README.md](README.md)

这是一个 Claude Code 技能，用于用 [Remotion](https://www.remotion.dev/) 制作抖音风格的科普与教学动画视频。完整流程包括：选题 → 带拍点的分镜 → 音乐卡点 → 音效与音量平衡 → 渲染 → 审片 → 封面 → 发布文案 → 数据复盘。

## 目录结构

```
remotion-science-video/
├── SKILL.md          # 英文版技能（Claude 加载的文件）
└── SKILL.zh-CN.md    # 中文版技能
```

## 安装

把技能文件夹复制到 Claude Code 的技能目录中：

```bash
cp -R remotion-science-video ~/.claude/skills/
```

之后直接让 Claude 制作 Remotion 科普视频即可，请求内容匹配技能描述时会自动加载。

## 包含什么

- 完整的操作流程、各阶段的确认点，以及分镜、音效、音量、封面、文案的规范。
- 推荐的 Remotion 项目结构和脚本名称。

## 不包含什么

- 项目代码和场景组件。技能描述的是流程和项目结构，不会自动生成项目。
- 音乐和音效文件，请自行准备。

## 环境要求

- Node.js，Remotion 4.x，ffmpeg（使用 Remotion 自带的即可渲染）。
- Python 3，并安装 `librosa` 和 `numpy`，用于节拍分析和音频混音。

## 许可

仅供学习和个人使用。使用时请自行确认音乐和素材的版权。
