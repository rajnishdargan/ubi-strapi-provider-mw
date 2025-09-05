# Production Deployment

Simple Docker deployment for production.

## Build Docker Image

```bash
# Build image
docker build -t ubi-provider:latest .

# Test locally
docker run --env-file .env -p 3000:3000 ubi-provider:latest
```

## Production Environment

### Create Production .env
```bash
# On production server
nano .env.production
```

### Configure for Production
```bash
# Database (production PostgreSQL)
DATABASE_URL="postgresql://prod_user:secure_pass@prod-db:5432/ubi_prod"

# Strapi (production)
STRAPI_URL="https://cms.yourdomain.com"
STRAPI_TOKEN="production_token"

# ONDC (production endpoints)
BPP_ID="production.provider.com"
BPP_URI="https://api.provider.com/"

# File Storage (S3 for production)
FILE_STORAGE_PROVIDER="s3"
AWS_S3_BUCKET_NAME="production-ubi-files"

# Security
ENCRYPTION_KEY="strong_production_key_base64"
NODE_ENV="production"
```

## Run in Production

```bash
# Stop existing container
docker stop ubi-provider || true
docker rm ubi-provider || true

# Run new container
docker run -d \
  --name ubi-provider \
  --env-file .env.production \
  -p 1338:3000 \
  --restart unless-stopped \
  ubi-provider:latest

# Check logs
docker logs -f ubi-provider
```

## Health Check

```bash
# Verify running
curl http://localhost:1338/

# Check API docs
curl http://localhost:1338/documentation

# View logs
docker logs ubi-provider
```

## Update Application

```bash
# Pull new image
docker pull your-registry/ubi-provider:latest

# Stop and restart
docker stop ubi-provider
docker rm ubi-provider

# Run with new image (same command as above)
docker run -d --name ubi-provider...
```

## Database Migrations

```bash
# Run production migrations
docker exec ubi-provider npx prisma migrate deploy
```