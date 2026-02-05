# APPENDIX A: Technical System Architecture

## 1. System Overview

The VWAI intelligent pipeline employs a **Human-in-the-Loop (HITL)** architecture that combines the speed of Artificial Intelligence with the precision of human judgment.

**Figure A.1: The 7-Phase Processing Pipeline**

The pipeline has been extended from 5 phases to 7 phases to incorporate the **Search & Rescue Methodology** using Knowledge Graphs and Predictive Spatial Modeling.

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
└──────────────────────────────────────────────────────────────────┘
                            ↓
┌──────────────────────────────────────────────────────────────────┐
│ Phase 5: Knowledge Graph Construction ("Digital Brain")         │
├──────────────────────────────────────────────────────────────────┤
│ EVENT-BASED MODEL: Search for "Death/Burial Events" not bodies  │
│                                                                  │
│ CIDOC CRM Entity Linking:                                        │
│ • Person (Name, Rank, Unit) ←→ Event (Battle, Burial)           │
│ • Event ←→ Place (Coordinates, Terrain, H3 Cell)                │
│ • Event ←→ Time (Date, Season, Time of Day)                     │
│                                                                  │
│ Ingest Heterogeneous Data:                                       │
│ • Text: Battle reports, burial lists, soldier diaries           │
│ • Oral History: Veteran/local transcripts → fuzzy spatial query │
│ • Maps: Georeferenced wartime maps (Indian 1960 → WGS84)        │
│                                                                  │
│ Infer Missing Links:                                             │
│ • If soldier ∈ Unit X AND Unit X @ Location Y on Date Z         │
│   → soldier has HIGH probability @ Location Y                    │
└──────────────────────────────────────────────────────────────────┘
                            ↓
┌──────────────────────────────────────────────────────────────────┐
│ Phase 6: Predictive Spatial Modeling ("Probability Map")        │
├──────────────────────────────────────────────────────────────────┤
│ H3 HEXAGONAL INDEXING (Resolution 9 = ~100m precision):         │
│ • Divide search area into H3 cells                               │
│ • Assign Probability of Containment (POC) score to each cell    │
│                                                                  │
│ POC Attribute Scoring:                                           │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ Direct Evidence   │ "Document says burial at X,Y" → HIGH   │ │
│ │ Contextual        │ "Unit retreated along river" → MEDIUM  │ │
│ │ Negative Evidence │ "Area bombed/bulldozed 1972" → LOWER   │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                  │
│ TAPHONOMIC ANALYSIS (50-year remains movement):                  │
│ • DEM slope/erosion → Expand search to downslope neighbors      │
│ • Soil acidity mapping → Flag preservation likelihood           │
│ • Flooding history → Model sediment transport vectors           │
│                                                                  │
│ Output: Heatmap of H3 cells ranked by search priority           │
└──────────────────────────────────────────────────────────────────┘
                            ↓
┌──────────────────────────────────────────────────────────────────┐
│ Phase 7: Field Operations & Bayesian Feedback Loop              │
├──────────────────────────────────────────────────────────────────┤
│ PRIORITIZED SEARCH ZONES:                                        │
│ • Field teams start at "Red Cells" (highest POC)                │
│ • AR tablets overlay historical maps on GPS position            │
│ • Display: "1968 trench line was HERE" relative to current pos  │
│                                                                  │
│ BAYESIAN FEEDBACK LOOP:                                          │
│ ┌────────────────────────────────────────────────────────────┐ │
│ │ IF remains FOUND in Cell A:                                 │ │
│ │   → BOOST probability of neighboring cells                  │ │
│ │   → Flag potential mass grave / field cemetery              │ │
│ │                                                              │ │
│ │ IF Cell A excavated and EMPTY:                              │ │
│ │   → Update KG: "Event did NOT occur here"                   │ │
│ │   → REDISTRIBUTE probability to other candidate cells       │ │
│ └────────────────────────────────────────────────────────────┘ │
│                                                                  │
│ Auto-generate Investigation Case Report (docxtpl):               │
│ • Pre-filled with transcriptions, maps, cross-references        │
│ • Interactive web viewer (IIIF + Allmaps)                       │
└──────────────────────────────────────────────────────────────────┘
```

**Figure A.2: Complete Workflow Summary**

```
┌─────────────┐   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
│  DIGITIZE   │ → │   CONNECT   │ → │   PREDICT   │ → │   VERIFY    │
│ Paper→Text  │   │ Build KG    │   │ H3+Terrain  │   │ Excavate    │
└─────────────┘   └─────────────┘   └─────────────┘   └─────────────┘
                                                              ↓
                                                      ┌─────────────┐
                                                      │    LEARN    │
                                                      │ Feed Back   │
                                                      └─────────────┘
