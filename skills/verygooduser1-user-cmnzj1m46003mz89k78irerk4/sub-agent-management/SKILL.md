---
name: Sub-Agent Management
description: Dictionary of available agents and sub-agent delegation patterns
---

# Sub-Agent Management

## Agent Dictionary

Last updated: 2026-05-02

### Published Agents

#### Embla (Meta-Agent / Agent Creator)
- **ID**: `agent-cmmk23e7b000dsh9k75x7h3j6`
- **Type**: claude
- **Developer**: xmz.ai
- **Description**: 引导完成 agent 创建全流程的元代理。通过交互式对话帮助设计 agent 架构、选组件（技能、MCP 工具、hooks），生成所有必要文件并注册。
- **When to use**: 需要创建新 agent 时。

#### Syn (Agent Tester)
- **ID**: `agent-cmnfv7wbv006geh9k16761lhn`
- **Type**: claude
- **Developer**: xmz.ai
- **Description**: 严格测试 draft agent，评估其是否达到设计目标，给出诚实裁定。
- **When to use**: 需要测试和验证 draft agent 质量时。

#### Claude Code
- **ID**: `agent-cmmk1yzmw0008sh9kfjpg8lwq`
- **Type**: claude
- **Developer**: xmz.ai
- **Description**: 内置 Claude Code agent，适用于通用软件开发任务。
- **When to use**: 需要执行编码、调试、代码分析等软件工程任务时。

#### Codex
- **ID**: `codex`
- **Type**: codex
- **Developer**: xmz.ai
- **Description**: 内置 Codex agent，适用于通用软件任务。
- **When to use**: 作为 Claude Code 的替代选项。

#### Planner
- **ID**: `planner`
- **Type**: claude
- **Developer**: xmz.ai
- **Description**: 内置编排 agent，用于群聊和多 agent 协作任务。
- **When to use**: 需要协调多个 agent 协作完成复杂任务时。

### Draft Agents

#### Web Design Agent
- **ID**: `draftagent-cmobb5nqn00h1z39k9w041dq5`
- **Type**: claude
- **Description**: Web 设计专家，使用 React/Next.js + Tailwind CSS 生成完整网站，构建并启动 dev server 以便实时预览，还能调研当前设计趋势。
- **When to use**: 需要设计和构建网站/网页时。

---

## Workflow

1. 收到任务后，先查阅本文件确认是否有合适的 agent
2. 如果没有合适的，调用 `mcp__agentrix__list_agents` 刷新列表
3. 仍然找不到？用 Embla 创建新 agent
4. 通过 `mcp__agentrix__create_task` 委派任务
5. 通过 `mcp__agentrix__emit_to_task` 与子任务交互

**Note:** 有新 agent 创建或发现时，及时更新本文件。
