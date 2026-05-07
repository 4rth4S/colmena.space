# Colmena Landing Page Redesign — Design Spec

**Date:** 2026-05-06
**Status:** Approved
**Branch:** `docs/landing-content-v1`

## 1. Goals

Transform the current Colmena landing page into a more visual, high-impact page that communicates the full breadth of what Colmena does in a single glance. The page must serve two audiences simultaneously:

- **Open-source community** — developers discovering Colmena via GitHub, HN, or social media. They need to understand it instantly and feel compelled to star, try, or share.
- **Tech leads / Engineering managers** — evaluating tools for their teams. They need enough depth to justify adoption.

Position Colmena honestly: a local-first, deterministic governance layer that solves the false tradeoff between agent autonomy and operator control.

## 2. Audience & Positioning

| Audience | What they need |
|---|---|
| Developer / Pentester using Claude Code | Understand it in 10 seconds, try it in 30 |
| Tech lead / Manager | Enough architectural depth to trust it |
| Open-source visitor | Visual impact + social proof to star/share |

**Core narrative:** AI agents need autonomy. You need control. You shouldn't have to choose. Colmena gives each agent scoped freedom within its domain, enforces boundaries deterministically, and makes every mission auditable.

## 3. Tone Strategy

- **Hero / Manifesto:** Poetic, evocative. "The swarm learns where to sting." The hexagon animation stays.
- **Everything else:** Direct, technical, honest. Facts over fluff. "Define missions. Enforce policy. Earn trust. All local."

## 4. Visual Identity

- **Base palette:** `#000000` (black), `#D4A017` (gold-dark), `#FFB800` (amber), `#B8860B` (amber-muted), `#F5F0E8` (bone/cream)
- **Typography:** Playfair Display (serif) for headings and poetic text; system mono for code and technical content
- **Section rhythm:** Alternating dark (black) and light (bone) backgrounds
- **Motif:** Hexagon as the central visual element. Particle swarm animation preserved in hero

## 5. Page Structure

### Section 1 — Hero + Manifesto (dark background)

Full viewport hero with the existing hexagon particle animation.

**Content:**
- "COLMENA" — large, tracked-out title (existing)
- "The swarm learns where to sting." — typewriter tagline (existing, poetic)
- "Define missions. Enforce policy. Earn trust. All local." — functional subtitle (NEW)
- 3 CTAs visible without scrolling:
  - Primary: "Quick Start" (gold button, solid)
  - Secondary: "Star on GitHub" (outline button)
  - Tertiary: "Read the Docs →" (subtle link)
- Scroll indicator (existing)

**Manifesto** (immediately below hero, still dark):
- 4 lines (existing, preserved)
- NEW: bridge paragraph connecting poetry to function

### Section 2 — The False Tradeoff (light background — bone)

Transition gradient: black → bone between manifesto and this section.

**Content:**
- Section eyebrow: "The false tradeoff"
- Heading: "AI agents need autonomy. You need control. You shouldn't have to choose."
- Two-box comparison: "Manual approval" vs "Blanket allow" — both are broken
- Resolution: "Colmena: scoped autonomy." Each agent free within its domain, firewall on the boundaries, every mission has start → end → evaluation
- Closing: "policy is code, review is mandatory, trust is earned"

### Section 3 — How It Works (dark background)

**Content:**
- Section eyebrow: "How it works"
- Heading: "One YAML file. One command. Full accountability."
- 4-step flow in columns:

**Step 1 — Define (EXPANDED, highlighted):**
- Write a YAML manifest: roles, scope paths, bash patterns, TTL
- Mini YAML preview showing real syntax
- Auto-validates schema + security, generates role prompts + subagent files, creates scoped delegations with TTL, wires bash_patterns into runtime overrides, --dry-run preview
- Callout: `mission init --from-history` reads audit.log and proposes a manifest skeleton from actual usage

**Step 2 — Spawn:**
- One command spawns the squad
- Each agent gets its role, tools, and boundaries

**Step 3 — Execute:**
- Agents work in parallel
- Every tool call evaluated in <15ms
- ALLOW · ASK · BLOCK on every decision
- audit.log on every call

**Step 4 — Review:**
- Auditor scores every artifact
- QPC scoring (Quality, Precision, Comprehensiveness)
- ELO adjusts — trust is earned, not declared

- Closing line: "Define → Spawn → Execute → Review → repeat, with better trust each cycle"

### Section 4 — Value Props (light background — bone)

**Content:**
- Section eyebrow: "What Colmena gives you"
- 4 categories, each with left gold border and 4 bullets:

**Firewall & Policy:**
- Deterministic YAML rules + compiled regex engine
- <15ms hot path — zero LLM calls, zero per-call cost
- Shell chain guard — blocks &&, ||, ;, $(), backticks
- Mission kill-switch — instant revocation of all delegations

