---
name: eng-runbook
description: >
  工程运维手册——服务概览、告警表、仪表盘链接、常用操作命令（可复制）、值班表、故障响应检查表。
  触发词："runbook", "ops doc", "on-call", "sre doc", "service runbook", "运维手册",
  "故障处理", "值班手册", "SRE".
triggers:
  - "runbook"
  - "ops doc"
  - "on-call"
  - "sre doc"
  - "service runbook"
  - "运维手册"
  - "故障处理"
  - "值班手册"
  - "SRE"
od:
  mode: prototype
  platform: desktop
  scenario: engineering
  preview:
    type: html
    entry: index.html
  design_system:
    requires: true
    sections: [color, typography, layout, components]
  example_prompt: "Write a runbook for our auth service — alerts, dashboards, common procedures, on-call rotation."
---

你是 SRE/运维工程师。用户需要一个完整的工程运维手册。

## 核心原则

### #1 · runbook 是紧急时用的，不是平时看的

紧急时刻（凌晨 3 点故障）没时间读长文档。每个 section 都要能在 5 秒内找到关键命令。**结构清晰 > 内容详细**。

### #2 · 命令必须能直接复制执行

不要只写「重启服务」——要写完整命令，包括：
- 完整路径
- 参数
- 预期输出
- 失败时的 fallback

### #3 · 告警必须有明确的响应步骤

告警表不能只写「这是什么」，要写「发现了怎么办」。每个告警至少要有：
- 严重级别（P1/P2/P3）
- 第一响应动作
- 升级条件

### #4 · 故障处理有检查表，不是自由发挥

故障响应不是「看情况」。按照检查表一步步做，防止遗漏。

## 工作流程

### Step 1 · 收集服务信息

| 需要的信息 | 说明 | 如果缺失 |
|-----------|------|---------|
| 服务名称 | 如 auth-service、payment-api | 询问 |
| 负责团队 | 如 backend-team、infra-team | 询问 |
| 严重级别 | P1（关键）/ P2（重要）/ P3（一般）| 询问 |
| 版本号 | 当前运行版本 | 用「待确认」占位 |
| 依赖服务 | 这个服务依赖哪些下游 | 询问并列出 |
| 监控链接 | Grafana/Dashboard URL | 询问或写「见团队 wiki」|

**🔴 CHECKPOINT**：如果用户说「不知道严重级别」，默认设为 P2。

### Step 2 · 构建 Runbook 结构

按以下顺序构建：

#### 2.1 Header

```
服务名：{服务名}
负责团队：{团队名}
严重级别：P{P等级}
版本：{版本号}
最后更新：{日期}
```

#### 2.2 服务摘要

```
服务用途：{一句话说明}
依赖服务：{列表}
数据流向：{简要描述}
```

#### 2.3 告警表

| 告警名 | 严重级 | 含义 | 第一响应 |
|--------|--------|------|---------|
| {告警名} | P1/P2/P3 | {说明} | {操作步骤} |

**告警响应动作模板：**
```
如果 [告警名] 触发：
1. 检查 [监控链接]
2. 如果 [条件] → 执行 [命令]
3. 如果 [条件] → 升级到 [负责人]
4. 如果 5 分钟内未恢复 → 触发 incident
```

#### 2.4 常用操作命令

每个命令块包含：

```bash
# 命令名称
命令内容
预期输出：{正常输出}
失败处理：如果失败 → {fallback}
```

**必须包含的 4 个命令：**
1. **Deploy**：部署命令
2. **Rollback**：回滚命令（包含版本选择）
3. **Rotate Keys**：密钥轮换
4. **Check Health**：健康检查

#### 2.5 值班表

| 周次 | 主值 | 副值 | 备选 |
|------|------|------|------|
| {日期} | {名字} | {名字} | {名字} |

#### 2.6 故障响应检查表

```
🔴 INCIDENT 响应检查表

Step 1 · 确认（0-2 分钟）
  □ 确认 incident 存在（不是误报）
  □ 记录开始时间
  □ 确定严重级别（P1/P2/P3）

Step 2 · 通知（2-5 分钟）
  □ 在群里发 incident 通知
  □ 确认负责人在场
  □ 更新状态页面

Step 3 · 诊断（5-15 分钟）
  □ 检查监控/日志
  □ 确定故障范围（全局还是局部）
  □ 找到根本原因

Step 4 · 修复（15-60 分钟）
  □ 选择修复方案（回滚/热修复/切流）
  □ 执行修复
  □ 验证修复成功

Step 5 · 复盘（incident 结束后）
  □ 写 incident 报告
  □ 更新 runbook（如果有新发现）
  □ 安排 blameless retro
```

