# avr-company

[![Paperclip](https://img.shields.io/badge/Powered%20by-Paperclip%20AI-black?logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0xOS41IDNhMy41IDMuNSAwIDAgMC0zLjUgMy41VjE3YTUgNSAwIDAgMS0xMCAwVjZhMS41IDEuNSAwIDAgMSAzIDB2MTFhMiAyIDAgMCAwIDQgMFY2LjVhMy41IDMuNSAwIDAgMSA3IDB2MTAuOGgtMlY2LjVBMS41IDEuNSAwIDAgMCAxOS41IDN6Ii8+PC9zdmc+)](https://paperclip.ing)
[![Discord](https://img.shields.io/discord/1347239846632226998?label=Discord&logo=discord)](https://discord.gg/DFTU69Hg74)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![agentcompanies/v1](https://img.shields.io/badge/schema-agentcompanies%2Fv1-blue)](https://github.com/paperclipai/companies)

**The AgentVoiceResponse zero-human company — a Paperclip AI configuration that autonomously develops, maintains, and grows the AVR open-source ecosystem.**

[Website](https://www.agentvoiceresponse.com/) • [Documentation](https://wiki.agentvoiceresponse.com) • [Community](https://discord.gg/DFTU69Hg74) • [Paperclip AI](https://paperclip.ing)

---

This repository contains the [Paperclip AI](https://paperclip.ing) company configuration for the AgentVoiceResponse project. It defines a team of five autonomous AI agents that collectively manage all 41 AVR open-source repositories — from connector development and infrastructure to documentation and community support — without requiring day-to-day human involvement.

The human founder operates as the board of directors: setting strategy, approving major decisions, and monitoring outcomes. The agents handle everything else.

## Table of Contents

- [🚀 Quick Start](#-quick-start)
- [📋 Overview](#-overview)
- [🏗️ Org Chart](#%EF%B8%8F-org-chart)
- [🤖 Agents](#-agents)
- [🧠 Skills](#-skills)
- [⚙️ Configuration](#%EF%B8%8F-configuration)
- [🔧 Installation](#-installation)
- [📈 Roadmap](#-roadmap)
- [🤝 Community & Support](#-community--support)
- [📄 License](#-license)

## 🚀 Quick Start

```bash
# 1. Install Paperclip
npx paperclipai onboard --yes

# 2. Clone this configuration
git clone https://github.com/agentvoiceresponse/avr-company.git

# 3. Import the company (dry run first)
npx paperclipai company import --from ./avr-company --dry-run

# 4. Import for real
npx paperclipai company import --from ./avr-company

# 5. Open the dashboard and configure agent adapters
open http://localhost:3100
```

After import, configure each agent's adapter and model in the Paperclip UI, then set your first company goal to start the agents working.

## 📋 Overview

`avr-company` is a Paperclip AI company template built specifically for the AgentVoiceResponse ecosystem. It encodes the organizational structure, operational workflows, skill library, and agent identities needed to run AVR as an autonomous AI organization.

### What the agents do

- **Develop** new connectors as AI providers release new voice APIs
- **Maintain** all existing ASR, LLM, TTS, and STS connector repositories
- **Deploy** every new connector with Docker Compose templates in avr-infra
- **Document** every feature on wiki.agentvoiceresponse.com
- **Triage** GitHub issues and respond to community contributions within 24 hours
- **Guard** the public/private boundary of the proprietary avr-core

### What the agents never do

- Access, probe, or reverse-engineer `avr-core` internals
- Merge PRs without the CTO's code review
- Commit secrets or API keys to any repository
- Make promises about roadmap features without board approval

## 🏗️ Org Chart

```
Board (Founder)
  └── CEO
       ├── CTO
       │    ├── Backend Developer
       │    └── DevOps Engineer
       └── Docs & Community Manager
```

The CEO reports to the board (the human founder) and escalates only decisions that are irreversible, security-sensitive, or involve new commercial agreements. All other decisions flow autonomously through the org chart.

## 🤖 Agents

| Agent | Model | Adapter | Reports To | Responsibilities |
|---|---|---|---|---|
| **CEO** | `opencode/big-pickle` | opencode_local | Board | Strategy, roadmap, community health, board reporting |
| **CTO** | `opencode/big-pickle` | opencode_local | CEO | Architecture, PR reviews, API contract, NPM packages |
| **Backend Developer** | `opencode/gpt-5-nano` | opencode_local | CTO | Connector development, bug fixes, utilities |
| **DevOps Engineer** | `opencode/minimax-m2.5-free` | opencode_local | CTO | avr-infra, Docker Compose, CI/CD pipelines |
| **Docs & Community Manager** | `opencode/nemotron-3-super-free` | opencode_local | CEO | Wiki, issue triage, community responses |

Each agent has three core identity files: `AGENTS.md` (role and rules), `SOUL.md` (values and character), and `HEARTBEAT.md` (operational checklist run at every session start).

## 🧠 Skills

Skills are reusable instruction modules available to all agents. Each agent's `AGENTS.md` specifies which skills to invoke and when.

| Skill | Used By | Purpose |
|---|---|---|
| `para-memory-files` | All agents | Three-layer memory system (knowledge graph, daily notes, plans) |
| `connector-scaffold` | Backend Dev, CTO | Generate new AVR connector boilerplate from template |
| `docker-compose-gen` | DevOps | Generate avr-infra Docker Compose templates for new connectors |
| `issue-triage` | Docs/Community, CEO | Categorize, label, and route new GitHub issues |
| `wiki-sync` | Docs/Community, CEO | Detect and fix drift between repo READMEs and wiki pages |
| `pr-review` | CTO, Backend Dev | Review PRs against AVR API contract and code quality standards |
| `release-connector` | CTO, DevOps | Version bump, changelog, Docker image publish, NPM release |
| `avr-test-call` | Backend Dev | Validate a connector works end-to-end with avr-core |

## ⚙️ Configuration

### Directory structure

```
avr-company/
├── COMPANY.md                    # Company definition (agentcompanies/v1)
├── README.md                     # This file
├── LICENSE
├── .paperclip.yaml               # Runtime configuration
├── agents/
│   ├── ceo/
│   │   ├── AGENTS.md             # Role, responsibilities, safety rules
│   │   ├── SOUL.md               # Values, character, communication style
│   │   ├── HEARTBEAT.md          # Operational checklist per session
│   │   └── TOOLS.md              # Available tools and MCP servers
│   ├── cto/
│   ├── backend-dev/
│   ├── devops-engineer/
│   └── docs-community/
└── skills/
    ├── para-memory-files/
    ├── connector-scaffold/
    ├── docker-compose-gen/
    ├── issue-triage/
    ├── wiki-sync/
    ├── pr-review/
    ├── release-connector/
    └── avr-test-call/
```

### Agent file anatomy

Every agent directory follows this convention:

**`AGENTS.md`** — The agent's operational identity. Defines role, key responsibilities, decision framework, safety rules, and which skills to use. Loaded by Paperclip at every heartbeat.

**`SOUL.md`** — The agent's character. Defines values, communication style, what it cares about, and how it handles ambiguity. Keeps the agent's behavior consistent across sessions.

**`HEARTBEAT.md`** — The agent's session checklist. A step-by-step procedure the agent follows every time it wakes up: reestablish identity, load memory, check assignments, execute work, store memory, report. Solves the stateless agent problem.

**`TOOLS.md`** — The agent's available tools. Documents MCP servers, CLI commands, and external integrations the agent can use.

### AVR API contract (enforced by CTO)

All AVR connectors must implement these exact interfaces:

| Type | Endpoint | Protocol | Audio |
|---|---|---|---|
| ASR | `POST /speech-to-text-stream` | HTTP SSE | 8kHz PCM in, text events out |
| LLM | `POST /prompt-stream` | HTTP SSE | Text in, text stream out |
| TTS | `POST /text-to-speech-stream` | HTTP stream | Text in, LINEAR16 PCM out |
| STS | WebSocket `/` | WS bidirectional | PCM in, PCM out |

### Private/public boundary

`avr-core` is proprietary. The agents work exclusively on the 41 public repositories surrounding it. This boundary is structural — the agents have no GitHub access to the private `avr-core` repository — and it is reinforced in every `AGENTS.md` and `SOUL.md` file.

## 🔧 Installation

### Prerequisites

- [Paperclip AI](https://paperclip.ing) installed (`npx paperclipai onboard`)
- [OpenCode](https://opencode.ai) installed (`npm install -g opencode`) for the `opencode_local` adapter
- GitHub Personal Access Token with `repo` scope for the GitHub MCP server
- Node.js 18+

### Step-by-step

**1. Import the company**

```bash
git clone https://github.com/agentvoiceresponse/avr-company.git
npx paperclipai company import --from ./avr-company
```

**2. Configure agent adapters**

Open the Paperclip dashboard at `http://localhost:3100`. For each agent, set:
- Adapter type: `opencode_local`
- Model: see the table in [Agents](#-agents) above
- Working directory: your local clone of the `agentvoiceresponse` GitHub org

**3. Configure the GitHub MCP server**

Each agent's `TOOLS.md` references the GitHub MCP server. Configure it with your GitHub PAT in the Paperclip agent settings under "MCP Servers".

**4. Set skills**

In the Paperclip Skills library, verify all eight skills from the [Skills](#-skills) table are imported. They are linked to agents via instructions in each `AGENTS.md`.

**5. Create the first goal**

In the Paperclip dashboard, create a company goal such as:

> "Triage all open unlabeled GitHub issues across AVR repositories and ensure the wiki documentation is current for all merged connectors."

The CEO agent will wake up, read the goal, and delegate work to the appropriate agents.

## 📈 Roadmap

This company configuration is designed to grow with the AVR project.

**Phase 1 — Early stage (current)**: 5 agents covering strategy, architecture, connector development, infrastructure, and documentation.

**Phase 2 — Community traction**: Add a `frontend-dev` agent for avr-app development, a `qa-engineer` for automated end-to-end testing, and split `docs-community` into dedicated documentation and community management roles.

**Phase 3 — Enterprise/commercial**: Add `sales-agent`, `support-engineer`, and `partnerships-agent` roles to handle inbound enterprise inquiries, technical support, and provider relationships.

See `COMPANY.md` for the full roadmap detail.

## 🤝 Community & Support

| Resource | Link |
|---|---|
| Website | [agentvoiceresponse.com](https://www.agentvoiceresponse.com) |
| Documentation | [wiki.agentvoiceresponse.com](https://wiki.agentvoiceresponse.com) |
| Discord | [discord.gg/DFTU69Hg74](https://discord.gg/DFTU69Hg74) |
| GitHub Issues | [github.com/agentvoiceresponse/avr-company/issues](https://github.com/agentvoiceresponse/avr-company/issues) |
| Docker Hub | [hub.docker.com/u/agentvoiceresponse](https://hub.docker.com/u/agentvoiceresponse) |
| Email | [info@agentvoiceresponse.com](mailto:info@agentvoiceresponse.com) |

AVR is free and open-source. Any support is entirely voluntary and intended as a personal gesture of appreciation. Donations do not provide access to features, services, or special benefits, and the project remains fully available regardless of donations.

[![Ko-fi](https://img.shields.io/badge/Support%20us%20on-Ko--fi-ff5e5b.svg)](https://ko-fi.com/agentvoiceresponse)

## 📄 License

MIT — see [LICENSE](LICENSE) for details.
