# VWAI Project Control Document

> **Last Updated:** 2026-02-05
> **Status:** Pilot Planning (Q2-Q3 2026)
> **Funding Request:** $60,000 (3 team members + variable costs)

---

## 1. Executive Summary (For PIs)

**Problem:** 300,000 Vietnamese soldiers remain missing. TTU holds 261,000 CDEC files (2.7M pages) — at current pace, processing takes 15-20 years.

**Solution:** Automation system of: (i) AI-assisted transcription pipeline to process documents 8-12× faster and at rougly the same accuracy as humans, (ii) knowledge graph construction and (iii) predictive burial site mapping.

**Timeline:**
- Q1 2026 (Feb-Mar): Setup & baseline testing (limited scope due to Tet holiday)
- Q2 2026 (Apr-Jun): Core pilot — validate feasibility, target 1,000+ documents
- Jun 2026: Conference demo at TTU Vietnam Center

**Ask:** Approve $60,000 for tech team (3 members) + variable costs (cloud/hardware/consulting), plus collaboration access to TTU AI/CV/ML faculty expertise.

---

## 2. How the System Works

### The Core Idea

Instead of manually reading 2.7 million pages, we use AI to do the first pass and humans to verify/correct. This is called **Human-in-the-Loop (HITL)** — AI handles volume, humans ensure quality.

### The Pipeline (4 Layers)

```
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 1: DIGITIZE                                              │
│  Scanned documents → AI reads text (OCR) → Humans verify        │
│                                                                 │
│  Input:  Microfilm scan of 1968 Vietnamese field report         │
│  Output: Searchable text with verified accuracy                 │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 2: CONNECT                                               │
│  Extract entities → Link relationships → Build knowledge graph  │
│                                                                 │
│  "Nguyễn Văn A, Unit 324, killed 15 Mar 1968 near Hill 881"     │
│       ↓              ↓              ↓              ↓            │
│    Person ──────── Unit ──────── Event ──────── Place           │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 3: PREDICT                                               │
│  Combine evidence → Calculate probability → Generate heatmap    │
│                                                                 │
│  Multiple documents mention burials near Hill 881               │
│  → High probability zone marked on map                          │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 4: SEARCH                                                │
│  Prioritized zones → Field teams excavate → Results feed back   │
│                                                                 │
│  "Start digging here (85% confidence), not there (20%)"         │
│  Found/not found → Update probabilities → Refine predictions    │
└─────────────────────────────────────────────────────────────────┘
```

### Challenges by Track

**Track A: Geospatial** (Lead: Tuệ)
| Challenge | Why It's Difficult | Approach |
|-----------|-------------------|----------|
| Coordinate systems | 1960s MGRS/Indian Datum ≠ modern GPS | MGRS → WGS84 transformation pipeline |
| Map alignment | Wartime maps distorted, no GPS ground truth | Manual georeferencing with control points |
| Spatial uncertainty | "Near the river" ≠ precise coordinates | H3 hexagonal grid with probability zones |
| Terrain change detection | 50+ years of erosion, development, vegetation | Multi-temporal imagery comparison (historical vs current) |
| Data integration | Multiple imagery sources, different resolutions | Geospatial Studio for unified visualization |

**Track B: OCR Pipeline** (Lead: Vũ)
| Challenge | Why It's Difficult | Approach |
|-----------|-------------------|----------|
| Degraded scans | 50-year-old microfilm, faded ink, torn pages | Preprocessing (denoise, binarize, deskew) |
| Vietnamese handwriting | Diacritics, cursive, non-standard spelling | TrOCR fine-tuned on CDEC samples |
| Typed Vietnamese | Old typewriters, inconsistent fonts | KQNBSEI model (97.7% on diacritics) |
| Quality assurance | AI makes mistakes, need human verification | Label Studio HITL with confidence routing |

