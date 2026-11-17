---
id: SRC-NASA-STD-7009B
type: source
title: NASA-STD-7009B 来源卡
maturity: L1
review_status: candidate
scope: global
sensitivity: public
risk_level: low
source_kind: primary
source_paths:
  - 'D:\666\学习资料\报告\NASA-STD-7009B-Final-3-5-2024.pdf'
source_versions:
  - 'sha256:7B95A2065325DC3035C35CE110557D817555D3F22210CE5F6CE0BB2FB4CCD8B4'
canonical_path: 'D:\666\obs\20_来源卡\NASA 7009B\NASA-STD-7009B 来源卡.md'
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

# NASA-STD-7009B 来源卡

> [!summary]
> 本来源提供 NASA 模型与仿真可信度要求的规范性骨架，适合为 CFD 气动数据交付建立用途、谱系、验证、确认、不确定度、评审和使用边界要求；它不是 CFD 学科专用标准。

## 来源定位

- 发布机构：National Aeronautics and Space Administration（NASA）。
- 文档编号与版本：NASA-STD-7009B，Revision B，批准日期 2024-03-05。
- 官方目录：[NASA-STD-7009](https://standards.nasa.gov/standard/nasa/nasa-std-7009)，核验时状态为 active。
- 权限与敏感级别：封面标明 `Approved for public release; distribution is unlimited`；本卡按 `public` 管理。
- 项目归属：NASA 通用 M&S 标准，不属于 V500E、volant_XJ 或其他本地项目；`scope=global`。
- 文件类型：PDF，88 页，1,417,365 bytes。
- 本地身份：SHA-256 `7B95A2065325DC3035C35CE110557D817555D3F22210CE5F6CE0BB2FB4CCD8B4`；本地修改时间 `2026-08-07T11:11:06.1398786+08:00`。
- 原始文件保持原位；本 Vault 只保存路径、身份和提炼结果，不复制或改写 PDF。

## 版本说明

- 本地 PDF 封面及文档历史将其标识为 Revision B，并说明其替代 Revision A with Change 1。
- NASA 官方目录的版本/Change 展示若与 PDF 修订史文字存在差异，应以“文档编号 + Revision B + 批准日期 + 本地 SHA-256”共同锚定本次证据，不静默改写版本记录。
- 项目或合同只有在明确采用该标准后，才可把相应条款转化为具体交付义务；本来源卡不替代项目合同裁决。

## 来源价值与阅读优先级

### P0：直接支撑 CFD 数据源验收骨架

- §4.1.1.5，PDF p.21：预先定义 Verification、Validation、Uncertainty、Sensitivity 等验收准则。
- §4.2.1，PDF pp.24–27：RWS 数据与 Data Pedigree、开发数据及支撑软件留存、单位/矢量坐标系、假设、结构、限制、允许用途与能力评估。
- §4.2.3、§4.2.5、§4.2.7，PDF pp.28–30：代码/解验证、验证域、经验确认和不确定度。
- §4.3.1，PDF pp.30–32：Proposed Use、Use Assessment、超域使用警示和运行输入谱系。
- Appendix E，PDF pp.65–70、74–77：Tables 6–10、14、16、17 的可信度分级依据。

### P1：理解如何补足 CFD 学科细则

- §4.1.3，PDF p.22：说明学科专用实施细节应由推荐实践补充，并列出 AIAA G-077 等参考。

### P2：本主题可略读

- 与具体 CFD 气动数据交付无直接关系的通用生命周期展开、完整要求矩阵和培训性说明。

## 可进入知识层的内容

- intended use / proposed use / permissible use 的区分。
- Data Pedigree、Input Pedigree 及其适用对象。
- 单位、坐标系、假设、限制、验证域和不确定度的证据要求。
- 代码验证、解验证、经验确认和技术评审的职责边界。
- 数据集与支撑软件的配置管理、评审和复现要求。

## 排除内容

- 原始 PDF、签署页和整表复制。
- 与目标主题无关的逐章摘要和长段原文。
- 将能力等级直接解释为所有 CFD 项目的固定准入等级。
- 文中没有给出的 CFD 网格阈值、文件格式、系数表 Schema 或统一误差值。

## 下游证据与候选

- [[证据卡/NASA-STD-7009B CFD气动数据源交付与验收单源证据|NASA-STD-7009B CFD 气动数据源单源证据]]
- [[../../30_知识维基/气动数据与模型可信度/CFD气动数据源交付与验收规范方法候选|CFD 气动数据源交付与验收规范方法候选]]

## 查重结论

- 2026-08-07 对 `index.md`、现有来源卡、单源证据和方法候选执行标题、编号及主题查重，未发现相同 source ID 或相同来源卡。
- 与路径数据库工具链、低阶滑流模型和 NASA CR-166537 验证闭环存在邻近关系，但来源、问题和证据作用不同，不合并正文。

## 待确认问题

1. 是否另行摄入 STD §4.1.3 指向的 CFD 学科实践，补齐数值验证细则？
2. 实际项目准备采用哪些条款为合同 `shall`，哪些只作为评审指南？
3. 项目所需 Technical Review 等级及独立性定义由谁批准？

## 知识增量记录

- delta_route：`obs-candidate`
- delta_operation：`create`
- 适用限制：来源卡只负责定位和准入，不作为默认工程结论。

