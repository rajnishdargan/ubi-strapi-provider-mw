# Prerequisites

Required software and services to run this project locally.

## Required

- **Node.js** 20.x+ and **npm**
- **PostgreSQL** 14+ (local install or Docker)
- **Strapi CMS** instance
  - Repository: https://github.com/PSMRI/ubi-strapi-provider
  - Must be running before starting this application

## Optional

- **AWS Account** (only if using S3 for file storage)
- **Sentry Account** (for error tracking in production)

## Verification

```bash
node --version  # Should be 20.x+
npm --version
psql --version  # If installed locally
```

## Database Setup Options

**Option 1: Docker (Recommended)**
```bash
docker run --name postgres-ubi \
  -e POSTGRES_DB=ubi_dev \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 -d postgres:15
```

**Option 2: Local PostgreSQL**
Install PostgreSQL on your system and create a database.

## Next Steps

1. **[Setup Guide](env-setup.md)** - Configure and run the application
2. **[Build & Run Guide](build-run.md)** - Build and deployment options