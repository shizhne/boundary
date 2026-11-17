---
id: LOG-000
type: retrospective
title: 知识库变更日志
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
canonical_path: 'D:\666\obs\log.md'
record_role: canonical
moved_to:
derived_from: []
supersedes: []
flags: []
created: 2026-07-31
updated: 2026-09-15
last_verified: 2026-08-20
review_due:
approvals:
  - action: initial_build
    reviewer: user
    reviewed_at: 2026-07-31
    decision: approved
    artifacts: []
    reason: 用户明确批准按方案实施
gate_records: []
reuse_count: 0
---

# 知识库变更日志

> [!info]
> 本页只追加知识变更元数据与摘要，不记录完整聊天、原始日志或敏感项目事实。

## 2026-08-20

- `decision | ADR-LITERATURE-ANALYSIS-001 / D-017 | 当前飞行动力学模型以转周平均或缓慢变化气动力为主体；高频旋翼阶次载荷默认不显式建立 | personal/pending_review | update`
- `workflow | PREF-LITERATURE-ANALYSIS-001 / E-014 / literature-analysis-guided | 按用户实际模型带宽区分来源现象与模型保留项；高频项降为已知省略效应和特殊验证扩展 | L1/candidate | update`
- `query-writeback | EVID-TILT-WU-2019 / MTH-AERO-002 | 周期波动来源事实保持不变；工程模型层级改为平均载荷主体、慢变历史修正按需、高频项条件启用 | L1/candidate | update`
- `derived-analysis | EVD-WU2019-ROTOR-WING-ANALYSIS-001 | Wu 2019 定向分析 | 默认模型不再强制加入 3/rev 或 RMS/有界扰动替代；补充结构、执行机构、传感器和高带宽控制例外 | pending_review | update`
- `skill-test | literature-analysis-guided | 官方 quick_validate + 独立模型带宽前向测试 | 正确输出平均项主体、慢变残差条件加入、3/rev 默认省略及重新评估条件 | candidate | passed`
- `lint | WU2019-EX04-20260820 | 8 个 Markdown、YAML/log、4 个唯一 ID、Wikilink、外部资源、来源哈希、evidence→method 哈希链及发布/暂存同一性 | 阻断项 0 | passed`
- `decision | ADR-LITERATURE-ANALYSIS-001 | 方法对比论文先提取共同物理现象；专业术语须用完整中文解释对象、工况、数字口径和边界 | personal/pending_review | update`
- `workflow | PREF-LITERATURE-ANALYSIS-001 / literature-analysis-guided | 新增共同现象分层与中文清晰表达闸门；稳定 flight-literature-reader 未修改 | L1/candidate | update`
- `query-writeback | EVID-TILT-WU-2019 | 图 6–7、13–18 与表 7–8 | 补入悬停作用方向、两方法共同现象、非单调趋势和常识审计 | L1/candidate | update`
- `query-writeback | MTH-AERO-002 | EVID-TILT-WU-2019 | 模型主次调整为平均载荷 + 桨叶周期扰动 + 可选历史修正 | L1/candidate | update`
- `derived-analysis | EVD-WU2019-ROTOR-WING-ANALYSIS-001 | Wu 2019 定向分析 | 共同物理现象移入主线，方法差异和历史项降为边界/可选扩展 | pending_review | update`
- `skill-test | literature-analysis-guided | 官方 quick_validate + 三轮独立清晰表达测试 | 首轮暴露长段落和代填基准问题；补入逐条改写与缺项不得代填后通过 | candidate | iterate`
- `lint | WU2019-EX03-20260820 | 8 个 Markdown、YAML/log、4 个唯一 ID、变更页 Wikilink、外部资源、来源/译文/备份哈希、evidence→method 哈希链及发布/暂存同一性 | 阻断项 0 | passed`

## 2026-08-19

