# MBD Workbench DO178C 中文操作手册

用户指南

本手册介绍 MBD Workbench 的安装、工程组织、模型与代码检查、测试编写和执行、质量跟踪以及常用 API，供使用 MBD Workflow Manager（启动命令 `mbdWorkflowTool`）时对照查阅。

原文：**MBD Workbench (DO-178C) — User Guide**，见 [英文操作手册](D:/666/VE25/sp/help/User_Guide.docx)。

译制日期：2026-09-08。生成方式：Codex 逐单元翻译及 Markdown 结构转换。适用范围：上述文件版本的中文阅读辅助，不替代英文受控原文。

源文件 SHA-256：`47c8eddb368d48c8c2d10e935125b7277b56e915373d504c702bc10c9fd35995`。

**阅读约定：** 正文说明已译为中文；产品名、界面按钮、字段、命令、参数与目录名保留英文。截图保留原始英文画面，截图中的模型与示例路径不代表 SP 当前构型。原文中的明显拼写或表述差异在相应位置说明。原 Word 页码不适用于本 Markdown。

**操作边界：** 手册中的创建、覆盖、删除、清理、代码生成和测试命令均属于原文操作说明，不代表已经执行。实际操作前应核对配套工具版本、许可证、工程配置与产物目录。按当前 SP 工作区规则，未经明确授权，不要将练习或过程产物写入 `sp`。

## 目录

