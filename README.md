# Enterprise Hospital Management System (HMS ERP) - Architecture Blueprint

This blueprint outlines the holistic data flow, modular architecture, and complete enterprise resource planning (ERP) life cycle of a global Hospital Management System. It demonstrates how modern healthcare facilities integrate patient care, supply chain, and back-office operations into a single scalable ecosystem.

### 📊 System Architecture Flowchart

```mermaid
graph TD
    %% Define Styles
    classDef frontend fill:#0366d6,stroke:#005cc5,stroke-width:2px,color:#fff,font-weight:bold;
    classDef backend fill:#6f42c1,stroke:#5a32a3,stroke-width:2px,color:#fff,font-weight:bold;
    classDef database fill:#28a745,stroke:#22863a,stroke-width:2px,color:#fff,font-weight:bold;
    classDef external fill:#d73a49,stroke:#cb2431,stroke-width:2px,color:#fff,font-weight:bold;
    classDef ai fill:#e36209,stroke:#b04c05,stroke-width:2px,color:#fff,font-weight:bold;

    %% External APIs & AI Integrations
    Ext_Comms[SMS / Email / WhatsApp Notification Gateways]:::external
    Ext_Pay[Global Payment & Insurance/TPA API Gateways]:::external
    Gemini[AI NLP Engine: Voice-to-Text & Auto-Charting]:::ai

    %% Frontend Layer - Angular (Modularized ERP)
    subgraph FrontendLayer [Frontend Angular SPA - Global ERP Modules]
        A[Patient CRM: Telemedicine & Appointments]:::frontend
        B[Front Office: OPD & IPD Registration]:::frontend
        C[Clinical Desk: Doctors & Nursing Station]:::frontend
        D[OT Management: Surgery & Anesthesia]:::frontend
        E[Ancillary: Laboratory, Radiology & Blood Bank]:::frontend
        F[Supply Chain: Pharmacy & Central Inventory]:::frontend
        G[Finance: Billing, TPA / Insurance Claims]:::frontend
        H[Back-Office: HRMS, Payroll & Asset Mgmt]:::frontend
    end

    %% User Journey & External Triggers
    Ext_Comms <-->|Reminders| A
    A --> B
    B --> C
    C --> D
    C --> E
    C --> F
    E --> G
    F --> G
    D --> G
    G <-->|Claim Processing| Ext_Pay
    C <-->|Voice Dictation & Smart Parsing| Gemini
    H -.->|Staff Roster & Assets| B

    %% Backend API Layer - Node.js & Express
    subgraph BackendLayer [Backend API - Microservices Architecture]
        API_GW(API Gateway: JWT Auth & Dynamic RBAC):::backend
        Svc_CRM(Patient CRM & Booking Service):::backend
        Svc_EMR(Core EMR & Clinical Service):::backend
        Svc_SCM(Supply Chain & Inventory Service):::backend
        Svc_ERP(Finance, Billing & HR Service):::backend
    end

    %% Routing Frontend Requests to Backend Gateway
    A & B & C & D & E & F & G & H == HTTPS JSON Requests ==> API_GW

    %% Internal Microservices Routing
    API_GW --> Svc_CRM
    API_GW --> Svc_EMR
    API_GW --> Svc_SCM
    API_GW --> Svc_ERP

    %% Database Layer - Scalable MySQL
    subgraph DatabaseLayer [Database - Scalable Relational Storage]
        DB_Pat[(Patients & Demographics)]:::database
        DB_EMR[(EMR: Vitals, Notes, Orders)]:::database
        DB_Inv[(Inventory & Pharmacy Stocks)]:::database
        DB_ERP[(Accounts, Billing & HR)]:::database
    end

    %% Database Connections (Stored Procedures / Joins)
    Svc_CRM ==> DB_Pat
    Svc_EMR ==> DB_EMR
    Svc_SCM ==> DB_Inv
    Svc_ERP ==> DB_ERP