- `ingest | EVID-TILT-WU-2019 | Wu 2019 PDF + 可读英文 Markdown | 连续倾转历史、周期载荷与筛选量级单源证据；过渡段无试验验证 | L1/candidate | create`
- `query-writeback | MTH-AERO-002 | EVID-TILT-WU-2019 | 准定常均值 + 低频历史修正 + 可选阶次项；旋翼乘性、机翼加性筛选场景 | L1/candidate | create`
- `decision | ADR-LITERATURE-ANALYSIS-001 | 默认完整中文阅读版 + 定向分析；低阶模型留存但暂不实施；辅助审计与控制分析边界 | personal/pending_review | update`
- `link-update | MOC-TILT-LIT / MOC-000 | Wu 2019 单源证据与 MTH-AERO-002 导航链接 | link-only | update`
- `lint | WU2019-INGEST-20260819 | 10 个受检 Markdown、2 个唯一新 ID、变更页 Wikilink、4 个外部相对资源、4 组来源/证据哈希、发布/暂存同一性、表 5–6 数值复算及候选 skill 官方 quick_validate | 阻断项 0 | passed`

## 2026-08-07

- `object-registry | V500E-KB-CONTENT-OBJECTS | 24/24 源页逐篇完成分类、摘要、关键词、锚点、关系和健康状态；9 个潜在抽象对象仅 G0 pending | L1/pending_review/project-only | create`
- `object-registry | XJ-KB-CONTENT-OBJECTS | 11/11 源页逐篇完成分类、摘要、关键词、锚点、关系和健康状态；4 个潜在抽象对象仅 G0 pending | L1/pending_review/project-only | create`
- `view-update | MOC-V500E-KB-CATALOG / MOC-XJ-KB-CATALOG | 35 个项目知识页增加稳定检索 ID、类型、脱敏摘要、关键词、稳定关系目标和来源指纹 | L1/candidate/derived_view | update`
- `route-review | V500E / volant_XJ | 13 个潜在抽象方向均为 G0 pending；35/35 源页实际 route=project-only | 0 event / 0 ticket | no-change`
- `lint | PROJECT-KB-CONTENT-OBJECTS-20260807 | 35/35 源页、35 个唯一 ID、37 组来源路径/版本对、关系/哈希/链接/YAML/UTF-8/检索回归；重点语义复核 V500E 8/24、volant_XJ 5/11 | 阻断项 0 | passed`
- `ingest | MOC-V500E-KB-CATALOG | D:\666\V500E\codex\kb | 24 个 Markdown 的路径与完整 SHA-256 指针 | L1/candidate/derived_view | create`
- `ingest | MOC-XJ-KB-CATALOG | D:\666\volant_XJ\docs\codex_kb | 11 个 Markdown 的路径与完整 SHA-256 指针 | L1/candidate/derived_view | create`
- `source-drift | SRC-XJ | 3 个来源哈希漂移、1 个旧 bridge 缺失；登记 6 个当前有效锚点 | pending_review + stale | update`
- `quarantine | EVID-XJ-FLT-20260803-86-LEGACY-SNAPSHOT | Vault 根目录遗留派生副本 | pending_review + stale | metadata-only`
- `route-review | V500E / volant_XJ | 13 个抽象方法主题具备 obs-candidate 潜力，但缺少稳定来源 ID 与 G0 | 0 event / 0 ticket | no-change`
- `lint | PROJECT-KB-INGEST-20260807 | 35 个源页、9 个 Vault 路径、35 组路径/哈希/打开链接、9 份 frontmatter、ID 与 Wikilink；人工抽查 V500E 5/24、volant_XJ 4/11 | 阻断项 0 | passed`
- `ingest | SRC-NASA-STD-7009B | NASA-STD-7009B Rev B；sha256:7B95...D8B4 | 公开通用来源卡 | candidate | create`
- `ingest | SRC-NASA-HDBK-7009B | NASA-HDBK-7009B Rev B；sha256:DDA7...AF31 | 公开通用来源卡；保留版本元数据异常 | candidate | create`
- `ingest | EVID-NASA-STD-7009B-CFD-AERODATA | 单一 primary 来源 | CFD 气动数据源规范性验收骨架 | candidate | create`
- `ingest | EVID-NASA-HDBK-7009B-CFD-AERODATA | 单一 primary 来源 | CFD 气动数据源实施证据 | candidate | create`
- `ingest | MTH-CFD-AERODATA-001 | 两张单源证据 | 双轨、六门、灰箱交付方法候选 | L1/candidate | create`
- `link-update | MTH-TOOL-001 / MTH-AERO-001 | CFD 数据源验收相关链接 | link-only | update`
- `lint | NASA-7009B-INGEST | 47 个 Markdown、45 个唯一 ID、5 个新页面、10 个变更页面及其 Wikilink/来源哈希 | 阻断项 0 | passed`

## 2026-08-01

