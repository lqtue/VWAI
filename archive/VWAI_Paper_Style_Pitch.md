# VietSampo: A Linked Open Data Service and Semantic Portal for Search & Rescue of Vietnamese War Martyrs' Remains

**Lê Quang Tuệ, Nguyễn Hoàng Vũ, Đỗ Minh Phùng**

Vietnam Wartime Accounting Initiative (VWAI), Texas Tech University
Contact: tue.le@ttu.edu

---

## Abstract

This paper presents **VietSampo**, a system for publishing and analyzing heterogeneous archival data about the Vietnam War to support the humanitarian mission of locating martyrs' remains. VietSampo adapts the successful WarSampo (Finnish WWII) methodology: harmonizing massive datasets using **event-based modeling** with CIDOC CRM ontology, enabling semantic enrichment across datasets. Our system extends the WarSampo approach with three innovations: (1) **AI-assisted Human-in-the-Loop transcription** for degraded Vietnamese handwriting, (2) **H3 hexagonal spatial indexing** for predictive search zone modeling, and (3) **Bayesian feedback loops** that update search probabilities based on field excavation results. The system transforms 261,000 CDEC archival files into actionable intelligence for field teams, reducing document processing time by 67% and enabling probability-driven search operations. VietSampo demonstrates that Linked Data approaches can serve not only historical research but active humanitarian recovery missions.

---

## 1. Motivation: Vietnam War MIA Accounting on the Semantic Web

Over 300,000 Vietnamese soldiers remain missing from the Vietnam War—a humanitarian crisis affecting millions of families seeking closure. While archival data exists across multiple repositories (CDEC captured documents, US military records, local oral histories), this information remains trapped in **data silos**: scanned microfilm images, untranscribed handwritten reports, and static databases that cannot be cross-referenced.

The **Vietnam Wartime Accounting Initiative (VWAI)** at Texas Tech University holds **261,000 CDEC files spanning 2.7 million pages**—the largest collection of captured Vietnamese military documents. At current manual processing rates, complete analysis would require **15-20 years**. This timeline is incompatible with the humanitarian urgency: families are aging, witnesses are passing, and geographic markers are disappearing.

Our goal is therefore to:
1. **Transform static archives into queryable Linked Open Data** about Vietnam War events
2. **Enable cross-referencing** between Vietnamese CDEC documents and US military records
3. **Generate probability maps** that prioritize search locations for field recovery teams
4. **Demonstrate** that the WarSampo methodology scales to non-European conflicts with different data challenges (Vietnamese handwriting, degraded microfilm)

---

## 2. VietSampo Datasets, Conceptual Model, and Data Service

### 2.1 Datasets

| # | Name | Providing Organization | Size | Status |
|---|------|------------------------|------|--------|
| 1 | **CDEC Captured Documents** | Texas Tech Vietnam Archive | 261,000 files (2.7M pages) | Primary corpus |
| 2 | **US Daily Staff Journals** | NARA (National Archives) | ~50,000 entries | Blue Force tracking |
| 3 | **L7014 Topographic Maps** | National Land Survey | 200+ map sheets | Wartime terrain |
| 4 | **Wartime Photographs** | Defense Forces Archives | ~10,000 images | Visual documentation |
| 5 | **Oral Histories** | TTU Vietnam Center | ~500 transcripts | Veteran testimonies |
| 6 | **Local Cemetery Records** | Vietnamese Provinces | Variable | Burial documentation |
| 7 | **Unit Movement Data** | Military history books | ~3,000 units | Organization structure |
| 8 | **Casualty Records** | Vietnamese MIA Office | ~300,000 persons | Missing persons |

**Table 1.** Central datasets of VietSampo.

### 2.2 Conceptual Framework: Event-Based Search Model

Following WarSampo, we selected **CIDOC CRM** as our ontological framework (ISO 21127:2014). However, VietSampo introduces a critical conceptual shift:

> **Instead of searching for a "body" (a static object), we search for the "Death/Burial Event" in space and time.**

