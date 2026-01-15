# Lyftr.ai Backend Assignment—Webhook Ingestion API

A FastAPI-based webhook ingestion service that verifies HMAC signatures, stores messages in SQLite with idempotency, and exposes retrieval & analytics endpoints. Includes Docker Compose setup, structured JSON logs, and automated tests.
## Tech Stack
- **Python 3.11**
- **FastAPI**
- **SQLite**(persistent volume in Docker)
- **Pytest**(tests)
- **Docker+Docker Compose**
## Features
- **POST `/webhook`**
  - HMAC-SHA256 signature verification using `X-Signature`
  - Payload validation
  - Idempotent insert using `message_id` (exactly-once)
- **GET `/messages`**
  - Pagination: `limit`, `offset`
  - Filters: `from`, `since`, `q`
  - Deterministic ordering: `ts ASC, message_id ASC`
- **GET `/stats`**
  - Total messages,unique senders,top senders,first/last timestamps
- **Health checks**
  - `/health/live`
  - `/health/ready`
- **Metrics (Prometheus format)**
  - `/metrics`
- **Structured JSON logging**
  - One JSON log per request
  - Webhook logs include `message_id`, `dup`, `result`

---

## Environment Variables
Create a `.env` file using `.env.example`.

Required:
- `WEBHOOK_SECRET`—Secret used to validate webhook HMAC signature
- `DATABASE_URL`—SQLite path

Example:
```env
WEBHOOK_SECRET=testsecret
DATABASE_URL=sqlite:////data/app.db
LOG_LEVEL=INFO
