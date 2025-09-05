# Setup Guide

Application setup and configuration.

> **Prerequisites**: Complete [Prerequisites](prerequisites.md) first.

## 1. Get the Code

```bash
git clone [repository-url]
cd ubi-strapi-provider-mw
npm install
```

## 2. Setup Required Services

### Strapi CMS Provider
Set up Strapi CMS from: https://github.com/PSMRI/ubi-strapi-provider  
This provides your `STRAPI_URL` and `STRAPI_TOKEN` values.

### Database Setup
Use Docker Compose for PostgreSQL with pgAdmin interface.

## 3. Configure Environment

```bash
cp .env.example .env
# Edit .env with your configuration values
```

**Configuration help**: [Environment Variables](configuration/environment-variables.md)

### Generate Encryption Key
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"
```
Copy output to `ENCRYPTION_KEY` in your `.env` file.

## 4. Initialize and Start

```bash
npx prisma generate
npx prisma migrate dev
npm run start:dev
```

## 5. Verify

- **Application**: http://localhost:3000  
- **API Documentation**: http://localhost:3000/documentation  
- **Strapi**: Check your STRAPI_URL

## Troubleshooting

**Common Issues**:
- Database connection: Verify Docker Compose services
- Strapi connection: Check STRAPI_URL and STRAPI_TOKEN in .env
- Environment variables: Review [Environment Variables](configuration/environment-variables.md)