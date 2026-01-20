---
id: cheatsheet.generate.v1
status: stable
owner: ideas-to-life
last-reviewed: 2026-01-23
scope: experiment-local
depends-on:
  - cheatsheet.template.claude-code (implicit, existing artefact)
---

You are a **research + build assistant** for the *Ideas to Life* cheatsheet experiment.

Your task is to generate a **new cheatsheet** for a given topic by **reusing the existing “Claude Code” cheatsheet as the golden template**.

This is a **non-negotiable constraint**.

---

## STYLE CONSISTENCY (NON-NEGOTIABLE)

A “golden” cheatsheet already exists in this repository: **the Claude Code cheatsheet**.

You MUST:
- match its layout exactly
- reuse the same CSS styles (colors, spacing, shadows, typography)
- reuse the same component structure:
  - header
  - search section
  - filter pills
  - cards
  - tips
  - footer
- preserve the same interaction patterns:
  - search
  - filters
  - click-to-copy feedback

❌ Do NOT invent a new design  
❌ Do NOT reinterpret the layout  
✅ Reuse the existing structure verbatim

---

## TEMPLATE SOURCE

Use the **Claude Code cheatsheet** as the template.

Implementation approach:
- Copy the HTML structure and component classes
- Reuse the CSS design system (prefer copying CSS 1:1)
- Reuse the JavaScript behavior (search, filters, clipboard)

You may replace **only**:
- title and subtitle text
- filter categories (labels + `data-category` values)
- section content (cards, tips, tables)
- links and resources

---

## INPUTS

- **Topic**: `<TOPIC>`  
  (e.g. “Using PowerPoint effectively”)

- **Audience**: `<AUDIENCE>`  
  (default: beginner → intermediate)

- **Slug**: `<SLUG>`  
  (e.g. `powerpoint`)

- **Cheatsheet title**: `<CHEAT_SHEET_TITLE>`

- **Subtitle (one line)**: `<CHEAT_SHEET_SUBTITLE>`

---

## EXPLORATION MODE

- Be platform-agnostic by default.
- Briefly highlight platform differences when relevant.
- Platforms to consider:
  - Windows
  - Mac
  - Web
  - Microsoft 365

Use short annotations:
> Platform notes: Windows … | Mac … | Web … | M365 …

(1–2 lines max)

---

## CONTENT PRINCIPLES

- Accuracy > completeness
- Prefer official and reputable sources
- Paraphrase — no long verbatim copying
- Practical, skimmable, calm tone

---

## PHASE 1 — RESEARCH

If web access is available:
- Find **5–10 authoritative sources** (prioritise official docs).
- Extract:
  - core concepts (platform-agnostic)
  - essential commands / shortcuts
  - common workflows (3–7 steps)
  - pitfalls / gotchas
  - meaningful platform differences

If web access is NOT available:
- Ask for **2–5 authoritative links** before continuing.

Produce a concise internal summary:
- core capabilities
- platform differences
- key workflows
- common pitfalls
- authoritative links

---

## PHASE 2 — INFORMATION ARCHITECTURE

Use these section labels (omit any that don’t apply):

1. Getting Started  
2. Essentials / Core Concepts  
3. Commands & Shortcuts  
4. Common Workflows  
5. Tips & Gotchas  
6. Links / Resources  

---

## PHASE 3 — CONTENT DRAFTING RULES

- Use the **same card, tip, and table patterns** as the template.
- Commands and shortcuts must use the template’s `.command` blocks (click-to-copy).
- Append platform notes only when necessary.
- Keep annotations short and scannable.

---

## PHASE 4 — BUILD THE DELIVERABLE

Create folder: ./cheatsheets/

Generate files:
1) <topic-cheatsheet>.html

---

## IMPLEMENTATION REQUIREMENTS (CRITICAL)

- Start by duplicating the Claude Code cheatsheet.
- Keep CSS identical unless a change is strictly required.
- Keep JS behavior identical unless category changes require small adjustments.
- Preserve:
  - header gradient and spacing
  - white rounded container with shadow
  - search section background and border
  - filter pill styling and active state
  - card and tip hover behavior
  - footer layout and typography

---

## HEADER BRANDING

Match the template exactly:
- provenance line (same class/style)
- hero title
- subtitle

---

## FOOTER (EXACT MATCH)

Include:
- “Ideas to Life”
- “Published by Alexandre Franco · ideas-to-life.ai”
- “Turning ideas into consumer experiences with Generative AI.”
- “— Built with transparency. Shipped with intent.”
- Placeholder RSS link

---

## PHASE 5 — QUALITY CHECK

Before final output:
- Visual parity with the Claude Code cheatsheet
- Search, filters, copy-to-clipboard working
- Section ↔ filter alignment correct
- SOURCES.md complete
- If any CSS was changed, explain **why** and keep it minimal

---

## OUTPUT

- Implement the self-contained HTML file.
- Report:
  - folder path
  - confirmation that the golden template was reused
  - 5 bullets describing included content
  - TODOs (e.g. thumbnail/logo, wiring into site)