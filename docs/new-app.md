# How to create a new app (or a new version)

## 1. Branch

```bash
git checkout main && git pull
git checkout -b feature/crm-app          # new app
git checkout -b feature/crm-app-alerts   # new version of an existing app
```

## 2. Folder and context (new app only)

```bash
cp -r fabric-apps/_template-app fabric-apps/crm-app
```

Fill `fabric-apps/crm-app/context/`:

| Folder | Put here | Why |
|---|---|---|
| `context/README.md` | The brief: problem, users, questions, data, out of scope | The agent's starting point |
| `context/images/` | Screenshots of the current report, sketches, an app style you like | **Images work very well as examples.** One screenshot replaces a page of description |
| `context/docs/` | Business definitions, meeting notes, requirements | The words the business uses |
| `context/source/` | Model definition (TMDL), sample DAX, existing code | What the agent must reuse, not reinvent |

For a new version, write what changes in `context/` too (a new image, a new note): the context grows with the app.

## 3. Build with the agent

```bash
cd fabric_rayfinapps
claude
```

```text
Follow CLAUDE.md and docs/new-app.md. Read everything in fabric-apps/crm-app/context/
and create the first version of the app in fabric-apps/crm-app: Rayfin project,
semantic model connector to the DEV model, the pages in the brief. Create the app's
CLAUDE.md, .deployment/crm-app.md from the template, and the metrics generator.
Don't deploy yet.
```

The first answer should be a plan, not code: check it uses measures of the model and doesn't copy data.

## 4. Deploy to DEV and test

```bash
cd fabric-apps/crm-app
npx rayfin up --workspace-id <dev-workspace-id> --item-name "CRM Hub" --dry-run
npx rayfin up --workspace-id <dev-workspace-id> --item-name "CRM Hub"
npm run docs:metrics
```

Open the DEV URL. Iterate with the agent until it's right.

## 5. PR and merge

The PR contains the code, the app's `CLAUDE.md`, `.docs/METRICS.md` and `.deployment/crm-app.md`. Review
`METRICS.md` with the business owner: it's the readable list of every number the app shows.

## 6. Release to PRO

Follow `fabric-apps/.deployment/crm-app.md` → Release to PRO, and the common checklist in
`fabric-apps/.deployment/README.md`. Switch back to the DEV target at the end.
