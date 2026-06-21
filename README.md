<div align="center">
  <h1>🤖 Multi-Agent Collaboration System</h1>
  <p><strong>多Agent自主协作系统</strong></p>
  <p>Build autonomous AI agent teams in Codex with role separation, cross-session messaging, and task pipeline automation.</p>
  <p>在 Codex 中搭建自主协作的多 Agent 团队——角色分离、跨线程通信、任务流水线自动化。</p>
</div>

---

## 📖 Overview / 概述

This project provides a **Codex Skill** that enables you to create and manage multi-agent teams within Codex's environment. Inspired by the tutorial video *"让Codex多Agent自主协同"* by 珍妮丁丁说AI (Jenny Ding Ding Talks AI), it implements the three-role architecture:

- **Manager Agent** — Task orchestration, decomposition, and decision making
- **Execution Agents** — Specialized planning and development roles
- **Review/QA Agent** — Independent validation and quality gate

The system leverages Codex's built-in thread management tools (`create_thread`, `send_message_to_thread`, etc.) to enable autonomous agent collaboration.

---

## 🎯 Features / 特性

- **Role Separation** — Agents with distinct, non-overlapping responsibilities
- **Cross-Session Messaging** — Agents communicate via Codex thread messages
- **Task Pipeline** — Plan → Execute → Review workflow automation
- **Work Logging** — Every action is logged for traceability
- **Identity Registration** — Each agent knows its role and collaboration rules
- **Context Isolation** — Each agent runs in its own thread, avoiding context pollution

---

## 🏗️ Architecture / 架构

```
┌──────────────────────────────────────────────────────────┐
│                    Manager Agent                         │
│          (Orchestration · Decision · Archive)             │
└────────┬─────────────────────────────────────┬───────────┘
         │ [Task]                              │ [Report]
         ▼                                     ▲
┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐
│  Planning Agent  │──▶│ Development Agent │──▶│   QA/Review      │
│  Requirements    │   │  Implementation  │   │   Agent           │
│  Spec Writing    │   │  Coding · Testing │   │   Validation      │
└──────────────────┘   └──────────────────┘   └──────────────────┘
                              ▲                      │
                              │   [Revise]           │
                              └──────────────────────┘
```

### Role Descriptions / 角色说明

| Role | Type | Responsibility | Principle |
|------|------|---------------|-----------|
| **Manager** | Management | Plan, decompose, dispatch, decide | Orchestrates only — never implements |
| **Planning Agent** | Execution | Requirements, PRD, feature specs | Plans only — never codes |
| **Development Agent** | Execution | Technical design, coding, testing | Implements only — never reviews own work |
| **QA/Review Agent** | Validation | Test cases, code review, quality gate | Reviews independently — never implements |

**Golden Rule / 核心原则:** Never let the same agent be both player and referee.

---

## 🚀 Quick Start / 快速开始

### Prerequisites / 前提条件

