# Colmena Landing Page Redesign — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Evolve the single-file `index.html` Colmena landing page from 3 sections to 8 sections with alternating dark/light backgrounds, expanded value props, Mission Manifest YAML integration, honest auto-mode comparison, use case grid, and 3-tier CTA hierarchy — while preserving the hexagon animation, language toggle, color palette, Playfair font, and scroll-reveal patterns.

**Architecture:** Single `index.html` file — no build step, no framework. CSS is in `<style>`, JS is in `<script>`, i18n is a JS object. All new sections get `data-i18n` attributes. New CSS classes follow existing naming conventions (kebab-case). IntersectionObserver handles scroll reveals. Section backgrounds alternate via CSS classes applied directly to `<section>` elements.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, clamp), vanilla JS (IntersectionObserver, Canvas 2D), Google Fonts (Playfair Display)

---

## File Map

| File | Action | Purpose |
|---|---|---|
| `index.html` | Modify | All HTML, CSS, JS in one file |
| `index.html:25-433` | Modify | Add new CSS for sections 2-8 |
| `index.html:437-519` | Modify | Replace old sections with new 8-section HTML structure |
| `index.html:521-1016` | Modify | Expand I18N object with new keys for EN and ES |
| `index.html:1017-1327` | Modify | Add IntersectionObserver for new sections |

No new files. No file splits. The file stays self-contained.

---

### Task 1: Add CSS for alternating backgrounds, new section styles, CTAs, and responsive grid

**Files:**
- Modify: `index.html:25-433` (inside `<style>` block)

- [ ] **Step 1: Add light-section base class and transition gradients**

Add after the `body` rule (around line 49):

```css
/* Section background alternation */
.section-dark {
  position: relative;
  z-index: 5;
  background: var(--black);
}

.section-light {
  position: relative;
  z-index: 5;
  background: var(--bone);
  color: #1a1a1a;
}

/* Transition gradients between sections */
.fade-dark-to-light {
  position: relative;
  z-index: 5;
  height: 8vh;
  background: linear-gradient(to bottom, var(--black), var(--bone));
}

.fade-light-to-dark {
  position: relative;
  z-index: 5;
  height: 8vh;
  background: linear-gradient(to bottom, var(--bone), var(--black));
}
```

- [ ] **Step 2: Add section padding, eyebrow, and heading utilities**

```css
.section-pad {
  padding: 5rem 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.section-inner {
  width: 100%;
  max-width: 880px;
}

.section-eyebrow {
  font-family: 'Playfair Display', serif;
  font-style: italic;
  font-size: clamp(0.75rem, 1.5vw, 0.9rem);
  letter-spacing: 0.3em;
  text-transform: uppercase;
  color: var(--amber-muted);
  opacity: 0.75;
  text-align: center;
  margin-bottom: 1rem;
}

.section-heading {
  font-family: 'Playfair Display', serif;
  font-weight: 700;
  text-align: center;
  margin-bottom: 2.5rem;
  line-height: 1.3;
}

.section-light .section-eyebrow { color: var(--amber-muted); opacity: 0.8; }
.section-light .section-heading { color: #1a1a1a; }
.section-dark .section-heading { color: var(--bone); }
```

- [ ] **Step 3: Add CTA button styles**

```css
/* CTA hierarchy */
.cta-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.cta-primary {
  display: inline-block;
  background: var(--gold-dark);
  color: var(--black);
  padding: 0.75rem 2.2rem;
  text-decoration: none;
  font-family: 'Playfair Display', serif;
  font-weight: 700;
  font-size: clamp(0.9rem, 1.8vw, 1.05rem);
  letter-spacing: 0.08em;
  transition: background 0.3s ease, box-shadow 0.3s ease;
}

.cta-primary:hover {
  box-shadow: 0 0 24px rgba(212,160,23,0.3);
}

.cta-secondary {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  color: var(--amber-muted);
  text-decoration: none;
  font-family: 'Playfair Display', serif;
  font-size: clamp(0.85rem, 1.6vw, 0.95rem);
  letter-spacing: 0.08em;
  padding-bottom: 0.2rem;
  border-bottom: 1px solid rgba(212,160,23,0.3);
  transition: color 0.3s ease, border-color 0.3s ease;
}

.cta-secondary:hover {
  color: var(--bone);
  border-color: var(--gold-dark);
}

.cta-subtle {
  color: rgba(245,240,232,0.45);
  text-decoration: none;
  font-family: 'Playfair Display', serif;
  font-size: clamp(0.8rem, 1.4vw, 0.9rem);
  transition: color 0.3s ease;
}

.cta-subtle:hover {
  color: var(--bone);
}

.cta-row {
  display: flex;
  gap: 2rem;
  align-items: center;
}

.cta-divider {
  color: rgba(245,240,232,0.15);
}
```

- [ ] **Step 4: Add "false tradeoff" two-box styles**

```css
/* False tradeoff comparison boxes */
.tradeoff-grid {
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  gap: 1rem;
  align-items: center;
  max-width: 650px;
  margin: 2rem auto;
}

.tradeoff-box {
  text-align: center;
  padding: 1.2rem;
  border: 1px solid rgba(0,0,0,0.1);
  background: rgba(0,0,0,0.02);
}

.tradeoff-box-label {
  font-weight: 700;
  font-size: 0.9rem;
  margin-bottom: 0.4rem;
  color: #1a1a1a;
}

.tradeoff-box-desc {
  font-size: 0.82rem;
  opacity: 0.6;
  line-height: 1.4;
}

.tradeoff-vs {
  font-family: 'Playfair Display', serif;
  font-style: italic;
  color: var(--amber-muted);
  font-size: 1rem;
}

.tradeoff-resolution {
  text-align: center;
  margin-top: 2rem;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(212,160,23,0.3);
  max-width: 500px;
  margin-left: auto;
  margin-right: auto;
}

.tradeoff-resolution-title {
  font-family: 'Playfair Display', serif;
  font-size: 1.15rem;
  color: var(--amber-muted);
  font-weight: 700;
  margin-bottom: 0.5rem;
}
```

- [ ] **Step 5: Add How It Works grid styles**

```css
/* How It Works 4-step grid */
.how-grid {
  display: grid;
  grid-template-columns: 1.25fr 1fr 1fr 1fr;
  gap: 0.8rem;
  max-width: 920px;
  margin: 0 auto;
}

.how-step {
  text-align: center;
  padding: 1.2rem 0.8rem;
  border: 1px solid rgba(212,160,23,0.1);
  color: var(--bone);
}

.how-step-highlight {
  border: 1px solid rgba(212,160,23,0.35);
  background: rgba(212,160,23,0.04);
}

.how-step-num {
  font-family: 'Playfair Display', serif;
  font-style: italic;
  color: var(--gold-dark);
  font-size: 1.5rem;
  margin-bottom: 0.3rem;
}

.how-step-title {
  font-weight: 700;
  font-size: 0.95rem;
  margin-bottom: 0.5rem;
}

.how-step-desc {
  font-size: 0.78rem;
  opacity: 0.55;
  line-height: 1.45;
  margin-bottom: 0.5rem;
}

.how-step-artifact {
  font-family: 'SFMono-Regular', Consolas, monospace;
  font-size: 0.68rem;
  color: var(--amber-muted);
}

/* YAML preview block inside step 1 */
.yaml-preview {
  background: rgba(10,8,0,0.6);
  border: 1px solid rgba(212,160,23,0.2);
  padding: 0.6rem 0.7rem;
  font-family: 'SFMono-Regular', Consolas, monospace;
  font-size: 0.62rem;
  line-height: 1.55;
  color: rgba(245,240,232,0.78);
  text-align: left;
  margin-bottom: 0.5rem;
  white-space: pre;
  overflow-x: auto;
}

.how-check-list {
  font-size: 0.65rem;
  opacity: 0.35;
  line-height: 1.5;
  text-align: left;
}
```

