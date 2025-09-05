# Database Configuration

PostgreSQL with Prisma ORM setup.

## Recommended Setup

### Docker Compose (Recommended)
Use Docker Compose for consistent PostgreSQL setup:
```bash
# Includes PostgreSQL + pgAdmin web interface
# See docker-compose.yml in project root
docker-compose up postgres pgadmin -d
```

### Connection Configuration
Set the `DATABASE_URL` environment variable:

```bash
# Local development
DATABASE_URL="postgresql://postgres:password@localhost:5432/ubi_dev"

# Production
DATABASE_URL="postgresql://user:pass@prod-db:5432/ubi_prod"
```

### Database Management Options
- **pgAdmin Web Interface**: http://localhost:5050 (via Docker Compose)
- **psql Command Line**: `psql -h localhost -U postgres -d ubi_dev`

## Prisma Setup

### Official Documentation Links

- **[Prisma Getting Started](https://www.prisma.io/docs/getting-started)** - Initial setup and installation
- **[Database Connection](https://www.prisma.io/docs/concepts/database-connectors/postgresql)** - PostgreSQL configuration
- **[Prisma Migrate](https://www.prisma.io/docs/concepts/components/prisma-migrate)** - Database migrations
- **[Prisma Studio](https://www.prisma.io/docs/concepts/components/prisma-studio)** - Database browser tool

### Quick Reference Commands

```bash
# Generate client (after schema changes)
npx prisma generate

# Apply migrations
npx prisma migrate dev

# Open database browser
npx prisma studio
```

For detailed commands and options, see the [official Prisma CLI reference](https://www.prisma.io/docs/reference/api-reference/command-reference).

## File Locations
- **Schema**: `prisma/schema.prisma`
- **Migrations**: `prisma/migrations/`

## Troubleshooting

### Common Issues
1. **Connection problems**: Check `DATABASE_URL` format
2. **Migration issues**: See [Prisma Migrate troubleshooting](https://www.prisma.io/docs/guides/database/troubleshooting-orm/help-articles/nextjs-prisma-client-dev-practices)
3. **Schema sync**: Refer to [Prisma schema documentation](https://www.prisma.io/docs/concepts/components/prisma-schema)

For comprehensive troubleshooting, visit the [Prisma troubleshooting guide](https://www.prisma.io/docs/guides/database/troubleshooting-orm).