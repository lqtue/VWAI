# Methodology: Search & Rescue of Martyrs’ Remains using Knowledge Graphs & Mapping

## **Methodology: Search & Rescue of Martyrs’ Remains using Knowledge Graphs & Mapping**
This methodology integrates **forensic archaeology**, **digital humanities (WarSampo framework)**, and **predictive spatial modeling** to optimize the search for remains that have been missing for 50+ years. It moves from passive archival storage to active, probability-driven search.
## **1\. Conceptual Framework: The "Event-Based" Search Model**
Instead of searching for a "body" (a static object), you search for the **"Death/Burial Event"** in space and time.
*   **Core Concept**: A martyr’s remains are physical evidence of a historical event. To find the remains, you must reconstruct the event using a Knowledge Graph (KG).
*   **Data Structure**: Use the **CIDOC CRM** ontology (like WarSampo) to link:
    *   **Person**: (Name, Rank, Unit)
    *   **Event**: (Battle, Ambush, Hospitalization, Burial)
    *   **Place**: (Coordinates, Terrain Type, H3 Cell)
    *   **Time**: (Date, Season, Time of Day)
## **2\. Phase 1: Knowledge Graph Construction (The "Digital Brain")**
Before any digging, you build a "digital twin" of the battlefield.
*   **Ingest Heterogeneous Data**:
    *   **Text**: Battle reports, burial lists, soldier diaries (use LLM/OCR to extract entities).
    *   **Oral History**: Transcripts from veterans or locals (extract "near the big banyan tree" → fuzzy spatial query).
    *   **Maps**: Georeferenced wartime maps (Indian 1960 datum) converted to modern WGS84.
*   **Link Entities**: Connect a "Casualty Record" in a US archive to a "Death Notice" in a Vietnamese local cemetery. The KG identifies contradictions (e.g., "Document A says Hill 881, Document B says Khe Sanh village").
*   **Infer Missing Links**: If a soldier belongs to _Unit X_ and _Unit X_ was at _Location Y_ on _Date Z_, the soldier has a high probability of being at _Location Y_, even if no specific document places him there.
## **3\. Phase 2: Predictive Spatial Modeling (The "Probability Map")**
Use the KG to feed a spatial probability model (Probability of Containment - POC).
*   **H3 Indexing as a Common Grid**:
    *   Divide the search area (e.g., a province) into H3 hexagonal cells (e.g., resolution 9 for ~100m precision).
    *   **Attribute Scoring**: Assign a "Probability Score" to each cell based on KG evidence.
        *   _Direct Evidence_: "Document says burial at coords X,Y" → **High Score** for that cell.
        *   _Contextual Evidence_: "Unit retreated along this river valley" → **Medium Score** for cells along the river.
        *   _Negative Evidence_: "Area heavily bombed/bulldozed in 1972" → **Lower Score** (remains likely destroyed or moved).
*   **Terrain & Flow Analysis**:
    *   Use Digital Elevation Models (DEM) to simulate **taphonomic processes** (how bodies move over 50 years).
    *   _Slope/Erosion_: If a burial was on a steep slope, remains may have slid downhill. The model expands the "search cell" to neighbor cells downslope.
    *   _Soil Acidity_: Flag cells with highly acidic soil (remains likely degraded) vs. sandy soil (better preservation).
## **4\. Phase 3: Field Operations Optimization (The "Smart Search")**
*   **Prioritized Search Zones**: The system outputs a "Heatmap" of H3 cells. Field teams don't search random spots; they start at "Red Cells" (highest probability).
*   **Augmented Reality (AR) in Field**: Field teams use tablets showing the H3 grid overlaid on reality. They can see: "Here was the 1968 trench line" (from georeferenced maps) relative to their current GPS position.
*   **Feedback Loop**:
    *   _Found_: If remains are found in Cell A, the model boosts the probability of neighboring cells (likely a mass grave or field cemetery).
    *   _Empty_: If Cell A is fully excavated and empty, the model updates the KG ("Event did NOT occur here") and redistributes probability to other candidate cells (Bayesian update).
## **5\. Summary Workflow**
![](https://t90182366948.p.clickup-attachments.com/t90182366948/dcec916d-cb87-44a4-8529-cb2a784726bd/image.png)
1. **Digitize**: Convert old paper into digital text/maps.
2. **Connect**: Build the Knowledge Graph to find logical connections.
3. **Predict**: Use H3 + Terrain data to guess _where_ the event footprint is today.
4. **Verify**: Dig in the high-probability hexagons.
5. **Learn**: Feed results back to improve the next prediction.