- [1 简介](#section-p011)
  - [1.1 产品说明](#section-p012)
  - [1.2 系统要求](#section-p019)
- [2 安装](#section-p042)
  - [2.1 软件安装](#section-p043)
  - [2.2 安装后的文件与文件夹](#section-p045)
  - [2.3 软件卸载](#section-p051)
- [3 入门](#section-p056)
  - [3.1 创建新的工具工程](#section-p057)
  - [3.2 创建新的设计工程](#section-p072)
  - [3.3 设计工程 MainProject 的目录结构](#section-p089)
  - [3.4 工具工程 ToolProject 的目录结构](#section-p122)
  - [3.5 设置标准模型配置](#section-p130)
  - [3.6 设置标准 Model Advisor 检查配置](#section-p151)
  - [3.7 添加新模型](#section-p167)
- [4 MBD 工作流管理器](#section-p189)
  - [4.1 启动 MBD Workflow Manager](#section-p190)
  - [4.2 模型与代码的交互式检查](#section-p200)
  - [4.3 静态验证 Static Verification](#section-p212)
  - [4.4 编写基于需求的测试 Req Based Tests](#section-p226)
  - [4.5 动态测试第一部分 基于需求的测试](#section-p320)
  - [4.6 生成基于结构的测试 Struct Based Tests](#section-p345)
  - [4.7 动态测试第二部分 基于结构的测试](#section-p356)
  - [4.8 质量跟踪 Quality Tracking](#section-p385)
- [5 MBD Workbench API 参考](#section-p447)


![图 1 封面 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image1.png)

*图 1 封面 原文界面*


<!-- source-unit: P011 -->
<a id="section-p011"></a>

## 1 简介

<!-- source-unit: P012 -->
<a id="section-p012"></a>

### 1.1 产品说明

<!-- source-unit: P013 -->
MBD Workbench 是一套模板，用于简化在高完整性软件开发中采用基于模型的设计（Model-Based Design，MBD）的过程。该模板用于创建新工程，集中保存与 DO-178C 应用相关的全部文档、需求、设计、代码、测试、结果和工具。工程预先提供以下内容：

<!-- source-unit: P014 -->
- 计划文档模板。

<!-- source-unit: P015 -->
- 推荐的建模规范和编码规范。

<!-- source-unit: P016 -->
- 以模型模板、模型配置和检查项形式体现的设计约束。

<!-- source-unit: P017 -->
- 用于自动执行关键开发和验证任务的工具。

<!-- source-unit: P018 -->
使用该模板创建新工程后，应添加软件需求、设计模型和基于需求的测试，并建立相应的追溯关系。然后使用工程提供的工具和实用程序执行必要的 MBD 任务，以满足适用的 DO 目标。有关如何将 MBD Workbench 应用于 DO-178C 基于模型的设计工作流，请参见示例工程（Example Project）。

<!-- source-unit: P019 -->
<a id="section-p019"></a>

### 1.2 系统要求

<!-- source-unit: P020 -->
- 支持的平台：

<!-- source-unit: P021 -->
- Windows

<!-- source-unit: P022 -->
- Linux

<!-- source-unit: P023 -->
- 最低 MATLAB 软件配置要求：

<!-- source-unit: P024 -->
- MATLAB®

<!-- source-unit: P025 -->
- Simulink®

<!-- source-unit: P026 -->
- MATLAB Report Generator™

<!-- source-unit: P027 -->
- Simulink Report Generator™

<!-- source-unit: P028 -->
- MATLAB Coder™

<!-- source-unit: P029 -->
- Simulink Coder™

<!-- source-unit: P030 -->
- Embedded Coder®

<!-- source-unit: P031 -->
- Requirements Toolbox™

<!-- source-unit: P032 -->
- Simulink Check™

<!-- source-unit: P033 -->
- Simulink Coverage™

<!-- source-unit: P034 -->
- Simulink Test™

<!-- source-unit: P035 -->
- Simulink Design Verifier™

<!-- source-unit: P036 -->
- Simulink Code Inspector™

<!-- source-unit: P037 -->
- Polyspace Bug Finder™

<!-- source-unit: P038 -->
- Polyspace Code Prover™

<!-- source-unit: P039 -->
- 外部软件要求：

<!-- source-unit: P040 -->
- 受支持的 C/C++ 编译器，参见 [MathWorks 支持的编译器列表](https://www.mathworks.com/support/compilers.html)。

<!-- source-unit: P042 -->
<a id="section-p042"></a>

## 2 安装

<!-- source-unit: P043 -->
<a id="section-p043"></a>

### 2.1 软件安装

<!-- source-unit: P044 -->
要安装 MBD Workbench，请运行应用安装包 `DO MBD Workbench R20XXx.mlappinstall`。安装程序会在 MATLAB 附加功能安装目录下创建 `DOMBDPlatformR20XXx` 文件夹。默认安装目录为 `C:\Users\username\AppData\Roaming\MathWorks\MATLAB Add-Ons\Apps`。

<!-- source-unit: P045 -->
<a id="section-p045"></a>

### 2.2 安装后的文件与文件夹

<!-- source-unit: P046 -->
使用 MBD Workbench 时，了解以下相关文件有助于开展工作：

<!-- source-unit: P047 -->
- MBD Workbench 提供名为 `MainProject_R20XXx.sltx` 的空白工程模板，用于简化按 DO-178C 开展基于模型的软件开发。开始新的开发工作时，使用该模板创建新的 Simulink 工程。

<!-- source-unit: P048 -->
- 为配合主工程，MBD Workbench 还提供名为 `ToolProject_R20XXx.sltx` 的工具工程模板（原文称 gadget project）。使用该模板创建工具工程，并与新的主工程配套使用；主工程将引用这个工具工程。如果交付的 MBD Workbench 不包含独立工具工程模板，则所需工具均已作为主工程的一部分一同提供。

<!-- source-unit: P049 -->
- MBD Workbench 还提供名为 `ExampleProject_R20XXx.sltx` 的示例模板。可使用该模板完整演示 MBD Workbench 及其整体工作流。

<!-- source-unit: P050 -->
大量复杂需求往往会形成复杂的软件架构。此时可以按分组和／或层级，将整体设计分解为多个设计工程。将工具工程与主工程分离后，各设计工程可以通过引用同一个工具工程复用工具，避免重复配置。此外，工具与设计数据通常具有不同的生命周期；这种分离方式使工具能够集中、独立地维护。

<!-- source-unit: P051 -->
<a id="section-p051"></a>

### 2.3 软件卸载

<!-- source-unit: P052 -->
要卸载 MBD Workbench，请执行以下操作：

<!-- source-unit: P053 -->
1. 在 MATLAB 桌面打开 `Apps` 选项卡。

<!-- source-unit: P054 -->
2. 在 `My Apps` 区域中右键单击 `DO MBD Workbench R20XXx`，然后选择 `Uninstall`。

<!-- source-unit: P056 -->
<a id="section-p056"></a>

## 3 入门

<!-- source-unit: P057 -->
<a id="section-p057"></a>

### 3.1 创建新的工具工程

<!-- source-unit: P058 -->
MBD Workbench 以工程模板的形式提供一组工具，用于采用 MBD 方法开展面向 DO-178C 的软件开发与验证。启动新项目时，可以基于该模板创建工具工程，以使用这些工具。操作如下：

<!-- source-unit: P059 -->
1. 在 MATLAB 桌面打开 `Apps` 选项卡。

<!-- source-unit: P060 -->
2. 在 `My Apps` 区域选择 `DO MBD Workbench R20XXx`，加载该应用。

<!-- source-unit: P061 -->
3. 出现下图所示对话框时，单击 `OK` 关闭对话框。


![图 2 创建新的工具工程 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image2.png)

*图 2 创建新的工具工程 原文界面*


<!-- source-unit: P063 -->
4. 在 MATLAB 的 `Home` 选项卡中单击 `Simulink`，打开 `Simulink Start Page`。

<!-- source-unit: P064 -->
5. 在 Simulink 起始页中展开 `My Templates`（如有必要），找到 `ToolProject`。如果所交付的 MBD Workbench 不包含工具工程模板，请跳过本节后续步骤，转到下一节。

<!-- source-unit: P065 -->
6. 将鼠标移至 `My Templates` 面板中的 `ToolProject` 上，然后单击 `Create Project`，打开创建工程对话框。


![图 3 创建新的工具工程 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image3.png)

*图 3 创建新的工具工程 原文界面*


<!-- source-unit: P067 -->
7. 在 `Create Project` 对话框中填写 `Name` 和 `Folder`，然后单击 `OK`，基于 `ToolProject` 模板创建新工程。设置 `Folder` 时，应将工程根目录放在一个新建的空父目录下。下图示例中，工具工程根目录位于新建空目录 `C:\Users\rsnah\MATLAB\Projects` 下。


![图 4 创建新的工具工程 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image4.png)

*图 4 创建新的工具工程 原文界面*


<!-- source-unit: P069 -->
8. 工程创建完成后，Simulink 会自动打开新的工具工程。如果出现 `Set Up Project` 对话框，请将其关闭。

<!-- source-unit: P070 -->
9. 关闭工程。

<!-- source-unit: P071 -->
至此，已成功创建工具工程。与该软件相关的各设计工程均需要使用它。

<!-- source-unit: P072 -->
<a id="section-p072"></a>

### 3.2 创建新的设计工程

<!-- source-unit: P073 -->
MBD Workbench 提供一个模板，用于管理使用 MathWorks 工具创建的、适用的 ISO 26262 工程制品。启动新项目时，可以基于此模板创建设计工程，以利用其中预设的工程结构。操作如下。

**译注：** 原文此处写作 ISO 26262，与本手册标题及前后文的 DO-178C 表述不一致。本译文保留原文，不据此认定 SP 项目适用 ISO 26262。

<!-- source-unit: P074 -->
1. 确认已加载 `DO MBD Workbench R20XXx` 应用；若未加载，请先加载。

<!-- source-unit: P075 -->
2. 在 MATLAB 的 `Home` 选项卡中单击 `Simulink`，打开 `Simulink Start Page`。

<!-- source-unit: P076 -->
3. 在 Simulink 起始页中展开 `My Templates`（如有必要），找到 `MainProject`。

<!-- source-unit: P077 -->
4. 将鼠标移至 `My Templates` 面板中的 `MainProject` 上，然后单击 `Create Project`，打开创建工程对话框。


![图 5 创建新的设计工程 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image5.png)

*图 5 创建新的设计工程 原文界面*


<!-- source-unit: P079 -->
5. 在 `Create Project` 对话框中填写 `Name` 和 `Folder`，然后单击 `OK`，基于 `MainProject` 模板创建新工程。设置 `Folder` 时，应使新工程根目录与此前创建的工具工程根目录具有同一个父目录。下图示例中，设计工程根目录位于 `C:\Users\rsnah\MATLAB\Projects` 下，该目录也包含工具工程根目录。如果所交付的 MBD Workbench 不包含工具工程模板，只需将设计工程根目录放在一个新建的空父目录下。


![图 6 创建新的设计工程 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image6.png)

*图 6 创建新的设计工程 原文界面*


<!-- source-unit: P081 -->
6. 工程创建完成后，Simulink 会自动打开新的设计工程。如果出现 `Set Up Project` 对话框，请将其关闭。如果交付包不包含工具工程模板，请跳过本节后续步骤。

<!-- source-unit: P082 -->
7. 在已打开的工程中选择 `PROJECT > ENVIRONMENT > References`，打开 `Add Reference` 对话框。


![图 7 创建新的设计工程 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image7.png)

*图 7 创建新的设计工程 原文界面*


<!-- source-unit: P084 -->
8. 浏览并找到此前创建的工具工程，然后单击 `Add`。


![图 8 创建新的设计工程 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image8.png)

*图 8 创建新的设计工程 原文界面*


<!-- source-unit: P086 -->
9. 这样即可将工具工程添加为设计工程的引用工程。


![图 9 创建新的设计工程 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image9.png)

*图 9 创建新的设计工程 原文界面*


<!-- source-unit: P088 -->
至此，已成功创建设计工程，可用它辅助采用 MBD 方法开发软件。

<!-- source-unit: P089 -->
<a id="section-p089"></a>

### 3.3 设计工程 MainProject 的目录结构

<!-- source-unit: P090 -->
设计工程创建完成后，Simulink 会自动打开它。新工程包含以下文件夹：

<!-- source-unit: P091 -->
<a id="section-p091"></a>

#### `01_Planning`

<!-- source-unit: P092 -->
- `checklists` 子目录提供检查单，用于评审需求、模型、代码、测试用例、测试结果及追溯数据。

<!-- source-unit: P093 -->
- `plans` 子目录提供软件合格审定计划（Plan for Software Aspects of Certification，PSAC）、软件开发计划（SDP）、软件验证计划（SVP）、软件配置管理计划（SCMP）和软件质量保证计划（SQAP）的模板。

<!-- source-unit: P094 -->
- `standards` 子目录提供软件需求标准（SRS）、软件设计标准（SDS）、软件代码标准（SCS）和软件模型标准（SMS）的模板。

<!-- source-unit: P095 -->
<a id="section-p095"></a>

#### `02_Requirements`

<!-- source-unit: P096 -->
- `specification` 子目录用于保存描述软件高层需求及派生需求的全部文档。

<!-- source-unit: P097 -->
- `verification_results` 子目录用于保存软件高层需求及派生需求全部评审活动对应的检查单。

<!-- source-unit: P098 -->
<a id="section-p098"></a>

#### `03_Architecture`

<!-- source-unit: P099 -->
- `analysis` 子目录用于保存控制流分析、数据流分析以及调度分析（如执行）的全部报告。

<!-- source-unit: P100 -->
- `specification` 子目录用于保存描述软件架构设计的全部文档与数据。

<!-- source-unit: P101 -->
- `verification_results` 子目录用于保存软件架构设计全部评审活动对应的检查单。

<!-- source-unit: P102 -->
<a id="section-p102"></a>

#### `04_Design`

<!-- source-unit: P103 -->
- `common` 子目录用于创建工程内多个模型共享的模块库与数据。工程已提供一个模块库，其中包含与 Simulink Code Inspector 兼容的所有基本模块。

<!-- source-unit: P104 -->
- `sample_model` 子目录是模型目录模板，用于按模型名称创建实际模型目录。创建模型目录后，在其 `specification` 子目录中创建模型及数据，在 `test_cases` 子目录中开发用于依据软件需求验证模型的测试用例，并在 `verification_results` 子目录中保存模型的全部验证结果。

<!-- source-unit: P105 -->
<a id="section-p105"></a>

#### `05_Code`

<!-- source-unit: P106 -->
- `specification` 子目录用于保存由工程内模型生成的全部源代码与可执行目标代码。

<!-- source-unit: P107 -->
- `verification_results` 子目录用于保存源代码与可执行目标代码的全部验证结果。

<!-- source-unit: P108 -->
<a id="section-p108"></a>

#### `06_ToolQualification`

<!-- source-unit: P109 -->
该目录提供工具鉴定生命周期数据的预留位置。

<!-- source-unit: P110 -->
<a id="section-p110"></a>

#### `continuous_integration`

<!-- source-unit: P111 -->
- `help` 子目录提供持续集成框架的用户指南。

<!-- source-unit: P112 -->
- `job` 子目录提供一个预定义的构建任务，规定可由工作台自动执行的一系列 MBD 任务。将该构建任务接入持续集成流水线后，可以规范对工程变更进行例行验证的方式。

<!-- source-unit: P113 -->
- `reports` 子目录用于保存构建任务生成的全部汇总报告。

<!-- source-unit: P114 -->
<a id="section-p114"></a>

#### `dashboard`

<!-- source-unit: P115 -->
- 该目录提供 Web 服务器和 Web 应用，用于通过仪表盘监控各设计模型的质量以及整个工程的状态。

<!-- source-unit: P116 -->
<a id="section-p116"></a>

#### `project_configuration`

<!-- source-unit: P117 -->
- `checks` 子目录提供 Model Advisor 检查和 Polyspace 编码规范检查的配置文件，分别与工程中的软件模型标准、软件代码标准配套使用。

<!-- source-unit: P118 -->
- `templates` 子目录提供与软件设计约束及建模指南兼容的模型配置。

<!-- source-unit: P119 -->
- 该目录本身还提供工程级配置，例如目录结构、命名约定和度量指标阈值。

<!-- source-unit: P120 -->
<a id="section-p120"></a>

#### `work`

<!-- source-unit: P121 -->
默认工作目录。该目录还包含 `cache` 文件夹，用于保存仿真所需的模型构建产物。

<!-- source-unit: P122 -->
<a id="section-p122"></a>

### 3.4 工具工程 ToolProject 的目录结构

<!-- source-unit: P123 -->
每个设计工程都需要配套工具工程，才能具备完整运行能力。被引用的工具工程包含以下文件夹：

<!-- source-unit: P124 -->
<a id="section-p124"></a>

#### `help`

<!-- source-unit: P125 -->
该目录提供工作台文档。

<!-- source-unit: P126 -->
<a id="section-p126"></a>

#### `tools`

<!-- source-unit: P127 -->
- `apps` 子目录提供图形用户界面的布局及回调函数，用于简化用户操作。

<!-- source-unit: P128 -->
- `icons` 子目录提供用于界面图形显示的数字图像。

<!-- source-unit: P129 -->
- `utilities` 子目录提供工程管理、需求管理、模型查询、代码生成、模型／代码验证、报告生成以及仪表盘显示的方法。如果工程定义了自定义 Model Advisor 检查项，也可在该子目录中找到它们。

<!-- source-unit: P130 -->
<a id="section-p130"></a>

### 3.5 设置标准模型配置

<!-- source-unit: P131 -->
从模板创建的新工程在 `04_Design\common\specification\configurations` 中提供 `csSingleInstance.sldd`、`csMultiInstance.sldd` 和 `csRateIndependent.sldd`。每个标准数据字典都保存一套模型配置，供工程内开发的模型使用。可复用（可多实例化）模型使用 `csMultiInstance.sldd` 中的配置；需要在不同速率下复用的模型使用 `csRateIndependent.sldd` 中的配置；其他模型使用 `csSingleInstance.sldd` 中的配置。这些配置分别由 `nonreusableModelConfig.m`、`reusableModelConfig.m` 和 `rateIndependentModelConfig.m` 创建，其标准设置来自 `do178Config.m`，而 `do178Config.m` 的标准设置又由模型 `baseModelConfiguration.slx` 导出。

<!-- source-unit: P132 -->
必须重新配置这些模型配置，使 `Hardware Implementation` 设置与实际应用的硬件一致。操作如下：

<!-- source-unit: P133 -->
1. 输入 `baseModelConfiguration`，打开该模型。

<!-- source-unit: P134 -->
2. 选择 `MODELING > SETUP > Model Settings`，打开 `Configuration Parameters` 对话框。


![图 10 设置标准模型配置 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image10.png)

*图 10 设置标准模型配置 原文界面*


<!-- source-unit: P136 -->
3. 在 `Configuration Parameters` 对话框中单击 `Hardware Implementation`，打开硬件实现设置面板。


![图 11 设置标准模型配置 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image11.png)

*图 11 设置标准模型配置 原文界面*


<!-- source-unit: P138 -->
4. 根据硬件规格设置 `Device vendor` 和 `Device type`，然后单击 `OK`。


![图 12 设置标准模型配置 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image12.png)

*图 12 设置标准模型配置 原文界面*


<!-- source-unit: P140 -->
5. 保存模型。

<!-- source-unit: P141 -->
6. 使用以下命令，从模型获取新的 `Simulink.ConfigSet` 对象：

<!-- source-unit: P142 -->
```matlab
configSet = getConfigSet('baseModelConfiguration', 'Configuration');
```

<!-- source-unit: P143 -->
7. 然后使用以下命令，将新的 `Simulink.ConfigSet` 对象导出到 `do178Config.m`，覆盖文件中已有的设置：

<!-- source-unit: P144 -->
```matlab
configSet.saveAs('..\project_configuration\templates\model_configurations\do178Config');
```

<!-- source-unit: P145 -->
8. 使用以下命令更新数据字典，其中保存的是非复用（不可多实例化）模型的配置：

<!-- source-unit: P146 -->
```matlab
dd = Simulink.data.dictionary.open('csSingleInstance.sldd');
cfg = dd.getSection('Configurations');
cfg.importFromFile('nonreusableModelConfig.m', 'existingVarsAction', 'overwrite');
dd.saveChanges();
```

<!-- source-unit: P147 -->
9. 使用以下命令更新数据字典，其中保存的是可复用（可多实例化）模型的配置：

<!-- source-unit: P148 -->
```matlab
dd = Simulink.data.dictionary.open('csMultiInstance.sldd');
cfg = dd.getSection('Configurations');
cfg.importFromFile('reusableModelConfig.m', 'existingVarsAction', 'overwrite');
dd.saveChanges();
```

<!-- source-unit: P149 -->
10. 使用以下命令更新数据字典，其中保存的是速率无关模型的配置：

<!-- source-unit: P150 -->
```matlab
dd = Simulink.data.dictionary.open('csRateIndependent.sldd');
cfg = dd.getSection('Configurations');
cfg.importFromFile('rateIndependentModelConfig.m', 'existingVarsAction', 'overwrite');
dd.saveChanges();
```

<!-- source-unit: P151 -->
<a id="section-p151"></a>

### 3.6 设置标准 Model Advisor 检查配置

<!-- source-unit: P152 -->
从模板创建的新工程在 `project_configuration\checks` 中提供 `do178Checks.json`。该文件包含与建议建模指南配套的初始 Model Advisor 配置。借助此配置，可方便地启用需要由 Model Advisor 执行的适用检查项。

<!-- source-unit: P153 -->
如果决定调整建模标准，也必须相应调整 Model Advisor 配置。操作如下：

<!-- source-unit: P154 -->
1. 使用以下命令打开 Model Advisor 配置编辑器：

<!-- source-unit: P155 -->
```matlab
Simulink.ModelAdvisor.openConfigUI
```

<!-- source-unit: P156 -->
2. 在 Model Advisor 配置编辑器中选择 `Open`。


![图 13 设置标准 Model Advisor 检查配置 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image13.png)

*图 13 设置标准 Model Advisor 检查配置 原文界面*


<!-- source-unit: P158 -->
3. 在 `Open` 对话框中浏览到 `project_configuration\checks`，选择 `do178Checks.json`，然后单击 `Open`。

<!-- source-unit: P159 -->
4. 在 Model Advisor 配置编辑器中选择 `Show Library`，显示 `Library` 面板。


![图 14 设置标准 Model Advisor 检查配置 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image14.png)

*图 14 设置标准 Model Advisor 检查配置 原文界面*


<!-- source-unit: P161 -->
5. 要向当前配置添加一个检查项或一组检查项，请在 `Library` 面板中找到相应内容，然后将其复制到 `Configuration` 面板。


![图 15 设置标准 Model Advisor 检查配置 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image15.png)

*图 15 设置标准 Model Advisor 检查配置 原文界面*


<!-- source-unit: P163 -->
6. 要从当前配置中移除一个检查项或一组检查项，请在 `Configuration` 面板中找到相应内容，然后删除。


![图 16 设置标准 Model Advisor 检查配置 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image16.png)

*图 16 设置标准 Model Advisor 检查配置 原文界面*


<!-- source-unit: P165 -->
7. 完成必要调整后，选择 `Save`，保存对配置所做的全部更改。


![图 17 设置标准 Model Advisor 检查配置 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image17.png)

*图 17 设置标准 Model Advisor 检查配置 原文界面*


<!-- source-unit: P167 -->
<a id="section-p167"></a>

### 3.7 添加新模型

<!-- source-unit: P168 -->
应根据软件高层需求创建模型，并以模型作为软件低层需求。本工具包要求每个模型在 `04_Design` 下都有独立的模型目录，其结构应与 `sample_model` 相同。

<!-- source-unit: P169 -->
创建新模型前，应先使用 `addModelFolder` 函数，按模型名称创建模型目录。例如，要为名为 `controller` 的新模型创建目录，请使用以下命令之一：

<!-- source-unit: P170 -->
```matlab
addModelFolder('controller', 'Nonreusable')
```

<!-- source-unit: P171 -->
```matlab
addModelFolder('controller', 'Reusable')
```

<!-- source-unit: P172 -->
```matlab
addModelFolder('controller', 'Rate Independent')
```

<!-- source-unit: P173 -->
该命令会在 `04_Design` 下创建新的 `controller` 文件夹。与 `sample_model` 一样，新目录包含 `specification`、`test_cases` 和 `verification_results` 子目录。`specification` 中提供空模型 `controller.slx`，`specification\data` 中提供配套数据字典 `DD_controller.sldd`。该字典会自动关联到模型，并引用 `csSingleInstance.sldd`、`csMultiInstance.sldd` 或 `csRateIndependent.sldd` 中的一个。如果创建的是可复用或速率无关模型，`specification\data` 中还会生成 `localDD_controller.m`。此时即可使用 `do178Lib` 库允许的模块搭建模型。在 Simulink Library Browser 的 `DO-178C/DO-331 Primitive Library` 下，可以找到 `do178Lib` 的全部模块。

<!-- source-unit: P174 -->
开发模型时，通常需要定义数据来描述模型行为。应注意，所需数据可能仅供该模型使用，也可能在多个模型之间共享。仅由该模型使用的数据集合称为模型专用数据字典；共享数据则构成公共数据字典。

<!-- source-unit: P175 -->
工程内开发的每个模型都关联一个配套 Simulink 数据字典。该字典位于模型目录的 `specification\data` 子目录中，以模型名称加前缀 `DD_` 命名。对于非复用模型：

<!-- source-unit: P176 -->
- 配套数据字典必须引用 `csSingleInstance.sldd`，其中保存由 `nonreusableModelConfig.m` 派生的模型配置。

<!-- source-unit: P177 -->
- 使用配套数据字典保存模型专用数据。

<!-- source-unit: P178 -->
对于可复用模型或速率无关模型：

<!-- source-unit: P179 -->
- 配套数据字典必须引用 `csMultiInstance.sldd` 或 `csRateIndependent.sldd`，两者分别保存由 `reusableModelConfig.m` 或 `rateIndependentModelConfig.m` 派生的模型配置。

<!-- source-unit: P180 -->
- 使用配套数据字典保存模型专用、但不随实例变化的数据。

<!-- source-unit: P181 -->
- 此外，可能还需要在模型工作区中创建实例专用数据，包括模型参数（model arguments）。

<!-- source-unit: P182 -->
使用 MATLAB 脚本保存实例专用数据。该脚本位于模型目录的 `specification\data` 子目录中，以模型名称加前缀 `localDD_` 命名。

<!-- source-unit: P183 -->
将模型配置为自动运行该脚本，把实例专用数据加载到模型工作区。

<!-- source-unit: P184 -->
以上述示例为例，应在 `04_Design\controller\specification\data` 中创建 `DD_controller.sldd`，如果模型可复用，还应创建 `localDD_controller.m`。

<!-- source-unit: P185 -->
对于共享数据，应在 `04_Design\common\specification\data` 中创建 Simulink 数据字典。使用这些数据的每个模型，其配套数据字典都必须引用相应的共享字典。

<!-- source-unit: P186 -->
为配合模型使用，可能需要创建别名、总线和枚举等用户定义数据类型。对于 `Simulink.AliasType` 和 `Simulink.Bus` 对象，应在 `04_Design\common\specification\data_types` 中创建 Simulink 数据字典，再按需引用这些字典。对于枚举类，直接在该目录中创建相应的类定义文件即可。

<!-- source-unit: P187 -->
开发模型及其数据字典时，可以启动 MBD Workflow Manager，辅助开展后续验证活动，以保障模型及其代码的符合性。

<!-- source-unit: P189 -->
<a id="section-p189"></a>

## 4 MBD 工作流管理器

<!-- source-unit: P190 -->
<a id="section-p190"></a>

### 4.1 启动 MBD Workflow Manager

<!-- source-unit: P191 -->
MBD Workbench 配有名为 MBD Workflow Manager 的应用。借助该应用，可以直观地对每个设计模型及其生成代码开展必要的开发与验证活动。

<!-- source-unit: P192 -->
应用提供的 MBD 活动通常可分为以下几类：

<!-- source-unit: P193 -->
1. 检查模型与生成代码的符合性：确认正在开发的模型及其生成代码符合标准且不存在缺陷。在模型开发阶段，使用相应工具以交互方式开展这些活动。

<!-- source-unit: P194 -->
2. 执行静态验证并生成报告：证明各模型及其生成代码符合标准且不存在缺陷。在模型开发完成后开展这些活动，为模型及代码生成正式分析报告。

<!-- source-unit: P195 -->
3. 编写用于验证模型与代码的测试：构建测试，用模型所实现的需求验证开发中的模型。在模型开发接近结束时开展这些活动，为模型及代码创建测试用例。测试用例可以基于需求，也可以基于结构：基于需求的测试由功能需求人工推导；基于结构的测试根据设计模型的覆盖目标自动生成。

<!-- source-unit: P196 -->
4. 执行动态测试并生成报告：证明各模型及其生成代码符合需求，并达到充分的结构覆盖。在模型开发与测试编写完成后开展这些活动，为模型及代码生成正式测试报告。

<!-- source-unit: P197 -->
5. 汇总结果以跟踪质量：收集上述第 2 类和第 4 类活动的结果，跟踪各模型及其生成代码的整体符合性。在模型验证完成后开展这些活动，通过仪表盘评估模型与代码的质量。

<!-- source-unit: P198 -->
要打开 MBD Workflow Manager，只需执行命令 `mbdWorkflowTool`。下图展示了上述工作流与 MBD Workflow Manager 界面的对应关系。


![图 18 启动 MBD Workflow Manager 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image18.png)

*图 18 启动 MBD Workflow Manager 原文界面*


<!-- source-unit: P200 -->
<a id="section-p200"></a>

### 4.2 模型与代码的交互式检查

<!-- source-unit: P201 -->
MBD Workflow Manager 面向已添加到工程中的设计模型开展操作，并会自动识别工程内的全部设计模型。


![图 19 模型与代码的交互式检查 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image19.png)

*图 19 模型与代码的交互式检查 原文界面*


<!-- source-unit: P203 -->
<a id="section-p203"></a>

#### 模型树选择器

<!-- source-unit: P204 -->
首次打开应用时，工程内找到的全部设计模型会按字母顺序显示在树形选择器中。如果在应用打开后添加新模型，必须单击选择器底部的 `Model List` 刷新列表，应用才能识别新模型。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image20.png)

<!-- source-unit: P205 -->
选中模型后，该模型会显示为当前系统（`Current System`），应用的操作范围随之限定为该系统。除非另有指定，所有操作均将当前模型视为引用模型。若要将其按顶层模型处理，请勾选中间面板顶部的 `Top Model`。

<!-- source-unit: P206 -->
注意：在选择系统之前，应用中的所有按钮均不可操作。

<!-- source-unit: P207 -->
<a id="section-p207"></a>

#### 快捷工具按钮

<!-- source-unit: P208 -->
选中系统后，应用菜单栏下方会显示更多快捷工具按钮。可以使用这些按钮打开模型及其数据字典，也可以交互式分析模型及其生成代码。

<!-- source-unit: P209 -->
开发模型时，可将这些快捷工具作为设计辅助工具，检查模型及生成代码，发现违反标准的情况和设计中非预期的缺陷。

<!-- source-table: T00 -->

| 图标或项目 | 说明 |
| --- | --- |
| ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image21.png) | 打开所选模型。 |
| ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image22.png) | 打开所选模型的数据字典。 |
| ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image23.png) | 编译所选模型，检查静态错误。 |
| ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image24.png) | 启动 Model Advisor，查找所选模型违反模型规范的情况。 |
| ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image26.png) | 启动 Model Advisor，收集所选模型的度量指标。 |
| ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image27.png) | 启动 Design Verifier，检测所选模型中的设计错误。 |
| ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image28.png) | 启动 Bug Finder，查找所选模型生成代码中的 MISRA 违规项。 |
| ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image29.png) | 启动 Bug Finder，收集所选模型生成代码的度量指标。 |
| ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image30.png) | 启动 Bug Finder，查找所选模型生成代码中的运行时错误。 |
| ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image31.png) | 启动 Code Prover，识别所选模型生成代码中的缺陷。 |

<!-- source-unit: P210 -->
注意：启动 Bug Finder 或 Code Prover 检查代码之前，必须先从所选模型生成代码。为此，请单击 `Static Verification` 选项卡中的 `Generate Code`。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image32.png)

<!-- source-unit: P211 -->
请注意，通过这些快捷工具进行的分析并非正式验证；产生的结果只是存放在 `work` 文件夹下的临时结果。

<!-- source-unit: P212 -->
<a id="section-p212"></a>

### 4.3 静态验证 Static Verification

<!-- source-unit: P213 -->
前述快捷工具允许模型开发人员交互式检查模型及生成代码的符合性。模型开发完成后，仍必须正式执行全部静态分析活动。要为模型与代码的静态分析生成正式报告，请使用 Workflow Manager 的 `Static Verification` 选项卡。


![图 20 静态验证 Static Verification 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image33.png)

*图 20 静态验证 Static Verification 原文界面*


<!-- source-unit: P215 -->
`Static Verification` 选项卡主要用于根据模型与代码分析生成正式验证报告。该选项卡中的任务均自动执行。由于不能在此自由控制分析规则的启用和禁用，因此不要把它当作模型开发阶段的交互式设计辅助工具。

<!-- source-unit: P216 -->
<a id="section-p216"></a>

#### 模型验证 Model Verification

<!-- source-unit: P217 -->
在 `Static Verification` 选项卡的 `Model Verification` 面板中，可以检查所选系统的符合性。

<!-- source-table: T01 -->

| 项目 | 说明 |
| --- | --- |
| 模型规范（`Model Standards`） ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image34.png) | 查找所选模型违反模型规范的情况。 |
| 模型指标（`Model Metrics`） ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image35.png) | 收集所选模型的度量指标。 |
| 设计错误（`Design Errors`） ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image36.png) | 检测所选模型中的设计错误。 |

<!-- source-unit: P218 -->
执行设计错误检测时，如有需要，可通过 `Max Process Time` 设置允许的最长分析时间。如果执行耗时过长，也可以取消勾选 `Detect Active Logic`，以停止进行穷尽式死逻辑分析，但不推荐这样做。

<!-- source-unit: P219 -->
默认情况下，`Model Advisor Report`、`Model Metrics Report` 和 `DED Report` 按钮不可用。只有在工程中找到对应报告后，这些按钮才会启用。需要时，可用这些按钮打开生成的报告。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image37.png)

<!-- source-unit: P220 -->
<a id="section-p220"></a>

#### 代码检查 Code Inspection

<!-- source-unit: P221 -->
在 `Static Verification` 选项卡的 `Model Verification` 面板下方，可以从所选系统生成代码，随后检查生成代码的可追溯性与等价性。

<!-- source-table: T02 -->

| 项目 | 说明 |
| --- | --- |
| 生成代码（`Generate Code`） ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image38.png) | 从所选模型生成代码。 |
| 检查代码（`Inspect Code`） ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image39.png) | 检查所选模型的生成代码。 |

<!-- source-unit: P222 -->
默认情况下，`Code Gen Report` 和 `Code Inspection Report` 按钮不可用。只有在工程中找到对应报告后，这些按钮才会启用。需要时，可用这些按钮打开生成的报告。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image37.png)

<!-- source-unit: P223 -->
<a id="section-p223"></a>

#### 代码验证 Code Verification

<!-- source-unit: P224 -->
在 `Static Verification` 选项卡的 `Code Verification` 面板中，可以检查所选系统生成代码的符合性。

<!-- source-table: T03 -->

| 项目 | 说明 |
| --- | --- |
| 代码规范（`Code Standards`） ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image40.png) | 查找所选模型生成代码中的 MISRA 违规项。 |
| 代码指标（`Code Metrics`） ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image41.png) | 收集所选模型生成代码的度量指标。 |
| Bug Finder ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image42.png) | 查找所选模型生成代码中的运行时错误。 |
| Code Prover ![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image43.png) | 证明所选模型的生成代码不存在缺陷。 |

<!-- source-unit: P225 -->
默认情况下，`Code Standards Report`、`Code Metrics Report`、`Bug Finder Report` 和 `Code Prover Report` 按钮不可用。只有在工程中找到对应报告后，这些按钮才会启用。需要时，可用这些按钮打开生成的报告。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image37.png)

<!-- source-unit: P226 -->
<a id="section-p226"></a>

### 4.4 编写基于需求的测试 Req Based Tests

<!-- source-unit: P227 -->
要有效测试系统，必须创建测试用例，根据系统所实现的需求验证其行为。执行由需求人工推导出的测试用例，称为基于需求的测试。要编写这类用例，请使用 Workflow Manager 的 `Req-Based Tests` 选项卡。


![图 21 编写基于需求的测试 Req Based Tests 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image44.png)

*图 21 编写基于需求的测试 Req Based Tests 原文界面*


<!-- source-unit: P229 -->
该选项卡包含两个主要面板：使用 `Test Case Authoring` 创建新测试用例，再使用 `Test Execution` 执行它们。

<!-- source-unit: P230 -->
<a id="section-p230"></a>

#### 测试用例编写

<!-- source-unit: P231 -->
选择 `Req-Based Tests` 选项卡时，与当前系统相关的全部测试用例会自动以表格形式列在 `Test Case Authoring` 面板中。如果尚无测试用例，则显示空表。要创建新测试用例，请执行以下操作：

<!-- source-unit: P232 -->
1. 单击 `Add Test Case`。系统会为当前系统生成测试台（test harness），并在表格中新增一行。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image45.png)


![图 22 测试用例编写 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image46.png)

*图 22 测试用例编写 原文界面*


<!-- source-unit: P234 -->
2. 在表格中双击新行的 `Name` 单元格，为新测试用例命名。


![图 23 测试用例编写 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image47.png)

*图 23 测试用例编写 原文界面*


<!-- source-unit: P236 -->
3. 在 `Name` 对话框中输入新测试用例的名称，然后单击 `OK`。注意：测试用例名称必须是有效标识符。


![图 24 测试用例编写 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image48.png)

*图 24 测试用例编写 原文界面*


<!-- source-unit: P238 -->
4. 可以在 `Description` 单元格中填写测试用例的说明，此项可选。


![图 25 测试用例编写 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image49.png)

*图 25 测试用例编写 原文界面*


<!-- source-unit: P240 -->
5. 双击 `SigSrc` 单元格，选择新测试用例的测试向量和通过／失败判据的输入方式。


![图 26 测试用例编写 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image50.png)

*图 26 测试用例编写 原文界面*


<!-- source-unit: P242 -->
6. 在 `Select Test Case Mode` 对话框中选择 `TS` 或 `SE`，分别使用 Test Sequence 或 Signal Editor 指定测试数据。


![图 27 测试用例编写 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image51.png)

*图 27 测试用例编写 原文界面*


<!-- source-unit: P244 -->
7. 输入测试数据，包括测试输入和预期输出。

<!-- source-unit: P245 -->
<a id="section-p245"></a>

##### 使用 Test Sequence

<!-- source-unit: P246 -->
如果选择 `TS`，即使用 Test Sequence 输入测试向量及通过／失败判据，请执行以下操作：

<!-- source-unit: P247 -->
1. 右键单击 `SigSrc` 单元格，选择 `Open Test Sequence`。这会同时打开 Test Sequence 测试台中 `Test Sequence` 模块和 `Test Assessment` 模块的 Test Sequence Editor。


![图 28 使用 Test Sequence 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image52.png)

*图 28 使用 Test Sequence 原文界面*


<!-- source-unit: P249 -->
2. 先使用 `Test Sequence` 模块的 Test Sequence Editor，输入各步骤的输入数据，以及从当前步骤转移到下一步骤的条件。


![图 29 使用 Test Sequence 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image53.png)

*图 29 使用 Test Sequence 原文界面*


<!-- source-unit: P251 -->
3. 单击 Test Sequence Editor 中的保存按钮，保存对 `Test Sequence` 模块所做的更改。保存后可以关闭该编辑器。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image54.png)

<!-- source-unit: P252 -->
4. 然后使用 `Test Assessment` 模块的 Test Sequence Editor，验证每个步骤的输出数据及其逻辑转移条件。


![图 30 使用 Test Sequence 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image56.png)

*图 30 使用 Test Sequence 原文界面*


<!-- source-unit: P254 -->
5. 同样，单击 Test Sequence Editor 中的保存按钮，保存对 `Test Assessment` 模块所做的更改。保存后也可以关闭该编辑器。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image54.png)