```

---

## 2. AI Processing Layer Components

### A. Handwritten Vietnamese Text (TrOCR)
*   **Model:** TrOCR (Transformer-based Optical Character Recognition) by Microsoft.
*   **Architecture:** Vision Encoder + Text Decoder (558M parameters).
*   **Implementation:**
    *   Base model fine-tuned on CDEC handwritten samples.
    *   **Confidence Scoring:** Outputs softmax probabilities (0-100%) for each character.
    *   **Performance:** ~2-3 seconds/page inference on Mac Mini M4 Pro (Neural Engine).
    *   **Trajectory:** Initial accuracy ~40-50% → Targets 70-75% after fine-tuning on 5,000+ corrections.

### B. Typed Vietnamese Text (KQNBSEI)
*   **Model:** KQNBSEI (Kieu Quoc Nu Be Sắc Em I) HTR Model.
*   **Framework:** Kraken (22MB lightweight model).
*   **Source:** Le, Tan & Bui, Marc (2024), EPHE-PSL University.
*   **Capabilities:**
    *   Specialized for 19th-20th century French-Vietnamese printed documents.
    *   **97.7% accuracy** on Vietnamese tone marks (combining diacritics).
    *   Extremely fast batch processing (0.02 sec/page).
*   **Application:** Administrative headers, typed summaries, official correspondence.

### C. Structured Data (Table Transformer)
*   **Model:** DETR (DEtection TRansformer) architecture.
*   **Application:** Unit rosters, casualty lists, radio logs.
*   **Function:** Detects table structures, rows, and columns to extract data into CSV/JSON formats rather than unstructured block text.

---

## 3. Human-in-the-Loop (HITL) Validation

### Label Studio Interface
We utilize **Label Studio** (Open Source Enterprise) as the validation frontend:
*   **Split-Screen View:** Original microfilm scan on left, editable text on right.
*   **Color-Coded Confidence:**
    *   **Green (>90%):** High confidence (quick scan).
    *   **Yellow (70-90%):** Medium confidence (detailed check).
    *   **Red (<70%):** Low confidence (requires careful reading).
*   **One-Click Correction:** Validators click low-confidence words to type corrections.

### Intelligent Routing & Quality Control
The system dynamically assigns review buffers based on AI confidence:
1.  **Single Review:** High confidence (>90%) regions without critical data.
2.  **Double Review (Cross-Validation):**
    *   Low confidence (<70%) regions.
    *   **Critical Fields:** All Coordinates, Personal Names, and Unit IDs are *always* double-blind verified.
3.  **Consensus Algorithm:**
    *   If Validator A == Validator B → **Approve**.
    *   If Validator A != Validator B → **Route to Senior Validator** (Tiebreaker).

---

## 4. Geospatial Integration Stack

### Coordinate Standardisation
*   **Input:** Wartime MGRS (Military Grid Reference System), often reliant on Indian 1960 Datum or Everest Spheroid.
*   **Transformation:** Automated conversion pipeline to modern **WGS84** (GPS standard) using GDAL/PROJ libraries with custom datum shift parameters for Vietnam-era grids.

### Knowledge Graph Schema (WarSampo Alignment)
*   **Standard:** **CIDOC CRM** (Conceptual Reference Model) ontology.
*   **Interoperability:** Aligns with the *WarSampo* (Finnish WWII) project standards to ensure data can be linked with international semantic web initiatives.
*   **Application:** Events (Battles) link Actors (Units/People) and Places (Coordinates) in a graph structure, enabling complex "6-degree" queries impossible in flat tables.

### Storage Optimization (Infrastructure)
*   **Strategy:** **2-Bit Binary PNG Conversion** (per *Bourne, 2025*).
*   **Benefit:** Reduces file size by ~55% vs. standard greyscale, facilitating the storage of the 2.7M page corpus on local Mac Mini hardware without sacrificing OCR accuracy.

### Database & Visualization
*   **PostGIS:** The industry-standard open-source geospatial database stores all extracted events (battles, burials, sightings) as geometric objects.
*   **IIIF + Allmaps:**
    *   **IIIF (International Image Interoperability Framework):** Serves high-resolution historical maps as tiled assets.
    *   **Allmaps:** A browser-based viewer that warps these historical maps onto modern satellite basemaps (Google/ESRI) in real-time, allowing transparency sliders to compare "then and now" terrain features.

---

## 5. Knowledge Graph Construction (Phase 5)

### Conceptual Framework: The Event-Based Search Model

**Core Innovation:** Instead of searching for a "body" (a static object), the system searches for the **"Death/Burial Event"** in space and time. A martyr's remains are physical evidence of a historical event—to find the remains, we must reconstruct the event using a Knowledge Graph.

### CIDOC CRM Ontology Implementation

Following the **WarSampo** (Finnish WWII) framework, VWAI implements the **CIDOC CRM** (Conceptual Reference Model) ontology:

```
┌─────────────────────────────────────────────────────────────────────┐
│                     CIDOC CRM Entity Structure                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌──────────┐     participatedIn     ┌──────────────┐              │
│  │  PERSON  │ ───────────────────→ │    EVENT     │              │
│  │  (E21)   │                        │    (E5)      │              │
│  └──────────┘                        └──────────────┘              │
│       │                                    │                        │
│       │ hasName                            │ tookPlaceAt            │
│       ↓                                    ↓                        │
│  ┌──────────┐                        ┌──────────────┐              │
│  │  Name    │                        │    PLACE     │              │
│  │ (E41)    │                        │    (E53)     │              │
│  └──────────┘                        └──────────────┘              │
│       │                                    │                        │
│       │ memberOf                           │ hasH3Index             │
│       ↓                                    ↓                        │
│  ┌──────────┐                        ┌──────────────┐              │
│  │  UNIT    │                        │   H3 CELL    │              │
│  │ (E74)    │                        │   (Custom)   │              │
│  └──────────┘                        └──────────────┘              │
│                                                                     │
│  Event Types: E6 (Battle), E69 (Death), E9 (Burial/Movement)       │
└─────────────────────────────────────────────────────────────────────┘
```

### Data Ingestion Pipeline

| Source Type | Processing | Output |
|-------------|------------|--------|
| **Text Documents** | TrOCR/KQNBSEI → NER extraction | Person, Unit, Date, Location entities |
| **Oral History** | Transcription → Fuzzy spatial parsing | "Near the big banyan tree" → candidate H3 cells |
| **Wartime Maps** | Georeferencing (Indian 1960 → WGS84) | Spatial features with modern coordinates |
| **US Unit Logs** | Structured parsing from NARA | Blue Force trajectories for cross-reference |

### Link Inference Engine

The Knowledge Graph enables inference of missing links:

```python
# Pseudocode: Unit Membership Inference
IF soldier.unit == "Unit X"
   AND UnitMovement.find(unit="Unit X", date="1968-03-15", location="Hill 881")
   THEN soldier.probable_location = "Hill 881" with confidence 0.85

