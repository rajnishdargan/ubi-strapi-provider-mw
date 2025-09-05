# Sample Data

Essential examples for testing the API.

## Basic Application Flow

### 1. Create Application
```bash
POST /applications
Authorization: Bearer YOUR_TOKEN

{
  "benefitId": "benefit-financial-001",
  "applicantData": {
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "+91-9876543210",
    "age": 28,
    "income": 45000
  }
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "id": 123,
    "benefitId": "benefit-financial-001",
    "status": "pending",
    "createdAt": "2024-01-15T10:30:00Z"
  }
}
```

### 2. Upload Document
```bash
POST /applicationFiles
Authorization: Bearer YOUR_TOKEN

{
  "applicationId": 123,
  "documentType": "identity_proof",
  "file": "data:application/pdf;base64,JVBERi0x...",
  "metadata": {
    "originalName": "document.pdf"
  }
}
```

### 3. Check Status
```bash
GET /applications/123
Authorization: Bearer YOUR_TOKEN
```

## ONDC Sample Messages

### Search Benefits
```json
{
  "context": {
    "domain": "ondc:dsep",
    "action": "search",
    "country": "IND"
  },
  "message": {
    "intent": {
      "category": {
        "descriptor": { "name": "Financial Services" }
      }
    }
  }
}
```

### Search Response
```json
{
  "context": { "action": "on_search" },
  "message": {
    "catalog": {
      "providers": [{
        "id": "provider-123",
        "items": [{
          "id": "benefit-001",
          "descriptor": { "name": "Skill Development Grant" },
          "price": { "currency": "INR", "value": "50000" }
        }]
      }]
    }
  }
}
```

## Error Examples

### Validation Error
```json
{
  "status": "error",
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid data",
    "details": [
      {
        "field": "email",
        "message": "Valid email required"
      }
    ]
  }
}
```

### Authentication Error
```json
{
  "status": "error",
  "error": {
    "code": "AUTHENTICATION_FAILED",
    "message": "Invalid or expired token"
  }
}
```

## Environment Examples

### Development
```bash
DATABASE_URL="postgresql://postgres:password@localhost:5432/ubi_dev"
STRAPI_URL="http://localhost:1337"
BPP_ID="dev.provider.local"
FILE_STORAGE_PROVIDER="local"
ENCRYPTION_KEY="dev_key_base64"
```

### Production
```bash
DATABASE_URL="postgresql://user:pass@prod-db:5432/ubi_prod"
STRAPI_URL="https://cms.yourdomain.com"
BPP_ID="production.provider.com"
FILE_STORAGE_PROVIDER="s3"
AWS_S3_BUCKET_NAME="production-files"
ENCRYPTION_KEY="strong_prod_key"
```

## Testing with Swagger

1. **Start app**: `npm run start:dev`
2. **Open Swagger**: http://localhost:3000/documentation
3. **Authenticate**: Click "Authorize" and enter JWT token
4. **Test endpoints**: Try the examples above