# UBI Strapi Provider Middleware

NestJS application for ONDC benefits processing with Strapi CMS integration.

## 🚀 Quick Start

**New to this project?** Follow this order:

1. **[Prerequisites](prerequisites.md)** - Install required software first
2. **[Setup Guide](setup.md)** - Configure and run the application
3. **[Environment Variables](configuration/environment-variables.md)** - Complete configuration reference

**Already have everything installed?**

```bash
# Clone and run
git clone [repository]
cd ubi-strapi-provider-mw
npm install
cp .env.example .env    # Edit with your settings
npx prisma generate && npx prisma migrate dev
npm run start:dev
```

**Access**: http://localhost:3000 • **API Docs**: http://localhost:3000/documentation

## 📖 Documentation

- **[Prerequisites](prerequisites.md)** - Required software (install first)
- **[Setup Guide](setup.md)** - Step-by-step application setup  
- **[Deploy Guide](deploy.md)** - Production deployment
- **[Contributing](contributing.md)** - Development guidelines

### Reference
- **[Environment Variables](configuration/environment-variables.md)** - All configuration options
- **[Database Config](configuration/database-config.md)** - Prisma and PostgreSQL setup
- **[Sample Data](sample-data.md)** - Example API requests and responses

## Tech Stack

- NestJS + TypeScript
- PostgreSQL + Prisma
- AWS S3 / Local storage
- Swagger/OpenAPI

## Quick Help

- **Database issues?** Check `DATABASE_URL` in `.env`
- **API not working?** Visit http://localhost:3000/documentation
- **File upload issues?** Check `FILE_STORAGE_PROVIDER` setting


