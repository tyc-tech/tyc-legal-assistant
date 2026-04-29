---
name: tyc-legal-risk
description: 立案前的对手方风险排查工具 V2.0，三层穿透扫描（企业当前 × 企业历史 × 核心人员）
category: legal
version: 2.0
---

# 诉讼风险评估 V2.0（三层诉讼全景）

## 触发条件

律师立案前评估对手方诉讼泥潭、企业胜诉概率研判、应诉策略制定。

关键词：诉讼风险、对手方分析、立案评估、应诉策略

## 输入要求

- **searchKey** (必填): 对手方企业名称 或 信用代码
- **humanName** (可选): 法定代表人或核心高管姓名（启用人员层穿透）

## 执行流程

### Step 1: 企业当前层（5 维司法 + 经营）
- `get_judicial_documents` — 裁判文书
- `get_dishonest_info` — 失信
- `get_judgment_debtor_info` — 被执行
- `get_high_consumption_restriction` — 限高
- `get_terminated_cases` — 终本案件

### Step 2: 企业历史层（V2.0 新增 - 时间序列）
- `get_historical_judicial_docs` — 历史裁判文书
- `get_historical_dishonest` — 历史失信
- `get_historical_judgment_debtor` — 历史被执行
- `get_historical_court_notice` — 历史法院公告
- `get_historical_hearing_notice` — 历史开庭公告

### Step 3: 核心人员层（若提供 humanName）
- `get_personnel_dishonest` (`searchKey`, `humanName`)
- `get_personnel_judgment_debtor` (`searchKey`, `humanName`)
- `get_personnel_high_consumption_ban` (`searchKey`, `humanName`)
- `get_personnel_terminated_cases` (`searchKey`, `humanName`)

### Step 4: 案件深度（如需详情）
- `get_lawsuit_detail` (`id`) — 关键案件文书详情
- `get_judicial_case` — 司法解析

### Step 5: 综合
- `get_risk_overview`

## 输出格式

```markdown
# 诉讼风险评估 V2.0 — {name}

## 一、企业当前诉讼地位
- 累计裁判文书: {n} 件
- 当前被执行: {n} 件
- 失信记录: {n} 条
- 终本案件: {n} 件（无可执行财产）

## 二、企业历史诉讼轨迹（10 年）
| 年度 | 裁判文书 | 失信 | 被执行 | 趋势 |
|------|---------|------|--------|------|

时间序列分析:
- 诉讼频率: 高/中/低
- 历史败诉率: {rate}
- 偿付能力趋势: 改善/稳定/恶化

## 三、核心人员涉诉档案（若提供 humanName）
- 个人失信记录: {n} 条
- 个人被执行: {n} 条
- 个人限高: {n} 条
- 综合判断: ...

## 四、关键案件文书摘要
（重点案件的裁判结果与赔偿金额）

## 五、立案建议
- 胜诉概率推演: ...
- 执行风险: 高 / 中 / 低（含终本警示）
- 建议措施: 立案 / 调解 / 财产保全 / 暂缓
```

## 示例

输入: `searchKey = "ABC 被告企业"`, `humanName = "张三"`

## V2.0 升级说明

V2.0 新增"企业历史层"（5 个 history 工具）+ "核心人员层"（4 个 executive 工具），实现真实时间序列趋势分析与个人涉诉穿透。
