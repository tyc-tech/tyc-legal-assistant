---
name: tyc-debt-recovery
description: 诉前财产保全决策的核心评估工具 V2.0，财务硬数据 + 资产清单 + 历史信用 + 实控人画像
category: legal
version: 2.0
---

# 债务清偿能力评估 V2.0（财务底盘升级版）

## 触发条件

诉前财产保全决策、债权追偿可行性评估、申请破产前的资产盘查。

关键词：债务清偿、财产保全、追偿评估、资产盘查、实控人兜底

## 输入要求

- **searchKey** (必填): 债务人企业名称 或 信用代码
- **humanName** (可选): 法定代表人或实控人姓名（启用个人兜底分析）

## 执行流程

### Step 1: 财务硬数据（V2.0 核心）
- `get_financial_data` (`searchKey`) — 直接获取 3 年完整财报（资产负债率/速动比率/所有者权益）
- `get_company_scale` — 企业规模（参保 / 分支 / 投资）

### Step 2: 可执行资产清单
- `get_chattel_mortgage_info` — 动产抵押
- `get_land_mortgage_info` — 土地抵押
- `get_equity_pledge_info` — 股权出质
- `get_external_investments` — 对外投资（潜在可执行股权）
- `get_branches` — 分支机构（潜在标的）

### Step 3: 历史偿债模式
- `get_historical_dishonest` — 历史失信
- `get_historical_judgment_debtor` — 历史被执行
- `get_historical_judicial_docs` — 历史裁判文书

### Step 4: 资产被冻结状态
- `get_equity_freeze` — 股权冻结
- `get_judicial_auction` — 司法拍卖

### Step 5: 终本警示
- `get_terminated_cases` — 终本案件（关键警示信号）

### Step 6: 实控人个人兜底（V2.0，若提供 humanName）
- `get_personnel_dishonest`
- `get_personnel_judgment_debtor`
- `get_personnel_high_consumption_ban`
- `get_personnel_controlled_companies` — 实控人控制的其他企业（潜在追偿对象）

## 输出格式

```markdown
# 债务清偿能力评估 V2.0 — {name}

## 一、财务硬指标（基于 3 年完整财报）
- 资产负债率: {x}%
- 速动比率: {x}
- 流动比率: {x}
- 所有者权益: {x} 元
- 经营性现金流: {x}

财务健康度评级: A / B / C / D

## 二、可执行资产清单（按优先级）
### 高优先级（流动性强）
- 银行存款 / 应收账款（推断）
- 现有股权资产（含对外投资）

### 中优先级
- 不动产 / 厂房（含分支机构所在地）
- 大额应收

### 低优先级
- 知产资产（专利 / 商标）

## 三、资产受限情况
| 资产类型 | 已抵押 / 冻结 | 处置可行性 |
|---------|-------------|----------|
| 股权 | ... | ... |
| 动产 | ... | ... |
| 土地 | ... | ... |

## 四、历史偿债模式
- 历史失信: {n} 条
- 历史被执行总金额: ...
- 历史败诉案件: {n}
- 偿债倾向: 主动履行 / 被动执行 / 抗拒履行

## 五、终本警示（重要）
- 终本案件数: {n}
- 终本累计未履行金额: ...
- 警示等级: 🚨 严重 / ⚠️ 警示 / ✓ 安全

## 六、实控人个人兜底（V2.0，若提供 humanName）
- 个人偿付能力评估: ...
- 实控人控制的其他企业（备选追偿主体）: {n} 家

## 七、追偿成功率精算
- 主体追偿可行性: {percentage}%
- 实控人兜底可行性: {percentage}%
- 综合追偿成功率: {percentage}%

## 八、保全决策建议
- 财产保全建议: 推进 / 暂缓
- 优先保全标的（按优先级排序）: ...
- 保全金额建议: ...
- 替代措施: 调解 / 和解 / 申请破产
```

## 示例

输入: `searchKey = "ABC 债务企业"`, `humanName = "李四"`

## V2.0 升级说明

V2.0 首次引入 T1.1 的 `get_financial_data` 直接返回完整财报，告别旧版靠失信/被执行事件信号反推偿债的模糊判断。同步叠加 `executive` 分类的 4 个个人维度工具，实现"企业 + 实控人"双兜底分析。
