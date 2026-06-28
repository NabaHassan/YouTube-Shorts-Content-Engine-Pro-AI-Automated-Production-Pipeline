# Installation Guide (Loom Script)

Use this as your 5-minute walkthrough script when recording the buyer onboarding video.

## Before You Record

- Have a clean n8n instance (or Docker) running
- Have blank API keys ready (do not show real keys on screen — blur or use placeholders)
- Import both workflow JSON files from the launch pack

---

## Scene 1 — What They Bought (30 seconds)

> "You didn't buy a script — you bought a Content Engine. One row in Google Sheets becomes a fully produced YouTube Short: AI script, voiceover, visuals, render, and upload. Everything is configured from a spreadsheet — no editing n8n nodes after setup."

Show the workflow canvas at a high level (Settings → AI → Media → Render → YouTube → Usage Log).

---

## Scene 2 — Google Sheet Setup (60 seconds)

1. Open `content-strategy-template.csv` and import into Google Sheets
2. Rename/copy tabs:
   - Tab 1: **Settings** (from CSV)
   - Tab 2: **Usage Log** with headers: `timestamp`, `title`, `niche`, `youtubeUrl`, `totalTokens`, `estimatedCostUsd`, `status`
3. Copy the Sheet ID from the URL (`docs.google.com/spreadsheets/d/SHEET_ID/edit`)
4. Paste into **Load Settings** and **Append Usage Log** nodes in n8n
5. Set `creatomateTemplateId` and mark one row `active=TRUE`

> "Your clients never touch n8n again — they only edit this sheet."

---

## Scene 3 — API Credentials (90 seconds)

### OpenAI (3 nodes)
1. n8n → Credentials → Add → OpenAI API
2. Paste API key from platform.openai.com
3. Open each OpenAI HTTP node → select credential → Save

### Creatomate (2 nodes)
1. Credentials → HTTP Header Auth
2. Name: `Authorization`, Value: `Bearer YOUR_CREATOMATE_KEY`
3. Assign to **Start Creatomate Render** and **Get Render Status**

### YouTube (1 node)
1. Credentials → YouTube OAuth2
2. Connect Google account with YouTube upload scope
3. Assign to **Upload to YouTube**

### Google Sheets (2 nodes)
1. Credentials → Google Sheets OAuth2
2. Connect Google account
3. Assign to **Load Settings** and **Append Usage Log**

---

## Scene 4 — Error Alerts (45 seconds)

1. Import `youtube-shorts-error-handler.json`
2. Activate the error handler workflow
3. Open main workflow → Settings (gear) → confirm **Error Workflow** = `YouTube Shorts — Error Alerts`
4. Edit **Alert Config** → paste Slack/Discord webhook URL
5. Run a deliberate test failure to verify alert delivery

> "Production systems fail loudly — you'll know within seconds, not days."

---

## Scene 5 — First Test Run (60 seconds)

1. Set Settings sheet: `privacyStatus=unlisted`, valid Creatomate template ID
2. Click **Execute workflow** → Manual Test
3. Watch execution progress node by node
4. Confirm:
   - YouTube video appears (unlisted)
   - **Usage Log** sheet gets a new row with tokens + estimated cost

> "First run should always be unlisted. Switch to public only when you're happy with output quality."

---

## Scene 6 — Daily Automation (30 seconds)

1. Toggle workflow **Active**
2. Daily Schedule runs at 9:00 AM (edit in node if needed)
3. Rotate topics by changing `active=TRUE` row in Settings sheet

---

## Closing (15 seconds)

> "Support includes 6 months of template updates. Check TROUBLESHOOTING-FAQ.md for common fixes. You're ready to publish."

---

## Recording Checklist

- [ ] Both workflows imported
- [ ] Sheet ID pasted in both Google Sheets nodes
- [ ] All 4 credential types connected
- [ ] Error webhook tested
- [ ] Successful unlisted Short published
- [ ] Usage Log row appended
