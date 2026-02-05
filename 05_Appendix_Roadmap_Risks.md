# APPENDIX C: Implementation Roadmap & Risk Assessment

## 1. Phase 1: Q1 Pilot (January–March 2026)

**Objective:** Validate HITL system, establish volunteer network, generate conference-worthy results.

### Weekly Schedule

| Week | Phase | Key Activities | Deliverable |
| :--- | :--- | :--- | :--- |
| **1-2** | **Infrastructure** | • Procure Mac Minis<br>• Install Docker, PostGIS, Label Studio<br>• Setup VPN | Functional Development Environment |
| **2-3** | **Model Integration** | • Containerize TrOCR & KQNBSEI<br>• Test inference on Mac Mini (Target <3s/page)<br>• Research preprocessing pipeline | Working AI Pipeline (Draft Output) |
| **3-4** | **Recruitment** | • Recruit 10-15 volunteers (HCMC, Hanoi, FUV)<br>• Conduct orientation & training<br>• Issue VPN credentials | Trained Volunteer Team |
| **4** | **Soft Launch** | • Process initial 50-100 docs<br>• Monitor routing logic & feedback | Validated Production Workflow |
| **5-10** | **Production** | • **Process 200-300 docs/week**<br>• Weekly AI retraining loop (Mon: Export, Wed: Train, Thu: Deploy)<br>• Gamification competitions | **1,200-1,800 Documents Processed** |
| **11-12** | **Analysis** | • Compile Q1 metrics (Accuracy, Speed)<br>• Draft Conference Presentation<br>• Prepare Q2 Proposal | Q1 Report & Conference Draft |

### Phase 1B: Decision Point (April 2026)
Based on Q1 metrics, we will recommend a scaling path:
*   **Option A (Local Cluster):** Expand to 20-30 volunteers using existing/additional Mac Minis. Best for cost-efficiency.
*   **Option B (TTU HPCC):** Migrate to Data Center if demand explodes (>30 volunteers). Best for massive scale.
*   **Option C (Hybrid API):** Use Commercial APIs if local AI accuracy plateaus. Best if handwritten text proves too difficult.

---

## 2. Risk Assessment

### Technical Risks

| Risk | Prob. | Impact | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **AI Accuracy Plateaus (<70%)** | Med | High | • Expand training data (continue HITL).<br>• Advanced preprocessing (denoising).<br>• Fallback: Test Commercial APIs (Google/Azure). |
| **Microfilm Illegible (Worst 10%)** | Med | High | • Flag as "Manual-Only".<br>• Apply deep-learning super-resolution (Real-ESRGAN).<br>• Request archival rescans. |
| **Hardware Bottleneck** | Low | Med | • Load balance across 2 Mac Minis.<br>• Offload inference to cloud GPU (RunPod) temporarily.<br>• Migrate to TTU HPCC. |

### Operational Risks

| Risk | Prob. | Impact | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **Volunteer Drop-off** | Med | High | • **Gamification:** Leaderboards, badges, small prizes.<br>• Reduce shift length (2hr min).<br>• Academic credit partnerships. |
| **Low Quality/Volume Data** | Med | High | • **Synthetic Mitigation:** Generate "Synthetic Corrupted Text" (per *ArXiv 2409*) to train models if human data volume is low.<br>• **Cross-Validation:** 2-person checking for all newbies. |

---

## 2B. Phase 2: Knowledge Graph & Predictive Modeling (Q2-Q3 2026)

**Objective:** Extend validated HITL pipeline with Search & Rescue methodology integration.

### Q2 2026 (April–June): Knowledge Graph Construction

| Week | Phase | Key Activities | Deliverable |
| :--- | :--- | :--- | :--- |
| **13-15** | **KG Schema** | • Implement CIDOC CRM ontology in PostGIS<br>• Define Person↔Event↔Place relationships<br>• Create entity extraction pipeline | Knowledge Graph Schema |
| **16-18** | **Data Linking** | • Link CDEC documents to KG entities<br>• Integrate US Unit Logs (Blue Force)<br>• Build contradiction detection system | Populated Knowledge Graph |
| **19-21** | **H3 Indexing** | • Implement H3 hexagonal grid (Resolution 9)<br>• Calculate initial POC scores from KG evidence<br>• Create heatmap visualization layer | H3 POC Grid (Province-level) |
| **22-24** | **Integration** | • Connect KG to field operations workflow<br>• Develop priority ranking algorithm<br>• June Conference presentation | Integrated System Demo |

