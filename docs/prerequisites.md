# Prerequisites

Software and services needed before setup.

## Required Software

### Core Requirements
- **Node.js v20+** - For running the application
- **Docker & Docker Compose** - For PostgreSQL and development environment
- **Git** - For code management

### Verify Installation
```bash
node --version      # Should show v20+
docker --version    # Should show recent version
docker-compose --version
git --version
```

## Required Services Setup

### 1. Strapi CMS Provider
Set up the Strapi CMS instance using the official repository:

**Repository**: [PSMRI/ubi-strapi-provider](https://github.com/PSMRI/ubi-strapi-provider)

Follow the setup instructions in that repository to:
- Configure PostgreSQL database
- Create admin account
- Set up custom collections
- Generate API token

### 2. ONDC Platform Registration
Register for ONDC platform access to obtain:
- Business Provider Platform (BPP) ID and URI
- Business Application Platform (BAP) ID and URI
- Domain configuration

## Database Setup Recommendation

### Use Docker Compose for Local Development
```bash
# Recommended: Use Docker Compose for PostgreSQL
# This provides consistent environment across all platforms
# Includes pgAdmin web interface for database management

# Alternative: Use psql command line for database operations
```

## Optional Services

### External SDKs (Choose Local Setup OR Deployed Endpoints)

#### UBI Verification SDK
**Purpose**: Document and credential verification  
**Options**:
- **Local Setup**: Clone and run [PSMRI/ubi-verification-sdk](https://github.com/PSMRI/ubi-verification-sdk)
- **Deployed Service**: Use existing endpoint via `VERIFICATION_SERVICE_URL`

#### UBI Eligibility SDK  
**Purpose**: Benefit eligibility processing and validation  
**Options**:
- **Local Setup**: Clone and run [PSMRI/ubi-eligibility-sdk](https://github.com/PSMRI/ubi-eligibility-sdk)
- **Deployed Service**: Use existing endpoint via `ELIGIBILITY_API_URL`

### Cloud Services

| Service | Purpose | When Needed |
|---------|---------|-------------|
| **AWS S3** | File storage | Production deployment |
| **Sentry** | Error tracking | Production monitoring |

## Next Steps

Once you have the prerequisites:
1. Follow [Setup Guide](setup.md) to configure the application
2. Use [Environment Variables](configuration/environment-variables.md) for configuration details

## Documentation Links

- **Node.js**: https://nodejs.org/docs/
- **Docker**: https://docs.docker.com/
- **Strapi Setup**: https://github.com/PSMRI/ubi-strapi-provider
- **Verification SDK**: https://github.com/PSMRI/ubi-verification-sdk
- **Eligibility SDK**: https://github.com/PSMRI/ubi-eligibility-sdk  
- **ONDC**: https://ondc.org/