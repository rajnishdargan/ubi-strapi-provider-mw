# Architecture Diagrams

This document provides a comprehensive overview of the Provider system architecture, focusing on application processing, storage systems, and background processes.

## High-Level System Architecture

```mermaid
graph TB
    subgraph "Frontend Layer"
        UI[Provider UI<br/>benefits-provider-app-ui]
        subgraph "Key Components"
            BF[Benefit Form]
            AL[Application List]
            AD[Application Details]
        end
        style UI fill:#e1f5fe,stroke:#01579b
        style BF fill:#e3f2fd,stroke:#1565c0
        style AL fill:#e3f2fd,stroke:#1565c0
        style AD fill:#e3f2fd,stroke:#1565c0
    end

    subgraph "Middleware Layer"
        MW[Provider MW<br/>ubi-strapi-provider-mw]
        subgraph "Core Services"
            BS[Benefits Service]
            AS[Applications Service]
            VS[Verification Service]
            ES[Eligibility Service]
        end
        subgraph "Background Jobs"
            EC[Eligibility Cron<br/>*/30 * * * *]
            BC[Benefit Calc Cron<br/>*/30 * * * *]
        end
        style MW fill:#fff8e1,stroke:#ffa000
        style BS fill:#fff8e1,stroke:#ffa000
        style AS fill:#fff8e1,stroke:#ffa000
        style VS fill:#fff8e1,stroke:#ffa000
        style ES fill:#fff8e1,stroke:#ffa000
        style EC fill:#ffe0b2,stroke:#ef6c00
        style BC fill:#ffe0b2,stroke:#ef6c00
    end

    subgraph "Storage Layer"
        DB[(PostgreSQL)]
        S3[File Storage<br/>Local/S3]
        style DB fill:#e8f5e9,stroke:#2e7d32
        style S3 fill:#e8f5e9,stroke:#2e7d32
    end

    subgraph "CMS Layer"
        CMS[Provider Strapi<br/>ubi-strapi-provider]
        style CMS fill:#f3e5f5,stroke:#7b1fa2
    end

    subgraph "ONDC Network"
        ONDC[ONDC Protocol Layer]
        style ONDC fill:#e8f5e9,stroke:#2e7d32
    end

    BF --> BS
    AL --> AS
    AD --> AS

    BS --> DB
    AS --> DB
    VS --> DB
    ES --> DB

    EC --> ES
    BC --> AS

    BS <--> CMS
    BS <--> ONDC
    AS <--> ONDC
```

## Application Processing Flow

```mermaid
sequenceDiagram
    participant U as Provider User
    participant UI as Provider UI
    participant AS as Application Service
    participant ES as Eligibility Service
    participant DB as PostgreSQL
    participant CMS as Provider Strapi
    participant ONDC as ONDC Network

    %% Application Form Submission
    U->>UI: Fill Application Form
    UI->>AS: POST /applications
    Note over AS: Process Application
    AS->>DB: Create Application Record
    AS-->>UI: Return Application ID
    UI-->>U: Show Confirmation

    %% Background Processing - Eligibility
    Note over ES: Eligibility Cron Job
    activate ES
    ES->>DB: Fetch Pending Applications
    ES->>CMS: Get Benefit Rules
    ES->>ES: Check Eligibility
    ES->>DB: Update Eligibility Status
    deactivate ES

    %% Background Processing - Benefit Calculation
    Note over AS: Benefit Calculation Cron
    activate AS
    AS->>DB: Fetch Eligible Applications
    AS->>AS: Calculate Benefit Amount
    AS->>DB: Update Calculated Amount
    deactivate AS

    %% Status Updates
    AS->>ONDC: Send Status Update
    ONDC-->>AS: Confirmation
    AS->>DB: Update Final Status
    
    %% Notify User
    AS-->>UI: Push Status Update
    UI-->>U: Show Updated Status
```

## Storage System Architecture

```mermaid
graph TB
    subgraph "Storage Provider Module"
        SPM[Storage Provider Factory]
        style SPM fill:#fff8e1,stroke:#ffa000
    end

    subgraph "Storage Adapters"
        LSA[Local Storage Adapter]
        S3A[S3 Storage Adapter]
        style LSA fill:#e3f2fd,stroke:#1565c0
        style S3A fill:#e3f2fd,stroke:#1565c0
    end

    subgraph "Storage Locations"
        LD[Local Directory<br/>/uploads]
        S3B[S3 Bucket]
        style LD fill:#e8f5e9,stroke:#2e7d32
        style S3B fill:#e8f5e9,stroke:#2e7d32
    end

    SPM -->|if FILE_STORAGE_PROVIDER=local| LSA
    SPM -->|if FILE_STORAGE_PROVIDER=s3| S3A
    
    LSA --> LD
    S3A --> S3B

    %% Interface Implementation
    subgraph "Storage Interface"
        UI[uploadFile]
        GF[getFile]
        DF[deleteFile]
        style UI fill:#f3e5f5,stroke:#7b1fa2
        style GF fill:#f3e5f5,stroke:#7b1fa2
        style DF fill:#f3e5f5,stroke:#7b1fa2
    end

    LSA -.->|implements| UI
    LSA -.->|implements| GF
    LSA -.->|implements| DF
    S3A -.->|implements| UI
    S3A -.->|implements| GF
    S3A -.->|implements| DF
```

## Background Job Processes

```mermaid
graph TB
    subgraph "Cron Jobs"
        EC[Eligibility Cron<br/>*/30 * * * *]
        BC[Benefit Calc Cron<br/>*/30 * * * *]
        style EC fill:#ffe0b2,stroke:#ef6c00
        style BC fill:#ffe0b2,stroke:#ef6c00
    end

    subgraph "Processing Pipeline"
        FP[Fetch Pending<br/>Applications]
        BP[Batch Processing]
        UP[Update Status]
        style FP fill:#fff8e1,stroke:#ffa000
        style BP fill:#fff8e1,stroke:#ffa000
        style UP fill:#fff8e1,stroke:#ffa000
    end

    subgraph "Services"
        ES[Eligibility Service]
        AS[Application Service]
        style ES fill:#e3f2fd,stroke:#1565c0
        style AS fill:#e3f2fd,stroke:#1565c0
    end

    subgraph "Storage"
        DB[(PostgreSQL)]
        style DB fill:#e8f5e9,stroke:#2e7d32
    end

    EC --> FP
    BC --> FP
    FP --> BP
    BP --> ES
    BP --> AS
    ES --> UP
    AS --> UP
    UP --> DB
```

### Component Descriptions

1. **Application Processing**:
   - Form submission through UI
   - Application record creation
   - Status tracking and updates
   - ONDC protocol integration

2. **Storage System**:
   - Configurable storage provider (local/S3)
   - Unified interface for file operations
   - Metadata storage in PostgreSQL
   - Secure file handling

3. **Background Jobs**:
   - Scheduled eligibility checks (every 30 minutes)
   - Automated benefit calculations (every 30 minutes)
   - Batch processing for efficiency
   - Status updates and notifications

### Key Processes

1. **Application Submission**:
   - Form data validation
   - Application record creation
   - Initial status assignment

2. **Background Processing**:
   - Eligibility verification
   - Benefit amount calculation
   - Status updates
   - ONDC protocol compliance

3. **Storage Operations**:
   - File upload handling
   - Metadata management
   - Storage provider selection
   - Error handling and recovery