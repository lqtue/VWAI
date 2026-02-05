# VWAI System Architecture - Visual Summary

## The Complete Pipeline (7 Phases)

```
                              VWAI INTELLIGENT PIPELINE
    ════════════════════════════════════════════════════════════════════

    DOCUMENT PROCESSING (Phases 1-4)          SEARCH & RESCUE (Phases 5-7)
    ────────────────────────────────          ────────────────────────────

    ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
    │  SCAN    │ → │   AI     │ → │  HUMAN   │ → │ KNOWLEDGE│ → │ PREDICT  │
    │  CDEC    │   │ PROCESS  │   │ VALIDATE │   │  GRAPH   │   │  WHERE   │
    │ Document │   │ TrOCR    │   │ Label    │   │ CIDOC    │   │ H3 Grid  │
    │          │   │ KQNBSEI  │   │ Studio   │   │ CRM      │   │ POC Map  │
    └──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘
         │              │              │              │              │
         │              │              │              │              ▼
         │              │              │              │        ┌──────────┐
         │              │              │              │        │  FIELD   │
         │              │              │              │        │  TEAMS   │
         │              │              │              │        │  + AR    │
         │              │              │              │        │ Tablets  │
         │              │              │              │        └──────────┘
         │              │              │              │              │
         │              ▼              │              │              │
         │    ┌─────────────────┐     │              │              │
         │    │ CONTINUOUS      │◄────┘              │              │
         │    │ AI LEARNING     │                    │              │
         │    │ (Weekly retrain)│                    │              │
         │    └─────────────────┘                    │              │
         │                                           │              │
         │                                           ▼              │
         │                                  ┌─────────────────┐     │
         │                                  │ BAYESIAN        │◄────┘
         │                                  │ FEEDBACK LOOP   │
         │                                  │ Found → Boost   │
         │                                  │ Empty → Redist. │
         │                                  └─────────────────┘
         │                                           │
         ▼                                           ▼
    ════════════════════════════════════════════════════════════════════
                            MISSION OUTCOME
                    ┌─────────────────────────────┐
                    │  FAMILIES RECEIVE CLOSURE   │
                    │  Remains Located & Returned │
                    └─────────────────────────────┘
```

## The Core Innovation: Event-Based Search

```
    TRADITIONAL APPROACH                    VWAI APPROACH
    ──────────────────────                  ─────────────────────

    "Search for a BODY"                     "Search for an EVENT"
    (Static object)                         (Reconstructed from evidence)

         ┌─────┐                                 ┌─────────┐
         │  ?  │                                 │  EVENT  │
         │ ??? │                                 │ (Death/ │
         │  ?  │                                 │ Burial) │
         └─────┘                                 └─────────┘
           │                                    ╱     │     ╲
           ▼                                   ╱      │      ╲
    Random search,                            ╱       │       ╲
    hope for luck                            ▼        ▼        ▼
                                        ┌──────┐ ┌──────┐ ┌──────┐
                                        │PERSON│ │PLACE │ │ TIME │
                                        │Name  │ │Coords│ │ Date │
                                        │Unit  │ │H3Cell│ │Season│
                                        └──────┘ └──────┘ └──────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ PROBABILITY MAP │
                                    │ "Dig HERE first"│
                                    └─────────────────┘
```

## H3 Probability Grid Visualization

```
    Search Area (Province Level)              Zoomed View (Search Zone)
    ─────────────────────────────            ─────────────────────────

    ┌─────────────────────────────┐          Each hexagon = ~100m
    │                             │          Color = Probability
    │    ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡      │
    │   ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡     │          ┌─────────────────────┐
    │    ⬡ ⬡ ⬢ ⬢ ⬢ ⬡ ⬡ ⬡ ⬡      │          │     ⬡   ⬡   ⬡      │
    │   ⬡ ⬡ ⬢ ⬢ ⬢ ⬢ ⬡ ⬡ ⬡ ⬡     │    →     │   ⬡  🔴  🟠   ⬡    │
    │    ⬡ ⬡ ⬢ ⬢ ⬡ ⬡ ⬡ ⬡ ⬡      │          │     🟠  🟡  ⬡      │
    │   ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡     │          │   ⬡   ⬡   ⬡   ⬡   │
    │    ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡ ⬡      │          └─────────────────────┘
    │                             │
    └─────────────────────────────┘          🔴 = 0.89 (Dig first!)
                                             🟠 = 0.67 (High priority)
    ⬢ = High probability cluster             🟡 = 0.45 (Medium)
    ⬡ = Low probability                      ⬡ = 0.10 (Low)
```

