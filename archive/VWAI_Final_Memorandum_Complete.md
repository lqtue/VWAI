# MEMORANDUM

**TO:** Dr. Alex-Thái Đình Võ  
**FROM:** Lê Quang Tuệ  
**DATE:** January 29, 2026  
**SUBJECT:** AI-Assisted Document Processing Initiative – Q1 Pilot Proposal & Team Structure

---

## 1. Executive Summary

Per the Letter of Invitation dated 7 January 2026, my role as Document, Image & Geospatial Mapping Technician – AI/LLM Specialist includes responsibility to:

> *"Apply AI/LLM and computer vision tools to automate data extraction from handwritten, degraded, or complex archival materials"*

> *"Develop workflows to leverage artificial intelligence for pattern recognition, cross-referencing, and entity linking across large volumes of captured documents"*

To fulfill these responsibilities at scale—**261,000 CDEC files spanning 2.7 million pages**—within the one-year term, I propose establishing a **three-person technical team** and implementing an **AI-assisted Human-in-the-Loop (HITL) validation system**.

### The Challenge

At current manual processing rates (30-60 minutes per document), the CDEC corpus represents **130,500 hours of work**—equivalent to 15-20 years with existing staffing. This timeline is incompatible with VWAI's humanitarian mission to provide closure to Vietnamese families.

### Proposed Solution

An intelligent pipeline that:
- **AI generates "first draft" transcriptions** with character-level confidence scores
- **Vietnamese student volunteers verify and correct** drafts via intuitive web interface
- **Corrections become training data** for continuous AI improvement (40% → 75% accuracy)
- **Cross-validation ensures accuracy** on critical fields (coordinates, names, units)
- **Gamification drives volunteer retention** through leaderboards, badges, and recognition

### Resource Request

**Year 1 Budget:**
- **Personnel:** 3 technical team members @ $15,000 each = **$45,000**
- **Hardware & Software:** Mac Mini workstations, peripherals, licenses = **$5,000**
- **Total Year 1:** **$50,000**

**Years 2-3:** $45,000/year (personnel only)

### Expected Q1 Outcomes (March 31, 2026)

- **1,600-2,400 CDEC documents** processed and verified
- **67% time reduction** vs. manual transcription (demonstrated with metrics)
- **Working HITL system** operational with 10-15 Vietnamese student validators
- **AI accuracy improvement:** 40% → 70-75% through iterative learning
- **Conference-ready results** for June 2026 TTU Vietnam Center Annual Conference
- **ROI demonstration:** $35,000-45,000 value created from $12,500 Q1 investment

### Decision Point (April 2026)

Q1 metrics will inform Year 2 scaling strategy:
- **Option A:** Expand to full production with volunteer network (1,500-2,000 docs/quarter)
- **Option B:** Migrate compute-intensive tasks to TTU HPCC (5,000-10,000 docs/quarter)
- **Option C:** Integrate with commercial OCR APIs if cost-effective

---

## 2. The Processing Challenge

### Current State: CDEC Archive

**Corpus Composition:**
- **261,000 files** containing **2.7 million pages**
- Microfilm scans of handwritten Vietnamese field reports
- Written by soldiers under combat conditions (variable handwriting quality)
- Composite documents: English summaries + Vietnamese narratives + structured rosters + sketched maps
- Document types: After-action reports, unit diaries, casualty lists, prisoner interrogations, captured correspondence

**Additional Datasets (Not Yet Integrated):**
- Blue Force tracking data from NARA (US Daily Staff Journals)
- L7014 1:50k topographic map series (requires georeferencing)
- Aerial reconnaissance photography

### The Impossible Timeline

**Manual Processing Capacity:**
- Average time per document: 30-60 minutes (transcription + analysis + report)
- Current staffing: 1-2 technical personnel
- Annual throughput: ~500-1,000 documents
- **Time to complete 261,000 files: 15-20 years minimum**

**Mathematical Reality:**
```
261,000 files × 45 min/file = 195,750 hours
195,750 hours ÷ 2,080 hrs/year (full-time) = 94 person-years

With 2 staff: 47 years
With 5 staff: 19 years
With 10 staff: 9.4 years
```

**Without AI-assisted automation, mission completion is infeasible within humanitarian timelines.**

### Critical Limitations

1. **OCR Failure on Vietnamese Handwriting:** Standard OCR (Tesseract) achieves only 20-40% accuracy on degraded microfilm handwriting, requiring 60-80% manual correction—minimal benefit over full manual transcription

2. **Geospatial Disconnect:** Historical maps exist as static images without coordinate systems, preventing spatial analysis ("Show me all incidents within 5km of Firebase Buttons")

3. **Data Silos:** Cannot cross-reference: "What US units were operating near this CDEC report location on the same date?"

4. **Scalability Crisis:** Linear processing model cannot accommodate 261,000-file backlog plus ongoing additions from archives

### The Vision: From Digitization to Digital Transformation

**Current State (Stage 2 - Digitalization):**
- Documents scanned and stored as images
- Basic metadata entry (manual, error-prone)
- Static file repository

**Target State (Stage 3 - Queryable Intelligence):**
- **AI-assisted Vietnamese transcription** with human verification (67% time savings)
- **Georeferenced maps** overlaid on modern satellite imagery via interactive viewer
- **Unified spatiotemporal database** linking CDEC + US unit logs + terrain
- **Spatial queries:** "Show all CDEC reports within 5km of coordinate X, March 1969, with nearby US activities"
- **Auto-generated draft reports** pre-populated with transcriptions, maps, and cross-references
- **Continuous AI improvement** as human corrections accumulate (self-learning system)

---

## 3. Proposed Solution: The Intelligent HITL Pipeline

### Core Innovation: Human-AI Collaboration

Rather than attempting fully automated transcription (unrealistic for degraded microfilm), we propose a **hybrid workflow** where AI and humans collaborate:

1. **AI drafts** (even at 50-60% accuracy) save typing time
2. **Humans verify** and correct (faster than transcribing from scratch)
3. **Corrections train AI** (accuracy improves weekly: 40% → 55% → 70% → 75%)
4. **Cross-validation ensures quality** (2 validators for low-confidence documents)
5. **Gamification sustains volunteers** (leaderboards, badges, competitions)

**Result:** Continuous improvement loop where AI handles the "mechanical" transcription and humans focus on verification and analytical conclusions.

### System Architecture Overview

```
┌──────────────────────────────────────────────────────────────────┐
│ Phase 1: Document Ingestion & Intelligent Routing               │
├──────────────────────────────────────────────────────────────────┤
│ • Import CDEC file with existing metadata (Log #, Date)         │
│ • Convert wartime MGRS coordinates → WGS84 (PostGIS)            │
│ • Human triage: Classify document regions by type               │
│ • Route to specialized AI processors                             │
└──────────────────────────────────────────────────────────────────┘
                            ↓
┌──────────────────────────────────────────────────────────────────┐
│ Phase 2: AI Processing with Confidence Scoring                  │
├──────────────────────────────────────────────────────────────────┤
│ Type A (English/French typed)  → Tesseract OCR (85-90% acc)     │
│ Type B1 (Vietnamese typed)     → KQNBSEI HTR (85-92% acc)       │
│ Type B2 (Vietnamese handwritten) → TrOCR + confidence (50-75%)  │
│ Type C (Tables/rosters)        → Table Transformer              │
│ Type D (Maps/sketches)         → Geospatial pipeline            │
│                                                                  │
│ Output: Draft transcription + character-level confidence (0-100%)│
└──────────────────────────────────────────────────────────────────┘
                            ↓
┌──────────────────────────────────────────────────────────────────┐
│ Phase 3: Human-in-the-Loop Validation (Label Studio)            │
├──────────────────────────────────────────────────────────────────┤
│ Intelligent Routing Based on AI Confidence:                     │
│ • High confidence (>90%)       → Single validator               │
│ • Medium confidence (70-90%)   → Single validator               │
│ • Low confidence (<70%)        → TWO validators (cross-validate)│
│ • Critical fields (coords, names) → ALWAYS doubled              │
│                                                                  │
│ Validator Interface:                                             │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Original Scan       │ AI Draft (color-coded confidence)   │ │
│ │ [Microfilm image]   │ Ngày 15 tháng 3 năm 1968           │ │
│ │                     │ ^^^^  95%  ^^^^^  89%  ^^^  62%    │ │
│ │                     │ Green      Yellow      Red          │ │
│ │                     │                                      │ │
│ │                     │ [Click any text to edit]            │ │
│ │                     │ [✓ Approve] [Flag] [Next]           │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                  │
│ Real-Time Leaderboard & Gamification                            │
└──────────────────────────────────────────────────────────────────┘
                            ↓
┌──────────────────────────────────────────────────────────────────┐
│ Phase 4: Quality Assurance & Continuous Learning                │
├──────────────────────────────────────────────────────────────────┤
│ Cross-Validation Logic:                                          │
│ • 2 validators agree     → Approved                             │
│ • 2 validators disagree  → 3rd validator (tiebreaker)           │
│ • Track inter-validator agreement rates (quality metric)        │
│                                                                  │
│ AI Improvement Loop (Weekly):                                    │
│ • Export human corrections from Label Studio                    │
│ • Fine-tune TrOCR model on corrected samples (overnight batch)  │
│ • Deploy improved model → Accuracy increases iteratively        │
│                                                                  │
│ Expected Learning Trajectory:                                    │
│ Week 1:  40% AI accuracy → Humans correct 60%                  │
│ Week 4:  55% AI accuracy → Humans correct 45%                  │
│ Week 8:  70% AI accuracy → Humans correct 30%                  │
│ Week 12: 75% AI accuracy → Humans correct 25%                  │
└──────────────────────────────────────────────────────────────────┘
                            ↓
┌──────────────────────────────────────────────────────────────────┐
│ Phase 5: Geospatial Integration & Report Generation             │
├──────────────────────────────────────────────────────────────────┤
│ • Store verified data in PostGIS database (geospatially enabled)│
│ • Spatial join: CDEC data ↔ US unit logs ↔ Terrain maps        │
│ • Auto-generate Investigation Case Report template (docxtpl)    │
│ • Interactive web map viewer (IIIF + Allmaps)                   │
│ • Deliver 70% pre-filled reports to researchers                 │
└──────────────────────────────────────────────────────────────────┘
```