<!-- source-unit: P255 -->
6. 现在可以关闭 Test Sequence 测试台。

<!-- source-unit: P256 -->
<a id="section-p256"></a>

##### 使用 Signal Editor

<!-- source-unit: P257 -->
如果选择 `SE`，即使用 Signal Editor 定义测试向量和基线判据，请执行以下操作：

<!-- source-unit: P258 -->
1. 右键单击 `SigSrc` 单元格，选择 `Open Signal Editor`，打开信号编辑器。注意：此处打开的不是 Signal Editor 测试台中 `Signal Editor` 模块自带的编辑器；这个编辑器允许同时定义被测系统的输入数据和输出数据。


![图 31 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image57.png)

*图 31 使用 Signal Editor 原文界面*


<!-- source-unit: P260 -->
2. 使用该 Signal Editor 指定测试输入和预期输出的波形。必须分别展开输入和输出 `Dataset` 对象，才能编辑其值。


![图 32 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image58.png)

*图 32 使用 Signal Editor 原文界面*


<!-- source-unit: P262 -->
3. 完全展开 `Dataset` 对象后，逐一选择信号，编辑其 `timeseries` 数据。


![图 33 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image59.png)

*图 33 使用 Signal Editor 原文界面*


<!-- source-unit: P264 -->
4. 既可以直接在图中编辑波形，也可以在 Signal Editor 的表格中输入数据。

