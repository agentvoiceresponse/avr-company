---
name: issue-triage
description: Categorize, label, and respond to new GitHub issues across AVR repositories. Use during every heartbeat to keep response time under 24 hours.
license: MIT
metadata:
  author: avr-company
  version: "1.0"
---

# Issue Triage Skill

## Triage Process

Every heartbeat, scan all AVR repositories for new unlabeled issues.

### Step 1: Categorize

Read the issue title and body. Determine:
- Is it a bug report, feature request, question, or connector request?
- Which repo(s) does it affect?
- What priority level? (high = affects call quality/crashes; medium = functionality gap; low = cosmetic/nice-to-have)

### Step 2: Label

Apply appropriate labels:
- Type: `bug`, `enhancement`, `connector-request`, `question`, `documentation`
- Priority: `priority:high`, `priority:medium`, `priority:low`
- Scope: `infra`, `app`, `connector-asr`, `connector-llm`, `connector-tts`, `connector-sts`, `utility`
- Community: `good-first-issue`, `help-wanted`

### Step 3: Respond

Post a comment acknowledging the issue:
- For bugs: "Thanks for reporting. We'll investigate." + ask for reproduction steps if missing
- For features: "Interesting idea. We'll evaluate this for our roadmap."
- For connector requests: "Thanks! We track provider requests and prioritize by community interest."
- For questions: Answer directly if possible, or link to relevant wiki page

### Step 4: Assign

- Bugs in connectors → assign to Backend Dev
- Bugs in avr-infra or Docker → assign to DevOps Engineer
- Documentation issues → self-assign
- Architecture questions → escalate to CTO
- Strategic/roadmap items → escalate to CEO