# Document Contradiction Detection
IF Document_A.burial_location == "Hill 881"
   AND Document_B.burial_location == "Khe Sanh Village"
   AND Document_A.subject == Document_B.subject
   THEN flag_for_expert_review(documents=[A, B], reason="Location Contradiction")
```

---

## 6. H3 Hexagonal Indexing & Probability Scoring (Phase 6)

### H3 Grid System

**Technology:** Uber's H3 geospatial indexing system
**Resolution:** Level 9 (~100m edge length, ~0.1 km² area per cell)
**Coverage:** Provincial-level search areas divided into hexagonal cells

**Why Hexagons?**
*   Uniform distance to all neighbors (unlike square grids)
*   Better approximation of circular search radii
*   Hierarchical aggregation (zoom in/out seamlessly)

### Probability of Containment (POC) Scoring

Each H3 cell receives a POC score (0.0 - 1.0) based on evidence weighting:

| Evidence Type | Weight | Example |
|---------------|--------|---------|
| **Direct** | 0.8 - 1.0 | "Document states burial at coordinates X,Y" |
| **Contextual** | 0.4 - 0.7 | "Unit retreated along this river valley" |
| **Inferred** | 0.2 - 0.5 | "Soldier was in Unit X which was in area Y" |
| **Oral History** | 0.3 - 0.6 | "Veteran says near the old temple" (fuzzy match) |
| **Negative** | -0.2 to -0.5 | "Area heavily bombed/bulldozed post-war" |

**POC Calculation Formula:**

```
POC(cell) = Σ(evidence_weight × confidence × recency_factor) / normalization_factor

