You are my “research + build” assistant for Ideas to Cheatsheet experiment.

IMPORTANT: STYLE CONSISTENCY (NON-NEGOTIABLE)
We already have a “golden” cheat sheet design in this repo: the Claude Code cheat sheet.
Your job is to produce a new interactive HTML/JavaScript cheat sheet that matches its look & feel EXACTLY:
- same layout
- same CSS styles (colors, spacing, shadows, typography)
- same component structure (header, search section, filter pills, cards, tips, footer)
- same interaction patterns (search + filters + click-to-copy feedback)

Do NOT invent a new design. Reuse the existing one.

STYLE TEMPLATE
Use the existing Claude Code cheat sheet as the template:
- Copy its HTML structure and component classes
- Reuse the same CSS design system (ideally copy the CSS as-is)
- Reuse the same JS behavior (search, filter, copy-to-clipboard)

Then replace ONLY:
- title/subtitle text
- filter categories (buttons + section data-category values)
- section content (cards/tips/tables)
- links/resources content

TOPIC INPUT
Topic: <TOPIC> (e.g., “Using PowerPoint effectively”)
Audience: <AUDIENCE> (default: general / beginner-to-intermediate)
Slug: <SLUG> (e.g., powerpoint)
Cheat sheet title: <CHEAT_SHEET_TITLE>
Subtitle (one line): <CHEAT_SHEET_SUBTITLE>

EXPLORATION MODE
- Be as generic/platform-agnostic as possible.
- Highlight platform differences briefly.
- Platforms to consider: Windows, Mac, Web, Microsoft 365.
- Use short “Platform notes:” lines (1–2 lines max).

NON-NEGOTIABLES
- Accuracy > completeness.
- Prefer official and reputable sources.
- No long verbatim copying; paraphrase.
- Practical, skimmable, calm tone.

--------------------------------------------------
PHASE 1 — RESEARCH
--------------------------------------------------
If you have web access:
- Find 5–10 high-quality sources (prioritize official docs).
- Extract:
  - core concepts (platform-agnostic)
  - essential shortcuts/commands
  - common workflows (3–7 steps)
  - pitfalls/gotchas
  - platform differences worth noting

If you do NOT have web access:
- Ask me for 2–5 authoritative links before continuing.

Create a concise internal summary:
- core capabilities
- platform differences (Windows/Mac/Web/M365)
- key workflows
- gotchas
- authoritative links

--------------------------------------------------
PHASE 2 — INFORMATION ARCHITECTURE
--------------------------------------------------
Use these section labels (omit any that don’t apply):
1) Getting Started
2) Essentials / Core Concepts
3) Commands & Shortcuts
4) Common Workflows
5) Tips & Gotchas
6) Links / Resources

--------------------------------------------------
PHASE 3 — CONTENT DRAFTING RULES
--------------------------------------------------
- Use the SAME card/tip/table patterns as the template cheat sheet.
- Commands/shortcuts go into the template’s `.command` blocks (so they are clickable-to-copy).
- For any platform-specific item append:
  “Platform notes: Windows … | Mac … | Web … | M365 …”
  Keep to 1–2 lines max.

--------------------------------------------------
PHASE 4 — BUILD THE DELIVERABLE
--------------------------------------------------
Target folder:
./cheatsheets/

Generate files:
1) <topic-cheatsheet>.html

IMPLEMENTATION REQUIREMENTS (CRITICAL)
- Start by duplicating the Claude Code cheat sheet template.
- Keep CSS as close to identical as possible.
  Preferred: copy the CSS 1:1 and only change what is necessary (if anything).
- Keep JS behavior identical unless the new categories require small adjustments.
- Preserve:
  - header gradient and spacing
  - white rounded container with shadow
  - search section background and border
  - filter pill styling and active state
  - card + tip styling and hover behavior
  - footer signature styling

HEADER BRANDING
Match template exactly:
- text-only provenance line (same class/style)
- main hero title
- subtitle

FOOTER
Match template exactly:
- Ideas to Life
- Published by Alexandre Franco · ideas-to-life.ai
- Turning ideas into consumer experiences with Generative AI.
- Built with transparency. Shipped with intent.
- © 2026 Ideas to Life. All rights reserved.

--------------------------------------------------
PHASE 5 — QUALITY CHECK
--------------------------------------------------
- Ensure the rendered page visually matches the Claude Code cheat sheet.
- Ensure search + filters + click-to-copy work.
- Ensure sections align with filter categories (no mismatches).
- Ensure SOURCES.md is complete.
- If you changed any CSS, explain why (and keep it minimal).

OUTPUT
- Implement the HTML file.
- Report:
  - Folder path
  - Confirmation you reused the template design
  - 5 bullets of what’s included
  - TODOs (thumbnail/logo, wiring into site)