# System Overview

## Repository Role

The **UBI Strapi Provider Middleware** is a NestJS-based backend service that processes benefit applications through the ONDC network.


### Primary Functions
- **Application Listing**: Lists applications for each benefit from Strapi CMS
- **Application Verification**: Verifies submitted application documents
- **Application Approval**: Approves or rejects benefit applications
- **ONDC Protocol Implementation**: Serves as provider for benefit services

### System Context
This service acts as a bridge between:
- **Strapi CMS** (hosted separately, manages benefits catalog)
- **ONDC Network** (standardized benefit discovery and application)
- **Consumer Applications** (web applications for end users)

## Core Features

### 📋 Application Management
- Lists applications for each benefit
- Manages application status (pending → verified → approved/rejected)
- Tracks application history

### 📄 Document Verification
- Accepts document uploads for verification
- Supports local file storage and AWS S3
- Verifies document metadata

### ✅ Application Approval
- Reviews verified applications
- Approves or rejects applications
- Updates application status

### 🔐 Authentication & Authorization
- JWT-based authentication
- Role-based access control
- Secure API endpoints

### 🔌 External Integrations
- **Strapi CMS**: Retrieves benefits catalog (hosted separately)
- **ONDC Network**: Protocol compliance
- **Storage Services**: Local filesystem or AWS S3

## Technical Architecture

### Technology Stack
- **Runtime**: Node.js 20+
- **Framework**: NestJS (TypeScript)
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: JWT tokens
- **File Storage**: Local filesystem or AWS S3
- **Background Jobs**: NestJS Schedule module
- **API Documentation**: Swagger/OpenAPI
- **Monitoring**: Sentry for error tracking

### Key Dependencies
- **@nestjs/core**: Application framework
- **@prisma/client**: Database ORM
- **@nestjs/swagger**: API documentation
- **@aws-sdk/client-s3**: Cloud file storage
- **@nestjs/schedule**: Cron job scheduling
- **@sentry/nestjs**: Error monitoring

### Database Schema
- **Applications**: Application records and status tracking
- **ApplicationFiles**: Document uploads and verification metadata
- **Users**: System users and authentication


## Security Model

### Data Protection
- **Encryption**: Sensitive data encrypted using configurable encryption keys
- **Key Rotation**: Support for encryption key rotation with migration
- **Environment Variables**: Secure configuration management
- **Input Validation**: Request validation using class-validator

### Access Control
- **JWT Authentication**: Secure token-based authentication
- **Role-Based Access**: Different access levels for users
- **API Security**: Protected endpoints with authorization middleware
- **Audit Logging**: Action logs for application changes

### Network Security
- **HTTPS**: Secure communication protocols
- **Rate Limiting**: Protection against abuse (configurable)
- **Input Sanitization**: Protection against injection attacks
- **File Upload Validation**: Secure file handling

## Performance & Configuration

### Scalability Features
- Stateless design allows multiple instances
- Database connection pooling
- Configurable background job scheduling
- Scalable storage options (local/cloud)

### Monitoring
- Health checks for application and dependencies
- Error tracking with Sentry
- Audit trails for application changes

### Configuration
- Environment-based configuration (development/production)
- Flexible database connection settings
- Switchable file storage (local/cloud)
- Configurable security settings

## System Requirements

### Infrastructure Dependencies
- **PostgreSQL Database**: Primary data storage
- **Strapi CMS Instance**: Benefits catalog management
- **File Storage**: Local filesystem or AWS S3 bucket

### Maintenance Operations
- Database migrations using Prisma
- Encryption key rotation support
- Health monitoring for application and dependencies

## Integration Points

### Internal Modules
- **Benefits Module**: ONDC protocol implementation
- **Applications Module**: Application management
- **ApplicationFiles Module**: Document management
- **Auth Module**: Authentication and authorization
- **Strapi Admin Module**: CMS integration

### External Systems
- **Strapi CMS**: Benefits catalog (hosted separately)
- **ONDC Network**: Protocol compliance
- **Consumer Applications**: Application submission
- **Storage Services**: File storage and retrieval

This system processes and manages benefit applications through standardized verification and approval workflows.