- [Codex](https://codex.so) desktop app
- Git (for installation)

### Installation Option 1: Install as Codex Skill (Recommended)

1. **Clone this repository:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/multi-agent-collab.git
   ```

2. **Install the skill:**
   ```bash
   # Copy the skill folder to Codex's skills directory
   cp -r multi-agent-collab/skill ~/.codex/skills/multi-agent-collab
   ```
   
   Or use the Codex skill installer if available:
   ```
   Codex will auto-discover the skill at ~/.codex/skills/multi-agent-collab
   ```

3. **Restart Codex** — the skill is now ready to use.

### Installation Option 2: Manual Setup

Follow the workflow below in any Codex conversation.

---

## 💡 Usage / 使用方法

### Step 1: Create Agent Threads

Use `create_thread` in Codex to create agent sessions:

```javascript
// Example: Codex command to create a Planning Agent
create_thread({
  target: { type: "projectless" },
  prompt: `You are the Planning Agent...`  // See skill/references/agent-templates.md
})
```

Each agent thread gets:
- A clear **role identity** (from `references/agent-templates.md`)
- **Collaboration rules** (from `references/collaboration-rules.md`)
- **Work log** requirements for traceability

### Step 2: Configure the Team

```javascript
// Rename threads for visibility
set_thread_title("📋 Planning Agent")
set_thread_title("⚙️ Development Agent")
set_thread_title("🔍 QA/Review Agent")

// Pin threads for quick access
set_thread_pinned(true)
```

### Step 3: Start Collaboration

```javascript
// As the Manager, send a task to the Planning Agent
send_message_to_thread("planning-agent-id", `
[Task] Design a user authentication module

Requirements:
- Email + password login
- JWT token-based auth
- Password reset flow
...
`)
```

The task flows automatically through the pipeline:
1. Planning Agent analyzes and produces specs
2. Development Agent implements
3. QA Agent reviews and reports back

### Step 4: Monitor Progress

```javascript
// Check agent status
read_thread("agent-id")

// List all active agents
list_threads()
```

---

## 📁 Project Structure / 项目结构

```
multi-agent-collab/
├── README.md                          # This file (文档)
├── LICENSE                            # MIT License
│
├── skill/                             # Codex Skill (可安装的Skill)
│   ├── SKILL.md                       # Skill instructions (核心指令)
│   ├── agents/
│   │   └── openai.yaml                # UI metadata
│   ├── references/
│   │   ├── agent-templates.md         # Agent identity registration templates
│   │   ├── collaboration-rules.md     # Collaboration protocols & best practices
│   │   └── workflow-examples.md       # Complete workflow examples
│   └── scripts/│├──script /
│       └── setup-agents.py            # Quick setup script│├──setup-agents.py #快速安装脚本
│
└── team-config/                       # Team configuration example──Team -config/ #团队配置示例
    ├── .agent-registry.md             # Agent registry (身份注册表)
    └── .agent-team-summary.md         # Team setup summary└──.agent-team-summary。团队设置总结
```

---

## 📋 Collaboration Protocol / 协作协议

### Message Types   ###消息类型

| Type | From → To | Purpose |   |类型|从&rarr；到|用途|
|------|-----------|---------|
| `[Task]` | Manager → Any | Assign new work |
| `[Plan]`| ' [Task] ' | Manager &rarr； Any |分配新工作| | Planner → Developer | Send specification |
| `[Submit]`| ' [Plan] ' | Planner &rarr； Developer |发送规范| | Developer → QA | Request review |
| `[Report]`| '[提交]' |开发人员&rarr； QA |请求审查| | QA → Manager | Report results |
| `[Revise]`| '[报告]' | QA &rarr；经理|报告结果| | QA → Developer | Request changes |
| `[Announce]`| '[修订]' | QA &rarr；开发人员|请求更改| | Manager → All | Team broadcast |
| `[Done]`| '[宣布]' |经理&rarr；所有|团队广播| | Any → Manager | Task completed |

### Message Format   ###消息格式

```
[Type] TaskID: Description[类型]TaskID：描述

Context:   背景:
- Background information   -背景资料
- Previous decisions   -以前的决定
- Constraints   ——约束

Expected Output:   预期的输出:
- Deliverable description-可交付物描述
- Format   - - -格式
- Deadline (if any)   -截止日期（如有）
```

---

## 🌐 Multi-Language Support / 多语言支持

- The **Skill** system prompts and references are primarily in Chinese (中文), optimized for the Codex ecosystem- **Skill**系统提示和参考主要使用中文（中文），并针对法典生态系统进行了优化
- This **README** is bilingual (Chinese + English) for global accessibility-本**自述文件**为中英文双语，方便全球查阅
- Skill descriptions in `openai.yaml` are bilingual for auto-triggering- openai中的技能描述。Yaml是自动触发的双语

## 🙏 Credits / 致谢

- **珍妮丁丁说AI** — Original tutorial video: *"让Codex多Agent自主协同"*- **珍妮丁丁说AI** — Original tutorial video: *"让Codex多Agent自主协同"   "让Codex多Agent自主协同"*
  - [抖音视频](https://v.douyin.com/3kYTJsPNUdY/)
- **Codex** — The AI coding platform that makes multi-agent collaboration possible- **Codex   食典委** &mdash；使多代理协作成为可能的AI编码平台

---

<div align="center">   <div align   对齐="center"   "center">
  <p>Built with ❤️ for the Codex community</p><p>；为食典界使用❤️构建<；/p>；
  <p><a href="https://codex.so">Codex</a> · <a href="https://v.douyin.com/3kYTJsPNUdY/">Original Tutorial</a></p><p><a href="https://codex.so">Codex</   & lt;a> · <a href="https://v.douyin.com/3kYTJsPNUdY/">Original Tutorial</   & lt;a></   & lt;p>
</div>   < / div>
