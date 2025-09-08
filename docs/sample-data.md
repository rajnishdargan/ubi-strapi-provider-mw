# Sample Data & API Examples

Comprehensive examples for testing and understanding the API functionality.

## Authentication

### Login Request
```bash
POST /auth/login
Content-Type: application/json

{
  "username": "test@example.com",
  "password": "testpassword"
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": 1,
      "email": "test@example.com",
      "first_name": "Test",
      "last_name": "User",
      "roles": ["provider"]
    }
  }
}
```

## ONDC Protocol Examples

### 1. Search Benefits (Discovery)

**Request:**
```bash
POST /benefits/search
Content-Type: application/json

{
  "context": {
    "domain": "ubi:financial-support",
    "action": "search",
    "country": "IND",
    "city": "std:080",
    "core_version": "1.0.0",
    "bap_id": "pilot.consumer.local",
    "bap_uri": "http://localhost:7000/"
  },
  "message": {
    "intent": {
      "category": {
        "descriptor": {
          "name": "Financial Services"
        }
      },
      "item": {
        "descriptor": {
          "name": "Skill Development Grant"
        }
      }
    }
  }
}
```

**Response (on_search):**
```json
{
  "context": {
    "domain": "ubi:financial-support",
    "action": "on_search",
    "country": "IND",
    "city": "std:080",
    "core_version": "1.0.0",
    "bap_id": "pilot.consumer.local",
    "bap_uri": "http://localhost:7000/",
    "bpp_id": "pilot.provider.local",
    "bpp_uri": "http://localhost:7000/"
  },
  "message": {
    "catalog": {
      "providers": [
        {
          "id": "provider-123",
          "descriptor": {
            "name": "Protean DSEP Scholarships and Grants BPP Platform"
          },
          "categories": [
            {
              "id": "financial-support",
              "descriptor": {
                "name": "Financial Support Services"
              }
            }
          ],
          "items": [
            {
              "id": "benefit-001",
              "descriptor": {
                "name": "Skill Development Grant",
                "short_desc": "Grant for skill development programs",
                "long_desc": "Financial assistance for certified skill development courses"
              },
              "category_ids": ["financial-support"],
              "price": {
                "currency": "INR",
                "value": "50000"
              },
              "tags": [
                {
                  "descriptor": {
                    "name": "Eligibility"
                  },
                  "list": [
                    {
                      "descriptor": {
                        "name": "Age Range"
                      },
                      "value": "18-35"
                    },
                    {
                      "descriptor": {
                        "name": "Income Limit"
                      },
                      "value": "500000"
                    }
                  ]
                }
              ]
            }
          ]
        }
      ]
    }
  }
}
```

### 2. Initialize Application

**Request:**
```bash
POST /benefits/init
Content-Type: application/json

{
  "context": {
    "domain": "ubi:financial-support",
    "action": "init",
    "country": "IND",
    "city": "std:080",
    "bap_id": "pilot.consumer.local",
    "bap_uri": "http://localhost:7000/",
    "bpp_id": "pilot.provider.local",
    "bpp_uri": "http://localhost:7000/"
  },
  "message": {
    "order": {
      "provider": {
        "id": "provider-123"
      },
      "items": [
        {
          "id": "benefit-001",
          "quantity": {
            "selected": {
              "count": 1
            }
          }
        }
      ],
      "customer": {
        "person": {
          "name": "John Doe",
          "gender": "Male",
          "age": "28",
          "skills": ["JavaScript", "Node.js"],
          "languages": ["English", "Hindi"]
        },
        "contact": {
          "phone": "+91-9876543210",
          "email": "john@example.com"
        }
      },
      "fulfillments": [
        {
          "customer": {
            "person": {
              "name": "John Doe"
            },
            "contact": {
              "phone": "+91-9876543210",
              "email": "john@example.com"
            }
          }
        }
      ]
    }
  }
}
```

**Response (on_init):**
```json
{
  "context": {
    "domain": "ubi:financial-support",
    "action": "on_init",
    "bap_id": "pilot.consumer.local",
    "bpp_id": "pilot.provider.local"
  },
  "message": {
    "order": {
      "id": "app-123456",
      "status": "CREATED",
      "provider": {
        "id": "provider-123"
      },
      "items": [
        {
          "id": "benefit-001",
          "descriptor": {
            "name": "Skill Development Grant"
          },
          "price": {
            "currency": "INR",
            "value": "50000"
          }
        }
      ],
      "quote": {
        "price": {
          "currency": "INR",
          "value": "50000"
        }
      }
    }
  }
}
```