### Key Technical Components

#### A. AI Processing Layer

**For Typed Vietnamese Text (20% of corpus):**
- **KQNBSEI HTR Model** (Kraken framework, 22MB)
  - Trained on 19th-20th century French-Vietnamese printed documents
  - 97.7% accuracy on Vietnamese tone marks (combining diacritics)
  - 0.02 sec/page batch processing (CPU-only, no GPU required)
  - Source: Le, Tan & Bui, Marc (2024), EPHE-PSL University
  - Applicability: CDEC typed summaries, administrative headers
  - **Limitation:** Trained on high-quality printed books, not handwritten microfilm—accuracy may drop to 85-90% on CDEC typed sections

**For Handwritten Vietnamese Text (60% of corpus):**
- **TrOCR Vision-Language Model** (Microsoft, 558M parameters)
  - Base model fine-tuned on CDEC handwritten samples
  - Character-level confidence scores (softmax probabilities 0-100%)
  - Initial accuracy: 40-50% (off-the-shelf) → 70-75% (after fine-tuning on 5,000 corrections)
  - 2-3 sec/page inference on Mac Mini M4 Pro Neural Engine
  - Continuous improvement through HITL corrections

**For Structured Data (15% of corpus):**
- **Table Transformer** (DETR architecture)
  - Extracts tables while preserving row/column structure
  - Outputs CSV/JSON with unit rosters, casualty lists, radio logs
  - 5-10 sec per table

**For Geospatial Data (5% of corpus):**
- **Coordinate transformation:** Wartime MGRS (Military Grid Reference System) → WGS84
- **Map georeferencing:** L7014 1:50k topographic series to modern datum
- **IIIF Image Server + Allmaps:** Interactive browser-based map overlay with transparency controls

#### B. Human-in-the-Loop Validation

**Label Studio Web Interface:**
- Open-source annotation platform (Apache 2.0 license)
- Split-screen display: Original microfilm scan | AI draft transcription
- Confidence color-coding:
  - **Green (>90%):** High confidence—quick verification
  - **Yellow (70-90%):** Medium confidence—careful review
  - **Red (<70%):** Low confidence—likely error, high attention
- One-click corrections: Click any text to edit in-place
- Metadata extraction: Auto-populated fields (Date, Unit, Location, Coordinates)
- Quality flags: Button to escalate difficult cases to expert review

**Intelligent Routing Logic:**

| AI Confidence | Document Type | Assignment | Rationale |
|---------------|---------------|------------|-----------|
| >90% | No critical fields | Single validator | High confidence, quick verify |
| 70-90% | No critical fields | Single validator | Medium confidence, careful check |
| <70% | Any type | Two validators | Low confidence, cross-validate |
| Any | Contains coordinates | Two validators | Geospatial accuracy critical |
| Any | Contains personal names | Two validators | MIA relevance, sensitivity |
| Any | Contains unit IDs | Two validators | Historical accuracy important |

**Cross-Validation Consensus Algorithm:**
```
IF 2 validators agree:
    → Document approved
ELSE IF 2 validators disagree:
    → Route to 3rd validator (tiebreaker)
    → Majority vote (2 of 3) determines final answer
    → Track disagreement patterns for training
```

#### C. Gamification System

**Real-Time Public Dashboard:**
```
╔════════════════════════════════════════════════════════╗
║  VWAI DOCUMENT PROCESSING - LIVE STATS (Jan 29, 2026) ║
╠════════════════════════════════════════════════════════╣
║  📊 Overall Progress                                   ║
║  ████████░░░░░░░░░░░░ 1,247 / 261,000 (0.5%)         ║
║                                                        ║
║  🏆 Today's Top Validators                             ║
║  1. 🥇 NguyenT_089    47 docs    98.2% acc    ⚡🎯   ║
║  2. 🥈 LeeM_024       43 docs    97.1% acc    🔍     ║
║  3. 🥉 TranA_156      39 docs    99.1% acc    🛡️    ║
║                                                        ║
║  👑 All-Time Hall of Fame                              ║
║  1. 💎 PhamH_003      1,842 docs    Diamond Tier      ║
║  2. 💎 VoD_091        1,205 docs    Diamond Tier      ║
║  3. 🏆 BuiK_047         891 docs    Gold Tier         ║
║                                                        ║
║  📈 AI Learning Progress                               ║
║  Week 1:  AI 40% → Human corrects 60% (12 min/doc)   ║
║  Week 4:  AI 55% → Human corrects 45% (9 min/doc)    ║
║  Week 8:  AI 70% → Human corrects 30% (6 min/doc)    ║
║  Week 12: AI 75% → Human corrects 25% (5 min/doc)    ║
║  [Projected based on fine-tuning trajectory]          ║
║                                                        ║
║  🎯 This Week's Challenge                              ║
║  "Coordinate Chaos" - 500 low-confidence coordinates  ║
║  ███████░░░ 342/500 (68%)                             ║
║  Prize: $50 gift card + Pizza party                   ║
╚════════════════════════════════════════════════════════╝
```

**Achievement Badge System:**

**Speed Badges:**
- ⚡ "Lightning Strike": 10+ docs in 1 hour
- 🏃 "Marathon Runner": 100+ docs in 1 week
- 🚀 "Speedster": Top 10% validation time (avg <8 min/doc)

**Accuracy Badges:**
- 🎯 "Perfectionist": 99%+ accuracy over 50+ docs
- 🔍 "Eagle Eye": Caught 20+ AI errors others missed
- 🛡️ "Quality Guardian": 98%+ accuracy over 200+ docs

**Specialty Badges:**
- 📍 "Coordinate Master": 100 coordinates verified correctly
- 👤 "Name Detective": 50 personal names corrected
- 🎖️ "Unit Expert": 100 military unit designations verified
- 🗓️ "Time Lord": 200 dates extracted/corrected
- 🗺️ "Cartographer": 50 location names verified

**Contribution Tiers:**
- 🥉 Bronze: 50 documents
- 🥈 Silver: 200 documents
- 🏆 Gold: 500 documents
- 💎 Platinum: 1,000 documents
- 👑 Diamond: 2,500 documents

**Reward Structure:**

| Achievement | Reward | Frequency |
|-------------|--------|-----------|
| Weekly Top 3 | $10-25 gift card | Weekly |
| Team Competition Winner | $100 team pool + pizza | Weekly |
| Monthly MVP | $50 gift card + featured profile | Monthly |
| Tier Milestones | Certificate of contribution | One-time per tier |
| Top 10 All-Time | Letter of recommendation from Dr. Võ | Semester end |
| Diamond Tier | LinkedIn endorsement + $100 bonus | Semester end |

#### D. Continuous AI Improvement Loop

**Weekly Retraining Cycle:**

**Monday:** Export corrections from previous week (Label Studio → CSV)
- Example: 500 documents validated, 250 corrections made

**Tuesday:** Prepare training dataset
- Align corrected text with original microfilm images
- Format as (image, ground_truth_text) pairs
- Add to cumulative training set

**Wednesday Night:** Fine-tune TrOCR model (overnight batch job, 4-8 hours)
```python
# Pseudocode
training_data = load_corrections(start="2026-01-20", end="2026-01-27")
# 500 new samples + 2,000 cumulative = 2,500 total

model = TrOCRModel.from_pretrained("trocr-cdec-week7")
trainer = Seq2SeqTrainer(
    model=model,
    train_dataset=training_data,
    num_train_epochs=5,
    learning_rate=5e-5,
)
trainer.train()  # Runs overnight on Mac Mini (4-8 hours)
model.save("trocr-cdec-week8")
```

**Thursday:** Deploy improved model
- Replace production model with week8 version
- Test on validation set (held-out 15% of data)
- Measure accuracy improvement

**Friday:** Analyze results
- Compare week8 vs. week7 accuracy on same test documents
- Report metrics to team

**Expected Learning Trajectory:**

| Week | Training Samples | AI Accuracy | Human Correction % | Time per Doc |
|------|------------------|-------------|--------------------|--------------|
| 1 | 0 (baseline) | 40% | 60% | 12 min |
| 2 | 200 | 45% | 55% | 11 min |
| 4 | 500 | 55% | 45% | 9 min |
| 6 | 1,500 | 63% | 37% | 7 min |
| 8 | 3,000 | 70% | 30% | 6 min |
| 12 | 6,000 | 75% | 25% | 5 min |

**Impact on Throughput:**
- Week 1: 5 docs/hour per validator (12 min/doc)
- Week 12: 12 docs/hour per validator (5 min/doc)
- **Throughput increase: 2.4× over Q1**

---

## 4. Technical Team Structure

To fulfill the AI automation responsibilities outlined in the Letter of Invitation within the one-year term, I propose structuring this as a **three-person technical team** with complementary expertise.

### Team Composition

#### **1. Lê Quang Tuệ - Project Lead & Geospatial Specialist**

**Role:** Document, Image & Geospatial Mapping Technician – AI/LLM Specialist  
**Compensation:** $15,000/year  
**Status:** ✅ Approved per LOI dated 7 January 2026

**Background:**
- Geospatial analysis and cartography specialist
- Document processing and archival digitization
- AI/LLM integration and prompt engineering
- System architecture and workflow design

**Responsibilities:**
- Overall project leadership and coordination
- Document classification and triage (Type A/B/C/D routing)
- Geospatial pipeline development:
  - MGRS → WGS84 coordinate transformation
  - Historical map georeferencing (L7014 series)
  - PostGIS spatial database management
  - IIIF + Allmaps interactive viewer deployment
- AI strategy and model selection (TrOCR, KQNBSEI, Table Transformer)
- Report generation automation (docxtpl templates)
- Volunteer network coordination (HCMC, Hanoi, FUV partnerships)
- Conference presentation preparation (June 2026)

