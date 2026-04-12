---
name: release-connector
description: Release a new version of an AVR connector. Handles version bump, changelog, Docker image build, and npm publish. Use when CTO approves a release.
license: MIT
metadata:
  author: avr-company
  version: "1.0"
---

# Release Connector Skill

## Release Process

1. **Version Bump**: Update version in package.json following semver
2. **Changelog**: Update CHANGELOG.md with all changes since last release
3. **Git Tag**: Create annotated tag `v{version}`
4. **Push**: Push tag to trigger GitHub Actions CI
5. **Verify**: Confirm Docker image published to Docker Hub (`agentvoiceresponse/{connector}:{version}`)
6. **NPM**: If applicable (avr-vad, avr-resampler), publish to NPM
7. **Notify**: Tell Docs agent to update wiki, DevOps to update avr-infra image tags