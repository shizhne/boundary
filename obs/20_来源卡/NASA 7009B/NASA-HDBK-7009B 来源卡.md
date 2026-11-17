---
id: SRC-NASA-HDBK-7009B
type: source
title: NASA-HDBK-7009B 来源卡
maturity: L1
review_status: candidate
scope: global
sensitivity: public
risk_level: low
source_kind: primary
source_paths:
  - 'D:\666\学习资料\报告\NASA-HDBK-7009B_Final 02-03-2026.pdf'
source_versions:
  - 'sha256:DDA7EF5EF938A021B6ECA6A0BC74FAF5A0CD27F609F4DCBA1ECEC7CA5F67AF31'
canonical_path: 'D:\666\obs\20_来源卡\NASA 7009B\NASA-HDBK-7009B 来源卡.md'
record_role: canonical
moved_to:
derived_from: []
supersedes: []
superseded_by: []
flags: []
status_reason:
invalidated_at:
invalidated_by:
decision_ref:
review_owner:
created: 2026-08-07
updated: 2026-08-07
last_verified: 2026-08-07
review_due:
approvals: []
gate_records: []
reuse_count: 0
---

# NASA-HDBK-7009B 来源卡

> [!summary]
> 本来源是 NASA-STD-7009B 的实施指导，直接解释 Data Pedigree、数值误差、验证参照、校准/留出分离、独立比较、发布与变更证据，最适合用于把标准骨架转化为 CFD 数据需求书和验收检查表。

## 来源定位

- 发布机构：National Aeronautics and Space Administration（NASA）。
- 文档编号与版本：NASA-HDBK-7009B，Revision B；封面批准日期 2026-02-03。
- 官方目录：[NASA-HDBK-7009](https://standards.nasa.gov/standard/nasa/nasa-hdbk-7009)，核验时状态为 active。
- 权限与敏感级别：封面标明 `Approved for public release; distribution is unlimited`；本卡按 `public` 管理。
- 项目归属：NASA 通用 M&S 实施手册，不属于 V500E、volant_XJ 或其他本地项目；`scope=global`。
- 文件类型：PDF，175 页，3,017,486 bytes。
- 本地身份：SHA-256 `DDA7EF5EF938A021B6ECA6A0BC74FAF5A0CD27F609F4DCBA1ECEC7CA5F67AF31`；本地修改时间 `2026-08-07T12:02:13.4781663+08:00`。
- 原始文件保持原位；本 Vault 只保存路径、身份和提炼结果，不复制或改写 PDF。

## 版本说明

- 封面和 NASA 官方目录支持将当前文件视为 Revision B 的 active/final 发布件。
- PDF 文档历史中的 Revision B 日期为 2025-09-24，封面批准日期为 2026-02-03，部分运行页眉仍保留 `DRAFT: NASA-HDBK-7009B`。本卡将其记录为元数据异常，不据此推翻官方 active 状态。
- 引用时应同时使用文档号、Revision B、封面批准日期和本地 SHA-256；若后续获得 NASA 发布校验和，应重新比对并触发依赖复核。
- HDBK 是实施指导，不应在合同中被无说明地当成独立强制规范。

## 来源价值与阅读优先级

### P0：直接转化为数据需求与验收证据

- §5.2.1.3.1，PDF pp.59–60：问题、预测量、精度、不确定度、应用/验证域、禁用情况、网格和数值误差要求。
- §5.5.1–§5.5.5，PDF pp.72–89：验证先后关系、解验证、校准与独立验证、预定准则、发布与变更控制。
- Appendix B，PDF pp.139–142：验证参照质量、模型构建方/使用方风险及独立比较。
- Appendix D，PDF pp.151–157：Data Pedigree、Verification、Validation、Technical Review 的评估解释。

### P1：建立谱系和灰箱证据交付

- §4.4，PDF p.33：通过现有证据引用满足文档要求，而非为每项活动重新造文档。
- §4.5.1–§4.5.2，PDF pp.34–36：Data/Input Pedigree 和代理模型继承限制。
- §5.1.1.2、§5.1.1.5，PDF pp.47–52：预定用途及 CFD/风洞气动数据库示例。
- §5.6.1.2–§5.6.1.3，PDF pp.91–95：Use Assessment、输入来源/质量/形式、派生量及相关输入处理。

### P2：本主题可略读

- AI 专题、完整 worksheet、培训性叙述和与 CFD 数据供应无直接关系的通用示例。

## 可进入知识层的内容

- 用于模型开发的数据按 Data Pedigree 管理，运行时输入按 Input Pedigree 管理。
- 需求方提前定义问题、输出、域、精度、UQ、允许和禁止用途。
- 数值收敛不能替代准确性证明；生产设置差异需要进入不确定度。
- 校准数据和留出验证数据分离，比较前冻结验收准则与允许不确定度。
- 验证参照自身也要验证；发布基线、变更理由和限制需可追溯。

## 排除内容

- 原始 PDF、完整 worksheet、整图整表和大段原文。
- 把示例性的置信度或误差百分比转写为通用验收阈值。
- 把“风洞和 CFD 经常共同使用”转写成每个项目都必须同时具备两类数据。
- 把运行页眉中的 `DRAFT` 单独作为文件非正式的结论。
- 文中没有给出的 CFD 数据 Schema、网格规模、湍流模型或统一误差预算。

## 下游证据与候选

- [[证据卡/NASA-HDBK-7009B CFD气动数据源交付与验收单源证据|NASA-HDBK-7009B CFD 气动数据源单源证据]]
- [[../../30_知识维基/气动数据与模型可信度/CFD气动数据源交付与验收规范方法候选|CFD 气动数据源交付与验收规范方法候选]]

## 查重结论

- 2026-08-07 对 `index.md`、现有来源卡、单源证据和方法候选执行标题、编号及主题查重，未发现相同 source ID 或相同来源卡。
- 与 NASA-STD-7009B 是“规范骨架—实施指导”关系，应建立双向关联，不合并为单一来源。

## 待确认问题

1. 是否需要获取 NASA 发布端的独立校验和，消除本地文件身份的剩余不确定性？
2. 项目采用 HDBK 指导时，哪些证据只需引用，哪些证据必须随数据包直接交付？
3. 独立验证的组织、盲态和复算深度应如何按风险分级？

## 知识增量记录

- delta_route：`obs-candidate`
- delta_operation：`create`
- 适用限制：来源卡只负责定位和准入，不作为默认工程结论。

