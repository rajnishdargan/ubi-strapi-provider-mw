# Setup Guide

Step-by-step guide to get the application running locally.

> **First time?** Complete [Prerequisites](prerequisites.md) before continuing.

## 1. Get the Code

```bash
git clone [repository-url]
cd ubi-strapi-provider-mw
npm install
```

## 2. Setup Database
```bash
# Connect to PostgreSQL
sudo -u postgres psql

# Create database and user
CREATE DATABASE ubi_provider;
CREATE USER ubi_user WITH PASSWORD 'secure_password';
GRANT ALL PRIVILEGES ON DATABASE ubi_provider TO ubi_user;
\q
```

## 3. Configure Environment
```bash
# Copy example file
cp .env.example .env

# Edit with your values
nano .env
```

**Need help with variables?** See [Environment Variables](configuration/environment-variables.md)

### Generate Encryption Key
```bash
# Generate secure key
openssl rand -base64 32
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
✅ **No errors** in console

## Troubleshooting

### Database Connection
```bash
# Check PostgreSQL is running
sudo systemctl status postgresql

# Test connection
psql -h localhost -U ubi_user -d ubi_provider
```

### Port Conflicts
- PostgreSQL: 5432
- Application: 3000

Change ports if needed or stop conflicting services.