## Bayesian Feedback Loop

```
    FIELD RESULT                    SYSTEM RESPONSE
    ────────────                    ───────────────

    ┌───────────────┐              ┌────────────────────────────────────┐
    │ REMAINS FOUND │      →       │ BOOST neighboring cells            │
    │ in Cell A     │              │ (Possible mass grave / cemetery)   │
    └───────────────┘              │                                    │
                                   │   Before:        After:            │
                                   │   ⬡ ⬡ ⬡         ⬡ 🟠 ⬡           │
                                   │   ⬡ 🔴 ⬡    →   🟠 ✓ 🟠           │
                                   │   ⬡ ⬡ ⬡         ⬡ 🟠 ⬡           │
                                   └────────────────────────────────────┘

    ┌───────────────┐              ┌────────────────────────────────────┐
    │ CELL EMPTY    │      →       │ REDISTRIBUTE probability           │
    │ after digging │              │ to other candidate cells           │
    └───────────────┘              │                                    │
                                   │   Before:        After:            │
                                   │   🟠 🔴 🟠       🟠 ✗ 🟠           │
                                   │   🟡 🟡 🟡   →   🟠 🟠 🟠           │
                                   │   (scattered)    (concentrated)    │
                                   └────────────────────────────────────┘
```

## Team Structure & Expertise

```
    ┌─────────────────────────────────────────────────────────────────┐
    │                         VWAI TECH TEAM                          │
    ├─────────────────────────────────────────────────────────────────┤
    │                                                                 │
    │   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐          │
    │   │    TUỆ      │   │     VŨ      │   │    PHÙNG   │          │
    │   │   (Lead)    │   │  (Backend)  │   │ (Architect) │          │
    │   └──────┬──────┘   └──────┬──────┘   └──────┬──────┘          │
    │          │                 │                 │                  │
    │   ┌──────▼──────┐   ┌──────▼──────┐   ┌──────▼──────┐          │
    │   │ Geospatial  │   │ ML Deploy   │   │ System      │          │
    │   │ + AI/LLM    │   │ + Infra     │   │ Design      │          │
    │   │ + Coord     │   │ + Optimize  │   │ + Research  │          │
    │   └─────────────┘   └─────────────┘   └─────────────┘          │
    │                                                                 │
    │   Combined: 13+ years geospatial • 10M+ POIs processed         │
    │             Native Vietnamese • OSM community leaders           │
    └─────────────────────────────────────────────────────────────────┘
```

## ROI Summary

```
    INVESTMENT                              RETURN
    ──────────                              ──────

    Year 1: $50,000                         Year 1: 8,000-10,000 docs
    ┌────────────────┐                      ┌────────────────────────┐
    │ Personnel: $45K│                      │ Value: $125K-175K      │
    │ Hardware:  $5K │                      │ ROI: 2.5-3.5×          │
    └────────────────┘                      └────────────────────────┘

    3-Year: $139K                           3-Year: 30,000-40,000 docs
    ┌────────────────┐                      ┌────────────────────────┐
    │ Same team      │                      │ Value: $500K-700K      │
    │ + operating    │                      │ ROI: 3.6-5×            │
    └────────────────┘                      └────────────────────────┘

    COST PER DOCUMENT COMPARISON:
    ┌──────────────────────────────────────────────────────────────┐
    │ Manual:        ████████████████████████████████████  $65-90  │
    │ VWAI Pipeline: ███                                   $3-5    │
    └──────────────────────────────────────────────────────────────┘
                                                    93% cost reduction
```
