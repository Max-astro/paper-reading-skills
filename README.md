# paper_notes

一个用于管理论文阅读与评审笔记的轻量项目，适合用「分层阅读 + 批判复盘」的方式沉淀长期知识；默认生成中文文档

## Directory Structure

- `sources/`：论文 PDF（建议按领域分子目录）
- `docs/`：阅读输出，通常为 `docs/<domain>/<paper>/`
- `skills/`：阅读与评审流程说明（`read-paper`、`review-paper`）

## Prerequisite

- 需预先安装 Anthropic 官方提供的 [`pdf`](https://github.com/anthropics/skills/tree/main/skills/pdf) skill（`read-paper` / `review-paper` 依赖该能力读取 PDF）

## Usage
1. 将论文放到 `sources/<domain>/`
2. 使用 `read-paper` 流程生成阅读笔记（triage / structure / deep-read / synthesis）
3. 使用 `review-paper` 流程对笔记做理解审计、批判评估和跨论文（当前docs下的其他笔记）整合
4. 在 `docs/<domain>/<paper>/` 持续迭代 `00~03` 笔记文件

## Use Cases

- 系统化论文精读与复盘
- 组会前快速形成可讲述的论文结构与关键结论
- 将零散阅读转成可检索、可复用的长期知识库
- 对同一方向多篇论文做横向对比与方法迁移