**Track C: Data Model** (Lead: Phụng)
| Challenge | Why It's Difficult | Approach |
|-----------|-------------------|----------|
| Entity extraction | Names, units, dates buried in free text | Manual tagging → future NER training |
| Contradictory records | Document A says X, Document B says Y | Flag contradictions, expert resolution |
| Incomplete data | Missing pages, partial records | Probabilistic linking (not certainty) |
| Schema design | Must link Person↔Unit↔Event↔Place | CIDOC CRM ontology (WarSampo model) |

### Layers → Tracks Mapping

```
LAYER                           TRACK           PILOT STATUS
──────────────────────────────────────────────────────────────
Layer 1: Digitize (OCR)         Track B (Vũ)    ✅ Core focus
Layer 2: Connect (Knowledge)    Track C (Phụng) ✅ Basic (50-100 entities)
Layer 3: Predict (Spatial)      Track A (Tuệ)   ⚠️ Manual demo only
Layer 4: Search (Field ops)     —               ❌ Year 2
```

The pilot answers: **"Can we make Layers 1-2 work reliably?"** If yes, Layers 3-4 become feasible.

Track A (Geospatial) supports all layers — coordinate transforms feed into Layer 1, map overlays visualize Layer 3.

---

## 3. What We Have (Current State)

### Track A: Geospatial
| Asset | Status | Notes |
|-------|--------|-------|
| Geospatial Studio | ✅ Working | Map overlay and visualization |
| L7014 Map Sheets | 🔄 Available | Need georeferencing |
| MGRS conversion tools | ✅ Available | GDAL/PROJ installed |
| H3 library | ✅ Available | Ready to implement |
| Satellite imagery | 🔄 To explore | Google Earth, Sentinel, Planet, historical archives |
| DEM data | 🔄 To explore | SRTM, ALOS for terrain analysis |

### Track B: OCR Pipeline
| Asset | Status | Notes |
|-------|--------|-------|
| TrOCR Model | ⏳ Not tested | Need baseline accuracy on handwritten |
| KQNBSEI Model | ⏳ Not tested | Need baseline accuracy on typed |
| Label Studio | ⏳ Not deployed | Ready to install |
| Sample CDEC scans | ✅ Available | Test corpus ready |

### Track C: Data Model
| Asset | Status | Notes |
|-------|--------|-------|
| PostGIS Database | ✅ Available | Schema not yet designed |
| CIDOC CRM reference | ✅ Available | WarSampo model documented |
| Sample entities | ⏳ Not started | Need manual extraction |

### Team
| Role | Person | Status |
|------|--------|--------|
| Tech Lead / Track A | Lê Quang Tuệ | ✅ Approved |
| ML Engineer / Track B | Nguyễn Hoàng Vũ | ⏳ Pending |
| Data Architect / Track C | Đỗ Minh Phụng | ⏳ Pending |

---

## 4. What We Need to Prove (Feasibility Questions)

### Track A: Geospatial
| Question | Success Threshold | Status |
|----------|-------------------|--------|
| Can we georeference L7014 maps accurately? | <50m alignment error | ⏳ Untested |
| Can we convert MGRS → WGS84 reliably? | >95% parse success | ⏳ Untested |
| Can H3 grid visualize probability zones? | Working heatmap demo | ⏳ Not started |
| Can we detect terrain changes over 50 years? | Visual comparison demo | ⏳ Not started |
| Can we integrate multi-source imagery? | 2+ sources overlaid | ⏳ Not started |

### Track B: OCR Pipeline
| Question | Success Threshold | Status |
|----------|-------------------|--------|
| Can OCR read typed Vietnamese? | >80% accuracy | ⏳ Untested |
| Can OCR read handwritten Vietnamese? | >55% accuracy (40% minimum) | ⏳ Untested |
| Can preprocessing improve degraded scans? | Measurable accuracy gain | ⏳ Untested |
| Can HITL workflow scale with volunteers? | 5-10 active validators | ⏳ Not started |

