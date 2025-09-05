# Database Configuration

PostgreSQL with Prisma ORM setup.

## Setup

### Database
Use Docker Compose for PostgreSQL with pgAdmin interface.

### Connection
Configure via `DATABASE_URL` environment variable:
```bash
DATABASE_URL="postgresql://user:pass@host:port/database?schema=public"
```

## Prisma Management

### Official Documentation
- **[Prisma Getting Started](https://www.prisma.io/docs/getting-started)** - Setup and installation
- **[Prisma Migrate](https://www.prisma.io/docs/concepts/components/prisma-migrate)** - Database migrations
- **[Prisma Studio](https://www.prisma.io/docs/concepts/components/prisma-studio)** - Database browser

### Key Commands
```bash
npx prisma generate     # Generate client
npx prisma migrate dev  # Apply migrations
npx prisma studio       # Database browser
```

## File Locations
- **Schema**: `prisma/schema.prisma`
- **Migrations**: `prisma/migrations/`

## Troubleshooting
For detailed troubleshooting: [Prisma Troubleshooting Guide](https://www.prisma.io/docs/guides/database/troubleshooting-orm)