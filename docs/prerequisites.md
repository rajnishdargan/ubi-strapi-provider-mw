# Prerequisites

Software and services needed before setup.

## Required Software

### Install These First
```bash
# Node.js v20+ (for running the app)
# Visit https://nodejs.org/ or use package manager:
# Ubuntu: curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - && sudo apt-get install -y nodejs
# macOS: brew install node@20

# PostgreSQL v13+ (for database)
# Ubuntu: sudo apt install postgresql postgresql-contrib
# macOS: brew install postgresql && brew services start postgresql  
# Windows: Download from https://www.postgresql.org/

# Git (for code)
# Usually pre-installed, or: sudo apt install git / brew install git
```

### Verify Installation
```bash
node --version    # Should show v20+
psql --version    # Should show v13+
git --version     # Should show version info
```

## Required Services

You'll need accounts/access to:

| Service | Purpose | What You Need |
|---------|---------|---------------|
| **Strapi CMS** | Content management | Running instance + API token |
| **ONDC Platform** | Benefits integration | BPP/BAP registration + IDs |

## Optional Services

| Service | Purpose | When Needed |
|---------|---------|-------------|
| **AWS S3** | File storage | Production deployment |
| **Sentry** | Error tracking | Production monitoring |

## Next Steps

Once you have the software installed:
1. Follow [Setup Guide](setup.md) to configure the application
2. Use [Environment Variables](configuration/environment-variables.md) for configuration details

## Need Help?

- **Node.js**: https://nodejs.org/docs/
- **PostgreSQL**: https://www.postgresql.org/docs/  
- **Strapi**: https://docs.strapi.io/
- **ONDC**: https://ondc.org/