### Track C: Data Model
| Question | Success Threshold | Status |
|----------|-------------------|--------|
| Can we design a working schema? | Person↔Unit↔Event↔Place linked | ⏳ Not started |
| Can we extract entities from text? | 50+ entities manually entered | ⏳ Not started |
| Can we query cross-references? | Working demo queries | ⏳ Not started |
| Can we detect contradictions? | Flag conflicting records | ⏳ Not started |

### Cross-Track Integration
| Question | Success Threshold | Status |
|----------|-------------------|--------|
| End-to-end demo working? | Scan → OCR → Entity → Map | ⏳ Not started |

---

## 5. Work Breakdown

### Phase 0 Tasks (Feb-Mar) — Setup & Baselines

| ID | Task | Lead | Status | Deliverable |
|----|------|------|--------|-------------|
| 0.1 | Finalize funding & onboard team | Tuệ | ⏳ | Team confirmed |
| 0.2 | Deploy PostGIS + Label Studio | Vũ | ⏳ | Infrastructure running |
| 0.3 | Baseline KQNBSEI (100 typed pages) | Vũ | ⏳ | Accuracy benchmark |
| 0.4 | Baseline TrOCR (100 handwritten pages) | Vũ | ⏳ | Accuracy + error analysis |
| 0.5 | Document baseline findings | All | ⏳ | Assessment report |

### Phase 1 Tasks (Apr-May) — Core Pilot

**Track A: Geospatial** (Lead: Tuệ)

| ID | Task | Status | Deliverable |
|----|------|--------|-------------|
| A1 | Georeference 3-5 L7014 sheets | ⏳ | WGS84-aligned tiles |
| A2 | Implement H3 grid overlay (res-9) | ⏳ | Visual grid layer |
| A3 | Create demo heatmap (manual POC) | ⏳ | Working visualization |
| A4 | Integrate multi-source imagery | ⏳ | Satellite/aerial layers in viewer |
| A5 | Historical vs current comparison | ⏳ | Terrain change detection demo |
| A6 | Evaluate DEM for terrain analysis | ⏳ | Slope/elevation overlays |

**Track B: OCR Pipeline** (Lead: Vũ)

| ID | Task | Status | Deliverable |
|----|------|--------|-------------|
| B1 | Test preprocessing pipelines | ⏳ | Best recipe documented |
| B2 | Set up HITL validation workflow | ⏳ | Label Studio project |
| B3 | First fine-tune cycle (500 corrections) | ⏳ | Before/after comparison |
| B4 | Process 500-800 documents | ⏳ | Validated transcriptions |

**Track C: Data Model** (Lead: Phụng)

| ID | Task | Status | Deliverable |
|----|------|--------|-------------|
| C1 | Design PostGIS schema | ⏳ | SQL schema file |
| C2 | Manual entry of 50+ entities | ⏳ | Populated test DB |
| C3 | MGRS → WGS84 coordinate pipeline | ⏳ | Validated converter |
| C4 | Demo cross-reference queries | ⏳ | Working examples |

### Phase 2 Tasks (Jun) — Integration & Demo

| ID | Task | Lead | Status | Deliverable |
|----|------|------|--------|-------------|
| 2.1 | End-to-end integration | All | ⏳ | Scan → OCR → Map demo |
| 2.2 | Prepare conference presentation | Tuệ | ⏳ | Slides + demo |
| 2.3 | Document feasibility findings | All | ⏳ | Written report |

---

## 6. Timeline

### Realistic Constraints
- **Tet Holiday:** Late Jan – mid Feb (limited work capacity)
- **Team availability:** Vũ & Phụng pending approval
- **Conference deadline:** June 2026 TTU Vietnam Center

### Phase 0: Setup (Feb – Mar 2026)
*Reduced scope — Tet recovery, team onboarding*

