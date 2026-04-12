---
name: "docker-compose-gen"
description: "Generate or update Docker Compose templates for avr-infra. Use when a new connector is added or a deployment configuration needs updating."
slug: "docker-compose-gen"
license: "MIT"
metadata:
  version: "1.0"
  author: "avr-company"
  paperclip:
    slug: "docker-compose-gen"
    skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/docker-compose-gen"
  paperclipSkillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/docker-compose-gen"
  skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/docker-compose-gen"
key: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/docker-compose-gen"
---

# Docker Compose Generator Skill

Generate Docker Compose files for avr-infra following the established patterns.

## Standard Service Template

Every compose file MUST include:

```yaml
services:
  avr-asterisk:
    image: agentvoiceresponse/avr-asterisk
    ports:
      - "5060:5060/tcp"
    volumes:
      - ./asterisk/conf:/etc/asterisk/conf
      - ./sounds:/var/lib/asterisk/sounds/custom
    restart: unless-stopped
    networks:
      - avr-network

  avr-core:
    image: agentvoiceresponse/avr-core
    ports:
      - "5001:5001"
    environment:
      - PORT=5001
      # For pipeline mode:
      - ASR_URL=http://avr-asr-{provider}:{port}/speech-to-text-stream
      - LLM_URL=http://avr-llm-{provider}:{port}/prompt-stream
      - TTS_URL=http://avr-tts-{provider}:{port}/text-to-speech-stream
      # OR for STS mode:
      - STS_URL=ws://avr-sts-{provider}:{port}
    restart: unless-stopped
    networks:
      - avr-network

networks:
  avr-network:
    driver: bridge
```

## Naming Convention

Files: `docker-compose-{provider}.yml` for STS, `docker-compose-{llm-provider}.yml` for pipeline.

## Checklist Before Committing

1. All service names match Docker Hub image names
2. All ports are unique and consistent with connector defaults
3. Environment variables reference `.env` file with `${VAR}` syntax
4. Network is `avr-network` for all services
5. `restart: unless-stopped` on all services
6. README updated with new compose option
