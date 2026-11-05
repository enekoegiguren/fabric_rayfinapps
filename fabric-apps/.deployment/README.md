# Fabric Apps — Deployment (DEV → PRO)

How the Fabric Apps in [`fabric-apps/`](..) are deployed. This file holds the rules common to every app. Each app has
its own file here, written with [STYLE.md](STYLE.md) from [_template.md](_template.md), with the same three sections:

1. **Model**: DEV model | PRO model. The semantic model (or other source) the app reads, and the lakehouse behind it.
2. **Destination workspace**: DEV workspace | PRO workspace. Where the app item is deployed.
3. **General info**: status, upstream dependencies, access, release commands, pending.

## Apps

| App | DEV model | PRO model | DEV workspace | PRO workspace | File |
|---|---|---|---|---|---|
| _(example)_ CRM Hub | CRM (DEV_SEM) | CRM (PRO_SEM) | DEV_Reporting Marketing | Reporting Marketing | `crm-hub.md` |

## How it works

Fabric App items are **not** synced by Fabric's Git integration, even in a Git-connected workspace, and deployment
pipelines don't move them. The source lives in this repo; the Rayfin CLI builds and deploys it.

```
repo: fabric-apps/<app>  ──npx rayfin up──►  <DEV target>   (DEV workspace)
                         ──npx rayfin up──►  <PRO target>   (PRO workspace)
```

- **Git** = history of the code (branches, PRs, merge to `main`).
- **Rayfin CLI** = deployment. `npx rayfin up` always deploys to the **active** target (`npx rayfin up list`).
- Each `rayfin up` deploys the static app, the schema of the app's own database (`rayfin/data`) and the runtime
  settings and connectors in `rayfin/rayfin.yml`. Data is never deployed.

## Rules for every app

1. **Order of release: upstream first.** Gold notebooks → semantic model → app.
2. **Switch back to DEV after every release.** `npm run dev` and `npx rayfin up` use the active target. If PRO is left
   active, local development runs against the **PRO database**.
3. **Each target has its own app database.** Only code and schema move; the DEV database holds test data.
4. **Schema changes go to DEV first.** Destructive changes (dropping or renaming a field or entity) need `--force`.
   Never use `--force` against PRO without a backup and a plan for the data.
5. **The connector is one value for the whole project**, not per target. Apps reading a semantic model switch the
   connector's `workspaceId` / `itemId` in `rayfin/rayfin.yml` to the PRO model only for the release. **The committed
   version always points to DEV.**
6. **Targets are local.** `rayfin/.deployments.json` and `rayfin/.env` are gitignored, so a new machine doesn't know
   them. Register them once with `npx rayfin up --workspace-id <id> --item-name "<item>"`, always with `--dry-run`
   first to check it reuses the existing item and doesn't create a new one.
7. **Sign-in redirect URIs.** Each target's URL must be in `services.auth.allowedRedirectUris` in `rayfin/rayfin.yml`.
   Don't remove any of them.
8. **Data access.** Delegated connectors (semantic model) run as the signed-in user: users need **Read + Build** on the
   model of their environment, and its RLS decides the rows.
9. **Check the deployed app**, not only the CLI: open the PRO URL and check it shows PRO data.

## Release flow

All commands from the app folder. Target names and the model to point to are in the app's file.

```bash
# 1. develop against DEV
npx rayfin up list                   # the active target is marked with *
npx rayfin up switch <DEV target>
npm run dev                          # local app on the DEV backend
npx rayfin up                        # deploy to DEV, test on the DEV URL

# 2. feature branch → PR → merge to main

# 3. release to PRO (from the merged code, upstream already in PRO)
npx rayfin up switch <PRO target>
#    rayfin/rayfin.yml: connector → PRO model
npx rayfin up --dry-run
npx rayfin up
#    rayfin/rayfin.yml: connector → back to DEV
npx rayfin up switch <DEV target>
```

## Common checklist

- [ ] Change tested on the DEV URL
- [ ] `.docs/METRICS.md` regenerated and committed
- [ ] PR merged to `main`
- [ ] Upstream changes (notebooks, model) released to PRO
- [ ] Schema change? Released to DEV first, no `--force` on PRO
- [ ] Switch to PRO target → PRO connector → `--dry-run` → `npx rayfin up` → DEV connector
- [ ] PRO URL checked
- [ ] Switched back to the DEV target (`npx rayfin up list`)

## Adding a new app

1. Copy [_template.md](_template.md) to `<app>.md` and fill its three sections. Unknown values: **Pending to confirm**.
2. Register both targets (`--dry-run` first).
3. Add the app to the [Apps](#apps) table.

## Future: CI release

Microsoft documents a GitHub Actions workflow that runs `rayfin up` with a service principal
([Deploy a Fabric app with GitHub Actions](https://learn.microsoft.com/fabric/apps/deploy-github-actions)). The PRO
release could become that workflow with a manual approval, once the connector switch is parameterised.