- `governance | D666-CODEX-GOVERNANCE-v2.2.0 | Obsidian 只读审核包短提示、Skill 索引与审计记录 | approved | implement`
- `skill | generate-obsidian-review-pack | 稳定 ID/路径/wikilink/Base 范围 → 证据链、问题分级、建议裁决；默认零写入 | approved | create`
- `skill-test | generate-obsidian-review-pack | 无目标候选清单 + Q-INITIAL-BUILD-001 对象级前向测试；修复 question 递归下钻歧义 | passed | iterate`
- `provenance | AI-GLOBAL-SOURCE-v2.2 | manifest、prompt、skill index、README 与只读审核 Skill 来源锚点刷新 | passed | update`

## 2026-07-31

- `design | KB-SCHEMA-v1.1 | D:\666\obs\知识库迭代机制方案.md | 方案完成复核 | approved | canonical 规则待同步`
- `design | KB-SCHEMA-v1.2 | D:\666\obs\知识库迭代机制方案.md | record_role 与跨项目策略补齐 | approved | implemented`
- `governance | AI-GLOBAL-CANONICAL | D:\666\AI_Global_Assistant | 来源证据、知识操作、状态与迁移规则同步 | approved | update`
- `ingest | SRC-AI-GLOBAL | D:\666\AI_Global_Assistant | 来源入口与治理视图 | candidate | create`
- `ingest | SRC-TILT-PACK | 倾转旋翼综合设计资料包 | 来源入口、方法候选与证据种子 | candidate | create`
- `ingest | METHOD-SEED-4 | 倾转旋翼资料包四份方法 Markdown | 四份 L1 方法候选 | candidate | create`
- `ingest | EVID-TILT-CHEN-2024 | Chen 2024 PDF | 统一 MPC 单源证据 | candidate | create`
- `ingest | EVID-TILT-BELAK-2025 | Belak 2025 PDF | 快速局部控制分配单源证据 | candidate | create`
- `ingest | EVID-TILT-NASA-CR166536 | NASA CR-166536 | 低阶干扰模型单源证据 | candidate | create`
- `ingest | EVID-TILT-NASA-CR166537 | NASA CR-166537 | 仿真验证闭环单源证据 | candidate | create`
- `ingest | SRC-V500E | D:\666\V500E\codex\kb\00_index.md | 只建立入口卡 | candidate | create`
- `ingest | SRC-XJ | D:\666\volant_XJ\docs\codex_kb\00_index.md | 只建立入口卡 | candidate | create`
- `scope | EXCLUDED-FCKB | D:\666\FlightControlKnowledgeableBase | 保持未接入 | approved | no-change`
- `promote | SRC-AI-GLOBAL | L1→L2 | 用户批准初步搭建与全局 canonical 同步 | approved | append`
- `promote | SRC-V500E | L1→L2 | 用户批准建立 V500E 只读入口 | approved | append`
- `promote | SRC-XJ | L1→L2 | 用户批准建立 volant_XJ 只读入口 | approved | append`
- `lint | INITIAL-BUILD-AUDIT | 54 个变更文件 | 修复模板过滤、批准记录、版本、状态和入链 | passed | append`
- `design | KB-SCHEMA-v2.0 | D:\666\obs\知识库迭代机制方案.md | 治理、通用知识与项目事实三层边界；通用 L0-L3 正文归 Obsidian | approved | update`
- `governance | PROJECT-TO-OBS | V500E / volant_XJ | 半自动流入、任务末四路由、项目增量事件与 G0-G5 审核 | approved | implement`
- `governance | KNOWLEDGE-RETIREMENT | D:\666\obs | 失效隔离、冲突裁决、替代、归档和删除闸门 | approved | implement`
- `design | DEC-KB-BOUNDARY-20260731 | D:\666\obs\50_决策与问题\工作区治理与知识职责边界决策.md | 记录 v2.0 职责边界 | approved | create`
- `view | KNOWLEDGE-HEALTH-BASES | D:\666\obs\00_控制台\审核仪表盘 | 当前有效、陈旧、冲突、已替代、已废弃 | approved | create`
- `governance | KB-GOVERNANCE-v2.0 | AI_Global_Assistant / Obsidian / V500E / volant_XJ | 治理、通用知识、项目事实三层职责及项目内 Retire | approved | implement`
- `schema | KNOWLEDGE-INCREMENT-EVENT | 22 个必填字段 + 5 个可选字段 | G2 草案、G3 candidate、append-only 失效事件 | approved | synchronize`
- `view | CURRENT-ENGINEERING-KNOWLEDGE | 当前有效工程知识.base | concept/method 与运营记录分视图 | approved | create`
- `retire | RETRO-KB-INITIAL-20260731 | v1.2 精确快照不可恢复 | pending_review + stale | preserve-history`
- `provenance | V2-SOURCE-ANCHORS | 41 组 source_paths/source_versions；26 个 SHA-256 | 全部匹配 | passed`
- `project-flow | V500E / volant_XJ | 两个 queue 均为 0 个实际事件；无项目事实迁移 | approved | no-change`
- `skill-test | v500e-kb-sync / xj-kb-sync | 相对路径、结构和假想正向/失效场景 | official validator 缺 PyYAML；等效校验与独立 forward test 通过 | passed`
- `lint | V2-FINAL-AUDIT | 41 个 Obsidian Markdown、30 个唯一 ID、9 个受管 Base、122 个有效 Wikilink、224 个本地路径 | 阻断项 0 | passed`
- `scope | EXCLUDED-FCKB-v2 | D:\666\FlightControlKnowledgeableBase | 未扫描、未摄入、未修改 | no-change`
- `correction | V2-FINAL-AUDIT-WIKILINKS | 上一条“122 个有效 Wikilink”应读作 122 个受检 link/embed，其中 117 个知识链接有效、5 个为 AGENTS/欢迎页语法示例 | 失效知识链接 0 | append`
- `governance | D666-CODEX-GOVERNANCE-v2.1.0 | manifest、启动回执、fail-closed 与受限治理维护模式 | approved | implement`
- `governance | TASK-CLOSEOUT-v2.1 | closeout_outcome 与 verification_status 分离；对话结束不等于任务完成 | approved | implement`
- `bootstrap | V500E / volant_XJ | 项目本地兜底与 V500E simmdl-master bridge；XJ 既有 bridge 回读父级 | approved | implement`
- `provenance | GOVERNANCE-BOOTSTRAP-v2.1 | 全局治理、项目 AGENTS 与 bridge 来源锚点刷新 | approved | update`
- `scope | EXCLUDED-FCKB-v2.1 | D:\666\FlightControlKnowledgeableBase | 未扫描、未摄入、未修改 | no-change`

