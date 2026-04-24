# Infrastructure

All infrastructure is managed as code. We use **Terraform** for AWS resources and **Helm** for Kubernetes workloads.

## Layout

```
infrastructure/
├── terraform/
│   ├── rds/          → PostgreSQL RDS instance
│   ├── sqs/          → SQS queues (main + DLQ)
│   ├── s3/           → Audit log + PDF storage
│   └── ses/          → SES sending identity
└── deployment/
    └── kubernetes.md → K8s manifests reference
```

## Environments

| Environment | AWS Account | EKS Cluster |
|-------------|-------------|-------------|
| dev | `123456789` | `cs-dev-cluster` |
| staging | `234567890` | `cs-staging-cluster` |
| prod | `345678901` | `cs-prod-cluster` |

## Applying changes

```bash
cd infrastructure/terraform/rds
terraform init
terraform plan -var-file=envs/prod.tfvars
terraform apply -var-file=envs/prod.tfvars
```

> **Never** apply to prod without a plan review in the PR.