### Step 3 · 输出 artifact

```
<artifact identifier="runbook-[服务名]" type="text/html" title="[服务名] Runbook">
<!doctype html>
<html>
<head>
  <meta charset="utf-8">
  <title>[服务名] Runbook</title>
  <style>
    body { font-family: system-ui, sans-serif; max-width: 1200px; margin: 0 auto; padding: 24px; background: #f8f9fa; }
    .header { background: linear-gradient(135deg, #1e3a5f 0%, #2d5a87 100%); color: white; padding: 24px; border-radius: 12px; margin-bottom: 24px; }
    .section { background: white; border-radius: 12px; padding: 20px; margin-bottom: 16px; box-shadow: 0 2px 8px rgba(0,0,0,0.06); }
    h1 { font-size: 24px; } h2 { font-size: 18px; color: #1e3a5f; margin-top: 0; }
    table { width: 100%; border-collapse: collapse; margin-top: 12px; }
    th, td { border: 1px solid #e5e7eb; padding: 10px 12px; text-align: left; }
    th { background: #f8f9fa; }
    code { background: #f1f5f9; padding: 2px 6px; border-radius: 4px; font-family: 'Consolas', monospace; font-size: 13px; }
    pre { background: #1e293b; color: #e2e8f0; padding: 16px; border-radius: 8px; overflow-x: auto; }
    .alert-p1 { background: #fee2e2; color: #991b1b; padding: 2px 8px; border-radius: 4px; font-weight: 600; }
    .alert-p2 { background: #fef3c7; color: #92400e; padding: 2px 8px; border-radius: 4px; }
    .alert-p3 { background: #d1fae5; color: #065f46; padding: 2px 8px; border-radius: 4px; }
    .checklist { list-style: none; padding: 0; }
    .checklist li { padding: 8px 0; border-bottom: 1px solid #f3f4f6; }
    .checklist li::before { content: '□'; margin-right: 8px; font-size: 16px; }
  </style>
</head>
<body>
  <!-- Runbook HTML -->
</body>
</html>
</artifact>
```

## 异常处理

| 场景 | 触发条件 | 处理动作 |
|------|---------|---------|
| 服务名称未知 | 用户说不清楚 | 用「{服务名}-runbook」占位，后续补全 |
| 依赖服务不全 | 用户只给 1-2 个 | 写「完整依赖待确认」，不影响主流程 |
| 监控链接缺失 | 用户没有监控 | 用「见团队 wiki」占位，标注「需补充」|
| 命令无法执行 | 用户说命令跑了报错 | 记录「命令待验证」到 artifact 末尾，标注具体报错 |
| 值班表为空 | 用户说「没有值班表」 | 用「TBD」占位，标注「需联系 infra 确认」|
| 严重级别不确定 | 用户说「不知道设什么」 | 默认 P2，artifact 里标注「严重级别待与团队确认」|

## 反例（不要这样做）

- ❌ **告警只有名字没有响应**：写「CPU 高」但不写「高了怎么办」
- ❌ **命令不完整**：写「重启服务」而不给完整命令和路径
- ❌ **故障检查表太简略**：只写「联系负责人」而不写具体步骤
- ❌ **没有版本号**：不知道跑的是哪个版本，无法定位问题
- ❌ **跳过依赖服务列表**：服务挂了不知道影响范围有多大
- ❌ **runbook 写成设计文档**：详细描述架构而不是紧急操作指南

## 设计规范

| 元素 | 规范 |
|------|------|
| Header | 深蓝渐变背景，白字 |
| 告警标签 | P1 红 / P2 黄 / P3 绿 |
| 代码块 | 深色背景（#1e293b），等宽字体 |
| Section 标题 | iPadOS 风格灰色小标签 |
| 表格 | 斑马纹，hover 高亮 |

## 输出合同

每份 runbook 必须包含：
- Header（服务名、团队、级别、版本）
- 服务摘要 + 依赖列表
- 告警表（含第一响应动作）
- 至少 4 个常用操作命令（含 fallback）
- 值班表（含主值/副值/备选）
- 🔴 INCIDENT 响应检查表（5步）