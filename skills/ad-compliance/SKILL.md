---
name: tyc-ad-compliance
description: 广告投放前的合规风险排查工具，核验广告主资质与潜在违规风险
category: legal
version: 1.0
---

# 广告合规审查

## 触发条件

广告代理机构客户准入、广告主背景调查、规避代理违规广告主的连带责任。

关键词：广告合规、广告主审查、代理风险、连带责任

## 输入要求

- **searchKey** (必填): 广告主企业名称 或 信用代码

## 执行流程

### Step 1: 主体合规
- `get_company_registration_info` — 工商基础
- `verify_company_accuracy` — 三要素

### Step 2: 资质许可（行业准入）
- `get_administrative_license` — 行政许可
- `get_qualifications` — 资质证书
- `get_telecom_license` — 电信类许可（互联网/广告类需）

### Step 3: 历史违规
- `get_administrative_penalty` — 行政处罚（含广告法处罚）
- `get_serious_violation` — 严重违法
- `get_news_sentiment` — 媒体负面舆情

### Step 4: 经营异常
- `get_business_exception`

## 输出格式

```markdown
# 广告主合规审查 — {name}

## 一、主体合规
## 二、行业准入资质
- 必备许可清单: ...
- 已获许可: ...
- 缺失许可（红线）: ...

## 三、历史违规记录
| 时间 | 违规类型 | 处罚机关 | 金额 |
|------|---------|---------|------|

## 四、媒体舆情
（近 3 个月负面新闻摘要）

## 五、合规审查结论
- 准入决策: ✓ 接受 / ⚠️ 加附约 / ❌ 拒绝
- 重点风险: ...
- 合同条款建议: 增加合规担保条款 / 知产授权条款 / ...
```

## 示例

输入: `searchKey = "某广告主企业"`
