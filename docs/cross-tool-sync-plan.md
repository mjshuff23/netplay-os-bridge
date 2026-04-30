# Cross-Tool Sync Plan

## Purpose

Keep Notion, Figma, GitHub, and Linear aligned without pretending they all own
the same kind of information.

## System Of Record By Artifact Type

| Artifact | System of record | Mirrors / backlinks |
| --- | --- | --- |
| Product brief | Notion | GitHub `README.md`, Linear project description |
| Architecture decisions | GitHub ADRs | Notion decision log, Linear issue links |
| Execution backlog | Linear | GitHub PRs/issues, Notion roadmap |
| Architecture diagrams | Figma | Notion spec, GitHub docs, Linear project links |
| UX flows | Figma | Linear epics, Notion product spec |
| Implementation | GitHub | Linear ticket, Notion changelog |
| Status updates | Linear | Notion project hub |

## Proposed Artifact Map

### Notion

Create a project page under the existing Projects/Learning area:

- Title: `Netplay OS Bridge`
- Sections:
  - Product goal
  - Current architecture decision
  - MVP scope
  - Open questions
  - Links to GitHub, Figma, and Linear
  - Decision log
  - Research notes

### Figma

Create one design file:

- File: `Netplay OS Bridge`
- Pages:
  - `00 System Architecture`
  - `01 Play Session Flow`
  - `02 Upload And Library Flow`
  - `03 Control Plane Wireframes`
  - `04 Friend Join Flow`

First diagrams to build:

- System context: players, browser portal, AWS account, Windows GPU host,
  Parsec, S3, import worker, RomM or library DB.
- Runtime sequence: start host, friend joins, controllers map, session ends,
  idle shutdown.
- Upload sequence: request upload URL, S3 upload, event import, metadata scan,
  host sync.

### GitHub

Use the repo as the durable technical source:

- `README.md` for the short project orientation.
- `docs/architecture-plan.md` for the current system plan.
- `docs/cross-tool-sync-plan.md` for artifact ownership.
- `docs/mvp-backlog.md` for the seed backlog before Linear is created.
- Future `docs/adr/` records for durable architecture decisions.

Branch and PR convention:

- `plan/bootstrap-docs`
- `infra/aws-gpu-host-poc`
- `app/control-plane-shell`
- `library/upload-import-pipeline`
- `streaming/parsec-host-hardening`

PR bodies should link the relevant Linear issue, Notion page, and Figma frame
when those artifacts exist.

### Linear

Create a project:

- Name: `Netplay OS Bridge MVP`
- Summary: `Cloud-hosted remote couch co-op with a Windows GPU host, Parsec play path, and browser control plane.`
- Milestones:
  - `M0 Planning Sync`
  - `M1 Streaming Proof Of Concept`
  - `M2 Control Plane Shell`
  - `M3 Upload And Library Pipeline`
  - `M4 Hardening And Cost Guardrails`

Use Linear for execution state only. Avoid storing long-form architecture there;
link back to Notion/GitHub for durable explanations.

## Sync Rules

1. Every major decision gets a GitHub ADR and a Notion decision entry.
2. Every Linear epic links to its GitHub doc and any Figma frame.
3. Every PR links to its Linear issue.
4. Every Figma page has a matching Notion/GitHub reference section.
5. Status lives in Linear; rationale lives in Notion/GitHub; diagrams live in
   Figma.
6. When a decision changes, update the ADR first, then update Notion, then add a
   Linear comment to the affected work.

## Minimum First Sync

To make the workspace coherent, create these four external anchors:

1. Notion page: `Netplay OS Bridge`
2. Figma file: `Netplay OS Bridge`
3. Linear project: `Netplay OS Bridge MVP`
4. GitHub branch/PR: `plan/bootstrap-docs`

Then cross-link them in this order:

1. Put GitHub repo URL on the Notion page.
2. Put Notion page URL in the Linear project description.
3. Put Figma file URL in Notion and Linear.
4. Put Notion, Figma, and Linear URLs in the GitHub PR body.

