# Correspondence System

> Central documentation hub — docs live wherever they make sense. MkDocs picks them all up.

[![Docs](https://github.com/JollyKrampus/documentation-github-pages/actions/workflows/docs.yml/badge.svg)](https://github.com/JollyKrampus/documentation-github-pages/actions/workflows/docs.yml)

## What is this?

The **Correspondence System** handles automated letter generation, routing, and delivery for SelectHealth member communications. It integrates with the member portal, claim systems, and print/mail vendors.

## Quick links

- [Architecture Overview](architecture/system-overview.md)
- [API Docs](src/api/README.md)
- [Runbooks](runbooks/on-call.md)
- [ADRs](docs/adrs/index.md)

## Repos in this org

| Repo | Purpose |
|------|---------|
| `cs-api` | REST API layer |
| `cs-worker` | Background job processor |
| `cs-portal` | Member-facing web UI |
| `cs-infra` | Terraform + Kubernetes configs |