| Week | Focus | Activities |
|------|-------|------------|
| Feb W2-3 | Post-Tet ramp-up | Finalize funding, onboard team |
| Feb W4 | Infrastructure | Deploy PostGIS, Label Studio locally |
| Mar W1-2 | Baselines | Run initial OCR tests (50-100 pages each type) |
| Mar W3-4 | Assessment | Document baseline accuracy, identify blockers |

**Deliverable:** Baseline accuracy report + infrastructure ready

### Phase 1: Core Pilot (Apr – May 2026)
*Main work phase — full team active*

| Week | Focus | Activities |
|------|-------|------------|
| Apr W1-2 | Track A | Georeference 3-5 L7014 sheets, H3 overlay |
| Apr W3-4 | Track B | Preprocessing experiments, Label Studio workflow |
| May W1-2 | Track B | First fine-tune cycle (500 corrections) |
| May W3-4 | Track C | Schema finalized, 50+ entities entered |

**Deliverable:** Working pipeline, 500-800 documents processed

### Phase 2: Integration & Demo (Jun 2026)
*Conference preparation*

| Week | Focus | Activities |
|------|-------|------------|
| Jun W1-2 | Integration | End-to-end demo (scan → OCR → map) |
| Jun W3 | Polish | Prepare presentation, document results |
| Jun W4 | Conference | Present at TTU Vietnam Center |

**Deliverable:** Conference demo + feasibility report

```
         Feb 2026       Mar 2026        Apr 2026        May 2026        Jun 2026
         ─────────────────────────────────────────────────────────────────────────
              🧧 TET
         [  Setup & Onboard  ][  Baselines  ][   Core Pilot   ][  Integration  ]
                                    ↑              ↑                   ↓
                              Accuracy Report   500-800 docs    CONFERENCE DEMO
```

---

## 7. Budget Justification

### Personnel (Fixed)

| Role | Person | Cost | Justification |
|------|--------|------|---------------|
| Tech Lead / Project Manager | Lê Quang Tuệ | $15,000 | Geospatial systems, AI/LLM integration, team coordination, PI liaison |
| ML Infrastructure Engineer | Nguyễn Hoàng Vũ | $15,000 | OCR pipeline, model fine-tuning, infrastructure optimization |
| Data/System Architect | Đỗ Minh Phụng | $15,000 | PostGIS, knowledge graph, 10M+ POI production experience |
| **Personnel Subtotal** | | **$45,000** | |

### Variable Costs

| Item | Budget | Usage |
|------|--------|-------|
| Hardware (Mac Mini M4 Pro × 2) | $5,000 | Local inference, data sovereignty |
| Cloud compute (burst capacity) | $3,000 | GPU training runs, API fallback testing |
| ML consulting / expert review | $5,000 | Model architecture review, troubleshooting OCR edge cases |
| Software licenses & tools | $2,000 | Label Studio enterprise (if needed), preprocessing tools |
| **Variable Subtotal** | **$15,000** | |

### Total Request

| Category | Amount |
|----------|--------|
| Personnel | $45,000 |
| Variable | $15,000 |
| **Total** | **$60,000** |

### Non-Monetary Request

> **TTU Faculty Collaboration:** Request access to TTU Computer Science / AI / Computer Vision faculty for:
> - Technical consultation on OCR model selection and fine-tuning strategy
> - Review of ML pipeline architecture
> - Potential co-authorship on methodology publications
> - Student project alignment (capstone/thesis opportunities)

This is a novel problem (degraded Vietnamese handwriting on 50-year-old microfilm) — expert guidance significantly de-risks the technical approach.

### ROI Analysis

| Metric | Manual | AI-Assisted | Savings |
|--------|--------|-------------|---------|
| Cost per document | $65 | ~$8-10 | 85% |
| Time per document | 45 min | 10-15 min | 67-78% |
| Break-even point | — | ~1,000 docs | — |

**Pilot phase (500-800 docs):** Primary goal is **feasibility validation**, not ROI. We're investing to answer: *Does this approach work?*

