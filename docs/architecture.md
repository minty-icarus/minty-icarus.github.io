---
title: Architecture
layout: page
permalink: /docs/architecture/
mermaid: true
---

System flow of Telegram <-> OpenClaw (Minty) <-> Zapier MCP <-> Gmail/Todoist/Drive/GitHub.

```mermaid
flowchart LR
  Anniek[[Anniek (you)]]
  TG[Telegram Bot (Clawbot)]
  OC[OpenClaw Minty Agent]
  CRON[OpenClaw Cron]
  MC[mcporter -> Zapier MCP]
  GMAIL[Gmail]
  TODO[Todoist]
  DRIVE[Google Drive/Docs]
  GITHUB[GitHub (PRs/Pages)]

  Anniek <--> TG
  TG --> OC
  CRON --> OC

  OC -- calls via mcporter --> MC
  MC --> GMAIL
  MC --> TODO
  MC --> DRIVE
  MC --> GITHUB

  %% Email routines
  CRON -. hourly 08-22, 2h overlap (label delta) .-> OC
  OC -->|find inbox last 2h| GMAIL
  OC -->|label new: "Seen by Minty"| GMAIL
  OC -->|summary -> Telegram| TG
  OC -->|auto-draft if actionable| GMAIL

  %% Quiet-hours (08:00)
  CRON -. 08:00 daily .-> OC
  OC -->|summarize 22:00-08:00| GMAIL
  OC -->|archive >1d (exclude starred/unread)| GMAIL
  OC -->|summary -> Telegram| TG

  %% Diary (22:00)
  CRON -. 22:00 daily .-> OC
  OC -->|create/append YYYY-MM-DD from Template| DRIVE
  OC -->|link -> Telegram| TG

  %% Tasks
  OC -->|create/update tasks| TODO

  %% Site
  OC -->|PR commits via API| GITHUB
  GITHUB -->|PR Preview (Actions)| TG
```

What the diagram means

The OpenClaw Minty agent orchestrates your workflows between Telegram, Gmail, Todoist, Google Drive, and GitHub via mcporter and the Zapier MCP bridge. Anniek talks to the system through the Telegram bot, while scheduled cron jobs trigger email triage, diary creation, and other maintenance tasks. Email routines run during active hours to label and summarize new mail and prepare draft replies when something looks actionable. Each night, a diary entry is created or updated in Google Docs and a link is sent to Telegram, and site changes flow through GitHub pull requests with automated CI previews.
