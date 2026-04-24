# On-Call Runbook

Welcome to the Correspondence System on-call. This page covers the most common alerts and how to handle them.

## Alert index

| Alert | Severity | Likely cause |
|-------|----------|-------------|
| `CorrespondenceQueueDepthHigh` | P2 | Worker scaled down or stuck |
| `CorrespondenceDLQNonEmpty` | P1 | Permanent failures, needs investigation |
| `CorrespondenceApiErrorRateHigh` | P2 | Bad deploy or downstream dependency |
| `CorrespondenceWorkerCrashLoop` | P1 | OOM or bad config |
| `RdsConnectionsNearLimit` | P2 | Connection pool leak |

---

## CorrespondenceQueueDepthHigh

**Threshold:** SQS `ApproximateNumberOfMessagesVisible` > 500 for 5 min.

```bash
# Check worker pod count
kubectl get pods -n correspondence -l app=cs-worker

# Check HPA status
kubectl describe hpa cs-worker -n correspondence

# Check for worker errors
kubectl logs -l app=cs-worker -n correspondence --since=10m | grep ERROR
```

If pods look healthy but queue isn't draining, check the SQS console for in-flight message count. High in-flight with low visible means workers are processing but slowly.

---

## CorrespondenceDLQNonEmpty

**This is a P1.** Messages in the DLQ mean permanent failures — items that never delivered.

```bash
# Count DLQ messages
aws sqs get-queue-attributes \
  --queue-url https://sqs.us-west-2.amazonaws.com/.../cs-jobs-dlq \
  --attribute-names ApproximateNumberOfMessages

# Peek at a message to understand the failure
aws sqs receive-message \
  --queue-url https://sqs.us-west-2.amazonaws.com/.../cs-jobs-dlq \
  --max-number-of-messages 1
```

Check the `correspondence_request` table:

```sql
SELECT id, member_id, template_id, status, retry_count, metadata, updated_at
FROM correspondence.correspondence_request
WHERE status = 'DEAD_LETTER'
ORDER BY updated_at DESC
LIMIT 20;
```

After fixing the root cause, redrive via the AWS console or:

```bash
aws sqs start-message-move-task \
  --source-arn arn:aws:sqs:us-west-2:...:cs-jobs-dlq \
  --destination-arn arn:aws:sqs:us-west-2:...:cs-jobs
```

---

## See also

- [P1 Incident Response](incidents/p1-response.md)
- [Data Flow](../architecture/data-flow.md)
