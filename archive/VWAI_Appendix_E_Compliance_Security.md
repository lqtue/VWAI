# APPENDIX E: Compliance, Security & Data Management

All activities proposed strictly adhere to the **VWAI Confidentiality, Ownership & Use Agreement** and Texas Tech University (TTU) policies.

## 1. Data Security: "Code-to-Data" Model

To ensure the integrity and privacy of the Sensitive CDEC corpus, we employ a "Code-to-Data" security model. **Data does not leave the secure environment; users come to the data.**

*   **Central Storage:** All CDEC files and databases reside on the physically secured Mac Mini servers at TTU.
*   **VPN Access:** Remote validators access the Label Studio interface via a secure VPN (TTU GlobalProtect or WireGuard).
*   **View-Only Clients:** The validation interface allows viewing images and inputting text. It **disables downloads**, printing, and clipboard copying for source images.
*   **Geofencing:** Access can be restricted to specific IP ranges (e.g., University labs) if required.

## 2. Access Control & Audit

*   **Role-Based Access Control (RBAC):**
    *   **Validators:** View assigned batch only. Edit text fields.
    *   **Reviewers:** Access "Flagged" queue. Approve/Reject data.
    *   **Admins:** System config and user management.
*   **Audit Logging:**
    *   **Application Level:** Label Studio logs every keystroke, annotation, and approval with UserID and Timestamp.
    *   **Network Level:** VPN logs track connection times and originating IPs.
    *   **Database Level:** PostgreSQL logs all transactions for forensic audit capability.

## 3. Intellectual Property (IP) & Ownership

*   **Software & Assets:** All code, models, Docker containers, and documentation produced by the technical team are the sole property of **Texas Tech University / VWAI**.
*   **Volunteer Contributions:**
    *   Volunteers sign a **Confidentiality & IP Assignment Agreement** prior to onboarding.
    *   All transcription data and corrections are "work for hire" and owned by VWAI.
    *   Volunteers receive certificates and academic credit but hold no claim to the data.

## 4. Backup & Disaster Recovery

*   **Daily:** Incremental encrypted backups to local external SSD (Air-gapped).
*   **Weekly:** Full dataset snapshots to TTU secure network storage.
*   **Monthly:** Offsite encrypted cold storage (AWS Glacier) for disaster recovery.
*   **Redundancy:** The dual-Mac Mini setup allows for immediate failover if the primary production server experiences hardware failure.
