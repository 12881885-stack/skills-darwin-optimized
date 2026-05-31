# Darwin Optimized Skills

> 达尔文.skill 优化成果——72 skills 基线评估 + 8 个 skills Phase 2 优化

## 优化成果

| Skill | 旧分 | 新分 | 增量 |
|-------|------|------|------|
| mobile-onboarding | 20 | ~60 | +40 |
| agency-finder | 36 | ~60 | +24 |
| pm-spec | 47 | ~68 | +21 |
| team-okrs | 48 | ~70 | +22 |
| refero-fetcher | 49 | ~60 | +11 |
| weekly-update | 49 | ~60 | +11 |
| eng-runbook | 55 | ~65 | +10 |
| skillrush-town | 55 | ~60 | +5 |

## 核心发现

85% 的 skills 存在以下系统性问题：

1. **无检查点** — 没有🔴/🛑/STOP显性标记
2. **无失败分支** — 没有"如果X失败→Y"显式分支
3. **无反例清单** — 反例缺失或只有1条
4. **软化措辞** — 含"if applicable"/"建议"/"可以考虑"

## 文件结构

```
optimized-skills/
├── mobile-onboarding/SKILL.md      # +205行，从8行极简到完整workflow
├── agency-finder/SKILL.md          # +133行，新增步骤+异常处理+artifact规范
├── pm-spec/SKILL.md                # +209行，重写PRD结构+三段式+检查点
├── team-okrs/SKILL.md              # +198行，新增OKR原则+三态系统+检查点
├── refero-fetcher/SKILL.md        # +156行，新增截图策略+异常处理
├── weekly-update/SKILL.md          # +151行，重写8页结构+指标要求
├── eng-runbook/SKILL.md            # +235行，新增告警表+5步检查表+命令块
├── skillrush-town/SKILL.md         # +181行，新增4路径分支+参考文档要求
└── README.md
```

## 基线评估数据

- **72 skills** 完成红灯扫描：68 CLEAN / 4 FLAGGED
- **4个FLAGGED:** self-improving-agent、video-shortform、lovart-skill、skillrush-town
- **Git分支:** `auto-optimize/20260531-1328`（所有改动在该分支上）

## 来源

- 达尔文.skill: https://github.com/alchaincyf/darwin-skill
- 原始 skills: `C:\Users\Administrator\.openclaw\workspace\skills\`

---
*优化日期: 2026-05-31*