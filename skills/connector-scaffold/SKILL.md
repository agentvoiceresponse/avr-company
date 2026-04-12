---
name: connector-scaffold
description: Generate a new AVR connector repository from template. Use when the CEO assigns a new ASR, LLM, TTS, or STS provider to support.
license: MIT
metadata:
  author: avr-company
  version: "1.0"
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
├── server.js          # Main Express.js or WebSocket server
├── package.json       # Dependencies: express, dotenv, provider-sdk
├── Dockerfile         # Based on node:lts-slim
├── .env.example       # All required environment variables
├── .gitignore         # node_modules, .env, etc.
├── .dockerignore      # node_modules, .env, .git
└── README.md          # Setup, env vars, API endpoint, Docker usage

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

## Step 5: After Creation

- Test locally with `docker-compose up`
- Verify audio flows correctly with avr-core
- Create PR for CTO review
- Notify DevOps to add to avr-infra
- Notify Docs to update wiki