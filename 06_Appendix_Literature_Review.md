# APPENDIX F: Literature Review & Methodological Justification

## 1. Introduction

This review analyzes recent scholarship in Digital Humanities (DH) and Computer Vision to validate the VWAI technical approach. Key themes include the transition from digital archives to **Linked Open Data (LOD)**, the efficacy of **Large Language Models (LLMs)** for post-OCR correction, and pipeline optimizations for massive historical corpora.

## 2. Linked Open Data in Military History

**Reference:** Hyvönen, E. et al. (2016). *"WarSampo: Finnish WWII Linked Open Data"*. Semantic Web.

*   **Context:** The WarSampo project transformed scattered Finnish military records (casualties, units, places) into a unified Knowledge Graph.
*   **Key Insight:** Utilizing the **CIDOC CRM** ontology allows disparate datasets to be "linked" by semantic events (e.g., a "Battle" event links a "Unit," a "Place," and a "Date").
*   **VWAI Application:**
    *   VWAI will adopt the **CIDOC CRM** standard for the L2 Battle Dataset to ensure international interoperability.
    *   We will replicate WarSampo's success in using domain-specific ontologies (dictionaries of Vietnamese unit names and ranks) to post-process OCR output, correcting common errors (e.g., resolving ambiguous abbreviations).

## 3. Advanced OCR Correction with LLMs

**Reference:** Thomas, A. et al. (2024). *"CLOCR-C: Leveraging LLMs for Post-OCR Correction of Historical Newspapers"*. arXiv:2408.17428.

*   **Context:** Evaluating modern LLMs for correcting "noisy" OCR text from historical 19th-century newspapers.
*   **Key Findings:**
    *   **Claude 3 Opus** significantly outperformed GPT-4 and Llama models in restoring degraded historical text, effectively acting as an "expert restorer."
    *   Standard metrics (WER/CER) are insufficient; **Complex Named Entity Scorer (CoNES)** is better for assessing how well critical entities (names, dates) are preserved.
*   **VWAI Application:**
    *   We validate the decision to use **Claude** models for the "First Draft" generation where privacy allows.
    *   We will implement "Persona-Based Prompting" (e.g., "You are an expert Vietnamese archivist...") which the study proved most effective.

## 4. Synthetic Data for Low-Resource Languages

**Reference:**  (2024). *"Can Synthetic Corrupted OCR Data Improve Correction Models?"*. arXiv:2409.19735.

*   **Context:** Training correction models when real "Ground Truth" data is scarce.
*   **Key Insight:** Using **Markov processes** to synthetically generate "bad OCR" from clean text—mimicking specific error patterns like letter confusion—can effectively train models before real human validation data is available.
*   **VWAI Application:**
    *   **Risk Mitigation:** If volunteer recruitment is slow in Q1, VWAI will generate a synthetic dataset from modern Vietnamese military texts to pre-train the TrOCR/KQNBSEI models, ensuring the pipeline remains viable even with low initial human throughput.

## 5. Storage & Pipeline Optimization

**Reference:** Bourne, W. (2025). *"Reading the Unreadable: Holistic OCR Pipeline"*. Digital Scholarship in the Humanities.

*   **Context:** Processing mass-scale newspaper archives (NCSE dataset) with limited resources.
*   **Key Insight:** Converting greyscale/color scans to **2-bit binary PNGs** reduced storage requirements by **55%** with negligible impact on OCR quality.
*   **VWAI Application:**
    *   **Infrastructure Strategy:** Integrating this compression step into Phase 1 (Ingestion). This allows the 2.7 million page corpus to fit comfortably on the proposed Mac Mini local storage (2TB), saving thousands in cloud storage fees and maximizing hardware ROI.

## 6. Predictive Spatial Modeling for Search & Rescue

### H3 Hexagonal Indexing

**Reference:** Uber Engineering. *"H3: Uber's Hexagonal Hierarchical Spatial Index"*. 2018.