Where:
- evidence_weight: Type-based weight from table above
- confidence: Document/source reliability (0-1)
- recency_factor: Time decay for older oral histories
- normalization_factor: Ensures POC ∈ [0, 1]
```

### PostGIS Schema Extension

```sql
-- H3 Cell Table with POC scoring
CREATE TABLE search_cells (
    h3_index VARCHAR(15) PRIMARY KEY,  -- H3 cell identifier
    geometry GEOMETRY(POLYGON, 4326),  -- Cell boundary
    poc_score DECIMAL(4,3) DEFAULT 0,  -- Probability 0.000-1.000
    evidence_count INTEGER DEFAULT 0,
    last_updated TIMESTAMP,
    search_status VARCHAR(20) DEFAULT 'pending'  -- pending/excavated/found
);

-- Evidence linkage to cells
CREATE TABLE cell_evidence (
    id SERIAL PRIMARY KEY,
    h3_index VARCHAR(15) REFERENCES search_cells(h3_index),
    document_id INTEGER REFERENCES documents(id),
    evidence_type VARCHAR(20),  -- direct/contextual/inferred/oral/negative
    weight DECIMAL(3,2),
    notes TEXT
);

-- Spatial index for fast queries
CREATE INDEX idx_cells_geometry ON search_cells USING GIST(geometry);
CREATE INDEX idx_cells_poc ON search_cells(poc_score DESC);
```

---

## 7. Taphonomic Analysis Module

### Purpose

Model how remains may have moved over 50+ years due to natural processes, adjusting search cell probabilities accordingly.

### Data Sources

| Dataset | Resolution | Application |
|---------|------------|-------------|
| **SRTM DEM** | 30m | Slope analysis, flow direction |
| **Soil Maps** | Variable | Acidity, preservation likelihood |
| **Flood History** | Event-based | Sediment transport modeling |
| **Land Use Change** | Decadal | Post-war development impacts |

### Slope & Erosion Modeling

```python
# Pseudocode: Taphonomic probability redistribution
def adjust_for_erosion(cell, dem_raster):
    slope = calculate_slope(cell.centroid, dem_raster)
    flow_direction = calculate_flow_direction(cell.centroid, dem_raster)

    if slope > 15:  # degrees
        # Remains may have moved downslope
        downslope_neighbors = get_downslope_cells(cell, flow_direction)
        for neighbor in downslope_neighbors:
            neighbor.poc_score += cell.poc_score * 0.3 * (slope / 45)
        cell.poc_score *= 0.7  # Reduce original cell probability

    return cell