### 3. Confirm Application

**Request:**
```bash
POST /benefits/confirm
Content-Type: application/json

{
  "context": {
    "domain": "ubi:financial-support",
    "action": "confirm",
    "bap_id": "pilot.consumer.local",
    "bpp_id": "pilot.provider.local"
  },
  "message": {
    "order": {
      "id": "app-123456",
      "status": "CONFIRMED",
      "customer": {
        "person": {
          "name": "John Doe"
        },
        "contact": {
          "phone": "+91-9876543210",
          "email": "john@example.com"
        }
      },
      "payments": [
        {
          "status": "NOT-PAID",
          "type": "PRE-ORDER",
          "collected_by": "BPP"
        }
      ]
    }
  }
}
```

## Application Management

### Create Application (Direct API)

**Request:**
```bash
POST /applications
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json

{
  "benefitId": "benefit-financial-001",
  "applicationData": {
    "applicant": {
      "name": "John Doe",
      "email": "john@example.com",
      "phone": "+91-9876543210",
      "age": 28,
      "gender": "Male",
      "address": {
        "street": "123 Main Street",
        "city": "Bangalore",
        "state": "Karnataka",
        "pincode": "560001"
      }
    },
    "eligibility": {
      "income": 45000,
      "employment_status": "unemployed",
      "education": "graduation",
      "skills": ["JavaScript", "Node.js", "React"]
    },
    "benefit_details": {
      "requested_amount": 50000,
      "purpose": "Skill development course",
      "course_name": "Full Stack Web Development",
      "institute": "Tech Academy Bangalore"
    }
  },
  "bapId": "pilot.consumer.local",
  "customerId": "customer-456"
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
    "applicationData": "...",
    "bapId": "pilot.consumer.local",
    "customerId": "customer-456",
    "createdAt": "2024-01-15T10:30:00Z",
    "updatedAt": "2024-01-15T10:30:00Z",
    "eligibilityStatus": "pending"
  }
}
```

### List Applications

**Request:**
```bash
GET /applications?page=1&limit=10&status=pending
Authorization: Bearer YOUR_JWT_TOKEN
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "applications": [
      {
        "id": 123,
        "benefitId": "benefit-financial-001",
        "status": "pending",
        "createdAt": "2024-01-15T10:30:00Z",
        "bapId": "pilot.consumer.local",
        "customerId": "customer-456",
        "eligibilityStatus": "pending",
        "finalAmount": null
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 1,
      "totalPages": 1
    }
  }
}
```

### Update Application Status

**Request:**
```bash
PUT /applications/123/status
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json

{
  "status": "approved",
  "remark": "Application approved after document verification",
  "finalAmount": "45000"
}
```

## Document Management

### Upload Document

**Request:**
```bash
POST /applicationFiles
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json

{
  "applicationId": 123,
  "documentType": "identity_proof",
  "documentSubtype": "aadhaar_card",
  "file": "data:application/pdf;base64,JVBERi0xLjQKMSAwIG9iago8PAovVHlwZSAvQ2F0YWxvZwo...",
  "documentSubmissionReason": {
    "reason": "identity_verification",
    "description": "Aadhaar card for identity verification"
  },
  "issuerName": "UIDAI"
}
```

**Response:**
```json
{
  "status": "success",
  "data": {
    "id": 456,
    "applicationId": 123,
    "storage": "local",
    "filePath": "uploads/pilot/123/aadhaar_456.pdf",
    "documentType": "identity_proof",
    "documentSubtype": "aadhaar_card",
    "verificationStatus": {
      "status": "pending",
      "submitted_at": "2024-01-15T10:35:00Z"
    },
    "issuerName": "UIDAI",
    "createdAt": "2024-01-15T10:35:00Z"
  }
}
```

### Get Application Files

**Request:**
```bash
GET /applicationFiles/application/123
Authorization: Bearer YOUR_JWT_TOKEN
```

**Response:**
```json
{
  "status": "success",
  "data": [
    {
      "id": 456,
      "applicationId": 123,
      "storage": "local",
      "filePath": "uploads/pilot/123/aadhaar_456.pdf",
      "documentType": "identity_proof",
      "documentSubtype": "aadhaar_card",
      "verificationStatus": {
        "status": "verified",
        "verified_at": "2024-01-15T11:00:00Z",
        "confidence_score": 95
      },
      "issuerName": "UIDAI",
      "createdAt": "2024-01-15T10:35:00Z",
      "updatedAt": "2024-01-15T11:00:00Z"
    }
  ]
}
```

## Error Responses

