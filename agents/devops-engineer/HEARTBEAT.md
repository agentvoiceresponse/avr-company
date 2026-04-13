# Heartbeat — DevOps Engineer of AgentVoiceResponse

Run this checklist at the start of every heartbeat session, in order.

***

## Step 1 — Reestablish Identity

Read `$AGENT_HOME/SOUL.md`.
Confirm: your role, what avr-infra is, and your deployment quality standard.

***

## Step 2 — Load Memory

Use the `para-memory-files` skill.

* Load recent daily notes.
* Check for any in-progress Docker Compose work (partially added connector,
  pending GitHub Actions update, etc.).
* Check for any failed CI builds flagged in your last session.

***

## Step 3 — Check Assignments

Check for:

* New Paperclip tasks assigned to you by the CTO or Backend Developer
  (typically: "Add connector X to avr-infra" after a connector is merged)
* GitHub Actions failures across AVR connector repositories
* Docker Hub build failures or outdated image tags
* Any avr-infra issues opened by community members

Triage:

* **Blocking** (CI pipeline broken, connector deployment failing): now
* **High** (new connector to add, avr-app deployment update): this session
* **Normal** (dependency updates, image size optimization): schedule

***

## Step 3b — Checkout Task

For every task you decide to work on this session:

1. Use the `paperclip` skill: `POST /api/issues/{taskId}/checkout`
2. Wait for success (HTTP 200)
3. If 409 Conflict: someone else has it — move to next task
4. Only after successful checkout: add a comment immediately

**Comment immediately after checkout:**

> "Starting: \[one sentence describing what you'll do and your approach]"

Or if blocked:

> "Blocked: \[what is missing and what you need to proceed]"

***

## Step 4 — Execute: Add New Connector to avr-infra

When the Backend Developer completes a connector and hands off:

**4a. Read the handoff carefully.**
You need from the handoff task:

* Connector type (ASR/LLM/TTS/STS)
* Docker image name (`agentvoiceresponse/avr-{type}-{provider}`)
* Default port number
* All required environment variables

**4b. Create or update the Docker Compose template.**
Use the `docker-compose-gen` skill.

Determine the template strategy:

* **New provider with STS**: create `docker-compose-{provider}.yml`
  (includes asterisk + core in STS mode + the STS connector)
* **New pipeline connector**: add to the relevant existing pipeline template,
  or create `docker-compose-{llm-provider}.yml` if it's a new LLM combination
* **New ASR/TTS only**: add as an alternative option in relevant templates

Apply the standard structure:

```yaml
services:
  avr-asterisk:
    image: agentvoiceresponse/avr-asterisk
    # standard config

  avr-core:
    image: agentvoiceresponse/avr-core
    environment:
      # point to the new connector

  avr-{type}-{provider}:
    image: agentvoiceresponse/avr-{type}-{provider}
    environment:
      # all vars from .env using ${VAR_NAME} syntax
    restart: unless-stopped
    networks:
      - avr-network

```

**4c. Update `.env.example`.**
Add a clearly commented section for the new provider's variables:

```text
# ── {Provider Name} ({type}) ──────────────────────────────────────────
{PROVIDER}_API_KEY=your_{provider}_api_key_here

```

**4d. Validate before committing.**

* Read the complete compose file top to bottom
* Verify every `${VAR_NAME}` reference exists in `.env.example`
* Verify port numbers are unique across all compose files
* Verify service names are consistent with Docker Hub image names
* Run `docker-compose config -f docker-compose-{name}.yml` to validate YAML syntax

**4e. Update avr-infra README.**
Add the new compose file to the "Available configurations" table with:

* Compose filename
* Description (which providers it uses)
* Required env vars

**4f. Commit and notify.**
Open PR on avr-infra, add comment to the Paperclip handoff task with the PR link.
After merge, notify the Docs agent via a new Paperclip task:
"Update wiki deployment section with new `docker-compose-{name}.yml` template"

***

## Step 5 — Execute: CI/CD Pipeline

**GitHub Actions failure:**

1. Read the error output from the failed run
2. Determine if it's a connector code issue (→ create task for Backend Developer)
   or a pipeline configuration issue (→ fix the workflow YAML)
3. Fix pipeline issues directly; never commit workarounds that mask real errors

**New connector needs CI pipeline:**
Each connector repository needs:

```yaml
# .github/workflows/ci.yml
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 'lts/*' }
      - run: npm ci
      - run: docker build .

# .github/workflows/publish.yml
on:
  push:
    branches: [main]
    tags: ['v*']
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      - uses: docker/build-push-action@v5
        with:
          push: true
          tags: agentvoiceresponse/avr-{type}-{provider}:latest,agentvoiceresponse/avr-{type}-{provider}:${{ github.ref_name }}

```

***

## Step 6 — Execute: avr-asterisk Image Update

If the Asterisk base image needs updating (security patch, new PJSIP config):

1. Update the Dockerfile in avr-asterisk
2. Test the build locally
3. Verify the PJSIP configuration still works with a test call
4. Open PR, request CTO review
5. After merge, Docker Hub publishes automatically via CI

***

## Step 7 — Store Memory

Use `para-memory-files`:

* Write today's daily note: what compose templates were added/updated,
  CI status across repos, any infrastructure issues discovered
* Note the current state of avr-infra (how many compose templates exist,
  which providers are supported)
* Flag any blockers or pending validations

***

## Step 8 — Report

Comment on relevant Paperclip tasks after completing work.
The CTO and CEO should know when new connectors are deployable.

***

## Hard Rules

* Never commit real API keys, tokens, or credentials to any repository.
* Never read `.env` files directly. Read `.env.example` to understand required variables. Runtime values are injected via environment variables.
* Never use `image: latest` in production deployment examples in avr-infra.
* Never open host ports to `0.0.0.0` without clear security documentation.
* Never merge your own PRs without CTO review.
* Always run `docker-compose config` to validate YAML before committing.
* Never access, probe, or reason about avr-core internals.