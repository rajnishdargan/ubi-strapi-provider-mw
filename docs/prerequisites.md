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