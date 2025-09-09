# System Overview

The **UBI Strapi Provider Middleware** is a NestJS-based backend service that processes benefit applications through the ONDC network.

## What It Does

- **Application Management**: Lists and manages benefit applications from Strapi CMS
- **Document Verification**: Verifies uploaded application documents
- **Application Processing**: Approves or rejects benefit applications
- **ONDC Protocol**: Implements ONDC provider standards

## System Context

This service connects:
- **Strapi CMS** (benefits catalog management)
- **ONDC Network** (standardized benefit discovery)
- **Consumer Applications** (end-user interfaces)

## Technology Stack

- **Runtime**: Node.js 20+
- **Framework**: NestJS (TypeScript)
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: JWT tokens
- **File Storage**: Local filesystem or AWS S3
- **API Documentation**: Swagger/OpenAPI

## Security Features

- JWT authentication with role-based access
- Data encryption with configurable keys
- Input validation and sanitization
- Secure file upload handling
- Audit logging for application changes

## Infrastructure Requirements

- PostgreSQL Database
- Strapi CMS Instance
- File Storage (Local or AWS S3)
