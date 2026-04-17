# HISD Summer School Professional Development Survey Analysis

**Author:** Jennifer McNew
**Tools:** Power BI · Microsoft Fabric · DAX · Power Query
**Dataset:** 1,149 survey responses · 31 facilitators · 27 campuses · Summer 2025

---

## Project Overview

This project analyzes survey feedback from Houston ISD's Summer School Professional Development program. Participants rated PD sessions across five dimensions — Clarity, Value, Engagement, Confidence, and Preparedness — and the goal was to surface actionable insights for program improvement.

The analysis identifies performance patterns across grade level bands, content categories, and individual facilitators, and concludes with concrete, data-driven recommendations for the district's next PD cycle.

**Key result:** District average feedback score of 4.62 / 5.0, with meaningful variation across subgroups that point to specific areas for targeted improvement.

---

## Dataset

| Field | Detail |
|---|---|
| Source | Summer School PD Session Survey (internal HISD data) |
| Responses | 1,149 |
| Columns | 15 (including 5 Likert-scale rating dimensions) |
| Facilitators | 31 |
| Campuses | 27 |
| Grade Level Bands | Elementary (56%) · High (27%) · Middle (16%) |
| Session Types | G1–G2, G3–G8, G9–G12, GK Curriculum, PreK Training |

---

## Tools & Methodology

**Platform:** Power BI Web via Microsoft Fabric (browser-based; no Desktop access)

### Data Preparation (Power Query)
- Imported raw CSV and performed column renaming and data type validation
- Standardized 164 unique free-text `Content Focus` entries into 10 meaningful content categories using a DAX calculated column
- Created shortened session name labels for improved visual clarity in charts

### DAX Logic
The `Content Focus` field was collected as free-text, resulting in 164 unique entries representing the same 10 subject areas. A DAX `SWITCH(TRUE(), CONTAINSSTRING(...))` formula was written to map all entries to standardized categories:

```dax
Content Category =
SWITCH(
    TRUE(),
    CONTAINSSTRING([Content Focus], "Math"),           "Mathematics",
    CONTAINSSTRING([Content Focus], "ELAR")
        || CONTAINSSTRING([Content Focus], "English")
        || CONTAINSSTRING([Content Focus], "Reading"),  "ELAR/English",
    CONTAINSSTRING([Content Focus], "Science"),        "Science",
    CONTAINSSTRING([Content Focus], "Social Studies"), "Social Studies",
    CONTAINSSTRING([Content Focus], "Special Ed"),     "Special Education",
    CONTAINSSTRING([Content Focus], "PreK"),           "PreK Training",
    CONTAINSSTRING([Content Focus], "summer school"),  "Summer School General",
    "Other"
)
```

`CONTAINSSTRING` is case-insensitive, catching variations like "grade 4 math," "TEKS Math Review," and "math camp prep" under a single category. Conditions are ordered from most specific to least specific, since `SWITCH(TRUE())` returns on the first match.

### Dashboard
Built an interactive 3-page Power BI report with slicers for:
- Grade Level Band
- Facilitator
- Content Category

![Dashboard Preview](dashboard_preview.png)

This allows stakeholders — including principals and PD coordinators — to explore findings for their specific context without any technical knowledge required.

---

## Key Findings

### Finding 1 — Grade Level Disparities
High school participants consistently rated PD sessions lower than elementary and middle school participants across both overall feedback and preparedness scores. The 0.13-point gap between middle and high school suggests high school teachers may have more specialized PD needs that current sessions are not fully addressing.

> **Recommendation:** Develop differentiated PD content specifically designed for high school instructional contexts.

### Finding 2 — Content Category Performance
A 0.28-point spread exists across content categories, indicating meaningful variation in PD quality and relevance by subject area.

| Content Category | Avg Score |
|---|---|
| Summer School General | 4.73 |
| PreK Training | 4.70 |
| Mathematics | ~4.65 |
| Social Studies | 4.45 ⚠️ |
| Special Education | 4.51 ⚠️ |

