---
name: dfw-ops
description: >
  Use this skill for all dfwroofingcontractors.com deploy, update, and connector
  operations. Contains repo identifiers, deploy workflow, sitemap format, and
  known workarounds. Triggers when: a plan has been approved and it's time to
  deploy a file, write to GitHub, or troubleshoot a failed deploy. Always
  consult dfw-plan FIRST — this skill handles execution, not planning.
---

# DFW Roofing Contractors — Ops Skill

Execution layer for dfwroofingcontractors.com. Only consult after a plan is approved.

---

## Plan Validation — Read This Before Executing Anything

Before taking any action, locate the `## APPROVED PLAN` block in the
current conversation:

```
---
## APPROVED PLAN
[plan content]
---
```

**If the block is present:** execute only the steps listed inside it, in order.
Do not add steps, skip steps, or deviate from the approved scope.

**If no `## APPROVED PLAN` block is found:** stop. Tell the user:
> "I don't see an approved plan in this conversation. Please run dfw-plan
> first so we have a reviewed plan before I start making changes."

---

## GitHub Repository

- **Account:** mcpdemo-com
- **Repository:** mcpdemo-com/dfwroofingcontractors-com
- **Live URL:** https://dfwroofingcontractors.com
- **GitHub Pages URL:** https://mcpdemo-com.github.io/dfwroofingcontractors-com/
- **Hosting:** GitHub Pages — static HTML/CSS/JS only, no server-side code
- **Custom domain file:** CNAME (must contain `dfwroofingcontractors.com`)

---

## Tool Selection

### 1. Git Operations (read/write/deploy files)
Use `mcpcio:gitmanager_*` for all GitHub operations:
- `mcpcio:gitmanager_list_files` — browse repo structure
- `mcpcio:gitmanager_read_file` — fetch current live file content
- `mcpcio:gitmanager_write_file` — create or update a file
- `mcpcio:gitmanager_list_commits` — check version history
- `mcpcio:gitmanager_rollback_to_commit` — restore a previous version

### 2. MCPDEMO Tools
Use `MCPDEMO:*` when working with:
- Lead tracking sheets (Google Sheets via MCPDEMO)
- Forms, widgets, contacts, broadcasts
- Any workflow that involves the MCPDEMO platform

### 3. Cloudflare (if needed in future)
Use `Cloudflare:execute` if adding a Worker proxy for lead routing or CORS.
Zone ID for dfwroofingcontractors.com: TBD — look up when Cloudflare is connected.

---

## Deploy Workflow (Standard)

```
Step 1 — Size check
  Confirm file size is under 50KB (aim for ~40KB)
  → If over 50KB: stop, tell user, discuss what to cut
  ✓ Success: file size confirmed under limit

Step 2 — Read current live version (if file already exists)
  → mcpcio:gitmanager_read_file
  → Confirm with user before overwriting
  ✓ Success: current content retrieved, user confirmed overwrite

Step 3 — Write the file to GitHub
  → mcpcio:gitmanager_write_file
  → Use exact path from approved plan (no leading slash, e.g. "services/index.html")
  ✓ Success: tool returns a commit SHA — record it
  → If it fails: stop immediately — report exact error, do not proceed

Step 4 — Update sitemap.xml
  → Read current sitemap.xml from GitHub first
  → Add new URL entry using standard format below
  → Set lastmod to today's date in YYYY-MM-DD format
  → Write updated sitemap.xml back to GitHub
  ✓ Success: sitemap write returns a commit SHA
  → If it fails: page is deployed but sitemap is stale —
    provide the XML entry verbatim for manual addition

Step 5 — Wait ~60 seconds for GitHub Pages propagation
  ✓ Success: 60 seconds elapsed

Step 6 — Verify the live URL loads correctly
  → Use web_fetch or ask user to confirm
  ✓ Success: page loads at expected URL with correct content
  → If not loading after 60s: ask user to check GitHub Pages settings,
    confirm CNAME is set, check that Pages is enabled on main branch
```

---

## CNAME File (Custom Domain)

Must exist at repo root for the custom domain to work.
Only needs to be created once.

```
dfwroofingcontractors.com
```

Deploy as:
```
path: CNAME
content: dfwroofingcontractors.com
repo: mcpdemo-com/dfwroofingcontractors-com
```

---

## Sitemap Format

Sitemap lives at `sitemap.xml`. Always read fresh before updating.