---

#### **2. Nguyễn Hoàng Vũ - Senior Software Engineer (Backend & ML Infrastructure)**

**Compensation:** $15,000/year  
**Status:** ⏳ Requesting approval

**Background:**
- **5+ years** backend systems engineering at Ahamove (Vietnam's leading last-mile delivery platform)
- **Proven track record:** Reduced map API costs by **70%** through self-hosted geospatial databases
- **Production ML deployment:** Integrated OSRM and Valhalla routing engines for multi-vehicle optimization
- **Optimization expertise:** Developed route optimization using Google OR-Tools and VROOM
- **Stack:** Node.js (Fastify), Python (FastAPI), Docker, AWS (EC2, EKS, Lambda), MongoDB, Redis, Elasticsearch
- **OpenStreetMap contributor:** Improved Vietnamese geospatial data quality

**Relevant Experience at Ahamove:**
| Achievement | VWAI Parallel |
|-------------|---------------|
| Built self-hosted geospatial databases | → Self-hosted TrOCR inference (local Mac Mini) |
| Cut API costs 70% ($XXK → $XXK savings) | → Local inference vs. cloud APIs (same cost optimization) |
| Integrated OSRM/Valhalla routing engines | → Integrate KQNBSEI/TrOCR models (similar complexity) |
| Route optimization (OR-Tools, VROOM) | → Optimize ML inference (batching, queuing, caching) |
| Fastify/FastAPI backend services | → Label Studio + ML backend architecture |
| Reduced driver navigation errors 30% | → Reduce validator correction time (same UX optimization) |
| Docker + AWS + K8S deployment | → Deploy entire HITL pipeline containerized |

**Responsibilities for VWAI:**
- **ML Model Integration:**
  - Deploy TrOCR, KQNBSEI, Table Transformer as API services
  - Containerize models (Docker) for reproducible deployment
  - Implement confidence scoring and routing logic
- **Backend Infrastructure:**
  - Set up Label Studio web platform
  - Configure PostgreSQL + PostGIS database
  - Build FastAPI ML backend for model inference
  - Implement Redis caching for performance
- **Inference Optimization:**
  - Batch processing (process 10 pages simultaneously)
  - Request queuing (handle 15 concurrent validators)
  - Model quantization (reduce memory footprint)
  - Load balancing (distribute inference across CPU/GPU/Neural Engine)
- **Performance Tuning:**
  - Reduce latency (target <3 sec per inference)
  - Optimize memory usage (48GB unified memory on Mac Mini)
  - Monitor system health (CPU, GPU, memory, disk)
- **Deployment & DevOps:**
  - Docker Compose orchestration
  - VPN setup for remote validators
  - Backup automation (daily PostgreSQL dumps)
  - Version control (Git) and CI/CD if scaling to HPCC

**Why Vũ Is Critical:**
This role requires someone who has **actually deployed ML systems in production**, not just trained models in notebooks. Vũ's experience building self-hosted geospatial services that handle millions of requests/day directly translates to the challenges of local ML inference optimization—the exact concern Tan (KQNBSEI author) raised about "vibing your way out of deep neural networks."

---

#### **3. Đỗ Minh Phùng - System Architect & Research Liaison**

**Compensation:** $15,000/year  
**Status:** ⏳ Requesting approval

**Background:**
- **Map Operations Team Leader** at Be Group (Vietnam's #2 ride-hailing platform, Oct 2022 - Nov 2025)
- **Research Assistant** at VinUniversity (Hanoi, Dec 2025 - Current)
  - Digital Twin Simulation for urban mobility and air quality
  - Agent-based modeling (GAMA platform)
- **Map Operation Associate** at Grab (Vietnam's #1 ride-hailing, Jul 2021 - Sep 2022)
- **Bachelor of Engineering:** Vietnamese-German University (Electrical Engineering & IT)
- **Admin:** Vietnam OpenStreetMap Community
- **Certifications:** OpenMapping Guru (Humanitarian OSM Team), Natural Resource Management (Fulbright Vietnam)

**Relevant Experience:**

| Achievement | Scale | VWAI Parallel |
|-------------|-------|---------------|
| Architected proprietary map operations tools at Be Group | 10M POIs, 99% independence from 3rd-party | → System architecture for HITL pipeline |
| Led 11-person team at Grab | Standardized 5M POIs | → Design validator workflows, training materials |
| Ingested street-level imagery pipeline | 35M images, 100,000 km coverage | → Handle 2.7M CDEC pages (smaller scale) |
| Built OCR workflow at Mercedes-Benz | 40% time reduction on PDF extraction | → Understand OCR challenges, preprocessing |
| Technical validation of OSM data | Turn restrictions, routing graphs, vehicle profiles | → Validate CDEC coordinate transformations |
| Maintained Technical Product Roadmap | Cross-functional (Engineering + Business) | → Q1→Q2→Q3 planning, stakeholder management |
| Monthly field studies with drivers | Identified navigation pain points | → Understand validator pain points, UX optimization |
| Data models for temporal geospatial attributes | Event-based routing optimization | → Design PostGIS schema for spatiotemporal queries |

**Responsibilities for VWAI:**
- **System Architecture & Design:**
  - Overall pipeline architecture (5-phase workflow)
  - Technology stack selection (Label Studio, TrOCR, PostGIS)
  - Integration points between components
  - Scalability planning (Q1 pilot → production)
- **Academic Research & Literature Review:**
  - Evaluate AI models (TrOCR vs. Donut vs. PaddleOCR)
  - Research preprocessing techniques (microfilm degradation mitigation)
  - Study Vietnamese OCR papers (KQNBSEI, related work)
  - Identify best practices from industry (Lyft, Mapbox mapping workflows)
- **Quality Assurance Methodology:**
  - Design cross-validation rules
  - Define accuracy metrics (character error rate, word error rate)
  - Establish inter-validator agreement thresholds
  - Create data validation logic (impossible Vietnamese syllables, coordinate sanity checks)
- **Geospatial Stack Coordination:**
  - PostGIS schema design (documents, validators, validations tables)
  - Spatial query optimization
  - Coordinate system validation (MGRS → UTM → WGS84)
  - Map georeferencing QA
- **Production Roadmap Planning:**
  - Q1 pilot execution plan (week-by-week)
  - Q2 decision framework (cluster vs. HPCC vs. commercial APIs)
  - Q3-Q4 scaling strategy
  - Risk mitigation plans
- **Conference Presentation Preparation:**
  - Structure June 2026 conference talk
  - Create visualizations (accuracy trajectories, throughput graphs)
  - Draft paper outline (methodology, results, future work)
- **Volunteer Program Design:**
  - Gamification mechanics (leaderboards, badges, rewards)
  - Training materials (orientation video, practice exercises)
  - Onboarding workflow (screening, certification)
  - Community management (Slack/Discord, office hours)

**Why Phùng Is Critical:**
This role requires someone who bridges **industry operations expertise** (large-scale data processing) with **academic research rigor** (literature review, conference presentations). Phùng's combination of 4+ years leading map operations at Be/Grab PLUS current research role at VinUniversity makes him ideal for ensuring the HITL system is both production-ready AND scholarly defensible.

---

### Team Synergy & Division of Labor

**Combined Team Credentials:**
- **13+ years** of geospatial systems experience
- **10M+ POIs** processed at scale (Phùng at Be Group)
- **5M POIs** standardized (Phùng at Grab)
- **35M street-level images** ingested (Phùng at Be Group)
- **OpenStreetMap leadership** (Phùng: Admin, Vũ: Contributor)
- **Production ML deployment** (Vũ: self-hosted routing engines)
- **Academic research background** (Phùng: VinUniversity, You: TTU)
- **Vietnamese linguistic expertise** (All three: native speakers)

**Why This Team Composition Works:**

The HITL pipeline requires **three distinct competencies**:

1. **Domain Expertise** (You)
   - Archival document processing
   - Geospatial analysis and cartography
   - AI/LLM strategy and integration
   - Humanitarian context understanding

2. **Systems Engineering** (Vũ)
   - Backend infrastructure (web services, databases)
   - ML model deployment and optimization
   - Performance tuning and cost optimization
   - Production reliability and monitoring

3. **Research & Operations** (Phùng)
   - System architecture and design
   - Academic literature and best practices
   - Data quality assurance methodology
   - Product planning and stakeholder management

**No single person possesses all three skillsets.** This is why industry ML projects typically require 3-5 engineers minimum.

**Comparison to KQNBSEI Project:**
- KQNBSEI (266 pages, printed text): 2-person team (Le Tan + Marc Bui)
- VWAI (261,000 files, handwritten microfilm): 3-person team
- **Our scope is 1,000× larger with more challenging data → 3-person team is proportionate**

---

## 5. Strategic Value & Return on Investment

### Q1 Pilot Performance Projections

**Volunteer Workforce:**
- 10-15 Vietnamese-fluent student volunteers (recruited via TTU partnerships with HCMC, Hanoi, FUV)
- 2-4 hours per week per student
- Total volunteer hours: 120-240 hours/week (10-15 students × 8-16 hrs/week)

**Processing Capacity:**

| Time Period | Docs/Hour/Validator | Weekly Output | 12-Week Total |
|-------------|---------------------|---------------|---------------|
| Week 1-2 (40% AI) | 5 | 150-300 docs | 450-600 |
| Week 3-4 (50% AI) | 6 | 180-360 docs | 360-720 |
| Week 5-8 (60% AI) | 8 | 240-480 docs | 960-1,920 |
| Week 9-12 (70% AI) | 10 | 300-600 docs | 1,200-2,400 |
| **Q1 Total** | | | **2,970-5,640** |

**Conservative Estimate: 1,600-2,400 documents processed in Q1**

**Comparison to Manual Baseline:**

| Metric | Manual Transcription | HITL Pipeline | Improvement |
|--------|---------------------|---------------|-------------|
| **Time per document** | 30-60 min | 10-12 min (Week 1) → 5-6 min (Week 12) | **67-80% reduction** |
| **Q1 throughput** | 200-400 docs (1-2 staff) | 1,600-2,400 docs (10-15 volunteers + 3 staff) | **8-12× faster** |
| **Accuracy** | 95-98% (single transcriber) | >95% (cross-validated) | Equivalent or better |
| **Total hours** | 100-400 hrs (staff) | 720-960 hrs (volunteers) + 180 hrs (staff oversight) | 2× volunteer hours but **8-12× output** |

### Three-Year Total Cost of Ownership

#### Personnel Costs

**Year 1:**
- Lê Quang Tuệ: $15,000
- Nguyễn Hoàng Vũ: $15,000
- Đỗ Minh Phùng: $15,000
- **Subtotal Year 1 Personnel:** $45,000

**Years 2-3:**
- Same personnel structure: $45,000/year
- **Subtotal Year 2-3:** $90,000 (2 years × $45,000)

**Total 3-Year Personnel:** $135,000

#### Hardware & Infrastructure Costs

**Year 1 Hardware Budget: $5,000**

**Primary Configuration (Recommended):**

| Item | Quantity | Unit Cost | Total | Notes |
|------|----------|-----------|-------|-------|
| **Mac Mini M4 Pro** | 2 | $1,999 | $3,998 | 48GB RAM, 1TB SSD each |
| External SSD (2TB) | 1 | $180 | $180 | Backup storage |
| Thunderbolt cables | 2 | $30 | $60 | Connectivity |
| **Subtotal Hardware** | | | **$4,238** | |
| **Claude Max** | 6 months | $60/mo | $360 | AI-assisted development |
| Gamification budget | - | - | $300 | Gift cards for Q1-Q2 |
| Contingency | - | - | $102 | Unexpected needs |
| **Total Year 1** | | | **$5,000** | |

**Rationale for 2× Mac Mini:**
- **Mac Mini #1:** Production (Label Studio + ML inference + PostgreSQL)
- **Mac Mini #2:** Development/Testing (model fine-tuning, experimentation, backup)
- Having 2 units allows:
  - Test new models without disrupting production
  - Fine-tune TrOCR overnight on Unit #2 while Unit #1 serves validators
  - Hardware redundancy (if Unit #1 fails, switch to Unit #2)
  - Parallel processing if volunteer load exceeds 15 users

**Alternative: Single Mac Mini Configuration ($2,500):**
If budget constraint requires, can start with 1 unit and add 2nd in Q2 based on Q1 results.

**Years 2-3 Operating Costs:**
- Electricity: $175/year (2 Mac Minis × 50W avg × $0.12/kWh × 8,760 hrs × 50% duty cycle)
- Maintenance: $200/year (thermal management, cleaning)
- Storage expansion: $100/year (additional backup drives)
- Software renewals: $100/year (if continuing Claude Max or other subscriptions)
- **Total Operating:** $575/year

**Total 3-Year Hardware & Infrastructure:**
- Year 1: $5,000
- Year 2: $575
- Year 3: $575
- **Subtotal:** $6,150

**Resale Value (End of Year 3):**
- 2× Mac Mini M4 Pro: ~$2,400 (60% residual value)
- Net hardware cost: $6,150 - $2,400 = **$3,750**

#### Total 3-Year Cost of Ownership

| Category | Cost |
|----------|------|
| Personnel (3 years) | $135,000 |
| Hardware & Infrastructure (net) | $3,750 |
| **Total 3-Year TCO** | **$138,750** |

### Return on Investment Analysis

#### Q1 Pilot ROI (First 3 Months)

**Investment:**
- Personnel (3 months): $11,250 ($45K/year ÷ 4)
- Hardware: $5,000
- **Total Q1:** $16,250

**Output:**
- Documents processed: 1,600-2,400
- Volunteer hours: 720-960 hours
- Staff oversight: 180 hours (3 staff × 60 hrs each)

**Value Created:**
- Manual processing time saved: 1,200-1,800 hours
  - (1,600 docs × 45 min avg) - (1,600 docs × 10 min avg) = 933 hours saved
  - Conservative: 800 hours saved
- Value at $25/hr volunteer rate: **$20,000-30,000**

**Q1 ROI: 1.2-1.8× return** (or 20-80% profit)

**Intangible Q1 Value:**
- Working HITL system validated
- Volunteer network established
- AI model improvement trajectory proven (40% → 70-75%)
- Conference-ready results (June 2026 presentation)

#### Year 1 Projection

**Investment:** $50,000 (personnel + hardware)

**Output:**
- Documents processed: 8,000-10,000 (scaling up volunteer network)
- Time saved vs. manual: 5,000-7,000 hours

**Value at $25/hr:** $125,000-175,000

**Year 1 ROI: 2.5-3.5× return**

#### 3-Year Projection

**Investment:** $138,750 (total TCO)

**Output:**
- Documents processed: 30,000-40,000 (sustained operations)
- Time saved vs. manual: 20,000-28,000 hours

**Value at $25/hr:** $500,000-700,000

**3-Year ROI: 3.6-5× return**

### Comparison to Alternatives

#### Alternative 1: Continue Manual Processing

**Approach:** Hire 5 full-time researchers @ $30,000/year

**Costs:**
- Year 1: $150,000
- 3-Year: $450,000

**Output:**
- ~5,000-7,000 documents over 3 years
- **Cost per document:** $64-90

**VWAI Proposal:**
- 3-Year: $138,750
- ~30,000-40,000 documents
- **Cost per document:** $3.47-4.63

**Savings: $311,250 (69% cost reduction)**

#### Alternative 2: Commercial OCR APIs

**Approach:** Use Google Cloud Vision or Azure Computer Vision

**Costs:**
- API: $0.0015-0.003 per page (Vietnamese text)
- 2.7M pages × $0.002 avg = **$5,400**
- Still requires human verification (60-70% correction)
- Personnel for verification: 3 staff × $30K = $90,000/year

**3-Year Total:** $275,400 (API + personnel)

**VWAI Proposal:**
- 3-Year: $138,750
- **Savings: $136,650 (50% cost reduction)**
- **Additional benefits:** Local data control, AI improves over time, no ongoing API fees

#### Alternative 3: Outsource to Third-Party OCR Service

**Approach:** Contract specialized Vietnamese OCR company

**Estimated Costs:**
- $0.50-1.00 per page for Vietnamese handwritten OCR
- 2.7M pages × $0.75 avg = **$2,025,000**

**VWAI Proposal:**
- 3-Year: $138,750
- **Savings: $1,886,250 (93% cost reduction)**

### Summary: VWAI Is Extraordinarily Cost-Effective

| Approach | 3-Year Cost | Docs Processed | Cost/Doc |
|----------|-------------|----------------|----------|
| **VWAI Proposal** | **$138,750** | **30,000-40,000** | **$3.47-4.63** |
| Manual (5 staff) | $450,000 | 5,000-7,000 | $64-90 |
| Commercial APIs | $275,400 | 30,000-40,000 | $6.89-9.18 |
| Outsourced OCR | $2,025,000 | 261,000 (full corpus) | $7.76 |

**The HITL approach delivers 3-10× better cost-efficiency than any alternative.**

---

## 6. Implementation Plan

### Phase 1: Q1 Pilot (January–March 2026)

**Objective:** Validate HITL system, establish volunteer network, generate conference-worthy results

#### Week 1-2: Infrastructure Setup (Technical Team)

**Vũ leads:**
- Procure 2× Mac Mini M4 Pro (48GB/1TB), peripherals
- Install Docker Desktop, PostgreSQL + PostGIS
- Deploy Label Studio (containerized)
- Set up FastAPI ML backend skeleton
- Configure VPN access (WireGuard or TTU GlobalProtect)

**You lead:**
- Set up development environment (VS Code, QGIS, Git)
- Organize CDEC sample dataset (100 priority files)
- Create document triage guidelines (Type A/B/C/D classification)

**Phùng leads:**
- Design Label Studio annotation interface (XML config)
- Draft validator training materials (orientation slides)
- Create leaderboard dashboard wireframe

**Deliverable:** Functional infrastructure ready for model integration

---

#### Week 2-3: Model Integration (Technical Team)

**Vũ leads:**
- Integrate TrOCR model as API service
  - Docker container with PyTorch + Transformers
  - Endpoint: POST /predict (image → text + confidence)
  - Test inference speed on Mac Mini (target <3 sec/page)
- Set up batch processing queue (Redis)

**You lead:**
- Integrate KQNBSEI (Kraken) for typed Vietnamese
  - CLI wrapper around kraken command
  - Routing logic: if doc_type == "B1" → use KQNBSEI, else TrOCR
- Test on 20 sample CDEC documents (10 typed, 10 handwritten)

**Phùng leads:**
- Research preprocessing techniques (literature review)
- Test preprocessing pipeline (dewarping, denoising, binarization)
- A/B test: raw images vs. preprocessed images → measure accuracy delta

**Deliverable:** Working AI pipeline processing test documents with confidence scores

---

#### Week 3-4: Volunteer Recruitment & Training

**You lead:**
- Coordinate with TTU partners at HCMC, Hanoi universities, FUV
- Post recruitment materials (flyers, social media, email)
- Screen applicants (Vietnamese reading proficiency test)
- Schedule orientation sessions (Zoom)

**Phùng leads:**
- Conduct 60-minute orientation workshops (2-3 sessions for different time zones)
  - VWAI mission and humanitarian context (10 min)
  - HITL workflow explanation (10 min)
  - Label Studio demo (20 min)
  - Gamification and rewards (10 min)
  - Q&A (10 min)
- Create practice exercises (10 documents with gold-standard answers)
- Issue VPN credentials and Label Studio accounts

**Vũ leads:**
- Set up validator accounts (role-based permissions)
- Configure rate limiting (prevent overwhelming system)
- Deploy leaderboard API (real-time stats)

**Deliverable:** 10-15 trained volunteers ready to start validation

---

#### Week 4: Soft Launch (Technical Team + Volunteers)

**All hands:**
- Process 50-100 documents with close monitoring
- Track metrics: time per doc, accuracy, confidence scores, inter-validator agreement
- Daily check-ins: Slack/Discord channel for questions
- Fix bugs and UX issues as they arise

**Phùng leads:**
- Monitor validator quality (flag low accuracy, provide feedback)
- Adjust routing logic based on initial confidence distributions
- Collect user feedback (what's confusing, what's helpful)

**Deliverable:** System validated, ready for full production

---

#### Week 5-10: Production Processing (200-300 docs/week)

**Volunteers:**
- Validate 200-300 documents per week
- Participate in weekly competitions (gamification)
- Report issues or edge cases

**Technical Team:**
- **Vũ:** Monitor system performance (CPU, memory, latency)
  - Optimize if bottlenecks appear (batching, caching)
  - Fix bugs, deploy patches
- **You:** Coordinate volunteers, answer questions
  - Weekly progress reports to Dr. Võ
  - Adjust gamification rewards based on engagement
- **Phùng:** Quality assurance
  - Review flagged documents (expert review queue)
  - Track accuracy metrics, inter-validator agreement
  - Identify systematic errors (AI or validator patterns)

**Weekly AI Retraining (Vũ + You):**
- **Monday:** Export corrections from Label Studio
- **Wednesday night:** Fine-tune TrOCR (overnight batch job)
- **Thursday:** Deploy improved model
- **Friday:** Measure accuracy improvement on held-out test set

**Expected Trajectory:**
- Week 5: AI 55% accurate → Validators correct 45%
- Week 7: AI 63% accurate → Validators correct 37%
- Week 10: AI 70% accurate → Validators correct 30%

**Deliverable:** 1,200-1,800 documents processed by Week 10

---

#### Week 11-12: Analysis & Conference Preparation

**Phùng leads:**
- Compile Q1 performance metrics
  - Total documents processed: X
  - Average time per document: X min (Week 1 vs. Week 12)
  - AI accuracy improvement: 40% → 70-75%
  - Inter-validator agreement rate: X%
  - Volunteer retention rate: X%
- Create visualizations (graphs, charts)
- Draft conference presentation outline
  - Title: "AI-Assisted Human-in-the-Loop Processing of Vietnamese Historical Military Documents"
  - Sections: Motivation, Methodology, Results, Future Work

**You lead:**
- Write Q1 report for Dr. Võ
  - Executive summary, metrics, lessons learned
  - Recommendations for Q2 scaling
- Conduct volunteer survey (satisfaction, suggestions)
- Host virtual celebration event (awards ceremony, certificates)

**All:**
- Prepare Q2 proposal (3 scaling options)

**Deliverable:**
- Q1 Report with metrics and ROI analysis
- Conference presentation draft
- Q2 funding recommendation

---

### Phase 1B: Decision Point (April 2026)

**Based on Q1 results, recommend one of three scaling paths:**

#### Option A: Expand Volunteer Network (Local Mac Mini Cluster)

**When to choose:**
- Q1 shows HITL system works well (>90% validator satisfaction)
- AI accuracy improved as expected (40% → 70-75%)
- Volunteer recruitment feasible (10-15 active in Q1 → expand to 20-30)
- Budget prefers incremental investment ($0 additional hardware in Q2, maybe $2K in Q3)

**Q2 Actions:**
- Recruit 10-15 additional volunteers (target 20-30 total)
- Continue Q1 workflow (weekly retraining, gamification)
- Process 1,500-2,000 documents in Q2

**Q3 Actions:**
- Consider adding 3rd Mac Mini if 2 units become bottleneck
- Expand to 40-50 volunteers if budget allows
- Process 2,500-3,000 documents in Q3

**Pros:**
- Minimal additional investment
- Maintain full control (no dependency on TTU IT)
- Volunteer-powered model is sustainable

**Cons:**
- Linear scalability (limited by Mac Mini hardware)
- Manual management overhead (gamification, training)

---

#### Option B: Migrate to TTU HPCC (Data Center Scale)

**When to choose:**
- Q1 proves system works AND demand is higher than expected
- Volunteer network grows beyond 30 (Mac Mini can't handle load)
- TTU approves GPU node contribution (~$30,000)
- Timeline requires faster processing (>5,000 docs/quarter)

**Q2 Actions:**
- Submit TTU HPCC provisioning request
- Work with TTU IT to deploy Docker containers on HPC
- Provision PostgreSQL database on TTU infrastructure
- Migrate volunteers to HPCC-hosted Label Studio

**Q3 Actions:**
- Full production on HPCC (20,000+ docs/quarter capacity)
- Keep Mac Minis as development/testing environment

**Pros:**
- Massive scalability (100+ concurrent validators)
- Enterprise reliability (redundant power, backup, support)
- Professional IT management

**Cons:**
- 8-12 week provisioning timeline (Q2 → Q3 transition)
- Higher operating cost ($7,900/year avg usage-based)
- Dependency on TTU IT (less control)

---

#### Option C: Hybrid (Commercial APIs + Local HITL)

**When to choose:**
- Q1 shows handwritten Vietnamese accuracy plateaus at 60-65%
- Commercial APIs (Google Vision, Azure) demonstrate >75% accuracy
- Cost analysis shows APIs are competitive ($0.002/page × 2.7M = $5,400 one-time)

**Q2 Actions:**
- Test commercial APIs on 1,000 sample CDEC documents
- Compare accuracy: TrOCR (fine-tuned) vs. Google Vision vs. Azure
- If commercial API wins (>10% better accuracy), switch to API for first-pass
- Keep HITL validation (humans still correct, but less work)

**Q3 Actions:**
- Process full corpus with commercial API + HITL validation
- Use savings to expand geospatial features (Allmaps, spatial queries)

**Pros:**
- May achieve higher accuracy faster
- Offloads compute burden (no local inference)
- Predictable costs

**Cons:**
- Recurring API fees (though small: $0.002/page)
- Data uploaded to external service (may violate confidentiality?)
- No continuous AI improvement (API is fixed)

---

### Phase 2: Q2-Q4 2026 (Scaling & Optimization)

#### Q2 (April-June): Infrastructure Expansion

**Selected Path from Phase 1B**
- Deploy chosen scaling approach (A, B, or C)

**Common Activities:**
- Recruit additional volunteers (target 20-30 active)
- Integrate Blue Force data from NARA
  - Parse US Daily Staff Journals into spatiotemporal trajectories
  - Load into PostGIS database (separate table)
- Begin georeferencing L7014 topographic maps
  - Identify Ground Control Points (GCPs)
  - Use GDAL for polynomial transformation
  - Generate IIIF image tiles
- Refine TrOCR model with Q1 corrections (5,000+ samples)

**Output:** 1,500-2,000 documents processed

---

#### Q3 (July-September): Production Rollout

**Geospatial Features:**
- Deploy Allmaps interactive viewer
  - Overlay historical maps on modern satellite imagery
  - Transparency controls, side-by-side comparison
- Implement spatial join queries
  - "Show all CDEC reports within 5km of Firebase X, March 1969"
  - Cross-reference CDEC (Red Force) + US logs (Blue Force)
- Automated report generation
  - docxtpl template population
  - Embedded map snippets (IIIF tiles)
  - 70% pre-filled reports delivered to researchers

**Volunteer Management:**
- Expand training program (onboard 10 new validators)
- Host mid-year virtual event (awards, Q2 retrospective)
- Adjust gamification based on Q1-Q2 feedback

**Output:** 2,000-3,000 documents processed

---

#### Q4 (October-December): Standardization

**Documentation:**
- Standard Operating Procedures (SOP) manual
  - System architecture overview
  - Model training and deployment guide
  - Validator onboarding checklist
  - Troubleshooting common issues
- Technical documentation for TTU IT
  - Docker Compose files
  - Database schema (PostGIS)
  - API endpoints and authentication

**Year-End Activities:**
- Train additional technical staff (if approved) on system maintenance
- Prepare 2027-2031 production scale proposal
  - Budget for processing full 261K corpus
  - Timeline: 3-4 year production phase
  - Staffing: Assess if 3-person team sufficient or need expansion
- Conduct annual retrospective
  - What worked well, what needs improvement
  - Validator feedback compilation
  - Technical debt assessment

**Output:** 2,000-3,000 documents processed

**Year 1 Total: 8,000-10,000 documents processed**

---

### Phase 3: 2027-2031 (Production Scale)

**Objective:** Process remaining 251,000 files over 4 years

**Annual Target:** 50,000-70,000 documents/year

**Sustained Operations:**
- Quarterly volunteer recruitment cycles (maintain 40-60 active validators)
- Quarterly AI model updates (cumulative training on 20K-40K corrections)
- Bi-annual technical reviews (system performance, optimization opportunities)
- Annual conference presentations (methodology papers, case studies)

**Integration Milestones:**
- **2027:** Complete Blue Force integration (all US unit logs in database)
- **2028:** Complete geospatial stack (all L7014 maps georeferenced)
- **2029:** Full spatial query capabilities (CDEC ↔ US ↔ Terrain)
- **2030:** Interactive web portal for researchers (public-facing if declassified)
- **2031:** Corpus processing complete, system transitioned to maintenance mode

---

## 7. Budget Summary

### Year 1 Budget (Detailed)

#### Personnel
| Role | Annual Compensation | Status |
|------|-------------------|---------|
| Lê Quang Tuệ (Project Lead) | $15,000 | ✅ Approved per LOI |
| Nguyễn Hoàng Vũ (Backend Engineer) | $15,000 | ⏳ Requesting approval |
| Đỗ Minh Phùng (System Architect) | $15,000 | ⏳ Requesting approval |
| **Subtotal Personnel** | **$45,000** | |

#### Hardware & Infrastructure
| Item | Quantity | Unit Cost | Total | Notes |
|------|----------|-----------|-------|-------|
| Mac Mini M4 Pro (48GB/1TB) | 2 | $1,999 | $3,998 | Production + Development |
| External SSD (2TB) | 1 | $180 | $180 | Backup storage |
| Thunderbolt cables | 2 | $30 | $60 | Connectivity |
| Claude Max subscription | 6 months | $60/mo | $360 | AI-assisted development |
| Gamification budget | - | - | $300 | Gift cards (Q1-Q2) |
| Contingency | - | - | $102 | Unexpected needs |
| **Subtotal Hardware** | | | **$5,000** | |

**Year 1 Total: $50,000**

---

### Years 2-3 Budget

#### Personnel (Annual)
| Role | Annual Compensation |
|------|-------------------|
| 3-person technical team | $45,000 |

#### Operating Costs (Annual)
| Item | Cost | Notes |
|------|------|-------|
| Electricity | $175 | 2× Mac Mini @ 50W avg |
| Maintenance | $200 | Thermal management, cleaning |
| Storage expansion | $100 | Additional backup drives |
| Software renewals | $100 | Subscriptions if needed |
| **Subtotal Operating** | **$575** | |

**Years 2-3 Annual: $45,575**

---

### 3-Year Total Cost of Ownership

| Category | Year 1 | Year 2 | Year 3 | Total |
|----------|--------|--------|--------|-------|
| **Personnel** | $45,000 | $45,000 | $45,000 | $135,000 |
| **Hardware** | $5,000 | - | - | $5,000 |
| **Operating** | - | $575 | $575 | $1,150 |
| **Subtotal** | $50,000 | $45,575 | $45,575 | $141,150 |
| **Resale (Mac Mini)** | - | - | -$2,400 | -$2,400 |
| **Net 3-Year TCO** | | | | **$138,750** |

---

### Budget Justification: Industry Comparison

**Academic ML Research Team (Typical):**
- Principal Investigator: $70,000-90,000
- Postdoc Researcher: $50,000-60,000
- PhD Students (2): $30,000-40,000 each
- Infrastructure (GPUs, cloud): $30,000-50,000
- **Total Annual:** $180,000-270,000
- **3-Year:** $540,000-810,000

**Commercial OCR Development:**
- ML Engineers (3): $120,000-180,000 each
- Infrastructure Engineer: $100,000-140,000
- Infrastructure (AWS, GPUs): $100,000-200,000
- **Total Annual:** $560,000-840,000
- **3-Year:** $1,680,000-2,520,000

**VWAI Proposal:**
- 3-person team: $45,000/year
- Infrastructure: $5,000 (one-time)
- **Total Annual:** $45,000-46,000
- **3-Year:** $138,750

**Our budget is 1/4 to 1/18 of comparable efforts**, justified by:
1. Humanitarian mission (team accepts significantly below-market compensation)
2. Leveraging open-source tools (TrOCR, KQNBSEI, Label Studio, PostgreSQL)
3. Local inference (no recurring cloud API costs)
4. Volunteer-powered validation (students contribute time for academic credit, certificates, letters of recommendation)
5. Academic partnership model (lower overhead than commercial)

**Despite the modest budget, the team's credentials are exceptional:**
- 13+ years combined geospatial systems experience
- Processed 10M+ POIs and 35M images at scale (larger datasets than CDEC corpus)
- Production ML deployment expertise
- Vietnamese linguistic and cultural expertise
- OpenStreetMap community leadership

---

## 8. Risk Assessment & Mitigation Strategies

### Technical Risks

#### Risk T1: Mac Mini Hardware Insufficient for >15 Concurrent Validators

**Probability:** Low  
**Impact:** Medium  
**Indicators:** CPU >90% sustained, inference latency >5 sec/page, validator complaints about lag

**Mitigation:**
- **Q1 pilot will reveal early** (within 2-3 weeks of volunteer onboarding)
- **Contingency Plan A:** Add 3rd Mac Mini ($2,000) in Q2 if load exceeds 15 users
- **Contingency Plan B:** Migrate ML inference to cloud GPU temporarily (RunPod, Lambda Labs ~$0.50/hr, $50-100/month)
- **Contingency Plan C:** Accelerate migration to TTU HPCC if bottleneck severe

**Budget Impact:** $2,000-3,000 additional if needed

---

#### Risk T2: TrOCR Accuracy Plateaus Below 70%

**Probability:** Medium  
**Impact:** High  
**Indicators:** AI accuracy not improving after 5,000 training samples, volunteer correction time not decreasing

**Mitigation:**
- **Try alternative models:** Donut, PaddleOCR, EasyOCR
- **Expand training data:** Continue HITL to gather 10,000-15,000 samples
- **Advanced preprocessing:** Implement super-resolution (e.g., Real-ESRGAN), advanced denoising
- **Architectural modifications:** Add diacritic-specific heads to TrOCR (requires ML research, may need consultant)
- **Fallback to commercial APIs:** Test Google Vision, Azure Computer Vision (may achieve 75-80% accuracy)

**Budget Impact:** $0 (try alternatives) to $10,000-15,000 (hire ML consultant for 1-2 months)

**Acceptance Criteria:** Even with 60-65% AI accuracy, HITL is still 2× faster than manual transcription, so project remains viable.

---

#### Risk T3: Microfilm Quality Too Degraded for Any OCR (<40% Accuracy)

**Probability:** Medium (affecting 10-20% of corpus)  
**Impact:** High  
**Indicators:** AI confidence consistently <30% even after preprocessing and fine-tuning, human validators report illegibility

**Mitigation:**
- **Advanced image restoration:**
  - Deep learning-based super-resolution (Real-ESRGAN, BasicSR)
  - GAN-based denoising (Noise2Noise)
  - Document binarization optimization (Sauvola, Wolf)
- **Flag worst documents for "manual-only"** processing (accept that 5-10% cannot be automated)
- **Request higher-quality rescans** from archives (if budget allows, ~$1-2/page = $50K-100K for worst 10%)

**Budget Impact:** $0 (software-based restoration) to $50K-100K (professional re-scanning)

**Acceptance Criteria:** If 10-15% of corpus requires manual-only, remaining 85-90% automated still represents massive time savings.

---

#### Risk T4: Database Corruption or Data Loss

**Probability:** Very Low  
**Impact:** Critical  
**Indicators:** PostgreSQL errors, disk failure, accidental deletion

**Mitigation:**
- **Daily automated backups** to external SSD (already budgeted)
- **Weekly backups** to TTU network storage (if available) or cloud (encrypted)
- **Monthly offsite backups** (encrypted cloud storage: Backblaze, AWS Glacier)
- **Test restore procedures** monthly to ensure backups are functional
- **Git version control** for all code and configuration files
- **PostgreSQL WAL (Write-Ahead Logging)** enabled for point-in-time recovery

**Budget Impact:** Already included ($180 external SSD, $10-20/month cloud storage)

---

### Operational Risks

#### Risk O1: Low Volunteer Retention (<50% Active After 4 Weeks)

**Probability:** Medium  
**Impact:** High  
**Indicators:** <5 active validators after Week 4, high churn rate (>50% dropout)

**Mitigation:**
- **Increase gamification rewards:** Larger gift cards ($25-50 vs. $10-25), more frequent prizes
- **Improve training materials:** Shorter orientation (30 min vs. 60 min), video tutorials, better onboarding
- **Reduce time commitment:** Lower minimum from 4 hrs/week to 2 hrs/week
- **Pair struggling validators with mentors:** Buddy system for first 2 weeks
- **Expand recruitment:** More universities, community organizations, Vietnamese diaspora
- **Offer academic credit:** Partner with professors to offer course credit for participation

**Budget Impact:** +$500 gamification budget (increase from $300 to $800), +20 hours recruitment time

---

#### Risk O2: Validator Quality Issues (Inter-Validator Agreement <80%)

**Probability:** Medium  
**Impact:** High  
**Indicators:** Multiple validators with <90% accuracy vs. expert review, frequent disagreements requiring tiebreakers

**Mitigation:**
- **Stricter screening:** Harder practice test (20 documents vs. 10, require 92% accuracy vs. 90%)
- **Intensive training:** 90-minute orientation vs. 60-minute, more practice exercises
- **Real-time feedback:** Immediate correction alerts ("This appears incorrect, please review")
- **Mentorship program:** Pair new validators with experienced validators for first 20 documents
- **Regular check-ins:** Weekly office hours, Slack/Discord support channel
- **Demotion/removal:** Validators with sustained <85% accuracy after 50 documents removed from active roster

**Budget Impact:** +10 hours/week oversight time (Phùng's responsibility)

---

#### Risk O3: Vietnamese University Partnerships Fall Through

**Probability:** Low  
**Impact:** High  
**Indicators:** TTU unable to establish MOUs with HCMC, Hanoi, FUV

**Mitigation:**
- **Backup recruitment channels:**
  - TTU local Vietnamese students (smaller pool but available)
  - Vietnamese community organizations in Texas (Dallas, Houston have large Vietnamese populations)
  - Remote volunteers (Vietnamese diaspora in California, Washington, etc.)
  - Post on Vietnamese student forums (Reddit r/VietNam, Facebook groups)
- **Paid validators:** If volunteer model fails, hire 3-5 part-time validators at $12-15/hr (~$15K-25K/year)

**Budget Impact:** $0 (backup volunteer channels) to $15K-25K (paid validators)

---

#### Risk O4: Volunteer Productivity Lower Than Projected (3-4 docs/hour vs. 6-10)

**Probability:** Low-Medium  
**Impact:** Medium  
**Indicators:** Week 1-2 shows volunteers averaging 5-6 min/doc even with 60% AI accuracy

**Mitigation:**
- **UX optimization:** Simplify Label Studio interface, reduce clicks required
- **Better preprocessing:** Improve image quality (dewarping, contrast) to make text more legible
- **Keyboard shortcuts:** Train validators on hotkeys for faster editing
- **Accept lower throughput:** Adjust Q1 target from 1,600-2,400 to 1,000-1,500 documents

**Budget Impact:** $0 (adjust expectations)

---

### Strategic Risks

#### Risk S1: VWAI Funding Cut Mid-Year

**Probability:** Very Low (project has Congressional mandate)  
**Impact:** Critical

**Mitigation:**
- **Q1 investment is minimal** ($16,250) and defensible even under budget cuts
- **Modular shutdown:** If funding cut, system can be suspended and restarted later (no data loss)
- **Deliverables prioritized:** Even if Year 2-3 canceled, Q1 pilot delivers immediate value (1,600+ docs processed, working system, volunteer network, conference presentation)

**Budget Impact:** Worst case, Q1 investment is complete sunk cost but still generates $20K-30K in value

---

#### Risk S2: Technical Staff Departures (Vũ or Phùng Leave Mid-Year)

**Probability:** Low-Medium  
**Impact:** Medium-High

**Mitigation:**
- **Documentation:** Comprehensive technical docs, SOPs, code comments ensure continuity
- **Cross-training:** All team members familiar with each other's work (pair programming, code reviews)
- **Backup hires:** Maintain list of qualified candidates from recruitment process
- **Phased onboarding:** New hire shadows departing member for 2-4 weeks before full handoff

**Budget Impact:** 2-4 weeks onboarding overlap (~$1,500-3,000 for temporary overlap period)

---

### Acceptance Criteria

The VWAI HITL system is considered **successful** if:

1. **Q1 Pilot:**
   - Process ≥1,000 documents (baseline target)
   - Achieve ≥60% time savings vs. manual baseline
   - Recruit and train ≥8 active validators
   - Demonstrate AI accuracy improvement (40% → ≥65%)

2. **Year 1:**
   - Process ≥6,000 documents
   - Achieve ≥65% AI accuracy (handwritten Vietnamese)
   - Maintain >80% volunteer retention
   - Present results at June 2026 conference

3. **3-Year:**
   - Process ≥25,000 documents (10% of corpus)
   - Achieve ≥75% AI accuracy
   - Establish sustainable volunteer network (30-50 active)
   - Position VWAI as methodological leader in AI-assisted archival research

**Even if only baseline targets are met, the project represents a 6-10× improvement over manual processing and justifies the investment.**

---

## 9. Compliance & Security

All proposed activities strictly adhere to the **VWAI Confidentiality, Ownership & Use Agreement** and TTU policies.

### Data Handling & Security (Per LOI Clause 4)

**"Code-to-Data" Model:**
- All data processing occurs on TTU-controlled infrastructure (Mac Mini workstations located on-site at TTU)
- No data leaves TTU network except through secure VPN access for validators
- Validators access Label Studio via VPN (TTU GlobalProtect or WireGuard) with TTU authentication
- No data download or export permitted for validators (view-only + edit corrections in-place)
- All sessions logged with timestamp, validator ID, IP address for audit trail

**Access Controls:**
- Multi-factor authentication (MFA) for all validator accounts
- Role-based permissions:
  - **Validator:** Can view assigned documents, edit transcriptions, submit corrections
  - **Reviewer:** Can access expert review queue, approve/reject flagged documents
  - **Admin:** Can manage users, view metrics, export data
- IP whitelisting for international validators (Vietnam universities)
- Automatic session timeout after 30 minutes inactivity
- Failed login alerts (>3 attempts triggers account lock)

**Audit Logging:**
- PostgreSQL logs all database transactions (who edited what, when)
- Label Studio logs all validation actions (document viewed, text edited, approved)
- Web server logs all HTTP requests (IP, timestamp, endpoint)
- Logs retained for 2 years, encrypted at rest

**Confidentiality Agreements:**
- All volunteers sign VWAI Confidentiality Agreement before receiving VPN credentials
- Agreement covers: No data download, no screenshots, no sharing information outside VWAI
- Violation results in immediate account termination and potential legal action

### Ownership & Intellectual Property (Per LOI Clause 5)

**Software Ownership:**
- All code developed for VWAI belongs to TTU
- Docker containers, scripts, and configuration files licensed under MIT or Apache 2.0 (open-source)
- Technical documentation (SOP, architecture diagrams) owned by TTU
- Can be shared with research community for reproducibility

**Volunteer Contributions:**
- Transcription corrections are work-for-hire, owned by TTU/VWAI
- Volunteers explicitly assign copyright to TTU in Confidentiality Agreement
- Volunteers may list contribution on resume/CV but cannot claim data ownership

**Third-Party Software:**
- All tools used are open-source (Label Studio, TrOCR, PostgreSQL, GDAL) or properly licensed (macOS, Claude Max)
- No proprietary software dependencies that could create licensing issues

### Data Backup & Recovery

**Backup Strategy:**
- **Daily:** Incremental backup to external SSD (2TB, encrypted, stored in locked cabinet)
- **Weekly:** Full backup to TTU network storage (if available) or encrypted cloud (Backblaze B2)
- **Monthly:** Offsite backup to encrypted cloud storage (AWS Glacier, low-cost)

**Recovery Testing:**
- Monthly restore drills to ensure backups are functional
- RTO (Recovery Time Objective): 4 hours (restore from daily backup)
- RPO (Recovery Point Objective): 24 hours (worst case, lose 1 day of work)

**Disaster Recovery Plan:**
- If Mac Mini #1 fails → Switch to Mac Mini #2 (production backup)
- If both fail → Order replacement, restore from backup within 72 hours
- If building fire/flood → Offsite cloud backup ensures data survives

---

## 10. Success Metrics

### Q1 Pilot Objectives (March 31, 2026)

**Quantitative Targets:**

| Metric | Target | Stretch Goal |
|--------|--------|--------------|
| Documents processed | 1,000+ | 1,600-2,400 |
| Time savings vs. manual | ≥60% | ≥67% |
| AI accuracy improvement | 40% → ≥65% | 40% → 70-75% |
| Active validators | ≥8 | 10-15 |
| Volunteer retention | ≥60% | ≥80% |
| Inter-validator agreement | ≥85% | ≥90% |
| Validator satisfaction | ≥4.0/5.0 | ≥4.5/5.0 |

**Qualitative Objectives:**
- ✅ HITL workflow validated as sustainable and scalable
- ✅ Volunteer network established with Vietnamese universities
- ✅ AI model improvement trajectory proven (continuous learning works)
- ✅ System can handle diverse document types (typed, handwritten, tables, maps)
- ✅ Docker containers portable to TTU HPCC (ready for production scaling)
- ✅ Conference-worthy results (June 2026 presentation)

---

### Year 1 Objectives (December 31, 2026)

**Quantitative Targets:**

| Metric | Target | Stretch Goal |
|--------|--------|--------------|
| Documents processed | ≥6,000 | 8,000-10,000 |
| AI accuracy (handwritten) | ≥65% | ≥70% |
| Active volunteers | 15-20 | 20-30 |
| Volunteer retention | ≥70% | ≥80% |
| Geospatial layers integrated | Blue Force (US logs) | Blue Force + L7014 maps |
| Auto-generated reports | 50+ | 100+ |
| Conference presentations | ≥1 | 2 (June + Fall) |

**Qualitative Objectives:**
- ✅ Position VWAI as methodological leader in AI-assisted archival research
- ✅ Establish sustainable volunteer network as ongoing community resource
- ✅ Deploy interactive geospatial viewer (IIIF + Allmaps)
- ✅ Demonstrate ROI to justify continued funding (Year 2-3)

---

### 2027-2031 Vision

**Long-Term Objectives:**
- Process **full 261K CDEC corpus** over 4 years
- Establish **VWAI Technical Framework** as reusable infrastructure for similar humanitarian projects
- Publish **methodological papers** on HITL for historical document processing in peer-reviewed journals
- Maintain **active volunteer network** (30-50 students) as sustainable community resource
- **Deliver closure** to Vietnamese families through identified grave locations and returned materials

**Legacy:**
- VWAI becomes **case study** in AI-assisted humanitarian research
- Technical framework **open-sourced** for other MIA accounting initiatives (Korean War, other conflicts)
- Volunteer model **replicated** by other digital humanities projects
- **Demonstrate** that AI + human collaboration can tackle "impossible" archival backlogs

---

## 11. Conference Presentation Plan (June 2026)

**Event:** Texas Tech University Vietnam Center Annual Conference  
**Co-organized by:** TTU + University of Social Sciences and Humanities (USSH), Vietnam National University, Hanoi  
**Date:** June 2026 (tentative)

**Presentation Title:**  
*"AI-Assisted Human-in-the-Loop Processing of Vietnamese Historical Military Documents: A Case Study in the Vietnam Wartime Accounting Initiative"*

**Presenter:** Đỗ Minh Phùng (primary) + Lê Quang Tuệ (co-presenter)  
**Duration:** 30-45 minutes + Q&A

**Presentation Structure:**

### I. Introduction & Motivation (5 min)
- VWAI humanitarian mission
- Scale of challenge: 261K files, 15-20 years manually
- Research question: Can AI + human collaboration make mission feasible?

### II. Methodology (10 min)
- **HITL Architecture Overview** (diagram)
- **AI Processing Layer:**
  - KQNBSEI for typed Vietnamese (97.7% accuracy)
  - TrOCR for handwritten Vietnamese (40% → 70-75% with fine-tuning)
  - Table Transformer for structured data
- **Human Validation Layer:**
  - Label Studio interface (screenshot demo)
  - Confidence-based routing (high/medium/low)
  - Cross-validation for critical fields
- **Continuous Learning Loop:**
  - Weekly model retraining on human corrections
  - Accuracy improvement trajectory (graph)

### III. Technical Implementation (5 min)
- **Infrastructure:**
  - Mac Mini M4 Pro (local inference, cost optimization)
  - PostgreSQL + PostGIS (geospatially-enabled database)
  - Docker containerization (reproducible deployment)
- **Team Composition:**
  - 3-person technical team (geospatial + backend + research)
  - Combined 13+ years geospatial systems experience
  - Vietnamese linguistic expertise

### IV. Results (10 min)
- **Q1 Pilot Metrics:**
  - Documents processed: X (bar chart)
  - Time savings: X% vs. manual baseline (comparison graph)
  - AI accuracy improvement: 40% → 70-75% (line chart over 12 weeks)
  - Volunteer engagement: X active validators, X% retention (pie chart)
- **Validator Feedback:**
  - Satisfaction scores (4.X/5.0)
  - Qualitative quotes: "AI drafts save so much time!", "Gamification is motivating"
- **Cost-Effectiveness:**
  - $3.47-4.63 per document vs. $64-90 manual (cost comparison table)
  - 3-year TCO: $138,750 vs. $450K manual (ROI chart)

### V. Discussion & Challenges (5 min)
- **What Worked Well:**
  - HITL dramatically reduces transcription time
  - Cross-validation ensures accuracy
  - Gamification sustains volunteer motivation
  - Continuous learning improves AI weekly
- **Challenges Encountered:**
  - Microfilm degradation (worst 10-15% documents barely legible)
  - Vietnamese tone marks on handwritten text (still challenging for AI)
  - Volunteer recruitment requires ongoing effort
- **Lessons Learned:**
  - Human-AI collaboration is key (neither alone is sufficient)
  - Domain expertise matters (geospatial + Vietnamese language critical)
  - Gamification is not frivolous (genuinely improves retention)

### VI. Future Work & Scaling (3 min)
- **Q2-Q4 2026 Plans:**
  - Expand volunteer network to 20-30 active
  - Integrate Blue Force data (US unit logs)
  - Deploy interactive geospatial viewer (Allmaps)
- **2027-2031 Vision:**
  - Process full 261K corpus over 4 years
  - 50,000-70,000 docs/year at scale
  - Establish VWAI Technical Framework as reusable infrastructure

### VII. Conclusion (2 min)
- **Impact:**
  - HITL makes "impossible" archival backlog feasible
  - AI + human collaboration is the future of digital humanities
  - Humanitarian AI: Technology serving families seeking closure
- **Call to Action:**
  - Encourage other MIA initiatives to adopt similar approaches
  - Invite collaboration on methodological improvements
  - Remind audience: Every document processed brings a family closer to answers

**Deliverables for Conference:**
- PowerPoint presentation (30-40 slides)
- Poster summarizing methodology and results
- Live demo of Label Studio interface (if internet available)
- Handout with QR code to GitHub repository (Docker containers, documentation)

---

## 12. Conclusion & Recommendation

The Vietnam Wartime Accounting Initiative stands at a critical juncture. With **261,000 CDEC files** representing **195,750 hours of manual processing** (94 person-years), the mission to bring closure to Vietnamese families cannot proceed at historical rates.

This proposal offers a **proven, cost-effective pathway** through AI-assisted human collaboration:

### The Solution in Summary

**Technical Approach:**
- AI generates "first draft" transcriptions (even at 50-60% accuracy, saves typing time)
- Vietnamese student volunteers verify and correct (faster than manual transcription)
- Corrections continuously improve AI (40% → 75% accuracy over Q1)
- Cross-validation ensures >95% accuracy for humanitarian work
- Gamification sustains volunteer engagement

**Team Composition:**
- **3-person technical team** with complementary expertise:
  1. Geospatial + AI strategy (You)
  2. Backend infrastructure + ML optimization (Vũ)
  3. System architecture + research (Phùng)
- Combined 13+ years geospatial systems experience
- Processed 10M+ POIs, 35M images (larger datasets than CDEC)
- Vietnamese linguistic and cultural expertise

**Investment:**
- **Year 1:** $50,000 ($45K personnel + $5K hardware)
- **Years 2-3:** $45,575/year
- **3-Year TCO:** $138,750

**Return on Investment:**
- **Q1 (3 months):** 1,600-2,400 docs, $20K-30K value, 1.2-1.8× ROI
- **Year 1:** 8,000-10,000 docs, $125K-175K value, 2.5-3.5× ROI
- **3-Year:** 30,000-40,000 docs, $500K-700K value, 3.6-5× ROI
- **Cost per document:** $3.47-4.63 (vs. $64-90 manual, 93% reduction)

### Why This Approach Works

**Realistic Expectations:**
- Acknowledges that Vietnamese handwritten OCR on microfilm is hard
- Sets achievable accuracy targets (70-75% not 95%)
- Embraces human-AI collaboration rather than full automation
- Accepts that 10-15% of corpus may require manual-only processing

**Exceptional Team:**
- Vũ's proven track record (reduced API costs 70% at Ahamove)
- Phùng's large-scale data operations experience (10M POIs, 35M images)
- Both are OpenStreetMap community leaders (geospatial credibility)
- Combined experience exceeds typical academic ML teams

**Modest Investment:**
- 1/4 to 1/18 of comparable academic or commercial efforts
- Justified by humanitarian mission, volunteer model, open-source tools
- Hardware: 2× Mac Mini ($4K) vs. GPU workstations ($10K-20K)

**Immediate Impact:**
- Q1 pilot delivers conference-worthy results by June 2026
- Aligns with LOI presentation requirement
- Demonstrates VWAI as methodological leader in AI-assisted archival research

### The Alternative Is Untenable

**Without AI automation:**
- 15-20 years to complete corpus (humanitarian timeline unacceptable)
- $450K-900K in personnel costs (vs. $139K with HITL)
- Linear scalability (hiring 10 staff is infeasible)

**Without this team:**
- Single-person execution of LOI responsibilities is mathematically impossible
- Lacks systems engineering expertise (Vũ's role)
- Lacks research rigor (Phùng's role)
- Misses June 2026 conference presentation opportunity

### This Proposal Fulfills the LOI Mandate

Per Letter of Invitation (7 January 2026), my role requires:

> *"Apply AI/LLM and computer vision tools to automate data extraction from handwritten, degraded, or complex archival materials"*

> *"Develop workflows to leverage artificial intelligence for pattern recognition, cross-referencing, and entity linking across large volumes of captured documents"*

**This proposal directly addresses these responsibilities** through:
- AI-assisted transcription (TrOCR, KQNBSEI)
- Workflow automation (HITL pipeline)
- Pattern recognition (confidence scoring, routing)
- Entity linking (geospatial cross-referencing)
- Large-scale processing (261K files)

**The 3-person team structure enables fulfilling these responsibilities within the one-year term.**

### Request for Approval

I respectfully request approval for:

1. **Personnel Expansion:**
   - Nguyễn Hoàng Vũ (Senior Software Engineer) @ $15,000/year
   - Đỗ Minh Phùng (System Architect) @ $15,000/year

2. **Hardware & Infrastructure:**
   - 2× Mac Mini M4 Pro (48GB/1TB) + peripherals = $4,238
   - Software subscriptions (Claude Max) = $360
   - Gamification budget = $300
   - Contingency = $102
   - **Total Hardware:** $5,000

**Year 1 Total: $50,000**

### Next Steps

If this proposal is approved:

1. **Immediately (Week 1):**
   - Finalize contracts with Vũ and Phùng
   - Procure hardware (Mac Mini × 2)
   - Set up development environment

2. **Week 2-3:**
   - Deploy Label Studio + ML backend
   - Integrate TrOCR and KQNBSEI models
   - Create validator training materials

3. **Week 3-4:**
   - Recruit 10-15 Vietnamese student volunteers
   - Conduct orientation sessions
   - Soft launch with 50-100 documents

4. **Week 5-12:**
   - Production processing (1,600-2,400 documents)
   - Weekly AI retraining
   - Metrics collection

5. **March 31, 2026:**
   - Deliver Q1 report with ROI analysis
   - Recommend Q2 scaling path
   - Submit conference presentation abstract

6. **June 2026:**
   - Present results at TTU Vietnam Center Annual Conference
   - Demonstrate VWAI as methodological leader

---

I am deeply committed to the VWAI mission and confident that this team can deliver transformational results. The proposal balances technical ambition with realistic expectations, leverages exceptional talent at modest cost, and positions VWAI to achieve its humanitarian objectives within meaningful timelines.

I welcome the opportunity to discuss this proposal in detail and address any questions about technical methodology, team composition, budget allocation, or implementation timeline.

**Respectfully submitted,**

**Lê Quang Tuệ**  
Document, Image & Geospatial Mapping Technician – AI/LLM Specialist  
Vietnam Wartime Accounting Initiative  
Texas Tech University

**Date:** January 29, 2026

---

## APPENDIX A: Technical Specifications

[Full technical appendix content from previous version - approximately 15 pages covering hardware specs, software stack, network architecture, data flow, AI model details, geospatial processing, Label Studio configuration, and performance benchmarks]

## APPENDIX B: AI Model Comparison & Selection Rationale

[Full model comparison appendix - approximately 8 pages covering Vietnamese OCR landscape, KQNBSEI deep dive, TrOCR fine-tuning strategy, confidence score interpretation]

## APPENDIX C: Volunteer Management & Gamification Details

[Full volunteer management appendix - approximately 6 pages covering recruitment strategy, onboarding procedures, gamification mechanics, quality assurance metrics]

## APPENDIX D: Infrastructure Deployment Guide

[Full deployment guide - approximately 10 pages covering Mac Mini setup (step-by-step), software installation, Label Studio deployment, TrOCR inference service, geospatial pipeline, VPN setup, testing procedures]

## APPENDIX E: Risk Register & Contingency Plans

[Full risk register - approximately 5 pages covering technical risks, operational risks, strategic risks, each with probability, impact, mitigation strategies, and budget implications]

**END OF MEMORANDUM**

---

**Attachments:**
1. Nguyễn Hoàng Vũ - Curriculum Vitae
2. Đỗ Minh Phùng - Curriculum Vitae
3. Letter of Invitation (7 January 2026) - Reference Document
4. KQNBSEI Model Documentation (Le, Tan & Bui, Marc, 2024)
5. Q1 Pilot Timeline (Gantt Chart)
6. Budget Breakdown Spreadsheet
7. Hardware Specifications Sheet (Mac Mini M4 Pro)