- [ ] **Step 6: Add Value Props categories and comparison table styles**

```css
/* Value props categories */
.vp-category {
  margin-bottom: 2rem;
  border-left: 3px solid var(--gold-dark);
  padding-left: 1.2rem;
}

.vp-category-title {
  font-family: 'Playfair Display', serif;
  font-weight: 700;
  font-size: 1.1rem;
  color: var(--amber-muted);
  margin-bottom: 0.8rem;
}

.vp-bullet-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0.5rem 2rem;
}

.vp-bullet {
  font-size: 0.85rem;
  line-height: 1.5;
  color: #1a1a1a;
  opacity: 0.85;
}

.vp-bullet-dot {
  color: var(--gold-dark);
}

/* Comparison table (Colmena vs Auto-Mode) */
.compare-table-wrap {
  max-width: 720px;
  margin: 0 auto;
  overflow-x: auto;
}

.compare-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.85rem;
}

.compare-table thead tr {
  border-bottom: 1px solid rgba(212,160,23,0.3);
}

.compare-table th {
  text-align: left;
  padding: 0.6rem 0.8rem;
  font-weight: 400;
}

.compare-table th:first-child {
  color: var(--amber-muted);
  font-style: italic;
}

.compare-table th:nth-child(2) {
  color: rgba(245,240,232,0.5);
}

.compare-table th:nth-child(3) {
  color: var(--gold-dark);
  font-weight: 700;
}

.compare-table td {
  padding: 0.6rem 0.8rem;
  border-bottom: 1px solid rgba(212,160,23,0.08);
}

.compare-table td:first-child {
  opacity: 0.7;
}

.compare-table td:nth-child(2) {
  opacity: 0.5;
}

.compare-table td:nth-child(3) {
  color: var(--gold-dark);
}
```

- [ ] **Step 7: Add Use Cases grid styles**

```css
/* Use cases grid */
.usecase-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
  max-width: 800px;
  margin: 0 auto;
}

.usecase-card {
  background: rgba(212,160,23,0.04);
  border: 1px solid rgba(212,160,23,0.1);
  padding: 1.3rem;
  color: #1a1a1a;
}

.usecase-icon {
  font-size: 1.3rem;
  margin-bottom: 0.3rem;
}

.usecase-title {
  font-weight: 700;
  font-size: 0.95rem;
  margin-bottom: 0.4rem;
}

.usecase-desc {
  font-size: 0.8rem;
  opacity: 0.6;
  line-height: 1.5;
  margin-bottom: 0.6rem;
}

.usecase-flow {
  font-family: 'SFMono-Regular', Consolas, monospace;
  font-size: 0.65rem;
  color: var(--amber-muted);
}

.usecase-footer {
  text-align: center;
  margin-top: 2rem;
  font-size: 0.8rem;
  opacity: 0.4;
  color: #1a1a1a;
}
```

- [ ] **Step 8: Add reveal animation and mobile overrides**

```css
/* Reveal animation for all new sections */
.reveal-item {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.reveal-item.revealed {
  opacity: 1;
  transform: translateY(0);
}

/* Hero CTA group */
.hero-cta-group {
  margin-top: 2rem;
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
  align-items: center;
}

/* Mobile overrides */
@media (max-width: 767px) {
  .how-grid {
    grid-template-columns: 1fr;
    gap: 0.6rem;
  }

  .tradeoff-grid {
    grid-template-columns: 1fr;
  }

  .tradeoff-vs {
    text-align: center;
    padding: 0.3rem 0;
  }

  .vp-bullet-grid {
    grid-template-columns: 1fr;
  }

  .usecase-grid {
    grid-template-columns: 1fr;
  }

  .compare-table {
    font-size: 0.72rem;
  }

  .compare-table th,
  .compare-table td {
    padding: 0.4rem 0.5rem;
  }

  .hero-cta-group {
    flex-direction: column;
    gap: 0.6rem;
  }

  .cta-row {
    flex-direction: column;
    gap: 0.6rem;
  }

  .cta-divider {
    display: none;
  }
}
```

- [ ] **Step 9: Commit**

```bash
git add index.html
git commit -m "style: add CSS for 8-section landing page with alternating backgrounds

Adds section layout classes (section-dark, section-light, fades),
CTA hierarchy styles (cta-primary/secondary/subtle), How It Works
4-column grid, value props category styles, comparison table,
use case grid, reveal animations, and mobile breakpoints.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 2: Implement Section 1 — Hero CTAs and functional subtitle

**Files:**
- Modify: `index.html:437-474` (hero content area)
- Modify: `index.html:523-553` (i18n — add hero subtitle + CTA labels)

- [ ] **Step 1: Add functional subtitle and CTAs to the hero content area**

Replace the current `.content` div (lines 446-449) with:

```html
<div class="content">
  <h1 class="title" id="title">COLMENA</h1>
  <p class="tagline" id="tagline"></p>
  <p class="tagline-functional" id="taglineFunctional" data-i18n="tagline_functional"></p>
  <div class="hero-cta-group">
    <a href="#quickstart" class="cta-primary" data-i18n="cta_quickstart">Quick Start</a>
    <a href="https://github.com/4rth4s/colmena" target="_blank" rel="noopener" class="cta-secondary" data-i18n="cta_star">Star on GitHub</a>
    <span class="cta-divider">|</span>
    <a href="https://github.com/4rth4s/colmena#readme" target="_blank" rel="noopener" class="cta-subtle" data-i18n="cta_docs">Read the Docs &rarr;</a>
  </div>
</div>
```

- [ ] **Step 2: Add CSS for functional subtitle**

Add inside `<style>` after `.tagline` rule:

```css
.tagline-functional {
  font-size: clamp(0.8rem, 1.8vw, 1rem);
  font-weight: 400;
  color: rgba(245,240,232,0.6);
  letter-spacing: 0.05em;
  margin-top: 0.6rem;
  min-height: 1.3em;
  text-shadow: 0 0 20px rgba(0,0,0,0.9), 0 0 40px rgba(0,0,0,0.7);
  transition: opacity 0.8s ease;
  opacity: 0;
}

