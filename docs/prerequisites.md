# Prerequisites

Software and services needed before setup.

## Required Software

- **Node.js v20+** - Application runtime
- **Docker & Docker Compose** - Database and services
- **Git** - Code management

## Required Services Setup

### 1. Strapi CMS Provider
Set up the Strapi CMS instance using: [PSMRI/ubi-strapi-provider](https://github.com/PSMRI/ubi-strapi-provider)

Requirements from Strapi setup:
- Configure PostgreSQL database
- Create admin account
- Set up custom collections
- Generate API token

### 2. ONDC Platform Registration
Obtain from ONDC platform:
- Business Provider Platform (BPP) ID and URI
- Business Application Platform (BAP) ID and URI
- Domain configuration

## Optional Services

### External SDKs (Choose Local Setup OR Deployed Endpoints)

#### UBI Verification SDK
- **Local Setup**: [PSMRI/ubi-verification-sdk](https://github.com/PSMRI/ubi-verification-sdk)
- **Deployed Service**: Use existing endpoint via `VERIFICATION_SERVICE_URL`

#### UBI Eligibility SDK  
- **Local Setup**: [PSMRI/ubi-eligibility-sdk](https://github.com/PSMRI/ubi-eligibility-sdk)
- **Deployed Service**: Use existing endpoint via `ELIGIBILITY_API_URL`

### Cloud Services

| Service | Purpose | When Needed |
|---------|---------|-------------|
| **AWS S3** | File storage | Production deployment |
| **Sentry** | Error tracking | Production monitoring |

## Next Steps

1. Follow [Setup Guide](setup.md) to configure the application
2. Use [Environment Variables](configuration/environment-variables.md) for configuration details