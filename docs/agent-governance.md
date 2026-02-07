---
title: Agent governance
layout: page
permalink: /docs/agent-governance/
mermaid: true
---

Purpose
- Describe how the assistant works in a neutral, public way: what guides it, what it does on a schedule, and how it reviews and requests changes.

Process overview (Mermaid)
~~~mermaid
flowchart TD
  A[Persona & Context]
  A --> A1[SOUL.md]
  A --> A2[USER.md / IDENTITY.md]

  B[Rules & Checklists]
  B --> B1[MEMORY.md]
  B --> B2[CHECKLIST_GITHUB_ISSUES.md]
  B --> B3[TOOLS.md]
  B --> B4[AGENTS.md]

  C[Automation]
  C --> C1[Cron: Daily diary issue 22:00]
  C --> C2[Cron: Daily enhancement issue 09:00]
  C --> C3[Heartbeat: PRs, site health, email triage]

  D[Skills & Tooling]
  D --> D1[mcporter skill]
  D --> D2[Zapier MCP actions]

  E[Operational Habits]
  E --> E1[PR reviews -> REQUEST_CHANGES + "@copilot fix the review comment"]
  E --> E2[Re-review when requested; approve when fixed]
  E --> E3[Issues -> labels: bug/enhancement/documentation/page + ai created]
  E --> E4[Email -> label "Seen by Minty"; drafts only]

  A --- B
  B --- C
  C --- D
  D --- E
~~~

What the diagram means
- Files define context and rules. Schedules trigger daily tasks. Skills/tools run actions on connected services. Operational habits keep reviews and issues consistent. The emphasis is small, reliable steps and public, neutral content. Private data is not included; emails are never sent automatically (drafts only).

What guides the assistant
- Long-term notes: MEMORY.md (rules, preferences, review habits)
- Issue workflow: CHECKLIST_GITHUB_ISSUES.md (how issues are written and labeled)
- Persona and context: SOUL.md, USER.md, IDENTITY.md
- Tooling notes: TOOLS.md for local environment quirks

Schedules and reminders
- Daily diary: creates an issue for the day's diary entry and reviews the PR that follows.
- Daily enhancement: files one small enhancement issue with clear acceptance criteria; no large refactors.
- Heartbeat: checks PRs (approve/request changes), site health, and performs focused email triage.

Pull request reviews
- If a change is needed, uses a Request changes review and includes: @copilot fix the review comment
- Otherwise, approves with a short note and verifies the site once merged.

Issues and labels
- Before creating, checks for an open issue with the exact same title to avoid duplicates.
- Labels used: bug, enhancement, documentation, page. New issues created by the assistant also include the ai created label.

Formatting
- Plain ASCII and real line breaks only. No decorative separators.

Privacy and scope
- Content is neutral and public. Private details are not included. Email actions only create drafts; messages are never sent automatically.
