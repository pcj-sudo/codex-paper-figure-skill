# Codex Paper Figure Skill · 本地版本存档

这是 pcj-sudo 使用的本地安装版本（skill 元数据版本 `0.0.2`），用于跨机器安装和复用。`codex-paper-figure-skill/` 中的文件按 2026-10-27 本机安装内容原样保存。

上游项目：[pengqianhan/codex-paper-figure-skill](https://github.com/pengqianhan/codex-paper-figure-skill)。原作者 Pengqian Han；沿用 MIT 许可证。本仓库独立保存本地快照，不代表上游最新版。

## 能做什么

从论文段落、方法描述或作图需求出发，先用 Codex 的图像生成工具探索构图，再重建为原生可编辑的 draw.io XML。文字、形状、连接线均可继续编辑；可按需导出 PDF、SVG、PNG。

这个本地版本包含科学依赖图检查、复刻字体与线宽校准、论文尺寸可读性检查、连接线避让，以及官方 draw.io 渲染与导出要求。

## 安装

在安装了 Codex 的机器上执行：

```bash
git clone https://github.com/pcj-sudo/codex-paper-figure-skill.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R codex-paper-figure-skill/codex-paper-figure-skill "${CODEX_HOME:-$HOME/.codex}/skills/"
```

如果目标目录已存在，请先备份并移走旧版本，再执行复制，避免合并遗留文件。安装后重新打开 Codex 会话。

也可以让 Codex 使用 skill-installer 安装本仓库中的 `codex-paper-figure-skill` 子目录。

## 使用

在 Codex 中输入：

> 用 $codex-paper-figure-skill，根据下面的方法描述绘制论文总览图，交付可编辑 .drawio，并导出 PDF、SVG 和 PNG：……

需要 Codex 提供图像生成能力。自动导出建议安装 draw.io Desktop，Linux 无图形界面的环境可能需要 Xvfb；无法渲染时仍可交付 `.drawio`，在 diagrams.net 中打开。

## 文件

- `codex-paper-figure-skill/SKILL.md`：完整工作流。
- `codex-paper-figure-skill/agents/openai.yaml`：Codex 界面配置。
- `snapshot.json`：本地快照来源、版本和文件 SHA256。
- `LICENSE`：上游 MIT 许可证。

仓库只包含 skill 和使用说明，不包含论文、实验数据、生成图片、账号信息或本机运行环境。
