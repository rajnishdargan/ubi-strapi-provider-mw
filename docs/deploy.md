# Production Deployment

Docker-based production deployment.

## Build and Deploy

```bash
# Build production image
docker build -t ubi-provider:latest .

# Run in production
docker run -d \
  --name ubi-provider \
  --env-file .env.production \
  -p 1338:3000 \
  --restart unless-stopped \
  ubi-provider:latest
```

## Production Configuration

Create `.env.production` with production values:

```bash
# Database
DATABASE_URL="postgresql://prod_user:secure_pass@prod-db:5432/ubi_prod"

# Strapi
STRAPI_URL="https://cms.yourdomain.com"
STRAPI_TOKEN="production_token"

# ONDC
BPP_ID="production.provider.com"
BPP_URI="https://api.provider.com/"

# Storage
FILE_STORAGE_PROVIDER="s3"
AWS_S3_BUCKET_NAME="production-ubi-files"

# Security
ENCRYPTION_KEY="strong_production_key_base64"
NODE_ENV="production"
```

## Database Migrations

```bash
# Apply production migrations
docker exec ubi-provider npx prisma migrate deploy
```

## Monitoring

- **Application**: http://localhost:1338/
- **API Documentation**: http://localhost:1338/documentation