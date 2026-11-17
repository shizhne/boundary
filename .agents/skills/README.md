# VE25 工作区级 Skill 目录

> 生成记录：2026-09-03T15:46:33+08:00；目的：定义 VE25 工作区级 skill 的存放与注册规则；依据：用户允许将 skill 库放在 `sp` 外层、`D:\666\VE25\AGENTS.md`；生成方式：Codex 初始化；适用范围：`D:\666\VE25\.agents\skills`；不确定性：当前尚未批准任何 SP 专用 skill。

本目录只保存 VE25 工作区级、跨目录编排型 skill。当前没有已启用的项目专用 skill。

新增 skill 时必须：

1. 使用 `.agents/skills/<skill-name>/SKILL.md`。
2. 在 `D:\666\VE25\codex\kb\20_skill_registry.md` 登记唯一名称、canonical path、scope、触发条件、依赖和状态。
3. 只写执行所需的最小程序性规则；工程事实、参数、接口和模型结构仍指回 `sp`。
4. 未经用户明确授权，不在 `sp` 内创建 Codex skill、过程脚本或配置副本。