> **Recommendation:** Review and redesign Social Studies and Special Education PD content. Use Summer School General sessions as a model for lower-performing areas.

### Finding 3 — Facilitator Performance & Context
Top performers (minimum 10 participants): Kathy (4.74 · 129 participants), Angel (4.68 · 107 participants), Angela (4.67 · 70 participants).

One facilitator — Juan — led 22% of all sessions (257 participants), primarily at the high school grade band. His preparedness score of 4.52 is consistent with the district-wide pattern of lower scores at the high school level, suggesting his results reflect the structural challenge of high school PD rather than individual facilitation quality.

> **Recommendation:** When evaluating facilitator performance, disaggregate scores by grade level band to ensure fair comparison. Facilitators working primarily with high school teachers should be benchmarked against other high school facilitators, not the district-wide average.

### Finding 4 — Session-Level Analysis
G9–G12 Curriculum sessions scored lowest on preparedness (4.54), confirming that the high school pattern exists at the curriculum design level — not only at the facilitator level.

> **Recommendation:** Redesign G9–G12 curriculum content and review facilitator assignments for high school sessions.

---

## Data Quality

### Strengths
- **100% field completion** across all 1,149 responses for all required fields (Facilitator, Campus Name, Key Lesson and Implementation, and all five rating dimensions)
- High completion rate reflects effective survey design and strong participant engagement

### Opportunities
The `Content Focus` field was collected as free-text, resulting in 164 unique entries that required significant post-collection standardization.

**Recommendation — Dual-field approach for future cycles:**
- `Content Category` (dropdown) — standardized categories for consistent reporting
- `Exact Course Title` (free-text) — preserves ability to drill down into individual course performance

This approach reduces analytical burden, enables more granular reporting, and eliminates the need for manual categorization after data collection.

---

## Challenges & Solutions

| Challenge | Solution |
|---|---|
| No Power BI Desktop access on Mac | Adapted entire workflow to Power BI Web via Microsoft Fabric, including alternative DAX syntax and manual column management |
| University tenant account defaulted to Viewing mode | Identified and resolved via Editing toggle in the report interface |
| `LargeModelMovedAcrossRegions` infrastructure error (HTTP 400) | Disabled large semantic model storage format setting and rebuilt the semantic model |
| 164 unique free-text `Content Focus` entries | Wrote custom DAX `SWITCH(TRUE(), CONTAINSSTRING(...))` formula to categorize into 10 standardized groups |
| Apparent blank values in slicers | Investigated via DAX queries — confirmed as Power BI display artifacts, not missing data. All required fields 100% complete. |

---

## Recommendations Summary

**Grade Level & Content**
- Differentiate PD for high school teachers — they consistently score lower on both feedback (4.56) and preparedness (4.54)
- Redesign Social Studies and Special Education PD content
- Use Summer School General and PreK sessions as design models for lower-performing areas

**Facilitator Evaluation**
- Implement coaching and support for lowest-performing facilitators (minimum 10-participant threshold for fair comparison)
- Control for grade level when evaluating facilitator performance — do not compare high school facilitators to the district-wide average
- Monitor quality vs. quantity tradeoffs for facilitators with disproportionately high session loads

**Data Collection**
- Restructure `Content Focus` as a dual-field (dropdown + free-text) before the next PD cycle
- Redesign G9–G12 curriculum content at the program level, not just the facilitator level

---

## Repository Structure

```
HISD_PD_Analysis/
├── HISD_PD_Analysis_.pbix        # Power BI report file
├── Sample_Summer_School_Survey_Data.csv  # Cleaned source data
└── README.md
```

---

## About

This project was completed as part of an applied data analysis portfolio, demonstrating proficiency in Power BI, Microsoft Fabric, DAX formula authoring, Power Query data transformation, and data storytelling for non-technical stakeholders.

For questions or feedback, contact: https://www.linkedin.com/in/jenleemcnew/
