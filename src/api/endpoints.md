# API Endpoints

## Correspondence

### `POST /v1/correspondence`

Submit a new correspondence request.

```json
{
  "memberId": "SH-123456",
  "templateSlug": "eob-summary",
  "triggerType": "CLAIM_PROCESSED",
  "triggerRef": "CLM-789",
  "metadata": {
    "claimAmount": 1250.00,
    "serviceDate": "2026-04-01"
  }
}
```

**Response `202 Accepted`:**

```json
{
  "id": "a3f2c1d0-...",
  "status": "PENDING",
  "createdAt": "2026-04-24T15:00:00Z"
}
```

---

### `GET /v1/correspondence/{id}`

Get the status of a correspondence request.

**Response `200 OK`:**

```json
{
  "id": "a3f2c1d0-...",
  "memberId": "SH-123456",
  "templateSlug": "eob-summary",
  "status": "DELIVERED",
  "deliveries": [
    {
      "channel": "DIGITAL",
      "deliveredAt": "2026-04-24T15:02:14Z"
    }
  ]
}
```

---

### `GET /v1/correspondence?memberId={memberId}`

List correspondence for a member.

**Query params:**

| Param | Type | Description |
|-------|------|-------------|
| `memberId` | string | Required |
| `status` | string | Filter by status |
| `from` | ISO date | Start of date range |
| `to` | ISO date | End of date range |
| `page` | int | Default: 1 |
| `pageSize` | int | Default: 20, max: 100 |

---

## Templates

### `GET /v1/templates`

List all active templates.

### `GET /v1/templates/{slug}`

Get a template by slug including the rendered HTML preview.

---

## Health

### `GET /health`

Returns `200` when the API is healthy. Used by the ALB target group.

```json
{ "status": "healthy", "db": "ok", "sqs": "ok" }
```