*   **Context:** Uber developed H3 for ride-matching optimization, requiring fast spatial queries with uniform distance properties.
*   **Key Innovation:** Hexagonal cells provide uniform adjacency (6 neighbors at equal distance), unlike square grids where diagonal neighbors are farther. The hierarchical structure allows seamless zoom from continental to meter-level precision.
*   **VWAI Application:**
    *   Resolution 9 (~100m cells) balances precision with manageable cell counts for provincial search areas.
    *   POC scoring per cell enables priority ranking for field teams.
    *   Hierarchical aggregation supports both strategic (district-level) and tactical (meter-level) search planning.

### Probability of Containment in Search & Rescue

**Reference:** Koester, R.J. (2008). *"Lost Person Behavior: A Search and Rescue Guide"*. dbS Productions.

*   **Context:** Standard reference for wilderness search and rescue, establishing the POC (Probability of Containment) framework.
*   **Key Insight:** POC represents the probability that a subject is within a defined search segment. Bayesian updates based on search results (found/not found) allow systematic refinement of search priorities.
*   **VWAI Application:**
    *   Adapt POC methodology from live SAR to historical remains search.
    *   Use archival evidence (documents, oral history) instead of behavioral profiles.
    *   Implement Bayesian feedback loop: empty excavations reduce cell POC and redistribute to alternatives.

### Taphonomic Modeling

**Reference:** Haglund, W.D. & Sorg, M.H. (1997). *"Forensic Taphonomy: The Postmortem Fate of Human Remains"*. CRC Press.

*   **Context:** Foundational text on how environmental factors affect remains over time.
*   **Key Factors:**
    *   **Slope/Erosion:** Remains on slopes >15° may move downhill over decades.
    *   **Soil Chemistry:** Acidic soils (pH <5.0) accelerate degradation; sandy/alkaline soils preserve better.
    *   **Water Action:** Flooding can transport remains significant distances.
*   **VWAI Application:**
    *   Integrate SRTM DEM data for slope analysis in Vietnam's varied terrain.
    *   Apply soil maps to weight preservation likelihood in POC calculations.
    *   Expand search cells to downslope neighbors for hillside burial locations.

---

## 7. Event-Based Historical Modeling

**Reference:** Hyvönen, E. et al. (2016). *"Semantic National Biography"* + *"WarSampo"*. Semantic Web Journal.

*   **Context:** The Finnish Semantic Computing Research Group pioneered event-centric modeling for historical data, treating historical facts as networks of interconnected events rather than isolated records.
*   **Key Pattern:** Instead of storing "Person X died at Location Y," model as:
    *   E69_Death event → P100_was_death_of → Person X
    *   E69_Death event → P7_took_place_at → Location Y
    *   E69_Death event → P4_has_time-span → Date Z
*   **VWAI Application:**
    *   The CDEC corpus describes thousands of "death/burial events"—modeling these as first-class entities (not just attributes of persons) enables:
        1. Linking multiple documents describing the same event.
        2. Detecting contradictions (two documents, different locations for same event).
        3. Inferring probable locations for undocumented events (unit movements → likely death locations).

---

## 8. Conclusion

The VWAI approach is not merely theoretical but grounded in the latest empirical success stories from the field. By synthesizing:

*   The **Linked Data** architecture of *WarSampo* for event-based knowledge graphs,
*   The **LLM correction methods** of *CLOCR-C* for AI-assisted transcription,
*   The **pipeline efficiencies** of *Bourne (2025)* for large-scale archival processing,
*   The **H3 spatial indexing** from Uber for efficient probability mapping,
*   The **POC framework** from search & rescue for prioritized field operations, and
*   The **taphonomic modeling** from forensic archaeology for remains displacement prediction,

this proposal represents a state-of-the-art, interdisciplinary methodology for humanitarian digital archival and remains recovery.
