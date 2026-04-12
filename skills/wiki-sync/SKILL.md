---
name: "wiki-sync"
description: "Detect documentation drift between repository READMEs and avr-docs pages, then commit and push updates to avr-docs so Wiki.js picks them up automatically."
slug: "wiki-sync"
license: "MIT"
metadata:
  version: "1.1"
  author: "avr-company"
  paperclip:
    slug: "wiki-sync"
    skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/wiki-sync"
  paperclipSkillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/wiki-sync"
  skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/wiki-sync"
key: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/wiki-sync"
---

# Wiki Sync Skill

## How the documentation pipeline works

`avr-docs` is the **single source of truth** for wiki.agentvoiceresponse.com.
Wiki.js reads directly from the `avr-docs` git repository.
Publishing a page = committing and pushing a `.md` file to `avr-docs`.

Do NOT use avr-docs-mcp. Do NOT write to any other location.

## Sync Process

### Step 1: Gather Current State

For each AVR repository, read the `README.md` and note:
- Environment variables (names, descriptions, required/optional)
- API endpoints and port numbers
- Setup and quick-start instructions
- Supported provider features (languages, voices, models)
- Known limitations or provider-specific quirks

### Step 2: Compare Against avr-docs

For each connector, find the corresponding `.md` file in `avr-docs` and compare:
- Are all env vars documented and accurate?
- Are port numbers correct?
- Are setup steps current with the README?
- Does the provider compatibility matrix include this connector?

### Step 3: Update avr-docs

For any drift detected, or when creating documentation for a new connector:

1. Edit or create the relevant `.md` file directly in the `avr-docs` repository
2. Follow the standard page structure (see Docs agent HEARTBEAT Step 5)
3. Commit with a clear message:
   ```
   docs({connector-name}): {brief description of what changed}
   ```
4. Push to the `main` branch of `avr-docs`

Wiki.js will pick up the changes automatically from git.

### Step 4: Report

After sync, comment on the Paperclip task with:
- Files created or updated in `avr-docs`
- Git commit hash(es)
- Any major changes that need human review