.tagline-functional.visible {
  opacity: 1;
}
```

- [ ] **Step 3: Add hero CTA group to fade behavior**

The hero `.content` div already fades on scroll. Since CTAs are inside `.content`, they inherit the fade. Verify the existing scroll handler at line 1290-1298 covers `.content` — it does (`heroContent.style.opacity = opacity`).

- [ ] **Step 4: Add i18n keys for hero functional subtitle and CTAs**

Add to `I18N.en`:

```js
tagline_functional: 'Define missions. Enforce policy. Earn trust. All local.',
cta_quickstart: 'Quick Start',
cta_star: '★ Star on GitHub',
cta_docs: 'Read the Docs →',
```

Add to `I18N.es`:

```js
tagline_functional: 'Define misiones. Enforcea política. Gana confianza. Todo local.',
cta_quickstart: 'Inicio Rápido',
cta_star: '★ Estrella en GitHub',
cta_docs: 'Lee la Documentación →',
```

- [ ] **Step 5: Wire up functional subtitle reveal in the animation JS**

In the phase transition block (around line 1177), after `REVEAL_TAGLINE` and `typeTagline()`, add reveal for the functional subtitle. The simplest approach: add a `setTimeout` to reveal the functional subtitle ~1 second after the typewriter starts. Insert after line 1177:

```js
if (phase === Phase.REVEAL_TAGLINE && t >= 7.2) {
  const el = document.getElementById('taglineFunctional');
  if (el) el.classList.add('visible');
}
```

And update the `applyLang` function (around line 578) to also update the functional subtitle text when language changes:

```js
// Inside applyLang(), after updating tagline:
const fnEl = document.getElementById('taglineFunctional');
if (fnEl) {
  fnEl.textContent = I18N[lang].tagline_functional;
}
```

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: add functional subtitle and 3-CTAs to hero section

Adds 'Define missions. Enforce policy. Earn trust. All local.'
subtitle below the poetic tagline, plus Quick Start (primary),
Star on GitHub (secondary), and Read the Docs (tertiary) CTAs
visible in the hero viewport.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 3: Add bridge paragraph to manifesto and update section structure

**Files:**
- Modify: `index.html:458-473` (manifesto section)

- [ ] **Step 1: Add bridge paragraph after manifesto lines**

Replace the manifesto section (lines 464-473) with:

```html
<!-- Manifesto -->
<section class="manifesto">
  <div class="manifesto-line accent" data-i18n="m1">One operator. Many agents. Full control.</div>
  <div class="manifesto-line" data-i18n="m2">Built in Rust. Local-first. Zero cloud dependencies.</div>
  <div class="manifesto-line dim" data-i18n="m3">&lt; 15ms latency. Every tool call evaluated.</div>
  <div class="manifesto-line accent" data-i18n="m4">The hive defends the colony.</div>
  <p class="manifesto-bridge" data-i18n="manifesto_bridge"></p>
  <a href="https://github.com/4rth4s/colmena" target="_blank" rel="noopener" class="repo-link">
    <svg viewBox="0 0 24 24"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0024 12c0-6.63-5.37-12-12-12z"/></svg>
    <span data-i18n="cta">View on GitHub</span>
  </a>
</section>
```

- [ ] **Step 2: Add CSS for bridge paragraph**

```css
.manifesto-bridge {
  font-size: clamp(0.85rem, 1.6vw, 0.95rem);
  font-weight: 400;
  color: rgba(245,240,232,0.45);
  letter-spacing: 0.03em;
  margin-top: 2rem;
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.8s ease, transform 0.8s ease;
  text-align: center;
  max-width: 500px;
  line-height: 1.6;
}

.manifesto-bridge.revealed {
  opacity: 1;
  transform: translateY(0);
}
```

- [ ] **Step 3: Add i18n keys**

```js
// EN
manifesto_bridge: 'Colmena doesn\'t replace your judgment. It codifies it into deterministic rules, executes them in under 15ms, and gives you an audited log of every decision — so you can trust your swarm without watching every move.',

// ES
manifesto_bridge: 'Colmena no reemplaza tu criterio. Lo codifica en reglas deterministas, lo ejecuta en menos de 15ms, y te da un log auditado de cada decisión — para que puedas confiar en tu enjambre sin tener que vigilar cada movimiento.',
```

- [ ] **Step 4: Add bridge paragraph to IntersectionObserver**

The existing observer at line 1301 watches `.manifesto-line, .repo-link`. Add `.manifesto-bridge` to that selector:

```js
const manifestoItems = document.querySelectorAll('.manifesto-line, .repo-link, .manifesto-bridge');
```

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add manifesto bridge paragraph connecting poetry to function

Adds a transition paragraph after the manifesto lines that explains
Colmena's philosophy in plain terms — deterministic rules, <15ms
execution, audited decisions — bridging poetic intro to technical depth.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 4: Implement Section 2 — The False Tradeoff

**Files:**
- Modify: `index.html:~475` (after manifesto + fade)

- [ ] **Step 1: Replace old value-props section block start**

After the manifesto section and the fade-to-black div, replace everything from the old `.value-props` down to `.cta-footer` with the new sections. For this task, just add Section 2 HTML after the fade-to-black:

```html
<!-- Section 2: The False Tradeoff -->
<section class="section-light">
  <div class="section-pad">
    <div class="section-inner">
      <div class="section-eyebrow reveal-item" data-i18n="s2_eyebrow">The false tradeoff</div>
      <h2 class="section-heading reveal-item" data-i18n="s2_heading">AI agents need autonomy.<br>You need control.<br>You shouldn't have to choose.</h2>

      <div class="tradeoff-grid reveal-item">
        <div class="tradeoff-box">
          <div class="tradeoff-box-label" data-i18n="s2_box1_label">Manual approval</div>
          <div class="tradeoff-box-desc" data-i18n="s2_box1_desc">Approve every tool call by hand. Safe, but doesn't scale past one agent.</div>
        </div>
        <div class="tradeoff-vs">vs</div>
        <div class="tradeoff-box">
          <div class="tradeoff-box-label" data-i18n="s2_box2_label">Blanket allow</div>
          <div class="tradeoff-box-desc" data-i18n="s2_box2_desc">--dangerously-skip-permissions. Scales, but you're trusting a black box.</div>
        </div>
      </div>

      <div class="tradeoff-resolution reveal-item">
        <p class="tradeoff-resolution-title" data-i18n="s2_resolution_title">Colmena: scoped autonomy.</p>
        <p style="font-size:0.9rem;line-height:1.55;opacity:0.75;" data-i18n="s2_resolution_desc">Each agent is free <strong>within its domain</strong>. The firewall enforces the boundaries. Every mission has a start, an end, and an evaluation. You stay in control — without the bottleneck.</p>
      </div>

      <p style="text-align:center;margin-top:2rem;font-size:0.95rem;opacity:0.6;font-family:'Playfair Display',serif;font-style:italic;" class="reveal-item" data-i18n="s2_closing">policy is code, review is mandatory, trust is earned.</p>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Adjust heading font size for section-heading in light sections**

The `.section-heading` rule needs a `font-size`. Add to the existing `.section-heading` block:

```css
.section-heading {
  /* ... existing props ... */
  font-size: clamp(1.2rem, 3vw, 1.6rem);
}
```

- [ ] **Step 3: Add i18n keys for EN**

```js
s2_eyebrow: 'The false tradeoff',
s2_heading_html: 'AI agents need autonomy.<br>You need control.<br>You shouldn\'t have to choose.',
s2_box1_label: 'Manual approval',
s2_box1_desc: 'Approve every tool call by hand. Safe, but doesn\'t scale past one agent.',
s2_box2_label: 'Blanket allow',
s2_box2_desc: '--dangerously-skip-permissions. Scales, but you\'re trusting a black box.',
s2_resolution_title: 'Colmena: scoped autonomy.',
s2_resolution_desc_html: 'Each agent is free <strong>within its domain</strong>. The firewall enforces the boundaries. Every mission has a start, an end, and an evaluation. You stay in control — without the bottleneck.',
s2_closing: 'policy is code, review is mandatory, trust is earned.',
```