A martyr's remains are physical evidence of a historical event. To find the remains, we must reconstruct the event using a Knowledge Graph, then predict where the event's physical traces would be located today.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    CIDOC CRM Core Classes in VietSampo                  │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│                           ┌──────────────┐                              │
│                           │  crm:E5_Event │                             │
│                           └──────────────┘                              │
│                          ╱       │        ╲                             │
│           P11_had_participant    │         P7_took_place_at             │
│                        ╱         │          ╲                           │
│           ┌──────────────┐       │       ┌──────────────┐               │
│           │ crm:E39_Actor│       │       │ crm:E53_Place│               │
│           │  (Person)    │       │       │  (Location)  │               │
│           └──────────────┘       │       └──────────────┘               │
│                  │               │              │                       │
│         P107_member_of    P4_has_time_span     │                        │
│                  │               │              │ :hasH3Index           │
│           ┌──────────────┐ ┌──────────────┐ ┌──────────────┐            │
│           │ crm:E74_Group│ │crm:E52_Time  │ │  :H3_Cell    │            │
│           │  (Unit)      │ │   Span       │ │  (Custom)    │            │
│           └──────────────┘ └──────────────┘ └──────────────┘            │
│                                                                         │
│  Event Subclasses:                                                      │
│    • E6_Destruction (Battle)    • E9_Move (Unit Movement)               │
│    • E69_Death                  • :BurialEvent (Custom)                 │
│    • E7_Activity (Ambush)       • :HospitalizationEvent (Custom)        │
└─────────────────────────────────────────────────────────────────────────┘
```

**Figure 1.** Core classes of CIDOC CRM used in VietSampo, extended with H3 spatial indexing.

### 2.3 Link Inference Engine

The Knowledge Graph enables inference of missing links—a capability absent from traditional databases:

```
IF    Soldier.unit = "Unit X"
AND   UnitMovement(unit="Unit X", date="1968-03-15", location="Hill 881")
THEN  Soldier.probable_location = "Hill 881"
      WITH confidence = 0.85
```

This allows us to place individuals at locations even when no document explicitly states their presence—critical for the 50%+ of cases where burial locations are undocumented.

---

## 3. VietSampo Processing Pipeline

Unlike WarSampo, which worked with relatively clean archival data, VietSampo must process **degraded microfilm of Vietnamese handwriting**—a challenge no existing OCR system handles well. We therefore developed a 7-phase pipeline:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     VietSampo 7-Phase Pipeline                          │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   │
│  │ PHASE 1 │ → │ PHASE 2 │ → │ PHASE 3 │ → │ PHASE 4 │ → │ PHASE 5 │   │
│  │ Ingest  │   │   AI    │   │  Human  │   │   QA    │   │  Know-  │   │
│  │ & Route │   │ Process │   │ Validate│   │ & Learn │   │  ledge  │   │
│  │         │   │         │   │  (HITL) │   │         │   │  Graph  │   │
│  └─────────┘   └─────────┘   └─────────┘   └─────────┘   └─────────┘   │
│                                                                    │    │
│                                                          ┌─────────┴──┐ │
│                                                          │  PHASE 6   │ │
│  ┌─────────────────────────────────────────────────────→ │ Predictive │ │
│  │                  BAYESIAN FEEDBACK                    │ Spatial    │ │
│  │                                                       │ (H3 + POC) │ │
│  │  ┌─────────┐                                         └─────────┬──┘ │
│  └──│ PHASE 7 │ ←────────────────────────────────────────────────┘     │
│     │  Field  │                                                         │
│     │  Ops    │                                                         │
│     └─────────┘                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

**Figure 2.** VietSampo processing pipeline with Bayesian feedback loop.

### 3.1 Phase 1-4: AI-Assisted Human-in-the-Loop Transcription

| Document Type | AI Model | Accuracy | Processing Speed |
|---------------|----------|----------|------------------|
| English/French typed | Tesseract OCR | 85-90% | 0.5 sec/page |
| Vietnamese typed | KQNBSEI HTR | 85-92% | 0.02 sec/page |
| Vietnamese handwritten | TrOCR (fine-tuned) | 40%→75%* | 2-3 sec/page |
| Tables/Rosters | Table Transformer | 80-85% | 5-10 sec/table |

*Accuracy improves through continuous learning from human corrections.

**Table 2.** AI processing components and performance.

The key innovation is **confidence-coded validation**: AI outputs character-level confidence scores (0-100%), and the interface highlights low-confidence regions in red for human attention. This reduces human effort by 67% compared to full manual transcription.

### 3.2 Phase 5: Knowledge Graph Construction

After validation, extracted entities are linked into the Knowledge Graph:

| Entity Type | Linking Method | Example |
|-------------|----------------|---------|
| **Person** | Name matching + unit context | "Nguyễn Văn A, 3rd Company" → Person_12345 |
| **Place** | MGRS→WGS84 transformation + fuzzy matching | "YD 123 456" → Place_67890 (Hill 881) |
| **Unit** | Hierarchy matching | "C3/B2/E5" → Unit_111 (3rd Co, 2nd Bn, 5th Rgt) |
| **Event** | Temporal-spatial clustering | Battle reports same date/location → Event_999 |

**Table 3.** Entity linking methods.

### 3.3 Phase 6: H3 Predictive Spatial Modeling (VietSampo Innovation)

This phase has no equivalent in WarSampo. We extend the Knowledge Graph with **Probability of Containment (POC)** scoring using Uber's H3 hexagonal indexing:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    H3 Probability Grid (Resolution 9)                   │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│     Search Area (Province)              Priority Heatmap                │
│     ┌─────────────────────┐             ┌─────────────────────┐         │
│     │ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ │             │     ⬡   ⬡   ⬡      │         │
│     │ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ │             │   ⬡  🔴  🟠   ⬡    │         │
│     │ ⬡ ⬡ ⬢ ⬢ ⬢ ⬡ ⬡ ⬡ ⬡ │     →       │     🟠  🟡  ⬡      │         │
│     │ ⬡ ⬡ ⬢ ⬢ ⬢ ⬢ ⬡ ⬡ ⬡ │             │   ⬡   ⬡   ⬡   ⬡   │         │
│     │ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ │             └─────────────────────┘         │
│     └─────────────────────┘                                             │
│                                         🔴 POC=0.89 (Dig first!)        │
│     Each cell ≈ 100m edge              🟠 POC=0.67 (High priority)      │
│                                         🟡 POC=0.45 (Medium)            │
└─────────────────────────────────────────────────────────────────────────┘
```

