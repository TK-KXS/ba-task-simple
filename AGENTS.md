# AGENTS.md — TK Pre-Sales Delivery Workspace

This workspace turns rough customer input into two formal TK deliverables using
CodeArts Agent Team. There is no application source code here — the "product" is
the documents and the agent configuration.

## What this repo does

1. A user goes to **Space → Agent Team**, picks the **`tk-presales-delivery`**
   scene, uploads a customer requirements document (meeting notes / email / messy
   `.md`) and gives a short instruction.
2. The team produces, in `.codeartsdoer/deliverables/<project>/`:
   - `TK-BA-<PROJECT>-REQ-01.html` — requirements package (Scope, TR/NFR/INT,
     Functional Spec, User Stories, Commercial notes).
   - `TK-SA-<PROJECT>-ARC-01.html` — solution architecture & workflow pack
     (As-Is/To-Be, C4, sequence, state, ERD).
   Both are standalone A4 HTML stamped "TK Company – Confidential".

## Layout

| Path | Purpose |
|---|---|
| `.codeartsdoer/skills/` | 3 TK skills (one BA method, one SA method, one shared A4 document format) |
| `.codeartsdoer/agent-team/scene/tk-presales-delivery.md` | The team scene / Delivery Lead workflow |
| `.codeartsdoer/agent-team/roles/*.md` | Roles: tk-ba, tk-sa, tk-verify |
| `.codeartsdoer/commands/tk-deliver.md` | Slash-command trigger |
| `.codeartsdoer/rule/tk-standards.mdc` | Always-on project standards |
| `.codeartsdoer/examples/` | Sample customer input for a demo |
| `.codeartsdoer/README.md` | Full setup, flow and demo guide |

## Ground rules

- Skills are the standard. Load the relevant skill before writing anything.
- Authors (`tk-ba`, `tk-sa`) write; the single verifier (`tk-verify`) only reports.
  Never silently resolve a conflict — record an `OI-`.
- Never claim a check or verification passed unless it was run this session.
- No application build/lint/test commands exist here; verification is document
  review plus rendering the HTML and checking it opens offline and prints to A4.