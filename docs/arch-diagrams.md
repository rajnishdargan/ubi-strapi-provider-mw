# Architecture Diagrams

Visual representations of the system architecture, data flows, and component interactions.

## System Architecture

### Provider System Overview

```mermaid
graph TB
    subgraph "Client Layer"
        UI[Provider UI]
        style UI fill:#e1f5fe,stroke:#01579b
        subgraph "Key UI Components"
            BL[Benefit List]
            AL[Application List]
            AD[Application Details]
            DV[Document Verification]
            style BL fill:#e3f2fd,stroke:#1565c0
            style AL fill:#e3f2fd,stroke:#1565c0
            style AD fill:#e3f2fd,stroke:#1565c0
            style DV fill:#e3f2fd,stroke:#1565c0
        end
    end

    subgraph "API Gateway Layer"
        NGINX[NGINX Reverse Proxy]
        style NGINX fill:#fff3e0,stroke:#ef6c00
    end

    subgraph "Middleware Layer - NestJS"
        style MW fill:#fff8e1,stroke:#ffa000
        MW[UBI Strapi Provider MW]
        subgraph "Core Services"
            AS[Application Service]
            BS[Benefits Service]
            VS[Verification Service]
            CS[Calculation Service]
            style AS fill:#fff8e1,stroke:#ffa000
            style BS fill:#fff8e1,stroke:#ffa000
            style VS fill:#fff8e1,stroke:#ffa000
            style CS fill:#fff8e1,stroke:#ffa000
        end
        subgraph "Background Jobs"
            EC[Eligibility Cron]
            BC[Benefit Calculation Cron]
            style EC fill:#ffecb3,stroke:#ffa000
            style BC fill:#ffecb3,stroke:#ffa000
        end
        DB[(PostgreSQL)]
        style DB fill:#e8f5e9,stroke:#2e7d32
    end

    subgraph "Strapi Layer"
        SP[Strapi CMS]
        style SP fill:#f3e5f5,stroke:#7b1fa2
        subgraph "Content Types"
            BT[Benefits]
            FT[Forms]
            style BT fill:#f3e5f5,stroke:#7b1fa2
            style FT fill:#f3e5f5,stroke:#7b1fa2
        end
    end

    subgraph "External Services"
        S3[AWS S3]
        SDK[Benefit Amount SDK]
        style S3 fill:#fce4ec,stroke:#c2185b
        style SDK fill:#fce4ec,stroke:#c2185b
    end

    UI --> BL
    UI --> AL
    UI --> AD
    UI --> DV

    BL --> NGINX
    AL --> NGINX
    AD --> NGINX
    DV --> NGINX
    NGINX --> MW

    MW --> AS
    MW --> BS
    MW --> VS
    MW --> CS
    AS --> DB
    BS --> DB
    VS --> DB
    CS --> DB

    EC --> AS
    BC --> CS

    VS --> S3
    CS --> SDK
    BS --> SP
    SP --> BT
    SP --> FT

    AS --> S3
```

### Application Processing Flow

```mermaid
sequenceDiagram
    participant P as Provider
    participant UI as Provider UI
    participant NG as NGINX
    participant MW as Middleware
    participant DB as PostgreSQL
    participant SP as Strapi
    participant S3 as AWS S3
    participant SDK as Benefit Amount SDK

    P->>UI: Login
    UI->>+NG: POST /auth/login
    NG->>+MW: Forward Request
    MW->>DB: Validate Credentials
    MW-->>-NG: JWT Token
    NG-->>-UI: Auth Response

    P->>UI: View Applications
    UI->>+NG: GET /applications
    NG->>+MW: Forward Request
    MW->>DB: Fetch Applications
    MW-->>-NG: Applications List
    NG-->>-UI: Display Applications

    P->>UI: Open Application Details
    UI->>+NG: GET /applications/{id}
    NG->>+MW: Forward Request
    MW->>DB: Fetch Application
    MW->>SP: Fetch Benefit Details
    MW-->>-NG: Application + Benefit Data
    NG-->>-UI: Display Details

    P->>UI: Verify Documents
    UI->>+NG: POST /verification/verify-vcs
    NG->>+MW: Forward Request
    MW->>S3: Fetch Documents
    MW->>MW: Verify Documents
    MW->>DB: Update Status
    MW-->>-NG: Verification Result
    NG-->>-UI: Show Status

    Note over UI,SDK: Benefit Amount Calculation (Automated)
    MW->>+SDK: Calculate Benefit Amount
    SDK-->>-MW: Amount Details
    MW->>DB: Update Amount

    P->>UI: Approve/Reject Application
    UI->>+NG: PUT /applications/{id}/status
    NG->>+MW: Forward Request
    MW->>DB: Update Status
    MW->>DB: Log Action
    MW-->>-NG: Updated Status
    NG-->>-UI: Show Confirmation

    Note over MW,DB: Background Jobs
    MW->>MW: Eligibility Cron
    MW->>MW: Benefit Calculation Cron
```