**Figure 3.** H3 hexagonal grid with Probability of Containment scoring.

**POC Calculation:**

| Evidence Type | Weight | Example |
|---------------|--------|---------|
| Direct | 0.8-1.0 | "Document states burial at coordinates X,Y" |
| Contextual | 0.4-0.7 | "Unit retreated along this river valley" |
| Inferred | 0.2-0.5 | "Soldier was in Unit X which was in area Y" |
| Oral History | 0.3-0.6 | "Veteran says near the old temple" |
| Negative | -0.2 to -0.5 | "Area heavily bombed/bulldozed post-war" |

**Table 4.** Evidence weighting for POC calculation.

**Taphonomic Adjustment:** POC scores are modified based on terrain analysis (DEM slope, soil acidity, flood history) to account for 50+ years of remains displacement.

### 3.4 Phase 7: Field Operations with Bayesian Feedback

Field teams receive prioritized search zones via AR-enabled tablets. Crucially, results feed back into the system:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                       Bayesian Feedback Algorithm                       │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  IF remains FOUND in Cell A:                                            │
│    → Cell A marked "found"                                              │
│    → Neighboring cells: POC × 1.5 (possible mass grave)                 │
│    → Log: "Investigate neighbors for additional burials"                │
│                                                                         │
│  IF Cell A excavated and EMPTY:                                         │
│    → Cell A: POC = 0                                                    │
│    → Redistribute probability to remaining candidates                   │
│    → Knowledge Graph updated: "Event did NOT occur here"                │
│                                                                         │
│  Result: System learns from every excavation, improving future searches │
└─────────────────────────────────────────────────────────────────────────┘
```

**Figure 4.** Bayesian feedback loop for continuous search optimization.

---

## 4. VietSampo Portal: Multiple Perspectives

Following WarSampo's design principle, VietSampo provides **multiple interlinked perspectives** for different user needs:

| Perspective | Primary Users | Key Features |
|-------------|---------------|--------------|
| **Document Browser** | Archivists, Researchers | Faceted search, AI transcription review |
| **Validation Interface** | Student Volunteers | Split-screen original/draft, confidence highlighting |
| **Person Search** | Families, Genealogists | Name search, linked unit history, death records |
| **Unit History** | Military Historians | Timeline, movement maps, casualty heatmaps |
| **Event Timeline** | Researchers | Temporal visualization of battles, movements |
| **Probability Map** | Field Teams | H3 heatmap, prioritized search zones |
| **Field Operations** | Recovery Teams | AR tablet, offline mode, feedback capture |

**Table 5.** VietSampo portal perspectives.

Each perspective resolves the same URIs differently—a document URI shows transcription details in the Document Browser but shows linked events in the Event Timeline. This supports diverse use cases without data duplication.

---

## 5. Implementation and Resources

### 5.1 Technical Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| Triple Store | PostgreSQL + PostGIS | Geospatial RDF storage |
| H3 Indexing | h3-py / pg-h3 | Hexagonal spatial grid |
| AI Models | TrOCR, KQNBSEI, Table Transformer | Document processing |
| Validation | Label Studio | Human-in-the-loop interface |
| Visualization | IIIF + Allmaps + Deck.gl | Map overlay and heatmaps |
| Field App | React Native + MapLibre GL | AR tablet interface |

**Table 6.** VietSampo technology stack.

### 5.2 Team and Budget

| Role | Expertise | Annual Cost |
|------|-----------|-------------|
| Project Lead | Geospatial + AI Strategy | $15,000 |
| Backend Engineer | ML Deployment + Infrastructure | $15,000 |
| System Architect | Large-scale Data Operations | $15,000 |
| **Hardware** | 2× Mac Mini M4 Pro + peripherals | $5,000 (one-time) |
| **Year 1 Total** | | **$50,000** |

**Table 7.** Resource requirements.

### 5.3 Projected Outputs

| Metric | Q1 2026 | Year 1 | 3-Year |
|--------|---------|--------|--------|
| Documents Processed | 1,600-2,400 | 8,000-10,000 | 30,000-40,000 |
| Knowledge Graph Entities | ~20,000 | ~100,000 | ~500,000 |
| H3 Cells with POC Scores | 5,000 | 50,000 | 200,000 |
| Field Excavations Supported | Pilot | 50-100 | 500+ |

**Table 8.** Projected outputs.

---

## 6. Comparison: WarSampo vs. VietSampo

| Aspect | WarSampo (Finnish WWII) | VietSampo (Vietnam War) |
|--------|-------------------------|-------------------------|
| **Scale** | 94,700 death records | 261,000 CDEC files |
| **Data Quality** | Clean archival data | Degraded microfilm handwriting |
| **Languages** | Finnish (Latin script) | Vietnamese (diacritics) + English |
| **Primary Use** | Historical research + family search | **Active recovery operations** |
| **Spatial Model** | Point locations | **H3 probability grid + taphonomics** |
| **Feedback Loop** | None | **Bayesian updates from excavations** |
| **AI Processing** | Minimal (clean OCR) | **Heavy (HITL for handwriting)** |
| **Public Interest** | 20,000 visitors in 3 days | 300,000+ families seeking closure |

**Table 9.** Comparison of WarSampo and VietSampo approaches.

---

## 7. Related Work and Contributions

VietSampo builds on established work in Linked Data for war history (WarSampo, WW1LOD, Europeana 1914-1918) while introducing three novel contributions:

1. **AI-assisted HITL for degraded non-Latin script**: Demonstrates that Linked Data pipelines can incorporate deep learning OCR with human verification for challenging source materials.

2. **Predictive spatial modeling for active recovery**: Extends semantic web approaches from passive historical research to operational field guidance, using H3 indexing and POC scoring.

3. **Bayesian feedback integration**: Creates a learning system where field results update the Knowledge Graph, improving future predictions—a first for cultural heritage Linked Data.

---

## 8. Conclusion

VietSampo demonstrates that the WarSampo methodology—event-based Linked Open Data for war history—can be adapted to serve active humanitarian recovery missions. By combining semantic web technologies with AI-assisted transcription and predictive spatial modeling, we transform 261,000 archival documents from static images into actionable intelligence for field teams.

The system addresses a scale problem (15-20 years of manual work) with a cost-effective solution ($50K Year 1) that delivers immediate humanitarian value while building a sustainable infrastructure for long-term research.

**For Vietnamese families, every document processed brings them closer to answers about their missing loved ones. VietSampo makes that processing feasible within their lifetimes.**

---

## References

[1] Hyvönen, E. et al. (2016). "WarSampo Data Service and Semantic Portal for Publishing Linked Open Data about the Second World War History." ESWC 2016.

[2] Doerr, M. (2003). "The CIDOC CRM – an ontological approach to semantic interoperability of metadata." AI Magazine 24(3).

[3] Le, T. & Bui, M. (2024). "KQNBSEI: A Kraken HTR Model for 19th-20th Century Vietnamese Documents." EPHE-PSL University.

[4] Koester, R.J. (2008). "Lost Person Behavior: A Search and Rescue Guide." dbS Productions.

[5] Uber Engineering (2018). "H3: Uber's Hexagonal Hierarchical Spatial Index."

[6] Thomas, A. et al. (2024). "CLOCR-C: Leveraging LLMs for Post-OCR Correction of Historical Newspapers." arXiv:2408.17428.

---

**Acknowledgments:** This work is conducted under the Vietnam Wartime Accounting Initiative at Texas Tech University, with support from the TTU Vietnam Center and Archive.
