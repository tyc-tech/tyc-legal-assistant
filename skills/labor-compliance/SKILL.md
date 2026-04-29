---
name: tyc-labor-compliance
description: 企业用工合规的系统性排查工具，识别潜在劳动争议隐患与用工违规风险
category: legal
version: 1.0
---

# 劳动合规风险排查

## 触发条件

并购尽调、供应商合规审查、新客户准入评估、HR 部门合规自查。

关键词：劳动合规、用工风险、社保欠缴、劳动争议、工会

## 输入要求

- **searchKey** (必填): 企业名称 或 信用代码

## 执行流程

### Step 1: 工商与规模
- `get_company_registration_info`
- `get_company_scale` — 参保人数 vs 员工规模

### Step 2: 用工相关风险
- `get_administrative_penalty` — 行政处罚（含劳动监察类）
- `get_serious_violation` — 严重违法
- `get_tax_arrears_notice` — 欠税（涉及社保汇缴）
- `get_business_exception` — 经营异常（含未公示事项）

### Step 3: 劳动相关司法
- `get_judicial_documents` — 裁判文书（筛选劳动争议类）
- `get_case_filing_info` — 立案信息

### Step 4: 招聘活跃度
- `get_recruitment_info` — 在招职位

## 输出格式

```markdown
# 劳动合规风险排查 — {name}

## 一、用工规模
- 参保人数: {socialStaffNum}
- 员工规模: {staffNumRange}
- 参保覆盖率: 推断 (参保 / 员工总数)

## 二、用工违规历史
| 时间 | 处罚类型 | 处罚金额 |
|------|---------|---------|

（重点关注：拖欠工资 / 不签合同 / 不缴社保 / 安全生产）

## 三、劳动争议司法案件
- 累计劳动争议案件: {n}
- 公司败诉率: ...
- 最近案例: ...

## 四、社保 / 税务异常
- 欠税记录: {n} 条
- 经营异常: 涉及社保未公示？是 / 否

## 五、招聘动态
- 在招职位: {n}
- 是否合规标注五险一金: ...

## 六、合规结论
- 用工合规等级: A / B / C
- 重点风险: ...
- 整改建议: ...
```

## 示例

输入: `searchKey = "某劳动密集型企业"`
