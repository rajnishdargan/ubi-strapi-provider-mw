# Build & Run Guide

Instructions for running the UBI Strapi Provider Middleware after completing [Environment Setup](env-setup.md).

> **Prerequisites**: Complete [Environment Setup](env-setup.md) before running these commands.

## A. Running Locally without Docker

### Development Server
```bash
# Start with hot reload (recommended for development)
npm run start:dev

# Start in debug mode
npm run start:debug
```

**Access Points:**
- **Application**: http://localhost:7000
- **API Documentation**: http://localhost:7000/documentation
- **Health Check**: http://localhost:7000 (basic response)

### Database Operations (Development)
These commands are for ongoing development work, not initial setup:

```bash
npx prisma studio        # Database GUI
npx prisma generate      # Regenerate client after schema changes
npx prisma migrate dev   # Apply new migrations in development
```

For more Prisma commands, see the [official Prisma documentation](https://www.prisma.io/docs).

### Production Build (Local)
```bash
# Build for production
npm run build

# Run production build
npm run start:prod
```

## B. Running with Docker

### Development with Docker
```bash
# Build image
docker build -t ubi-strapi-provider-mw .

# Run container
docker run -d \
  --name ubi-provider \
  --env-file .env \
  -p 7000:7000 \
  ubi-strapi-provider-mw
```

### Docker Compose (Recommended)
```yaml
# docker-compose.yml example
version: '3.8'
services:
  ubi-provider:
    build: .
    ports:
      - "7000:7000"
    environment:
      - DATABASE_URL=postgresql://postgres:password@postgres:5432/ubi_prod
      - STRAPI_URL=http://strapi:1337
      - STRAPI_TOKEN=${STRAPI_TOKEN}
      - ENCRYPTION_KEY=${ENCRYPTION_KEY}
    depends_on:
      - postgres
      - strapi
      
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: ubi_prod
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
      
volumes:
  postgres_data:
```

```bash
# Start services
docker-compose up -d

# View logs
docker-compose logs -f ubi-provider
```

### Production Docker Deployment
```bash
# Build production image
docker build -t ubi-provider:latest .

# Run in production
docker run -d \
  --name ubi-provider \
  --env-file .env.production \
  -p 7000:7000 \
  --restart unless-stopped \
  ubi-provider:latest
```


## Requirements & Troubleshooting

### System Requirements
- Node.js 20+ environment
- PostgreSQL 12+ database
- Strapi CMS instance
- HTTPS for production

### Common Runtime Issues
- **Port conflicts**: Change `PORT` environment variable or stop conflicting services
- **Application crashes on startup**: Check environment variables in [Environment Variables](environment-variables.md)
- **Database connection errors**: Ensure PostgreSQL is running and `DATABASE_URL` is correct
- **Strapi connection errors**: Verify Strapi CMS is running and accessible
- **File upload failures**: Check storage permissions (local) or AWS credentials (S3)

### Health Check
```bash
curl http://localhost:7000/
```
