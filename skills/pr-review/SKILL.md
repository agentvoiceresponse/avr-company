---
name: "pr-review"
description: "Review pull requests for code quality, API contract compliance, and AVR standards. Use when assigned a PR to review."
slug: "pr-review"
license: "MIT"
metadata:
  version: "1.0"
  author: "avr-company"
  paperclip:
    slug: "pr-review"
    skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/pr-review"
  paperclipSkillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/pr-review"
  skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/pr-review"
key: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/pr-review"
---

# PR Review Skill

## Review Checklist

### Code Quality
- [ ] No hardcoded API keys or secrets
- [ ] Error handling for all external API calls
- [ ] Graceful degradation on provider errors
- [ ] Console logging with connector name prefix and X-UUID

### API Contract Compliance
- [ ] ASR connector: `POST /speech-to-text-stream` returns SSE text events
- [ ] LLM connector: `POST /prompt-stream` returns SSE response stream
- [ ] TTS connector: `POST /text-to-speech-stream` returns LINEAR16 PCM audio
- [ ] STS connector: WebSocket at root, bidirectional PCM audio
- [ ] X-UUID header handled correctly

### Docker Standards
- [ ] Dockerfile uses node:lts-slim base
- [ ] .env.example includes all required variables
- [ ] .dockerignore excludes node_modules, .env, .git
- [ ] Package.json has correct start script

### Documentation
- [ ] README updated with any new env vars or configuration changes
- [ ] CHANGELOG updated if applicable

## Review Actions
- **Approve**: All checks pass, code is clean
- **Request Changes**: List specific issues with suggested fixes
- **Escalate to CTO**: Architectural concerns or cross-repo impact
