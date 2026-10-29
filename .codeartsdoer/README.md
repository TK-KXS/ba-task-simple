# CodeArts Agent setup: TK Pre-Sales Delivery Team

This folder turns CodeArts Agent into a **TK pre-sales delivery pod**. A business
user uploads rough customer input (kickoff notes, emails, a messy `.md`), types a
short instruction, and the team delivers two formal, customer-ready documents:

1. **Requirements package** — `TK-BA-<PROJECT>-REQ-01.html`
   (Scope · Business Rules · TR · NFR · INT · Functional Spec · User Stories ·
   Commercial notes)
2. **Solution architecture & workflow pack** — `TK-SA-<PROJECT>-ARC-01.html`
   (As-Is/To-Be process · C4 context & container · integration sequence ·
   state model · ERD with PDPA classification)

Both are **single, self-contained A4 HTML files** stamped
**"TK Company – Confidential"** in the header and footer of every page.

The "product" is the documents and this agent configuration — there is no app
source code here.

## What's here and how CodeArts loads it

| Path | CodeArts feature | Purpose |
|---|---|---|
| `agent-team/scene/tk-presales-delivery.md` | Agent Team **scene** | Single-pass Delivery Lead workflow (one "Deliver" pass) over the three roles below |
| `agent-team/roles/tk-ba.md` | Team role | Business Analyst — authors Output 2 (requirements) |
| `agent-team/roles/tk-sa.md` | Team role | Solution Architect — authors Output 3 (architecture) |
| `agent-team/roles/tk-verify.md` | Team role (**verifier**) | Single final gate: audits both files + cross-traceability, returns PASS / fix list |
| `skills/*/SKILL.md` | Project skills (3) | The TK standards: one BA method skill, one SA method skill, one shared A4 document format |
| `skills/ProjectSkillStatus.txt` | Skill switches | All 3 skills enabled |
| `rule/tk-standards.mdc` | Project rule, **always** | Non-negotiables injected into every chat |
| `rule/metadata.properties` | Rule registry | Name, description, trigger type of the rule |
| `commands/tk-deliver.md` | Slash command | `/tk-deliver` (run the whole lean team) |
| `examples/siammart-returns-input.md` | Sample input | Demo scenario (SiamMart Returns) |
| `deliverables/<project>/` | Output | Where the two HTML files are written |

## The team and its skill permissions (set wisely)

Each role can load **only** the skills it needs. Skills are the standard; a role
that lacks a skill cannot use that standard (and is told so by the frontmatter
`permission.skill` block).

| Role | Type | Skills it may load |
|---|---|---|
| `tk-ba` | Author | `tk-ba-requirements`, `tk-doc-a4-confidential` |
| `tk-sa` | Author | `tk-solution-architecture`, `tk-doc-a4-confidential` |
| `tk-verify` | Verifier (read-only) | **All 3** — the single final gate checks both standards + document format + cross-traceability |

Rationale: authors get exactly their discipline's skill (BA method ≠ SA method, so
neither over-reaches into the other's standard); the one verifier gets all three
because it checks both documents against everything. Every role denies `'*'` by
default so built-in/generic skills never leak in.

## The flow (what the user does)

1. **Space → Agent Team →** pick the **`tk-presales-delivery`** scene.
2. **Upload** the customer document (`.md`, `.txt`, email export, notes) and type
   a short instruction (see the triggered prompts below).
3. **Wait** — the Delivery Lead runs a single pass: `tk-ba` → `tk-sa` →
   `tk-verify` → hand-over. It asks at most 3 blocking questions, each with a
   suggested default.
4. **Collect** both HTML files from `.codeartsdoer/deliverables/<project>/`.

### Triggered prompts (copy-paste)

```
/tk-deliver Turn this kickoff notes into a full TK requirements package and a
solution architecture pack. Project is SiamMart Returns (code SMR). Phase 1 only;
put Shopee/Lazada returns in Phase 2. Flag every unresolved conflict.
```

or, in the Agent Team scene without a slash command:

> From the attached meeting notes, produce (1) the TK requirements package and
> (2) the solution architecture & workflow pack, as confidential A4 HTML. Use
> project code SMR. Keep phase-1 scope; list everything else as out of scope.
> Call out the CS-vs-Finance auto-approval conflict and any number that needs
> confirming.

To re-verify existing deliverables later, just re-run `/tk-deliver` and ask it to
re-check the current files and return a consolidated fix list.

## Demo script (≈ 10 min)

1. Open this repo in CodeArts Agent and trust the folder. Check **Skills** shows
   the 3 `tk-*` skills and **Rules** shows `tk-standards` (always).
2. Space → Agent Team → `tk-presales-delivery`; upload
   `examples/siammart-returns-input.md`.
3. Run the `/tk-deliver` prompt above.
4. Watch: the analyst produces `TK-BA-SMR-REQ-01.html`; the architect produces
   `TK-SA-SMR-ARC-01.html` citing the requirements' TR IDs; the single verifier
   (`tk-verify`) then audits both files — it should catch that the ≤ 500 THB
   auto-approval conflict must be an `OI-`, that the 18k/25k returns-per-month
   figure must be sized and flagged, the no-direct-SAP rule, the PDPA tags, the
   confidential stamp, A4 pagination and offline single-file rendering.
5. Open both HTML files in a browser, `Cmd/Ctrl-P` → A4 preview to confirm nothing
   is clipped and the stamp is on every page.
6. Read the hand-over: the two files, the value story, and the open issues that
   still need a SiamMart decision.

## Notes / caveats (check against your CodeArts version)

- Formats follow the installed kernel and the existing `code-reviewer` /
  `no-code-website-3-agent-team` projects: team roles from `agent-team/roles/*.md`
  (frontmatter `name`, `description`, `permission.skill`), the scene from
  `agent-team/scene/*.md` (frontmatter `name`, `description`, `scenes`), skills
  from `skills/*/SKILL.md`, rules from `rule/*.mdc` + `metadata.properties`.
- `skills/ProjectSkillStatus.txt` lists the 3 skills as enabled.
- If CodeArts resets `.gitignore` to `*`, commit the folder with
  `git add -f .codeartsdoer/<path>`.
- The two HTML deliverables are ignored by `.gitignore` if you re-run a demo; the
  folder structure and this configuration are what you commit.