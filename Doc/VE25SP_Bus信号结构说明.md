# VE25SP Bus 信号结构说明

> 依据当前模型和数据字典的静态定义整理。适用入口：VE25SP；快照日期：2026-09-23。
> 工程提交：`a895a43a19209c076e679fcc2e2f8953e9f49bb4`，分支 `AC102_Rebuild`。具体来源以文末文件 SHA-256 为准。

<a id="scope"></a>

## 1. 阅读方式与覆盖范围

本文先按信号链路说明 Bus 在哪里进入、经过哪个模型、从哪里输出，再给出完整、有序的字段定义。点击类型名可跳到相应结构；嵌套 Bus 不展开为重复的扁平字段。

| 范围 | 本次内容 |
|---|---|
| 主模型接口 | 20 个输入、9 个输出，全部显式声明 Bus 类型 |
| 模型层级 | VE25SP 及递归 ModelReference 共 18 个模型；156 个保存的 System |
| 字典 | 各模型 DataDictionary 的递归闭包共 27 份；其中共有 85 个唯一 Bus 名称、1430 个直接字段定义 |
| 当前引用 | 61 个 Bus 在活动模型/实际链接库的类型声明、ModelReference 保存接口或其嵌套成员中出现；共 611 个直接字段 |
| 字典可见的其他定义 | 24 个 Bus 未在上述当前引用链中定位到使用；单独列出，仍提供完整字段 |
| 字段属性 | 名称、顺序、原始类型声明、Dimensions、DimensionsMode、Complexity、DocUnits、Description、Min/Max |
| 验证边界 | 静态 ZIP/XML 提取与交叉核对；未加载、编译或仿真模型，未读取运行时 CompiledPortDimensions/CompiledPortDataTypes |

**维度的两个层次：**端口 `PortDimensions=1` 表示该端口的声明维度；Bus 某个成员 `Dimensions=3` 表示该成员有 3 个元素。字段本身是 Bus 时，就是该嵌套 Bus 的数组。`3` 不能直接称为“三维空间坐标”或“三维矩阵”。原始 `1.0/3.0` 等在字段表显示为等值整数 `1/3`；不改变数组层级。

**原文优先：**保留 `SpIn _McuInfo` 中的空格、`SpIn_Sitck` 等源拼写。字典未填的单位、描述和限值记为 `—`，不根据名称补造物理意义。端口未保存维度时记为“未显式存储”，不当作已编译确认的标量。文中的分组标题用于阅读导航，不替代类型原名。

**引用与使用的区别：**字典中存在一个 Bus，不代表当前算法实际使用它。字段定义也不证明每个字段都被读取或写入；Bus Assignment 的实际写入范围见第 6 节。ModelReference 的保存接口属于静态元数据，本次已和被引用模型的端口声明核对，不当作新的编译结果。

目录：

