# <App display name>

<One line: what the app is.> Code in [`../<app>/`](../<app>). Rules and release flow: [README.md](README.md).

## 1. Model

| | DEV model | PRO model |
|---|---|---|
| Type | Semantic model (Direct Lake / Import) · or own database only | Same |
| Workspace | Pending to confirm | Pending to confirm |
| Item | Pending to confirm | Pending to confirm |
| Reads from | DEV gold lakehouse | PRO gold lakehouse |
| Connector | `<name>` (`fabric-semanticmodel`, delegated) | Same, switched to the PRO model for the release |

## 2. Destination workspace

| | DEV workspace | PRO workspace |
|---|---|---|
| Workspace | Pending to confirm | Pending to confirm |
| Rayfin target | `<dev-target>` | `<pro-target>` |
| Item name | `<App display name>` | `<App display name>` |
| Item id | Pending to confirm | Pending to confirm |
| App database | `<Entity>`, ... | Same tables |
| URL | Pending to confirm | Pending to confirm |

## 3. General info

### Status
In development.

### Upstream dependencies
Notebooks and model changes this app needs, in release order. None.

### Access
Users need **Read + Build** on the model of their environment.

### First-time setup

```bash
npx rayfin up --workspace-id <dev-workspace-id> --item-name "<App display name>" --dry-run
npx rayfin up --workspace-id <dev-workspace-id> --item-name "<App display name>"
npx rayfin up --workspace-id <pro-workspace-id> --item-name "<App display name>" --dry-run
npx rayfin up --workspace-id <pro-workspace-id> --item-name "<App display name>"   # with the PRO connector
```

### Release to PRO

```bash
npx rayfin up switch <pro-target>
# rayfin/rayfin.yml: connector <name> → <PRO workspace> / <PRO model id>
npx rayfin up --dry-run
npx rayfin up
# rayfin/rayfin.yml: connector <name> → back to DEV
npx rayfin up switch <dev-target>
```

### Pending and incidences
None.
