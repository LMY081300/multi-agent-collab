# Workflow Examples

## Example 1: Simple Feature Development

**User request:** "Add a dark mode toggle to the settings page"

### Step 1: Manager analyzes and sends to Developer
```
[Task] DARK-001: User requested dark mode toggle on settings page.
- Feature: Toggle switch in Settings > Appearance
- Frontend only, no backend changes needed
- Must persist preference in localStorage
```

### Step 2: Developer implements and submits to QA
```
[Submit] DARK-001: Dark mode toggle implemented
- Added toggle switch component
- Created CSS variable system for theming
- Implemented localStorage persistence
```

### Step 3: QA reviews and reports to Manager
```
[Report] DARK-001: Review complete
- Toggle works correctly
- Theme persists across page reloads
- All accessibility checks passed
- Status: PASS
```

### Step 4: Manager closes
```
[Done] DARK-001: Approved and merged
```

---

## Example 2: Full Pipeline with Planner

**User request:** "Build a data dashboard for monitoring server health"

### Manager to Planner
```
[Task] DASH-001: Design server health monitoring dashboard
Requirements:
- Real-time CPU/memory/disk metrics
- Historical trend charts
- Alert configuration panel
- Tech stack: React + Node.js + PostgreSQL
```

### Planner to Developer
```
[Plan] DASH-001: Dashboard specification
Modules:
1. Metrics collector (Python, cron-based)
2. REST API (Node.js + Express)
3. Frontend dashboard (React + Recharts)
4. Alert engine (Node.js scheduled tasks)
Priority: API > Frontend > Collector > Alerts
```

### Developer to QA
```
[Submit] DASH-001: Core modules implemented
- API endpoints: /api/metrics, /api/alerts
- Frontend: dashboard with real-time charts
- Collector: system metrics polling script
```

### QA to Developer (if fails)
```
[Revise] DASH-001: Issues found
- API returns 500 for empty metrics table
- Frontend chart does not handle null values
- Collector missing error handling for Windows
```

### QA to Manager (if passes)
```
[Report] DASH-001: All tests pass
- Status: PASS
```