### Validation Error
```json
{
  "status": "error",
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "value": "invalid-email",
        "message": "Email must be a valid email address"
      },
      {
        "field": "age",
        "value": 17,
        "message": "Age must be at least 18"
      }
    ]
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Authentication Error
```json
{
  "status": "error",
  "error": {
    "code": "AUTHENTICATION_FAILED",
    "message": "Invalid or expired JWT token"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Resource Not Found
```json
{
  "status": "error",
  "error": {
    "code": "NOT_FOUND",
    "message": "Application with ID 999 not found"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Server Error
```json
{
  "status": "error",
  "error": {
    "code": "INTERNAL_SERVER_ERROR",
    "message": "An unexpected error occurred"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

## Database Sample Data

### Applications Table
```sql
INSERT INTO "Applications" (
  "benefitId", "status", "applicationData", "bapId", "customerId", 
  "eligibilityStatus", "createdAt", "updatedAt"
) VALUES (
  'benefit-financial-001',
  'pending',
  '{"applicant":{"name":"John Doe","email":"john@example.com"}}',
  'pilot.consumer.local',
  'customer-456',
  'pending',
  NOW(),
  NOW()
);
```

### ApplicationFiles Table
```sql
INSERT INTO "ApplicationFiles" (
  "applicationId", "storage", "filePath", "documentType", 
  "documentSubtype", "verificationStatus", "issuerName", 
  "createdAt", "updatedAt"
) VALUES (
  1,
  'local',
  'uploads/pilot/1/aadhaar_1.pdf',
  'identity_proof',
  'aadhaar_card',
  '{"status":"verified","confidence_score":95}',
  'UIDAI',
  NOW(),
  NOW()
);
```

### Users Table
```sql
INSERT INTO "Users" (
  "s_id", "first_name", "last_name", "email", "enabled", 
  "roles", "s_roles", "createdAt"
) VALUES (
  '1',
  'Test',
  'User',
  'test@example.com',
  true,
  ARRAY['provider'],
  ARRAY['Provider Admin'],
  NOW()
);
```

## Environment Configuration Examples

### Development Environment
```bash
# Copy to .env for development
DATABASE_URL="postgresql://postgres:password@localhost:5432/ubi_dev?schema=public"
STRAPI_URL="http://localhost:1337"
STRAPI_TOKEN="dev_token_here"
BPP_ID="dev.provider.local"
BPP_URI="http://localhost:7000/"
BAP_ID="dev.consumer.local"
BAP_URI="http://localhost:7000/"
DOMAIN="ubi:financial-support"
FILE_STORAGE_PROVIDER="local"
FILE_PREFIX_ENV="dev"
ENCRYPTION_KEY="ZGV2X2VuY3J5cHRpb25fa2V5XzMyX2J5dGVz"
PORT=7000
```

### Production Environment
```bash
DATABASE_URL="postgresql://user:pass@prod-db.com:5432/ubi_prod?schema=public"
STRAPI_URL="https://cms.provider.com"
STRAPI_TOKEN="prod_secure_token"
BPP_ID="production.provider.com"
BPP_URI="https://api.provider.com/"
BAP_ID="production.consumer.com"
BAP_URI="https://api.consumer.com/"
DOMAIN="ubi:financial-support"
FILE_STORAGE_PROVIDER="s3"
FILE_PREFIX_ENV="prod"
AWS_S3_BUCKET_NAME="production-provider-files"
AWS_REGION="us-east-1"
ENCRYPTION_KEY="cHJvZF9lbmNyeXB0aW9uX2tleV8zMl9ieXRlcw=="
SENTRY_DSN="https://key@sentry.io/project"
SENTRY_ENVIRONMENT="production"
PORT=7000
```

## Testing with Swagger

1. **Start the application**: `npm run start:dev`
2. **Open Swagger UI**: http://localhost:7000/documentation
3. **Authenticate**: 
   - Click "Authorize" button
   - Enter JWT token in the format: `Bearer your_jwt_token_here`
4. **Test endpoints**: Use the examples above with the interactive Swagger interface

## Postman Collection

Import this basic Postman collection structure:

```json
{
  "info": {
    "name": "UBI Provider API",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "auth": {
    "type": "bearer",
    "bearer": [
      {
        "key": "token",
        "value": "{{jwt_token}}",
        "type": "string"
      }
    ]
  },
  "variable": [
    {
      "key": "base_url",
      "value": "http://localhost:7000"
    },
    {
      "key": "jwt_token",
      "value": "your_jwt_token_here"
    }
  ]
}
```

This sample data covers all major API endpoints and provides realistic examples for testing and development.