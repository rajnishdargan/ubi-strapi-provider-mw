# Architecture Diagrams

This document provides a comprehensive overview of the Provider system architecture through visual diagrams. These diagrams illustrate the system components, their interactions, and key processes.

## High-Level System Architecture

The following diagram shows the main components of the Provider system and their relationships:

```mermaid
graph TB
    subgraph "Frontend Layer"
        UI[Provider UI<br/>benefits-provider-app-ui]
        style UI fill:#e1f5fe,stroke:#01579b
    end

    subgraph "Middleware Layer"
        MW[Provider MW<br/>ubi-strapi-provider-mw]
        style MW fill:#fff8e1,stroke:#ffa000
    end

    subgraph "CMS Layer"
        CMS[Provider Strapi<br/>ubi-strapi-provider]
        style CMS fill:#f3e5f5,stroke:#7b1fa2
    end

    subgraph "ONDC Network"
        ONDC[ONDC Protocol Layer]
        style ONDC fill:#e8f5e9,stroke:#2e7d32
    end

    UI --> MW
    MW <--> ONDC
    MW <--> CMS
```

### Component Description

- **Provider UI (benefits-provider-app-ui)**: Frontend application that provides the user interface for providers to manage benefits and applications
- **Provider MW (ubi-strapi-provider-mw)**: Middleware service that handles business logic, data processing, and integration with ONDC
- **Provider Strapi (ubi-strapi-provider)**: CMS system that manages benefit configurations and form schemas
- **ONDC Network**: Protocol layer that enables discovery and transactions across the network

## Process Flows

The sequence diagram below illustrates the key processes in the system:

```mermaid
sequenceDiagram
    participant U as Provider User
    participant UI as Provider UI
    participant MW as Provider MW
    participant CMS as Provider Strapi
    participant ONDC as ONDC Network

    %% Benefits List Flow
    U->>UI: View Benefits List
    UI->>MW: GET /benefits
    MW->>CMS: Query benefits catalog
    CMS-->>MW: Benefits data
    MW-->>UI: Formatted benefits list
    UI-->>U: Display benefits

    %% Applications List Flow
    U->>UI: View Applications
    UI->>MW: GET /applications
    MW->>MW: Fetch applications list
    MW-->>UI: Applications data
    UI-->>U: Display applications

    %% Application Details Flow
    U->>UI: View Application Details
    UI->>MW: GET /applications/{id}
    MW->>MW: Fetch application data
    MW->>CMS: Get benefit details
    CMS-->>MW: Benefit data
    MW-->>UI: Application + benefit data
    UI-->>U: Display details

    %% Document Verification
    U->>UI: Upload Documents
    UI->>MW: POST /applications/{id}/documents
    MW->>MW: Verify documents
    MW-->>UI: Verification status
    UI-->>U: Show verification result

    %% Application Form
    U->>UI: Request Application Form
    UI->>MW: GET /benefits/{id}/form
    MW->>CMS: Get form schema
    CMS-->>MW: Form configuration
    MW-->>UI: Form data
    UI-->>U: Display form
```

### Process Description

1. **Benefits List Flow**: Shows how providers can view available benefits
2. **Applications List Flow**: Demonstrates the process of viewing submitted applications
3. **Application Details Flow**: Details how providers can view specific application information
4. **Document Verification**: Shows the document upload and verification process
5. **Application Form**: Illustrates how application forms are fetched and displayed

## Data Flow Architecture

The following diagram shows how data flows between different components of the system:

```mermaid
graph LR
    subgraph "User Interface"
        BL[Benefits List]
        AL[Applications List]
        AD[Application Details]
        AF[Application Form]
        style BL fill:#e3f2fd,stroke:#1565c0
        style AL fill:#e3f2fd,stroke:#1565c0
        style AD fill:#e3f2fd,stroke:#1565c0
        style AF fill:#e3f2fd,stroke:#1565c0
    end

    subgraph "Provider MW"
        BH[Benefits Handler]
        AH[Applications Handler]
        VH[Verification Handler]
        FH[Forms Handler]
        style BH fill:#fff8e1,stroke:#ffa000
        style AH fill:#fff8e1,stroke:#ffa000
        style VH fill:#fff8e1,stroke:#ffa000
        style FH fill:#fff8e1,stroke:#ffa000
    end

    subgraph "Data Sources"
        CMS[Strapi CMS]
        DB[(Applications DB)]
        ONDC[ONDC Network]
        style CMS fill:#f3e5f5,stroke:#7b1fa2
        style DB fill:#e8f5e9,stroke:#2e7d32
        style ONDC fill:#e8f5e9,stroke:#2e7d32
    end

    BL --> BH
    AL --> AH
    AD --> AH
    AF --> FH

    BH --> CMS
    AH --> DB
    VH --> DB
    FH --> CMS

    BH <--> ONDC
    AH <--> ONDC
```

### Data Flow Description

1. **User Interface Components**:
   - Benefits List: Displays available benefits
   - Applications List: Shows list of applications
   - Application Details: Presents detailed application information
   - Application Form: Handles form display and submission

2. **Middleware Handlers**:
   - Benefits Handler: Manages benefit-related operations
   - Applications Handler: Processes application operations
   - Verification Handler: Handles document verification
   - Forms Handler: Manages form operations

3. **Data Sources**:
   - Strapi CMS: Stores benefit and form configurations
   - Applications DB: Stores application data
   - ONDC Network: Enables network-wide operations

These diagrams provide a clear visualization of the system's architecture and its operations. They serve as a reference for understanding component interactions and data flows within the Provider system.