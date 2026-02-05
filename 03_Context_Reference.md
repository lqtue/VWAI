# VWAI Project Context Reference

## Mission
Build an intelligence system to locate MIA/KIA burial sites by matching Red Force (Vietnamese) and Blue Force (US/Allied) military records, creating a Battle Dataset, and performing geospatial inference.

## The Pipeline (4 Layers)

```
CDEC Docs (Red Force) + NARA Logs (Blue Force) + Terrain Data
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ L1: DATA EXTRACTION                                         │
│ • OCR: TrOCR (handwritten), KQNBSEI (typed Vietnamese)      │
│ • NER: Unit IDs, Dates, MGRS Coords, Personal Names         │
│ • HITL: Label Studio + Vietnamese student validators        │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ L2: BATTLE DATASET                                          │
│ • Spatiotemporal matching: Red Force ↔ Blue Force           │
│ • Event records: engagements, ambushes, operations          │
│ • Storage: PostGIS (geospatially-enabled)                   │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ L3: PROBABILISTIC INFERENCE                                 │
│ • P(soldier X in unit Y participated in battle Z)           │
│ • P(killed | participated) based on casualty rates          │
│ • Burial zone estimation: buffer 1-5km around engagement    │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│ L4: SEARCH AREA PRIORITIZATION                              │
│ • Subtract low-probability terrain (rivers, cliffs, roads)  │
│ • Rank polygons by confidence score                         │
│ • Output: prioritized search zones for field teams          │
└─────────────────────────────────────────────────────────────┘
```

## Data Sources

| Source | Content | Volume |
|--------|---------|--------|
| **CDEC** (TTU) | Vietnamese field reports, casualty lists, unit diaries | 261K files, 2.7M pages |
| **NARA** | US Daily Staff Journals (Blue Force tracking) | TBD |
| **L7014** | 1:50k topographic maps (requires georeferencing) | Series coverage |

## Team

| Role | Person | Expertise | Status |
|------|--------|-----------|--------|
| Project Lead / Geospatial | Lê Quang Tuệ | GIS, AI/LLM, cartography | ✅ Approved |
| Backend / ML Infra | Nguyễn Hoàng Vũ | 5yr Ahamove, self-hosted geo DBs, 70% API cost reduction | ⏳ Pending |
| System Arch / Research | Đỗ Minh Phùng | Be Group (10M POIs), Grab, VinUni researcher, OSM admin | ⏳ Pending |

## Tech Stack

- **OCR:** TrOCR (handwritten, fine-tunable), KQNBSEI/Kraken (typed Vietnamese 97.7%)
- **Validation:** Label Studio (HITL), gamification for volunteer retention
- **Database:** PostgreSQL + PostGIS
- **Geospatial:** GDAL, QGIS, IIIF + Allmaps (interactive viewer)
- **Infra:** 2x Mac Mini M4 Pro (48GB), Docker, local inference

## Budget Summary

| Category | Amount |
|----------|--------|
| Personnel (3 × $15k) | $45,000 |
| Variable (hardware, cloud, consulting) | $15,000 |
| **Total Request** | **$60,000** |

## Timeline (Realistic)

| Phase | When | Goal |
|-------|------|------|
| Setup | Feb-Mar 2026 | Infrastructure + baseline testing |
| Pilot | Apr-May 2026 | Process 500-800 docs |
| Demo | Jun 2026 | TTU Vietnam Center conference |

## Full VWAI Team Structure

| Role | Members | Count |
|------|---------|-------|
| **Co-PIs (TTU-VWAI)** | Dr. Stephen Maxner (VNCA), Dr. Ron Milam (IPAC), Dr. Alex-Thái Đình Võ | 3 |
| **Historical Research & Analysis** | Ls. Tạ Thu Phong, Ts. Phạm Hồng Hà, Lê Đức Anh, Trần Nguyễn Phương Thảo, Trần Công Đại, Ts. Hoàng Cẩm Thanh | 6 |
| **Fulbright University Leaders** | Ts. Nguyễn Thành Trung, Ts. Vũ Minh Hoàng | 2 |
| **USSH Hanoi Leaders** | Ts. Nguyễn Hữu Mạnh, Ts. Nguyễn Hồng Duy, ThS. Lương Ngọc Vinh | 3 |
| **Student Research Specialists** | From USSH, Huế, Fulbright | 18 |
| **Oral History Specialists** | ThS. Nguyễn Thùy Linh, Nguyễn Thị Thanh Thủy, Đặng Hoàng Hải, Lam Nguyễn | 4 |
| **Community & Family Coordinators** | Nguyễn Thị Thanh Thủy, ĐT Đặng Vương Hưng | 2 |
| **Digital Communications** | Nguyễn Nhật Thủy Tiên | 1 |
| **Report Editors** | Nguyễn Thị Kim Hoa, ThS. Trang Bùi, Lam Nguyễn, Phó Đỗ Quyên | 4 |
| **QC: Military History & GIS** | Lâm Hồng Tiên, Nguyễn Xuân Thắng | 2 |
| **NARA Liaison** | Ts. Erik Villard | 1 |
| **Tech Team (AI/LLM)** | Lê Quang Tuệ ✅, Nguyễn Hoàng Vũ ⏳, Đỗ Minh Phùng ⏳ | 1→3 |

## Tech Team ↔ Existing Team Integration

```
EXISTING TEAM                          TECH TEAM ENABLES
─────────────────────────────────────────────────────────────────
Historical Research & Analysis (6)  ←  L2: Battle Dataset queries
                                       "Show all events near X coord"
                                       Auto-generated 70% pre-filled reports

Student Research Specialists (18)   →  L1: HITL Validators
                                       Verify AI transcriptions via Label Studio
                                       Gamification badges & leaderboards

QC: Military History & GIS (2)      ←  L3-L4: Probabilistic output
                                       Validate burial zone predictions
                                       Cross-check coordinate transforms

Report Editors (4)                  ←  Auto-generated templates (docxtpl)
                                       Embedded maps from IIIF/Allmaps

Oral History Specialists (4)        ←  Cross-reference interview data with
                                       Battle Dataset for corroboration
```

## Key Insight

> OCR is just Layer 1. The real value is the **Battle Dataset** (L2) that enables **probabilistic inference** (L3) to predict where soldiers likely died and were buried, outputting **prioritized search zones** (L4) for field recovery teams.

## Folder Structure

```
VWAI/
├── README.md                        ← Start here
├── 01_One_Pager.md                  ← 2 min summary
├── 02_Project_Control.md            ← Full proposal
├── 03_Context_Reference.md          ← This file
├── 04-06_Appendix_*.md              ← Technical deep-dives
│
├── team/                            ← CVs
├── reference/                       ← Academic papers
├── ttu-docs/                        ← Official documents
├── maps/                            ← Sample L7014 map
└── archive/                         ← Old versions
```
