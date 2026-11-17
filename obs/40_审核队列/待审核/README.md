---
id: MOC-REVIEW
type: moc
title: 待审核队列说明
maturity: L2
review_status: approved
scope: personal
sensitivity: internal
risk_level: low
source_kind: derived
source_paths:
  - 'D:\666\obs\知识库迭代机制方案.md'
source_versions:
  - 'v2.0'
canonical_path: 'D:\666\obs\40_审核队列\待审核\README.md'
record_role: canonical
moved_to:
derived_from: []
supersedes: []
flags: []
created: 2026-07-31
updated: 2026-07-31
last_verified: 2026-07-31
review_due: 2026-10-31
approvals:
  - action: initial_build
    reviewer: user
    reviewed_at: 2026-07-31
    decision: approved
    artifacts:
      - 'D:\666\obs\知识库迭代机制方案.md'
    reason: 用户批准按方案完成知识库初步搭建
gate_records: []
reuse_count: 0
---

# 待审核队列说明

本目录不复制候选正文。[[../../00_控制台/审核仪表盘/待审核.base|待审核 Base]] 按 YAML 状态自动汇总候选；若需要单独审核票据，只保存 diff、批准记录和 canonical 路径。

`stale`、`conflicted`、复核到期或来源发生变化的既有知识也进入隔离复核。隔离是先退出默认检索，不代表自动废弃；恢复、修订、替代或废弃均需留下裁决依据。
