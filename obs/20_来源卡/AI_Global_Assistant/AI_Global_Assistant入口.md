---
id: SRC-AI-GLOBAL
type: source
title: AI_Global_Assistant 入口
maturity: L2
review_status: approved
scope: global
sensitivity: internal
risk_level: low
cross_project_policy: allowed
source_kind: primary
source_paths:
  - 'C:\Users\20257\.codex\AGENTS.md'
  - 'D:\666\AGENTS.md'
  - 'D:\666\AI_Global_Assistant\global_governance_manifest.md'
  - 'D:\666\AI_Global_Assistant\GLOBAL_AGENTS.md'
  - 'D:\666\AI_Global_Assistant\global_knowledge_rules.md'
  - 'D:\666\AI_Global_Assistant\global_prompt_templates.md'
  - 'D:\666\AI_Global_Assistant\global_skill_index.md'
  - 'D:\666\AI_Global_Assistant\cross_project_learning_rules.md'
  - 'D:\666\AI_Global_Assistant\workflow_feedback_capture_rules.md'
  - 'D:\666\AI_Global_Assistant\source_evidence_rules.md'
  - 'D:\666\AI_Global_Assistant\knowledge_operations_rules.md'
  - 'D:\666\AI_Global_Assistant\README.md'
  - 'D:\666\AI_Global_Assistant\skills\generate-obsidian-review-pack\SKILL.md'
  - 'D:\666\AI_Global_Assistant\skills\generate-obsidian-review-pack\references\review-checklists.md'
  - 'D:\666\AI_Global_Assistant\skills\generate-obsidian-review-pack\agents\openai.yaml'
source_versions:
  - 'sha256:D07C5A9808107ED5E9ED8F0ADA4B8B9A4D3D597731F38A8FA719BF46B626981A'
  - 'sha256:C1FB9419E7E73976A15ECA5790A4F6673681533A28385FF02B8291EEA1C3E39F'
  - 'sha256:34AE665F2A768292558B2790512FA04C6E521CFDAA3A9B80A0D6225EDADFE001'
  - 'sha256:3F9D5CDB0408A189115B264C1D56399F959DAC263980A352340C6E1A27381007'
  - 'sha256:3C66BF1D0DC53FC0159E9F6515AD2CD90F3BF3D5EB049D4029FBB2033CBE8437'
  - 'sha256:E77EBC8B2A64A7D473679ADDC3941BE7673CD57A55F431287EDF82EEE773ED6D'
  - 'sha256:69584F83686A89B4A17E0E456E3A9160AB6F164E7C54802F183FCB6E07C06BA9'
  - 'sha256:223C9116CB43B20A8A5D3BE0E2D364692B47D634F743903CAED95563033A79CC'
  - 'sha256:840A4645ABCBBBF2F37CF3B34F94350804860C19DC9B71138D2507FBC907929C'
  - 'sha256:746171E43BFA2B41659F40301CD7A40BC1048A9B64750136A0F151250BE616D6'
  - 'sha256:AD59A41449BAEF7C66E287526D540B9E9B8D067FF67ACDA1A3066F1FC23D483E'
  - 'sha256:C59FDA27C2E29BCC81D3C4E9B229B4F524B7EF621F524FEDFA12237F0C867E4B'
  - 'sha256:C7A6E93088C9AED9A9169DF0CF88BB34634A3CC57B983BC532B059A08AC8E4C0'
  - 'sha256:B1AC7F16916CADCDB7255003D76D03CBCBE87993A132B5DF7DD92F5B99412C89'
  - 'sha256:1643688D6B5E56E12200C4816075CDA6FB49D1EBFB115AA190AD8F97D5C1794F'
canonical_path: 'D:\666\obs\20_来源卡\AI_Global_Assistant\AI_Global_Assistant入口.md'
record_role: canonical
moved_to:
derived_from: []
supersedes: []
flags: []
created: 2026-07-31
updated: 2026-08-01
last_verified: 2026-08-01
review_due: 2026-10-31
approvals:
  - action: initial_ingest
    reviewer: user
    reviewed_at: 2026-07-31
    decision: approved
    artifacts: []
    reason: 用户指定为首批知识来源
  - action: source_refresh
    reviewer: user
    reviewed_at: 2026-08-01
    decision: approved
    artifacts:
      - 'D:\666\AI_Global_Assistant\skills\generate-obsidian-review-pack\SKILL.md'
      - 'D:\666\AI_Global_Assistant\global_prompt_templates.md'
      - 'D:\666\AI_Global_Assistant\global_skill_index.md'
    reason: 用户要求把只读审核包固化为短提示模板和 Skill
gate_records: []
reuse_count: 0
---

# AI_Global_Assistant 入口

## 来源定位

`D:\666\AI_Global_Assistant` 是当前工作区 Codex 的全局治理与配置目录，回答“Codex 应怎样工作”。本页只负责导航，不复制规则正文；领域知识、跨项目方法和通用 L0–L3 正文不归该目录所有。

## 关键入口

- `GLOBAL_AGENTS.md`：全局职责、权限和安全边界。
- `global_knowledge_rules.md`：成熟度、状态、范围、敏感级别、默认检索和 canonical 规则。
- `source_evidence_rules.md`：来源、证据、引用和失效传播。
- `cross_project_learning_rules.md`：脱敏、G0–G5 和 canonical 交接。
- `knowledge_operations_rules.md`：Ingest、Query、Lint、Promote。
- `workflow_feedback_capture_rules.md`：对话反馈候选和防过拟合。
- `global_prompt_templates.md`：可复用短提示和任务入口。
- `global_skill_index.md`：稳定 Skill 的路径、用途和触发边界。
- `skills/generate-obsidian-review-pack/SKILL.md`：Obsidian 只读审核包流程。

## 日常审核调用

```text
生成只读审核包：<稳定 ID、路径、wikilink 或 Base 范围>
```

该调用默认只在当前 Codex 对话生成审核包；人工给出裁决和写入授权前，不修改知识库文件。

## 与 Obsidian 的边界

- AI_Global_Assistant：工作规范、权限、安全边界、Prompt、Skill、知识治理规则和 Obsidian 检索入口。
- Obsidian：通用 L0–L3 工程知识正文、来源证据、审核、迁移票据和知识健康运营。
- 从工程知识中提炼出的 Codex 操作规范可以形成新的配置候选进入 AI_Global_Assistant，但工程知识正文仍留在 Obsidian。
- “让 Codex 找得到”通过索引和路由解决，不通过复制知识正文解决。

## 排除内容

不得写入领域知识正文、项目参数、接口、模型结构、原始日志、数据、客户/供应商资料或完整聊天。
