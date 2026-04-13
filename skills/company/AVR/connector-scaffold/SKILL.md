---
name: "connector-scaffold"
description: "Generate a new AVR connector repository from template. Use when the CEO assigns a new ASR, LLM, TTS, or STS provider to support."
slug: "connector-scaffold"
license: "MIT"
metadata:
  version: "1.0"
  author: "avr-company"
  paperclip:
    slug: "connector-scaffold"
    skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/connector-scaffold"
  paperclipSkillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/connector-scaffold"
  skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/connector-scaffold"
key: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/connector-scaffold"
---

# Connector Scaffold Skill

When creating a new AVR connector, follow this exact structure.

## Step 1: Determine Connector Type

Identify the type from the task description:
- **ASR** (Speech-to-Text): Accepts audio stream, returns text via SSE
- **LLM** (Language Model): Accepts text messages, returns text response via SSE
- **TTS** (Text-to-Speech): Accepts text, returns LINEAR16 PCM audio stream
- **STS** (Speech-to-Speech): Bidirectional WebSocket with PCM audio

## Step 2: Create Repository Structure

avr-{type}-{provider}/
├── server.js                        # Main Express.js or WebSocket server
├── package.json                     # Dependencies: express, dotenv, provider-sdk
├── Dockerfile                       # Based on node:lts-slim
├── .env.example                     # All required environment variables
├── .gitignore                       # node_modules, .env
├── .dockerignore                    # node_modules, .env, .git
├── .github/
│   └── workflows/
│       └── main.yml                 # Docker build & push to Docker Hub on push to main
└── README.md                        # Setup, env vars, API endpoint, Docker usage

## Step 3: Standard Server Template

For ASR/LLM/TTS (HTTP):
```javascript
import express from 'express';
import dotenv from 'dotenv';
dotenv.config();

const app = express();
const PORT = process.env.PORT || {default_port};

// ASR: app.post('/speech-to-text-stream', ...)
// LLM: app.post('/prompt-stream', ...)
// TTS: app.post('/text-to-speech-stream', ...)

app.listen(PORT, () => console.log(`[avr-{type}-{provider}] listening on ${PORT}`));
```

For STS (WebSocket):
```javascript
import { WebSocketServer } from 'ws';
// Bidirectional PCM audio at root path
```

## Step 4: Dockerfile Template

```dockerfile
FROM node:lts-slim
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev
COPY . .
EXPOSE {port}
CMD ["node", "server.js"]
```

## Step 4b: .gitignore

```
node_modules
.env
```

## Step 4c: .dockerignore

```
node_modules
.env
.git
```

## Step 4d: GitHub Actions — Version bump, Docker build & push

Create `.github/workflows/main.yml` with this exact content, replacing `{type}` and `{provider}`:

```yaml
name: Build Docker Image
on:
  push:
    branches:
      - main
jobs:
    build:
      name: push docker image to docker hub
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v2

        # Bump patch version automatically
        - name: bump version
          id: bump-version
          run: |
            npm version patch --no-git-tag-v
            echo "NEW_VERSION=$(jq -r '.version' package.json)" >> $GITHUB_OUTPUT

        # Commit and push the version bump
        - name: commit version bump
          run: |
            git config --global user.email "info@agentvoiceresponse.com"
            git config --global user.name "AgentVoiceResponse"
            git add package.json
            git commit -m "chore: bump version to ${{ steps.bump-version.outputs.NEW_VERSION }}"
            git push

        - name: login to docker hub
          id: docker-hub
          env:
            username: ${{secrets.DOCKERHUB_USERNAME}}
            password: ${{secrets.DOCKERHUB_PASSWORD}}
          run: docker login -u $username -p $password

        - name: get-npm-version
          id: package-version
          uses: martinbeentjes/npm-get-version-action@v1.3.1

        - name: build the docker image
          id: build-docker-image
          run: docker build -t agentvoiceresponse/avr-{type}-{provider}:latest -t agentvoiceresponse/avr-{type}-{provider}:${{ steps.package-version.outputs.current-version}} .

        - name: push the docker image
          id: push-docker-image
          run: docker push agentvoiceresponse/avr-{type}-{provider}:latest && docker push agentvoiceresponse/avr-{type}-{provider}:${{ steps.package-version.outputs.current-version}}
```

**How it works:**
1. On every push to `main`, the workflow triggers
2. `npm version patch` automatically increments the version (1.0.0 → 1.0.1)
3. The version bump is committed and pushed back to main
4. Docker image is built and pushed with both `latest` and versioned tags
5. Secrets `DOCKERHUB_USERNAME` and `DOCKERHUB_PASSWORD` are configured at org level in GitHub

## Step 5: After Creation

- Test locally with `docker-compose up`
- Verify audio flows correctly with avr-core
- Create PR for CTO review
- Notify DevOps to add to avr-infra
- Notify Docs to update wiki
