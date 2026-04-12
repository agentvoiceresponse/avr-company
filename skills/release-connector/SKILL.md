---
name: "release-connector"
description: "Release a new version of an AVR connector. Handles version bump, changelog, Docker image build, and npm publish. Use when CTO approves a release."
slug: "release-connector"
license: "MIT"
metadata:
  version: "1.0"
  author: "avr-company"
  paperclip:
    slug: "release-connector"
    skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/release-connector"
  paperclipSkillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/release-connector"
  skillKey: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/release-connector"
key: "company/fd1d3622-4a13-43cb-b553-5a924b7693b4/release-connector"
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
