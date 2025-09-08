# Environment Setup Guide

Complete setup instructions for configuring this repository locally. After setup, see [Build & Run Guide](build-run.md) to start the application.

> **Prerequisites**: Complete [Prerequisites](prerequisites.md) first.

## 1. Get the Repository

```bash
git clone <repository-url>
cd ubi-strapi-provider-mw
npm install
```

## 2. Setup External Services

### Strapi CMS (Required)

This application requires a running Strapi CMS instance for benefits catalog management.

**Setup Strapi Provider:**
1. Clone: https://github.com/PSMRI/ubi-strapi-provider
2. Follow its setup instructions
3. Start Strapi (typically runs on port 1337)
4. Note your `STRAPI_URL` and obtain `STRAPI_TOKEN`

### Database Setup

**Option A: Docker Compose (Recommended)**
```bash
# Use project's Docker setup if available
docker-compose up -d postgres
```

**Option B: Docker Container**
```bash
docker run --name postgres-ubi \
  -e POSTGRES_DB=ubi_pilot \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 -d postgres:15
```

**Option C: Local PostgreSQL**
Create database manually after installing PostgreSQL locally.

> **Important**: After creating your database (using any option above), you must set the `DATABASE_URL` in your environment variables to match your database configuration.

## 3. Configure Environment

### Create Environment File
```bash
cp .env.example .env
```

> **Note**: The `.env.example` file contains all required variables with placeholders. [[memory:8198252]]

### Configure Variables
Edit your `.env` file with the required values. See **[Environment Variables Guide](environment-variables.md)** for:
- Complete list of all variables
- Copy-paste ready sample template
- Detailed descriptions of each variable

**Key variables to configure:**
- `DATABASE_URL` - Set to match your database setup from Step 2
- `STRAPI_URL` and `STRAPI_TOKEN` - From your Strapi setup  
- `ENCRYPTION_KEY` - Generate using: `openssl rand -base64 32`
- `BPP_ID` and `BPP_URI` - Your provider identifiers

## 4. Initialize Database

```bash
npx prisma generate
npx prisma migrate dev
```

## 5. Setup Complete

Your environment is now configured and ready to run. 

**Next Step**: See **[Build & Run Guide](build-run.md)** to start the application in development or production mode.

## Troubleshooting Setup

### Common Setup Issues

**Database Migration Failed**
- Verify PostgreSQL is running and accessible
- Check `DATABASE_URL` format and credentials  
- Ensure database exists and is empty for first setup

**Strapi Setup Issues**
- Ensure Strapi CMS is running before configuring this application
- Verify `STRAPI_URL` is accessible
- Obtain valid `STRAPI_TOKEN` from Strapi admin

**Environment Configuration**
- Verify all required variables are set in `.env` file
- Generate proper `ENCRYPTION_KEY` using: `openssl rand -base64 32`
- Ensure `.env` file format is correct (no spaces around `=`)

### Test Setup
```bash
# Verify database connection
npx prisma db pull

# Verify Prisma client generation
npx prisma generate --help
```

## Next Steps

1. **[Build & Run Guide](build-run.md)** - Start the application
2. **[Environment Variables](environment-variables.md)** - Complete configuration reference
3. **[System Overview](system-overview.md)** - Understand the architecture