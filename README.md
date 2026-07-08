# 🌍 Hospital Management System (HMS ERP) - Architecture Blueprint

This blueprint maps out the complete digital journey of a modern, large-scale hospital network. It shows how our system smoothly connects patient care, surgical theaters, global supply chains, and back-office staff into one secure, lightning-fast platform. 

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
    Ext_Comms[WhatsApp & Email: Global Patient Alerts]:::external
    Ext_Pay[Global Payment Gateways & Insurance/TPA APIs]:::external
    Gemini[AI NLP Engine: Voice-to-Text & Auto-Charting]:::ai

    %% Frontend Layer - Angular (Modularized ERP)
    subgraph FrontendLayer [Frontend Angular SPA - Global ERP Modules]
        A[Patient CRM: Telemedicine & Web Booking]:::frontend
        B[Front Office: OPD & IPD Registration]:::frontend
        C[Clinical Desk: Doctors & Nursing Station]:::frontend
        D[OT Management: Surgery & Anesthesia]:::frontend
        E[Ancillary: Laboratory & Radiology]:::frontend
        F[Supply Chain: Global Pharmacy & Inventory]:::frontend
        G[Finance: Billing & Insurance Claims]:::frontend
        H[Back-Office: HRMS, Payroll & Assets]:::frontend
    end

    %% User Journey & External Triggers
    Ext_Comms <-->|Reminders & Telemed Links| A
    A --> B
    B --> C
    C --> D
    C --> E
    C --> F
    E --> G
    F --> G
    D --> G
    G <-->|Instant Claim Processing| Ext_Pay
    C <-->|Voice Dictation & Smart Parsing| Gemini
    H -.->|Staff Roster & Asset Availability| B

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
