# System Overview

The Correspondence System is a pipeline: triggers come in, letters are composed, routed to the right output channel, and delivered. Here's the full picture.

## High-level architecture

```mermaid
flowchart TD
    subgraph Triggers
        A1[Claim Event] --> BUS
        A2[Enrollment Event] --> BUS
        A3[Manual Batch] --> BUS
    end

    BUS[(Event Bus\nRabbitMQ)]

    BUS --> W[cs-worker\nJob Processor]

    W --> TMPL[Template Engine\nHandlebars]
    TMPL --> PDF[PDF Renderer\nPuppeteer]

    PDF --> ROUTER{Output Router}

    ROUTER -->|digital| PORTAL[Member Portal\ncs-portal]
    ROUTER -->|print| VENDOR[Print Vendor\nSFTP]
    ROUTER -->|email| SES[AWS SES]

    PORTAL --> MEMBER([Member])
    VENDOR --> MAIL([Physical Mail])
    SES --> INBOX([Member Email])

    W --> DB[(PostgreSQL\ncs-db)]
    W --> AUDIT[Audit Log\nS3]
```

## Component responsibilities

| Component | Language | Owns |
|-----------|----------|------|
| `cs-api` | .NET 8 | REST endpoints, auth, request validation |
| `cs-worker` | .NET 8 | Job queue, template rendering, output routing |
| `cs-portal` | React | Digital correspondence viewer |
| `cs-infra` | Terraform | AWS infra, RDS, SQS, SES |

## Deployment topology

```mermaid
flowchart LR
    subgraph AWS us-west-2
        subgraph EKS Cluster
            API[cs-api\nDeployment]
            WORKER[cs-worker\nDeployment]
            PORTAL[cs-portal\nDeployment]
        end
        RDS[(RDS PostgreSQL\nMulti-AZ)]
        SQS[SQS Queue]
        S3[S3 Bucket\nAudit + PDFs]
    end

    ALB[ALB] --> API
    ALB --> PORTAL
    API --> SQS
    SQS --> WORKER
    WORKER --> RDS
    WORKER --> S3
    API --> RDS
```

## Related docs

- [Data Flow](data-flow.md)
- [Database Schema](database-schema.md)
- [Kubernetes Deployment](../infrastructure/deployment/kubernetes.md)
