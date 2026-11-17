# 可视化构建架构（Compose Architectures Visually）

原文：MathWorks《[Compose Architectures Visually](https://jp.mathworks.com/help/systemcomposer/ug/author-composition.html)》  
译文整理日期：2026-09-10

译文说明：本文依据你提供的整页截图，核对对应网页后逐节翻译，覆盖文章正文、操作步骤、注意与提示、代码、图示及“另请参阅”。网站导航、地区选择和页脚不属于译文正文。菜单、函数和代码标识保留英文，便于在软件中对照。原图保存在同目录下的 `SystemComposer_可视化架构建模_assets` 文件夹中；移动本文时请同时移动该文件夹。原文和图示版权归 MathWorks 所有，本文件为非官方中文译文。

---

你可以在 System Composer™ 中创建和编辑可视化图表，以表示系统架构。在系统的组合结构中使用组件、端口和连接等架构元素，通过分解组件建立架构的层次结构，并在各层之间导航。

借助 MATLAB® 代码以及 [`importModel`][ref01] 和 [`exportModel`][ref02] 函数，可以将外部架构描述导入 System Composer。更多信息请参阅[导入和导出架构模型][ref03]。

也可以通过 MATLAB 编程来创建和自定义各种架构元素。详细信息请参阅[以编程方式构建架构模型][ref04]。

> **注意：架构数据与数据字典**  
> 加载已有数据字典时，Simulink® 会自动将其迁移到 Simulink 数据字典的 **Architectural Data（架构数据）** 部分。除了[接口编辑器（Interface Editor）][ref05]，还可以使用[架构数据编辑器（Architectural Data Editor）][ref06] 和 [`Simulink.dictionary.ArchitecturalData`][ref07] 编程接口，管理和修改接口及值类型。  
> 有关架构数据管理的更多信息，请参阅[在架构数据部分存储共享数据][ref08]和[以编程方式在架构数据部分存储数据][ref09]。

> **提示**  
> 要了解 System Composer 中的概念如何应用于系统工程设计，请参阅 [System Composer 概念][ref10]。

## 创建架构模型（Create Architecture Model）

System Composer 架构表示由组件组成的系统，以及组件之间在结构和行为上的接口关系。不同类型的架构用于描述系统的不同方面。你可以通过视图呈现架构中一部分组件，也可以使用[参数编辑器（Parameter Editor）][ref11]在架构层级定义参数。

System Composer 模型是保存架构信息的文件，其中包含组件、端口、连接器、接口和行为等内容。架构模型包含一个顶层架构，用于承载系统的组合结构。通过这一顶层架构，还可以定义本系统与其他系统之间的接口。

要建立系统的物理架构和逻辑架构，可以从空白架构模型开始。以下三种方法均可用于创建架构模型。

**方法一：使用 System Composer 启动页面。** 在 MATLAB 命令行窗口中输入：

```matlab
systemcomposer
```

选择 **Architecture Model（架构模型）**。

![在启动页面中选择 Architecture Model，再单击 Create Model 创建空白架构模型。](SystemComposer_可视化架构建模_assets/system-composer-start-page.png)

System Composer 提供 **Architecture Model（架构模型）** 和 **Software Architecture Model（软件架构模型）** 两种架构模型模板，分别面向系统工程和软件架构建模中的不同领域。所有架构模型都提供相同的结构建模能力，同时具有一些与领域和平台有关的约束。有关架构模板的更多信息，请参阅[选择用于系统设计的架构模板][ref12]。

**方法二：从已有模型中新建。** 在 Simulink 模型或 System Composer 架构模型中，打开 **Simulation（仿真）** 选项卡，选择 **New（新建）** ![新建图标](SystemComposer_可视化架构建模_assets/icon-new-model.png)，然后选择 **Architecture（架构）** ![架构图标](SystemComposer_可视化架构建模_assets/icon-new-architecture.png)。

![新建的 System Composer 架构模型。](SystemComposer_可视化架构建模_assets/composition-editor-new.png)

**方法三：使用 MATLAB 命令创建并打开。** 在 MATLAB 命令行窗口中输入：

```matlab
archModel = systemcomposer.createModel("modelName");
systemcomposer.openModel("modelName");
```

参数 `modelName` 是新模型的名称。

创建后保存架构模型。在 **Simulation（仿真）** 选项卡中选择 **Save（保存）** ![保存图标](SystemComposer_可视化架构建模_assets/icon-save-model.png)。架构模型会保存为 `SLX` 文件。在 **Save As（另存为）** 对话框中，也可以选择将架构模型保存为 `MDL` 文件。

架构模型中的顶层架构承载系统的组合结构，同时允许定义该系统与其他系统之间的接口。组合结构可以理解为一份具有层次关系的部件清单：组件按照层级组织，并包含各自的接口和相互连接。在 **Composition Editor（组合编辑器）** 中编辑这一结构。

![System Composer 组合编辑器：顶部为工具条，左侧为模型浏览器，画布左侧为元素面板，中间为组合结构，右侧为属性检查器，底部为接口编辑器。](SystemComposer_可视化架构建模_assets/composition-editor.png)

图中的示例是一个运动控制架构：传感器从电机获取信息，将信息传给控制器；控制器处理这些信息后向电机发送控制信号，使电机按预期方式运动。可以先从这种粗略描述开始，随着设计推进，再逐步添加组件属性、接口定义和需求。

### 设置根架构模型的颜色（Set Color of Root Architecture Model）

要为架构模型的标题栏设置颜色，选择 **Format > Color（格式 > 颜色）**，打开调色板。所选颜色的优先级高于构造型（stereotype）定义的颜色。要移除该颜色，单击 **Unset Color（取消颜色设置）**。为一个架构设置颜色时，其父组件也会采用相同的颜色。

## 组件（Components）

组件是系统中可替换的组成部分，在架构中承担明确的功能。组件定义一个架构元素，例如某项功能、另一个系统、硬件、软件或其他概念实体。组件也可以是子系统或子功能。

System Composer 中的 **Component** 元素可以表示系统层次结构中任意一级的组件。它既可以表示包含许多子系统的主要系统组件，例如包含硬件和软件的控制器，也可以表示最底层的组件，例如用于消息传递的软件模块。组件以模块形式显示，是架构模型中可以分离出来并复用的部分。

组件之间通过端口接口和参数传递信息。可以使用[接口编辑器][ref05]和[参数编辑器][ref11]配置端口接口和参数。

可以通过以下方法向架构中添加组件：

- **直接绘制组件。** 在画布上按住鼠标左键拖动，绘制一个矩形。松开鼠标后会显示组件轮廓；选择 **Component** 模块选项，确认创建。
- **从元素面板创建组件。** 在元素面板中按住鼠标左键拖出一个组件，松开鼠标，确认创建。

![通过单击、拖动并确认来创建组件。](SystemComposer_可视化架构建模_assets/component-free-draw.png)

- **创建多个组件。** 在一个组件上按住鼠标右键并拖动，创建第二个组件。松开鼠标，确认创建。

![从已有组件拖出并确认第二个组件。](SystemComposer_可视化架构建模_assets/component-from-palette.png)

每个组件的名称在同一架构层级内必须唯一。组件刚创建时，其名称处于选中状态，可以直接输入名称。要修改组件名称，先单击组件，再单击其名称。

![名称为 Sensor 的组件。](SystemComposer_可视化架构建模_assets/component-name-1.png)

![单击 Sensor 组件名称后进入编辑状态。](SystemComposer_可视化架构建模_assets/component-name.png)

单击并拖动组件即可移动它。移动时可能出现蓝色辅助线，帮助组件与其他组件对齐。

![拖动 Component1，并利用蓝色辅助线与其他组件对齐。](SystemComposer_可视化架构建模_assets/component-move.png)

拖动组件的角可以调整大小：

1. 将鼠标指针停留在组件的一个角上，直到出现双向箭头。

   ![鼠标指针位于 Component1 的右下角。](SystemComposer_可视化架构建模_assets/component-resize-1.png)

2. 单击该角并按住鼠标拖动。要按比例调整组件大小，同时按住 **Shift** 键。

   ![拖动 Component1 的右下角以调整组件大小。](SystemComposer_可视化架构建模_assets/component-resize-2.png)

3. 组件达到所需大小后，松开鼠标。

要删除一个组件，单击组件并按 **Delete**。要删除多个组件，按住 **Shift** 选中这些组件，然后按 **Delete**。

### 添加组件图像（Add Component Image）

你可以向 System Composer 组件添加图像，使图像覆盖组件主体。组件图像可以让架构图更直观地表达设计内容。

1. 选择一个 [Component][ref13]、[Reference Component][ref14] 或 [Variant Component][ref15] 模块。
2. 在工具条中选择 **Format > Add Image（格式 > 添加图像）**，打开 **Add image（添加图像）** 对话框。
3. 单击 **Browse（浏览）**，从文件中选择图像。图像所在目录必须位于 MATLAB 搜索路径中。
4. 要让图像覆盖整个组件，选中 **Show image on entire component（在整个组件上显示图像）**。
5. 要移除组件图像，在工具条中选择 **Format > Edit Image > Remove Image（格式 > 编辑图像 > 移除图像）**。

应用组件图像后，可以通过[属性检查器（Property Inspector）][ref16]查看组件名称或端口名称。要以编程方式配置组件图像，使用 [`setMaskImage`][ref17] 函数。

有关在 [Component][ref13] 和 [Variant Component][ref15] 模块中使用组件图像的示例，请参阅[利用四旋翼飞行器架构设计计算续航时间][ref18]。

### 更改默认字体（Change Default Fonts）

可以修改顶层模型、组件标题、链接的模型或子系统名称、端口名称，以及子系统组件连线标签的默认字体设置。

要修改字体，打开 **Font Styles（字体样式）** 对话框。在 System Composer 工具条的 **Format（格式）** 选项卡中，单击 **Font（字体）** 按钮 ![字体图标](SystemComposer_可视化架构建模_assets/font-icon.png) 旁边的向下箭头，选择 **Fonts for Model（模型字体）**。

### 更改组件标题颜色（Change Component Header Color）

选择 **Format > Color（格式 > 颜色）** 打开调色板，即可修改组件标题栏的颜色。所选颜色的优先级高于构造型的颜色。要移除该颜色，单击 **Unset Color（取消颜色设置）**，或选择默认的白色。

## 端口（Ports）

端口是组件或架构上的一个节点，表示它与环境的交互点。信息可以通过端口在组件或系统之间流入和流出。**组件端口**是组件与其他组件交互的位置。**架构端口**位于系统边界上；这里的边界既可以位于某个组件内部，也可以是整个架构模型的边界。根架构的边界由其端口定义。

例如，传感器可以具有与电机和控制器通信的数据端口。传感器的输入端口接收来自电机的数据，输出端口则向控制器传送数据。可以通过定义接口来指定数据值，方法见[定义组件之间的端口接口][ref19]。

> **注意：接口继承**  
> 新创建端口的接口规范默认为 `<inherit>`，以支持架构设计早期接口尚未确定的情况。在构建架构时，如果在另一个端口上指定了接口，`<inherit>` 机制会将该接口传播到其他相连的端口。接口规范不会自动跨越模型引用和子系统引用传播。

### 添加组件端口（Add Component Port）

通过定义具有方向的接口端口，表示组件之间的关系。为了组织架构图，可以将端口放置在组件任意一条边的任意位置。

1. 将鼠标指针停留在组件的一条边上。此时会出现一个 **+** 号和端口轮廓。

   ![在 Sensor 组件右侧添加端口；确认创建前，端口显示为浅蓝色轮廓。](SystemComposer_可视化架构建模_assets/port-create-1.png)

2. 单击端口轮廓，会出现 **Input（输入）**、**Output（输出）** 和 **Physical（物理）** 端口选项。

   ![确认创建端口前显示的端口类型选项。](SystemComposer_可视化架构建模_assets/port-create-2.png)

3. 选择 **Output（输出）**，确认创建该端口。此时也可以为端口命名。

   ![确认创建后，输出端口命名为 OutBus。](SystemComposer_可视化架构建模_assets/port-complete.png)

输出端口以 ![输出端口图标](SystemComposer_可视化架构建模_assets/icon-out.png) 图标显示，输入端口以 ![输入端口图标](SystemComposer_可视化架构建模_assets/icon-in.png) 图标显示，物理端口以 ![物理端口图标](SystemComposer_可视化架构建模_assets/icon-physical.png) 图标显示。

> **注意：物理端口**  
> 物理端口没有方向，用于采用物理网络方法的 Simscape™ 物理建模。有关使用 Simscape 定义组件行为的更多信息，请参阅[使用 Simscape 实现组件行为][ref20]。

端口创建后，可以使用方向键将其移动到组件的任意一条边上。

### 添加架构端口（Add Architecture Port）

也可以为包含组件的架构创建端口。位于边界上的这些系统端口承载该系统与其他系统之间的接口。根位于架构层次结构的顶端。根架构具有包围目标系统的边界，该边界由其架构端口定义。根架构的系统边界包围整个架构模型，你可以添加架构端口，定义跨越该边界的接口。

将鼠标指针停留在系统外框的任意一条边上，出现 **+** 号时单击。在左边单击可创建输入端口，在右边单击可创建输出端口。

![包含三个组件的架构模型，边界上具有一个 InBus 架构端口和一个 OutBus 架构端口。](SystemComposer_可视化架构建模_assets/port-system.png)

要为架构端口设置颜色，选择 **Format > Color（格式 > 颜色）**，打开调色板。所选颜色的优先级高于构造型的颜色。要移除该颜色，单击 **Unset Color（取消颜色设置）**，或选择默认的黑色。为架构端口设置颜色时，其父组件上对应的端口也会采用相同的颜色。

### 命名、移动和删除端口（Name, Move, and Delete Ports）

每个 [Port][ref21] 模块创建时都会带有名称。要修改名称，单击名称并进行编辑。

![选中端口名称后可以编辑。](SystemComposer_可视化架构建模_assets/port-name.png)

同一组件的各个端口必须具有不同的名称。

可以将端口移动到组件的任意一边。选中端口，然后使用方向键移动。同一边上各端口的间距会自动调整。同一条边上可以同时存在输入端口和输出端口。

要删除端口，选中该端口并按 **Delete**。

### 更改端口类型（Change Port Type）

要更改端口类型，右键单击端口，选择 **Change port action（更改端口动作）**，然后选择 **Input（输入）**、**Output（输出）** 或 **Physical（物理）**。

也可以在[属性检查器][ref16]中更改端口类型。在 **Interface（接口）** 部分的 **Action（动作）** 列表中，选择适当的端口动作。

使用 `systemcomposer.arch.PortDirection` 枚举，也可以更改端口动作。在下面的示例中，`compPort` 表示一个 [`systemcomposer.arch.ComponentPort`][ref22] 对象，`archPort` 表示一个 [`systemcomposer.arch.ArchitecturePort`][ref23] 对象。每个对象都使用 `Direction` 属性标识端口动作。

```matlab
compPort.Direction = systemcomposer.arch.PortDirection.Input
compPort.Direction = systemcomposer.arch.PortDirection.Physical
 
archPort.Direction = systemcomposer.arch.PortDirection.Output
archPort.Direction = systemcomposer.arch.PortDirection.Physical
```

对于 **Input** 或 **Output** 端口，可以右键单击端口，在上下文菜单中选择 **Conjugate port（共轭端口）**，将其转换为共轭类型：**Input** 端口转换为 **Output** 端口，**Output** 端口转换为 **Input** 端口。

### 切换端口标签位置（Switch Port Label Position）

选择一个带有端口的组件，然后选择 **Format > Block Layout > Switch Port Label（格式 > 模块布局 > 切换端口标签）**。选择 **Position Inside Component（放置在组件内部）** 或 **Position Outside Component（放置在组件外部）**，切换端口标签的位置。

## 连接器（Connectors）

连接器是用于连接端口的线，描述信息如何在组件或架构之间流动。连接器允许两个组件交互，但本身不定义交互的具体性质。通过在端口上设置接口，定义组件如何交互。

连接器以可视化方式表示从输出端口到输入端口的数据流。例如，从电机到传感器的连接可以传递位置信息。

### 连接现有端口（Connect Existing Ports）

通过拖动连线连接两个端口：

1. 单击其中一个端口。
2. 按住鼠标，将连线拖动到另一个端口。
3. 在目标端口处松开鼠标。黑色连线表示连接已完成；如果连接未完成，则显示红色点线。

![拖动连线连接两个已有端口。](SystemComposer_可视化架构建模_assets/connector-middle.png)

拖动操作可以沿两个方向进行：从输入端口拖向输出端口，或从输出端口拖向输入端口。不能连接方向相同的端口。

**架构端口与组件端口之间的连接使用标签显示，而不是使用连线显示。**

![Motor 组件与架构边界端口的连接以标签形式显示。](SystemComposer_可视化架构建模_assets/connector-system.png)

### 连接没有端口的组件（Connect Components Without Ports）

要同时快速创建端口和连接，可以从一个组件的边缘拖出连线，连接到另一个组件的边缘。连接方向取决于使用了组件的哪一条边：左边和上边被视为输入，右边和下边被视为输出。也可以从已有端口拖动到组件边缘，执行这一操作。

![将已有端口连接到没有端口的组件时，自动创建端口。](SystemComposer_可视化架构建模_assets/port-automatic.png)

被视为输入的边只能与被视为输出的边建立连接。例如，上边被视为输入，因此不能将一个组件的上边连接到另一个组件的上边，除非其中一边已经具有输出端口。

### 对连接创建分支（Branch Connections）

通过为连接创建分支，可以将一个输出端口连接到多个输入端口。要创建分支，右键单击已有连接，按住鼠标并拖动到一个输入端口。松开鼠标，确认新连接。

![在已有连接上按住鼠标右键拖动，创建连接分支。](SystemComposer_可视化架构建模_assets/connector-branch.png)

### 通过连接创建新组件（Create New Components Through Connections）

如果从一个输出端口开始拖动连线，并在没有目标端口的位置松开鼠标，会暂时显示一个新组件。单击该组件，确认创建。

![从输出端口拖出连接后出现浅蓝色候选组件，单击以确认创建。](SystemComposer_可视化架构建模_assets/component-automatic.png)

### 自动连接同名端口（Auto-Connect Ports with Same Name）

可以使用工具条中的 **Smart Connect（智能连接）** 菜单按钮，自动连接模型画布上不同组件之间的同名端口。有关智能连接及其各项选项的信息，请参阅[使用自定义策略自动连接端口][ref24]和 [`smartConnect`][ref25] 函数。

也可以通过以下步骤连接同名端口：

- 选中包含连接起点端口的组件，使其高亮显示。
- 按住 **Ctrl**，右键单击另一个组件，按照同名规则将端口连接到该组件的端口。

更多信息请参阅[连接模块][ref26]。

要通过编程接口连接同名端口，使用 [`connect`][ref27] 函数，并将 `Rule` 名称-值参数指定为 `"name"`。要定位端口和组件，可以结合使用 [`lookup`][ref28] 与 [`getQualifiedName`][ref29] 函数。

### 更改重叠连接的交叉样式（Change Line Crossing Style for Overlapping Connections）

在复杂架构图中，连接器可能发生交叉重叠。可以通过选择其他连线交叉样式，提高图表的可读性。

选择 **Modeling > Environment > Simulink Settings（建模 > 环境 > Simulink 设置）**。在 **Simulink Settings** 中选择 **Editor（编辑器）**，然后选择 **Line crossing style（连线交叉样式）**。默认的交叉样式为 **Tunnel（隧道）**，如下图所示。

![Tunnel：隧道式连线交叉样式。](SystemComposer_可视化架构建模_assets/connector-line-crossing-tunnel.png)

另一种可选样式为 **Line Hop（跨线）**，如下图所示。

![Line Hop：跨线式连线交叉样式。](SystemComposer_可视化架构建模_assets/connector-line-crossing-hop.png)

有关连线交叉样式参数的更多信息，请参阅 [Line crossing style（连线交叉样式）][ref30]。

## 另请参阅（See Also）

### 函数（Functions）

[`createModel`][ref31] | [`addComponent`][ref32] | [`addPort`][ref33] | [`connect`][ref27] | [`setMaskImage`][ref17] | [`exportModel`][ref02] | [`importModel`][ref01]

### 模块（Blocks）

[Component][ref13]

### 主题（Topics）

- [分解和复用组件][ref34]
- [在封装中添加图像][ref35]
- [定义组件之间的端口接口][ref19]
- [在 System Composer 中使用参数编辑器定义参数][ref36]
- [使用序列图描述系统行为][ref37]
- [使用 System Composer 工作流仿真移动机器人][ref38]

[ref01]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.importmodel.html
[ref02]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.exportmodel.html
[ref03]: https://jp.mathworks.com/help/systemcomposer/ug/import-and-export-architectures.html
[ref04]: https://jp.mathworks.com/help/systemcomposer/ug/build-architecture-command-line.html
[ref05]: https://jp.mathworks.com/help/systemcomposer/ref/interfaceeditor.html
[ref06]: https://jp.mathworks.com/help/simulink/slref/architecturaldataeditor.html
[ref07]: https://jp.mathworks.com/help/simulink/slref/simulink.dictionary.architecturaldata.html
[ref08]: https://jp.mathworks.com/help/simulink/ug/manage-shared-interfaces-and-data-types-using-editor.html
[ref09]: https://jp.mathworks.com/help/simulink/ug/programmatically-manage-shared-interfaces-and-data-types-for-architecture-models.html
[ref10]: https://jp.mathworks.com/help/systemcomposer/gs/system-composer-concepts.html
[ref11]: https://jp.mathworks.com/help/systemcomposer/ref/parametereditor.html
[ref12]: https://jp.mathworks.com/help/systemcomposer/ug/choose-architecture-template-for-system-design.html
[ref13]: https://jp.mathworks.com/help/systemcomposer/ref/component.html
[ref14]: https://jp.mathworks.com/help/systemcomposer/ref/referencecomponent.html
[ref15]: https://jp.mathworks.com/help/systemcomposer/ref/variantcomponent.html
[ref16]: https://jp.mathworks.com/help/simulink/slref/propertyinspector.html
[ref17]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.arch.architecture.setmaskimage.html
[ref18]: https://jp.mathworks.com/help/systemcomposer/ug/calculate-endurance-quadcopter.html
[ref19]: https://jp.mathworks.com/help/systemcomposer/ug/define-port-interfaces-between-components.html
[ref20]: https://jp.mathworks.com/help/systemcomposer/ug/describe-component-behavior-using-simscape.html
[ref21]: https://jp.mathworks.com/help/systemcomposer/ref/port.html
[ref22]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.arch.componentport.html
[ref23]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.arch.architectureport.html
[ref24]: https://jp.mathworks.com/help/systemcomposer/ug/smart-connect-ports.html
[ref25]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.arch.architecture.smartconnect.html
[ref26]: https://jp.mathworks.com/help/simulink/ug/connect-blocks.html
[ref27]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.arch.architecture.connect.html
[ref28]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.arch.model.lookup.html
[ref29]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.arch.element.getqualifiedname.html
[ref30]: https://jp.mathworks.com/help/simulink/slref/simulinksettings.html#mw_34582f94-4e82-48d8-8821-fb1a622d1e86
[ref31]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.createmodel.html
[ref32]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.arch.architecture.addcomponent.html
[ref33]: https://jp.mathworks.com/help/systemcomposer/ref/systemcomposer.arch.architecture.addport.html
[ref34]: https://jp.mathworks.com/help/systemcomposer/ug/decompose-and-reuse-components.html
[ref35]: https://jp.mathworks.com/help/simulink/ug/adding-images-in-masks.html
[ref36]: https://jp.mathworks.com/help/systemcomposer/ug/top-down-parameters-workflow.html
[ref37]: https://jp.mathworks.com/help/systemcomposer/ug/define-sequence-diagrams.html
[ref38]: https://jp.mathworks.com/help/systemcomposer/ug/mobile-robot-workflow.html
