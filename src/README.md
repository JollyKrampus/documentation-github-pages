# Source Code

The `src/` directory contains all application code for the Correspondence System.

## Structure

```
src/
├── api/          → cs-api: REST API, auth, EF Core migrations
├── worker/       → cs-worker: job processor, template engine, PDF renderer
├── portal/       → cs-portal: React member portal
└── shared/       → Shared domain models and interfaces
```

## Getting started

```bash
# API
cd src/api && dotnet run

# Worker
cd src/worker && dotnet run

# Portal
cd src/portal && npm install && npm run dev
```

## See also

- [API Docs](api/README.md)
- [Correspondence Service](services/correspondence/README.md)
