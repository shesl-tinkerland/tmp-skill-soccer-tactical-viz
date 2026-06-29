---
name: soccer-tactical-viz
source_url: https://github.com/Ddhjx-code/soccer_plan
description: |
  从世界杯比赛事件数据中提取精彩时刻，生成战术板可视化图 + AI中文解说文案。
  支持单场分析、整届批量处理、实时监控三种模式。
---

# 足球战术可视化 + AI 解说

来源: [Ddhjx-code/soccer_plan](https://github.com/Ddhjx-code/soccer_plan)
发现: [小红书笔记](https://www.xiaohongshu.com/explore/6a229431000000000803fdf0)

## 总览

本技能将世界杯比赛的事件数据（传球、射门、带球等）转化为战术板图和中文解说文案，
适合发布到小红书或微信公众号。底层使用 Python CLI 工具 `soccer_plan`，支持
StatsBomb 开放数据、Understat 射门数据和 api-football 实时数据。

## 用户任务

用户可以要求：

- **单场分析**：给出一场比赛（如"阿根廷 vs 法国 2022世界杯决赛"），获得该场
  比赛的战术板图和解说
- **批量处理**：指定一届赛事（如"2022世界杯"），批量生成所有比赛的分析
- **实时监控**：开启 2026 世界杯实时监控，比赛结束后自动生成分析

## 输入

- 比赛描述或赛事名称（自然语言，中英文均可）
- 可选：输出风格（小红书/微信/默认）、高光数量（默认 top 5）

## 输出

- 每场比赛的战术板 PNG 图（进攻路线、传球网络、射门分布）
- 每个高光时刻的 150-200 字中文战术解读（小红书风格）
- 汇总 HTML 页面

## 子技能路由

根据用户意图选择对应模式：

### `/soccer analyze <比赛描述>`
单场分析流水线：解析比赛 → 拉取事件数据 → 提取高光 → 生成战术板图 → AI 解说 → 组装 HTML。
详见 [soccer-analyze.md](https://raw.githubusercontent.com/Ddhjx-code/soccer_plan/main/skill/soccer-analyze.md)

### `/soccer batch <赛事>`
批量模式：列出赛事全部比赛 → 逐场执行分析流水线 → 汇总报告。
详见 [soccer-batch.md](https://raw.githubusercontent.com/Ddhjx-code/soccer_plan/main/skill/soccer-batch.md)

### `/soccer realtime`
实时监控：轮询 api-football 接口 → 比赛结束自动触发分析 → 通知用户。
需要 `API_FOOTBALL_KEY` 环境变量。
详见 [soccer-realtime.md](https://raw.githubusercontent.com/Ddhjx-code/soccer_plan/main/skill/soccer-realtime.md)

## 环境要求

```bash
cd <项目根目录>
pip install -e .
```

Python 3.11+，依赖 mplsoccer、matplotlib、click、pydantic、requests、Jinja2。

## 数据源

| 数据源 | 覆盖 | 费用 |
|--------|------|------|
| StatsBomb Open Data | 2018/2022 世界杯 128 场完整事件+坐标 | 免费 |
| Understat | 五大联赛射门+xG | 免费 |
| api-football | 全球赛事实时比分 | ~$20/月 |

## CLI 命令参考

```bash
python -m soccer_plan --help              # 查看帮助
python -m soccer_plan list-matches ...    # 列出比赛
python -m soccer_plan fetch ...           # 拉取数据
python -m soccer_plan analyze ...         # 提取高光
python -m soccer_plan viz ...             # 生成战术板
python -m soccer_plan export-html ...     # 组装 HTML
python -m soccer_plan monitor ...         # 实时监控
```
