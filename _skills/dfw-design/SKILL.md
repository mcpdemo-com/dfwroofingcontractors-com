---
name: dfw-design
description: >
  Use this skill whenever building or updating any dfwroofingcontractors.com
  HTML page. Contains the complete brand design system: CSS variables,
  typography, color palette, component library (nav, footer, cards, buttons,
  forms), and page templates. Triggers when: writing HTML for any page,
  checking brand colors or fonts, building a nav or footer, creating forms or
  cards. Always use this skill before writing any CSS or HTML — never guess
  brand values from memory.
---

# DFW Roofing Contractors — Design System Skill

Single source of truth for all dfwroofingcontractors.com visual design.
Copy components from here directly — do not improvise brand values.

---

## Brand Identity

**Site:** DFW Roofing Contractors
**Tagline:** "Dallas-Fort Worth's Trusted Roofing Network"
**Tone:** Professional, trustworthy, local. Not corporate. Not flashy.
**Audience:** DFW homeowners who need a roofer — often urgently.

---

## Core Rules

1. **File size target:** under 50KB, aim for ~40KB per page
2. **Never embed base64 images** in HTML except the favicon `<link rel="icon">`
3. **Lead form is the #1 priority on every page** — CTA must always be visible
4. **Phone number must appear in the nav and footer** on every page
5. **Trust signals** (licensed, insured, local) must appear above the fold on homepage

---

## CSS Variables (copy into every `<style>` block)

```css
:root {
  --navy:    #1a2744;   /* Primary dark — headings, nav bg */
  --blue:    #2563eb;   /* Primary accent — CTAs, links */
  --blue2:   #1d4ed8;   /* CTA hover state */
  --orange:  #ea580c;   /* Urgency accent — "Emergency", badges */
  --bg:      #ffffff;   /* Page background */
  --bg2:     #f8fafc;   /* Section alternate background */
  --bg3:     #f1f5f9;   /* Card / box background */
  --border:  #e2e8f0;   /* All borders and dividers */
  --txt:     #0f172a;   /* Primary text */
  --muted:   #64748b;   /* Secondary / muted text */
  --dim:     #94a3b8;   /* Dimmed text — fine print */
  --green:   #16a34a;   /* Trust / success signals */
}
```

---

## Typography

```html
<!-- Always include both fonts — put in <head> before styles -->
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700;800&family=Open+Sans:wght@400;500;600&display=swap" rel="stylesheet" />
```

| Use | Font | Weight |
|-----|------|--------|
| Page headings h1, h2 | Montserrat | 700–800 |
| Nav site name | Montserrat | 800 |
| Card titles | Montserrat | 700 |
| Body text, paragraphs | Open Sans | 400 |
| Form labels, buttons | Open Sans | 500–600 |

```css
h1 { font-size: clamp(2rem, 5vw, 3.2rem); font-family: "Montserrat", sans-serif; font-weight: 800; color: var(--navy); }
h2 { font-size: clamp(1.5rem, 3vw, 2.2rem); font-family: "Montserrat", sans-serif; font-weight: 700; color: var(--navy); }
h3 { font-family: "Montserrat", sans-serif; font-weight: 700; color: var(--navy); }
body { font-family: "Open Sans", sans-serif; color: var(--txt); background: var(--bg); }
```

---

## Page Shell Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>[PAGE TITLE] | DFW Roofing Contractors</title>
  <meta name="description" content="[DESCRIPTION — 150-160 chars]" />
  <meta property="og:title" content="[OG TITLE]" />
  <meta property="og:description" content="[OG DESCRIPTION]" />
  <link rel="canonical" href="https://dfwroofingcontractors.com/[path]/" />
  <link rel="icon" type="image/png" href="/favicon.png" />
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700;800&family=Open+Sans:wght@400;500;600&display=swap" rel="stylesheet" />
  <style>
    /* 1. CSS variables */
    /* 2. Reset + base */
    /* 3. Nav */
    /* 4. Page-specific styles */
    /* 5. Footer */
    /* 6. Responsive */
  </style>
</head>
<body>
  <!-- NAV -->
  <!-- PAGE CONTENT -->
  <!-- FOOTER -->
  <script>document.getElementById('yr').textContent = new Date().getFullYear();</script>
</body>
</html>
```

---

## Navigation Component

```html
<nav>
  <div class="nav-inner">
    <a href="/" class="nav-brand">DFW Roofing Contractors</a>
    <div class="nav-links">
      <a href="/services/">Services</a>
      <a href="/areas/">Service Areas</a>
      <a href="/about/">About</a>
      <a href="/estimate/" class="btn-nav">Free Estimate</a>
    </div>
  </div>