<!-- source-unit: P265 -->
5. 在 Signal Editor 中指定全部信号的值后，单击 `SIGNAL EDITOR > Save and Sync > Save`，将全部更改保存并同步到 Signal Editor 测试台。


![图 34 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image60.png)

*图 34 使用 Signal Editor 原文界面*


<!-- source-unit: P267 -->
6. 保存后可以关闭 Signal Editor。

<!-- source-unit: P268 -->
8. 如有需要，输入标定数据。系统参数会自动采用数据字典中原先定义的值。如果测试过程要求修改一个或多个系统参数，必须使用非默认标定集覆盖这些值。操作如下：

<!-- source-unit: P269 -->
1. 在测试用例表中，双击新测试用例的 `CalSet` 单元格。


![图 35 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image61.png)

*图 35 使用 Signal Editor 原文界面*


<!-- source-unit: P271 -->
2. 输入要用于覆盖系统参数值的标定集名称。可以指定新标定集的名称，也可以指定已在其他测试用例中创建、准备复用的标定集名称。


![图 36 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image62.png)

*图 36 使用 Signal Editor 原文界面*


<!-- source-unit: P273 -->
3. 如果这是新标定集，请右键单击 `CalSet` 单元格，选择 `Open Calibration Set`。标定集会以表格形式显示在测试用例表下方。如果系统没有参数，消息框会显示错误。


![图 37 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image63.png)

*图 37 使用 Signal Editor 原文界面*


<!-- source-unit: P275 -->
4. 在标定集表中检查每个参数的原始值。注意：只有可调参数的值能够在该表中编辑。


![图 38 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image64.png)

*图 38 使用 Signal Editor 原文界面*


<!-- source-unit: P277 -->
5. 要修改可调参数的值，请在标定集表中双击该参数的 `Value` 单元格。


![图 39 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image65.png)

*图 39 使用 Signal Editor 原文界面*


<!-- source-unit: P279 -->
6. 在参数属性对话框中，将 `Value` 设置为新值，然后单击 `OK`。


![图 40 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image66.png)

*图 40 使用 Signal Editor 原文界面*


<!-- source-unit: P281 -->
7. 完成全部必要修改后，单击 `Save & Close`，将新的参数值保存到标定集中。


![图 41 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image67.png)

*图 41 使用 Signal Editor 原文界面*


<!-- source-unit: P283 -->
9. 如果软件需求使用 Requirements Toolbox 表达，可以直接在测试用例表中，为新测试用例及其验证的需求建立双向追溯关系。操作如下：

<!-- source-unit: P284 -->
1. 在测试用例表中，双击新测试用例的 `ReqFile` 单元格，打开文件浏览器。


![图 42 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image68.png)

*图 42 使用 Signal Editor 原文界面*


<!-- source-unit: P286 -->
2. 浏览到保存软件需求的文件夹，选择要链接的需求集，然后单击 `Open` 打开。


![图 43 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image69.png)

*图 43 使用 Signal Editor 原文界面*


<!-- source-unit: P288 -->
3. 在 Requirements Editor 中选择由新测试用例验证的需求。如适用，可以选择多条需求。


![图 44 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image70.png)

*图 44 使用 Signal Editor 原文界面*


<!-- source-unit: P290 -->
4. 返回 Workflow Manager 的测试用例表，右键单击 `ReqID` 单元格，选择 `Add Selected Requirements`，将新测试用例链接到所选需求。


![图 45 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image71.png)

*图 45 使用 Signal Editor 原文界面*


<!-- source-unit: P292 -->
5. 确认新测试用例的 `ReqFile` 和 `ReqID` 单元格均已正确填写。


![图 46 使用 Signal Editor 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image72.png)

*图 46 使用 Signal Editor 原文界面*


<!-- source-unit: P294 -->
10. 核对表格中新测试用例的设置，并确认它已启用。成功指定 `SigSrc` 后，`IsActive` 会自动切换为 `Y`。如果测试用例尚未完成或已经过时，可将 `IsActive` 设为 `N`，禁用该用例。

<!-- source-unit: P295 -->
重复上述操作，继续添加测试用例，直到与当前系统相关的全部需求均得到充分测试。如果测试用例数量超出 `Test Case Authoring` 面板的常规显示范围，滚动条会自动出现，可据此访问全部行。也可以单击展开按钮，扩大该面板以显示更多行；要恢复常规大小，请单击收起按钮。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image73.png)
![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image74.png)

<!-- source-unit: P296 -->
除了禁用测试用例，还可以将其从表格中完全移除。选中要移除的用例后，单击 `Delete Test Case`。单击 `Delete All` 将移除表格中的全部测试用例，同时删除测试台。使用一段时间后，可能积累了不再需要的测试数据和标定数据；要删除未使用的数据文件，请单击 `Cleanup`。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image75.png)
![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image76.png)
![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image77.png)

<!-- source-unit: P297 -->
**注意：** 如果需求变更影响了系统接口，必须重新创建测试用例，包括相应的测试台。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image78.png)

<!-- source-unit: P298 -->
<a id="section-p298"></a>

#### 测试执行与试运行

<!-- source-unit: P299 -->
只有 `IsActive` 为 `Y` 的测试用例才能执行。用例启用后，会自动列在 `Test Execution` 面板的 `Available Test Cases` 下。在该面板中，应通过试运行（Dry Run）确认测试用例本身是否有效，其目标是评估全部启用用例的正确性和有效性。检查试运行结果后，可进行必要修改，纠正并完善正在开发的测试用例。执行试运行的步骤如下：

<!-- source-unit: P300 -->
1. 确认所有已启用用例都列在 `Available Test Cases` 下。如需向该列表添加用例或从中移除用例，请在 `Test Case Authoring` 面板中切换其 `IsActive` 设置。


![图 47 测试执行与试运行 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image79.png)

*图 47 测试执行与试运行 原文界面*


<!-- source-unit: P302 -->
2. 单击 `>` 或 `>>`，将 `Available Test Cases` 中的用例添加到 `Selected Test Cases`。`>` 添加指定用例，`>>` 添加全部可用用例。同样，使用 `<` 或 `<<` 可从 `Selected Test Cases` 移除指定用例或全部用例。


![图 48 测试执行与试运行 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image80.png)

*图 48 测试执行与试运行 原文界面*


<!-- source-unit: P304 -->
3. 单击 `Dry Run`，执行 `Selected Test Cases` 中列出的测试用例。


![图 49 测试执行与试运行 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image81.png)

*图 49 测试执行与试运行 原文界面*


<!-- source-unit: P306 -->
4. 执行结束后，Test Manager 会自动打开。在 Test Manager 中切换到 `Results and Artifacts`，浏览执行结果。判断测试用例是否按预期执行，以及这些用例合起来是否达到了充分的结构覆盖。


![图 50 测试执行与试运行 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image82.png)

*图 50 测试执行与试运行 原文界面*


<!-- source-unit: P308 -->
5. 注意：由 Signal Editor 提供激励的测试用例使用基线判据来约束预期结果。数值比较的默认绝对容差和相对容差分别为 `0.0001` 和 `0.01`。根据基线数据的范围和分辨率，可能需要微调这些容差。要调整某个用例的基线判据容差，请执行以下操作：

<!-- source-unit: P309 -->
1. 在 Test Manager 中切换到 `Test Browser`，选择相应测试用例。

<!-- source-unit: P310 -->
2. 在该测试用例的 `BASELINE CRITERIA` 下，找到需要微调的基线信号。

<!-- source-unit: P311 -->
3. 双击需要编辑的容差，输入新值。可以分别控制绝对容差、相对容差、时间超前容差和时间滞后容差。


![图 51 测试执行与试运行 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image83.png)

*图 51 测试执行与试运行 原文界面*


<!-- source-unit: P313 -->
4. 完成必要编辑后，在 Test Manager 中单击 `Save`。


![图 52 测试执行与试运行 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image84.png)

*图 52 测试执行与试运行 原文界面*


<!-- source-unit: P315 -->
5. 返回 Workflow Manager，单击 `Dry Run`，重新执行测试用例。

<!-- source-unit: P316 -->
6. 核实新容差对结果的影响。

<!-- source-unit: P317 -->
6. 测试用例集完成后，单击 `Create RBT Test File`，使用 `Available Test Cases` 下的全部测试用例创建正式测试文件。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image85.png)


![图 53 测试执行与试运行 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image86.png)

*图 53 测试执行与试运行 原文界面*


<!-- source-unit: P319 -->
**注意：** 测试台、测试数据或测试过程发生更改后，必须重新确认测试用例的有效性。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image78.png)

<!-- source-unit: P320 -->
<a id="section-p320"></a>

### 4.5 动态测试第一部分 基于需求的测试

<!-- source-unit: P321 -->
试运行的主要目标是确认所编写测试用例的有效性，而不是验证软件设计。测试用例正式定版后，即可使用它们验证模型及生成代码。要依据软件需求正式验证模型及生成代码，请使用 Workflow Manager 的 `Dynamic Testing` 选项卡。


![图 54 动态测试第一部分 基于需求的测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image87.png)

*图 54 动态测试第一部分 基于需求的测试 原文界面*


<!-- source-unit: P323 -->
该选项卡包含两个主要面板。第一部分重点介绍 `Requirement-Based Testing` 面板。选择 `Dynamic Testing` 选项卡时，可以看到该面板的 `Test File` 字段会自动填入文件名，即此前在 `Req-Based Tests` 选项卡中创建的正式测试文件。如果正式测试文件不存在，该字段为空。

<!-- source-unit: P324 -->
<a id="section-p324"></a>

#### 基于需求的模型测试

<!-- source-unit: P325 -->
要根据需求验证所选系统，请执行以下操作：

<!-- source-unit: P326 -->
1. 单击 `RBT Test File`，打开基于需求的测试文件。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image88.png)


![图 55 基于需求的模型测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image89.png)

*图 55 基于需求的模型测试 原文界面*


<!-- source-unit: P328 -->
2. 检查测试文件，确认它是最新版本。


![图 56 基于需求的模型测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image90.png)

*图 56 基于需求的模型测试 原文界面*


<!-- source-unit: P330 -->
3. 确认 `Execution Mode` 设置为 `MIL`。


![图 57 基于需求的模型测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image91.png)

*图 57 基于需求的模型测试 原文界面*


<!-- source-unit: P332 -->
4. 单击 `RBT`，执行测试文件。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image92.png)


![图 58 基于需求的模型测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image93.png)

*图 58 基于需求的模型测试 原文界面*


<!-- source-unit: P334 -->
5. 所有测试用例执行完成后，会为当前系统创建三类基于需求的模型测试产物：仿真结果文件、模型测试报告和模型覆盖率报告。可以直接在 Test Manager 中查看结果。

