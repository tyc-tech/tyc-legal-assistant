# ⚖️ tyc-legal-assistant

> **法务 / 合规 AI SKILL 集** — 天眼查 OpenAPI + MCP 协议驱动的 8 个法务向 SKILL

[![License](https://img.shields.io/badge/License-Apache%202.0-green.svg)](LICENSE)
[![MCP](https://img.shields.io/badge/MCP-TYC%20天眼查-orange.svg)](https://agent.tianyancha.com)

---

## 1. 项目简介

`tyc-legal-assistant` 为企业法务、合规官、IP 部门、HR 法务、广告 / 食品合规官员提供 AI Agent SKILL 集，覆盖：

- 合同交易对手核验
- 广告合规审查
- 知识产权资产盘点 / 侵权排查
- 债权回收尽调
- 劳动用工合规
- 法律风险可视化
- 营业执照真伪核验

> 律师向的合同审查 / 法律尽调 / 法律研究 3 个 SKILL 放在姐妹仓 [`tyc-vibe-lawyering`](../tyc-vibe-lawyering/)。

---

## 2. 核心价值

```
+---------------------------------------------------------------+
|    企业法务 AI SKILL × MCP × 天眼查数据                       |
|                                                               |
|    不只是文本审核，更是「实时尽调」的确定性决策工具            |
|                                                               |
|    /tyc-contract-party   → 合同交易对手核验（秒级）            |
|    /tyc-ad-compliance    → 广告合规审查                        |
|    /tyc-ip-asset         → 知识产权资产盘点                    |
|    /tyc-ip-infringement  → 知识产权侵权排查                    |
|    /tyc-debt-recovery    → 债权回收尽调                        |
|    /tyc-labor-compliance → 劳动用工合规                        |
|    /tyc-legal-risk       → 法律风险可视化                      |
|    /tyc-license-validation → 营业执照真伪与状态核验            |
|                                                               |
+---------------------------------------------------------------+
```

---

## 3. 快速开始（2 种装法选一）

### 方式 A · bash 一键脚本（推荐 · 30 秒搞定）

```bash
bash <(curl -sL https://raw.githubusercontent.com/tyc-opensource/tyc-legal-assistant/main/install_tyc_mcp.sh)
```

脚本会：① 提示输入 `TYC_MCP_API_KEY`（[申请](https://agent.tianyancha.com)）→ ② 自动写入 `~/.claude/.mcp.json` → ③ 复制 SKILL 到 `~/.claude/skills/tyc-*` → ④ 复制命令到 `~/.claude/commands/`。完成后**重启 Claude Code** 即可使用。

### 方式 B · 本地 plugin-dir（开发 / 调试）

```bash
git clone https://github.com/tyc-opensource/tyc-legal-assistant.git
cd tyc-legal-assistant
export TYC_MCP_API_KEY="your_api_key_here"
claude --plugin-dir .
```

启动后项目级 `.mcp.json` 自动加载。

---

### 试用

```
/tyc-contract-party 某合同对方名称
/tyc-legal-risk 某目标企业
```

---

## 4. SKILL / Command 列表（8 个）

| # | 命令 | 名称 |
|---|------|------|
| 1 | `/tyc-contract-party` | 合同交易对手核验 |
| 2 | `/tyc-ad-compliance` | 广告合规审查 |
| 3 | `/tyc-ip-asset` | 知识产权资产盘点 |
| 4 | `/tyc-ip-infringement` | 知识产权侵权排查 |
| 5 | `/tyc-debt-recovery` | 债权回收尽调 |
| 6 | `/tyc-labor-compliance` | 劳动用工合规 |
| 7 | `/tyc-legal-risk` | 法律风险可视化 |
| 8 | `/tyc-license-validation` | 营业执照真伪与状态核验 |

### 目录结构（标准 Claude Code 插件结构）

```
tyc-legal-assistant/
├── .claude-plugin/plugin.json
├── .mcp.json
├── skills/
│   ├── ad-compliance/SKILL.md
│   ├── contract-party/SKILL.md
│   ├── debt-recovery/SKILL.md
│   ├── ip-asset/SKILL.md
│   ├── ip-infringement/SKILL.md
│   ├── labor-compliance/SKILL.md
│   ├── legal-risk/SKILL.md
│   └── license-validation/SKILL.md
├── commands/                       # /tyc-* 命令入口
│   └── tyc-<name>.md × 8
├── evals/
└── docs/
    └── MCP_CONFIGURATION.md
```

每个 SKILL 目录内可放 `references/` 与 `scripts/`（参考资料 + 辅助脚本）。

---

## 5. 典型场景

### 场景 A: 合同签订前 · 对手核验（30 秒）

```
/tyc-contract-party 某甲方公司
  ↓ 主体 / 经营 / 司法 / 财产 / 税务 六维扫描
Claude: 风险报告 + 签约建议（放心签 / 慎签 / 加对赌条款 / 拒签）
```

### 场景 B: 广告文案出稿前 · 合规预审

```
/tyc-ad-compliance {广告主企业} --advert "广告文案片段"
  ↓ 企业资质 + 广告法关键词扫描
Claude: 合规风险等级 + 具体整改建议
```

### 场景 C: 营业执照真伪核验

```
/tyc-license-validation {待核验企业名称或统一社会信用代码}
  ↓ 对比官方登记数据
Claude: 核验结果（真 / 假 / 部分一致）+ 异常字段列表
```

---

## 6. 天眼查 MCP 集成

所有 SKILL 通过单一 `tyc` MCP server 调用业务语义聚合层 163 个工具。

主要使用的工具分类：

| Go 包 | 工具数 | 本仓使用场景 |
|-------|--------|--------------|
| `company` | 49 | 主体核验（contract-party / license-validation）|
| `risk` | 35 | 法律风险扫描（legal-risk / debt-recovery）|
| `intellectual_property` | 14 | IP 资产盘点（ip-asset / ip-infringement）|
| `operation` | 32 | 行政许可 / 资质（ad-compliance / license-validation）|
| `executive` | 15 | 劳动 / HR 背调（labor-compliance）|

详见 [docs/MCP_CONFIGURATION.md](docs/MCP_CONFIGURATION.md)。

---

## 7-12. 配置 / FAQ / 贡献 / License / 致谢 / 联系

Apache-2.0 · [LICENSE](LICENSE) · contact@tianyancha.com
