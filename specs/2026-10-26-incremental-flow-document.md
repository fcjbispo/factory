---
type: design
status: draft
owner: senior-architect
readers: [po, fullstack-developer, qa-tester, devops-sre]
updated: 2026-10-26
related:
  - ../src/docs/templates/design/_template.md
  - ../src/factory-init.sh
---

# Incremental FLOW.md Project Document

## Context and problem

Factory manages project documentation under `<FACTORY_ROOT>/<project>/docs/` and keeps source code under `<FACTORY_PROJECTS_DIR>/<project>/`. Approval work often spans several documents and roles. The current approach has no compact, durable record showing what to read, edit, or approve, who is responsible, what the PO returned, and what action follows. Long-running approval histories also need to keep the newest flow near the top rather than requiring readers to scroll to the bottom.

A prototype `FLOW.md` was developed in the MyVideoLanClient Factory project. This specification captures the agreed concept for a reusable framework feature. The prototype is illustrative; this file is the implementation target.

## Proposed solution

Provide a `FLOW.md` in the root of each Factory-managed documentation project: `<FACTORY_ROOT>/<project>/FLOW.md`, alongside `.factory` and `docs/`. It is a compact, human-editable Markdown flow log. Do not place it under `docs/`, because the intended use is project-level approval/status tracking rather than a regular section document.

### Document header

The header states:

- what the FLOW file is for and that canonical decisions remain in their decision/ADR documents;
- current active flow and its status;
- session/workflow creation date and closure date (blank/pending while active);
- the incremental convention: newest flow first, completed flows retained below.

Keep header brief. Project-specific dates, current flow and status are populated/maintained in each project copy rather than baked into the reusable template.

### Flow block

Each flow is one self-contained block with a unique flow ID, short title, state, start date, end date, and one-line objective. The block contains **one Markdown table**; sequential tasks or substeps are rows in that same table.

Required table columns:

| Column | Meaning |
|---|---|
| `#` | Step order within the flow |
| `Action` | Concise action, such as read, edit, review, approve, execute, or record |
| `Document / evidence` | Relative link to a project document or URL to an external reference |
| `Participant / role` | Responsible participant(s) and what each role must do |
| `PO response` | PO's actual analysis, approval, rejection, or requested changes; `Pending` while awaiting response and `—` when not applicable |
| `Outcome / next action` | Result, next step, and responsible role when follow-up is needed |

A new flow receives the next unique ID and is inserted **above** all prior flows. Existing blocks are retained as history. On closure, update that block's state and end date; do not erase the PO's recorded response or outcomes. Keep chronological order newest-first.

### Format and authoring rules

- Use portable Markdown tables and ordinary relative links; use absolute URLs for external references.
- Do not require Mermaid or other diagram renderers. A short plain-text sequence (for example, `review → approval → execution`) is optional, never a replacement for the table.
- Keep each cell concise. Put detailed criteria, rationale, technical notes and full plans in the relevant Factory document and link to it rather than duplicating them.
- Avoid extra prose sections or repeated participant lists; roles belong in table rows.
- Preserve user-entered content. Framework commands must not regenerate, reorder, or overwrite an existing `FLOW.md`.
- The format is language-neutral; generated default copy follows the selected Factory locale. Field/heading localization follows existing `src/locales/*.env` conventions.

### Lifecycle and distribution

- Add a canonical template to the Factory source templates and include it in the localized release packages.
- `factory-init.sh new` and `adopt` create `<FACTORY_ROOT>/<project>/FLOW.md` from the template only when no file exists.
- A later `sync` may add the file to an existing project only when absent; it must never replace an existing user-maintained FLOW file. Do not provide destructive/automatic migration of prior FLOW content.
- Initialization remains through `factory-init.sh`; no manual project structure creation is required.
- This specification is internal to the Factory source repository and is not part of the shipped documentation package.

## Affected components

