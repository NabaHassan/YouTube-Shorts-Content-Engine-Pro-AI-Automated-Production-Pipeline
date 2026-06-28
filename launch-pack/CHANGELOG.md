# Changelog

## v1.0.0 — Production Launch (2026-06-29)

### Added
- Google Sheets settings loader (replaces in-workflow Config node)
- Token usage logging after OpenAI script generation
- Image + TTS cost estimates with per-run total
- Usage Log sheet append on successful publish
- Separate error alert workflow with Slack/Discord webhook support
- Hook-Body-CTA strategist system prompt with 50–55s pacing validation
- Launch pack: installation guide, troubleshooting FAQ, content strategy CSV, versioning policy

### Changed
- Workflow renamed to "YouTube Shorts Automation — AI Create & Publish (Pro)"
- Error workflow linked via settings (`YtShortsErr001`)
- Script word-count validation (110–160 words guardrail)

### Required Buyer Setup
- Google Sheets OAuth2 credential
- OpenAI, Creatomate, YouTube OAuth2 credentials
- Creatomate 9:16 template with standard element names
- Slack/Discord webhook for error alerts (optional but recommended)
