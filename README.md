# cos-filing-claim-checker

> **状态：RESERVED（占位 · 开放认领）** — 本仓已按平台协议规范建好行业接入四件套骨架，
> 等待具备本域资质的运营方认领并填充真实规则。

把「这个化妆品有没有备案、能不能这么宣称」拆成可核验的属性，让 AI 只做要件提示，不做宣称与放行结论。

## 这个域管什么

化妆品注册备案状态与功效宣称依据核验

特殊化妆品注册、普通化妆品备案，两条路径要求不同；功效宣称必须有公布的依据摘要。市场上超范围宣称、借医疗用语暗示疗效是重灾区。AI 能做的是把状态与依据摆清楚，不得代拟宣称。

## 域标识

| 项 | 值 |
|---|---|
| 域 ID | `cos`（全局唯一，一经分配不复用） |
| 域名称 | 化妆品 · 注册备案与功效宣称 |
| Profile 版本 | `domain/1.0` |
| 当前状态 | `RESERVED` |
| 占位时间 | 2026-09-12 |

## 属性清单

| 属性键 | 类型 | 说明 |
|---|---|---|
| `cos.product_type` | enum | 特殊化妆品须注册，普通化妆品备案，牙膏参照普通化妆品管理 · 取值 special/general/toothpaste/unclassified |
| `cos.filing_status` | enum | 注册证或备案信息当前状态，以药监部门公示为准 · 取值 registered/filed/pending/expired/not_required |
| `cos.claim_evidence` | enum | 功效宣称依据摘要是否已公布并可查 · 取值 published/partial/none/not_applicable |
| `cos.ingredient_compliance` | enum | 原料是否超出准用限量或检出禁用组分 · 取值 compliant/restricted_exceeded/prohibited_detected/unknown |
| `cos.label_compliance` | enum | 标签是否含医疗用语或超范围宣称 · 取值 compliant/non_compliant/pending |

## 本域红线（不可逾越，机器可读）

1. 不得输出「可祛斑美白」「可治疗」等超出化妆品定义的功效或医疗用语表述
2. 功效宣称须有已公布的依据摘要，AI 不得代拟或补写宣称内容
3. 不得输出「已备案」「已注册」结论，以药监部门公示为唯一依据

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。

## 行业接入四件套

| 文件 | 作用 |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 |
| `checker` | 本域核验器（MCP 工具，**只出示核验，不下判定**） |

## 核心原则

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**隐私是准入条件，不是整改事项。** 缺失 `privacy.json` 或任一必填字段不符，
符合性校验直接失败——不是警告，是拒绝接入。

## 参考依据

- 化妆品监督管理条例
- 化妆品注册备案管理办法
- 化妆品功效宣称评价规范
- 化妆品标签管理办法
- 化妆品生产经营监督管理办法

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。

## 认领方式

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.