## 2026-09-15

- `ingest | TRF-TRIM-ITERATION-20260915 | 4个新抽象方法、1个来源指针、1个审核票据 | L1/pending_review | 未跨项目放行或升格`
- `route | V500E-OBS-20260915-001..004 | 项目事实保持原位；只接收来源指针和方法草案 | G0-G5人工审核pending`
- `link-update | 配平专题 | 方法、现象、验证框架与来源建立双向导航；不构成循环primary证据`
- `source-drift | SRC-V500E / MOC-V500E-KB-CATALOG | 本批项目索引/注册表/队列变更；旧快照标记pending_review+stale，新专题由独立来源卡导航 | 未重写历史摘要或批准记录`
- `lint | TRIM-KB-20260915 | 20份Markdown、4事件、45项目来源指纹、7个更新注册锚点、关键证据包19成员 | 0阻断 | passed；仅本批文档与来源校验，未重跑MATLAB或验证新数据库`

## 2026-10-04

- `ingest | EVID-TILT-SU-2022 | Su 2022 原始 PDF、可读英文 Markdown、完整中文阅读版、公式与表格核对稿、定向分析及19张图 | 低频建模—配平—LPV—约束 MPC 单源证据 | L1/pending_review | create`
- `link-update | MOC-000 | Su 2022 单源证据导航链接 | link-only | update`
- `lint | SU2022-GOLD-20261004 | 4份包内 Markdown、1份原始 PDF、19张图、38个图片引用、YAML frontmatter、禁用词、相对图片链接及暂存/发布24文件 SHA-256 | 包校验器0错误0警告；暂存与发布逐文件一致 | passed`
- `ingest | EVID-TILT-ZHOU-2022 | Zhou 2022 原始 PDF、英文语义阅读版、完整中文阅读版、公式与表格核对稿、定向分析及15张图 | 四倾转旋翼低频气动干扰、配平和操纵接口单源证据 | L1/pending_review | create`
- `ingest | EVID-TILT-NABI-2021 | Nabi 2021 原始 PDF、英文语义阅读版、完整中文阅读版、公式与表格核对稿、定向分析及14张图 | qLPV 模型拼接、配平数据库和控制设计接口单源证据 | L1/pending_review | create`
- `link-update | MOC-000 | Zhou 2022、Nabi 2021 单源证据导航链接 | link-only | update`
- `lint | ZHOU-NABI-FIRST-BATCH-20261004 | Zhou 20个文件、Nabi 19个文件；包校验器、禁用词、图片引用、来源哈希及暂存/发布逐文件 SHA-256 | 两包均0错误0警告，39/39文件一致 | passed`