### Q3 2026 (July–September): Taphonomic & Field Operations

| Week | Phase | Key Activities | Deliverable |
| :--- | :--- | :--- | :--- |
| **25-28** | **Taphonomic Module** | • Integrate SRTM DEM data<br>• Implement slope/erosion modeling<br>• Add soil preservation factors | Taphonomic Analysis Layer |
| **29-32** | **Field App** | • Develop AR tablet prototype<br>• Implement historical map overlay<br>• Build offline caching system | Field Tablet MVP |
| **33-36** | **Bayesian Feedback** | • Implement feedback loop algorithm<br>• Create field data collection forms<br>• Test with pilot field teams | Complete Feedback System |

### Phase 2 Decision Points

| Milestone | Success Criteria | Contingency |
|-----------|------------------|-------------|
| **KG Population** | >5,000 linked entities | Prioritize high-value CDEC files |
| **POC Accuracy** | Field validation >60% hit rate | Refine evidence weighting |
| **Tablet Usability** | Field team NPS >7/10 | Simplify UI, add training |

---

## 3. Success Metrics (Q1 Targets)

**Quantitative:**
*   **Throughput:** 1,600–2,400 documents processed.
*   **Time Savings:** ≥67% reduction vs. manual baseline (from 45m to 15m/doc).
*   **AI Improvement:** 40% initial accuracy → 70-75% post-training.
*   **Cost Efficiency:** <$5.00 per document.

**Qualitative:**
*   Sustainable volunteer network established.
*   Conference-ready methodology proven.
*   Humanitarian mission advanced (families closer to answers).

---

## 4. Success Metrics (Phase 2 Targets - Q2-Q3 2026)

**Quantitative (Knowledge Graph & Predictive Modeling):**
*   **KG Entities:** >10,000 linked Person↔Event↔Place relationships.
*   **H3 Coverage:** Provincial-level grid with POC scores for priority search areas.
*   **POC Validation:** >60% accuracy on field-validated search cells.
*   **Taphonomic Integration:** DEM analysis for top 50 high-priority cells.

**Quantitative (Field Operations):**
*   **Tablet Deployment:** 5+ field tablets with AR overlay capability.
*   **Field Tests:** 10+ excavation sites with Bayesian feedback captured.
*   **Feedback Loop:** Demonstrated probability redistribution from field results.

**Qualitative:**
*   Event-based search model validated (searching for "events" not just "locations").
*   Cross-reference capability demonstrated (CDEC ↔ US Unit Logs ↔ Terrain).
*   Field teams report improved efficiency using prioritized search zones.
*   Methodology publishable in Digital Humanities / Forensic Archaeology journals.

---

## 5. Risk Assessment (Phase 2 Additions)

### Knowledge Graph Risks

| Risk | Prob. | Impact | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **Entity Extraction Low Recall** | Med | High | • Manual entity tagging for training data.<br>• Use Claude for NER on complex passages.<br>• Iterative refinement with domain experts. |
| **KG Contradictions Overwhelming** | Low | Med | • Implement priority queue for expert review.<br>• Develop confidence-weighted resolution rules.<br>• Accept ambiguity for low-confidence links. |

### Predictive Modeling Risks

| Risk | Prob. | Impact | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **POC Scores Not Predictive** | Med | High | • Validate with known burial sites (ground truth).<br>• Adjust evidence weights empirically.<br>• Combine with expert intuition as hybrid. |
| **Taphonomic Models Inaccurate** | Med | Med | • Use conservative slope thresholds.<br>• Prioritize direct evidence over modeled displacement.<br>• Treat as "area expansion" not "relocation". |

### Field Operations Risks

| Risk | Prob. | Impact | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| **Tablet Connectivity Issues** | Med | Med | • Implement robust offline mode.<br>• Pre-cache all map tiles and evidence.<br>• Sync on return to base. |
| **Field Team Adoption Resistance** | Low | High | • Involve field teams in design process.<br>• Demonstrate clear value (less wasted effort).<br>• Provide thorough training. |
| **AR Overlay Calibration Drift** | Med | Low | • Use multiple GPS fix points.<br>• Allow manual adjustment.<br>• Warn when confidence is low. |
