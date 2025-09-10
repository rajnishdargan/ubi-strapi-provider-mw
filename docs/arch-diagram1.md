# Provider System - Architecture Diagrams

This document provides a simple overview of the UBI Provider system architecture with three essential diagrams.

## System Overview

The UBI Provider system enables providers to manage benefit applications through ONDC protocol. It consists of three main parts:

- **Provider UI**: React frontend for providers
- **Provider Middleware**: NestJS backend handling business logic  
- **Provider CMS**: Strapi CMS for content management

## 1. High-Level Architecture Diagram

```mermaid
graph TB
    subgraph "Frontend"
        UI[Provider UI<br/>React App]
        style UI fill:#e1f5fe,stroke:#01579b
    end

    subgraph "Backend"
        MW[Provider Middleware<br/>NestJS API]
        DB[(PostgreSQL<br/>Database)]
        FS[File Storage<br/>Local/S3]
        style MW fill:#fff8e1,stroke:#ffa000
        style DB fill:#e8f5e9,stroke:#2e7d32
        style FS fill:#e8f5e9,stroke:#2e7d32
    end

    subgraph "CMS Layer"
        STRAPI[Strapi CMS<br/>Content Management]
        style STRAPI fill:#f3e5f5,stroke:#7b1fa2
    end

    subgraph "External"
        ONDC[UBI / ONEST Network]
        style ONDC fill:#fce4ec,stroke:#c2185b
    end

    UI --> MW
    MW --> DB
    MW --> FS
    MW --> STRAPI
    MW --> ONDC
```

## 2. Sequence Diagrams

### 2.1 Login and View Benefits

```mermaid
sequenceDiagram
    participant User as User
    participant UI as Provider UI
    participant MW as Middleware
    participant CMS as Strapi CMS

    %% Login Flow
    User->>UI: Login
    UI->>MW: POST /auth/login
    MW-->>UI: JWT Token
    UI-->>User: Login Success

    %% View Benefits
    User->>UI: View Benefits
    UI->>MW: GET /benefits
    MW->>CMS: Fetch benefits catalog
    CMS-->>MW: Benefits data
    MW-->>UI: Benefits list
    UI-->>User: Display benefits
```

### 2.2 View Applications and Application Details

```mermaid
sequenceDiagram
    participant User as User
    participant UI as Provider UI
    participant MW as Middleware
    participant DB as Database

    %% View Applications
    User->>UI: View Applications
    UI->>MW: GET /applications
    MW->>DB: Fetch applications
    DB-->>MW: Applications data
    MW-->>UI: Applications list
    UI-->>User: Display applications

    %% View Application Details
    User->>UI: Click Application
    UI->>MW: GET /applications/{id}
    MW->>DB: Fetch application details
    DB-->>MW: Application data
    MW-->>UI: Application details
    UI-->>User: Display details
```

### 2.3 Application Form Submission and Processing

```mermaid
sequenceDiagram
    participant User as User
    participant UI as Provider UI
    participant MW as Middleware
    participant DB as Database
    participant FS as File Storage
    participant CMS as Strapi CMS
    participant ONDC as UBI / ONEST Network

    %% Get Form
    User->>UI: Request Form
    UI->>MW: GET /benefits/{id}/form
    MW->>CMS: Get form schema
    CMS-->>MW: Form configuration
    MW-->>UI: Form schema
    UI-->>User: Display form

    %% Submit Application
    User->>UI: Submit Application
    UI->>MW: POST /applications
    MW->>DB: Save application
    MW->>FS: Store documents
    MW-->>UI: Application created
    UI-->>User: Show confirmation

    %% Background Processing
    Note over MW: Background Jobs
    MW->>DB: Check eligibility
    MW->>DB: Calculate benefits
    MW->>ONDC: Send updates
```

## 3. Data Flow Diagram

```mermaid
graph LR
    subgraph "User Actions"
        LOGIN[Login]
        VIEW_BEN[View Benefits]
        VIEW_APP[View Applications]
        VIEW_DET[View Details]
        SUBMIT[Submit Form]
        style LOGIN fill:#e3f2fd,stroke:#1565c0
        style VIEW_BEN fill:#e3f2fd,stroke:#1565c0
        style VIEW_APP fill:#e3f2fd,stroke:#1565c0
        style VIEW_DET fill:#e3f2fd,stroke:#1565c0
        style SUBMIT fill:#e3f2fd,stroke:#1565c0
    end

    subgraph "API Endpoints"
        AUTH_API["/auth/login"]
        BEN_API["/benefits"]
        APP_API["/applications"]
        FORM_API["/benefits/form"]
        style AUTH_API fill:#fff8e1,stroke:#ffa000
        style BEN_API fill:#fff8e1,stroke:#ffa000
        style APP_API fill:#fff8e1,stroke:#ffa000
        style FORM_API fill:#fff8e1,stroke:#ffa000
    end

    subgraph "Data Sources"
        DB[Database<br/>Applications]
        STRAPI_CMS[Strapi CMS<br/>Benefits & Forms]
        FS[File Storage<br/>Documents]
        ONDC[UBI / ONEST Network<br/>Status Updates]
        style DB fill:#e8f5e9,stroke:#2e7d32
        style STRAPI_CMS fill:#f3e5f5,stroke:#7b1fa2
        style FS fill:#e8f5e9,stroke:#2e7d32
        style ONDC fill:#fce4ec,stroke:#c2185b
    end

    LOGIN --> AUTH_API
    VIEW_BEN --> BEN_API --> STRAPI_CMS
    VIEW_APP --> APP_API --> DB
    VIEW_DET --> APP_API --> DB
    SUBMIT --> APP_API --> DB
    SUBMIT --> APP_API --> FS
    FORM_API --> STRAPI_CMS
    
    APP_API --> ONDC
```

## Key Features

### Frontend (Provider UI)
- Login and authentication
- View available benefits
- View submitted applications
- View application details
- Submit new applications

### Backend (Provider Middleware)
- JWT authentication
- CRUD operations for applications
- File storage management
- Background processing (eligibility & calculations)
- ONDC protocol integration

### CMS (Strapi)
- Benefit configurations
- Form schemas
- Content management

### Background Processing
- Eligibility checking (every 30 minutes)
- Benefit amount calculations (every 30 minutes)
- Document verification
- Status updates to ONDC

This simplified architecture focuses on the core user flows and system interactions that are essential for understanding how the provider system works.