## 2026-10-05

- `correction | 2026-08-20 日志术语 | 当时写入的“转周平均”不是来源文献或稳定专业术语，应读作“相对稳态或缓慢变化的气动力”；该旧词仅为历史纠错对象，后续产物禁止使用 | append-only correction`
- `ingest | Wang 2025 倾转旋翼动态过渡走廊单源证据 | 原始 PDF、语义阅读版、完整中文阅读版、公式图表核对稿、定向分析及9张图 | 加减速条件下动态过渡走廊、约束和路径安全 | L1/pending_review | create`
- `ingest | Simmons 2025 RAVEN-SWFT 动态建模与飞控软件单源证据 | 原始 PDF、双层阅读、核对稿、定向分析及8张图 | 风洞—模型—配平—控制—软件—逐级飞试链 | L1/pending_review | create`
- `ingest | Mancinelli 2025 统一增量非线性过渡控制单源证据 | 本地 arXiv:2311.09185v1 阅读包及9张图；正式 T-RO 记录单列 | 统一增量非线性控制、非线性分配、保护约束和实机过渡证据 | L1/pending_review | create`
- `ingest | Bauersfeld 2021 倾转旋翼全包线 MPC 单源证据 | 作者接收稿阅读包及9张图 | 分层全包线 MPC、约束、控制分配、实时失解处置和飞行验证 | L1/pending_review | create`
- `link-update | MOC-000 | Wang、Simmons、Mancinelli、Bauersfeld 四张第二批单源证据导航链接 | link-only | update`
- `lint | SECOND-BATCH-CORRIDOR-CONTROL-20261005 | 4个包共55文件；来源哈希、语义表格、图片引用、禁用词、版本边界及暂存/发布逐文件 SHA-256 | 四包 validator 均 passed、55/55文件一致；Mancinelli 正式路径有1条接近 Windows 路径长度上限的非阻断警告 | passed-with-warning`

## 2026-10-14

- `ingest | EVI-LU-2018-CORRIDOR-001 | Lu 2018 原PDF、完整中文转录、定向分析、公式图表核对稿及7图 | 参考工作点动态矫正、操纵前馈与增益调度单源证据 | L1/pending_review | create`
- `ingest | EV-TILT-LIU2024-INDI | Liu 2024 原PDF、完整中文转录、定向分析、公式图表核对稿及13图 | 推力分量反解、按需倾转、INDI混控与验证边界单源证据 | L1/pending_review | create`
- `ingest | EV-YAN2017-DYNAMIC-CONVERSION-001 | Yan 2017 原PDF、完整中文转录、定向分析、公式图表核对稿及4图 | 操纵变化率输入与离线动态过渡优化单源证据 | L1/pending_review | create`
- `link-update | MOC-000 | 第三批三张单源证据各增加一个导航入口；优先级清单前10篇完成处理 | link-only | update`
- `lint | LITERATURE-EX09-20261014 | 3包36文件、9MD、24图；原PDF哈希、逐文件一致性、术语、模板字段、图片及独立抽审 | 暂存和正式validator均0错误0警告，36/36文件一致，三卡L1/pending_review | passed`
- `retrospective | RETRO-LIT-EX09-20261014 | 第三批文献处理记录 | 中文原文完整转录即阅读版；本地OCR辅助损坏字码；原文科学疑点与数据口径单列；未升格skill或方法 | create`
- `ingest | EV-TILT-YU2024-TRIROTOR-FLIGHT | 完整中文转录、定向分析、核对稿及13图 | 串级分配、悬停参数校正、可达集合权重及正反转换实飞单源证据 | L1/pending_review | create`
- `ingest | EV-TILT-LIAO2025-TWOLEVEL-ALLOCATION | 完整中文转录、定向分析、核对稿及14图 | 力/力矩两级分配与动态逆/DOB接口；源文疑点单列，验证仅仿真 | L1/pending_review | create`
- `ingest | EV-SHENG2022-MODEL-STABILITY-001 | 完整英文语义版、完整中文阅读版、定向分析、核对稿及8图 | 五部件载荷、配平与局部A/B；矩阵/特征值独立复算审计 | L1/pending_review | create`
- `link-update | MOC-000 | 第四批三张证据卡各增加一个导航入口，优先级计划扩为13篇已处理 | link-only | update`
- `lint | LITERATURE-EX10-20261014 | 3包48文件、10MD、35图；源哈希、模板字段、链接、术语及独立抽审 | 暂存/正式validator均0错误0警告，48/48文件一致，三卡L1/pending_review | passed`
- `retrospective | RETRO-LIT-EX10-20261014 | 第四批文献处理记录 | 实飞/仿真/公开矩阵分别建证；数据重算与原表并列，不把结构校验当科学验证 | create`

