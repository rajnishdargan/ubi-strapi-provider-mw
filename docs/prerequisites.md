# Prerequisites

Required software and services to run this project locally.

## Required

- **Node.js** 20.x+ and **npm**
- **PostgreSQL** 14+ (local install or Docker)
- **Strapi CMS** instance
  - Repository: https://github.com/PSMRI/ubi-strapi-provider
  - Must be running before starting this application
- **UBI Verification SDK** service
  - Repository: https://github.com/PSMRI/ubi-verification-sdk
  - Provides credential verification capabilities
  - Must be accessible at the URL specified in VERIFICATION_SERVICE_URL
- **UBI Eligibility SDK** service
  - Repository: https://github.com/PSMRI/ubi-eligibility-sdk
  - Provides eligibility checking capabilities
  - Must be accessible at the URL specified in ELIGIBILITY_API_URL

## Optional

- **AWS Account** (only if using S3 for file storage)
- **Sentry Account** (for error tracking in production)

## Verification

### System Requirements
```bash
node --version  # Should be 20.x+
npm --version
psql --version  # If installed locally
```

### Service Health Checks
```bash
# Check Strapi CMS
curl http://localhost:1337/_health

# Check UBI Verification SDK
curl http://your-verification-service-url/health

# Check UBI Eligibility SDK  
curl http://your-eligibility-service-url/health
```

Replace `your-verification-service-url` and `your-eligibility-service-url` with the actual URLs from your environment configuration.