```mermaid
graph TB
    subgraph "Consumer Side"
        BAP[Business Application Platform<br/>Consumer Apps]
    end
    
    subgraph "ONDC Network"
        ONDC[ONDC Protocol Layer<br/>Discovery & Transactions]
    end
    
    subgraph "Provider Infrastructure"
        BPP[UBI Strapi Provider MW<br/>Business Provider Platform]
        CMS[Strapi CMS<br/>Content Management]
        DB[(PostgreSQL<br/>Application Data)]
        FS[File Storage<br/>Local/S3]
    end
    
    subgraph "External Services"
        VERIF[Verification Service<br/>Document Validation]
        ELIG[Eligibility API<br/>Benefit Eligibility]
        SENTRY[Sentry<br/>Error Monitoring]
    end
    
    BAP <--> ONDC
    ONDC <--> BPP
    BPP <--> CMS
    BPP <--> DB
    BPP <--> FS
    BPP <--> VERIF
    BPP <--> ELIG
    BPP --> SENTRY
    
    style BPP fill:#e1f5fe
    style CMS fill:#f3e5f5
    style DB fill:#e8f5e8
    style FS fill:#fff3e0
```

### Application Component Architecture

```mermaid
graph TB
    subgraph "NestJS Application"
        MAIN[Main Application<br/>Bootstrap & Config]
        
        subgraph "Core Modules"
            AUTH[Auth Module<br/>JWT Authentication]
            BENEFITS[Benefits Module<br/>ONDC Protocol]
            APPS[Applications Module<br/>Lifecycle Management]
            FILES[Application Files<br/>Document Management]
            ADMIN[Strapi Admin<br/>User Management]
        end
        
        subgraph "Common Services"
            PRISMA[Prisma Service<br/>Database ORM]
            STORAGE[Storage Service<br/>File Management]
            MIDDLEWARE[Auth Middleware<br/>Request Validation]
            FILTERS[Exception Filters<br/>Error Handling]
        end
        
        subgraph "Background Jobs"
            CALC[Benefit Calculator<br/>Cron Job]
            ELIG_JOB[Eligibility Checker<br/>Cron Job]
        end
    end
    
    MAIN --> AUTH
    MAIN --> BENEFITS
    MAIN --> APPS
    MAIN --> FILES
    MAIN --> ADMIN
    
    AUTH --> PRISMA
    BENEFITS --> PRISMA
    APPS --> PRISMA
    FILES --> STORAGE
    ADMIN --> PRISMA
    
    APPS --> CALC
    APPS --> ELIG_JOB
    
    style MAIN fill:#e3f2fd
    style PRISMA fill:#e8f5e8
    style STORAGE fill:#fff3e0
    style CALC fill:#fce4ec
    style ELIG_JOB fill:#fce4ec
```

## Data Flow Diagrams

### Benefit Discovery Flow

```mermaid
sequenceDiagram
    participant BAP as Consumer App (BAP)
    participant ONDC as ONDC Network
    participant BPP as Provider Middleware
    participant CMS as Strapi CMS
    
    BAP->>ONDC: Search benefits request
    ONDC->>BPP: Forward search request
    BPP->>CMS: Query benefits catalog
    CMS-->>BPP: Return benefits data
    BPP->>BPP: Filter & format response
    BPP-->>ONDC: On_search response
    ONDC-->>BAP: Benefits catalog
    
    Note over BAP,CMS: ONDC Protocol Implementation
```

