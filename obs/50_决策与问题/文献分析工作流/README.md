---
id: MOC-LITERATURE-ANALYSIS-WORKFLOW-001
type: moc
title: 文献分析工作流校准入口
maturity: L1
review_status: pending_review
scope: personal
sensitivity: internal
risk_level: low
source_kind: derived
source_paths:
  - 'D:\666\obs\.agents\tasks\LT-LITERATURE-ANALYSIS-CALIBRATION-20260819.md'
source_versions: []
canonical_path: 'D:\666\obs\50_决策与问题\文献分析工作流\README.md'
record_role: canonical
moved_to:
derived_from: []
supersedes: []
superseded_by: []
flags: []
status_reason: 已同步共同物理现象优先和清晰中文约束；工作流仍需第二篇不同类型文献前向验证
invalidated_at:
invalidated_by:
decision_ref: ADR-LITERATURE-ANALYSIS-001
review_owner: user
created: 2026-08-19
updated: 2026-08-20
last_verified: 2026-08-20
review_due:
approvals: []
gate_records: []
reuse_count: 0
---

# 文献分析工作流校准入口

> [!summary]
> 本目录保存“如何阅读和拆解文献”的控制记录、用户明确决策、偏好候选与 skill 候选，不保存论文事实正文。论文翻译和单源定向分析留在对应文献目录；跨文献通用工程方法如后续形成，再按审核流程进入 `30_知识维基`。

## 当前校准样本

- 文献：*Numerical Simulation of Rotor-Wing Transient Interaction for a Tiltrotor in the Transition Mode*
- 作者：Zhenlong Wu、Can Li、Yihua Cao
- 年份：2019
- 研究对象：0.658 缩比 V-22 的旋翼–机翼组合体
- 任务台账：`D:\666\obs\.agents\tasks\LT-LITERATURE-ANALYSIS-CALIBRATION-20260819.md`
- 来源 PDF SHA-256：`2D2C0898978DCE92D2732986DCB6C0C2D8DAF2E39774D2F825E9EE1AC1019DD3`
- 中文阅读版：`D:\666\学习资料\倾转旋翼综合设计方法论_打包_20260628_203550\文献\倾转过渡路径设计_近十年_2016-2026\PDFs\Joby倾转旋翼与V尾旋翼干扰文献补充\2019_Wu_Rotor_Wing_Transition\衍生阅读\倾转旋翼过渡模态下旋翼-机翼瞬态干扰数值模拟_中文阅读版.md`
- 定向分析：`D:\666\学习资料\倾转旋翼综合设计方法论_打包_20260628_203550\文献\倾转过渡路径设计_近十年_2016-2026\PDFs\Joby倾转旋翼与V尾旋翼干扰文献补充\2019_Wu_Rotor_Wing_Transition\衍生阅读\Wu 2019 旋翼-机翼瞬态干扰_面向飞控建模的定向分析.md`
- 单源证据：[[../../20_来源卡/倾转旋翼资料包/证据卡/Wu 2019 过渡模态旋翼-机翼瞬态干扰单源证据|Wu 2019 单源证据]]
- 低阶模型方法候选：[[../../30_知识维基/气动干扰/过渡段旋翼-机翼动态干扰低阶建模方法候选|过渡段动态干扰低阶建模方法候选]]

## 记录入口

- [[文献拆解决策记录]]
- [[文献分析偏好候选]]
- skill 候选：`literature-analysis-guided/SKILL.md`

## 内容分层

| 内容 | 保存位置 | 当前状态 |
|---|---|---|
| 原始 PDF、英文 Markdown、图表资源 | 原文献目录 | 只读来源 |
| 中文阅读版 | 原文献 `衍生阅读` 目录 | 默认交付形式已获用户确认 |
| 面向飞控建模的定向分析 | 原文献 `衍生阅读` 目录 | 已把共同物理现象、悬停证据方向、非单调趋势和常识审查移到主线 |
| 单源论文证据 | `20_来源卡/.../证据卡` | L1 / candidate |
| 可复用低阶模型结构 | `30_知识维基/气动干扰` | L1 / candidate；不含 MATLAB 实施 |
| 用户明确决定 | 本目录 `文献拆解决策记录.md` | 已记录共同现象优先和清晰表达要求 |
| 助手归纳的阅读偏好 | 本目录 `文献分析偏好候选.md` | 用户显式确认项与待验证推断继续分列 |
| 可复用执行流程 | 本目录 `literature-analysis-guided/` | 候选 skill 已加入可执行语言闸门；仍未发布为稳定工作区 skill |

## 当前阶段

1. 用户已确认后续默认采用“完整中文阅读版 + 定向分析”，并确认低阶模型留存但暂不实施。
2. Wu 2019 定向分析已按“控制律深化、两项机理审计降为辅助、准定常精度只保留定性范围”修订。
3. 本篇已形成单源证据和低阶动态干扰方法候选；表 5–8 只提供本算例的模型差异测试范围，不发布为通用干扰系数。
4. 分析主线已经调整为“共同物理现象—显著趋势—是否违背常识—方法差异与证据边界”。
5. 当前用户的飞行动力学模型默认以转周平均或缓慢变化的气动力为主体；旋翼转速倍频等高频短周期载荷仍作为论文事实保留，但不作为默认显式模型项。
6. 高频项只在结构载荷、振动、执行机构 RMS、传感器混叠/滤波、高带宽控制或用户明确要求时重新提高权重。
7. 中文表达默认写清具体对象、工况、比较基准、数字口径、工程用途和禁用边界，不再用“宽范围扫描”“基线接近零”等未解释短语代替完整结论。
8. 候选 skill 完成结构校验和本轮前向测试后，仍需第二篇不同类型文献验证，再决定是否发布为稳定 skill。

## 最新同步状态

| 对象 | 状态 | 说明 |
|---|---|---|
| 决策与偏好 | 已更新 | 最新反馈已按用户明确决定与助手推断候选分层记录 |
| Wu 2019 分析、单源证据、方法候选 | 已更新 | 保留周期现象为来源事实；默认模型改为平均/慢变化载荷主体，高频项降为特殊验证扩展 |
| `literature-analysis-guided` | 已更新但仍是候选 | 已加入按用户模型带宽筛选物理现象的闸门，并通过官方校验和独立前向测试；尚未安装为稳定 skill |
| 稳定 `flight-literature-reader` | 未修改 | 避免用一篇论文的反馈直接改变跨任务稳定配置 |

## 使用边界

- 当前记录只适用于用户个人在本工作区内的工程文献阅读。
- 现阶段不把一次选择固化为全局绝对规则。
- 控制分配、控制律推导、试飞分析、综述与纯 CFD 复现任务可能需要不同信息权重。
- 当前用户明确要求始终优先于本目录中的历史候选。
