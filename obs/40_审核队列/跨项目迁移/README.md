---
id: MOC-TRANSFER
type: moc
title: 跨项目迁移队列说明
maturity: L2
review_status: approved
scope: cross-project
sensitivity: internal
risk_level: high
cross_project_policy: conditional
source_kind: derived
source_paths:
  - 'D:\666\obs\知识库迭代机制方案.md'
source_versions:
  - 'v2.0'
canonical_path: 'D:\666\obs\40_审核队列\跨项目迁移\README.md'
record_role: canonical
moved_to:
derived_from: []
supersedes: []
flags: []
created: 2026-07-31
updated: 2026-09-15
last_verified: 2026-07-31
review_due: 2026-10-31
approvals:
  - action: initial_build
    reviewer: user
    reviewed_at: 2026-07-31
    decision: approved
    artifacts:
      - 'D:\666\obs\知识库迭代机制方案.md'
    reason: 用户批准建立 V500E 与 volant_XJ 受控知识流入口
gate_records: []
reuse_count: 0
---

# 跨项目迁移队列说明

- 本目录保存 G1–G5 迁移审核票据；票据记录来源路径、抽象与脱敏、批准、验证产物、失败边界和关闭原因。
- 脱敏后的方法候选正文位于 `D:\666\obs\30_知识维基`，从 L1 到 L2/L3 原位升格；票据与方法正文使用不同 ID。
- V500E 与 volant_XJ 不直接复制项目 KB 页面。
- 迁移必须依次通过 G0–G5；G0/G4 写入前分别取得项目授权并执行对应 kb-sync。
- 目标项目验证失败时，保留票据并将结果回写为候选方法的不适用边界，不静默丢弃。
- G5 后关闭票据并保留审计记录；通用方法正文不迁入 `AI_Global_Assistant`。
- 当前没有已放行迁移项。2026-09-15已建立[[40_审核队列/跨项目迁移/配平建模与零速诊断经验迁移审核|配平经验审核票据]]及L1草案；人工审核与目标项目验证仍待完成。