**Year 2+ projection:** If feasibility proven, scaling to 5,000+ docs/year yields significant ROI. The pilot de-risks that investment.

---

## 8. Dependencies & Access Needs

### What We Need from TTU

| Need | Purpose | Priority |
|------|---------|----------|
| **CDEC Archive Access** | Source documents for OCR testing and processing | 🔴 Critical |
| **L7014 Map Scans** | Historical maps for georeferencing | 🔴 Critical |
| **Server/Storage Space** | Host PostGIS, Label Studio, processed outputs | 🟡 High |
| **VPN Access** | Remote team access to TTU systems | 🟡 High |
| **Faculty Introduction** | Connect with CS/AI faculty for consultation | 🟡 High |

### What We Need from Broader VWAI Team

| Need | From Whom | Purpose |
|------|-----------|---------|
| Sample validated transcriptions | Historical Research team | Ground truth for OCR accuracy testing |
| Entity examples (names, units, places) | Historical Research team | Schema validation, test data |
| Coordinate verification | QC: Military History & GIS | Validate MGRS → WGS84 conversions |
| Volunteer recruitment | Student Research Specialists | HITL validators for Label Studio |
| Domain expertise | Oral History Specialists | Fuzzy location interpretation |

### External Dependencies

| Dependency | Risk if Unavailable | Mitigation |
|------------|---------------------|------------|
| Vũ & Phụng approval | Tracks B & C delayed | Tuệ covers critical path; hire contractors |
| Mac Mini hardware | Local inference not possible | Use cloud GPU (RunPod, Lambda) |
| TTU archive access | No source documents | Work with sample set already obtained |

---

## 9. Reporting & Communication

### Reporting Structure

| Report | Frequency | Audience | Content |
|--------|-----------|----------|---------|
| **Weekly Update** | Every Friday | Tech team internal | Task progress, blockers, next week plan |
| **Bi-weekly Summary** | Every 2 weeks | PIs | Milestone status, key metrics, decisions needed |
| **Monthly Report** | End of month | Full VWAI team | Progress summary, demos, integration points |
| **Milestone Report** | At each decision point | PIs + stakeholders | Detailed assessment, recommendations |

### Communication Channels

| Channel | Purpose | Participants |
|---------|---------|--------------|
| Slack/Discord | Daily coordination | Tech team |
| Email | Formal updates, PI communication | All |
| Video call | Bi-weekly sync, demos | Tech team + PIs |
| Shared folder | Documents, reports, outputs | All VWAI |

### Decision Escalation

```
Technical decisions (tool choices, architecture)
    → Tech team decides, inform PIs

Resource decisions (budget reallocation, hiring)
    → Tech Lead proposes, PIs approve

Strategic decisions (pivot, major scope change)
    → Tech Lead recommends, PIs decide
```

---

## 10. Integration with Broader VWAI Team