## 2026-10-15

- `closeout | LITERATURE-EX10 | 第四批收尾 | 三包48文件与暂存一致、三原PDF不变、三卡L1/pending_review；任务处理完成，原文科学疑点继续待审 | completed / verification passed`

## 2026-10-28

- `scope-update | LITERATURE-ALL-INDEX | 用户持续目标 | 按当前索引全部独立文献连续分批处理，保留全文缺失与旧阅读材料缺口；不以13篇子集视为完成 | active`
- `inventory | LITERATURE-COVERAGE-20261028 | 索引与下载清单、实际PDF/MD/证据卡 | 147对象中84份来源PDF哈希匹配、63项缺PDF；14项阅读/分析/证据路径齐全；NASA基础补充单列 | artifact-presence verified，非科学验收`
- `dispatch | LITERATURE-EX11 | Qu2022、Qu2023、Li2026 | 三个隔离暂存范围构建完整中文阅读与定向分析；共享记录仅主线程写入 | active`
- `ingest | EV-QU2022-LPV-TRANSITION-001 | 完整英文/中文、定向分析、核对稿和12图 | 动态轨迹线化、名义量调度与MPC-DRC接口 | L1/pending_review | create`
- `ingest | EV-TILT-QU2023-LPV-MPC-FAILURE | 完整英文/中文、定向分析、核对稿和8图 | 故障转速修正局部模型、约束及有限故障仿真证据 | L1/pending_review | create`
- `ingest | EV-TILT-LI2026-ASYNCHRONOUS-TRANSITION | 完整英文/中文、定向分析、核对稿和11图 | 异步倾转俯仰平衡、两级分配与ESO对照 | L1/pending_review | create`
- `lint | LITERATURE-EX11 | 三包46文件、12MD、31图；114编号式中英覆盖、元数据与源版本、独立语义/视觉抽审 | 六处validator passed且0错误0警告，46/46哈希一致，三原PDF不变 | passed`
- `closeout | LITERATURE-EX11 | 第五批记录与精确清单 | 本批completed，全索引goal仍active；后续67份已有PDF需处理/补齐、63对象待全文，NASA基础补充另列 | obs-candidate`
- `goal-state-observed | LITERATURE-ALL-INDEX | 收尾get_goal返回paused，覆盖执行阶段active快照 | 本线程未调用update_goal；EX11完成结果不变，全目标仍未完成；未启动下一批 | product-paused`

## 2026-10-29

