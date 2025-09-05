# Environment Variables

Configuration variables for the UBI Strapi Provider Middleware application.

## Required Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `DATABASE_URL` | PostgreSQL connection string | `postgresql://user:pass@host:port/database?schema=public` |
| `STRAPI_URL` | Strapi CMS instance URL | `http://localhost:1337` |
| `STRAPI_TOKEN` | Strapi API authentication token | `your_strapi_api_token` |
| `BPP_ID` | Business Provider Platform ID | `your-provider-id` |
| `BPP_URI` | Provider endpoint URI | `https://your-provider.com/` |
| `BAP_ID` | Business Application Platform ID | `your-consumer-id` |
| `BAP_URI` | Consumer platform endpoint URI | `https://your-consumer.com/` |
| `DOMAIN` | ONDC domain identifier | `ubi:financial-support` |
| `FILE_STORAGE_PROVIDER` | Storage backend | `s3` or `local` |
| `ENCRYPTION_KEY` | 32-byte base64 encryption key | Generate with command below |

## Optional Variables

| Variable | Description | Default | Example |
|----------|-------------|---------|---------|
| `PORT` | Application port | `7000` | `3000` |
| `PROVIDER_UBA_UI_URL` | Provider UI URL | - | `https://provider-ui.com` |
| `FILE_PREFIX_ENV` | File prefix for storage | `local` | `dev`, `prod` |

## Background Jobs Configuration

| Variable | Description | Default | Example |
|----------|-------------|---------|---------|
| `BENEFIT_CALCULATIONS_CRON_TIME` | Cron for benefit calculations | - | `*/30 * * * *` |
| `BENEFIT_CALCULATIONS_BATCH_SIZE` | Batch size for calculations | `10` | `20` |
| `BENEFIT_CALCULATIONS_LAST_PROCESS_HOURS` | Process window in hours | - | `24` |
| `ELIGIBILITY_CHECK_CRON_TIME` | Cron for eligibility checks | - | `*/30 * * * *` |
| `ELIGIBILITY_CHECK_BATCH_SIZE` | Batch size for eligibility | - | `10` |
| `ELIGIBILITY_CHECK_LAST_PROCESS_HOURS` | Process window in hours | - | `24` |

## External Services

| Variable | Description | Default | Example |
|----------|-------------|---------|---------|
| `VERIFICATION_SERVICE_URL` | Document verification service | `http://localhost:3000/verification` | Custom URL |
| `DEFAULT_ISSUER_NAME` | Default document issuer | `dhiway` | `your-issuer` |
| `ELIGIBILITY_API_URL` | Eligibility service endpoint | `http://localhost:5678` | Custom URL |

## Monitoring & Error Tracking

| Variable | Description | Required | Example |
|----------|-------------|----------|---------|
| `SENTRY_DSN` | Sentry error tracking DSN | No | `https://key@sentry.io/project` |
| `SENTRY_ENVIRONMENT` | Sentry environment label | No | `production`, `staging` |

## AWS S3 Configuration (if using S3 storage)

| Variable | Description | Required for S3 | Example |
|----------|-------------|-----------------|---------|
| `AWS_REGION` | AWS region | Yes | `us-east-1` |
| `AWS_ACCESS_KEY_ID` | AWS access key | Yes | `AKIAIOSFODNN7EXAMPLE` |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key | Yes | `wJalrXUtnFEMI/K7MDENG/bPxRfiCY` |
| `AWS_S3_BUCKET_NAME` | S3 bucket name | Yes | `my-app-files` |

## Key Rotation

| Variable | Description | When Needed |
|----------|-------------|-------------|
| `OLD_ENCRYPTION_KEY` | Previous encryption key | Only during key rotation |

## Key Generation

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"
```

## Environment Examples

### Development
```bash
PORT=7000
DATABASE_URL="postgresql://postgres:password@localhost:5432/ubi_dev?schema=public"
STRAPI_URL="http://localhost:1337"
STRAPI_TOKEN="dev_token"
BPP_ID="dev.provider.local"
BPP_URI="http://localhost:7000/"
BAP_ID="dev.consumer.local"
BAP_URI="http://localhost:7000/"
DOMAIN="ubi:financial-support"
FILE_STORAGE_PROVIDER="local"
FILE_PREFIX_ENV="dev"
ENCRYPTION_KEY="dev_key_base64_32_bytes"
```

### Production
```bash
PORT=7000
DATABASE_URL="postgresql://user:pass@prod-db:5432/ubi_prod?schema=public"
STRAPI_URL="https://cms.yourdomain.com"
STRAPI_TOKEN="prod_secure_token"
BPP_ID="production.provider.com"
BPP_URI="https://api.provider.com/"
BAP_ID="production.consumer.com"
BAP_URI="https://api.consumer.com/"
DOMAIN="ubi:financial-support"
FILE_STORAGE_PROVIDER="s3"
FILE_PREFIX_ENV="prod"
AWS_S3_BUCKET_NAME="production-files"
AWS_REGION="us-east-1"
ENCRYPTION_KEY="strong_production_key_base64"
```

## Notes

- **Copy from sample**: Use `cp env.sample .env` to start
- **Never commit** `.env` files to version control
- **Key rotation**: Use `OLD_ENCRYPTION_KEY` only when rotating encryption keys
- **File storage**: `local` stores in `./uploads`, `s3` uses AWS S3 with prefix
- **Cron format**: `minute hour day month weekday` (e.g., `*/30 * * * *` = every 30 minutes)