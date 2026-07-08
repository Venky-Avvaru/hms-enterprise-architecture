# Hospital Management System (HMS) - Data Flow Blueprint

This is a high-level architecture flow of a modern Hospital Management System (HMS) based on my experience. It shows how data moves securely from the front desk to the doctor's screen, and finally to billing.

### How It Works

1. **Frontend (Angular):** The UI is strictly role-based. Receptionists, nurses, and doctors only load the modules they need.
2. **Backend (Node.js/Express):** All requests hit an Auth & RBAC (Role-Based Access Control) gateway first. This ensures a front-desk user can never access a patient's private clinical notes.
3. **Database (MySQL):** A normalized relational database mapping patients to their vitals, doctor notes, and invoices.

### 🤖 The AI Integration (Reducing Doctor Burnout)
A major bottleneck in hospitals is the time doctors spend typing clinical notes. To solve this, the architecture includes an AI module at the Doctor Desk level.
*   **Voice-to-Text:** Doctors can dictate notes directly into the system.
*   **Gemini AI:** The AI parses the dictation, structures it into proper medical formats (Symptoms, Diagnosis, Prescriptions), and drops it directly into a custom Rich Text Editor. 
*   **Impact:** This dramatically cuts down documentation time, allowing doctors to see more patients rather than doing data entry.
