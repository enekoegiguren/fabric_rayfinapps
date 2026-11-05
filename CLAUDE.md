# CLAUDE.md — fabric_rayfinapps

Fabric Apps (Rayfin) built with Claude Code and deployed to a DEV and a PRO Fabric workspace. Fabric App items are
**not** in Fabric's Git integration: this repo is the only version history of each app.

## Read before working on an app
| File | What it gives you |
|------|-------------------|
| `fabric-apps/<app>/context/` | The brief, images, docs and source the user prepared. Read all of it. Images are examples to follow (layout, style, the report being replaced), not assets to ship |
| `fabric-apps/<app>/CLAUDE.md` | App-specific rules, once the app exists (create it with the first version) |
| `fabric-apps/.deployment/README.md` | Rules common to every deployment and the release flow |
| `fabric-apps/.deployment/<app>.md` | DEV / PRO model and DEV / PRO destination workspace of this app |
| `docs/new-app.md` | Step-by-step workflow for a new app or a new version |
| `docs/metrics-strategy.md` | How `.docs/METRICS.md` is generated and why it's never edited by hand |

In the app folder, load `.agents/skills/rayfin/SKILL.md` (scaffolded by Rayfin) before writing Rayfin code, and the
connectors skill before touching `rayfin/connectors/` or `rayfin/rayfin.yml`.

## Rules
1. **Work on a feature branch** (`feature/<app>-app` or `feature/<app>-<change>`). Never commit to `main` directly.
2. **Read the business from the semantic model.** Every number the app shows that also exists in a report comes from a
   measure of the model (`fabric-semanticmodel` connector, `executeQuery`). If the measure doesn't exist, say so and
   propose it for the model; don't rebuild it in TypeScript.
3. **The app's own database is only for what the app creates**: saved views, comments, approvals, usage events.
4. **The committed connector points to DEV.** Only the release switches it to the PRO model, and back.
5. **Deploy only to DEV, and only when asked.** `npx rayfin up list` first: the active target is marked with `*`.
   Never deploy to PRO, never pass `--force`, unless the user explicitly asks in this session.
6. **Keep the docs in step with the code** in the same commit: `.docs/METRICS.md` (regenerate it), the app's
   `CLAUDE.md`, and `fabric-apps/.deployment/<app>.md` when a model, workspace or step changes.
7. **Unknown values are written as `Pending to confirm`**, never guessed (workspace IDs, item IDs, owners).
8. Secrets, tenant IDs and `rayfin/.env*` / `rayfin/.deployments.json` are never committed (see `.gitignore`).