- [ ] **Step 4: Add i18n keys for ES**

```js
s2_eyebrow: 'La falsa dicotomía',
s2_heading_html: 'Los agentes IA necesitan autonomía.<br>Vos necesitás control.<br>No deberías tener que elegir.',
s2_box1_label: 'Aprobación manual',
s2_box1_desc: 'Aprobás cada tool call a mano. Seguro, pero no escala más allá de un agente.',
s2_box2_label: 'Permiso total',
s2_box2_desc: '--dangerously-skip-permissions. Escala, pero estás confiando en una caja negra.',
s2_resolution_title: 'Colmena: autonomía con alcance.',
s2_resolution_desc_html: 'Cada agente es libre <strong>dentro de su dominio</strong>. El firewall impone los límites. Cada misión tiene un inicio, un fin y una evaluación. Vos mantenés el control — sin ser el cuello de botella.',
s2_closing: 'la política es código, la revisión es obligatoria, la confianza se gana.',
```

- [ ] **Step 5: Handle i18n for innerHTML elements**

Update the `applyLang` function to handle `_html` suffixed keys. The existing code already does this at line 570:

```js
const htmlKey = `${key}_html`;
if (I18N[lang][htmlKey]) {
  el.innerHTML = I18N[lang][htmlKey];
}
```

This already works for elements with `data-i18n="s2_heading"` → looks up `s2_heading_html` → sets innerHTML. The existing logic handles it. No change needed.

- [ ] **Step 6: Wire up the section-heading to use innerHTML for br tags**

Change the `data-i18n` attribute for headings that use `<br>`:

```html
<!-- Use data-i18n key that matches _html lookup -->
<h2 class="section-heading reveal-item" data-i18n="s2_heading">AI agents need autonomy.<br>You need control.<br>You shouldn't have to choose.</h2>
```

The fallback text in HTML will be overwritten by `applyLang`, so it doesn't matter which language is in the HTML source. Keep English as default.

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "feat: add Section 2 — The False Tradeoff (autonomy vs control)

Adds light-background section explaining the core tension Colmena
resolves: manual approval doesn't scale, blanket allow is blind trust.
Positions 'scoped autonomy' as the synthesis. Bridges manifesto
poetry to technical depth.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 5: Implement Section 3 — How It Works with Mission Manifest

**Files:**
- Modify: `index.html` (add HTML after Section 2, before Section 4)

- [ ] **Step 1: Add fade transition and Section 3 HTML**

After Section 2's closing `</section>`, add:

```html
<!-- Fade to dark -->
<div class="fade-light-to-dark"></div>

<!-- Section 3: How It Works -->
<section class="section-dark">
  <div class="section-pad">
    <div class="section-inner" style="max-width:960px;">
      <div class="section-eyebrow reveal-item" data-i18n="s3_eyebrow">How it works</div>
      <h2 class="section-heading reveal-item" data-i18n="s3_heading">One YAML file. One command. Full accountability.</h2>

      <div class="how-grid">
        <!-- Step 1: Define (expanded) -->
        <div class="how-step how-step-highlight reveal-item">
          <div class="how-step-num">1</div>
          <div class="how-step-title" data-i18n="s3_step1_title">Define</div>
          <div class="how-step-desc" data-i18n="s3_step1_desc">Write a YAML manifest. Pick roles, scope paths, bash patterns, set a TTL.</div>
          <div class="yaml-preview">mission: review-auth
pattern: code-review-cycle
roles:
  - name: developer
    scope:
      paths: [src/auth/]
  - name: auditor</div>
          <div class="how-check-list">✓ Auto-validates schema + security
✓ Generates role prompts + subagent files
✓ Creates scoped delegations with TTL
✓ Wires bash_patterns into runtime overrides
✓ --dry-run to preview before executing</div>
          <div class="how-step-artifact">mission.yaml</div>
        </div>

        <!-- Step 2: Spawn -->
        <div class="how-step reveal-item">
          <div class="how-step-num">2</div>
          <div class="how-step-title" data-i18n="s3_step2_title">Spawn</div>
          <div class="how-step-desc" data-i18n="s3_step2_desc">One command spawns the squad. Each agent gets its role, its tools, its boundaries.</div>
          <div class="how-step-artifact">colmena mission spawn</div>
        </div>

        <!-- Step 3: Execute -->
        <div class="how-step reveal-item">
          <div class="how-step-num">3</div>
          <div class="how-step-title" data-i18n="s3_step3_title">Execute</div>
          <div class="how-step-desc" data-i18n="s3_step3_desc">Agents work in parallel. Every tool call evaluated in &lt;15ms. Audit.log on every decision.</div>
          <div class="how-step-artifact">ALLOW · ASK · BLOCK</div>
        </div>

        <!-- Step 4: Review -->
        <div class="how-step reveal-item">
          <div class="how-step-num">4</div>
          <div class="how-step-title" data-i18n="s3_step4_title">Review</div>
          <div class="how-step-desc" data-i18n="s3_step4_desc">Auditor scores every artifact. ELO adjusts. Trust is earned — not declared.</div>
          <div class="how-step-artifact">QPC scoring</div>
        </div>
      </div>

      <!-- From-history callout -->
      <p class="reveal-item" style="text-align:center;margin-top:1.2rem;font-size:0.75rem;opacity:0.35;color:var(--bone);" data-i18n="s3_history_hint">Already running agents? mission init --from-history reads audit.log and proposes a manifest from your patterns.</p>

      <!-- Closing line -->
      <p class="reveal-item" style="text-align:center;margin-top:1rem;opacity:0.25;font-family:monospace;font-size:0.75rem;color:var(--bone);">Define → Spawn → Execute → Review → repeat, with better trust each cycle</p>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add i18n keys EN**

```js
s3_eyebrow: 'How it works',
s3_heading: 'One YAML file. One command. Full accountability.',
s3_step1_title: 'Define',
s3_step1_desc: 'Write a YAML manifest. Pick roles, scope paths, bash patterns, set a TTL.',
s3_step2_title: 'Spawn',
s3_step2_desc: 'One command spawns the squad. Each agent gets its role, its tools, its boundaries.',
s3_step3_title: 'Execute',
s3_step3_desc: 'Agents work in parallel. Every tool call evaluated in <15ms. Audit.log on every decision.',
s3_step4_title: 'Review',
s3_step4_desc: 'Auditor scores every artifact. ELO adjusts. Trust is earned — not declared.',
s3_history_hint: 'Already running agents? mission init --from-history reads audit.log and proposes a manifest from your patterns.',
```

- [ ] **Step 3: Add i18n keys ES**

```js
s3_eyebrow: 'Cómo funciona',
s3_heading: 'Un archivo YAML. Un comando. Responsabilidad total.',
s3_step1_title: 'Define',
s3_step1_desc: 'Escribí un manifiesto YAML. Elegí roles, scope paths, bash patterns, definí un TTL.',
s3_step2_title: 'Despliega',
s3_step2_desc: 'Un comando lanza el equipo. Cada agente recibe su rol, sus herramientas, sus límites.',
s3_step3_title: 'Ejecuta',
s3_step3_desc: 'Los agentes trabajan en paralelo. Cada tool call evaluado en <15ms. Audit.log en cada decisión.',
s3_step4_title: 'Revisa',
s3_step4_desc: 'El auditor califica cada artefacto. El ELO se ajusta. La confianza se gana — no se declara.',
s3_history_hint: '¿Ya tenés agentes corriendo? mission init --from-history lee audit.log y propone un manifiesto desde tus patrones.',
```

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add Section 3 — How It Works with Mission Manifest YAML

4-step flow (Define→Spawn→Execute→Review) with expanded Step 1
showing YAML manifest preview, auto-validation checklist, and
--from-history callout. Dark background section.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 6: Implement Section 4 — Expanded Value Props (4 categories, 17 bullets)

**Files:**
- Modify: `index.html` (add HTML after Section 3)

- [ ] **Step 1: Add fade transition and Section 4 HTML**

After Section 3's closing `</section>`, add:

```html
<!-- Fade to light -->
<div class="fade-dark-to-light"></div>

