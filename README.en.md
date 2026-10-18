# cos-filing-claim-checker · Cosmetic filing & efficacy-claim verification domain

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain: cos](https://img.shields.io/badge/domain-cos-9cf.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED-orange.svg)](domain.manifest.json)

> **Status: RESERVED (placeholder · open for adoption)** — This repo scaffolds the
> industry-access four-file skeleton per the platform profile and awaits a qualified
> operator to claim it and fill in real rules.

Turns "is this cosmetic registered/filed, and may it make this claim" into verifiable
attributes: the AI only surfaces missing prerequisites, it never drafts a claim nor hands
down a pass/fail verdict.

**Keywords**: cosmetic filing · efficacy claim · registration status · regulator notice ·
attribute verification · red lines · gate-map · privacy-by-default · regulatory compliance ·
MCP checker · domain profile

## What this domain covers

Cosmetic registration/filing status and efficacy-claim evidence. Special cosmetics require a
registration certificate; ordinary cosmetics require filing. Any efficacy claim must have a
published evidence summary. Over-claiming and borrowing medical wording to imply therapeutic
effect are the recurring risks. The checker only lays out status and evidence.

## Domain identity

| Item | Value |
|---|---|
| Domain ID | `cos` (globally unique, never reused) |
| Domain name | Cosmetics · registration/filing & efficacy claim |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Reserved since | 2026-09-12 |

## Attribute catalogue

| Key | Type | Meaning |
|---|---|---|
| `cos.product_type` | enum | special/general/toothpaste/unclassified |
| `cos.filing_status` | enum | registered/filed/pending/expired/not_required |
| `cos.claim_evidence` | enum | published/partial/none/not_applicable |
| `cos.ingredient_compliance` | enum | compliant/restricted_exceeded/prohibited_detected/unknown |
| `cos.label_compliance` | enum | compliant/non_compliant/pending |

## Red lines (machine-readable)

1. Never output efficacy or medical wording beyond the cosmetic definition (e.g. "can treat", "can whiten spots").
2. An efficacy claim needs a published evidence summary; the AI must never draft or complete claim copy.
3. Never output "already filed" / "already registered"; the regulator notice is the only source of truth.

## The four-file access kit

| File | Role |
|---|---|
| `domain.manifest.json` | Domain declaration: attributes, issuer requirements, TTL, red lines |
| `gate-map.json` | Friction per action: silent / warn / confirm / block / require-owner |
| `privacy.json` | Privacy statement: off by default, minimization, withdrawable, erasable |
| `checker.io.json` | Checker MCP tool I/O contract — shows & verifies only, never judges |

## Quick Start

```bash
git clone https://github.com/SynomosAI/cos-filing-claim-checker.git
cd cos-filing-claim-checker

# Validate the four JSON files with the Python stdlib
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json > /dev/null
python -m json.tool privacy.json > /dev/null
python -m json.tool checker.io.json > /dev/null
```

## Examples

```bash
# List declared attribute keys and their allowed values
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));[print(a['key'],'->','/'.join(a['values'])) for a in m['attributes']]"
```

```json
{
  "subject_ref": "mpp:cn:9f2c1a7b",
  "attribute_keys": ["cos.product_type", "cos.filing_status"],
  "evidence": ["tsa:2026-09-12#sha256:3b1f..."],
  "request_id": "req-20260912-0001"
}
```

The checker answers only with `shown / missing / expired / unverifiable` plus the
provenance chain. It never returns "pass / fail", "compliant", or "approve".

## Core principles

The platform is an arena, not a shelf. The checker answers only "is this claim verifiable,
and what prerequisite is missing" — never "is this compliant, should it be done". The
verdict belongs to the qualified operator, the regulator, and the person.

Privacy is a gating condition, not a remediation item.

## How to adopt

This domain is open to qualified operators: fork the repo, fill in `operator` and
`checker_endpoint`, align attributes and red lines with current rules, run the platform
self-checker (all five criteria must pass), then open a PR with credentials.

## License

Code and configuration are MIT-licensed. Documentation copyright remains with SynomosAI.
See [LICENSE](LICENSE).

© 2026 SynomosAI. All rights reserved.