<!-- source-unit: P335 -->
默认情况下，`RBT Results`、`RBT Report` 和 `RBT Coverage Report` 按钮不可用，测试执行后会自动启用。需要时，可用这些按钮打开 MIL 测试结果和生成的报告。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image88.png)
![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image37.png)

<!-- source-unit: P336 -->
<a id="section-p336"></a>

#### 基于需求的代码测试

<!-- source-unit: P337 -->
完成模型验证后，即可继续验证其生成代码。要根据需求验证所选系统的生成代码，请执行以下操作：

<!-- source-unit: P338 -->
1. 将 `Execution Mode` 设置为 `SIL` 或 `PIL`。


![图 59 基于需求的代码测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image94.png)

*图 59 基于需求的代码测试 原文界面*


<!-- source-unit: P340 -->
2. 单击 `RBT`，执行测试文件。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image92.png)


![图 60 基于需求的代码测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image95.png)

*图 60 基于需求的代码测试 原文界面*


<!-- source-unit: P342 -->
3. 所有测试用例执行完成后，会为当前系统创建三类基于需求的代码测试产物：背靠背测试结果文件、代码测试报告和代码覆盖率报告。可以直接在 Test Manager 中查看结果。

<!-- source-unit: P343 -->
注意：将 `Execution Mode` 设置为 `SIL` 或 `PIL` 时，`RBT Results`、`RBT Report` 和 `RBT Coverage Report` 默认不可用，测试执行后会自动启用。需要时，可用这些按钮打开 SIL/PIL 测试结果和生成的报告。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image88.png)
![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image37.png)

<!-- source-unit: P344 -->
基于需求的测试出现失败是不可接受的，必须解决所报告的问题。如果没有报告失败，则必须检查覆盖率结果。如果执行所编写的用例后未达到完整模型覆盖，必须调查覆盖缺口。如果覆盖缺失由遗漏需求、漏设测试用例或实现不当造成，应妥善解决这些问题。否则，覆盖缺口可能对应模型中的某些低层结构，而现有人工编写用例未执行到这些结构；这类缺口可通过基于结构的测试处理。

<!-- source-unit: P345 -->
<a id="section-p345"></a>

### 4.6 生成基于结构的测试 Struct Based Tests

<!-- source-unit: P346 -->
基于需求的测试旨在根据需求验证模型及生成代码，因此应达到完整的需求测试覆盖，即验证与被测系统关联的全部需求。但是，这并不保证模型及其生成代码的结构覆盖完整。为满足结构覆盖要求，可以利用 Design Verifier 生成额外测试用例，针对基于需求的测试尚未达到的覆盖目标进行补充。

<!-- source-unit: P347 -->
与根据需求编写的测试用例不同，为弥补覆盖缺口而开发的用例主要针对条件和判定。由于这些用例由模型算法中的覆盖目标推导而来，其执行称为基于结构的测试。要针对尚未达到的覆盖目标生成测试用例，请使用 Workflow Manager 的 `Struct-Based Tests` 选项卡。


![图 61 生成基于结构的测试 Struct Based Tests 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image96.png)

*图 61 生成基于结构的测试 Struct Based Tests 原文界面*


<!-- source-unit: P349 -->
选择 `Struct-Based Tests` 选项卡时，`Test Generation Settings` 面板中的已有覆盖数据字段 `Coverage Data` 会自动填入文件名。该文件包含从此前基于需求的测试中提取的模型覆盖数据。在基于需求的测试文件成功执行之前，该字段为空。要根据所选系统尚未达到的覆盖目标生成测试，请执行以下操作：

<!-- source-unit: P350 -->
1. 设置 `Test Generation Settings`、`Coverage Filter Settings` 和 `Baseline Criteria Settings` 面板中的选项。默认设置通常较合适，但有时需要尝试其他选项，尤其是在为大型复杂系统生成测试时。全部可用选项见下表。

<!-- source-table: T04 -->

| 项目 | 说明 |
| --- | --- |
| 已有覆盖数据（`Existing Coverage Data`） | 从基于需求的测试中收集的覆盖数据。除非准备使用非正式覆盖结果进行试验，否则不要修改此设置。 |
| 模型覆盖目标（`Model Coverage Objectives`） | 生成的测试用例必须达到的模型覆盖类型。应按目标结构覆盖的严格程度设置。 |
| 测试套件优化（`Test Suite Optimization`） | 生成测试用例时使用的优化策略。如果默认策略未产生可接受的结果，可尝试其他策略。 |
| 测试用例最大步数（`Max Test Case Steps`） | 允许生成的测试用例执行的最大仿真步数。用此选项控制生成用例的长度。 |
| 最长处理时间（`Max Process Time`） | 允许测试生成过程使用的最长时间。如果 Design Verifier 超时，可增大此上限。 |
| 覆盖率过滤器（`Coverage Filter`） | 应用覆盖率过滤器。如果希望在生成测试时排除某些覆盖目标，请启用此选项。 |
| 过滤器文件名（`Filter File Name`） | 所应用覆盖率过滤器的名称。 |
| 绝对容差（原表拼写 `Absolute Toleance`） | 自动生成测试用例的基线判据所使用的绝对容差。 |
| 相对容差（原表拼写 `Relative Toleance`） | 自动生成测试用例的基线判据所使用的相对容差。 |
| 时间超前容差（原表拼写 `Leading Toleance`） | 自动生成测试用例的基线判据所使用的时间超前容差。 |
| 时间滞后容差（原表拼写 `Loagging Toleance`） | 自动生成测试用例的基线判据所使用的时间滞后容差。 |

<!-- source-unit: P351 -->
2. 单击 `Generate Tests`，生成补充测试。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image92.png)


![图 62 生成基于结构的测试 Struct Based Tests 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image97.png)

*图 62 生成基于结构的测试 Struct Based Tests 原文界面*


<!-- source-unit: P353 -->
3. 测试生成完成后，检查测试生成报告，判断那些基于需求的测试未达到的覆盖目标，是否已经由生成的用例实现。

<!-- source-unit: P354 -->
如果所有目标处理完成前分析就已超时，可考虑增大 `Max Process Time` 后重试。如果操作因错误而失败，可考虑直接使用 Design Verifier 应用诊断原因。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image98.png)

<!-- source-unit: P355 -->
默认情况下，`Test Gen Report` 按钮不可用。只有在工程中找到相应报告后，该按钮才会启用。需要时，可以单击它打开生成的报告。注意：测试生成成功时，除了报告，还会自动生成新的测试台和基于结构的测试文件。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image37.png)

<!-- source-unit: P356 -->
<a id="section-p356"></a>

### 4.7 动态测试第二部分 基于结构的测试

<!-- source-unit: P357 -->
测试生成的主要目标，是产生能够执行基于需求的测试未覆盖条件和判定的用例。补充用例生成后，可以用来验证此前未测试到的模型与代码部分。要在验证模型及生成代码时满足结构覆盖要求，请使用 Workflow Manager 的 `Dynamic Testing` 选项卡。


![图 63 动态测试第二部分 基于结构的测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image99.png)

*图 63 动态测试第二部分 基于结构的测试 原文界面*


<!-- source-unit: P359 -->
第二部分重点介绍 `Structure-Based Testing` 面板。选择 `Dynamic Testing` 选项卡时，该面板的 `Test File` 字段会自动填入文件名，即此前在 `Struct-Based Tests` 选项卡中生成的测试文件。如果生成的测试文件不存在，该字段为空。

<!-- source-unit: P360 -->
<a id="section-p360"></a>

#### 基于结构的模型测试

<!-- source-unit: P361 -->
要使用补充测试用例验证所选系统，请执行以下操作：

<!-- source-unit: P362 -->
1. 单击 `SBT Test File`，打开基于结构的测试文件。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image88.png)


![图 64 基于结构的模型测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image100.png)

*图 64 基于结构的模型测试 原文界面*


<!-- source-unit: P364 -->
2. 检查测试文件，确认它是最新版本。


![图 65 基于结构的模型测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image101.png)

*图 65 基于结构的模型测试 原文界面*


<!-- source-unit: P366 -->
3. 确认 `Execution Mode` 设置为 `MIL`。


![图 66 基于结构的模型测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image102.png)

*图 66 基于结构的模型测试 原文界面*


<!-- source-unit: P368 -->
4. 单击 `SBT`，执行测试文件。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image92.png)


![图 67 基于结构的模型测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image103.png)

*图 67 基于结构的模型测试 原文界面*


<!-- source-unit: P370 -->
5. 所有测试用例执行完成后，会为当前系统创建三类基于结构的模型测试产物：仿真结果文件、模型测试报告和模型覆盖率报告。可以直接在 Test Manager 中查看结果。


![图 68 基于结构的模型测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image104.png)

*图 68 基于结构的模型测试 原文界面*


<!-- source-unit: P372 -->
6. 单击 `Merge Coverage`，汇总刚刚对该系统执行的基于需求和基于结构的模型测试覆盖结果。该操作将全部覆盖数据合并到一份报告中，用于说明整体模型测试的完整性。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image105.png)

<!-- source-unit: P373 -->
默认情况下，`SBT Results`、`SBT Report` 和 `SBT Coverage Report` 按钮不可用，测试执行后会自动启用。`Merged Coverage Report` 同样默认不可用，只有成功合并覆盖数据后才会启用。需要时，可用这些按钮打开测试结果和生成的报告。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image88.png)
![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image37.png)

<!-- source-unit: P374 -->
<a id="section-p374"></a>

#### 基于结构的代码测试

<!-- source-unit: P375 -->
完成模型验证后，即可继续验证生成代码。要使用补充测试用例验证所选系统的生成代码，请执行以下操作：

<!-- source-unit: P376 -->
1. 将 `Execution Mode` 设置为 `SIL` 或 `PIL`。


![图 69 基于结构的代码测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image106.png)

*图 69 基于结构的代码测试 原文界面*


<!-- source-unit: P378 -->
2. 单击 `SBT`，执行测试文件。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image92.png)


![图 70 基于结构的代码测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image107.png)

*图 70 基于结构的代码测试 原文界面*


<!-- source-unit: P380 -->
3. 所有测试用例执行完成后，会为当前系统创建三类基于结构的代码测试产物：背靠背测试结果文件、代码测试报告和代码覆盖率报告。可以直接在 Test Manager 中查看结果。


![图 71 基于结构的代码测试 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image108.png)

*图 71 基于结构的代码测试 原文界面*


<!-- source-unit: P382 -->
4. 单击 `Merge Coverage`，汇总刚刚对系统生成代码执行的基于需求和基于结构的代码测试覆盖结果。该操作将全部覆盖数据合并到一份报告中，用于说明整体代码测试的完整性。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image105.png)

<!-- source-unit: P383 -->
注意：将 `Execution Mode` 设置为 `SIL` 或 `PIL` 时，`SBT Results`、`SBT Report` 和 `SBT Coverage Report` 默认不可用，测试执行后会自动启用。`Merged Coverage Report` 同样默认不可用，只有成功合并覆盖数据后才会启用。需要时，可用这些按钮打开测试结果和生成的报告。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image88.png)
![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image37.png)

<!-- source-unit: P384 -->
基于结构的测试不应出现失败。如果报告了失败，必须调查并解决。如果没有报告失败，则必须检查汇总后的覆盖率结果，判断整体测试是否完整，或至少是否充分。

<!-- source-unit: P385 -->
<a id="section-p385"></a>

### 4.8 质量跟踪 Quality Tracking

<!-- source-unit: P386 -->
MBD Workbench 提供基于浏览器的仪表盘，用于跟踪每个模型及其生成代码的质量，包括测试的全面程度。可以随时收集质量指标，获得各模型进展的状态快照。汇总全部模型的质量度量后，即可反映工程的整体状态。

<!-- source-unit: P387 -->
可以为模型生成以下几类仪表盘：

<!-- source-unit: P388 -->
设计质量仪表盘（Design Quality Dashboard）：根据对模型及其生成代码执行的全部静态验证活动，对相关质量指标进行评分，展示模型及代码对所要求建模和编码指南的符合程度。工程中的每个设计模型都有独立的设计质量仪表盘。

<!-- source-unit: P389 -->
测试质量仪表盘（Testing Quality Dashboard）：根据对模型及其生成代码执行的全部动态测试活动，对相关质量指标进行评分，展示模型与代码依据需求得到测试的全面程度。工程中的每个设计模型都有独立的测试质量仪表盘。

<!-- source-unit: P390 -->
工程质量仪表盘（Project Quality Dashboard）：根据工程中找到的全部正式结果，针对各项质量指标为模型评分。随着软件生命周期数据不断归档，可将各模型对每项规定指标的满足程度汇总为数据网格，以便全面检查。工程中的全部设计模型共用这一仪表盘。

<!-- source-unit: P391 -->
要生成质量仪表盘，请使用 Workflow Manager 的 `Quality Tracking` 选项卡。


![图 72 质量跟踪 Quality Tracking 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image109.png)

*图 72 质量跟踪 Quality Tracking 原文界面*


<!-- source-unit: P393 -->
<a id="section-p393"></a>

#### 生成仪表盘

<!-- source-unit: P394 -->
显示仪表盘之前，必须先收集仪表盘数据。汇总数据的步骤如下：

<!-- source-unit: P395 -->
1. 单击 `Design Data`，对所选系统的全部静态验证结果进行评分。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image110.png)


![图 73 生成仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image111.png)

*图 73 生成仪表盘 原文界面*


<!-- source-unit: P397 -->
2. 单击 `Testing Data`，对所选系统的全部动态测试结果进行评分。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image110.png)


![图 74 生成仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image112.png)

*图 74 生成仪表盘 原文界面*


