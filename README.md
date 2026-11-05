# fabric_rayfinapps

A repository layout for building **Microsoft Fabric Apps** (Rayfin) with an AI agent (Claude Code) and releasing them
to a **DEV** and a **PRO** Fabric workspace.

## Why this repo exists

Fabric App items aren't supported by Fabric's Git integration or by deployment pipelines yet (they're not in either
[supported items list](https://learn.microsoft.com/fabric/cicd/git-integration/intro-to-git-integration#supported-items)).
A workspace connected to Git simply ignores them. So nothing in Fabric versions your app for you: **the source code
in this repo is the app**, and the Rayfin CLI deploys it.

```
repo ──► app (source) ──npx rayfin up──► DEV workspace   latest version   reads the DEV semantic model
                      ──npx rayfin up──► PRO workspace   production       reads the PRO semantic model
```

## Layout

```
fabric_rayfinapps/
├── CLAUDE.md                     instructions for the agent: read this first
├── fabric-apps/
│   ├── .deployment/              how each app is deployed
│   │   ├── README.md             rules common to every app + release flow + apps table
│   │   ├── STYLE.md              how to write an app's deployment file
│   │   └── _template.md          copy it for a new app: model, destination workspace, general info
│   ├── _template-app/            copy it to start a new app
│   │   ├── context/              what the agent reads before building
│   │   │   ├── README.md         the brief: who, what for, which questions, which data
│   │   │   ├── images/           screenshots, sketches, the current report (images work very well)
│   │   │   ├── docs/             business definitions, meeting notes, requirements
│   │   │   └── source/           model definitions, sample queries, existing code
│   │   └── .docs/
│   │       └── METRICS.md        generated: what each page shows and how (see docs/metrics-strategy.md)
│   ├── <App1>/
│   └── <App2>/
└── docs/
    ├── new-app.md                how to create a new app or a new version, step by step
    └── metrics-strategy.md       METRICS.md: the master of every number the app shows
```

## Quick start: a new app

```bash
git checkout -b feature/crm-app
cp -r fabric-apps/_template-app fabric-apps/crm-app
# fill fabric-apps/crm-app/context/ (brief, images, docs, source)
claude
> Follow CLAUDE.md and docs/new-app.md and create the first version of fabric-apps/crm-app.
```

Then deploy to DEV, test, merge, release to PRO: [docs/new-app.md](docs/new-app.md).
