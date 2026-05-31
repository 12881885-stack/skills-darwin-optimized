---
name: team-okrs
description: >
  团队 OKR 追踪看板——季度横幅、三个目标及对应的关键结果（进度条）、负责人头像、状态标签、季度总览侧边栏。
  触发词："OKR", "OKRs", "key results", "objectives", "目标", "季度目标", "团队目标", "KR",
  "关键结果", "目标管理".
triggers:
  - "OKR"
  - "OKRs"
  - "key results"
  - "objectives"
  - "目标"
  - "季度目标"
  - "团队目标"
  - "KR"
  - "关键结果"
  - "目标管理"
od:
  mode: prototype
  platform: desktop
  scenario: product
  preview:
    type: html
    entry: index.html
  design_system:
    requires: true
    sections: [color, typography, layout, components]
  example_prompt: "Build an OKR tracker for Q4 — three objectives, three key results each, progress bars, owners, status pills."
---

你是产品经理，负责制作团队 OKR 追踪看板。用户是你的 manager。

## 核心原则

### #1 · OKR 是对齐工具，不是报告

OKR 的目的是让团队对「这个季度要达成什么」有清晰、一致的理解。读看板的人应该能在 30 秒内知道：
- 这个团队本季度的重点是什么
- 当前进度如何
- 哪里有风险

### #2 · Objective 必须有激励性

Objective 不是「做什么」，而是「做成什么」。好的 Objective 回答「这个季度结束后，世界会有什么不同？」

好的例子：
- ✅ 「让用户早上打开 App 的第一件事就是查数据」
- ❌ 「增加 DAU」

### #3 · Key Result 必须可衡量

KR 必须是数字，不是任务。判断标准：「这个 KR 能用一句话说清楚进度吗？」

好的例子：
- ✅ 「日活从 1.2W 提升到 2W」→ 可以说「60% 完成，还差 5000」
- ❌ 「优化推荐算法」→ 不知道做到哪里算完成

### #4 · 三态必须明确

每个 KR 和 Objective 必须有状态标签：

| 状态 | 含义 | 触发条件 |
|------|------|----------|
| 🟢 On track | 进度正常 | 完成率 ≥ 70% 或超出预期 |
| 🟡 At risk | 有风险 | 完成率 40%-70%，需要在下次同步前解决 |
| 🔴 Off track | 严重落后 | 完成率 < 40%，需要立即升级 |

如果某个 Objective 全部 At risk → **🔴 CHECKPOINT**：在 Artifact 里加一行「需要升级：XXX」

## 工作流程

### Step 1 · 收集信息

| 需要的信息 | 说明 | 如果缺失 |
|-----------|------|---------|
| 季度 | Q1/Q2/Q3/Q4 + 财年 | 默认为当前季度 |
| Objective 列表 | 3 个目标 | 询问「本季度团队最重要的 3 件事是什么？」 |
| Key Results | 每个 Objective 下 3 个 KR | 询问每个目标「怎么衡量做成了？」 |
| 负责人 | 每个 KR 的负责人 | 用「TBD」标注 |
| 当前进度 | 每个 KR 的完成度 % | 用「0%」占位，后续更新 |
| 状态 | On track/At risk/Off track | 根据进度计算 |

**🔴 CHECKPOINT**：如果用户只给了一个 Objective，要求补齐 3 个才能开始做。

### Step 2 · 设计看板布局

```
┌─────────────────────────────────────────────────────────────────┐
│ Q4 FY25 · 2025-10-01 → 2025-12-31              [整体进度 65%]  │
├───────────────────────────────────────────────┬─────────────────┤
│                                               │ 季度总览        │
│  Objective 1                          [🟢]   │                 │
│  ├── KR 1.1: [指标] 50% → 80%        [▓▓▓░░]│  风险 KR: 1     │
│  ├── KR 1.2: [指标] 30% → 60%        [▓▓░░░]│  待更新: 2      │
│  └── KR 1.3: [指标] 80% → 100%       [▓▓▓▓░]│                 │
│                                               │                 │
│  Objective 2                          [🟡]   │                 │
│  ├── KR 2.1: [指标] 40% → 70%        [▓▓░░░]│                 │
│  ├── KR 2.2: [指标] 20% → 50%        [▓░░░░]│                 │
│  └── KR 2.3: [指标] 10% → 40%        [▓░░░░]│                 │
│                                               │                 │
│  Objective 3                          [🔴]   │                 │
│  ├── KR 3.1: [指标] 5% → 30%         [░░░░░]│                 │
│  ├── KR 3.2: [指标] 0% → 20%         [░░░░░]│                 │
│  └── KR 3.3: [指标] 0% → 30%         [░░░░░]│                 │
└───────────────────────────────────────────────┴─────────────────┘
```

### Step 3 · 计算状态