- `resume | LITERATURE-ALL-INDEX | 用户恢复持续目标，get_goal=active；治理2.5.0八文件哈希匹配 | EX11正式46文件与验收清单同哈希；既有规则复用 | active`
- `dispatch | LITERATURE-EX12 | Saetti2025、He2025、Yu2020 | 三来源冻结，三独立暂存writer，完整阅读/分析/L1入库 | active`
- `ingest | EV-TILT-SAETTI2025-AUTOMATIC-DI | 完整英文/中文、分析、核对稿及16图 | 低频模型约简、构型相关配平和动态逆模型调度 | L1/pending_review | create`
- `ingest | EV-TILT-HE2025-CALIBRATED-MPC-PID | 中文全文、分析、核对稿及12图 | 飞行间模型校正、两层混合权重及有限实飞取舍 | L1/pending_review | create`
- `ingest | EV-TILT-YU2020-QUAD-CORRIDOR | 中文全文、分析、核对稿及10图 | 俯仰平衡/前后承载分配及单台功率静态边界 | L1/pending_review | create`
- `lint | LITERATURE-EX12 | 51文件、10MD、38图，77编号组覆盖及独立语义/视觉抽审 | He图7裁切修复后，暂存/正式validator均0错误0警告，51/51哈希一致，三原PDF不变 | passed`
- `closeout | LITERATURE-EX12 | 第六批处理记录与精确清单 | 本批completed/passed/obs-candidate；全索引仍active，20项完整交付、64份本地待处理/补齐、63项待全文 | continuing`
- `dispatch | LITERATURE-EX13 | Zhou2019、Xin Yu2022、Ma2022 | 前批51文件当前同哈希；三来源53页冻结，完整阅读/分析/入库继续，Yu注明作者稿版本 | active`
- `ingest | EV-TILT-ZHOU2019-OPTIMAL-TILT-CURVE | 完整中文、分析、核对稿及13图 | 倾转曲线/分配耦合与舵机动态折中，阻力功非电耗 | L1/pending_review | create`
- `ingest | EV-TILT-YU2022-CONVERSION-WORKLOAD | 作者稿完整英中、分析、核对稿及22图 | 正反转换操纵/权限/功率优化，小波指标不当真实驾驶员评估 | L1/pending_review | create`
- `ingest | EV-TILT-MA2022-3D-ENVELOPE | 完整英中、分析、核对稿及11图 | 三维稳态状态域、SEP量纲与完整配平/动态可达性边界 | L1/pending_review | create`
- `lint | LITERATURE-EX13 | 60文件、11MD、46图；48编号式覆盖与独立审校 | 暂存/正式validator均0错误0警告、60/60哈希一致、三原PDF不变，三卡index唯一 | passed`
- `closeout | LITERATURE-EX13 | 第七批记录与精确清单 | 本批completed/passed/obs-candidate；全目标active，23项完整交付，61份本地待处理/补齐、63项待全文 | continuing`
- `dispatch | LITERATURE-EX14 | Deng2025、Chen2024、Belak2025既有材料补齐 | 旧树/卡冻结；复用完整内容，新增中文/分析/审校或证据，必要最小修复先核源 | active`
- `stage-verify | LITERATURE-EX14 | 候选57文件；较原树新增9、修改2；Deng原22不变；Belak精确恢复/Chen页码已审 | 父级qa-staging passed，正式未发布 | pending-publication`
- `goal-state-observed | LITERATURE-ALL-INDEX | EX14发布前get_goal返回paused | 未调用update_goal；停止新动作，保存第八批续接记录及已审卡补丁，所有writer停止 | product-paused / incomplete`

## 2026-11-04

- `resume | LITERATURE-ALL-INDEX | 用户继续处理；get_goal=active，治理2.5.0八文件通过 | EX14旧树/卡与冻结基线一致，接续发布不重做翻译 | active`
- `ingest | EVID-TILT-DENG-2025 | 复用既有英中/分析，补公式图表核对和证据卡 | 三维走廊与离散路径优化边界 | L1/pending_review | create`
- `ingest | EVID-TILT-CHEN-2024 | 新增完整中文/定向分析/核对；维护旧卡身份和关系 | NMPC/下层QP接口、源文维数与参数疑点；修正10处页码 | L1/pending_review | update`
- `ingest | EVID-TILT-BELAK-2025 | 新增完整中文/定向分析/核对；最小恢复英文转录并修正两句实现工具事实 | 局部二维逆映射及物理解筛选边界 | L1/pending_review | update`
- `lint | LITERATURE-EX14 | 三包57文件一致、六处validator passed且0错误0警告、107本地链接和2旧wikilink通过 | 新增9包文件/修改2根稿；旧根稿及旧卡有字节备份；原始PDF和40图不变 | passed`
- `closeout | LITERATURE-EX14 | 第八批记录及final_changes.json | 本批completed/accepted，科学结论仍L1；累计26项完整交付，58份本地待处理/补齐、63项待全文 | 全目标active`

## 2026-11-10

