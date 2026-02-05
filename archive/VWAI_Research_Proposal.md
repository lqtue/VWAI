# RESEARCH PROPOSAL: AI-Assisted Document Processing Initiative

**TO:** Dr. Alex-Thái Đình Võ  
**FROM:** Lê Quang Tuệ  
**DATE:** January 29, 2026  
**SUBJECT:** VWAI Q1 Pilot Proposal & Technical Team Structure  

---

## 1. Specific Aims

The **Vietnam Wartime Accounting Initiative (VWAI)** faces a critical bottleneck: processing 261,000 archival files (2.7 million pages) to locate Missing in Action (MIA) remains. Current manual methods are mathematically insufficient to meet this humanitarian mission within a reasonable timeframe.

This proposal aims to deploy an **AI-Assisted Human-in-the-Loop (HITL) Pipeline** to accelerate data extraction and analysis.

*   **Aim 1 (Q1 Pilot):** Establish a 3-person technical team and a volunteer network to process **1,600–2,400 documents** by March 31, 2026, validating the system's efficiency and accuracy.
*   **Aim 2 (Technology):** Deploy a self-improving AI framework (TrOCR + KQNBSEI) where human corrections fine-tune the model, targeting a **40% → 75% accuracy** improvement loop.
*   **Aim 3 (Sustainability):** Demonstrate a cost-effective model (**<$5/document**) that reduces manual labor by **67%**, justifying long-term scaling.

## 2. Distinction & Significance

**The Challenge:**
At the current manual rate of 45 minutes per file, the CDEC corpus requires **~94 person-years** to process. With existing staffing, this would take nearly two decades. The sheer volume of handwritten, degraded microfilm renders standard commercial OCR solutions ineffective (<40% accuracy) or prohibitively expensive.

**The Solution:**
We propose moving from *Digitization* (static images) to *Digital Transformation* (queryable intelligence). By combining open-source AI with a motivated volunteer workforce (HITL), we can create a "Centaur" system—where AI handles the rote transcription draft, and humans focus on verification and high-value analysis.

**Significance:**
This project will not only accelerate the VWAI mission but also establish a **methodological precedent** for AI in digital humanities. It transforms a static repository into a specialized **Battle Dataset**, enabling geospatial queries ("Show all events near X coordinates") that directly guide field recovery teams.

## 3. Innovation

1.  **Human-in-the-Loop (HITL) Learning:** Unlike static OCR, our system learns. Every volunteer correction becomes training data, making the AI smarter weekly.
2.  **Gamified Volunteer Model:** We leverage student volunteers not just as labor, but as partners, using gamification (badges, leaderboards) to maintain high engagement and retention.
3.  **Local "Code-to-Data" Inference:** By using Mac Mini M4 Pro servers, we perform high-performance inference locally. This ensures **zero data leakage** (privacy compliance) and **zero recurring cloud API costs** (fiscal responsibility).

## 4. Approach

We will execute the project in three phases (See **Appendix C** for detailed Roadmap):

*   **Phase 1: Infrastructure & Triage (Weeks 1-3):** Set up the PostGIS database, Label Studio interface, and specialized AI models (TrOCR for handwritten, KQNBSEI for typed text).
*   **Phase 2: Pilot & Training (Weeks 3-4):** Recruit and train 10-15 Vietnamese student volunteers. Deploy the "Split-Screen" validation interface.
*   **Phase 3: Production & Iteration (Weeks 5-12):** Process 200-300 documents/week. Weekly "Retraining Cycles" will update the AI model with fresh human corrections to boost accuracy.

*See **Appendix A** for Technical System Architecture.*

## 5. Investigator & Environment

To execute this sophisticated pipeline within the **one-year term**, we propose a lean **3-person technical team** (See **Appendix B** for Personnel details):

1.  **Lê Quang Tuệ (Project Lead):** Domain expertise in Archives & GIS.
2.  **Nguyễn Hoàng Vũ (Systems Engineer):** 5+ years backend/ML experience (Ahamove). Critical for infrastructure and deployment.
3.  **Đỗ Minh Phùng (Architect/Research):** Ops lead for 10M+ spatial points (Be Group). Critical for QA rigor and scalability.

**Environment:**
Processing will occur on **Texas Tech University** premises using dedicated local hardware (2x Mac Mini M4 Pro), ensuring strict compliance with the **Code-to-Data** security model.

## 6. Budget Justification

**Total Year 1 Request: $50,000** (See **Appendix D** for breakdown).

*   **Personnel ($45,000):** Three specialists at $15,000/year each. This is 1/4th the cost of commercial equivalents, feasible due to the team's humanitarian commitment.
*   **Hardware ($5,000):** One-time purchase of 2x Mac Mini M4 Pro units. This capital expense eliminates ~$275,000 in potential commercial API fees over 3 years.

**Return on Investment:**
The proposal delivers a **93% cost reduction** vs. manual processing ($4.63 vs $64.00 per doc) and reduces the timeline from 20 years to ~4-5 years.

---
**List of Appendices:**
*   **Appendix A:** Technical System Architecture
*   **Appendix B:** Team Structure & Personnel
*   **Appendix C:** Implementation Roadmap & Risks
*   **Appendix D:** Budget & ROI Analysis
*   **Appendix E:** Compliance, Security & Data Management
*   **Appendix F:** Literature Review & Methodological Justification
