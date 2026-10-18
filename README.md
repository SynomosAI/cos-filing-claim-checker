# cos-filing-claim-checker · 化妆品注册备案与功效宣称核验域

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain: cos](https://img.shields.io/badge/domain-cos-9cf.svg)](domain.manifest.json)
[![Profile: domain/1.0](https://img.shields.io/badge/profile-domain%2F1.0-blue.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED-orange.svg)](domain.manifest.json)
[![Gate levels](https://img.shields.io/badge/gate-5%20levels-lightgrey.svg)](gate-map.json)

> **Status: RESERVED (placeholder · open for adoption)** — This repo has scaffolded the
> industry-access four-file skeleton per the platform profile, and awaits a qualified
> operator in this domain to claim it and fill in real rules.

把「这个化妆品有没有备案、能不能这么宣称」拆成可核验的属性，让 AI 只做要件提示，不做宣称与放行结论。

> Turns "is this cosmetic registered/filed, and may it make this claim" into verifiable
> attributes: the AI only surfaces missing prerequisites, it never drafts a claim nor
> hands down a pass/fail verdict.

**Keywords / 关键词**：化妆品备案 · 功效宣称 · 注册状态 · 药监公示 · 属性核验 · 红线 · gate-map · privacy-by-default · cosmetic filing · efficacy claim · regulatory compliance · MCP checker · domain profile

---

## ✨ 这个域管什么 / What this domain covers

化妆品注册备案状态与功效宣称依据核验。

特殊化妆品注册、普通化妆品备案，两条路径要求不同；功效宣称必须有公布的依据摘要。市场上超范围宣称、借医疗用语暗示疗效是重灾区。AI 能做的是把状态与依据摆清楚，不得代拟宣称。

This domain governs **cosmetic registration/filing status and efficacy-claim evidence**.
Special cosmetics require a registration certificate while ordinary cosmetics require filing;
any efficacy claim must have a published evidence summary. Over-claiming and borrowing
medical wording to imply therapeutic effect are the recurring risks. The checker only lays
out status and evidence — it never drafts claims on your behalf.

---

## 📇 域标识 / Domain identity

| 项 / Item | 值 / Value |
|---|---|
| 域 ID / Domain ID | `cos`（全局唯一，一经分配不复用 / globally unique, never reused） |
| 域名称 / Domain name | 化妆品 · 注册备案与功效宣称 |
| Profile 版本 / Profile version | `domain/1.0` |
| 当前状态 / Status | `RESERVED` |
| 占位时间 / Reserved since | 2026-09-12 |
| Checker 端点 / Checker endpoint | `mcp://pending-claim/cos-checker`（待认领） |

---

## 🧾 属性清单 / Attribute catalogue

| 属性键 / Key | 类型 / Type | 说明 / Meaning |
|---|---|---|
| `cos.product_type` | enum | 特殊化妆品须注册，普通化妆品备案，牙膏参照普通化妆品管理 · special/general/toothpaste/unclassified |
| `cos.filing_status` | enum | 注册证或备案信息当前状态，以药监部门公示为准 · registered/filed/pending/expired/not_required |
| `cos.claim_evidence` | enum | 功效宣称依据摘要是否已公布并可查 · published/partial/none/not_applicable |
| `cos.ingredient_compliance` | enum | 原料是否超出准用限量或检出禁用组分 · compliant/restricted_exceeded/prohibited_detected/unknown |
| `cos.label_compliance` | enum | 标签是否含医疗用语或超范围宣称 · compliant/non_compliant/pending |

---

## 🚫 本域红线（不可逾越，机器可读）/ Red lines

1. 不得输出「可祛斑美白」「可治疗」等超出化妆品定义的功效或医疗用语表述
2. 功效宣称须有已公布的依据摘要，AI 不得代拟或补写宣称内容
3. 不得输出「已备案」「已注册」结论，以药监部门公示为唯一依据

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。
> Every red line has a matching blocking rule in `gate-map.json`; the platform validator
> fails the build if any red line is uncovered — policy and system must never drift.

---

## 📦 行业接入四件套 / The four-file access kit

| 文件 / File | 作用 / Role |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 / domain declaration |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 / privacy-by-default |
| `checker.io.json` | 本域核验器（MCP 工具，**只出示核验，不下判定**）/ checker I/O contract |

---

## 🚀 快速开始 / Quick Start

> 前置：本仓是**声明式配置仓**，无可执行二进制；你只需要 git 与 Python 3（用于 JSON 自检）。
> This is a declarative config repo — no binary to run. You only need git and Python 3.

```bash
# 1. 克隆本仓
git clone https://github.com/SynomosAI/cos-filing-claim-checker.git
cd cos-filing-claim-checker

# 2. 自检四件套 JSON 是否合法（Python 标准库即可）
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json > /dev/null
python -m json.tool privacy.json > /dev/null
python -m json.tool checker.io.json > /dev/null
```

跑通后，用自然语言提问即可，例如「**这款特殊化妆品的注册证状态该怎么核验？**」
「**这个美白宣称有没有公布的依据摘要？**」。

---

## 📖 示例 / Examples

### 示例 1：列出本域声明的全部属性键

```bash
# 从 manifest 里抽出属性键与取值
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));[print(a['key'],'->','/'.join(a['values'])) for a in m['attributes']]"
```

### 示例 2：checker 工具的一次调用契约（见 checker.io.json）

```json
{
  "subject_ref": "mpp:cn:9f2c1a7b",
  "attribute_keys": ["cos.product_type", "cos.filing_status"],
  "evidence": ["tsa:2026-09-12#sha256:3b1f..."],
  "request_id": "req-20260912-0001"
}
```

checker 只回 `shown / missing / expired / unverifiable` 四种状态与出处链，
**永远不回**「合格 / 不合格 / 已合规 / 建议批准」。

---

## 🗂 仓库结构 / Repository layout

```text
cos-filing-claim-checker/
├── README.md              # 本文件
├── README.en.md           # 英文版说明
├── domain.manifest.json   # 域声明（属性 / 签发方 / 红线）
├── gate-map.json          # 动作摩擦分级
├── privacy.json           # 隐私声明（默认关闭 / 最小必要）
├── checker.io.json        # checker MCP 工具 I/O 契约
├── CODEOWNERS             # 代码所有者 @SynomosAI/synomosai-gov
├── LICENSE                # MIT License（© 2026 SynomosAI）
├── SECURITY.md            # 安全策略
├── AGENTS.md              # 给 AI Agent 的工作指引
├── llms.txt               # 给 AI 的最小索引
└── llms-full.txt          # 给 AI 的完整文本
```

---

## 🧭 核心原则 / Core principles

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**Privacy is a gating condition, not a remediation item.** Missing `privacy.json` or any
required field mismatch fails conformance outright — it is a rejection, not a warning.

---

## 📚 参考依据 / References

- 化妆品监督管理条例
- 化妆品注册备案管理办法
- 化妆品功效宣称评价规范
- 化妆品标签管理办法
- 化妆品生产经营监督管理办法

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。
> The references above state the source and scope of each attribute; they are not legal advice.

---

## 📮 认领方式 / How to adopt

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

This domain is open to qualified operators. Fork the repo, fill in `operator` and
`checker_endpoint`, align attribute values and red lines with current rules, run the
platform self-checker (five criteria must all pass), then open a PR with credentials.

---

## 🔒 安全 / Security

如发现安全问题（例如配置泄露敏感信息、checker 契约被用于越权下结论），请按
[`SECURITY.md`](SECURITY.md) 的方式私下报告，不要在公开 issue 中披露。

## 🤝 贡献 / Contributing

欢迎提 Issue / PR。本仓 main 分支受组织 ruleset 保护，请从个人分支发起 Pull Request，
等待 lgd-ci-gate 通过后合并。改动属性 / 红线时须同步 `gate-map.json` 并重新跑校验。

---

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.

完整条款见根目录 [LICENSE](LICENSE)（MIT License）。