- [2. 总体链路与外部输入](#overview)
- [3. 分层模型接口](#model-hierarchy)
- [4. 对外输出](#outputs)
- [5. 完整 Bus 字段定义](#catalog)
- [6. 模型内 Bus 构造与赋值位置](#bus-operations)
- [7. 枚举定义](#enums)
- [8. 数据字典与文件来源](#sources)
- [9. 核验与使用限制](#verification)

<a id="overview"></a>

## 2. 总体链路与外部输入

以下只表示结构传递和组包关系；模块内部的有效性判断、表决条件和执行时序不在本文展开。

```text
VE25SP
├─ SpIn_ADAHRS / SpIn_RA / SpIn_FTADC / RmsPublicMsg
│  └─ NavSpApp：预处理 → 来源选择/表决 → 导航数据与状态
│     ├─ SpIn_Nav2Display、NavDataState
│     └─ Calc_NAVMsg_ToClaw → NAV_Root
├─ SpIn _McuInfo / SpIn_ScuInfo → Calc_MCUSCUData → DevData
├─ SpIn_MpioInfo → Calc_MPIOData → MpioData
├─ SpIn_Gcs_Curr/Left/Right → Calc_GCSData → GcsData
├─ VDT_Stick_Curr/Left/Right / Dis_Key_Curr/Left/Right
│  └─ Calc_PilotInData → PilotInData
├─ Sys_Status_Curr/Left/Right / SpIn_SysInfo_Curr
│  └─ Calc_SysInfo → SysInfoData
└─ 以上业务数据进入控制律接口、显示、CCDL及DRMS组包
   ├─ Calc_PilotIn_ToClaw → Cockpit_In
   ├─ Calc_SYSMsg_ToClaw → SYSInfo_In
   ├─ Calc_FMSMsg_ToClaw → FMS_In
   ├─ Calc_SPDispMsg_Out → SPDispMsgOut
   ├─ Calc_CcdlMsg_Out → SpCcdlMsgOut
   └─ 根层 Bus Assignment → SP2DRMS
```

这张图省略交叉输入，例如 RmsPublicMsg 还向其他处理提供公共信息，MpioData/DevData/PilotInData 等有多个消费者。完整模型接口列在第 3 节，主层直接连接列在下方。

### 2.1 输入端口

| 端口 | 模型原始名称 | 类型 | 端口声明维度 | SID |
|---|---|---|---|---|
| 1 | SpIn_ADAHRS | [Bus: SpIn_Adahrus](#bus-spin_adahrus) | 1 | 4 |
| 2 | SpIn_RA | [Bus: SpIn_Ra](#bus-spin_ra) | 1 | 5 |
| 3 | SpIn _McuInfo | [Bus: SpIn_McuInfos](#bus-spin_mcuinfos) | 1 | 13 |
| 4 | SpIn_ScuInfo | [Bus: SpIn_ScuInfos](#bus-spin_scuinfos) | 1 | 14 |
| 5 | SpIn_MpioInfo | [Bus: SpIn_Mpio](#bus-spin_mpio) | 1 | 15 |
| 6 | Sys_Status_Curr | [Bus: SpIn_CcdlMsg](#bus-spin_ccdlmsg) | 1 | 16 |
| 7 | Sys_Status_Left | [Bus: SpIn_CcdlMsg](#bus-spin_ccdlmsg) | 1 | 17 |
| 8 | Sys_Status_Right | [Bus: SpIn_CcdlMsg](#bus-spin_ccdlmsg) | 1 | 18 |
| 9 | SpIn_SysInfo_Curr | [Bus: SpIn_SysInfos](#bus-spin_sysinfos) | 1 | 19 |
| 10 | RmsPublicMsg | [Bus: BUS_DRMS2SP_MSG](#bus-bus_drms2sp_msg) | 1 | 68 |
| 11 | SpIn_FTADC | [Bus: SpIn_ftadc](#bus-spin_ftadc) | 未显式存储 | 359 |
| 12 | SpIn_Gcs_Curr | [Bus: SpIn_GcsMsg](#bus-spin_gcsmsg) | 1 | 371 |
| 13 | SpIn_Gcs_Left | [Bus: SpIn_GcsMsg](#bus-spin_gcsmsg) | 1 | 372 |
| 14 | SpIn_Gcs_Right | [Bus: SpIn_GcsMsg](#bus-spin_gcsmsg) | 1 | 373 |
| 15 | VDT_Stick_Curr | [Bus: SpIn_Sitck](#bus-spin_sitck) | 1 | 6 |
| 16 | VDT_Stick_Left | [Bus: SpIn_Sitck](#bus-spin_sitck) | 1 | 7 |
| 17 | VDT_Stick_Right | [Bus: SpIn_Sitck](#bus-spin_sitck) | 1 | 8 |
| 18 | Dis_Key_Curr | [Bus: SpIn_Key](#bus-spin_key) | 1 | 10 |
| 19 | Dis_Key_Left | [Bus: SpIn_Key](#bus-spin_key) | 1 | 11 |
| 20 | Dis_Key_Right | [Bus: SpIn_Key](#bus-spin_key) | 1 | 12 |

### 2.2 容易混淆的数组层级

| 输入结构 | 成员结构 | 如何阅读 |
|---|---|---|
| [SpIn_Adahrus](#bus-spin_adahrus) | 31 个字段，各字段 Dimensions=3 | 每个字段是 3 个 [SpData_Single](#bus-spdata_single) 或 [SpData_Double](#bus-spdata_double)；具体名称和顺序见定义 |
| [SpIn_Ra](#bus-spin_ra) | L164_Radio_Altitude：[SpData_Single](#bus-spdata_single)，Dimensions=2 | 一个顶层 RA Bus，内部这个成员含 2 份记录 |
| [SpIn_McuInfos](#bus-spin_mcuinfos) | MCU：[Sp_McuInfo](#bus-sp_mcuinfo)，Dimensions=11 | 11 个设备记录，每个记录另有 7 个字段；字段数与设备数属于不同层次 |
| [SpIn_ScuInfos](#bus-spin_scuinfos) | SCU：[Sp_ScuInfo](#bus-sp_scuinfo)，Dimensions=8 | 8 个设备记录，每个记录另有 5 个字段 |
| [SpIn_Mpio](#bus-spin_mpio) | 31 个直接字段，成员均 Dimensions=1 | 多个独立字段组成一个 Bus；不是 31 台设备 |
| [Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 成员标量定义；使用该类型的端口另有维度 | RAMsgsPreprocess 的输出与 Calc_VotedRA 的对应输入声明 Dimensions=3，成员标量不意味着外层 Bus 数组只有1个 |

例如 MCU 的字段路径可读作 `SpIn _McuInfo.MCU(i).McuSpdFbk`，`i` 属于设备数组层。Bus 中成员维度与模型内 Selector / For Each 的分区共同决定每个实例处理的数据；本表不据此推断硬件通道编号或设备安装位置。

### 2.3 主模型直接连接

下表由根层 Line/Branch 提取；同系统的 From/Goto 已按标签连接。`inN/outN` 表示保存的端口编号；本表不把普通处理块解释成透明传输。

| 来源 | 消费者／输出 | 保存的线名 |
|---|---|---|
| SpIn_ADAHRS | NavSpApp.in1 | — |
| SpIn_RA | NavSpApp.in2 | — |
| RmsPublicMsg | NavSpApp.in3 | — |
| SpIn_FTADC | NavSpApp.in4 | — |
| SpIn_MpioInfo | Calc_MPIOData.SpIn_MpioInfo | — |
| SpIn _McuInfo | Calc_MCUSCUData.SpIn _McuInfo | — |
| SpIn_ScuInfo | Calc_MCUSCUData.SpIn _ScuInfo | — |
| NavSpApp.out1 | SpIn_Nav2Display | — |
| NavSpApp.out2 | NavDataState | — |
| SpIn_Gcs_Curr | Calc_GCSData.SpIn_Gcs_Curr | — |
| SpIn_Gcs_Left | Calc_GCSData.SpIn_Gcs_Left | — |
| SpIn_Gcs_Right | Calc_GCSData.SpIn_Gcs_Right | — |
| RmsPublicMsg | Calc_GCSData.Rms2PublicMsg | — |
| Calc_GCSData.GcsData | Calc_PilotIn_ToClaw.GcsData | — |
| Calc_MPIOData.MpioData | Calc_SYSMsg_ToClaw.MpioData | — |
| Calc_MCUSCUData.DevData | Calc_SYSMsg_ToClaw.DevData | — |
| Calc_PilotInData.PilotInData | Calc_PilotIn_ToClaw.PilotInData | — |
| Calc_MPIOData.MpioData | Calc_PilotIn_ToClaw.MpioData | — |
| Sys_Status_Curr | Calc_SysInfo.SpIn_CCDL_Curr | — |
| VDT_Stick_Curr | Calc_PilotInData.VDT_Stick_Curr | — |
| VDT_Stick_Left | Calc_PilotInData.VDT_Stick_Left | — |
| VDT_Stick_Right | Calc_PilotInData.VDT_Stick_Right | — |
| Dis_Key_Curr | Calc_PilotInData.Dis_Key_Curr | — |
| Dis_Key_Left | Calc_PilotInData.Dis_Key_Left | — |
| Dis_Key_Right | Calc_PilotInData.Dis_Key_Right | — |
| Calc_SYSMsg_ToClaw.Claw_SYS_Msg | SYSInfo_In | — |
| Bus Selector.out1 | Calc_SysInfo.isFccExist | &lt;isFccExist&gt; |
| Calc_PilotInData.VdtCBIT | Calc_SysInfo.VdtCBIT | — |
| Calc_PilotIn_ToClaw.PilotData | Cockpit_In | — |
| Calc_MPIOData.MpioData | Calc_FMSMsg_ToClaw.MpioData | — |
| Calc_FMSMsg_ToClaw.Claw_FMS_In_Msg | FMS_In | — |
| NavSpApp.out1 | Calc_NAVMsg_ToClaw.NavData | — |
| Calc_PilotIn_ToClaw.Air2Gnd | Calc_NAVMsg_ToClaw.Air2Gnd | — |
| Calc_PilotIn_ToClaw.Air2GndValid | Calc_NAVMsg_ToClaw.Air2GndValid | — |
| Calc_NAVMsg_ToClaw.Claw_Nav_Msg | NAV_Root | — |
| Sys_Status_Left | Calc_SysInfo.SpIn_CCDL_Left | — |
| Sys_Status_Right | Calc_SysInfo.SpIn_CCDL_Right | — |
| SpIn_SysInfo_Curr | Calc_SysInfo.SpIn_SysInfo_Curr | — |
| Calc_SysInfo.SysInfoData | Calc_CcdlMsg_Out.SysInfoData | — |
| Calc_MPIOData.MpioData | Calc_CcdlMsg_Out.MPIOData | — |
| Calc_MPIOData.MpioData | Calc_SPDispMsg_Out.MPIOData | — |
| Calc_PilotIn_ToClaw.PilotIn | Calc_SPDispMsg_Out.PilotIn | — |
| Calc_PilotIn_ToClaw.PilotIn | Calc_CcdlMsg_Out.PilotIn | — |
| Calc_PilotIn_ToClaw.RC | Calc_SPDispMsg_Out.RC | — |
| Calc_PilotIn_ToClaw.RC | Calc_CcdlMsg_Out.RC | — |
| Calc_PilotIn_ToClaw.PilotData | Calc_CcdlMsg_Out.Cockpit_In | — |
| Calc_PilotIn_ToClaw.PilotData | Calc_SPDispMsg_Out.Cockpit_In | — |
| Calc_CcdlMsg_Out.SysReady | Calc_SPDispMsg_Out.SysReady | — |
| Calc_CcdlMsg_Out.LimitDispatch | Calc_SPDispMsg_Out.LimitDispatchInfo | — |
| Calc_SPDispMsg_Out.SpDispMsg | SPDispMsgOut | — |
| Calc_CcdlMsg_Out.SpCcdlOut | SpCcdlMsgOut | — |
| Calc_CcdlMsg_Out.SysReady | Calc_SYSMsg_ToClaw.SysReady | — |
| Calc_MCUSCUData.DevData | Calc_CcdlMsg_Out.DevData | — |
| Calc_NAVMsg_ToClaw.Claw_Nav_Msg | Calc_CcdlMsg_Out.Claw_Nav_Msg | — |
| Bus Assignment.out1 | SP2DRMS | — |

### 2.4 NavSpApp 内部连接

| 来源 | 消费者／输出 | 保存的线名 |
|---|---|---|
| AdahruMsgsPreprocess.adahruNormalInfos | Subsystem.in1 | adahruNormalInfos |
| AdahruMsgsPreprocess.adahruNormalInfos | Calc_NavSourceSelection.adahruNormalInfos | adahruNormalInfos |
| RAMsgsPreprocess.raNormalInfos | Calc_VotedRA.raNormalInfos | — |
| Calc_NavSourceSelection.NavDataState | Calc_VotedRA.NavDataState | — |
| Calc_NavSourceSelection.AdahruData | Calc_VotedRA.AdahruData | — |
| SpIn_ADAHRS | AdahruMsgsPreprocess.SpIn_ADAHRS | — |
| Bus Creator2.out1 | NavLog | — |
| SpIn_RA | RAMsgsPreprocess.RAIn | — |
| OR.out1 | Calc_NavSourceSelection.OnGnd | — |
| OR.out1 | Calc_VotedRA.OnGnd | — |
| SpIn_FTADC | Calc_VotedFTADC.SpIn_FTADC | — |
| Calc_VotedRA.raState | NavDataState | — |
| Calc_VotedRA.raData | Calc_VotedFTADC.raData | — |
| Calc_VotedFTADC.ftadcData | Subsystem.in2 | — |
| OR.out1 | Subsystem.in3 | — |
| Subsystem.out1 | NavData | — |

导航数值链：Calc_NavSourceSelection → Calc_VotedRA → Calc_VotedFTADC → NavSpApp/Subsystem。最后一个子系统以 ftadcData 为 Bus Assignment（SID78）的输入基底，仅覆写 ftadcVoterCAS_mps 后输出 NavData。导航状态链：Calc_VotedRA.raState → NavDataState，未经过该 FTADC 数值处理链。定位：VE25SP/NavSpApp/Subsystem 及 NavSpApp 的连接表。

<a id="model-hierarchy"></a>

## 3. 分层模型接口

每张表列出该模型根层端口的原始类型声明及维度。非 Bus 的控制信号也保留，以免将权重、来源号、有效性或使能错误地当成 Bus。模型路径和 SID 可回到源文件定位。

### 3.1 导航预处理与表决

<a id="model-adahrumsgspreprocess"></a>

#### AdahruMsgsPreprocess

调用位置：`VE25SP/NavSpApp/AdahruMsgsPreprocess`。

模型：[AdahruMsgsPreprocess.slx](<D:/666/VE25/sp/04_Design/AdahruMsgsPreprocess/specification/AdahruMsgsPreprocess.slx>)；字典：`DD_AdahruMsgsPreprocess.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | SpIn_ADAHRS | [Bus: SpIn_Adahrus](#bus-spin_adahrus) | 1 | 4 |
| 输出 | 1 | adahruNormalInfos | [Bus: BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos) | 1 | 139 |

结构操作：Bus Selector 4 个；Bus Creator 0 个；Bus Assignment 4 个；For Each 3 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-ramsgspreprocess"></a>

#### RAMsgsPreprocess

调用位置：`VE25SP/NavSpApp/Model`。

模型：[RAMsgsPreprocess.slx](<D:/666/VE25/sp/04_Design/RAMsgsPreprocess/specification/RAMsgsPreprocess.slx>)；字典：`DD_RAMsgsPreprocess.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | RAIn | [Bus: SpIn_Ra](#bus-spin_ra) | 未显式存储 | 4 |
| 输出 | 1 | raNormalInfos | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | 20 |

结构操作：Bus Selector 1 个；Bus Creator 0 个；Bus Assignment 0 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_navsourceselection"></a>

#### Calc_NavSourceSelection

调用位置：`VE25SP/NavSpApp/AdahruVoter`。

模型：[Calc_NavSourceSelection.slx](<D:/666/VE25/sp/04_Design/Calc_NavSourceSelection/specification/Calc_NavSourceSelection.slx>)；字典：`DD_Calc_NavSourceSelection.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | adahruNormalInfos | [Bus: BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos) | 1 | 4 |
| 输入 | 2 | OnGnd | boolean | 1 | 521 |
| 输出 | 1 | AdahruWeight | uint8 | 3 | 525 |
| 输出 | 2 | adahruSrc | uint8 | 1 | 526 |
| 输出 | 3 | AdahruData | [Bus: BUS_NavData](#bus-bus_navdata) | 1 | 523 |
| 输出 | 4 | NavDataState | [Bus: BUS_NavDataState](#bus-bus_navdatastate) | 1 | 524 |

结构操作：Bus Selector 54 个；Bus Creator 35 个；Bus Assignment 1 个；For Each 1 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_votedcas"></a>

#### Calc_VotedCAS

调用位置：`Calc_NavSourceSelection/Model`。

模型：[Calc_VotedCAS.slx](<D:/666/VE25/sp/04_Design/Calc_VotedCAS/specification/Calc_VotedCAS.slx>)；字典：`DD_Calc_VotedCAS.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | CAS | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | 4 |
| 输出 | 1 | voterCAS | [Bus: tSingleData](#bus-tsingledata) | 1 | 252 |

结构操作：Bus Selector 3 个；Bus Creator 1 个；Bus Assignment 0 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_votedra"></a>

#### Calc_VotedRA

调用位置：`VE25SP/NavSpApp/raVoter`。

模型：[Calc_VotedRA.slx](<D:/666/VE25/sp/04_Design/Calc_VotedRA/specification/Calc_VotedRA.slx>)；字典：`DD_Calc_VotedRA.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | AdahruData | [Bus: BUS_NavData](#bus-bus_navdata) | 1 | 6 |
| 输入 | 2 | NavDataState | [Bus: BUS_NavDataState](#bus-bus_navdatastate) | 1 | 7 |
| 输入 | 3 | OnGnd | boolean | 1 | 4 |
| 输入 | 4 | raNormalInfos | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | 5 |
| 输出 | 1 | RaSrc | uint8 | 1 | 93 |
| 输出 | 2 | raData | [Bus: BUS_NavData](#bus-bus_navdata) | 1 | 94 |
| 输出 | 3 | raState | [Bus: BUS_NavDataState](#bus-bus_navdatastate) | 1 | 95 |

结构操作：Bus Selector 4 个；Bus Creator 2 个；Bus Assignment 3 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_votedftadc"></a>

#### Calc_VotedFTADC

调用位置：`VE25SP/NavSpApp/Model1`。

模型：[Calc_VotedFTADC.slx](<D:/666/VE25/sp/04_Design/Calc_VotedFTADC/specification/Calc_VotedFTADC.slx>)；字典：`DD_Calc_VotedFTADC.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | SpIn_FTADC | [Bus: SpIn_ftadc](#bus-spin_ftadc) | 未显式存储 | 4 |
| 输入 | 2 | raData | [Bus: BUS_NavData](#bus-bus_navdata) | 未显式存储 | 62 |
| 输出 | 1 | ftadcData | [Bus: BUS_NavData](#bus-bus_navdata) | 未显式存储 | 64 |

结构操作：Bus Selector 6 个；Bus Creator 5 个；Bus Assignment 1 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

### 3.2 设备反馈与MPIO

<a id="model-calc_mcuscudata"></a>

#### Calc_MCUSCUData

调用位置：`VE25SP/Model2`。

模型：[Calc_MCUSCUData.slx](<D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/Calc_MCUSCUData.slx>)；字典：`DD_Calc_MCUSCUData.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | SpIn _McuInfo | [Bus: SpIn_McuInfos](#bus-spin_mcuinfos) | 未显式存储 | 5 |
| 输入 | 2 | SpIn _ScuInfo | [Bus: SpIn_ScuInfos](#bus-spin_scuinfos) | 未显式存储 | 108 |
| 输出 | 1 | DevData | [Bus: BUS_DevData](#bus-bus_devdata) | 1 | 223 |

结构操作：Bus Selector 5 个；Bus Creator 13 个；Bus Assignment 2 个；For Each 3 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_mpiodata"></a>

#### Calc_MPIOData

调用位置：`VE25SP/Model`。

模型：[Calc_MPIOData.slx](<D:/666/VE25/sp/04_Design/Calc_MPIOData/specification/Calc_MPIOData.slx>)；字典：`DD_Calc_MPIOData.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | SpIn_MpioInfo | [Bus: SpIn_Mpio](#bus-spin_mpio) | 未显式存储 | 4 |
| 输出 | 1 | MpioData | [Bus: BUS_MpioData](#bus-bus_mpiodata) | 1 | 113 |

结构操作：Bus Selector 1 个；Bus Creator 27 个；Bus Assignment 1 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

### 3.3 地面站与驾驶员输入

<a id="model-calc_gcsdata"></a>

#### Calc_GCSData

调用位置：`VE25SP/Model3`。

模型：[Calc_GCSData.slx](<D:/666/VE25/sp/04_Design/Calc_GCSData/specification/Calc_GCSData.slx>)；字典：`DD_Calc_GCSData.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | Rms2PublicMsg | [Bus: BUS_DRMS2SP_MSG](#bus-bus_drms2sp_msg) | 未显式存储 | 4 |
| 输入 | 2 | SpIn_Gcs_Curr | [Bus: SpIn_GcsMsg](#bus-spin_gcsmsg) | 1 | 124 |
| 输入 | 3 | SpIn_Gcs_Left | [Bus: SpIn_GcsMsg](#bus-spin_gcsmsg) | 1 | 125 |
| 输入 | 4 | SpIn_Gcs_Right | [Bus: SpIn_GcsMsg](#bus-spin_gcsmsg) | 1 | 126 |
| 输出 | 1 | GcsData | [Bus: BUS_GcsData](#bus-bus_gcsdata) | 1 | 519 |

结构操作：Bus Selector 49 个；Bus Creator 42 个；Bus Assignment 0 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_pilotindata"></a>

#### Calc_PilotInData

调用位置：`VE25SP/Model7`。

模型：[Calc_PilotInData.slx](<D:/666/VE25/sp/04_Design/Calc_PilotInData/specification/Calc_PilotInData.slx>)；字典：`DD_Calc_PilotInData.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | VDT_Stick_Curr | [Bus: SpIn_Sitck](#bus-spin_sitck) | 1 | 4 |
| 输入 | 2 | VDT_Stick_Left | [Bus: SpIn_Sitck](#bus-spin_sitck) | 1 | 5 |
| 输入 | 3 | VDT_Stick_Right | [Bus: SpIn_Sitck](#bus-spin_sitck) | 1 | 6 |
| 输入 | 4 | Dis_Key_Curr | [Bus: SpIn_Key](#bus-spin_key) | 1 | 211 |
| 输入 | 5 | Dis_Key_Left | [Bus: SpIn_Key](#bus-spin_key) | 1 | 212 |
| 输入 | 6 | Dis_Key_Right | [Bus: SpIn_Key](#bus-spin_key) | 1 | 213 |
| 输出 | 1 | PilotInData | [Bus: BUS_PilotInData](#bus-bus_pilotindata) | 未显式存储 | 1510 |
| 输出 | 2 | VdtCBIT | uint32 | 未显式存储 | 1594 |

结构操作：Bus Selector 55 个；Bus Creator 33 个；Bus Assignment 0 个；For Each 4 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

### 3.4 系统状态

<a id="model-calc_sysinfo"></a>

#### Calc_SysInfo

调用位置：`VE25SP/Model1`。

模型：[Calc_SysInfo.slx](<D:/666/VE25/sp/04_Design/Calc_SysInfo/specification/Calc_SysInfo.slx>)；字典：`DD_Calc_SysInfo.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | isFccExist | boolean | 3 | 5 |
| 输入 | 2 | VdtCBIT | uint32 | 未显式存储 | 103 |
| 输入 | 3 | SpIn_CCDL_Left | [Bus: SpIn_CcdlMsg](#bus-spin_ccdlmsg) | 未显式存储 | 111 |
| 输入 | 4 | SpIn_CCDL_Right | [Bus: SpIn_CcdlMsg](#bus-spin_ccdlmsg) | 未显式存储 | 112 |
| 输入 | 5 | SpIn_CCDL_Curr | [Bus: SpIn_CcdlMsg](#bus-spin_ccdlmsg) | 未显式存储 | 113 |
| 输入 | 6 | SpIn_SysInfo_Curr | [Bus: SpIn_SysInfos](#bus-spin_sysinfos) | 1 | 179 |
| 输出 | 1 | SysInfoData | [Bus: BUS_SysInfoData](#bus-bus_sysinfodata) | 未显式存储 | 32 |

结构操作：Bus Selector 5 个；Bus Creator 1 个；Bus Assignment 0 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

### 3.5 控制律及外部报文组包

<a id="model-calc_navmsg_toclaw"></a>

#### Calc_NAVMsg_ToClaw

调用位置：`VE25SP/Model4`。

模型：[Calc_NAVMsg_ToClaw.slx](<D:/666/VE25/sp/04_Design/Calc_NAVMsg_ToClaw/specification/Calc_NAVMsg_ToClaw.slx>)；字典：`DD_Calc_NAVMsg_ToClaw.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | NavData | [Bus: BUS_NavData](#bus-bus_navdata) | 未显式存储 | 4 |
| 输入 | 2 | Air2Gnd | 未显式声明／继承 | 未显式存储 | 331 |
| 输入 | 3 | Air2GndValid | 未显式声明／继承 | 未显式存储 | 332 |
| 输出 | 1 | FtadcLog | [Bus: BUS_FtadcLog](#bus-bus_ftadclog) | 未显式存储 | 333 |
| 输出 | 2 | Claw_Nav_Msg | [Bus: BUS_NAV_IN](#bus-bus_nav_in) | 未显式存储 | 334 |

结构操作：Bus Selector 68 个；Bus Creator 5 个；Bus Assignment 7 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_pilotin_toclaw"></a>

#### Calc_PilotIn_ToClaw

调用位置：`VE25SP/Model5`。

模型：[Calc_PilotIn_ToClaw.slx](<D:/666/VE25/sp/04_Design/Calc_PilotIn_ToClaw/specification/Calc_PilotIn_ToClaw.slx>)；字典：`DD_Calc_PilotIn_ToClaw.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | GcsData | [Bus: BUS_GcsData](#bus-bus_gcsdata) | 1 | 4 |
| 输入 | 2 | PilotInData | [Bus: BUS_PilotInData](#bus-bus_pilotindata) | 未显式存储 | 5 |
| 输入 | 3 | MpioData | [Bus: BUS_MpioData](#bus-bus_mpiodata) | 1 | 6 |
| 输出 | 1 | PilotData | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | 未显式存储 | 807 |
| 输出 | 2 | PilotIn | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | 未显式存储 | 808 |
| 输出 | 3 | RC | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | 未显式存储 | 809 |
| 输出 | 4 | Air2Gnd | 未显式声明／继承 | 未显式存储 | 811 |
| 输出 | 5 | Air2GndValid | 未显式声明／继承 | 未显式存储 | 812 |

结构操作：Bus Selector 121 个；Bus Creator 3 个；Bus Assignment 2 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_sysmsg_toclaw"></a>

#### Calc_SYSMsg_ToClaw

调用位置：`VE25SP/Model6`。

模型：[Calc_SYSMsg_ToClaw.slx](<D:/666/VE25/sp/04_Design/Calc_SYSMsg_ToClaw/specification/Calc_SYSMsg_ToClaw.slx>)；字典：`DD_Calc_SYSMsg_ToClaw.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | MpioData | [Bus: BUS_MpioData](#bus-bus_mpiodata) | 未显式存储 | 6 |
| 输入 | 2 | DevData | [Bus: BUS_DevData](#bus-bus_devdata) | 未显式存储 | 7 |
| 输入 | 3 | SysReady | 未显式声明／继承 | 未显式存储 | 8 |
| 输出 | 1 | Claw_SYS_Msg | [Bus: BUS_SYSINFO_IN](#bus-bus_sysinfo_in) | 未显式存储 | 65 |

结构操作：Bus Selector 7 个；Bus Creator 5 个；Bus Assignment 0 个；For Each 1 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_fmsmsg_toclaw"></a>

#### Calc_FMSMsg_ToClaw

调用位置：`VE25SP/Model8`。

模型：[Calc_FMSMsg_ToClaw.slx](<D:/666/VE25/sp/04_Design/Calc_FMSMsg_ToClaw/specification/Calc_FMSMsg_ToClaw.slx>)；字典：`DD_Calc_FMSMsg_ToClaw.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | MpioData | [Bus: BUS_MpioData](#bus-bus_mpiodata) | 1 | 4 |
| 输出 | 1 | Claw_FMS_In_Msg | [Bus: BUS_FMS_IN](#bus-bus_fms_in) | 未显式存储 | 21 |

结构操作：Bus Selector 21 个；Bus Creator 0 个；Bus Assignment 1 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_spdispmsg_out"></a>

#### Calc_SPDispMsg_Out

调用位置：`VE25SP/Model10`。

模型：[Calc_SPDispMsg_Out.slx](<D:/666/VE25/sp/04_Design/Calc_SPDispMsg_Out/specification/Calc_SPDispMsg_Out.slx>)；字典：`DD_Calc_SPDispMsg_Out.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | SysReady | boolean | 1 | 56 |
| 输入 | 2 | LimitDispatchInfo | uint8 | 1 | 57 |
| 输入 | 3 | MPIOData | [Bus: BUS_MpioData](#bus-bus_mpiodata) | 未显式存储 | 58 |
| 输入 | 4 | PilotIn | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | 未显式存储 | 59 |
| 输入 | 5 | RC | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | 未显式存储 | 60 |
| 输入 | 6 | Cockpit_In | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | 未显式存储 | 61 |
| 输出 | 1 | SpDispMsg | [Bus: BUS_SpDispMsg](#bus-bus_spdispmsg) | 未显式存储 | 62 |

结构操作：Bus Selector 17 个；Bus Creator 1 个；Bus Assignment 0 个；For Each 0 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="model-calc_ccdlmsg_out"></a>

#### Calc_CcdlMsg_Out

调用位置：`VE25SP/Model9`。

模型：[Calc_CcdlMsg_Out.slx](<D:/666/VE25/sp/04_Design/Calc_CcdlMsg_Out/specification/Calc_CcdlMsg_Out.slx>)；字典：`DD_Calc_CcdlMsg_Out.sldd`。

| 方向 | 端口 | 信号名 | 原始类型声明 | 端口声明维度 | SID |
|---|---|---|---|---|---|
| 输入 | 1 | SysInfoData | [Bus: BUS_SysInfoData](#bus-bus_sysinfodata) | 未显式存储 | 269 |
| 输入 | 2 | Claw_Nav_Msg | [Bus: BUS_NAV_IN](#bus-bus_nav_in) | 未显式存储 | 270 |
| 输入 | 3 | DevData | [Bus: BUS_DevData](#bus-bus_devdata) | 未显式存储 | 271 |
| 输入 | 4 | MPIOData | [Bus: BUS_MpioData](#bus-bus_mpiodata) | 未显式存储 | 272 |
| 输入 | 5 | PilotIn | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | 未显式存储 | 273 |
| 输入 | 6 | RC | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | 未显式存储 | 274 |
| 输入 | 7 | Cockpit_In | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | 未显式存储 | 275 |
| 输出 | 1 | SpCcdlOut | [Bus: BUS_SpMsgOut](#bus-bus_spmsgout) | 未显式存储 | 279 |
| 输出 | 2 | SysReady | boolean | 未显式存储 | 280 |
| 输出 | 3 | LimitDispatch | uint8 | 未显式存储 | 281 |

结构操作：Bus Selector 27 个；Bus Creator 0 个；Bus Assignment 1 个；For Each 1 个。数量按当前模型保存内容统计，不重复展开链接库内部实例。

<a id="outputs"></a>

## 4. 对外输出

| 端口 | 输出名 | Bus 类型 | 端口声明维度 | SID |
|---|---|---|---|---|
| 1 | SpIn_Nav2Display | [Bus: BUS_NavData](#bus-bus_navdata) | 未显式存储 | 20 |
| 2 | NAV_Root | [Bus: BUS_NAV_IN](#bus-bus_nav_in) | 未显式存储 | 22 |
| 3 | FMS_In | [Bus: BUS_FMS_IN](#bus-bus_fms_in) | 未显式存储 | 23 |
| 4 | SYSInfo_In | [Bus: BUS_SYSINFO_IN](#bus-bus_sysinfo_in) | 未显式存储 | 24 |
| 5 | NavDataState | [Bus: BUS_NavDataState](#bus-bus_navdatastate) | 1 | 367 |
| 6 | Cockpit_In | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | 未显式存储 | 21 |
| 7 | SPDispMsgOut | [Bus: BUS_SpDispMsg](#bus-bus_spdispmsg) | 未显式存储 | 424 |
| 8 | SpCcdlMsgOut | [Bus: BUS_SpMsgOut](#bus-bus_spmsgout) | 未显式存储 | 425 |
| 9 | SP2DRMS | [Bus: BUS_SP2DRMS_MSG](#bus-bus_sp2drms_msg) | 未显式存储 | 458 |

`SpIn_Nav2Display` 虽以 SpIn 开头，在当前 VE25SP 中实际是 Outport；本文按块类型和连接定位它。`BUS_SpMsgOut.Data` 的原始 DataType 为裸名称 `SpCcdlMsg`，下文按同名 Bus 定义建立结构链接，保留原始声明。

<a id="catalog"></a>

## 5. 完整 Bus 字段定义

本节覆盖当前字典闭包内全部 85 个 Bus。字段序号保持数据字典定义顺序；嵌套结构通过类型链接展开。每种结构只列一次，多个信号实例共用同一类型时不会复制定义。

| 分组 | Bus 数 | 类型索引 |
|---|---|---|
| 外部输入与原始记录 | 16 | [SpIn_Adahrus](#bus-spin_adahrus)、[SpIn_Ra](#bus-spin_ra)、[SpIn_McuInfos](#bus-spin_mcuinfos)、[Sp_McuInfo](#bus-sp_mcuinfo)、[SpIn_ScuInfos](#bus-spin_scuinfos)、[Sp_ScuInfo](#bus-sp_scuinfo)、[SpIn_Mpio](#bus-spin_mpio)、[SpIn_CcdlMsg](#bus-spin_ccdlmsg)、[SpIn_SysInfos](#bus-spin_sysinfos)、[BUS_DRMS2SP_MSG](#bus-bus_drms2sp_msg)、[SpIn_ftadc](#bus-spin_ftadc)、[SpIn_GcsMsg](#bus-spin_gcsmsg)、[RemotePilotCtrl](#bus-remotepilotctrl)、[RemoteFMCP](#bus-remotefmcp)、[SpIn_Sitck](#bus-spin_sitck)、[SpIn_Key](#bus-spin_key) |
| 导航预处理与表决结果 | 5 | [BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos)、[BUS_NavData](#bus-bus_navdata)、[BUS_NavDataState](#bus-bus_navdatastate)、[BUS_FtadcLog](#bus-bus_ftadclog)、[BUS_CmpState](#bus-bus_cmpstate) |
| 设备与MPIO业务数据 | 2 | [BUS_DevData](#bus-bus_devdata)、[BUS_MpioData](#bus-bus_mpiodata) |
| 地面站与驾驶员业务数据 | 5 | [BUS_GcsData](#bus-bus_gcsdata)、[BUS_PilotInData](#bus-bus_pilotindata)、[BUS_PilotCmpState](#bus-bus_pilotcmpstate)、[Bus_GPIOState](#bus-bus_gpiostate)、[Bus_VDTState](#bus-bus_vdtstate) |
| 系统状态 | 1 | [BUS_SysInfoData](#bus-bus_sysinfodata) |
| 对外输出与报文 | 16 | [BUS_NAV_IN](#bus-bus_nav_in)、[BUS_FCS_NAV_Valid_Status](#bus-bus_fcs_nav_valid_status)、[BUS_FCS_NAV_Reset_Status](#bus-bus_fcs_nav_reset_status)、[BUS_FMS_IN](#bus-bus_fms_in)、[BUS_SYSINFO_IN](#bus-bus_sysinfo_in)、[BUS_SYSINFO_VCU](#bus-bus_sysinfo_vcu)、[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr)、[BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state)、[BUS_SYSINFO_ACT](#bus-bus_sysinfo_act)、[BUS_SYSINFO_ACT_State](#bus-bus_sysinfo_act_state)、[BUS_PILOT_IN](#bus-bus_pilot_in)、[BUS_TEST_IN](#bus-bus_test_in)、[BUS_SpDispMsg](#bus-bus_spdispmsg)、[BUS_SpMsgOut](#bus-bus_spmsgout)、[SpCcdlMsg](#bus-spccdlmsg)、[BUS_SP2DRMS_MSG](#bus-bus_sp2drms_msg) |
| 公共数据与状态结构 | 16 | [Bus_DoubleSignalNormalInfo](#bus-bus_doublesignalnormalinfo)、[tSignalState](#bus-tsignalstate)、[Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo)、[SpData_Double](#bus-spdata_double)、[SpData_Enum](#bus-spdata_enum)、[SpData_Single](#bus-spdata_single)、[tDoubleData](#bus-tdoubledata)、[tMcuMsg](#bus-tmcumsg)、[tSingleData](#bus-tsingledata)、[tU8Data](#bus-tu8data)、[tU64Data](#bus-tu64data)、[tNavLog](#bus-tnavlog)、[tRedundantDataState](#bus-tredundantdatastate)、[tVoterState](#bus-tvoterstate)、[tServoMsg](#bus-tservomsg)、[tU32Data](#bus-tu32data) |
| 字典可见但未定位当前使用 | 24 | [BUS_DevDataState](#bus-bus_devdatastate)、[BUS_GcsDataState](#bus-bus_gcsdatastate)、[BUS_MpioDataState](#bus-bus_mpiodatastate)、[BUS_PilotInDataState](#bus-bus_pilotindatastate)、[BUS_RMS_Public_Msgs](#bus-bus_rms_public_msgs)、[BUS_RMS_Voter_Data](#bus-bus_rms_voter_data)、[BUS_SpLog](#bus-bus_splog)、[tFccStatus](#bus-tfccstatus)、[BUS_UTC](#bus-bus_utc)、[BusOut_CcdlMsg](#bus-busout_ccdlmsg)、[BusOut_SpDispMsg](#bus-busout_spdispmsg)、[Bus_SpCPT](#bus-bus_spcpt)、[Claw_Nav_Msg](#bus-claw_nav_msg)、[Claw_Sys_Msg](#bus-claw_sys_msg)、[SpIn_IpCBIT](#bus-spin_ipcbit)、[SpNavData_Double](#bus-spnavdata_double)、[SpNavData_Single](#bus-spnavdata_single)、[SpOut_Nav](#bus-spout_nav)、[tBoolData](#bus-tbooldata)、[tS16Data](#bus-ts16data)、[tS32Data](#bus-ts32data)、[tS64Data](#bus-ts64data)、[tS8Data](#bus-ts8data)、[tU16Data](#bus-tu16data) |

### 5.1 外部输入与原始记录

<a id="bus-spin_adahrus"></a>

#### SpIn_Adahrus

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_Adahrus)`。直接字段：**31**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`AdahruMsgsPreprocess/SpIn_ADAHRS`；`VE25SP/SpIn_ADAHRS`、`VE25SP/NavSpApp/AdahruMsgsPreprocess`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `L310_Combined_Latitude` | [Bus: SpData_Double](#bus-spdata_double) | 3 | Fixed | real | — | — | — |
| 2 | `L311_Combined_Longitude` | [Bus: SpData_Double](#bus-spdata_double) | 3 | Fixed | real | — | — | — |
| 3 | `L076_GPS_Height` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 4 | `L107_Angle_Of_Attack` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 5 | `L110_Sideslip_Angle` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 6 | `L112_Ground_Speed` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 7 | `L203_Pressure_Altitude` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 8 | `L204_Baro_Corrected_Altitude` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 9 | `L205_Mach` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 10 | `L206_Computed_Airspeed` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 11 | `L210_True_Airspeed` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 12 | `L211_Comb_Altitude_Rate` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 13 | `L212_Altitude_Rate` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 14 | `L213_Static_Air_Temperature` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 15 | `L242_Total_Pressure` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 16 | `L245_Static_Pressure` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 17 | `L320_Magnetic_Heading` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 18 | `L321_True_Heading` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 19 | `L322_Mag_Declination` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 20 | `L365_Vertical_Velocity` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 21 | `L366_North_Velocity` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 22 | `L367_East_Velocity` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 23 | `L324_Pitch_Angle` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 24 | `L325_Roll_Angle` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 25 | `L331_Body_Longit_Accel` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 26 | `L332_Body_Lateral_Accel` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 27 | `L333_Body_Normal_Accel` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 28 | `L336_Pitch_Attitude_Rate` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 29 | `L337_Roll_Attitude_Rate` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 30 | `L340_Heading_Angle_Rate` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |
| 31 | `AdaComb_Altitude_Rate` | [Bus: SpData_Single](#bus-spdata_single) | 3 | Fixed | real | — | — | — |

<a id="bus-spin_ra"></a>

#### SpIn_Ra

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_Ra)`。直接字段：**1**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`RAMsgsPreprocess/RAIn`；`VE25SP/SpIn_RA`、`VE25SP/NavSpApp/Model`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `L164_Radio_Altitude` | [Bus: SpData_Single](#bus-spdata_single) | 2 | Fixed | real | — | — | — |

<a id="bus-spin_mcuinfos"></a>

#### SpIn_McuInfos

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_McuInfos)`。直接字段：**1**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_MCUSCUData/SpIn _McuInfo`；`VE25SP/SpIn _McuInfo`、`VE25SP/Model2`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `MCU` | [Bus: Sp_McuInfo](#bus-sp_mcuinfo) | 11 | Fixed | real | — | — | — |

<a id="bus-sp_mcuinfo"></a>

#### Sp_McuInfo

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=Sp_McuInfo)`。直接字段：**7**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[SpIn_McuInfos](#bus-spin_mcuinfos).`MCU` [11]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Valid` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `McuFaultLevelFbk` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `McuWorkModFbk` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `McuSpdFbk` | single | 1 | Fixed | real | — | — | — |
| 5 | `McuPowerFbk` | single | 1 | Fixed | real | — | — | — |
| 6 | `McuSpdFbkInvalid` | boolean | 1 | Fixed | real | — | — | — |
| 7 | `McuPowerFbkInvalid` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-spin_scuinfos"></a>

#### SpIn_ScuInfos

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_ScuInfos)`。直接字段：**1**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_MCUSCUData/SpIn _ScuInfo`；`VE25SP/SpIn_ScuInfo`、`VE25SP/Model2`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `SCU` | [Bus: Sp_ScuInfo](#bus-sp_scuinfo) | 8 | Fixed | real | — | — | — |

<a id="bus-sp_scuinfo"></a>

#### Sp_ScuInfo

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=Sp_ScuInfo)`。直接字段：**5**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[SpIn_ScuInfos](#bus-spin_scuinfos).`SCU` [8]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Valid` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `INT_STS` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `EN_STS` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `STK_MON` | uint8 | 1 | Fixed | real | — | — | — |
| 5 | `Position_Fdb` | single | 1 | Fixed | real | — | — | — |

<a id="bus-spin_mpio"></a>

#### SpIn_Mpio

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_Mpio)`。直接字段：**31**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_MPIOData/SpIn_MpioInfo`；`VE25SP/SpIn_MpioInfo`、`VE25SP/Model`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Valid` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `EgyWkSts` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `HiVltgSts` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `Usable_Pwr` | single | 1 | Fixed | real | — | — | — |
| 5 | `Usable_Pwr_Valid` | uint8 | 1 | Fixed | real | — | — | — |
| 6 | `FMS_Alt_Target` | single | 1 | Fixed | real | — | — | — |
| 7 | `FMS_Alt_Target_Valid` | uint8 | 1 | Fixed | real | — | — | — |
| 8 | `FMS_Spd_Target` | single | 1 | Fixed | real | — | — | — |
| 9 | `FMS_TrueHeading_Target` | single | 1 | Fixed | real | — | — | — |
| 10 | `FMS_Track_Target` | single | 1 | Fixed | real | — | — | — |
| 11 | `FMS_Spd_Target_Valid` | uint8 | 1 | Fixed | real | — | — | — |
| 12 | `FMS_TrueHeading_Target_Valid` | uint8 | 1 | Fixed | real | — | — | — |
| 13 | `FMS_Track_Target_Valid` | uint8 | 1 | Fixed | real | — | — | — |
| 14 | `HiVltgSW` | uint8 | 1 | Fixed | real | — | — | — |
| 15 | `FMS_Roll_Command` | single | 1 | Fixed | real | — | — | — |
| 16 | `FMS_Vertical_Speed_Target` | single | 1 | Fixed | real | — | — | — |
| 17 | `FMS_Roll_Command_Status` | uint8 | 1 | Fixed | real | — | — | — |
| 18 | `FMS_Vertical_Speed_Target_Valid` | uint8 | 1 | Fixed | real | — | — | — |
| 19 | `FMS_Trans_Request` | uint8 | 1 | Fixed | real | — | — | — |
| 20 | `FMS_LNAV_Request` | uint8 | 1 | Fixed | real | — | — | — |
| 21 | `FMS_VNAV_Request` | uint8 | 1 | Fixed | real | — | — | — |
| 22 | `FMS_VNAV_Status` | uint8 | 1 | Fixed | real | — | — | — |
| 23 | `Center_Gravity` | single | 1 | Fixed | real | — | — | — |
| 24 | `Center_Gravity_Valid` | uint8 | 1 | Fixed | real | — | — | — |
| 25 | `Gross_Weight` | single | 1 | Fixed | real | — | — | — |
| 26 | `Gross_Weight_Valid` | uint8 | 1 | Fixed | real | — | — | — |
| 27 | `Motor_Check` | uint8 | 1 | Fixed | real | — | — | — |
| 28 | `Surface_Check` | uint8 | 1 | Fixed | real | — | — | — |
| 29 | `Preflight_Check_Valid` | uint8 | 1 | Fixed | real | — | — | — |
| 30 | `FMS_FPA_Target` | single | 1 | Fixed | real | — | — | — |
| 31 | `FMS_FPA_Target_Valid` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-spin_ccdlmsg"></a>

#### SpIn_CcdlMsg

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_CcdlMsg)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_SysInfo/SpIn_CCDL_Left`、`Calc_SysInfo/SpIn_CCDL_Right`、`Calc_SysInfo/SpIn_CCDL_Curr`；`VE25SP/Sys_Status_Curr`、`VE25SP/Sys_Status_Left`、`VE25SP/Sys_Status_Right` 等 4 处。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Valid` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `SysReady` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `LimitDispatch` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-spin_sysinfos"></a>

#### SpIn_SysInfos

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_SysInfos)`。直接字段：**8**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_SysInfo/SpIn_SysInfo_Curr`；`VE25SP/SpIn_SysInfo_Curr`、`VE25SP/Model1`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `MCU_HealthBit` | uint16 | 1 | Fixed | real | — | — | — |
| 2 | `SCU_HealthBit` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `ADAHRS_HealthBit` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `RA_HealthBit` | uint8 | 1 | Fixed | real | — | — | — |
| 5 | `MPIO_HealthBit` | uint8 | 1 | Fixed | real | — | — | — |
| 6 | `GCS_HealtBit` | uint8 | 1 | Fixed | real | — | — | — |
| 7 | `FTADC_HealtBit` | uint8 | 1 | Fixed | real | — | — | — |
| 8 | `FccHealth` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-bus_drms2sp_msg"></a>

#### BUS_DRMS2SP_MSG

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_DRMS2SP_MSG)`。直接字段：**4**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_GCSData/Rms2PublicMsg`；`VE25SP/RmsPublicMsg`、`VE25SP/Model3`、`VE25SP/NavSpApp/RmsPublicMsg`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `vtolState` | [Enum: ENUM_DRMS_VTOL_STATE](#enum-enum_drms_vtol_state) | 1 | Fixed | real | — | — | — |
| 2 | `fccId` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `activeFccId` | [Enum: ENUM_DRMS_FccId](#enum-enum_drms_fccid) | 1 | Fixed | real | — | — | — |
| 4 | `isFccExist` | boolean | 3 | Fixed | real | — | — | — |

<a id="bus-spin_ftadc"></a>

#### SpIn_ftadc

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_ftadc)`。直接字段：**6**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_VotedFTADC/SpIn_FTADC`；`VE25SP/SpIn_FTADC`、`VE25SP/NavSpApp/SpIn_FTADC`、`VE25SP/NavSpApp/Model1`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Psi` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 2 | `Pti` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 3 | `AOA` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 4 | `AOS` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 5 | `CAS` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 6 | `Valid` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-spin_gcsmsg"></a>

#### SpIn_GcsMsg

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_GcsMsg)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_GCSData/SpIn_Gcs_Curr`、`Calc_GCSData/SpIn_Gcs_Left`、`Calc_GCSData/SpIn_Gcs_Right`；`VE25SP/SpIn_Gcs_Curr`、`VE25SP/SpIn_Gcs_Left`、`VE25SP/SpIn_Gcs_Right` 等 4 处。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `RemotePilotCtrl` | [Bus: RemotePilotCtrl](#bus-remotepilotctrl) | 1 | Fixed | real | — | — | — |
| 2 | `RemoteFMCP` | [Bus: RemoteFMCP](#bus-remotefmcp) | 1 | Fixed | real | — | — | — |

<a id="bus-remotepilotctrl"></a>

#### RemotePilotCtrl

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=RemotePilotCtrl)`。直接字段：**26**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[SpIn_GcsMsg](#bus-spin_gcsmsg).`RemotePilotCtrl` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `RC_SW0` | uint8 | 1 | Fixed | real | — | — | — |
| 2 | `RC_SW1` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `RC_SW2` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `RC_SW3` | uint8 | 1 | Fixed | real | — | — | — |
| 5 | `RC_SW4` | uint8 | 1 | Fixed | real | — | — | — |
| 6 | `RC_SW5` | uint8 | 1 | Fixed | real | — | — | — |
| 7 | `RC_SW6` | uint8 | 1 | Fixed | real | — | — | — |
| 8 | `RC_SW7` | uint8 | 1 | Fixed | real | — | — | — |
| 9 | `RC_SW8` | uint8 | 1 | Fixed | real | — | — | — |
| 10 | `RC_SW9` | uint8 | 1 | Fixed | real | — | — | — |
| 11 | `Stick_Type` | uint8 | 1 | Fixed | real | — | — | — |
| 12 | `Key_AP` | uint8 | 1 | Fixed | real | — | — | — |
| 13 | `Key_TRC` | uint8 | 1 | Fixed | real | — | — | — |
| 14 | `ValidBit` | uint8 | 1 | Fixed | real | — | — | — |
| 15 | `Key_Emergency` | uint8 | 1 | Fixed | real | — | — | — |
| 16 | `Key_Multcopter_Prot` | uint8 | 1 | Fixed | real | — | — | — |
| 17 | `Key_BackupFcc_Act` | uint8 | 1 | Fixed | real | — | — | — |
| 18 | `Key_FC_Mode_Sel` | uint8 | 1 | Fixed | real | — | — | — |
| 19 | `Key_Trans_Act` | uint8 | 1 | Fixed | real | — | — | — |
| 20 | `Key_AirGround_SW` | uint8 | 1 | Fixed | real | — | — | — |
| 21 | `Key_Lock_SW` | uint8 | 1 | Fixed | real | — | — | — |
| 22 | `chAileron` | single | 1 | Fixed | real | — | — | — |
| 23 | `chElevator` | single | 1 | Fixed | real | — | — | — |
| 24 | `chThrottle` | single | 1 | Fixed | real | — | — | — |
| 25 | `chRudder` | single | 1 | Fixed | real | — | — | — |
| 26 | `PilotCtrlMsgValid` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-remotefmcp"></a>

#### RemoteFMCP

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=RemoteFMCP)`。直接字段：**15**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[SpIn_GcsMsg](#bus-spin_gcsmsg).`RemoteFMCP` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `FD_Pushbutton` | uint8 | 1 | Fixed | real | — | — | — |
| 2 | `AP_Pushbutton` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `AT_Pushbutton` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `FMS_Spd_Pushbutton` | uint8 | 1 | Fixed | real | — | — | — |
| 5 | `SYC_Pushbutton` | uint8 | 1 | Fixed | real | — | — | — |
| 6 | `HDG_Pushbutton` | uint8 | 1 | Fixed | real | — | — | — |
| 7 | `LNAV_Pushbutton` | uint8 | 1 | Fixed | real | — | — | — |
| 8 | `VNAV_Pushbutton` | uint8 | 1 | Fixed | real | — | — | — |
| 9 | `VS_Pushbutton` | uint8 | 1 | Fixed | real | — | — | — |
| 10 | `ALT_Pushbutton` | uint8 | 1 | Fixed | real | — | — | — |
| 11 | `Altitude_Target` | single | 1 | Fixed | real | — | — | — |
| 12 | `Vertical_Speed_Target` | single | 1 | Fixed | real | — | — | — |
| 13 | `Heading_Target` | single | 1 | Fixed | real | — | — | — |
| 14 | `Airspeed_Target` | single | 1 | Fixed | real | — | — | — |
| 15 | `FmcpMsgValid` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-spin_sitck"></a>

#### SpIn_Sitck

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_Sitck)`。直接字段：**6**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_PilotInData/VDT_Stick_Curr`、`Calc_PilotInData/VDT_Stick_Left`、`Calc_PilotInData/VDT_Stick_Right`；`VE25SP/VDT_Stick_Curr`、`VE25SP/VDT_Stick_Left`、`VE25SP/VDT_Stick_Right` 等 4 处。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `CSSU_X` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 2 | `CSSU_Y` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 3 | `RSSU_X` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 4 | `RSSU_Y` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 5 | `LSSU_X` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 6 | `LSSU_Y` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |

<a id="bus-spin_key"></a>

#### SpIn_Key

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_Key)`。直接字段：**14**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_PilotInData/Dis_Key_Curr`、`Calc_PilotInData/Dis_Key_Left`、`Calc_PilotInData/Dis_Key_Right`；`VE25SP/Dis_Key_Curr`、`VE25SP/Dis_Key_Left`、`VE25SP/Dis_Key_Right` 等 4 处。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Key_AirGnd` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 2 | `Key_ELFS` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 3 | `Key_PfcModeSW` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 4 | `Key_McProtect` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 5 | `Key_Backup` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 6 | `Key_TRC` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 7 | `Key_Ap_L` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 8 | `Key_Ap_R` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 9 | `Key_RSSU_BackupActive` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 10 | `Key_CSSU_BackupActive` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 11 | `Key_SSU_BackupUp` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 12 | `Key_SSU_BackupDown` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 13 | `Key_SSU_BackupLeft` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 14 | `Key_SSU_BackupRight` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |

### 5.2 导航预处理与表决结果

<a id="bus-bus_adahrunormalinfos"></a>

#### BUS_adahruNormalInfos

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_adahruNormalInfos)`。直接字段：**31**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`AdahruMsgsPreprocess/Constant`、`AdahruMsgsPreprocess/adahruNormalInfos`；`Calc_NavSourceSelection/adahruNormalInfos`、`Calc_NavSourceSelection/SourceSelection/SignalNormalInfos`、`Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/SignalNormalInfo`；`VE25SP/NavSpApp/AdahruMsgsPreprocess`、`VE25SP/NavSpApp/AdahruVoter`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `CombLate` | [Bus: Bus_DoubleSignalNormalInfo](#bus-bus_doublesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 2 | `CombLong` | [Bus: Bus_DoubleSignalNormalInfo](#bus-bus_doublesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 3 | `GpsHeight` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 4 | `AOA` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 5 | `AOS` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 6 | `GroundSpeed` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 7 | `PressureAlt` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 8 | `BaroCorrectedAlt` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 9 | `Mach` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 10 | `CAS` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 11 | `TAS` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 12 | `GpsAltRate` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 13 | `AltRate` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 14 | `StaticAirTemp` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 15 | `TotalPressure` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 16 | `StaticPressure` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 17 | `MagnHeading` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 18 | `TrueHeaading` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 19 | `MagDeclination` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 20 | `VertVelocity` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 21 | `NorthVelocity` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 22 | `EastVelocity` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 23 | `Pitch` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 24 | `Roll` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 25 | `SforX` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 26 | `SforY` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 27 | `SforZ` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 28 | `Q` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 29 | `P` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 30 | `R` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |
| 31 | `AdAltRate` | [Bus: Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo) | 3 | Fixed | real | — | — | — |

<a id="bus-bus_navdata"></a>

#### BUS_NavData

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_NavData)`。直接字段：**41**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_NAVMsg_ToClaw/NavData`；`Calc_NavSourceSelection/AdahruData`、`Calc_NavSourceSelection/SourceSelection/output/calcData/Bus Creator`、`Calc_NavSourceSelection/SourceSelection/output/calcData/NavData`；`Calc_VotedFTADC/raData`、`Calc_VotedFTADC/ftadcData`；`Calc_VotedRA/AdahruData`、`Calc_VotedRA/raData`、`Calc_VotedRA/SourceSelection/output/calcData/data`；`VE25SP/Model4`、`VE25SP/SpIn_Nav2Display`、`VE25SP/NavSpApp/AdahruVoter` 等 6 处。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `adahruLongitude_Deg` | [Bus: tDoubleData](#bus-tdoubledata) | 1 | Fixed | real | — | — | — |
| 2 | `adahruLatitude_Deg` | [Bus: tDoubleData](#bus-tdoubledata) | 1 | Fixed | real | — | — | — |
| 3 | `adahruGpsHeight_M` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 4 | `adahruPressureAlt_M` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 5 | `adahruBaroCorrectedAlt_M` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 6 | `adahruAOA_Deg` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 7 | `adahruAOS_Deg` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 8 | `adahruGroundSpeed_MPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 9 | `adahruMach_Ma` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 10 | `adahruCAS_MPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 11 | `adahruTAS_MPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 12 | `adahruGpsAltRate_MPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 13 | `adahruAdAltRate_MPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 14 | `adahruAltRate_MPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 15 | `adahruStaticTemp_C` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 16 | `adahruTotalPressure_hPa` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 17 | `adahruStaticPressure_hPa` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 18 | `adahruMagnHeading_Deg` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 19 | `adahruTrueHeading_Deg` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 20 | `adahruMagDeclination_Deg` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 21 | `adahruVelU_MPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 22 | `adahruVelN_MPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 23 | `adahruVelE_MPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 24 | `adahruPitch_Deg` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 25 | `adahruRoll_Deg` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 26 | `adahruSforX_MPS2` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 27 | `adahruSforY_MPS2` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 28 | `adahruSforZ_MPS2` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 29 | `adahruP_DegPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 30 | `adahruQ_DegPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 31 | `adahruR_DegPS` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 32 | `raRadioAlt_M` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 33 | `ra1RadioAlt_M` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 34 | `ra2RadioAlt_M` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 35 | `ftadcStaticPressure_kPa` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 36 | `ftadcTotalPressure_kPa` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 37 | `ftadcAttackOfAngle_Deg` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 38 | `ftadcSideslipAngle_Deg` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 39 | `ftadcCAS_mps` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 40 | `VoterCAS_mps` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 41 | `ftadcVoterCAS_mps` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_navdatastate"></a>

#### BUS_NavDataState

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_NavDataState)`。直接字段：**33**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_NavSourceSelection/NavDataState`、`Calc_NavSourceSelection/SourceSelection/output/calcData/Bus Creator1`、`Calc_NavSourceSelection/SourceSelection/output/calcData/NavDataState`；`Calc_VotedRA/NavDataState`、`Calc_VotedRA/raState`、`Calc_VotedRA/SourceSelection/output/calcData/NavDataState` 等 4 处；`VE25SP/NavDataState`、`VE25SP/NavSpApp/AdahruVoter`、`VE25SP/NavSpApp/raVoter` 等 4 处。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `adahruLongitude_Deg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 2 | `adahruLatitude_Deg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 3 | `adahruGpsHeight_M` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 4 | `adahruPressureAlt_M` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 5 | `adahruBaroCorrectedAlt_M` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 6 | `adahruAOA_Deg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 7 | `adahruAOS_Deg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 8 | `adahruGroundSpeed_MPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 9 | `adahruMach_Ma` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 10 | `adahruCAS_MPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 11 | `adahruTAS_MPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 12 | `adahruGpsAltRate_MPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 13 | `adahruAdAltRate_MPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 14 | `adahruAltRate_MPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 15 | `adahruStaticTemp_C` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 16 | `adahruTotalPressure_hPa` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 17 | `adahruStaticPressure_hPa` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 18 | `adahruMagnHeading_Deg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 19 | `adahruTrueHeading_Deg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 20 | `adahruMagDeclination_Deg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 21 | `adahruVelU_MPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 22 | `adahruVelN_MPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 23 | `adahruVelE_MPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 24 | `adahruPitch_Deg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 25 | `adahruRoll_Deg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 26 | `adahruSforX_MPS2` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 27 | `adahruSforY_MPS2` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 28 | `adahruSforZ_MPS2` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 29 | `adahruP_DegPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 30 | `adahruQ_DegPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 31 | `adahruR_DegPS` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 32 | `raRadioAlt_M` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 33 | `ftadcMsg` | [Bus: tSignalState](#bus-tsignalstate) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_ftadclog"></a>

#### BUS_FtadcLog

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_FtadcLog)`。直接字段：**6**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_NAVMsg_ToClaw/FtadcLog`、`Calc_NAVMsg_ToClaw/If Action Subsystem/Bus Creator3`、`Calc_NAVMsg_ToClaw/If Action Subsystem/Ftadclog`；`VE25SP/Model4`。

作为成员出现于：[BUS_SpLog](#bus-bus_splog).`Ftadc` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Baro` | single | 1 | Fixed | real | — | — | — |
| 2 | `CAS` | single | 1 | Fixed | real | — | — | — |
| 3 | `TAS` | single | 1 | Fixed | real | — | — | — |
| 4 | `BaroValid` | boolean | 1 | Fixed | real | — | — | — |
| 5 | `CASValid` | boolean | 1 | Fixed | real | — | — | — |
| 6 | `TASValid` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-bus_cmpstate"></a>

#### BUS_CmpState

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_CmpState)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_PilotInData/SignalComparotor1/For Each Subsystem/Bus Creator`、`Calc_PilotInData/SignalComparotor1/For Each Subsystem/Bus Creator1`、`Calc_PilotInData/SignalComparotor1/For Each Subsystem/Bus Creator2` 等 6 处。

作为成员出现于：[BUS_PilotCmpState](#bus-bus_pilotcmpstate).`Current` [1]；[BUS_PilotCmpState](#bus-bus_pilotcmpstate).`Left` [1]；[BUS_PilotCmpState](#bus-bus_pilotcmpstate).`Right` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `isSynced` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `isMissCompare` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `isBanned` | boolean | 1 | Fixed | real | — | — | — |

### 5.3 设备与MPIO业务数据

<a id="bus-bus_devdata"></a>

#### BUS_DevData

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_DevData)`。直接字段：**10**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_CcdlMsg_Out/DevData`；`Calc_MCUSCUData/Bus Creator`、`Calc_MCUSCUData/DevData`；`Calc_SYSMsg_ToClaw/DevData`；`VE25SP/Model2`、`VE25SP/Model6`、`VE25SP/Model9`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `LiftMCU` | [Bus: tMcuMsg](#bus-tmcumsg) | 8 | Fixed | real | — | — | — |
| 2 | `ThrustMCU` | [Bus: tMcuMsg](#bus-tmcumsg) | 3 | Fixed | real | — | — | — |
| 3 | `AilLO` | [Bus: tServoMsg](#bus-tservomsg) | 1 | Fixed | real | — | — | — |
| 4 | `AilLI` | [Bus: tServoMsg](#bus-tservomsg) | 1 | Fixed | real | — | — | — |
| 5 | `AilRO` | [Bus: tServoMsg](#bus-tservomsg) | 1 | Fixed | real | — | — | — |
| 6 | `AilRI` | [Bus: tServoMsg](#bus-tservomsg) | 1 | Fixed | real | — | — | — |
| 7 | `EleL` | [Bus: tServoMsg](#bus-tservomsg) | 1 | Fixed | real | — | — | — |
| 8 | `EleR` | [Bus: tServoMsg](#bus-tservomsg) | 1 | Fixed | real | — | — | — |
| 9 | `RudL` | [Bus: tServoMsg](#bus-tservomsg) | 1 | Fixed | real | — | — | — |
| 10 | `RudR` | [Bus: tServoMsg](#bus-tservomsg) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_mpiodata"></a>

#### BUS_MpioData

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_MpioData)`。直接字段：**28**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_CcdlMsg_Out/MPIOData`；`Calc_FMSMsg_ToClaw/MpioData`；`Calc_MPIOData/Constant`、`Calc_MPIOData/MpioData`；`Calc_PilotIn_ToClaw/MpioData`、`Calc_PilotIn_ToClaw/handleGCS/Subsystem15/MpioData`、`Calc_PilotIn_ToClaw/handlePilotIn/Subsystem15/MpioData`；`Calc_SPDispMsg_Out/MPIOData`；`Calc_SYSMsg_ToClaw/MpioData`；`VE25SP/Model`、`VE25SP/Model10`、`VE25SP/Model5` 等 6 处。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `EgyWkSts` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 2 | `HiVltgSts` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 3 | `UsablePwr_KW` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 4 | `HiVltgSW` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 5 | `SelectedFCC` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 6 | `CenterGravity` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 7 | `MotorCheck` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 8 | `SurfaceCheck` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 9 | `PreflightCheckValid` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 10 | `GrossWeight` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 11 | `FMS_Alt_Target` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 12 | `FMS_Alt_Target_Valid` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 13 | `FMS_Spd_Target` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 14 | `FMS_TrueHeading_Target` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 15 | `FMS_Track_Target` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 16 | `FMS_Spd_Target_Valid` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 17 | `FMS_TrueHeading_Target_Valid` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 18 | `FMS_Track_Target_Valid` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 19 | `FMS_Roll_Command` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 20 | `FMS_Vertical_Speed_Target` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 21 | `FMS_Roll_Command_Status` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 22 | `FMS_Vertical_Speed_Target_Valid` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 23 | `FMS_Trans_Request` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 24 | `FMS_LNAV_Request` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 25 | `FMS_VNAV_Request` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 26 | `FMS_VNAV_Status` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 27 | `FMS_FPA_Target` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 28 | `FMS_FPA_Target_Valid` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |

### 5.4 地面站与驾驶员业务数据

<a id="bus-bus_gcsdata"></a>

#### BUS_GcsData

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_GcsData)`。直接字段：**39**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_GCSData/Bus Creator41`、`Calc_GCSData/GcsData`；`Calc_PilotIn_ToClaw/GcsData`；`VE25SP/Model3`、`VE25SP/Model5`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `SW0` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 2 | `SW1` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 3 | `SW2` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 4 | `SW3` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 5 | `SW4` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 6 | `SW5` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 7 | `SW6` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 8 | `SW7` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 9 | `SW8` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 10 | `SW9` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 11 | `StickType` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 12 | `KeyAP` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 13 | `KeyTRC` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 14 | `ValidBits` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 15 | `KeyEmergency` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 16 | `KeyMultcopterProt` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 17 | `KeyBackupFccAct` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 18 | `KeyFcModeSel` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 19 | `KeyTransAct` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 20 | `KeyAirGroundSW` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 21 | `KeyLockSW` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 22 | `chAileron` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 23 | `chElevator` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 24 | `chThrottle` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 25 | `chRudder` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 26 | `PB_FD` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 27 | `PB_AP` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 28 | `PB_AT` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 29 | `PB_FMSSpd` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 30 | `PB_Syc` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 31 | `PB_Hdg` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 32 | `PB_LNav` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 33 | `PB_VNav` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 34 | `PB_VS` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 35 | `PB_Alt` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 36 | `AltTarget` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 37 | `VertSpdTarget` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 38 | `HdgTarget` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 39 | `AirSpdTarget` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_pilotindata"></a>

#### BUS_PilotInData

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_PilotInData)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_PilotInData/PilotInData`、`Calc_PilotInData/Voter/Bus Creator`；`Calc_PilotIn_ToClaw/PilotInData`；`VE25SP/Model5`、`VE25SP/Model7`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `VDT` | [Bus: tSingleData](#bus-tsingledata) | 6 | Fixed | real | — | — | — |
| 2 | `GPIO` | [Bus: tU32Data](#bus-tu32data) | 14 | Fixed | real | — | — | — |

<a id="bus-bus_pilotcmpstate"></a>

#### BUS_PilotCmpState

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_PilotCmpState)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_PilotInData/SignalComparotor1/For Each Subsystem/Bus Creator3`、`Calc_PilotInData/SignalComparotor1/For Each Subsystem1/Bus Creator3`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Current` | [Bus: BUS_CmpState](#bus-bus_cmpstate) | 1 | Fixed | real | — | — | — |
| 2 | `Left` | [Bus: BUS_CmpState](#bus-bus_cmpstate) | 1 | Fixed | real | — | — | — |
| 3 | `Right` | [Bus: BUS_CmpState](#bus-bus_cmpstate) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_gpiostate"></a>

#### Bus_GPIOState

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=Bus_GPIOState)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_PilotInData/Subsystem1/Bus Creator10`、`Calc_PilotInData/Subsystem1/Bus Creator11`、`Calc_PilotInData/Subsystem1/Bus Creator12` 等 14 处。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Current` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 2 | `Left` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |
| 3 | `Right` | [Bus: SpData_Enum](#bus-spdata_enum) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_vdtstate"></a>

#### Bus_VDTState

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=Bus_VDTState)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_PilotInData/Subsystem1/Bus Creator`、`Calc_PilotInData/Subsystem1/Bus Creator1`、`Calc_PilotInData/Subsystem1/Bus Creator2` 等 6 处。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Current` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 2 | `Left` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |
| 3 | `Right` | [Bus: SpData_Single](#bus-spdata_single) | 1 | Fixed | real | — | — | — |

### 5.5 系统状态

<a id="bus-bus_sysinfodata"></a>

#### BUS_SysInfoData

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SysInfoData)`。直接字段：**17**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_CcdlMsg_Out/SysInfoData`；`Calc_SysInfo/Bus Creator1`、`Calc_SysInfo/SysInfoData`；`VE25SP/Model1`、`VE25SP/Model9`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `CurrentFccReady` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `FccLossRedundancy` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `LiftMcuLossRedundancy` | boolean | 1 | Fixed | real | — | — | — |
| 4 | `ThrustLossRedundancy` | boolean | 1 | Fixed | real | — | — | — |
| 5 | `ScuLossRedundancy` | boolean | 1 | Fixed | real | — | — | — |
| 6 | `AdahruLossRedundancy` | boolean | 1 | Fixed | real | — | — | — |
| 7 | `RaLossRedundancy` | boolean | 1 | Fixed | real | — | — | — |
| 8 | `MpioLossRedundancy` | boolean | 1 | Fixed | real | — | — | — |
| 9 | `GcsLossRedundancy` | boolean | 1 | Fixed | real | — | — | — |
| 10 | `FtadcLossRedundancy` | boolean | 1 | Fixed | real | — | — | — |
| 11 | `VdtLossRedundancy` | boolean | 1 | Fixed | real | — | — | — |
| 12 | `CurrentSysReady` | boolean | 1 | Fixed | real | — | — | — |
| 13 | `CurrentLimitDispatchInfo` | uint8 | 1 | Fixed | real | — | — | — |
| 14 | `LeftSysReady` | boolean | 1 | Fixed | real | — | — | — |
| 15 | `LeftLimitDispatchInfo` | uint8 | 1 | Fixed | real | — | — | — |
| 16 | `RightSysReady` | boolean | 1 | Fixed | real | — | — | — |
| 17 | `RightLimitDispatchInfo` | uint8 | 1 | Fixed | real | — | — | — |

### 5.6 对外输出与报文

<a id="bus-bus_nav_in"></a>

#### BUS_NAV_IN

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_NAV_IN)`。直接字段：**42**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_CcdlMsg_Out/Claw_Nav_Msg`、`Calc_CcdlMsg_Out/Calc_SysReady_LimitDispatchInfo/Claw_Nav_Msg`；`Calc_NAVMsg_ToClaw/Claw_Nav_Msg`、`Calc_NAVMsg_ToClaw/If Action Subsystem/Bus Creator2`、`Calc_NAVMsg_ToClaw/If Action Subsystem/Claw_Nav_Msg`；`VE25SP/Model4`、`VE25SP/Model9`、`VE25SP/NAV_Root`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Valid_Status` | [Bus: BUS_FCS_NAV_Valid_Status](#bus-bus_fcs_nav_valid_status) | 1 | Fixed | real | — | — | — |
| 2 | `Reset_Status` | [Bus: BUS_FCS_NAV_Reset_Status](#bus-bus_fcs_nav_reset_status) | 1 | Fixed | real | — | — | — |
| 3 | `Lat_deg` | double | 1 | Fixed | real | — | — | — |
| 4 | `Lon_deg` | double | 1 | Fixed | real | — | — | — |
| 5 | `h_AMSL_m` | single | 1 | Fixed | real | — | — | — |
| 6 | `h_BARO_m` | single | 1 | Fixed | real | — | — | — |
| 7 | `h_AGL_m` | single | 1 | Fixed | real | — | — | — |
| 8 | `Vel_x_REO_mps` | single | 1 | Fixed | real | — | — | — |
| 9 | `Vel_y_REO_mps` | single | 1 | Fixed | real | — | — | — |
| 10 | `Vel_z_REO_mps` | single | 1 | Fixed | real | — | — | — |
| 11 | `Vel_CAS_mps` | single | 1 | Fixed | real | — | — | — |
| 12 | `Vel_TAS_mps` | single | 1 | Fixed | real | — | — | — |
| 13 | `Att_phi_rad` | single | 1 | Fixed | real | — | — | — |
| 14 | `Att_theta_rad` | single | 1 | Fixed | real | — | — | — |
| 15 | `Att_psi_rad` | single | 1 | Fixed | real | — | — | — |
| 16 | `Sfor_x_RB_mps2` | single | 1 | Fixed | real | — | — | — |
| 17 | `Sfor_y_RB_mps2` | single | 1 | Fixed | real | — | — | — |
| 18 | `Sfor_z_RB_mps2` | single | 1 | Fixed | real | — | — | — |
| 19 | `Nx_IRU_g` | single | 1 | Fixed | real | — | — | — |
| 20 | `Ny_IRU_g` | single | 1 | Fixed | real | — | — | — |
| 21 | `Nz_IRU_g` | single | 1 | Fixed | real | — | — | — |
| 22 | `Rot_p_rps` | single | 1 | Fixed | real | — | — | — |
| 23 | `Rot_q_rps` | single | 1 | Fixed | real | — | — | — |
| 24 | `Rot_r_rps` | single | 1 | Fixed | real | — | — | — |
| 25 | `Alpha_ADS_deg` | single | 1 | Fixed | real | — | — | — |
| 26 | `Beta_ADS_deg` | single | 1 | Fixed | real | — | — | — |
| 27 | `WOW_LG1_b` | boolean | 1 | Fixed | real | — | — | — |
| 28 | `WOWVld_LG1_b` | boolean | 1 | Fixed | real | — | — | — |
| 29 | `WOW_LG2_b` | boolean | 1 | Fixed | real | — | — | — |
| 30 | `WOWVld_LG2_b` | boolean | 1 | Fixed | real | — | — | — |
| 31 | `WOW_LG3_b` | boolean | 1 | Fixed | real | — | — | — |
| 32 | `WOWVld_LG3_b` | boolean | 1 | Fixed | real | — | — | — |
| 33 | `WOW_LG4_b` | boolean | 1 | Fixed | real | — | — | — |
| 34 | `WOWVld_LG4_b` | boolean | 1 | Fixed | real | — | — | — |
| 35 | `AirGroundSwitchVld_ALL_b` | boolean | 1 | Fixed | real | — | — | — |
| 36 | `AirGroundSwitch_OnGnd_b` | boolean | 1 | Fixed | real | — | — | — |
| 37 | `Radio_Altitude1` | single | 1 | Fixed | real | — | — | — |
| 38 | `Radio_Altitude2` | single | 1 | Fixed | real | — | — | — |
| 39 | `Radio_Altitude1_Valid_b` | boolean | 1 | Fixed | real | — | — | — |
| 40 | `Radio_Altitude2_Valid_b` | boolean | 1 | Fixed | real | — | — | — |
| 41 | `Frt_psi_Pa` | single | 1 | Fixed | real | — | — | — |
| 42 | `Frt_psi_Valid_b` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-bus_fcs_nav_valid_status"></a>

#### BUS_FCS_NAV_Valid_Status

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_FCS_NAV_Valid_Status)`。直接字段：**14**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_NAVMsg_ToClaw/If Action Subsystem/Bus Creator`。

作为成员出现于：[BUS_NAV_IN](#bus-bus_nav_in).`Valid_Status` [1]；[Claw_Nav_Msg](#bus-claw_nav_msg).`Valid_Status` [1]；[BUS_SP2DRMS_MSG](#bus-bus_sp2drms_msg).`navValid` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Pos_XY_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `h_AMSL_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `h_BARO_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 4 | `h_AGL_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 5 | `Vel_XY_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 6 | `Vel_Z_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 7 | `Vel_CAS_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 8 | `Vel_TAS_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 9 | `Alpha_ADS_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 10 | `Beta_ADS_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 11 | `Att_RP_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 12 | `Att_Y_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 13 | `Rot_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 14 | `Sfor_Valid` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-bus_fcs_nav_reset_status"></a>

#### BUS_FCS_NAV_Reset_Status

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_FCS_NAV_Reset_Status)`。直接字段：**14**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_NAVMsg_ToClaw/If Action Subsystem/Bus Creator1`。

作为成员出现于：[BUS_NAV_IN](#bus-bus_nav_in).`Reset_Status` [1]；[Claw_Nav_Msg](#bus-claw_nav_msg).`Reset_Status` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Pos_XY_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `h_AMSL_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `h_BARO_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 4 | `h_AGL_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 5 | `Vel_XY_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 6 | `Vel_Z_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 7 | `Vel_CAS_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 8 | `Vel_TAS_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 9 | `Alpha_ADS_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 10 | `Beta_ADS_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 11 | `Att_RP_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 12 | `Att_Y_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 13 | `Rot_Reset` | boolean | 1 | Fixed | real | — | — | — |
| 14 | `Sfor_Reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-bus_fms_in"></a>

#### BUS_FMS_IN

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_FMS_IN)`。直接字段：**20**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_FMSMsg_ToClaw/Constant`、`Calc_FMSMsg_ToClaw/Claw_FMS_In_Msg`；`VE25SP/Model8`、`VE25SP/FMS_In`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Mass_AC_Kg` | single | 1 | Fixed | real | — | Min=0.0；Max=3500.0 | — |
| 2 | `Xcg_AC_100MAC` | single | 1 | Fixed | real | — | Min=0.0；Max=90.0 | — |
| 3 | `FMS_Alt_Target` | single | 1 | Fixed | real | — | — | — |
| 4 | `FMS_Alt_Target_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 5 | `FMS_Spd_Target` | single | 1 | Fixed | real | — | — | — |
| 6 | `FMS_Heading_Target` | single | 1 | Fixed | real | — | — | — |
| 7 | `FMS_Track_Target` | single | 1 | Fixed | real | — | — | — |
| 8 | `FMS_Flight_Path_Angle_Target` | single | 1 | Fixed | real | — | — | — |
| 9 | `FMS_Spd_Target_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 10 | `FMS_Heading_Target_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 11 | `FMS_Track_Target_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 12 | `FMS_Flight_Path_Angle_Target_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 13 | `FMS_Roll_Command` | single | 1 | Fixed | real | — | — | — |
| 14 | `FMS_Vertical_Speed_Target` | single | 1 | Fixed | real | — | — | — |
| 15 | `FMS_Roll_Command_Status` | boolean | 1 | Fixed | real | — | — | — |
| 16 | `FMS_Vertical_Speed_Target_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 17 | `Trans_Request` | boolean | 1 | Fixed | real | — | — | — |
| 18 | `FMS_LNAV_Request` | boolean | 1 | Fixed | real | — | — | — |
| 19 | `FMS_VNAV_Status` | boolean | 1 | Fixed | real | — | — | — |
| 20 | `FMS_VNAV_Request` | [Enum: ENUM_FMS_VNAV_Request](#enum-enum_fms_vnav_request) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_sysinfo_in"></a>

#### BUS_SYSINFO_IN

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SYSINFO_IN)`。直接字段：**5**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_SYSMsg_ToClaw/Bus Creator2`、`Calc_SYSMsg_ToClaw/Claw_SYS_Msg`；`VE25SP/Model6`、`VE25SP/SYSInfo_In`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `SysReady_SYSINFO_b` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `Reset_FCM_b` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `VCU_Status` | [Bus: BUS_SYSINFO_VCU](#bus-bus_sysinfo_vcu) | 1 | Fixed | real | — | — | — |
| 4 | `Motor_Status` | [Bus: BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr) | 1 | Fixed | real | — | — | — |
| 5 | `Actuator_Status` | [Bus: BUS_SYSINFO_ACT](#bus-bus_sysinfo_act) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_sysinfo_vcu"></a>

#### BUS_SYSINFO_VCU

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SYSINFO_VCU)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_SYSMsg_ToClaw/Bus Creator3`。

作为成员出现于：[BUS_SYSINFO_IN](#bus-bus_sysinfo_in).`VCU_Status` [1]；[Claw_Sys_Msg](#bus-claw_sys_msg).`VCU_Status` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `VCU_Valid` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `VCU_SOP_kw` | single | 1 | Fixed | real | — | — | — |

<a id="bus-bus_sysinfo_mtr"></a>

#### BUS_SYSINFO_MTR

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SYSINFO_MTR)`。直接字段：**11**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_SYSMsg_ToClaw/Bus Creator`。

作为成员出现于：[BUS_SYSINFO_IN](#bus-bus_sysinfo_in).`Motor_Status` [1]；[Claw_Sys_Msg](#bus-claw_sys_msg).`Motor_Status` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `LiftMotor1` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |
| 2 | `LiftMotor2` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |
| 3 | `LiftMotor3` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |
| 4 | `LiftMotor4` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |
| 5 | `LiftMotor5` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |
| 6 | `LiftMotor6` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |
| 7 | `LiftMotor7` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |
| 8 | `LiftMotor8` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |
| 9 | `ThrustMotor1` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |
| 10 | `ThrustMotor2` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |
| 11 | `ThrustMotor3` | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_sysinfo_mtr_state"></a>

#### BUS_SYSINFO_MTR_State

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SYSINFO_MTR_State)`。直接字段：**4**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_SYSMsg_ToClaw/For Each Subsystem/Bus Creator`、`Calc_SYSMsg_ToClaw/For Each Subsystem/McuState`。

作为成员出现于：[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`LiftMotor1` [1]；[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`LiftMotor2` [1]；[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`LiftMotor3` [1]；[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`LiftMotor4` [1]；[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`LiftMotor5` [1]；[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`LiftMotor6` [1]；[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`LiftMotor7` [1]；[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`LiftMotor8` [1]；[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`ThrustMotor1` [1]；[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`ThrustMotor2` [1]；[BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr).`ThrustMotor3` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Fault_Status` | uint8 | 1 | Fixed | real | — | — | — |
| 2 | `WorkMode` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `Power_kw` | single | 1 | Fixed | real | — | — | — |
| 4 | `Spd_rpm` | single | 1 | Fixed | real | — | — | — |

<a id="bus-bus_sysinfo_act"></a>

#### BUS_SYSINFO_ACT

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SYSINFO_ACT)`。直接字段：**8**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_SYSMsg_ToClaw/Bus Creator1`。

作为成员出现于：[BUS_SYSINFO_IN](#bus-bus_sysinfo_in).`Actuator_Status` [1]；[Claw_Sys_Msg](#bus-claw_sys_msg).`Actuator_Status` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `AilLO` | [Bus: BUS_SYSINFO_ACT_State](#bus-bus_sysinfo_act_state) | 1 | Fixed | real | — | — | — |
| 2 | `AilLI` | [Bus: BUS_SYSINFO_ACT_State](#bus-bus_sysinfo_act_state) | 1 | Fixed | real | — | — | — |
| 3 | `AilRO` | [Bus: BUS_SYSINFO_ACT_State](#bus-bus_sysinfo_act_state) | 1 | Fixed | real | — | — | — |
| 4 | `AilRI` | [Bus: BUS_SYSINFO_ACT_State](#bus-bus_sysinfo_act_state) | 1 | Fixed | real | — | — | — |
| 5 | `EleL` | [Bus: BUS_SYSINFO_ACT_State](#bus-bus_sysinfo_act_state) | 1 | Fixed | real | — | — | — |
| 6 | `EleR` | [Bus: BUS_SYSINFO_ACT_State](#bus-bus_sysinfo_act_state) | 1 | Fixed | real | — | — | — |
| 7 | `RudL` | [Bus: BUS_SYSINFO_ACT_State](#bus-bus_sysinfo_act_state) | 1 | Fixed | real | — | — | — |
| 8 | `RudR` | [Bus: BUS_SYSINFO_ACT_State](#bus-bus_sysinfo_act_state) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_sysinfo_act_state"></a>

#### BUS_SYSINFO_ACT_State

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SYSINFO_ACT_State)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`SignalProcessingLib/GetActState/Bus Creator`。

作为成员出现于：[BUS_SYSINFO_ACT](#bus-bus_sysinfo_act).`AilLO` [1]；[BUS_SYSINFO_ACT](#bus-bus_sysinfo_act).`AilLI` [1]；[BUS_SYSINFO_ACT](#bus-bus_sysinfo_act).`AilRO` [1]；[BUS_SYSINFO_ACT](#bus-bus_sysinfo_act).`AilRI` [1]；[BUS_SYSINFO_ACT](#bus-bus_sysinfo_act).`EleL` [1]；[BUS_SYSINFO_ACT](#bus-bus_sysinfo_act).`EleR` [1]；[BUS_SYSINFO_ACT](#bus-bus_sysinfo_act).`RudL` [1]；[BUS_SYSINFO_ACT](#bus-bus_sysinfo_act).`RudR` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Fault_Status` | [Enum: ENUM_ACTState](#enum-enum_actstate) | 1 | Fixed | real | — | — | — |
| 2 | `Surface_deg` | single | 1 | Fixed | real | — | Min=-35.0；Max=35.0 | — |

<a id="bus-bus_pilot_in"></a>

#### BUS_PILOT_IN

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_PILOT_IN)`。直接字段：**22**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_CcdlMsg_Out/PilotIn`、`Calc_CcdlMsg_Out/RC`、`Calc_CcdlMsg_Out/Cockpit_In`；`Calc_PilotIn_ToClaw/PilotData`、`Calc_PilotIn_ToClaw/PilotIn`、`Calc_PilotIn_ToClaw/RC` 等 9 处；`Calc_SPDispMsg_Out/PilotIn`、`Calc_SPDispMsg_Out/RC`、`Calc_SPDispMsg_Out/Cockpit_In` 等 4 处；`VE25SP/Model10`、`VE25SP/Model5`、`VE25SP/Model9` 等 4 处。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `SSX_CP_deg` | single | 1 | Fixed | real | — | Min=-11.0；Max=11.0 | — |
| 2 | `SSXVld_CP_b` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `SSY_CP_deg` | single | 1 | Fixed | real | — | Min=-11.0；Max=11.0 | — |
| 4 | `SSYVld_CP_b` | boolean | 1 | Fixed | real | — | — | — |
| 5 | `SSX_FO_deg` | single | 1 | Fixed | real | — | Min=-11.0；Max=11.0 | — |
| 6 | `SSXVld_FO_b` | boolean | 1 | Fixed | real | — | — | — |
| 7 | `SSY_FO_deg` | single | 1 | Fixed | real | — | Min=-11.0；Max=11.0 | — |
| 8 | `SSYVld_FO_b` | boolean | 1 | Fixed | real | — | — | — |
| 9 | `CSX_PltIn_deg` | single | 1 | Fixed | real | — | Min=-12.0；Max=10.0 | — |
| 10 | `CSXVld_PltIn_b` | boolean | 1 | Fixed | real | — | — | — |
| 11 | `CSY_PltIn_deg` | single | 1 | Fixed | real | — | Min=-11.0；Max=11.0 | — |
| 12 | `CSYVld_PltIn_b` | boolean | 1 | Fixed | real | — | — | — |
| 13 | `PwrOn_PltIn_b` | boolean | 1 | Fixed | real | — | — | — |
| 14 | `PwrOnVld_PltIn_b` | boolean | 1 | Fixed | real | — | — | — |
| 15 | `TRCOn_PltIn_b` | boolean | 1 | Fixed | real | — | — | — |
| 16 | `MCProtOn_PltIn_b` | boolean | 1 | Fixed | real | — | — | — |
| 17 | `MFD_PltIn` | [Enum: ENUM_Key_MFD](#enum-enum_key_mfd) | 1 | Fixed | real | — | — | — |
| 18 | `TEST_IN` | [Bus:BUS_TEST_IN](#bus-bus_test_in) | 1 | Fixed | real | — | — | — |
| 19 | `HiVltgSts` | [Enum:ENUM_HiVltg_Sts](#enum-enum_hivltg_sts) | 1 | Fixed | real | — | — | — |
| 20 | `AP_Disengage_SW` | boolean | 1 | Fixed | real | — | — | — |
| 21 | `LiftMotor_lock_SW_b` | boolean | 1 | Fixed | real | — | — | — |
| 22 | `PilotSource` | [Enum: ENUM_PilotSource](#enum-enum_pilotsource) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_test_in"></a>

#### BUS_TEST_IN

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_TEST_IN)`。直接字段：**7**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_PilotIn_ToClaw/handlePilotIn/Constant1`。

作为成员出现于：[BUS_PILOT_IN](#bus-bus_pilot_in).`TEST_IN` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Enable_Degrade_b` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `Enable_THRDir_b` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `Enable_PLTLost_b` | boolean | 1 | Fixed | real | — | — | — |
| 4 | `Request_Stop_GNDStation_b` | boolean | 1 | Fixed | real | — | — | — |
| 5 | `Enable_SwpTest_b` | boolean | 1 | Fixed | real | — | — | — |
| 6 | `Enable_ResetFromExt_b` | boolean | 1 | Fixed | real | — | — | — |
| 7 | `Init_X0_Vector` | single | 5 | Fixed | real | — | — | — |

<a id="bus-bus_spdispmsg"></a>

#### BUS_SpDispMsg

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SpDispMsg)`。直接字段：**19**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_SPDispMsg_Out/SpDispMsg`、`Calc_SPDispMsg_Out/Calc_SpDispMsg/Bus Creator`、`Calc_SPDispMsg_Out/Calc_SpDispMsg/SpDispMsg`；`VE25SP/Model10`、`VE25SP/SPDispMsgOut`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `PilotState` | uint8 | 1 | Fixed | real | — | — | — |
| 2 | `Captain_remote_Pos_vaild` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `Central_remote_Pos_valid` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `Captain_remote_Pitch_Pos` | single | 1 | Fixed | real | — | — | — |
| 5 | `Captain_remote_Roll_Pos` | single | 1 | Fixed | real | — | — | — |
| 6 | `Central_remote_Pitch_Pos` | single | 1 | Fixed | real | — | — | — |
| 7 | `Central_remote_Roll_Pos` | single | 1 | Fixed | real | — | — | — |
| 8 | `Captain_SSU_Pos_vaild` | uint8 | 1 | Fixed | real | — | — | — |
| 9 | `Central_SSU_Pos_valid` | uint8 | 1 | Fixed | real | — | — | — |
| 10 | `FO_SSU_Pos_valid` | uint8 | 1 | Fixed | real | — | — | — |
| 11 | `Captain_sidestick_Pitch_Pos` | single | 1 | Fixed | real | — | — | — |
| 12 | `Captain_sidestick_Roll_Pos` | single | 1 | Fixed | real | — | — | — |
| 13 | `Central_sidestick_Pitch_Pos` | single | 1 | Fixed | real | — | — | — |
| 14 | `Central_sidestick_Roll_Pos` | single | 1 | Fixed | real | — | — | — |
| 15 | `FO_sidestick_Pitch_Pos` | single | 1 | Fixed | real | — | — | — |
| 16 | `FO_sidestick_Roll_Pos` | single | 1 | Fixed | real | — | — | — |
| 17 | `systemReady` | uint8 | 1 | Fixed | real | — | — | — |
| 18 | `LimitDispatchInfo` | uint8 | 1 | Fixed | real | — | — | — |
| 19 | `HiVltgSW` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-bus_spmsgout"></a>

#### BUS_SpMsgOut

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SpMsgOut)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_CcdlMsg_Out/SpCcdlOut`、`Calc_CcdlMsg_Out/Calc_SysReady_LimitDispatchInfo/Constant11`；`VE25SP/Model9`、`VE25SP/SpCcdlMsgOut`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Data` | [SpCcdlMsg](#bus-spccdlmsg) | 1 | Fixed | real | — | — | — |
| 2 | `Sno` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `AppCRC` | uint16 | 1 | Fixed | real | — | — | — |

<a id="bus-spccdlmsg"></a>

#### SpCcdlMsg

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpCcdlMsg)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[BUS_SpMsgOut](#bus-bus_spmsgout).`Data` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `SysReady` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `LimitDispathInfo` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-bus_sp2drms_msg"></a>

#### BUS_SP2DRMS_MSG

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SP2DRMS_MSG)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`VE25SP/Constant`、`VE25SP/SP2DRMS`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `navValid` | [Bus: BUS_FCS_NAV_Valid_Status](#bus-bus_fcs_nav_valid_status) | 1 | Fixed | real | — | — | — |
| 2 | `isBfccActvValid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `isBfccActv` | boolean | 1 | Fixed | real | — | — | — |

### 5.7 公共数据与状态结构

<a id="bus-bus_doublesignalnormalinfo"></a>

#### Bus_DoubleSignalNormalInfo

来源：[DD_SpLib.sldd](<D:/666/VE25/sp/04_Design/common/specification/data/DD_SpLib.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=Bus_DoubleSignalNormalInfo)`。直接字段：**4**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`SignalProcessingLib/Calc_SignalState_Double/For Each Subsystem1/Bus Creator`。

作为成员出现于：[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`CombLate` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`CombLong` [3]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `state` | [Bus: tSignalState](#bus-tsignalstate) | 1 | Fixed | real | — | — | — |
| 2 | `data` | double | 1 | Fixed | real | — | — | — |
| 3 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 4 | `isSynced` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-tsignalstate"></a>

#### tSignalState

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tSignalState)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_NavSourceSelection/SourceSelection/output/calcData/Constant1`；`SignalProcessingLib/Calc_SignalState/For Each Subsystem1/Bus Creator1`、`SignalProcessingLib/Calc_SignalState_Double/For Each Subsystem1/Bus Creator1`。

作为成员出现于：[BUS_MpioDataState](#bus-bus_mpiodatastate).`IduDataMsg` [1]；[BUS_DevDataState](#bus-bus_devdatastate).`LiftMcuFbkMsg` [8]；[BUS_DevDataState](#bus-bus_devdatastate).`LiftMcuNormalMsg` [8]；[BUS_DevDataState](#bus-bus_devdatastate).`ThrustMcuFbkMsg` [3]；[BUS_DevDataState](#bus-bus_devdatastate).`ThrustMcuNormalMsg` [3]；[BUS_DevDataState](#bus-bus_devdatastate).`ServoFbkMsg` [8]；[BUS_NavDataState](#bus-bus_navdatastate).`ftadcMsg` [1]；[tRedundantDataState](#bus-tredundantdatastate).`ch1SignalState` [1]；[tRedundantDataState](#bus-tredundantdatastate).`ch2SignalState` [1]；[tRedundantDataState](#bus-tredundantdatastate).`ch3SignalState` [1]；[Bus_DoubleSignalNormalInfo](#bus-bus_doublesignalnormalinfo).`state` [1]；[Bus_SingleSignalNormalInfo](#bus-bus_singlesignalnormalinfo).`state` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `lastTimestamp` | uint32 | 1 | Fixed | real | Ms | — | — |
| 2 | `stateBits` | uint32 | 1 | Fixed | real | — | — | Bits define:<br>0 - valid<br>1 - updated<br>2 - isTimeout<br>3 - isAbnormal<br>4 - isOutOfRange<br>5 - isMissCompare<br>6 - isBanned<br><br>Others - undefined |

**字典描述与当前实现的差异：**上表保留 Description 原文，其 stateBits 描述列出 valid、updated、isTimeout、isAbnormal、isOutOfRange、isMissCompare、isBanned 七位。当前 SignalProcessingLib/SignalStateSet_New 实际按 valid、isSynced、isMissCompare、isBanned 的权重 1、2、4、8 生成 stateBits。解释该 New 实现时应依据对应块及连线；此处只记录来源差异，未完成设计一致性确认，不能据此直接认定运行故障。

实现定位：[SignalProcessingLib.slx](<D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx>)；system_2514.xml 的输入 SID2515/2520/2521/2522，常量 SID2524/2529/2530/2531，Product SID2532/2537/2538/2539 → Add SID2523 → stateBits SID2540；system_2566.xml 中 SID2582 → Bus Creator SID2574 的 stateBits 输入，输出类型 Bus: tSignalState。

<a id="bus-bus_singlesignalnormalinfo"></a>

#### Bus_SingleSignalNormalInfo

来源：[DD_SpLib.sldd](<D:/666/VE25/sp/04_Design/common/specification/data/DD_SpLib.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=Bus_SingleSignalNormalInfo)`。直接字段：**4**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_NavSourceSelection/Model`；`Calc_VotedCAS/CAS`；`Calc_VotedRA/raNormalInfos`；`RAMsgsPreprocess/raNormalInfos`；`SignalProcessingLib/Calc_SignalState/For Each Subsystem1/Bus Creator`；`VE25SP/NavSpApp/Model`、`VE25SP/NavSpApp/raVoter`。

作为成员出现于：[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`GpsHeight` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`AOA` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`AOS` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`GroundSpeed` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`PressureAlt` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`BaroCorrectedAlt` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`Mach` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`CAS` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`TAS` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`GpsAltRate` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`AltRate` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`StaticAirTemp` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`TotalPressure` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`StaticPressure` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`MagnHeading` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`TrueHeaading` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`MagDeclination` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`VertVelocity` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`NorthVelocity` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`EastVelocity` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`Pitch` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`Roll` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`SforX` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`SforY` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`SforZ` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`Q` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`P` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`R` [3]；[BUS_adahruNormalInfos](#bus-bus_adahrunormalinfos).`AdAltRate` [3]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `state` | [Bus: tSignalState](#bus-tsignalstate) | 1 | Fixed | real | — | — | — |
| 2 | `data` | single | 1 | Fixed | real | — | — | — |
| 3 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 4 | `isSynced` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-spdata_double"></a>

#### SpData_Double

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpData_Double)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`AdahruMsgsPreprocess/Subsystem/IN`；`SignalProcessingLib/Calc_SignalState_Double/DataIn1`、`SignalProcessingLib/Calc_SignalState_Double/ForEachSubsystem/In1`。

作为成员出现于：[SpIn_Adahrus](#bus-spin_adahrus).`L310_Combined_Latitude` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L311_Combined_Longitude` [3]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | double | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-spdata_enum"></a>

#### SpData_Enum

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpData_Enum)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[Bus_GPIOState](#bus-bus_gpiostate).`Current` [1]；[Bus_GPIOState](#bus-bus_gpiostate).`Left` [1]；[Bus_GPIOState](#bus-bus_gpiostate).`Right` [1]；[SpIn_Key](#bus-spin_key).`Key_AirGnd` [1]；[SpIn_Key](#bus-spin_key).`Key_ELFS` [1]；[SpIn_Key](#bus-spin_key).`Key_PfcModeSW` [1]；[SpIn_Key](#bus-spin_key).`Key_McProtect` [1]；[SpIn_Key](#bus-spin_key).`Key_Backup` [1]；[SpIn_Key](#bus-spin_key).`Key_TRC` [1]；[SpIn_Key](#bus-spin_key).`Key_Ap_L` [1]；[SpIn_Key](#bus-spin_key).`Key_Ap_R` [1]；[SpIn_Key](#bus-spin_key).`Key_RSSU_BackupActive` [1]；[SpIn_Key](#bus-spin_key).`Key_CSSU_BackupActive` [1]；[SpIn_Key](#bus-spin_key).`Key_SSU_BackupUp` [1]；[SpIn_Key](#bus-spin_key).`Key_SSU_BackupDown` [1]；[SpIn_Key](#bus-spin_key).`Key_SSU_BackupLeft` [1]；[SpIn_Key](#bus-spin_key).`Key_SSU_BackupRight` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | uint32 | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-spdata_single"></a>

#### SpData_Single

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpData_Single)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`AdahruMsgsPreprocess/Subsystem1/IN`、`AdahruMsgsPreprocess/Subsystem2/IN`；`RAMsgsPreprocess/Constant`；`SignalProcessingLib/Calc_SignalState/DataIn1`、`SignalProcessingLib/Calc_SignalState/ForEachSubsystem/In1`。

作为成员出现于：[Bus_VDTState](#bus-bus_vdtstate).`Current` [1]；[Bus_VDTState](#bus-bus_vdtstate).`Left` [1]；[Bus_VDTState](#bus-bus_vdtstate).`Right` [1]；[SpIn_Ra](#bus-spin_ra).`L164_Radio_Altitude` [2]；[SpIn_Sitck](#bus-spin_sitck).`CSSU_X` [1]；[SpIn_Sitck](#bus-spin_sitck).`CSSU_Y` [1]；[SpIn_Sitck](#bus-spin_sitck).`RSSU_X` [1]；[SpIn_Sitck](#bus-spin_sitck).`RSSU_Y` [1]；[SpIn_Sitck](#bus-spin_sitck).`LSSU_X` [1]；[SpIn_Sitck](#bus-spin_sitck).`LSSU_Y` [1]；[SpIn_ftadc](#bus-spin_ftadc).`Psi` [1]；[SpIn_ftadc](#bus-spin_ftadc).`Pti` [1]；[SpIn_ftadc](#bus-spin_ftadc).`AOA` [1]；[SpIn_ftadc](#bus-spin_ftadc).`AOS` [1]；[SpIn_ftadc](#bus-spin_ftadc).`CAS` [1]；[SpIn_Adahrus](#bus-spin_adahrus).`L076_GPS_Height` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L107_Angle_Of_Attack` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L110_Sideslip_Angle` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L112_Ground_Speed` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L203_Pressure_Altitude` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L204_Baro_Corrected_Altitude` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L205_Mach` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L206_Computed_Airspeed` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L210_True_Airspeed` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L211_Comb_Altitude_Rate` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L212_Altitude_Rate` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L213_Static_Air_Temperature` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L242_Total_Pressure` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L245_Static_Pressure` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L320_Magnetic_Heading` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L321_True_Heading` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L322_Mag_Declination` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L365_Vertical_Velocity` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L366_North_Velocity` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L367_East_Velocity` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L324_Pitch_Angle` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L325_Roll_Angle` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L331_Body_Longit_Accel` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L332_Body_Lateral_Accel` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L333_Body_Normal_Accel` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L336_Pitch_Attitude_Rate` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L337_Roll_Attitude_Rate` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`L340_Heading_Angle_Rate` [3]；[SpIn_Adahrus](#bus-spin_adahrus).`AdaComb_Altitude_Rate` [3]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | single | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-tdoubledata"></a>

#### tDoubleData

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tDoubleData)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_NavSourceSelection/SourceSelection/output/calcData/Bus Creator3`、`Calc_NavSourceSelection/SourceSelection/output/calcData/Bus Creator4`。

作为成员出现于：[BUS_NavData](#bus-bus_navdata).`adahruLongitude_Deg` [1]；[BUS_NavData](#bus-bus_navdata).`adahruLatitude_Deg` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | double | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-tmcumsg"></a>

#### tMcuMsg

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tMcuMsg)`。直接字段：**10**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_MCUSCUData/ForEachSubsystem/Constant`、`Calc_MCUSCUData/ForEachSubsystem/LiftMcuData`、`Calc_MCUSCUData/ForEachSubsystem1/Constant` 等 4 处。

作为成员出现于：[BUS_DevData](#bus-bus_devdata).`LiftMCU` [8]；[BUS_DevData](#bus-bus_devdata).`ThrustMCU` [3]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `SpdFbk` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 2 | `Power` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 3 | `FaultLevel` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 4 | `WorkMode` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 5 | `PowerPercentInvalid` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 6 | `PowerPercent` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 7 | `Temp` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 8 | `MotorTemp` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 9 | `WarnCode` | [Bus: tU64Data](#bus-tu64data) | 1 | Fixed | real | — | — | — |
| 10 | `FaultCode` | [Bus: tU64Data](#bus-tu64data) | 1 | Fixed | real | — | — | — |

<a id="bus-tsingledata"></a>

#### tSingleData

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tSingleData)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_GCSData/Bus Creator21`、`Calc_GCSData/Bus Creator22`、`Calc_GCSData/Bus Creator23` 等 8 处；`Calc_MCUSCUData/ForEachSubsystem/Bus Creator1`、`Calc_MCUSCUData/ForEachSubsystem/Bus Creator2`、`Calc_MCUSCUData/ForEachSubsystem1/Bus Creator1` 等 5 处；`Calc_MPIOData/Bus Creator10`、`Calc_MPIOData/Bus Creator12`、`Calc_MPIOData/Bus Creator13` 等 10 处；`Calc_NAVMsg_ToClaw/If Action Subsystem/Subsystem1/Bus Creator`；`Calc_NavSourceSelection/Model`、`Calc_NavSourceSelection/SourceSelection/output/calcData/Bus Creator10`、`Calc_NavSourceSelection/SourceSelection/output/calcData/Bus Creator11` 等 40 处；`Calc_PilotInData/Voter/For Each Subsystem/Bus Creator`；`Calc_VotedCAS/Bus Creator`、`Calc_VotedCAS/voterCAS`；`Calc_VotedFTADC/Bus Creator1`、`Calc_VotedFTADC/Bus Creator2`、`Calc_VotedFTADC/Bus Creator3` 等 5 处；`Calc_VotedRA/SourceSelection/output/calcData/Bus Creator1`；`VE25SP/NavSpApp/Subsystem/Subsystem/Bus Creator`。

作为成员出现于：[BUS_NavData](#bus-bus_navdata).`adahruGpsHeight_M` [1]；[BUS_NavData](#bus-bus_navdata).`adahruPressureAlt_M` [1]；[BUS_NavData](#bus-bus_navdata).`adahruBaroCorrectedAlt_M` [1]；[BUS_NavData](#bus-bus_navdata).`adahruAOA_Deg` [1]；[BUS_NavData](#bus-bus_navdata).`adahruAOS_Deg` [1]；[BUS_NavData](#bus-bus_navdata).`adahruGroundSpeed_MPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruMach_Ma` [1]；[BUS_NavData](#bus-bus_navdata).`adahruCAS_MPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruTAS_MPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruGpsAltRate_MPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruAdAltRate_MPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruAltRate_MPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruStaticTemp_C` [1]；[BUS_NavData](#bus-bus_navdata).`adahruTotalPressure_hPa` [1]；[BUS_NavData](#bus-bus_navdata).`adahruStaticPressure_hPa` [1]；[BUS_NavData](#bus-bus_navdata).`adahruMagnHeading_Deg` [1]；[BUS_NavData](#bus-bus_navdata).`adahruTrueHeading_Deg` [1]；[BUS_NavData](#bus-bus_navdata).`adahruMagDeclination_Deg` [1]；[BUS_NavData](#bus-bus_navdata).`adahruVelU_MPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruVelN_MPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruVelE_MPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruPitch_Deg` [1]；[BUS_NavData](#bus-bus_navdata).`adahruRoll_Deg` [1]；[BUS_NavData](#bus-bus_navdata).`adahruSforX_MPS2` [1]；[BUS_NavData](#bus-bus_navdata).`adahruSforY_MPS2` [1]；[BUS_NavData](#bus-bus_navdata).`adahruSforZ_MPS2` [1]；[BUS_NavData](#bus-bus_navdata).`adahruP_DegPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruQ_DegPS` [1]；[BUS_NavData](#bus-bus_navdata).`adahruR_DegPS` [1]；[BUS_NavData](#bus-bus_navdata).`raRadioAlt_M` [1]；[BUS_NavData](#bus-bus_navdata).`ra1RadioAlt_M` [1]；[BUS_NavData](#bus-bus_navdata).`ra2RadioAlt_M` [1]；[BUS_NavData](#bus-bus_navdata).`ftadcStaticPressure_kPa` [1]；[BUS_NavData](#bus-bus_navdata).`ftadcTotalPressure_kPa` [1]；[BUS_NavData](#bus-bus_navdata).`ftadcAttackOfAngle_Deg` [1]；[BUS_NavData](#bus-bus_navdata).`ftadcSideslipAngle_Deg` [1]；[BUS_NavData](#bus-bus_navdata).`ftadcCAS_mps` [1]；[BUS_NavData](#bus-bus_navdata).`VoterCAS_mps` [1]；[BUS_NavData](#bus-bus_navdata).`ftadcVoterCAS_mps` [1]；[tMcuMsg](#bus-tmcumsg).`SpdFbk` [1]；[tMcuMsg](#bus-tmcumsg).`Power` [1]；[tMcuMsg](#bus-tmcumsg).`Temp` [1]；[tMcuMsg](#bus-tmcumsg).`MotorTemp` [1]；[tServoMsg](#bus-tservomsg).`Position` [1]；[BUS_GcsData](#bus-bus_gcsdata).`chAileron` [1]；[BUS_GcsData](#bus-bus_gcsdata).`chElevator` [1]；[BUS_GcsData](#bus-bus_gcsdata).`chThrottle` [1]；[BUS_GcsData](#bus-bus_gcsdata).`chRudder` [1]；[BUS_GcsData](#bus-bus_gcsdata).`AltTarget` [1]；[BUS_GcsData](#bus-bus_gcsdata).`VertSpdTarget` [1]；[BUS_GcsData](#bus-bus_gcsdata).`HdgTarget` [1]；[BUS_GcsData](#bus-bus_gcsdata).`AirSpdTarget` [1]；[BUS_PilotInData](#bus-bus_pilotindata).`VDT` [6]；[BUS_MpioData](#bus-bus_mpiodata).`UsablePwr_KW` [1]；[BUS_MpioData](#bus-bus_mpiodata).`CenterGravity` [1]；[BUS_MpioData](#bus-bus_mpiodata).`GrossWeight` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Alt_Target` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Spd_Target` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_TrueHeading_Target` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Track_Target` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Roll_Command` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Vertical_Speed_Target` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_FPA_Target` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | single | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-tu8data"></a>

#### tU8Data

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tU8Data)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_GCSData/Bus Creator1`、`Calc_GCSData/Bus Creator10`、`Calc_GCSData/Bus Creator11` 等 31 处；`Calc_MCUSCUData/ForEachSubsystem/Bus Creator3`、`Calc_MCUSCUData/ForEachSubsystem/Bus Creator4`、`Calc_MCUSCUData/ForEachSubsystem1/Bus Creator3` 等 6 处；`Calc_MPIOData/Bus Creator`、`Calc_MPIOData/Bus Creator1`、`Calc_MPIOData/Bus Creator11` 等 17 处。

作为成员出现于：[tMcuMsg](#bus-tmcumsg).`FaultLevel` [1]；[tMcuMsg](#bus-tmcumsg).`WorkMode` [1]；[tMcuMsg](#bus-tmcumsg).`PowerPercentInvalid` [1]；[tMcuMsg](#bus-tmcumsg).`PowerPercent` [1]；[tServoMsg](#bus-tservomsg).`FaultStatus` [1]；[tServoMsg](#bus-tservomsg).`ClawFaultStatus` [1]；[BUS_GcsData](#bus-bus_gcsdata).`SW0` [1]；[BUS_GcsData](#bus-bus_gcsdata).`SW1` [1]；[BUS_GcsData](#bus-bus_gcsdata).`SW2` [1]；[BUS_GcsData](#bus-bus_gcsdata).`SW3` [1]；[BUS_GcsData](#bus-bus_gcsdata).`SW4` [1]；[BUS_GcsData](#bus-bus_gcsdata).`SW5` [1]；[BUS_GcsData](#bus-bus_gcsdata).`SW6` [1]；[BUS_GcsData](#bus-bus_gcsdata).`SW7` [1]；[BUS_GcsData](#bus-bus_gcsdata).`SW8` [1]；[BUS_GcsData](#bus-bus_gcsdata).`SW9` [1]；[BUS_GcsData](#bus-bus_gcsdata).`StickType` [1]；[BUS_GcsData](#bus-bus_gcsdata).`KeyAP` [1]；[BUS_GcsData](#bus-bus_gcsdata).`KeyTRC` [1]；[BUS_GcsData](#bus-bus_gcsdata).`ValidBits` [1]；[BUS_GcsData](#bus-bus_gcsdata).`KeyEmergency` [1]；[BUS_GcsData](#bus-bus_gcsdata).`KeyMultcopterProt` [1]；[BUS_GcsData](#bus-bus_gcsdata).`KeyBackupFccAct` [1]；[BUS_GcsData](#bus-bus_gcsdata).`KeyFcModeSel` [1]；[BUS_GcsData](#bus-bus_gcsdata).`KeyTransAct` [1]；[BUS_GcsData](#bus-bus_gcsdata).`KeyAirGroundSW` [1]；[BUS_GcsData](#bus-bus_gcsdata).`KeyLockSW` [1]；[BUS_GcsData](#bus-bus_gcsdata).`PB_FD` [1]；[BUS_GcsData](#bus-bus_gcsdata).`PB_AP` [1]；[BUS_GcsData](#bus-bus_gcsdata).`PB_AT` [1]；[BUS_GcsData](#bus-bus_gcsdata).`PB_FMSSpd` [1]；[BUS_GcsData](#bus-bus_gcsdata).`PB_Syc` [1]；[BUS_GcsData](#bus-bus_gcsdata).`PB_Hdg` [1]；[BUS_GcsData](#bus-bus_gcsdata).`PB_LNav` [1]；[BUS_GcsData](#bus-bus_gcsdata).`PB_VNav` [1]；[BUS_GcsData](#bus-bus_gcsdata).`PB_VS` [1]；[BUS_GcsData](#bus-bus_gcsdata).`PB_Alt` [1]；[BUS_MpioData](#bus-bus_mpiodata).`EgyWkSts` [1]；[BUS_MpioData](#bus-bus_mpiodata).`HiVltgSts` [1]；[BUS_MpioData](#bus-bus_mpiodata).`HiVltgSW` [1]；[BUS_MpioData](#bus-bus_mpiodata).`SelectedFCC` [1]；[BUS_MpioData](#bus-bus_mpiodata).`MotorCheck` [1]；[BUS_MpioData](#bus-bus_mpiodata).`SurfaceCheck` [1]；[BUS_MpioData](#bus-bus_mpiodata).`PreflightCheckValid` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Alt_Target_Valid` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Spd_Target_Valid` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_TrueHeading_Target_Valid` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Track_Target_Valid` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Roll_Command_Status` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Vertical_Speed_Target_Valid` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_Trans_Request` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_LNAV_Request` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_VNAV_Request` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_VNAV_Status` [1]；[BUS_MpioData](#bus-bus_mpiodata).`FMS_FPA_Target_Valid` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | uint8 | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-tu64data"></a>

#### tU64Data

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tU64Data)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[tMcuMsg](#bus-tmcumsg).`WarnCode` [1]；[tMcuMsg](#bus-tmcumsg).`FaultCode` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | uint64 | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-tnavlog"></a>

#### tNavLog

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tNavLog)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`VE25SP/NavSpApp/Bus Creator2`、`VE25SP/NavSpApp/NavLog`。

作为成员出现于：[BUS_SpLog](#bus-bus_splog).`NavLog` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `AdahruWeight` | uint8 | 3 | Fixed | real | — | — | — |
| 2 | `AdahruSrc` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `RaSrc` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-tredundantdatastate"></a>

#### tRedundantDataState

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tRedundantDataState)`。直接字段：**4**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_NavSourceSelection/SourceSelection/output/calcData/Constant`；`SignalProcessingLib/SrcSelect/Bus Creator4`、`SignalProcessingLib/SrcSelect/state`。

作为成员出现于：[BUS_GcsDataState](#bus-bus_gcsdatastate).`PilotCtrlMsg` [1]；[BUS_GcsDataState](#bus-bus_gcsdatastate).`FMCPMsg` [1]；[BUS_PilotInDataState](#bus-bus_pilotindatastate).`VDT` [6]；[BUS_PilotInDataState](#bus-bus_pilotindatastate).`GPIO` [6]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruLongitude_Deg` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruLatitude_Deg` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruGpsHeight_M` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruPressureAlt_M` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruBaroCorrectedAlt_M` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruAOA_Deg` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruAOS_Deg` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruGroundSpeed_MPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruMach_Ma` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruCAS_MPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruTAS_MPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruGpsAltRate_MPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruAdAltRate_MPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruAltRate_MPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruStaticTemp_C` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruTotalPressure_hPa` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruStaticPressure_hPa` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruMagnHeading_Deg` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruTrueHeading_Deg` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruMagDeclination_Deg` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruVelU_MPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruVelN_MPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruVelE_MPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruPitch_Deg` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruRoll_Deg` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruSforX_MPS2` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruSforY_MPS2` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruSforZ_MPS2` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruP_DegPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruQ_DegPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`adahruR_DegPS` [1]；[BUS_NavDataState](#bus-bus_navdatastate).`raRadioAlt_M` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `ch1SignalState` | [Bus: tSignalState](#bus-tsignalstate) | 1 | Fixed | real | — | — | — |
| 2 | `ch2SignalState` | [Bus: tSignalState](#bus-tsignalstate) | 1 | Fixed | real | — | — | — |
| 3 | `ch3SignalState` | [Bus: tSignalState](#bus-tsignalstate) | 1 | Fixed | real | — | — | — |
| 4 | `voterState` | [Bus: tVoterState](#bus-tvoterstate) | 1 | Fixed | real | — | — | — |

<a id="bus-tvoterstate"></a>

#### tVoterState

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tVoterState)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_GCSData/Subsystem1/Bus Creator`、`Calc_GCSData/Subsystem4/Bus Creator`；`Calc_NavSourceSelection/SourceSelection/output/calcState/Bus Creator`、`Calc_NavSourceSelection/SourceSelection/output/calcState/voterstate`、`Calc_NavSourceSelection/SourceSelection/output/calcState1/Bus Creator` 等 4 处；`Calc_VotedRA/SourceSelection/output/calcState/Bus Creator`、`Calc_VotedRA/SourceSelection/output/calcState/voterstate`。

作为成员出现于：[tRedundantDataState](#bus-tredundantdatastate).`voterState` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `VoterType` | [Enum: Enum_VoterType](#enum-enum_votertype) | 1 | Fixed | real | — | — | — |
| 2 | `ChannelUsed` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-tservomsg"></a>

#### tServoMsg

来源：[SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tServoMsg)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_MCUSCUData/ForEachSubsystem2/Bus Creator5`、`Calc_MCUSCUData/ForEachSubsystem2/ServoData`。

作为成员出现于：[BUS_DevData](#bus-bus_devdata).`AilLO` [1]；[BUS_DevData](#bus-bus_devdata).`AilLI` [1]；[BUS_DevData](#bus-bus_devdata).`AilRO` [1]；[BUS_DevData](#bus-bus_devdata).`AilRI` [1]；[BUS_DevData](#bus-bus_devdata).`EleL` [1]；[BUS_DevData](#bus-bus_devdata).`EleR` [1]；[BUS_DevData](#bus-bus_devdata).`RudL` [1]；[BUS_DevData](#bus-bus_devdata).`RudR` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Position` | [Bus: tSingleData](#bus-tsingledata) | 1 | Fixed | real | — | — | — |
| 2 | `FaultStatus` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |
| 3 | `ClawFaultStatus` | [Bus: tU8Data](#bus-tu8data) | 1 | Fixed | real | — | — | — |

<a id="bus-tu32data"></a>

#### tU32Data

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tU32Data)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

模型／库中的类型声明：`Calc_PilotInData/Voter/For Each Subsystem1/Bus Creator`。

作为成员出现于：[BUS_PilotInData](#bus-bus_pilotindata).`GPIO` [14]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | uint32 | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

### 5.8 字典可见但未定位当前使用

以下类型能够通过当前字典引用找到，但本次未在活动模型/实际链接库的显式类型、保存接口或其嵌套成员中定位到使用。它们可能是预留、历史兼容或其他用途；没有据此认定其设计目的，也没有判定可删除。

<a id="bus-bus_devdatastate"></a>

#### BUS_DevDataState

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_DevDataState)`。直接字段：**5**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `LiftMcuFbkMsg` | [Bus: tSignalState](#bus-tsignalstate) | 8 | Fixed | real | — | — | — |
| 2 | `LiftMcuNormalMsg` | [Bus: tSignalState](#bus-tsignalstate) | 8 | Fixed | real | — | — | — |
| 3 | `ThrustMcuFbkMsg` | [Bus: tSignalState](#bus-tsignalstate) | 3 | Fixed | real | — | — | — |
| 4 | `ThrustMcuNormalMsg` | [Bus: tSignalState](#bus-tsignalstate) | 3 | Fixed | real | — | — | — |
| 5 | `ServoFbkMsg` | [Bus: tSignalState](#bus-tsignalstate) | 8 | Fixed | real | — | — | — |

<a id="bus-bus_gcsdatastate"></a>

#### BUS_GcsDataState

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_GcsDataState)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `PilotCtrlMsg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |
| 2 | `FMCPMsg` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_mpiodatastate"></a>

#### BUS_MpioDataState

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_MpioDataState)`。直接字段：**1**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `IduDataMsg` | [Bus: tSignalState](#bus-tsignalstate) | 1 | Fixed | real | — | — | — |

<a id="bus-bus_pilotindatastate"></a>

#### BUS_PilotInDataState

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_PilotInDataState)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `VDT` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 6 | Fixed | real | — | — | — |
| 2 | `GPIO` | [Bus: tRedundantDataState](#bus-tredundantdatastate) | 6 | Fixed | real | — | — | — |

<a id="bus-bus_rms_public_msgs"></a>

#### BUS_RMS_Public_Msgs

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_RMS_Public_Msgs)`。直接字段：**5**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `chId` | uint8 | 1 | Fixed | real | — | — | — |
| 2 | `isActive` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `activeChId` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `voter` | [Bus: BUS_RMS_Voter_Data](#bus-bus_rms_voter_data) | 1 | Fixed | real | — | — | — |
| 5 | `isFccExist` | boolean | 3 | Fixed | real | — | — | — |

<a id="bus-bus_rms_voter_data"></a>

#### BUS_RMS_Voter_Data

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_RMS_Voter_Data)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[BUS_RMS_Public_Msgs](#bus-bus_rms_public_msgs).`voter` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `vtolState` | uint8 | 1 | Fixed | real | — | — | — |
| 2 | `ctrlMode` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `isInAir` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-bus_splog"></a>

#### BUS_SpLog

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_SpLog)`。直接字段：**4**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `fccStatus` | [Bus: tFccStatus](#bus-tfccstatus) | 3 | Fixed | real | — | — | — |
| 2 | `localStatus` | [Bus: tFccStatus](#bus-tfccstatus) | 1 | Fixed | real | — | — | — |
| 3 | `NavLog` | [Bus: tNavLog](#bus-tnavlog) | 1 | Fixed | real | — | — | — |
| 4 | `Ftadc` | [Bus: BUS_FtadcLog](#bus-bus_ftadclog) | 1 | Fixed | real | — | — | — |

<a id="bus-tfccstatus"></a>

#### tFccStatus

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tFccStatus)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[BUS_SpLog](#bus-bus_splog).`fccStatus` [3]；[BUS_SpLog](#bus-bus_splog).`localStatus` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `SysReady` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `LimitDispatchInfo` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-bus_utc"></a>

#### BUS_UTC

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BUS_UTC)`。直接字段：**8**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `dateValid` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `timeValid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `year` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `month` | uint8 | 1 | Fixed | real | — | — | — |
| 5 | `day` | uint8 | 1 | Fixed | real | — | — | — |
| 6 | `hour` | uint8 | 1 | Fixed | real | — | — | — |
| 7 | `minute` | uint8 | 1 | Fixed | real | — | — | — |
| 8 | `second` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-busout_ccdlmsg"></a>

#### BusOut_CcdlMsg

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BusOut_CcdlMsg)`。直接字段：**2**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `SysReady` | uint8 | 1 | Fixed | real | — | — | — |
| 2 | `LimitDispatch` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-busout_spdispmsg"></a>

#### BusOut_SpDispMsg

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=BusOut_SpDispMsg)`。直接字段：**19**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `PilotState` | uint8 | 1 | Fixed | real | — | — | — |
| 2 | `Captain_remote_Pos_vaild` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `Central_remote_Pos_valid` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `Captain_remote_Pitch_Pos` | single | 1 | Fixed | real | — | — | — |
| 5 | `Captain_remote_Roll_Pos` | single | 1 | Fixed | real | — | — | — |
| 6 | `Central_remote_Pitch_Pos` | single | 1 | Fixed | real | — | — | — |
| 7 | `Central_remote_Roll_Pos` | single | 1 | Fixed | real | — | — | — |
| 8 | `Captain_SSU_Pos_vaild` | uint8 | 1 | Fixed | real | — | — | — |
| 9 | `Central_SSU_Pos_valid` | uint8 | 1 | Fixed | real | — | — | — |
| 10 | `FO_SSU_Pos_valid` | uint8 | 1 | Fixed | real | — | — | — |
| 11 | `Captain_sidestick_Pitch_Pos` | single | 1 | Fixed | real | — | — | — |
| 12 | `Captain_sidestick_Roll_Pos` | single | 1 | Fixed | real | — | — | — |
| 13 | `Central_sidestick_Pitch_Pos` | single | 1 | Fixed | real | — | — | — |
| 14 | `Central_sidestick_Roll_Pos` | single | 1 | Fixed | real | — | — | — |
| 15 | `FO_sidestick_Pitch_Pos` | single | 1 | Fixed | real | — | — | — |
| 16 | `FO_sidestick_Roll_Pos` | single | 1 | Fixed | real | — | — | — |
| 17 | `systemReady` | uint8 | 1 | Fixed | real | — | — | — |
| 18 | `LimitDispatchInfo` | uint8 | 1 | Fixed | real | — | — | — |
| 19 | `HiVltgSW` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-bus_spcpt"></a>

#### Bus_SpCPT

来源：[SP_Parameters.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_Parameters.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=Bus_SpCPT)`。直接字段：**657**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `PilotInSelect` | int32 | 1 | Fixed | real | — | — | — |
| 2 | `AirDataSelect` | int32 | 1 | Fixed | real | — | — | — |
| 3 | `CASVoterType` | int32 | 1 | Fixed | real | — | — | — |
| 4 | `AOADataSelect` | int32 | 1 | Fixed | real | — | — | — |
| 5 | `AltRateCASSelect` | single | 1 | Fixed | real | — | — | — |
| 6 | `Mass_Default` | single | 1 | Fixed | real | — | — | — |
| 7 | `AC_Deafult` | single | 1 | Fixed | real | — | — | — |
| 8 | `Enable_RaCheck` | int32 | 1 | Fixed | real | — | — | — |
| 9 | `Enable_GpsCheck` | int32 | 1 | Fixed | real | — | — | — |
| 10 | `Enable_AdahruCheck` | int32 | 1 | Fixed | real | — | — | — |
| 11 | `Enable_AdcCheck` | int32 | 1 | Fixed | real | — | — | — |
| 12 | `Enable_AdcExpCheck` | int32 | 1 | Fixed | real | — | — | — |
| 13 | `AilLO_Gain` | single | 1 | Fixed | real | — | — | — |
| 14 | `AilLO_Bias` | single | 1 | Fixed | real | — | — | — |
| 15 | `AilLI_Gain` | single | 1 | Fixed | real | — | — | — |
| 16 | `AilLI_Bias` | single | 1 | Fixed | real | — | — | — |
| 17 | `AilRO_Gain` | single | 1 | Fixed | real | — | — | — |
| 18 | `AilRO_Bias` | single | 1 | Fixed | real | — | — | — |
| 19 | `AilRI_Gain` | single | 1 | Fixed | real | — | — | — |
| 20 | `AilRI_Bias` | single | 1 | Fixed | real | — | — | — |
| 21 | `EleL_Gain` | single | 1 | Fixed | real | — | — | — |
| 22 | `EleL_Bias` | single | 1 | Fixed | real | — | — | — |
| 23 | `EleR_Gain` | single | 1 | Fixed | real | — | — | — |
| 24 | `EleR_Bias` | single | 1 | Fixed | real | — | — | — |
| 25 | `RudL_Gain` | single | 1 | Fixed | real | — | — | — |
| 26 | `RudL_Bias` | single | 1 | Fixed | real | — | — | — |
| 27 | `RudR_Gain` | single | 1 | Fixed | real | — | — | — |
| 28 | `RudR_Bias` | single | 1 | Fixed | real | — | — | — |
| 29 | `Check_RmsSysReady` | uint32 | 1 | Fixed | real | — | — | — |
| 30 | `Check_FCC_Bit` | uint32 | 1 | Fixed | real | — | — | — |
| 31 | `check_LiftMCU_Bit` | uint32 | 1 | Fixed | real | — | — | — |
| 32 | `check_ThrustMCU_Bit` | uint32 | 1 | Fixed | real | — | — | — |
| 33 | `check_SCU_Bit` | uint32 | 1 | Fixed | real | — | — | — |
| 34 | `check_ADAHRS_Bit` | uint32 | 1 | Fixed | real | — | — | — |
| 35 | `check_RA_Bit` | uint32 | 1 | Fixed | real | — | — | — |
| 36 | `check_MPIO_Bit` | uint32 | 1 | Fixed | real | — | — | — |
| 37 | `check_GCS_Bit` | uint32 | 1 | Fixed | real | — | — | — |
| 38 | `check_FTADC_Bit` | uint32 | 1 | Fixed | real | — | — | — |
| 39 | `check_VDT_Bit` | uint32 | 1 | Fixed | real | — | — | — |
| 40 | `AdahruVoterType` | int32 | 1 | Fixed | real | — | — | — |
| 41 | `AdahruPosLabelGap` | int32 | 1 | Fixed | real | — | — | — |
| 42 | `RaVoterType` | int32 | 1 | Fixed | real | — | — | — |
| 43 | `GpsHeight_data0` | single | 1 | Fixed | real | — | — | — |
| 44 | `GpsHeight_increase` | int32 | 1 | Fixed | real | — | — | — |
| 45 | `GpsHeight_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 46 | `GpsHeight_resume` | int32 | 1 | Fixed | real | — | — | — |
| 47 | `GpsHeight_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 48 | `GpsHeight_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 49 | `GpsHeight_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 50 | `GpsHeight_max` | single | 1 | Fixed | real | — | — | — |
| 51 | `GpsHeight_min` | single | 1 | Fixed | real | — | — | — |
| 52 | `GpsHeight_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 53 | `GpsHeight_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 54 | `GpsHeight_maxerror` | single | 1 | Fixed | real | — | — | — |
| 55 | `GpsHeight_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 56 | `GpsHeight_banned` | int32 | 1 | Fixed | real | — | — | — |
| 57 | `GpsHeight_synced` | int32 | 1 | Fixed | real | — | — | — |
| 58 | `GpsHeight_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 59 | `GpsHeight_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 60 | `AOA_data0` | single | 1 | Fixed | real | — | — | — |
| 61 | `AOA_increase` | int32 | 1 | Fixed | real | — | — | — |
| 62 | `AOA_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 63 | `AOA_resume` | int32 | 1 | Fixed | real | — | — | — |
| 64 | `AOA_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 65 | `AOA_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 66 | `AOA_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 67 | `AOA_max` | single | 1 | Fixed | real | — | — | — |
| 68 | `AOA_min` | single | 1 | Fixed | real | — | — | — |
| 69 | `AOA_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 70 | `AOA_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 71 | `AOA_maxerror` | single | 1 | Fixed | real | — | — | — |
| 72 | `AOA_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 73 | `AOA_banned` | int32 | 1 | Fixed | real | — | — | — |
| 74 | `AOA_synced` | int32 | 1 | Fixed | real | — | — | — |
| 75 | `AOA_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 76 | `AOA_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 77 | `AOS_data0` | single | 1 | Fixed | real | — | — | — |
| 78 | `AOS_increase` | int32 | 1 | Fixed | real | — | — | — |
| 79 | `AOS_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 80 | `AOS_resume` | int32 | 1 | Fixed | real | — | — | — |
| 81 | `AOS_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 82 | `AOS_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 83 | `AOS_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 84 | `AOS_max` | single | 1 | Fixed | real | — | — | — |
| 85 | `AOS_min` | single | 1 | Fixed | real | — | — | — |
| 86 | `AOS_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 87 | `AOS_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 88 | `AOS_maxerror` | single | 1 | Fixed | real | — | — | — |
| 89 | `AOS_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 90 | `AOS_banned` | int32 | 1 | Fixed | real | — | — | — |
| 91 | `AOS_synced` | int32 | 1 | Fixed | real | — | — | — |
| 92 | `AOS_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 93 | `AOS_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 94 | `GroundSpeed_data0` | single | 1 | Fixed | real | — | — | — |
| 95 | `GroundSpeed_increase` | int32 | 1 | Fixed | real | — | — | — |
| 96 | `GroundSpeed_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 97 | `GroundSpeed_resume` | int32 | 1 | Fixed | real | — | — | — |
| 98 | `GroundSpeed_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 99 | `GroundSpeed_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 100 | `GroundSpeed_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 101 | `GroundSpeed_max` | single | 1 | Fixed | real | — | — | — |
| 102 | `GroundSpeed_min` | single | 1 | Fixed | real | — | — | — |
| 103 | `GroundSpeed_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 104 | `GroundSpeed_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 105 | `GroundSpeed_maxerror` | single | 1 | Fixed | real | — | — | — |
| 106 | `GroundSpeed_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 107 | `GroundSpeed_banned` | int32 | 1 | Fixed | real | — | — | — |
| 108 | `GroundSpeed_synced` | int32 | 1 | Fixed | real | — | — | — |
| 109 | `GroundSpeed_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 110 | `GroundSpeed_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 111 | `PressureAlt_data0` | single | 1 | Fixed | real | — | — | — |
| 112 | `PressureAlt_increase` | int32 | 1 | Fixed | real | — | — | — |
| 113 | `PressureAlt_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 114 | `PressureAlt_resume` | int32 | 1 | Fixed | real | — | — | — |
| 115 | `PressureAlt_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 116 | `PressureAlt_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 117 | `PressureAlt_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 118 | `PressureAlt_max` | single | 1 | Fixed | real | — | — | — |
| 119 | `PressureAlt_min` | single | 1 | Fixed | real | — | — | — |
| 120 | `PressureAlt_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 121 | `PressureAlt_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 122 | `PressureAlt_maxerror` | single | 1 | Fixed | real | — | — | — |
| 123 | `PressureAlt_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 124 | `PressureAlt_banned` | int32 | 1 | Fixed | real | — | — | — |
| 125 | `PressureAlt_synced` | int32 | 1 | Fixed | real | — | — | — |
| 126 | `PressureAlt_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 127 | `PressureAlt_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 128 | `BaroCorrectedAlt_data0` | single | 1 | Fixed | real | — | — | — |
| 129 | `BaroCorrectedAlt_increase` | int32 | 1 | Fixed | real | — | — | — |
| 130 | `BaroCorrectedAlt_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 131 | `BaroCorrectedAlt_resume` | int32 | 1 | Fixed | real | — | — | — |
| 132 | `BaroCorrectedAlt_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 133 | `BaroCorrectedAlt_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 134 | `BaroCorrectedAlt_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 135 | `BaroCorrectedAlt_max` | single | 1 | Fixed | real | — | — | — |
| 136 | `BaroCorrectedAlt_min` | single | 1 | Fixed | real | — | — | — |
| 137 | `BaroCorrectedAlt_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 138 | `BaroCorrectedAlt_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 139 | `BaroCorrectedAlt_maxerror` | single | 1 | Fixed | real | — | — | — |
| 140 | `BaroCorrectedAlt_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 141 | `BaroCorrectedAlt_banned` | int32 | 1 | Fixed | real | — | — | — |
| 142 | `BaroCorrectedAlt_synced` | int32 | 1 | Fixed | real | — | — | — |
| 143 | `BaroCorrectedAlt_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 144 | `BaroCorrectedAlt_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 145 | `Mach_data0` | single | 1 | Fixed | real | — | — | — |
| 146 | `Mach_increase` | int32 | 1 | Fixed | real | — | — | — |
| 147 | `Mach_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 148 | `Mach_resume` | int32 | 1 | Fixed | real | — | — | — |
| 149 | `Mach_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 150 | `Mach_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 151 | `Mach_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 152 | `Mach_max` | single | 1 | Fixed | real | — | — | — |
| 153 | `Mach_min` | single | 1 | Fixed | real | — | — | — |
| 154 | `Mach_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 155 | `Mach_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 156 | `Mach_maxerror` | single | 1 | Fixed | real | — | — | — |
| 157 | `Mach_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 158 | `Mach_banned` | int32 | 1 | Fixed | real | — | — | — |
| 159 | `Mach_synced` | int32 | 1 | Fixed | real | — | — | — |
| 160 | `Mach_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 161 | `Mach_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 162 | `CAS_data0` | single | 1 | Fixed | real | — | — | — |
| 163 | `CAS_increase` | int32 | 1 | Fixed | real | — | — | — |
| 164 | `CAS_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 165 | `CAS_resume` | int32 | 1 | Fixed | real | — | — | — |
| 166 | `CAS_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 167 | `CAS_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 168 | `CAS_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 169 | `CAS_max` | single | 1 | Fixed | real | — | — | — |
| 170 | `CAS_min` | single | 1 | Fixed | real | — | — | — |
| 171 | `CAS_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 172 | `CAS_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 173 | `CAS_maxerror` | single | 1 | Fixed | real | — | — | — |
| 174 | `CAS_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 175 | `CAS_banned` | int32 | 1 | Fixed | real | — | — | — |
| 176 | `CAS_synced` | int32 | 1 | Fixed | real | — | — | — |
| 177 | `CAS_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 178 | `CAS_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 179 | `TAS_data0` | single | 1 | Fixed | real | — | — | — |
| 180 | `TAS_increase` | int32 | 1 | Fixed | real | — | — | — |
| 181 | `TAS_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 182 | `TAS_resume` | int32 | 1 | Fixed | real | — | — | — |
| 183 | `TAS_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 184 | `TAS_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 185 | `TAS_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 186 | `TAS_max` | single | 1 | Fixed | real | — | — | — |
| 187 | `TAS_min` | single | 1 | Fixed | real | — | — | — |
| 188 | `TAS_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 189 | `TAS_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 190 | `TAS_maxerror` | single | 1 | Fixed | real | — | — | — |
| 191 | `TAS_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 192 | `TAS_banned` | int32 | 1 | Fixed | real | — | — | — |
| 193 | `TAS_synced` | int32 | 1 | Fixed | real | — | — | — |
| 194 | `TAS_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 195 | `TAS_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 196 | `CombAltRate_data0` | single | 1 | Fixed | real | — | — | — |
| 197 | `CombAltRate_increase` | int32 | 1 | Fixed | real | — | — | — |
| 198 | `CombAltRate_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 199 | `CombAltRate_resume` | int32 | 1 | Fixed | real | — | — | — |
| 200 | `CombAltRate_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 201 | `CombAltRate_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 202 | `CombAltRate_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 203 | `CombAltRate_max` | single | 1 | Fixed | real | — | — | — |
| 204 | `CombAltRate_min` | single | 1 | Fixed | real | — | — | — |
| 205 | `CombAltRate_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 206 | `CombAltRate_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 207 | `CombAltRate_maxerror` | single | 1 | Fixed | real | — | — | — |
| 208 | `CombAltRate_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 209 | `CombAltRate_banned` | int32 | 1 | Fixed | real | — | — | — |
| 210 | `CombAltRate_synced` | int32 | 1 | Fixed | real | — | — | — |
| 211 | `CombAltRate_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 212 | `CombAltRate_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 213 | `AltRate_data0` | single | 1 | Fixed | real | — | — | — |
| 214 | `AltRate_increase` | int32 | 1 | Fixed | real | — | — | — |
| 215 | `AltRate_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 216 | `AltRate_resume` | int32 | 1 | Fixed | real | — | — | — |
| 217 | `AltRate_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 218 | `AltRate_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 219 | `AltRate_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 220 | `AltRate_max` | single | 1 | Fixed | real | — | — | — |
| 221 | `AltRate_min` | single | 1 | Fixed | real | — | — | — |
| 222 | `AltRate_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 223 | `AltRate_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 224 | `AltRate_maxerror` | single | 1 | Fixed | real | — | — | — |
| 225 | `AltRate_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 226 | `AltRate_banned` | int32 | 1 | Fixed | real | — | — | — |
| 227 | `AltRate_synced` | int32 | 1 | Fixed | real | — | — | — |
| 228 | `AltRate_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 229 | `AltRate_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 230 | `StaticAirTemp_data0` | single | 1 | Fixed | real | — | — | — |
| 231 | `StaticAirTemp_increase` | int32 | 1 | Fixed | real | — | — | — |
| 232 | `StaticAirTemp_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 233 | `StaticAirTemp_resume` | int32 | 1 | Fixed | real | — | — | — |
| 234 | `StaticAirTemp_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 235 | `StaticAirTemp_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 236 | `StaticAirTemp_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 237 | `StaticAirTemp_max` | single | 1 | Fixed | real | — | — | — |
| 238 | `StaticAirTemp_min` | single | 1 | Fixed | real | — | — | — |
| 239 | `StaticAirTemp_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 240 | `StaticAirTemp_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 241 | `StaticAirTemp_maxerror` | single | 1 | Fixed | real | — | — | — |
| 242 | `StaticAirTemp_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 243 | `StaticAirTemp_banned` | int32 | 1 | Fixed | real | — | — | — |
| 244 | `StaticAirTemp_synced` | int32 | 1 | Fixed | real | — | — | — |
| 245 | `StaticAirTemp_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 246 | `StaticAirTemp_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 247 | `TotalPressure_data0` | single | 1 | Fixed | real | — | — | — |
| 248 | `TotalPressure_increase` | int32 | 1 | Fixed | real | — | — | — |
| 249 | `TotalPressure_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 250 | `TotalPressure_resume` | int32 | 1 | Fixed | real | — | — | — |
| 251 | `TotalPressure_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 252 | `TotalPressure_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 253 | `TotalPressure_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 254 | `TotalPressure_max` | single | 1 | Fixed | real | — | — | — |
| 255 | `TotalPressure_min` | single | 1 | Fixed | real | — | — | — |
| 256 | `TotalPressure_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 257 | `TotalPressure_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 258 | `TotalPressure_maxerror` | single | 1 | Fixed | real | — | — | — |
| 259 | `TotalPressure_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 260 | `TotalPressure_banned` | int32 | 1 | Fixed | real | — | — | — |
| 261 | `TotalPressure_synced` | int32 | 1 | Fixed | real | — | — | — |
| 262 | `TotalPressure_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 263 | `TotalPressure_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 264 | `StaticPressure_data0` | single | 1 | Fixed | real | — | — | — |
| 265 | `StaticPressure_increase` | int32 | 1 | Fixed | real | — | — | — |
| 266 | `StaticPressure_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 267 | `StaticPressure_resume` | int32 | 1 | Fixed | real | — | — | — |
| 268 | `StaticPressure_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 269 | `StaticPressure_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 270 | `StaticPressure_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 271 | `StaticPressure_max` | single | 1 | Fixed | real | — | — | — |
| 272 | `StaticPressure_min` | single | 1 | Fixed | real | — | — | — |
| 273 | `StaticPressure_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 274 | `StaticPressure_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 275 | `StaticPressure_maxerror` | single | 1 | Fixed | real | — | — | — |
| 276 | `StaticPressure_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 277 | `StaticPressure_banned` | int32 | 1 | Fixed | real | — | — | — |
| 278 | `StaticPressure_synced` | int32 | 1 | Fixed | real | — | — | — |
| 279 | `StaticPressure_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 280 | `StaticPressure_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 281 | `CombLate_data0` | double | 1 | Fixed | real | — | — | — |
| 282 | `CombLate_increase` | int32 | 1 | Fixed | real | — | — | — |
| 283 | `CombLate_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 284 | `CombLate_resume` | int32 | 1 | Fixed | real | — | — | — |
| 285 | `CombLate_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 286 | `CombLate_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 287 | `CombLate_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 288 | `CombLate_max` | double | 1 | Fixed | real | — | — | — |
| 289 | `CombLate_min` | double | 1 | Fixed | real | — | — | — |
| 290 | `CombLate_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 291 | `CombLate_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 292 | `CombLate_maxerror` | double | 1 | Fixed | real | — | — | — |
| 293 | `CombLate_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 294 | `CombLate_banned` | int32 | 1 | Fixed | real | — | — | — |
| 295 | `CombLate_synced` | int32 | 1 | Fixed | real | — | — | — |
| 296 | `CombLate_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 297 | `CombLate_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 298 | `CombLong_data0` | double | 1 | Fixed | real | — | — | — |
| 299 | `CombLong_increase` | int32 | 1 | Fixed | real | — | — | — |
| 300 | `CombLong_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 301 | `CombLong_resume` | int32 | 1 | Fixed | real | — | — | — |
| 302 | `CombLong_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 303 | `CombLong_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 304 | `CombLong_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 305 | `CombLong_max` | double | 1 | Fixed | real | — | — | — |
| 306 | `CombLong_min` | double | 1 | Fixed | real | — | — | — |
| 307 | `CombLong_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 308 | `CombLong_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 309 | `CombLong_maxerror` | double | 1 | Fixed | real | — | — | — |
| 310 | `CombLong_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 311 | `CombLong_banned` | int32 | 1 | Fixed | real | — | — | — |
| 312 | `CombLong_synced` | int32 | 1 | Fixed | real | — | — | — |
| 313 | `CombLong_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 314 | `CombLong_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 315 | `MagnHeading_data0` | single | 1 | Fixed | real | — | — | — |
| 316 | `MagnHeading_increase` | int32 | 1 | Fixed | real | — | — | — |
| 317 | `MagnHeading_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 318 | `MagnHeading_resume` | int32 | 1 | Fixed | real | — | — | — |
| 319 | `MagnHeading_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 320 | `MagnHeading_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 321 | `MagnHeading_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 322 | `MagnHeading_max` | single | 1 | Fixed | real | — | — | — |
| 323 | `MagnHeading_min` | single | 1 | Fixed | real | — | — | — |
| 324 | `MagnHeading_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 325 | `MagnHeading_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 326 | `MagnHeading_maxerror` | single | 1 | Fixed | real | — | — | — |
| 327 | `MagnHeading_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 328 | `MagnHeading_banned` | int32 | 1 | Fixed | real | — | — | — |
| 329 | `MagnHeading_synced` | int32 | 1 | Fixed | real | — | — | — |
| 330 | `MagnHeading_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 331 | `MagnHeading_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 332 | `TrueHeading_data0` | single | 1 | Fixed | real | — | — | — |
| 333 | `TrueHeading_increase` | int32 | 1 | Fixed | real | — | — | — |
| 334 | `TrueHeading_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 335 | `TrueHeading_resume` | int32 | 1 | Fixed | real | — | — | — |
| 336 | `TrueHeading_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 337 | `TrueHeading_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 338 | `TrueHeading_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 339 | `TrueHeading_max` | single | 1 | Fixed | real | — | — | — |
| 340 | `TrueHeading_min` | single | 1 | Fixed | real | — | — | — |
| 341 | `TrueHeading_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 342 | `TrueHeading_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 343 | `TrueHeading_maxerror` | single | 1 | Fixed | real | — | — | — |
| 344 | `TrueHeading_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 345 | `TrueHeading_banned` | int32 | 1 | Fixed | real | — | — | — |
| 346 | `TrueHeading_synced` | int32 | 1 | Fixed | real | — | — | — |
| 347 | `TrueHeading_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 348 | `TrueHeading_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 349 | `MagDeclin_data0` | single | 1 | Fixed | real | — | — | — |
| 350 | `MagDeclin_increase` | int32 | 1 | Fixed | real | — | — | — |
| 351 | `MagDeclin_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 352 | `MagDeclin_resume` | int32 | 1 | Fixed | real | — | — | — |
| 353 | `MagDeclin_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 354 | `MagDeclin_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 355 | `MagDeclin_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 356 | `MagDeclin_max` | single | 1 | Fixed | real | — | — | — |
| 357 | `MagDeclin_min` | single | 1 | Fixed | real | — | — | — |
| 358 | `MagDeclin_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 359 | `MagDeclin_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 360 | `MagDeclin_maxerror` | single | 1 | Fixed | real | — | — | — |
| 361 | `MagDeclin_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 362 | `MagDeclin_banned` | int32 | 1 | Fixed | real | — | — | — |
| 363 | `MagDeclin_synced` | int32 | 1 | Fixed | real | — | — | — |
| 364 | `MagDeclin_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 365 | `MagDeclin_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 366 | `VertVelocity_data0` | single | 1 | Fixed | real | — | — | — |
| 367 | `VertVelocity_increase` | int32 | 1 | Fixed | real | — | — | — |
| 368 | `VertVelocity_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 369 | `VertVelocity_resume` | int32 | 1 | Fixed | real | — | — | — |
| 370 | `VertVelocity_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 371 | `VertVelocity_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 372 | `VertVelocity_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 373 | `VertVelocity_max` | single | 1 | Fixed | real | — | — | — |
| 374 | `VertVelocity_min` | single | 1 | Fixed | real | — | — | — |
| 375 | `VertVelocity_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 376 | `VertVelocity_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 377 | `VertVelocity_maxerror` | single | 1 | Fixed | real | — | — | — |
| 378 | `VertVelocity_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 379 | `VertVelocity_banned` | int32 | 1 | Fixed | real | — | — | — |
| 380 | `VertVelocity_synced` | int32 | 1 | Fixed | real | — | — | — |
| 381 | `VertVelocity_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 382 | `VertVelocity_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 383 | `NorthVelocity_data0` | single | 1 | Fixed | real | — | — | — |
| 384 | `NorthVelocity_increase` | int32 | 1 | Fixed | real | — | — | — |
| 385 | `NorthVelocity_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 386 | `NorthVelocity_resume` | int32 | 1 | Fixed | real | — | — | — |
| 387 | `NorthVelocity_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 388 | `NorthVelocity_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 389 | `NorthVelocity_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 390 | `NorthVelocity_max` | single | 1 | Fixed | real | — | — | — |
| 391 | `NorthVelocity_min` | single | 1 | Fixed | real | — | — | — |
| 392 | `NorthVelocity_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 393 | `NorthVelocity_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 394 | `NorthVelocity_maxerror` | single | 1 | Fixed | real | — | — | — |
| 395 | `NorthVelocity_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 396 | `NorthVelocity_banned` | int32 | 1 | Fixed | real | — | — | — |
| 397 | `NorthVelocity_synced` | int32 | 1 | Fixed | real | — | — | — |
| 398 | `NorthVelocity_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 399 | `NorthVelocity_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 400 | `EastVelocity_data0` | single | 1 | Fixed | real | — | — | — |
| 401 | `EastVelocity_increase` | int32 | 1 | Fixed | real | — | — | — |
| 402 | `EastVelocity_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 403 | `EastVelocity_resume` | int32 | 1 | Fixed | real | — | — | — |
| 404 | `EastVelocity_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 405 | `EastVelocity_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 406 | `EastVelocity_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 407 | `EastVelocity_max` | single | 1 | Fixed | real | — | — | — |
| 408 | `EastVelocity_min` | single | 1 | Fixed | real | — | — | — |
| 409 | `EastVelocity_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 410 | `EastVelocity_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 411 | `EastVelocity_maxerror` | single | 1 | Fixed | real | — | — | — |
| 412 | `EastVelocity_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 413 | `EastVelocity_banned` | int32 | 1 | Fixed | real | — | — | — |
| 414 | `EastVelocity_synced` | int32 | 1 | Fixed | real | — | — | — |
| 415 | `EastVelocity_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 416 | `EastVelocity_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 417 | `Pitch_data0` | single | 1 | Fixed | real | — | — | — |
| 418 | `Pitch_increase` | int32 | 1 | Fixed | real | — | — | — |
| 419 | `Pitch_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 420 | `Pitch_resume` | int32 | 1 | Fixed | real | — | — | — |
| 421 | `Pitch_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 422 | `Pitch_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 423 | `Pitch_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 424 | `Pitch_max` | single | 1 | Fixed | real | — | — | — |
| 425 | `Pitch_min` | single | 1 | Fixed | real | — | — | — |
| 426 | `Pitch_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 427 | `Pitch_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 428 | `Pitch_maxerror` | single | 1 | Fixed | real | — | — | — |
| 429 | `Pitch_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 430 | `Pitch_banned` | int32 | 1 | Fixed | real | — | — | — |
| 431 | `Pitch_synced` | int32 | 1 | Fixed | real | — | — | — |
| 432 | `Pitch_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 433 | `Pitch_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 434 | `Roll_data0` | single | 1 | Fixed | real | — | — | — |
| 435 | `Roll_increase` | int32 | 1 | Fixed | real | — | — | — |
| 436 | `Roll_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 437 | `Roll_resume` | int32 | 1 | Fixed | real | — | — | — |
| 438 | `Roll_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 439 | `Roll_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 440 | `Roll_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 441 | `Roll_max` | single | 1 | Fixed | real | — | — | — |
| 442 | `Roll_min` | single | 1 | Fixed | real | — | — | — |
| 443 | `Roll_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 444 | `Roll_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 445 | `Roll_maxerror` | single | 1 | Fixed | real | — | — | — |
| 446 | `Roll_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 447 | `Roll_banned` | int32 | 1 | Fixed | real | — | — | — |
| 448 | `Roll_synced` | int32 | 1 | Fixed | real | — | — | — |
| 449 | `Roll_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 450 | `Roll_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 451 | `SforX_data0` | single | 1 | Fixed | real | — | — | — |
| 452 | `SforX_increase` | int32 | 1 | Fixed | real | — | — | — |
| 453 | `SforX_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 454 | `SforX_resume` | int32 | 1 | Fixed | real | — | — | — |
| 455 | `SforX_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 456 | `SforX_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 457 | `SforX_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 458 | `SforX_max` | single | 1 | Fixed | real | — | — | — |
| 459 | `SforX_min` | single | 1 | Fixed | real | — | — | — |
| 460 | `SforX_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 461 | `SforX_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 462 | `SforX_maxerror` | single | 1 | Fixed | real | — | — | — |
| 463 | `SforX_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 464 | `SforX_banned` | int32 | 1 | Fixed | real | — | — | — |
| 465 | `SforX_synced` | int32 | 1 | Fixed | real | — | — | — |
| 466 | `SforX_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 467 | `SforX_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 468 | `SforY_data0` | single | 1 | Fixed | real | — | — | — |
| 469 | `SforY_increase` | int32 | 1 | Fixed | real | — | — | — |
| 470 | `SforY_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 471 | `SforY_resume` | int32 | 1 | Fixed | real | — | — | — |
| 472 | `SforY_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 473 | `SforY_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 474 | `SforY_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 475 | `SforY_max` | single | 1 | Fixed | real | — | — | — |
| 476 | `SforY_min` | single | 1 | Fixed | real | — | — | — |
| 477 | `SforY_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 478 | `SforY_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 479 | `SforY_maxerror` | single | 1 | Fixed | real | — | — | — |
| 480 | `SforY_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 481 | `SforY_banned` | int32 | 1 | Fixed | real | — | — | — |
| 482 | `SforY_synced` | int32 | 1 | Fixed | real | — | — | — |
| 483 | `SforY_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 484 | `SforY_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 485 | `SforZ_data0` | single | 1 | Fixed | real | — | — | — |
| 486 | `SforZ_increase` | int32 | 1 | Fixed | real | — | — | — |
| 487 | `SforZ_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 488 | `SforZ_resume` | int32 | 1 | Fixed | real | — | — | — |
| 489 | `SforZ_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 490 | `SforZ_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 491 | `SforZ_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 492 | `SforZ_max` | single | 1 | Fixed | real | — | — | — |
| 493 | `SforZ_min` | single | 1 | Fixed | real | — | — | — |
| 494 | `SforZ_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 495 | `SforZ_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 496 | `SforZ_maxerror` | single | 1 | Fixed | real | — | — | — |
| 497 | `SforZ_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 498 | `SforZ_banned` | int32 | 1 | Fixed | real | — | — | — |
| 499 | `SforZ_synced` | int32 | 1 | Fixed | real | — | — | — |
| 500 | `SforZ_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 501 | `SforZ_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 502 | `Q_data0` | single | 1 | Fixed | real | — | — | — |
| 503 | `Q_increase` | int32 | 1 | Fixed | real | — | — | — |
| 504 | `Q_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 505 | `Q_resume` | int32 | 1 | Fixed | real | — | — | — |
| 506 | `Q_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 507 | `Q_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 508 | `Q_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 509 | `Q_max` | single | 1 | Fixed | real | — | — | — |
| 510 | `Q_min` | single | 1 | Fixed | real | — | — | — |
| 511 | `Q_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 512 | `Q_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 513 | `Q_maxerror` | single | 1 | Fixed | real | — | — | — |
| 514 | `Q_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 515 | `Q_banned` | int32 | 1 | Fixed | real | — | — | — |
| 516 | `Q_synced` | int32 | 1 | Fixed | real | — | — | — |
| 517 | `Q_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 518 | `Q_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 519 | `P_data0` | single | 1 | Fixed | real | — | — | — |
| 520 | `P_increase` | int32 | 1 | Fixed | real | — | — | — |
| 521 | `P_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 522 | `P_resume` | int32 | 1 | Fixed | real | — | — | — |
| 523 | `P_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 524 | `P_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 525 | `P_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 526 | `P_max` | single | 1 | Fixed | real | — | — | — |
| 527 | `P_min` | single | 1 | Fixed | real | — | — | — |
| 528 | `P_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 529 | `P_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 530 | `P_maxerror` | single | 1 | Fixed | real | — | — | — |
| 531 | `P_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 532 | `P_banned` | int32 | 1 | Fixed | real | — | — | — |
| 533 | `P_synced` | int32 | 1 | Fixed | real | — | — | — |
| 534 | `P_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 535 | `P_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 536 | `R_data0` | single | 1 | Fixed | real | — | — | — |
| 537 | `R_increase` | int32 | 1 | Fixed | real | — | — | — |
| 538 | `R_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 539 | `R_resume` | int32 | 1 | Fixed | real | — | — | — |
| 540 | `R_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 541 | `R_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 542 | `R_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 543 | `R_max` | single | 1 | Fixed | real | — | — | — |
| 544 | `R_min` | single | 1 | Fixed | real | — | — | — |
| 545 | `R_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 546 | `R_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 547 | `R_maxerror` | single | 1 | Fixed | real | — | — | — |
| 548 | `R_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 549 | `R_banned` | int32 | 1 | Fixed | real | — | — | — |
| 550 | `R_synced` | int32 | 1 | Fixed | real | — | — | — |
| 551 | `R_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 552 | `R_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 553 | `RadioAlt_data0` | single | 1 | Fixed | real | — | — | — |
| 554 | `RadioAlt_increase` | int32 | 1 | Fixed | real | — | — | — |
| 555 | `RadioAlt_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 556 | `RadioAlt_resume` | int32 | 1 | Fixed | real | — | — | — |
| 557 | `RadioAlt_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 558 | `RadioAlt_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 559 | `RadioAlt_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 560 | `RadioAlt_max` | single | 1 | Fixed | real | — | — | — |
| 561 | `RadioAlt_min` | single | 1 | Fixed | real | — | — | — |
| 562 | `RadioAlt_valerror` | int32 | 1 | Fixed | real | — | — | — |
| 563 | `RadioAlt_valresume` | int32 | 1 | Fixed | real | — | — | — |
| 564 | `RadioAlt_maxerror` | single | 1 | Fixed | real | — | — | — |
| 565 | `RadioAlt_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 566 | `RadioAlt_banned` | int32 | 1 | Fixed | real | — | — | — |
| 567 | `RadioAlt_synced` | int32 | 1 | Fixed | real | — | — | — |
| 568 | `RadioAlt_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 569 | `RadioAlt_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 570 | `Ftadc_increase` | int32 | 1 | Fixed | real | — | — | — |
| 571 | `Ftadc_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 572 | `Ftadc_resume` | int32 | 1 | Fixed | real | — | — | — |
| 573 | `Ftadc_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 574 | `FtadcPs_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 575 | `FtadcPs_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 576 | `FtadcPt_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 577 | `FtadcPt_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 578 | `FtadcAoa_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 579 | `FtadcAoa_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 580 | `FtadcAos_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 581 | `FtadcAos_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 582 | `FtadcCas_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 583 | `FtadcCas_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 584 | `McuFbkMsg_increase` | int32 | 1 | Fixed | real | — | — | — |
| 585 | `McuFbkMsg_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 586 | `McuFbkMsg_resume` | int32 | 1 | Fixed | real | — | — | — |
| 587 | `McuFbkMsg_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 588 | `McuFbkMsg_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 589 | `McuFbkMsg_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 590 | `McuNormalMsg_increase` | int32 | 1 | Fixed | real | — | — | — |
| 591 | `McuNormalMsg_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 592 | `McuNormalMsg_resume` | int32 | 1 | Fixed | real | — | — | — |
| 593 | `McuNormalMsg_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 594 | `McuNormalMsg_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 595 | `McuNormalMsg_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 596 | `ScuFbkMsg_increase` | int32 | 1 | Fixed | real | — | — | — |
| 597 | `ScuFbkMsg_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 598 | `ScuFbkMsg_resume` | int32 | 1 | Fixed | real | — | — | — |
| 599 | `ScuFbkMsg_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 600 | `PilotCtrlMsg_increase` | int32 | 1 | Fixed | real | — | — | — |
| 601 | `PilotCtrlMsg_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 602 | `PilotCtrlMsg_resume` | int32 | 1 | Fixed | real | — | — | — |
| 603 | `PilotCtrlMsg_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 604 | `FMCPMsg_increase` | int32 | 1 | Fixed | real | — | — | — |
| 605 | `FMCPMsg_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 606 | `FMCPMsg_resume` | int32 | 1 | Fixed | real | — | — | — |
| 607 | `FMCPMsg_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 608 | `IduMsg_increase` | int32 | 1 | Fixed | real | — | — | — |
| 609 | `IduMsg_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 610 | `IduMsg_resume` | int32 | 1 | Fixed | real | — | — | — |
| 611 | `IduMsg_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 612 | `IduMsg_abnormal` | int32 | 1 | Fixed | real | — | — | — |
| 613 | `IduMsg_abresume` | int32 | 1 | Fixed | real | — | — | — |
| 614 | `VDTCh1_data0` | single | 1 | Fixed | real | — | — | — |
| 615 | `VDTCh2_data0` | single | 1 | Fixed | real | — | — | — |
| 616 | `VDTCh3_data0` | single | 1 | Fixed | real | — | — | — |
| 617 | `VDTCh4_data0` | single | 1 | Fixed | real | — | — | — |
| 618 | `VDTCh5_data0` | single | 1 | Fixed | real | — | — | — |
| 619 | `VDTCh6_data0` | single | 1 | Fixed | real | — | — | — |
| 620 | `VDT_increase` | int32 | 1 | Fixed | real | — | — | — |
| 621 | `VDT_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 622 | `VDT_resume` | int32 | 1 | Fixed | real | — | — | — |
| 623 | `VDT_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 624 | `VDTCCDL_increase` | int32 | 1 | Fixed | real | — | — | — |
| 625 | `VDTCCDL_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 626 | `VDTCCDL_resume` | int32 | 1 | Fixed | real | — | — | — |
| 627 | `VDTCCDL_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 628 | `VDT_maxerror` | single | 1 | Fixed | real | — | — | — |
| 629 | `VDT_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 630 | `VDT_banned` | int32 | 1 | Fixed | real | — | — | — |
| 631 | `VDT_synced` | int32 | 1 | Fixed | real | — | — | — |
| 632 | `VDT_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 633 | `VDT_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 634 | `GPIOCh1_data0` | int32 | 1 | Fixed | real | — | — | — |
| 635 | `GPIOCh2_data0` | int32 | 1 | Fixed | real | — | — | — |
| 636 | `GPIOCh3_data0` | int32 | 1 | Fixed | real | — | — | — |
| 637 | `GPIOCh4_data0` | int32 | 1 | Fixed | real | — | — | — |
| 638 | `GPIOCh5_data0` | int32 | 1 | Fixed | real | — | — | — |
| 639 | `GPIOCh6_data0` | int32 | 1 | Fixed | real | — | — | — |
| 640 | `GPIO_increase` | int32 | 1 | Fixed | real | — | — | — |
| 641 | `GPIO_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 642 | `GPIO_resume` | int32 | 1 | Fixed | real | — | — | — |
| 643 | `GPIO_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 644 | `GPIOCCDL_increase` | int32 | 1 | Fixed | real | — | — | — |
| 645 | `GPIOCCDL_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 646 | `GPIOCCDL_resume` | int32 | 1 | Fixed | real | — | — | — |
| 647 | `GPIOCCDL_timeout` | int32 | 1 | Fixed | real | — | — | — |
| 648 | `GPIO_maxerror` | single | 1 | Fixed | real | — | — | — |
| 649 | `GPIO_misscmp` | int32 | 1 | Fixed | real | — | — | — |
| 650 | `GPIO_banned` | int32 | 1 | Fixed | real | — | — | — |
| 651 | `GPIO_synced` | int32 | 1 | Fixed | real | — | — | — |
| 652 | `GPIO_banenable` | int32 | 1 | Fixed | real | — | — | — |
| 653 | `GPIO_calctype` | int32 | 1 | Fixed | real | — | — | — |
| 654 | `CCDL_increase` | int32 | 1 | Fixed | real | — | — | — |
| 655 | `CCDL_decrease` | int32 | 1 | Fixed | real | — | — | — |
| 656 | `CCDL_resume` | int32 | 1 | Fixed | real | — | — | — |
| 657 | `CCDL_timeout` | int32 | 1 | Fixed | real | — | — | — |

<a id="bus-claw_nav_msg"></a>

#### Claw_Nav_Msg

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=Claw_Nav_Msg)`。直接字段：**42**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `Valid_Status` | [Bus: BUS_FCS_NAV_Valid_Status](#bus-bus_fcs_nav_valid_status) | 1 | Fixed | real | — | — | — |
| 2 | `Reset_Status` | [Bus: BUS_FCS_NAV_Reset_Status](#bus-bus_fcs_nav_reset_status) | 1 | Fixed | real | — | — | — |
| 3 | `Lat_deg` | double | 1 | Fixed | real | — | — | — |
| 4 | `Lon_deg` | double | 1 | Fixed | real | — | — | — |
| 5 | `h_AMSL_m` | single | 1 | Fixed | real | — | — | — |
| 6 | `h_BARO_m` | single | 1 | Fixed | real | — | — | — |
| 7 | `h_AGL_m` | single | 1 | Fixed | real | — | — | — |
| 8 | `Vel_x_REO_mps` | single | 1 | Fixed | real | — | — | — |
| 9 | `Vel_y_REO_mps` | single | 1 | Fixed | real | — | — | — |
| 10 | `Vel_z_REO_mps` | single | 1 | Fixed | real | — | — | — |
| 11 | `Vel_CAS_mps` | single | 1 | Fixed | real | — | — | — |
| 12 | `Vel_TAS_mps` | single | 1 | Fixed | real | — | — | — |
| 13 | `Att_phi_rad` | single | 1 | Fixed | real | — | — | — |
| 14 | `Att_theta_rad` | single | 1 | Fixed | real | — | — | — |
| 15 | `Att_psi_rad` | single | 1 | Fixed | real | — | — | — |
| 16 | `Sfor_x_RB_mps2` | single | 1 | Fixed | real | — | — | — |
| 17 | `Sfor_y_RB_mps2` | single | 1 | Fixed | real | — | — | — |
| 18 | `Sfor_z_RB_mps2` | single | 1 | Fixed | real | — | — | — |
| 19 | `Nx_IRU_g` | single | 1 | Fixed | real | — | — | — |
| 20 | `Ny_IRU_g` | single | 1 | Fixed | real | — | — | — |
| 21 | `Nz_IRU_g` | single | 1 | Fixed | real | — | — | — |
| 22 | `Rot_p_rps` | single | 1 | Fixed | real | — | — | — |
| 23 | `Rot_q_rps` | single | 1 | Fixed | real | — | — | — |
| 24 | `Rot_r_rps` | single | 1 | Fixed | real | — | — | — |
| 25 | `Alpha_ADS_deg` | single | 1 | Fixed | real | — | — | — |
| 26 | `Beta_ADS_deg` | single | 1 | Fixed | real | — | — | — |
| 27 | `WOW_LG1_b` | boolean | 1 | Fixed | real | — | — | — |
| 28 | `WOWVld_LG1_b` | boolean | 1 | Fixed | real | — | — | — |
| 29 | `WOW_LG2_b` | boolean | 1 | Fixed | real | — | — | — |
| 30 | `WOWVld_LG2_b` | boolean | 1 | Fixed | real | — | — | — |
| 31 | `WOW_LG3_b` | boolean | 1 | Fixed | real | — | — | — |
| 32 | `WOWVld_LG3_b` | boolean | 1 | Fixed | real | — | — | — |
| 33 | `WOW_LG4_b` | boolean | 1 | Fixed | real | — | — | — |
| 34 | `WOWVld_LG4_b` | boolean | 1 | Fixed | real | — | — | — |
| 35 | `AirGroundSwitchVld_ALL_b` | boolean | 1 | Fixed | real | — | — | — |
| 36 | `AirGroundSwitch_OnGnd_b` | boolean | 1 | Fixed | real | — | — | — |
| 37 | `Radio_Altitude1` | single | 1 | Fixed | real | — | — | — |
| 38 | `Radio_Altitude2` | single | 1 | Fixed | real | — | — | — |
| 39 | `Radio_Altitude1_Valid_b` | boolean | 1 | Fixed | real | — | — | — |
| 40 | `Radio_Altitude2_Valid_b` | boolean | 1 | Fixed | real | — | — | — |
| 41 | `Frt_psi_Pa` | single | 1 | Fixed | real | — | — | — |
| 42 | `Frt_psi_Valid_b` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-claw_sys_msg"></a>

#### Claw_Sys_Msg

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=Claw_Sys_Msg)`。直接字段：**5**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `SysReady_SYSINFO_b` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `Reset_FCM_b` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `VCU_Status` | [Bus: BUS_SYSINFO_VCU](#bus-bus_sysinfo_vcu) | 1 | Fixed | real | — | — | — |
| 4 | `Motor_Status` | [Bus: BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr) | 1 | Fixed | real | — | — | — |
| 5 | `Actuator_Status` | [Bus: BUS_SYSINFO_ACT](#bus-bus_sysinfo_act) | 1 | Fixed | real | — | — | — |

<a id="bus-spin_ipcbit"></a>

#### SpIn_IpCBIT

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpIn_IpCBIT)`。直接字段：**7**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `MCU_HealthBit` | uint16 | 1 | Fixed | real | — | — | — |
| 2 | `SCU_HealthBit` | uint8 | 1 | Fixed | real | — | — | — |
| 3 | `ADAHRS_HealthBit` | uint8 | 1 | Fixed | real | — | — | — |
| 4 | `RA_HealthBit` | uint8 | 1 | Fixed | real | — | — | — |
| 5 | `MPIO_HealthBit` | uint8 | 1 | Fixed | real | — | — | — |
| 6 | `GCS_HealtBit` | uint8 | 1 | Fixed | real | — | — | — |
| 7 | `FTADC_HealtBit` | uint8 | 1 | Fixed | real | — | — | — |

<a id="bus-spnavdata_double"></a>

#### SpNavData_Double

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpNavData_Double)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[SpOut_Nav](#bus-spout_nav).`L310_Combined_Latitude` [1]；[SpOut_Nav](#bus-spout_nav).`L311_Combined_Longitude` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | double | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-spnavdata_single"></a>

#### SpNavData_Single

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpNavData_Single)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

作为成员出现于：[SpOut_Nav](#bus-spout_nav).`L076_GPS_Height` [1]；[SpOut_Nav](#bus-spout_nav).`L107_Angle_Of_Attack` [1]；[SpOut_Nav](#bus-spout_nav).`L110_Sideslip_Angle` [1]；[SpOut_Nav](#bus-spout_nav).`L112_Ground_Speed` [1]；[SpOut_Nav](#bus-spout_nav).`L203_Pressure_Altitude` [1]；[SpOut_Nav](#bus-spout_nav).`L204_Baro_Corrected_Altitude` [1]；[SpOut_Nav](#bus-spout_nav).`L205_Mach` [1]；[SpOut_Nav](#bus-spout_nav).`L206_Computed_Airspeed` [1]；[SpOut_Nav](#bus-spout_nav).`L210_True_Airspeed` [1]；[SpOut_Nav](#bus-spout_nav).`L211_Comb_Altitude_Rate` [1]；[SpOut_Nav](#bus-spout_nav).`L212_Altitude_Rate` [1]；[SpOut_Nav](#bus-spout_nav).`L213_Static_Air_Temperature` [1]；[SpOut_Nav](#bus-spout_nav).`L242_Total_Pressure` [1]；[SpOut_Nav](#bus-spout_nav).`L245_Static_Pressure` [1]；[SpOut_Nav](#bus-spout_nav).`L320_Magnetic_Heading` [1]；[SpOut_Nav](#bus-spout_nav).`L321_True_Heading` [1]；[SpOut_Nav](#bus-spout_nav).`L322_Mag_Declination` [1]；[SpOut_Nav](#bus-spout_nav).`L365_Vertical_Velocity` [1]；[SpOut_Nav](#bus-spout_nav).`L366_North_Velocity` [1]；[SpOut_Nav](#bus-spout_nav).`L367_East_Velocity` [1]；[SpOut_Nav](#bus-spout_nav).`L324_Pitch_Angle` [1]；[SpOut_Nav](#bus-spout_nav).`L325_Roll_Angle` [1]；[SpOut_Nav](#bus-spout_nav).`L331_Body_Longit_Accel` [1]；[SpOut_Nav](#bus-spout_nav).`L332_Body_Lateral_Accel` [1]；[SpOut_Nav](#bus-spout_nav).`L333_Body_Normal_Accel` [1]；[SpOut_Nav](#bus-spout_nav).`L336_Pitch_Attitude_Rate` [1]；[SpOut_Nav](#bus-spout_nav).`L337_Roll_Attitude_Rate` [1]；[SpOut_Nav](#bus-spout_nav).`L340_Heading_Angle_Rate` [1]；[SpOut_Nav](#bus-spout_nav).`L164_Radio_Altitude` [1]。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | single | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-spout_nav"></a>

#### SpOut_Nav

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=SpOut_Nav)`。直接字段：**31**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `L310_Combined_Latitude` | [Bus: SpNavData_Double](#bus-spnavdata_double) | 1 | Fixed | real | — | — | — |
| 2 | `L311_Combined_Longitude` | [Bus: SpNavData_Double](#bus-spnavdata_double) | 1 | Fixed | real | — | — | — |
| 3 | `L076_GPS_Height` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 4 | `L107_Angle_Of_Attack` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 5 | `L110_Sideslip_Angle` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 6 | `L112_Ground_Speed` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 7 | `L203_Pressure_Altitude` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 8 | `L204_Baro_Corrected_Altitude` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 9 | `L205_Mach` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 10 | `L206_Computed_Airspeed` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 11 | `L210_True_Airspeed` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 12 | `L211_Comb_Altitude_Rate` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 13 | `L212_Altitude_Rate` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 14 | `L213_Static_Air_Temperature` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 15 | `L242_Total_Pressure` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 16 | `L245_Static_Pressure` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 17 | `L320_Magnetic_Heading` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 18 | `L321_True_Heading` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 19 | `L322_Mag_Declination` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 20 | `L365_Vertical_Velocity` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 21 | `L366_North_Velocity` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 22 | `L367_East_Velocity` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 23 | `L324_Pitch_Angle` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 24 | `L325_Roll_Angle` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 25 | `L331_Body_Longit_Accel` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 26 | `L332_Body_Lateral_Accel` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 27 | `L333_Body_Normal_Accel` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 28 | `L336_Pitch_Attitude_Rate` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 29 | `L337_Roll_Attitude_Rate` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 30 | `L340_Heading_Angle_Rate` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |
| 31 | `L164_Radio_Altitude` | [Bus: SpNavData_Single](#bus-spnavdata_single) | 1 | Fixed | real | — | — | — |

<a id="bus-tbooldata"></a>

#### tBoolData

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tBoolData)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | boolean | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-ts16data"></a>

#### tS16Data

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tS16Data)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | int16 | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-ts32data"></a>

#### tS32Data

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tS32Data)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | int32 | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-ts64data"></a>

#### tS64Data

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tS64Data)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | int64 | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-ts8data"></a>

#### tS8Data

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tS8Data)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | int8 | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-tu16data"></a>

#### tU16Data

来源：[SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>)；`data/chunk0.xml` → `DD.ENTRY(Name=tU16Data)`。直接字段：**3**。

Bus 对象属性：`Alignment=-1`；`PreserveElementDimensions=0`；`DataScope=Auto`。

本结构字段共有属性：`DimensionsMode=Fixed`；`Complexity=real`；`SamplingMode=Sample based`；`SampleTime=-1`。

| 序号 | 字段 | 类型（原文） | Dimensions | 维度模式 | 复实性 | 单位（原文） | 范围（原文） | 说明（原文） |
|---|---|---|---|---|---|---|---|---|
| 1 | `data` | uint16 | 1 | Fixed | real | — | — | — |
| 2 | `valid` | boolean | 1 | Fixed | real | — | — | — |
| 3 | `reset` | boolean | 1 | Fixed | real | — | — | — |

<a id="bus-operations"></a>

## 6. 模型内 Bus 构造与赋值位置

Bus Selector 提取成员，Bus Creator 按输入组装结构，Bus Assignment 在既有 Bus 上指定写入成员。下表保留当前模型中的组装/赋值位置，帮助从字段定义回到传递链路；不把一条赋值列表误当成 Bus 的完整定义。

### 6.1 Bus Assignment

| 模型／块路径 | SID | AssignedSignals（原文） |
|---|---|---|
| VE25SP/Bus<br>Assignment | 446 | navValid,isBfccActv,isBfccActvValid |
| VE25SP/NavSpApp/Subsystem/Bus<br>Assignment | 78 | ftadcVoterCAS_mps |
| Calc_MPIOData/Bus<br>Assignment | 120 | EgyWkSts,HiVltgSts,UsablePwr_KW,HiVltgSW,CenterGravity,MotorCheck,SurfaceCheck,PreflightCheckValid,GrossWeight,FMS_Alt_Target,FMS_Alt_Target_Valid,FMS_Spd_Target,FMS_TrueHeading_Target,FMS_Track_Target,FMS_Spd_Target_Valid,FMS_TrueHeading_Target_Valid,FMS_Track_Target_Valid,FMS_Roll_Command,FMS_Vertical_Speed_Target,FMS_Roll_Command_Status,FMS_Vertical_Speed_Target_Valid,FMS_Trans_Request,FMS_LNAV_Request,FMS_VNAV_Request,FMS_VNAV_Status,FMS_FPA_Target,FMS_FPA_Target_Valid |
| Calc_MCUSCUData/ForEachSubsystem/Bus<br>Assignment | 74 | SpdFbk,Power,FaultLevel,WorkMode |
| Calc_MCUSCUData/ForEachSubsystem1/Bus<br>Assignment | 256 | SpdFbk,Power,FaultLevel,WorkMode |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Bus<br>Assignment | 23 | Valid_Status.Vel_TAS_Valid,Vel_TAS_mps,Reset_Status.Vel_TAS_Reset |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Bus<br>Assignment1 | 24 | Vel_CAS_mps,Valid_Status.Vel_CAS_Valid,Reset_Status.Vel_CAS_Reset |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Bus<br>Assignment2 | 25 | Valid_Status.h_BARO_Valid,h_BARO_m,Reset_Status.h_BARO_Reset |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Subsystem/Bus<br>Assignment2 | 192 | Valid_Status.Vel_Z_Valid,Vel_z_REO_mps,Reset_Status.Vel_Z_Reset |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Subsystem2/Bus<br>Assignment1 | 302 | Valid_Status.Alpha_ADS_Valid,Reset_Status.Alpha_ADS_Reset,Alpha_ADS_deg,Reset_Status.Beta_ADS_Reset,Valid_Status.Beta_ADS_Valid,Beta_ADS_deg |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Subsystem3/Bus<br>Assignment | 313 | Vel_CAS_mps,Valid_Status.Vel_CAS_Valid,Reset_Status.Vel_CAS_Reset |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Subsystem3/Bus<br>Assignment1 | 314 | Vel_CAS_mps,Valid_Status.Vel_CAS_Valid,Reset_Status.Vel_CAS_Reset |
| Calc_PilotIn_ToClaw/handleGCS/Bus<br>Assignment1 | 113 | PwrOn_PltIn_b,PwrOnVld_PltIn_b,HiVltgSts,MFD_PltIn |
| Calc_PilotIn_ToClaw/handlePilotIn/Bus<br>Assignment1 | 556 | TEST_IN.Enable_PLTLost_b,PwrOn_PltIn_b,PwrOnVld_PltIn_b,HiVltgSts,MFD_PltIn,TEST_IN.Enable_Degrade_b |
| Calc_FMSMsg_ToClaw/Bus<br>Assignment | 23 | Mass_AC_Kg,Xcg_AC_100MAC,FMS_Alt_Target,FMS_Alt_Target_Valid,FMS_Spd_Target,FMS_Heading_Target,FMS_Track_Target,FMS_Spd_Target_Valid,FMS_Heading_Target_Valid,FMS_Track_Target_Valid,FMS_Roll_Command,FMS_Vertical_Speed_Target,FMS_Roll_Command_Status,FMS_Vertical_Speed_Target_Valid,Trans_Request,FMS_LNAV_Request,FMS_VNAV_Request,FMS_VNAV_Status,FMS_Flight_Path_Angle_Target,FMS_Flight_Path_Angle_Target_Valid |
| Calc_CcdlMsg_Out/Calc_SysReady_LimitDispatchInfo/Bus<br>Assignment | 18 | Data.LimitDispathInfo,Data.SysReady |
| AdahruMsgsPreprocess/Bus<br>Assignment | 138 | CombLate,CombLong,GpsHeight,AOA,AOS,GroundSpeed,PressureAlt,BaroCorrectedAlt,Mach,CAS,TAS,GpsAltRate,AltRate,StaticAirTemp,TotalPressure,StaticPressure,MagnHeading,TrueHeaading,MagDeclination,VertVelocity,NorthVelocity,EastVelocity,Pitch,Roll,SforX,SforY,SforZ,Q,P,R,AdAltRate |
| AdahruMsgsPreprocess/Subsystem/Bus<br>Assignment | 150 | data |
| AdahruMsgsPreprocess/Subsystem1/Bus<br>Assignment | 155 | data |
| AdahruMsgsPreprocess/Subsystem2/Bus<br>Assignment | 163 | data |
| Calc_NavSourceSelection/Bus<br>Assignment | 528 | VoterCAS_mps |
| Calc_VotedFTADC/Bus<br>Assignment1 | 63 | ftadcStaticPressure_kPa,ftadcTotalPressure_kPa,ftadcAttackOfAngle_Deg,ftadcSideslipAngle_Deg,ftadcCAS_mps |
| Calc_VotedRA/SourceSelection/output/calcData/Bus<br>Assignment | 62 | raRadioAlt_M |
| Calc_VotedRA/SourceSelection/output/calcData/Bus<br>Assignment1 | 63 | raRadioAlt_M |
| Calc_VotedRA/SourceSelection/output/calcData/Bus<br>Assignment2 | 64 | ra1RadioAlt_M.data,ra1RadioAlt_M.valid,ra2RadioAlt_M.data,ra2RadioAlt_M.valid |

### 6.2 Bus Creator

`OutDataTypeStr` 未显式保存或为继承时，仅报告模型原文，不能将其直接等同某个命名 Bus。本表提供输入数、数据类型设置和源路径；实际字段结构仍需结合上游线名及下游类型约束。字段表已覆盖当前可定位的命名 Bus，未运行编译来确定匿名/继承 Bus 的最终类型。

| 模型／块路径 | SID | Inputs | OutDataTypeStr | NonVirtualBus |
|---|---|---|---|---|
| VE25SP/NavSpApp/Bus<br>Creator2 | 49 | 3 | [Bus: tNavLog](#bus-tnavlog) | on |
| VE25SP/NavSpApp/Subsystem/Subsystem/Bus<br>Creator | 89 | 3 | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator | 85 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator1 | 91 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator10 | 160 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator11 | 33 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator12 | 165 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator13 | 169 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator14 | 174 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator15 | 179 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator16 | 187 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator17 | 191 | 3 | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator18 | 195 | 3 | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator19 | 199 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator2 | 18 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator20 | 210 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator21 | 214 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator22 | 218 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator23 | 222 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator24 | 228 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator25 | 232 | 3 | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator26 | 236 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator3 | 152 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator4 | 19 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator5 | 35 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator6 | 36 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator7 | 37 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MPIOData/Bus<br>Creator8 | 20 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MPIOData/Bus<br>Creator9 | 156 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_SysInfo/Bus<br>Creator1 | 31 | 17 | [Bus: BUS_SysInfoData](#bus-bus_sysinfodata) | on |
| Calc_SPDispMsg_Out/Calc_SpDispMsg/Bus<br>Creator | 16 | 19 | [Bus: BUS_SpDispMsg](#bus-bus_spdispmsg) | on |
| Calc_MCUSCUData/Bus<br>Creator | 211 | 10 | [Bus: BUS_DevData](#bus-bus_devdata) | on |
| Calc_MCUSCUData/ForEachSubsystem/Bus<br>Creator1 | 25 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MCUSCUData/ForEachSubsystem/Bus<br>Creator2 | 15 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MCUSCUData/ForEachSubsystem/Bus<br>Creator3 | 29 | 3 | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MCUSCUData/ForEachSubsystem/Bus<br>Creator4 | 34 | 3 | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MCUSCUData/ForEachSubsystem1/Bus<br>Creator1 | 257 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MCUSCUData/ForEachSubsystem1/Bus<br>Creator2 | 258 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MCUSCUData/ForEachSubsystem1/Bus<br>Creator3 | 259 | 3 | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MCUSCUData/ForEachSubsystem1/Bus<br>Creator4 | 260 | 3 | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MCUSCUData/ForEachSubsystem2/Bus<br>Creator | 111 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_MCUSCUData/ForEachSubsystem2/Bus<br>Creator2 | 164 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MCUSCUData/ForEachSubsystem2/Bus<br>Creator3 | 175 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_MCUSCUData/ForEachSubsystem2/Bus<br>Creator5 | 206 | 3 | [Bus: tServoMsg](#bus-tservomsg) | on |
| Calc_GCSData/Bus<br>Creator1 | 229 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator10 | 260 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator11 | 261 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator12 | 262 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator13 | 263 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator14 | 264 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator15 | 265 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator16 | 266 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator17 | 267 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator18 | 268 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator19 | 269 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator2 | 270 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator20 | 271 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator21 | 230 | 3 | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_GCSData/Bus<br>Creator22 | 231 | 3 | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_GCSData/Bus<br>Creator23 | 232 | 3 | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_GCSData/Bus<br>Creator24 | 233 | 3 | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_GCSData/Bus<br>Creator25 | 272 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator26 | 273 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator27 | 274 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator28 | 275 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator29 | 276 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator3 | 277 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator30 | 278 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator31 | 279 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator32 | 280 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator33 | 281 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator34 | 282 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator35 | 283 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_GCSData/Bus<br>Creator36 | 284 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_GCSData/Bus<br>Creator37 | 285 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_GCSData/Bus<br>Creator38 | 286 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_GCSData/Bus<br>Creator4 | 287 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator41 | 469 | 39 | [Bus: BUS_GcsData](#bus-bus_gcsdata) | on |
| Calc_GCSData/Bus<br>Creator42 | 288 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator5 | 289 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator6 | 290 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator7 | 291 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator8 | 292 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Bus<br>Creator9 | 293 | data,valid,reset | [Bus: tU8Data](#bus-tu8data) | on |
| Calc_GCSData/Subsystem1/Bus<br>Creator | 163 | 2 | [Bus: tVoterState](#bus-tvoterstate) | on |
| Calc_GCSData/Subsystem4/Bus<br>Creator | 70 | 2 | [Bus: tVoterState](#bus-tvoterstate) | on |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Bus<br>Creator | 26 | Pos_XY_Valid,h_AMSL_Valid,h_BARO_Valid,h_AGL_Valid,Vel_XY_Valid,Vel_Z_Valid,Vel_CAS_Valid,Vel_TAS_Valid,Alpha_ADS_Valid,Beta_ADS_Valid,Att_RP_Valid,Att_Y_Valid,Rot_Valid,Sfor_Valid | [Bus: BUS_FCS_NAV_Valid_Status](#bus-bus_fcs_nav_valid_status) | on |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Bus<br>Creator1 | 27 | Pos_XY_Reset,h_AMSL_Reset,h_BARO_Reset,h_AGL_Reset,Vel_XY_Reset,Vel_Z_Reset,Vel_CAS_Reset,Vel_TAS_Reset,Alpha_ADS_Reset,Beta_ADS_Reset,Att_RP_Reset,Att_Y_Reset,Rot_Reset,Sfor_Reset | [Bus: BUS_FCS_NAV_Reset_Status](#bus-bus_fcs_nav_reset_status) | on |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Bus<br>Creator2 | 28 | 42 | [Bus: BUS_NAV_IN](#bus-bus_nav_in) | on |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Bus<br>Creator3 | 29 | Baro,CAS,TAS,BaroValid,CASValid,TASValid | [Bus: BUS_FtadcLog](#bus-bus_ftadclog) | on |
| Calc_NAVMsg_ToClaw/If Action<br>Subsystem/Subsystem1/Bus<br>Creator | 206 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_PilotIn_ToClaw/handleGCS/Bus<br>Creator | 114 | 7 | 未显式存储／继承 | 未显式存储 |
| Calc_PilotIn_ToClaw/handleGCS/Bus<br>Creator1 | 115 | 22 | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | on |
| Calc_PilotIn_ToClaw/handlePilotIn/Bus<br>Creator1 | 557 | 22 | [Bus: BUS_PILOT_IN](#bus-bus_pilot_in) | on |
| Calc_SYSMsg_ToClaw/Bus<br>Creator | 9 | LiftMotor1,LiftMotor2,LiftMotor3,LiftMotor4,LiftMotor5,LiftMotor6,LiftMotor7,LiftMotor8,ThrustMotor1,ThrustMotor2,ThrustMotor3 | [Bus: BUS_SYSINFO_MTR](#bus-bus_sysinfo_mtr) | on |
| Calc_SYSMsg_ToClaw/Bus<br>Creator1 | 10 | AilLO,AilLI,AilRO,AilRI,EleL,EleR,RudL,RudR | [Bus: BUS_SYSINFO_ACT](#bus-bus_sysinfo_act) | on |
| Calc_SYSMsg_ToClaw/Bus<br>Creator2 | 11 | 5 | [Bus: BUS_SYSINFO_IN](#bus-bus_sysinfo_in) | on |
| Calc_SYSMsg_ToClaw/Bus<br>Creator3 | 12 | 2 | [Bus: BUS_SYSINFO_VCU](#bus-bus_sysinfo_vcu) | on |
| Calc_SYSMsg_ToClaw/For Each<br>Subsystem/Bus<br>Creator | 19 | Fault_Status,WorkMode,Power_kw,Spd_rpm | [Bus: BUS_SYSINFO_MTR_State](#bus-bus_sysinfo_mtr_state) | on |
| Calc_PilotInData/SignalComparotor1/Bus<br>Creator | 1256 | 2 | 未显式存储／继承 | 未显式存储 |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem/Bus<br>Creator | 1259 | 3 | [Bus: BUS_CmpState](#bus-bus_cmpstate) | on |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem/Bus<br>Creator1 | 1260 | 3 | [Bus: BUS_CmpState](#bus-bus_cmpstate) | on |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem/Bus<br>Creator2 | 1261 | 3 | [Bus: BUS_CmpState](#bus-bus_cmpstate) | on |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem/Bus<br>Creator3 | 1262 | 3 | [Bus: BUS_PilotCmpState](#bus-bus_pilotcmpstate) | on |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem1/Bus<br>Creator | 1273 | 3 | [Bus: BUS_CmpState](#bus-bus_cmpstate) | on |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem1/Bus<br>Creator1 | 1274 | 3 | [Bus: BUS_CmpState](#bus-bus_cmpstate) | on |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem1/Bus<br>Creator2 | 1275 | 3 | [Bus: BUS_CmpState](#bus-bus_cmpstate) | on |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem1/Bus<br>Creator3 | 1276 | 3 | [Bus: BUS_PilotCmpState](#bus-bus_pilotcmpstate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator | 93 | 3 | [Bus: Bus_VDTState](#bus-bus_vdtstate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator1 | 100 | 3 | [Bus: Bus_VDTState](#bus-bus_vdtstate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator10 | 391 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator11 | 410 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator12 | 428 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator13 | 446 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator14 | 464 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator15 | 482 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator16 | 500 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator17 | 518 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator18 | 536 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator19 | 554 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator2 | 121 | 3 | [Bus: Bus_VDTState](#bus-bus_vdtstate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator3 | 142 | 3 | [Bus: Bus_VDTState](#bus-bus_vdtstate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator4 | 166 | 3 | [Bus: Bus_VDTState](#bus-bus_vdtstate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator40 | 1217 | 2 | 未显式存储／继承 | 未显式存储 |
| Calc_PilotInData/Subsystem1/Bus<br>Creator5 | 184 | 3 | [Bus: Bus_VDTState](#bus-bus_vdtstate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator6 | 305 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator7 | 336 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator8 | 354 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Subsystem1/Bus<br>Creator9 | 373 | 3 | [Bus: Bus_GPIOState](#bus-bus_gpiostate) | on |
| Calc_PilotInData/Voter/Bus<br>Creator | 1350 | 2 | [Bus: BUS_PilotInData](#bus-bus_pilotindata) | on |
| Calc_PilotInData/Voter/For Each<br>Subsystem/Bus<br>Creator | 1369 | 3 | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_PilotInData/Voter/For Each<br>Subsystem1/Bus<br>Creator | 1436 | 3 | [Bus: tU32Data](#bus-tu32data) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator | 151 | 41 | [Bus: BUS_NavData](#bus-bus_navdata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator1 | 152 | 33 | [Bus: BUS_NavDataState](#bus-bus_navdatastate) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator10 | 153 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator11 | 154 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator12 | 155 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator13 | 156 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator14 | 157 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator15 | 158 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator16 | 159 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator17 | 160 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator18 | 161 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator19 | 162 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator2 | 163 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator20 | 164 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator21 | 165 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator22 | 166 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator23 | 167 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator24 | 168 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator25 | 169 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator26 | 170 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator27 | 171 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator28 | 172 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator29 | 173 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator3 | 174 | data,valid,reset | [Bus: tDoubleData](#bus-tdoubledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator30 | 175 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator31 | 176 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator32 | 177 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator4 | 178 | data,valid,reset | [Bus: tDoubleData](#bus-tdoubledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator5 | 179 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator6 | 180 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator7 | 181 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator8 | 182 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcData/Bus<br>Creator9 | 183 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcState/Bus<br>Creator | 485 | VoterType,ChannelUsed | [Bus: tVoterState](#bus-tvoterstate) | on |
| Calc_NavSourceSelection/SourceSelection/output/calcState1/Bus<br>Creator | 500 | VoterType,ChannelUsed | [Bus: tVoterState](#bus-tvoterstate) | on |
| Calc_VotedFTADC/Bus<br>Creator1 | 22 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_VotedFTADC/Bus<br>Creator2 | 23 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_VotedFTADC/Bus<br>Creator3 | 24 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_VotedFTADC/Bus<br>Creator4 | 25 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_VotedFTADC/Bus<br>Creator5 | 26 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_VotedRA/SourceSelection/output/calcData/Bus<br>Creator1 | 65 | data,valid,reset | [Bus: tSingleData](#bus-tsingledata) | on |
| Calc_VotedRA/SourceSelection/output/calcState/Bus<br>Creator | 75 | VoterType,ChannelUsed | [Bus: tVoterState](#bus-tvoterstate) | on |
| Calc_VotedCAS/Bus<br>Creator | 248 | 3 | [Bus: tSingleData](#bus-tsingledata) | on |

### 6.3 For Each 的结构分区

分区参数保存在 For Each 所在系统的 Inport/Outport 上。下表逐端口列出保存值；单个 Bus 的成员维度、输入分区和输出拼接属于不同层次。13 组实例的输入分区均沿第 1 维、每片宽度 1、偏移 0，输出均沿第 1 维拼接。

| For Each 所在系统 | 方向 | 端口名 | SID | Partition | 分区维 | 分区宽度 | 分区偏移 | 拼接维 |
|---|---|---|---|---|---|---|---|---|
| Calc_MCUSCUData/ForEachSubsystem | 输入 | MCU | 10 | on | 1 | 1 | 0 | — |
| Calc_MCUSCUData/ForEachSubsystem | 输出 | LiftMcuData | 75 | — | — | — | — | 1 |
| Calc_MCUSCUData/ForEachSubsystem1 | 输入 | MCU | 253 | on | 1 | 1 | 0 | — |
| Calc_MCUSCUData/ForEachSubsystem1 | 输出 | LiftMcuData | 299 | — | — | — | — | 1 |
| Calc_MCUSCUData/ForEachSubsystem2 | 输入 | SCU | 119 | on | 1 | 1 | 0 | — |
| Calc_MCUSCUData/ForEachSubsystem2 | 输出 | ServoData | 210 | — | — | — | — | 1 |
| Calc_SYSMsg_ToClaw/For Each<br>Subsystem | 输入 | MCU | 18 | on | 1 | 1 | 0 | — |
| Calc_SYSMsg_ToClaw/For Each<br>Subsystem | 输出 | McuState | 28 | — | — | — | — | 1 |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem | 输入 | dataIn | 1258 | on | 1 | 1 | 0 | — |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem | 输出 | GpioCmpState | 1270 | — | — | — | — | 1 |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem1 | 输入 | dataIn | 1272 | on | 1 | 1 | 0 | — |
| Calc_PilotInData/SignalComparotor1/For Each<br>Subsystem1 | 输出 | VdtCmpState | 1284 | — | — | — | — | 1 |
| Calc_PilotInData/Voter/For Each<br>Subsystem | 输入 | data | 1365 | on | 1 | 1 | 0 | — |
| Calc_PilotInData/Voter/For Each<br>Subsystem | 输入 | cmpState | 1366 | on | 1 | 1 | 0 | — |
| Calc_PilotInData/Voter/For Each<br>Subsystem | 输入 | data0 | 1367 | on | 1 | 1 | 0 | — |
| Calc_PilotInData/Voter/For Each<br>Subsystem | 输出 | VdtOut | 1429 | — | — | — | — | 1 |
| Calc_PilotInData/Voter/For Each<br>Subsystem1 | 输入 | data | 1432 | on | 1 | 1 | 0 | — |
| Calc_PilotInData/Voter/For Each<br>Subsystem1 | 输入 | cmpState | 1433 | on | 1 | 1 | 0 | — |
| Calc_PilotInData/Voter/For Each<br>Subsystem1 | 输入 | data0 | 1434 | on | 1 | 1 | 0 | — |
| Calc_PilotInData/Voter/For Each<br>Subsystem1 | 输出 | GpioOut | 1496 | — | — | — | — | 1 |
| Calc_CcdlMsg_Out/Calc_SysReady_LimitDispatchInfo/For Each<br>Subsystem | 输入 | In1 | 71 | on | 1 | 1 | 0 | — |
| Calc_CcdlMsg_Out/Calc_SysReady_LimitDispatchInfo/For Each<br>Subsystem | 输出 | Out1 | 77 | — | — | — | — | 1 |
| AdahruMsgsPreprocess/Subsystem | 输入 | IN | 151 | on | 1 | 1 | 0 | — |
| AdahruMsgsPreprocess/Subsystem | 输出 | OUT | 146 | — | — | — | — | 1 |
| AdahruMsgsPreprocess/Subsystem1 | 输入 | IN | 154 | on | 1 | 1 | 0 | — |
| AdahruMsgsPreprocess/Subsystem1 | 输出 | OUT | 159 | — | — | — | — | 1 |
| AdahruMsgsPreprocess/Subsystem2 | 输入 | IN | 162 | on | 1 | 1 | 0 | — |
| AdahruMsgsPreprocess/Subsystem2 | 输出 | OUT | 167 | — | — | — | — | 1 |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;GpsHeight&gt; | 28 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;CombLate&gt; | 29 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;CombLong&gt; | 30 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;GpsAltRate&gt; | 31 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;AdAltRate&gt; | 32 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;NorthVelocity&gt; | 33 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;EastVelocity&gt; | 34 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;TrueHeaading&gt; | 35 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;Pitch&gt; | 36 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;Roll&gt; | 37 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;SforX&gt; | 38 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;SforY&gt; | 39 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;SforZ&gt; | 40 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;Q&gt; | 41 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;P&gt; | 42 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输入 | &lt;R&gt; | 43 | on | 1 | 1 | 0 | — |
| Calc_NavSourceSelection/SourceSelection/AdAhruWeightCalc/For Each<br>Subsystem | 输出 | Out1 | 74 | — | — | — | — | 1 |

<a id="enums"></a>

## 7. 枚举定义

只列本次当前引用 Bus 的字段实际使用的枚举。数值和枚举项保持定义原文；不将枚举原名翻译为未经接口文件确认的业务码表。

<a id="enum-enum_actstate"></a>

### ENUM_ACTState

来源：[ENUM_ACTState.m](<D:/666/VE25/sp/04_Design/common/libs/sldd/ENUM_ACTState.m>)；MATLAB 枚举类，基类 `int32`。该类型由 `.m` 定义，未在 SLDD 中重复定义。

getDefaultValue 函数原文：`retVal = ENUM_ACTState.PowerOff;`。

| 枚举项 | 数值原文 |
|---|---|
| PowerOff | 0 |
| Jam | 1 |
| Float | 2 |
| Osi | 3 |
| Test | 4 |
| Valid | 5 |

<a id="enum-enum_drms_fccid"></a>

### ENUM_DRMS_FccId

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`DD.ENTRY(Name=ENUM_DRMS_FccId)`。

属性：`StorageType=未显式指定`；`DefaultValue=未显式指定`；`AddClassNameToEnumNames=1`。

| 枚举项 | 数值原文 | 说明原文 |
|---|---|---|
| NONE | 0 | — |
| PFCC1 | 1 | — |
| PFCC2 | 2 | — |
| PFCC3 | 3 | — |
| BFCC | 4 | — |

<a id="enum-enum_drms_vtol_state"></a>

### ENUM_DRMS_VTOL_STATE

来源：[SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>)；`DD.ENTRY(Name=ENUM_DRMS_VTOL_STATE)`。

属性：`StorageType=未显式指定`；`DefaultValue=未显式指定`；`AddClassNameToEnumNames=1`。

| 枚举项 | 数值原文 | 说明原文 |
|---|---|---|
| Unknown | 0 | — |
| Disarm | 1 | — |
| Arm | 2 | — |
| MtrCheck | 3 | — |
| SurfCheck | 4 | — |
| LiftOff | 5 | — |
| MC | 6 | — |
| TRANS | 7 | — |
| FW | 8 | — |

<a id="enum-enum_fms_vnav_request"></a>

### ENUM_FMS_VNAV_Request

来源：[SP_Enum.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_Enum.sldd>)；`DD.ENTRY(Name=ENUM_FMS_VNAV_Request)`。

属性：`StorageType=int32`；`DefaultValue=Invalid`；`AddClassNameToEnumNames=1`。

| 枚举项 | 数值原文 | 说明原文 |
|---|---|---|
| Invalid | 0 | — |
| VCLB | 1 | — |
| VALT | 2 | — |
| VALTC | 3 | — |

<a id="enum-enum_hivltg_sts"></a>

### ENUM_HiVltg_Sts

来源：[ENUM_HiVltg_Sts.m](<D:/666/VE25/sp/04_Design/common/libs/sldd/ENUM_HiVltg_Sts.m>)；MATLAB 枚举类，基类 `int32`。该类型由 `.m` 定义，未在 SLDD 中重复定义。

getDefaultValue 函数原文：`retVal = ENUM_HiVltg_Sts.Unknow;`。

| 枚举项 | 数值原文 |
|---|---|
| Unknow | 0 |
| offline | 1 |
| Online | 2 |
| Fault | 3 |

<a id="enum-enum_key_mfd"></a>

### ENUM_Key_MFD

来源：[ENUM_Key_MFD.m](<D:/666/VE25/sp/04_Design/common/libs/sldd/ENUM_Key_MFD.m>)；MATLAB 枚举类，基类 `int32`。该类型由 `.m` 定义，未在 SLDD 中重复定义。

getDefaultValue 函数原文：`retVal = ENUM_Key_MFD.None;`。

| 枚举项 | 数值原文 |
|---|---|
| None | 0 |
| ModeUpgrade | 1 |
| MtrCheckIn | 2 |
| MtrCheckOut | 3 |
| SurfCheckIn | 4 |
| SurfCheckOut | 5 |
| SysReady | 6 |

<a id="enum-enum_pilotsource"></a>

### ENUM_PilotSource

来源：[SP_Enum.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_Enum.sldd>)；`DD.ENTRY(Name=ENUM_PilotSource)`。

属性：`StorageType=int32`；`DefaultValue=Primary`；`AddClassNameToEnumNames=1`。

| 枚举项 | 数值原文 | 说明原文 |
|---|---|---|
| Primary | 1 | — |
| SideStick | 3 | — |

<a id="enum-enum_votertype"></a>

### Enum_VoterType

来源：[SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>)；`DD.ENTRY(Name=Enum_VoterType)`。

属性：`StorageType=uint8`；`DefaultValue=未显式指定`；`AddClassNameToEnumNames=1`。

| 枚举项 | 数值原文 | 说明原文 |
|---|---|---|
| None | 0 | — |
| Fixed | 1 | — |
| SourceSelect | 2 | — |
| Voter | 3 | — |

<a id="sources"></a>

## 8. 数据字典与文件来源

### 8.1 字典引用层级

主模型直接绑定 `DD_VE25SP.sldd`。各引用模型还各自绑定其 DD 字典。文件名均在当前 `sp/04_Design` 范围内唯一定位；这属于静态文件解析，不替代 MATLAB 运行时搜索路径解析。

| 字典 | 直接引用 | 本文件 Bus / 枚举数 |
|---|---|---|
| [DD_AdahruMsgsPreprocess.sldd](<D:/666/VE25/sp/04_Design/AdahruMsgsPreprocess/specification/data/DD_AdahruMsgsPreprocess.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_CcdlMsg_Out.sldd](<D:/666/VE25/sp/04_Design/Calc_CcdlMsg_Out/specification/data/DD_Calc_CcdlMsg_Out.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_FMSMsg_ToClaw.sldd](<D:/666/VE25/sp/04_Design/Calc_FMSMsg_ToClaw/specification/data/DD_Calc_FMSMsg_ToClaw.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_GCSData.sldd](<D:/666/VE25/sp/04_Design/Calc_GCSData/specification/data/DD_Calc_GCSData.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_MCUSCUData.sldd](<D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/data/DD_Calc_MCUSCUData.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_MPIOData.sldd](<D:/666/VE25/sp/04_Design/Calc_MPIOData/specification/data/DD_Calc_MPIOData.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_NAVMsg_ToClaw.sldd](<D:/666/VE25/sp/04_Design/Calc_NAVMsg_ToClaw/specification/data/DD_Calc_NAVMsg_ToClaw.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_NavSourceSelection.sldd](<D:/666/VE25/sp/04_Design/Calc_NavSourceSelection/specification/data/DD_Calc_NavSourceSelection.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_PilotInData.sldd](<D:/666/VE25/sp/04_Design/Calc_PilotInData/specification/data/DD_Calc_PilotInData.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_PilotIn_ToClaw.sldd](<D:/666/VE25/sp/04_Design/Calc_PilotIn_ToClaw/specification/data/DD_Calc_PilotIn_ToClaw.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_SPDispMsg_Out.sldd](<D:/666/VE25/sp/04_Design/Calc_SPDispMsg_Out/specification/data/DD_Calc_SPDispMsg_Out.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_SYSMsg_ToClaw.sldd](<D:/666/VE25/sp/04_Design/Calc_SYSMsg_ToClaw/specification/data/DD_Calc_SYSMsg_ToClaw.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_SysInfo.sldd](<D:/666/VE25/sp/04_Design/Calc_SysInfo/specification/data/DD_Calc_SysInfo.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_VotedCAS.sldd](<D:/666/VE25/sp/04_Design/Calc_VotedCAS/specification/data/DD_Calc_VotedCAS.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_VotedFTADC.sldd](<D:/666/VE25/sp/04_Design/Calc_VotedFTADC/specification/data/DD_Calc_VotedFTADC.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_Calc_VotedRA.sldd](<D:/666/VE25/sp/04_Design/Calc_VotedRA/specification/data/DD_Calc_VotedRA.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_RAMsgsPreprocess.sldd](<D:/666/VE25/sp/04_Design/RAMsgsPreprocess/specification/data/DD_RAMsgsPreprocess.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [DD_VE25SP.sldd](<D:/666/VE25/sp/04_Design/VE25SP/specification/data/DD_VE25SP.sldd>) | `csSingleInstance.sldd`<br>`SP_ALL.sldd` | 0 / 0 |
| [SP_Enum.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_Enum.sldd>) | — | 0 / 11 |
| [SP_Parameters.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_Parameters.sldd>) | — | 1 / 0 |
| [SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>) | `SP_Enum.sldd` | 13 / 1 |
| [SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>) | — | 11 / 0 |
| [csSingleInstance.sldd](<D:/666/VE25/sp/04_Design/common/specification/configurations/csSingleInstance.sldd>) | — | 0 / 0 |
| [DD_SpLib.sldd](<D:/666/VE25/sp/04_Design/common/specification/data/DD_SpLib.sldd>) | `SP_Parameters.sldd`<br>`SP_State.sldd`<br>`SpMdlPrivate_DataType.sldd`<br>`SP_Step_New.sldd` | 2 / 0 |
| [SP_ALL.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_ALL.sldd>) | `SP_Step_New.sldd`<br>`SpMdlPrivate_DataType.sldd`<br>`SP_Parameters.sldd`<br>`DD_SpLib.sldd`<br>`SP_State.sldd`<br>`SP_InteriorBus.sldd` | 0 / 0 |
| [SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>) | `SP_Enum.sldd`<br>`SP_Parameters.sldd`<br>`SpMdlPrivate_DataType.sldd`<br>`SP_State.sldd`<br>`DD_SpLib.sldd` | 13 / 0 |
| [SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>) | `SP_Enum.sldd` | 45 / 2 |

`SpMdlPrivate_DataType.sldd` 虽带有旧名称，仍在当前实际引用链内，因此保留；停用的旧 SpMdl 模型及其不被当前链引用的字典不会因为存在于工作区就纳入当前模型。全盘盘点中的同名旧 Bus 已按引用闭包排除，当前 85 个定义不存在同名冲突。

### 8.2 来源快照

| 类别 | 文件 | SHA-256 |
|---|---|---|
| 模型 | [AdahruMsgsPreprocess.slx](<D:/666/VE25/sp/04_Design/AdahruMsgsPreprocess/specification/AdahruMsgsPreprocess.slx>) | `A5F9E7FAC266448447F929CF4BDC5CFF6C9B322A81D479995C61799977E65143` |
| 数据字典 | [DD_AdahruMsgsPreprocess.sldd](<D:/666/VE25/sp/04_Design/AdahruMsgsPreprocess/specification/data/DD_AdahruMsgsPreprocess.sldd>) | `3BA9146AE82C301122FF6EE2A527F5B3AFB931DE5B0B0FED5AD7F255CDE6FAC2` |
| 模型 | [Calc_CcdlMsg_Out.slx](<D:/666/VE25/sp/04_Design/Calc_CcdlMsg_Out/specification/Calc_CcdlMsg_Out.slx>) | `A7FBAC6DFFE00A2EB2B838E0F65B9312D04260E5820BD39CE02A3CA698B40DF8` |
| 数据字典 | [DD_Calc_CcdlMsg_Out.sldd](<D:/666/VE25/sp/04_Design/Calc_CcdlMsg_Out/specification/data/DD_Calc_CcdlMsg_Out.sldd>) | `7937B7AC114C9C6EFF1E1E5F41017D57424C72A99EE23E44B62FC1CA00E88CF9` |
| 模型 | [Calc_FMSMsg_ToClaw.slx](<D:/666/VE25/sp/04_Design/Calc_FMSMsg_ToClaw/specification/Calc_FMSMsg_ToClaw.slx>) | `8AFD3DAFFF28413E431F93C6924235202687E863F70C3981F42A52C7253A6F70` |
| 数据字典 | [DD_Calc_FMSMsg_ToClaw.sldd](<D:/666/VE25/sp/04_Design/Calc_FMSMsg_ToClaw/specification/data/DD_Calc_FMSMsg_ToClaw.sldd>) | `3261CF102E2420C0009B086536F9FD27849FA91147E668FD8F7701B4587F155B` |
| 模型 | [Calc_GCSData.slx](<D:/666/VE25/sp/04_Design/Calc_GCSData/specification/Calc_GCSData.slx>) | `D92EB5C7C942656ED04C6AF72D11EEB821AA3316256DB308E27DBF1E9F0E8BCF` |
| 数据字典 | [DD_Calc_GCSData.sldd](<D:/666/VE25/sp/04_Design/Calc_GCSData/specification/data/DD_Calc_GCSData.sldd>) | `C4B36F61FD0F5342DD7DA9EA4EB91244D85A67EB3EA7CEB33BAC0E6E4D81110C` |
| 模型 | [Calc_MCUSCUData.slx](<D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/Calc_MCUSCUData.slx>) | `0892EDC640482881BAE00D55D151F23181CA1027A479F6AC0B6F51E9E9978A04` |
| 数据字典 | [DD_Calc_MCUSCUData.sldd](<D:/666/VE25/sp/04_Design/Calc_MCUSCUData/specification/data/DD_Calc_MCUSCUData.sldd>) | `073F52895CE3A17D40E56197252AC45107D14DFD85E65632252A2E5F5E24CE0B` |
| 模型 | [Calc_MPIOData.slx](<D:/666/VE25/sp/04_Design/Calc_MPIOData/specification/Calc_MPIOData.slx>) | `A5B051AE97E92967CCF9E101D137D566B78DC55C57FC90B78910BA7F841F2FE6` |
| 数据字典 | [DD_Calc_MPIOData.sldd](<D:/666/VE25/sp/04_Design/Calc_MPIOData/specification/data/DD_Calc_MPIOData.sldd>) | `DADABF7DCF8427E30CC4C7B123376D2C4384781CD02C5FFE55ECB59E7EAEEA61` |
| 模型 | [Calc_NAVMsg_ToClaw.slx](<D:/666/VE25/sp/04_Design/Calc_NAVMsg_ToClaw/specification/Calc_NAVMsg_ToClaw.slx>) | `62DDD6AFB7D4CD3E99C2FF7D7F1902B557FDDE2EE948A34B9FF29984C60F161C` |
| 数据字典 | [DD_Calc_NAVMsg_ToClaw.sldd](<D:/666/VE25/sp/04_Design/Calc_NAVMsg_ToClaw/specification/data/DD_Calc_NAVMsg_ToClaw.sldd>) | `89561C9ED14EEFC4886979FD73BC60B535656708981D55311BCD1062B1320328` |
| 模型 | [Calc_NavSourceSelection.slx](<D:/666/VE25/sp/04_Design/Calc_NavSourceSelection/specification/Calc_NavSourceSelection.slx>) | `BE517B33C48086D7B6173C32205CEE0F3C77AFE5D24BE37914388EF6A80143DE` |
| 数据字典 | [DD_Calc_NavSourceSelection.sldd](<D:/666/VE25/sp/04_Design/Calc_NavSourceSelection/specification/data/DD_Calc_NavSourceSelection.sldd>) | `85D9A9F30A8F8CF63B570891714BF1F2CDAE5FE01D769FBCAD27F66279BE6944` |
| 模型 | [Calc_PilotInData.slx](<D:/666/VE25/sp/04_Design/Calc_PilotInData/specification/Calc_PilotInData.slx>) | `7DEB3921F4C73ACB63650353A6498AA629BA674A366511183B9F5F10D5BD89A2` |
| 数据字典 | [DD_Calc_PilotInData.sldd](<D:/666/VE25/sp/04_Design/Calc_PilotInData/specification/data/DD_Calc_PilotInData.sldd>) | `BDC54A94D7FF975AF447A748C200340262BF5430D55C9D3C9F07567BD33A777A` |
| 模型 | [Calc_PilotIn_ToClaw.slx](<D:/666/VE25/sp/04_Design/Calc_PilotIn_ToClaw/specification/Calc_PilotIn_ToClaw.slx>) | `3CE1556CC2EBED6260936810D9EB2FCB82DFF0BB0608205C79CAA6CD51623462` |
| 数据字典 | [DD_Calc_PilotIn_ToClaw.sldd](<D:/666/VE25/sp/04_Design/Calc_PilotIn_ToClaw/specification/data/DD_Calc_PilotIn_ToClaw.sldd>) | `32B2533D46B85F18467D5D84A9619BA5C1A14A88331E649B7073CFBE46C6EC8C` |
| 模型 | [Calc_SPDispMsg_Out.slx](<D:/666/VE25/sp/04_Design/Calc_SPDispMsg_Out/specification/Calc_SPDispMsg_Out.slx>) | `D9296A92646921701B7831E9A129C021326442FE001971639A4E33B45739A557` |
| 数据字典 | [DD_Calc_SPDispMsg_Out.sldd](<D:/666/VE25/sp/04_Design/Calc_SPDispMsg_Out/specification/data/DD_Calc_SPDispMsg_Out.sldd>) | `3A2D40663089BC871859A2475FC6ECC3769B8CB5D6E8A0A97D286A70F8685B52` |
| 模型 | [Calc_SYSMsg_ToClaw.slx](<D:/666/VE25/sp/04_Design/Calc_SYSMsg_ToClaw/specification/Calc_SYSMsg_ToClaw.slx>) | `9B5A3035D9D0165AAB3319301D24F40A59FFFE80E5C691143F516E396FA4617E` |
| 数据字典 | [DD_Calc_SYSMsg_ToClaw.sldd](<D:/666/VE25/sp/04_Design/Calc_SYSMsg_ToClaw/specification/data/DD_Calc_SYSMsg_ToClaw.sldd>) | `B708FF622A438FA37854D8108A0C9BEE86C2DA940389212D0C6EB8EB1189723F` |
| 模型 | [Calc_SysInfo.slx](<D:/666/VE25/sp/04_Design/Calc_SysInfo/specification/Calc_SysInfo.slx>) | `67B5607BC06A5AB57EB9CE7DC017A2B22E2CECE00E3153EC6EEF0E27AD6B1C43` |
| 数据字典 | [DD_Calc_SysInfo.sldd](<D:/666/VE25/sp/04_Design/Calc_SysInfo/specification/data/DD_Calc_SysInfo.sldd>) | `CCF552C9B90FBA2D83EDB540BF32832B3D04EE9C884B617F7548121D1AFA1CD5` |
| 模型 | [Calc_VotedCAS.slx](<D:/666/VE25/sp/04_Design/Calc_VotedCAS/specification/Calc_VotedCAS.slx>) | `807AD66E0B5C08BBB3592F75D0363879DF7B29EB20A2146373576764F12BC6D6` |
| 数据字典 | [DD_Calc_VotedCAS.sldd](<D:/666/VE25/sp/04_Design/Calc_VotedCAS/specification/data/DD_Calc_VotedCAS.sldd>) | `99AFA0063FFA5B912B5B25C4DF1C11BEE2EE566313561730D8CC48CA3A67E9E9` |
| 模型 | [Calc_VotedFTADC.slx](<D:/666/VE25/sp/04_Design/Calc_VotedFTADC/specification/Calc_VotedFTADC.slx>) | `8D469E282C99EF5DD471EA848EA0AA65B30B2B30342753FF06AAD6E1B1D5BEE6` |
| 数据字典 | [DD_Calc_VotedFTADC.sldd](<D:/666/VE25/sp/04_Design/Calc_VotedFTADC/specification/data/DD_Calc_VotedFTADC.sldd>) | `5CA2C82156D2A78A013FF71A82E07E2993672FF376045AC032CE8E266C1DA109` |
| 模型 | [Calc_VotedRA.slx](<D:/666/VE25/sp/04_Design/Calc_VotedRA/specification/Calc_VotedRA.slx>) | `3C07C33A91C9B4920D527FC12DE87DE20E76502F8ECA540F757F2C92DD099197` |
| 数据字典 | [DD_Calc_VotedRA.sldd](<D:/666/VE25/sp/04_Design/Calc_VotedRA/specification/data/DD_Calc_VotedRA.sldd>) | `AE7A883646960F9DE4C79D4080E8FA6C9B8901E38FE95A25DECF997D0AB6E1C6` |
| 模型 | [RAMsgsPreprocess.slx](<D:/666/VE25/sp/04_Design/RAMsgsPreprocess/specification/RAMsgsPreprocess.slx>) | `6FDCBEAD97A9EB0023601C9D44E2A23EB9768DA8814910681943E5E826815213` |
| 数据字典 | [DD_RAMsgsPreprocess.sldd](<D:/666/VE25/sp/04_Design/RAMsgsPreprocess/specification/data/DD_RAMsgsPreprocess.sldd>) | `AA70A6DDF3B44A87DDE3C43CF0B78FF34764E2547E8DF9F4C955BCD11E4C765F` |
| 模型 | [VE25SP.slx](<D:/666/VE25/sp/04_Design/VE25SP/specification/VE25SP.slx>) | `20D8C7DE2A103D8FF298BCD5BB0C28F73AA6CCC2E27D43D0F5AB9952365F77E4` |
| 数据字典 | [DD_VE25SP.sldd](<D:/666/VE25/sp/04_Design/VE25SP/specification/data/DD_VE25SP.sldd>) | `9BCDC82722DE11771CF1C1665544200829C9AD50DC2C69BF2EBF1D46F313B93E` |
| 枚举类 | [ENUM_ACTState.m](<D:/666/VE25/sp/04_Design/common/libs/sldd/ENUM_ACTState.m>) | `69999573d46faec0f502ae7a7c328637e82cbb5a31dac699be479698fc8b3cff` |
| 枚举类 | [ENUM_HiVltg_Sts.m](<D:/666/VE25/sp/04_Design/common/libs/sldd/ENUM_HiVltg_Sts.m>) | `ba1c5f96c93e3dadb268f42d1dd58e547c31ce42755b8ee72c0c1430f2c53c1c` |
| 枚举类 | [ENUM_Key_MFD.m](<D:/666/VE25/sp/04_Design/common/libs/sldd/ENUM_Key_MFD.m>) | `096555ac80d2c1b7cc650f2deea9e9d48f07599b44d13937e62dc95d60a7357b` |
| 数据字典 | [SP_Enum.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_Enum.sldd>) | `3744F05AE641BD9EAD969DA7A10458C9CE018EE92828E8CF37CC2AD6E7F5BD04` |
| 数据字典 | [SP_Parameters.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_Parameters.sldd>) | `3B89CDE7F5987DD0CDBCC440A1A4EC91CE7BE8CFE84F0761296C656FA10F38AB` |
| 数据字典 | [SP_State.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SP_State.sldd>) | `10B55E65511E21A7AD3E376653F985FAFAD2C37CB6D1B3FB8A187E69062B0164` |
| 数据字典 | [SpMdlPrivate_DataType.sldd](<D:/666/VE25/sp/04_Design/common/libs/sldd/SpMdlPrivate_DataType.sldd>) | `E759D089EE8CF57876AD182E553ACE204672624A678F6C0F0F0A9F8D747F311D` |
| 实际链接共享库 | [SignalProcessingLib.slx](<D:/666/VE25/sp/04_Design/common/specification/block_libraries/SignalProcessingLib.slx>) | `0353145198448149a44b5d72c19fa4cab505275148c06219b414bb2c4e44f490` |
| 数据字典 | [csSingleInstance.sldd](<D:/666/VE25/sp/04_Design/common/specification/configurations/csSingleInstance.sldd>) | `2CBF24E1031A79F1EE037E49B6A4E0B250327A5CB3513DCECF5F52BEA2ACF9FD` |
| 数据字典 | [DD_SpLib.sldd](<D:/666/VE25/sp/04_Design/common/specification/data/DD_SpLib.sldd>) | `557DBF7AE4B2D8C13B876DAEFACF648F64E2F32680621F7F2FFCE5EEE59D367D` |
| 数据字典 | [SP_ALL.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_ALL.sldd>) | `4823DA964D265B93AE39A59A9D1954E8A88AE3F440C55E151D548602219544DC` |
| 数据字典 | [SP_InteriorBus.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_InteriorBus.sldd>) | `5356B8602E6B3948291CA5410F5CE444D1277E5141AB93C21CF77D11D81C01F5` |
| 数据字典 | [SP_Step_New.sldd](<D:/666/VE25/sp/04_Design/common/specification/data_types/SP_Step_New.sldd>) | `10929FF4B70DAEB560A628EFAA5B75134F83E1E859699C83C823312138524DAC` |

<a id="verification"></a>

## 9. 核验与使用限制

- 18 个模型的系统 XML 均通过 System Ref 可达；17 个 ModelReference 均定位到实际文件。
- 4,686 条已保存连接的两端均可定位；805 个 From 的同系统 local Goto 关系均唯一解析。注释块与其连接不作为活动链路证据。
- 66 个非空 ModelReference 保存 Bus 接口类型与对应子模型根端口声明一致。
- 字典字段的顺序、类型、维度已用独立 XML 解析方式交叉核对；全量字段表覆盖 85 个 Bus、1,430 个直接字段。
- 当前 Bus 引用均能定位到字典定义；裸类型名 `SpCcdlMsg` 已单独保留并解析。相关外部枚举由实际 `.m` 源文件补足。
- 数据字典的绝大多数 Description/DocUnits 为空；没有为这些字段补写来源不明的单位、码表或物理含义。
- 匿名或继承类型的 Bus Creator 按保存参数列示；未将静态解析称为编译验证。模型内接口取值、采样时序、活动变体、运行时维度与实际报文布局应另行验证。
- 本文是当前工作区定义的阅读索引。模型、字典或枚举源码发生变化后，应重新生成并对照来源 SHA-256；本文不替代工程文件正文。
