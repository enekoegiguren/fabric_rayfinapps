# Style for deployment files

Every app file in `.deployment/` reads the same way, so anyone (or any agent) can release any app without asking.

## Shape
- Title = the app's display name. One line under it: what the app is, and a link to its folder and to `README.md`.
- Always the same three sections, in this order: `## 1. Model`, `## 2. Destination workspace`, `## 3. General info`.
- Sections 1 and 2 are **two-column tables: DEV | PRO**. Same rows in every app file, in the same order.
- Section 3 has the same sub-headings in every app: Status, Upstream dependencies, Access, First-time setup,
  Release to PRO, Pending and incidences. Leave a heading with "None" rather than removing it.

## Content
- Names **and** IDs: `Reporting Marketing` `762d0937-…`. Names change; IDs don't.
- Commands are copy-pasteable, in `bash` blocks, with the real target names.
- Only what's different from `README.md`. Common rules are linked, not repeated.
- Unknown values are written **Pending to confirm**, never guessed. Missing environments are **Pending** in bold.
- Warnings that cost data or a broken PRO go in a `> **Never ...**` blockquote, next to the step they belong to.
- Short sentences. Dates as `YYYY-MM-DD`. English.

## Keep it current
- The file changes in the same PR as the change that makes it wrong (new model, new workspace, new step).
- Update the Apps table in `README.md` at the same time.
