---
name: dfw-plan
description: >
  Use this skill FIRST before any create, update, remove, or test request
  involving dfwroofingcontractors.com pages or files. Triggers on: "build the
  homepage", "add the services page", "fix the nav", "deploy", "check the live
  site", or any request that would create, change, or delete a file on
  dfwroofingcontractors.com. This skill fetches live state from GitHub, audits
  what exists, identifies risks, and produces a written plan for approval before
  any work begins. Never skip this skill to "save time" — it exists to prevent
  broken nav links, overwritten live pages, and file-size budget overruns.
---

# DFW Roofing Contractors — Plan Skill

You are the planning gate for all dfwroofingcontractors.com work. No file gets
created, modified, or deployed without a written plan reviewed by the user first.

---

## Step 0 — Resolve Ambiguity Before Fetching Anything

Before making any tool calls, check whether the request names a specific
file, page, or operation.

**If the request is specific** (e.g. "build the services page", "fix the nav
on the homepage") — proceed to Step 1.

**If the request is vague** (e.g. "update the site", "fix something") — ask
ONE clarifying question before doing anything:

> "Which page or file should I look at? For example: homepage, services,
> areas, about, estimate page, or a blog post?"

Do not fetch live state, do not list files, do not make assumptions.
Wait for a specific answer before proceeding.

---

## Step 1 — Classify the Request

| Operation | Examples |
|-----------|---------|
| **CREATE** | "Build the services page", "Add the estimate form page" |
| **UPDATE** | "Fix the nav on the homepage", "Add Plano to service areas" |
| **REMOVE** | "Delete that section", "Remove that card" |
| **TEST/VERIFY** | "Check if the site is working", "Does the form submit correctly?" |
| **DEPLOY** | Deploying an already-built file to GitHub Pages |

---

## Step 2 — Fetch Live State from GitHub

Use `mcpcio:gitmanager_list_files` and `mcpcio:gitmanager_read_file` to
inspect what currently exists before touching anything.

### Files to always check:
- The specific file(s) that will be affected
- `index.html` (homepage) — check nav links and footer
- Any page that links TO the affected file
- Any page the affected file links TO

### What to look for:
- Does the file already exist? (Risk: overwrite)
- Current file size in KB (Budget: keep under 50KB, aim for ~40KB)
- Are nav links consistent across pages?
- Are there any hardcoded paths that would break if a file moves?
- Does the footer match the standard template?

### For REMOVE operations, additionally check:
- Every page that contains an `<a href>` pointing to the file being removed
- Whether removing this file would create any 404s

---

## Step 2b — Browse for Current Information

Before finalizing any plan that involves third-party tools, APIs, or platform
behavior, use web search and web_fetch to verify current status.

### Always browse when the plan involves:
- Formspree — verify current endpoint format and free tier limits
- GitHub Pages — verify current propagation behavior and custom domain setup
- Any external API endpoint — confirm it's still active
- Any uncertainty about how a platform currently works

---

## Step 3 — Identify Risks

After fetching live state, explicitly list every risk:

```
RISKS IDENTIFIED:
- [HIGH] /services/index.html already exists — this would overwrite live content
- [MEDIUM] Homepage nav does not include a Services link yet — needs update
- [LOW] File will be ~38KB — within budget
- [NONE] No other pages link to this file
```

Rate each risk: HIGH / MEDIUM / LOW / NONE

---

## Step 4 — Write the Plan

Produce a clear, numbered plan. Name every file, every section, every tool call.

**The plan must close with the `## APPROVED PLAN` block — this is the
handoff contract that `dfw-ops` reads before executing.**

### Plan Template:

```
## Plan: [Operation] — [Page/File Name]

### Files Affected:
- CREATE: services/index.html
- UPDATE: index.html (add Services to nav)

### Steps:
1. Read current index.html from GitHub to extract existing nav HTML
2. Build services/index.html using dfw-design skill for brand components
3. Update nav in index.html to include Services link
4. Size-check both files before deploy (target <40KB each)
5. Deploy services/index.html via dfw-ops skill
6. Deploy updated index.html via dfw-ops skill
7. Verify live URLs

### Risks:
- Homepage will be modified — step 1 captures current state first
- Services page is new — no overwrite risk

### NOT in scope:
- Nav on other pages — will note for a future session

### Estimated file sizes:
- services/index.html: ~36KB (estimated)
- index.html: delta ~200 bytes

---
## APPROVED PLAN
[Repeat the plan content here verbatim — dfw-ops will locate this block]
---
```

---

## Step 5 — Get Approval

Present the plan and ask:

> "Does this plan look right? Anything to add, remove, or change before I start?"

**Do not proceed until the user explicitly approves.**

---

## Step 6 — Hand Off to Execution Skills

Once approved:
- For design/build work → consult `dfw-design` skill
- For deploy/ops work → consult `dfw-ops` skill

**Keep the `## APPROVED PLAN` block visible and unmodified in the conversation.**

---

## Guardrails

- **Never overwrite a live file without first reading its current content**
- **Never deploy a file over 50KB** — split or remove content first
- **Never add a nav link on one page without auditing all other pages**
- **Never assume a page doesn't exist** — always check GitHub first
- **If a fetch fails**, tell the user and ask how to proceed — do not guess
- **Never start fetching on a vague request** — resolve Step 0 first

---

## Page Inventory

⚠️ Always verify against live GitHub — this table is a starting point only.
Run `mcpcio:gitmanager_list_files` to confirm current state before planning.

| Path | Status | Notes |
|------|--------|-------|
| `index.html` | ⬜ Not built | Homepage — primary lead capture |
| `404.html` | ⬜ Not built | Auto-redirect to homepage |
| `services/index.html` | ⬜ Not built | Services overview |
| `areas/index.html` | ⬜ Not built | Service areas with city sections |
| `about/index.html` | ⬜ Not built | About / trust page |
| `estimate/index.html` | ⬜ Not built | Free estimate form — primary CTA |
| `blog/` | ⬜ Not built | SEO content pages |
| `sitemap.xml` | ⬜ Not built | Create after first page deploy |
| `CNAME` | ⬜ Not built | Required for custom domain |

Last updated: April 2026. Always fetch live state before acting.