### Application Submission Flow

```mermaid
sequenceDiagram
    participant BAP as Consumer App
    participant BPP as Provider MW
    participant DB as Database
    participant FS as File Storage
    participant VERIF as Verification Service
    
    BAP->>BPP: Submit application (init)
    BPP->>DB: Store application data
    BPP-->>BAP: Application ID & status
    
    BAP->>BPP: Upload documents
    BPP->>FS: Store files (local/S3)
    BPP->>DB: Store file metadata
    BPP->>VERIF: Submit for verification
    VERIF-->>BPP: Verification results
    BPP->>DB: Update verification status
    BPP-->>BAP: File upload confirmation
    
    Note over BPP,VERIF: Async document processing
```

### Background Processing Flow

```mermaid
graph LR
    subgraph "Scheduled Jobs"
        CRON1[Eligibility Cron<br/>Every 30 minutes]
        CRON2[Benefit Calc Cron<br/>Every 30 minutes]
    end
    
    subgraph "Processing Pipeline"
        QUERY[Query pending<br/>applications]
        BATCH[Process in batches<br/>configurable size]
        API[Call external APIs<br/>eligibility/calculation]
        UPDATE[Update database<br/>with results]
    end
    
    subgraph "External APIs"
        ELIG_API[Eligibility Service]
        CALC_API[Calculation Service]
    end
    
    CRON1 --> QUERY
    CRON2 --> QUERY
    QUERY --> BATCH
    BATCH --> API
    API --> ELIG_API
    API --> CALC_API
    API --> UPDATE
    
    style CRON1 fill:#fce4ec
    style CRON2 fill:#fce4ec
    style API fill:#e8f5e8
```

## Database Schema Diagram

```mermaid
erDiagram
    Applications {
        int id PK
        string benefitId
        string status
        string applicationData
        datetime createdAt
        datetime updatedAt
        string bapId
        string customerId
        string finalAmount
        string orderId
        string remark
        json actionLog
        int updatedBy
        datetime calculationsProcessedAt
        json calculatedAmount
        string documentVerificationStatus
        datetime eligibilityCheckedAt
        json eligibilityResult
        string eligibilityStatus
    }
    
    ApplicationFiles {
        int id PK
        string storage
        string filePath
        json verificationStatus
        int applicationId FK
        datetime createdAt
        datetime updatedAt
        string issuerName
        json documentSubmissionReason
        string documentSubtype
        string documentType
    }
    
    Users {
        int id PK
        string s_id
        string first_name
        string middle_name
        string last_name
        string email
        string phone
        boolean enabled
        boolean blocked
        datetime createdAt
        datetime updatedAt
        string[] roles
        string[] s_roles
    }
    
    Provider {
        int id PK
        string catalogManagerId
        string catalogManagerDocumentId
        string name
        string catalogManagerCode
        string[] catalogManagerRole
        string description
        datetime createdAt
        datetime updatedAt
        datetime publishedAt
        string locale
    }
    
    Applications ||--o{ ApplicationFiles : "has files"
```

## Security Architecture

```mermaid
graph TB
    subgraph "Security Layers"
        subgraph "Network Security"
            HTTPS[HTTPS/TLS<br/>Encrypted Transport]
            CORS[CORS Policy<br/>Cross-Origin Control]
        end
        
        subgraph "Application Security"
            AUTH[JWT Authentication<br/>Token Validation]
            AUTHZ[Authorization<br/>Role-Based Access]
            VALID[Input Validation<br/>Request Sanitization]
        end
        
        subgraph "Data Security"
            ENCRYPT[Data Encryption<br/>Sensitive Fields]
            KEYS[Key Management<br/>Rotation Support]
            AUDIT[Audit Logging<br/>Action Tracking]
        end
        
        subgraph "Infrastructure Security"
            ENV[Environment Variables<br/>Secret Management]
            FILES[File Upload Security<br/>Validation & Scanning]
            MONITOR[Security Monitoring<br/>Error Tracking]
        end
    end
    
    style AUTH fill:#e8f5e8
    style ENCRYPT fill:#fff3e0
    style MONITOR fill:#fce4ec
```

