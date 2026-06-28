# Troubleshooting FAQ

**YouTube Shorts Content Engine — Production Support Guide**

Print or export this document to PDF for your launch pack.

---

## Quick Diagnostics

| Symptom | Most Likely Cause | Fix |
|---------|-------------------|-----|
| Workflow stops at Load Settings | Google Sheets credential or Sheet ID wrong | Re-auth Google OAuth; verify Sheet ID and tab name `Settings` |
| OpenAI 401 Unauthorized | Invalid or missing API key | Regenerate key at platform.openai.com; re-assign credential |
| Script length error | AI returned too short/long script | Re-run; adjust topic in Settings sheet; check tone/niche clarity |
| Creatomate render failed | Template element names mismatch | Rename elements to: Voice-Audio, Background-Image, Hook-Text, Caption-Text |
| YouTube upload fails | OAuth expired or quota exceeded | Reconnect YouTube credential; check Google Cloud quota |
| No error alert received | Error workflow not active or webhook wrong | Activate error handler; verify webhook URL in Alert Config |

---

## OpenAI Errors

### "Insufficient quota" / 429 Rate Limit

**Cause:** OpenAI billing limit reached or too many requests.

**Fix:**
1. Add payment method at platform.openai.com/account/billing
2. Wait 60 seconds and retry
3. Reduce daily schedule frequency

### "Invalid API key" (401)

**Fix:**
1. Create a new secret key
2. Update n8n OpenAI credential
3. Re-save all three OpenAI HTTP nodes

### Script JSON parse error

**Cause:** Model returned non-JSON or missing fields.

**Fix:**
1. Re-run workflow (usually transient)
2. Simplify topic in Settings sheet
3. Ensure niche/tone values are plain text (no special characters)

---

## Creatomate Errors

### Render status: `failed`

**Fix:**
1. Open Creatomate dashboard → Renders → view error detail
2. Confirm template is **1080×1920** (9:16)
3. Verify modification keys match exactly:
   - `Voice-Audio`
   - `Background-Image`
   - `Hook-Text`
   - `Caption-Text`
4. Test template manually in Creatomate editor with sample URLs

### Render stuck on `planned` / `rendering`

**Fix:**
1. Increase **Wait for Render** from 45s to 90s
2. Re-run **Get Render Status** manually
3. Check Creatomate service status

### Invalid template ID

**Fix:** Copy template ID from Creatomate URL when template editor is open. Paste into Settings sheet `creatomateTemplateId` column.

---

## YouTube Errors

### "The user has exceeded the number of videos they may upload"

**Cause:** YouTube API daily quota (default 10,000 units; upload ≈ 1,600 units).

**Fix:**
1. Wait until quota resets (midnight Pacific)
2. Request quota increase in Google Cloud Console
3. Limit to 5–6 uploads per day on default quota

### OAuth / permission errors

**Fix:**
1. Disconnect and reconnect YouTube OAuth2 credential
2. Ensure Google Cloud project has YouTube Data API v3 enabled
3. Re-authorize with the correct YouTube channel account

### Video uploads but not classified as Short

**Fix:**
1. Keep video under 60 seconds
2. Use 9:16 aspect ratio (1080×1920)
3. Include `#Shorts` in description (added automatically)

---

## Google Sheets Errors

### "Settings sheet is empty"

**Fix:** Import `content-strategy-template.csv` and ensure at least one data row exists.

### "Set a valid creatomateTemplateId"

**Fix:** Replace `YOUR_CREATOMATE_TEMPLATE_ID` in the active Settings row.

### Usage Log not updating

**Fix:**
1. Confirm tab name is exactly `Usage Log`
2. Header row must match: timestamp, title, niche, youtubeUrl, totalTokens, estimatedCostUsd, status
3. Re-auth Google Sheets credential with edit permission

---

## Error Alert Workflow

### Alerts not firing

**Fix:**
1. Activate **YouTube Shorts — Error Alerts** workflow
2. Main workflow Settings → Error Workflow = `YtShortsErr001`
3. Test webhook URL with curl or Postman
4. Slack webhooks require JSON with `text` field (already configured)

### Too many alerts

**Fix:** Add filtering in **Format Error Alert** code node for known transient errors, or route to a dedicated `#automation-errors` Slack channel.

---

## Cost Tracking Notes

Token usage is logged from OpenAI `usage` object on script generation. TTS and image costs use conservative estimates:

| Step | Tracking Method |
|------|-----------------|
| Script (GPT-4o-mini) | Exact tokens from API |
| Image (DALL-E) | ~$0.04 estimate per image |
| TTS | ~$0.015 estimate per Short |

Actual costs may vary. Review OpenAI and Creatomate dashboards monthly.

---

## Support Escalation Template

When contacting support, include:

```
Workflow: YouTube Shorts Automation — AI Create & Publish (Pro)
Execution ID: [from n8n Executions tab]
Failed Node: [node name]
Error Message: [exact text]
Settings Row: [niche + topic from sheet]
Timestamp: [UTC]
```

---

## Version

Document version: 1.0.0 — included with launch pack v1.0