<!-- Section 4: Value Props -->
<section class="section-light">
  <div class="section-pad">
    <div class="section-inner">
      <div class="section-eyebrow reveal-item" data-i18n="s4_eyebrow">What Colmena gives you</div>

      <!-- Firewall & Policy -->
      <div class="vp-category reveal-item">
        <h3 class="vp-category-title" data-i18n="s4_cat1_title">Firewall &amp; Policy</h3>
        <div class="vp-bullet-grid">
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat1_b1">Deterministic YAML rules + compiled regex engine</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat1_b2">&lt;15ms hot path — zero LLM calls, zero per-call cost</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat1_b3">Shell chain guard — blocks &amp;&amp;, ||, ;, $(), backticks</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat1_b4">Mission kill-switch — instant revocation of all delegations</span></div>
        </div>
      </div>

      <!-- Multi-Agent Missions -->
      <div class="vp-category reveal-item">
        <h3 class="vp-category-title" data-i18n="s4_cat2_title">Multi-Agent Missions</h3>
        <div class="vp-bullet-grid">
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat2_b1">One-command squad spawn with scoped autonomy</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat2_b2">11 orchestration patterns + 7 topologies</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat2_b3">15 built-in roles — pentest, dev, devops, SRE, reviewer...</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat2_b4">Mission Manifest YAML — define once, spawn anywhere</span></div>
        </div>
      </div>

      <!-- Trust & Accountability -->
      <div class="vp-category reveal-item">
        <h3 class="vp-category-title" data-i18n="s4_cat3_title">Trust &amp; Accountability</h3>
        <div class="vp-bullet-grid">
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat3_b1">Mandatory auditor review — no artifact ships unreviewed</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat3_b2">QPC scoring: Quality + Precision + Comprehensiveness</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat3_b3">ELO trust calibration — 5 tiers, earned from behavior</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat3_b4">Append-only audit.log + findings store + alerts</span></div>
        </div>
      </div>

      <!-- Efficiency & Control -->
      <div class="vp-category reveal-item">
        <h3 class="vp-category-title" data-i18n="s4_cat4_title">Efficiency &amp; Control</h3>
        <div class="vp-bullet-grid">
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat4_b1">Output filtering — 30-50% token savings on noisy commands</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat4_b2">Terse inter-agent protocol — facts + path:line, minimal tokens</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat4_b3">Runtime delegations with mandatory TTL (max 24h)</span></div>
          <div class="vp-bullet"><span class="vp-bullet-dot">&#8226;</span> <span data-i18n="s4_cat4_b4">Custom roles &amp; patterns — library is yours to extend</span></div>
        </div>
      </div>

    </div>
  </div>
</section>
```

- [ ] **Step 2: Add i18n keys EN** (17 bullet strings + 4 category titles + 1 eyebrow)

```js
s4_eyebrow: 'What Colmena gives you',
s4_cat1_title: 'Firewall & Policy',
s4_cat1_b1: 'Deterministic YAML rules + compiled regex engine',
s4_cat1_b2: '<15ms hot path — zero LLM calls, zero per-call cost',
s4_cat1_b3: 'Shell chain guard — blocks &&, ||, ;, $(), backticks',
s4_cat1_b4: 'Mission kill-switch — instant revocation of all delegations',
s4_cat2_title: 'Multi-Agent Missions',
s4_cat2_b1: 'One-command squad spawn with scoped autonomy',
s4_cat2_b2: '11 orchestration patterns + 7 topologies',
s4_cat2_b3: '15 built-in roles — pentest, dev, devops, SRE, reviewer...',
s4_cat2_b4: 'Mission Manifest YAML — define once, spawn anywhere',
s4_cat3_title: 'Trust & Accountability',
s4_cat3_b1: 'Mandatory auditor review — no artifact ships unreviewed',
s4_cat3_b2: 'QPC scoring: Quality + Precision + Comprehensiveness',
s4_cat3_b3: 'ELO trust calibration — 5 tiers, earned from behavior',
s4_cat3_b4: 'Append-only audit.log + findings store + alerts',
s4_cat4_title: 'Efficiency & Control',
s4_cat4_b1: 'Output filtering — 30-50% token savings on noisy commands',
s4_cat4_b2: 'Terse inter-agent protocol — facts + path:line, minimal tokens',
s4_cat4_b3: 'Runtime delegations with mandatory TTL (max 24h)',
s4_cat4_b4: 'Custom roles & patterns — library is yours to extend',
```

- [ ] **Step 3: Add i18n keys ES**

```js
s4_eyebrow: 'Qué te da Colmena',
s4_cat1_title: 'Firewall y Política',
s4_cat1_b1: 'Reglas YAML deterministas + motor de regex compilado',
s4_cat1_b2: '<15ms en hot path — cero llamadas LLM, cero costo por llamada',
s4_cat1_b3: 'Chain guard del shell — bloquea &&, ||, ;, $(), backticks',
s4_cat1_b4: 'Kill-switch de misión — revocación instantánea de todas las delegaciones',
s4_cat2_title: 'Misiones Multi-Agente',
s4_cat2_b1: 'Despliegue de equipos con un comando y autonomía acotada',
s4_cat2_b2: '11 patrones de orquestación + 7 topologías',
s4_cat2_b3: '15 roles predefinidos — pentest, dev, devops, SRE, reviewer...',
s4_cat2_b4: 'Mission Manifest YAML — definí una vez, desplegá donde quieras',
s4_cat3_title: 'Confianza y Accountability',
s4_cat3_b1: 'Revisión de auditoría obligatoria — nada se publica sin revisión',
s4_cat3_b2: 'Scoring QPC: Quality + Precision + Comprehensiveness',
s4_cat3_b3: 'Calibración de confianza ELO — 5 niveles, ganados con comportamiento',
s4_cat3_b4: 'Audit.log + findings store + alerts — todo append-only',
s4_cat4_title: 'Eficiencia y Control',
s4_cat4_b1: 'Filtrado de output — 30-50% de ahorro de tokens en comandos ruidosos',
s4_cat4_b2: 'Protocolo inter-agente escueto — hechos + path:line, tokens mínimos',
s4_cat4_b3: 'Delegaciones runtime con TTL obligatorio (máx 24h)',
s4_cat4_b4: 'Roles y patrones personalizados — la biblioteca es tuya para extender',
```

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add Section 4 — expanded value props (4 categories, 17 bullets)

Replaces the old 3-item value props with 4 categories: Firewall &
Policy, Multi-Agent Missions, Trust & Accountability, Efficiency &
Control. Each has 4 specific, verifiable capability bullets with
left gold border styling on light background.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 7: Implement Section 5 — Better Together (comparison table)

**Files:**
- Modify: `index.html` (add HTML after Section 4)

- [ ] **Step 1: Add fade transition and Section 5 HTML**

After Section 4's closing `</section>`, add:

```html
<!-- Fade to dark -->
<div class="fade-light-to-dark"></div>

