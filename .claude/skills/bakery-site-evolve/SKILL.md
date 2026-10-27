---
name: bakery-site-evolve
description: Research bakery and competitor websites, find patterns that work, and use them to improve the Al-Sunbula bakery site (index.html) or add new sections to it. Use this whenever the user wants to develop, upgrade, improve, or redesign the bakery website, add a section (قسم/سكشن) to it, compare it with competitors or other bakeries, or asks for ideas to make it more attractive or sell more — even if they don't say "competitors" or "research" explicitly (e.g. "طور الموقع", "ضيف سكشن", "شوف المنافسين", "الموقع محتاج يبقى أحلى").
---

# Bakery site evolve

The goal is to make the bakery site better by learning from what good bakery sites do, without
copying anyone and without inventing a single fact about the business.

## Ground rules (from the project's CLAUDE.md, and why they matter)

- **Start every reply with "انا جاهز"** and write to the user in Egyptian Arabic, short and direct.
- **Facts come only from `bakery-facts.md`.** Prices, hours, products, address, delivery rules, payment
  methods — if it isn't in that file, it doesn't go on the site. A made-up review, award, ingredient
  claim or "since 1990" line is worse than an empty spot: customers act on it, and it's dishonest.
  When a good idea needs facts that don't exist yet, propose it and list exactly what the owner
  would need to provide.
- **Ask before anything hard to undo**: overwriting big parts of `index.html`, deleting sections,
  pushing/deploying. Always back up first (see step 5).
- If `CLAUDE.md` describes a deploy or form-check step that is actually configured, do it after
  editing; if it isn't configured (no git repo, no Supabase keys), say so plainly instead of
  pretending it was done.

Re-read `CLAUDE.md` and `bakery-facts.md` at the start of every run — the user edits them.

## Workflow

### 1. Understand the current site
Read `index.html` and note: existing sections (by `id`), the CSS tokens in `:root` and
`:root[data-theme="dark"]`, the fonts, animation style, and what JS features exist (cart, theme
toggle, open/closed badge, form). New work must fit into this, not fight it.

### 2. Research (skip only if the user says so)
If the user named competitors or URLs, start with those. Otherwise ask once whether they have
specific competitors in mind; if they don't, search on your own.

Search broadly enough to see real patterns — aim for 6–10 sites, mixing:
- Egyptian bakeries / patisseries (search in Arabic and English, e.g. "مخبز اطلب اونلاين", "Cairo bakery order online")
- Strong international bakery sites known for good design
- Any local competitors the user mentioned

For each site, note what it does well that a small family bakery could realistically use:
section types, how products are shown, ordering flow, trust signals, CTAs, mobile behaviour,
micro-interactions. Look at structure and ideas only — never copy text, images, logos, or brand
names onto our site.

Save findings to `research/competitors.md` in the project (create the folder if needed) using the
format in `references/research-template.md`, so later runs can build on it instead of re-searching.
If that file already exists, read it first and only add new sites.

### 3. Gap analysis
Compare the patterns you found against the current site. For each candidate improvement decide:
- **Fits the facts?** Can it be built only from `bakery-facts.md`? (yes / needs owner info: list it)
- **Value**: does it help people order, trust, or enjoy the site?
- **Effort**: small / medium / large

`references/section-ideas.md` has a catalog of common bakery sections with the facts each one needs —
use it as a starting point, not a limit.

### 4. Propose, then let the user choose
Show a short list (3–6 items), best first, as a compact table in Arabic:
improvement · why (which sites do it) · needs extra info? · effort.
Ask which ones to build. Don't start editing big changes before they pick — they know their
business and taste better than the research does. Small obvious fixes (a broken link, contrast
problem in dark mode) can be bundled in and mentioned.

### 5. Build
- Back up first: copy `index.html` to `backups/index-<YYYY-MM-DD-HHMM>.html`.
- Match the existing code: same CSS tokens (never hard-code `#fff` surfaces — use `var(--surface)`
  etc. so dark mode keeps working), same naming, same comment density, RTL, Arabic copy in the
  same warm Egyptian tone.
- Every new section must work in **both light and dark mode**, at **phone width** (no horizontal
  scroll), and respect `prefers-reduced-motion`. Use the existing `.reveal` class for scroll-in.
- Add new sections to the nav if they're top-level destinations.
- Where an idea needs owner info that doesn't exist, either leave the section out or show a clearly
  visible placeholder only if the user asked for that — never fill it with invented content.

### 6. Verify
- Check every `<script>` parses:
  `node -e "const h=require('fs').readFileSync('index.html','utf8');h.split('<script>').slice(1).map(x=>x.split('</script>')[0]).forEach(s=>new Function(s));console.log('OK')"`
- Grep the diff for any price, time, phone or claim, and confirm each one is in `bakery-facts.md`.
- Open the page in the browser so the user can look (`start index.html` on Windows).
- Run the CLAUDE.md deploy / form check if it's configured.

### 7. Report
Start with "انا جاهز". In a few lines: what was researched (how many sites, where the notes are),
what was added/changed and where, what was verified and what wasn't, and the ideas still waiting
on info from the owner.