Priority values by page type:
- Homepage: changefreq weekly, priority 1.0
- Estimate page: changefreq monthly, priority 0.9
- Services page: changefreq monthly, priority 0.8
- Service areas page: changefreq monthly, priority 0.8
- Blog posts: changefreq monthly, priority 0.7
- About page: changefreq monthly, priority 0.6
- Legal pages: changefreq yearly, priority 0.3

Sitemap template:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://dfwroofingcontractors.com/</loc>
    <lastmod>YYYY-MM-DD</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
</urlset>
```

Entry template for new pages:
```xml
  <url>
    <loc>https://dfwroofingcontractors.com/[path]/</loc>
    <lastmod>YYYY-MM-DD</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.8</priority>
  </url>
```

---

## Lead Form — Formspree Setup

Formspree handles form submissions on the static site.

1. Go to formspree.io → New Form → name it "DFW Roofing Lead"
2. Copy the form ID (e.g. `xpwzabcd`)
3. Replace `YOUR_FORMSPREE_ID` in all HTML files with the real ID
4. Free tier: 50 submissions/month. Upgrade if volume exceeds this.
5. Formspree sends each lead to the registered email immediately
6. Form endpoint format: `https://formspree.io/f/[FORM_ID]`

**Current Formspree form ID:** TBD — add here once created.

---

## Lead Tracking Sheet (MCPDEMO)

Use MCPDEMO tools to maintain a Google Sheet for lead tracking.

Suggested columns:
- Date | Name | Phone | Email | Address | Service | Message | Status | Notes

Sheet operations:
- `MCPDEMO:add_row_to_sheet` — log a new lead manually
- `MCPDEMO:read_google_sheet` — review current leads
- `MCPDEMO:update_sheet_row` — update lead status

**Lead Sheet ID:** TBD — create sheet and record ID here once set up.

---

## Error Handling

### gitmanager_write_file fails
- Stop immediately — do not proceed to sitemap update
- Report: exact error message, file path attempted
- Do not retry automatically — tell user and ask to retry or abort

### Sitemap update fails
- HTML file is already deployed — record-keeping failure only
- Provide the complete XML `<url>` block verbatim for manual addition
- Do not re-deploy the HTML file

### Page not loading after 60s
- Check that GitHub Pages is enabled (Settings → Pages → main branch)
- Check that CNAME file exists in repo root
- Check DNS: dfwroofingcontractors.com should have a CNAME pointing to
  `mcpdemo-com.github.io`
- If CNAME DNS not propagated yet: normal — can take up to 24 hours

### Rollback Procedure
```
1. mcpcio:gitmanager_list_commits → find the last good commit SHA
2. mcpcio:gitmanager_rollback_to_commit → restore the file
   ✓ Success: rollback returns a new commit SHA
3. Verify live URL is restored
4. Report to user what was rolled back and why
```

---

## Known Issues & Workarounds

### File Size
- Keep all pages under 50KB, aim for ~40KB
- Never embed base64 images in HTML except favicon

### GitHub Pages Custom Domain
- Always maintain a CNAME file at repo root
- If custom domain stops working: check CNAME file still exists, re-enable
  Pages in GitHub settings, verify DNS hasn't changed

### Formspree Spam
- The `_gotcha` hidden field in the form template catches most bots
- If spam becomes an issue, enable Formspree's reCAPTCHA option

---

## Page Inventory

⚠️ Always verify against live GitHub before planning.

| Path | Status | Notes |
|------|--------|-------|
| `index.html` | ⬜ Not built | Homepage — primary lead capture |
| `404.html` | ⬜ Not built | Redirect to homepage |
| `services/index.html` | ⬜ Not built | Services overview |
| `areas/index.html` | ⬜ Not built | Service areas |
| `about/index.html` | ⬜ Not built | About / trust page |
| `estimate/index.html` | ⬜ Not built | Free estimate form |
| `sitemap.xml` | ⬜ Not built | Create after first deploy |
| `CNAME` | ⬜ Not built | Required for custom domain |

Last updated: April 2026.

---

## Contact & Legal

- **Contact email:** robert@mcpdemo.com (or create robert@dfwroofingcontractors.com)
- **Legal entity:** Effinsoftware.com, INC. (Wyoming corp)
- **Address:** 2232 Dell Range Blvd, Suite 245-3069, Cheyenne, WY 82009
- **Legal pages:** Need to be created at /legal/ — adapt from mcpdemo.com legal pages
