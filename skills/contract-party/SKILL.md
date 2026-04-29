---
name: tyc-contract-party
description: 合同签署前的主体快速核验工具，自动查验对方工商状态/法定代表人/经营异常
category: legal
version: 1.0
---

# 合同相对方主体核验

## 触发条件

合同签字盖章前的快速主体核验、防范无效合同 / 履约失败风险。

关键词：合同主体、签约前核查、主体真实性、注销吊销

## 输入要求

- **searchKey** (必填): 合同对方企业名称 或 信用代码
- **legalPersonName** (可选): 合同甲方法定代表人姓名（启用三要素核验）

## 执行流程

### Step 1: 主体存续
- `get_company_registration_info` — 工商状态
- `get_history_names` — 曾用名（避免老合同名称失效）

### Step 2: 三要素一致性
- `verify_company_accuracy` — 名称/法人/信用代码

### Step 3: 红线检查
- `get_business_exception` — 经营异常
- `get_serious_violation` — 严重违法
- `get_cancellation_record_info` — 注销备案

### Step 4: 联系信息
- `get_contact_info` — 联系电话/邮箱（用于交付）

## 输出格式

```markdown
# 合同主体快速核验 — {name}

## 一、主体存续状态
- ✓ / ❌ 工商登记: {regStatus}
- ✓ / ❌ 三要素核验: 一致 / 不一致

## 二、企业真实性
- 当前名称: {name}
- 曾用名: {historyNames}（如有）
- 注册时间: {estiblishTime}
- 注册资本 / 实缴: ...

## 三、红线警告
| 红线 | 触发 | 建议 |
|------|------|------|
| 营业执照注销 | 是/否 | 终止签约 |
| 营业执照吊销 | 是/否 | 终止签约 |
| 经营异常 | {n} 条 | 加强审核 |
| 严重违法 | {n} 条 | 加保函 |

## 四、联系方式核验
- 电话: ...
- 邮箱: ...
- 官网: ...

## 五、签约决策
- 是否可以签约: ✓ 是 / ❌ 否
- 加固条款建议: ...
```

## 示例

输入: `searchKey = "ABC 公司"`, `legalPersonName = "张三"`
