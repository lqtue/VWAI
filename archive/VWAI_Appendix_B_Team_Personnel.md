# APPENDIX B: Team Structure & Personnel

To fulfill the responsibilities outlined in the Letter of Invitation—specifically applying AI/LLM tools to large-scale archival processing—we propose a **three-person technical team**. This structure provides the necessary mix of domain, systems, and research expertise.

## 1. Team Composition

| Role | Person | Key Expertise | Status |
| :--- | :--- | :--- | :--- |
| **Project Lead** | **Lê Quang Tuệ** | Archival Research, Geospatial Analysis, AI Strategy | ✅ Approved |
| **systems Engineer** | **Nguyễn Hoàng Vũ** | Backend Engineering, ML Deployment, Optimization | ⏳ Proposal |
| **System Architect** | **Đỗ Minh Phùng** | Map Operations, Research Rigor, Data QA | ⏳ Proposal |

---

## 2. Personnel Profiles

### 1. Lê Quang Tuệ (Project Lead)
**Role:** Document, Image & Geospatial Mapping Technician – AI/LLM Specialist

**Responsibilities:**
*   Overall project leadership and coordination.
*   Document classification and triage strategy.
*   Geospatial pipeline development (MGRS → WGS84).
*   Volunteer network coordination (HCMC, Hanoi, FUV partnerships).
*   AI strategy and prompt engineering.

**Credentials:**
*   Domain expert in Vietnam War archival documents.
*   Specialist in GIS and cartography.
*   Native Vietnamese linguistic expertise.

### 2. Nguyễn Hoàng Vũ (Senior Software Engineer)
**Role:** Backend & ML Infrastructure Specialist

**Responsibilities:**
*   **ML Model Integration:** Containerizing TrOCR/KQNBSEI models for local inference.
*   **Backend Infrastructure:** Setting up Label Studio, PostgreSQL, and Redis caching.
*   **Performance Optimization:** Batch processing and memory tuning for Mac Mini M4 Pro.
*   **DevOps:** Docker orchestration, backup automation, and VPN security.

**Relevant Experience (Ahamove):**
*   **5+ years** in backend systems for Vietnam's leading logistics platform.
*   **70% cost reduction** achieved by migrating from Google Maps API to self-hosted OSRM/Valhalla.
*   **Production ML:** Deployed route optimization engines serving millions of requests.
*   **Stack:** Node.js, Python (FastAPI), Docker, AWS, PostGIS.

### 3. Đỗ Minh Phùng (System Architect & Research Liaison)
**Role:** Research & Operations Lead

**Responsibilities:**
*   **System Architecture:** Designing the end-to-end 5-phase pipeline.
*   **Academic Research:** Literature review on OCR methods and ensuring scholarly rigor.
*   **Quality Assurance:** Designing the cross-validation logic and accuracy metrics.
*   **Volunteer Program:** Creating training materials and gamification mechanics.
*   **Conference Prep:** Structuring the June 2026 presentation and paper.

**Relevant Experience (Be Group / Grab / VinUni):**
*   **Map Operations Lead** at Be Group (#2 ride-hailing app): Managed **10M+ POIs** and **35M street-level images**.
*   **Research Assistant (VinUniversity):** Digital Twin simulation and agent-based modeling.
*   **OpenStreetMap Admin:** Leader in the Vietnam OSM community.
*   **Operations:** Led 11-person data team at Grab, standardized 5M POIs.

---

## 3. Team Synergy

The proposed team covers the three critical pillars required for a successful AI/Data project:

1.  **Domain (Tuệ):** "What do we need to extract?" (Historical context, document types).
2.  **Systems (Vũ):** "How do we build it?" (Infrastructure, code, stability).
3.  **Operations/Research (Phùng):** "How do we scale and validate it?" (Process, QA, methods).

**Why 3 People?**
Processing **261,000 active war files** is not a solitary task. It requires industrial-grade data operations.
*   **Comparison:** The KQNBSEI project processed **266 pages**. We are tackling **2.7 million pages**.
*   **Benchmark:** Commercial teams for this scale typically employ 3-5 engineers + multiple data analysts. Our 3-person lean team leverages open-source tools and volunteer labor to achieve similar results at a fraction of the cost.
