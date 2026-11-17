---
id: EVID-NASA-STD-7009B-CFD-AERODATA
type: evidence
title: NASA-STD-7009B CFD气动数据源交付与验收单源证据
maturity: L1
review_status: candidate
scope: global
sensitivity: public
risk_level: high
source_kind: primary
source_paths:
  - 'D:\666\学习资料\报告\NASA-STD-7009B-Final-3-5-2024.pdf'
source_versions:
  - 'sha256:7B95A2065325DC3035C35CE110557D817555D3F22210CE5F6CE0BB2FB4CCD8B4'
canonical_path: 'D:\666\obs\20_来源卡\NASA 7009B\证据卡\NASA-STD-7009B CFD气动数据源交付与验收单源证据.md'
record_role: canonical
moved_to:
derived_from:
  - 'SRC-NASA-STD-7009B'
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

# NASA-STD-7009B CFD气动数据源交付与验收单源证据

> [!warning]
> 本页只对应 NASA-STD-7009B 这一份 primary 来源。它提取的是通用 M&S 可信度要求，不等于已经形成 CFD 专用合同条款，也不可作为 L2/L3 默认工程结论。

## 一句话结论

NASA-STD-7009B 要求在使用前明确用途和验收准则，并保存数据谱系、单位/坐标系、假设、限制、验证域、不确定度、技术评审与发布边界证据；这些内容可构成 CFD 气动数据验收骨架，但标准没有规定 CFD 专用实现细节和数值阈值。

## 文献基本信息

- 文档：NASA-STD-7009B，Revision B，批准日期 2024-03-05。
- 类型：NASA 模型与仿真标准；正式文档，公开发布。
- 研究对象：跨学科 M&S 的开发、评估和使用可信度。
- 本页主题：作为下游建模输入的 CFD 气动数据如何证明可用、可追溯、可验证和受限使用。
- 页码口径：本页 `PDF p.` 指本地文件页序；查阅时以条款号、`[M&S xx]` 和表号优先。

## 证据类别约定

- **来源明确事实**：标准正文明确提出的要求或定义。
- **作者推导**：标准附录或解释性文字给出的分级、比较或使用逻辑。
- **合理推测**：本卡为映射到 CFD 数据交付所作的结构化解释。
- **工程建议**：留待综合方法页提出，不作为本来源结论。
- **文中未说明**：该标准没有给出，禁止自行补成 NASA 要求。

## 关键假设和适用边界

- **来源明确事实**：标准围绕模型/仿真的预定用途和风险组织可信度活动，不要求所有项目达到同一固定能力等级。
- **来源明确事实**：学科专用实现细节应由适用的推荐实践补充；§4.1.3（PDF p.22）明确列出 AIAA G-077 等参考。
- **合理推测**：CFD 结果若用于拟合气动数据库、代理模型或查表模型，应作为开发期 RWS 数据和 Data Pedigree 证据对象处理。
- **适用边界**：本标准适合规定“必须证明什么”，不适合单独规定“网格多密、误差多小、交什么格式”。

## 核心问题

一个用于指定建模用途的 CFD 数据源，在被接受前最低需要哪些可审计的可信度证据，以及怎样限定其允许用途和验证域？

## 方法输入、输出与步骤

以下结构是对标准条款的**合理推测**，不是标准原文给出的 CFD 工作流。

### 输入

- 预定用途、关键问题和所需预测量。
- CFD 数据集、生成它的模型/软件及真实世界系统（RWS）参照数据。
- 精度、验证、确认、不确定度和敏感性准则。
- 单位、坐标系、假设、抽象、限制及使用范围。

### 输出

- 数据谱系与版本化证据包。
- Verification/Validation 域、数值误差和不确定度说明。
- 模型能力评估、Permissible Use、Use Assessment 和必要的超域警示。
- 技术评审记录及可复查/可复现依据。

### 条款链

1. 记录 intended use，并预先建立 V&V、UQ、敏感性和评估阈值。
2. 保存相关 RWS 数据、Data Pedigree、开发数据集及支撑软件。
3. 明确单位、矢量坐标系、假设、数学结构、限制和允许用途。
4. 分开执行代码验证、解验证和经验确认，并记录各自覆盖域。
5. 评估参照数据与模型不确定度。
6. 通过能力评估、技术评审和 proposed-use assessment 决定可用范围。

## 关键公式或算法

- **文中未说明**：没有 CFD 专用的编号公式、统一误差合成式、网格收敛率或通过阈值。
- Appendix E 的 Validation 说明（PDF pp.67–69）要求比较模型与参照的均值差异及其不确定度，并把区间重叠作为最低层次的判断方式之一；这不是一个可脱离项目准则使用的通用验收公式。
- **工程限制**：不得把能力等级表或区间重叠解释成“只要重叠就一定接受”。

