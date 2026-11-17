---
id: GOV-005
type: moc
title: Codex 启动与任务收尾规范
maturity: L2
review_status: approved
scope: global
sensitivity: internal
risk_level: medium
source_kind: derived
source_paths:
  - 'C:\Users\20257\.codex\AGENTS.md'
  - 'D:\666\AGENTS.md'
  - 'D:\666\AI_Global_Assistant\global_governance_manifest.md'
  - 'D:\666\AI_Global_Assistant\GLOBAL_AGENTS.md'
source_versions:
  - 'sha256:D07C5A9808107ED5E9ED8F0ADA4B8B9A4D3D597731F38A8FA719BF46B626981A'
  - 'sha256:C1FB9419E7E73976A15ECA5790A4F6673681533A28385FF02B8291EEA1C3E39F'
  - 'sha256:34AE665F2A768292558B2790512FA04C6E521CFDAA3A9B80A0D6225EDADFE001'
  - 'sha256:3F9D5CDB0408A189115B264C1D56399F959DAC263980A352340C6E1A27381007'
canonical_path: 'D:\666\AI_Global_Assistant\GLOBAL_AGENTS.md'
record_role: derived_view
moved_to:
derived_from:
  - 'D:\666\AI_Global_Assistant\governance_candidates\GOV-20260731-project-bootstrap-task-close.md'
supersedes: []
flags: []
created: 2026-07-31
updated: 2026-08-01
last_verified: 2026-08-01
review_due: 2026-10-31
approvals:
  - action: publish_derived_view
    reviewer: user
    reviewed_at: 2026-07-31
    decision: approved
    artifacts:
      - 'C:\Users\20257\.codex\AGENTS.md'
      - 'D:\666\AGENTS.md'
      - 'D:\666\AI_Global_Assistant\global_governance_manifest.md'
      - 'D:\666\AI_Global_Assistant\GLOBAL_AGENTS.md'
    reason: 用户明确批准按启动与任务收尾优化方案实施
gate_records: []
reuse_count: 0
---

# Codex 启动与任务收尾规范

> [!warning]
> 本页只是 Obsidian 中的只读导航视图。治理正文以 `D:\666\AI_Global_Assistant\GLOBAL_AGENTS.md` 和 `global_governance_manifest.md` 为准。

## 启动闭环

`发现本地 AGENTS → 读取 manifest → 8/8 SHA-256 一致性校验 → 读取始终必读规则 → 读取项目 AGENTS/索引/匹配 skill → 输出带项目根与校验时间的启动回执 → 开始项目写入或形成可留存结论`

- V500E 与 volant_XJ 各自保留本地启动指针；V500E `simmdl-master` 以及 volant_XJ 的 `sim_model`、`flight_data_analysis` 设有子仓库 bridge。
- 校验失败进入 `fail-closed`，仅允许恢复治理所需的只读诊断。
- 用户明确授权治理维护时可进入受限 `governance-maintenance`；收尾前必须恢复全部校验通过。
- 回执是本次一致性检查的审计证据，不是外部签名或独立防篡改证明。

## 任务收尾

| 字段 | 可选值 | 作用 |
|---|---|---|
| `closeout_outcome` | `completed / paused / blocked / cancelled` | 判断当前对话任务如何收尾 |
| `verification_status` | `passed / partial / waived / not-run / failed` | 单独说明验证充分程度 |

对话结束不等于任务完成。`completed` 要求当前明确范围内的必需交付、动作和验证已经完成或由用户明确豁免，未完成项和风险已披露，必要交接与 ΔK 路由已处理。可选增强和未来建议不阻止完成。

这些字段不替代项目任务卡的 `status`，也不替代知识条目的 `review_status`。所有收尾结果都只对已经实际形成的知识增量执行 `no-change / project-only / obs-candidate / global-config-candidate` 四路由。

## 权威入口

- `D:\666\AGENTS.md`
- `D:\666\AI_Global_Assistant\global_governance_manifest.md`
- `D:\666\AI_Global_Assistant\GLOBAL_AGENTS.md`
- `D:\666\V500E\AGENTS.md`
- `D:\666\volant_XJ\AGENTS.md`