<!-- Section 5: Better Together -->
<section class="section-dark">
  <div class="section-pad">
    <div class="section-inner">
      <div class="section-eyebrow reveal-item" data-i18n="s5_eyebrow">Better together</div>
      <h2 class="section-heading reveal-item" data-i18n="s5_heading">Colmena + Claude Code auto-mode.<br>Different layers. Same mission.</h2>

      <div class="compare-table-wrap reveal-item">
        <table class="compare-table">
          <thead>
            <tr>
              <th data-i18n="s5_dim">Dimension</th>
              <th data-i18n="s5_am">Auto-Mode</th>
              <th data-i18n="s5_col">Colmena</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td data-i18n="s5_r1_dim">Decision model</td>
              <td data-i18n="s5_r1_am">Probabilistic (LLM)</td>
              <td data-i18n="s5_r1_col">Deterministic (YAML rules)</td>
            </tr>
            <tr>
              <td data-i18n="s5_r2_dim">Per-call cost</td>
              <td data-i18n="s5_r2_am">Model tokens per classification</td>
              <td data-i18n="s5_r2_col">Zero — no network, no LLM</td>
            </tr>
            <tr>
              <td data-i18n="s5_r3_dim">Explainability</td>
              <td data-i18n="s5_r3_am">Classifier output (opaque)</td>
              <td data-i18n="s5_r3_col">Rule ID in audit.log</td>
            </tr>
            <tr>
              <td data-i18n="s5_r4_dim">Scope</td>
              <td data-i18n="s5_r4_am">Single-agent intent</td>
              <td data-i18n="s5_r4_col">Single + multi-agent with auditor</td>
            </tr>
            <tr>
              <td data-i18n="s5_r5_dim">Accountability</td>
              <td data-i18n="s5_r5_am">Per tool call</td>
              <td data-i18n="s5_r5_col">Per agent + per role, ELO over time</td>
            </tr>
            <tr>
              <td data-i18n="s5_r6_dim">Storage</td>
              <td data-i18n="s5_r6_am">Cloud-side</td>
              <td data-i18n="s5_r6_col">Local filesystem (git-versionable)</td>
            </tr>
          </tbody>
        </table>
      </div>

      <p class="reveal-item" style="text-align:center;margin-top:2rem;padding-top:1.5rem;border-top:1px solid rgba(212,160,23,0.15);max-width:550px;margin-left:auto;margin-right:auto;font-size:0.9rem;line-height:1.6;opacity:0.7;color:var(--bone);" data-i18n="s5_closing_html"><strong style="color:#D4A017;">Auto-mode</strong> catches semantic intent the rule base can't — prompt injection, mass-delete nudges, context the agent shouldn't have.<br><br><strong style="color:#D4A017;">Colmena</strong> enforces the policy you wrote and keeps a tamper-evident local record.<br><br><span style="font-family:'Playfair Display',serif;font-style:italic;color:#B8860B;">Use them together. They're designed to be.</span></p>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add i18n keys EN**

```js
s5_eyebrow: 'Better together',
s5_heading: 'Colmena + Claude Code auto-mode.<br>Different layers. Same mission.',
s5_dim: 'Dimension',
s5_am: 'Auto-Mode',
s5_col: 'Colmena',
s5_r1_dim: 'Decision model', s5_r1_am: 'Probabilistic (LLM)', s5_r1_col: 'Deterministic (YAML rules)',
s5_r2_dim: 'Per-call cost', s5_r2_am: 'Model tokens per classification', s5_r2_col: 'Zero — no network, no LLM',
s5_r3_dim: 'Explainability', s5_r3_am: 'Classifier output (opaque)', s5_r3_col: 'Rule ID in audit.log',
s5_r4_dim: 'Scope', s5_r4_am: 'Single-agent intent', s5_r4_col: 'Single + multi-agent with auditor',
s5_r5_dim: 'Accountability', s5_r5_am: 'Per tool call', s5_r5_col: 'Per agent + per role, ELO over time',
s5_r6_dim: 'Storage', s5_r6_am: 'Cloud-side', s5_r6_col: 'Local filesystem (git-versionable)',
s5_closing_html: '<strong style="color:#D4A017;">Auto-mode</strong> catches semantic intent the rule base can\'t — prompt injection, mass-delete nudges, context the agent shouldn\'t have.<br><br><strong style="color:#D4A017;">Colmena</strong> enforces the policy you wrote and keeps a tamper-evident local record.<br><br><span style="font-family:\'Playfair Display\',serif;font-style:italic;color:#B8860B;">Use them together. They\'re designed to be.</span>',
```

- [ ] **Step 3: Add i18n keys ES**

```js
s5_eyebrow: 'Mejor juntos',
s5_heading: 'Colmena + Claude Code auto-mode.<br>Capas diferentes. Misma misión.',
s5_dim: 'Dimensión',
s5_am: 'Auto-Mode',
s5_col: 'Colmena',
s5_r1_dim: 'Modelo de decisión', s5_r1_am: 'Probabilístico (LLM)', s5_r1_col: 'Determinista (reglas YAML)',
s5_r2_dim: 'Costo por llamada', s5_r2_am: 'Tokens del modelo por clasificación', s5_r2_col: 'Cero — sin red, sin LLM',
s5_r3_dim: 'Explicabilidad', s5_r3_am: 'Salida opaca del clasificador', s5_r3_col: 'Rule ID en audit.log',
s5_r4_dim: 'Alcance', s5_r4_am: 'Intención de un solo agente', s5_r4_col: 'Uno + multi-agente con auditor',
s5_r5_dim: 'Accountability', s5_r5_am: 'Por tool call', s5_r5_col: 'Por agente + por rol, ELO en el tiempo',
s5_r6_dim: 'Almacenamiento', s5_r6_am: 'En la nube', s5_r6_col: 'Sistema de archivos local (git-versionable)',
s5_closing_html: '<strong style="color:#D4A017;">Auto-mode</strong> detecta intención semántica que las reglas no pueden — intentos de prompt injection, sugerencias de borrado masivo, contexto que el agente no debería tener.<br><br><strong style="color:#D4A017;">Colmena</strong> hace cumplir la política que escribiste y mantiene un registro local a prueba de manipulaciones.<br><br><span style="font-family:\'Playfair Display\',serif;font-style:italic;color:#B8860B;">Usalas juntas. Están diseñadas para eso.</span>',
```

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add Section 5 — Better Together (Colmena vs Auto-Mode)

