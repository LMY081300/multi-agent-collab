---
name: multi-agent-collab
description: "Set up and manage multi-agent collaboration teams in Codex. Use this skill when the user wants to: (1) create multiple AI agents with specific roles working together, (2) set up a task pipeline with planning, execution, and validation phases, (3) use Codex thread features for cross-agent communication, or (4) build an autonomous agent team for complex projects."
---

# Multi-Agent Collaboration

Capabilities-based structure for setting up autonomous agent teams.

## Core Capabilities

### 1. Agent Team Setup

Create specialized agent threads with distinct roles, identity registration, and collaboration rules.

**Role separation principle (from the tutorial):** Always separate management, execution, and validation roles. Never let the same agent be both "player and referee."

**Standard team roles:**

| Role | Type | Responsibility |
|------|------|---------------|
| Manager Agent | Management | Task planning, decompose, dispatch, decision making |
| Planning Agent | Execution | Requirements analysis, feature planning, PRD writing |
| Development Agent | Execution | Technical design, coding, unit testing |
| QA/Review Agent | Validation | Test cases, functional verification, code review |

**Setup workflow:**

1. Call create_thread for each agent with a projectless target
2. Each thread gets a role definition message as its initial prompt (see references/agent-templates.md)
3. Rename threads with set_thread_title to make roles visible
4. Pin threads with set_thread_pinned(true) for easy access
5. Read each thread to confirm it responded with its ready confirmation

### 2. Cross-Session Messaging

Use send_message_to_thread to enable agents to communicate across threads.

**Message protocol:**

| Direction | Format | Purpose |
|-----------|--------|---------|
| Manager → Planner | [Task] <description> | Assign new work |
| Planner → Developer | [Plan] <spec + tasks> | Send planning doc |
| Developer → QA | [Submit] <deliverable> | Submit for review |
| QA → Manager | [Report] <results> | Report findings |
| Manager → All | [Announce] <update> | Team announcements |

Use ead_thread to check agent status and list_threads to see all active agents.

### 3. Thread Management

Use Codex's thread tools to manage the agent team:

- **create_thread** - Create new agent threads (use projectless target for general tasks)
- **send_message_to_thread** - Send cross-agent messages (omit model to keep current settings)
- **ead_thread** - Check agent progress and outputs
- **list_threads** - See all running agent threads
- **set_thread_title** - Give agents human-readable names
- **set_thread_pinned** - Keep important threads visible in the sidebar
- **set_thread_archived** - Archive completed agent threads
- **ork_thread** - Fork an agent's thread when it needs context from a previous conversation

### 4. Work Logging

Each agent maintains a work log for traceability. Include a log section in every message sent to an agent:

`
Log: [time] AgentName: operation details
`

Example log entries:
- "Log: Received task: develop user login feature"
- "Log: Starting implementation of login module"
- "Log: Implementation complete, submitting for review"
- "Log: Review passed, all tests green"

### 5. Full Pipeline Flow

Complete workflow for a task:

`
1. Manager receives user request
2. Manager sends [Task] to Planning Agent via send_message_to_thread
3. Planning Agent analyzes, writes PRD, sends [Plan] to Developer
4. Developer implements, sends [Submit] to QA Agent
5. QA Agent tests and sends [Report] back to Manager
6. If QA fails: Manager sends task back to Developer with notes
7. If QA passes: Manager archives the task
`

## Triggering the Skill

The skill is triggered when the user mentions:
- "multi-agent" or "多Agent"
- "agent team" or "agent collaboration"
- "create agents" or "设置分身"
- "task pipeline" or "协作流程"
- Any request that involves multiple AI agents working together

## Resources

### scripts/
See scripts/README.md for the agent setup helper script.

### references/
- **references/agent-templates.md**: Agent identity registration templates for each role
- **references/collaboration-rules.md**: Detailed collaboration protocols and best practices
- **references/workflow-examples.md**: Complete workflow examples with sample prompts
