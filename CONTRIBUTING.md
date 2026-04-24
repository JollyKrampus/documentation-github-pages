# Contributing

## Adding documentation

Drop a `.md` file anywhere in the repo. MkDocs will find it automatically on the next push to `main`. No nav config needed.

### Naming conventions

| Type | Location | Example |
|------|----------|---------|
| Architecture | `architecture/` | `architecture/caching-strategy.md` |
| ADR | `docs/adrs/` | `docs/adrs/0004-switch-to-redis.md` |
| Feature | `docs/features/` | `docs/features/letter-preview.md` |
| Runbook | `runbooks/` | `runbooks/database-failover.md` |
| Component README | next to code | `src/api/README.md` |

### Adding Mermaid diagrams

````md
```mermaid
flowchart LR
  A --> B
```
````

## Pull request process

1. Branch from `main`
2. Make changes
3. Open PR — docs deploy preview runs automatically
4. Squash merge

## Code style

We use `pre-commit`. Run `pre-commit install` after cloning.