<!-- source-unit: P399 -->
3. 单击 `Project Data`，针对规定指标为工程内的每个设计模型评分。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image110.png)


![图 75 生成仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image113.png)

*图 75 生成仪表盘 原文界面*


<!-- source-unit: P401 -->
现在可以查看仪表盘。仪表盘组件是 Web 服务的客户端，因此需要本地 Web 服务器来显示汇总数据。查看步骤如下：

<!-- source-unit: P402 -->
1. 单击 `Start Web Server`，启动本地 Web 服务。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image114.png)


![图 76 生成仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image115.png)

*图 76 生成仪表盘 原文界面*


<!-- source-unit: P404 -->
2. 单击 `Design Dashboard`，打开所选系统的设计质量仪表盘。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image116.png)


![图 77 生成仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image117.png)

*图 77 生成仪表盘 原文界面*


<!-- source-unit: P406 -->
3. 单击 `Testing Dashboard`，打开所选系统的测试质量仪表盘。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image116.png)


![图 78 生成仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image118.png)

*图 78 生成仪表盘 原文界面*


<!-- source-unit: P408 -->
4. 单击 `Project Dashboard`，打开整个工程的工程质量仪表盘。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image116.png)


![图 79 生成仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image119.png)

*图 79 生成仪表盘 原文界面*


<!-- source-unit: P410 -->
5. 检查质量数据后，可以关闭仪表盘。

<!-- source-unit: P411 -->
6. 单击 `Stop Web Server`，停止本地 Web 服务。

![原文按钮图标](MBD_Workbench_DO178C_中文操作手册_assets/image120.png)


![图 80 生成仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image121.png)

*图 80 生成仪表盘 原文界面*


<!-- source-unit: P413 -->
注意：质量仪表盘中汇总显示的信息主要用于监控进度。与正式验证报告不同，这些仪表盘不包含详细结果。

<!-- source-unit: P414 -->
<a id="section-p414"></a>

#### 设计质量仪表盘

<!-- source-unit: P415 -->
设计质量仪表盘主要用于展示模型及其生成代码对指南的符合程度。该仪表盘包含三个主要面板：`Overview`（概览）、`Model Quality`（模型质量）和 `Code Quality`（代码质量）。


![图 81 设计质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image122.png)

*图 81 设计质量仪表盘 原文界面*


<!-- source-unit: P417 -->
在 `Overview` 面板中，可以看到模型及其生成代码在六个设计质量维度上的得分。雷达图的六个坐标轴如下表所示：

<!-- source-table: T05 -->

| 项目 | 说明 |
| --- | --- |
| 模型规范（Model Standards） | 衡量所选系统对全部已执行 Model Advisor 检查的符合程度。 |
| 模型复杂度（Model Complexity） | 衡量所选系统对全部强制模型指标要求的符合程度。 |
| 无设计错误程度（Absence of Design Errors） | 衡量所选系统不存在错误的程度。 |
| 代码规范（Code Standards） | 衡量所选系统生成代码对全部已执行 MISRA 检查的符合程度。 |
| 代码复杂度（Code Complexity） | 衡量所选系统生成代码对全部强制代码指标要求的符合程度。 |
| 无代码缺陷程度（Absence of Code Defects） | 衡量所选系统生成代码不存在缺陷的程度。 |

<!-- source-unit: P418 -->
`Model Quality` 面板分为三个区域：`Model Standards`（模型规范）、`Model Complexity Metrics`（模型复杂度指标）和 `Design Errors`（设计错误）。


![图 82 设计质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image123.png)

*图 82 设计质量仪表盘 原文界面*


<!-- source-unit: P420 -->
在 `Model Standards` 子面板中，可以查看 Model Advisor 检查项的总体通过百分比，以及下表所列的建模规范分类结果：

<!-- source-table: T06 -->

| 项目 | 说明 |
| --- | --- |
| DO-178C | DO-178C/DO-331 建模规范检查项的通过百分比。 |
| SLCI 兼容性（SLCI Compatibility） | Simulink Code Inspector 兼容性检查项的通过百分比。 |
| MISRA C:2012 | MISRA C:2012 建模规范检查项的通过百分比。 |
| 人工评审（Manual Review） | 若该组件关联了自定义检查单，显示人工模型检查的通过百分比。 |


![图 83 设计质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image124.png)

*图 83 设计质量仪表盘 原文界面*


<!-- source-unit: P422 -->
在 `Model Complexity Metrics` 子面板中，可以查看处于规定限值内的模型指标总体百分比。单击 `Expand Details` 后，还可以查看下表所列的逐项模型度量：

<!-- source-table: T07 -->

| 项目 | 说明 |
| --- | --- |
| **汇总模型复杂度（Aggregated Model Complexity）** |  |
| 圈复杂度（Cyclomatic Complexity） | 所选系统及其全部子系统的累计圈复杂度。 |
| 系统深度（System Depth） | 所选系统中子系统层级的总数。 |
| 系统输入／输出数量（Number of System Inputs/Outputs） | 所选系统根级输入和输出的总数。 |
| 系统参数数量（Number of System Parameters） | 所选系统中参数的总数。 |
| Simulink 模块数量（Number of Simulink Blocks） | 所选系统中 Simulink 模块的总数。 |
| Stateflow 对象数量（Number of Stateflow Objects） | 所选系统中 Stateflow 对象的总数。 |
| Stateflow 代码行数（Stateflow Lines of Code，LOC） | 所选系统中 Stateflow 代码的总行数。 |
| MATLAB 代码行数（MATLAB Lines of Code，LOC） | 所选系统中 MATLAB 代码的总行数。 |
| 查表模块数量（Number of Lookup Table Blocks） | 所选系统中查表（LUT）模块的总数。 |
| 一维查表数量（Number of 1-D Lookup Tables） | 所选系统中一维 LUT 模块的总数。 |
| 二维查表数量（Number of 2-D Lookup Tables） | 所选系统中二维 LUT 模块的总数。 |
| 三维查表数量（Number of 3-D Lookup Tables） | 所选系统中三维 LUT 模块的总数。 |
| 缺少描述性的模块名称（Nondescriptive Block Names） | 所选系统中不具描述性的模块名称总数。 |
| 层级分离违规（Layer Separation Violations） | 所选系统中层级分离违规的总数。同一层级内同时存在非虚拟原生模块和子系统模块时，即发生层级分离违规。 |
| 诊断警告（Diagnostics Warnings） | 所选系统中诊断警告的总数。 |
| 代码分析器警告（Code Analyzer Warnings） | 所选系统中 Code Analyzer 警告的总数。 |
| 克隆数量（Number of Clones） | 所选系统中克隆的总数。 |
| 潜在复用比例（Potential Reuse） | 所选系统中被识别为克隆的子组件所占比例。 |
| 库链接数量（Number of Library Links） | 所选系统中库链接的总数。 |
| 实际复用比例（Actual Reuse） | 所选系统中链接库模块所占的子组件比例。 |
| 模型文件数量（Number of Model Files） | 所选系统使用的模型文件总数。 |
| 已链接需求数量（Number of Linked Requirements） | 链接到所选系统的需求总数。 |
| **分项模型复杂度（Model Complexity）** |  |
| 圈复杂度（Cyclomatic Complexity） | 所选系统每个层级的圈复杂度。 |
| 子系统数量（Number of Subsystems） | 所选系统每个层级中的子系统数量。 |
| Stateflow 图数量（Number of Stateflow Charts） | 所选系统每个层级中的 Stateflow 图数量。 |
| MATLAB 函数数量（Number of MATLAB Functions） | 所选系统每个层级中的 MATLAB 函数数量。 |
| Simulink 模块数量（Number of Simulink Blocks） | 所选系统每个层级中的 Simulink 模块数量。 |
| Stateflow 对象数量（Number of Stateflow Objects） | 所选系统每个 Stateflow 图中的 Stateflow 对象数量。 |
| Stateflow 代码行数（Stateflow Lines of Code，LOC） | 所选系统每个 Stateflow 模块中的代码行数。 |
| MATLAB 代码行数（MATLAB Lines of Code，LOC） | 所选系统每个 MATLAB 函数中的代码行数。 |
| 每个模块的需求数量（Number of Requirements Per Block） | 链接到所选系统中每个模块的需求数量。 |
| 每条需求的模块数量（Number of Blocks Per Requirement） | 链接到所选系统中每条需求的模块数量。 |
| 查表大小（Lookup Table Sizes） | 所选系统中每个 LUT 模块的表大小。 |
| 每条非虚拟总线的元素数量（Number of Elements Per Nonvirtual Bus） | 所选系统中每条非虚拟总线承载的元素数量。 |
| 每条非虚拟总线的层级数量（Number of Levels Per Nonvirtual Bus） | 所选系统中每条非虚拟总线的嵌套层级数量。 |


![图 84 设计质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image125.png)

*图 84 设计质量仪表盘 原文界面*


<!-- source-unit: P424 -->
在 `Design Errors` 子面板中，可以查看无错误设计错误类别的总体百分比，以及下表所列的设计错误分类结果：

<!-- source-table: T08 -->

| 项目 | 说明 |
| --- | --- |
| 死逻辑（Dead Logic） | 所选系统中发现的死逻辑数量。 |
| 除零（Division by Zero） | 所选系统中发现的除零错误数量。 |
| 违反指定最小值与最大值（Specified Min and Max Value Violations） | 所选系统中违反用户指定最小值或最大值的情况数量。 |
| 数组越界访问（Out of Bound Array Access） | 所选系统中发现的数组越界访问错误数量。 |
| 整数溢出（Integer Overflow） | 所选系统中发现的定点数据溢出错误数量。 |
| 违反指定模块输入范围（Specified Block Input Range Violations） | 所选系统中发现的输入范围违规数量。 |

<!-- source-unit: P425 -->
`Code Quality` 面板也分为三个区域：`Code Standards`（代码规范）、`Code Complexity Metrics`（代码复杂度指标）以及 `Run-Time Errors and Defects`（运行时错误与缺陷）。


![图 85 设计质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image126.png)

*图 85 设计质量仪表盘 原文界面*


<!-- source-unit: P427 -->
在 `Code Standards` 子面板中，可以查看 MISRA 检查项的总体通过百分比，以及下表所列的编码规范分类结果：

<!-- source-table: T09 -->

| 项目 | 说明 |
| --- | --- |
| MISRA 指令（MISRA Directives） | 已执行的 MISRA 指令检查的通过百分比。 |
| MISRA 规则（MISRA Rules） | 已执行的 MISRA 规则检查的通过百分比。 |
| 人工评审（原表拼写 Manuel Review） | 若该组件关联了自定义检查单，显示人工代码检查的通过百分比。 |


![图 86 设计质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image127.png)

*图 86 设计质量仪表盘 原文界面*


<!-- source-unit: P429 -->
在 `Code Complexity Metrics` 子面板中，可以查看处于规定限值内的代码指标总体百分比。单击 `Expand Details` 后，还可以查看下表所列的逐项代码度量：

<!-- source-table: T10 -->

| 项目 | 说明 |
| --- | --- |
| 源文件数量（Number of Source Files） | 为所选系统生成的源文件数量。 |
| 头文件数量（Number of Header Files） | 为所选系统生成的头文件数量。 |
| 递归数量（Number of Recursion） | 发现的递归数量。递归指函数直接或间接调用自身。 |
| 直接递归数量（Number of Direct Recursion） | 发现的直接递归数量。直接递归指递归函数在自身函数体中调用自己。 |
| 全局变量内存占用（Global Variables，bytes） | 所选系统生成代码所使用的全部全局变量的内存占用。 |
| **文件指标（File Metrics）** |  |
| 行数（Number of Lines） | 每个代码文件的总行数。 |
| 注释密度（Comment Density） | 每个代码文件中注释数量与语句数量的比值。 |
| **函数指标（Function Metrics）** |  |
| 圈复杂度（Cyclomatic Complexity） | 每个函数体中线性独立路径的数量。 |
| 语言作用域（Language Scope） | 用于反映各函数维护成本的指标。 |
| 函数体行数（Number of Lines Within Body） | 每个函数体中的行数。 |
| 可执行行数（Number of Executable Lines） | 每个函数体中可执行代码的行数。 |
| 指令／语句数量（Number of Instructions/Statements） | 每个函数中的指令数量。 |
| 路径数量（Number of Paths） | 每个函数中的静态路径数量。数值越大，可读性越低。 |
| 控制流语句嵌套深度（Nesting Depth of Control Flow Statements） | 每个函数中控制流语句的嵌套深度。 |
| goto 语句数量（Number of Goto Statements） | 每个函数中 `goto` 语句的数量。 |
| return 语句数量（Number of Return Statements） | 每个函数中 `return` 语句的数量。 |
| 调用者函数数量（Number of Calling Functions） | 调用每个函数的不同调用者数量。 |
| 被调用函数数量（Number of Called Functions） | 每个函数体内调用的不同函数数量。 |
| 调用发生次数（Number of Call Occurrences） | 每个函数体中的函数调用次数。 |
| 函数参数数量（Number of Function Parameters） | 每个函数的参数数量。 |
| 局部静态变量数量（Number of Local Static Variables） | 每个函数中局部静态变量的总数。 |
| 局部非静态变量数量（Number of Local Non-static Variables） | 每个函数中局部变量的总数。译注：原表标题限定为非静态变量，但说明仅写作局部变量，本译文保留该差异。 |
| 累计栈大小（Accumulated Stack Sizes） | 每个函数的累计栈大小。 |


![图 87 设计质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image128.png)

*图 87 设计质量仪表盘 原文界面*


<!-- source-unit: P431 -->
在 `Run-Time Errors and Defects` 子面板中，可以查看无错误运行时错误类别的总体百分比，以及下表所列的运行时错误分类结果：