### Team Structure Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         VWAI PROJECT                                │
├─────────────────────────────────────────────────────────────────────┤
│  PIs: Dr. Maxner (VNCA), Dr. Milam (IPAC), Dr. Võ                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐      │
│  │ TECH TEAM (3)   │  │ RESEARCH (6)    │  │ SPECIALISTS     │      │
│  │ Tuệ, Vũ, Phụng  │  │ Historical      │  │ Oral History(4) │      │
│  │                 │  │ Analysis        │  │ QC/GIS (2)      │      │
│  │ Builds tools    │  │                 │  │ Editors (4)     │      │
│  │ & pipelines     │  │ Uses outputs    │  │                 │      │
│  └────────┬────────┘  └────────┬────────┘  └────────┬────────┘      │
│           │                    │                    │               │
│           └────────────────────┴────────────────────┘               │
│                                │                                    │
│                    ┌───────────┴───────────┐                        │
│                    │ STUDENT VALIDATORS    │                        │
│                    │ (18 from USSH, Huế,   │                        │
│                    │  Fulbright)           │                        │
│                    │                       │                        │
│                    │ HITL annotation in    │                        │
│                    │ Label Studio          │                        │
│                    └───────────────────────┘                        │
└─────────────────────────────────────────────────────────────────────┘
```

### How Tech Team Enables Others

| VWAI Team | What They Do Now | What Tech Team Provides |
|-----------|------------------|-------------------------|
| **Historical Research (6)** | Manual document reading | Searchable transcriptions, auto-generated report templates |
| **Student Specialists (18)** | Manual transcription | HITL interface — verify AI output instead of transcribe from scratch |
| **QC: Military History & GIS (2)** | Manual coordinate work | Automated MGRS → WGS84, validation tools |
| **Oral History (4)** | Interview transcripts | Cross-reference with document evidence |
| **Report Editors (4)** | Manual report writing | Pre-filled templates with embedded maps |

### Integration Touchpoints

| Phase | Integration Activity | With Whom |
|-------|---------------------|-----------|
| **Phase 0** | Get sample validated transcriptions for ground truth | Historical Research |
| **Phase 0** | Recruit 5-10 validators for pilot | Student Specialists |
| **Phase 1** | Validate coordinate conversions | QC: GIS team |
| **Phase 1** | Test Label Studio workflow with validators | Student Specialists |
| **Phase 2** | Demo to full team, gather feedback | All VWAI |
| **Ongoing** | Provide searchable outputs for research | Historical Research |

### Feedback Loops

```
Student Validators                    Historical Researchers
       │                                      │
       │ Corrections to AI output             │ "This name is wrong"
       │                                      │ "This location doesn't exist"
       ▼                                      ▼
┌─────────────────────────────────────────────────────────────┐
│                     TECH TEAM                               │
│  • Retrain models on corrections                            │
│  • Fix systematic errors                                    │
│  • Improve preprocessing                                    │
└─────────────────────────────────────────────────────────────┘
       │
       │ Better AI outputs
       ▼
   Fewer corrections needed → Faster throughput
