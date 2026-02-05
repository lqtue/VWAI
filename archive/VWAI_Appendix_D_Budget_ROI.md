# APPENDIX D: Budget & ROI Analysis

## 1. Budget Summary

### Year 1 Budget (Q1 Pilot + Rollout)

| Category | Item | Cost | Notes |
| :--- | :--- | :--- | :--- |
| **Personnel** | **Lê Quang Tuệ**<br>(Project Lead) | $15,000 | ✅ Approved per LOI |
| | **Nguyễn Hoàng Vũ**<br>(Backend/Systems) | $15,000 | ⏳ Proposed (Essential) |
| | **Đỗ Minh Phùng**<br>(Architect/Research) | $15,000 | ⏳ Proposed (Essential) |
| **Hardware** | **Mac Mini M4 Pro** (x2)<br>(48GB RAM, 1TB SSD) | $3,998 | Production + Dev/Backup Unit |
| | Storage & Peripherals | $240 | 2TB SSD, Cables |
| **Operations** | Software & Gamification | $762 | Claude Max, Volunteer Rewards |
| **TOTAL** | | **$50,000** | |

### Years 2-3 (Sustained Production)
*   **Personnel:** $45,000 / year (Fixed)
*   **Operations:** ~$575 / year (Maintenance, Electricity)
*   **3-Year Total Cost of Ownership (TCO):** ~$138,750

---

## 2. Infrastructure Justification

**Why Mac Mini M4 Pro?**
*   **Unified Memory (48GB):** Critical for loading large Vision Models (TrOCR) and Vector Databases without VRAM bottlenecks.
*   **Neural Engine:** Optimized for CoreML/PyTorch inference, allowing local processing without cloud GPU costs.
*   **Redundancy:** Two units allow one to serve the live Label Studio application while the second performs intensive overnight model fine-tuning.

---

## 3. Return on Investment (ROI)

### The Cost of Alternatives
Estimates based on processing the full 261,000-file corpus.

**1. Manual Processing (Status Quo)**
*   Rate: 500-1,000 docs/year per person.
*   Requirement: 5 full-time staff for 15+ years.
*   Cost: **~$64 - $90 per document**.
*   *Verdict: Economically and temporally infeasible.*

**2. Commercial OCR APIs (Google/Azure)**
*   API Fees: ~$5,400 (Low).
*   Validation: Still requires massive human effort (roughly equal to VWAI proposal).
*   Privacy: Data leaves TTU control.
*   Cost: **~$7 - $9 per document** (Including personnel for validation).

**3. VWAI Proposal (AI + Volunteer HITL)**
*   Throughput: 10,000 (Year 1) → 50,000+ (Year 2+).
*   3-Year TCO: $138,750.
*   Output: ~30,000-40,000 docs (Conservative).
*   Cost: **$3.47 - $4.63 per document**.

### Conclusion
The VWAI proposal offers a **93% cost reduction** compared to manual processing and is **~50% cheaper** than commercial API routes, while retaining full data sovereignty and creating a reusable academic asset.

| Metric | VWAI Proposal | Manual Baseline |
| :--- | :--- | :--- |
| **Cost per Doc** | **$3.47 - $4.63** | $64.00 - $90.00 |
| **Time to Complete** | **4-5 Years** | 15-20 Years |
| **Scalability** | **High** (Add volunteers) | **Low** (Add salaries) |
