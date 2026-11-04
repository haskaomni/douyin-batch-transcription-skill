# 抖音博主文稿归档 Skill

用于 Hermes Agent 的抖音账号作品采集与文稿归档工作流。支持视频语音转写、图文作品 OCR、断点续跑、逐条摘要与归档完整性校验。

## 安装

需要先安装 [Hermes Agent](https://hermes-agent.nousresearch.com/docs)。将本仓库克隆到当前 Hermes profile 的 skills 目录；默认 profile 示例：

```bash
git clone https://github.com/haskaomni/douyin-batch-transcription-skill.git ~/.hermes/skills/douyin-batch-transcription
```

如果目标目录已存在，请先备份原有 skill。重新启动会话，让 Hermes 加载新增 skill。

## 使用

向 Hermes 提供抖音博主主页链接，例如：

> 请使用 douyin-batch-transcription，归档这个抖音博主可以正常访问的作品，提取视频逐字稿和图文 OCR，保留时间戳或图片页码，并报告已发现、下载、转写、摘要的数量及缺失原因：<博主主页链接>

也可以先要求处理一条代表作品，确认识别质量与耗时后再批量处理。

## 环境依赖

这是供 Agent 遵循的操作指南，不是独立爬虫，也没有随附一键运行脚本。Agent 需要根据实际环境准备以下工具：

- 已连接正常浏览器的 OpenCLI，用于读取可访问的抖音作品；需要登录时由使用者在浏览器正常登录。
- FFmpeg / ffprobe，用于验证媒体和提取 16 kHz 单声道音频。
- faster-whisper 或适合环境的 ASR 工具，用于语音转写。已记录的兼容组合为 faster-whisper 1.2.1 与 PyAV 16.1.0。
- 具备 OCR 能力的工具，用于图文作品逐页提取。
- 足够的本地存储和计算资源；批量执行前先测量单条转写耗时。
- 如需发布归档结果，再配置 Git 和 GitHub CLI；本地归档不需要 GitHub。

浏览器连接、模型下载和工具安装均需按各工具的文档单独配置；安装这个 skill 不会自动完成这些步骤。

## 工作流要点

1. 分页发现作品，保存游标和作品 ID，去重并核对可见数量。
2. 区分视频与图文，图文背景音乐的识别结果不能当作文稿。
3. 保存并验证本地媒体，为每条作品记录独立处理状态。
4. 视频保留原始带时间戳 ASR，图文保留原始图片及逐页 OCR。
5. 将整理后的摘要与原始识别结果分开保存，注明识别不确定性和证据范围。
6. 验收时分别统计下载、转写、OCR、摘要与发布数量，明确登录限制和缺失项。

完整规则见 [SKILL.md](SKILL.md)。其中还包含财务表述、案例证据边界和跨博主内容研究的整理规范。

## 数据与分享

本仓库只包含 skill 与使用说明，不包含博主作品、转写文稿、Cookie、登录凭据或个人浏览器配置。使用者应自行确认素材的使用与分享权限，保留来源标注。遇到登录或访问限制，应正常登录或报告限制。

## License

MIT，见 [LICENSE](LICENSE)。许可证适用于本仓库的指南内容，不授予任何第三方作品的权利。