## Deployment Architecture

### Local Development

```mermaid
graph TB
    subgraph "Developer Machine"
        DEV[NestJS App<br/>npm run start:dev]
        PGLOCAL[(PostgreSQL<br/>Local/Docker)]
        UPLOADS[Local Uploads<br/>./uploads/]
    end
    
    subgraph "External Services"
        STRAPI_DEV[Strapi CMS<br/>localhost:1337]
        VERIF_DEV[Verification Service<br/>Mock/Local]
    end
    
    DEV --> PGLOCAL
    DEV --> UPLOADS
    DEV <--> STRAPI_DEV
    DEV <--> VERIF_DEV
    
    style DEV fill:#e1f5fe
    style PGLOCAL fill:#e8f5e8
```

### Production Deployment

```mermaid
graph TB
    subgraph "Load Balancer"
        LB[Load Balancer<br/>HTTPS Termination]
    end
    
    subgraph "Application Tier"
        APP1[App Instance 1<br/>Container/PM2]
        APP2[App Instance 2<br/>Container/PM2]
        APP3[App Instance N<br/>Container/PM2]
    end
    
    subgraph "Data Tier"
        DB[(PostgreSQL<br/>Managed Database)]
        S3[AWS S3<br/>File Storage]
        CACHE[(Redis<br/>Optional Cache)]
    end
    
    subgraph "External Services"
        STRAPI_PROD[Strapi CMS<br/>Production]
        VERIF_PROD[Verification APIs<br/>Production]
        SENTRY_PROD[Sentry<br/>Monitoring]
    end
    
    LB --> APP1
    LB --> APP2  
    LB --> APP3
    
    APP1 --> DB
    APP1 --> S3
    APP1 --> CACHE
    APP2 --> DB
    APP2 --> S3
    APP2 --> CACHE
    APP3 --> DB
    APP3 --> S3
    APP3 --> CACHE
    
    APP1 <--> STRAPI_PROD
    APP1 <--> VERIF_PROD
    APP1 --> SENTRY_PROD
    APP2 <--> STRAPI_PROD
    APP2 <--> VERIF_PROD
    APP2 --> SENTRY_PROD
    APP3 <--> STRAPI_PROD
    APP3 <--> VERIF_PROD
    APP3 --> SENTRY_PROD
    
    style LB fill:#e3f2fd
    style DB fill:#e8f5e8
    style S3 fill:#fff3e0
    style CACHE fill:#fce4ec
```

## Integration Patterns

### ONDC Protocol Flow

```mermaid
graph LR
    subgraph "ONDC Message Flow"
        SEARCH[search<br/>Discovery]
        ON_SEARCH[on_search<br/>Catalog Response]
        INIT[init<br/>Application Start]
        ON_INIT[on_init<br/>Application Created]
        CONFIRM[confirm<br/>Final Submission]
        ON_CONFIRM[on_confirm<br/>Confirmation]
        STATUS[status<br/>Status Inquiry]
        ON_STATUS[on_status<br/>Status Response]
    end
    
    SEARCH --> ON_SEARCH
    ON_SEARCH --> INIT
    INIT --> ON_INIT
    ON_INIT --> CONFIRM
    CONFIRM --> ON_CONFIRM
    ON_CONFIRM --> STATUS
    STATUS --> ON_STATUS
    
    style SEARCH fill:#e8f5e8
    style INIT fill:#e8f5e8
    style CONFIRM fill:#e8f5e8
    style STATUS fill:#e8f5e8
    style ON_SEARCH fill:#fff3e0
    style ON_INIT fill:#fff3e0
    style ON_CONFIRM fill:#fff3e0
    style ON_STATUS fill:#fff3e0
```

These diagrams provide a comprehensive view of the system architecture, data flows, and deployment patterns. Use them to understand component relationships and system behavior.