</nav>
```

```css
nav {
  position: sticky; top: 0; z-index: 100;
  background: var(--navy);
  border-bottom: 3px solid var(--blue);
  height: 64px;
}
.nav-inner {
  max-width: 1100px; margin: 0 auto; padding: 0 2rem;
  height: 100%; display: flex; align-items: center; justify-content: space-between;
}
.nav-brand {
  color: #fff; text-decoration: none;
  font-family: "Montserrat", sans-serif; font-weight: 800; font-size: 1.1rem;
}
.nav-links { display: flex; align-items: center; gap: 2rem; }
.nav-links a { color: rgba(255,255,255,0.8); text-decoration: none; font-size: 0.9rem; transition: color 0.2s; }
.nav-links a:hover { color: #fff; }
.btn-nav {
  padding: 0.45rem 1.2rem;
  background: var(--blue); color: #fff !important;
  border-radius: 6px; font-weight: 600;
  font-family: "Open Sans", sans-serif; transition: background 0.2s;
}
.btn-nav:hover { background: var(--blue2) !important; }
```

---

## Footer Component

```html
<footer>
  <div class="foot-inner">
    <div class="foot-cols">
      <div>
        <div class="foot-brand">DFW Roofing Contractors</div>
        <p style="color:var(--dim); font-size:0.85rem; margin-top:0.5rem">
          Connecting Dallas-Fort Worth homeowners<br>with trusted local roofing professionals.
        </p>
        <p style="color:var(--dim); font-size:0.78rem; margin-top:1rem">
          &copy; <span id="yr"></span> DFW Roofing Contractors<br>
          Operated by Effinsoftware.com, INC. — Wyoming, USA
        </p>
      </div>
      <div>
        <h4 class="foot-heading">Services</h4>
        <a href="/services/#repair" class="foot-link">Roof Repair</a>
        <a href="/services/#replacement" class="foot-link">Roof Replacement</a>
        <a href="/services/#storm" class="foot-link">Storm Damage</a>
        <a href="/services/#gutters" class="foot-link">Gutters</a>
      </div>
      <div>
        <h4 class="foot-heading">Company</h4>
        <a href="/areas/" class="foot-link">Service Areas</a>
        <a href="/about/" class="foot-link">About</a>
        <a href="/estimate/" class="foot-link">Free Estimate</a>
        <a href="/legal/privacy-policy.html" class="foot-link">Privacy Policy</a>
        <a href="/legal/terms-of-service.html" class="foot-link">Terms of Service</a>
      </div>
      <div>
        <h4 class="foot-heading">Get a Free Estimate</h4>
        <a href="/estimate/" class="btn-primary" style="display:inline-block; margin-bottom:1rem">Request Now</a>
        <p style="color:var(--dim); font-size:0.8rem">
          Serving Dallas, Fort Worth, Plano,<br>Frisco, McKinney, Arlington & more.
        </p>
      </div>
    </div>
    <div class="foot-bottom">
      <span style="color:var(--muted); font-size:0.76rem">
        DFW Roofing Contractors is a lead generation service connecting homeowners with licensed, 
        insured roofing contractors in the Dallas-Fort Worth metroplex. We are not a roofing 
        company. Contractor licensing verified independently.
      </span>
    </div>
  </div>
</footer>
```

```css
footer { background: var(--navy); color: #fff; padding: 3rem 2rem 2rem; margin-top: 5rem; }
.foot-inner { max-width: 1100px; margin: 0 auto; }
.foot-cols { display: grid; grid-template-columns: 1.4fr 1fr 1fr 1.2fr; gap: 3rem; margin-bottom: 2rem; }
.foot-brand { font-family: "Montserrat", sans-serif; font-weight: 800; font-size: 1.1rem; color: #fff; }
.foot-heading { color: var(--blue); font-size: 0.68rem; text-transform: uppercase;
  letter-spacing: 0.1em; margin-bottom: 0.75rem; font-family: "Montserrat", sans-serif; font-weight: 700; }
.foot-link { display: block; color: rgba(255,255,255,0.65); text-decoration: none;
  font-size: 0.85rem; margin-bottom: 0.4rem; transition: color 0.2s; }
.foot-link:hover { color: #fff; }
.foot-bottom { border-top: 1px solid rgba(255,255,255,0.1); padding-top: 1.5rem; }
@media (max-width: 768px) { .foot-cols { grid-template-columns: 1fr 1fr; gap: 2rem; } }
@media (max-width: 480px) { .foot-cols { grid-template-columns: 1fr; } }
```

---

## Buttons

```html
<a href="/estimate/" class="btn-primary">Get a Free Estimate</a>
<a href="/services/" class="btn-secondary">View Services</a>
```

```css
.btn-primary {
  display: inline-block; padding: 0.85rem 2.2rem;
  background: var(--blue); color: #fff; border: none; border-radius: 8px;
  font-weight: 700; font-family: "Open Sans", sans-serif; font-size: 1rem;
  text-decoration: none; cursor: pointer; transition: all 0.2s;
}
.btn-primary:hover { background: var(--blue2); transform: translateY(-1px); box-shadow: 0 4px 16px rgba(37,99,235,0.35); }

.btn-secondary {
  display: inline-block; padding: 0.85rem 2.2rem;
  background: transparent; color: var(--navy);
  border: 2px solid var(--navy); border-radius: 8px;
  font-family: "Open Sans", sans-serif; font-weight: 600;
  text-decoration: none; cursor: pointer; transition: all 0.2s;
}
.btn-secondary:hover { background: var(--navy); color: #fff; }

.btn-orange {
  display: inline-block; padding: 0.85rem 2.2rem;
  background: var(--orange); color: #fff; border-radius: 8px;
  font-weight: 700; font-family: "Open Sans", sans-serif;
  text-decoration: none; cursor: pointer; transition: all 0.2s;
}
.btn-orange:hover { background: #c2410c; }
```

---

## Lead Capture Form (Formspree)

Replace `YOUR_FORMSPREE_ID` with the actual Formspree form ID once created.

```html
<form class="lead-form" action="https://formspree.io/f/YOUR_FORMSPREE_ID" method="POST">
  <div class="form-row">
    <div class="form-group">
      <label for="name">Your Name *</label>
      <input type="text" id="name" name="name" required placeholder="John Smith" />
    </div>
    <div class="form-group">
      <label for="phone">Phone Number *</label>
      <input type="tel" id="phone" name="phone" required placeholder="(214) 555-0100" />
    </div>
  </div>
  <div class="form-group">
    <label for="email">Email Address *</label>
    <input type="email" id="email" name="email" required placeholder="john@email.com" />
  </div>
  <div class="form-group">
    <label for="address">Property Address</label>
    <input type="text" id="address" name="address" placeholder="123 Main St, Dallas, TX" />
  </div>
  <div class="form-group">
    <label for="service">What do you need? *</label>
    <select id="service" name="service" required>
      <option value="">Select a service...</option>
      <option value="repair">Roof Repair</option>
      <option value="replacement">Full Replacement</option>
      <option value="storm">Storm / Hail Damage</option>
      <option value="inspection">Free Inspection</option>
      <option value="gutters">Gutters</option>
      <option value="other">Other / Not Sure</option>
    </select>
  </div>
  <div class="form-group">
    <label for="message">Additional Details</label>
    <textarea id="message" name="message" rows="3" placeholder="Describe the issue, approximate roof age, etc."></textarea>
  </div>
  <input type="hidden" name="_subject" value="New Roofing Lead — DFW Roofing Contractors" />
  <input type="text" name="_gotcha" style="display:none" />
  <button type="submit" class="btn-primary" style="width:100%; font-size:1.1rem; padding:1rem">
    Request My Free Estimate
  </button>
  <p style="font-size:0.8rem; color:var(--muted); text-align:center; margin-top:0.75rem">
    No obligation. A licensed local contractor will contact you within 24 hours.
  </p>
</form>
```

```css
.lead-form { background: var(--bg); border: 1px solid var(--border); border-radius: 12px; padding: 2rem; }
.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
.form-group { display: flex; flex-direction: column; gap: 0.4rem; margin-bottom: 1rem; }
.form-group label { font-size: 0.9rem; font-weight: 600; color: var(--navy); }
.form-group input, .form-group select, .form-group textarea {
  padding: 0.75rem 1rem; border: 1px solid var(--border); border-radius: 8px;
  font-family: "Open Sans", sans-serif; font-size: 0.95rem; color: var(--txt);
  background: #fff; outline: none; transition: border-color 0.2s;
}
.form-group input:focus, .form-group select:focus, .form-group textarea:focus {
  border-color: var(--blue); box-shadow: 0 0 0 3px rgba(37,99,235,0.1);
}
@media (max-width: 600px) { .form-row { grid-template-columns: 1fr; } }
```

---

## Trust Badge Strip

Use above or below the lead form on every high-conversion page.

```html
<div class="trust-strip">
  <div class="trust-item">
    <svg viewBox="0 0 24 24" width="22" height="22" fill="none" stroke="var(--green)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <span>Licensed & Insured</span>
  </div>
  <div class="trust-item">
    <svg viewBox="0 0 24 24" width="22" height="22" fill="none" stroke="var(--green)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <span>Free Estimates</span>
  </div>
  <div class="trust-item">
    <svg viewBox="0 0 24 24" width="22" height="22" fill="none" stroke="var(--green)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <span>DFW Local Contractors</span>
  </div>
  <div class="trust-item">
    <svg viewBox="0 0 24 24" width="22" height="22" fill="none" stroke="var(--green)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <span>24-Hour Response</span>
  </div>
</div>
```

```css
.trust-strip {
  display: flex; flex-wrap: wrap; gap: 1.5rem; justify-content: center;
  padding: 1.25rem 2rem; background: var(--bg2); border-radius: 10px;
  border: 1px solid var(--border);
}
.trust-item { display: flex; align-items: center; gap: 0.5rem; font-size: 0.9rem; font-weight: 600; color: var(--navy); }
```

---

## Service Cards (3-column grid)

```html
<div class="cards">
  <div class="card">
    <!-- SVG icon -->
    <h3>Roof Repair</h3>
    <p>Fast, reliable repairs for leaks, missing shingles, storm damage, and more. DFW contractors on call.</p>
    <a href="/estimate/" class="card-link">Get a Quote →</a>
  </div>
</div>
```

```css
.cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px,1fr)); gap: 1.5rem; }
.card {
  background: var(--bg); border: 1px solid var(--border);
  border-top: 3px solid var(--blue); border-radius: 12px; padding: 1.75rem;
  transition: transform 0.2s, box-shadow 0.2s;
}
.card:hover { transform: translateY(-3px); box-shadow: 0 8px 24px rgba(37,99,235,0.1); }
.card h3 { font-family: "Montserrat", sans-serif; font-weight: 700; color: var(--navy); margin: 0.75rem 0 0.5rem; font-size: 1.1rem; }
.card p { color: var(--muted); font-size: 0.9rem; line-height: 1.65; margin: 0 0 1rem; }
.card-link { color: var(--blue); font-weight: 600; font-size: 0.9rem; text-decoration: none; }
.card-link:hover { text-decoration: underline; }
```

---

## Hero Section Pattern

```html
<section class="hero">
  <div class="hero-inner">
    <div class="hero-badge">Serving Dallas-Fort Worth Since 2018</div>
    <h1>DFW's Trusted Roofing Contractors</h1>
    <p class="hero-sub">Get matched with licensed, insured local roofers. Free estimates. 24-hour response. Hail damage specialists.</p>
    <div class="hero-ctas">
      <a href="/estimate/" class="btn-primary">Get a Free Estimate</a>
      <a href="/services/" class="btn-secondary">View Services</a>
    </div>
  </div>
</section>
```

```css
.hero {
  padding: 5rem 2rem 4rem;
  background: linear-gradient(135deg, var(--navy) 0%, #2d3f6b 100%);
  text-align: center; color: #fff;
}
.hero-inner { max-width: 760px; margin: 0 auto; }
.hero h1 { color: #fff; margin-bottom: 1rem; }
.hero-badge {
  display: inline-block; font-size: 0.8rem; font-weight: 700;
  letter-spacing: 0.08em; text-transform: uppercase; color: var(--blue);
  background: rgba(37,99,235,0.15); border: 1px solid rgba(37,99,235,0.4);
  border-radius: 4px; padding: 0.25rem 0.75rem; margin-bottom: 1.25rem;
  font-family: "Montserrat", sans-serif;
}
.hero-sub { color: rgba(255,255,255,0.8); font-size: 1.1rem; line-height: 1.7; max-width: 560px; margin: 0 auto 2.5rem; }
.hero-ctas { display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap; }
```

---

## SEO Schema Markup

Include on homepage and service area pages for local SEO.

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "DFW Roofing Contractors",
  "description": "Roofing contractor referral service for the Dallas-Fort Worth metroplex",
  "url": "https://dfwroofingcontractors.com",
  "areaServed": ["Dallas", "Fort Worth", "Plano", "Frisco", "McKinney", "Arlington", "Irving", "Garland", "Mesquite", "Carrollton"],
  "serviceType": ["Roof Repair", "Roof Replacement", "Storm Damage", "Gutters"]
}
</script>
```

---

## Writing Checklist

Before finalizing any page:
- [ ] Phone/CTA visible in nav and above fold
- [ ] Trust badges present on conversion pages
- [ ] Formspree form ID replaced (not placeholder)
- [ ] Meta description is 150-160 chars, includes "DFW" and service keyword
- [ ] Footer disclaimer present (lead gen service, not a roofing company)
- [ ] Schema markup on homepage and area pages
- [ ] All internal links use relative paths
- [ ] Mobile responsive (test at 375px)