```

### Soil Preservation Factors

| Soil Type | pH Range | Preservation | POC Modifier |
|-----------|----------|--------------|--------------|
| Sandy/Alluvial | 6.5-7.5 | Excellent | ×1.2 |
| Laterite | 4.5-5.5 | Poor | ×0.6 |
| Clay | 6.0-7.0 | Good | ×1.0 |
| Peat/Acidic | 3.5-4.5 | Very Poor | ×0.3 |

---

## 8. Field Operations Interface (Phase 7)

### AR Tablet Application

**Platform:** iPad/Android tablet with GPS and cellular connectivity
**Framework:** MapLibre GL + AR overlay

**Features:**
1. **Historical Map Overlay:** View 1968 L7014 topographic maps warped onto current satellite imagery
2. **H3 Heatmap Display:** Color-coded cells by POC score (Red = High, Blue = Low)
3. **Trench/Feature Visualization:** "The 1968 trench line was HERE" relative to current GPS position
4. **Offline Mode:** Pre-cached tiles for areas without cellular coverage

### Field Data Collection

```
┌─────────────────────────────────────────────────────────────────────┐
│                    FIELD TABLET INTERFACE                           │
├─────────────────────────────────────────────────────────────────────┤
│  ┌───────────────────────────────────────────────────────────────┐ │
│  │                     [MAP VIEW]                                 │ │
│  │   ┌─────┐ ┌─────┐ ┌─────┐                                     │ │
│  │   │ RED │ │ORG  │ │ YLW │  ← H3 cells colored by POC         │ │
│  │   │0.89 │ │0.67 │ │0.45 │                                     │ │
│  │   └─────┘ └─────┘ └─────┘                                     │ │
│  │         ◉ ← Your GPS position                                 │ │
│  │   - - - - - - - - - ← 1968 trench line overlay               │ │
│  │                                                                │ │
│  └───────────────────────────────────────────────────────────────┘ │
│                                                                     │
│  Cell: 8928308280fffff    POC: 0.89    Status: [▼ Pending]        │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │ Field Notes:                                                 │   │
│  │ ________________________________________________            │   │
│  │ ________________________________________________            │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  [ EXCAVATED - EMPTY ]  [ REMAINS FOUND ]  [ FLAG ISSUE ]          │
└─────────────────────────────────────────────────────────────────────┘
```

### Bayesian Feedback Algorithm

When field results are recorded, the system updates probabilities:

```python
def bayesian_update(cell, result):
    """
    Update POC scores based on field excavation results.

    Args:
        cell: H3 cell that was excavated
        result: 'found' | 'empty' | 'partial'
    """
    neighbors = get_adjacent_cells(cell.h3_index)

    if result == 'found':
        cell.search_status = 'found'
        # Boost neighboring cells (potential mass grave/cemetery)
        for neighbor in neighbors:
            neighbor.poc_score = min(1.0, neighbor.poc_score * 1.5)
        log_event("Remains found", cell, "Investigate neighbors for additional burials")

    elif result == 'empty':
        cell.search_status = 'excavated'
        cell.poc_score = 0.0
        # Redistribute probability to remaining candidate cells
        total_remaining = sum(c.poc_score for c in get_all_pending_cells())
        redistribution = cell.original_poc / total_remaining
        for candidate in get_all_pending_cells():
            candidate.poc_score += candidate.poc_score * redistribution
        log_event("Cell empty", cell, "Probability redistributed")

    elif result == 'partial':
        cell.search_status = 'partial'
        # Evidence of activity but no remains - moderate boost to neighbors
        for neighbor in neighbors:
            neighbor.poc_score = min(1.0, neighbor.poc_score * 1.2)
        log_event("Partial evidence", cell, "Continue search in area")

    # Trigger model recalculation
    recalculate_search_priorities()
```

### Search Optimization Output

The system generates a prioritized excavation list:

| Rank | H3 Cell | POC Score | Evidence Summary | Terrain Notes |
|------|---------|-----------|------------------|---------------|
| 1 | 8928308280fffff | 0.89 | Direct burial record + oral history | Flat, sandy soil |
| 2 | 8928308281fffff | 0.76 | Unit position + inferred | Moderate slope |
| 3 | 8928308282fffff | 0.67 | Contextual only | Near river, possible movement |
| ... | ... | ... | ... | ... |

---

## 9. Technology Stack Summary

### Software Components (Extended)

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Database** | PostgreSQL 16 + PostGIS 3.4 | Geospatial storage, H3 indexing |
| **H3 Library** | h3-py (Python) / pg-h3 (PostGIS) | Hexagonal grid operations |
| **DEM Processing** | GDAL + rasterio | Taphonomic analysis |
| **Knowledge Graph** | Neo4j or PostgreSQL JSON | CIDOC CRM entity linking |
| **Field App** | React Native + MapLibre GL | AR tablet interface |
| **Visualization** | IIIF + Allmaps + Deck.gl | Web-based heatmaps |

### Data Flow Architecture

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   CDEC      │    │  Knowledge  │    │    H3       │    │   Field     │
│  Documents  │ →  │   Graph     │ →  │  POC Grid   │ →  │   Teams     │
│  (TrOCR)    │    │  (CIDOC)    │    │  (PostGIS)  │    │  (Tablets)  │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────────┘
       ↑                                                        │
       │                    FEEDBACK LOOP                       │
       └────────────────────────────────────────────────────────┘
```
