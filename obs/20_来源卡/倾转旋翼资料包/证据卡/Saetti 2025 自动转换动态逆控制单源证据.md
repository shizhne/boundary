---
id: EV-TILT-SAETTI2025-AUTOMATIC-DI
type: evidence
title: Saetti 2025 自动转换动态逆控制单源证据
maturity: L1
review_status: pending_review
scope: personal
sensitivity: internal
risk_level: high
source_kind: primary
source_paths:
  - 'D:\666\学习资料\倾转旋翼综合设计方法论_打包_20260628_203550\文献\倾转过渡路径设计_近十年_2016-2026\PDFs\2025_saetti_Dynamic_Inversion_Flight_Control_Laws_for_Automatic_Transition_of_Tilt-Rotor_Wing_Aircraft.pdf'
source_versions:
  - 'sha256:44514BDDF3BB968D81B84BF290D0E71D0BFA457DACFCCC461CDED11509F5276F'
canonical_path: 'D:\666\obs\20_来源卡\倾转旋翼资料包\证据卡\Saetti 2025 自动转换动态逆控制单源证据.md'
record_role: canonical
derived_from: []
supersedes: []
flags: []
created: 2026-10-29
updated: 2026-10-29
last_verified: 2026-10-29
review_due:
approvals: []
gate_records: []
reuse_count: 0
---

# Saetti 2025 自动转换动态逆控制单源证据

## 结论及边界

本文将高阶多旋翼/机翼模型约简为低频控制设计模型，并以固定反馈增益、随速度调度的动态逆实现四构型自动转换仿真。价值在模型—控制接口组织；不证明真实Joby/Archer参数、全包线操纵品质、实际控制权限或执行器约束均可满足。

来源：Umberto Saetti，*Dynamic Inversion Flight Control Laws for Automatic Transition of Tilt-Rotor/Wing Aircraft*，Journal of the American Helicopter Society 70,032005(2025)，DOI10.4050/JAHS.70.032005。正式排印版21页、45编号组（56标签）、8表、16技术图、59参考。

## 关键假设

TR近似XV-15；TMR、TMR2、TW分别近似Joby S4、Archer Midnight与NASA倾转机翼，但后三者机身和质量惯量来自缩放，部分几何来自公开图片。低频降阶假设快状态稳定且有时间尺度分离；控制逆矩阵需要可逆。总速度/构型变化通过局部矩阵调度进入控制；航向系与机体系及倾角符号需按原文审计统一。闭环转换为数值仿真，试飞数据只用于TR对象模型对照。

## 单源声明

|ID|类别|事实或方法|来源锚点与限制|
|---|---|---|---|
|SAETTI-01|作者模型|可变数量旋翼/机翼、可选入流/挥舞复杂度，允许旋翼间及旋翼—机翼干扰|Simulation Models；没有独立开/关干扰对照或通用干扰系数|
|SAETTI-02|作者方法|令快状态导数为零，保留消元后的耦合修正，得到8状态低频模型|式10–17；Af稳定/可逆前提，式14/16记号有疑点|
|SAETTI-03|作者方法|TR预设倾角配平；TMR/TW固定零俯仰反求倾角；TMR2不能各速度零俯仰配平，改预设倾角|Trim段、图8；不是全构型共同安全走廊|
|SAETTI-04|作者控制|5状态姿态内环、3状态速度外环，反馈PI/PID增益共用而逆模型按速度调度|式18–44、图2–4、表5–8；每20kt离线计算，线上插值；无全域求逆条件数保证|
|SAETTI-05|模型对照|TR在悬停及170kt等条件与试飞辨识及文献模型比较；约简与全阶响应接近|图5–7；部分试飞状态明显不吻合，真实试验配置信息不完整，非全模型高精度界|
|SAETTI-06|频域仿真|展示悬停各轴裕度、DRB/DRP和指令跟踪，采用4dB/45°标准，正文称总体满足且承认例外|图10–12；TR俯仰、TMR2滚转及部分扰动指标在图示合格域外；非全包线验收|
|SAETTI-07|闭环仿真|四构型进行向160kt加速及转弯爬升转换，前向速度跟踪较好|图13–16；文字60s/90°与曲线时间轴/航向终值不一致，只保留限定趋势|
|SAETTI-08|作者限制|结论明确研究探索性，未考虑详细控制权限、执行器约束与结构载荷，需驾驶员参与及鲁棒性评价|Conclusions；不能据批仿真声称可直接工程部署|

## 工程建议和未知项

建议把名义配平、局部矩阵、状态约简、参考模型、反馈补偿和混控接口分别保存。保留低频模型并不等于简单删除快状态，须检查消元耦合以及约简前后低频响应。以上是读者建议，未提升为跨来源方法。

原文未公开全部矩阵/混控关系、具体气动数据库、执行器饱和/速率处置、奇异退化后备、闭环控制实飞或机载计算耗时。100Hz与各50ms处理/保持延迟的关系未说明；式29/31b垂向符号、式16输出记号、式25后单块求逆等须先复核。完整问题见核对稿。

## 阅读与验证入口

- [完整中文阅读](</D:/666/学习资料/倾转旋翼综合设计方法论_打包_20260628_203550/文献/倾转过渡路径设计_近十年_2016-2026/PDFs/2025_Saetti_Automatic_Transition/衍生阅读/倾转旋翼及倾转机翼飞行器自动转换的动态逆飞行控制律_完整中文阅读版.md>)
- [定向分析](</D:/666/学习资料/倾转旋翼综合设计方法论_打包_20260628_203550/文献/倾转过渡路径设计_近十年_2016-2026/PDFs/2025_Saetti_Automatic_Transition/衍生阅读/定向分析.md>)
- [公式图表核对](</D:/666/学习资料/倾转旋翼综合设计方法论_打包_20260628_203550/文献/倾转过渡路径设计_近十年_2016-2026/PDFs/2025_Saetti_Automatic_Transition/衍生阅读/公式图表核对稿.md>)

全部21来源页和16裁图已视觉核对，公式及8表逐项重建，编号和参考完整。结构检查不证明源文科学模型正确；此卡L1/pending_review。作者勘误、公开实现或新实测资料改变上述边界时需重新核验。

## 知识增量

- delta_route：obs-candidate。
- delta_operation：create。
- 增量：低频状态消元与控制设计链、构型相关配平未知量、反馈增益固定与对象逆调度的区别。
- 无MATLAB/Simulink实施，无L2/L3升格。
