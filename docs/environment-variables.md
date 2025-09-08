# Environment Variables

Configuration settings for the UBI Strapi Provider Middleware application.

## Environment Variables Description

### Required Variables
- **`DATABASE_URL`**: PostgreSQL database connection string
- **`STRAPI_URL`**: Base URL of the Strapi CMS instance  
- **`STRAPI_TOKEN`**: Authentication token for Strapi API access
- **`BPP_ID`**: ONDC Buyer Platform Provider identifier
- **`BPP_URI`**: Base URI endpoint for the BPP API
- **`ENCRYPTION_KEY`**: Base64 encoded key for data encryption/decryption

### Optional Variables
- **`NODE_ENV`**: Runtime environment (`development`, `production`) - Default: `development`
- **`PORT`**: Application server port - Default: `7000`
- **`LOG_LEVEL`**: Application log level (`error`, `warn`, `info`, `debug`) - Default: `info`
- **`SENTRY_DSN`**: Sentry DSN for error tracking and monitoring

### File Storage Configuration
- **`FILE_STORAGE_PROVIDER`**: File storage provider (`local`, `s3`) - Default: `local`

### AWS S3 Variables (Required only when `FILE_STORAGE_PROVIDER=s3`)
- **`AWS_S3_BUCKET_NAME`**: S3 bucket name for file storage
- **`AWS_ACCESS_KEY_ID`**: AWS access key for S3 operations
- **`AWS_SECRET_ACCESS_KEY`**: AWS secret key for S3 operations
- **`AWS_REGION`**: AWS region where S3 bucket is located - Default: `us-east-1`

## Sample Environment File

Create a `.env` file in your project root and copy the template below:

```bash
# ========================================
# UBI STRAPI PROVIDER MIDDLEWARE CONFIG
# ========================================

# Application Settings
NODE_ENV=
PORT=

# Database Configuration (Required)
DATABASE_URL=

# Strapi CMS Integration (Required)
STRAPI_URL=
STRAPI_TOKEN=

# ONDC Network Configuration (Required)
BPP_ID=
BPP_URI=

# Security (Required)
ENCRYPTION_KEY=

# File Storage
FILE_STORAGE_PROVIDER=

# AWS S3 Configuration (Only if FILE_STORAGE_PROVIDER=s3)
AWS_S3_BUCKET_NAME=
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=

# Monitoring (Optional)
SENTRY_DSN=

# Logging
LOG_LEVEL=
```

## Setup Instructions

1. **Copy the sample template** above into a new `.env` file
2. **Generate encryption key**: `openssl rand -base64 32`
3. **Fill in all required values** marked as (Required) above
4. **Configure optional values** as needed for your environment

## Security Best Practices

1. **Never commit `.env` files** to version control
2. **Use strong encryption keys** - generate with `openssl rand -base64 32`
3. **Rotate tokens regularly** especially in production environments
4. **Use IAM roles** instead of access keys when running on AWS
5. **Restrict database access** to specific IP ranges in production
6. **Use HTTPS** for all external service URLs in production
7. **Keep environment-specific values** in separate files

## Validation

The application validates required environment variables on startup and will exit with an error if any are missing or invalid.
