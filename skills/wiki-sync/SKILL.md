---
name: wiki-sync
description: Detect documentation drift between repository READMEs and wiki.agentvoiceresponse.com pages. Use to keep the wiki accurate and current.
license: MIT
metadata:
  author: avr-company
  version: "1.0"
---

# Wiki Sync Skill

## Sync Process

### Step 1: Gather Current State

For each AVR repository, read the README.md and note:
- Supported configuration options
- Environment variables
- API endpoints and ports
- Setup instructions
- Known limitations

### Step 2: Compare Against Wiki

Fetch corresponding wiki page and compare:
- Are all env vars documented?
- Are port numbers correct?
- Are setup steps current?
- Does the provider compatibility matrix reflect all connectors?

### Step 3: Update

For any drift detected:
- Update the wiki page via the avr-docs-mcp server or direct git commit to avr-docs
- Keep the wiki as the single source of truth for end-user docs
- Keep READMEs as developer-focused quick-start guides

### Step 4: Report

After sync, report to CEO:
- Number of pages checked
- Number of updates made
- Any major changes that need human review