<!-- source-table: T11 -->

| 项目 | 说明 |
| --- | --- |
| **Bug Finder 分析（原表拼写 Big Finder Analysis）** |  |
| 数值（Numerical） | 与代码中的变量、变量值、数据类型及其用法相关的错误数量。包括：数学运算、类型转换溢出、运算溢出。 |
| 静态内存（Static Memory） | 与静态分配内存的使用相关的错误数量。包括：数组越界访问、空指针、指针类型转换。 |
| 动态内存（Dynamic Memory） | 与动态分配内存的使用相关的错误数量。包括：动态分配内存的释放、未受保护的内存分配。 |
| 数据流（Data Flow） | 与信息在代码中流动方式相关的错误数量。包括：死代码或不可达代码、未使用的代码、未初始化的信息。 |
| 编程（Programming） | 与编程语法相关的错误数量。包括：混淆赋值与相等比较运算符、变量限定符或声明不匹配、字符串格式不正确。 |
| 并发（Concurrency） | 与多任务代码相关的错误数量。包括：多个任务操作未受保护变量时发生的数据竞争，以及涉及临界区死锁的加锁缺陷。 |
| **Code Prover 验证（Code Prover Verification）** |  |
| 数值（Numerical） | 与算术运算相关的运行时错误数量，例如溢出或除零。 |
| 静态内存（Static Memory） | 与静态分配内存相关的运行时错误数量，例如数组访问或指针解引用越界。 |
| 数据流（Data Flow） | 与程序中信息流动相关的运行时错误数量，例如：读取的变量尚未初始化；条件语句的某个分支不可达；某个函数在运行时未被调用。 |
| 控制流（Control Flow） | 会改变程序执行流程、并可能导致确定性运行时错误的语句数量。 |
| 共享变量（Shared Variables） | 对多个任务共享的受保护全局变量进行不当访问所导致的问题数量。 |
| 其他（Other） | 与某些 C/C++ 库相关的运行时错误数量，例如：使用标准库数学函数时发生定义域错误；使用 AUTOSAR 运行时环境（Run Time Environment，RTE）函数时超出设计范围。 |

<!-- source-unit: P432 -->
<a id="section-p432"></a>

#### 测试质量仪表盘

<!-- source-unit: P433 -->
测试质量仪表盘主要用于展示模型及其生成代码依据所实现需求得到验证的程度。该仪表盘包含三个主要面板：`Overview`（概览）、`Test Case Analysis`（测试用例分析）和 `Test Result Analysis`（测试结果分析）。


![图 88 测试质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image129.png)

*图 88 测试质量仪表盘 原文界面*


<!-- source-unit: P435 -->
在 `Overview` 面板中，可以看到模型及其生成代码在五个测试质量维度上的得分。雷达图的五个坐标轴如下表所示：

<!-- source-table: T12 -->

| 项目 | 说明 |
| --- | --- |
| 需求测试覆盖（Test Coverage） | 衡量与所选系统关联的全部需求得到测试的程度及完整性。 |
| 模型测试（Model Testing） | 衡量所选系统对其所实现需求的符合程度。 |
| 模型覆盖（Model Coverage） | 衡量所选系统得到测试的完整程度。 |
| 代码测试（Code Testing） | 衡量由所选系统生成的可执行程序对需求的符合程度。 |
| 代码覆盖（Code Coverage） | 衡量所选系统生成代码得到测试的完整程度。 |


![图 89 测试质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image130.png)

*图 89 测试质量仪表盘 原文界面*


<!-- source-unit: P437 -->
在 `Test Case Analysis` 面板中，可以查看下表所列的分类信息：

<!-- source-table: T13 -->

| 项目 | 说明 |
| --- | --- |
| 至少由 1 个用例验证的需求（Requirements verified by at least 1 test case） | 与所选系统关联的全部需求中，至少由一个测试用例验证的需求所占的总体百分比。 |
| 未测试需求（Untested requirements） | 与所选系统关联、但未由任何测试用例验证的需求数量。 |
| 关联 10+ 个用例的需求（Requirements with 10+ test cases） | 与所选系统关联、且由超过 10 个测试用例验证的需求数量。这表明相关需求可能需要进一步分解。 |
| 至少关联 1 条需求的测试用例（Test cases linked to at least 1 requirement） | 用于验证所选系统的全部测试用例中，至少关联一条需求的用例所占的总体百分比。 |
| 未关联需求的测试用例（Unlinked test cases） | 用于验证所选系统、但未关联任何需求的测试用例数量。 |
| 关联 10+ 条需求的测试用例（Test cases with 10+ requirements） | 用于验证所选系统、且关联超过 10 条需求的测试用例数量。这表明相关测试用例可能需要进一步分解。 |
| 测试构成（Test Breakdown） | 基于需求的测试用例数量与基于结构的测试用例数量之比。原文认为，如果基于结构的用例多于基于需求的用例，通常说明测试不够充分。 |

<!-- source-unit: P438 -->
`Test Result Analysis` 面板包含两个选项卡：`Requirement-Based and Structure-Based Tests`（基于需求及基于结构的测试）和 `Requirement-Based Tests Only`（仅基于需求的测试）。每个选项卡又分为 `Model Simulation Result Analysis`（模型仿真结果分析）和 `Code Test Result Analysis`（代码测试结果分析）两个区域。第一个选项卡的信息覆盖所选系统的全部测试用例；第二个选项卡则专门显示人工编写的基于需求的测试用例所达到的结果。


![图 90 测试质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image131.png)

*图 90 测试质量仪表盘 原文界面*


<!-- source-unit: P440 -->
在 `Model Simulation Result Analysis` 子面板中，可以查看下表所列的分类信息：

<!-- source-table: T14 -->

| 项目 | 说明 |
| --- | --- |
| MIL 测试状态（MIL Test Status） | 模型在环（model-in-the-loop，MIL）测试的总体通过百分比。 |
| 模型覆盖（Model Coverage） | 模型覆盖结果，包括：执行覆盖、判定覆盖、条件覆盖、修正条件／判定覆盖（MC/DC）。 |

<!-- source-unit: P441 -->
同样，在 `Code Test Result Analysis` 子面板中，可以查看下表所列的分类信息：

<!-- source-table: T15 -->

| 项目 | 说明 |
| --- | --- |
| SIL 测试状态（SIL Test Status） | 软件在环（software-in-the-loop，SIL）测试的总体通过百分比。 |
| PIL 测试状态（PIL Test Status） | 处理器在环（processor-in-the-loop，PIL）测试的总体通过百分比。 |
| 代码覆盖（Code Coverage） | 代码覆盖结果，包括：语句覆盖、判定覆盖、条件覆盖、修正条件／判定覆盖（MC/DC）、函数覆盖和函数调用覆盖。 |

<!-- source-unit: P442 -->
<a id="section-p442"></a>

#### 工程质量仪表盘

<!-- source-unit: P443 -->
工程质量仪表盘用于监控整个工程的开发情况，全面展示各设计模型针对每项规定目标的进展。


![图 91 工程质量仪表盘 原文界面](MBD_Workbench_DO178C_中文操作手册_assets/image132.png)

*图 91 工程质量仪表盘 原文界面*


<!-- source-unit: P445 -->
该仪表盘显示 `Project Readiness (%)` 得分，其值为工程内各设计模型完成率的平均值。此外，还提供热力图，展示各模型或系统满足下表规定要求的情况：

<!-- source-table: T16 -->

译注：下表 A-4、A-5 等编号沿用原文引用，不是本译稿的表格编号。

| 项目 | 说明 |
| --- | --- |
| RBT | 在模型在环环境中执行的全部已启用、基于需求的测试用例的通过百分比。（表 A-4） |
| MIL | 在模型在环环境中执行的全部已启用测试用例的通过百分比。（表 A-4） |
| MSTD | 符合建模规范的总体百分比。（表 A-4） |
| MMET | 符合模型复杂度指标要求的总体百分比。（表 A-4） |
| DED | 理想状态与设计错误密度之间的差值。设计错误密度由已识别错误数量除以 Simulink Design Verifier 目标总数进行归一化得到。（表 A-4） |
| SLCI | 已建立追溯关系并完成验证的代码总体百分比。（表 A-5） |
| CSTD | 符合编码规范的总体百分比。（表 A-5） |
| CMET | 符合代码复杂度指标要求的总体百分比。（表 A-5） |
| RTE | 理想状态与代码缺陷密度之间的差值。缺陷密度由 Bug Finder 和 Code Prover 识别的运行时错误数量除以可执行代码总行数进行归一化得到。（表 A-5 和 A6，编号沿用原文） |
| PIL | 如果具备处理器在环环境，则为其中执行的全部已启用用例的通过百分比；否则为软件在环环境中的通过百分比。（表 A-6） |
| MCOV | 已达到的模型覆盖总体百分比。（表 A-4 和 A-7） |
| CCOV | 已达到的代码覆盖总体百分比。（表 A-7） |
| REQ | 所选系统实现的需求中，已关联到测试用例的需求总体百分比。（表 A-7） |

<!-- source-unit: P447 -->
<a id="section-p447"></a>

## 5 MBD Workbench API 参考

<!-- source-unit: P448 -->
本节按原表顺序介绍 MBD Workbench 提供的常用 API，包含 API 名称与说明。为便于阅读和复制命令，原文的两列长表在此展开为逐函数条目。

<!-- source-table: T17 -->

<!-- source-unit: T17R01C0, T17R01C1 -->

### addModelFolder

基于目录模板 `04_Design\sample_model` 创建新的模型目录，同时在 `05_Code\verification_results` 的各子目录下创建模型专用目录。

语法：

```matlab
addModelFolder('MODEL', 'Nonreusable')
addModelFolder('MODEL', 'Reusable')
addModelFolder('MODEL', 'Rate Independent')
```

如果省略第二个参数，新模型默认为可复用模型。

<!-- source-unit: T17R02C0, T17R02C1 -->

### removeModelFolder

从当前工程中移除模型目录。此操作只将模型目录排除出工程，不会从文件系统删除该目录。

语法：

```matlab
removeModelFolder('MODEL')
```

<!-- source-unit: T17R03C0, T17R03C1 -->

### deleteModelFolder

删除模型目录。在将模型目录从工程中移除后，可使用此函数从文件系统中彻底删除该目录。

语法：

```matlab
deleteModelFolder('MODEL')
```

<!-- source-unit: T17R04C0, T17R04C1 -->

### cleanProject

删除工程根目录下所有未纳入该工程的文件。

语法：

```matlab
cleanProject()
```

<!-- source-unit: T17R05C0, T17R05C1 -->

### clearCoverageData

清除全部模型覆盖数据和代码覆盖数据。

语法：

```matlab
clearCoverageData()
```

<!-- source-unit: T17R06C0, T17R06C1 -->

### clearTestManager

清空 Test Manager 中已加载的全部测试文件和结果集。

语法：

```matlab
clearTestManager()
```

<!-- source-unit: T17R07C0, T17R07C1 -->

### resetWorkspace

清除工作区内除默认模型配置以外的全部变量。可用此函数代替 `clear`。

语法：

```matlab
resetWorkspace()
```

<!-- source-unit: T17R08C0, T17R08C1 -->

### returnToWorkFolder

切换工作目录，返回 `work` 文件夹。

语法：

```matlab
returnToWorkFolder()
```

<!-- source-unit: T17R10C0, T17R10C1 -->

### resetDefaultMdlAdvCfg

将默认 Model Advisor 配置恢复为出厂默认配置。

语法：

```matlab
resetDefaultMdlAdvCfg()
```

<!-- source-unit: T17R11C0, T17R11C1 -->

### setDefaultMdlAdvCfg

设置默认 Model Advisor 配置，按 `project_configuration\checks\do178Checks.json` 启用检查项。

语法：

```matlab
setDefaultMdlAdvCfg()
```

<!-- source-unit: T17R13C0, T17R13C1 -->

### viewModelDependency

启动 Model Dependency Viewer，显示模型层级。

语法：

```matlab
viewModelDependency('MODEL')
```

<!-- source-unit: T17R14C0, T17R14C1 -->

### viewFileDependency

启动 Model Dependency Viewer，显示模型与库文件的依赖关系。

语法：

```matlab
viewFileDependency('MODEL')
```

<!-- source-unit: T17R16C0, T17R16C1 -->

### genSrcCode

使用 Embedded Coder 从模型生成源代码。

语法：

```matlab
genSrcCode('MODEL')
genSrcCode('MODEL', 'TreatAsTopMdl')
```

指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。

该函数生成以下内容：

- 源代码，位于 `05_Code\specification` 下的 `MODEL_ert_rtw` 或 `slprj\ert\MODEL`。

<!-- source-unit: T17R18C0, T17R18C1 -->

### genSDD

从模型生成系统设计说明（System Design Description，SDD）报告。

语法：

```matlab
genSDD('MODEL')
genSDD('MODEL', 'AUTHOR')
genSDD('MODEL', [], 'CI')
```

第二个参数可用于指定作者姓名。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告。

该函数生成以下内容：

- 以模型命名的报告，位于 `04_Design\MODEL\specification\documents`。

<!-- source-unit: T17R19C0, T17R19C1 -->

### genReqReport

根据需求集生成需求报告。

语法：

```matlab
genReqReport('REQSET')
genReqReport('REQSET', 'AUTHOR')
genReqReport('REQSET', [], 'CI')
```

第二个参数可用于指定作者姓名。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告。

该函数生成以下内容：

- 以需求集命名的报告，位于 `02_Requirements\specification\documents`。

<!-- source-unit: T17R21C0, T17R21C1 -->

