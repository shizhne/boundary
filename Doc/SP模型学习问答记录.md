# SP 模型学习问答记录

> 本记录用于当前 VE25SP 模型的学习问答与后续学习进度核对。工程权威来源为 `D:/666/VE25/sp`；本记录是学习材料，不替代模型、需求或正式设计说明。当前条目为有来源的学习候选，尚未取得用户对掌握程度的反馈。

## 记录约定

- 问题使用稳定编号；问题处理状态为“待解答、部分澄清、已解答、待核实”。修正结论时保留修订说明。
- 学习表现单独记录。没有用户反馈或展示时写“掌握程度未确认”；提供答案、阅读材料和问题数量均不证明阶段完成。
- 阶段关联使用[学习计划](D:/666/VE25/Doc/SP模型学习与维护进阶计划.md)的实际 S1—S6 任务与验收项，兼容第 5.2、5.3、7.8 节。可关联多个阶段；无对应条目时标为“拓展”。
- 日后用户要求核对进度时，按“计划任务—关联问题编号—掌握/成果证据—缺口—下一步”汇总。S6-A 方案级与 S6-B 获准实践级分别判断；当前不计算完成率或勾选计划。
- 主线校准：用户已明确 SpMdl 是不再维护的旧版，当前学习以 VE25SP 及其真实引用链为主。计划第 2 节、S1 和第 7.8 节仍含旧的主线待确认表述，相关条目待校准；本记录不据此评价进度，也不改写计划。

## 问题索引

| 编号 | 日期 | 主题/模块 | 关联阶段 | 问题处理状态 | 学习表现 |
|---|---|---|---|---|---|
| Q001 | 2026-09-15 | VE25SP 输入及消息命名：sp、msg、rms、ADAHRS、FTADC | S1；关联 S2 的输入定位准备 | 部分澄清 | 掌握程度未确认 |
| Q002 | 2026-09-15 | AdahruMsgsPreprocess 等模块名称拆解与引用定位 | S1、S2；关联 S4 的模块定位准备 | 部分澄清 | 掌握程度未确认 |
| Q003 | 2026-09-15 | 当前主线常见缩写词表：模块、设备、物理量、状态和单位 | S1、S2、S3、S4 的术语准备 | 部分澄清 | 掌握程度未确认 |
| Q004 | 2026-09-15 | DRMS 的接口含义、双向消息及词表遗漏修正 | S1、S2 的接口识别准备 | 部分澄清 | 掌握程度未确认 |
| Q005 | 2026-09-16 | Calc_SignalState_Double：拍、三路输入、失配监控与锁存禁用 | S1、S3；关联 S4 导航输入 | 部分澄清 | 掌握程度未确认 |
| Q006 | 2026-09-16 | SpIn_ADAHRS 的 L+数字命名与数据标签 | S1、S2 的输入接口识别准备 | 部分澄清 | 掌握程度未确认 |
| Q007 | 2026-09-16 | Label 编号表查询、For Each 状态组装与状态位描述差异 | S1、S2；关联 S3 状态编码 | 部分澄清 | 掌握程度未确认 |
| Q008 | 2026-09-16 | 总线字段 Dimensions=1 与三路总线数组的层级区别 | S1、S2；关联 S3 For Each | 已解答 | 掌握程度未确认 |
| Q009 | 2026-09-16 | For Each 如何收集并拼接三份总线结果 | S2；关联 S3 分区与输出机制 | 已解答 | 掌握程度未确认 |
| Q010 | 2026-09-16 | For Each 控制块、内建迭代机制与特殊外观 | S2、S3 | 已解答 | 掌握程度未确认 |
| Q011 | 2026-09-16 | 创建和配置 For Each Subsystem | S2；关联 S3 分区与总线输出 | 已解答 | 掌握程度未确认 |
| Q012 | 2026-09-16 | 四状态位编码的原理、需求依据与位布局差异 | S3；关联 S2 状态接口 | 部分澄清 | 掌握程度未确认 |
| Q013 | 2026-09-16 | NormalInfos 的含义、结构及预处理到选源的连接 | S1、S2 | 已解答 | 掌握程度未确认 |
| Q014 | 2026-09-16 | ICD 的含义与接口约定的作用 | S1、S2；关联 S3 状态接口 | 已解答 | 掌握程度未确认 |
| Q015 | 2026-09-16 | 当前工作区 ICD 检索结果与现有接口资料 | S1、S2；关联 S3 状态接口 | 已解答（检索范围内未发现正式 ICD） | 掌握程度未确认 |
| Q016 | 2026-09-16 | Voter 的 Switch Case Action Subsystem：特点、分支与创建方法 | S2、S3 | 已解答（静态核对与操作说明） | 掌握程度未确认 |
| Q017 | 2026-09-17 | FCC_SP_HLR_00055 的需求文档出处追踪 | S5；关联 S2 选源链 | 待核实（原始文档未定位） | 掌握程度未确认 |
| Q018 | 2026-09-17 | ADAHRU 是否可能是具体导航产品的名称或型号 | S1、S2；关联 S5 设备接口依据 | 部分澄清（项目产品身份待确认） | 掌握程度未确认 |
| Q019 | 2026-09-17 | 两路 RA 为何接入三路一致性检查及输出三元素数组 | S2、S3；关联 S4 RA 选源 | 已解答（静态核验） | 掌握程度未确认 |
| Q020 | 2026-09-17 | tSingleData 的命名拆解与实际类型定义 | S1、S2 | 部分澄清（t 前缀正式定义待确认） | 掌握程度未确认 |
| Q021 | 2026-09-18 | 一拍 Delay 的执行、Level hold 复位与使用场景 | S2、S3 | 部分澄清（具体实例待定位） | 掌握程度未确认 |
| Q022 | 2026-09-18 | 学习进度评估：阶段覆盖、已观察表现与下一步验收 | S1—S6 | 已解答（本任务记录范围内评估） | 局部学习表现已观察，阶段掌握待验收 |
| Q023 | 2026-09-18 | MCU、SCU 的设备含义与反馈接口 | S1、S2；关联 S4 设备反馈 | 部分澄清（正式英文全称待确认） | 掌握程度未确认 |
| Q024 | 2026-09-18 | MCU 输入层级、字段与两级有效性的实际作用 | S2、S3、S4 | 已解答（结构与处理；码值及单位待确认） | 掌握程度未确认 |
| Q025 | 2026-09-20 | MCU 的 7 个字段与 11 元素数组及索引分组 | S2、S3 | 已解答（静态核验） | 掌握程度未确认 |
| Q026 | 2026-09-20 | MCU For Each Subsystem 的图标、分区与结果组合 | S2、S3 | 已解答（静态核验） | 掌握程度未确认 |
| Q027 | 2026-09-20 | MCU 与 FTADC 的公共监控逻辑及实例差异 | S2、S3、S4 | 已解答（静态核验） | 掌握程度未确认 |
| Q028 | 2026-09-20 | SCU 输入五字段、舵面映射与位置／故障状态输出 | S2、S3、S4 | 已解答（结构与实现；输入码表及单位待确认） | 掌握程度未确认 |
| Q029 | 2026-09-20 | MPIO 综合输入的业务分组、处理与两层有效性 | S2、S3、S4 | 已解答（结构与实现；单位及完整码表待确认） | 掌握程度未确认 |
| Q030 | 2026-09-21 | 学习进度更新：设备反馈／MPIO展开与下一步闭环 | S1—S6 | 已解答（本任务可见证据范围内评估） | 局部学习表现已观察，阶段掌握待验收 |
| Q031 | 2026-09-23 | 需求／测试文档中的STS与SRS缩写 | S5 | SRS本地定义已确认，STS全称待确认 | 区分通用释义与具体文档定义 |
| Q032 | 2026-09-23 | ADA在当前文档语境中的含义 | S5 | 待确认具体原文；已解释Ada语言候选 | 不将通用名称当作项目定义 |
| Q033 | 2026-09-23 | 当前VE25SP的Bus结构文档 | S1/S2/S4 | 已生成，静态核验通过 | 按链路查接口与嵌套定义 |
| Q034 | 2026-09-23 | RA输入、预处理、选源与控制律输出主链 | S1/S2/S3/S4 | 主链静态核验；计数语义待复核 | valid选源、源号保持与reset事件 |
| Q035 | 2026-09-23 | GCS含义与Calc_GCSData作用 | S1/S2/S3/S4 | 已静态核验 | 两组独立选源、有效性与reset封装 |
| Q036 | 2026-09-23 | 功能梳理第3章补充信号术语解释 | S1/S2/S4 | 已修改原文并核验 | 17模型与9根输出中文释义 |
| Q037 | 2026-09-23 | RemoteFMCP与FMCP全称 | S1/S2 | 行业释义确认；项目映射有依据 | 飞行模式控制面板的远程输入数据 |
| Q038 | 2026-09-23 | VDT_Stick与Dis_Key六个输入 | S1/S2 | 结构与连线确认；硬件映射待确认 | 六项杆位、十四项按键的三路输入 |
| Q039 | 2026-09-23 | MPIO复核与历史接口名称集中整理 | S1/S2/S4/S5 | 用途与接口依据确认；全称待确认 | 按功能链集中查阅此前名称 |
| Q040 | 2026-09-24 | Curr/Left/Right分路与设计原因追溯 | S1/S2/S4/S5 | 实现约定确认；历史动机与整机映射待补证 | 区分相对通道、绝对编号和物理来源 |
| Q041 | 2026-09-24 | 同源操纵信息与三路选源/比较表决 | S1/S2/S4 | 软件处理确认；物理独立性待补证 | 区分GCS选源、VDT比较和源头共同错误 |
| Q042 | 2026-09-24 | GCS/Sys_Status通道标签与FCC身份关系 | S1/S2/S4 | 选源映射确认；端到端来源待补证 | 区分数据类别、逻辑通道和物理来源 |
| Q043 | 2026-09-24 | VDT/Dis_Key采集通道与ADA/RA设备来源 | S1/S2/S4 | FICD补证；上游映射和构型对应待核 | 区分采集通道、设备源号及RA占位 |
| Q044 | 2026-09-24 | FCC采集操纵量与GCS报文来源标签 | S1/S2/S4 | 采集背景与协议源节点确认；端口填充待核 | 区分采集、接收、协议源ID和选源编号 |
| Q045 | 2026-09-24 | handleGCS的SSX/SSY与CP/FO含义 | S1/S2/S4 | 中文业务定义和当前连线确认 | 区分侧杆操纵轴、接口位置和姿态角 |
| Q046 | 2026-09-24 | handleGCS完整处理及GcsData/MpioData共67项成员 | S1/S2/S3/S4 | 当前结构和静态处理已解答，部分协议/构型待确认 | 掌握程度未确认 |
| Q047 | 2026-09-24 | VdtCBIT位编码、检查范围与下游作用 | S1/S2/S3/S4 | 当前实现已解答 | 掌握程度未确认 |
| Q048 | 2026-10-08 | 学习进度更新：操纵接口、来源追溯与系统健康入口 | S1—S6 | 已评估（本任务证据范围） | 局部分析表现已观察，阶段掌握待验收 |
| Q049 | 2026-10-08 | 顶层浅紫色输出区域及Area/模型引用区别 | S1/S2 | 已解答（静态结构确认） | 掌握程度未确认 |
| Q050 | 2026-10-08 | MPIO发送设备、节点与原始数据来源 | S1/S5 | 已解答（设备类别与协议节点确认，上游路径待核） | 掌握程度未确认 |
| Q051 | 2026-10-10 | SP在飞控系统中的定位与上下游边界 | S1/S2/S5 | 已解答（静态接口/职责确认，实际集成待核） | 掌握程度未确认 |
| Q052 | 2026-10-10 | MPIO业务输入与SYS健康/输出接口的分层区别 | S1/S2/S4 | 已解答（当前结构与消费链静态确认） | 掌握程度未确认 |
| Q053 | 2026-10-12 | isSynced独立字段与stateBits同步位的双重表示 | S2/S3/S4 | 已解答（同源及消费者确认，历史动机待核） | 掌握程度未确认 |
| Q054 | 2026-10-14 | errorCnt的Delay反馈、动态限幅与复位计数机制 | S2/S3 | 已解答（计数支路静态确认，运行时触发边界待核） | 掌握程度未确认 |
| Q055 | 2026-10-14 | Update保持数据与异常计数Delay的区别 | S2/S3 | 已解答（端口、连线及调用关系静态确认） | 掌握程度未确认 |
| Q056 | 2026-10-16 | chUsed参与组合编码与外层Terminator终止 | S2/S3 | 已解答（当前表决模型静态确认） | 掌握程度未确认 |
| Q057 | 2026-10-16 | SP_SW_HLR-673传感器检查需求的当前/旧模型定位 | S3/S4 | 已定位（静态核对，符合性未判定） | 掌握程度未确认 |
| Q058 | 2026-10-16 | 紫色区域模块的输出生成与综合判定定位 | S1/S4 | 已解答（职责及连接静态确认） | 掌握程度未确认 |
| Q059 | 2026-10-16 | From[max]门限路由及连续有效确认回路 | S2/S3 | 已解答（路由及同源比较参数确认） | 掌握程度未确认 |
| Q060 | 2026-10-16 | 紫色区域六模型及SP2DRMS的处理链与输出意义汇总 | S3/S4 | 已汇总（主要输入组及处理链静态核对） | 掌握程度未确认 |
| Q061 | 2026-10-16 | HiVltgSW.data/valid的请求码与记录有效性 | S2/S3 | 已解答（模型及FICD码表确认） | 掌握程度未确认 |
| Q062 | 2026-10-16 | Baro气压相关术语与气压高度 | S1 | 已解答（术语及字段确认） | 掌握程度未确认 |
| Q063 | 2026-10-16 | SP/CCDL缩写与SpCcdlMsgOut名称拆解 | S1 | 已解释（当前CCDL命名语境） | 掌握程度未确认 |
| Q064 | 2026-10-16 | 故障相关处理模块、状态类别与传递关系 | S3/S4 | 已定位（引用链及关键连接静态核对） | 掌握程度未确认 |

## Q001：信号名称中的 sp、msg、rms、ADAHRS、FTADC 分别是什么意思？

### 日期、阶段与具体条目

- 日期：2026-09-15，Asia/Shanghai；记录修订：R002。
- S1：对应第 4 章“画出导航、设备反馈、MPIO、地面站、驾驶员输入、系统状态、输出组织七类功能的数据关系”，本题仅为术语和输入来源识别准备。相关验收为“能够从输入来源讲到输出消费者”。
- S2：关联“在选定主线中定位输入、处理模型、组包模型及输出”，仅涉及 ADAHRS/FTADC 输入名称；没有完成 CAS 全链路或故障场景推演。
- 第 7.8 节映射参考：“SP 输入、处理和输出如何组织？”对应 S1、S2；所有定位以当前 VE25SP 为准。

### 简明结论与确认程度

| 缩写 | 解释 | 当前模型中的含义与依据 | 子项状态 |
|---|---|---|---|
| SP / Sp / sp | Signal Processing，信号处理 | 本工程的信号处理功能。`SpIn_ADAHRS` 可读作“信号处理模块的 ADAHRS 输入”。需求 SP_SW_HLR-2 明确本软件为信号处理软件；当前顶层接口见 P1。英文为常用展开，不声称需求缩略语表逐字定义了 SP。 | 已解答 |
| Msg / msg | Message，消息/报文；Msgs 是复数 | 用于表示一组传递的数据。`RmsPublicMsg` 是总线类型输入，`SPDispMsgOut` 是总线类型输出；仅凭 Msg 不能确定传输协议或报文发送周期。见 P1、P4。 | 已解答 |
| RMS / Rms | 从接口语境推测为冗余管理模块；常见行业展开是 Redundancy Management System（冗余管理系统） | `VE25SP/RmsPublicMsg` 的类型是 `BUS_DRMS2SP_MSG`，包含 `vtolState`、`fccId`、`activeFccId`、`isFccExist`。这些字段支持冗余/主用飞控计算机管理的解释，但字典 Description 为空；项目是否正式采用 System、Software 等展开尚未核实；DRMS 中的 D 已由用户确认为 Device（设备），来源见 Q004 的 U-DRMS-20260915。见 P1、P4、W2。 | 待核实：正式英文全称 |
| ADAHRS | Air Data, Attitude and Heading Reference System，大气数据、姿态与航向参考系统 | 当前 `SpIn_ADAHRS` 的类型为 `SpIn_Adahrus`，字段涵盖气压高度、空速、航向、俯仰/滚转角、加速度与角速度等。行业全称见 W1；本项目字段见 P1、P4。 | 已解答 |
| FTADC | 当前模型中的大气数据设备输入；ADC 的行业含义是 Air Data Computer，大气数据计算机 | `SpIn_ftadc` 包含 `Psi`、`Pti`、`AOA`、`AOS`、`CAS`、`Valid`。参数脚本把 FTADC 与 ADAHRU 列为大气信号来源；所查项目资料和公开来源未找到 FTADC 的明确全称。FT 是否为 Flight Test（试飞）仅为候选解释，不能确认为“试飞大气数据计算机”。见 P4、P5、P6、W3。 | 待核实：FT 的展开及设备正式名称 |

### 已确认的静态证据

- `VE25SP` 在 `simulink/blockdiagram.xml` 中指定 `DD_VE25SP.sldd`；该字典引用 `SP_ALL.sldd`，后者引用 `SP_Step_New.sldd`。这是此次接口字段核对使用的真实引用链，未加载模型或执行初始化。
- 顶层 `SpIn_ADAHRS` 为 SID 4，`RmsPublicMsg` 为 SID 68、端口 10，`SpIn_FTADC` 为 SID 359、端口 11；输出 `SP2DRMS` 为 SID 458，类型 `BUS_SP2DRMS_MSG`。
- `VE25SP/NavSpApp/Model1` 为 SID 71，在 `simulink/systems/system_54.xml` 中引用 `Calc_VotedFTADC.slx`。
- `BUS_SP2DRMS_MSG` 包含 `navValid`、`isBfccActvValid`、`isBfccActv`。字段存在是已确认事实，字段的完整工程语义及 RMS 全称仍需接口文件支持。
- `SpCPT_define.m` 第 6、8 行分别将 FTADC/ADAHRU 列为 CAS/TAS/BARO 和 AOA/AOS 的输入来源。这是脚本声明与注释，未核实运行时实际选源。

### 读取来源与版本

工程读取基线：`AC102_Rebuild`，提交 `a895a43a19209c076e679fcc2e2f8953e9f49bb4`。本次来源均只读；写入前 Git 状态仅有两处既有 `resources/project` XML 修改。以下为原文件 SHA-256，不把内存解析结果当成独立 primary 来源。

| ID | 来源与精确锚点 | SHA-256 / 版本 |
|---|---|---|
| P1 | [VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)：`system_root.xml` 的 SID 4、68、359、424、458；`system_54.xml` SID 71；`blockdiagram.xml` 的 DataDictionary | `20D8C7DE2A103D8FF298BCD5BB0C28F73AA6CCC2E27D43D0F5AB9952365F77E4` |
| P2 | [DD_VE25SP.sldd](D:/666/VE25/sp/04_Design/VE25SP/specification/data/DD_VE25SP.sldd)：`data/chunk0.xml` 的 Subdictionary | `9BCDC82722DE11771CF1C1665544200829C9AD50DC2C69BF2EBF1D46F313B93E` |
| P3 | [SP_ALL.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_ALL.sldd)：`data/chunk0.xml` 的 Subdictionary | `4823DA964D265B93AE39A59A9D1954E8A88AE3F440C55E151D548602219544DC` |
| P4 | [SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)：`data/chunk0.xml` 的 `SpIn_Adahrus`、`SpIn_ftadc`、`BUS_DRMS2SP_MSG`、`BUS_SP2DRMS_MSG` | `10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC` |
| P5 | [SpCPT_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPT_define.m:6)：第 6、8 行选源注释 | `49AAF3CBA343CA4B8E1A4C4B8959B1BBD4824C56378C36B61B74119F5DB218B0` |
| P6 | [SP_SW_HLR.slreqx](D:/666/VE25/sp/02_Requirements/specification/SP_SW_HLR.slreqx)：`slrequirements/data.xml` 的 SP_SW_HLR-2、-4、-778、-755、-279。缩略语表 -778 没有解释 RMS 或 FTADC；需求适用性仍以 -4 所述版本边界为限。 | `D48948EB3E54C2129D0C18B4C80DCF0628597F3621EEC78BC0EBC5A8BCC4338D` |
| P7 | [SP 模型学习与维护进阶计划](D:/666/VE25/Doc/SP模型学习与维护进阶计划.md:194)：S1、S2，第 5.2、5.3、7.8 节 | `8D3F164BF800D48DD311D01C9BD5D03517FD971433DA3E09EA915B3C623AB27B` |
| W1 | [Innovative Aerosystems：ADAHRS 产品说明](https://iascorp.com/products/adahrs/)：标题及首段的行业全称 | 访问日期 2026-09-15；只用于通用术语，不认定为本项目设备型号 |
| W2 | [Redundancy Management of Onboard Equipment](https://journals.rcsi.science/1819-3161/article/view/350919)：摘要中 RMS 的展开 | 访问日期 2026-09-15；只证明一种行业用法，不证明项目正式展开 |
| W3 | [SFTE Flight Test Engineering Reference Handbook](https://society-of-flight-test-engineers.github.io/quarto-handbook/01-general.html)：缩略语 ADC = air data computer | 访问日期 2026-09-15；不能据此确认 FT 的含义 |

### 待确认、学习表现与下一步

- Q001-U1：RMS 的项目正式英文全称。待有适用接口定义或用户/责任人明确说明时补齐。
- Q001-U2：FTADC 的 FT 展开及设备正式名称；“Flight Test”目前只是候选解释。
- 学习表现：**掌握程度未确认**。尚无用户确认理解、独立复述/定位、场景推演或实际验证的证据。
- 本次成果：五项缩写解释表、顶层接口与字典来源锚点；未形成全链路图、故障场景或逐拍推演。
- 阶段验收：本题不足以判断 S1 或 S2 完成；未勾选第 5.3 节，也未评估 S6-A/S6-B。
- 下一次最小任务：按用户后续问题，选择一个实际输入名称拆解并定位其总线定义；若提供 RMS/FTADC 的明确来源，先修订 Q001 的对应待确认项。
- 记录范围：只维护本问答文件；本题过程未向 `sp` 写入任何文件，未执行模型更新、仿真或代码生成。知识路由为 `project-only`，不自动发布到项目 KB 或其他目录。

### 修订记录

- R001（2026-09-15）：首次记录；整体为“部分澄清”，保留两个术语全称待确认项。回答与记录动作完成不等于所有全称已获确认或用户已掌握。
- R002（2026-09-15）：根据本任务用户明确说明，补充 DRMS 的 D = Device（设备），并指向 Q004 的用户来源；RMS 和 FTADC 其余待核实项保留。

## Q002：AdahruMsgsPreprocess 等模块名称怎样拆解？

### 日期、阶段与问题状态

- 日期：2026-09-15，Asia/Shanghai；修订：R001；主题：当前 VE25SP 引用模型的名称与功能导航。
- 问题状态：部分澄清。名称结构与主要引用关系已说明；部分设备缩写的项目正式英文展开仍待确认。
- S1：对应第 4 章“画出导航、设备反馈、MPIO、地面站、驾驶员输入、系统状态、输出组织七类功能的数据关系”；本题提供名称与功能入口对照。
- S2：对应“在选定主线中定位输入、处理模型、组包模型及输出”；本题确认 ADAHRS 预处理到源选择的一段连接，并定位其内引用的 CAS 计算模型。
- S4：关联“为每个模块填写第 5 节的模块说明卡”和“汇总公共库、接口总线和共享参数的调用关系”的前置定位；没有完成模块说明卡或变更影响分析。
- 阶段关联沿用学习计划第 5.2、5.3、7.8 节；仅名称解释和局部连线不足以通过 S1、S2 或 S4 验收。

### 重点名称：AdahruMsgsPreprocess

`AdahruMsgsPreprocess = Adahru + Msgs + Preprocess`，可读作“ADAHRU 报文预处理”。

- ADAHRU 的常见行业展开为 **Air Data, Attitude and Heading Reference Unit**，即“大气数据、姿态与航向参考单元”。Meggitt 署名产品说明使用该展开，见 W4。U 表示 Unit（单元）；前题 ADAHRS 的 S 表示 System（系统）。这里引用的是行业用语，不认定本项目采用该厂商设备。
- Msgs 是 Messages 的简写，表示多条消息/报文；Preprocess 表示预处理。
- 已确认的当前模型接口：`AdahruMsgsPreprocess/SpIn_ADAHRS`（SID 4）输入，`AdahruMsgsPreprocess/adahruNormalInfos`（SID 139）输出。
- 在 `VE25SP/NavSpApp` 中，SID 55 输入连接至 SID 25 的预处理模型；SID 25 的输出 `adahruNormalInfos` 分支接到 SID 44 的 `AdahruVoter`，该块实际引用 `Calc_NavSourceSelection.slx`。此段是静态数据连接，不代表执行调度顺序，也未据名称断言预处理一定包含滤波、解码或全部异常监控。

### 常见名称片段

| 片段 | 英文/中文释义 | 阅读用途 |
|---|---|---|
| Calc | Calculate / Calculation，计算 | 一项计算或数据处理 |
| Preprocess | 预处理 | 后续处理之前的准备步骤 |
| Voter / Voted | 表决器 / 经表决的 | 提示表决或选源相关职责；具体算法需回到实现 |
| Nav / NAV | Navigation，导航 | 导航数据或功能 |
| Source / Src / Selection | 来源 / 来源的缩写 / 选择 | 例如 SourceSelection 为源选择 |
| PilotIn | Pilot Input，驾驶员输入 | 操纵与驾驶员输入相关接口 |
| Sys / Info / Data | System / Information / Data，系统 / 信息 / 数据 | 数据所属对象及内容类别 |
| ToClaw | To Control Law，送往控制律 | 面向控制律的接口组织 |
| Disp / Out / In | Display / Output / Input，显示 / 输出 / 输入 | 用途和流向 |
| Mdl / App | Model / Application，模型 / 应用 | 例如 NavSpApp 为导航信号处理应用 |
| Msg / Msgs | Message / Messages，消息 / 多条消息 | 见 Q001；名称不指定通信协议 |

### 当前主线中的常见模型名称

下表提供名称释读与功能定位。除特别列出的端口和引用外，未开展各模型内部算法审计。

| 模型名称 | 如何理解 | 证据锚点 |
|---|---|---|
| AdahruMsgsPreprocess | ADAHRU 报文预处理 | P1：NavSpApp，SID 25；P8 |
| RAMsgsPreprocess | 无线电高度计/雷达高度报文预处理；RA 用于高度测量语境 | P1：NavSpApp，SID 46；P10；P6 的 SP_SW_HLR-255、-278 |
| Calc_NavSourceSelection | 导航源选择计算 | P1：NavSpApp/AdahruVoter，SID 44；P9 |
| Calc_VotedCAS | 校准空速（CAS，Calibrated Airspeed）的表决相关计算 | P9：Model，SID 529；P6 的 SP_SW_HLR-532；W5 |
| Calc_VotedRA | 无线电高度的表决/选源相关计算 | P1：NavSpApp/raVoter，SID 45 |
| Calc_VotedFTADC | FTADC 大气数据处理；FT 展开仍见 Q001-U2 | P1：NavSpApp/Model1，SID 71 |
| Calc_MCUSCUData | MCU、SCU 设备反馈数据处理；本项目需求把 MCU 对应到电机反馈、SCU 对应到舵面执行机构反馈 | P1：顶层 Model2，SID 364；P11；P6 的 SP_SW_HLR-345、-357、-368、-369 |
| Calc_MPIOData | MPIO 接口数据处理；MPIO 的项目正式英文展开待核实 | P1：顶层 Model，SID 362；P12 |
| Calc_GCSData | 地面站（GCS，通常称 Ground Control Station）数据处理 | P1：顶层 Model3，SID 370；P6 的 SP_SW_HLR-761 提供地面站功能语境 |
| Calc_PilotInData | 驾驶员输入数据处理 | P1：顶层 Model7，SID 388 |
| Calc_SysInfo | 系统信息处理 | P1：顶层 Model1，SID 363 |
| Calc_NAVMsg_ToClaw | 面向控制律的导航消息处理/组织 | P1：顶层 Model4，SID 375 |
| Calc_PilotIn_ToClaw | 面向控制律的驾驶员输入处理/组织 | P1：顶层 Model5，SID 376；P6 的 SP_SW_HLR-496 |
| Calc_SYSMsg_ToClaw | 面向控制律的系统消息处理/组织 | P1：顶层 Model6，SID 377 |
| Calc_FMSMsg_ToClaw | 面向控制律的 FMS 接口消息组织；本题仅核对其输入 MpioData、输出 Claw_FMS_In_Msg | P1：顶层 Model8，SID 397；P14 |
| Calc_CcdlMsg_Out | CCDL 通道间数据链消息输出组织；行业展开为 Cross-Channel Data Link | P1：顶层 Model9，SID 406；P13；W6 |
| Calc_SPDispMsg_Out | SP 显示消息输出组织 | P1：顶层 Model10，SID 408；输出 SPDispMsgOut，SID 424 |

MCU 常见候选展开为 Motor Control Unit；SCU 在此设备语境下可按伺服/舵面控制单元理解，但未找到项目正式英文词表，因此不把 Servo Control Unit 或 Surface Control Unit 中的任一种写成已确认展开。MPIO 和 FMS 的具体项目释义也保留接口文档核对项。

### 一个实用的定位规则

**图上块名和引用模型名分别核对。** 当前 `VE25SP/NavSpApp/AdahruVoter` 引用 `Calc_NavSourceSelection.slx`；该模型内部的 `Model` 又引用 `Calc_VotedCAS.slx`。不能只按图上块名去猜文件路径。同样，Voter/Voted 只提示表决或选源主题，不能据此确定采用中值、平均值、优先级或其他算法。

### 新增来源与版本

复用 Q001 的工程基线：`AC102_Rebuild`、提交 `a895a43a19209c076e679fcc2e2f8953e9f49bb4`。P1 的 SHA-256 本轮复核未变化；P6 仍作为有适用边界的需求来源，不视作当前全部行为已获验证。P8—P14 的端口锚点均位于各 SLX 包内 `simulink/systems/system_root.xml`。

| ID | 来源与精确锚点 | SHA-256 / 来源性质 |
|---|---|---|
| P8 | [AdahruMsgsPreprocess.slx](D:/666/VE25/sp/04_Design/AdahruMsgsPreprocess/specification/AdahruMsgsPreprocess.slx)：输入 SID 4、输出 SID 139 | `A5F9E7FAC266448447F929CF4BDC5CFF6C9B322A81D479995C61799977E65143` |
| P9 | [Calc_NavSourceSelection.slx](D:/666/VE25/sp/04_Design/Calc_NavSourceSelection/specification/Calc_NavSourceSelection.slx)：输入 SID 4、521；输出 SID 523—526；ModelReference SID 529 | `BE517B33C48086D7B6173C32205CEE0F3C77AFE5D24BE37914388EF6A80143DE` |
| P10 | [RAMsgsPreprocess.slx](D:/666/VE25/sp/04_Design/RAMsgsPreprocess/specification/RAMsgsPreprocess.slx)：输入 RAIn SID 4、输出 raNormalInfos SID 20 | `6FDCBEAD97A9EB0023601C9D44E2A23EB9768DA8814910681943E5E826815213` |
| P11 | [Calc_MCUSCUData.slx](D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/Calc_MCUSCUData.slx)：输入 SID 5、108；输出 DevData SID 223 | `0892EDC640482881BAE00D55D151F23181CA1027A479F6AC0B6F51E9E9978A04` |
| P12 | [Calc_MPIOData.slx](D:/666/VE25/sp/04_Design/Calc_MPIOData/specification/Calc_MPIOData.slx)：输入 SID 4、输出 SID 113 | `A5B051AE97E92967CCF9E101D137D566B78DC55C57FC90B78910BA7F841F2FE6` |
| P13 | [Calc_CcdlMsg_Out.slx](D:/666/VE25/sp/04_Design/Calc_CcdlMsg_Out/specification/Calc_CcdlMsg_Out.slx)：输入 SID 269—275；输出 SID 279—281 | `A7FBAC6DFFE00A2EB2B838E0F65B9312D04260E5820BD39CE02A3CA698B40DF8` |
| P14 | [Calc_FMSMsg_ToClaw.slx](D:/666/VE25/sp/04_Design/Calc_FMSMsg_ToClaw/specification/Calc_FMSMsg_ToClaw.slx)：输入 MpioData SID 4、输出 Claw_FMS_In_Msg SID 21 | `8AFD3DAFFF28413E431F93C6924235202687E863F70C3981F42A52C7253A6F70` |
| W4 | [Meggitt Avionics 的 ADAHRU 产品说明（AeroExpo 托管）](https://www.aeroexpo.online/prod/meggitt-avionics/product-184912-46266.html)：Description 的完整展开；正文以制造商口吻发布 | 2026-09-15 读取；仅作行业术语依据，不认定项目设备厂商 |
| W5 | [FAA 航空缩略语表](https://www.faa.gov/air_traffic/publications/atpubs/cnt_html/chap2_section_1.html)：CAS = calibrated airspeed | 2026-09-15 读取；语义选择结合项目需求和接口 |
| W6 | [Curtiss-Wright MCDL-202A-1 产品说明](https://defense-solutions.curtisswright.com/products/flight-test/data-acquisition/ttcmdas/bus/mcdl202a1)：Cross Channel Data Link (CCDL) | 2026-09-15 读取；不据此推定本项目协议、硬件或发送周期 |

### 学习表现、待确认与下一步

- 学习表现：**掌握程度未确认**。本轮用户继续询问名称，不构成已能独立拆解、定位或复述的证据。
- 成果：拆词表、17 个当前主线模型名称对照、预处理到源选择的一段静态连线核对；未完成完整信号链、场景推演或实际运行验证。
- Q002-U1：MCU、SCU、MPIO、FMS 的项目正式英文名称与设备/软件边界，待适用接口说明或责任人确认；不自动扩展为设备完整职责。
- Q002-U2：各 Preprocess/Voter 模型的实际监控、归一化与表决算法不在本题命名解释范围，后续按用户指定对象查阅。
- 下一次最小任务：若用户继续选择 `AdahruMsgsPreprocess`，再围绕一个输入通道解释 `adahruNormalInfos` 的字段和生成逻辑；当前不自主展开。
- 验收：记录内容、编号与链接进行静态检查；S1、S2、S4 掌握程度未确认，未勾选学习计划。工程保持只读，过程文件未进入 `sp`。
- Delta-K route：`project-only`。本轮只更新本问答文件；Q001 的结论及未决事项保留。

### 修订记录

- R001（2026-09-15）：新增模块名称拆解与来源锚点；问题状态“部分澄清”，学习表现“掌握程度未确认”。

## Q003：当前主线常见缩写词表（含 RA、Dev 及其他命名片段）

### 日期、范围与学习计划关联

- 日期：2026-09-15，Asia/Shanghai；修订：R003；问题状态：部分澄清，通用命名已解释，项目专用缩写仍有待核实项。
- 用户要求：主动覆盖当前模型中的常见缩写，不局限于用户列举或上一题的完整模块名称。
- 核对范围：从 VE25SP 出发的真实 ModelReference 引用链，共 18 个模型（含顶层）；读取模型名称、端口、相关信号/标签，以及已引用的 SP_Step_New、SP_InteriorBus 等字典。没有把独立旧版 SpMdl 的模型组织作为当前主线。
- S1：对应第 4 章“画出导航、设备反馈、MPIO、地面站、驾驶员输入、系统状态、输出组织七类功能的数据关系”的术语与入口准备。
- S2：对应“记录各关键点的数值、类型、单位、有效性、更新信息和历史状态”的字段阅读准备。
- S3：关联“阅读公共库中的 TimeoutMonitor、AbnormalMonitor、RangeMonitor、CmpMonitor、Voter 和状态设置模块”的名称识别。
- S4：关联“设备反馈与 MPIO”“系统状态与输出组织”的模块阅读准备。上述关联均不表示已完成对应任务或验收。

### 使用说明

以下区分三类依据：当前模型/字典可直接确认的名称、含义明确的通用英文词与行业术语、需要项目接口文件确认的专用缩写。通用术语不能单独证明本项目的设备型号、故障位编码、坐标轴、正负方向或运行时参数。

### A. 功能模块、软件与输入来源

| 缩写/片段 | 英文与中文 | 当前语境或证据边界 |
|---|---|---|
| SP / Sp | Signal Processing，信号处理 | VE25SP 及 SpIn、SpOut 等命名；见 Q001 |
| Nav / NAV | Navigation，导航 | NavData、NavSpApp、BUS_NAV_IN |
| **Dev / DEV** | **Device，设备** | DevData 为设备反馈数据；BUS_DevData 汇总 LiftMCU、ThrustMCU 与 Ail/Ele/Rud 等舵面反馈（P15） |
| Sys / SYS | System，系统 | SysInfo、SysReady、Sys_Status |
| Claw / CLAW | Control Law，控制律 | ToClaw 为提供给控制律的接口组织 |
| FCS | Flight Control System，飞行控制系统 | BUS_FCS_NAV_Valid_Status 等；行业通用展开 |
| FCC | Flight Control Computer，飞控计算机 | fccId、activeFccId、SelectedFCC；行业通用展开 |
| BFCC / Bfcc | 按接口语境理解为备用/备份 FCC | 同时出现 Key_BackupFcc_Act 与 isBfccActv；项目正式展开和主备策略仍待核实 |
| GCS | Ground Control Station，地面控制站 | SpIn_GcsMsg、Calc_GCSData；行业全称见 W9 |
| RC | Remote Control，遥控 | 当前关联 RemotePilotCtrl 与 RC_SW 等遥控输入；具体协议不由 RC 三字母确定 |
| FMS | Flight Management System，飞行管理系统 | BUS_FMS_IN 包含高度、速度、航向、航迹等目标；行业全称见 W7。本题补齐 Q002 的通用展开，项目软件/设备边界仍待接口说明 |
| LNAV / VNAV | Lateral Navigation / Vertical Navigation，横向导航 / 垂直导航 | FMS_LNAV_Request、FMS_VNAV_Request、FMS_VNAV_Status；行业用语见 W7 |
| AP | Autopilot，自动驾驶仪/自动驾驶功能 | AP_Disengage_SW、Key_AP；名称定位到相关指令，不宣称具体接通逻辑 |
| MFD | Multi-Function Display，多功能显示器 | MFD_PltIn 在本工程中是驾驶员相关指令枚举；ENUM_Key_MFD 可见电机/舵面检查与 SysReady 等值（P17） |
| GPIO | General-Purpose Input/Output，通用输入输出 | BUS_PilotInData.GPIO、Bus_GPIOState；本题未核对引脚与电气极性 |
| CBIT | Continuous Built-In Test，连续内建自检 | VdtCBIT、SpIn_IpCBIT；行业展开见 W8；名字不证明测试覆盖范围 |
| RMS | 冗余管理相关命名 | RMS 常见展开和当前接口见 Q001；项目正式全称待核实 |
| DRMS | D = Device（设备），已由用户明确确认；从当前接口判断，属于飞控计算机余度管理相关模块 | DRMS→SP 提供 fccId、activeFccId、isFccExist、vtolState；SP→DRMS 返回导航有效性和备份飞控激活相关输入。来源 U-DRMS-20260915 及接口证据见 Q004；RMS 部分的正式展开和模块内部职责边界继续待确认 |
| Mdl / App / Lib | Model / Application / Library，模型 / 应用 / 库 | NavSpApp、SignalProcessingLib 等 |
| BUS / ENUM | 总线 / Enumeration（枚举） | BUS_DevData 为数据结构类型；ENUM_Key_MFD 为离散取值类型；Simulink 总线不自动对应某种物理通信总线 |
| HLR | High-Level Requirement，高级需求 | 模型中的 FCC_SP_HLR_* 标注；编号存在不等于建立了适用且有效的追踪链接 |

### B. 传感器与设备名称

| 缩写 | 英文与中文 | 当前语境或证据边界 |
|---|---|---|
| **RA / Ra** | **Radio Altimeter（无线电高度计）；量值语境可称 Radio Altitude（无线电高度）** | SpIn_Ra 包含 L164_Radio_Altitude；BUS_NavData 有 raRadioAlt_M；需求 SP_SW_HLR-529 将其关联到对地高度。项目也称“雷达高度” |
| ADAHRS | Air Data, Attitude and Heading Reference System，大气数据、姿态与航向参考系统 | 见 Q001 |
| ADAHRU / Adahru | Air Data, Attitude and Heading Reference Unit，大气数据、姿态与航向参考单元 | 见 Q002；AdahruMsgsPreprocess 为其报文预处理 |
| ADC | Air Data Computer，大气数据计算机 | FTADC 中 ADC 的通用展开 |
| FTADC | 大气数据设备输入名称 | FT 的正式展开仍为 Q001-U2 待核实 |
| MCU | 电机控制相关单元；常见候选为 Motor Control Unit | 当前 Sp_McuInfo 输入包含转速、功率、工作模式与故障等级反馈；功能对应已核对，正式英文命名仍待项目词表；Q023 复核 |
| SCU | 伺服/舵面执行机构控制相关单元 | 当前 SCU 提供位置、使能/状态等反馈；不强行选定 Servo Control Unit 或 Surface Control Unit |
| GPS | Global Positioning System，全球定位系统 | L076_GPS_Height、adahruGpsHeight_M |
| ADS | Air Data System，大气数据系统 | Alpha_ADS、Beta_ADS 的常用行业解释；具体接口来源回到模型 |
| IRU | Inertial Reference Unit，惯性基准单元 | Nx_IRU_g 等接口名；当前设备归属及其与 ADAHRU 的关系不能只凭名称判定 |
| IDU | IDU 设备/航电数据来源名称 | 需求 SP_SW_HLR-758、-771 使用 IDU，但未给出本项目英文展开；保留待核实 |
| VDT | 操纵杆输入相关命名 | VDT_Stick、BUS_PilotInData.VDT 可确认用途；不据名称认定具体传感器原理 |

### C. 飞行状态、物理量与导航目标

| 缩写/片段 | 英文与中文 | 当前例子与说明 |
|---|---|---|
| CAS | Calibrated Airspeed，校准空速 | Vel_CAS_mps、Calc_VotedCAS；见 W5 |
| TAS | True Airspeed，真空速 | Vel_TAS_mps、L210_True_Airspeed |
| BARO / Baro | Barometric，气压相关 | h_BARO_m、BaroCorrectedAlt；不能直接与 GPS 高度或 RA 高度混用 |
| AGL | Above Ground Level，距地面高度 | h_AGL_m；需求 SP_SW_HLR-529、-543 |
| AMSL | Above Mean Sea Level，海拔高度/相对平均海平面的高度 | h_AMSL_m；需求 SP_SW_HLR-527、-541；此处只解释命名，未核实运行数据的垂直基准 |
| AOA / Alpha | Angle of Attack / alpha，迎角 | AOA、Alpha_ADS_deg |
| AOS / Beta | Angle of Sideslip / beta，侧滑角 | AOS、Beta_ADS_deg |
| Ps / Pt | Static Pressure / Total Pressure，静压 / 总压 | ftadcStaticPressure_kPa、ftadcTotalPressure_kPa；Psi、Pti 的末尾 i 含义未由资料定义 |
| Lat / Lon | Latitude / Longitude，纬度 / 经度 | Lat_deg、Lon_deg |
| Alt / h | Altitude / height，高度 | FMS_Alt_Target、h_AGL_m；高度基准由其他片段确定 |
| Vel / Spd | Velocity / Speed，速度/速率 | Vel_CAS_mps 为飞行空速；McuSpdFbk 为电机转速反馈，具体物理量依对象确定 |
| Pos | Position，位置 | Pos_XY_Valid、Position_Fdb |
| Att | Attitude，姿态 | Att_phi_rad、Att_theta_rad、Att_psi_rad |
| Rot | Rotation，转动相关量 | Rot_p/q/r_rps 为角速度接口名；坐标系与正向另需定义 |
| Accel | Acceleration，加速度 | L331_Body_Longit_Accel 等 |
| Sfor | 常见写法为 Specific Force，比力 | 项目需求 SP_SW_HLR-539、-553 使用“三轴加速度”表述。当前只确认接口对应，重力处理和物理定义仍待核实 |
| Temp | Temperature，温度 | MotorTemp、adahruStaticTemp_C |
| Pwr / Power | Power，功率 | UsablePwr_KW、McuPowerFbk |
| HiVltg | High Voltage，高压 | HiVltgSts、HiVltgSW |
| Gnd / OnGnd | Ground / On Ground，地面 / 在地面状态 | OnGnd、AirGroundSwitch_OnGnd_b |
| WOW | Weight On Wheels，轮载/起落架承重状态 | WOW_LG1_b 等；行业缩写见 W10，具体开关极性仍需接口定义 |
| LG | Landing Gear，起落架 | WOW_LG1_b 至 WOW_LG4_b |
| FPA | Flight Path Angle，航迹倾角 | FMS_FPA_Target 与 FMS_Flight_Path_Angle_Target 是可见命名线索 |
| CG / Xcg | Center of Gravity，重心 / 重心纵向位置 | Center_Gravity、Xcg_AC_100MAC；参考点与归一化定义待接口说明 |
| MAC | Mean Aerodynamic Chord，平均气动弦长 | Xcg_AC_100MAC 的行业读法；不能只按名称断言实际计算基准 |
| Head / Heading / Track | 航向相关 / 航向 / 航迹 | FMS_Heading_Target、FMS_Track_Target；Heading 与 Track 是不同量 |
| VTOL | Vertical Take-Off and Landing，垂直起降 | vtolState、ENUM_DRMS_VTOL_STATE；枚举对应的状态机语义另查 |

### D. 数据、状态、动作与通道后缀

| 片段 | 通常含义 | 当前读法提示 |
|---|---|---|
| Msg / Msgs | Message / Messages，消息 / 多条消息 | McuMsg、RmsPublicMsg、AdahruMsgsPreprocess |
| Data / Info / Infos | 数据 / 信息 / 信息集合 | DevData、SysInfo、adahruNormalInfos |
| In / Out | Input / Output，输入 / 输出 | SpIn、SpCcdlMsgOut；相对于所属模块解释 |
| Calc | Calculate / Calculation，计算 | Calc_SysInfo |
| Preprocess | 预处理 | AdahruMsgsPreprocess |
| Voter / Voted | 表决器 / 经表决的 | 只提示表决或选源主题，算法另查 |
| Src / Source | 来源 | RaSrc、adahruSrc |
| Sel / Select / Selection | 选择 | PilotSource、lastSelect、SourceSelection |
| Fbk / Fdb | Feedback，反馈 | McuSpdFbk、Position_Fdb；本项目使用了两种缩写 |
| Cmd / Ctrl | Command / Control，指令 / 控制 | RemotePilotCtrl、CmdConv |
| Sts / Status / State | 状态 | HiVltgSts、Valid_Status、NavDataState；不能只按拼写区别判定是否为状态机变量 |
| Vld / Valid / Invalid | 有效 / 有效 / 无效 | WOWVld_LG1_b、Vel_CAS_Valid、McuSpdFbkInvalid；具体有效判据另查 |
| Reset | 重置/复位相关标志 | h_AGL_Reset；是否为单拍脉冲或持续状态需查逻辑 |
| SW | Switch，开关 | HiVltgSW、AP_Disengage_SW |
| EN / Enable | 使能 | EN_STS；编码与极性未核实 |
| Actv / Active | 激活/当前生效 | activeFccId、isBfccActv |
| Curr / Current | 当前、本通道 | Sys_Status_Curr 与 Left/Right 并列，指当前通道语境；此处不是据缩写推定电流量 |
| Left / Right、L / R | 左 / 右 | 当前通道标签或设备位置；两种语境分别解释，不自动推定物理接线 |
| Cmp | Compare / Comparison，比较 | CmpMonitor |
| Monitor | 监控器 | TimeoutMonitor、RangeMonitor、AbnormalMonitor |
| Timeout / Abnormal / Range | 超时 / 异常 / 范围 | 监控主题；实际阈值与恢复条件另查 |
| Target / Request | 目标值 / 请求 | FMS_Spd_Target、FMS_Trans_Request |
| Prot / Protect | Protection / Protect，保护 | MCProtOn、Key_McProtect；具体保护含义与触发条件待实现/需求 |
| Warn / Fault / Health | 告警 / 故障 / 健康状态 | WarnCode、FaultCode、FccHealth |
| Id / Bit / Log | Identifier（标识号）/ 位 / 记录信息 | fccId、HealthBit、NavLog；位编码不按名称猜测 |

### E. 设备数据中的部位和功能片段

| 片段 | 含义 | 来源 |
|---|---|---|
| Ail | Aileron，副翼 | BUS_DevData.AilLO/AilLI/AilRO/AilRI 与 ENUM_Act_Name.Aileron_* |
| Ele | Elevator，升降舵 | BUS_DevData.EleL/EleR 与 ENUM_Act_Name.Elevator_* |
| Rud | Rudder，方向舵 | BUS_DevData.RudL/RudR 与 ENUM_Act_Name.Rudder_* |
| Mtr / Motor | 电机 | ENUM_Key_MFD.MtrCheckIn/MtrCheckOut、MotorTemp |
| Surf / Surface | 舵面 | ENUM_Key_MFD.SurfCheckIn/SurfCheckOut、Surface_Check |
| Lift / Thrust | 升力 / 推力 | BUS_DevData.LiftMCU/ThrustMCU；不由名称推定具体安装方向 |
| LO / LI / RO / RI | 按舵面命名惯例为左外 / 左内 / 右外 / 右内 | 此处是名称释读；具体设备编号到舵面的接线映射仍需接口/构型核对 |

### F. 常见单位与转换片段

| 片段 | 含义 | 边界 |
|---|---|---|
| deg / Deg | degree，度 | 角度命名后缀 |
| rad | radian，弧度 | Att_phi_rad 等 |
| m / M | metre，米 | raRadioAlt_M、h_AGL_m |
| mps / MPS | metres per second，米/秒 | Vel_CAS_mps |
| mps2 / MPS2 | 米/秒² | Sfor_x_RB_mps2 |
| DegPS | degrees per second，度/秒 | adahruP_DegPS 等 |
| rpm | revolutions per minute，转/分钟 | Calc_SYSMsg_ToClaw 内 Spd_rpm |
| Pa / hPa / kPa | 帕 / 百帕 / 千帕 | 压力量命名后缀 |
| KW | kilowatt，千瓦 | 工程拼写 UsablePwr_KW；单位标准符号通常写 kW |
| b | 布尔量的命名后缀 | 如 OnGnd_b；最终类型以总线定义为准 |
| r2d / d2r | radians to degrees / degrees to radians，弧度转度 / 度转弧度 | SpCPF_define.m 第 2、3 行有明确换算式（P18） |
| Ts | 采样周期/时间步参数的惯用名 | SpCPF_define.m 第 5 行有声明；本题没有据此认定实际任务调度或模型步长 |

单位后缀用于阅读导航；当前表未逐信号验证单位转换。特别是 `rps` 在不同场景可能有不同用法，本题不只凭三字母判定为弧度/秒或转/秒。

### G. 项目专用或仍缺定义的缩写

| 名称 | 当前可确认到的用途 | 尚缺的定义 |
|---|---|---|
| FTADC 的 FT；RMS/DRMS 的完整正式英文名称 | DRMS 的 D = Device 已由用户确认，见 Q004；其他词义与接口见 Q001 | 剩余待核实项为 Q001-U1/U2 及 Q004-U1 中 RMS 部分的正式展开与文档出处 |
| MCU、SCU | 电机、舵面执行机构反馈数据来源 | 项目正式英文设备名称 |
| MPIO | 输入包含能源/高压状态、可用功率、重心/重量、检查状态与 FMS 目标 | 项目正式全称及设备职责边界 |
| IDU | 需求中的航电输入来源 | 项目正式英文全称、当前接口对应关系 |
| VDT、CSSU、RSSU、LSSU、SSU | 操纵杆输入及相关按键/通道组 | 设备/传感器全称，位置及通道映射 |
| FMCP | SpIn_GcsMsg.RemoteFMCP 对应的遥控输入对象 | 项目正式展开 |
| ELFS、TRC、Pfc、Mc/MCProt | 驾驶员按键或模式/保护相关标签 | 正式名称、功能定义；不由首字母猜模式含义 |
| CP、FO、PltIn、CSX/CSY、SSX/SSY | BUS_PILOT_IN 内驾驶员输入分组和轴量名称 | PltIn 可按 Pilot Input 理解；其他位置/操纵形式需项目接口确认 |
| REO、RB、IRU 后缀，Rot 的 p/q/r | 导航输出中坐标/设备/分量相关标识 | 坐标原点、轴方向、符号、参考设备与物理定义 |
| Sfor | 与三轴加速度接口对应 | 比力/加速度的精确定义、重力处理和参考系 |
| CPT、CPF | SpCPT、SpCPF 参数对象命名 | 两个缩写的正式展开；参数声明值与运行解析值仍分开 |
| INT_STS、STK_MON、EgyWkSts、Frt_psi | 设备状态或特定接口字段 | 厂商/项目正式解释、位定义、量纲与编码 |

### H. 把词表用于实际名称

- `DevData`：Device + Data，设备数据。
- `raRadioAlt_M`：RA 来源 + Radio Altitude + 米，无线电高度数据。
- `McuSpdFbkInvalid`：MCU + Speed + Feedback + Invalid，电机控制相关单元的转速反馈无效标志；无效编码与监控逻辑仍按接口实现核对。
- `h_AGL_Valid`：对地高度的有效标志。
- `Sys_Status_Curr`：当前通道的系统状态。
- `FMS_Spd_Target_Valid`：FMS 速度目标的有效标志。

### 来源、版本与证据边界

工程基线仍为 `AC102_Rebuild`、提交 `a895a43a19209c076e679fcc2e2f8953e9f49bb4`。本轮复核 P1、P4、P6 的原文件哈希与 Q001 一致；读取当前引用链时只在内存解析 SLX/SLDD XML。未运行 MATLAB 回调、更新模型、仿真或生成代码。

- 当前引用链共 18 个模型：VE25SP、AdahruMsgsPreprocess、RAMsgsPreprocess、Calc_NavSourceSelection、Calc_VotedCAS、Calc_VotedRA、Calc_VotedFTADC、Calc_MPIOData、Calc_SysInfo、Calc_SPDispMsg_Out、Calc_MCUSCUData、Calc_GCSData、Calc_NAVMsg_ToClaw、Calc_PilotIn_ToClaw、Calc_SYSMsg_ToClaw、Calc_PilotInData、Calc_FMSMsg_ToClaw、Calc_CcdlMsg_Out。
- P4 精确字典条目：SpIn_Ra、SpIn_GcsMsg、SpIn_Mpio、SpIn_Sitck（源文件拼写如此）、SpIn_Key、SpIn_SysInfos、Sp_McuInfo、Sp_ScuInfo、BUS_NAV_IN、BUS_FMS_IN、BUS_PILOT_IN、BUS_DRMS2SP_MSG、BUS_SP2DRMS_MSG。
- P6 补充锚点：SP_SW_HLR-255、-529、-543（RA/AGL）；-527、-541（AMSL）；-539、-553（Sfor/三轴加速度表述）；-345、-357、-368、-369（设备反馈）；-758、-771（IDU）；-763（CBIT 名称）。这些历史需求的适用性边界沿用 Q001-P6。

| ID | 新增来源及精确锚点 | 原文件 SHA-256 / 使用范围 |
|---|---|---|
| P15 | [SP_InteriorBus.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd)：data/chunk0.xml 中 BUS_DevData、BUS_NavData、tMcuMsg、BUS_PilotInData、BUS_MpioData、Bus_GPIOState、Bus_VDTState | `5356B8602E6B3948291CA5410F5CE444D1277E5141AB93C21CF77D11D81C01F5` |
| P16 | [ENUM_Act_Name.m](D:/666/VE25/sp/04_Design/common/libs/sldd/ENUM_Act_Name.m:4)：第 4—11 行 Aileron/Elevator/Rudder 枚举名 | `413D357620B32D4F06C57FA9B6AB1BFC3D494D0CE769D9AF15F19EE7FF40B22C` |
| P17 | [ENUM_Key_MFD.m](D:/666/VE25/sp/04_Design/common/libs/sldd/ENUM_Key_MFD.m:5)：第 5—9 行 MtrCheck、SurfCheck、SysReady | `096555AC80D2C1B7CC650F2DEEA9E9D48F07599B44D13937E62DC95D60A7357B` |
| P18 | [SpCPF_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPF_define.m:2)：第 2、3、5 行 r2d、d2r、Ts 声明 | `1F3395EAC6BB802E7B90D5F78EFF7DE00E29A8BCD52FEFE4B754FAE425812345` |
| W7 | [FAA AIM：Air Navigation](https://www.faa.gov/Air_traffic/publications/atpubs/aim_html/chap1_section_2.html)：Flight Management System 与 LNAV/VNAV 的展开 | 2026-09-15；只作行业术语依据，不代替项目 FMS 架构 |
| W8 | [Curtiss-Wright：Continuous Built-In Test](https://defense-solutions.curtisswright.com/resources/kadbit101-continuous-built-test-module-data-sheet)：CBIT 全称 | 2026-09-15；只引用公开说明，未获取需登录资料 |
| W9 | [FAA 公开文件：GCS 用语](https://www.faa.gov/media/42171)：Ground Control Station (GCS) | 2026-09-15；只作行业缩写依据 |
| W10 | [NASA 口述史中的 WOW 用语](https://www.nasa.gov/wp-content/uploads/2025/08/springsc-11-18-03.pdf?emrc=7632c6)：Weight On Wheels | 2026-09-15；只作通用术语佐证，不推定项目开关实现 |

其余行业缩写沿用 Q001/Q002 的 W1—W6。Device、Feedback、Current、Command 等通用英文词的释义结合当前代码标识和数据结构解释；没有把这些词义直接升级为未经核实的工程行为。

### 学习表现、未决事项与下一步

- 学习表现：**掌握程度未确认**。本轮是用户要求扩大词表范围，没有独立复述、定位、场景推演或实际验证的证据。
- 本次成果：分类词表、六个实际名称拆解例子、当前引用链范围和项目专用待确认清单。
- 问题处理状态：部分澄清；G 节保留未查到正式定义的项目缩写。词表覆盖常见命名片段，不声称已经逐字段解释所有模型内容。
- 阶段验收：不据词条数量评估进度；S1—S4 对应任务未自动勾选，也未评估 S6-A/S6-B。
- 下一次最小任务：按用户指定信号或模块，将名称拆解接到总线字段与真实处理逻辑；有新的项目词表/接口来源时，逐项修订 G 节。
- 写入范围：仅更新本问答记录；Q001、Q002 正文保持原样。工程 Git 状态复核仍只有原两处 resources/project XML 修改；未在 sp 中生成中间文件。
- Delta-K route：project-only。

### 修订记录

- R001（2026-09-15）：按用户“不要局限于列举名称”的澄清，新增主动检索的分类词表。FMS/LNAV/VNAV 的行业展开获补充来源；此前关于项目接口/设备正式定义的待确认项按本节继续区分。
- R002（2026-09-15）：用户指出 DRMS 解释遗漏后，将 A 节 RMS/DRMS 合并项拆开，明确补充 DRMS 已知接口用途并链接 Q004；正式全称仍保留待确认。Q001、Q002 正文保持原样。
- R003（2026-09-15）：记录用户确认 D = Device（设备），同步更新 A 节 DRMS 词条和 G 节待确认范围；保留 RMS 部分正式名称与模块职责边界的待核实状态。

## Q004：DRMS 表示什么意思，为什么上一轮漏掉了？

### 日期、阶段与具体条目

- 日期：2026-09-15，Asia/Shanghai；记录修订：R002。
- S1：关联第 4 章七类功能数据关系及“能够从输入来源讲到输出消费者”的验收，本题补充 SP 与 DRMS 的接口对象识别。
- S2：关联“在选定主线中定位输入、处理模型、组包模型及输出”，本题实际定位 RmsPublicMsg、SP2DRMS 及字段消费者，属于接口阅读准备；没有完成 CAS 全链路或故障场景推演。
- 第 7.8 节映射参考：“SP 输入、处理和输出如何组织？”对应 S1、S2。学习表现按第 5.2、5.3 节单独记录，不自动认定任何阶段完成。

### 直接回答与遗漏修正

**用户已确认：DRMS 中的 D = Device（设备）。** 来源为 2026-09-15 本任务用户明确说明，编号 U-DRMS-20260915。

**从当前 SP 接口判断，DRMS 属于飞控计算机余度管理相关模块。** SP 接收它提供的本机编号、当前激活的飞控计算机编号、各通道存在标志和飞行状态；SP 向它输出导航有效性与备份飞控激活相关输入及有效性。

上述模块用途依据当前双向接口解释；D = Device 则依据用户明确说明。已读模型及字典没有给出完整的正式英文名称，RMS 部分的正式展开仍待核实；SP 一侧接口也不足以确定 DRMS 内部的全部管理、选择和切换算法。

上一轮的遗漏是答复组织不完整：把“正式全称待确认”放进了待确认项，却没有把已能解释的接口用途单独讲清楚。此前已在 Q003 的 A 节将 DRMS 单独列项；本次按用户说明补齐 D = Device，其余正式名称的待确认范围已同步收窄。

### 双向接口与字段

| 方向/位置 | 原始名称 | 本题可解释的含义 | 证据与边界 |
|---|---|---|---|
| DRMS → SP；VE25SP 顶层输入端口 10 | RmsPublicMsg；类型 BUS_DRMS2SP_MSG | 来自 DRMS 一侧的公共消息总线 | P1，system_root.xml，SID 68；名字中的 Rms 与类型中的 DRMS 在这一输入上指向同一接口，不能因此推广到项目所有 RMS/DRMS 命名 |
| 同上 | fccId | 本机/当前计算机编号 | P4：uint8 标量；P19 的 system_6.xml 按 fccId 分支，并结合 activeFccId 处理；不把两者视为同一编码 |
| 同上 | activeFccId | 当前激活的飞控计算机编号 | P4：Enum: ENUM_DRMS_FccId，枚举为 NONE=0、PFCC1=1、PFCC2=2、PFCC3=3、BFCC=4 |
| 同上 | isFccExist | 各 FCC 通道的存在标志 | P4：长度为 3 的 boolean 数组；“存在”不能直接替换为“健康”或“可用”，数组的具体通道映射需结合接口定义 |
| 同上 | vtolState | 垂直起降/飞行阶段相关状态 | P4：Enum: ENUM_DRMS_VTOL_STATE；枚举包含 Unknown、Disarm、Arm、MtrCheck、SurfCheck、LiftOff、MC、TRANS、FW；本题不由名称推出状态转换条件 |
| SP → DRMS；VE25SP 顶层输出端口 9 | SP2DRMS；类型 BUS_SP2DRMS_MSG | SP 发送给 DRMS 的消息总线 | P1，system_root.xml，SID 458 |
| 同上 | navValid | 导航信息有效性状态集合 | P4：Bus: BUS_FCS_NAV_Valid_Status；P1 中 Claw_Nav_Msg.Valid_Status 经 SID 442 接入 SID 446 的 navValid |
| 同上 | isBfccActv | 备份飞控激活相关输入的处理结果 | P1：来自 GcsData.KeyBackupFccAct.data/valid 与 PilotInData.GPIO 选定元素的 data/valid，经过 Switch、boolean 转换再组包；不能只凭字段名认定为 BFCC 已实际接管的反馈 |
| 同上 | isBfccActvValid | 上述激活相关输入的有效性 | P1：两路 valid 经 OR（SID 457）进入总线赋值 SID 446；P4：boolean 标量 |

名称拆解：

- BUS_DRMS2SP_MSG：BUS（总线数据结构）+ DRMS + 2（to，送往）+ SP + MSG（Message，消息）。
- BUS_SP2DRMS_MSG：SP 送往 DRMS 的消息总线。
- RmsPublicMsg：Rms + Public（公共/共享）+ Msg（消息）。当前顶层输入的实际类型是 BUS_DRMS2SP_MSG。
- 这里的总线名称描述模型数据结构和方向，未核实外部传输协议。

### 在当前 SP 模型中的实际用途

1. **导航处理的地面状态输入**：P1 的 NavSpApp 从 RmsPublicMsg 选取 vtolState（system_54.xml，SID 61→62）；与 Unknown、Disarm、Arm、MtrCheck、SurfCheck 五个枚举值分别比较（SID 461—465）后经 OR（SID 466）生成 OnGnd，并送入导航选择/表决及后续处理。这里记录模型中的判定表达式，不将其扩展为真实飞机状态定义。
2. **系统信息处理**：P1 顶层 RmsPublicMsg 经 From11（SID 392）、Bus Selector（SID 393）取出 isFccExist，连接到引用 Calc_SysInfo 的 Model1（SID 363）第 1 输入。
3. **地面站相关的主用通道识别**：P1 的 From4（SID 374）连接引用 Calc_GCSData 的 Model3（SID 370）第 1 输入；P19 中 Rms2PublicMsg 进入 Subsystem（SID 6），取出 fccId 与 activeFccId（system_6.xml，SID 8），处理结果接至 ActiveFCC 标签（顶层 SID 5）。
4. **给 DRMS 的反馈/输入组织**：P1 顶层 Bus Assignment（SID 446）组装 navValid、isBfccActv、isBfccActvValid，输出 SP2DRMS（SID 458）。isBfccActv 的实际来源按上表解释，避免把激活输入、激活命令与已完成接管状态混为一谈。

### 来源、版本与核查范围

工程基线沿用 AC102_Rebuild、提交 a895a43a19209c076e679fcc2e2f8953e9f49bb4。本题只读解析文件压缩包中的 XML；未运行 MATLAB、加载/更新模型、仿真或生成代码。

| ID | 来源与精确锚点 | SHA-256 / 范围 |
|---|---|---|
| P1（复核） | [VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)：simulink/systems/system_root.xml、system_54.xml，具体 SID 见上文 | 20D8C7DE2A103D8FF298BCD5BB0C28F73AA6CCC2E27D43D0F5AB9952365F77E4 |
| P4（复核） | [SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)：data/chunk0.xml，BUS_DRMS2SP_MSG、BUS_SP2DRMS_MSG、ENUM_DRMS_FccId、ENUM_DRMS_VTOL_STATE | 10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC |
| P19 | [Calc_GCSData.slx](D:/666/VE25/sp/04_Design/Calc_GCSData/specification/Calc_GCSData.slx)：system_root.xml，SID 4/5/6；system_6.xml，SID 8/14/62 | D92EB5C7C942656ED04C6AF72D11EEB821AA3316256DB308E27DBF1E9F0E8BCF |
| P20（补充检索） | [高级需求工作簿](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)：xl/sharedStrings.xml | 读取快照 SHA-256：E3F28F03B948613CCAB451EE469B9FD5A87313393398BECD541BDA3126406BEB；仅搜索 DRMS、RMS、冗余管理、余度管理，未命中；不据此认定整个工作簿或全部工程不存在定义 |
| U-DRMS-20260915 | 2026-09-15，本任务用户明确说明：“D是device意思，记录一下” | 用户提供的项目术语说明：DRMS 中 D = Device（设备）；该说明未另行确认 RMS 部分的完整正式展开或模块全部职责 |

- P4 中上述总线、枚举及所读字段的 Description 为空，没有可用于正式全称展开的文字定义。
- 对 sp 内 02_Requirements、03_Architecture、04_Design、50_Delivery 的限定文本类型，以及补充需求工作簿的共享字符串作了定向检索；本题没有声称穷尽所有工程材料。
- 通用公开术语检索未取得能证明本项目 DRMS 正式全称的来源，因此没有把外部相近用法作为项目定义。

### 问题状态、学习表现与下一步

- 问题处理状态：**部分澄清**。D 的含义子项已解答：Device（设备），来源为用户明确确认；双向接口、字段用途及遗漏原因已解释。RMS 部分正式展开和模块内部职责仍待核实。
- 未决项 Q004-U1：D = Device 已由 U-DRMS-20260915 确认；待核实范围收窄为 RMS 部分的正式展开及适用项目文档出处。关联 Q001-U1 与 Q003-G。
- 未决项 Q004-U2：DRMS 内部职责和接管状态定义未由 SP 一侧接口证明；需要对应模块或接口要求，不能从名字推断。
- 学习表现：用户主动补充并确认 D = Device，记录为该术语的用户说明；对 DRMS 模块的完整理解仍为**掌握程度未确认**，没有据此认定模型定位、场景推演或阶段验收完成。
- 下一步最小任务：用户继续指定接口或字段时，按真实连线解释；出现正式术语来源时修订 Q004-U1 和 Q003，不要求用户先补资料才能继续问答。
- 写入范围：仅本问答记录；R001 新增 Q004 并更新索引、词表，R002 根据用户说明同步修订 Q001/Q003/Q004 的 D 释义、待确认范围及来源说明；不修改模型、学习计划、设计说明、知识索引、主台账或治理文件。
- Delta-K route：project-only。

### 修订记录

- R001（2026-09-15）：补充 DRMS 的实际输入输出、使用位置及英文全称证据边界，承认并修正上一轮词表对 DRMS 功能解释的遗漏。
- R002（2026-09-15）：记录用户明确确认 DRMS 中 D = Device（设备），增加来源 U-DRMS-20260915，关闭 D 含义的待确认子项；同步修正词表，并将正式名称未决项的交叉引用校正为 Q001-U1。其余术语和模块职责未作额外推断。

## Q005：Calc_SignalState_Double 中“拍”、三路输入、失配监控和锁存禁用是什么意思？

### 日期、问题与学习阶段

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户问题：拍的含义、输入为何常见长度 3、适配监控与锁存禁用的含义。
- 术语校准：结合本模块的 MissCompare 参数，将“适配监控”按“失配监控”解释；若用户另指名为“适配”的其他功能，需要具体模块来源。
- S1：输入来源与路数；S3：公共监控机制、故障进入/保持/退出、禁用和复位、运行周期与报文频率、初始化/首拍、逐拍表及参数声明/引用/运行解析值；关联 S4 的 ADAHRU 导航输入。
- 按学习计划第 5.2、5.3、7.8 节分别记录问题状态与学习表现。本题为解释和静态计数示意，不自动认定阶段验收。

### A. “拍”指一次运行周期

一拍是离散监控逻辑执行一次、推进一次计数或历史状态的运行周期；k 为本拍，k-1 为上一拍。需求 SP_SW_HLR-105 明确“计数每个运行周期增加 1”（P20，C91）。模型通过 Delay 保存历史计数（P21，system_1271.xml，SID 1283/1284，DelayLength=1、初值 int32(0)）。

- 同一拍可以处理三路数据。For Each 按元素处理，不表示时间上经过三拍。
- 拍的单位是执行次数，换算时间需要监控实例的实际调用周期，不能把设备报文更新一次与算法调用一次自动等同。
- 若已知某实例每 10 ms 调用一次，可以据此理解运行节奏；从第一个被计数的采样点到第十个采样点只有九个采样间隔，故障发生到输出变化的延迟还涉及故障相位、首拍、计数和状态判断顺序。
- SpCPF_define.m 第 5 行声明 Ts=0.01；SpCPT_define.m 的 CombLate、CombLong 章节注释写 20ms。本轮只确认声明/注释及模型引用，未核实该实例的编译采样时间、调度和运行解析值，不宣布一拍实际就是 10 ms 或 20 ms。

有效比较、正阈值、初始计数为零且未触及上限时，静态计数示意如下：

| 拍 | 两路数据是否在容许误差内 | missCnt | accordantCnt |
|---|---|---:|---:|
| 1 | 否 | 1 | 0 |
| 2 | 否 | 2 | 0 |
| 3 | 是 | 0 | 1 |
| 4 | 否 | 1 | 0 |

计数表达式来自 P21 的 system_1286/1297.xml：不一致时 missCnt=min(missCnt上一拍+1,max(MISS,BAN,0))，accordantCnt=0；一致时 accordantCnt=min(accordantCnt上一拍+1,SYNC)，missCnt=0。这是连续计数示意，未据此认定故障输出的精确触发拍，也不是仿真结果。

### B. 长度 3 来自三路 ADAHRU 输入与比较选源设计

Calc_SignalState_Double 的 DataIn1（P21，system_2563.xml，SID 2564）为 Bus: SpData_Double，PortDimensions=3：长度为 3 的一维总线数组，每个元素是一个来源的 data 与 valid。Double 表示 data 为双精度类型。

例如纬度输入的三个元素分别是 ADAHRU1、ADAHRU2、ADAHRU3 提供的纬度及有效性。当前需求 SP_SW_HLR-250（P20，C131）明确输入为“三路 ADAHRU、两路 RA、一路 FTADC”；SP_SW_HLR-269（C146）明确单路监控、跨路比较同步监控、冗余算法输出流程。因此长度 3 有当前项目接口和需求依据，不能概括成所有 SP 输入必须为 3。

| 字典对象/字段 | 核对维度 | 含义 |
|---|---:|---|
| SpIn_Adahrus 的全部 31 个字段 | 每字段 3 | 同一个量的三路 ADAHRU 来源 |
| SpIn_Ra.L164_Radio_Altitude | 2 | 两路 RA |
| SpIn_ftadc 的 Psi/Pti/AOA/AOS/CAS/Valid | 每字段 1 | 一路 FTADC |
| SpIn_McuInfos.MCU | 11 | 电机控制相关数据元素数量 |
| SpIn_ScuInfos.SCU | 8 | 舵面执行机构相关数据元素数量 |
| SpData_Double.data / valid | 各 1 | 元素内部为 double 数据与 boolean 有效标志 |

三个设备来源与三个物理轴是两个概念：L331_Body_Longit_Accel、L332_Body_Lateral_Accel、L333_Body_Normal_Accel 已分别代表三个轴，每个轴字段又有三路来源。顶层 SpIn_ADAHRS 为一个总线对象（PortDimensions=1），其内部字段各含三个元素，两者可以同时成立。

当前纬度调用链（P8，system_root.xml）为：SpIn_ADAHRS SID 4 → Bus Selector SID 5 第 1 输出 → L310_Combined_Latitude 标签（Goto SID 31 / From SID 125）→ Calc_SignalState_Double SID 127。经度实例为 SID 129。库内 system_2584.xml 按第 1 维、每次 1 元素提取 data/valid；system_2602.xml 的 Demux SID 2607/2608 分成三路，送比较监控。

通用工程解释：两路不一致时，仅凭两路数值通常难以分辨哪路偏离；三路为识别“两路相符、另一路偏离”提供更多依据。本题没有读取选择三台设备的完整系统安全权衡，不将此解释当成项目全部设计理由，也没有证据称其为通用法规强制路数。Simulink 支持不同向量/矩阵维度，见 W12。更改通道数需同步检查三对比较与选源逻辑，不能仅改 PortDimensions。

### C. 失配监控：多路数据的持续不一致检查

MissCompare 表示通道间不匹配。输入自身的 valid 与跨通道比较结果是不同信息。

- CmpMonitor 中三个 Comparator 分别比较 1–2、2–3、3–1（P21，system_1326.xml，SID 1481/1482/1483；P20，SP_SW_HLR-169，C104）。
- 每对比较明确使用 abs(data1-data2)<=MAXERROR 视为一致（P21，system_1271.xml，SID 1311），等于误差门限也算一致。
- 不一致时失配计数递增，一致时清零；计数记录连续性，不跨不同片段累计。
- 通道状态还结合第三路与比较有效性，不能仅凭任意一对不一致就确定哪路有问题。
- 概念例子：同一个量的三路读数为 100、100.2、130，容许误差为 1，前两路彼此相符，第三路与它们都不一致，为识别第三路偏离提供依据。这不是项目参数或实测数据。

以 m12/m31 表示两对失配请求，v12/v31 表示对应比较有效性，通道 1 的实现组合为：

    Miss1=(m12 AND m31) OR (m12 AND NOT v31) OR (m31 AND NOT v12)

当 MISS<=0、SYNC<=0 或任一路无效时，Comparator 转入不运行分支，当拍 syncReq/missReq/banReq 均为 false。重新进入有效比较分支时其计数重置（system_1220/1233/1241.xml，ActionPort SID 1244 的 InitializeStates=reset）；这不清除外层已锁存的 Banned。

模型先更新本拍计数，再送请求判断。需求 SP_SW_HLR-107/-108/-669（P20，C93/C94/C95）规定达到门限等请求条件，但本次没有把需求中的边界自动当成已运行核验的实现边界，见 Q005-U2。

### D. 锁存禁用：置位后保存禁用历史

以通道 1 为例：

    Banned1(k)=Banned1(k-1)
               OR (reqBan12(k) AND reqBan31(k)
                   AND inAir(k) AND (P_BANENABLE>0))

P21 的 system_1326.xml：SID 1359 为新禁用条件，1351 与历史值作 OR，1383 为初值 false 的一拍 Delay。一次新置位需要本路涉及的两对比较器均提出禁用请求，并满足空中与许可门控。

- 一旦锁存，仅靠数值恢复一致、inAir 变 false、随后关闭 banEnable，都不会清掉已有 Banned。
- 本模块无显式外部复位口；重新初始化相关内部状态可以恢复初值 false，但本题不指定整机需要采用何种复位/重启操作。
- 此 Double 包装将 inAir 固定接 true（system_2602.xml，SID 2606 → CmpMonitor SID 2605 第 7 输入）；这里没有真实地面状态参与此门控。
- 上一拍 Banned 会屏蔽下一拍该通道的内部比较有效性，当前 Banned 也压制 isSynced。下游最终选源结果仍取决于消费者实现。
- 外层继续输出原 data、valid，并额外组织监控状态。SignalStateSet_New（system_2514.xml）编码为 stateBits=valid+2*isSynced+4*isMissCompare+8*isBanned。因此“禁用”在此表示监控/使用资格相关状态，不能直接理解为清零输入数据、关闭硬件或切断通信。

### E. 当前纬度/经度实例的声明值

P8 的 SID 127/129 分别将 Param_* 绑定到 SpCPT.CombLate_* / SpCPT.CombLong_*。P5 的 334—338、353—357 行均声明：

| 参数后缀 | 声明值 | 含义 |
|---|---:|---|
| maxerror | 0.1 | 容许误差，按相应信号量纲理解 |
| misscmp | 10 | 失配请求计数门限 |
| banned | 50 | 禁用请求计数门限 |
| synced | 2 | 一致/同步请求计数门限 |
| banenable | 0 | 不允许新置永久禁用；不是清除已有锁存 |

P5 第 72 行明确开关含义为 0 不允许、1 允许。当前只能确认脚本声明及引用，未取得运行解析值。失配请求与禁用请求使用同一连续失配计数，不能简单解释为先数 10 拍、再额外数 50 拍。也不能只看到 banned=50 就说当前实例一定会禁用，因为声明的 banenable=0。

### 来源与版本

工程基线：AC102_Rebuild，提交 a895a43a19209c076e679fcc2e2f8953e9f49bb4。本轮只读解析文件，未加载 MATLAB、更新模型、仿真、生成代码或修改 sp。

| ID | 来源与精确锚点 | SHA-256 / 边界 |
|---|---|---|
| P21 | [SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)：根 Calc_SignalState_Double SID 2563；system_2563/2566/2584/2602.xml；Comparator system_1220/1233/1241/1271/1286/1297.xml；CmpMonitor system_1326.xml；SignalStateSet_New system_2514.xml | 0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490 |
| P4 | [SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)：data/chunk0.xml 中上述总线与维度 | 10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC |
| P8 | [AdahruMsgsPreprocess.slx](D:/666/VE25/sp/04_Design/AdahruMsgsPreprocess/specification/AdahruMsgsPreprocess.slx)：system_root.xml，SID 4/5/31/125/127/129/138 | A5F9E7FAC266448447F929CF4BDC5CFF6C9B322A81D479995C61799977E65143 |
| P5 | [SpCPT_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPT_define.m:334)：第 72 行禁用说明、第 322/341 行周期注释、第 334—338/353—357 行参数声明 | 49AAF3CBA343CA4B8E1A4C4B8959B1BBD4824C56378C36B61B74119F5DB218B0 |
| P18 | [SpCPF_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPF_define.m:5)：Ts 声明 | 1F3395EAC6BB802E7B90D5F78EFF7DE00E29A8BCD52FEFE4B754FAE425812345 |
| P20（扩展核对） | [当前用户指定需求工作簿](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)：工作表“信号处理软件高级需求-SP_SW_HLR”；C91—C95、C104、C131、C146、C151 | 快照 E3F28F03B948613CCAB451EE469B9FD5A87313393398BECD541BDA3126406BEB；G 列仍有 NavSpMdl 旧路径，本题使用条款正文并对照当前主线，不沿旧路径推定现行模型 |
| W11 | [MathWorks：What Is Sample Time?](https://www.mathworks.com/help/simulink/ug/what-is-sample-time.html) | 2026-09-16；采样时间与执行节奏 |
| W12 | [MathWorks：Inport](https://www.mathworks.com/help/simulink/slref/inport.html) | 2026-09-16；PortDimensions=N 的向量长度含义 |
| W13 | [MathWorks：Delay](https://www.mathworks.com/help/simulink/slref/delay.html) | 2026-09-16；样本延迟与历史状态 |
| W14 | [MathWorks：For Each](https://www.mathworks.com/help/simulink/slref/foreach.html) | 2026-09-16；按元素/分区处理语义 |

### 已答内容、待核实问题与学习表现

- 问题处理状态：**部分澄清**。四部分概念及接口、计数、锁存结构已静态解释；实际时序和上层设计取舍仍有证据缺口。
- Q005-U1：监控实例实际调用周期、报文更新频率、编译采样时间及运行参数解析值未核实。
- Q005-U2：system_1241.xml 的阈值 RelationalOperator SID 1267—1270 省略 Operator，bddefaults.xml 仅有 MaskDefaults；本次未解析其 factory 默认比较符。不得由需求“达到门限”直接声称实际第 10/50/2 拍触发已核实。本轮不作编译/仿真验收，也不将该解析缺口直接判为模型缺陷。
- Q005-U3：选择三路 ADAHRU 的完整系统安全、架构取舍依据未读取；当前需求可证明路数和处理用途。
- Q005-U4：锁存状态的整机复位生命周期及全部下游消费者的选源/隔离行为未完整追踪。
- 学习表现：**掌握程度未确认**。本轮提供解释，没有用户独立复述、定位或逐拍推演证据。
- 下一步最小任务：围绕用户后续选择的具体实例继续逐拍追踪；实际编译/运行证据在相应授权范围内另行开展。
- 写入范围：仅本问答记录 Q005 及索引；保留 Q001—Q004 内容。未修改计划、设计说明、知识卡、索引、治理或工程文件。
- Delta-K route：project-only。

### 修订记录

- R001（2026-09-16）：记录四部分问题、项目需求/实现来源、连续计数示例及禁用锁存；注明当前 banenable=0 的声明层级，并保留实际时序和上层架构理由的待核实项。

## Q006：SpIn_ADAHRS 接出的信号为什么用 L+数字命名？

### 日期、问题与学习阶段

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户问题：SpIn_ADAHRS 接出的信号采用 L+数字命名的含义。
- S1：输入来源与信号含义识别；关联 S2“在选定主线中定位输入、处理模型、组包模型及输出”和关键点的数据类型、单位、有效性记录。本题仅完成输入字段命名与数据组织的静态识别，未完成全链路追踪。
- 按学习计划第 5.2、5.3、7.8 节分别记录问题处理状态与学习表现。

### 简明解释

这里的 L 可按 Label（数据标签）理解，数字标识某类数据，后缀说明物理量。例如 L324_Pitch_Angle 可读作“标签 324：俯仰角”。当前模型、字典和需求直接支持下表中的字段与物理量对应关系。

这一命名与 ARINC 429 的标签惯例相符：Label 是信息标识符，占 8 bit，通常写成三位八进制数，见 W15。按该惯例，310 是八进制标签的写法，不表示第 310 路设备，也不是十进制量值。项目采用该惯例是结合名称与行业资料的解释；本轮没有找到明确声明本接口协议及编号进制的设备 ICD 条款，不将推断升级为已确认的项目协议定义。

| 当前真实字段 | 名称对应的物理量 | 数据组织 |
|---|---|---|
| L076_GPS_Height | GPS 高度 | 3 个 SpData_Single 元素 |
| L107_Angle_Of_Attack | 迎角 | 3 个 SpData_Single 元素 |
| L203_Pressure_Altitude | 气压高度 | 3 个 SpData_Single 元素 |
| L310_Combined_Latitude | 组合纬度 | 3 个 SpData_Double 元素 |
| L311_Combined_Longitude | 组合经度 | 3 个 SpData_Double 元素 |
| L324_Pitch_Angle | 俯仰角 | 3 个 SpData_Single 元素 |
| L325_Roll_Angle | 滚转角 | 3 个 SpData_Single 元素 |

### 标签编号与三路来源的区别

- 标签编号回答“是什么数据”；数组下标回答“来自哪一路设备”。例如 L324_Pitch_Angle 的三个元素对应 ADAHRU1、ADAHRU2、ADAHRU3 的俯仰角数据及有效性，长度 3 的需求与模型依据见 Q005。
- SpIn_Adahrus 共 31 个字段，其中 30 个采用 L 前缀，另一个字段为 AdaComb_Altitude_Rate；不能概括成该总线所有字段都遵循 L+数字格式。
- L310/L311 的元素类型为 SpData_Double，其他上表示例为 SpData_Single；两种元素类型均包含标量 data 和 boolean 类型的 valid。当前 SP 输入边界已经是结构化数值及有效性信息，仅凭名称不能认定本模块直接按位解析原始 ARINC 429 字。
- 模型中的 Bus Selector 选出这些总线字段；Goto/From 的 Tag 沿用相同名称，用于模型内部信号连接。模型连接标签与协议数据标签是不同层面的用途。
- 编号不是连续枚举或计算公式。项目中的具体含义应以当前字典、需求及设备接口定义为准，不应拿其他设备的通用标签表直接覆盖本工程映射。
- 本表只解释字段名称和数据组织，不据此指定全部信号的单位、缩放、原始编码或消息更新周期。

### 来源与版本

工程读取基线：AC102_Rebuild，提交 a895a43a19209c076e679fcc2e2f8953e9f49bb4。当前学习使用 VE25SP 及实际引用模型；未加载 MATLAB、更新模型、仿真或生成代码。

| ID | 来源与精确锚点 | SHA-256 / 证据边界 |
|---|---|---|
| P8 | [AdahruMsgsPreprocess.slx](D:/666/VE25/sp/04_Design/AdahruMsgsPreprocess/specification/AdahruMsgsPreprocess.slx)：simulink/systems/system_root.xml，SpIn_ADAHRS SID 4、Bus Selector SID 5 的 OutputSignals；纬度 Goto SID 31 / From SID 125 | A5F9E7FAC266448447F929CF4BDC5CFF6C9B322A81D479995C61799977E65143；确认实际字段名及模型连接 |
| P4 | [SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)：data/chunk0.xml 的 DD.ENTRY SpIn_Adahrus、SpData_Double、SpData_Single，Elements_internal | 10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC；确认 31 个字段、每字段维度 3 及 data/valid 结构；字段 Description 为空 |
| P20（扩展核对） | [当前用户指定需求工作簿](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)：工作表“信号处理软件高级需求-SP_SW_HLR”，SP_SW_HLR-289（第 157 行）中的 L307 纬度细值与 L310 纬度，SP_SW_HLR-309（第 177 行）中的 L324 俯仰角 | 读取快照 E3F28F03B948613CCAB451EE469B9FD5A87313393398BECD541BDA3126406BEB；这些条款支持物理量映射，未找到 Label/八进制的正式定义 |
| P5 | [SpCPT_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPT_define.m:53)：AdahruPosLabelGap 及经纬度粗值、细值视为一帧的时间间隔注释 | 49AAF3CBA343CA4B8E1A4C4B8959B1BBD4824C56378C36B61B74119F5DB218B0；辅助支持 Label 用词，不能替代协议定义 |
| W15 | [AIM 官方 ARINC 429 Tutorial](https://www.aim-online.com/wp-content/uploads/2019/07/aim-tutorial-oview429-190712-u.pdf)：Label / Information Identifier 说明 | 2026-09-16 核对；支持通用协议中 Label 为 8 bit、三位八进制表达，不替代项目设备 ICD |

### 已答内容、待核实问题与学习表现

- 问题处理状态：**部分澄清**。已解释 L/标签含义、实际物理量示例及其与三路来源的区别；正式协议映射仍需项目接口依据。
- Q006-U1：尚未读取到直接规定本 ADAHRU 接口协议、标签进制、设备标识与全部字段映射的正式 ICD 条款。
- Q006-U2：原始报文接收/解码位置、各字段编码与缩放、经纬度粗细值组合细节不在本题核对范围，不能由字段名直接推定。
- 学习表现：**掌握程度未确认**。本轮提供命名解释，没有用户独立定位或复述证据。
- 下一步最小任务：围绕一个具体字段定位其需求、上游接口和当前消费者；若需要确认线缆协议或编码，再核对该设备 ICD。
- 写入范围：本问答记录 Q006 及索引；保留 Q001—Q005 原文，未修改工程文件或生成 sp 内中间文件。
- Delta-K route：project-only。

### 修订记录

- R001（2026-09-16）：新增 L+数字命名解释、7 个当前字段示例、数据标签与设备来源的区别，以及项目静态证据和通用 ARINC 429 知识之间的边界。

## Q007：哪里查询 Label 编号表，截图中的 For Each Subsystem1 做什么？

### 日期、问题与学习阶段

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户问题：查询 L+数字编号表，以及截图中六输入、单输出的 For Each Subsystem1 的功能。
- S1、S2：输入字段与接口查询、当前模型数据组织；关联 S3 的状态位编码和监控结果传递。承接 Q005 的三路来源和 Q006 的 Label 命名解释。
- 学习表现与问题处理状态分开；本轮为文件静态核对，没有执行模型、仿真或认定阶段验收。

### A. 编号表的查询入口

| 查询目的 | 入口与定位 | 使用边界 |
|---|---|---|
| 查询正式标准标签分配 | [SAE/AEEC：ARINC429P1-19-DL Data Labels](https://saemobilus.sae.org/standards/arinc429p1-19-dl-429p1-19-data-labels)，Attachment 1-1：Label Codes（Excel）；附件包还含 Equipment Codes、BCD/BNR Data | 产品介绍公开；正文获取受订阅/购买权限控制。本轮只核实目录，未购买或复制标准正文 |
| 免费学习和查看例表 | [Alta 官方 ARINC Protocol Summary](https://www.altadt.com/wp-content/uploads/dlm_uploads/2013/11/ARINC-Protocol-Summary.pdf#page=9)，印刷页 6 / PDF 第 9 页：Standard EQID/Label Definitions；下一页为编码示例 | 厂商教程及示例，不是完整项目接口定义 |
| 查本项目实际用了哪些字段 | P4 的 SpIn_Adahrus，以及下表 | 反映当前 SP 输入字段；原始协议编码、单位、分辨率、量程和周期需设备 ICD 对照 |

通用 ARINC 429 的 Label 按三位八进制表达；查标准表还需结合 Equipment ID（EQID，设备标识）。同一 Label 在不同 EQID 下可能有不同定义（W17），因此不能只查一个数字就替代设备接口控制文件。项目正式协议及进制声明的证据边界沿用 Q006-U1。

### B. 当前 SpIn_Adahrus 的 30 个 L 前缀字段

从 P4 的实际字典字段提取，按编号排序；中文仅解释字段名称，不据此补充单位、原始报文编码或坐标正方向。数组维度均为 3：同一物理量的三路 ADAHRU 来源。L310/L311 的元素是 SpData_Double，其他这些字段是 SpData_Single，内部均有 data/valid。

| 编号（保留原三位写法） | 当前字典字段 | 按名称解释的物理量 |
|---|---|---|
| 076 | L076_GPS_Height | GPS 高度 |
| 107 | L107_Angle_Of_Attack | 迎角 |
| 110 | L110_Sideslip_Angle | 侧滑角 |
| 112 | L112_Ground_Speed | 地速 |
| 203 | L203_Pressure_Altitude | 气压高度 |
| 204 | L204_Baro_Corrected_Altitude | 气压修正高度 |
| 205 | L205_Mach | 马赫数 |
| 206 | L206_Computed_Airspeed | 计算空速 |
| 210 | L210_True_Airspeed | 真空速 |
| 211 | L211_Comb_Altitude_Rate | 组合高度变化率 |
| 212 | L212_Altitude_Rate | 高度变化率 |
| 213 | L213_Static_Air_Temperature | 静温 |
| 242 | L242_Total_Pressure | 总压 |
| 245 | L245_Static_Pressure | 静压 |
| 310 | L310_Combined_Latitude | 组合纬度 |
| 311 | L311_Combined_Longitude | 组合经度 |
| 320 | L320_Magnetic_Heading | 磁航向 |
| 321 | L321_True_Heading | 真航向 |
| 322 | L322_Mag_Declination | 磁偏角 |
| 324 | L324_Pitch_Angle | 俯仰角 |
| 325 | L325_Roll_Angle | 滚转角 |
| 331 | L331_Body_Longit_Accel | 机体纵向加速度 |
| 332 | L332_Body_Lateral_Accel | 机体横向加速度 |
| 333 | L333_Body_Normal_Accel | 机体法向加速度 |
| 336 | L336_Pitch_Attitude_Rate | 俯仰姿态角变化率 |
| 337 | L337_Roll_Attitude_Rate | 滚转姿态角变化率 |
| 340 | L340_Heading_Angle_Rate | 航向角变化率 |
| 365 | L365_Vertical_Velocity | 垂直速度 |
| 366 | L366_North_Velocity | 北向速度 |
| 367 | L367_East_Velocity | 东向速度 |

该总线还有 AdaComb_Altitude_Rate，共 31 个字段；它不属于 L 前缀编号表。字典当前没有 L307 字段，而需求中有 L307 纬度细值条款（见 Q006）：此处是 SP 输入字段表，不宣称覆盖设备在线路上传输的全部标签。尤其 L336/L337/L340 的名称是姿态角/航向角变化率，不能仅凭名称直接等同于机体角速率 p/q/r。

### C. 截图模块定位与主要功能

截图与 P21 的下列块及输入连线一致：

    SignalProcessingLib/Calc_SignalState_Double/For Each Subsystem1
    父系统 system_2563.xml；子系统 SID 2566 / system_2566.xml

其功能是对三路数据逐路执行同一套状态编码与总线组装，形成三个 Bus_DoubleSignalNormalInfo 元素。模块外部端口名为 state，但输出包含数值、有效性及状态信息，不能把整条输出当成单一布尔标志或单一整数。

六个输入均设置 Partition=on、PartitionDimension=1、PartitionWidth=1、PartitionOffset=0；输出 ConcatenationDimension=1。结合父系统长度 3 的输入，它按相同下标分别取一路数据及标志，组装后按原顺序拼成三元素总线数组（W14 的 For Each 语义）。

    第 1 路 data/valid/监控标志 → 第 1 个状态数据包
    第 2 路 data/valid/监控标志 → 第 2 个状态数据包
    第 3 路 data/valid/监控标志 → 第 3 个状态数据包
                            → state：三个元素组成的总线数组

这是同次调用中的逐元素处理，不表示相隔三拍，也未证明实际执行采用多核并行。图标的 N 表示对元素/分区重复应用内部逻辑；右侧 Outport 中的 1 是端口号，不是输出数组长度。

### D. 各输入的用途

| 图中输入 | 当前连线及用途 |
|---|---|
| data / [Data] | 原始数值；逐路写入输出 data，不在此处选源或求平均 |
| validOut / [Vld] | 上游数据有效性；原样写入输出 valid，并参加状态编码。validOut 在该模块左侧实际是输入端口名称 |
| lastTimestamp | 父系统 Constant SID 2565 固定输入 uint32([0 0 0])；每路写入 0。本模块没有生成当前时间戳或由此检查超时 |
| isSynced / [isASynced] | 来自前级 CmpMonitor 的同步/一致监控结果；写入输出 isSynced，并参加状态编码。[isASynced] 是外层 Goto/From 标签名，实际正向连到 isSynced |
| isMissComp / [isMissCompare] | 前级给出的失配标志；参加状态编码 |
| isBanned / [isBanned] | 前级给出的禁用标志；参加状态编码 |

三路比较、计数和禁用锁存由前级 CmpMonitor 完成（父系统 Subsystem SID 2602 内的 CmpMonitor SID 2605）。截图模块接收其结果并组装输出；本模块内部没有进一步的跨路比较、计数、Delay 或禁用锁存逻辑。

### E. 输出结构与当前实际状态编码

Bus Creator SID 2574 先组装 tSignalState，Bus Creator SID 2573 再组装 Bus_DoubleSignalNormalInfo。当前读到的两个类型分别定义于 P23、P22，均不在 P4 的 SP_Step_New.sldd 中。P22 的 data/chunk0.xml 中 Subdictionary 引用 SP_State.sldd；沿当前主线字典的磁盘引用闭包检查，这两个类型各只有一个 DD.ENTRY 定义。该检查不等于 MATLAB 运行时路径和工作区覆盖验证。

    state(i)                           Bus_DoubleSignalNormalInfo，i=1..3
      state                            tSignalState
        lastTimestamp                  uint32，当前写入 0
        stateBits                      uint32，四个布尔标志的编码结果
      data                             double，本路原始值
      valid                            boolean，本路输入有效性
      isSynced                         boolean，本路同步/一致监控结果

SignalStateSet_New（SID 2582 引用库块；实现 system_2514.xml）以 uint32 的 1、2、4、8 分别乘四个布尔标志，再相加：

    stateBits = valid + 2*isSynced + 4*isMissCompare + 8*isBanned

| 位（从 0 起） | 权重 | 当前 SignalStateSet_New 实现 |
|---|---:|---|
| bit0 | 1 | valid |
| bit1 | 2 | isSynced |
| bit2 | 4 | isMissCompare |
| bit3 | 8 | isBanned |

例如 valid=true、isSynced=true、另两项 false 时，stateBits=3（二进制低四位 0011）。这是本模块状态编码的示意，不是仿真结果，也不能脱离状态组合直接把任意整数统称为“故障编号”。valid 和 isSynced 同时作为独立字段保留，这是当前总线结构；其设计理由未在本题中确认。

### F. 发现的状态位描述差异：待复核

P23 的 tSignalState.stateBits.Description 写的是另一套含义：

| 位 | P23 字典文字描述 | 当前 P21 打包实现 |
|---|---|---|
| bit0 | valid | valid |
| bit1 | updated | isSynced |
| bit2 | isTimeout | isMissCompare |
| bit3 | isAbnormal | isBanned |
| bit4 | isOutOfRange | 该块不置位 |
| bit5 | isMissCompare | 该块不置位 |
| bit6 | isBanned | 该块不置位 |

这是文件中可直接观察到的描述与实现差异，不能把两套定义混写。上文对截图的解释依据当前实际块和连线；字典说明需与正式接口约定及下游解析一起复核。本轮未进行 MATLAB 编译解析或下游全链路核对，不能据此直接认定运行时产生故障。未修改字典、模型或设计说明。

本节补充 Q005 中状态编码的证据边界：Q005 所列四位公式仍对应当前实现，但不表示字典文字定义与实现已经一致。

### 来源与版本

工程基线：AC102_Rebuild，提交 a895a43a19209c076e679fcc2e2f8953e9f49bb4。仅离线读取 ZIP/XML 和公开资料，未初始化 MATLAB、编译、仿真、生成代码或修改 sp。

| ID | 来源与精确锚点 | SHA-256 / 边界 |
|---|---|---|
| P4 | [SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)：data/chunk0.xml，SpIn_Adahrus.Elements_internal | 10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC |
| P21 | [SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)：system_2563.xml 的 SID 2564/2565/2566；system_2566.xml 的 SID 2567—2583；system_2514.xml 状态编码；system_2602.xml 的 CmpMonitor SID 2605 | 0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490 |
| P22 | [DD_SpLib.sldd](D:/666/VE25/sp/04_Design/common/specification/data/DD_SpLib.sldd)：data/chunk0.xml，Bus_DoubleSignalNormalInfo.Elements_internal | 557DBF7AE4B2D8C13B876DAEFACF648F64E2F32680621F7F2FFCE5EEE59D367D；state/data/valid/isSynced 均为标量字段 |
| P23 | [SP_State.sldd](D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd)：data/chunk0.xml，tSignalState.Elements_internal，stateBits.Description | 10B55E65511E21A7AD3E376653F985FAFAD2C37CB6D1B3FB8A187E69062B0164；lastTimestamp/stateBits 均 uint32 标量；lastTimestamp.DocUnits 原文为 Ms，本例只写零值 |
| W14 | [MathWorks：For Each](https://www.mathworks.com/help/simulink/slref/foreach.html)：Description、Partition Input Signals、Concatenate Output | 2026-09-16 核对；按分区逐元素处理和输出拼接语义 |
| W16 | [SAE/AEEC：ARINC429P1-19-DL](https://saemobilus.sae.org/standards/arinc429p1-19-dl-429p1-19-data-labels)：Scope / Content | 2026-09-16 核对公开目录，未访问受限标准正文 |
| W17 | [Alta 官方教程](https://www.altadt.com/wp-content/uploads/dlm_uploads/2013/11/ARINC-Protocol-Summary.pdf#page=9)：Standard EQID/Label Definitions，印刷页 6 | 版本 A1，2010-04-07；2026-09-16 核对。通用协议知识及例表，不替代设备 ICD |

### 已答内容、待核实问题与学习表现

- 问题处理状态：**部分澄清**。已给出查询入口、本项目完整的 L 前缀字段表及截图模块静态功能；发现的状态位定义差异仍待复核。
- Q007-U1：正式设备 ICD 与标准表的 EQID、标签、单位/编码映射未全部核实，沿用 Q006-U1/U2。
- Q007-U2：需对照当前实际字典解析结果、正式接口约定和下游 stateBits 消费者，确定字典描述与四位实现的差异应如何处理；本轮不作运行故障判断。
- 学习表现：**掌握程度未确认**。没有用户独立查表、解释输出结构或推演编码的证据。
- 下一步最小任务：若继续追踪该模块，选一路 state 输出，核对消费者如何读取 valid、isSynced 与 stateBits；运行验证需在相应授权范围内开展。
- 写入范围：仅本问答记录 Q007 及索引；保留 Q001—Q006 正文。sp 无本轮改动或新增中间文件。
- Delta-K route：project-only。

### 修订记录

- R001（2026-09-16）：新增正式/免费标签查询入口、当前 30 个 L 前缀字段及 For Each 状态组装解释；记录四位打包实现与字典七位描述的差异，保留后续接口/消费者复核项。

## Q008：总线字段 Dimensions 都为 1，为什么还能输出三路数据？

### 日期、问题与学习阶段

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户问题：截图中的 Bus_DoubleSignalNormalInfo 四个字段 Dimensions 都为 1，而前述输出应有三路，二者是否矛盾。
- S1、S2：总线类型、总线数组及信号尺寸的层级区别；关联 S3 的 For Each 按元素处理。承接 Q005、Q007。
- 问题处理状态：已解答（概念与静态配置）；学习表现：掌握程度未确认。

### 核心结论

截图定义的是“一份 Bus_DoubleSignalNormalInfo 中各字段的大小”。本例的三路输出则是在外层放三份这样的总线。字段大小为 1 与外层数组长度为 3 可以同时成立。

术语需区分：此处“三路”或“长度为 3”更准确。Dimensions=3 表示长度为 3 的一维向量，不表示有三个维度的数组；真正的三维数组需要三个方向的尺寸，例如 [2 3 4]。截图中的 Dimensions=1 表示该字段只有一个元素，不能只把这个数字读成“这是第几维”。

### A. 截图描述一份结果的内部结构

| 字段 | 类型 | Dimensions=1 的含义 |
|---|---|---|
| state | Bus: tSignalState | 一份 tSignalState；它内部仍包含 lastTimestamp、stateBits 两个字段 |
| data | double | 一个数值 |
| valid | boolean | 一个有效性标志 |
| isSynced | boolean | 一个同步/一致性标志 |

总线对象定义每个字段的名称、类型和形状；它可作为多份总线元素共同使用的类型。截图不会因为外部用了三份该类型，就自动把类型内部的标量字段改成 3（W18、W19）。

### B. 外部三路结构长什么样

以下 out 只是为避免外部端口 state 与内部字段 state 重名而使用的示意名称，不是改名后的模型信号：

    out：长度为 3 的总线数组
      out(1)：{state, data, valid, isSynced}   第 1 路的一份结果
      out(2)：{state, data, valid, isSynced}   第 2 路的一份结果
      out(3)：{state, data, valid, isSynced}   第 3 路的一份结果

因此 out(1).data 是第 1 路的一个数，out(2).data 是第 2 路的一个数。三份结果使用相同的 Bus_DoubleSignalNormalInfo 类型定义，各自拥有数值和状态。

| 结构 | 三路放在哪里 | 示意访问 |
|---|---|---|
| 本例：总线数组（array of buses） | 外层有三份总线，每份内部字段为标量 | out(1).data、out(2).data、out(3).data |
| 另一种可设计的结构：总线内含数组字段 | 外层一份总线，其 data 等字段各存三元素数组 | out.data(1)、out.data(2)、out.data(3) |

两种结构表达的字段层级不同。把截图里的 data.Dimensions 改成 3，会改变“每份结果内部”的结构，并不能当作对本例三路输出尺寸的修正。本轮没有修改类型或模型。

### C. 本例的 3 与 1 分别来自哪里

| 位置 | 当前静态配置 | 含义 |
|---|---|---|
| Calc_SignalState_Double/DataIn1，SID 2564 | Bus: SpData_Double；PortDimensions=3 | 输入有三个来源元素 |
| For Each Subsystem1 的六个输入，SID 2567—2572 | Partition=on；Dimension=1；Width=1；Offset=0 | 每次取相同下标的一个元素 |
| 内部 Bus Creator，SID 2573/2574 | Bus_DoubleSignalNormalInfo / tSignalState | 每次组装一份本路结果 |
| 内部 Outport，SID 2583 | ConcatenationDimension=1 | 沿第 1 维拼接各次结果 |

由长度为 3 的输入与上述分区、组装、拼接配置，可以静态解释为生成三份总线元素。ConcatenationDimension=1 指定拼接方向，不是指定只输出一份；最终元素个数来自分区个数及各次结果的尺寸。

可在 For Each 的输入分区和输出拼接配置中核对上述设置；模型信号尺寸显示反映外层信号形状，总线类型编辑器显示内部字段定义，二者观察层级不同。本轮没有编译模型，未将静态推导表述为已核验实时编译尺寸。

### 来源与版本

沿用当前 AC102_Rebuild、提交 a895a43a19209c076e679fcc2e2f8953e9f49bb4。P21/P22 的当前文件哈希与 Q007 一致，复用已完整读取的 XML；未执行 MATLAB 初始化、编译或仿真。

| ID | 来源与精确锚点 | 版本/边界 |
|---|---|---|
| P22 | [DD_SpLib.sldd](D:/666/VE25/sp/04_Design/common/specification/data/DD_SpLib.sldd)：data/chunk0.xml，Bus_DoubleSignalNormalInfo.Elements_internal 的 Dimensions | SHA-256：557DBF7AE4B2D8C13B876DAEFACF648F64E2F32680621F7F2FFCE5EEE59D367D；四字段均 1 |
| P21 | [SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)：system_2563.xml 的 SID 2564；system_2566.xml 的 SID 2567—2583 | SHA-256：0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490；输入长度、分区、总线组装及拼接设置 |
| W18 | [MathWorks：Simulink.BusElement](https://www.mathworks.com/help/simulink/slref/simulink.buselement.html)，Dimensions、DataType | 2026-09-16 核对；属性描述总线内部元素 |
| W19 | [MathWorks：Group Nonvirtual Buses in Arrays of Buses](https://www.mathworks.com/help/simulink/ug/combining-buses-into-an-array-of-buses.html) | 2026-09-16 核对；相同总线类型构成数组，等价于结构体数组 |
| W14 | [MathWorks：For Each](https://www.mathworks.com/help/simulink/slref/foreach.html)，输入分区和输出拼接 | 沿用 Q007 已核对的官方语义 |

### 后续与学习表现

- 本题的类型层级与静态维度疑问已解释。实际编译尺寸不在本轮只读核对范围。
- Q007 中状态位文字描述与实现的差异仍独立待复核，不影响本题对字段大小与外层数组长度的区分。
- 学习表现：掌握程度未确认；用户提出层级疑问，本轮没有独立定位或复述结果。
- 下一步最小任务：在后续学习中分别定位“单路类型定义”和“外层三路信号尺寸”，说明 out(2).data 与 out.data(2) 的区别。
- 写入范围：仅本问答记录 Q008 和索引，保留 Q001—Q007 正文；工程文件未改动，sp 无新增中间文件。
- Delta-K route：project-only。

### 修订记录

- R001（2026-09-16）：补充单份总线字段尺寸与总线数组长度的区别，校准“三维/三路”术语，列出当前 For Each 分区及拼接依据。

## Q009：For Each 怎样把三份总线拼成输出数组？

### 日期、阶段与结论

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户问题：承接 Q008，三路总线结果的拼接过程如何实现。
- 阶段：S2 的数据流与接口组织；关联 S3 的 For Each 分区及输出机制。
- 问题处理状态：已解答（机制与静态配置）；学习表现：掌握程度未确认。

### 实现过程：取一路、组成一份、放入对应输出位置

拼接由 For Each 子系统的输出机制自动完成。内部 Bus Creator 把当前分区的不同字段组成一份总线，For Each 再按输出拼接维度收集各分区的总线结果。

1. 六个输入均按第 1 维分区，宽度 1、偏移 0。对本例长度为 3 的输入，第 i 次处理同时取得 data(i)、validOut(i)、lastTimestamp(i)、isSynced(i)、isMissComp(i)、isBanned(i)。这些字段按同一下标配对。
2. SignalStateSet_New 生成该路 stateBits；Bus Creator1（SID 2574）组合 lastTimestamp 与 stateBits，形成一份 tSignalState。
3. Bus Creator（SID 2573）组合 state、data、valid、isSynced，形成一份非虚拟 Bus_DoubleSignalNormalInfo。此时各字段依然是单路大小。
4. 这份总线直接进入内部 Outport（SID 2583）。其 ConcatenationDimension=1 使 For Each 沿第 1 维收集各次结果；三次分区各生成一份，外部得到三个元素。

    固定在同一次调用：
    第1路输入 → 内部处理/Bus Creator → out(1)
    第2路输入 → 内部处理/Bus Creator → out(2)
    第3路输入 → 内部处理/Bus Creator → out(3)
                                  → 外部 out：三元素总线数组

out 是说明用名称，模型外部实际端口名为 state。局部的 Bus Creator 输出线承载本次分区的一份结果；整个 For Each 子系统的外部输出线承载收集后的数组。

### 两种组合分别发生在哪里

| 操作 | 负责者 | 组成的内容 |
|---|---|---|
| 当前一路的字段打包 | 内部 Bus Creator | 一份 {state, data, valid, isSynced} |
| 三路同类型结果的数组拼接 | For Each 的输出拼接机制 | out(1)、out(2)、out(3) 三份总线 |

当前 system_2566.xml 中，SID 2573 直接接 SID 2583，没有额外的 Vector Concatenate 或 Mux 块负责这一步。父级其他模块中的 Concatenate 块有各自的数据流用途，不能将它们与这里的输出机制混为一谈。

### 怎样在模型中找到设置

进入 Calc_SignalState_Double/For Each Subsystem1，打开里面的 For Each 控制块（SID 2575）：

- Input Partition：本例六路输入的 Partition 开启，Partition Dimension=1、Partition Width=1、Partition Offset=0。
- Output Concatenation：查看输出端口对应的 Concatenation Dimension=1。
- 该输出拼接属性在保存文件中位于内部 Outport SID 2583：P Name="ConcatenationDimension"，值为 1。

这里的 1 指拼接方向；结果有几个元素由输入分区数量和每次产生的结果大小决定。本例三次分区、每次一份，因而得到三份。

### 便于理解的伪代码

下列仅是数据对应关系示意；packOneChannel 表示内部现有编码和两级 Bus Creator 的整体功能，不是工程中的实际 MATLAB 函数。

    本次调用内，out 固定包含 3 份结果：
    for i = 1..3
        out(i) = packOneChannel(
            data(i), validOut(i), lastTimestamp(i),
            isSynced(i), isMissComp(i), isBanned(i))
    end

输出表达的是每路结果放入哪个位置，没有跨采样周期逐个追加或累计历史的含义；一次调用内生成本次的三路结果。此示意也不证明实际生成代码采用 for 循环、循环展开或特定内存分配方式。本轮没有编译或生成代码。

### 来源与核对

- 工程基线：AC102_Rebuild，提交 a895a43a19209c076e679fcc2e2f8953e9f49bb4。
- P21：[SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)，simulink/systems/system_2566.xml。本轮重新读取 SID 2567—2583 的分区、总线类型、NonVirtualBus=on、拼接参数及 SID 2573 → 2583 的直接连接；SHA-256 仍为 0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490。
- W14：[MathWorks：For Each](https://www.mathworks.com/help/simulink/slref/foreach.html)，Description、Partition Input Signals、Output Concatenation；2026-09-16 核对，说明各分区结果按指定方向组成输出。
- W20：[MathWorks：Bus Creator](https://www.mathworks.com/help/simulink/slref/buscreator.html)，2026-09-16 核对，说明输入字段组装成总线的功能。
- 未执行初始化、编译、仿真或代码生成；“三元素输出”依据输入声明与分区/拼接配置静态解释。

### 后续与记录范围

- 学习表现：掌握程度未确认；没有用户独立定位 Output Concatenation 设置或复述数据路径的证据。
- 下一步最小任务：在模型中分别找到负责“单路字段打包”的 Bus Creator 和负责“跨分区收集”的输出拼接设置。实际生成代码的实现方式需另行查看已生成代码或在获准范围内生成验证。
- 仅追加本问答记录 Q009 及索引，保留 Q001—Q008 正文；未修改工程文件，sp 无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-16）：记录输入分区、逐路总线组装、输出收集的具体路径和可查看的参数位置，给出同次调用内的数据关系示意。

## Q010：For Each 是否调用循环函数，为什么外形不同于普通子系统？

### 日期、阶段与结论

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户追问：收集三路结果是否通过调用某个循环函数实现；For Each 子系统为什么与一般子系统外形不同。
- 关联：S2 的模块组织；S3 的执行机制；补充 Q009“谁负责重复执行与收集”的问题。
- 问题处理状态：已解答（内建机制与静态模型）；学习表现：掌握程度未确认。

### 1. 执行机制由内部 For Each 控制块声明

该模块属于 Simulink 的 For Each Subsystem，是具有按分区重复执行规则的子系统。模型作者用 For Each 控制块声明输入如何分区、输出如何拼接；Simulink 识别该控制块后，按这些规则执行内部算法并形成外部输出。

本例外围块仍为 BlockType=SubSystem（SID 2566），内部有 BlockType=ForEach 的控制块（SID 2575）。因此决定其特殊执行方式的是实际控制块和配置，不是仅把名称改为 For Each。

这里无需调用一个名为 for_each() 的 MATLAB 用户函数。For Each 控制块也不携带供用户填写的 MATLAB 循环脚本：它是 Simulink 内建的执行控制声明。若从普通子系统的信号连线中寻找“回到开头的循环反馈线”，也找不到该迭代机制的定义位置；该规则在控制块中配置。

### 2. 三路如何处理和收集

沿用 Q009 核对的配置，输入含三个元素、每次分区宽度为一。一次子系统调用中的数据关系可写成：

    第 i 个分区的六个输入
          ↓
    内部现有算法与 Bus Creator
          ↓
    本路结果 y_i
          ↓
    For Each 输出规则：放入外部 out 的第 i 个位置

    i 对应第 1、2、3 路，外部 out 因而包含三份结果。

从用户建模的角度，输入分区索引确定本次处理哪一路，输出拼接规则确定本次结果在外部数组中的位置。内部不需要再画一个 Vector Concatenate 块或手写逐次追加数组的函数。

可把行为类比为“对每路应用同一算法并得到 out(i)”，但这不指定 Simulink 引擎内部的源码函数名，也不等于该项目生成代码中必定存在某个 C for 循环。实际函数名、内联/复用和循环表达需查看生成代码；本轮未编译或生成代码。

### 3. 与普通子系统以及外观的区别

| 项目 | 普通 Subsystem | 本例 For Each Subsystem |
|---|---|---|
| 外层基本块类型 | SubSystem | 同样为 SubSystem |
| 按输入分区重复内部算法 | 普通容器不自动添加这套机制，处理由内部块定义 | 内部 ForEach 控制块声明该机制 |
| 输出组织 | 由内部算法、连线和端口定义 | 单个分区的结果还按输出拼接规则形成数组 |
| 内部算法视角 | 取决于所建算法和输入尺寸 | 本例只需处理一路数据 |

外观应与功能分开看：

- 叠层、N 等迭代图标用于标示 For Each 子系统；N 表示重复应用内部算法的概念，不代表固定为三次或实际多核并行。
- 本例 BackgroundColor=green，绿色属于项目设置的显示颜色；颜色不定义循环规则。
- ContentPreviewEnabled=on，启用了内容预览；外层块的长宽和预览属于显示设置。
- 如果内部包含有历史状态的块，For Each 按分区分别保持状态（W21）；当前截图这段打包逻辑本身没有 Delay，不能把输出字段 state 与“动态历史状态”混为一谈。

### 4. 在模型里观察的位置

进入 Calc_SignalState_Double/For Each Subsystem1，可找到内部的 For Each 控制块；打开它查看 Input Partition 与 Output Concatenation。Simulink 库位置为 Ports & Subsystems / For Each Subsystem（W21）。本轮只读核对，没有更改控制块或搭建新模型。

### 来源、边界与记录

- P21：[SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)，simulink/systems/system_2563.xml 的 SID 2566（SubSystem、BackgroundColor、ContentPreviewEnabled），system_2566.xml 的 SID 2575（ForEach）。本轮重新读取上述 XML；SHA-256：0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490。
- W21：[MathWorks：For Each Subsystem](https://www.mathworks.com/help/simulink/slref/foreachsubsystem.html)，Description、Configure the Subsystem、Code Reuse Support；2026-09-16 核对。支持同一仿真时间步内对分区重复处理、控制块定义规则及输出拼接；不作为本项目已生成代码形态的证据。
- 当前工程仍为 AC102_Rebuild，提交 a895a43a19209c076e679fcc2e2f8953e9f49bb4；未初始化、编译、仿真或生成代码。
- 学习表现：掌握程度未确认；用户继续追问实现机制，没有独立定位控制块或解释执行规则的证据。
- 后续最小任务：需要观察真实代码层面的循环/函数形式时，核对当前构型对应的生成代码；不能只根据图标猜测。
- 本轮仅追加问答记录 Q010 和索引，保留 Q001—Q009；工程文件未修改，sp 无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-16）：明确 For Each 控制块与 Simulink 内建执行机制的关系，区分模型迭代规则、外观设置和代码生成的具体实现。

## Q011：如何创建并配置这种 For Each Subsystem？

### 日期、阶段与结论

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户问题：如何创建前述 For Each 子系统。
- 阶段：S2 的模型结构与接口；关联 S3 的分区与输出设置。操作步骤说明不等于已完成 S6-B 实践验证。
- 问题处理状态：已解答（操作指引）；学习表现：掌握程度未确认。

### 1. 从库中创建模板

在 Simulink Library Browser 中进入：

    Simulink → Ports & Subsystems → For Each Subsystem

将 For Each Subsystem 拖入目标模型。双击进入，模板内已经有 For Each 控制块和输入、输出端口。保留控制块，在内部搭建“只处理一路数据”的算法。选择名称为 For Each Subsystem 的模板，避免混淆 For Iterator Subsystem。

如果已有普通子系统中的单路算法，可以新建上述模板，将单路算法及所需端口复制到模板内，避免保留重复端口，再按下述方法配置。MathWorks 的 Repeat an Algorithm 示例采用了这一迁移思路（W22）。本轮仅提供步骤，没有在用户工程中执行这些操作。

### 2. 对照当前六输入模块设置端口

前述状态组装例子内部需要六个 Inport 和一个 Outport，输入名称依次为：

    data、validOut、lastTimestamp、isSynced、isMissComp、isBanned

内部保留单路的状态编码与两级 Bus Creator 逻辑，具体连接见 Q007、Q009。这里的六个输入端口分别代表六类数据；每个外部输入又包含三路来源元素，两种数量不能混淆。

### 3. 配置输入分区与输出拼接

双击内部的 For Each 控制块，打开 Input Partition 页。对本例六个输入分别设置：

| 参数 | 本例设置 |
|---|---|
| Partition | 勾选 |
| Partition Dimension | 1 |
| Partition Width | 1 |
| Partition Offset | 0 |

在 Output Concatenation 页，将输出的 Concatenation Dimension 设为 1。保持处理全部分区的默认行为；六个外部输入各有三个元素，且均按上述规则分区时，即对应三次分区处理，不需要另写循环函数。

各个被分区的输入应产生一致的分区数量。本例使用长度为 3 的一维信号；若另建练习使用真正的二维行矩阵，其分区方向需按实际形状选择，不能只根据三个数就机械套用 Dimension=1。

### 4. 输出本例的总线数组

- 内部 Bus Creator1 将 lastTimestamp 和 stateBits 组成 tSignalState。
- 外层 Bus Creator 将 state、data、valid、isSynced 组成 Bus_DoubleSignalNormalInfo。
- 按当前模型配置，两级 Bus Creator 的 Output data type 分别使用 Bus: tSignalState 和 Bus: Bus_DoubleSignalNormalInfo，并启用 Output as nonvirtual bus。
- 外层 Bus Creator 输出接内部 Outport；For Each 收集各分区结果，形成三元素总线数组。
- 总线类型描述一份结果；各字段 Dimensions 仍为 1，见 Q008。

在当前 SP 工程环境中使用这些现有类型；独立新建模型时，必须让模型能够解析所用总线类型定义。本指引没有在其他目录复制工程字典，也没有执行工程初始化。

绿色背景、尺寸及内容预览可按显示需要设置，不决定 For Each 的执行机制。完成建模后需要核对接口、数据类型与实际尺寸；本轮没有代替用户执行更新模型或仿真，也未将步骤当作运行通过证据。

### 来源与记录边界

- W21：[MathWorks：For Each Subsystem](https://www.mathworks.com/help/simulink/slref/foreachsubsystem.html)，Libraries、Configure the Subsystem；2026-09-16 核对，支持库入口及控制块配置方式。
- W14：[MathWorks：For Each](https://www.mathworks.com/help/simulink/slref/foreach.html)，Input Partition / Output Concatenation；支持分区与输出参数含义。
- W22：[MathWorks：Repeat an Algorithm Using a For-Each Subsystem](https://www.mathworks.com/help/simulink/ug/repeat-an-algorithm-using-a-for-each-subsystem.html)，Repeat an Algorithm；2026-09-16 核对，示例将已有算法复制到新建 For Each 模板并启用输入分区。
- W19：[MathWorks：Group Nonvirtual Buses in Arrays of Buses](https://www.mathworks.com/help/simulink/ug/combining-buses-into-an-array-of-buses.html)，Requirements of Arrays of Buses；相同类型的非虚拟总线组成数组。
- P21/P22：当前工程六输入及总线类型配置沿用 Q007—Q010 的已读来源和锚点；本题没有新建或修改模型。
- 学习表现：掌握程度未确认；未收到用户完成搭建、参数核对或运行结果。
- 下一步最小任务：按模板定位内部 For Each 控制块，并核对 Input Partition / Output Concatenation 设置；工程修改和运行不由本轮操作指引自动宣称完成。
- 仅追加学习问答记录 Q011 与索引，保留 Q001—Q010；sp 无本轮改动或新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-16）：记录创建入口、当前六输入分区参数、非虚拟总线输出及复制已有单路逻辑的方法。

## Q012：四状态编码公式有什么依据，为什么采用 1、2、4、8？

### 日期、阶段与结论

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户问题：stateBits = valid + 2*isSynced + 4*isMissCompare + 8*isBanned 的参考依据与设计理由。
- 阶段：S3 的状态编码、接口定义与需求追踪；关联 S2 的数据组织。
- 结论层级：二进制编码原理明确；这些状态有项目需求依据；四个标志的具体位序仍需正式接口和消费者一致性核对。
- 问题处理状态：部分澄清；学习表现：掌握程度未确认。

### A. 1、2、4、8 是二进制位权

四个输入均为 boolean，取值为 0 或 1。分别乘以 2 的不同幂，就把各个标志放在互不重叠的位上：

| 标志 | 权重 | 置位时的低四位 | 占用位置（从 0 起） |
|---|---:|---|---|
| valid | 1 = 2^0 | 0001 | bit0 |
| isSynced | 2 = 2^1 | 0010 | bit1 |
| isMissCompare | 4 = 2^2 | 0100 | bit2 |
| isBanned | 8 = 2^3 | 1000 | bit3 |

这是位标志编码。权重表示存放位置，不表示严重程度或算法重要性；8 不表示禁用比有效性重要八倍。

各项只可能贡献一个不同的位，彼此不会重叠或产生进位，所以在这四个布尔输入的前提下，加法与按位 OR 合并等价。四位可唯一表示 16 种布尔组合，对应整数 0—15；这描述编码容量，不意味着上游逻辑的全部 16 种组合都实际可达。

若简单使用 1、2、3、4 作为四个系数，就会有歧义：第一、第二个标志同时为真得到 1+2=3，与仅第三个标志为真相同。采用不同二进制位权可分别恢复每个标志。

示例：valid=1、isSynced=0、isMissCompare=1、isBanned=0 时：

    stateBits = 1 + 0 + 4 + 0 = 5
    低四位 = 0101

这表示保留“上游有效”与“跨路比较失配”两个标志。它们描述不同层面的状态，不能仅因为 valid 为真就认定不存在失配。

按这一布局，可用如下 MATLAB 表达式判断失配位：

    bitand(stateBits, uint32(4)) ~= 0

该表达式是位掩码读取方法的示例，不是本次找到的工程消费者代码。按位合并/读取的通用语义参见 W23、W24。

### B. 状态项有明确需求依据

在用户指定的当前需求工作簿 P20，工作表“信号处理软件高级需求-SP_SW_HLR”：

| 条款 | 单元格 | 与本题相关的要求 |
|---|---|---|
| SP_SW_HLR-257 | A134 / C134 | 状态表应包含有效位，并列出 Synced、MissCompare、banned 等状态，还包括重置、更新、超时、设备异常、数值异常 |
| SP_SW_HLR-165 | A99 / C99 | 比较监控分别输出三个通道的 Synced、MissCompare、Banned |
| SP_SW_HLR-170 | A105 / C105 | 通道 1 的失配状态计算规则 |
| SP_SW_HLR-171 | A106 / C106 | 通道 1 的禁用条件及上一时刻已禁用时保持禁用 |
| SP_SW_HLR-176 | A107 / C107 | 通道 1 的同步状态计算规则 |
| SP_SW_HLR-261 | A137 / C137 | 导航信号及监控状态详细定义指向 SpNavMdl_IO.sldd、SP_State.sldd |

这些需求支持“要保存哪些状态及其行为”，不能自动推出“isSynced 必须放 bit1”。本轮对该工作表单元格文本的定向检索，没有找到 stateBits 名称、此四位布局或 1/2/4/8 公式的直接规定；未检查表内图片文字。需求中沿用的字典名称还需对照当前主线位置与版本，不据旧名称转用旧模型。

### C. 公式的直接实现依据

P21 的 SignalStateSet_New，根块 SID 2514、simulink/systems/system_2514.xml：

| 输入 | 输入 SID | uint32 常量 / SID | Product SID |
|---|---:|---|---:|
| valid | 2515 | 1 / 2524 | 2532 |
| isSynced | 2520 | 2 / 2529 | 2537 |
| isMissCompare | 2521 | 4 / 2530 | 2538 |
| isBanned | 2522 | 8 / 2531 | 2539 |

四个 Product 输出进入 Add SID 2523 的四个输入，再送往 uint32 类型的 stateBits Outport SID 2540。四个源输入明确声明为 boolean。

当前 Double 包装中的 system_2566.xml，引用块 SID 2582 的 SourceBlock=$bdroot/SignalStateSet_New。因此该公式是对实际块和连接的概括，不能将它当成另行规定或修改的算法。

### D. 为什么采用这种表示方式

从接口设计角度，这种方式有以下作用；它们是技术解释，不是本次发现的设计决策记录：

- 用一个整数保存多个可同时存在的标志，保留各标志信息。
- 使用按位掩码检查单个或组合状态，便于不同消费者遵循统一位定义。
- 接口可以约定固定状态字，并在明确版本规则后分配其他位。

具体哪个标志占哪一位属于双方共享的接口约定。换一组不重叠的位置也能编码，但编码端、读取端和接口文档必须同步，否则同一个数会被读成不同含义。

本次没有找到证据证明“这四个位序由 ARINC 429 或其他外部标准直接规定”，也没有读取到为何本项目选择 uint32、此位序和当前重复字段的完整决策记录。不能仅因打包成整数就宣称已经节省内存或带宽：当前总线还单独保留 valid、isSynced，实际占用需结合生成类型和对齐核对。

### E. 现有定义差异进一步核对

Q007 已记录 P23 字典描述与新编码块不一致。本轮进一步发现，同库还存在未带 _New 的 SignalStateSet（SID 378 / system_378.xml），它采用八位布局：

| bit | SignalStateSet_New 实现 | SignalStateSet 实现 | tSignalState.stateBits 的文字描述 |
|---:|---|---|---|
| 0 | valid | valid | valid |
| 1 | isSynced | updated | updated |
| 2 | isMissCompare | isTimeout | isTimeout |
| 3 | isBanned | isAbnormal | isAbnormal |
| 4 | 该块不置位 | isOutOfRange | isOutOfRange |
| 5 | 该块不置位 | isSynced | isMissCompare |
| 6 | 该块不置位 | isMissCompare | isBanned |
| 7 | 该块不置位 | isBanned | 未定义 |

所以，字典描述与这两个编码块都不完全对应，不能简单认定它“就是另一块的旧说明”。未带 _New 的块名称也不足以证明其当前是否弃用，本轮不作使用范围判断。

本轮在同一库中没有定位到 SignalStateGet_New/SignalStateGet 或相应的状态位解码链；未扩展为全工程消费者审计。因此目前证明的是“编码端这样写”，还未证明全部消费者按同一布局读取。该差异保持待复核，不凭静态差异直接判定运行故障，也不修改工程来源。

### 来源、边界与后续

| ID | 来源与锚点 | SHA-256 / 核对范围 |
|---|---|---|
| P20 | [当前需求工作簿](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)，上述工作表与单元格 | 共享只读快照 E3F28F03B948613CCAB451EE469B9FD5A87313393398BECD541BDA3126406BEB；单元格文本定向检索 |
| P21 | [SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)，system_2514/2566/378.xml | 0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490；编码块和同库读取逻辑查找 |
| P23 | [SP_State.sldd](D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd)，data/chunk0.xml，tSignalState.stateBits.Description | 10B55E65511E21A7AD3E376653F985FAFAD2C37CB6D1B3FB8A187E69062B0164；当前哈希与 Q007 已读描述一致 |
| W23 | [MathWorks：bitor](https://www.mathworks.com/help/matlab/ref/bitor.html) | 2026-09-16 核对；按位 OR 及位打包示例，支持通用原理，不规定本项目位序 |
| W24 | [MathWorks：bitand](https://www.mathworks.com/help/matlab/ref/bitand.html) | 2026-09-16 核对；按位 AND，支持读取示例 |

- 工程基线：AC102_Rebuild，提交 a895a43a19209c076e679fcc2e2f8953e9f49bb4；本轮离线只读解析，未初始化、编译、仿真或生成代码。
- Q012-U1：该四位布局的受控接口定义、选择理由和版本迁移依据待核实。
- Q012-U2：当前主线实际 stateBits 消费者与三套定义的对应关系待核实；Q007-U2 继续保留。
- 学习表现：掌握程度未确认；本轮没有用户独立编码/解码演算或来源定位结果。
- 下一步最小任务：围绕当前一个 stateBits 输出追踪消费者，检查实际按位读取规则并与指定接口版本比对。
- 写入范围：仅本问答记录 Q012 与索引，保留 Q001—Q011 正文；sp 无本轮改动或新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-16）：分开记录二进制原理、状态需求与位布局依据；补充另一编码块的八位布局，防止将字典七位说明误认成与该块一致。

## Q013：NormalInfos 表示什么意思？

### 日期、阶段与结论

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户问题：NormalInfos 字段的含义。
- 阶段：S1、S2 的术语、总线结构与输入到消费者的数据关系；承接当前 ADAHRU 预处理链。
- 问题处理状态：已解答（按结构与用途解释）；学习表现：掌握程度未确认。

### 在当前链路中的含义

结合当前模型，NormalInfos 可按“信号数据及状态信息集合”理解；Infos 是项目名称中表示多项信息的用法。这里的集合由预处理环节输出，按统一总线类型组织各物理量及各路来源的数值、有效性和监控状态。

Normal 可作“常规”的命名理解，但本轮读取的相关总线及字段 Description 均为空，没有找到它在项目中的正式英文释义。不能仅凭名字认定它表示所有数据已经正常、全部有效，或执行了数值归一化。

本轮在五个当前相关字典中没有找到精确名为 NormalInfos/NormalInfo 的 BusElement；当前链路实际使用的是：

- 端口/信号名：adahruNormalInfos。
- 集合类型名：BUS_adahruNormalInfos。
- 单个信号、单路结果的类型名：Bus_DoubleSignalNormalInfo / Bus_SingleSignalNormalInfo。

因此按当前上下文解释的是这些名称中的共同部分；并不宣称工程中任何位置都不存在同名字字段。

### 一份 NormalInfo 包含什么

P22 中两个单路类型结构相同，仅 data 的数值类型不同：

| 字段 | 含义 | 类型 / 单份尺寸 |
|---|---|---|
| data | 本路数值 | double 或 single，标量 |
| valid | 本路输入有效性 | boolean，标量 |
| isSynced | 本路同步/一致监控结果 | boolean，标量 |
| state | 状态信息 | 一份 tSignalState，含 lastTimestamp、stateBits |

isMissCompare、isBanned 等在前述当前实现中编码进 state.stateBits；位布局及字典描述差异继续参见 Q012，不在本题中宣称已解决。

数值与状态一起保留，所以某一份信息的 valid 可以为 false，或存在失配/禁用状态。NormalInfos 的名字不能取代这些标志的判断。前述 Calc_SignalState_Double 打包环节将输入 data 接入结果，并没有仅保留“正常通道”或将所有数据都变成 0—1。

### ADAHRU 集合怎样组织

P15 的 BUS_adahruNormalInfos 共 31 个物理量字段，每字段 Dimensions=3。CombLate、CombLong 使用 Bus_DoubleSignalNormalInfo，其他 29 项使用 Bus_SingleSignalNormalInfo。

    adahruNormalInfos
      CombLate(1..3)   三路组合纬度信息
      CombLong(1..3)   三路组合经度信息
      Pitch(1..3)      三路俯仰角信息
      Roll(1..3)       三路滚转角信息
      ...             其余物理量信息

例如，下列是结构访问示意：

    adahruNormalInfos.CombLate(2).data
        第 2 路 ADAHRU 的组合纬度数值

    adahruNormalInfos.CombLate(2).valid
        该路纬度的输入有效性

这里每个物理量仍保留三路结果，外部总集合本身是一份总线。P8 的输出端口 adahruNormalInfos（SID 139）PortDimensions=1，与各物理量内部长度为 3 并不矛盾。

### 实际连接与用途

P8 的 Calc_SignalState_Double SID 127 / 129 分别将纬度、经度的三路结果送入 Bus Assignment SID 138 的 CombLate、CombLong；其他信号也在该总线中组织，输出到 adahruNormalInfos SID 139。

P1 的 VE25SP/NavSpApp（system_54.xml）中：

    AdahruMsgsPreprocess（SID 25）
       → adahruNormalInfos
       → AdahruVoter（SID 44，实际引用 Calc_NavSourceSelection.slx）

该输出还分支到 SID 74。本题仅核对上述接口连接，没有开展完整选源算法审计。名称含义应结合这条实际数据流理解：它向后续模块提供各路信号值及其监控信息。

### 来源、边界与记录

| ID | 来源与锚点 | SHA-256 |
|---|---|---|
| P22 | [DD_SpLib.sldd](D:/666/VE25/sp/04_Design/common/specification/data/DD_SpLib.sldd)，data/chunk0.xml，Bus_DoubleSignalNormalInfo、Bus_SingleSignalNormalInfo.Elements_internal | 557DBF7AE4B2D8C13B876DAEFACF648F64E2F32680621F7F2FFCE5EEE59D367D |
| P15（扩展核对） | [SP_InteriorBus.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd)，data/chunk0.xml，BUS_adahruNormalInfos 的 31 个字段与维度 | 5356B8602E6B3948291CA5410F5CE444D1277E5141AB93C21CF77D11D81C01F5 |
| P8 | [AdahruMsgsPreprocess.slx](D:/666/VE25/sp/04_Design/AdahruMsgsPreprocess/specification/AdahruMsgsPreprocess.slx)，system_root.xml，SID 127/129/135/138/139 及对应连线 | A5F9E7FAC266448447F929CF4BDC5CFF6C9B322A81D479995C61799977E65143 |
| P1 | [VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)，system_54.xml，SID 25 → SID 44 输入 1，及 SID 74 分支；SID 44 的 ModelNameDialog | 20D8C7DE2A103D8FF298BCD5BB0C28F73AA6CCC2E27D43D0F5AB9952365F77E4 |

- 精确 BusElement 名检索范围：SP_Step_New、DD_SpLib、SP_State、SP_InteriorBus、SpMdlPrivate_DataType 五个相关字典；未将局部未检出概括为全工程不存在。
- 工程基线：AC102_Rebuild，提交 a895a43a19209c076e679fcc2e2f8953e9f49bb4；离线只读解析，无 MATLAB 初始化、编译、仿真或代码生成。
- 命名边界：Normal 的正式项目释义未见字典注释；上文按实际结构和用途解释。
- 学习表现：掌握程度未确认；未获得用户独立定位具体字段和消费者的证据。
- 下一步最小任务：需要继续追踪时选一个物理量，从三路 NormalInfo 查看其下游选源和状态使用方式。
- 仅追加本问答记录 Q013 与索引，保留 Q001—Q012；工程文件未改动，sp 无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-16）：记录 NormalInfos 的信号/类型名称位置、单路结构、三路物理量集合及下游连接，并限制“正常/归一化”的名称推断。

## Q014：ICD 指的是什么？

### 日期、阶段与结论

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户问题：ICD 的含义。
- 阶段：S1、S2 的接口术语与输入约定；关联 S3 状态接口。
- 问题处理状态：已解答；学习表现：掌握程度未确认。

ICD = Interface Control Document，通常译为“接口控制文件”或“接口控制文档”。它把设备、系统或软件模块之间的接口约定写清楚，使接口两端对传递内容和解释方法保持一致。

这里的 Control 指接口定义及其版本、变更受到管理。NASA 的接口管理说明将 ICD 列为接口控制文档的一种，并说明这些文档纳入配置管理；此处引用作术语依据，不把 NASA 的流程当作 SP 的强制要求（W25）。

### 在当前讨论中怎样理解

数据接口通常需要约定以下事项，具体内容由接口范围决定：

| 事项 | 需要说明的内容举例 |
|---|---|
| 传递关系与标识 | 谁发送、谁接收；消息、Label、字段名称 |
| 数值解释 | 数据类型、维度、单位、范围、比例与编码 |
| 状态解释 | 有效性、状态位位置及每一位的含义 |
| 时间与异常 | 更新周期、超时判据、无效数据的处理约定 |

结合前面的问题：

- Q006 / Q007 的 L310 等 Label：对应设备接口文件应明确 Label 对应的物理量、编码、单位及状态含义。
- Q012 的 stateBits：对应软件接口约定应明确每一位代表什么，发送方和接收方必须使用一致的布局。
- 设备通信 ICD 和软件内部接口文件可能是不同文档；不能把设备报文字段直接等同于模型内部总线字段。

以上是 ICD 用途的解释，不表示已经找到或核实本项目完整、现行的正式 ICD。Q012 中实现与字典描述的位布局差异仍待按受控接口版本及消费者核对。

### 来源与记录

- W25：[NASA — 6.3 Interface Management](https://www.nasa.gov/reference/6-3-interface-management/)，6.3.1.2.2、6.3.1.3；页面更新 2023-07-26，查阅 2026-09-16。用于接口管理与 ICD 名称的通用释义。
- 项目例子承接 Q006、Q007、Q012 的已有来源；本题未新增工程扫描，也未定位新的 ICD 文件。
- 仅追加 Q014 与索引；工程文件未改动，未运行 MATLAB、编译或仿真，sp 无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-16）：记录 ICD 英文全称、中文含义及在 Label 和状态位约定中的用途。

## Q015：当前工作区存在 ICD 吗？

### 日期、阶段与结论

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户问题：当前工作区是否存在 ICD。
- 阶段：S1、S2 的接口资料定位；关联 S3 状态接口。
- 问题处理状态：已解答（限定当前检索范围）；学习表现：掌握程度未确认。

在 D:/666/VE25 当前工作区内，未检索到可确认身份的独立、正式 ICD 文档。已找到的是工程中的 ICD 提及、需求条目以及实际接口数据字典；这些信息不能直接等同于完整、现行的设备 ICD。

### 实际找到的线索

| 来源 | 精确位置 | 发现及含义 |
|---|---|---|
| P20 [信号处理软件高级需求-SP_SW_HLR-20250722.xlsx](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx) | 工作表“信号处理软件高级需求-SP_SW_HLR”，C10，SP_SW_HLR-778 | 缩略语表定义 ICD 为接口控制文档；这是术语条目，没有附 ICD 正文或文件路径。 |
| 同一需求表 P20 | C137，SP_SW_HLR-261 | 导航输出、监控状态、表决结果的详细定义引用 SpNavMdl_IO.sldd 和 SP_State.sldd。当前工作区没有找到名为 SpNavMdl_IO.sldd 的文件；SP_State.sldd 实际存在。不能据此推断前者已改名为某个现有文件。 |
| P5 [SpCPT_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPT_define.m:61) | 第 61 行 | 注释“根据信号ICD定义的频率判断是否有效”；没有具体 ICD 名称、编号、版本或路径。 |
| P1 [VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx) | simulink/systems/system_54.xml，Annotation SID 440，第 724 行 | 注释“需要复查一下ICD单位对不对”；该注释没有文件指向。 |

### 当前可直接查阅的接口定义

| 资料 | 当前讨论中可查的内容 |
|---|---|
| P4 [SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd) | SpIn_Adahrus 等输入总线，L310 等字段名称、数据类型和维度。 |
| P23 [SP_State.sldd](D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd) | tSignalState 等状态结构及字段说明；stateBits 描述与实现的差异仍见 Q012。 |
| P15 [SP_InteriorBus.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd) | BUS_adahruNormalInfos 等软件内部总线结构。 |

这些字典可用于理解当前模型接口，但没有证据将其认定为完整设备 ICD。原始报文编码、Label 对照、单位、分辨率、量程及通信时序等仍需适用的受控设备接口资料核实。

### 检索范围与证据边界

- 全工作区文件名检索包含隐藏和忽略文件，排除 .git；本次枚举 12,336 个文件，未找到 ICD、接口控制或 Interface Control 命名的独立候选文件，也未找到 SpNavMdl_IO。
- 对 sp 与 ve25_tool 中全部 25 份 DOCX/XLSX 做内存只读文字检索：Word 主文档及存在的页眉、页脚、脚注、尾注、评论，Excel 工作表单元格。ICD / Interface Control / 接口控制 / 接口协议 / 接口定义 / 接口文档 / 接口文件关键词仅命中上述需求表 C10。
- 检查工程文本、XML、说明引用，并针对当前 VE25SP 模型注释回源。外层知识卡片中的 ICD 提示属于派生说明，不作为正式 ICD 存在的证据。
- 未对文档内图片做 OCR，未展开嵌入附件或全量解析所有模型二进制内容；通用知识库工具的发行 ZIP 不作为 SP 工程接口资料。结论为“在本次范围内未发现”，不推断工作区外或团队文档系统中的文件情况。
- P20 快照 SHA-256：E3F28F03B948613CCAB451EE469B9FD5A87313393398BECD541BDA3126406BEB；P4、P15、P23 本轮复核哈希与前述记录一致。
- 工程基线：AC102_Rebuild，既有两项 project XML 改动保留；未修改工程文件，未运行 MATLAB、编译或仿真，sp 无新增中间文件。
- 仅追加 Q015 与索引；原有 Q001—Q014 保留。
- Delta-K route：project-only。
- R001（2026-09-16）：记录工作区 ICD 检索范围、命中线索、未找到的 SpNavMdl_IO 引用及可查的接口字典。

## Q016：Voter 中的 Switch Case Action Subsystem 是什么，怎样创建？

### 日期、阶段与结论

- 日期：2026-09-16，Asia/Shanghai；修订 R001。
- 用户路径：VE25SP/NavSpApp/AdahruVoter（Calc_NavSourceSelection）/Model（Calc_VotedCAS）/Voter。
- 阶段：S2 的 CAS 链；S3 的条件执行和状态保持机制。
- 问题处理状态：已解答（静态结构与创建步骤）；学习表现：掌握程度未确认。未实际创建或运行练习模型。

Switch Case Action Subsystem 是 Simulink 的分支动作子系统，属于条件执行子系统。外部 Switch Case 根据选择值发出 action 控制信号，只有被选中的分支执行。内部 Action Port 使普通子系统具备这种执行机制；其外部 action 端口和点划控制线不同于普通数值输入（W26、W27）。

### 当前模型的实际对应

P1 的 system_54.xml：AdahruVoter SID 44 实际引用 P9 Calc_NavSourceSelection；P9 的 system_root.xml：Model SID 529 实际引用 P24 Calc_VotedCAS。P24 的 Voter SID 14 对应 system_14.xml。

Voter 内 select SID 178 的第 3 输出 chUsed 接 Switch Case SID 77。CaseConditions={1,2,3,4,5,6,7}，共有 8 个 action 输出，第 8 个接默认分支。chUsed 表示通道组合，不是有效通道数量；三种两路组合分别对应 3、5、6。

| 选择值 | 使用通道 | 分支名称（均以 Switch Case Action Subsystem 为前缀） | SID / 内部系统 | 主要作用 |
|---|---|---|---|---|
| 1 | 第 1 路 | 无数字后缀 | 78 / system_78 | 直接输出 data1 |
| 2 | 第 2 路 | 后缀 1 | 82 / system_82 | 直接输出 data2 |
| 3 | 第 1、2 路 | 后缀 3 | 90 / system_90 | 两路平均 |
| 4 | 第 3 路 | 后缀 2 | 86 / system_86 | 直接输出 data3 |
| 5 | 第 1、3 路 | 后缀 4 | 98 / system_98 | 两路平均 |
| 6 | 第 2、3 路 | 后缀 5 | 106 / system_106 | 两路平均 |
| 7 | 全部三路 | 后缀 6 | 114 / system_114 | 进入三路表决逻辑，内部含均值与中值计算路径 |
| default | 不匹配上述值；正常组合中为 0 | 后缀 7 | 174 / system_174 | z1 直通，保持上一拍表决值 |

- 上述是分支内部的数值处理；通道选择依据及 valid/reset 等其他输出不在本题做完整算法审计。
- 三路分支的配置 P_CALCFUNC=1；内部 Switch SID 170 的 Threshold 未在本轮读取的 XML 中显式保存，因此不以名称或默认猜测判定当前最终选中均值还是中值。
- 8 路分支输出经 Goto/From 路由汇入 Merge SID 76，Inputs=8；输出到 dataOut SID 243，并送 Delay1 SID 26 保存历史。Merge 是互斥分支输出汇合，不是拼出一个 8 元素数组。
- 默认分支 z1 来自外层 Delay1，DelayLength=1、初值端口接 data0；不是依赖默认分支内部保存状态。

### 主要特点

1. 执行选择：每次 Switch Case 执行时选择匹配分支；有默认分支时，未匹配值进入 default。执行完该 case 后结束本次分支选择，无需手写 break，也不会继续落入下一 case（W27）。
2. 控制与数据分开：action 线决定“是否运行”，普通 Inport 传入待处理的数据。Switch Case 的 action 输出应接子系统的动作端口，不能当普通数据使用（W26）。
3. 状态可配置：Action Port 的 States when execution is resumed 支持 held 或 reset。held 保留上次状态；reset 在停止执行后重新被选中时恢复初值，不是在连续选中期间每拍复位（W28）。
4. 当前 8 个 ActionPort（SID 80/84/88/93/101/109/118/176）均显式 InitializeStates=reset。默认分支仍可输出上一拍结果，因为历史 Delay 位于这些分支之外。
5. 与 For Each 的区别：For Each 对输入分区重复执行同一算法；这里根据组合码选择一个处理分支。不能用动作子系统名称推断它会遍历三路。
6. 分支块与驱动 Switch Case 应采用相容、相同执行速率；一般将内部块采样时间设为继承 -1（W26）。

### 如何创建相同结构

以下为操作说明，适合在练习模型中搭建；本题没有执行工程修改。

1. 打开 Library Browser，进入 Simulink / Ports & Subsystems，拖入 Switch Case。
2. 双击 Switch Case，将 Case conditions 设为 {1,2,3,4,5,6,7}，勾选 Show default case，得到 8 个动作输出。
3. 从同一库拖入 Switch Case Action Subsystem，共放置 8 个。该模板已包含 Action Port；仅创建普通 Subsystem 或修改名字不会产生条件执行机制。
4. 将各 case / default 输出连接到对应子系统的 Action 控制端口；普通数值另接 Inport。连接后子系统的动作标识随所接 case 显示。
5. 双击进入各分支，保留 Action Port，按需要布置 Inport、计算块和 Outport。若仿照当前配置，将 Action Port 的 States when execution is resumed 设为 reset。
6. 从 Simulink / Signal Routing 添加 Merge，将 Number of inputs 设为 8，各分支同一含义的输出接到 Merge。参与汇合的信号类型、尺寸应一致；驱动 Merge 的分支 Outport 的 Output when disabled 使用 held，分支之间保持互斥（W29）。
7. 在练习模型中按 Ctrl+D 更新模型，检查类型、维度、采样时间和动作连线。可随后以选择值 1、3、7、0 验证分支选择；更新和运行需要在实际环境完成，本题没有代为执行。

也可以复制一个现有 Switch Case Action Subsystem 模板并修改内部算法。Action Port 不作为独立块出现在 Library Browser，直接使用动作子系统模板最方便（W26、W30）。

### 来源与验证边界

| ID | 来源与锚点 | 版本或核对信息 |
|---|---|---|
| P1 | [VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)，system_54.xml，SID 44 | 本轮哈希与已有记录一致 |
| P9 | [Calc_NavSourceSelection.slx](D:/666/VE25/sp/04_Design/Calc_NavSourceSelection/specification/Calc_NavSourceSelection.slx)，system_root.xml，SID 529 | BE517B33C48086D7B6173C32205CEE0F3C77AFE5D24BE37914388EF6A80143DE |
| P24 | [Calc_VotedCAS.slx](D:/666/VE25/sp/04_Design/Calc_VotedCAS/specification/Calc_VotedCAS.slx)，system_root.xml 的 Voter SID 14；system_14.xml 及各分支系统 | 807AD66E0B5C08BBB3592F75D0363879DF7B29EB20A2146373576764F12BC6D6 |
| W26 | [MathWorks — Switch Case Action Subsystem](https://www.mathworks.com/help/simulink/slref/switchcaseactionsubsystem.html) | 2026-09-16 查阅；类型、动作端口、库位置与速率要求 |
| W27 | [MathWorks — Switch Case](https://www.mathworks.com/help/simulink/slref/switchcase.html) | 2026-09-16 查阅；CaseConditions、default、隐含 break |
| W28 | [MathWorks — Action Port](https://www.mathworks.com/help/simulink/slref/actionport.html) | 2026-09-16 查阅；held/reset 的重新执行语义 |
| W29 | [MathWorks — Merge](https://www.mathworks.com/help/simulink/slref/merge.html) | 2026-09-16 查阅；汇合、互斥及输出保持要求 |
| W30 | [MathWorks — Select Subsystem Execution](https://www.mathworks.com/help/simulink/ug/select-subsystem-execution.html) | 2026-09-16 查阅；Switch Case 结构创建步骤 |

- 工程基线 AC102_Rebuild；全程离线只读解析，未运行 MATLAB、模型更新、仿真或代码生成。
- 仅追加 Q016 与索引，保留 Q001—Q015；工程文件未改动，sp 无新增中间文件。
- 通用创建方法已核对官方文档；实际搭建及动态行为未验证，不据此标记学习阶段完成。
- Delta-K route：project-only。
- R001（2026-09-16）：记录条件执行机制、当前 8 分支映射、状态配置和创建方法。

## Q017：FCC_SP_HLR_00055 具体出自哪份需求文档？

### 日期、阶段与结论

- 日期：2026-09-17，Asia/Shanghai；修订 R001。
- 用户问题：FCC_SP_HLR_00055 的原始需求文档出处。
- 阶段：S5 需求与设计依据追踪；关联 S2 导航选源链。
- 问题处理状态：待核实（原始文档未定位）；学习表现：掌握程度未确认。

当前工作区证据不足以确认该编号出自哪份原始需求文档。实际定位到的是模型中的编号注释，没有需求正文、文档名称、文档版本或超链接。不能将它直接归入现有 20250722 需求表，也不能将其等同于 SP_SW_HLR-55。

### 已核对的来源

| 来源 | 精确位置与检索结果 |
|---|---|
| P9 [Calc_NavSourceSelection.slx](D:/666/VE25/sp/04_Design/Calc_NavSourceSelection/specification/Calc_NavSourceSelection.slx) | Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc；压缩成员 simulink/systems/system_8.xml，第 883 行，Annotation SID 534。富文本内容仅为 FCC_SP_HLR_00055，没有超链接。 |
| P20 [信号处理软件高级需求-SP_SW_HLR-20250722.xlsx](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx) | 全部工作表可检索单元格文字未找到 FCC_SP_HLR 前缀及下划线、空格、连字符变体；R1 为“原需求编号”，R2:R528 全空，未发现该编号的映射。 |
| P6 [SP_SW_HLR.slreqx](D:/666/VE25/sp/02_Requirements/specification/SP_SW_HLR.slreqx) | 全部 XML / rels 成员未检出该编号及 FCC_SP_HLR 前缀。 |
| P25 [importReqFromXlsx.m](D:/666/VE25/sp/02_Requirements/specification/documents/importReqFromXlsx.m:24) | 第 24、26 行指定上述 Excel 和工作表；第 27—28 行将 A 列用作 ID；第 95 行将输出目标设为 SP_SW_HLR.slreqx。脚本未提供 FCC_SP_HLR 与 SP_SW_HLR 的编号对照。本轮只读脚本，未运行导入。 |

外层知识卡 S_Calc_NavSourceSelection_002.md 第 363 行也出现该编号，但所在标题为“原模型注释”，来源回指上述模型。该卡是派生记录，不是另一份需求原文。

### 检索范围与待确认项

- 读取 sp、Doc、codex/kb 的文本引用，回查当前 Calc_NavSourceSelection 模型。
- 检查当前需求 Excel 的全部工作表单元格、1 个 SLREQX、5 个现存 SLMX 的 XML / rels；SLMX 仅用于追踪出处，不将其历史模型支线视为当前实现依据。
- 补查 sp 与 ve25_tool 中 14 份 DOCX 的正文及存在的页眉、页脚、脚注、尾注和评论文字，未检出 FCC_SP_HLR 前缀。未对嵌入图片做 OCR；未访问工作区外需求库或远端文档系统。
- Q017-U1：原始需求文档的名称、版本及 FCC_SP_HLR_00055 的完整正文未定位。
- Q017-U2：该编号与当前 SP_SW_HLR-* 条目的映射未找到。即使编号尾数或算法主题相近，也不能据此认定是同一条需求。
- 下一步所需证据：包含精确编号的原始需求文件，或经确认的编号迁移/对应表。仅凭模型注释无法补出文档名称。

### 版本与记录

- P20 本轮共享读取快照 SHA-256：E3F28F03B948613CCAB451EE469B9FD5A87313393398BECD541BDA3126406BEB。
- P9 SHA-256：BE517B33C48086D7B6173C32205CEE0F3C77AFE5D24BE37914388EF6A80143DE。
- P6 SHA-256：D48948EB3E54C2129D0C18B4C80DCF0628597F3621EEC78BC0EBC5A8BCC4338D。
- P25 SHA-256：454DBB0683515FAE5FCB3D01D3B08D41A9F5D7516E14C890CD09801E7D119B29。
- 工程基线 AC102_Rebuild，保留已有两项 project XML 改动；未修改模型、需求或代码，未运行 MATLAB、导入脚本、编译或仿真，sp 无新增中间文件。
- 仅追加 Q017 与索引，保留 Q001—Q016；原始出处仍未闭合，不宣称已经找到。
- Delta-K route：project-only。
- R001（2026-09-17）：记录编号的实际注释位置、现有需求资料检索结果及原始文档/编号映射缺口。

## Q018：ADAHRU 有没有可能指某个具体导航产品的型号？

### 日期、阶段与结论

- 日期：2026-09-17，Asia/Shanghai；修订 R001。
- 阶段：S1、S2 设备与信号命名；关联 S5 接口依据。
- 问题处理状态：部分澄清；学习表现：掌握程度未确认。

有可能是项目对某一具体导航设备或产品系列的简称。一个名称可以同时是功能缩写和产品称呼；但单凭 ADAHRU 六个字母，不能唯一确定厂商、型号、件号或软硬件构型。

这补充了 Q002、Q003 的解释：此前给出的 Air Data, Attitude and Heading Reference Unit 是有公开资料支持的一种展开，并不意味着已经确认 VE25 的设备身份，也不能认定所有资料中的 ADAHRU 都只能采用这一展开。

### 公开资料证据

- W4（本轮复核）：Meggitt Avionics 署名、AeroExpo 托管的产品介绍直接以 ADAHRU 展示产品，正文展开为 Air Data, Attitude, Heading Reference Unit，并说明可按应用定制。它证明这种称呼可以用于产品，但该页没有给出足以唯一确认 VE25 配套构型的依据。
- W31：IS&S 厂商官网的 ADAHRS 产品资料在 Technical Overview 中直接使用 ADAHRU 称呼该单元，另有 ADAHRU Signal Inputs/Outputs 标题。说明该缩写并非仅能与 Meggitt 一家关联。
- W32：Honeywell 申请的专利 US9688416B2 在自身语境中将 ADAHRU 展开为 Air Data Attitude Hybrid Reference Unit。这是另一种公开用法，不能据此判定 VE25 就采用 Honeywell 产品或 Hybrid 展开；专利中的示意标号也不是产品型号。

### 对当前工程能确认到哪一步

当前已读项目证据包含 P20 的 SP_SW_HLR-250（C131）“三路ADAHRU”输入描述，以及 P5 第 6、8 行把 ADAHRU 作为大气数据输入来源。它们能确定软件所处理的设备角色和输入路数，尚不能建立具体厂家、型号和件号的对应关系。

识别实际产品应核对项目设备选型/构型清单、设备铭牌、适用 ICD 或安装手册中的厂家、Model/Type、P/N（件号）与软硬件版本。仅凭 L310 等标签名、输出物理量或三路配置不足以唯一识别产品。

- Q018-U1：VE25 所用 ADAHRU 的厂商、正式产品名、型号、件号及适用接口版本待确认。
- Q018-U2：本项目对 ADAHRU 的正式英文展开仍待项目设备资料确认；当前模型名字可按已有功能理解，不据公开同名产品自动补写厂家。
- 本条公开实例仅用于解释命名可能性，不作为 VE25 的设备选型结论。

### 来源与记录

| ID | 来源 | 支持范围 |
|---|---|---|
| W4 | [Meggitt Avionics 署名 ADAHRU 产品介绍，AeroExpo 托管](https://www.aeroexpo.online/prod/meggitt-avionics/product-184912-46266.html) | 产品名称及 Heading 展开；2026-09-17 复核 |
| W31 | [IS&S 官方 ADAHRS 产品资料](https://innovative-ss.com/wp-content/uploads/2020/03/ISS_ADAHRS.pdf) | Technical Overview 与 ADAHRU Signal Inputs/Outputs；2026-09-17 查阅 |
| W32 | [Honeywell 专利 US9688416B2](https://patents.google.com/patent/US9688416B2/en) | Description / Definitions 对 Hybrid 展开的用法；2017-06-27 公开，2026-09-17 查阅 |
| P20 | [信号处理软件高级需求-SP_SW_HLR-20250722.xlsx](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)，C131 / SP_SW_HLR-250 | 项目三路输入描述；不含具体产品身份 |
| P5 | [SpCPT_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPT_define.m:6)，第 6、8 行 | 输入来源注释；本轮 SHA-256 为 49AAF3CBA343CA4B8E1A4C4B8959B1BBD4824C56378C36B61B74119F5DB218B0 |

- 本轮项目补查范围为 P5 参数注释与 P20 的相关可检索文字，未做全工程设备构型审计。
- 未运行 MATLAB、初始化、编译或仿真；工程文件未修改，sp 无新增中间文件。
- 仅追加 Q018 与索引，保留原有问答；补充 Q002/Q003 的术语解释边界，不将公开产品资料升级为项目设备事实。
- Delta-K route：project-only。
- R001（2026-09-17）：区分设备缩写、产品称呼与唯一型号识别，并记录公开实例和项目身份缺口。

## Q019：RA 只有两路输入，为什么预处理仍有三路一致性比较？

### 日期、阶段与结论

- 日期：2026-09-17，Asia/Shanghai；修订 R001。
- 阶段：S2 信号连接与总线数组、S3 有效性与一致性判断；关联 S4 RA 选源。
- 问题处理状态：已解答（静态核验）；学习表现：掌握程度未确认。

当前实现将两路真实 RA 加上一条无效占位通道，适配固定三路的 Calc_SignalState / CmpMonitor 接口。第三路的 data=single(0)、valid=false；它不是第三台 RA，也不是有效的零高度测量。

模型原文直接支持这一结论：RAMsgsPreprocess 的 Annotation SID 21 写明“对2路RA信号做预处理”，并注明“3路信号两两比较（第3路为0，invld）”。

### 两路如何变成三元素

| 环节 | 当前实现 |
|---|---|
| 原始输入 | P4 的 SpIn_Ra.L164_Radio_Altitude：类型 Bus: SpData_Single，Dimensions=2。每个元素含 data（single）与 valid（boolean）。 |
| 补位 | P10 的 Constant，SID 17：Value=0，OutDataTypeStr=Bus: SpData_Single，输出该结构的零值，因此 data=0、valid=false。这种 Bus 零值用法见 W33。 |
| 拼接 | P10 的 Vector Concatenate，SID 18：第1输入来自 Bus Selector SID 5 的两路 L164_Radio_Altitude，第2输入来自补位常量 SID 17。得到 [RA1, RA2, 无效占位]。 |
| 一致性计算 | P10 的 Calc_SignalState1，SID 16，引用 P21 的 SignalProcessingLib/Calc_SignalState。库内 DataIn1 SID 2550 明确 PortDimensions=3。 |
| 输出 | P10 的 raNormalInfos，SID 20，明确类型 Bus: Bus_SingleSignalNormalInfo、PortDimensions=3。 |

### 三组比较中哪些真正有效

有效比较还要求对应真实通道满足有效性与未禁用条件、监控已启用。第三路恒无效时：

| 比较组 | 作用 |
|---|---|
| 通道1与通道2 | 两路满足参与条件时执行有效的一致性判断。 |
| 通道2与通道3 | 第三路无效，配对无效；syncReq、missReq、banReq 均为 false。 |
| 通道3与通道1 | 第三路无效，配对无效；syncReq、missReq、banReq 均为 false。 |

因此，补入的 0 不会被作为一条有效高度与两路真实高度比较，并据此制造失配。框图仍保留三组 Comparator，是共用模块的结构；本场景可用的比较对至多只有 1—2 一组。

P21 精确路径：Calc_SignalState/system_2551 的 Bus Selector SID 2553 提取 data、valid；system_2474 的 SID 2477 引用 CmpMonitor。CmpMonitor 包含 12、23、31 三个配对（SIDs 1481、1482、1483）。Comparator/system_1220 的 SID 1227 对两个 valid 做 AND；任一无效经 SIDs 1320、1321 进入 system_1233，SIDs 1235—1237 将三个请求置 false。

### 图中的 (3) 表示什么

Bus_SingleSignalNormalInfo (3) 表示长度为3的总线数组，依次保存 RA1、RA2 和占位通道的处理结果。它不表示三台真实 RA，也不表示空间三维数据。

P21 的 Calc_SignalState/For Each Subsystem1（system_2452）逐元素组装结果，Bus Creator SID 2463 的类型为 Bus_SingleSignalNormalInfo，输出沿第1维拼接。原始 valid 直接进入该组装路径及状态编码，没有被一致性结果改写；第三份结果的 valid 保持 false。

下游 P26 的 Calc_VotedRA/raNormalInfos（SID 5）同样定义为三元素输入；SourceSelection/Subsystem（system_15）中的 Selector SIDs 43、44 分别选索引1、2，并由 Bus Selector SIDs 21、22读取这两路 valid。该接口安排与两路真实 RA 的选源相匹配。

从当前结构可以确认，这是一种复用通用三路监控与状态接口的实现方式。不能据此说 Simulink 或某项航空标准强制两路 RA 必须补成三路；本轮未找到这样的强制规定或单独的设计决策记录。

### 来源与验证边界

| ID | 来源与定位 |
|---|---|
| P10 | [RAMsgsPreprocess.slx](D:/666/VE25/sp/04_Design/RAMsgsPreprocess/specification/RAMsgsPreprocess.slx)，simulink/systems/system_root.xml；SIDs 4、5、16、17、18、20、21。SHA-256：6FDCBEAD97A9EB0023601C9D44E2A23EB9768DA8814910681943E5E826815213。 |
| P4 | [SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)，data/chunk0.xml 的 SpIn_Ra、SpData_Single。SHA-256：10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC。 |
| P21 | [SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)，system_2421、system_2452、system_2474、system_2551、system_1326、system_1220、system_1233。SHA-256：0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490。 |
| P26 | [Calc_VotedRA.slx](D:/666/VE25/sp/04_Design/Calc_VotedRA/specification/Calc_VotedRA.slx)，system_root.xml 的 SID 5，system_15.xml 的 SIDs 21、22、43、44。SHA-256：3C07C33A91C9B4920D527FC12DE87DE20E76502F8ECA540F757F2C92DD099197。 |
| W33 | [MathWorks：Constant](https://www.mathworks.com/help/simulink/slref/constant.html)，Constant value 参数中 Bus 输出配合 0 的定义；2026-09-17 查阅，仅支持通用块语义。 |

- 本轮通过模型与字典 ZIP XML 静态核验；未运行 MATLAB、初始化、编译、仿真或代码生成，不声称已验证运行日志或目标代码。
- 仅追加 Q019 和索引，保留 Q001—Q018；工程文件未修改，sp 无新增中间文件，保留已有两项 project XML 改动。
- Delta-K route：project-only。
- R001（2026-09-17）：记录 RA 第三路的明确占位来源、无效比较门控及输出三元素接口的含义。

## Q020：tSingleData 为什么这样命名？

### 日期、阶段与结论

- 日期：2026-09-17，Asia/Shanghai；修订 R001。
- 阶段：S1 术语、S2 总线类型与接口；学习表现：掌握程度未确认。
- 问题处理状态：部分澄清；Single 与字段结构已确认，t 前缀的正式项目定义待确认。

可以将 tSingleData 理解为“承载单精度数值及其附加标志的数据类型”。

| 部分 | 解释及证据边界 |
|---|---|
| t | 从同系列 tSingleData、tDoubleData、tBoolData、tU8Data、tU32Data 及其 Simulink.Bus 定义判断，可按 type（类型）前缀理解。这是命名模式推断；已读字典 Description 为空，尚未取得正式项目规范证明其唯一展开，也不能据此认定 t 必须展开为 typedef。 |
| Single | 表示 data 字段使用 single 单精度浮点类型，已由字典直接确认。MATLAB single 为32位浮点数，见 W34。这里不表达通道路数。 |
| Data | 表达该类型承载的数据；整体封装了数值、有效性及 reset 标志。 |

### 当前字典定义

P27 的 tSingleData 是 Simulink.Bus，包含以下三个标量字段：

| 字段 | 类型 | 本题可确认的含义 |
|---|---|---|
| data | single | 数据值。 |
| valid | boolean | 有效性标志。 |
| reset | boolean | reset 标志；具体何时置位、下游复位什么须沿使用模块核对，本题不从字段名推定。 |

同一字典中，tDoubleData 的 data 为 double，tBoolData 为 boolean，tU8Data 为 uint8，tU32Data 为 uint32；这几种类型均另含 boolean 类型的 valid、reset。这支持“用名称中间部分区分数值类型”的解释。

tSingleData 整体是自定义总线类型，只有其 data 字段为 single。名称本身不决定外层数组有几路：一份还是多份，由使用位置的维度定义决定。由此也不能把 Single 理解成单路 RA 或单余度。

### 来源、待确认项与记录

- P27：[SpMdlPrivate_DataType.sldd](D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd)，data/chunk0.xml 中 tSingleData、tDoubleData、tBoolData、tU8Data、tU32Data 的 DD.ENTRY。SHA-256：E759D089EE8CF57876AD182E553ACE204672624A678F6C0F0F0A9F8D747F311D。
- 当前主线引用依据：P3 [SP_ALL.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_ALL.sldd) 与 P22 [DD_SpLib.sldd](D:/666/VE25/sp/04_Design/common/specification/data/DD_SpLib.sldd) 的 Subdictionary 均明确引用 SpMdlPrivate_DataType.sldd。因此本题使用它是依据当前字典引用关系，并未将已停用的 SpMdl 主模型作为依据。两文件 SHA-256 分别为 4823DA964D265B93AE39A59A9D1954E8A88AE3F440C55E151D548602219544DC、557DBF7AE4B2D8C13B876DAEFACF648F64E2F32680621F7F2FFCE5EEE59D367D。
- W34：[MathWorks：single](https://www.mathworks.com/help/matlab/ref/single.html)，Description；2026-09-17 查阅，仅用于 single 为32位单精度的通用语义。
- Q020-U1：t 前缀的正式项目命名规定尚未取得，type 为有结构依据的解释，保持推断状态。
- 仅追加 Q020 与索引；已有问答保持不变。未修改工程文件，未运行 MATLAB、初始化、编译或仿真，sp 无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-17）：记录命名拆解、实际字段与相邻类型对照，区分单精度和通道路数。

## Q021：图中的 Delay 怎样执行，什么情况下使用？

### 日期、阶段与结论

- 日期：2026-09-18，Asia/Shanghai；修订 R001。
- 阶段：S2 模型连接、S3 离散状态与计数；学习表现：掌握程度未确认。
- 问题处理状态：截图参数的语义已解答；实例的完整模型路径及复位线来源待定位。

截图显示一个带外部复位的一拍 Delay：Delay length=1，Initial condition=0.0，External reset=Level hold，Sample time=-1，Show enable port 未勾选。上方 u 是待保存的数据，下方是 reset 输入。

### 执行逻辑

1. 当所属离散任务到达采样时刻、且所在子系统执行时，由 Simulink 调度这个块，无需用户另写循环或主动调用函数。图中没有单独的使能端口。
2. 正常、连续执行且没有复位时，本拍输出上一拍输入，随后保存本拍输入供下一拍使用。初次没有历史值，输出初值0。
3. reset 本拍非零时，Level hold 触发复位，本拍输出回到初值0；reset 持续非零时，每拍都会产生复位效果。它保持的是复位效果，不是把复位前的任意数值冻结。
4. reset 回到0后不再满足 Level hold 的复位条件，恢复延迟工作。Level 模式还会在非零到0的转换时复位，Level hold 不因这个下降沿额外复位；不要混淆两种模式。
5. -1 表示采样时间继承。延迟1表示一拍，不是固定1秒；例如实际继承周期为0.01秒时，一拍对应10毫秒。这里只是假设例子，未据截图确认实际调度周期。
6. 若位于条件执行子系统中，还必须结合该子系统何时执行以及状态保持/重置设置理解历史值；不能把每个求解器步都当作这块执行了一次。

无复位的连续执行示例：

| 执行拍 | 本拍输入u | 本拍输出y |
|---|---:|---:|
| 第1拍 | 10 | 0（初值） |
| 第2拍 | 20 | 10 |
| 第3拍 | 30 | 20 |
| 第4拍 | 40 | 30 |

这是保存离散历史值的机制，不是让程序阻塞等待一段墙钟时间。示例不推导复位释放首拍或特定计数器的门限触发拍号。

### 什么时候用

- 保存上一拍数值或状态，供本拍比较、递推或状态转换判断。
- 连续异常、连续恢复或连续越界计数：用上一拍计数计算下一拍计数，条件中断时复位，避免把不连续事件累加。
- 需要重新开始计算时，清除滤波器、递推算法或计数器中的旧历史；具体复位条件由下方 reset 连线定义。
- 反馈算法本身需要使用上一拍结果时，以 Delay 明确表达这个时间关系；是否允许加入一拍延迟必须服从算法要求。

当前 P21 中有四个明确保存 Level hold 配置的例子，均 DelayLength=1、InputPortMap=u0,r5：

| 库内路径 | SID、XML | reset 条件与作用 |
|---|---|---|
| Abnormal/abnormalCounter/Delay | 2222，system_2115.xml | !dataValid OR !valid；连续恢复计数。 |
| Abnormal/abnormalCounter/Delay1 | 2223，同上 | dataValid OR !valid；连续异常计数。 |
| Range/abnormalCounter/Delay | 2191，system_2146.xml | dataValid OR reset；连续越界计数。 |
| Range/abnormalCounter/Delay1 | 2192，同上 | !dataValid OR reset；连续范围内恢复计数。 |

这里的逻辑符号 ! 表示“非”。Range 上层的 reset 由输入 valid 取反产生（system_2121.xml，SID2144）；其 dataValid 来自范围比较器 SID2128。上述计数回路均为 Delay输出→饱和→计数输出，并分支加1送回 Delay.u。不能直接套用 Q005 中另一套 Comparator 计数器的更新与门限判断先后关系。

### 来源与待确认项

- 用户截图：codex-clipboard-184d670c-1067-43bb-94d8-f948fa40b71e.png，2026-09-18。本题参数来自可见对话框；截图未展示完整模型路径和两条输入线的来源。
- W13（本轮复核）：[MathWorks：Delay](https://www.mathworks.com/help/simulink/slref/delay.html)，Description、External reset、Show enable port、Sample time；支持一拍历史、复位条件与采样时间继承语义。
- P21：[SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)，system_2115、system_2146、system_2083、system_2121。SHA-256：0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490。
- Q021-U1：截图中的具体实例尚未唯一定位。P21 的四处匹配块都未显式保存 InitialCondition、SampleTime、ShowEnablePort；该库系统 XML 未找到截图背景可见的 resTime 文本，不能据外观认定是上述某一处。需要完整块路径才能确定本实例实际保存什么、何时复位。
- 仅追加 Q021 与索引，保留既有问答；模型与代码保持只读，未运行 MATLAB、初始化、编译或仿真，sp 无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-18）：解释截图配置、无复位逐拍示例及计数用途，保留具体实例定位缺口。

## Q022：当前模型学习进度评估

### 评估范围与结论

- 日期：2026-09-18，Asia/Shanghai；修订 R001。
- 范围：本任务可见的用户提问与 Q001—Q021 共21项记录，对照 P7 学习计划第4章、5.2、5.3、7.8节。未评估用户在其他对话或线下完成而本任务未见的成果。
- 当前定位：S2 局部信号链阅读进行中，并交叉深入 S3 的基础状态机制；S1 总体地图尚未验收，S4 已触及 RA，S5 已开始追问依据，S6 尚无可核验成果。
- 本条是基于记录的学习评估，不是模型工程验证，也不代表阶段考试通过。题数和提供答案不换算成掌握率或学习完成百分比。

### 计划任务与证据对照

| 阶段 | 已覆盖内容及关联问题 | 可观察表现／现有成果 | 关键缺口与下一步 |
|---|---|---|---|
| S1 总体架构与工程入口 | Q001—Q008、Q013—Q015、Q018、Q020：术语、设备输入、总线与资料入口 | 已持续对照模型和字典提问；Q004 主动补充 D=Device。已有说明材料，但未见自主总体架构讲解 | 说明七类功能的输入输出关系，以及工程入口、依赖、参数与字典来源；形成可复述的总体地图 |
| S2 CAS 全链路 | Q007—Q011、Q013、Q016、Q019—Q021：接口组织、预处理输出、CAS Voter 局部分支 | Q008 察觉字段维度1与三路数组的差异；Q016给出深入Voter的具体模型路径，表明阅读进入内部结构 | 自主串起当前 VE25SP 的 CAS 原始输入→监控→选源/表决→最终输出，并解释一个故障场景 |
| S3 公共机制与时序 | Q005、Q007—Q012、Q016、Q019、Q021：计数、锁存、For Each、状态编码、动作分支、Delay复位 | 已从名称追问转向运行机制；Q019主动指出两路RA与三元素处理接口的差异 | 自主给出一个监控器的输入、计数、状态、输出逐拍表，解释进入、保持、恢复和复位条件 |
| S4 业务横向扩展 | Q002—Q005、Q019：模块命名及导航RA局部处理 | 已将通道路数与公共监控的观察用于RA，是导航内部的横向比较 | 驾驶员、地面站、设备反馈、MPIO、系统状态及输出组织仍缺自主模块说明与反向定位案例 |
| S5 需求和设计依据 | Q012、Q014—Q018：状态编码依据、ICD、需求编号出处、设备身份 | 主动追问“实现为何如此、依据在哪里”，已有来源意识 | 建立至少一条需求—模型—参数—输出—测试的完整追踪，以及一次纸面变更影响分析 |
| S6-A 验证方案 | Q011、Q016的创建方法咨询仅构成实践准备 | 有搭建结构的学习意向，未见实际测试方案 | 选择边界明确的假设变更，列正常、边界、故障、恢复用例与判断标准 |
| S6-B 获准实践 | 当前21项记录无实际修改与运行验证成果 | 不能由操作说明推定已搭建、仿真或掌握维护 | 后续按明确授权与环境条件开展；当前不能认定完成 |

### 已出现的学习进展

1. 主动阅读和核对结构：Q008、Q019 的疑问来自用户观察到的尺寸／路数差异，不能因记录为“掌握程度未确认”就否认这种表现。
2. 连续追问机制：Q009—Q011 从结果拼接追到 For Each 执行方式及创建方法；Q021进一步关注历史状态和复位。
3. 开始审查设计依据：Q012、Q017涉及编码理由和具体需求出处，已超出单纯识别块名。

这些证据支持“已进入局部结构与机制分析”的判断；尚不足以证明能独立解释整条链、所有边界条件或完成模型维护。Q007 中由分析过程发现的状态位描述冲突，不记成用户独立发现。

### 下一步最小成果

优先围绕计划既定的 CAS 主线形成两份相互衔接的成果：

- 一张当前 VE25SP 的 CAS 输入到最终消费者的链路图，在关键节点标出 data、valid、reset 的来源和处理位置，并标明配置与参数来源。
- 一张故障发生到恢复的逐拍表，列出输入、计数、通道状态、选源结果、输出值与有效性。先选一个明确的场景和配置；表格长度覆盖实际门限与状态转换，不机械限定拍数。

以能独立说明“哪路输入变化→什么判据成立→哪个历史状态改变→最终输出怎样变化”为验收重点。经核对后，再将对应局部问题更新为有证据的掌握记录，并判断 S2/S3 是否达到计划验收。

### 评估边界与来源

- P7：[SP模型学习与维护进阶计划](D:/666/VE25/Doc/SP模型学习与维护进阶计划.md:53)，S1—S6及第5.3节；SHA-256：8D3F164BF800D48DD311D01C9BD5D03517FD971433DA3E09EA915B3C623AB27B。
- 学习证据：[本记录问题索引](D:/666/VE25/Doc/SP模型学习问答记录.md:13)及Q004、Q008—Q012、Q016—Q021；本条追加前文件SHA-256：E6690A94DAAFECC7A6A873FB568A6F5B2EDDD66F90F597B326A8991E42EE324B。
- 计划仍保留 SpMdl 主线关系待确认的旧描述；当前用户已明确以 VE25SP 及其真实引用链为主，不把旧描述重新计作主线未确定。
- 正式ICD、旧需求编号出处及设备身份的资料缺口，不等同于用户理解能力不足。阶段掌握缺少证据，也不等同于用户必然没有掌握。
- 本条只追加进度快照与索引，未改写既有学习表现，未勾选原计划，未修改工程或运行 MATLAB；sp 无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-18）：建立基于21项问答的阶段覆盖与表现评估，指出 CAS 全链路和逐拍推演是下一步验收重点。

## Q023：MCU 和 SCU 分别指什么？

### 结论与确认程度

- 日期：2026-09-18，Asia/Shanghai；修订 R001。
- 阶段：S1、S2 设备与接口识别，关联 S4 设备反馈；学习表现：掌握程度未确认。
- 问题处理状态：部分澄清；设备功能对应已确认，项目正式英文全称待确认。

| 缩写 | 当前模型中的设备角色 | 公开资料中的常见英文展开 |
|---|---|---|
| MCU | 电机控制相关单元，提供电机反馈 | Motor Control Unit，电机控制单元，见W35。 |
| SCU | 伺服／舵面执行机构控制相关单元，提供位置及状态反馈 | Servo Control Unit，伺服控制单元，见W36。 |

这两种英文展开有公开一手资料支持，且符合当前接口语境；当前已读项目字典没有正式英文释义，不将行业展开认定为已取得本项目术语表确认。厂商、型号和设备完整职责也不由缩写推出。此前 Q002、Q003 对正式名称的待确认边界继续保留。

### 当前输入接口证据

P6 的 SP_SW_HLR-357 将 MCU1 对应到升力电机1，-365—-367 将 MCU9—11 对应到推力电机1—3；-368、-369 将 SCU1、SCU2 对应到左右内副翼舵，-370—-375 覆盖外副翼、升降舵与方向舵。这直接支持两类设备的功能归属，但未写出正式英文全称。

P4 的 SpIn_McuInfos.MCU 为 Sp_McuInfo 的11元素数组；SpIn_ScuInfos.SCU 为 Sp_ScuInfo 的8元素数组。数组长度是当前软件接口定义，不单凭它认定硬件数量或构型。

- Sp_McuInfo：Valid、McuFaultLevelFbk、McuWorkModFbk、McuSpdFbk、McuPowerFbk、McuSpdFbkInvalid、McuPowerFbkInvalid。分别体现有效性、故障等级、工作模式、转速、功率及相应无效标志。
- Sp_ScuInfo：Valid、INT_STS、EN_STS、STK_MON、Position_Fdb；其中位置反馈和状态类字段支持舵面执行机构反馈的功能解释。本题不推定各状态码的取值含义。
- 两类输入均属于已定位的 Calc_MCUSCUData 设备反馈处理范围（沿用 Q002 的 P11 及当前主线引用证据）。SP 中该模型的存在不等于其中实现了设备控制器内部的完整控制算法。

### 对早期词表的订正

Q003 的 MCU 词条曾写“包含转速、功率、温度与故障反馈”。当前 P4 的 Sp_McuInfo 没有温度字段，本轮将该词条订正为“转速、功率、工作模式与故障等级反馈”。P6 的 SP_SW_HLR-345、-683—-686 确实存在温度监控或消息要求，因此原词条的温度说法有需求依据，但不能直接当作当前输入总线已有该字段。这里区分需求内容与当前接口结构；其差异需结合需求适用版本及处理链另行核对，不据此认定实现缺陷，也不推定实际控制器没有温度监测能力。

### 来源与记录

- P4：[SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)，data/chunk0.xml 的 SpIn_McuInfos、SpIn_ScuInfos、Sp_McuInfo、Sp_ScuInfo；本轮SHA-256：10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC。
- P6：[SP_SW_HLR.slreqx](D:/666/VE25/sp/02_Requirements/specification/SP_SW_HLR.slreqx)，slrequirements/data.xml 的 SP_SW_HLR-357、-365—-375 及温度相关条目 -345、-683—-686；本轮SHA-256：D48948EB3E54C2129D0C18B4C80DCF0628597F3621EEC78BC0EBC5A8BCC4338D。需求设备对应可供语义核对，不据此自动确认全部条款对当前实现构型的适用性。
- P11：[Calc_MCUSCUData.slx](D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/Calc_MCUSCUData.slx)；输入SIDs 5、108，输出DevData SID223，沿用Q002已核验定位，不在本题重新审计完整处理算法。
- W35：[KYOCERA：Motor Control Unit (MCU)](https://ele.kyocera.com/en/technical/kavx_mcu/)，标题；2026-09-18查阅，仅证明MCU的一种行业展开，不是VE25设备选型依据。
- W36：[AHS 2008飞行控制研究论文](https://www.sjsu.edu/researchfoundation/docs/AHS_2008_Fletcher.pdf)，PDF第1页缩略语表SCU=Servo Control Unit；2026-09-18查阅，仅证明SCU的航空领域用法，不是VE25正式名称或架构依据。
- Q023-U1：本项目 MCU、SCU 正式英文名称仍待适用设备资料或项目术语表确认；补充Q002-U1和Q003的相关待确认项。
- 仅追加Q023与索引，并局部订正Q003的MCU字段描述；其余问答保持不变。未修改工程、运行MATLAB或生成中间文件，sp保持原有两项project XML改动。
- Delta-K route：project-only。
- R001（2026-09-18）：区分已确认设备角色与候选英文展开，核对实际输入字段并订正旧词表。

## Q024：MCU 输入信号结构的实际含义

### 范围与结论

- 日期：2026-09-18，Asia/Shanghai；修订 R001。
- 阶段：S2 接口追踪、S3 状态与有效性、S4 设备反馈；学习表现：掌握程度未确认。
- 当前输入是11台 MCU 的反馈集合：每台包含4项反馈信息，以及1个公共有效标志和2个单项无效标志。各台独立处理，随后分成8台升力电机与3台推力电机的反馈输出。
- 核对链：P4 SpIn_McuInfos → P11 Calc_MCUSCUData → P21 AbnormalMonitor / Update → P15 BUS_DevData。仅解释本题相关实现，未运行模型。

### 输入层级与字段

SpIn_McuInfos 是外层总线类型，其 MCU 字段是11元素的 Sp_McuInfo 总线数组。MCU(i) 保存第i台 MCU 的整组信息，组内7个字段均为标量。11是设备索引数量；字段自身 Dimensions=1 与外层数组长度11并不矛盾。

P11 的 Selector SID41 取索引1—8，SID42取9—11，分别送 For Each SID9、SID252；分区维度1、宽度1，逐台形成输出。P6 的 SP_SW_HLR-357—-367提供电机编号对应；P15 的 BUS_DevData 定义 LiftMCU[8]、ThrustMCU[3]。

| 字段 | 类型 | 当前处理中的实际作用 |
|---|---|---|
| Valid | boolean | 本台这组反馈的公共有效性条件；控制转速/功率监控，并作为故障等级/工作模式输出的valid。它不表示设备必然没有故障。 |
| McuFaultLevelFbk | uint8 | 设备上报的故障等级代码，送FaultLevel.data。 |
| McuWorkModFbk | uint8 | 设备上报的当前工作模式代码，送WorkMode.data；属于反馈。 |
| McuSpdFbk | single | 转速反馈数值，经过监控及历史保持后送SpdFbk.data。 |
| McuPowerFbk | single | 功率反馈数值，经过监控及历史保持后送Power.data。 |
| McuSpdFbkInvalid | boolean | 转速单项无效标志；true表示无效，模型取反后作为转速监控的dataInValid。 |
| McuPowerFbkInvalid | boolean | 功率单项无效标志；true表示无效，模型取反后作为功率监控的dataInValid。 |

Fbk 表示 Feedback（反馈），Spd 表示 Speed（转速），WorkMod 表示工作模式。uint8代码的物理语义需要码值表，不能按大小随意推断工作状态或故障严重程度。

### 有效性如何影响输出

转速与功率各有一个独立的 AbnormalMonitor，四个输入分别是：

1. dataIn：当前反馈数值；
2. dataInValid：对应 Invalid 标志取反；
3. valid：本台 MCU 的公共 Valid；
4. data0：single(0)，用于没有历史接纳值时的初始数值。

记 A 为该单项监控内部的 isAbnormal 状态，则当前实现满足：

- 转速输出valid = Valid AND NOT McuSpdFbkInvalid AND NOT A_spd。
- 功率输出valid = Valid AND NOT McuPowerFbkInvalid AND NOT A_power。
- 输出valid为true时接纳当前数值；false时保持上一次接纳的数值。自启动尚未接纳任何值时保持初值0。
- 原始Valid为false或单项Invalid为true，会让对应输出valid当拍变false，不需要等待异常计数达到门限。
- 若已经形成内部异常状态，原始标志恢复后仍需满足恢复判据才清除状态并重新接纳数值；不把输出valid简单等同原始标志的组合。
- 故障等级和工作模式在本模块中直接透传到各自data，valid均取公共Valid，没有经过上述两路异常监控；四项输出的reset均固定false。

实例参数绑定为 SpCPT.McuFbkMsg_abnormal、SpCPT.McuFbkMsg_abresume；P5第667—668行声明均为int32(5)。脚本声明值不等于已验证的运行时解析值；本题不推断具体第几拍或多少毫秒触发。

例如，假设当前功率监控没有内部历史异常，本台Valid=true、McuSpdFbkInvalid=true、McuPowerFbkInvalid=false，则转速输出无效并保持历史值，功率可以接纳当前值。故障等级/模式的输出valid仍为true；即使故障等级代码表示某种设备故障，该代码本身也可以是有效反馈。

这一结构支持分别表达“数据值是什么”和“这项数据能否被使用”，并让某个测量项失效时其余有效反馈继续独立处理。

### 输出组织与字段边界

每台输出为tMcuMsg，当前P11只写入以下4项：

| 输入反馈 | 输出结构 | 数值／有效性来源 |
|---|---|---|
| McuSpdFbk | SpdFbk：tSingleData | 异常监控的dataOut、validOut |
| McuPowerFbk | Power：tSingleData | 异常监控的dataOut、validOut |
| McuFaultLevelFbk | FaultLevel：tU8Data | 原始代码、公共Valid |
| McuWorkModFbk | WorkMode：tU8Data | 原始代码、公共Valid |

tMcuMsg还定义Temp、MotorTemp等其他字段，但本模型由零值tMcuMsg常量起步，Bus Assignment只覆写上述4项；其余字段保留零值结构。不能把输出类型中存在的字段，当成当前输入已提供的数据。这进一步补充Q023关于温度需求、输入字段和实际实现的区分。

### 待确认与来源

- 上游如何形成公共Valid及两个Invalid标志，本题未追溯；不能自行补写CRC错误、报文超时等具体原因。
- 字典中的Description和DocUnits为空。P6 的 -596/-597以Power_kw、Spd_rpm命名下游马达反馈，提供kW/rpm的单位线索；当前P11未见数值比例换算，但本题未闭合到相应需求消费点的完整链路，因此原始输入单位仍待适用接口依据确认。
- P6 的 -346只给出超时异常时FaultLevel=Level3(3)的需求规则，不构成完整输入故障等级枚举，也不能当作本模块已实现该规则的证明；未找到McuWorkModFbk的完整数字映射。
- P4：[SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)，data/chunk0.xml的SpIn_McuInfos、Sp_McuInfo；SHA-256：10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC。
- P11：[Calc_MCUSCUData.slx](D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/Calc_MCUSCUData.slx)，system_root.xml的SIDs41/42、9/252、211/223；system_9.xml的SIDs13、229/230、234/243、74及system_252.xml对应分支。SHA-256：0892EDC640482881BAE00D55D151F23181CA1027A479F6AC0B6F51E9E9978A04。
- P21：[SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)，system_1047的四输入及Abnormal/Update引用，system_2083的SID2106有效性门、system_1988的SID2077/2079更新与历史保持；SHA-256：0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490。
- P15：[SP_InteriorBus.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd)，tMcuMsg、BUS_DevData；SHA-256：5356B8602E6B3948291CA5410F5CE444D1277E5141AB93C21CF77D11D81C01F5。
- P27：[SpMdlPrivate_DataType.sldd](D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd)，tSingleData、tU8Data；SHA-256：E759D089EE8CF57876AD182E553ACE204672624A678F6C0F0F0A9F8D747F311D。
- P5：[SpCPT_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPT_define.m:667)，McuFbkMsg异常/恢复门限声明；SHA-256：49AAF3CBA343CA4B8E1A4C4B8959B1BBD4824C56378C36B61B74119F5DB218B0。
- P6：[SP_SW_HLR.slreqx](D:/666/VE25/sp/02_Requirements/specification/SP_SW_HLR.slreqx)，slrequirements/data.xml的-329、-336、-346、-357—-367、-595—-597、-680—-682；条款适用性不由本题自动确认。
- W37：[MathWorks：Switch](https://www.mathworks.com/help/simulink/slref/switch.html)，Block Behavior for Boolean Control Input；2026-09-18核对，用于确认Update内布尔门控true选择当前输入、false选择历史输入。
- 仅追加Q024与索引；保留既有问答和工程改动。未运行MATLAB、初始化、编译或仿真，sp无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-18）：沿当前实现解释设备数组、7字段、单项门控、历史保持和输出结构，并保留单位与编码缺口。

## Q025：MCU 字段数量与数组索引

### 范围与结论

- 日期：2026-09-20，Asia/Shanghai；修订 R001。
- 阶段：S2 接口与维度，关联 S3 For Each；学习表现：掌握程度未确认。
- 问题：SpIn _McuInfo 看起来只有7路信号，为什么 Selector 的输入大小为11，还能选择索引9—11？
- 结论：7是每台 MCU 的字段数量，11是 MCU 记录的数组长度。Selector 按设备记录索引，选中一台就保留该台完整的7个字段。本题细化Q024的输入层级，不改动既有结论。

### 字典定义与数据组织

P4 的外层类型 SpIn_McuInfos 只有一个字段 MCU，该字段的类型是 Bus: Sp_McuInfo，Dimensions=11。Sp_McuInfo 定义单台 MCU 的7个标量字段，各字段 Dimensions=1。

以下为结构示意，不是运行结果或数值矩阵：

```text
SpIn _McuInfo                     类型：SpIn_McuInfos
└─ MCU                           长度为11的总线数组
   ├─ MCU(1)                     第1台：完整7字段
   ├─ MCU(2)                     第2台：完整7字段
   ├─ ...
   └─ MCU(11)                    第11台：完整7字段

每个 MCU(i) 内部：
  Valid
  McuFaultLevelFbk
  McuWorkModFbk
  McuSpdFbk
  McuPowerFbk
  McuSpdFbkInvalid
  McuPowerFbkInvalid
```

例如 MCU(11).McuSpdFbk 表示“第11台 MCU 的转速反馈”；11定位设备，McuSpdFbk定位该设备中的字段。字段含boolean、uint8、single等不同类型，因此这里是总线数组，不是11×7的同质数值矩阵。

### 两次选择的分工

| 模块 | 选择依据 | 结果 |
|---|---|---|
| Bus Selector，SID7 | 字段名 MCU | 从外层总线取出整个11元素 MCU 数组 |
| Selector14，SID41 | 设备索引 [1:8] | 8台升力电机 MCU 的完整记录，每台仍有7字段 |
| Selector18，SID42 | 设备索引 [9:11] | 3台推力电机 MCU 的完整记录，每台仍有7字段 |

两组分别进入 ForEachSubsystem SID9 和 ForEachSubsystem1 SID252，承接Q024已核验的逐台处理。单台处理内部才进一步按字段名取出转速、功率、有效性等信息。

### 截图参数的含义与查看位置

- 输入维数=1：Selector处理一维数组。
- 输入端口大小=11：该一维输入数组含11个元素，每个元素是一台 MCU 的总线记录。
- 索引模式“从1开始”：第一个元素编号1，第11个元素编号11。
- 索引 [1:8]：取8条记录，输出仍是一维数组；下支路 [9:11] 取3条记录。
- “输入端口大小11”声明预期输入长度，不会把7个字段扩充成11个字段。
- 查看P4的数据字典时，先查看 SpIn_McuInfos 的 MCU 行，会看到 Dimensions=11；再进入 Sp_McuInfo，才会看到7个字段各自 Dimensions=1。这两个页面描述不同层级。

### 来源与记录

- P4：[SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)，data/chunk0.xml 的 SpIn_McuInfos.MCU（Bus: Sp_McuInfo，Dimensions=11）及 Sp_McuInfo 的7字段；SHA-256：10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC。
- P11：[Calc_MCUSCUData.slx](D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/Calc_MCUSCUData.slx)，simulink/systems/system_root.xml：输入SID5类型、Bus Selector SID7选MCU、Selector14 SID41的InputPortWidth=11/Indices=[1:8]、Selector18 SID42的InputPortWidth=11/Indices=[9:11]，以及连接到SID9/252的实际连线；SHA-256：0892EDC640482881BAE00D55D151F23181CA1027A479F6AC0B6F51E9E9978A04。
- W38：[MathWorks：Selector](https://www.mathworks.com/help/simulink/slref/selector.html)，Description、Number of input dimensions、Index mode、Input port size；2026-09-20核对，支持总线数组作为输入并按索引选择元素，以及一维输入长度与维数的区别。
- W19：[MathWorks：Group Nonvirtual Buses in Arrays of Buses](https://www.mathworks.com/help/simulink/ug/combining-buses-into-an-array-of-buses.html)，2026-09-20复查总线数组说明；工程中的11和7以P4/P11为准。
- 静态字典及模型连线已核验；未运行MATLAB、初始化、更新、编译或仿真。只追加Q025与索引，sp无新增中间文件，保留原有两项project XML修改。
- 本题维度关系无新增待确认项；不扩展为对实际运行时输入内容或整模型接口兼容性的验证。
- Delta-K route：project-only。
- R001（2026-09-20）：区分字段数、数组长度及输入维数，补充两层选择的分工与字典查看位置。

## Q026：MCU 的 For Each Subsystem

### 结论与处理链

- 日期：2026-09-20，Asia/Shanghai；修订 R001。
- 阶段：S2 接口，S3 迭代与状态；学习表现：掌握程度未确认。
- 用户截图为 P11 Calc_MCUSCUData 的 ForEachSubsystem（SID9），即 For Each Subsystem，逐元素处理子系统。
- 它使用同一套内部算法分别处理输入数组的各个元素。前面的 Selector14 取 MCU(1:8)，当前输入因此包含8份完整 MCU 数据，每份含Q025所述7个字段；当前分区宽度1，每次处理一份。
- 图标中的叠层方块与N是重复应用同一算法的示意，不是实际层数或固定迭代次数的显示，也不能据此判断实际采用多核并行。

```text
8份MCU数据
   ↓ 按设备拆分，每份保留7字段
第1份 → 单台MCU处理 → 第1份结果
第2份 → 单台MCU处理 → 第2份结果
 ……
第8份 → 单台MCU处理 → 第8份结果
   ↓ 自动组合
LiftMcuData：8个tMcuMsg元素
```

这里的“分别处理8次”是同一次子系统执行中的迭代，不是每拍处理一台、等8拍才完成。内部包含转速/功率处理、有效性判定与输出封装，详细实现沿用Q024。本题只核验分区与输出组织。

### 当前参数与模块特点

| 定位 | 参数／结构 | 含义 |
|---|---|---|
| Inport MCU，SID10 | Partition=on，PartitionDimension=1，PartitionWidth=1，PartitionOffset=0 | 沿一维输入数组逐个取出完整MCU元素 |
| For Each，SID11 | BlockType=ForEach | 子系统内部的迭代控制块，用于配置分区和输出组合 |
| Bus Selector，SID13 | 7个输出字段 | 对本次分区得到的单台MCU记录提取7字段 |
| Outport LiftMcuData，SID75 | Bus: tMcuMsg，PortDimensions=1，ConcatenationDimension=1 | 每次产生一个结果，沿第1维组合成外部数组 |

普通子系统本身不提供这种自动分区、逐份重复处理和结果组合机制；普通子系统内部仍可使用支持数组运算的块。

For Each 中的有状态块为各输入分区维护各自状态。例如内部的历史值保持与异常监控状态按设备分开，不会把第1台的状态当成第2台的历史。这里引用For Each的状态语义及Q024已经核验的有状态处理链，不代表实际运行验证。

在Simulink中双击该子系统进入内部，再双击 For Each 控制块，可查看 Input Partition（输入分区）和 Output Concatenation（输出组合）配置。该模块可从 Simulink / Ports & Subsystems / For Each Subsystem 获取；本题未创建或修改模块。

### 来源与记录

- P11：[Calc_MCUSCUData.slx](D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/Calc_MCUSCUData.slx)，simulink/systems/system_9.xml 的 SIDs10、11、13、75；根系统Selector14与SID9连接沿用Q025已核验来源；SHA-256：0892EDC640482881BAE00D55D151F23181CA1027A479F6AC0B6F51E9E9978A04。
- W21：[MathWorks：For Each Subsystem](https://www.mathworks.com/help/simulink/slref/foreachsubsystem.html)，Description、Configure the Subsystem、Libraries；2026-09-20复查同一时间步内迭代、独立状态及组合语义。
- W14：[MathWorks：For Each](https://www.mathworks.com/help/simulink/slref/foreach.html)，输入分区和输出组合；2026-09-20复查。
- 关联Q007—Q011的For Each解释及创建方法、Q024的单台MCU处理、Q025的7字段与11元素结构。
- 只追加Q026及索引；静态参数核验，未运行MATLAB、初始化、编译或仿真。未修改模型与设计说明，保留sp原有两项project XML修改，无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-20）：结合当前8元素MCU分支识别For Each图标，解释逐设备处理、结果组合和独立状态。

## Q027：MCU 与 FTADC 数据处理的异同

### 范围与结论

- 日期：2026-09-20，Asia/Shanghai；修订 R001。
- 阶段：S2 接口、S3 状态与有效性、S4 设备反馈；学习表现：掌握程度未确认。
- 问题：MCU 数据处理是否和 FTADC 一致？
- 结论：MCU的转速/功率与FTADC五个测量项确实复用同一个 SignalProcessingLib/AbnormalMonitor，公共的异常监控和无效值保持机制相同；整个模块不完全相同，差异包括单项有效性接口、参数绑定、初值、reset生成与处理对象。MCU故障等级/工作模式也不走这一公共监控。
- 本题比较当前 Calc_MCUSCUData 的MCU分支与 Calc_VotedFTADC，不扩展到SCU或后续导航选源。

### 公共机制

对每个被监控的测量项，公共库以两个有效性条件和异常状态共同判定输出：

```text
validOut = 公共Valid AND 单项有效 AND NOT isAbnormal

validOut=true  → dataOut采用当前输入
validOut=false → dataOut保持上一次接纳的值
尚未接纳过值  → 使用该实例的data0
```

原始公共Valid=false或单项有效=false时，输出valid当拍即可变false；异常/恢复计数用于形成及解除内部异常状态，不应将它们解释成“等待门限后才让无效数据停止更新”。完整公共逻辑沿用Q024，本轮复核AbnormalMonitor→Abnormal/Update、Switch和Delay连线。

### 实例差异

| 比较项 | MCU 转速／功率 | FTADC |
|---|---|---|
| 处理对象 | 11台MCU，分为8台升力组和3台推力组，由For Each逐台处理 | 一路FTADC，分别处理Psi、Pti、AOA、AOS、CAS五项 |
| 单项有效输入 | 对应 McuSpdFbkInvalid 或 McuPowerFbkInvalid 取反 | 对应 SpData_Single.valid 直接输入 |
| 公共有效输入 | 当前台MCU的Valid | SpIn_FTADC.Valid |
| 当前异常／恢复参数 | McuFbkMsg_abnormal / McuFbkMsg_abresume，脚本声明5/5 | 五项实例均绑定 FtadcPt_abnormal / FtadcPt_abresume，脚本声明100/100 |
| data0初值 | 转速、功率均single(0) | Psi/Pti均single(101)，AOA/AOS/CAS均single(0) |
| 输出单项结构 | 转速/功率为tSingleData(data,valid,reset) | 五项均为tSingleData(data,valid,reset) |
| 输出reset | 固定false | validOut由false变true时产生一拍true |
| 其他字段及输出组织 | FaultLevel/WorkMode直接透传数值，valid取公共Valid，reset=false；组入各台tMcuMsg及BUS_DevData | 将五项处理结果写入传入的BUS_NavData（raData），其余字段保留 |

参数是当前脚本声明及实际实例绑定的静态结果，不代表已验证的运行时解析值；本题不推断首拍触发边界、具体毫秒数或两类监控的实际响应时间比例。

尤其注意：P5虽然分别声明FtadcPs、FtadcAoa、FtadcAos、FtadcCas等门限，当前P28的五个实例全部引用FtadcPt这组参数。不能仅按信号名认为它们各自绑定同名参数；仅修改其他几组声明，不会通过这些实例当前的参数引用改变其门限。统一绑定的设计意图未确认，本题不认定缺陷。

### reset差异的实际含义

FTADC的每项输出都采用如下逻辑，Delay保存该项上一次的validOut，初值false：

```text
reset(k) = validOut(k) AND NOT validOut(k-1)
```

例如某项经过完整监控后的validOut依次为：

| 项目 | 第1拍 | 第2拍 | 第3拍 |
|---|---:|---:|---:|
| validOut（假设） | false | true | true |
| FTADC输出reset | false | true | false |
| MCU输出reset | false | false | false |

首次变为有效也满足这一条件。这里reset是随输出发布的有效性上升沿标志，不是本处重新启动异常计数或清空历史值的输入；具体下游模块如何消费该标志，本题未追溯。

当前Calc_VotedFTADC中未见For Each、跨设备两两比较、投票、Timeout调用或数值单位换算。名称中的Voted不能作为已经进行多路表决的证据。

### 来源与记录

- P28（新增）：[Calc_VotedFTADC.slx](D:/666/VE25/sp/04_Design/Calc_VotedFTADC/specification/Calc_VotedFTADC.slx)，simulink/systems/system_root.xml：AbnormalMonitor SID67/72/80/94/102；Delay32—36及NOT14/16/18/20/13、AND12/15/17/19/21生成reset；BusCreator22—26、BusAssignment63及输出64。SHA-256：8D469E282C99EF5DD471EA848EA0AA65B30B2B30342753FF06AAD6E1B1D5BEE6。
- P11：[Calc_MCUSCUData.slx](D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/Calc_MCUSCUData.slx)，system_9.xml的SID229/230、NOT234/243、初值236/248、reset常量14/26/31/35、BusAssignment74；system_252.xml的SID254/255等对应分支。SHA-256：0892EDC640482881BAE00D55D151F23181CA1027A479F6AC0B6F51E9E9978A04。
- P21：[SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)，system_1047.xml的Abnormal2237/Update2120；system_2083.xml的有效性门2106；system_1988.xml的Switch2077/Delay2079。SHA-256：0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490。
- P4：[SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)，SpIn_ftadc、SpData_Single、Sp_McuInfo；SHA-256：10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC。
- P5：[SpCPT_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPT_define.m:650)，650—659行FTADC各门限声明，667—668行MCU门限声明；实例绑定分别以P28/P11为准。SHA-256：49AAF3CBA343CA4B8E1A4C4B8959B1BBD4824C56378C36B61B74119F5DB218B0。
- 只追加Q027及索引；保留原问答及sp两项project XML修改。仅静态核验，未运行MATLAB、初始化、编译或仿真，未修改工程与设计说明，sp无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-20）：区分公共算法复用与实例配置，补充FTADC有效性上升沿reset及五项共用Pt门限的实现事实。

## Q028：SCU 输入信号结构的实际含义

### 范围与结论

- 日期：2026-09-20，Asia/Shanghai；修订 R001。
- 阶段：S2 接口与映射，S3 有效性和状态，S4 舵面反馈；学习表现：掌握程度未确认。
- 当前 SpIn _ScuInfo 是8个 SCU 反馈记录的集合，每份含1个位置值、3个状态码和1个公共有效标志。其实际作用是分别表达“当前位置”“收到的数据能否使用”“设备处于哪类状态”。
- 以当前P4字典、P11模型接线为实现依据；需求P6用于设备对应与输出枚举语义，不将不同输入结构的需求判据直接等同当前实现。

### 输入层级与舵面对应

```text
SpIn _ScuInfo                  类型：SpIn_ScuInfos
└─ SCU                        长度8，元素类型Sp_ScuInfo
   ├─ SCU(1)：5个字段
   ├─ SCU(2)：5个字段
   ├─ ……
   └─ SCU(8)：5个字段
```

外层SCU的Dimensions=8；每份记录的5个字段自身均Dimensions=1。8是设备记录数量，5是每份记录的字段数量。

| 索引 | 当前输出字段 | 对应舵面 |
|---:|---|---|
| 1 | AilLI | 左内副翼 |
| 2 | AilRI | 右内副翼 |
| 3 | AilLO | 左外副翼 |
| 4 | AilRO | 右外副翼 |
| 5 | EleL | 左升降舵 |
| 6 | EleR | 右升降舵 |
| 7 | RudL | 左方向舵 |
| 8 | RudR | 右方向舵 |

该表同时由当前Selector至BUS_DevData的接线及P6的SP_SW_HLR-368—-375支持。模型Annotation SID305仍保留“确定一下SCU1-8对应的舵面名字”字样；这里报告已读实现与需求一致的映射，不代表替代硬件接线或ICD确认。

### 五字段的作用

| 字段 | 类型 | 实现中的实际作用与解释边界 |
|---|---|---|
| Valid | boolean | 本份输入的公共有效标志；直接作为Position.valid，并参与两个故障状态的判定。true不等同舵机无故障。 |
| INT_STS | uint8 | 设备状态码；等于0时参与FaultStatus与ClawFaultStatus的异常分类。正式英文展开和完整输入码表未确认，不将INT擅自展开为初始化或中断。 |
| EN_STS | uint8 | 按命名可理解为使能状态类字段；当前实现以是否等于0参与故障分类。正式定义和非零码含义待ICD，不将uint8视作已经确认的布尔量。 |
| STK_MON | uint8 | 当前实现中等于0时优先归入ClawFaultStatus=1；结合需求的Jam(1)枚举，可判断其在此链路承担卡滞分类输入的作用。该作用推导不替代原始字段的正式全称与码表。 |
| Position_Fdb | single | 位置反馈原始数值，直接传到Position.data。本模块未作单位换算；原始单位、正方向和零位未由当前字典说明。 |

P4的Description与DocUnits为空。P6未检出四个原始字段名INT_STS、EN_STS、STK_MON、Position_Fdb；未据此声称整个项目不存在ICD或其他字段定义。

### 处理与输出

输入SCU数组经Bus Selector SID300进入ForEachSubsystem2 SID118。其Inport SID119分区维度1、宽度1、偏移0，逐份处理8个记录；每次输出tServoMsg，随后组合并按上表分配舵面。

每台输出结构为：

```text
tServoMsg
├─ Position        ：tSingleData(data, valid, reset)
├─ FaultStatus     ：tU8Data(data, valid, reset)
└─ ClawFaultStatus ：tU8Data(data, valid, reset)
```

**位置：**

```text
Position.data  = Position_Fdb
Position.valid = Valid
Position.reset = false
```

当前分支只作Signal Copy和总线封装，未使用MCU/FTADC的AbnormalMonitor、Delay历史值保持或异常恢复门限。因此Valid=false时，位置数值仍沿线传递，同时有效性为false；不能说本模块会冻结上一次位置或清零。

**一般故障状态：**

```text
若 NOT Valid OR INT_STS==0 OR EN_STS==0：
    FaultStatus.data = 1
否则：
    FaultStatus.data = 0

FaultStatus.valid = true
FaultStatus.reset = false
```

此处没有把STK_MON纳入FaultStatus的判据。P6的-662给出Failure(1)、Normal(0)及Unknown(2)语义，但其完整判据与当前简化输入不一致；例如需求使用“报文超时异常”，不能直接等同本模型的Valid=false并认定实现符合或违反该条款。

**控制律故障类别：**按Switch串联关系，优先级从上到下：

| 优先级 | 当前输入条件 | ClawFaultStatus.data | P6输出枚举语义 |
|---:|---|---:|---|
| 1 | Valid=false | 2 | Float |
| 2 | Valid=true 且 STK_MON=0 | 1 | Jam |
| 3 | Valid=true、STK_MON非0，且INT_STS=0或EN_STS=0 | 2 | Float |
| 4 | Valid=true、STK_MON非0、INT_STS非0、EN_STS非0 | 5 | Valid |

ClawFaultStatus.valid固定true，reset固定false。上述Jam/Float/Valid的数值定义来自P6的SP_SW_HLR-663、-628；当前原始码到输出码的条件来自P11。这里不把Float进一步解释成已经发生特定机械脱开动作。

Constant SID167/182没有显式保存Value，按标准Constant默认值1解释；模型bddefaults未覆盖该值，W33官方说明支持默认值1。这是静态缺省参数解释，未运行MATLAB查询实例解析值。

### 有效性与故障状态为什么分开

Position.valid表示位置反馈是否被标记为有效，FaultStatus/ClawFaultStatus.data则表达设备状态分类。后两者的valid固定true，表示当前模型生成并发布该分类结果，不是输入Valid的透传。

例如假设Valid=true、INT_STS=0、EN_STS非0、STK_MON非0：Position.valid仍为true，FaultStatus.data=1、ClawFaultStatus.data=2。这样可以同时表达“收到有效的位置反馈”和“设备状态触发异常分类”。

P6的SP_SW_HLR-629另规定下游Surface_deg由RVDT位置乘gain再加bias得到，并在无效时保持输出；本题未追踪该下游实现，不能把那里的角度单位和保持逻辑倒推成本输入或当前模块已经具有的功能。

### 来源与记录

- P4：[SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)，data/chunk0.xml的SpIn_ScuInfos、Sp_ScuInfo；SHA-256：10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC。
- P11：[Calc_MCUSCUData.slx](D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/Calc_MCUSCUData.slx)，system_root.xml的SID108/300/118、215—222、211及Annotation305；system_118.xml的SID119/131/146、位置封装111/112/302—304、故障逻辑158—170、197/201/202/187—191、状态封装164/175、输出206/210。SHA-256：0892EDC640482881BAE00D55D151F23181CA1027A479F6AC0B6F51E9E9978A04。
- P15：[SP_InteriorBus.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd)，tServoMsg及BUS_DevData；SHA-256：5356B8602E6B3948291CA5410F5CE444D1277E5141AB93C21CF77D11D81C01F5。
- P6：[SP_SW_HLR.slreqx](D:/666/VE25/sp/02_Requirements/specification/SP_SW_HLR.slreqx)，slrequirements/data.xml的SP_SW_HLR-333、-335、-368—-375、-662、-663、-628、-629；SHA-256：D48948EB3E54C2129D0C18B4C80DCF0628597F3621EEC78BC0EBC5A8BCC4338D。
- W33：[MathWorks：Constant](https://www.mathworks.com/help/simulink/slref/constant.html)，Constant value参数；2026-09-20补核标准Constant默认值1。
- Q028-U1：INT_STS的正式名称，三个状态码的完整原始定义及合法值，Position_Fdb的单位/方向/零位，仍待适用ICD或设备资料；补充Q003相关原始字段待确认项。
- 仅追加Q028和索引，保留既有问答与sp两项project XML修改。静态检查，未运行MATLAB、初始化、编译或仿真；未修改工程、设计说明或共享索引，sp无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-20）：建立SCU八设备五字段与舵面输出的关系，区分输入有效性、位置透传、一般故障状态及控制律故障分类，并保留原始码表与单位缺口。

## Q029：MPIO 输入数据结构的实际意义

### 范围与结论

- 日期：2026-09-20，Asia/Shanghai；修订 R001。
- 阶段：S2 接口与消费者，S3 有效性，S4 航电综合输入；学习表现：掌握程度未确认。
- SpIn_MpioInfo是端口名，当前类型为SpIn_Mpio。该类型是一份含31个不同业务字段的标量总线，全部字段Dimensions=1；没有MCU/SCU那样的外层设备数组。31是字段数，不是31台设备。
- 从实际内容与消费者看，可以把它理解为航电综合信息进入SP的一个入口：提供能源状态和能力、飞机参数、航前检查，以及FMS目标/请求/状态。
- MPIO的项目正式英文展开和完整硬件职责边界仍未确认，不因接口内容而将它等同某一具体设备或传感器。

### 字段分组与现实含义

| 分组 | 当前字段 | 实际业务含义 |
|---|---|---|
| 公共有效性（1项） | Valid | 这份输入数据的公共有效标志，boolean |
| 能源与高压（5项） | EgyWkSts、HiVltgSts、HiVltgSW、Usable_Pwr、Usable_Pwr_Valid | 能源系统工作模式/状态、高压连接状态、高压开关量，以及可用功率和其单项有效码 |
| 飞机参数（4项） | Center_Gravity、Center_Gravity_Valid、Gross_Weight、Gross_Weight_Valid | 飞机重心、飞机总重，以及各自的单项有效码 |
| 航前检查（3项） | Motor_Check、Surface_Check、Preflight_Check_Valid | 航前桨/电机检查、舵面检查、检查单状态或有效性相关码 |
| FMS目标/指令及配套码（14项） | FMS_Alt_Target、FMS_Spd_Target、FMS_TrueHeading_Target、FMS_Track_Target、FMS_Vertical_Speed_Target、FMS_FPA_Target及各自的_Valid；FMS_Roll_Command及FMS_Roll_Command_Status | 高度、速度、真航向、航迹方向、垂直速度、飞行路径角目标，滚转指令，以及目标有效码或指令状态码 |
| FMS请求和状态（4项） | FMS_Trans_Request、FMS_LNAV_Request、FMS_VNAV_Request、FMS_VNAV_Status | 转换请求、横向导航请求、垂直导航请求及垂直导航状态；具体请求编码与转换类型需接口依据 |

合计1+5+4+3+14+4=31。数值字段为single，状态/请求/单项有效码为uint8，只有公共Valid是boolean。

Target表达期望达到或保持的飞行目标；例如高度目标说明希望飞机跟踪的高度。Request表达请求，当前输入字段本身不能证明模式已经接通或转换已经完成。Status提供状态类信息。

P6的SP_SW_HLR-387、-488把能源、高压、功率、重心、重量和航前检查列为IDU类输入；P14把FMS_FPA_Target写入FMS_Flight_Path_Angle_Target，直接支持FPA在此为飞行路径角用途。以上是当前业务用途，不替代设备选型或接口码表。

### 两类实际处理

P12的Calc_MPIOData从31项输入中消费公共Valid和3个连续量有效码，形成27个已赋值的业务输出：

1. **可用功率、重心、重量：**
   - 单项有效条件分别为Usable_Pwr_Valid==uint8(1)、Center_Gravity_Valid==uint8(1)、Gross_Weight_Valid==uint8(1)。
   - 与公共Valid一起进入SignalProcessingLib/AbnormalMonitor。
   - 输出valid为true时接纳当前值，否则保持历史接纳值；尚未接纳过值时使用single(0)。
   - 三个实例均绑定SpCPT.IduMsg_abnormal / IduMsg_abresume，P5第703—704行声明为int32(50)/int32(50)。本题不推断实际毫秒或首拍边界。

2. **其余24项：**
   - 包含能源/高压状态、检查状态、全部FMS目标值及其业务有效码、Request和Status。
   - 直接封装：data=原始字段，valid=公共Valid，reset=false。
   - 当前模块没有对这些字段执行异常保持、请求码解码、模式切换或目标选择。

上述27项输出的reset全部固定false；当前P12未见For Each、显式Timeout或单位换算。P6的-395是更完整的IDU处理需求流程，不能把需求中列举的超时监控当成本模型已存在对应调用的证明。

### 两层有效性

FMS字段的业务有效码与SP封装的valid是不同层次。例如：

```text
输入 FMS_Alt_Target
    → 输出 FMS_Alt_Target.data

输入 FMS_Alt_Target_Valid
    → 输出 FMS_Alt_Target_Valid.data

输入 Valid
    → 输出 FMS_Alt_Target.valid
    → 输出 FMS_Alt_Target_Valid.valid
```

因此，当前模块没有将FMS_Alt_Target_Valid直接接到FMS_Alt_Target.valid，也没有用前者冻结目标数值。它保留目标和业务有效码两份信息，并分别带着公共有效性向下游传递。

例如假设公共Valid=true、输入FMS_Alt_Target_Valid=0，则输出目标总线的.valid仍为true，同时独立的目标有效码.data仍为0。必须结合业务有效码及下游规则解释目标是否可使用，不能只看目标总线的.valid。

P14的Calc_FMSMsg_ToClaw进一步提取FMS目标和配套码的.data，多项有效/请求码转换为boolean，VNAV请求转换为ENUM_FMS_VNAV_Request；其根层没有使用这些目标封装的.valid去拦截目标数值。本题仅核对组包接线，不推断控制律内部是否接纳或执行这些请求。

### 输出与消费者

P12输出类型BUS_MpioData含28个字段，其中27项在当前Bus Assignment中写入；额外的SelectedFCC不来自本输入，也未在此赋值，保持初始化零结构。27个已赋值项包括10个tSingleData与17个tU8Data。

当前主线数据流为：

```text
VE25SP/SpIn_MpioInfo
    → Calc_MPIOData
    → MpioData
       ├─ Calc_FMSMsg_ToClaw：飞管目标/请求及飞机参数
       ├─ Calc_PilotIn_ToClaw：驾驶员输入组包消费者
       ├─ Calc_SYSMsg_ToClaw：系统信息组包消费者
       ├─ Calc_CcdlMsg_Out
       └─ Calc_SPDispMsg_Out
```

本题确认了这些实际连接；除FMS入口外未展开其他消费者的内部算法。

### 待确认与来源

- P4各字段Description与DocUnits为空。输出名UsablePwr_KW，以及P6的Mass_AC_Kg、Xcg_AC_100MAC提供单位/表达线索，但不替代原始输入ICD；输入高度基准、速度定义、角度/重量/功率单位、重心参考系仍应按适用接口确认。
- 完整能源/高压/检查/FMS状态和请求码表、MPIO正式全称仍待确认。P6的-668引用《VE-IC10-001-01航电飞控CAN报文定义》，-493引用《VE25-100_ControlLaw_IO》；这里只发现引用，不宣称已取得文件正文或确认其适用版本。
- P4：[SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)，data/chunk0.xml的SpIn_Mpio全部31字段；SHA-256：10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC。
- P12：[Calc_MPIOData.slx](D:/666/VE25/sp/04_Design/Calc_MPIOData/specification/Calc_MPIOData.slx)，system_root.xml的SID4/5、监控73/76/80、比较121—123、FMS示例152/156及From146/147/153/157、初始化119/赋值120/输出113；SHA-256：A5B051AE97E92967CCF9E101D137D566B78DC55C57FC90B78910BA7F841F2FE6。
- P15：[SP_InteriorBus.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd)，BUS_MpioData的28字段；SHA-256：5356B8602E6B3948291CA5410F5CE444D1277E5141AB93C21CF77D11D81C01F5。
- P1：[VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)，system_root.xml的SID15→362→381以及MpioData各From至SID376/377/397/406/408；SHA-256：20D8C7DE2A103D8FF298BCD5BB0C28F73AA6CCC2E27D43D0F5AB9952365F77E4。
- P14：[Calc_FMSMsg_ToClaw.slx](D:/666/VE25/sp/04_Design/Calc_FMSMsg_ToClaw/specification/Calc_FMSMsg_ToClaw.slx)，system_root.xml的SID4/6、目标.data提取及转换42—45/52—58、BusAssignment23与输出21；SHA-256：8AFD3DAFFF28413E431F93C6924235202687E863F70C3981F42A52C7253A6F70。
- P5：[SpCPT_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPT_define.m:703)，IduMsg异常/恢复门限；SHA-256：49AAF3CBA343CA4B8E1A4C4B8959B1BBD4824C56378C36B61B74119F5DB218B0。
- P6：[SP_SW_HLR.slreqx](D:/666/VE25/sp/02_Requirements/specification/SP_SW_HLR.slreqx)，slrequirements/data.xml的SP_SW_HLR-387、-395、-488、-493、-588、-589、-666、-668、-781；SHA-256：D48948EB3E54C2129D0C18B4C80DCF0628597F3621EEC78BC0EBC5A8BCC4338D。
- 公共AbnormalMonitor语义沿用Q024/Q027已核验的P21；本题不重复审计内部计数时序。
- 仅追加Q029及索引，保留旧问答与sp两项project XML修改。静态核验，未运行MATLAB、初始化、编译或仿真；未修改模型或设计说明，sp无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-20）：按实际业务分组解释31字段综合输入，区分三项连续量监控、直接封装与FMS两层有效性，并定位下游消费者。

## Q030：学习进度更新

### 当前定位

- 日期：2026-09-21，Asia/Shanghai；修订 R001。
- 范围：本任务可见问题与Q001—Q029记录，重点比较2026-09-18的Q022与其后Q023—Q029；按P7学习计划S1—S6任务和验收项评估，不推定其他任务或线下学习情况。
- 当前阅读范围已经推进到S4的设备反馈和航电综合输入，覆盖MCU、SCU、MPIO；同时继续深化S2的接口追踪与S3的公共机制理解。
- 这是按多条线交叉推进的学习进度，不等于S1—S3已全部通过验收。完整CAS链、独立逐拍推演和验证方案仍未见完成证据。
- 不用题数、已提供答案或对话日期换算学习完成率；不把未展示掌握解释成必然不会。

### 相较Q022的新增进展

| 方向 | 新增问题与已讨论材料 | 可以观察到的用户表现 |
|---|---|---|
| 设备反馈接口 | Q023—Q026：MCU/SCU角色、MCU七字段与十一设备记录、8/3分组和For Each | Q025主动指出“7字段与索引11”的表面矛盾，说明正在对照字典与连线检查结构 |
| 共用机制与实例差异 | Q027：MCU和FTADC的AbnormalMonitor复用、初值、有效性、reset与参数绑定差异 | 用户主动提出跨模块比较，已从识别块名扩展到判断处理逻辑是否相同 |
| SCU业务处理 | Q028：八设备五字段、位置透传、一般故障与控制律故障类别 | 持续追问结构的现实含义；尚未展示独立推演位置有效性与设备故障的组合场景 |
| MPIO综合输入 | Q029：能源、飞机参数、检查与FMS目标/请求；三项连续量监控和两层有效性 | 业务覆盖从执行机构反馈扩展到航电综合输入；尚未展示独立解释其下游消费链 |

FTADC共用Pt参数、SCU故障分支、MPIO字段分组与两层有效性等具体结论由分析过程查得，计为已提供学习材料，不计为用户独立发现或已经掌握的证明。

### 对照学习计划

| 阶段 | 当前进度 | 下一项可验证成果 |
|---|---|---|
| S1 总体架构与工程入口 | 当前主线明确为VE25SP；对设备输入、总线与模块职责的认识继续扩大，总体地图尚未验收 | 用一张图讲清七类业务输入、主要处理和输出消费者，并区分工程入口/依赖 |
| S2 CAS全链路 | 局部接口与分支阅读已有积累，近期重点在设备输入；原计划CAS完整链仍未见自主闭合 | 独立串起CAS原始输入至最终控制律输出，并解释至少一个故障场景 |
| S3 公共机制与时序 | 已讨论For Each、Delay、状态编码、异常监控、历史保持及reset差异，跨模块比较增多 | 给出一个监控器的输入、历史状态、计数、data/valid/reset逐拍表 |
| S4 全部业务模块 | 已实质扩展到MCU、SCU、MPIO和FTADC对照，不能再按“仅触及RA”评价 | 补驾驶员/地面站输入、系统状态与输出组包，并形成可独立说明的输入到消费者链 |
| S5 需求与设计依据 | 已有追问来源和实现/需求对照的材料，原始ICD码表、单位及部分编号映射仍有缺口 | 一条需求—模型—参数—输出—测试的追踪链及一次纸面影响分析 |
| S6-A 验证方案 | 当前记录没有完整测试方案 | 为一个明确对象列正常、边界、故障、恢复场景及判定标准 |
| S6-B 实际维护与验证 | 当前记录没有获准模型变更、仿真或回归验证成果；既有工作为静态学习 | 后续在明确授权和环境条件具备时完成基线、限定改动及验证；当前不认定已完成 |

### 下一步重点

建议先把近期的横向比较转成一份可以核对的独立成果：

1. 选一台MCU的速度反馈与一台SCU的位置反馈。
2. 自行填写“正常、输入无效、恢复”场景下的data、valid、reset，并标出决定结果的块。
3. 对MCU注明进入恢复场景时是否已有内部异常状态；不能只凭原始有效标志恢复就假定立即重新接纳数值。计数边界尚未核验时保留待确认。
4. 沿其中一个输出继续追到实际消费者，说明保持、转换或默认值发生在哪里。

这份局部对照能够检验近期Q024—Q028的解释是否转化为自主理解；随后回到计划既定的CAS完整链与故障恢复推演，闭合S2/S3的验收。当前只提出下一步重点，不自动启动模型运行或维护练习。

### 来源与评估边界

- P7：[SP模型学习与维护进阶计划](D:/666/VE25/Doc/SP模型学习与维护进阶计划.md:53)，第4章S1—S6及第5.3节；本轮SHA-256仍为8D3F164BF800D48DD311D01C9BD5D03517FD971433DA3E09EA915B3C623AB27B。
- 学习证据：[问题索引](D:/666/VE25/Doc/SP模型学习问答记录.md:13)、Q022的上次评估、Q023—Q029的用户提问和记录；本条追加前文件SHA-256：22F26297F451ACDA7401EEF70DBA962D51BB20F846C88D53C92297EB3374ED93。
- 主线继续遵循用户已确认的VE25SP及其真实引用链。原计划关于SpMdl是否废弃等旧待确认描述，不重新计作用户的学习缺口。
- 当前缺少的ICD、单位、码表或历史设计依据属于资料/追踪缺口，不直接等同学习能力不足。未勾选阶段也不表示近期没有推进。
- 本题是学习进度评估，核验的是记录与计划的对应，不是用户能力考试或模型运行验证。
- 只追加Q030及索引，不修改原计划状态、既有学习表现或工程。未运行MATLAB，sp保留原有两项project XML修改，无新增中间文件。
- Delta-K route：project-only。
- R001（2026-09-21）：更新Q022之后设备反馈和MPIO的覆盖进展，认可结构检查与跨模块比较的可观察表现，并保留完整链、逐拍和验证闭环的验收边界。

## Q031：需求／测试文档中的STS和SRS

### 问题与结论

- 日期：2026-09-23，Asia/Shanghai；修订 R001。
- 用户明确语境为“需求／测试文档名称”，本题不沿用信号字段中Sts=Status的解释。
- **本地明确的SRS定义：Software Requirements Standards，软件需求标准。** 当前工作区的MB_11_06_SRS.docx标题、文档标题栏和缩写表一致；其作用是规定开发高级需求的方法、规则和工具。可以理解为“需求应当怎样编写”。
- **另一种通用SRS用法：Software Requirements Specification，软件需求规格说明书。** 此类文档描述软件必须满足的功能、性能、接口等要求，可以理解为“软件必须做什么”。NASA手册采用此全称，但不能据此覆盖本地MB_11_06_SRS.docx的明确标题。
- **STS在测试文档中的候选：Software Test Specification（软件测试规范／规格说明书），或System Test Specification（系统测试规范／规格说明书）。** 这两种用法均有公开一手示例；主要规定测试对象、条件、步骤及预期结果，可以理解为“怎样检查是否满足要求”。两者均尚未被确认为本项目STS的正式全称。
- 因此不能仅凭SRS/STS三个字母，认定文件类型、软件/系统层级或相互配套关系，应以具体文档标题、缩写表和范围为准。

### 本地证据与限制

1. [MB_11_06_SRS.docx](D:/666/VE25/sp/01_Planning/standards/MB_11_06_SRS/MB_11_06_SRS.docx)：
   - word/document.xml按非空段落计数，第2、10段为Software Requirements Standards，第18段文档标题含for <Project>；第89—90段缩写表为SRS及Software Requirements Standards for <Project>。
   - 第55段描述需求开发的方法、规则和工具；第62段说明它是模板。正文仍有<Project>占位，本次只确认文档自身定义，不将其视为已项目化批准的需求规格说明。
   - SHA-256：FD2B706B05079D010F5D5CD3FD6E1DEA8F84E0E71BE44D0B0321CB76CEBCE094。

2. [STS0P42变更，系统级需求分析后测试点.xmind](D:/666/VE25/sp/SimulationTest/STS0P42变更，系统级需求分析后测试点.xmind)：
   - content.json根主题与文件同名；读取197个主题标题，存在STS0P42及带条款后缀的标识，未取得STS英文全称定义。
   - “系统级需求分析后测试点”提供测试语境，但不能单独证明STS中的S=System，或证明STS0P42本身就是测试规范。其正式全称及编号/版本规则仍待原文封面或缩写表确认。
   - SHA-256：2B1BF5B6934BECE8F0CFB5A3341DE648B4DA9329E7B7191988C06B3A9268F057。

3. 本轮对P6需求正文的限定检索未命中独立STS/SRS及所查中英文展开式。此负结果不表示整个工程不存在相关定义；也未断定用户看到的文件就是上述本地候选。

### 通用术语来源

- [NASA：SRS—Software Requirements Specification](https://swehb.nasa.gov/pages/viewpage.action?pageId=103776647)，页面标题和Rationale节；这里只引用缩写与文档职责，不作为VE25项目定义。
- [Zephyr：Safety development concept](https://github.com/zephyrproject-rtos/zephyr/issues/57516)，Test specifications列Software Test Specification (STS)，为其项目自身用法示例。
- [California State University, Sacramento：Guide to Preparing the System Test Specification](https://athena.ecs.csus.edu/~buckley/CSc191/std.pdf)，第1页Introduction/Purpose及缩写示例给出System Test Specification (STS)。
- 本轮仅追加Q031及索引，未修改模型、字典、代码、需求或设计说明；sp保持既有两项project XML修改，无新增中间文件，未运行MATLAB。
- Delta-K route：project-only。
- R001（2026-09-23）：按用户补充的文档语境回答；优先记录本地SRS=Standards的明确证据，将Specification及STS两个展开式标明为通用用法/待确认候选。

## Q032：ADA在当前文档语境中的含义

### 结论与边界

- 日期：2026-09-23，Asia/Shanghai；修订 R001。
- 本题紧接STS/SRS的需求／测试文档讨论，但用户尚未给出包含ADA的完整文档名、编号或句子。
- **当前本地证据不足以确定ADA的项目含义。** 不能因航空软件背景，就直接认定它是某类文档缩写、导航设备名称或开发语言。
- 若原文在讨论开发语言、编码规范、编译器，**Ada编程语言**是一个有依据的候选：它用于嵌入式、实时及航空等高可靠性软件。其名称来自Ada Lovelace，**不是三个英文单词首字母构成的缩写**。
- 上述候选仅说明通用用法，不证明VE25/SP采用Ada，也不证明用户所见ADA就是该语言。若ADA出现在文档编号或标题中，需要封面、缩写表或上下文才能确认。
- 已向用户请求完整名称/编号/句子；确认后应在本条修订并保留候选与正式定义的区别。

### 本轮限定检索

- sp/01_Planning下9份DOCX的word/document.xml正文：独立ADA/Ada及下划线、连字符相邻ADA均未命中；不包含图片文字、批注、页眉页脚或嵌入附件。
- [SP_SW_HLR.slreqx](D:/666/VE25/sp/02_Requirements/specification/SP_SW_HLR.slreqx)的需求description（HTML解码后）未命中独立ADA；本轮SHA-256为D48948EB3E54C2129D0C18B4C80DCF0628597F3621EEC78BC0EBC5A8BCC4338D。
- [高级需求表](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)的共享字符串、[集成测试报告](<D:/666/VE25/sp/SimulationTest/VE25 42版本控制律集成测试结果.docx>)的拼接段落文本均未命中。
- 当前VE25SP.slx的simulink/systems XML及SP_Step_New.sldd的数据XML，所查独立ADA模式未命中。Adahru/ADAHRS等更长标识不作为独立ADA定义。
- 以上为指定文件和正文范围的负结果，不等于全工程任何位置都不存在ADA，也不替代用户正在阅读的原文。

### 通用来源与变更

- [AdaCore：Programming Languages for Space Software，2.1.2 Ada language background](https://learn.adacore.com/booklets/adacore-technologies-for-space-systems-software/chapters/programming.html)：说明Ada名称非缩写、命名来源及航空嵌入式软件应用。
- 仅追加Q032及索引，未修改工程或设计说明；sp保留已有两项project XML修改，无新增中间文件，未运行MATLAB。
- Delta-K route：project-only。
- R001（2026-09-23）：记录限定检索无定义的结果，给出Ada语言的条件性解释，保留ADA项目含义待原文确认。

## Q033：当前VE25SP的Bus结构文档

- 日期：2026-09-23；修订R001。用户明确要求生成独立Markdown，按模型层级或信号链路组织，内容以模型和字典定义为准。
- 成品：[VE25SP_Bus信号结构说明.md](D:/666/VE25/Doc/VE25SP_Bus信号结构说明.md)。
- 覆盖当前VE25SP递归引用的18模型、27字典；20输入/9输出；85个Bus的1430个直接字段，以及8种当前引用枚举。
- 文档分为输入、导航预处理/表决、MCU/SCU/MPIO、地面站/驾驶员输入、系统状态和输出组包；字段表保存原名、顺序、原始类型、维度、嵌套关系、单位/描述/限值原文和来源。
- 61个Bus（611字段）有当前类型或嵌套引用证据；24个仅在字典闭包可见而未定位当前使用的Bus另列，未假定均参与当前算法。
- 补充25个Bus Assignment、175个Bus Creator、13组For Each（32输入和13输出端口）的实际保存设置；区分端口维度和成员数组维度。
- 保留BUS_SpMsgOut.Data原始裸类型名SpCcdlMsg；保留tSignalState.stateBits字典七位Description，并标注SignalStateSet_New四位实现差异及primary来源，未擅改模型或字典。
- 核验：85个结构/1430字段逐行与原始SLDD XML一致；49个来源哈希未变；187本地链接、1081内部链接及表格列数通过。模型接口及主要连接另经独立静态审核。
- 成品SHA-256：367C83C5C81EE50104217E53D05C9F4AE11206591F3CE1C61635664E9DA81E8A。
- 过程证据：[validation.json](D:/666/VE25/.runtime/sp_learning_qa/bus_structures_20260923/validation.json)、[检查点](D:/666/VE25/.runtime/sp_learning_qa/bus_structures_20260923/checkpoint.md)。仅当前任务新增成品和过程文件；其他任务的设计说明未改动。
- 静态文档核验，不代表加载、编译或仿真通过。工程保持既有两项project XML修改，sp无新增中间文件。
- closeout_outcome=completed；verification_status=passed（静态文档核验）；Delta-K route=project-only。

## Q034：RA信号从输入到控制律输出的完整处理链

### 问题、结论与阅读地图

- 日期：2026-09-23，Asia/Shanghai；修订 R001。
- 用户请求：“模型中关于ra信号的处理你帮我理一条线出来”。
- 范围：当前VE25SP及其实际引用的RAMsgsPreprocess、Calc_VotedRA、Calc_VotedFTADC、Calc_NAVMsg_ToClaw和SignalProcessingLib；以当前保存模型、参数与字典为准。
- **主线是两台RA输入 → 补一个无效占位以复用三路预处理 → 一致性状态计算 → 按两路valid和OnGnd选择一个源 → 形成高度/有效性/reset → 写入导航总线 → 映射到控制律接口。**
- 本次选源已核实为`Enum_VoterType.SourceSelect`。一致性状态用于状态输出，当前选源决策只读取两台RA的`valid`；不能将“存在两两比较”理解成“根据失配状态自动剔除或按高度多数表决”。

```text
VE25SP/SpIn_RA
  SpIn_Ra.L164_Radio_Altitude[2]
  每台：{data: single, valid: boolean}
       |
       v
VE25SP/NavSpApp/Model → RAMsgsPreprocess
  提取两份输入 + {data=0, valid=false}
       |
       v
SignalProcessingLib/Calc_SignalState
  原data/valid ──────────────────────┐
  三组两两比较 → 同步/失配/禁用状态  |
       |                            |
       └────→ raNormalInfos[3] ←────┘
       |
       v
VE25SP/NavSpApp/raVoter → Calc_VotedRA
  前两路valid + 上一拍源号 + OnGnd
       |
       +→ RaSrc：1或2 → NavSpApp/NavLog
       |
       +→ raState.raRadioAlt_M
       |    三路signalState + voterState
       |    → 根层NavDataState
       |
       +→ raData.raRadioAlt_M = {data, valid, reset}
            同时记录ra1RadioAlt_M和ra2RadioAlt_M的data/valid
               |
               v
       Calc_VotedFTADC → NavSpApp/Subsystem
       补写FTADC/CAS字段，保留RA字段
               |
               v
       NavData（BUS_NavData）
          +→ 根层SpIn_Nav2Display
          └→ Calc_NAVMsg_ToClaw
                raRadioAlt_M.data  → h_AGL_m
                raRadioAlt_M.valid → Valid_Status.h_AGL_Valid
                raRadioAlt_M.reset → Reset_Status.h_AGL_Reset
                       → 根层NAV_Root
```

### 1. 原始输入：两台设备，每台带数值和有效性

根输入`VE25SP/SpIn_RA`是一个`SpIn_Ra`总线；该总线唯一直接成员`L164_Radio_Altitude`是长度2的`SpData_Single`数组。每个数组元素的`data`和`valid`都是标量。因此这里的“2”表示两份通道记录，不能把顶层端口维度1、成员数组长度2和后续预处理数组长度3混为一谈。

来源：SP_Step_New.sldd的`DD.ENTRY[19](SpIn_Ra)`、`DD.ENTRY[3](SpData_Single)`；VE25SP根系统SID5。

### 2. RAMsgsPreprocess：对接统一三路接口并计算状态

实际连接是：

1. Bus Selector SID5提取两路`L164_Radio_Altitude`。
2. Constant SID17输出`0, Bus: SpData_Single`，作为`data=0、valid=false`的第三份占位。
3. Vector Concatenate SID18拼成`[RA1, RA2, 无效占位]`。
4. Calc_SignalState1 SID16调用`SignalProcessingLib/Calc_SignalState`。
5. 输出SID20为`raNormalInfos[3]`，元素类型`Bus_SingleSignalNormalInfo`。

库内有RA1–RA2、RA2–占位、占位–RA1三组比较。每组须双方valid均为true才能成为有效比较；因此后两组始终不可用，不会因真实高度与占位0不同而产生有效的“与零失配”。两真实通道都有效时，数值一致的判据明确是`abs(data1-data2) <= 3`。只有两份实际测量，两者不一致也不能仅凭这一比较识别哪台设备错误。

当前可达的Calc_SignalState实现将输入data/valid直接送往最终组包，同时计算比较状态；**没有在这条调用链中增加单路超时、范围、异常值监控或数值保持，也没有用失配/禁用结果改写输出valid。** 参数文件中存在RadioAlt_timeout/max/min等变量，不等于本实例已经使用它们。

实际五个参数绑定如下；保存字典与定义脚本一致：

| 实例参数 | 保存值 | 本次可确认的用途 |
|---|---:|---|
| SpCPT.RadioAlt_maxerror | single(3) | 两路绝对差的一致性界限 |
| SpCPT.RadioAlt_misscmp | int32(10) | 传入比较器MISS计数判据；精确触发关系见待复核项 |
| SpCPT.RadioAlt_banned | int32(50) | 传入比较器BAN判据 |
| SpCPT.RadioAlt_synced | int32(2) | 传入比较器SYNC计数判据；精确触发关系见待复核项 |
| SpCPT.RadioAlt_banenable | int32(0) | 禁用许可关闭；结合false初值，当前isBanned保持false |

输出每份记录为：

```text
raNormalInfos(i)
  state
    lastTimestamp : uint32，当前固定0
    stateBits     : uint32
  data            : single，保留该路输入data
  valid           : boolean，保留该路输入valid
  isSynced        : boolean，来自比较状态
```

当前`SignalStateSet_New`编码为纯文本公式：

```text
stateBits = valid + 2*isSynced + 4*isMissCompare + 8*isBanned
```

For Each逐元素组包，再沿第1维收集为三份记录。第三份仍为无效占位。SP_State中旧七位Description与当前四位实现的差异已在Q033配套Bus文档标注，不能用旧Description覆盖本链实际编码。

### 3. Calc_VotedRA：只从前两路选择一个源

路径`Calc_VotedRA/SourceSelection/Subsystem`的Selector SID43/44只取第1、2份NormalInfo；Bus Selector SID21/22只提取它们的valid。有效赋权2，无效赋权1。

| RA1 valid | RA2 valid | OnGnd=true | OnGnd=false |
|---|---|---|---|
| true | false | 选RA1 | 选RA1 |
| false | true | 选RA2 | 选RA2 |
| true | true | 选RA1 | 上一拍为RA2则保留RA2，否则选RA1 |
| false | false | 选RA1，但输出无效 | 上一拍为RA2则保留RA2，否则选RA1；输出无效 |

`SourceSelection/Delay` SID14保存上一拍源号；初始源号来自SID13的`uint8(0)`，所以初始两路同级时选RA1。这里的保持对象是源号优先权，选中通道的高度仍逐拍更新。

`OnGnd`改变同级时的优先规则，没有接到此Delay的复位端口。它在`VE25SP/NavSpApp`内由`RmsPublicMsg.vtolState`与以下五个状态比较后取OR得到：`Unknown、Disarm、Arm、MtrCheck、SurfCheck`。不能把它解释成由RA高度计算出的落地判断。

另须区分：RAMsgsPreprocess没有OnGnd输入。其`Calc_SignalState/CmpMonitor`输入inAir在`system_2474.xml` SID2478被常量true接入，不能说这里使用OnGnd取反，或在落地时清除预处理禁用锁存。

**由此可直接推得：**即便两路高度差很大，只要其原始valid都保持true，本模块仍按上述地面/空中规则选源；isSynced、isMissCompare、isBanned均未进入本选源条件。

### 4. SrcSelect：数值、有效性和reset分别怎么来

`Calc_VotedRA/SourceSelection/output/calcData` SID70调用`SignalProcessingLib/SrcSelect`。记本拍源号为s：

```text
data  = raNormalInfos(s).data
valid = raNormalInfos(s).valid

q     = valid为true时取s，否则取0
reset = valid AND (q != 上一拍q)
```

data和valid直接来自所选记录；本模块没有在无效时另行锁存旧高度。若输入端已经带有设备上游保持的数据，那属于本次SP链路之外的行为，不能据此认定由当前SrcSelect完成。

| 事件 | 本模块reset |
|---|---|
| 第一次产生有效输出 | true |
| 有效源从RA1切到RA2，或反向 | true |
| 同一路失效后恢复有效 | true |
| 同一源持续有效 | false |
| 本拍输出无效 | false |

SID65将三者组成`tSingleData`，SID63写入`raData.raRadioAlt_M`。SID64还将前两路data/valid分别写到`ra1RadioAlt_M`、`ra2RadioAlt_M`；这两个原始通道记录的reset在此处没有重写，沿用输入总线已有值。

例：空中原先选RA1，两路有效；RA1变无效、RA2仍有效时切到RA2，输出RA2的当前高度，valid=true、reset=true。随后RA1恢复、两路重新同级，继续保留RA2，若RA2持续有效则reset=false。

### 5. 三条输出线分开看

| 输出线 | 内容与去向 | 读模型时要注意 |
|---|---|---|
| raData | BUS_NavData中的raRadioAlt_M及两路原始RA字段 | selected data/valid/reset是给后级使用的结果 |
| raState | BUS_NavDataState.raRadioAlt_M，含三路SignalState和voterState | VoterType明确为SourceSelect；公共结构仍保留第三占位的state |
| RaSrc | uint8，当前只能为1或2；送NavSpApp的NavLog | 源号不因输出无效变成0，不能只看源号判断结果有效 |

voterState.ChannelUsed是通道位掩码：源1为1，源2为2；公共映射还支持源3为4，但当前RA选源不产生源3。ChannelUsed也没有按valid另行清零。两路全无效时，必须读取输出valid判定，不能只看RaSrc或ChannelUsed。

根层`VE25SP/NavSpApp`的NavLog下游Goto5（SID368，tag=NavLog）保存为`Commented=on`。当前只确认RaSrc被组入NavSpApp内的NavLog，不将其描述成已经接到有效根输出的日志接口。

### 6. 高度如何到达最终控制律接口

RA处理是向已经包含ADAHRS结果的BUS_NavData补写RA字段：

1. `Calc_VotedRA`输出raData进入`Calc_VotedFTADC`。
2. Calc_VotedFTADC的Bus Assignment SID63以raData为基础，只写ftadcStaticPressure_kPa、ftadcTotalPressure_kPa、ftadcAttackOfAngle_Deg、ftadcSideslipAngle_Deg、ftadcCAS_mps。
3. 后续`VE25SP/NavSpApp/Subsystem`的Bus Assignment SID78只写ftadcVoterCAS_mps。
4. 因此RA高度字段保留到NavData，分别送根输出SpIn_Nav2Display和Calc_NAVMsg_ToClaw。
5. Calc_NAVMsg_ToClaw通过Bus Selector与Signal Copy直接映射：

| 输入字段 | 最终BUS_NAV_IN字段 |
|---|---|
| NavData.raRadioAlt_M.data | h_AGL_m |
| NavData.raRadioAlt_M.valid | Valid_Status.h_AGL_Valid |
| NavData.raRadioAlt_M.reset | Reset_Status.h_AGL_Reset |

以上三条映射未插入新的RA表决、数值换算或保持块。`h_AGL_m`是控制律接口中该高度的字段名；字典未填写的原始RA单位仍不能仅凭接口名字替代为已确认的上游ICD单位。

RA状态走另一路：`raVoter`的raState直接接NavSpApp/NavDataState，再接根层NavDataState，并未随高度一起经过FTADC的数据补写链。

### 7. 已确认事实与待复核边界

已确认：输入两路；第三路为无效占位；比较有效性门控；绝对差判据；data/valid直通；选源条件表；源号历史；reset事件；三条输出及最终字段映射。以上由保存模型的连接、类型和参数交叉核对。

仍须保留的两项实现细节：

- Comparator的`system_1241.xml` SID1267–1270未显式保存Operator，bddefaults也没有对应覆盖；其中SID1267输入是missCnt、MISS，SID1268输入是accoCnt、SYNC。[MathWorks Relational Operator文档](https://www.mathworks.com/help/simulink/slref/relationaloperator.html)给出的标准默认为`<=`。不能只凭MISS=10、SYNC=2的命名，写成“连续10拍必置失配、连续2拍必置同步”。本次未加载模型读取有效参数或执行逐拍验证，不直接认定其运行故障。
- SpCPT_define.m第626行注释写“RA - 20ms”，而RAMsgsPreprocess输出声明SampleTime=SpCPF.Ts，保存参数SpCPF.Ts=0.01。未经实际调度核验，本次不将计数阈值换算成毫秒。

这两项不会改变已经直接由连线确认的事实：当前选源只使用前两路valid，不使用上述比较计数结果。静态追踪不等于仿真、调度或系统有效性验证。

### 8. 来源锚点与复核记录

| primary来源 | 关键锚点 |
|---|---|
| [VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx) | 根系统SpIn_RA SID5、NavSpApp SID54、Model4 SID375；system_54.xml的Model SID46、raVoter SID45、Model1 SID71、OnGnd SID461–466、NavLog SID49；system_74.xml SID78 |
| [RAMsgsPreprocess.slx](D:/666/VE25/sp/04_Design/RAMsgsPreprocess/specification/RAMsgsPreprocess.slx) | system_root.xml SID5/17/18/16/20/21 |
| [Calc_VotedRA.slx](D:/666/VE25/sp/04_Design/Calc_VotedRA/specification/Calc_VotedRA.slx) | system_8.xml SID13/14；system_15.xml SID19–49；system_56.xml SID62–70；system_73.xml SID75–86 |
| [SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx) | Calc_SignalState system_2421/2551/2474/2452；CmpMonitor system_1326；Comparator system_1220/1241/1271；SrcSelect system_407 SID418/428/435/426/427/413/417 |
| [Calc_VotedFTADC.slx](D:/666/VE25/sp/04_Design/Calc_VotedFTADC/specification/Calc_VotedFTADC.slx) | system_root.xml SID62/63/64：RA作为总线基础，AssignedSignals仅FTADC字段 |
| [Calc_NAVMsg_ToClaw.slx](D:/666/VE25/sp/04_Design/Calc_NAVMsg_ToClaw/specification/Calc_NAVMsg_ToClaw.slx) | system_6.xml：data SID56→155→28；valid SID83→160→26；reset SID39→187→27 |
| [SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd) | data/chunk0.xml DD.ENTRY[3] SpData_Single、DD.ENTRY[19] SpIn_Ra |
| [SpCPT_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPT_define.m:638)及[SP_Parameters.sldd](D:/666/VE25/sp/04_Design/common/libs/sldd/SP_Parameters.sldd) | 脚本638–642；字典data/chunk0.xml DD.ENTRY[2] SpCPT.Value与DD.ENTRY[3] SpCPF.Value |

- 核验基线：sp HEAD=`a895a43a19209c076e679fcc2e2f8953e9f49bb4`；复用的静态提取与本轮实际来源哈希一致。
- VE25SP SHA-256：`20D8C7DE2A103D8FF298BCD5BB0C28F73AA6CCC2E27D43D0F5AB9952365F77E4`。
- RAMsgsPreprocess SHA-256：`6FDCBEAD97A9EB0023601C9D44E2A23EB9768DA8814910681943E5E826815213`。
- Calc_VotedRA SHA-256：`3C07C33A91C9B4920D527FC12DE87DE20E76502F8ECA540F757F2C92DD099197`。
- SignalProcessingLib SHA-256：`0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490`。
- SP_Parameters SHA-256：`3B89CDE7F5987DD0CDBCC440A1A4EC91CE7BE8CFE84F0761296C656FA10F38AB`。
- 对省略的标准块默认参数采用官方参考交叉检查：[Constant](https://www.mathworks.com/help/simulink/slref/constant.html)、[Switch](https://www.mathworks.com/help/simulink/slref/switch.html)、[Logical Operator](https://www.mathworks.com/help/simulink/slref/logicaloperator.html)、[Delay](https://www.mathworks.com/help/simulink/slref/delay.html)；Comparator关系运算符另列待复核，不凭块名称补齐。
- 本条承接Q019的两路RA/三份预处理结构，补全选源、reset、状态与最终消费者。仅追加Q034及索引；未改模型、字典、参数、设计说明或上一轮Bus文档，未运行MATLAB。
- sp保留原有两项project XML修改，无新增工程中间文件。
- closeout_outcome=completed；verification_status=passed（主链静态核验，比较计数运行语义另列待复核）；Delta-K route=project-only。

## Q035：GCS含义与Model3（Calc_GCSData）的作用

### 结论与责任边界

- 日期：2026-09-23，Asia/Shanghai；修订 R001。
- 用户请求：“GCS指的是什么，Model3 (Calc_GCSData)这模块做什么用”。
- **GCS的通用英文全称为Ground Control Station，中文地面控制站／地面站。** 当前项目需求正文也明确使用“地面站信号”和“远程控制信号”的表述；本轮限定范围未找到工程内英文全称原句，英文展开由官方通用来源佐证。
- `VE25SP/Model3`是Model Reference块的实例名，实际引用`Calc_GCSData.slx`。其责任是：**依据主控FCC和报文有效性，从Curr/Left/Right三份输入中分别选择遥控操纵组、远程面板组，并将选中数据封装为带data/valid/reset的BUS_GcsData。**
- 当前模块的实际处理为源选择与组包；没有在本模块定位到通信收包、协议字节解码、超时监控、三路数值比较、平均、滤波或数值保持实现。上游通信和有效性产生过程不在本次模块边界内。

### 输入、处理与输出链

```text
Rms2PublicMsg.fccId + activeFccId
               ↓
将主控FCC映射为相对输入位置：Curr / Left / Right
               ↓
SpIn_Gcs_Curr ─┐
SpIn_Gcs_Left ─┼─→ 分出RemotePilotCtrl与RemoteFMCP
SpIn_Gcs_Right─┘           ↓
                 两组分别按有效性选源
                          ↓
              字段拆出并封装data/valid/reset
                          ↓
                 GcsData : BUS_GcsData
                          ↓
      Calc_PilotIn_ToClaw/handleGCS等后续消费者
```

四个实际输入为：

| 输入 | 类型 | 在模块中的作用 |
|---|---|---|
| Rms2PublicMsg | BUS_DRMS2SP_MSG | 提供fccId、activeFccId，确定优先输入位置 |
| SpIn_Gcs_Curr | SpIn_GcsMsg | Curr报文 |
| SpIn_Gcs_Left | SpIn_GcsMsg | Left报文 |
| SpIn_Gcs_Right | SpIn_GcsMsg | Right报文 |

Curr/Left/Right按模型的FCC相对位置参与映射。模型并未据此声明存在三个独立地面站；其具体物理接收和跨机转发路径需要继续追踪上游。

每份`SpIn_GcsMsg`有两组标量Bus：

| 报文组 | 原始字段数 | 主要内容 | 选源有效标志 |
|---|---:|---|---|
| RemotePilotCtrl | 26 | RC_SW0–9、遥控器类型、模式/控制按键、操纵杆状态字、四轴操纵值 | PilotCtrlMsgValid |
| RemoteFMCP | 15 | 10项按钮、4项目标值 | FmcpMsgValid |

RemoteFMCP的项目正式英文展开仍待确认，本条以其字段功能称为“远程面板组”。四项目标值为高度、垂向速度、航向、空速目标。

需求原文对字段业务的说明包括：`Key_AP`为AP断开开关，`AP_Pushbutton`为自动驾驶激活开关；`ValidBit`对应操纵杆状态字；`chAileron/chElevator/chThrottle/chRudder`对应滚转、俯仰、油门、偏航轴操纵值。不要将四轴操纵值仅凭名称解释为实际舵面角度。当前字典各字段没有填写单位、范围和Description，也未用Enum定义uint8开关编码，因此不能只凭字典给出码值含义。

### 优先输入位置如何产生

`Calc_GCSData/Subsystem`读取fccId和activeFccId，输出内部`ActiveFCC`：

- 1：Curr；
- 2：Left；
- 3：Right；
- 0：未识别出对应位置。

保存模型的映射为：

| fccId原始值 | activeFccId=PFCC1 | PFCC2 | PFCC3 | 其他 |
|---|---|---|---|---|
| 0 | Curr/1 | Right/3 | Left/2 | 0 |
| 1 | Left/2 | Curr/1 | Right/3 | 0 |
| 2 | Right/3 | Left/2 | Curr/1 | 0 |
| 其他 | 0 | 0 | 0 | 0 |

左列保留模型中实际比较数值，右列保留模型使用的枚举成员名。该映射使不同本机位置下的相对Curr/Left/Right能指向同一个主控FCC。

### 两组独立选源

- RemotePilotCtrl由`Subsystem4`处理。
- RemoteFMCP由`Subsystem1`处理。
- 两组使用同一套优先位置，各自检查本组MsgValid，因此可以选择不同来源。

每组规则：

| 条件，按先后判断 | 动作 |
|---|---|
| ActiveFCC对应输入的本组MsgValid=true | 使用该优先输入 |
| 优先输入不可用，Curr有效 | 使用Curr |
| 优先输入不可用、Curr无效，Left有效 | 使用Left |
| 上述均不可用，Right有效 | 使用Right |
| 三路本组均无效 | 输出valid=false，ChannelUsed=0 |

例如主控对应Left：若Left的PilotCtrlMsgValid=true，则操纵组选Left；若其FmcpMsgValid=false而Curr的FmcpMsgValid=true，则面板组选Curr。这是两组独立有效性判定的直接效果。

当前逻辑逐拍重新按上述规则选择；优先通道恢复有效后会重新获得优先权。没有类似RA空中同级时保留原源号的选择规则。

内部`ChannelUsed`采用位掩码：Curr=1、Left=2、Right=4、无有效通道=0。因此`ActiveFCC`的Right=3与`ChannelUsed`的Right=4不能互换。

三路全无效时，数据开关仍选择Curr报文的字段，并将输出valid置false；没有清零数值，也没有保持上一拍有效数据。后级必须结合valid使用。

### 输出字段与reset的不同处理

唯一输出为`GcsData : BUS_GcsData`，包含39个业务字段。原始报文中的两个MsgValid用于生成各组输出有效性，其余25+14个字段分别封装：

| 分组 | 业务字段数 | data | valid | reset |
|---|---:|---|---|---|
| RemotePilotCtrl | 25 | 所选Pilot报文字段 | 共用本组ChannelUsed>0 | 共用本组源变化通知 |
| RemoteFMCP | 14 | 所选FMCP报文字段 | 共用本组ChannelUsed>0 | 每个字段固定false |

离散字段采用`tU8Data`，操纵量与目标值采用`tSingleData`。二者均含标量data、valid、reset；data类型分别为uint8和single。

Pilot组的reset：

```text
reset = 本拍ChannelUsed != 上一拍ChannelUsed
```

一拍Delay保存上一拍ChannelUsed，初值为0；不对reset再加valid门控。因此：

- 初次取得有效通道：reset=true；
- 切到另一通道：reset=true；
- 从有效变为三路全无效：reset=true；
- 从全无效恢复到有效：reset=true；
- 同一选择状态持续，包括一直全无效：reset=false。

该规则与Q034中RA的`valid AND 源变化`规则不同，不能套用。

FMCP内部虽也计算源变化reset，但根层SID226将其接到Terminator；最终14个FMCP字段的reset由SID328–341常量false赋值。

两组选源器内部都生成`VoterType=Enum_VoterType.SourceSelect`和ChannelUsed，但两路voterState分别在根层SID521/524终止，未对外组入GcsData。

**ValidBit在Calc_GCSData中只作为普通data字段封装，没有在本模块展开其位含义，也没有用它给其余操纵字段生成valid。** 本模块的操纵字段valid来自整组PilotCtrlMsgValid的选源结果。

### 下游用途

VE25SP根层Model3的GcsData通过Goto7（tag=GcsData）：

1. 经From5进入`Model5 (Calc_PilotIn_ToClaw)`的第1输入。在其`handleGCS`内，四轴、模式按键等继续转换和组织为`BUS_PILOT_IN`，送出RC路径及后续驾驶员输入选择逻辑；根层`Cockpit_In`来自该后级模块。不能将Calc_GCSData本身解释为已经完成全部控制律操纵输入计算。
2. 根层From32→Bus Selector2另行提取`KeyBackupFccAct.data/valid`，进入备用FCC激活相关组合逻辑；该逻辑还结合其他输入，本条不将其效果全部归于GCS。

### 来源与核验

- 当前模型：[Calc_GCSData.slx](D:/666/VE25/sp/04_Design/Calc_GCSData/specification/Calc_GCSData.slx)，SHA-256=`D92EB5C7C942656ED04C6AF72D11EEB821AA3316256DB308E27DBF1E9F0E8BCF`；已回读全部8个系统。
  - 主控映射：system_6.xml SID8/14/61；system_19/33/47.xml及默认system_15.xml。
  - Pilot选源：system_63.xml，SID67–76/85/105/112–119。
  - FMCP选源：system_155.xml，SID160–172/181/204–212。
  - 最终组包：system_root.xml SID469（39输入）→SID519；FMCP reset终止SID226、false常量SID328–341；状态终止SID521/524。
- 上下游：[VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)根层SID370 Model3、SID371–374输入、SID379 Goto7、SID380 From5→SID376 Model5、SID447→448；[Calc_PilotIn_ToClaw.slx](D:/666/VE25/sp/04_Design/Calc_PilotIn_ToClaw/specification/Calc_PilotIn_ToClaw.slx)根层SID4→108 handleGCS，handleGCS输出3到根输出RC。
- 字典：[SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)，data/chunk0.xml DD.ENTRY[43] SpIn_GcsMsg、[40] RemotePilotCtrl、[39] RemoteFMCP；[SP_InteriorBus.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd) DD.ENTRY[7] BUS_GcsData；[SpMdlPrivate_DataType.sldd](D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd) DD.ENTRY[2] tU8Data、[9] tSingleData。
- 项目业务术语：[需求Excel](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)，工作表“信号处理软件高级需求-SP_SW_HLR”：A275/C275为SP_SW_HLR-761；A277/C277/G277为SP_SW_HLR-424；业务字段说明主要在287–324行。SHA-256=`E3F28F03B948613CCAB451EE469B9FD5A87313393398BECD541BDA3126406BEB`。
- 需求表保留旧GcsSpMdl/SP_Step.sldd等路径，本条仅用其佐证地面站及字段业务语义；当前实现、有效性和reset结论以本轮Calc_GCSData为准，未将旧超时流程套入当前模型，也未宣称已完成需求符合性验证。
- 通用缩写来源：[MathWorks：Develop and Verify a Quadcopter System Architecture](https://www.mathworks.com/help/simulink/slref/develop-verify-quadcopter-system-architecture.html)，GroundControlStation段明确写Ground Control Station (GCS)，仅作为英文全称来源。
- 相关结构详表：[Bus信号结构说明：GCS输入](D:/666/VE25/Doc/VE25SP_Bus信号结构说明.md:814)。
- 当前主链模型与复用静态提取哈希一致；只追加Q035和索引，未修改模型、字典、代码、需求、设计说明或Bus文档，未运行MATLAB。sp保留既有两项project XML修改，无新增工程中间文件。
- closeout_outcome=completed；verification_status=passed（静态分析与记录核验）；Delta-K route=project-only。

## Q036：在模型功能介绍中解释信号术语

- 日期：2026-09-23；修订R001。用户指出截图章节堆叠缩写和接口名、难以理解，明确要求在该章节的功能介绍中解释术语。
- 截图定位为[SP工作区模型主要功能梳理第3章](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md:62)。本轮按用户新授权直接修改该文件；未修改另一任务的设计说明文档。
- 实际修改：增加通用信号命名/状态/选源/物理量解释；将17个模型按导航、设备、操纵、系统与输出分组，每行说明处理对象、功能和输出含义；为9个根输出增加中文释义；同步文首修订日期和第7.2节记录。其他章节正文经反向哈希核验保持原样。
- 解释覆盖ADAHRS/Adahru、RA、CAS、FTADC、MCU/SCU、MPIO、GCS、VDT/GPIO/CBIT、FCC、CCDL、FMS、Claw、RMS/DRMS及NormalInfos、OnGnd、data/valid/reset、源权重、余度等；对FT、MPIO、VDT、RemoteFMCP等缺少项目正式全称的名称，使用已确认的功能释义并保留未确认状态；保留用户已确认D=Device。
- 准确性复核：NormalInfos允许存在无效记录；VdtCBIT按实际指定VDT输入有效性检查解释；LimitDispatch为整数编码；输入SysReady在控制律系统报文中的实际字段写为SysReady_SYSINFO_b。未将术语名扩展成未经核实的完整监控、通信或硬件自检功能。
- 当前18模型来源哈希与本轮复用静态提取一致。项目术语依据当前模型、SP_Step_New/SP_InteriorBus字典及已有需求术语锚点；航空通用概念的FAA/NASA出处已放在第三章相关说明旁。旧需求模型路径不作为当前算法依据。
- 文档核验：17模型、9根输出、21本地链接、6张表及UTF-8通过；源文件与修订范围检查通过。证据：[validation.json](D:/666/VE25/.runtime/sp_learning_qa/chapter3_terms_20260923/validation.json)。
- 修订后SHA-256：5680926711279716757A1EF8B6A6FED952EF55EFBB3122634839E3245F88CB3E。
- 未运行MATLAB、编译或仿真；sp保留原有两项project XML修改，无新增工程中间文件。
- closeout_outcome=completed；verification_status=passed（文档与静态来源核验）；Delta-K route=project-only。

## Q037：RemoteFMCP与FMCP全称

- 日期：2026-09-23；修订R001。
- FMCP在当前航空自动飞行语境中采用的通用展开为**Flight Mode Control Panel，飞行模式控制面板**。行业全称已有原始公开论文明确支持。
- 结合当前接口，RemoteFMCP可理解为**远程飞行模式控制面板输入数据／报文**：用于传递自动飞行模式按钮及高度、垂直速度、航向、空速目标设定。Remote表示远程，当前结构归属地面站SpIn_GcsMsg输入。
- 证据层次：英文/中文对应由行业来源确认；将它对应到VE25当前RemoteFMCP，是结合实际字段与GCS输入位置作出的有依据解释。尚未在本次限定读取范围内取得项目词表或ICD中的正式全称定义，不据此确认物理面板型号、厂家或布局。

### 字典与当前模型证据

[SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)，data/chunk0.xml DD.ENTRY[39](RemoteFMCP)，全部Dimensions=1、Description为空：

| 分组 | 字段 |
|---|---|
| 10个uint8按钮 | FD_Pushbutton、AP_Pushbutton、AT_Pushbutton、FMS_Spd_Pushbutton、SYC_Pushbutton、HDG_Pushbutton、LNAV_Pushbutton、VNAV_Pushbutton、VS_Pushbutton、ALT_Pushbutton |
| 4个single目标值 | Altitude_Target、Vertical_Speed_Target、Heading_Target、Airspeed_Target |
| 1个boolean有效标志 | FmcpMsgValid |

同字典DD.ENTRY[43](SpIn_GcsMsg)将RemoteFMCP与RemotePilotCtrl列为两个成员。[Calc_GCSData.slx](D:/666/VE25/sp/04_Design/Calc_GCSData/specification/Calc_GCSData.slx)根SID137–139分别取出两组数据；system_155.xml SID156–158接收Current/Left/Right FMCP，SID164–166读取有效性。14个业务字段与1个有效标志合计15项，和Q035输出封装中的14项业务字段并不矛盾。

### 全称来源及本地检索范围

- [王艳艳、张景皓：基于SCADE软件的驾驶舱控制板仿真建模方法与实践](https://myfj.ijournals.cn/myfjsjyyj/article/html/20240217)，作者单位上海飞机设计研究院，DOI 10.19416/j.cnki.1674-9804.2024.02.017。中文摘要明确“飞行模式控制板（FMCP）”，英文摘要对应flight mode control panel (FMCP)。仅用于通用术语，不将论文中的具体飞机面板实现迁入VE25。
- [项目需求Excel](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)：A10/C10的缩略语表未列FMCP；整份sharedStrings.xml未命中所查FMCP和两个候选英文全称/中文面板名。A311–A324/C311–C324列出了地面站转发按钮和目标值，但未提供面板全称。本结论限于上述文件/内容，不代表全工程没有定义。
- 当前字典、Calc_GCSData及需求Excel哈希与本会话已核实来源一致。
- 已同步补充[功能梳理第3章](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md:109)的FMCP说明、行业出处及修订记录；反向哈希核验确认只改获准目标。Q003/Q035/Q036原有“项目正式全称待确认”仍成立，本条补充的是行业全称及项目语境解释。
- 未修改工程，未运行MATLAB；sp保留原有两项project XML修改，无新增中间文件。
- closeout_outcome=completed；verification_status=passed（术语来源和静态接口）；Delta-K route=project-only。

## Q038：VDT_Stick与Dis_Key六个输入的现实意义

- 日期：2026-09-23；修订R001。
- 用户要求将上一条FMCP解释完整写入功能梳理文档，并解释截图中的六个根输入。本轮在[功能梳理第3章](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)新增RemoteFMCP和VDT_Stick/Dis_Key两个小节，补齐FMCP全称、15个字段的含义、处理位置，以及操纵输入的结构和用途。
- 截图15～20为VE25SP的Inport序号，依次是`VDT_Stick_Curr/Left/Right`、`Dis_Key_Curr/Left/Right`。前组三个端口各传一份`SpIn_Sitck`，后组三个端口各传一份`SpIn_Key`；均以一份Bus承载多项业务数据。
- 每个`SpIn_Sitck`包含`CSSU_X、CSSU_Y、RSSU_X、RSSU_Y、LSSU_X、LSSU_Y`六项杆位连续量，每项`SpData_Single`含标量`data:single`、`valid:boolean`。
- 每个`SpIn_Key`包含十四项离散输入：`Key_AirGnd、Key_ELFS、Key_PfcModeSW、Key_McProtect、Key_Backup、Key_TRC、Key_Ap_L、Key_Ap_R、Key_RSSU_BackupActive、Key_CSSU_BackupActive、Key_SSU_BackupUp、Key_SSU_BackupDown、Key_SSU_BackupLeft、Key_SSU_BackupRight`。每项`SpData_Enum`含标量`data:uint32`、`valid:boolean`。名称中的Enum并不提供码表；valid=true仅表示数据有效，不能等同于按键按下。
- `Curr/Left/Right`是输入通道标签。三个VDT端口都包含同样六个字段，因此Left/Right不能直接解释成左右物理操纵杆，也不能单凭这段连线绑定固定FCC编号、板卡或线束。
- `Calc_PilotInData`把相同业务量的三路记录组合后比较、表决：例如`Curr.CSSU_X / Left.CSSU_X / Right.CSSU_X`形成第一项VDT的三路输入；最终输出`PilotInData.VDT[6]`及`PilotInData.GPIO[14]`。
- 后级`Calc_PilotIn_ToClaw/handlePilotIn`映射：CSSU_X/Y→VDT[1/2]→CalcCSX/Y→CSX_PltIn_deg/CSY_PltIn_deg；RSSU_X/Y→VDT[3/4]→CalcFOSSX/Y→SSX_FO_deg/SSY_FO_deg；LSSU_X/Y→VDT[5/6]→CalcCPSSX/Y→SSX_CP_deg/SSY_CP_deg。
- 证据边界：输入字典Description、单位和范围为空；CSSU/RSSU/LSSU、VDT的正式全称、物理安装位置、轴向和正负约定未取得直接定义。BUS_PILOT_IN上述六个输出的Description和DocUnits也为空，不据输出名称推定输入单位。十四键中文解释按名称保守释读，具体动作及编码需接口或消费者证据。
- 需求Excel中的独立CSSU/RSSU/LSSU/SSU/VDT定义及十四键名未找到；VDT子串属于RVDT舵机反馈，不能移用。旧需求SP_SW_HLR-503…510（A347:C354，旧SpOutMdl路径）提及主驾/副驾SSX/SSY，但不足以确认当前输入组安装映射。

### 来源与静态核验

- [VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)：system_root.xml中SID6/7/8/10/11/12，Port15～20，连接SID388 Model7的输入1～6。
- [SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)：data/chunk0.xml，ENTRY20 SpIn_Sitck、ENTRY26 SpIn_Key、ENTRY3 SpData_Single、ENTRY2 SpData_Enum；ENTRY29 BUS_PILOT_IN的元素1/3/5/7/9/11。SHA-256=10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC。
- [Calc_PilotInData.slx](D:/666/VE25/sp/04_Design/Calc_PilotInData/specification/Calc_PilotInData.slx)：system_1524.xml，SID58/70/82、214/217/218、794/1216。SHA-256=7DEB3921F4C73ACB63650353A6498AA629BA674A366511183B9F5F10D5BD89A2。
- [Calc_PilotIn_ToClaw.slx](D:/666/VE25/sp/04_Design/Calc_PilotIn_ToClaw/specification/Calc_PilotIn_ToClaw.slx)：system_552.xml，SID692/693、696/697、698/699。SHA-256=3CE1556CC2EBED6260936810D9EB2FCB82DFF0BB0608205C79CAA6CD51623462。
- 模型与字典的当前哈希和提取快照一致；文档采用基线哈希及唯一锚点修改，并以反向还原哈希检查无关正文保持原样。仅编辑功能梳理和本问答记录；工程模型、字典、需求均未修改，未运行MATLAB。
- closeout_outcome=completed；verification_status=passed（静态来源与文档核验）；Delta-K route=project-only。

## Q039：MPIO具体含义复核与历史接口名称集中整理

- 日期：2026-09-23；修订R001。用户要求重新判断MPIO含义，并将此前问过的接口名称统统写入功能梳理文档。
- 已直接修改[功能梳理第3章](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)：重写Calc_MPIOData行，新增MPIO专题说明和3.5节名称索引；原有FMCP与六个操纵输入详表保留。索引覆盖Q001～Q038中的根输入、内部Bus/结果、设备字段、地面站操纵组、L标签、状态/类型、通用片段与单位、文档缩写和处理块名称。
- MPIO目前最有证据的定位是**航电—飞控接口相关的数据来源/消息类别**。并非已确定其硬件板卡身份，也不是已证明它就是IDU或FMS。当前用途：输入综合飞机状态及飞管目标/请求，经Calc_MPIOData整理并处理必要的有效性和异常监控。
- 新增primary锚点：[需求Excel](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)，工作表“信号处理软件高级需求-SP_SW_HLR”，A343=SP_SW_HLR-668，C343：“MPIO消息参见《VE-IC10-001-01航电飞控CAN报文定义》。”A240/C240的381、A244/C244的383、A247/C247的387、A255/C255的395分别支持航电信号处理、IDU来源及其业务内容。所引CAN接口原文件未在本轮文件名检索中找到。
- 这些需求保留旧MpioSpMdl路径，仅用于接口背景与术语；未把旧UTC/更新时间/超时逻辑套入当前Calc_MPIOData。
- 当前[Calc_MPIOData.slx](D:/666/VE25/sp/04_Design/Calc_MPIOData/specification/Calc_MPIOData.slx)：system_root.xml SID4/5输入及字段提取，SID73/76/80处理可用功率、重心、总重，SID121/122/123判定各自有效码是否等于uint8(1)，SID120组包、SID113输出。SHA-256=A5B051AE97E92967CCF9E101D137D566B78DC55C57FC90B78910BA7F841F2FE6。
- 字典[SP_Step_New](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd) ENTRY47的SpIn_Mpio共31个标量成员；[SP_InteriorBus](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd) ENTRY13的BUS_MpioData声明28项，当前SID120赋27项、不含SelectedFCC；成员Description为空，未给出MPIO全称。
- 英文候选：Multi-Purpose Input/Output（多用途输入/输出）有[NXP原厂手册](https://www.nxp.com/docs/en/user-guide/MC56F8323EVMUM.pdf#page=11)支持，但原文讲芯片端口，不能据此确认为VE25命名。项目文本、所查需求与字典没有取得MPIO正式展开。
- 整理时保留订正：DRMS的D=Device由用户确认，RMS正式展开仍待确认；FMCP行业全称已确认而项目正式命名待ICD；本地SRS=Software Requirements Standards，STS两个展开只是候选；独立ADA含义未确认；tSingleData中的Single为精度而非路数；Curr/Left/Right不能固定映射为左右操纵杆。
- 新增完整30项L前缀表并保留AdaComb_Altitude_Rate；未将L307写进当前SpIn_Adahrus。状态stateBits当前实例的低四位布局与字典描述差异继续保留，不宣称已解决。MCU输入无温度字段，输出类型有Temp/MotorTemp不等于实际已取得温度反馈。
- 来源复核：复用提取中的57个模型/字典文件原始SHA-256全部一致；需求Excel为E3F28F03B948613CCAB451EE469B9FD5A87313393398BECD541BDA3126406BEB，SRS模板为FD2B706B05079D010F5D5CD3FD6E1DEA8F84E0E71BE44D0B0321CB76CEBCE094。历史术语的一手来源与原问题边界仍保留在Q001～Q038。
- 仅编辑功能梳理和本问答记录，以基线哈希、唯一锚点、反向还原检查保护其他正文；校验记录为[Q039文档核验](D:/666/VE25/.runtime/sp_learning_qa/q039_validation.json)。未修改工程、未运行MATLAB，sp既有两项project XML改动保留，无新增工程中间文件。
- closeout_outcome=completed；verification_status=passed（静态来源与文档核验）；Delta-K route=project-only。

## Q040：Curr/Left/Right的分路含义、映射及设计原因追溯

- 日期：2026-09-24；修订R001。用户追问地面站与系统分路为何使用Curr/Left/Right，以及特殊原因能否追溯。
- 已补充[功能梳理](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)的3.1词条和3.5.1专题；明确实现事实、设计效果推导、尚缺的原始设计理由。
- 已确认：GCS按本机视角将绝对FCC身份转换为相对输入位置；Calc_SysInfo直接接收外部分好的Curr/Left/Right三路，不在本模块内做FCC编号转换。
- Calc_GCSData的两条直接注释：system_63.xml Annotation SID131、system_155.xml Annotation SID217，原文“当前、左、右分别是1 2 3”及“顺序不能变”。
- GCS映射（列顺序Curr/Left/Right）：fccId=0对应PFCC1/PFCC3/PFCC2；fccId=1对应PFCC2/PFCC1/PFCC3；fccId=2对应PFCC3/PFCC2/PFCC1。其他未匹配值为0。来源system_6.xml SID8/14/61、system_19/33/47.xml的枚举比较及选择连线。ActiveFCC的位置号1/2/3与ChannelUsed位掩码1/2/4不同。
- 例如activeFccId=PFCC2，在本机0/1/2的视角分别为Right/Curr/Left；这是优先位置映射，最终选源仍受各组有效性和回退规则约束。
- 顶层系统状态连接：VE25SP根SID16/17/18（端口6/7/8）分别连接Calc_SysInfo SID363的输入5/3/4；子模型对应SpIn_CCDL_Curr/Left/Right，SID113/111/112。
- 三路SpIn_CcdlMsg均含Valid:boolean、SysReady:uint8、LimitDispatch:uint8；SP_Step_New的ENTRY23。Calc_SysInfo/system_115.xml各路Valid门控SysReady，限制码无效时输出5；保留六个Current/Left/RightSysReady和LimitDispatchInfo字段。根SID179/181/182另将本机健康输入FccHealth送到CurrentFccReady。
- Calc_CcdlMsg_Out/system_4.xml消费三路状态并形成CCDL输出；模型没有完整画出通信硬件、拓扑或输出返回输入的外部路径。
- 可以从结构推导出复用同一业务模型、区分来源身份、统一端口顺序等作用；尚未找到选择Left/Right名称的正式历史决策或强制标准。循环编号关系不证明物理环网，也不证明三台地面站或飞机左/右安装位置。
- 需求核查：[高级需求表](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)，工作表“信号处理软件高级需求-SP_SW_HLR”，424（A277/C277）写一路地面站数据/有效性/更新时间；434（A286/C286）写获取、监控、汇总；409（A265/C265）、411（A267/C267）、760（A269/C269）涉及平台就绪/系统状态。未查到本/左/右映射或命名动机条款；GCS下游追溯为旧GcsSpMdl，不套作当前实现依据。未以已停用SpMdl架构支线作当前架构证明。
- 有限Git检查：对两个准确模型路径分别读取最多8条，实际各3条。Calc_GCSData为ef92c182（修改跟DRMS的接口总线定义）、cb2e4da6（模型重构）、7fab6ff1（AC102 SP重构）；Calc_SysInfo为4db350b6（更新模型）、7fab6ff1、35332d40（更新AC102 SP重构模型）。提交说明无命名理由，未扩大历史检索。
- 尚存疑问：Calc_GCSData/system_root.xml Annotation SID154：“问题：Rms2PublicMsg里面的CHA指的是FCM，为啥会跟GCS通道有关？”此为保留的疑问注释，不是已完成确认的接口定义。端到端GCS来源/FCM通道关系仍需适用整机架构或接口资料。
- 当前Calc_GCSData、Calc_SysInfo、Calc_CcdlMsg_Out、VE25SP及相关字典已静态核对；57份复用提取来源哈希均未变化。未运行MATLAB、未修改工程，既有两项project XML改动保留。
- 文档采用基线哈希、唯一锚点、反向哈希和回读校验；仅修改功能梳理与本记录。证据：[Q040核验](D:/666/VE25/.runtime/sp_learning_qa/q040_validation.json)。
- closeout_outcome=completed；verification_status=passed（静态来源与文档核验）；Delta-K route=project-only。

## Q041：同源的地面站和遥控操纵为何有三路输入、是否比较表决

- 日期：2026-09-24；修订R001。用户追问同源地面站/遥控器为何分三路、比较表决能解决什么问题。
- 已在[功能梳理](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)3.5.1新增“同源操纵信息与三路输入处理”。本次区分外部操纵源、采集/通信通道和SP输入记录，不用端口数量推定三台地面站、三台遥控器或三个独立传感器。
- 核心订正：当前GCS链采用有效性与优先级选源，没有三路业务数值比较；VDT/Dis_Key是另一条带比较和表决的驾驶员输入链。模型中的Voter名称不必然表示多数投票或求中值。
- `RemotePilotCtrl`与`RemoteFMCP`是`SpIn_GcsMsg`的两个成员，前者包含四轴遥控操纵及开关；两组分别以`PilotCtrlMsgValid`和`FmcpMsgValid`独立选源。主控FCC对应输入有效则优先使用，否则按Curr→Left→Right寻找本组有效输入；全无效则输出valid=false。内部记录`Enum_VoterType.SourceSelect`。
- 因此，某一路异常被上游标成无效时可切换到另一有效路；错误数据若仍被标成有效，当前GCS模块没有通过数值比较识别它。两个MsgValid在SP上游如何产生，当前来源未闭合。
- `Calc_PilotIn_ToClaw`的`RC`输出来自`GcsData→handleGCS`；`PilotIn`输出来自`PilotInData→handlePilotIn`。不能将VDT/GPIO比较器当成GCS遥控报文的处理步骤。

### VDT/GPIO比较链的实际条件

- `Calc_PilotInData`根SID1524整理三路输入，经SID1253 `SignalComparotor1`到SID1343 `Voter`；整理后的数据另直接进入Voter。
- 六项VDT杆位：`system_1271.xml`的For Each逐项处理，SID1281调用`SignalProcessingLib/CmpMonitor`；后接`system_1364.xml`的SID1428 Voter。
- 十四项GPIO按键：`system_1257.xml`的SID1267调用同一监控器；后接`system_1431.xml`的SID1495 Voter。比较器接各路data/valid，Voter接data/valid/isSynced，确认是已连接的处理链。
- 公共库`system_1220.xml`的SID1227合成配对有效性，SID1228/1229检查MISS/SYNC阈值，结合SID1321和If1232选择正常比较或复位分支。不能称作所有输入情况下都进行数值比较。
- [SpCPT_define.m](D:/666/VE25/sp/04_Design/common/libs/parameters/SpCPT_define.m:722)第722～727、744～749行的静态声明：VDT为maxerror=0.5，GPIO为maxerror=0；两组misscmp=5、synced=2、banenable=0。banenable是禁用许可，库`system_1326.xml`的SID1377→1375单独判断；值0不等于关闭比较。
- 两处监控实例的inAir均接常量true（SID1282/1268），此处未接实时空地信号。以上是静态绑定和脚本声明，未运行MATLAB解析实际工作空间参数或执行算法。

### 同源与独立性边界

- 同一报文经不同通道接收、缓存、转发或处理，可能形成不一致的记录；同一操纵动作也可能经多路传感器/采集电路形成输入。以上为设计可能性，本次没有取得VE25硬件接线、接收机数量或通道独立性的直接证明。
- 如三份数据为0.2、0.2、0.8，数值比较可以暴露分歧；是否判失配及如何表决仍由阈值、持续时间、有效性和算法决定，单凭分歧不能直接定位物理故障。
- 如源头已经给出错误的0.8，随后三路只是复制成0.8、0.8、0.8，比较结果仍一致，不能识别这个共同错误。单纯复制三份不会创造三个独立信息源。
- 六个VDT/Dis_Key输入只有data/valid，无原始采样时间戳；端口均声明SampleTime=SpCPF.Ts，也不能证明三路物理采样同一时刻。这里isSynced由数值一致性监控产生，不能据此宣称完成了时间戳对齐。
- 当前可以确认处理结构与限制；为什么整机选择这些物理路径、各路径独立到什么程度，仍须以适用硬件架构和ICD确认。不得把三路结构直接宣传成消除源头共同错误。

### 来源锚点与核验边界

- [Calc_GCSData.slx](D:/666/VE25/sp/04_Design/Calc_GCSData/specification/Calc_GCSData.slx)：system_63.xml读取PilotCtrlMsgValid的SID136/152/153；system_155.xml读取FmcpMsgValid的SID164/165/166；SID84/180写入SourceSelect。其比较块用于通道编号、掩码与选源变化判断，未比较三路业务值。
- [VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)：system_root.xml中SID371/372/373→370→GcsData→376输入1；SID6/7/8/10/11/12→388→PilotInData→376输入2。
- [Calc_PilotIn_ToClaw.slx](D:/666/VE25/sp/04_Design/Calc_PilotIn_ToClaw/specification/Calc_PilotIn_ToClaw.slx)：根SID4→108 handleGCS，108#out:3→809 RC；SID5经Goto/From到552 handlePilotIn，552#out:1→808 PilotIn。
- [Calc_PilotInData.slx](D:/666/VE25/sp/04_Design/Calc_PilotInData/specification/Calc_PilotInData.slx)与[SignalProcessingLib.slx](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)：比较及Voter锚点见上；输入类型为[SP_Step_New.sldd](D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd)的SpIn_Sitck、SpIn_Key、SpData_Single、SpData_Enum，以及SpIn_GcsMsg中的两组远程操纵类型。
- 本次四个业务模型当前SHA-256均与已核验提取一致：Calc_GCSData=D92EB5C7C942656ED04C6AF72D11EEB821AA3316256DB308E27DBF1E9F0E8BCF；Calc_PilotInData=7DEB3921F4C73ACB63650353A6498AA629BA674A366511183B9F5F10D5BD89A2；Calc_PilotIn_ToClaw=3CE1556CC2EBED6260936810D9EB2FCB82DFF0BB0608205C79CAA6CD51623462；VE25SP=20D8C7DE2A103D8FF298BCD5BB0C28F73AA6CCC2E27D43D0F5AB9952365F77E4。
- 公共库SHA-256=0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490；参数脚本=49AAF3CBA343CA4B8E1A4C4B8959B1BBD4824C56378C36B61B74119F5DB218B0。
- 仅编辑功能梳理和本问答，以基线哈希、唯一锚点、反向还原与回读保护其他正文。核验记录：[Q041文档核验](D:/666/VE25/.runtime/sp_learning_qa/q041_validation.json)。未修改模型、字典或参数；未运行MATLAB、编译或仿真。sp既有两项project XML修改保留，无新增工程中间文件。
- closeout_outcome=completed；verification_status=passed（静态来源与文档核验）；Delta-K route=project-only。

## Q042：GCS和Sys_Status为何采用与FCC有关的相对通道标签

- 日期：2026-09-24；修订R001。用户质疑GCS和系统状态并非FCC产生，为何用FCC的相对位置标记。已订正功能梳理3.1词条和3.5.1首段，并补充三种概念的区别。
- 订正表述：Curr/Left/Right是本机视角下的逻辑通道位置，不能笼统理解为FCC之间的物理位置，也不能从后缀推定业务数据产生者或实际发送方。Q040的编号表仍是GCS选源实现事实，不是通用的物理来源表。
- GCS的数据类别和通道后缀回答不同问题：GCS说明输入内容类别，Curr/Left/Right标识三份候选记录的位置。将某位置用于FCC编号相关的选源，不会改变原始操纵内容的来源。
- 当前`Calc_GCSData`确实用fccId/activeFccId计算优先候选位置；该结果作为两组选源的default输入。它属于选择控制信息，不能证明三份报文分别由三个FCC接收或转发。
- 例：fccId=0、activeFccId=PFCC2时优先位置为Right；如果Right上的本组数据有效，则选Right。这只是“选哪个输入口”，没有证明“FCC2产生地面站指令”或“FCC2就是本报文发送端”。
- `Sys_Status_Curr/Left/Right`是VE25SP根输入，实际类型SpIn_CcdlMsg，连接`Calc_SysInfo/SpIn_CCDL_Curr/Left/Right`。该模块保留各路SysReady/LimitDispatchInfo并使用各自Valid，无fccId或activeFccId编号转换输入，不能擅自把GCS映射表扩展为它的物理接线表。
- SP模型边界与FCC整机边界不是同一概念。从SP输入口进入只能说明内容由SP上游提供；当前局部图没有足够信息确认是否由FCC其他软件产生、经通道通信接收，或由其他设备提供。因此“不是SP内部生成”不能直接推导为“不是从FCC传出”。
- 一般设计上，外部设备产生业务内容、各通道形成记录、模型按通道选择记录可以共存；这只是解释两种身份不矛盾，不是本项目实际路由的证明。具体端到端关联仍须上游适配代码、ICD或整机架构资料。
- 当前模型保留的`Calc_GCSData/system_root.xml` Annotation SID154：“问题：Rms2PublicMsg里面的CHA指的是FCM，为啥会跟GCS通道有关？”说明内部尚有相关疑问记录；注释没有给出已经解决的架构依据。本次不将FCM与FCC等同。
- 准确结论：已确认GCS的FCC身份到选源位置映射，以及Sys_Status三路记录的处理；尚未闭合原始产生者、实际发送者、通信路径与输入口之间的完整映射，也未取得GCS为何跟随主控FCC的正式设计理由。

### 静态来源与文档修订

- [Calc_GCSData.slx](D:/666/VE25/sp/04_Design/Calc_GCSData/specification/Calc_GCSData.slx)：system_6.xml SID8取fccId/activeFccId，SID14进入映射，SID62输出ActiveFCC；根层ActiveFCC连接SID63/155两组选源的第4输入default。当前SHA-256=D92EB5C7C942656ED04C6AF72D11EEB821AA3316256DB308E27DBF1E9F0E8BCF。
- [VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)：根SID16/17/18为Sys_Status_Curr/Left/Right，端口6/7/8、类型SpIn_CcdlMsg；另有SID19 SpIn_SysInfo_Curr，端口9、类型SpIn_SysInfos，不能混淆。
- [Calc_SysInfo.slx](D:/666/VE25/sp/04_Design/Calc_SysInfo/specification/Calc_SysInfo.slx)：根SID111/112/113分别为SpIn_CCDL_Left/Right/Curr；system_115.xml SID116/117/118接收三路，SID159/160/131提取Valid/SysReady/LimitDispatch，SID153～158保留三路结果。fccId/activeFccId不在本模块输入接口内。
- 当前GCS、SysInfo、VE25SP模型哈希与此前静态提取一致；直接复查原SLX端口及GCS控制连线。只修改功能梳理和学习问答，未修改工程、未运行MATLAB。文档核验见[Q042核验](D:/666/VE25/.runtime/sp_learning_qa/q042_validation.json)。
- closeout_outcome=completed；verification_status=passed（静态来源与文档核验）；Delta-K route=project-only。

## Q043：VDT/Dis_Key采集通道与ADA/RA设备来源为何采用不同标签

- 日期：2026-09-24；修订R001。用户追问操纵输入为何有FCC相关标签、字段如何定义，以及ICD中的ADA/RA多路为何未按FCC编号划分。
- 本轮新增读取FICD接口资料，订正此前“只看到模型、缺少物理接口背景”的证据状态。主目录名V1.4，主文封面Rev. V2、更改记录V2有效性AC102+；当前模型与该文档的构型完全一致性未获确认，来源分层记录。
- 模型事实：Calc_PilotInData只接VDT_Stick_Curr/Left/Right和Dis_Key_Curr/Left/Right六个输入，不读取fccId/activeFccId、不在内部重新映射FCC身份。其全部simulink XML无这两个名称引用；根SID4/5/6、211/212/213为六路输入。
- 每份SpIn_Sitck有CSSU_X/Y、RSSU_X/Y、LSSU_X/Y六项SpData_Single(data:single, valid:boolean)；每份SpIn_Key有十四项SpData_Enum(data:uint32, valid:boolean)。同一业务量的Current/Left/Right构成一组：system_1524.xml SID93组织CSSU_X三路，SID305组织Key_AirGnd三路，SID794/1216汇总六项/十四项。三个端口不是三个不同物理操纵杆的代称。
- 比较器输入顺序：system_1271.xml SID1277/1281及system_1257.xml SID1263/1267依次为Current.data/valid、Left.data/valid、Right.data/valid。SP_InteriorBus ENTRY10/11定义Bus_VDTState/Bus_GPIOState三成员；ENTRY12定义输出VDT:tSingleData[6]和GPIO:tU32Data[14]。
- 当前SpIn_Adahrus ENTRY46含31项业务量，每项Dimensions=3，经纬度用SpData_Double，其余用SpData_Single；ADA输入把候选记录存于各字段数组内，形如L324_Pitch_Angle[1..3]。
- 当前SpIn_Ra ENTRY19只有L164_Radio_Altitude[2]。RAMsgsPreprocess根SID5输出接SID18 Concatenate输入1；SID17零初始化SpData_Single接输入2；合成3项后进入SID16。第3项有效性为false，不是新增第三台RA。
- 本次直接核查AdahruMsgsPreprocess、RAMsgsPreprocess、Calc_NavSourceSelection、Calc_VotedRA的simulink XML，均未出现fccId/activeFccId引用。该结论限于当前模型文件，不替代整个SP上游适配审查。

### FICD补充证据及其解释

- 01-03综合开关Discrete附件《4-离散量功能逻辑表》J2/J3/J4/J6/J8要求3台FCC分别监控3个channel开关状态并表决。01-08侧杆Discrete附件同名表D2列FCC1&2&3:J1:50&51，E2三余度，J2要求检测上升沿、余度表决后送PFC；其他相应行也有三余度规定。但Backup_Control部分为单余度；右侧杆Backup_Control的接收列指FCC1、逻辑说明却写FCC3，存在附件内部不一致，不将其作为确定接线映射。
- 01-07侧杆Analog附件《4-侧杆角度与RVDT输出关系&逻辑表》K2:K6、K8:K12规定三余度表决取中间值输出给PFC。同附件《6-AC102记录表》B2:B4、H2:H4列CSSU:1/2/3与RSSU:1/2/3，提供杆位三余度记录背景；表内称CSSU为中央侧杆、RSSU为右侧杆。没有因此认定已读到所有RVDT绕组到FCC针脚的接线映射。
- 01-09 CCDL附件《4-CCDL_Msg》B259:B270列Data.VDT[0..5].Valid/Value，B271:B282列Data.GPIO[0..5].Valid/Value。B257将该报文命名为CCDL_PilotInMsg；N260称VDT[0]为Aileron (Roll) physical analog value。它证明CCDL中有这类操纵量交换字段；不能仅凭字段名把其处理阶段、量纲或具体上游代码与当前SP端口逐项等同。GPIO为六项，而当前SP为十四项，该版本/覆盖差异需保留。
- 01-02 ADAHRS附件《4-A429_Tx_1》《4-A429_Tx_2》M5明确SDI为源设备ID，01=ADAHRU1、10=ADAHRU2、11=ADAHRU3；前一表U35要求三台ADA_BIT全部收集转发航电。B3:E3为NAV/ADAHRU/FCS/FCC，区分发送测量设备与接收FCC。
- 同表V108、V262、V271、V279、V287、V295虽有“FCC1记录ADA1；FCC2记录ADA2；FCC3记录ADA3”，上下文是“仅FTDR记录数据”，不推广为全部测量数据的物理接收分配。
- 01-04 RA附件《4-A429_Tx》L4定义源设备ID RA1=01、RA2=10；B2:E2为NAV/RA/FCS/FCC；U2也提两台RA。U2属于“更改后信号处理”栏，不能将其中姿态补偿和差值监控套成当前模型行为。本轮未从该附件取得第三台RA定义。
- 设计解释：操纵量的多路具有三余度采集、FCC分别监控及CCDL交换背景；导航数据的源号直接标识ADAHRU/RA设备。因此，接收/采集通道身份与测量设备身份是两个维度。把某设备的数据放到另一个FCC接收，并不会改变该设备的源身份；不能把同一设备的多个接收副本直接当成多个独立传感器。
- 覆盖边界：当前十四项Dis_Key都放在三份同构Bus内，不等于十四项物理按键均有三套硬件。对单余度项如何复制、置无效或适配到三路，当前SP入口没有给出填充逻辑，仍需上游接口实现。
- 待确认：操纵端口与具体FCC编号的上游轮换/填充、SDI到SP数组下标的适配映射、GPIO六项与十四项的对应，以及是否每台ADA/RA向所有FCC发送。ARINC429表接收方只写FCC统称，没有据此建立三接收方广播拓扑。本轮证据不解决GCS/FCM历史关联问题。

### 文件来源和核验

- [FICD主文](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD).docx>)；本次读取快照SHA-256=9AE5D0583FAE1A9F22BF1E690529E420EFB05632A598AFC9C5A3C6A69E063FD7。
- [FICD附件01-02：飞控计算机&ADAHRS-ARINC429](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/附件1：VE-IC11-003-01_VE25-100飞控系统FICD构型索引基线/VE-IC11-003-01-02-飞控计算机&ADAHRS-ARINC429/VE-IC11-003-01-02-飞控计算机&ADAHRS-ARINC429.xlsx>)；SHA-256=44631DC47814602C470F66D47397E4365B3D3B0B9614B4DD6942A894948D5532。
- [FICD附件01-03：飞控计算机&综合开关面板-Discrete](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/附件1：VE-IC11-003-01_VE25-100飞控系统FICD构型索引基线/VE-IC11-003-01-03-飞控计算机&综合开关面板-Discrete/VE-IC11-003-01-03-飞控计算机&综合开关面板-Discrete.xlsx>)；SHA-256=5C2BFF827D83B121F04EA658D600DDA66F3A4B3394EBCB50AEC0873BCA7630B6。
- [FICD附件01-04：飞控计算机&无线电高度表RA-ARINC429](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/附件1：VE-IC11-003-01_VE25-100飞控系统FICD构型索引基线/VE-IC11-003-01-04-飞控计算机&无线电高度表RA-ARINC429/VE-IC11-003-01-04-飞控计算机&无线电高度表RA-ARINC429.xlsx>)；SHA-256=91847466EBD394E5B3C90DC67B3EED22F42AA3C251F1E3C88A7EC499F5FD743F。
- [FICD附件01-07：飞控计算机&侧杆-Analog](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/附件1：VE-IC11-003-01_VE25-100飞控系统FICD构型索引基线/VE-IC11-003-01-07-飞控计算机&侧杆-Analog/VE-IC11-003-01-07-飞控计算机&侧杆-Analog.xlsx>)；SHA-256=AABB35EA519865F963E9FCFF030DA8E4D600E12C6A68C3CECF4CD5BADF147102。
- [FICD附件01-08：飞控计算机&侧杆-Discrete](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/附件1：VE-IC11-003-01_VE25-100飞控系统FICD构型索引基线/VE-IC11-003-01-08-飞控计算机&侧杆-Discrete/VE-IC11-003-01-08-飞控计算机&侧杆-Discrete.xlsx>)；SHA-256=500EE6CCA004499319012170E4A882155B4F9E0746D110018C2A9823698E19EE。
- [FICD附件01-09：飞控计算机CCDL-RS422](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/附件1：VE-IC11-003-01_VE25-100飞控系统FICD构型索引基线/VE-IC11-003-01-09-飞控计算机CCDL-RS422/VE-IC11-003-01-09-飞控计算机CCDL-RS422.xlsx>)；SHA-256=C532ABCA0770CB4DE21CE28F969BBECA75312449AB88C4FF5183CEE2CA6F9C6A。
- [接口附件指纹清单](D:/666/VE25/.runtime/sp_learning_qa/q043_ficd_sources.json)。

- 所读附件更改说明：01-03版本B（2026-06-09）；01-07版本B（2026-06-02）；01-08版本A（2026-02-26）；01-09版本A（2026-05-14）。主文第5节图5-1标注不含试飞改装，本轮未渲染图形、未据图核实线束。
- 当前模型和SP_Step_New/SP_InteriorBus哈希与此前提取一致；新FICD附件按本次实际读取版本记录，不用外层文档覆盖sp中的当前结构。
- 已补入功能梳理3.5.1“操纵采集通道与导航设备来源的区别（FICD补证）”，并在原VDT说明增加指针。仅修改这两份学习文档；未修改工程、未运行MATLAB，sp未新增中间文件。核验记录：[Q043文档核验](D:/666/VE25/.runtime/sp_learning_qa/q043_validation.json)。
- closeout_outcome=completed；verification_status=passed（静态来源与文档核验）；Delta-K route=project-only。

## Q044：FCC采集操纵量与地面站报文的通道标签

- 日期：2026-09-24；修订R001。用户确认VDT/Dis_Key是否由FCC采集，以及GCS是否也要标记采集设备。
- 严格区分：VDT/Dis_Key的Curr/Left/Right是端口后缀，不是每份业务数据中的fccId字段。当前记录为data/valid；Calc_PilotInData无fccId/activeFccId输入或编号转换。
- FICD支持在FCC设备边界上称“FCC采集/监控操纵输入”：01-03、01-08明确三个FCC分别监控相关开关通道；01-07给出Analog杆位三余度处理背景。实际采集接口向SP提供软件记录，不能把SP算法说成直接读取传感器电压。具体RVDT与各FCC的电路对应仍未闭合，部分按键为单余度，沿用Q043边界。
- GCS应描述为地面站产生报文、飞控侧接收处理。Calc_GCSData按fccId/activeFccId决定候选输入优先位置；这是软件选源控制，不是地面站原始指令的产生者定义，也不证明SP往原始协议添加了fccId字段。
- 新增直接来源：[FICD试飞改装附件02-01：飞控计算机与地面站RS422](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/附件2：VE-IC11-003-02_VE25-100飞控系统FICD试飞改装构型索引基线/VE-IC11-003-02-01-飞控计算机&地面站-RS422/VE-IC11-003-02-01-飞控计算机&地面站-RS422.xlsx>)。
- 《3-协议综述》G12=消息源节点；I12写地面站单片机（MCU），MCU1:0x2A、MCU2:0x3A。这一MCU为地面站单片机，与SP电机接口中的MCU语境不同。C21:D22中SrcDevice/DstDevice为GCS→PFCCS；I11写上行报文目的ID恒定0x5A。
- 这证明协议有发送节点身份信息，但该源ID不是FCC采集设备标签。必须分开协议发送节点ID、SP候选通道位置、FCC选源控制编号。
- 《5-架构参考》嵌入图经只读查看，画有电台1/2及标注RS422的链路通往FCC1/2/3和BFCC；支持多个FCC接收地面站链路的架构理解，图中未定义接收数据到SP输入口的装配关系。
- 所查单元格文本未给出Curr/Left/Right与FCC1/2/3的填充对应。该附件位于试飞改装构型附件内；两个地面站MCU不能仅凭数量或名称直接映射成当前SP的三份GCS记录。
- 新附件SHA-256=C4CE30E29E69E930825716089AC4293C9DAA7E806C23504E8DA72CBD5BFFEF37。此前操纵接口01-03/07/08/09及Calc_GCSData文件指纹未变化，沿用已读取证据。
- 已将澄清和协议源节点定义补入功能梳理3.5.1。仅修改功能梳理和本问答；未修改模型、未运行MATLAB，sp无新增中间文件。验证见[Q044文档核验](D:/666/VE25/.runtime/sp_learning_qa/q044_validation.json)。
- closeout_outcome=completed；verification_status=passed（静态来源与文档核验）；Delta-K route=project-only。

## Q045：handleGCS中的SSX、SSY及CP/FO含义

- 日期：2026-09-24；修订R001。用户根据handleGCS截图询问SSX与SSY。
- 中文业务定义已有直接需求依据：SSX=侧杆纵向操纵输入，对应本路径的遥控俯仰轴；SSY=侧杆横向操纵输入，对应遥控滚转轴。CP为主驾接口，FO为副驾接口，deg为角度单位度。
- `SS≈Side Stick`是命名释读，本次需求条款及限定核查的侧杆Analog附件未给出SS/CP/FO项目正式英文展开，不强行把CP或FO扩成未核实的英文短语。
- 当前模型直接连线：`GcsData.chElevator.data→CalcSSX→SSX_CP_deg`；`GcsData.chAileron.data→CalcSSY→SSY_CP_deg`。它们是操纵量，不是飞机当前俯仰角或滚转角；X/Y也不能直接套为机体系转动轴编号。
- 截图中的SSXVld_CP_b和SSYVld_CP_b为各轴有效标志，分别由CalcSSXValid、CalcSSYValid生成。本问题未展开StickType、ValidBits组合判据。
- handleGCS将遥控数据整理进统一BUS_PILOT_IN的主驾侧杆字段，CP不是这份报文必然来自座舱主驾实物的证据。
- [需求Excel](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)，工作表“信号处理软件高级需求-SP_SW_HLR”：SP_SW_HLR-503（A347/C347）=主驾侧杆纵向SSX_CP_deg；-505（A349/C349）=主驾横向SSY_CP_deg；-507（A351/C351）、-509（A353/C353）为副驾纵向/横向SSX_FO_deg、SSY_FO_deg。SHA-256=E3F28F03B948613CCAB451EE469B9FD5A87313393398BECD541BDA3126406BEB。
- 上述条款保留旧模型路径。本次仅用于术语定义，不用其中“首飞无副驾信号”等旧构型措辞判定当前完整模型功能状态。
- [Calc_PilotIn_ToClaw.slx](D:/666/VE25/sp/04_Design/Calc_PilotIn_ToClaw/specification/Calc_PilotIn_ToClaw.slx)，system_108.xml：SID116提取chElevator.data→SID195 CalcSSX→SID115输入1，线名SSX_CP_deg；SID117提取chAileron.data→SID227 CalcSSY→SID115输入3，线名SSY_CP_deg。有效支路为SID213→115输入2、SID245→115输入4。当前模型SHA-256=3CE1556CC2EBED6260936810D9EB2FCB82DFF0BB0608205C79CAA6CD51623462，与提取一致。
- 已在功能梳理新增术语小节，并收窄原有“CP/FO、SSX/SSY均无直接定义”的旧措辞。只修改功能梳理和本问答，未修改工程模型、未运行MATLAB；核验见[Q045验证](D:/666/VE25/.runtime/sp_learning_qa/q045_validation.json)。
- closeout_outcome=completed；verification_status=passed（静态来源与文档核验）；Delta-K route=project-only。

## Q046：handleGCS完整处理链与GcsData/MpioData结构

- 日期：2026-09-24；修订R001。用户要求解释VE25SP/Model5 (Calc_PilotIn_ToClaw)/handleGCS及两个输入总线的全部成员；实际名称为MpioData。
- 当前实现：handleGCS接收已选源GcsData和MPIO处理结果MpioData，完成四轴死区/比例/限幅、独立有效性、开关锁存、边沿事件、检查枚举和高压/空地转换；输出Air2GndValid、Air2Gnd及BUS_PILOT_IN类型Gcs，后者连接外层RC。
- GcsData共有39项（31 tU8Data、8 tSingleData），MpioData共有28项（18 tU8Data、10 tSingleData）；包装均为标量data/valid/reset。handleGCS分别读取20项和5项；“未读”限定本子树，不推断全工程未用。
- 四轴死区均为[-0.05,+0.05]，余量除0.95；Elevator→SSX ±10.5°，Aileron→SSY ±10°，Throttle→CSX -11.5/+9.5°，Rudder→CSY ±10°。数值无效不会自动清零，限幅不代替有效性检查。
- 轴有效性=该轴valid AND (StickType.data==1 OR (StickType.data==2 AND ValidBits对应位为1))；掩码依Aileron/Elevator/Throttle/Rudder为1/2/4/8。不额外读取StickType.valid、ValidBits.valid。
- 五个布尔锁存初值false、置位优先，未满足置位/清除时保持；输入reset不清除这些状态。TRC和停站请求分别做边沿；TRC再做历史事件OR展宽。SID515采用Delay缺省长度2的静态解释，未把拍数换算为秒。
- MFD按升模→桨检查进入/退出→舵面检查进入/退出→航前检查完成排序；初值None，未触发时按条件清除ModeUpgrade或保持上次枚举，不是自动单拍脉冲。旧HLR-518的传感器门控及SW4条件与当前实现存在表述/结构差异，暂未认定构型关系。
- 两个重要边界：PwrOnVld当前为valid且data不等于0，不是仅允许1/2；Air2Gnd把KeyAirGroundSW.data==1直接转为true，而FICD原文描述为改变空地状态的请求，不能用它证明实际接地。
- MPIO同名协议字段补证：CenterGravity为%MAC、GrossWeight注释为起飞重量kg、FMS_Roll_Command为滚转角指令度、垂直速度为m/min且上正下负；UsablePwr_KW单位Kw与“可用电量”注释有差异。FMS_..._Valid.data业务码与外层.valid不同。
- SelectedFCC当前未赋值，默认data0/validfalse/resetfalse；全部已赋值MPIO记录reset=false。GCS前25项遥控reset来自选源变化，后14项FMCP reset=false。
- 已将完整67行字段表、逻辑与精确来源补入[功能梳理第3.6节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)。使用当前SLX/字典作实现依据，协议及旧需求仅作字段术语、单位和差异依据。
- 中间核验：[GCS字段与证据](D:/666/VE25/.runtime/sp_learning_qa/q046/gcs_fields.json)、[MPIO字段与证据](D:/666/VE25/.runtime/sp_learning_qa/q046/mpio_fields.json)、[离散分支复核](D:/666/VE25/.runtime/sp_learning_qa/q046/discrete_logic.md)、[最终静态核验](D:/666/VE25/.runtime/sp_learning_qa/q046_validation.json)。这些外层材料不替代sp权威来源。
- 仅更新功能梳理与本学习问答；未修改模型或数据字典，未运行MATLAB、编译或仿真；sp未新增中间文件。问题处理状态=当前结构和静态处理已解答，少量协议定义/构型差异待确认；学习表现=掌握程度未确认。
- closeout_outcome=completed；verification_status=passed（静态来源与文档核验，未运行模型）；Delta-K route=project-only。

## Q047：VdtCBIT的含义、编码和作用

- 日期：2026-09-24；修订R001。问题处理状态=当前实现已解答；学习表现=掌握程度未确认。
- CBIT通用展开Continuous Built-In Test；当前VdtCBIT实际是uint32标量有效性检查状态字，最低三位以1/2/4分别表示Current/Left/Right受检失败。
- 每路只检查VDT[1,2,5,6]，对应CSSU_X/Y、LSSU_X/Y，任一原始valid=false即置位；RSSU_X/Y的索引3/4不纳入。未在本局部找到排除原因。
- 原始输入重组后与比较/表决支路并行计算，无Delay、确认计数或锁存。因此不是操纵表决有效标志，也不是三路一致性结果；VdtCBIT=0只说明受检valid通过。
- Calc_SysInfo/Subsystem9按VdtCBIT与SpCPT.check_VDT_Bit及uint32(7)的按位交集是否非零，形成VdtLossRedundancy。
- 当前SP_Parameters.sldd保存check_VDT_Bit=uint32(0)，按此配置该冗余丧失输出恒false，但VdtCBIT仍正常计算。未验证运行时参数实例。
- Calc_CcdlMsg_Out将此标志与操纵源丢失合并，在PilotInSelect!=1条件下参与LimitDispatch候选码4判定；最终LimitDispatch==0参与SysReady。没有由本状态字直接反馈操纵表决器的连线。
- 已补入[功能梳理第3.7节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)，含检查范围、位表、0/2/5/7例子、配置作用及精确SLX锚点。静态来源与文档验证见[Q047记录](D:/666/VE25/.runtime/sp_learning_qa/q047_validation.json)。
- 本轮仅更新功能梳理与问答；未修改模型、参数或字典，未编译/仿真，sp未增加中间产物。
- closeout_outcome=completed；verification_status=passed（静态来源与文档核验）；Delta-K route=project-only。

## Q048：学习进度更新——操纵接口与系统健康判断入口

- 评估日期：2026-10-08（Asia/Shanghai）。范围为本任务可见对话及Q001—Q047，最新业务问答记录到2026-09-24；不推定其他任务或线下学习情况。
- 当前定位：按阅读范围，已实质推进到S4的横向业务模块学习，重点从设备反馈/MPIO延伸至地面站、机上操纵输入和控制律接口转换，并进入系统健康判断入口；同时深化S2/S3并开始S5的接口依据追溯。
- 按验收证据，仍处于静态阅读、链路理解与局部设计分析阶段，尚未证明独立贯通原计划CAS故障链、完成逐拍推演或形成验证闭环。未展示不等于不会，但不能按题数、材料页数或经过的时间认定掌握与完成率。

### 相较Q030的新增进展

| 主题 | 新增记录与材料 | 进展的准确含义 |
|---|---|---|
| 完整信号链与Bus层级 | Q033结构文档；Q034 RA从原始输入、预处理、选源到控制律输出 | 已提供跨模型追踪材料；不能以RA资料已成稿替代S2指定的CAS独立场景验收 |
| 地面站与操纵输入 | Q035两组GCS选源；Q037—Q038 FMCP、VDT/Dis_Key；Q045—Q046 handleGCS及67项字段 | 学习内容进入来源选择、缩放限幅、有效性、状态保持及控制律接口封装；完整机上操纵与最终选路仍待集中贯通 |
| 通道身份与来源依据 | Q040—Q044 Curr/Left/Right、FCC、设备源号及FICD位置 | 用户连续追问“同源为何三路”“为什么与FCC关联”“依据在哪里”，体现主动核对物理来源和软件通道之间的关系 |
| 状态/时序与配置 | Q046锁存、边沿、reset差异；Q047 VdtCBIT及检查掩码 | 已讨论从输入、内部历史到输出的影响；具体实现结论由分析过程提供，不能归为用户独立推演成果 |
| 系统状态与需求依据 | Q047连接到VdtLossRedundancy、LimitDispatch/SysReady；Q031/Q039/Q043/Q045—Q046文档与协议追溯 | 已触及系统健康链和S5证据意识；尚未完成全部系统状态逻辑或“需求—模型—参数—测试”闭环 |

### 对照S1—S6计划

| 阶段 | 当前覆盖情况 | 仍缺的可验证成果 |
|---|---|---|
| S1 总体架构与工程入口 | 当前学习主线明确为VE25SP；已有功能地图、术语索引和Bus资料 | 用户独立讲清总体输入到消费者；工程初始化、配置与有效运行基线的完整核对 |
| S2 CAS完整链 | CAS已有局部阅读，另已提供RA完整静态链作为参照 | 独立串起当前CAS原始输入至最终控制律接口，并解释一个异常/恢复场景，关联参数和适用需求 |
| S3 公共机制与逐拍时序 | 已讨论For Each、Delay、比较、表决、锁存、位编码、valid/reset和实例差异 | 至少一份独立“输入—历史状态/计数—data/valid/reset”的逐拍表 |
| S4 全业务模块 | 当前主要推进阶段；已覆盖导航、设备反馈、MPIO、GCS、操纵转换，并到达系统健康入口 | 补齐机上操纵与最终选路、SysReady/LimitDispatch、CCDL/显示/其他输出组织的整体关系，能从异常现象反向定位 |
| S5 需求与设计依据 | 已主动追问ICD/FICD、编号出处、协议码值与设计原因；有局部精确锚点 | 一条适用需求—当前模型—参数—输出—测试的追踪链及一次纸面变更影响分析 |
| S6-A 验证方案 | 现有记录未见完整方案 | 正常、边界、故障、恢复用例，观测量、判定标准与回归范围 |
| S6-B 实践与验证 | 本任务已有分析均为静态，未见获准改模、仿真和回归成果 | 在授权及环境条件具备时建立运行基线、实施限定改动并验证；当前不认定完成 |

计划正文的“待开始”及部分旧主线措辞是初始安排，不代表本次实际覆盖状态；VE25SP主线沿用用户已明确的决定。当前不改计划勾选、不把旧SpMdl作为现行实现依据，也不将S4阅读覆盖解释为S1—S3均已验收。

### 下一步建议

1. **就当前阅读位置继续贯通操纵链。** 把机上VDT/Dis_Key、GCS两类输入一直连到控制律操纵接口，补清最终选路、无效时输出及reset消费。
2. **沿VdtCBIT进入系统状态链。** 重点阅读Calc_SysInfo到Calc_CcdlMsg_Out中的SysReady/LimitDispatch条件、优先级和配置作用；此后再扩展CCDL、显示等输出组织。
3. **补一份自主推演作为阶段证据。** 先自行写一个操纵输入失效/恢复场景，再回到原计划CAS故障链和监控器逐拍表。材料至少列输入、配置、上一拍状态、当拍data/valid/reset、下游影响及来源位置；不自动启动模型运行。

### 依据与本次改动

- [学习计划](D:/666/VE25/Doc/SP模型学习与维护进阶计划.md:53)：S1—S6任务与验收项；SHA-256=8D3F164BF800D48DD311D01C9BD5D03517FD971433DA3E09EA915B3C623AB27B。
- Q030为上次进度评估；本次对照Q031—Q047及对应用户提问。追加前问答记录SHA-256=7AD44CB615E88CD09644E1EECD55C3932B00437D19B9013A787160F9B9E74FA1。
- 已有[功能梳理](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)和[Bus结构文档](D:/666/VE25/Doc/VE25SP_Bus信号结构说明.md)计为已提供学习材料，不自动计为用户自主成果。
- 本轮仅追加本条及索引，不改变既有学习表现、计划状态、模型或参数。核验对象是记录与计划对应，不是用户能力考试或模型运行。核验记录：[Q048](D:/666/VE25/.runtime/sp_learning_qa/q048_validation.json)。
- closeout_outcome=completed；verification_status=passed（学习记录与计划对照）；Delta-K route=project-only。

## Q049：VE25SP右侧浅紫色大框与模型块边框

- 日期：2026-10-08；修订R001。用户根据顶层截图询问右侧模块集中框选的特殊含义。
- 原SLX根系统Annotation SID386=area_annotation；SID387标题“计算SP分区输出信号”，颜色属性为[0.901961,0.901961,1.000000]。
- Area用于视觉组织与整体移动；框内Model Reference块仍位于VE25SP根层，区域未增加可执行子系统层级、端口、使能/触发或原子执行边界。标题及画布位置不定义执行顺序或操作系统分区。
- 业务作用是集中展示输出接口转换/组包：控制律操纵、系统、导航、FMS，以及CCDL/显示等；与左侧内部输入处理形成阅读上的分组。
- 已补入[功能梳理第2.3节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)。通用Area机制参见[MathWorks](https://www.mathworks.com/help/simulink/ug/visually-organize-models-using-areas.html)，项目对象类型以原SLX为准。
- 模型SHA-256=20D8C7DE2A103D8FF298BCD5BB0C28F73AA6CCC2E27D43D0F5AB9952365F77E4。仅更新两份学习文档，未改变模型、运行配置或计划状态；未编译/仿真。核验记录：[Q049](D:/666/VE25/.runtime/sp_learning_qa/q049_validation.json)。
- 问题处理状态=已解答（静态结构确认）；学习表现=掌握程度未确认。
- closeout_outcome=completed；verification_status=passed（静态对象与文档核验）；Delta-K route=project-only。

## Q050：MPIO数据的发送设备与原始来源

- 问题：MPIO的数据是从哪一个/哪些设备中传来的。
- 回答日期：2026-10-08（Asia/Shanghai）；来源为本轮实际读取的FICD主文及附件，当前模型为VE25SP/Calc_MPIOData。
- **确认结论：MPIO在FICD图5-1中标为显控计算机；直接向FCC发送这组数据的协议节点是MPIO1和MPIO2。** 报文名称为`IDU_Data_Msg`，不能仅由IDU名称推定实体显示屏是发送设备。MPIO正式英文展开仍未定义。
- [FICD主文](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD).docx>)第5章图5-1，内嵌`word/embeddings/Microsoft_Visio_Drawing.vsdx → visio/pages/page1.xml`，Shape153文字为“显控计算机／MPIO”。该图注明“不含试飞改装”；文件名V1.4、正文封面Rev.V2，更改记录列适用性AC102+，本轮不认定其实际发布状态。
- [FICD附件01-01](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/附件1：VE-IC11-003-01_VE25-100飞控系统FICD构型索引基线/VE-IC11-003-01-01-飞控计算机&显控系统-ARINC825/VE-IC11-003-01-01-飞控计算机&显控系统-ARINC825.xlsx>)：`03-Msg ID!A136/K136/J136`为`IDU_Data_Msg / MPIO1 / 84A8CC0`，A138/K138/J138为同名报文/MPIO2/84A8CC1；`04-显控相关参数!F1/F3`为Transmitter/MPIO，T1/T3为Receiver/FCC，B3为报文名，D3为100 ms周期。接入类型为该附件所定义的ARINC825。节点标识与具体硬件机箱/板卡/安装位置的对应仍未确认。
- **字段原始来源的范围：** S3/S4支持能源系统、BMS、电池包与高压业务；S7/S16/S22支持FMS目标、滚转指令、垂直模式请求。未说明具体BMS节点、运行FMS的物理设备及其到MPIO的路径。S41/S53重心/重量、S43/S44/S55检查单字段亦未指定生产设备或录入终端。S62关于IDU转换FMCP航向的说明仅适用于其具体字段，不扩展到全部MpioData成员。
- **模型边界：** VE25SP根输入SID15`SpIn_MpioInfo : SpIn_Mpio`、`PortDimensions=1`，SID362调用Calc_MPIOData；该模型整理一份标量Bus，输出`MpioData : BUS_MpioData`。FICD两个发送节点的接收、选择/组合及输入装配属于待核的集成环节，当前字段处理模型不足以证明具体接入规则。
- 补证关系：Q039关于“只能定位航电接口相关来源”的限制已被本次显控计算机标签及节点表补足；正式英文全称、原始生产者及接入选路仍保持未确认。历史HLR的`MpioSpMdl/SP_IDU_Msg`和IDU命名不覆盖当前FICD发送节点证据。
- 文档更新：功能梳理3.2节MPIO小节、对应模型表及接口索引同步补入设备与节点依据；3.6节MpioData说明承接这一定位。本次不改设计说明任务文档、模型、数据字典或学习计划完成状态。
- 来源SHA-256：主文9AE5D0583FAE1A9F22BF1E690529E420EFB05632A598AFC9C5A3C6A69E063FD7；附件864D7977DBBDFEE9E45CDE4BF4C5116D1489DB7E8CEDE3B3A9FE4E877E3AB01E；VE25SP模型20D8C7DE2A103D8FF298BCD5BB0C28F73AA6CCC2E27D43D0F5AB9952365F77E4；Calc_MPIOData模型A5B051AE97E92967CCF9E101D137D566B78DC55C57FC90B78910BA7F841F2FE6。
- 验证：静态DOCX/内嵌Visio/XLSX/SLX来源检查及文档内容核验；未加载、编译或仿真模型。记录：[Q050核验](D:/666/VE25/.runtime/sp_learning_qa/q050_validation.json)。用户掌握程度未确认；Delta-K route=project-only。

## Q051：SP的系统定位与上下游

- 问题：sp模型的定位以及它在整个飞控系统中的上游和下游各是什么。
- 日期：2026-10-10（Asia/Shanghai）；关联S1总体关系图与从输入讲到输出消费者、S2入口/组包定位，兼及S5来源追溯；提供说明不证明这些阶段已经掌握。
- **定位：** 当前VE25SP为信号处理软件的算法模型，功能上处于飞控侧输入采集/解析与控制律输入之间；执行状态监控、有效性/一致性处理、冗余选源/表决及数据转换/封装，并提供显示、CCDL与DRMS接口。
- **直接上游依据：** [高级需求表](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)工作表“信号处理软件高级需求-SP_SW_HLR”，A26/C26=SP_SW_HLR-766，明确输入来自信号输入处理软件解析的信号；A28/C28=SP_SW_HLR-767说明生成C代码供集成。此表保留REG300/VE25-101及旧模型背景，不据它将旧组成、周期或构型自动认定为当前部署。
- **上游设备/信息类别：** ADAHRS/RA/FTADC导航测量，MCU/SCU设备反馈，VDT/Dis_Key与GCS操纵，MPIO显控综合信息，以及本机系统、候选通道状态和DRMS公共信息。VE25SP根层20个Bus输入确认；实际驱动、解码、输入适配与记录填充不在当前根模型中。
- **直接下游：** 控制律四类输入`NAV_Root、Cockpit_In、SYSInfo_In、FMS_In`；显示/状态接口`SpIn_Nav2Display、NavDataState、SPDispMsgOut`；跨通道报文`SpCcdlMsgOut`；余度管理反馈`SP2DRMS`，合计9个根输出。控制律类别接口分别由Calc_NAVMsg_ToClaw SID375、Calc_PilotIn_ToClaw SID376、Calc_SYSMsg_ToClaw SID377、Calc_FMSMsg_ToClaw SID397形成；CCDL/显示来自SID406/408。根SID446赋值`navValid、isBfccActv、isBfccActvValid`，送SID458 SP2DRMS。
- **名称与边界：** FMS_In等In后缀是消费者侧的输入定位，在SP根层实际为输出；FMS_In不证明SP向外部FMS硬件发送命令。DRMS是双向软件接口邻居，本机/激活FCC等信息经RmsPublicMsg输入，导航有效性/备份激活相关信息经SP2DRMS输出；D=Device已由用户确认，不能当成记录系统。Curr/Left/Right仍是候选位置标签，不直接证明物理来源。
- **系统链解释：** 控制律使用经SP整理的数据后，继续进入控制指令计算和执行链。MCU/SCU给SP的反馈属于上游数据，而这些设备接受飞控指令时属于执行端；上下游要按数据方向划分。本轮未读取控制律及输出驱动实现，后半执行链只作职责示意。
- **FICD锚点：** [主文](<D:/666/VE25/Doc/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD)/VE-IC11-003_V1.4_VE25-100飞控系统功能接口控制文件(FICD).docx>)图5-1内嵌Visio Shape195/196=ADAHRU/RA到FCC，153=显控计算机MPIO；`6附件01-05/01-06=FCC与舵机、电升力/电推力，01-09=FCC CCDL。GCS/标准大气机见试飞改装02-01/02-02，不归入注明“不含试飞改装”的图5-1。FICD只确认物理交联，不能证明SP内部调度或输出接收软件的实际集成实例。
- **实现与交付来源：** [VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)根`system_root.xml`及导航`system_54.xml`；[代码生成入口](D:/666/VE25/sp/50_Delivery/VE25SP_generate_code.m:3)modelName=VE25SP。模型SHA-256=20D8C7DE2A103D8FF298BCD5BB0C28F73AA6CCC2E27D43D0F5AB9952365F77E4；需求表=E3F28F03B948613CCAB451EE469B9FD5A87313393398BECD541BDA3126406BEB；FICD=9AE5D0583FAE1A9F22BF1E690529E420EFB05632A598AFC9C5A3C6A69E063FD7。
- **文档与验证：** 功能梳理新增2.4节定位/上下游表及职责示意图，更新修订日期/范围；问答追加本条。模型/字典/工程配置、设计说明和学习计划不修改。静态来源、格式、有限内容写入及工程状态核验见[Q051](D:/666/VE25/.runtime/sp_learning_qa/q051_validation.json)。未加载、编译、仿真或生成代码；掌握程度未确认。Delta-K route=project-only。

## Q052：MPIO与SYS信号数据的区别

- 问题：如何理解sp模型中MPIO和SYS信号数据的区别。
- 日期：2026-10-10（Asia/Shanghai）；关联S1/S2接口分层及S4系统信息处理；掌握程度未确认。
- **定位：** MPIO按显控输入来源组织能源/高压、功率、重量/重心、检查与飞管目标/请求等业务信息；SYS原始输入/内部SysInfoData聚焦设备和通道健康、余度、就绪及放行；最终SYSInfo_In按控制律需要组织综合系统数据，包含一部分MPIO内容。SYS在此是系统信息类别，不是另一个可由名字直接认定的实体设备。
- **输入/内部层：** SpIn_Mpio31项→Calc_MPIOData→BUS_MpioData28声明成员（当前赋27项，SelectedFCC未赋），各业务项使用data/valid/reset包装。SpIn_SysInfos8项为各类HealthBit及FccHealth，SpIn_CcdlMsg3项为Valid/SysReady/LimitDispatch；Calc_SysInfo另读isFccExist与VdtCBIT，形成17项BUS_SysInfoData（14boolean/3uint8），字段均为标量，不使用上述业务包装。
- **MPIO业务与MPIO健康：** Calc_SysInfo没有MpioData输入，只以MPIO_HealthBit形成MpioLossRedundancy；`(uint32(MPIO_HealthBit) & SpCPT.check_MPIO_Bit & uint32(3)) > 0`，位于system_79.xml SID85/81/83/84/82。本轮不读取check_MPIO_Bit实际配置值，不宣称启用哪些位。FccHealth经根SID181/182直达CurrentFccReady，与Curr通道Valid AND SysReady形成的CurrentSysReady不同。
- **后级汇合：** SysInfoData→Calc_CcdlMsg_Out→独立SysReady→Calc_SYSMsg_ToClaw→SYSInfo_In；MpioData同时进入Calc_CcdlMsg_Out和Calc_SYSMsg_ToClaw。前者抽取PreflightCheckValid.data/valid参与本机CCDL Data.SysReady生成；该成员与模型独立SysReady输出不是同一直接接点。后者接MpioData、DevData、SysReady，未直接接SysInfoData。
- **控制律SYS内容：** BUS_SYSINFO_IN五个直接成员为SysReady_SYSINFO_b、Reset_FCM_b、VCU_Status、Motor_Status、Actuator_Status。SysReady透传，Reset_FCM_b当前false；VCU_Valid由HiVltgSts.valid AND data<=uint8(2)形成，VCU_SOP_kw由UsablePwr_KW有效时本拍数据/无效时Delay保持形成；Motor/Actuator来自DevData。不因VCU字段名推定实体设备身份或正式全称。
- 来源：[Calc_SysInfo](D:/666/VE25/sp/04_Design/Calc_SysInfo/specification/Calc_SysInfo.slx)根SID179/37/181/182/31及system_79/115.xml；[Calc_SYSMsg_ToClaw](D:/666/VE25/sp/04_Design/Calc_SYSMsg_ToClaw/specification/Calc_SYSMsg_ToClaw.slx)根SID13/52/58/12/11、system_52.xml SID54/55/56、system_58.xml SID60/61/62；[Calc_CcdlMsg_Out](D:/666/VE25/sp/04_Design/Calc_CcdlMsg_Out/specification/Calc_CcdlMsg_Out.slx)system_4.xml SID27等；VE25SP根SID363/406/377及相应Goto/From；SP_Step_New和SP_InteriorBus中的准确Bus条目。
- 来源SHA-256：Calc_SysInfo=67B5607BC06A5AB57EB9CE7DC017A2B22E2CECE00E3153EC6EEF0E27AD6B1C43；Calc_SYSMsg_ToClaw=9B5A3035D9D0165AAB3319301D24F40A59FFFE80E5C691143F516E396FA4617E；Calc_CcdlMsg_Out=A7FBAC6DFFE00A2EB2B838E0F65B9312D04260E5820BD39CE02A3CA698B40DF8；Calc_MPIOData=A5B051AE97E92967CCF9E101D137D566B78DC55C57FC90B78910BA7F841F2FE6；根模型和两字典当前哈希见核验记录。
- 已更新功能梳理2.5节及修订范围，追加本问答；只静态读取模型/字典，未修改sp、运行模型或修改设计说明与学习计划。核验：[Q052](D:/666/VE25/.runtime/sp_learning_qa/q052_validation.json)。Delta-K route=project-only。

## Q053：isSynced已编码为何还保留独立字段

- 问题：图中Synced已注入stateBits，为何Bus_SingleSignalNormalInfo仍保留isSynced，意义是什么。
- 日期：2026-10-12（Asia/Shanghai）；关联S2结构/消费者链、S3状态编码及S4导航表决；掌握程度未确认。
- **生产者事实：** 截图为SignalProcessingLib/Calc_SignalState/For Each Subsystem1，外层system_2421.xml的SID2452，内部system_2452.xml。isSynced输入SID2460→Goto[Synced] SID2544；From SID2548→SignalStateSet_New实例SID2541输入2，From SID2547→最终Bus Creator SID2463输入4。两支同源、无反相/Delay/额外门控。
- DD_SpLib中Bus_SingleSignalNormalInfo四成员为state:tSignalState、data:single、valid:boolean、isSynced:boolean；SP_State中tSignalState含lastTimestamp/stateBits:uint32。字段均为标量，本字段重复不表示另一路设备或另一套同步计算。
- 当前New编码为valid+2*isSynced+4*isMissCompare+8*isBanned，system_2514.xml权重1/2/4/8。因此在本实例同一元素/同次组包中，`isSynced == (bitand(state.stateBits,uint32(2)) ~= 0)`；valid也有双重表达。示意输入valid=true、isSynced=true、其余false→stateBits=3；这是静态逻辑示例，不是仿真结果。
- **独立字段的实际消费者：** [Calc_VotedCAS](D:/666/VE25/sp/04_Design/Calc_VotedCAS/specification/Calc_VotedCAS.slx)根BusSelector SID7/8/9取data/valid/isSynced；Voter/select的system_178.xml SID185/186/187分别计算valid AND isSynced，经SID193形成候选并影响Switch Case选用分支。后续有有效性回退与CASVoterType条件，不宣称不同步一律不能输出。
- **编码状态的实际转发：** [SignalProcessingLib](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)/SrcSelect的system_407.xml SID419/2337/2338取三路完整state，经Signal Copy SID2334/2335/2336进SID417 tRedundantDataState（ch1SignalState/ch2SignalState/ch3SignalState/voterState），SID439为第4输出。明确实例为[Calc_VotedRA](D:/666/VE25/sp/04_Design/Calc_VotedRA/specification/Calc_VotedRA.slx)/output/calcData SID70输出4→SID62 BusAssignment写raRadioAlt_M→SID72状态出口。没有据此断言外部一定显示/记录或按位解码。
- **作用推导：** 独立boolean供算法直接使用，stateBits供完整状态封装；算法读取前者无需知道位序。同源表达不构成独立检测余度。理论上可删字段后解码，但必须同步变更Bus定义及实际消费者，本次不实施。未取得强制重复字段的需求或原始设计动机/兼容性决策，不能声称重复已节省内存或带宽。
- **与旧记录关系：** 本题补足Q007/Q012此前缺少的独立字段消费者与状态转发证据。SP_State.stateBits文字描述仍与当前New编码不一致，且旧SignalStateSet另有布局；本题不更改或合并这些定义，不把bit1解释推广到所有接口，不据静态差异认定运行故障。
- SHA-256：SignalProcessingLib=0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490；DD_SpLib=557DBF7AE4B2D8C13B876DAEFACF648F64E2F32680621F7F2FFCE5EEE59D367D；SP_State=10B55E65511E21A7AD3E376653F985FAFAD2C37CB6D1B3FB8A187E69062B0164；CAS=807AD66E0B5C08BBB3592F75D0363879DF7B29EB20A2146373576764F12BC6D6；RA=3C07C33A91C9B4920D527FC12DE87DE20E76502F8ECA540F757F2C92DD099197。
- 更新功能梳理3.5.6状态字段节并追加本记录；模型、字典、设计说明、学习计划与工程配置只读。核验：[Q053](D:/666/VE25/.runtime/sp_learning_qa/q053_validation.json)。未加载/编译/仿真模型；Delta-K route=project-only。

## Q054：errorCnt反馈计数回路的机理

- 问题：再解释截图中常量1→加法器→Delay1→动态饱和→errorCnt反馈模块的机理。
- 日期：2026-10-14（Asia/Shanghai）；关联S2跨拍连线、S3延迟/复位/计数机制；掌握程度未确认。
- **当前来源：** 截图结构对应`SignalProcessingLib/Abnormal/abnormalCounter`的errorCnt支路，`system_2115.xml`：errorCnt SID2234来自Saturation Dynamic SID2230；反馈→Sum1 SID2233（另输入为int32(1) SID2216）→Delay1 SID2223→饱和u。up=max(abnormal,0)，SID2215/2218→Max2224；lo=int32(0) SID2217。abnormal为int32计数参数，不是本拍异常boolean。
- **状态与递推：** DelayLength=1、ExternalReset=Level hold、InputPortMap=u0,r5显式保存；初值与采样时间未显式保存且无bddefaults覆盖，按标准默认0、-1继承作静态解释，未运行读取有效参数。本拍errorCnt=clamp(Delay输出,0,max(abnormal,0))；本拍候选=errorCnt+1，留给后续执行。到顶后errorCnt停在H，候选及Delay历史可能为H+1，不把输出限幅误当加法器也限幅。
- **初始化示例：** 明确abnormal=3、初值0、初始化起reset始终0且每拍执行，则errorCnt为0、1、2、3、3…，本拍候选依次1、2、3、4、4…。为静态推演，不是仿真曲线。初始化起点不能直接当成解除复位首拍。
- **复位与连续性：** r=dataValid OR NOT(valid)，由SID2213、SID2212→NOT1 SID2227→OR1 SID2229接Delay.r。valid=false或dataValid=true时复位为0；只在valid=true且dataValid=false时允许累计。dataValid来自父dataInValid SID2087，valid来自父SID2088；因此统计的是公共有效性满足期间的一段连续单项无效，而非所有无效状态都无条件累加。
- Level hold仅本拍r非零时复位，释放拍不另按下降沿复位。按标准输出后更新语义，复位拍输出0仍会形成候选1，后续释放拍可取得该1；当前未仿真验证释放首拍，不把它固定说成0。另一路恢复计数Delay SID2222的复位条件为NOT(dataValid) OR NOT(valid)，不要与本图error支路互换。
- **父层使用边界：** errorCnt→Abnormal/system_2083.xml关系块SID2110.in1，ABNORMAL经abnormalGain进in2；随后进入历史异常/恢复状态路径。SID2110比较符XML省略，未加载确认，不自动声称>=或固定触发拍号。ABNORMAL>0在SID2091明文保存；父有效性为dataInValid AND valid AND NOT isAbnormal（SID2106）。原始有效性为false不需要等待计数门限才使validOut无效；errorCnt清零也不等同直接清除全部历史异常。
- **标准块来源：** [MathWorks Delay](https://www.mathworks.com/help/simulink/slref/delay.html)，一拍历史、初值0、采样时间继承、External reset模式；[Saturation Dynamic](https://www.mathworks.com/help/simulink/slref/saturationdynamic.html)，up/lo输入的限幅规则；[Constant](https://www.mathworks.com/help/simulink/slref/constant.html)默认值1。公开资料只用于标准块语义，复位条件及实际连接以原库为准。
- 过程只读原工程；功能梳理3.5.7新增计数回路段并追加本条。旧Q021首次参数截图的唯一实例定位待确认项不因本图结构而静默关闭。验证见[Q054](D:/666/VE25/.runtime/sp_learning_qa/q054_validation.json)；未加载、编译或仿真模型，未修改模型/字典/设计说明/学习计划。库SHA-256=0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490；Delta-K route=project-only。

## Q055：更新保持Delay与异常计数Delay的区别

- 日期：2026-10-14。
- 用户问题：这一部分的delay模块功能与上一模块有何不同。
- 学习阶段：S2/S3；已解答（端口、连线及调用关系静态确认）；掌握程度未确认。
- 截图：`codex-clipboard-ff0a0443-2831-4f9d-be77-f8ca00464a92.png`。
- 对应实现：`SignalProcessingLib/Update`；对比Q054的`Abnormal/abnormalCounter`。

两处Delay均延迟一个采样，上一处保存`errorCnt + 1`这个计数候选值，本处保存最终输出数据。上一处下方输入是Level hold外部复位；本处下方`data0`是外部初始条件。端口位置相似，不代表端口功能相同。

本处`updated > 0`时，Switch直接将当前`dataIn`送到输出，因而新值在本拍生效；`updated <= 0`时选择Delay保存的前一拍输出。输出再写回Delay，于是连续不允许更新时保持最近一次接纳的数据。初始化且尚未接纳新值时，保持值为`data0`。

```text
初始化 history = data0
若 updated > 0：data = dataIn
否则：data = history
下一拍的 history = 本拍 data
```

静态示例：设`data0=0`，五拍`updated`依次为`0,1,0,0,1`，`dataIn`依次为`100,10,999,888,20`，则输出依次为`0,10,10,10,20`。这是对连线的逐拍推演，不是运行仿真取得的结果。

这里没有外部复位或使能输入。`data0`不是复位命令，初始化后仅改变它不会主动重置当前状态；保持由Switch选择历史值实现，Delay继续逐拍保存最终输出。若上层另有重新初始化，需结合调用环境另行分析。

`updated`的业务含义依调用处确定。在`AbnormalMonitor`内，`Abnormal`第一输出分支连接`validOut`及`Update.updated`，因此它使用监测后的有效性控制更新。保持旧数值与有效性输出是不同信息，旧数值被保持不代表`validOut`恢复为true。

依据：库`SignalProcessingLib.slx`的`system_1988.xml`：Delay SID2079，`DelayLength=1`、`InputPortMap=u0,p4`、`InitialConditionSource=Input port`；Switch SID2077，`Criteria=u2 > Threshold`，Threshold未显式写入XML，按标准块默认0并与截图相符。`updated` SID1989→Switch第二输入，`dataIn` SID2078→第一输入，Delay输出→第三输入；Switch输出同时送输出SID1990和Delay第一输入，`data0` SID2080接Delay第二输入。上一段Delay SID2223的端口为`u0,r5`、`ExternalReset=Level hold`。调用连线见`system_1047.xml`：SID2237输出1→SID2120输入1及SID1106输入1。

来源链接：[当前库](D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx)、[功能梳理第3.5.7节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)、MathWorks [Delay](https://www.mathworks.com/help/simulink/slref/delay.html)与[Switch](https://www.mathworks.com/help/simulink/slref/switch.html)。模型源SHA-256为`0353145198448149A44B5D72C19FA4CAB505275148C06219B414BB2C4E44F490`；[静态核验](D:/666/VE25/.runtime/sp_learning_qa/q055_validation.json)。本次未运行MATLAB、编译或仿真，未确认实际采样周期、生成代码执行顺序及上层重新初始化条件；模型保持只读。

## Q056：chUsed单独引出的含义及当前终止连接

- 日期：2026-10-16。
- 用户问题：图中这里chUsed信号单独引出了是有什么特殊含义吗。
- 学习阶段：S2/S3；已解答（当前表决模型静态确认）；掌握程度未确认。
- 截图：`codex-clipboard-bcbbb42d-08b5-4553-92b8-3d48d0640929.png`。
- 对应实现：`Calc_VotedCAS/Voter`。

`chUsed`表示本拍进入表决分支的通道组合，标量类型`uint8`。低三位分别代表第1、2、3路，组合值为0～7。权重为1、2、4：例如1代表第1路，4代表第3路，3=1+2代表第1、2路，7代表进入三路表决分支，0表示本拍没有可采用的组合。它与单个源编号、有效通道数量及数值结果的含义不同。

该编码由`select`结合有效性、同步状态及`SpCPT.CASVoterType`控制的降级条件形成。生成过程中，`valid AND isSynced`构成同步有效资格；配置允许降级时，还可能采用恰好一路原始valid成立的通道。因此不能无条件写成“将三路valid直接按位相加”。本次未加载字典或核对运行覆盖值，不据参数脚本断言当前运行模式。

Voter内部的`chUsed`一支驱动Switch Case选择参与组合对应的计算分支，另一支进入Voter第4输出。截图外层将第4输出接到Terminator，表示当前应用不继续消费该信息。voterCAS的Bus Creator只有三个字段：数值`data`、有效性`valid`及通知`reset`。chUsed未加入该总线；外层终止不改变其内部控制作用。`chUsed=0`走默认历史分支，仍可输出Delay保存的旧数值；初始化历史由`data0`提供，不能把历史数值当作本拍还有通道参与的证据。

单独提供端口使其他调用方能够获得参与组合；若接到显示或记录模块可用于观察，但当前连接没有这条处理链。未发现支持“专供故障记录”或其他特殊设计意图的直接证据，不作此断言。Terminator用于显式结束不使用的输出并避免未连接端口提示，其一般语义见MathWorks官方说明。

精确定位：[Calc_VotedCAS.slx](D:/666/VE25/sp/04_Design/Calc_VotedCAS/specification/Calc_VotedCAS.slx)，SHA-256=`807AD66E0B5C08BBB3592F75D0363879DF7B29EB20A2146373576764F12BC6D6`。`system_root.xml`：SID14输出4→Terminator SID251；SID248将SID249数值换算输出及SID14的valid/reset封装后送SID252。`system_14.xml`：SID178输出3→SID77输入1及SID247输入1，SID247类型uint8/维度1。`system_193.xml`：Bit Set SID203～205设置位0～2；SID231/232决定同步有效掩码或单路有效降级，SID206比较CASVoterType与2。

已更新[功能梳理第3.5.8节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)。[Terminator官方说明](https://www.mathworks.com/help/simulink/slref/terminator.html)；[静态核验](D:/666/VE25/.runtime/sp_learning_qa/q056_validation.json)。本次仅静态读取当前SLX；未运行MATLAB、编译、仿真或代码生成，最初保留外部chUsed端口的设计决策待确认。

## Q057：SP_SW_HLR-673传感器检查需求的模型定位

- 日期：2026-10-16。
- 用户问题：这一需求体现在模型中的哪里，帮我找一下。
- 学习阶段：S3/S4；已定位（原需求表和当前/旧模型静态核对）；整条需求符合性未判定，掌握程度未确认。
- 截图：`codex-clipboard-d38a1220-411b-4b78-8b5b-799a78c08d00.png`。
- 需求原件：[信号处理软件高级需求-SP_SW_HLR-20250722.xlsx](D:/666/VE25/sp/02_Requirements/specification/documents/信号处理软件高级需求-SP_SW_HLR-20250722.xlsx)，sheet“信号处理软件高级需求-SP_SW_HLR”，A435=`SP_SW_HLR-673`，C435为正文，G435=`SpOutMdl/NavCheck`。

当前VE25SP中的同类传感器检查逻辑位于：

```text
VE25SP
  → Model9（引用Calc_CcdlMsg_Out）
    → Calc_SysReady_LimitDispatchInfo
      → NavCheck
```

直接模型块路径是`Calc_CcdlMsg_Out/Calc_SysReady_LimitDispatchInfo/NavCheck`。其输入`Claw_Nav_Msg`为控制律使用的导航报文；本层检查的是`Valid_Status`中的有效性，不在此重新进行三路传感器原始数据比较。

| 需求中的检查内容 | 当前NavCheck中的有效性字段（均位于Valid_Status） | 分支与参数 |
|---|---|---|
| RA对地高度 | `h_AGL_Valid` | Subsystem5；`SpCPT.Enable_RaCheck` |
| FTADC气压高度、校准空速、真空速 | `h_BARO_Valid、Vel_CAS_Valid、Vel_TAS_Valid` | AND1→Subsystem2；`SpCPT.Enable_AdcCheck` |
| 水平速度、垂直速度、姿态、航向、角速度、加速度 | `Vel_XY_Valid、Vel_Z_Valid、Att_RP_Valid、Att_Y_Valid、Rot_Valid、Sfor_Valid` | AND→Subsystem1；`SpCPT.Enable_AdahruCheck` |
| 水平位置、海拔高度 | `Pos_XY_Valid、h_AMSL_Valid` | AND4→Subsystem4；`SpCPT.Enable_GpsCheck` |
| 迎角、侧滑角 | `Alpha_ADS_Valid、Beta_ADS_Valid` | AND2→Subsystem3；`SpCPT.Enable_AdcExpCheck` |

四个非RA检查分支的结果进入AND3形成`AdaCheck`；Subsystem5独立输出`RaCheck`。父层把检查不通过与相关余度损失标志合并，参与放行限制判定；最终限制结果又参与本机就绪条件。NavCheck没有一个直接命名为“传感器检查状态”的单独总输出，不能把`AdaCheck`误当作已包含RA的全体检查结果。

截图G435所列路径属于旧架构且是短写。旧[SpOutMdl.slx](D:/666/VE25/sp/04_Design/SpOutMdl/specification/SpOutMdl.slx)的完整路径为`SpOutMdl/Calc_SpDisplayMsg/Calc_SysReady_LimitDispatchInfo/NavCheck`，不是根层直接子系统。旧实现也分离GPS/ADA检查与RA输出；本次读取旧模型只用于解释该需求映射，不把旧模型作为当前VE25SP的实现权威。

当前五个检查分支含Delay、加1、动态限幅及比较结构，参数绑定到各自`Enable_*`项参与连续有效确认。需求文字把水平位置/海拔高度放在ADAHRU相关条件内，模型则由独立GPS检查参数控制该分组；需求注释“当前无有效真空速信号”仍须保留，不能据一个`Vel_TAS_Valid`接口存在推定实际真空速可用。本次不核运行覆盖值，不断言每项确认精确触发拍，也不宣布整条673完全满足或不满足。

在工程已打开、模型可解析的MATLAB中，可用以下命令进入当前检查模块：

```matlab
open_system('Calc_CcdlMsg_Out');
open_system('Calc_CcdlMsg_Out/Calc_SysReady_LimitDispatchInfo/NavCheck');
```

当前来源：[VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)根Model9 SID406，`ModelNameDialog=Calc_CcdlMsg_Out.slx`；[Calc_CcdlMsg_Out.slx](D:/666/VE25/sp/04_Design/Calc_CcdlMsg_Out/specification/Calc_CcdlMsg_Out.slx)根子系统SID4，`system_4.xml`的NavCheck SID102，`system_102.xml`的Bus Selector SID109～113、参数SID114～118、分支SID119/134/149/164/179及输出SID194/195；AND3 SID107合并非RA四路结果。旧来源SID链为5109/4680/1091，系统XML对应`system_root→system_5109→system_4680→system_1091`。

原模型、需求表和父子层级由当前读取核对；需求表及旧路径另经只读独立核查。[本轮核验](D:/666/VE25/.runtime/sp_learning_qa/q057_validation.json)。未加载、编译、仿真、生成代码或修改工程模型。[功能梳理中的定位入口](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)已同步补充。

## Q058：紫色区域中各模块在SP处理链中的定位

- 日期：2026-10-16。
- 用户问题：图中紫色区域中的模块定位是什么。
- 学习阶段：S1/S4；已解答（架构职责归纳及接口连接静态确认）；掌握程度未确认。
- 截图：`codex-clipboard-633c7a2a-796a-4585-931f-0148466e2289.png`。
- 对应实现：[VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)右侧Area“计算SP分区输出信号”。

本问题关注框中模块承担什么职责；Q049说明颜色/区域对象，两者适用范围不同。按数据流可将这一组理解为SP后段的输出生成和综合状态判定部分：承接左侧整理好的`NavData、DevData、MpioData、GcsData、SysInfoData、PilotInData`，为下游构造接口，并计算系统层面的检查、就绪及放行限制信息。该称谓是分析归纳，不将Area解释成独立运行分区。

| 模块 | 作用 | 实际接口/使用 |
|---|---|---|
| `Calc_PilotIn_ToClaw` | 形成控制律操纵数据及空地标志 | `PilotData→Cockpit_In`；`PilotIn/RC`进入CCDL和显示；`Air2Gnd/Air2GndValid`进入导航接口模型 |
| `Calc_SYSMsg_ToClaw` | 组合设备反馈、MPIO信息与SysReady | `Claw_SYS_Msg→SYSInfo_In` |
| `Calc_NAVMsg_ToClaw` | 根据NavData及空地标志组织控制律导航报文 | `Claw_Nav_Msg→NAV_Root`及后续检查；`FtadcLog`当前接Terminator |
| `Calc_FMSMsg_ToClaw` | 组织供控制律使用的飞管目标/请求接口 | `Claw_FMS_In_Msg→FMS_In` |
| `Calc_CcdlMsg_Out` | 跨通道信息组织及系统就绪/放行限制判定 | `SpCcdlOut→SpCcdlMsgOut`；`SysReady`进入系统接口和显示；`LimitDispatch`进入显示 |
| `Calc_SPDispMsg_Out` | 组织显示所需的就绪、限制、操纵与MPIO信息 | `SpDispMsg→SPDispMsgOut` |
| 根层`SP2DRMS`组包 | 向设备余度管理反馈导航有效性、备用FCC激活相关状态 | `navValid、isBfccActv、isBfccActvValid`写入SP2DRMS |

ToClaw在当前接口语境中表示面向控制律，`FMS_In`也按控制律消费者的输入理解，不据此推定向外部飞管设备发送控制命令。根输出由模型连接确认，实际通信硬件和分区调度不由颜色决定。

这一区域还包含实质判定。例如当前选中的`Calc_CcdlMsg_Out`，其`Calc_SysReady_LimitDispatchInfo/NavCheck`检查控制律导航报文的有效性，形成`AdaCheck/RaCheck`后供父层使用；它与左侧原始输入的预处理、余度选源处于不同处理位置。详见Q057。模块按连线配合，画布上下排列不代表运行顺序。

主源锚点：VE25SP根`system_root.xml`中Area SID386/标题SID387，六个模型引用SID376/377/375/397/406/408；根输出SID21/24/22/23/425/424及SP2DRMS SID458。`Calc_PilotIn_ToClaw`的空地输出经Goto400/401与From402/403进入SID375；导航输出经Goto432与From433进入SID406；`SysReady`经Goto420/From427进入SID377，经From421进入SID408；限制经Goto422/From423进入SID408。NAV的FtadcLog输出进入Terminator SID426。

底部Bus Assignment SID446写入`navValid、isBfccActv、isBfccActvValid`，来源分别为导航报文的`Valid_Status`以及操纵/地面站的备用FCC激活相关数据。本次确认其接口职责和根层来源，GPIO对应的物理采集设备不在本轮确认范围。

核对方式：root静态读取六个引用模型的根输入输出，复用已核NavCheck当前源证据；独立只读复核根层六个模型输出的实际消费者与SP2DRMS组装。来源指纹与范围见[Q058核验](D:/666/VE25/.runtime/sp_learning_qa/q058_validation.json)。已补充[功能梳理第2.3节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)。未运行MATLAB、编译、仿真或代码生成；模型及独立设计文档未修改。

## Q059：From[max]信号路由与所在连续有效确认回路

- 日期：2026-10-16。
- 用户问题：图中这一模块的作用是什么。
- 学习阶段：S2/S3；已解答（From/Goto连线静态确认，比较参数已在Q060补证）；掌握程度未确认。
- 截图：`codex-clipboard-eba0594e-b538-423f-bda0-235ca0e20be3.png`。
- 问题对象：截图蓝色选中的`[max]`为From块；周围整段是NavCheck的连续有效确认回路，二者职责分别解释。

上方Goto[max]接收`max`输入经uint32转换后的门限。选中的From[max]读取该标签的值，送给动态限幅块上限；右下方From1[max]读取同一值，送给比较器门限。From/Goto提供等效连线，不在此计算门限、累计计数或保存跨拍历史。两处使用相同来源，使限幅与通过判定使用一致门限。图标方括号按标准块约定表示局部标签。

所在回路的用途是让本组有效条件持续成立后再输出检查通过。按截图的`>=`，定义转换后的门限为`M`、限幅输出为`c`：`res=(c>=M)`。`valid=false`时，NOT驱动Delay复位，`c=0`；有效持续时，反馈加1经一拍延迟推进计数，并限幅到`0～M`。到达门限后，只要有效持续就保持通过；再次无效时，对于`M>0`结果取消并重新累计。

必须区分Level与Level hold。本实例Delay明确`ExternalReset=Level`，复位既在`NOT(valid)`当前为真时发生，也在该复位信号由真变假时发生。因此设`M=3`不变、此前无效且复位已建立，恢复有效后按标准离散Delay语义推演，计数`0,1,2,3,3…`对应`res=false,false,false,true,true…`。初始化即持续有效时首拍同样先使用初值0。例子是静态推演，不是本次运行结果，不将M=3简单写成第三个有效样本通过。

零门限边界同样按截图`>=`：`M=0`时限幅输出为0，`res=true`，包含valid为假时。这里没有valid与res的额外AND门；父层Enable_*名称中的使能实际接入确认计数门限，应依据该结构解释。具体父层五分组与参数绑定见Q057。

精确来源：[Calc_CcdlMsg_Out.slx](D:/666/VE25/sp/04_Design/Calc_CcdlMsg_Out/specification/Calc_CcdlMsg_Out.slx)，SHA-256=`A7FBAC6DFFE00A2EB2B838E0F65B9312D04260E5820BD39CE02A3CA698B40DF8`。`system_119.xml`中SID124→SID128(Goto)；SID126/127(From)标签同为max，分别→SID131输入1及SID130输入2；NOT SID129→Delay SID125复位输入；SID131→SID132→SID125→SID131形成反馈。五个检查子系统119/134/149/164/179的Delay均为len1、初值uint32(0)、ExternalReset Level。

来源边界与Q060后续补证：初次以截图≥解释，原SLX各实例未显式保存Operator，会话连接返回failed to attach to MATLAB session，没有取得运行回读。Q060随后查到R2023b原生参数读取记录，并核对采集脚本、源基线和副本与当前原SLX字节一致，确认NavCheck五个比较器Operator均为>=。因此保存模型的比较参数已有同源证据，初次会话失败仍为历史事实，当前运行覆盖仍未核。未从省略字段或新版文档默认值推定该实例；证据见[Q060](D:/666/VE25/.runtime/sp_learning_qa/q060_validation.json)。

标准语义引用：MathWorks [From](https://www.mathworks.com/help/simulink/slref/from.html)、[Goto](https://www.mathworks.com/help/simulink/slref/goto.html)、[Delay](https://www.mathworks.com/help/simulink/slref/delay.html)。独立只读复核检查了原SLX参数、比较符保存边界及Level释放沿。已同步[功能梳理3.5.7节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)；[Q059核验](D:/666/VE25/.runtime/sp_learning_qa/q059_validation.json)。未加载或仿真模型、未编译或生成代码，工程模型保持只读。

## Q060：紫色区域模型的功能、输入处理与输出意义集中汇总

- 日期：2026-10-16。
- 用户问题摘要：这些模型的功能、输入处理方法和输出信号现实意义是否已经汇总到已有Doc中。
- 学习阶段：S3/S4；已汇总（六个引用模型及根层SP2DRMS静态核对）；掌握程度未确认。
- 本轮判断：已有功能定位与部分详细机制，但按模型查阅的处理链不够集中；补充说明，不修改工程算法或独立设计文档。

修改前，功能梳理2.3/3.2/3.3已有区域职责、模型及根输出概览，3.6详讲handleGCS，2.5包含SYS分层；独立设计说明5～7章按数值转换、事件/指令和系统判定展开。不能说“此前完全没整理”，但这些位置没有把紫色区域每个模型的输入对象、处理条件、无效/默认/历史行为和输出含义放在同一模型条目下。

本轮在[功能梳理](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)新增`3.8`，覆盖：`Calc_PilotIn_ToClaw、Calc_NAVMsg_ToClaw、Calc_SYSMsg_ToClaw、Calc_FMSMsg_ToClaw、Calc_CcdlMsg_Out、Calc_SPDispMsg_Out`以及根层SP2DRMS。2.3增加阅读入口，保留3.6详解及Bus完整字段说明的分工。每个条目均给出功能/输入、主要处理和条件、默认或保持行为、输出用途及原始模型锚点。

关键处理差异已写清：

- 操纵：两份候选分别适配，按配置选择；区分轴角度、有效性、开关保持、事件及请求，空地开关不等于落地检测。
- 导航：角度/压力/比力转换、垂速补充来源、FTADC计算、各分项覆盖规则及当前保存配置；数值分支与有效性分别说明，CAS不能一概写成失效即回退。
- SYS：高压有效判据、可用功率保持、11台电机重组、8路作动器反算及分支内保持；反馈与目标指令区分。
- FMS：目标/请求直接适配，输出目标有效标志来自独立data字段；质量和重心无效保持及参数初值。
- CCDL：本机原因优先级及本机就绪报告，与独立的三机SysReady/LimitDispatch汇总分别解释。
- 显示：候选操纵位置、成组有效码、当前来源、就绪/限制及高压开关默认，不将无效位置自动写成0。
- DRMS：导航有效性集合、备用FCC激活相关输入优先选择及有效性OR，不补造硬件执行确认或外部接线。

三份独立原SLX核查报告检查了上述分工和条件，候选反向复核修正了两处表述：气压高度的数值分支仅按压力门限选择公式/11000，静压valid只影响有效性；作动器保持限定在正常计算分支内Position.valid=false，小增益分支实际输出0。CP/FO/CS中文术语从原需求C347/349/351/353/355/357补证，仅用于名称对照，不以旧公式取代当前实现。

另补齐Q059参数证据：既有R2023b原生读取记录来自model-copies副本，六个副本与当前原SLX字节指纹一致，源基线及采集脚本已核。该记录确认NavCheck及三机确认的八个关系块Operator为`>=`，NAV差压SID255也为`>=`。Q059初次会话连接失败仍保留为历史事实；本次复用同源读取证据，不声称当前运行会话已回读或仿真已通过。

[本轮核验与覆盖](D:/666/VE25/.runtime/sp_learning_qa/q060_validation.json)；[操纵核查](D:/666/VE25/.runtime/sp_learning_qa/q060_pilot_review.md)、[NAV/FMS核查](D:/666/VE25/.runtime/sp_learning_qa/q060_nav_fms_review.md)、[SYS/显示核查](D:/666/VE25/.runtime/sp_learning_qa/q060_sys_display_review.md)。主要来源是六个当前引用SLX、VE25SP根层、活动SignalProcessingLib、SP_Step_New/SP_Parameters/SP_Enum及相关字典链；来源指纹集中在核验文件。

修改前两份学习文档已按字节保存在`.runtime/sp_learning_qa/q060_baseline/`。本轮只更新功能梳理、此问答记录及各自过程文件，未改工程模型、字典、参数、需求、独立设计说明或控制台账；未加载、编译、仿真或生成代码。FMS未填量纲、部分模式缩写、VCU完整设备定义及外部GPIO接线保留待确认，不补造成已确认事实。

## Q061：HiVltgSW.data与HiVltgSW.valid的含义

- 日期：2026-10-16。
- 用户问题：这两条信号表示什么。
- 学习阶段：S2/S3；已解答（原模型与FICD码表确认）；掌握程度未确认。
- 截图：`codex-clipboard-1a9eff08-9c9d-493b-b85d-e0c8a84a28d4.png`。

两项是同一份高压开关记录的不同成员。`HiVltgSW.data`表示操作请求码：0 Unknown、1 OFF、2 ON。`HiVltgSW.valid`表示该记录的有效性，当前来自MPIO输入的整体Valid直接包装，不是高压实际接通确认。实际高压状态另由HiVltgSts提供。

| 组合 | 含义及当前消费 |
|---|---|
| data=2，valid=true | 有效ON/上高压请求；PwrOn=true，PwrOnVld=true |
| data=1，valid=true | 有效OFF/下高压请求；PwrOn=false，PwrOnVld=true |
| data=2，valid=false | 数值仍为ON码，但请求记录无效；PwrOn值仍可true，配对PwrOnVld=false |
| data=0，valid=true | 包装记录有效但开关码未知；后级排除0，PwrOnVld=false |

当前handleGCS/handlePilotIn均按`PwrOn_PltIn_b=(data==2)`、`PwrOnVld_PltIn_b=valid AND(data!=0)`计算；此级没有进一步把data限制到1/2。值和有效性分开输出，不将valid=false当成OFF，也不将valid=true或data=ON当成设备已执行成功。

模型源：[Calc_MPIOData.slx](D:/666/VE25/sp/04_Design/Calc_MPIOData/specification/Calc_MPIOData.slx)根SID5的输出1 Valid和输出13 HiVltgSW，经Goto6/114、From101/115填入Bus Creator33的valid/data；reset由常量43给false，记录写入BusAssignment120。下游[Calc_PilotIn_ToClaw.slx](D:/666/VE25/sp/04_Design/Calc_PilotIn_ToClaw/specification/Calc_PilotIn_ToClaw.slx)中system_108的BusSelector120分别送比较259（==2）和260（!=0），valid与后者经AND111形成请求有效性；本地system_552有同样结构SID563/675/676/555。

协议源：V1.4附件01-01“飞控计算机&显控系统-ARINC825.xlsx”，sheet“04-显控相关参数”，G15=HiVltgSW、Q15=enum、R15为上述码表，S15为空。协议只直接定义data码值，软件valid的含义及来源由原始模型补证。当前协议SHA-256为`864D7977DBBDFEE9E45CDE4BF4C5116D1489DB7E8CEDE3B3A9FE4E877E3AB01E`，两模型指纹及来源路径见[Q061核验](D:/666/VE25/.runtime/sp_learning_qa/q061_validation.json)。

已补入[功能梳理3.6.2节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)。仅静态检查，未运行模型或仿真，未修改工程源或独立设计说明；开关请求的外部执行和状态确认过程不在本次验证范围。

## Q062：Baro的含义

- 日期：2026-10-16。
- 用户问题：baro表示什么意思。
- 学习阶段：S1；已解答（术语及当前字段确认）；掌握程度未确认。

`Baro/BARO`是`barometric`的常用缩写，表示气压相关。在当前SP导航模型里主要指气压高度，即依据静压及大气关系换算的高度。`h_BARO_Valid`表示气压高度数据是否有效。若名称带Corrected，还要按气压修正高度及其实现理解，不能把所有Baro名称当作同一高度，也不能直接当作RA对地高度。

当前原始来源：[Calc_CcdlMsg_Out.slx](D:/666/VE25/sp/04_Design/Calc_CcdlMsg_Out/specification/Calc_CcdlMsg_Out.slx)的NavCheck/system_102.xml，BusSelector SID111选取`Valid_Status.h_BARO_Valid`；[Calc_NAVMsg_ToClaw.slx](D:/666/VE25/sp/04_Design/Calc_NAVMsg_ToClaw/specification/Calc_NAVMsg_ToClaw.slx)的system_279.xml，活动Fcn SID286按静压计算气压高度，另一高空公式SID287为注释块。本次仅用该源确认术语所指物理量，不展开高空分段、QNH设定或导航误差结论。

已在[功能梳理3.5.6节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)将Baro单独列为术语。[静态核验Q062](D:/666/VE25/.runtime/sp_learning_qa/q062_validation.json)。仅静态读取，未运行或修改工程模型。

## Q063：SP与CCDL的缩写及当前名称拆解

- 日期：2026-10-16。
- 用户问题摘要：模型中的sp ccd可能代指什么，是哪一个名词的缩写。
- 学习阶段：S1；已解释（按当前CCDL模型命名语境）；掌握程度未确认。

结合上一轮紫色区域的`Calc_CcdlMsg_Out`及顶层`SpCcdlMsgOut`，用户所说sp ccd按`SP CCDL`语境解释；实际模型名称含末尾L。SP=Signal Processing，信号处理。CCDL的行业展开为Cross Channel Data Link，交叉通道数据链；Msg=Message，报文；Out=Output，输出。因未给出另一完整字段，不把孤立CCD写法另行确定成项目专有缩写。

`SpCcdlMsgOut`表示SP提供给通道交换接口的输出报文。在当前模型里，Calc_CcdlMsg_Out组织本机系统就绪和限制派遣信息等结果，顶层将其SpCcdlOut接到SpCcdlMsgOut。它提供交换用的软件报文，不能据名称认定该模型包含完整通信硬件发送过程。独立SysReady/LimitDispatch另有汇总路径，见功能梳理3.8.5，不将它们与本机报文字段混同。

当前原始依据：[VE25SP.slx](D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx)根Model9 SID406引用Calc_CcdlMsg_Out，输出SpCcdlMsgOut为SID425；[Calc_CcdlMsg_Out.slx](D:/666/VE25/sp/04_Design/Calc_CcdlMsg_Out/specification/Calc_CcdlMsg_Out.slx)根SpCcdlOut为SID279，system_4的BusAssignment18填写Data.LimitDispathInfo/Data.SysReady。

英文行业展开由[Curtiss-Wright原始产品说明](https://defense-solutions.curtisswright.com/products/flight-test/data-acquisition/ttcmdas/bus/mcdl202a1)支持；该外部来源只用于CCDL术语，不证明VE25采用该产品、速率或接口协议。已在[功能梳理3.1节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)补名称拆解；[静态核验Q063](D:/666/VE25/.runtime/sp_learning_qa/q063_validation.json)。未运行或修改工程模型。

## Q064：当前SP模型中故障相关处理的模块分布

- 日期：2026-10-16。
- 用户问题：当前sp模型中关于故障信号处理的内容在哪些模块中有所体现；后续要求继续。
- 学习阶段：S3/S4；已定位（当前18模型引用链及关键连接静态核对）；掌握程度未确认。

故障相关处理分布于前级信号质量、设备数据/状态码、故障后的数据使用、余度汇总和系统级判定。主要入口为导航AdahruMsgsPreprocess/RAMsgsPreprocess及顶层内联CAS比较，设备Calc_VotedFTADC/Calc_MCUSCUData/Calc_MPIOData，操纵Calc_PilotInData/Calc_PilotIn_ToClaw，系统Calc_SysInfo与Calc_CcdlMsg_Out，以及NAV/SYS/显示等输出适配。完整模块表、具体入口及并行传递关系已新增到[功能梳理第3.9节](D:/666/VE25/Doc/SP工作区模型主要功能梳理_20260908.md)。

关键区别：

- CmpMonitor/Calc_SignalState处理一致性、失配和禁用状态；AbnormalMonitor配合有效性监控与data保持；这类状态不能单独确诊硬件故障。
- MCU故障等级有设备上报来源，SCU分支另根据有效性及上报状态组合FaultStatus/ClawFaultStatus；不能把全部设备码都说成由数值比较产生。
- VdtCBIT来自指定四轴原始valid，不等于完整硬件CBIT或三路数值比较结果；操纵接口还另行生成Enable_PLTLost_b。
- SysInfo按FCC存在性、上报设备状态位及VdtCBIT作配置筛选，形成LossRedundancy；根输入没有DevData，不是所有设备故障码先统一进入该模型。
- CCDL模型既直接用部分设备FaultLevel，也用余度、导航、操纵和预检条件；本机报告与独立三机汇总SysReady/LimitDispatch仍须分别理解。
- 当前MCU四个、MPIO三个、FTADC五个AbnormalMonitor的isAbnormal引出端均终止，但data/valid处理仍有实际作用，不能据此认定已经形成统一对外报警或全部监控无效。

主要原始定位：SignalProcessingLib根Calc_SignalState SID2421/2563→内CmpMonitor2477/2605与SignalStateSet_New2541/2582；设备监控位置为FTADC根67/72/80/94/102、MCU system_9的229/230及system_252的254/255、MPIO根73/76/80；SCU system_118；PilotInData比较1267/1281、Voter1428/1495、CBIT system_1532；SysInfo位检查6/38/46/55/63/71/79/87/95、VDT104、三机消息处理115；CCDL的NavCheck102及其父层。顶层内联CAS比较是VE25SP的system_96 SID104，当前仍是实际引用。

NavDataState根输出从raVoter.raState导出，RA模块还接收上游NavDataState，应按保留/覆写理解；不把它叫全模型统一故障集合。NavLog标签节点368已注释，FtadcLog根路径接Terminator426；本次没有核查其他日志配置或生成代码保留情况，不作“完全没有故障记录”的结论。

来源与当前指纹见[Q064模块地图](D:/666/VE25/.runtime/sp_learning_qa/q064_source_map.json)及[核验](D:/666/VE25/.runtime/sp_learning_qa/q064_validation.json)。本轮尝试的三个独立只读复核通道均未完成，改由本对话直接读取原始模型、跟踪所列关键连接并反向核对候选；同源的既有详细机制按指纹复用。核查范围是模块定位和主要传递关系，不是全部故障算法、故障覆盖率或需求符合性的验收。未运行或修改模型，未改独立设计说明或控制台账；过程产物仅在本任务.runtime目录。
