# Database Schema

PostgreSQL schema for the `cs-db` database. All tables are in the `correspondence` schema.

## Entity relationships

```mermaid
erDiagram
    MEMBER {
        uuid id PK
        string member_id UK
        string first_name
        string last_name
        string email
        string preferred_channel
        timestamp created_at
        timestamp updated_at
    }

    CORRESPONDENCE_REQUEST {
        uuid id PK
        uuid member_id FK
        uuid template_id FK
        string status
        string trigger_type
        string trigger_ref
        int retry_count
        jsonb metadata
        timestamp created_at
        timestamp updated_at
    }

    TEMPLATE {
        uuid id PK
        string slug UK
        string name
        string version
        text html_content
        boolean is_active
        timestamp created_at
    }

    DELIVERY_RECORD {
        uuid id PK
        uuid request_id FK
        string channel
        string vendor_ref
        string status
        timestamp delivered_at
        jsonb response_payload
    }

    AUDIT_EVENT {
        uuid id PK
        uuid request_id FK
        string actor
        string action
        jsonb before_state
        jsonb after_state
        timestamp occurred_at
    }

    MEMBER ||--o{ CORRESPONDENCE_REQUEST : "receives"
    TEMPLATE ||--o{ CORRESPONDENCE_REQUEST : "used by"
    CORRESPONDENCE_REQUEST ||--o{ DELIVERY_RECORD : "produces"
    CORRESPONDENCE_REQUEST ||--o{ AUDIT_EVENT : "tracked by"
```

## Key indexes

```sql
-- Hot path: worker picks up pending jobs
CREATE INDEX idx_requests_status_created
    ON correspondence.correspondence_request(status, created_at)
    WHERE status IN ('PENDING', 'FAILED');

-- Member history lookup
CREATE INDEX idx_requests_member_id
    ON correspondence.correspondence_request(member_id, created_at DESC);

-- Audit log queries by request
CREATE INDEX idx_audit_request_id
    ON correspondence.audit_event(request_id, occurred_at DESC);
```

## Migrations

Migrations live in `src/api/Migrations/` and run automatically on startup via EF Core.
