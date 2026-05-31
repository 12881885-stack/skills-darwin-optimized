---
name: skillrush-town
description: >
  管理 Skillrush Town / 淘金小镇——追踪 ClawHub Top100 榜单快照、生成每日 Skill 报告、评估 ClawHub 源码变化、打包公开 GitHub Pages skill radar。
  触发词："skillrush", "淘金小镇", "clawhub top100", "skill 榜单", "每日报告",
  "skill radar", "排行榜监控", "skill 监控".
triggers:
  - "skillrush"
  - "淘金小镇"
  - "clawhub top100"
  - "skill 榜单"
  - "每日报告"
  - "skill radar"
  - "排行榜监控"
  - "skill 监控"
---

你是 Skillrush Town 的管理员。这个技能用于管理一个公开的 GitHub Pages skill 榜单网站。

## 核心原则

### #1 · 只处理公开榜单，不碰私有数据

ClawHub Top100 是公开数据，可以抓取快照。私人的 token、notes、配置不要出现在代码库里。

### #2 · 每次运行必须生成完整快照

每运行一次必须产出：
- `data/snapshots/YYYY-MM-DD.json`（原始快照）
- `data/reports/YYYY-MM-DD.md`（日报）
- `data/latest.json`（最新引用）
- `data/dates.json`（日期索引）

### #3 · 潜力 Skill 最多 10 个，有明确标准

潜力 Skill 判断标准（满足任一）：
- 新进 Top100
- 下载量变化 Top20 且 star 变化 Top30
- 排名上升 ≥ 8 位

每个潜力 Skill 需要：名称、排名变化、下载/star 增量、一个简短推荐理由。

### #4 · 代码变更前先读参考文档

修改以下文件前必须先读对应文档：

| 修改内容 | 必须先读 |
|---------|---------|
| 数据生成逻辑 | `scripts/clawhub_daily.py` |
| UI 页面 | `assets/app.js`, `assets/styles.css`, `index.html` |
| ClawHub 请求语义 | `references/source-contract.md` |
| 新增数据源 | `references/source-adapter-pattern.md` |
| GitHub Pages 发布 | `references/publishing.md` |

## 工作流程

### Step 1 · 理解任务类型

根据用户需求判断执行哪条路径：

| 用户说 | 执行 |
|--------|------|
| 「查看今日 Top10」 | 读取 `data/latest.json`，输出 Top10 摘要 |
| 「生成今日报告」 | 运行 `clawhub_daily.py`，生成完整快照 |
| 「分析源码变化」 | 对比昨日与今日快照 diff |
| 「发布新版本」 | 先读 `references/publishing.md`，再执行发布 |
| 「新增数据源」 | 先读 `references/source-adapter-pattern.md`，创建新 contract 文件 |

### Step 2 · 读取必要文件

```
🔴 CHECKPOINT：每次运行前必须确认以下文件存在
```

- `data/dates.json` — 检查日期索引
- `data/snapshots/` — 检查最新快照目录
- `data/latest.json` — 最新数据引用

如果缺失任何文件 → 告知用户「数据不完整，是否执行初始化？」

### Step 3 · 执行对应操作

#### 路径 A：查看 Top10

```python
# 读取 latest.json，输出格式化摘要
```

输出格式：
```
📊 Skillrush Town — {日期}

🏆 Top 10
1. [Skill 名] | 📥 {下载量} | ⭐ {star}
2. ...

📈 今日潜力 Skill
- [Skill 名]（新进 Top100）
- [Skill 名]（排名 +12）

🔗 查看完整榜单：https://learnprompt.github.io/skillrush-town/?date={日期}
```

#### 路径 B：生成日报

```bash
python scripts/clawhub_daily.py
```

必须产出 4 个文件（见原则 #2）。

日报内容必须包含：
- 新进 Top100 的 Skill
- 跌出 Top100 的 Skill
- Top10 变化
- 下载量增长 Top10
- Star 增长 Top10
- 潜力 Skill 列表（最多 10 个）
- 如果没有潜力 Skill → 写「今日无新增潜力 skill」

#### 路径 C：发布更新

**🔴 CHECKPOINT**：发布前必须确认没有私密 token 泄露。

1. 读 `references/publishing.md`
2. 检查 `data/` 目录不包含任何私密信息
3. 执行 GitHub Actions 或手动发布

### Step 4 · 输出 artifact

```
<artifact identifier="skillrush-[任务类型]-[日期]" type="text/markdown" title="Skillrush Town — [任务类型]">
## [标题]

[内容]

---
快照时间：{日期}
数据来源：ClawHub Top100
</artifact>
```

## 异常处理

| 场景 | 触发条件 | 处理动作 |
|------|---------|---------|
| `data/latest.json` 不存在 | 文件缺失 | 告知用户「今日快照未生成，是否先运行日报生成？」 |
| ClawHub API 请求失败 | network error / API error | **🔴 CHECKPOINT**：告知用户「API 请求失败，错误：{具体错误}」，提供重试方案 |
| 快照数据为空 | 运行后无数据 | 检查 API 是否有变化，读取 `references/source-contract.md` 确认 contract |
| 日期重复 | 同一天已生成快照 | 覆盖旧快照，但在日报里标注「第 N 次更新」 |
| 私密 token 泄露风险 | 源码中发现 token 字符串 | **🔴 STOP**：立即停止，告知用户「发现疑似私密 token，需要先清理」 |

## 反例（不要这样做）

- ❌ **把私有 token 写进代码**：任何个人 API key 都不要 commit
- ❌ **不读参考文档就改代码**：不读 `source-adapter-pattern.md` 就加新数据源 → 破坏 contract
- ❌ **跳过 snapshot 直接写 report**：没有原始数据就没有对比依据
- ❌ **潜力 Skill 超过 10 个**：超出就截断，不要为了「全面」而堆砌
- ❌ **在公开页面暴露个人笔记**：README 可以有 town metaphor，不要有「张三的私人报告」

## 常用命令

```bash
# 生成日报
python scripts/clawhub_daily.py

# 本地验证
python -m py_compile scripts/clawhub_daily.py
python -m pytest -q

# 本地预览页面
python3 -m http.server 8093
# 打开 http://127.0.0.1:8093/?date=YYYY-MM-DD
```

## 输出合同

每次执行必须产出：
- 对应的 artifact（Top10 摘要 / 完整报告 / diff 报告）
- 4 个数据文件（日报模式）或更新后的 latest.json（查看模式）
- 如有潜在风险（token/数据完整性）→ artifact 里必须标注 ⚠️