Honest 6-dimension comparison table showing Colmena and Claude Code
auto-mode as complementary, not competing. Deterministic vs
probabilistic, local vs cloud, per-agent ELO vs per-call.
Closes with 'Use them together. They're designed to be.'

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 8: Implement Section 6 — Use Cases grid

**Files:**
- Modify: `index.html` (add HTML after Section 5)

- [ ] **Step 1: Add fade transition and Section 6 HTML**

```html
<!-- Fade to light -->
<div class="fade-dark-to-light"></div>

<!-- Section 6: Use Cases -->
<section class="section-light">
  <div class="section-pad">
    <div class="section-inner">
      <div class="section-eyebrow reveal-item" data-i18n="s6_eyebrow">Who is this for?</div>
      <h2 class="section-heading reveal-item" data-i18n="s6_heading">Pick your role. Colmena adapts.</h2>

      <div class="usecase-grid">
        <div class="usecase-card reveal-item">
          <div class="usecase-icon">&#x2694;&#xFE0F;</div>
          <div class="usecase-title" data-i18n="s6_u1_title">Pentest engagement</div>
          <div class="usecase-desc" data-i18n="s6_u1_desc">Scoped Caido-native agents. Web + API surface covered. Findings store for triage.</div>
          <div class="usecase-flow">Spawn → Scan → Find → Report → Auditor</div>
        </div>

        <div class="usecase-card reveal-item">
          <div class="usecase-icon">&#x1F4BB;</div>
          <div class="usecase-title" data-i18n="s6_u2_title">Code review cycle</div>
          <div class="usecase-desc" data-i18n="s6_u2_desc">Developer → Reviewer → Auditor. Reviewer is read-only. QPC scored. ELO tracking.</div>
          <div class="usecase-flow">Implement → Review → Score → Calibrate</div>
        </div>

        <div class="usecase-card reveal-item">
          <div class="usecase-icon">&#x2699;&#xFE0F;</div>
          <div class="usecase-title" data-i18n="s6_u3_title">DevOps kubectl ops</div>
          <div class="usecase-desc" data-i18n="s6_u3_desc">kubectl + helm + terraform pre-approved. Secrets blocked. Destructive ops gated.</div>
          <div class="usecase-flow">Delegate → Apply → Audit.log → Deactivate</div>
        </div>

        <div class="usecase-card reveal-item">
          <div class="usecase-icon">&#x1F4E1;</div>
          <div class="usecase-title" data-i18n="s6_u4_title">SRE runbook execution</div>
          <div class="usecase-desc" data-i18n="s6_u4_desc">Read-side ops pre-approved. Writes gated. Alerts append-only. Runbook-friendly.</div>
          <div class="usecase-flow">Investigate → Diagnose → Draft → Incident note</div>
        </div>
      </div>

      <p class="usecase-footer reveal-item" data-i18n="s6_footer">Not your profile? Colmena is domain-agnostic. Create your own roles and patterns.</p>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add i18n keys EN and ES**

EN:
```js
s6_eyebrow: 'Who is this for?',
s6_heading: 'Pick your role. Colmena adapts.',
s6_u1_title: 'Pentest engagement',
s6_u1_desc: 'Scoped Caido-native agents. Web + API surface covered. Findings store for triage.',
s6_u2_title: 'Code review cycle',
s6_u2_desc: 'Developer → Reviewer → Auditor. Reviewer is read-only. QPC scored. ELO tracking.',
s6_u3_title: 'DevOps kubectl ops',
s6_u3_desc: 'kubectl + helm + terraform pre-approved. Secrets blocked. Destructive ops gated.',
s6_u4_title: 'SRE runbook execution',
s6_u4_desc: 'Read-side ops pre-approved. Writes gated. Alerts append-only. Runbook-friendly.',
s6_footer: 'Not your profile? Colmena is domain-agnostic. Create your own roles and patterns.',
```

ES:
```js
s6_eyebrow: '¿Para quién es?',
s6_heading: 'Elegí tu rol. Colmena se adapta.',
s6_u1_title: 'Pentest',
s6_u1_desc: 'Agentes nativos Caido con alcance definido. Cobertura web + API. Findings store para triage.',
s6_u2_title: 'Ciclo de code review',
s6_u2_desc: 'Developer → Reviewer → Auditor. El reviewer es solo lectura. Scoring QPC. Seguimiento ELO.',
s6_u3_title: 'DevOps kubectl ops',
s6_u3_desc: 'kubectl + helm + terraform pre-aprobados. Secrets bloqueados. Operaciones destructivas con puerta.',
s6_u4_title: 'SRE ejecución de runbooks',
s6_u4_desc: 'Operaciones de lectura pre-aprobadas. Escrituras con puerta. Alertas append-only.',
s6_footer: '¿No es tu perfil? Colmena es domínio-agnóstico. Creá tus propios roles y patrones.',
```

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Section 6 — Use Cases grid (2x2, 4 profiles)

Pentest, Code Review, DevOps, and SRE use cases in a 2x2 card grid
on light background. Each card shows icon, title, description, and
mini flow. Footer invites custom role/pattern creation.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 9: Implement Sections 7 & 8 — Quick Start + CTA Footer

**Files:**
- Modify: `index.html` (add HTML after Section 6)

- [ ] **Step 1: Add fade transition, Quick Start, and CTA Footer HTML**

```html
<!-- Fade to dark -->
<div class="fade-light-to-dark"></div>

<!-- Section 7: Quick Start -->
<section class="section-dark" id="quickstart">
  <div class="section-pad">
    <div class="section-inner" style="max-width:720px;">
      <div class="section-eyebrow reveal-item" data-i18n="s7_eyebrow">Start in 30 seconds</div>

      <pre class="quickstart-code reveal-item"><span class="prompt">$</span>cargo install colmena-cli colmena-mcp
<span class="prompt">$</span>colmena setup
<span class="prompt">$</span>colmena doctor</pre>

      <p class="reveal-item" style="text-align:center;margin-top:1rem;font-size:0.8rem;opacity:0.4;color:var(--bone);" data-i18n="s7_sub">Two commands after install. The firewall is active from now on.</p>
    </div>
  </div>
</section>

<!-- Section 8: CTA Footer -->
<section class="section-dark" style="padding-bottom:6rem;">
  <div class="cta-group reveal-item" style="text-align:center;">
    <div class="section-eyebrow reveal-item" data-i18n="s8_eyebrow">Ready to trust your swarm?</div>
    <a href="#quickstart" class="cta-primary reveal-item" data-i18n="s8_cta_primary">Quick Start Guide &rarr;</a>
    <div class="cta-row reveal-item" style="margin-top:0.75rem;">
      <a href="https://github.com/4rth4s/colmena" target="_blank" rel="noopener" class="cta-secondary">
        <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2l2.955 6.728L22 9.751l-5.334 4.717L18.18 22 12 18.27 5.82 22l1.514-7.532L2 9.751l7.045-1.023L12 2z"/></svg>
        <span data-i18n="s8_cta_secondary">Star Colmena on GitHub</span>
      </a>
      <span class="cta-divider">|</span>
      <a href="https://github.com/4rth4s/colmena#readme" target="_blank" rel="noopener" class="cta-subtle" data-i18n="s8_cta_tertiary">Read the Docs &rarr;</a>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add i18n keys EN and ES**

