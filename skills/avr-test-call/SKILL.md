---
name: "avr-test-call"
description: "Make a test voice call through AVR to validate that a connector is working end-to-end. Use after deploying a new or modified connector to verify audio flow."
slug: "avr-test-call"
license: "MIT"
metadata:
  version: "1.0"
  author: "avr-company"
  paperclip:
    slug: "avr-test-call"
    skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/avr-test-call"
  paperclipSkillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/avr-test-call"
  skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/avr-test-call"
key: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/avr-test-call"
---

# AVR Test Call Skill

Use this skill to validate that a connector is responding correctly before closing a task or creating a PR.

## When to Use

- After scaffolding a new connector and implementing its logic
- After fixing a bug in an existing connector
- When the CTO or CEO asks to verify a deployment

## Step 1: Verify Connector Is Running

Check that the connector service is up and listening on its expected port:

```bash
curl -s http://localhost:{PORT}/health || echo "No health endpoint — checking process"
docker ps | grep avr-{type}-{provider}
```

If not running, start it:
```bash
docker-compose up -d avr-{type}-{provider}
```

## Step 2: Test the Endpoint Directly

### ASR (Speech-to-Text)
Send a raw PCM audio chunk and verify SSE text events are returned:
```bash
curl -X POST http://localhost:{PORT}/speech-to-text-stream \
  -H "Content-Type: application/octet-stream" \
  -H "X-UUID: test-call-001" \
  --data-binary @test-audio-8khz.raw \
  --no-buffer
```
Expected: one or more `data: {"text": "..."}` SSE events.

### LLM (Language Model)
```bash
curl -X POST http://localhost:{PORT}/prompt-stream \
  -H "Content-Type: application/json" \
  -H "X-UUID: test-call-001" \
  -d '{"messages": [{"role": "user", "content": "Say hello in one sentence."}]}' \
  --no-buffer
```
Expected: SSE stream of text chunks ending with `data: [DONE]`.

### TTS (Text-to-Speech)
```bash
curl -X POST http://localhost:{PORT}/text-to-speech-stream \
  -H "Content-Type: application/json" \
  -H "X-UUID: test-call-001" \
  -d '{"text": "Hello, this is a test."}' \
  --output test-output.raw
file test-output.raw
```
Expected: binary file containing LINEAR16 PCM audio at 8kHz.

### STS (Speech-to-Speech)
Use `websocat` or a test WebSocket client:
```bash
websocat ws://localhost:{PORT}/ --binary
```
Expected: bidirectional PCM audio exchange.

## Step 3: Validate Audio Quality

For TTS and STS output, convert and listen:
```bash
ffplay -f s16le -ar 8000 -ac 1 test-output.raw
```

Acceptable output:
- Audible speech with no distortion
- Correct language and voice
- No clipping or silence artifacts

## Step 4: Test with avr-infra Stack

If a Docker Compose template exists for this connector, test the full stack:
```bash
cd avr-infra
docker-compose -f docker-compose-{provider}.yml up -d
# Make a SIP call via Asterisk to trigger the pipeline
```

## Step 5: Report Results

After testing, report to CTO with:
- Connector name and version tested
- Endpoints tested and results (pass/fail)
- Any errors observed and how they were resolved
- Whether a full stack test was performed
