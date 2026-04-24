# cs-api

REST API for the Correspondence System. Built on .NET 8 / ASP.NET Core.

## Base URL

| Environment | URL |
|-------------|-----|
| Local | `http://localhost:5000` |
| Staging | `https://cs-api.staging.selecthealth.org` |
| Production | `https://cs-api.selecthealth.org` |

## Authentication

All endpoints require a valid JWT bearer token issued by the internal IdP.

```http
Authorization: Bearer <token>
```

## Endpoints

See [endpoints.md](endpoints.md) for the full reference.

## Running locally

```bash
dotnet restore
dotnet run --project src/api
```

Or with Docker:

```bash
docker compose up api
```

## Environment variables

| Variable | Required | Description |
|----------|----------|-------------|
| `ConnectionStrings__Default` | ✅ | PostgreSQL connection string |
| `Sqs__QueueUrl` | ✅ | SQS queue URL |
| `Jwt__Issuer` | ✅ | JWT issuer |
| `Jwt__Audience` | ✅ | JWT audience |
| `Feature__DigitalDelivery` | | Enable digital delivery (default: `true`) |
