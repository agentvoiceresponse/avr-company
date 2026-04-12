---
name: "AgentVoiceResponse"
description: "AI-powered company that develops, maintains, and grows the AVR open-source platform — a conversational AI system replacing traditional IVR with intelligent voice agents on Asterisk PBX."
slug: avr-company
schema: agentcompanies/v1
version: "1.0.0"
license: MIT
authors:
  - name: "Giuseppe Careri"
goals:
  - "Maintain and improve all 41 AVR open-source repositories across ASR, LLM, TTS, and STS connector categories"
  - "Keep avr-infra Docker Compose templates updated for every supported provider combination"
  - "Ensure wiki.agentvoiceresponse.com documentation stays accurate and complete"
  - "Triage GitHub issues and review community pull requests within 24 hours"
  - "Develop new connectors as AI providers release new real-time voice APIs"
  - "Grow the AVR community through quality documentation, examples, and responsive support"
  - "Protect the private avr-core codebase — never expose proprietary code, secrets, or internal architecture details in public repositories"
---

# AgentVoiceResponse AI Company

AVR is a conversational AI platform for Asterisk PBX that replaces traditional IVR with AI-driven voice agents. The platform uses a microservices architecture with a proprietary core (`avr-core`) that orchestrates audio between Asterisk (via AudioSocket) and AI services.

## Architecture

Two operating modes:
- **Pipeline (ASR → LLM → TTS)**: Speech recognition → language model → speech synthesis, each independently swappable
- **STS (Speech-to-Speech)**: Single-hop voice-in/voice-out for ultra-low latency (OpenAI Realtime, Gemini Live, etc.)

## Repository Organization

- **Infrastructure**: avr-infra, avr-asterisk, avr-app, avr-docs
- **ASR connectors** (5): Deepgram, Google, Vosk, Soniox, Sarvam
- **LLM connectors** (7): OpenAI, OpenAI Assistant, Anthropic, OpenRouter, N8N, Typebot, Sarvam
- **TTS connectors** (6): Deepgram, Google, ElevenLabs, CoquiTTS, Kokoro, Sarvam
- **STS connectors** (7): OpenAI Realtime, Ultravox, Deepgram, Gemini, ElevenLabs, HumeAI, Speechmatics
- **Utilities**: avr-vad, avr-resampler, avr-ami, avr-webhook, avr-phone, avr-docs-mcp

## Critical Boundary

The `avr-core` Docker image is proprietary. Agents MUST NEVER attempt to access, reverse-engineer, decompile, or expose avr-core internals. All work is on the open-source ecosystem surrounding it.