- `resume | LITERATURE-EX15 | 旧执行宿主stale且无活跃分包writer，任务书R001与治理8/8匹配 | 复用已完成提取和逐页图像，三独占暂存范围转交G2内部writer，继续全文构建 | active`
- `source-acquisition | doi:10.1155/2018/8641731; doi:10.1155/2020/4757381 | 两个公开PDF镜像 | 11-04下载并写入，11-10核正式哈希；Kong13物理页含12正文页及末页广告，Chen14页；原84来源哈希仍匹配 | no-change / 来源取得未作工程摄入`
- `coverage | LITERATURE-ALL-INDEX | acquisition_delta_20261110.json | 147对象中86已有PDF、61缺PDF；26分析完成、60本地待处理；NASA基础补充另列 | goal active`
- `source-acquisition | doi:10.1155/2020/8876867 | Wang2020公开PDF镜像 | 13页身份/首末页核验，新增原PDF目录，发布哈希92E90557…70F468；未完成分析 | no-change / 来源取得`
- `coverage | LITERATURE-ALL-INDEX | 当日3项补收合并映射 | 87已有PDF、60缺PDF；26分析完成、61本地待处理；旧84哈希匹配 | goal active`

- `ingest | PAT-US20230393588A1 | 2026-11-10 | 英文阅读版、完整中文译文、定向分析、核对记录、来源与单源证据 | L1/pending_review | obs-candidate/create`
- `lint | PAT-US20230393588A1 | 暂存与正式逐文件同哈希、原PDF SHA-256匹配；4份MD，阅读包共13文件 | 结构检查passed`

- `ingest | PAT-US12105533B1 | 2026-11-10 | 英文阅读版、完整中文译文、定向分析、核对记录、来源与单源证据 | L1/pending_review | obs-candidate/create`
- `lint | PAT-US12105533B1 | 暂存与正式逐文件同哈希、原PDF SHA-256匹配；4份MD，阅读包共17文件 | 结构检查passed`

- `ingest | PAT-US10974826B2 | 2026-11-10 | 英文阅读版、完整中文译文、定向分析、核对记录、来源与单源证据 | L1/pending_review | obs-candidate/create`
- `lint | PAT-US10974826B2 | 暂存与正式逐文件同哈希、原PDF SHA-256匹配；4份MD，阅读包共48文件 | 结构检查passed`

- `ingest | PAT-US12539963B2 | 2026-11-10 | 英文阅读版、完整中文译文、定向分析、核对记录、来源与单源证据 | L1/pending_review | obs-candidate/create`
- `lint | PAT-US12539963B2 | 暂存与正式逐文件同哈希、原PDF SHA-256匹配；4份MD，阅读包共8文件 | 结构检查passed`

- `ingest | PAT-US11964755B2 | 2026-11-10 | 英文阅读版、完整中文译文、定向分析、核对记录、来源与单源证据 | L1/pending_review | obs-candidate/create`
- `lint | PAT-US11964755B2 | 暂存与正式逐文件同哈希、原PDF SHA-256匹配；4份MD，阅读包共44文件 | 结构检查passed`

- `ingest | PAT-US20240326984A1 | 2026-11-10 | 英文阅读版、完整中文译文、定向分析、核对记录、来源与单源证据 | L1/pending_review | obs-candidate/create`
- `lint | PAT-US20240326984A1 | 暂存与正式逐文件同哈希、原PDF SHA-256匹配；4份MD，阅读包共11文件 | 结构检查passed`

- `ingest | PAT-US12391364B2 | 2026-11-10 | 英文阅读版、完整中文译文、定向分析、核对记录、来源与单源证据 | L1/pending_review | obs-candidate/create`
- `lint | PAT-US12391364B2 | 暂存与正式逐文件同哈希、原PDF SHA-256匹配；4份MD，阅读包共26文件 | 结构检查passed`

- `ingest | PAT-US12404020B1 | 2026-11-10 | 英文阅读版、完整中文译文、定向分析、核对记录、来源与单源证据 | L1/pending_review | obs-candidate/create`
- `lint | PAT-US12404020B1 | 暂存与正式逐文件同哈希、原PDF SHA-256匹配；4份MD，阅读包共44文件 | 结构检查passed`

- `ingest | PAT-US12545401B2 | 2026-11-10 | 英文阅读版、完整中文译文、定向分析、核对记录、来源与单源证据 | L1/pending_review | obs-candidate/create`
- `lint | PAT-US12545401B2 | 暂存与正式逐文件同哈希、原PDF SHA-256匹配；4份MD，阅读包共39文件 | 结构检查passed`