### checkModelStds

根据软件模型标准检查模型，并生成 Model Advisor 报告。

语法：

```matlab
checkModelStds('MODEL')
checkModelStds('MODEL', 'TreatAsTopMdl')
checkModelStds('MODEL', [], 'CI')
```

指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容：

- 以模型命名的报告，位于 `04_Design\MODEL\verification_results\design_standard_checks`。

<!-- source-unit: T17R22C0, T17R22C1 -->

### computeModelMetrics

计算模型指标，并生成 Model Advisor 报告。

语法：

```matlab
computeModelMetrics('MODEL')
computeModelMetrics('MODEL', 'TreatAsTopMdl')
computeModelMetrics('MODEL', [], 'CI')
```

指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容：

- 以模型命名的报告，位于 `04_Design\MODEL\verification_results\model_metrics`。

<!-- source-unit: T17R23C0, T17R23C1 -->

### detectDesignErrs

检测模型中的设计错误或死逻辑，并生成 Design Verifier 报告。

语法：

```matlab
detectDesignErrs('MODEL')
detectDesignErrs('MODEL', 'DetectActiveLogic')
detectDesignErrs('MODEL', [], 300)
detectDesignErrs('MODEL', [], [], 'CI')
```

指定 `DetectActiveLogic` 时启用穷尽式分析；否则跳过用于确认模型中活动逻辑的深入分析。第三个参数控制分析时间上限。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容：

- 以模型命名的报告，位于 `04_Design\MODEL\verification_results\design_error_detections`。

<!-- source-unit: T17R24C0, T17R24C1 -->

### checkCodeStds

根据软件代码标准检查模型的生成代码，并生成 Bug Finder 报告。

语法：

```matlab
checkCodeStds('MODEL')
checkCodeStds('MODEL', 'TreatAsTopMdl')
checkCodeStds('MODEL', [], 'CI')
```

指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容：

- 以模型命名的报告，位于 `05_Code\verification_results\code_standard_checks\MODEL`。

<!-- source-unit: T17R25C0, T17R25C1 -->

### computeCodeMetrics

计算模型生成代码的指标，并生成 Bug Finder 报告。

语法：

```matlab
computeCodeMetrics('MODEL')
computeCodeMetrics('MODEL', 'TreatAsTopMdl')
computeCodeMetrics('MODEL', [], 'CI')
```

指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容：

- 以模型命名的报告，位于 `05_Code\verification_results\code_metrics\MODEL`。

<!-- source-unit: T17R26C0, T17R26C1 -->

### detectCodingErrs

检测模型生成代码中的编码错误，并生成 Bug Finder 报告。

语法：

```matlab
detectCodingErrs('MODEL')
detectCodingErrs('MODEL', 'TreatAsTopMdl')
detectCodingErrs('MODEL', [], 'CI')
```

指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容：

- 以模型命名的报告，位于 `05_Code\verification_results\coding_error_detections\MODEL`。

<!-- source-unit: T17R27C0, T17R27C1 -->

### proveCodeQuality

证明模型的生成代码不存在缺陷，并生成 Code Prover 报告。

语法：

```matlab
proveCodeQuality('MODEL')
proveCodeQuality('MODEL', 'TreatAsTopMdl')
proveCodeQuality('MODEL', 'IncludeAllChildMdls')
proveCodeQuality('MODEL', 'CI')
proveCodeQuality('MODEL', 'IncludeAllChildMdls', 'CI')
```

指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。`IncludeAllChildMdls` 选项将全部引用模型的生成代码一并纳入分析。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容：

- 以模型命名的报告，位于 `05_Code\verification_results\code_proving\MODEL`。

<!-- source-unit: T17R28C0, T17R28C1 -->

### verifySrcCode2Model

验证生成代码是否与模型一致、并可追溯到模型，然后生成 Simulink Code Inspector 报告。

语法：

```matlab
verifySrcCode2Model('MODEL')
verifySrcCode2Model('MODEL', 'TreatAsTopMdl')
verifySrcCode2Model('MODEL', 'IncludeAllChildMdls')
verifySrcCode2Model('MODEL', 'CI')
verifySrcCode2Model('MODEL', 'IncludeAllChildMdls', 'CI')
```

指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。`IncludeAllChildMdls` 选项将全部引用模型的生成代码一并纳入检查。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容：

- 以模型命名的报告，位于 `05_Code\verification_results\code_reviews\MODEL`。

<!-- source-unit: T17R30C0, T17R30C1 -->

### verifyModel2Reqs

验证模型是否符合软件高层需求，并执行模型覆盖分析。全部测试通过标准仿真，在主机上执行编译后的模型。

语法：

```matlab
verifyModel2Reqs('MODEL')
verifyModel2Reqs('MODEL', 'TreatAsTopMdl')
verifyModel2Reqs('MODEL', [], 'AUTHOR')
verifyModel2Reqs('MODEL', [], [], 'CI')
```

指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。第三个参数可用于指定作者姓名。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容，均以模型命名：

- 测试报告：`04_Design\MODEL\verification_results\simulation_results\HLR`。
- 覆盖率报告：`04_Design\MODEL\verification_results\model_coverages\HLR`。

<!-- source-unit: T17R31C0, T17R31C1 -->

### verifyObjCode2Reqs

验证模型的可执行目标代码是否符合软件高层需求。原文的概述说明，全部测试通过 PIL 仿真在目标计算机上执行编译代码。

**译注：** 该概述只提到 PIL，但下方语法和参数说明同时支持 SIL 与 PIL；实际执行环境由第二个参数决定。

语法：

```matlab
verifyObjCode2Reqs('MODEL', 'SIL')
verifyObjCode2Reqs('MODEL', 'PIL')
verifyObjCode2Reqs('MODEL', 'SIL', 'DisableCoverage')
verifyObjCode2Reqs('MODEL', 'SIL', [], 'TreatAsTopMdl')
verifyObjCode2Reqs('MODEL', 'SIL', [], [], 'AUTHOR')
verifyObjCode2Reqs('MODEL', 'SIL', [], [], [], 'CI')
```

第二个参数决定通过 SIL 仿真在主机上运行编译代码，还是通过 PIL 仿真在目标计算机上运行编译代码。`DisableCoverage` 选项用于关闭覆盖分析；否则会对代码插桩，以执行覆盖分析。指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。第五个参数可用于指定作者姓名。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

使用 SIL 时生成以下报告，均以模型命名：

- 测试报告：`05_Code\verification_results\eoc_test_results\MODEL\host\HLR\instrumented`。
- 覆盖率报告：`05_Code\verification_results\code_coverages\MODEL\host\HLR`。

使用 PIL 时生成以下报告，均以模型命名：

- 测试报告：`05_Code\verification_results\eoc_test_results\MODEL\target\HLR\instrumented`。
- 覆盖率报告：`05_Code\verification_results\code_coverages\MODEL\target\HLR`。

使用 SIL 并指定 `DisableCoverage` 时：

- 在 `05_Code\verification_results\eoc_test_results\MODEL\host\HLR\uninstrumented` 下生成以模型命名的测试报告。

使用 PIL 并指定 `DisableCoverage` 时：

- 在 `05_Code\verification_results\eoc_test_results\MODEL\target\HLR\uninstrumented` 下生成以模型命名的测试报告。

<!-- source-unit: T17R32C0, T17R32C1 -->

### genLowLevelTests

使用 Simulink Design Verifier，根据已有覆盖数据为模型生成低层测试。

语法：

```matlab
genLowLevelTests('MODEL')
genLowLevelTests('MODEL', ...
    'ModelCoverageObjectives', 'MCDC', ...
    'TestSuiteOptimization', 'IndividualObjectives', ...
    'MaxProcessTime', 300, ...
    'AbsTol', 1e-6, ...
    'RelTol', 1e-3, ...
    'CI', true)
```

使用 `ModelCoverageObjectives` 选择覆盖分析类型，可选 `Decision`、`ConditionDecision` 或 `MCDC`。使用 `TestSuiteOptimization` 选择测试生成策略，可选 `Auto`、`IndividualObjectives`、`LongTestCases` 或 `LargeModel (Nonlinear Extended)`。`MaxProcessTime` 控制分析时间上限。`AbsTol` 和 `RelTol` 控制生成测试用例的基线判据容差。`CI` 控制是否按持续集成场景执行；设为 `true` 后，不会在任务结束时自动打开报告。

未指定时，`ModelCoverageObjectives`、`TestSuiteOptimization`、`MaxProcessTime`、`AbsTol`、`RelTol` 和 `CI` 的默认值依次为 `MCDC`、`IndividualObjectives`、`300`、`1e-6`、`1e-3` 和 `false`。

**译注：** 此处自动生成测试的默认绝对／相对容差为 `1e-6`／`1e-3`，与前文使用 Signal Editor 编写用例时的 `0.0001`／`0.01` 不同。两处数值按各自原文保留，不应混用。

该函数生成以下内容，均以模型命名：

- 报告：位于 `04_Design\MODEL\test_cases\LLR`。
- 测试文件：位于 `04_Design\MODEL\test_cases\LLR`。

<!-- source-unit: T17R33C0, T17R33C1 -->

### verifyModel2LowLevelTests

使用 Simulink Design Verifier 生成的低层测试验证模型，并执行模型覆盖分析。全部测试通过普通仿真，在主机上执行编译后的模型。

语法：

```matlab
verifyModel2LowLevelTests('MODEL')
verifyModel2LowLevelTests('MODEL', 'TreatAsTopMdl')
verifyModel2LowLevelTests('MODEL', [], 'AUTHOR')
verifyModel2LowLevelTests('MODEL', [], [], 'CI')
```

指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。第三个参数可用于指定作者姓名。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容，均以模型命名：

- 测试报告：`04_Design\MODEL\verification_results\simulation_results\LLR`。
- 覆盖率报告：`04_Design\MODEL\verification_results\model_coverages\LLR`。

<!-- source-unit: T17R34C0, T17R34C1 -->

### verifyObjCode2LowLevelTests

使用 Simulink Design Verifier 生成的低层测试，验证模型的可执行目标代码。原文的概述说明，全部测试通过 PIL 仿真在目标计算机上执行编译代码。

**译注：** 该概述只提到 PIL，但下方语法和参数说明同时支持 SIL 与 PIL；实际执行环境由第二个参数决定。

语法：

```matlab
verifyObjCode2LowLevelTests('MODEL', 'SIL')
verifyObjCode2LowLevelTests('MODEL', 'PIL')
verifyObjCode2LowLevelTests('MODEL', 'SIL', ...
    'DisableCoverage')
verifyObjCode2LowLevelTests('MODEL', 'SIL', [], ...
    'TreatAsTopMdl')
verifyObjCode2LowLevelTests('MODEL', 'SIL', [], [], ...
    'AUTHOR')
verifyObjCode2LowLevelTests('MODEL', 'SIL', [], [], [], 'CI')
```

第二个参数决定通过 SIL 仿真在主机上运行编译代码，还是通过 PIL 仿真在目标计算机上运行编译代码。`DisableCoverage` 选项用于关闭覆盖分析；否则会对代码插桩，以执行覆盖分析。指定 `TreatAsTopMdl` 时，将模型作为顶层模型处理；否则按引用模型处理。第五个参数可用于指定作者姓名。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

使用 SIL 时生成以下报告，均以模型命名：

- 测试报告：`05_Code\verification_results\eoc_test_results\MODEL\host\LLR\instrumented`。
- 覆盖率报告：`05_Code\verification_results\code_coverages\MODEL\host\LLR`。

使用 PIL 时生成以下报告，均以模型命名：

- 测试报告：`05_Code\verification_results\eoc_test_results\MODEL\target\LLR\instrumented`。
- 覆盖率报告：`05_Code\verification_results\code_coverages\MODEL\target\LLR`。

使用 SIL 并指定 `DisableCoverage` 时：

- 在 `05_Code\verification_results\eoc_test_results\MODEL\host\LLR\uninstrumented` 下生成以模型命名的测试报告。

使用 PIL 并指定 `DisableCoverage` 时：

- 在 `05_Code\verification_results\eoc_test_results\MODEL\target\LLR\uninstrumented` 下生成以模型命名的测试报告。

<!-- source-unit: T17R35C0, T17R35C1 -->

### mergeModelCoverage

合并两类模型测试的覆盖结果：根据软件高层需求进行的验证，以及使用 Simulink Design Verifier 生成的低层测试进行的验证。

语法：

```matlab
mergeModelCoverage('MODEL')
mergeModelCoverage('MODEL', 'CI')
```

`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

该函数生成以下内容：

- 以模型命名的覆盖率报告，位于 `04_Design\MODEL\verification_results\model_coverages`。

<!-- source-unit: T17R36C0, T17R36C1 -->

### mergeCodeCoverage

合并两类生成代码测试的覆盖结果：根据软件高层需求进行的验证，以及使用 Simulink Design Verifier 生成的低层测试进行的验证。

语法：

```matlab
mergeCodeCoverage('MODEL', 'SIL')
mergeCodeCoverage('MODEL', 'PIL')
mergeCodeCoverage('MODEL', 'SIL', 'CI')
```

第二个参数决定所收集的覆盖结果来自主机上的 SIL 仿真，还是目标计算机上的 PIL 仿真。`CI` 选项用于持续集成场景，启用后不会在任务结束时自动打开报告，同时返回记录操作结果的结果对象。

使用 SIL 时生成以下内容：

- 以模型命名的覆盖率报告，位于 `05_Code\verification_results\code_coverages\MODEL\host`。

使用 PIL 时生成以下内容：

- 以模型命名的覆盖率报告，位于 `05_Code\verification_results\code_coverages\MODEL\target`。