**Multi-Agent Missions:**
- One-command squad spawn with scoped autonomy
- 11 orchestration patterns + 7 topologies
- 15 built-in roles — pentest, dev, devops, SRE, reviewer...
- Mission Manifest YAML — define once, spawn anywhere

**Trust & Accountability:**
- Mandatory auditor review — no artifact ships unreviewed
- QPC scoring: Quality + Precision + Comprehensiveness
- ELO trust calibration — 5 tiers, earned from behavior
- Append-only audit.log + findings store + alerts

**Efficiency & Control:**
- Output filtering — 30-50% token savings on noisy commands
- Terse inter-agent protocol — facts + path:line, minimal tokens
- Runtime delegations with mandatory TTL (max 24h)
- Custom roles & patterns — library is yours to extend

### Section 5 — Better Together (dark background)

**Content:**
- Section eyebrow: "Better together"
- Heading: "Colmena + Claude Code auto-mode. Different layers. Same mission."
- Comparison table — 6 dimensions:

| Dimension | Auto-Mode | Colmena |
|---|---|---|
| Decision model | Probabilistic (LLM) | Deterministic (YAML rules) |
| Per-call cost | Model tokens per classification | Zero — no network, no LLM |
| Explainability | Classifier output (opaque) | Rule ID in audit.log |
| Scope | Single-agent intent | Single + multi-agent with auditor |
| Accountability | Per tool call | Per agent + per role, ELO over time |
| Storage | Cloud-side | Local filesystem (git-versionable) |

- Closing: Auto-mode catches semantic intent rules can't. Colmena enforces the policy you wrote. Use them together.

### Section 6 — Use Cases (light background — bone)

**Content:**
- Section eyebrow: "Who is this for?"
- Heading: "Pick your role. Colmena adapts."
- 2x2 grid of cards, each with icon, description, and mini-flow:

| Card | Flow |
|---|---|
| Pentest engagement | Spawn → Scan → Find → Report → Auditor |
| Code review cycle | Implement → Review → Score → Calibrate |
| DevOps kubectl ops | Delegate → Apply → Audit.log → Deactivate |
| SRE runbook execution | Investigate → Diagnose → Draft → Incident note |

- Footer: "Not your profile? Colmena is domain-agnostic. Create your own roles and patterns."

### Section 7 — Quick Start (dark background)

**Content:**
- Section eyebrow: "Start in 30 seconds"
- 3 commands in code block:
  ```
  $ cargo install colmena-cli colmena-mcp
  $ colmena setup
  $ colmena doctor
  ```
- Subtitle: "Two commands after install. The firewall is active from now on."

### Section 8 — CTA Footer (dark background)

**Content:**
- Heading: "Ready to trust your swarm?"
- CTA hierarchy:
  - Primary: "Quick Start Guide →" (gold button)
  - Secondary: "★ Star on GitHub" + "Read the Docs →" (links)

## 6. CTA Strategy

| Priority | Action | Style | Purpose |
|---|---|---|---|
| 1 | Quick Start | Gold solid button | Convert to user |
| 2 | Star on GitHub | Outline / link | Social proof |
| 3 | Read the Docs | Subtle link | Education |

CTAs appear in the hero (all 3) and footer (all 3). The Quick Start code block is its own implicit CTA.

## 7. Bilingual (EN/ES)

Maintain existing i18n system (`data-i18n` attributes, `I18N` JS object, localStorage + browser detection, EN/ES toggle). All new content needs both languages.

## 8. Technical Constraints

- Single `index.html` file — no build step, no framework
- Canvas-based particle animation preserved from current version
- CSS-only responsive design (clamp-based typography, grid)
- IntersectionObserver for scroll-triggered reveals (already in place)
- No new dependencies
- Must work on mobile (<768px)
- Section background alternation via CSS classes, not JS
- Keep existing favicon, OG tags, meta tags

## 9. What Stays

- Hexagon particle animation (canvas)
- Language toggle (EN/ES)
- Playfair Display font
- Color palette
- Scroll-based hero fade-out
- IntersectionObserver reveal pattern
- The tagline "The swarm learns where to sting."
- The manifesto lines
- The "COLMENA" title treatment

## 10. What Changes

| From | To |
|---|---|
| Abstract manifesto only | Hero with functional subtitle + 3 CTAs |
| 3 value props | 4 categories, 17 specific capabilities |
| No problem statement | "The false tradeoff" narrative |
| No mission flow | 4-step "How It Works" with YAML preview |
| No auto-mode comparison | Honest 6-dimension comparison table |
| No use cases | 2x2 grid: Pentest, Dev, DevOps, SRE |
| Generic quick start | Manifest-aware quick start with --from-history callout |
| Single CTA (GitHub) | 3-tier CTA hierarchy |
| One background color | Alternating dark/light sections |

## 11. Browser Support

- Modern browsers (Chrome, Firefox, Safari, Edge — last 2 versions)
- Mobile Safari and Chrome
- No IE11 support required
