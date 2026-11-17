---
id: MOC-INBOX
type: moc
title: 收件箱使用规则
maturity: L2
review_status: approved
scope: personal
sensitivity: internal
risk_level: low
source_kind: derived
source_paths:
  - D:\666\obs\知识库迭代机制方案.md
source_versions:
  - v2.0
canonical_path: D:\666\obs\10_收件箱\README.md
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
      - D:\666\obs\知识库迭代机制方案.md
    reason: 用户批准按方案完成知识库初步搭建
gate_records: []
reuse_count: 0
---

# 收件箱使用规则

- 只捕获可能具有复用价值的 L0 线索，不保存完整聊天。
- 进入 L1 前必须补齐来源、范围、敏感级别、证据缺口并去重。
- 临时上下文、缓存、原始日志、项目参数和接口不得放入本目录。
- 每周清理；合法结果可以是 `no-change` 或删除候选提案，但实际删除文件需再次获得明确授权。

项目任务结束先执行四路由：`no-change` 不入库；`project-only` 留在项目；`obs-candidate` 才进入本 Vault 的 L0/L1 候选；`global-config-candidate` 交给 AI_Global_Assistant。用户不需要手工复制项目笔记。
