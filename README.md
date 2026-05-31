# skills-darwin-optimized

> 达尔文.skill 优化成果——72 skills 基线评估 + 8 个 skills Phase 2 优化

## 优化成果（已优化）

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

## 基线评估数据

- **72 skills** 完成红灯扫描：68 CLEAN / 4 FLAGGED
- **4个FLAGGED:** self-improving-agent、video-shortform、lovart-skill、skillrush-town

## 目录结构

```
skills-darwin-optimized/
├── README.md                    # 本文件
├── optimized/                   # 已优化的 8 个 skills
│   ├── mobile-onboarding/
│   ├── agency-finder/
│   ├── pm-spec/
│   ├── team-okrs/
│   ├── refero-fetcher/
│   ├── weekly-update/
│   ├── eng-runbook/
│   └── skillrush-town/
└── original/                    # 其余 64 个 skills（基线评分版）
    ├── audio-jingle/
    ├── blog-post/
    ├── critique/
    ├── dashboard/
    ├── dating-web/
    ├── design-brief/
    ├── digital-eguide/
    ├── docs-page/
    ├── email-marketing/
    ├── finance-report/
    ├── gamified-app/
    ├── guizang-html-to-pptx/
    ├── guizang-ppt/
    ├── hatch-pet/
    ├── hr-onboarding/
    ├── html-ppt/
    ├── html-ppt-*/
    ├── huashu-design/
    ├── hyperframes/
    ├── image-poster/
    ├── invoice/
    ├── kami-deck/
    ├── kami-landing/
    ├── kanban-board/
    ├── lovart-skill/
    ├── magazine-poster/
    ├── meeting-notes/
    ├── mobile-app/
    ├── motion-frames/
    ├── open-design-landing/
    ├── open-design-landing-deck/
    ├── pptx-html-fidelity-audit/
    ├── pricing-page/
    ├── product-copywriting/
    ├── replit-deck/
    ├── saas-landing/
    ├── self-improving-agent/
    ├── simple-deck/
    ├── social-carousel/
    ├── sprite-animation/
    ├── taste-skill/
    ├── tweaks/
    ├── video-shortform/
    ├── web-prototype/
    ├── web-prototype-*/
    └── wireframe-sketch/
```

## 来源

- 达尔文.skill: https://github.com/alchaincyf/darwin-skill
- 原始 skills: `C:\Users\Administrator\.openclaw\workspace\skills\`

---
*备份日期: 2026-05-31*