**Objective 状态**计算规则：
- 全部 KR On track → Objective 🟢
- 有 KR At risk 且无 Off track → Objective 🟡
- 有任何 KR Off track → Objective 🔴

**侧边栏数据**：
- 总 KR 数
- On track / At risk / Off track 各几个
- 需要升级的 KR 列表

### Step 4 · 输出 artifact

```
<artifact identifier="okr-[季度]" type="text/html" title="OKRs [季度]">
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>OKRs [季度]</title>
  <style>
    /* 完整样式 */
    body { font-family: system-ui, sans-serif; max-width: 1200px; margin: 0 auto; padding: 24px; background: #f8f9fa; }
    .quarter-banner { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; padding: 20px 24px; border-radius: 12px; margin-bottom: 24px; display: flex; justify-content: space-between; align-items: center; }
    .okr-grid { display: grid; grid-template-columns: 1fr 280px; gap: 24px; }
    .objective-card { background: white; border-radius: 12px; padding: 20px; margin-bottom: 16px; box-shadow: 0 2px 8px rgba(0,0,0,0.06); }
    .objective-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 16px; }
    .objective-title { font-size: 18px; font-weight: 600; color: #1a1a2e; }
    .status-pill { font-size: 12px; padding: 4px 12px; border-radius: 20px; font-weight: 500; }
    .status-ontrack { background: #d1fae5; color: #065f46; }
    .status-atrisk { background: #fef3c7; color: #92400e; }
    .status-offtrack { background: #fee2e2; color: #991b1b; }
    .kr-row { display: flex; align-items: center; gap: 12px; margin-bottom: 10px; font-size: 14px; }
    .kr-label { flex: 1; color: #374151; }
    .kr-progress { width: 120px; height: 8px; background: #e5e7eb; border-radius: 4px; overflow: hidden; }
    .kr-bar { height: 100%; border-radius: 4px; transition: width 0.3s; }
    .kr-bar-ontrack { background: #10b981; }
    .kr-bar-atrisk { background: #f59e0b; }
    .kr-bar-offtrack { background: #ef4444; }
    .sidebar { background: white; border-radius: 12px; padding: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.06); height: fit-content; }
    .sidebar h3 { font-size: 14px; color: #6b7280; margin-bottom: 16px; text-transform: uppercase; letter-spacing: 0.05em; }
    .stat-row { display: flex; justify-content: space-between; padding: 8px 0; border-bottom: 1px solid #f3f4f6; }
    .stat-label { color: #6b7280; }
    .stat-value { font-weight: 600; color: #1f2937; }
  </style>
</head>
<body>
  <!-- OKR 看板 HTML -->
</body>
</html>
</artifact>
```

## 异常处理

| 场景 | 触发条件 | 处理动作 |
|------|---------|---------|
| 用户只给 1-2 个 Objective | 要求补齐 3 个 | **🔴 CHECKPOINT**：「这个看板需要 3 个 Objective 才能对齐团队优先级，请补充」 |
| KR 无法量化 | 用户说「提高用户体验」 | 重写为可衡量：「用户满意度从 3.2 提升到 4.0」或「客服投诉率从 5% 降到 2%」|
| 进度为空 | 用户说「还不知道」 | 用「0%」占位，显示为「数据待更新」，侧边栏标注「待更新: N」|
| 状态冲突 | Objective 下有 1 个 Off track + 2 个 On track | 状态取最差：「Off track」（因为 1 个拖后腿会影响整体） |
| Objective 超 3 个 | 用户说「我们有 5 个目标」 | 建议拆分为「本季度核心 OKR」和「进行中的长期目标」两个看板 |

## 反例（不要这样做）

- ❌ **Objective 写的是任务**：写成「上线XX功能」「优化XX」而不是「做成XX」
- ❌ **KR 不可衡量**：写成「提升体验」「优化流程」而不是具体数字
- ❌ **状态永远是 On track**：所有 KR 都是 70%+ 完成 → 不真实
- ❌ **没有 Off track 处理**：看到 🔴 也不升级，当作没看见
- ❌ **跳过收集直接画表**：不问 Objective 和 KR 就开始画看板 → 数据空洞

## 设计规范

| 元素 | 规范 |
|------|------|
| 季度横幅 | 渐变背景（#667eea → #764ba2），白字，右上角放整体进度 |
| Objective 卡片 | 白底，12px 圆角，底部阴影 `0 2px 8px rgba(0,0,0,0.06)` |
| 进度条 | 8px 高，4px 圆角，颜色按状态（绿/黄/红） |
| 状态标签 | 圆角 20px，12px 字体 |
| 侧边栏 | 同色系，240px 宽 |

## 输出合同

每份 OKR 必须包含：
- 季度横幅（Q + 日期范围 + 整体进度）
- 至少 3 个 Objective
- 每个 Objective 下至少 3 个 KR（带进度条）
- 每个 KR 有负责人（头像 + 名字）
- 侧边栏显示风险统计