## 结果与验证证据

- §4.1.1.1 `[M&S 40]`，PDF p.20：记录 intended use。
- §4.1.1.5 `[M&S 43]`，PDF p.21：为 Verification、Validation、Uncertainty、Sensitivity 和 Assessment 建立验收准则；solution accuracy/precision 属于 V&V 范围。
- §4.2.1 `[M&S 10/45/46/11–14/48/47]`，PDF pp.24–27：RWS 数据和谱系、开发数据与支撑软件留存、单位/坐标系、假设、数学结构、限制、允许用途、能力评估和使用指南。
- §4.2.3 `[M&S 15/16]`，PDF p.28：区分代码验证和解验证，要求记录 Verification Domain 与数值误差估计。
- §4.2.5 `[M&S 17/18]`，PDF pp.29–30：记录 Validation Domain、参照、指标、数据集及尚未验证部分；经验数据的来源、采集和处理应采用相称严谨度。
- §4.2.7 `[M&S 19/21]`，PDF p.30：评估参照和模型不确定度。
- §4.3.1 `[M&S 22–27]`，PDF pp.30–32：Proposed Use、Use Assessment、Input Pedigree、设置理由及超出允许/验证域时的 placard。
- Appendix E，Table 7（p.66）、Table 8（p.67）、Table 9（p.69）、Table 10（p.70）：Data Pedigree、Verification、Validation 和 Technical Review 分级；Technical Review Level 3 引入独立同行评审，Level 4 进一步要求独立因素评估和复现验证结果。
- Appendix E，Table 14（p.74）、Table 16（pp.75–76）、Table 17（p.77）：Permissible/Proposed Use、Input Pedigree 和 UQ。

## 工程价值与不可直接采用部分

### 可直接借鉴为规范骨架

- 要求供应方说明数据的预定用途、允许用途、验证域和失效边界。
- 要求交付数据版本、单位/坐标系、假设、限制、谱系和支撑软件证据。
- 把代码验证、解验证、经验确认、UQ 和独立技术评审分开验收。
- 对超出验证域的使用显式标记，而不是静默外推。

### 不可直接采用

- 不可把任一 capability level 当作 NASA 对所有 CFD 数据的最低强制等级。
- 不可从该标准推出具体网格、湍流模型、时间步、残差或误差百分比。
- 不可用通用 M&S 能力评估代替 CFD 学科验证方案和项目验收用例。

## 可复现性

- 复现一次能力评估至少需要：预定用途、需求/准则、数据及版本、支撑软件、验证案例、参照数据、数值误差和 UQ 证据。
- 标准只规定证据类别和评估思想，未给出统一评分脚本、数据 Schema 或计算程序；不同项目的等级判定仍依赖人工解释。
- 数据或支撑软件版本变化后，应重新检查 Data Pedigree、Verification/Validation Domain、UQ 和 Use Assessment。

## 文中未说明

- CFD 气动力/力矩系数的变量清单、符号、参考面积/长度和参考点模板。
- 文件格式、字段 Schema、缺失值/质量标记、案例 manifest 的机器可读结构。
- 网格类型、单元数、湍流/转捩模型、壁面处理和边界条件的统一选择。
- 网格/时间步收敛阈值、残差阈值、允许偏差、置信度和误差预算数值。
- 独立验证必须由哪个组织承担，以及盲态、抽样复算和签署深度。
- 面向下游查表或代理建模的覆盖度、平滑性、留出重构误差和外推判据。

## 精确锚点

- §4.1.1.1，PDF p.20；§4.1.1.5，PDF p.21；§4.1.3，PDF p.22。
- §4.2.1，PDF pp.24–27；§4.2.3，PDF p.28；§4.2.5、§4.2.7，PDF pp.29–30。
- §4.3.1，PDF pp.30–32。
- Appendix E，Tables 6–10，PDF pp.65–70；Table 14，p.74；Table 16，pp.75–76；Table 17，p.77。

## 知识增量记录

- delta_route：`obs-candidate`
- delta_operation：`create`
- 关联来源：[[../NASA-STD-7009B 来源卡|NASA-STD-7009B 来源卡]]
- 关联方法：[[../../../30_知识维基/气动数据与模型可信度/CFD气动数据源交付与验收规范方法候选|CFD 气动数据源交付与验收规范方法候选]]
- 适用限制：只支持通用验收骨架，不支持 CFD 专用数值阈值和交付 Schema。
- 升格建议：无；保持 L1/candidate，等待学科专标和实际数据包试验。

