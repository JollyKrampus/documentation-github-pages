# Kubernetes Deployment

All workloads run on EKS in the `correspondence` namespace.

## Workload overview

```mermaid
flowchart TD
    subgraph EKS ["EKS — correspondence namespace"]
        subgraph API Pod
            C1[cs-api container]
            C2[envoy sidecar]
        end
        subgraph Worker Pod
            C3[cs-worker container]
            C4[envoy sidecar]
        end
        subgraph Portal Pod
            C5[cs-portal container]
        end
        HPA_API[HPA\nmin:2 max:10]
        HPA_WORKER[HPA\nmin:1 max:20]
        HPA_API --> API Pod
        HPA_WORKER --> Worker Pod
    end

    ALB[AWS ALB\nIngress] --> C2
    ALB --> C5
    C1 --> RDS[(RDS)]
    C1 --> SQS[SQS]
    C3 --> SQS
    C3 --> RDS
    C3 --> S3[S3]
```

## Resource requests

| Workload | CPU request | CPU limit | Memory request | Memory limit |
|----------|-------------|-----------|----------------|--------------|
| cs-api | 250m | 1000m | 256Mi | 512Mi |
| cs-worker | 500m | 2000m | 512Mi | 1Gi |
| cs-portal | 100m | 500m | 128Mi | 256Mi |

## HPA scaling triggers

```yaml
# cs-worker scales on SQS queue depth (custom metric via KEDA)
apiVersion: keda.sh/v1alpha1
kind: ScaledObject
metadata:
  name: cs-worker-scaler
spec:
  scaleTargetRef:
    name: cs-worker
  minReplicaCount: 1
  maxReplicaCount: 20
  triggers:
    - type: aws-sqs-queue
      metadata:
        queueURL: https://sqs.us-west-2.amazonaws.com/.../cs-jobs
        queueLength: "10"     # scale up when >10 messages per replica
```

## Deploying

Deployments are managed via GitHub Actions (`cs-infra` repo). To force a rollout:

```bash
kubectl rollout restart deployment/cs-api -n correspondence
kubectl rollout restart deployment/cs-worker -n correspondence
```

## Checking rollout status

```bash
kubectl rollout status deployment/cs-worker -n correspondence
kubectl get pods -n correspondence -l app=cs-worker
```
