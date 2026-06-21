# Collaboration Rules and Best Practices

## Core Principles

### Role Separation
- **Manager**: Orchestrates, never implements
- **Planner**: Plans, never codes
- **Developer**: Implements, never reviews own work
- **QA**: Reviews independently, never implements
- Never let one agent be both player and referee

### Message Protocol
Every cross-agent message should follow this structure:
```
[MessageType] TaskID: description

Context:
- Background information
- Relevant previous decisions
- Technical constraints

Expected Output:
- What the receiving agent should produce
- Format and delivery method
- Deadline if applicable
```

### Message Types
| Type | From | To | Purpose |
|------|------|----|---------|
| [Task] | Manager | Any | Assign new work |
| [Plan] | Planner | Developer | Send specification |
| [Submit] | Developer | QA | Request review |
| [Report] | QA | Manager | Report results |
| [Revise] | QA | Developer | Request changes |
| [Announce] | Manager | All | Team broadcast |

### Thread Management

**Creating agents:**
- Use create_thread with projectless target
- Rename with set_thread_title
- Pin with set_thread_pinned(true)

**Monitoring agents:**
- Use read_thread to check latest output
- Use list_threads to see all active agents

**Cleaning up:**
- Archive completed threads with set_thread_archived(true)
- Keep threads for reference until project is done

### Best Practices

1. Start with 3 agents minimum: Manager, Developer, QA
2. Give clear, specific instructions with concrete details
3. Use work logs for traceability
4. Check agent progress between steps
5. Handle failures with specific revision requests
6. Avoid context pollution - keep each thread focused
