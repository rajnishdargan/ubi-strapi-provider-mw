# Build & Run Guide

Complete guide for setting up and running the UBI Strapi Provider Middleware.

> **Prerequisites**: Complete [Prerequisites](prerequisites.md) first.

## Build and Deploy Steps

## A. Running Locally without Docker

### Clone Repository

```bash
# Clone the Repository
git clone https://github.com/PSMRI/ubi-strapi-provider-mw.git
cd ubi-strapi-provider-mw

# Check out the main branch
git checkout main
```

### Install Dependencies

```bash
# Install dependencies
npm install
```

### Optional - Set up Git hooks

```bash
# Set up Git hooks (Husky)
npm run prepare
git config core.hooksPath .husky/_
chmod +x .husky/pre-commit
chmod +x .husky/commit-msg
```

### Setup External Services

**Strapi CMS (Required)**

This application requires a running Strapi CMS instance for benefits catalog management.

1. Clone: https://github.com/PSMRI/ubi-strapi-provider
2. Follow its setup instructions
3. Start Strapi (typically runs on port 1337)
4. Note your `STRAPI_URL` and obtain `STRAPI_TOKEN`

**Database Setup**

Choose one of the following options:

**Option A: Docker Container**
```bash
docker run --name postgres-ubi \
  -e POSTGRES_DB=ubi_strapi_provider_mw \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=password \
  -p 5432:5432 -d postgres:14
```

**Option B: Local PostgreSQL**
Create database manually after installing PostgreSQL locally.

### Create / Update .env

Copy `.env.sample` to `.env` and update with your values:

```bash
# Copy .env.sample to .env
cp .env.sample .env

# Edit .env file with your actual values
# - DATABASE_URL (match your database setup)
# - STRAPI_URL and STRAPI_TOKEN (from your Strapi setup)
# - ENCRYPTION_KEY (generate with: openssl rand -base64 32)
# - BPP_ID and BPP_URI (your provider identifiers)
```

### Initialize Database

```bash
npx prisma generate
npx prisma migrate dev
```

### Running in Dev Mode

```bash
npm run start:dev
```

The backend will be available at: 👉 **http://localhost:7000**

**Access Points:**
- **Application**: http://localhost:7000
- **API Documentation**: http://localhost:7000/documentation
- **Health Check**: http://localhost:7000

### Running in Production Mode

```bash
npm run build
npm run start:prod
```

## B. Running with Docker

### Create / Update .env

Copy `.env.sample` to `.env` and update with your values:

```bash
# Copy .env.sample to .env
cp .env.sample .env

# Edit the .env file with your actual configuration values
```

For all available environment variables, see [Environment Variables](environment-variables.md).

### Example docker-compose.yml

Create a `docker-compose.yml` file in your project root:

```yaml
version: '3.8'
services:
  ubi-provider:
    build: .
    ports:
      - "7000:7000"
    environment:
      - NODE_ENV=production
      - PORT=7000
      - DATABASE_URL=postgresql://postgres:password@postgres:5432/ubi_strapi_provider_mw
      - STRAPI_URL=${STRAPI_URL}
      - STRAPI_TOKEN=${STRAPI_TOKEN}
      - BPP_ID=${BPP_ID}
      - BPP_URI=${BPP_URI}
      - ENCRYPTION_KEY=${ENCRYPTION_KEY}
      - FILE_STORAGE_PROVIDER=local
      - LOG_LEVEL=info
    depends_on:
      - postgres
    volumes:
      - ./uploads:/app/uploads
      
  postgres:
    image: postgres:14
    environment:
      POSTGRES_DB: ubi_strapi_provider_mw
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
      
volumes:
  postgres_data:
```

### Run with Docker Compose

```bash
# Build and start all services
docker-compose up -d

# Build specific service only  
docker-compose build ubi-provider

# Start services
docker-compose up -d
```

### More about docker-compose

The docker-compose.yml provisions the following services:

- **PostgreSQL** (port 5432)
- **UBI Provider API** (port 7000)

### Verify Services

- **Application**: http://localhost:7000
- **API Documentation**: http://localhost:7000/documentation
- **Database**: localhost:5432

### View Logs

```bash
# View logs
docker-compose logs -f ubi-provider
```

## Database Operations (Development)

These commands are for ongoing development work:

```bash
npx prisma studio        # Database GUI
npx prisma generate      # Regenerate client after schema changes
npx prisma migrate dev   # Apply new migrations in development
```

For more Prisma commands, see the [official Prisma documentation](https://www.prisma.io/docs).

## Requirements & Troubleshooting

### System Requirements
- Node.js 20+ environment
- PostgreSQL 14+ database
- Strapi CMS instance
- HTTPS for production

### Common Issues

**Setup Issues**
- **Database migration failed**: Verify PostgreSQL is running and `DATABASE_URL` is correct
- **Strapi connection errors**: Ensure Strapi CMS is running before starting this application
- **Environment configuration**: Check all required variables are set in `.env` file

**Runtime Issues**
- **Port conflicts**: Change `PORT` environment variable or stop conflicting services
- **Application crashes on startup**: Check environment variables in [Environment Variables](environment-variables.md)
- **Database connection errors**: Ensure PostgreSQL is running and accessible
- **File upload failures**: Check storage permissions (local) or AWS credentials (S3)

### Health Check
```bash
curl http://localhost:7000/
```
