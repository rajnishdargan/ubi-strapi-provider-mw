# Setup Guide

Step-by-step guide to get the application running locally.

> **First time?** Complete [Prerequisites](prerequisites.md) before continuing.

## 1. Get the Code

```bash
git clone [repository-url]
cd ubi-strapi-provider-mw
npm install
```

## 2. Setup Required Services

### Strapi CMS Provider
Set up the Strapi CMS using the official repository:
```bash
# Clone and setup Strapi (in separate directory)
git clone https://github.com/PSMRI/ubi-strapi-provider.git
cd ubi-strapi-provider

# Follow the setup instructions in that repository
# This will provide your STRAPI_URL and STRAPI_TOKEN
```

### Database Setup
**Recommended**: Use Docker Compose for PostgreSQL:
```bash
# Use docker-compose for consistent database setup
# Includes PostgreSQL + pgAdmin web interface
# Check docker-compose.yml in project root or create one
```

## 3. Configure Environment

```bash
# Copy example file
cp .env.example .env

# Edit with your values (including Strapi details from step 2)
nano .env
```

**Need help with variables?** See [Environment Variables](configuration/environment-variables.md)

### Generate Encryption Key
```bash
# Generate secure key
node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"
```

Copy the output to `ENCRYPTION_KEY` in your `.env` file.

## 4. Initialize and Start

```bash
# Setup database
npx prisma generate
npx prisma migrate dev

# Start application
npm run start:dev
```

## 5. Verify It's Working

✅ **App running**: http://localhost:3000  
✅ **API docs**: http://localhost:3000/documentation  
✅ **Strapi running**: Check your STRAPI_URL  
✅ **No errors** in console

## Troubleshooting

### Common Issues
- **Database connection**: Check Docker Compose services are running
- **Strapi connection**: Verify STRAPI_URL and STRAPI_TOKEN in .env
- **Port conflicts**: Ensure ports 3000, 5432 are available
- **Environment variables**: Review [Environment Variables](configuration/environment-variables.md)

### Database Management
- **pgAdmin**: http://localhost:5050 (if using Docker Compose)
- **psql**: `psql -h localhost -U postgres -d ubi_dev`