# Example Feature

**Status:** Active  
**Owner:** @handle  
**Related ADR:** [0001 - Example Decision](../adrs/0001-example-decision.md)

---

## Overview

<!-- One paragraph: what this feature does and why it exists. -->

<Feature name> allows <users/systems> to <do something> so that <outcome/value>.

## Usage

<!-- How does a developer or operator use this feature? Include code snippets. -->

```bash
# Example command or API call
curl -X POST https://api.example.com/v1/feature \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"key": "value"}'
```

## Configuration

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `FEATURE_ENABLED` | `bool` | `true` | Enables/disables the feature |
| `FEATURE_TIMEOUT_MS` | `int` | `5000` | Request timeout in milliseconds |

## API Surface

<!-- List relevant endpoints, events, or interfaces. -->

### `POST /v1/feature`

**Request body:**
```json
{
  "key": "string"
}
```

**Response:**
```json
{
  "id": "string",
  "status": "string"
}
```

## Known Limitations

- <limitation 1>

## Changelog

| Version | Change |
|---------|--------|
| v1.0.0 | Initial release |
