# Enterprise Hospital Management System (HMS) - Architecture Blueprint

This blueprint outlines the comprehensive data flow, external integrations, and role-based architecture for a scalable, enterprise-grade Hospital Management System (HMS).

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
    WhatsApp[WhatsApp Cloud API: Auto-Scheduling & Alerts]:::external
    Razorpay[Razorpay: Fee Processing]:::external
    Gemini[Google Gemini AI: Voice-to-Text & Note Generation]:::ai

    %% Frontend Layer - Angular (Modularized)
    subgraph FrontendLayer [Frontend - Angular SPA]
        A[Patient Portal / Web Booking]:::frontend
        B[Front Desk: Registration & Triage]:::frontend
        C[Nursing Desk: Vitals]:::frontend
        D[Doctor Desk: EMR & Prescriptions]:::frontend
        E[Laboratory: Custom Diagnostic Templates]:::frontend
        F[Pharmacy & Inventory: Custom Medicines]:::frontend
        G[Billing & Checkout]:::frontend
        H[Admin: HRMS, Payroll & Leave Mgmt]:::frontend
    end

    %% User Journey & External Triggers
    WhatsApp <-->|Webhooks| A
    A --> B
    B --> C
    C --> D
    D --> E
    D --> F
    E --> G
    F --> G
    G <-->|Payment Token| Razorpay
    D <-->|Voice Dictation| Gemini

    %% Backend API Layer - Node.js & Express
    subgraph BackendLayer [Backend API - Node.js & Express]
        API_GW(API Gateway: JWT Auth & Dynamic RBAC):::backend
        Svc_Appt(Appointment & Comm Service):::backend
        Svc_Clin(Clinical EMR Service):::backend
        Svc_LabPharm(Lab & Pharmacy Service):::backend
        Svc_Admin(Billing & HRMS Service):::backend
    end

    %% Routing Frontend Requests to Backend Gateway
    A & B & C & D & E & F & G & H == HTTPS JSON Requests ==> API_GW

    %% Internal Microservices Routing
    API_GW --> Svc_Appt
    API_GW --> Svc_Clin
    API_GW --> Svc_LabPharm
    API_GW --> Svc_Admin

    %% Database Layer - MySQL
    subgraph DatabaseLayer [Database - Scalable MySQL]
        DB_Pat[(Patients & Appointments)]:::database
        DB_Clin[(Vitals, Notes, Prescriptions)]:::database
        DB_Lab[(Lab Results & Inventory Logs)]:::database
        DB_Fin[(Invoices & Employee Data)]:::database
    end

    %% Database Connections (Stored Procedures / Joins)
    Svc_Appt ==> DB_Pat
    Svc_Clin ==> DB_Clin
    Svc_LabPharm ==> DB_Lab
    Svc_Admin ==> DB_Fin
