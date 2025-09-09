# Environment Variables

Create a `.env` file in your project root and copy the variables below:

```bash
# ========================================
# UBI STRAPI PROVIDER MIDDLEWARE CONFIG
# ========================================

# Runtime environment (development, production)
NODE_ENV=development

# Application server port
PORT=7000

# PostgreSQL database connection string (Required)
DATABASE_URL=postgresql://postgres:password@localhost:5432/ubi_strapi_provider_mw

# Base URL of the Strapi CMS instance (Required)
STRAPI_URL=http://localhost:1337

# Authentication token for Strapi API access (Required)
STRAPI_TOKEN=your_strapi_api_token_here

# BPP identifier (Required)
BPP_ID=your_bpp_id_here

# Base URI endpoint for the BPP API (Required)
BPP_URI=https://your-domain.com

# BAP identifier (Required)
BAP_ID=your_bap_id_here

# Base URI endpoint for the BAP API (Required)
BAP_URI=https://your-bap-domain.com

# ONDC domain specification (Required)
DOMAIN=ubi:financial-support

# Base64 encoded key for data encryption/decryption (Required)
# Generate with: openssl rand -base64 32
ENCRYPTION_KEY=your_base64_encryption_key_here

# UBI Provider UI URL (Required)
PROVIDER_UBA_UI_URL=https://your-provider-ui-domain.com

# UBI Verification Service configuration (Required)
VERIFICATION_SERVICE_URL=https://your-verification-service-domain.com/verification
DEFAULT_ISSUER_NAME=dhiway

# UBI Eligibility Service configuration (Required)
ELIGIBILITY_API_URL=http://your-eligibility-service-domain:port

# Environment prefix for file names (Required)
# Options: local, dev, prod
FILE_PREFIX_ENV=local

# File storage provider (local or s3)
FILE_STORAGE_PROVIDER=local

# AWS S3 bucket name for file storage (Required only if FILE_STORAGE_PROVIDER=s3)
AWS_S3_BUCKET_NAME=your_s3_bucket_name

# AWS access key for S3 operations (Required only if FILE_STORAGE_PROVIDER=s3)
AWS_ACCESS_KEY_ID=your_aws_access_key

# AWS secret key for S3 operations (Required only if FILE_STORAGE_PROVIDER=s3)
AWS_SECRET_ACCESS_KEY=your_aws_secret_key

# AWS region where S3 bucket is located
AWS_REGION=us-east-1

# Application log level (error, warn, info, debug)
LOG_LEVEL=info

# Sentry DSN for error tracking and monitoring (Optional)
SENTRY_DSN=your_sentry_dsn_here

# Benefit calculations cron job configuration
BENEFIT_CALCULATIONS_BATCH_SIZE=50
BENEFIT_CALCULATIONS_LAST_PROCESS_HOURS=8
BENEFIT_CALCULATIONS_CRON_TIME=*/1 * * * *

# Eligibility check cron job configuration
ELIGIBILITY_CHECK_LAST_PROCESS_HOURS=8
ELIGIBILITY_CHECK_BATCH_SIZE=50
ELIGIBILITY_CHECK_CRON_TIME=*/1 * * * *
```

## Additional Configuration Notes

### Required Variables
All variables marked as "(Required)" must be configured for the application to start properly.

### Generate Encryption Key
```bash
openssl rand -base64 32
```

### File Storage Options
- **local**: Files stored in the `uploads/` directory
- **s3**: Files stored in AWS S3 bucket (requires AWS credentials)

### Database URL Format
```
postgresql://username:password@host:port/database_name
```

### Verification Service
- **VERIFICATION_SERVICE_URL**: Full URL to the UBI Verification SDK endpoint
- **DEFAULT_ISSUER_NAME**: Default issuer for credential verification (e.g., "dhiway")

### Eligibility Service
- **ELIGIBILITY_API_URL**: Base URL for the UBI Eligibility SDK service

### Cron Job Configuration
- **BATCH_SIZE**: Number of records to process in each batch
- **LAST_PROCESS_HOURS**: Look back period in hours for unprocessed records
- **CRON_TIME**: Cron expression for job scheduling (format: minute hour day month weekday)

### File Storage Configuration
- **FILE_PREFIX_ENV**: Environment prefix for file naming (local/dev/prod)
- **FILE_STORAGE_PROVIDER**: Storage backend (local or s3)

## Security Best Practices

1. **Never commit `.env` files** to version control
2. **Use strong encryption keys** - generate with the command above
3. **Rotate tokens regularly** especially in production
4. **Use IAM roles** instead of access keys when running on AWS
5. **Use HTTPS** for all external service URLs in production

## Validation

The application validates required environment variables on startup and will exit with an error if any are missing or invalid.