EN:
```js
s7_eyebrow: 'Start in 30 seconds',
s7_sub: 'Two commands after install. The firewall is active from now on.',
s8_eyebrow: 'Ready to trust your swarm?',
s8_cta_primary: 'Quick Start Guide →',
s8_cta_secondary: 'Star Colmena on GitHub',
s8_cta_tertiary: 'Read the Docs →',
```

ES:
```js
s7_eyebrow: 'Empezá en 30 segundos',
s7_sub: 'Dos comandos después de instalar. El firewall está activo desde ahora.',
s8_eyebrow: '¿Listo para confiar en tu enjambre?',
s8_cta_primary: 'Guía de Inicio Rápido →',
s8_cta_secondary: 'Estrella en GitHub',
s8_cta_tertiary: 'Leé la Documentación →',
```

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Sections 7 & 8 — Quick Start + CTA Footer

3-command quick start block with 'Start in 30 seconds' eyebrow.
CTA footer with 3-tier hierarchy: Quick Start Guide (primary gold
button), Star on GitHub (secondary link), Read the Docs (tertiary).

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 10: Wire up IntersectionObserver for all new sections and final cleanup

**Files:**
- Modify: `index.html` (JS section — add observers for new content)

- [ ] **Step 1: Add IntersectionObserver for all `.reveal-item` elements**

At the end of the `<script>` block, after the existing content observer, add:

```js
// === NEW SECTIONS REVEAL — IntersectionObserver for all reveal-item elements ===
const revealItems = document.querySelectorAll('.reveal-item');
const revealObserver = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      entry.target.classList.add('revealed');
      revealObserver.unobserve(entry.target);
    }
  });
}, { threshold: 0.1 });

revealItems.forEach(el => revealObserver.observe(el));
```

- [ ] **Step 2: Remove old sections that are being replaced**

The old `.value-props`, `.quickstart`, `.complement-note`, and `.cta-footer` sections need to be removed since their content is replaced by sections 4, 5, 6, 7, 8. Remove these HTML blocks.

Specifically, remove these sections from the HTML:
- Lines 477-496: Old `.value-props` section
- Lines 499-506: Old `.quickstart` section
- Lines 509-511: Old `.complement-note` section
- Lines 514-519: Old `.cta-footer` section

And remove the old observer selectors that referenced them. Replace:

```js
const contentItems = document.querySelectorAll('.value-row, .quickstart-inner, .complement-inner, .cta-link');
```

With:

```js
// Old content observers removed — content now handled by .reveal-item observers
```

- [ ] **Step 3: Remove old CSS that is no longer used**

Remove these CSS blocks (their elements are being deleted):
- `.value-props`, `.value-props-inner` (lines 239-248)
- `.value-row`, `.value-row.revealed`, `.value-row:last-of-type` (lines 267-283)
- `.value-index` (lines 285-293)
- `.value-body`, `.value-body strong` (lines 295-309)
- `.quickstart`, `.quickstart-inner`, `.quickstart-inner.revealed` (lines 312-332)
- `.quickstart-label` (lines 334-347)
- `.quickstart-code`, `.quickstart-code .prompt` (lines 349-357)
- `.complement-note`, `.complement-inner`, `.complement-inner.revealed` (lines 366-390)
- `.cta-footer` (lines 393-403)
- `.cta-link`, `.cta-link.revealed`, `.cta-link:hover`, `.cta-link svg` (lines 405-433)

Keep `.quickstart-code` and `.quickstart-code .prompt` styles — these are reused in the new Quick Start section.

- [ ] **Step 4: Verify the page loads without errors**

Open `index.html` in a browser and check:
- No console errors
- All sections are present in DOM
- Language toggle works
- Hero animation plays
- Scroll reveals trigger

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: wire up IntersectionObserver for all new sections + cleanup old

Adds reveal-on-scroll for all .reveal-item elements, removes old
value-props/quickstart/complement/cta HTML and CSS, cleans up old
observer selectors. Page now has 8 full sections with scroll reveals.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 11: Bilingual QA — verify EN/ES toggle works for all new content

**Files:**
- None modified. Verification only.

- [ ] **Step 1: Open page, switch to ES, scroll through all sections**

Check every section for untranslated text:
- Hero functional subtitle → ES
- Hero CTAs → ES
- Manifesto bridge paragraph → ES
- Section 2 (False Tradeoff) → all ES
- Section 3 (How It Works) → all ES
- Section 4 (Value Props) → all 4 categories, all 17 bullets → ES
- Section 5 (Better Together) → all table cells + closing → ES
- Section 6 (Use Cases) → all 4 cards + footer → ES
- Section 7 (Quick Start) → ES
- Section 8 (CTA Footer) → ES

- [ ] **Step 2: Verify localStorage persistence**

Switch to ES, reload page. Verify ES stays selected.

- [ ] **Step 3: Verify browser language auto-detection**

Clear localStorage. Verify EN/ES auto-detects from `navigator.language`.

- [ ] **Step 4: Fix any missing or broken i18n keys**

If any text is missing, add the key to both EN and ES in the I18N object.

- [ ] **Step 5: Commit any i18n fixes**

```bash
git add index.html
git commit -m "fix: complete bilingual i18n coverage for all 8 sections

Ensures all new content has both EN and ES translations. Fixes any
missing keys found during QA sweep.

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

### Task 12: Mobile responsive QA and final polish

**Files:**
- Possibly modify: `index.html` (CSS media queries)

- [ ] **Step 1: Test on mobile viewport (375px width)**

Check:
- Hero CTAs stack vertically
- How It Works grid collapses to single column
- False tradeoff boxes stack
- Value props bullet grid collapses to single column
- Comparison table is scrollable horizontally
- Use cases grid collapses to single column
- Quick Start code block is scrollable
- CTA footer stacks correctly
- Canvas animation adapts (particle count already set via `isMobile`)

- [ ] **Step 2: Test on tablet (768px width)**

Check 2-column layouts work correctly at intermediate breakpoints.

- [ ] **Step 3: Fix any layout issues**

Add/adjust media queries as needed.

- [ ] **Step 4: Test scroll performance**

With the canvas animation running and all sections loaded, scroll through the page. Check for jank. The canvas already uses `requestAnimationFrame` and clears each frame — should be fine.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "fix: mobile responsive polish for all 8 sections

Ensures single-column layouts on mobile, horizontal scroll for
comparison table, stacked CTAs, and correct spacing at all
breakpoints (375px, 768px, 1024px+).

Co-Authored-By: Claude Opus 4.7 <noreply@anthropic.com>"
```

---

## Post-Implementation Verification

After all tasks complete, verify:

- [ ] Page loads without console errors
- [ ] All 8 sections visible with alternating backgrounds
- [ ] EN/ES toggle works across all content
- [ ] Hero animation plays (canvas)
- [ ] Scroll reveals trigger for all sections
- [ ] Hero fades on scroll
- [ ] Mobile layout works at 375px
- [ ] All links point to correct destinations
- [ ] No visual regressions in existing animation/manifesto
- [ ] `git diff --stat master` shows changes to index.html only
