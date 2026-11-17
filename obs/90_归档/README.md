---
id: MOC-ARCHIVE
type: moc
title: 归档说明
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
canonical_path: 'D:\666\obs\90_归档\README.md'
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

# 归档说明

归档保留历史原因、失效证据、替代路径和迁移指针。`deprecated`、`superseded` 或被推翻内容退出默认检索，但不因“保持纯洁”而静默删除。

## 处理顺序

1. 先设置状态和健康标记，让条目逻辑退出默认检索。
2. 检查来源、`derived_from`、反向链接和下游依赖。
3. 建立并验证替代关系。
4. 获得移动批准后才考虑移入本目录；不为目录整齐频繁移动。

## 物理删除闸门

仅缓存、空候选、无独立价值的完全重复副本、可再生成视图或误摄入敏感内容可以提出删除。删除前必须有已验证的恢复机制、无未处理依赖、替代页已生效、满足观察期，并向用户列出准确路径、理由和恢复方式后取得明确批准。删除后必须检查断链、索引和查询回归。

Vault 的 Git 或快照恢复机制尚未选定并验证前，不执行批量物理清理。