```

---

## 11. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| OCR accuracy too low (<50%) | Medium | High | Test commercial APIs as fallback; **TTU AI faculty consultation** |
| Handwritten text illegible | Medium | High | Prioritize typed documents; flag worst 10% for manual |
| Model fine-tuning fails | Medium | High | **Variable budget for ML expert consulting**; pivot to pre-trained APIs |
| Volunteer dropout | Medium | Medium | Gamification, shorter shifts, academic credit |
| Hardware bottleneck | Low | Medium | Cloud GPU burst via variable budget |
| Team unavailable (Vũ/Phụng) | Low | High | Tuệ can cover critical path; defer non-essential tracks |

---

## 12. Decision Points

### End of March 2026 (Baseline Assessment)
Based on initial OCR tests, recommend one of:
- **A) Proceed as planned:** Baseline accuracy promising (>50%), continue with fine-tuning
- **B) Adjust approach:** Accuracy low — prioritize typed docs, test commercial APIs
- **C) Major pivot:** OCR not viable — focus on manual transcription tooling

### End of May 2026 (Go/No-Go for Conference)
- Is end-to-end demo working?
- Do we have meaningful results to present?
- Decide scope of June presentation

### End of June 2026 (Feasibility Verdict)
- Present results at TTU Vietnam Center conference
- Document what works / what doesn't
- Recommend Year 2 scope based on evidence

---

## 13. Success Metrics

### By End of March (Phase 0)
| Metric | Target | Purpose |
|--------|--------|---------|
| Infrastructure deployed | PostGIS + Label Studio running | Foundation ready |
| Baseline OCR tested | 100+ pages each type | Know what we're working with |
| Team onboarded | 3 members active | Capacity confirmed |

### By End of June (Phase 1-2)
| Metric | Target | Purpose |
|--------|--------|---------|
| Documents processed | 500-800 | Prove pipeline works |
| OCR accuracy (typed) | >80% | Validate KQNBSEI |
| OCR accuracy (handwritten) | >55% | Validate TrOCR feasibility |
| Maps georeferenced | 3-5 sheets | Prove spatial overlay works |
| Conference demo | Working end-to-end | Communicate results |

### Feasibility Thresholds
| Question | Minimum to Continue | Ideal |
|----------|---------------------|-------|
| Typed OCR accuracy | >70% | >85% |
| Handwritten OCR accuracy | >40% | >60% |
| Pipeline throughput | 50 docs/week | 200 docs/week |
| Volunteer retention | 5 active | 10+ active |

---

## 14. What's NOT in Scope (Pilot Phase)

These items are deferred until feasibility is proven:

**Deferred to Year 2:**
- Full Bayesian feedback loop
- Taphonomic modeling (erosion/slope)
- AR tablet field application
- Automated named entity recognition (NER)
- Production scaling (>200 docs/week)
- Complete H3 POC formula implementation
- Field team integration

**Explicit Non-Goals for Pilot:**
- Perfect OCR accuracy (we're testing feasibility, not production quality)
- Large volunteer network (5-10 validators sufficient for pilot)
- Complete knowledge graph (50-100 entities enough to prove concept)
- Full provincial coverage (1-2 sample areas sufficient)

---

## Appendix: Team

| Role | Person | Status | Expertise |
|------|--------|--------|-----------|
| Tech Lead / Project Manager | Lê Quang Tuệ | ✅ Approved | GIS, AI/LLM, cartography, team coordination |
| ML Infrastructure Engineer | Nguyễn Hoàng Vũ | ⏳ Pending | Ahamove (5yr), 70% API cost reduction, production ML |
| Data/System Architect | Đỗ Minh Phụng | ⏳ Pending | Be Group (10M POIs), VinUni researcher, large-scale data |

### Responsibilities

**Tuệ (Tech Lead / PM):**
- Overall technical direction and architecture decisions
- PI communication and progress reporting
- Track A (Geospatial) hands-on work
- Coordinate across tracks, unblock team members
- Manage variable cost budget

**Vũ (ML Infrastructure):**
- Track B (OCR Pipeline) ownership
- Model training, fine-tuning, deployment
- Label Studio setup and maintenance
- Performance optimization

**Phụng (Data/System Architect):**
- Track C (Data Model) ownership
- PostGIS schema and knowledge graph design
- Query optimization and data pipelines
- Integration architecture

---

## Appendix: Folder Structure

See [README.md](README.md) for the full folder index.

```
VWAI/
├── README.md                        ← Start here
├── 01_One_Pager.md                  ← 2 min summary
├── 02_Project_Control.md            ← This file
├── 03_Context_Reference.md          ← Team & pipeline
├── 04-06_Appendix_*.md              ← Technical deep-dives
│
├── team/                            ← CVs
├── reference/                       ← Academic papers
├── ttu-docs/                        ← Official documents
├── maps/                            ← Sample L7014 map
└── archive/                         ← Old versions
```

---

## Appendix: Terminology

### Data & Documents

| Term | Definition |
|------|------------|
| **CDEC** | Captured Document Exploitation Center — US military program that translated/analyzed captured Vietnamese documents during the war. TTU holds 261,000 CDEC files. |
| **Microfilm** | Photographic film containing miniaturized document images. Our source material is 50+ year old microfilm scans, often degraded. |
| **NARA** | National Archives and Records Administration — US government archives holding Blue Force (American) military records. |

### AI & Machine Learning

| Term | Definition |
|------|------------|
| **OCR** | Optical Character Recognition — AI that converts images of text into machine-readable text. |
| **TrOCR** | Transformer-based OCR model by Microsoft, good for handwritten text. We fine-tune it on Vietnamese handwriting samples. |
| **KQNBSEI** | Specialized OCR model for typed Vietnamese text (97.7% accuracy on diacritics). Developed by EPHE-PSL University. |
| **Fine-tuning** | Training a pre-built AI model on our specific data to improve accuracy for our use case. |
| **HITL** | Human-in-the-Loop — system where AI does first pass, humans verify/correct. Combines AI speed with human accuracy. |
| **NER** | Named Entity Recognition — AI that extracts structured info (names, dates, places) from unstructured text. Deferred to Year 2. |

### Geospatial

| Term | Definition |
|------|------------|
| **MGRS** | Military Grid Reference System — coordinate system used by US/Allied forces in Vietnam. Must convert to modern GPS coordinates. |
| **WGS84** | World Geodetic System 1984 — modern GPS coordinate standard. All our output uses WGS84. |
| **PostGIS** | PostgreSQL database extension for storing and querying geographic data. Our primary database. |
| **H3** | Uber's hexagonal grid system. Divides earth into hexagonal cells for spatial analysis. We use resolution 9 (~100m cells). |
| **Georeferencing** | Process of aligning a scanned map to real-world coordinates so it overlays correctly on modern maps. |
| **L7014** | US Army 1:50,000 topographic map series covering Vietnam. Our primary historical map source. |

### Remote Sensing & Imagery

| Term | Definition |
|------|------------|
| **DEM** | Digital Elevation Model — 3D representation of terrain surface. Used for slope analysis and terrain change detection. |
| **SRTM** | Shuttle Radar Topography Mission — NASA elevation data at 30m resolution. Free global coverage. |
| **ALOS** | Advanced Land Observing Satellite (Japan) — provides higher resolution DEM data for terrain analysis. |
| **Sentinel** | European Space Agency satellite constellation providing free multispectral imagery. Updated every 5-10 days. |
| **Planet** | Commercial satellite provider with daily global imagery. Higher resolution but paid access. |
| **Change detection** | Technique comparing imagery from different time periods to identify terrain modifications (erosion, development, vegetation). |
| **Multispectral** | Imagery capturing multiple wavelength bands beyond visible light. Useful for vegetation and land cover analysis. |

### Knowledge Graph

| Term | Definition |
|------|------------|
| **Knowledge Graph** | Database structure that stores entities (people, places, events) and relationships between them. Enables complex queries like "find all events involving Unit X near location Y." |
| **CIDOC CRM** | International standard ontology (data model) for cultural heritage. Used by WarSampo and adopted by VWAI for interoperability. |
| **Entity** | A distinct thing in the knowledge graph: a Person, Unit, Event, or Place. |
| **Ontology** | Formal definition of entity types and their relationships. Our schema for how data connects. |

### Probability & Search

| Term | Definition |
|------|------------|
| **POC** | Probability of Containment — likelihood (0-100%) that remains are located in a given H3 cell. Based on evidence weighting. |
| **Heatmap** | Visual map showing POC scores as colors (red = high probability, blue = low). Guides field team priorities. |
| **Bayesian update** | Mathematical method to update probabilities based on new evidence. When a cell is excavated, we update neighboring cell probabilities. |
| **Taphonomy** | Study of how remains move/degrade over time (erosion, flooding, soil). Used to adjust search areas. Deferred to Year 2. |

### Tools & Infrastructure

| Term | Definition |
|------|------------|
| **Label Studio** | Open-source software for human annotation/validation. Our HITL interface where validators verify AI output. |
| **IIIF** | International Image Interoperability Framework — standard for serving high-resolution images. Used for historical map viewing. |
| **Allmaps** | Tool for overlaying georeferenced historical maps on modern basemaps with transparency controls. |

### Project-Specific

| Term | Definition |
|------|------------|
| **WarSampo** | Finnish WWII Linked Open Data project. Our methodological model — we adapt their approach for Vietnamese context. |
| **Red Force** | Vietnamese military forces (the subjects of CDEC documents). |
| **Blue Force** | US/Allied military forces (documented in NARA records). |
| **Track A/B/C** | Our three parallel work streams: A = Geospatial, B = OCR Pipeline, C = Data Model. |
