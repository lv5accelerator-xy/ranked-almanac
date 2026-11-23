# 上分黄历：游戏娱乐黄历

[![CI](https://github.com/lv5accelerator-xy/ranked-almanac/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/lv5accelerator-xy/ranked-almanac/actions/workflows/ci.yml)

基于干支、五行、十二时辰与基础命宫意象制作的游戏娱乐黄历：看今天哪个时辰更适合排位、ARAM 还是 TFT。纯前端，数据只保存在浏览器 localStorage，无服务器、无账号。

**[▶ 在线使用](https://lv5accelerator-xy.github.io/ranked-almanac/)**

> 仅供娱乐与传统文化体验，不代表科学预测，也不保证游戏胜负。

## 功能

- 切换日期，联动查看农历、纳音、日柱与全天评分。
- 查看当前时辰更适合：排位 / ARAM / TFT，以及十二时辰全天评分和各模式黄金三时辰。
- 首次填写出生日期和时间，自动换算农历月份与时辰；之后可关闭命宫加成。
- AI 解签可选：默认使用本地规则，未配置端点时不联网。
- 桌面两侧为本地 SVG 八卦纹样背景。

## 本地运行

无需安装依赖、无需构建，运行 `python -m http.server 8000` 后访问 <http://localhost:8000/>。

## 数据与隐私

- 首次填写只保存 `enabled`、`lunarMonth`、`birthHourIndex`；已有有效资料仍可读取，不自动清理旧字段。命盘弹窗中的“清除本地命盘资料”只删除本项目的 `shangfenHuangliProfileV1` 键，随后恢复首次填写。
- AI 只有配置了 `window.SHANGFEN_AI_ENDPOINT` 且点击“重新解签”时才请求；超过 15 秒自动取消并回退本地规则，也可点击“取消请求”。相同输入刷新保留当前结果；输入变化即取消旧请求并清除结果，只保留当前输入的单条内存结果，不保留历史缓存。
- 日期和时辰按设备本地时区，日柱零点换日；闰月按同名月份处理。本版仅使用基础命宫权重，不是完整八字或紫微排盘。

## 仓库结构

| 路径 | 内容 |
| --- | --- |
| `index.html`、`styles.css` | 入口页与样式 |
| `calendar.js` | 干支、时辰与关系计算（统一在此） |
| `app.js`、`extras.js`、`onboarding.js`、`ai-fortune.js` | 页面状态、扩展面板、首次填写与可选 AI 解签 |
| `assets/` | 八卦纹样背景 |
| `tests/` | Node 回归测试与冻结评分样本 |
| `.github/` | PR 检查与 Pages 部署工作流 |

## 开发与测试

需要 Node.js 24，不是使用前提。

```bash
node --test tests/*.test.cjs
```

- 提交前对 `calendar.js`、`app.js`、`extras.js`、`ai-fortune.js`、`onboarding.js` 分别运行 `node --check`，并运行 `git diff --check`。
- 回归测试使用 mock fetch、可控时钟和定时器，不访问 AI 服务；评分样本冻结自 main `cb01f71a200569926475624e8f8e8da90619a430`。
- PR CI 只读检查语法和全部测试。

## 发布

GitHub Pages 只在 `main` push 或手动触发时部署，部署前执行与 CI 相同的验证。

## 授权

仓库当前未附带开源许可证。仓库公开可见不代表代码可被任意复制、修改或再发布；如需使用，请先联系仓库所有者取得授权。
