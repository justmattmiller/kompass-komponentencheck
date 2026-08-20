# Kompass-Komponentencheck — Project Context

## What this is
A single-file, offline HTML tool that reads a CSV of design-system component
audit data and flags where implementation deviates from spec. Built as a solo
exercise for a "KI Use Cases" course, now treated as a portfolio piece.

Fictional scenario: Terra Marken GmbH, three brands (Terra Frisch, Terra
Wohnen, Terra Kids) sharing a design system called "Kompass."

## Non-negotiable constraints
- **Single HTML file, no external libraries, no server, no build step.**
  Must run fully offline via double-click.
- **The CSV never leaves the browser.** No network calls of any kind in the
  core tool. This is a stated privacy design principle, referenced throughout
  the project's presentation materials — do not silently break it.
- All UI labels and text are in German.

## Repo setup
- Repo name: `kompass-komponentencheck`
- `index.html` = the final tool (was `kompass_v6.html`)
- No walkthrough/staged landing page — single final version only
- `sample-data/` — include the test CSV so visitors to the live GitHub Pages
  URL can try the tool without supplying their own file
- License: MIT
- No mentions of HelloFresh anywhere in this repo (code, comments, README,
  commit messages) — this is a public, portfolio-facing repo and that
  reference was deliberately scrubbed from all class-facing material

## Status / scope
- This repo is portfolio-grade, not just coursework — polish and honest
  framing matter more than speed
- Working solo — branch/PR workflow is being done for real (not skipped),
  as deliberate practice, even though solo work technically permits skipping it

## Planned work (in order)
1. Initial commit: current `index.html` + `sample-data/` + README + LICENSE
2. Branch `verbesserung-1`: add per-brand `Erwartet` differentiation
   - Currently the tool assumes one global expected value per component type
   - Needs 1-2 new CSV rows demonstrating a *legitimate* brand-specific spec
     (not a drift case) to actually exercise the new logic
   - This closes a real gap named in the project's own "what I'd build next"
     notes — do it properly, not as a stub
   - Open a real PR, write a real diff description, merge deliberately (not
     auto-merged in the same breath it's opened)
3. GitHub Pages: publish `index.html` at the repo's Pages URL
4. PWA: manifest + service worker, installable on desktop/mobile
5. GitHub Actions: build both an Android APK (Capacitor) and Windows EXE
   (Electron), attached to a GitHub Release
   - EXE will be verified on a real Windows PC (tower available) — treat a
     green build as a checkpoint, not done, until confirmed running
   - APK will likely only reach "green build, unverified" unless a classmate
     with an Android device tests it — do not claim it as confirmed working
     without that
6. Reason-quality heuristic on `Begründung` (see section below) — local,
   offline, rule-based. Not urgent, can happen whenever fits naturally
   (possibly as its own branch/PR, same discipline as step 2)

## Reason-quality checking on `Begründung` — decided approach
Right now the tool only checks whether `Begründung` is non-empty, not
whether the reason given is any good. This is explicitly flagged as a real,
named limitation in the project's Q&A prep.

**Decision: build a local heuristic, not an NLP/API call, for now.** This
keeps the tool inside the original exercise's constraints (single file, no
libraries, no network, works fully offline). A reasonable local heuristic:
- flag reasons under some minimum length (e.g. ~15-20 characters) as weak
- flag reasons that don't contain a date pattern or a reference-like token
  (ticket number, "siehe ...", a name) as weak
- still binary-ish, but strictly better signal than "empty vs non-empty"
- surface the rule used, visibly on the page, same pattern as the existing
  sort-rule and aging-threshold disclosures

**NLP-based quality checking stays an explicit future option, not current
scope.** If it's picked up later: it very likely requires an external API
call, which breaks the "everything stays local" principle above. Do not
implement it silently — it requires updating the "Bekannte Grenzen" section
and presentation talking points to disclose that one specific feature now
sends data externally, and that's a real narrative change, not just a code
change. Ask before starting that work.

Other classification logic (e.g. the age-threshold rule, match/deviation
detection) may get similar "maybe smarter later" ideas — same rule applies:
local heuristics are the default; trading the offline guarantee for
capability is a deliberate, flagged decision, not a default upgrade path.

**Note on why this is scoped differently from "new data types" below:**
NLP-on-`Begründung` is real, motivated future work (explicit goal: learning
NLP integration), not speculative scope creep — the limitation is already
named in the project's own Q&A prep, and there's a genuine reason to build
it beyond "might be nice." Because the driver is learning rather than
shipping a polished feature, this work does not need to meet the same
polish bar as the rest of the repo when it happens: it's fine for it to
live on its own experimental branch, be rougher, get reworked, or even get
discarded, without that reflecting on the main branch's portfolio quality.
Don't hold it to "must ship clean" — hold it to "worth learning from."

## New data types / columns — deliberately NOT decided now
There's been discussion of eventually expanding the CSV schema (new
columns, new data types) for a more detailed demo. This is explicitly
**not being decided or designed for right now** — no concrete need for a
new column exists yet, and building schema flexibility ahead of a real
requirement is the scope-creep trap, not good practice. New *rows* in the
existing 9-column schema are always fine, any time, no discussion needed.
New *columns* are a deliberate, need-driven decision to make only when a
real, specific demo requirement forces it — e.g. "I want to show a
`Plattform` column because someone asked about mobile vs. web drift" — not
preemptively. If asked to speculatively add extensibility for future
columns, push back and ask what the concrete need is first.

One relevant note: while doing the per-brand `Erwartet` fix (item 2 above),
it's fine to notice if the implementation is being written in a way that
would make a future column painful to add — but that's "don't paint into a
corner," not "build extension points." Don't over-engineer step 2 for
hypothetical future schema changes.

## Known limitation baked into the tool itself
INST-010 (a Terra Kids loading indicator) passes every check but hasn't been
touched since Jan 2023 and is barely used — the deliberate "pattern isn't
meaning" failure case. Don't "fix" this in a way that resolves it
automatically — the point is that a human has to look, not that the tool
should get smarter about guessing.