| Component | Impact | Responsible |
|---|---|---|
| `src/docs/templates/` | Add canonical FLOW template and relevant template index entry | Senior Architect |
| `src/locales/en.env`, `src/locales/pt.env` | Add translations for localized template fields, if needed | Senior Architect / translators |
| `src/factory-init.sh` | Create FLOW on new/adopt; add-if-missing behavior on sync | Full-Stack Developer |
| `src/Makefile` / localized artifacts | Ensure template is packaged and builds for supported locales | DevOps/SRE |
| Documentation for initialization/sync | Explain ownership, order, statuses and non-overwrite safety rule | Senior Architect |

## API Contract

Not applicable. FLOW is a local Markdown file; it does not define a network API or service contract.

## Data model

No database or service storage. The Markdown file is the human-readable source of truth. Flow IDs are unique within the file; dates use ISO `YYYY-MM-DD`. PO responses are free-text records, not inferred from agent status or automated sync.

## Acceptance criteria

- [ ] The canonical template explains purpose, ownership of canonical decisions, newest-first ordering, and session/flow creation and closure dates.
- [ ] Each flow is a compact block with one table and no required Mermaid diagram.
- [ ] The table includes ordered action, linked document/evidence, participant/role, PO response, and outcome/next action.
- [ ] The template explains `Pending` and `—` semantics for PO response.
- [ ] New flow blocks are added above existing blocks with unique IDs; older flows remain available as history.
- [ ] Project-specific header fields are not falsely presented as completed or approved by default.
- [ ] New/adopt creates a root-level FLOW file; sync adds it only if missing and preserves all existing user content byte-for-byte.
- [ ] Relative links to project files and absolute external links render in supported Markdown viewers.
- [ ] English and Portuguese package builds contain a usable localized template, with no unresolved `$i18n_*` markers.
- [ ] Existing project initialization, adoption and sync behavior continues to pass available validation.

## Testing approach

| Type | What to test | Responsible |
|---|---|---|
| Template/build | `make clean && make build-all`; inspect English and Portuguese archives for the template and unresolved markers | DevOps/SRE |
| Lifecycle integration | Run `new`, `adopt`, and `sync` in disposable fixture directories; verify root location and content | QA / Full-Stack Developer |
| Non-overwrite regression | Put custom PO text and multiple flows in FLOW.md, run sync, assert exact content remains unchanged | QA |
| Markdown validation | Check local links and render tables in the supported Markdown target(s) | QA |
| Regression | Validate existing Factory commands and docs packaging | Full-Stack Developer / QA |

## Security considerations

FLOW contains approval notes and project links, but no credentials or secrets. Do not automatically fetch/execute linked files. Treat PO response text as project data, not executable instructions. Do not expose the full user's home path in the reusable template; use relative project links.

## Performance considerations

The file is human-maintained and expected to be small. No indexing or runtime parsing is required. Avoid automatic whole-file rewriting; this also prevents accidental loss of user-authored approvals.

## Discarded alternatives

- **Mermaid as required flow representation:** rejected because it adds rendering dependence and length; a table is the canonical sequence.
- **One section/table per approval step:** rejected because it repeats headings and grows vertically; keep all steps in one table per flow block.
- **Checklist grouped by role:** rejected because it obscures cross-role order and gates.
- **Appending new flows at the end:** rejected because users would need to scroll past older history to find current work.
- **Storing all approval detail in FLOW:** rejected; FLOW links to canonical specifications, ADRs and decisions instead of duplicating them.
- **Overwriting FLOW during template sync:** rejected because the file accumulates user-authored approvals and history.

## Open decisions for implementation

- Confirm the final flow-state vocabulary and whether any labels are localized or standardized.
- Confirm whether per-project session start/end dates in the header are mandatory, or only flow-level start/end dates.
- Confirm exact template filename/location and whether docs template registry or a new top-level template registry is the canonical index.
- Confirm whether sync should add a missing FLOW.md to existing projects by default or require an explicit option.
- Confirm whether the stable format should retain literal `PO response` column naming when the PO role has another localized title.

## Change history

| Date | Change | By |
|---|---|---|
| 2026-10-26 | Initial feature specification based on the MyVideoLanClient FLOW prototype and user feedback | senior-architect |
