---
name: dfw-debug
description: >
  Diagnoses incomplete or failed dfwroofingcontractors.com workflows by
  checking all expected state locations and producing a gap report with
  specific recovery actions. Use whenever something went wrong, a deploy
  stopped mid-run, or state is unclear. Triggers on: "something went wrong",
  "check the state of [page]", "why isn't [page] live", "what's missing",
  "diagnose", "resume a failed deploy", "check if [step] completed". Always
  use this skill — do not investigate ad hoc.
---

# DFW Roofing Contractors — Debug Skill

Checks all expected state locations for a given page or workflow,
then produces a structured Gap Report with recovery actions.

---

## Fixed Asset IDs

| Asset | Value |
|---|---|
| **GitHub Repo** | `mcpdemo-com/dfwroofingcontractors-com` |
| **Lead Sheet ID** | TBD |

---

## Step 1 — Classify the Subject

| Subject type | Examples | Workflow to check |
|---|---|---|
| **Page deploy** | "homepage not live", "services page missing" | Deploy workflow |
| **Lead** | "lead L003 not in sheet", "form submission missing" | Lead workflow |
| **Custom domain** | "site not loading at dfwroofingcontractors.com" | DNS / GitHub Pages config |
| **Unclear** | "something went wrong" | Ask: "Is this about a page deploy, a lead, or the domain?" |

If unclear, ask one clarifying question before doing anything.

---

## Step 2 — Check State Locations

### For Page Deploys

```
1. mcpcio:gitmanager_read_file — [path/to/file]
   → Does the file exist? What size is it?

2. mcpcio:gitmanager_read_file — sitemap.xml
   → Is the page URL in the sitemap?

3. mcpcio:gitmanager_read_file — index.html
   → Is there a nav link to this page?

4. web_fetch — https://dfwroofingcontractors.com/[path]/
   → Does the live URL load correctly?
```

### For Custom Domain Issues

```
1. mcpcio:gitmanager_read_file — CNAME
   → Does it exist? Does it contain exactly "dfwroofingcontractors.com"?

2. web_fetch — https://mcpdemo-com.github.io/dfwroofingcontractors-com/
   → Does the github.io URL load? (confirms Pages is working, isolates DNS)

3. web_fetch — https://dfwroofingcontractors.com/
   → Does the custom domain load?
```

### For Lead Workflow Issues

```
1. MCPDEMO:search_sheet
   → Search for lead by name or ID
   → Confirm row exists and Status is correct

2. Check Formspree dashboard (manual step — ask user to verify)
   → Did the submission arrive in Formspree?
   → Was the notification email sent?
```

---

## Step 3 — Gap Report

```
## Gap Report — [Subject]

| Check | Status | Detail | Recovery Action |
|---|---|---|---|
| File exists in GitHub | ✅ / ❌ | [path] — [size or "not found"] | Re-run deploy from dfw-plan |
| File in sitemap | ✅ / ❌ | URL present / missing | Re-run sitemap step in dfw-ops |
| Nav link present | ✅ / ❌ | Found / not found in index.html | Update index.html nav |
| Live URL loads | ✅ / ❌ | [URL] — loads / 404 / wrong content | Wait 60s + check GitHub Pages settings |
| CNAME exists | ✅ / ❌ | Contains: [value] | Write CNAME file via dfw-ops |
| Custom domain loads | ✅ / ❌ | [URL] — loads / DNS not propagated | Check DNS settings, wait up to 24h |

### Summary
[One sentence: what completed, what's missing, recommended next action]
```

---

## Step 4 — Propose Recovery

- All checks passed → "No gaps found. Everything looks good."
- Gaps found → name the exact skill and step:
  - Page gap → "Run **dfw-plan** for [page], then **dfw-ops** deploy"
  - Domain gap → "Create CNAME via **dfw-ops**, verify DNS settings"
  - Lead gap → "Run **dfw-leads** Workflow A to log the missing lead"

Do not execute recovery within this skill. Propose, then let user decide.

---

## Rules

| Rule | Detail |
|---|---|
| **Classify before fetching** | Determine subject type before any tool calls |
| **Ask on ambiguity** | One clarifying question if subject is unclear |
| **Check all locations** | Don't declare complete based on one check |
| **Report what you found** | If a check errors, report the exact error |
| **Propose recovery, don't execute** | Diagnose and recommend — don't run the fix |
