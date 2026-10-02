# Data Quality Report

This document catalogs the data quality problems found in the source dataset and summarizes metrics for each processed snapshot.

## Source Data Overview

| Property | Value |
|---|---|
| **Source** | [data.gov.ua](https://data.gov.ua/dataset/06779371-308f-42d7-895e-5a39833375f0), dataset `06779371-308f-42d7-895e-5a39833375f0` |
| **Time span** | 2013 – 2026-08-30 |
| **Release** | 0.2.0, rebuilt 2026-10-02 |
| **Source files used** | exactly one MIA revision per period (list and SHA-256 in [SOURCES.md](SOURCES.md)) |
| **Published rows** | 24,853,336 |
| **Row definition** | one row = one unique source event; exact duplicate rows of the source collapse into one (`record_id` = MD5 of the trimmed raw fields) |

The dataset is built only from the MIA files; no internal enrichment is used. The column `record_ids` no longer exists. The current MIA 2026 file ends on 2026-08-30 (there are no 2026-08-31 rows in the source).

## Catalog of Source Data Problems

All source files (2013–2026) were analyzed. The following issues were found systematically across the dataset. Each issue includes a description, affected years where observed, and the remediation applied.

### 1. Broken Character Encoding (Mojibake)

**Problem:** Source CSV files use inconsistent character encodings — some are Windows-1251, others UTF-8 with BOM, others UTF-8 without BOM. When read with the wrong encoding, Ukrainian characters (є, і, ї, ґ) appear as garbled text ("крякозябри" / mojibake).

**Affected:** Most files are UTF-8. At least four 2019 files are encoded in CP1251 (Windows-1251).

**Remediation:** Automatic encoding detection (`chardet` / `charset-normalizer`) followed by re-encoding to UTF-8 without BOM. Manual spot-checking of Ukrainian-specific characters.

---

### 2. Inconsistent Column Names

**Problem:** Column headers change between years — different naming, different casing, different abbreviations. The schema also evolved structurally: files before mid-2021 have 19 columns, while files from July 2021 onward include a `VIN` column (20 columns in source). In the output, all records have 20 columns — VIN is set to `null` for pre-2021 data. One 2019 file shipped with an extra `BIRTHDAY` column (personal data that should not have been published). Examples of naming variation:
- `MAKE_YEAR` in some years vs. `VYP` or `rik_vypusku` in others
- `CAPACITY` vs. `OB_DVYG`
- `OWN_WEIGHT` vs. `VLASNA_VAGA`
- `DEP_CODE` vs. `DEP`
- Uppercase in older files, mixed case in newer ones

**Affected:** Virtually all years. No two consecutive years use exactly the same headers.

**Remediation:** Canonical column mapping (see `schema/schema.md`). All variants mapped to a fixed set of lowercase names.

---

### 3. Delimiter Inconsistency

**Problem:** Some yearly files use `;` (semicolon) as the CSV delimiter, others use `,` (comma). In some files, malformed rows contain extra or missing delimiters. Occasionally, file extensions use Cyrillic `с` instead of Latin `c` (`.сsv` vs `.csv`).

**Affected:** The majority of files use `;`. Comma-delimited files found in 2018 (2 files) and 2023 (1 file). Cyrillic extensions appear in some 2019–2020 files.

**Remediation:** Delimiter auto-detection per file. Malformed rows flagged and handled individually.

---

### 4. Duplicate Records

**Problem:** Exact duplicate rows (identical in every source field) appear within a single source file. In addition, the source was historically published as cumulative and partial files, and the same registration event can appear in several revisions with different spellings (plate in Cyrillic vs. Latin, a body type filled in later, a reworded operation name).

**Affected:** All years. Exact duplicates inside the chosen source files are listed per period in [Verification per period](#verification-per-period); 2025 has the most (31,785).

**Remediation:** Each period is taken from exactly one source revision, so revisions are never mixed. Rows identical in every source field collapse into one (`record_id` = MD5 of the trimmed raw fields). No fuzzy matching.

---

### 5. Invalid and Mixed Data Types

**Problem:** Fields that should be numeric (engine capacity, weight, year) are stored as text strings with inconsistent formatting. Dates appear in multiple formats: `DD.MM.YYYY`, `YYYY-MM-DD`, and occasionally malformed values. Numeric fields additionally contain mixed decimal separators, range values, embedded units, and other non-numeric artifacts (see §12 for detailed breakdown).

**Affected:** All years.

**Remediation:** Type coercion with explicit format parsing. Multi-stage numeric normalization for weight and capacity fields. Unparseable values set to `null` and counted as parse failures.

---

### 6. Leading Zeros Stripped from Codes

**Problem:** KOATUU codes and other zero-padded identifiers had their leading zeros removed. This is a classic symptom of opening CSV files in Microsoft Excel, which auto-converts text cells to numbers, silently truncating `0123456789` to `123456789`.

**Affected:** Multiple years — particularly visible in `reg_addr_koatuu` and some operation codes.

**Remediation:** KOATUU codes stored as `STRING` type (not integer) in the output schema. Known KOATUU values cross-referenced to restore leading zeros where possible.

---

### 7. Inconsistent Brand and Model Names

**Problem:** The same vehicle make or model is recorded in dozens of variations — different transliterations, abbreviations, typos, uppercase/lowercase mixing. For example:
- `TOYOTA` / `Toyota` / `ТОЙОТА` / `ТОУОТА`
- `MERCEDES-BENZ` / `MERSEDES-BENZ` / `МЕРСЕДЕС БЕНЦ` / `МЕРСЕДЕС-БЕНЗ`

**Affected:** All years.

**Remediation:** Normalization mapping to canonical brand/model names: a single Latin spelling per brand and model families (see issue #5 below). Each dictionary has exactly one entry per source value.

---

### 8. Placeholder Values Instead of Nulls

**Problem:** Instead of empty cells or proper null markers, source data uses various placeholders:
- `"невизначено"` / `"Не визначено"`
- Single space `" "`
- Dash `"-"`
- Zero `"0"` in fields where zero is not a valid value
- Literal string `"NULL"` / `"null"` — the word NULL stored as text rather than a true null value

**Affected:** All years, across most text and some numeric fields. The literal `"NULL"` string appears across multiple years and fields.

**Remediation:** All placeholder variants — including the literal text `"NULL"` — converted to `null`. No imputation performed.

---

### 9. Garbled Values (Data Corruption)

**Problem:** Some cells contain byte-level corruption — partial encoding conversions, control characters, or truncated multibyte sequences. These manifest as garbled strings in text fields (brand, model, operation name, etc.).

**Affected:** Sporadic across all years. Most severe in 2022, where two files contained ~107 K corrupt rows total (66 986 and 39 927 respectively) and ~930 rows with wrong field counts. Smaller amounts (~1 K corrupt rows) found in 2019.

**Remediation:** Detected via encoding validation heuristics and control-character pattern matching (`[\x00-\x08\x0b\x0c\x0e-\x1f]`). Irrecoverable values set to `null`.

---

### 10. Literal `NULL` Text Instead of Null Values

**Problem:** In addition to the placeholder values described in §8, some cells contain the literal string `"NULL"` (or `"null"`, `"Null"`) — the textual representation of a null marker written as data rather than represented as a true missing value. This is distinct from the `"невизначено"` placeholder — it indicates that the exporting system serialized null values as the literal text `NULL` instead of leaving the cell empty.

**Affected:** Multiple years and fields. Found across text and numeric columns.

**Remediation:** All case-insensitive variants of the literal string `NULL` are detected and converted to true `null` values.

---

### 11. Orphan KOATUU Codes (Missing from Public Dictionaries)

**Problem:** Over 100 distinct `reg_addr_koatuu` values in the dataset do not match any code in the publicly available KOATUU registry (the official Ukrainian classification of administrative-territorial units). These "orphan" codes may represent:
- Deprecated codes from older KOATUU revisions that were removed or merged
- Data-entry errors (typos, transpositions)
- Codes from interim administrative reforms not yet reflected in published dictionaries
- Artifacts of the KOATUU-to-KATOTTG transition (Ukraine is migrating to a new territorial code system)

**Affected:** Multiple years. The 100+ orphan codes collectively cover a non-trivial number of records.

**Remediation:** Orphan codes are preserved as-is in the output (not discarded or nullified). A reconciliation effort is underway to map these codes to their correct KOATUU or KATOTTG equivalents using archival sources and cross-referencing with neighboring code ranges. Codes that cannot be resolved are flagged for manual review.

---

### 12. Mixed Formats in Numeric Fields (Weight, Capacity, etc.)

**Problem:** Fields that should contain a single numeric value (`capacity`, `own_weight`, `total_weight`) instead contain a wide variety of non-standard representations:
- **Decimal separators:** both `.` (dot) and `,` (comma) are used — e.g., `1.5`, `1,5`
- **Range values:** slash-separated or dash-separated ranges — e.g., `1500/2000`, `1500-2000`, `від 1500 до 2000`
- **Multiple values:** several numbers in one cell — e.g., `1500 2000`
- **Units included:** values with embedded units — e.g., `1500 кг`, `1.6 л`
- **Invalid characters:** letters, special characters, or formatting artifacts mixed into numeric strings
- **Boolean-like values:** fields containing `0` or `1` where a numeric measurement is expected

This prevents direct numeric parsing and produces widespread type coercion failures if not handled.

**Affected:** All years. Most prevalent in `capacity`, `own_weight`, and `total_weight` columns, but also observed in `make_year`.

**Remediation:** Multi-stage parsing: (1) normalize decimal separators (comma → dot), (2) extract the first valid numeric value from range/multi-value cells, (3) strip embedded units and non-numeric characters, (4) values that remain unparseable after normalization are set to `null` and counted as parse failures. No averaging or interpolation of range values is performed — only the first value is taken.

---

## Known Anomalies in Export Data

These anomalies are present in the raw MIA source data and are inherited in the export as-is (values are not corrected). They are not errors introduced by the pipeline.

| # | Anomaly | Condition | Description |
|---|---|---|---|
| 1 | Negative weight | `own_weight < 0` or `total_weight < 0` | Data-entry errors in the source |
| 2 | Vehicles from the "future" | `make_year > 2027` | Data-entry errors (year of manufacture beyond plausible range) |
| 3 | Weight > 200 tonnes | `own_weight > 200000` | Data-entry errors (weight in grams instead of kg, or similar) |
| 4 | Curb weight > gross weight | `own_weight > total_weight` | Logically impossible: curb weight cannot exceed gross weight (`payload` is NULL in this case) |
| 5 | Engine capacity > 50 L | `capacity > 50000` | Data-entry errors (capacity in mL instead of cm³, or similar) |
| 6 | Duplicate source records | — | Rows identical in every source field are published once; the column `record_ids` no longer exists |

> **Note:** These anomalies are flagged in the per-release Data Quality report (Markdown file included in each GitHub Release).

## Per-Release Data Quality Reports

Each GitHub Release includes a Data Quality (DQ) report in Markdown format. The report contains:

- Record counts (source vs. output)
- Null-rate statistics per column
- Anomaly detection results (see table above)
- Validation pass/fail summary for plates and VINs

Reports are generated automatically by the pipeline and attached to the release alongside data files.

---

## Verification per period

For every period the published rows were matched against the source file: the MD5 of each source row was computed independently from the published one. In every period there are **0 missing, 0 extra and 0 duplicate `record_id`**.

| Period | Source rows | Exact duplicates collapsed | Published |
|---|---:|---:|---:|
| 2013 | 1,935,496 | 1,155 | 1,934,341 |
| 2014 | 1,439,551 | 1,231 | 1,438,320 |
| 2015 | 1,296,256 | 1,279 | 1,294,977 |
| 2016 | 1,432,560 | 1,192 | 1,431,368 |
| 2017 | 1,417,655 | 1,246 | 1,416,409 |
| 2018 | 1,547,418 | 1,212 | 1,546,206 |
| 2019 | 2,079,481 | 1,413 | 2,078,068 |
| 2020 | 1,771,329 | 839 | 1,770,490 |
| 2021 | 2,201,307 | 841 | 2,200,466 |
| 2022 | 1,745,908 | 756 | 1,745,152 |
| 2023 | 2,124,732 | 6,554 | 2,118,178 |
| 2024 | 2,344,544 | 5,593 | 2,338,951 |
| 2025 | 2,229,904 | 31,785 | 2,198,119 |
| 2026-01-01 … 2026-04-29 (MVS revision 508698) | 693,929 | 1,492 | 692,437 |
| 2026-04-30 … 2026-08-30 (current MVS 2026 file) | 650,466 | 612 | 649,854 |
| **Total** | | | **24,853,336** |

Published rows per period = source rows − exact duplicates collapsed. Total published: 24,853,336.

## Fixes in 0.2.0 (GitHub issues #1–#7)

All seven issues were reported by Qyperion.

| Issue | Problem | Fix in 0.2.0 |
|---|---|---|
| #1 | The last day of every month was dropped from the monthly files (month end was computed as an exclusive `MonthEnd`). | A month is `[first day, first day of next month)`. |
| #2 | More rows than in the source: the old build mixed rows of older MIA revisions and repeated the same event with different spellings. | One revision per period; exact duplicates collapsed. `v2025.full` has 2,198,119 rows = unique source events. |
| #3 | `ЕЛЕКТРО АБО ДИЗЕЛЬНЕ ПАЛИВО` was mapped to a petrol hybrid. | Fuel `Дизель`, secondary fuel `Електро`. |
| #4 | `POWER_KWT` with a decimal comma (e.g. `154,6`) was lost. | Parsed; `power_kwt` is a float with decimals. Present in the source from 2026-05; carried to other records of the same valid VIN from the source. |
| #5 | The same brand or model was spelled in several ways. | Dictionaries fixed: a single Latin spelling per brand, model families. |
| #6 | Values differed from the source. | `own_weight`, `total_weight`, `capacity`, `make_year` as in the source (no "corrections"); `payload` is NULL when `total_weight < own_weight`; `is_valid_vin` is true only for valid 17-character VINs; a VIN keeps non-lookalike Cyrillic letters instead of deleting them; an empty plate gives `is_valid_plate` NULL. |
| #7 | 2019 had about 204,000 rows that are not in the current MIA file (an older revision). | Removed. Colour `ПОМАРАНЧЕВИЙ (ОРАНЖЕВИЙ)` unified to `Оранжевий`; Latin `I` inside Cyrillic words of `oper_name` replaced by Cyrillic `І`; body types mapped consistently. |

## Publication Rules

* Registration plates (`n_reg_new`, `n_reg_latin`, `is_valid_plate`) are published only for 2021-01…2026-04: not for 2013–2020 (by decision), and absent in the source from 2026-05.
* `vin`, `raw_vin`, `is_valid_vin` only from 2021 — the source has no VIN before.
* `reg_addr_koatuu` is absent in the source from 2026-05.
* Columns that are entirely empty in a period are dropped from that period's file.

## Known Data Quality Issues in Source (Summary)

| # | Issue | Severity | Years affected |
|---|---|---|---|
| 1 | Broken encoding (mojibake) | Critical | 2019 (CP1251), others UTF-8 |
| 2 | Inconsistent column names / schema change | High | All; VIN added mid-2021 |
| 3 | Delimiter inconsistency / Cyrillic extensions | Medium | 2018, 2019–2020, 2023 |
| 4 | Duplicate records (exact duplicates; overlapping revisions) | High | All; resolved in 0.2.0 |
| 5 | Invalid/mixed data types | High | All |
| 6 | Leading zeros stripped | High | Multiple |
| 7 | Inconsistent brand/model names | High | All |
| 8 | Placeholder values instead of nulls | Medium | All |
| 9 | Garbled values (data corruption) | Critical | 2019, 2022 (107 K+ rows) |
| 10 | Literal `NULL` text instead of null values | Medium | Multiple |
| 11 | Orphan KOATUU codes (not in public dictionaries) | High | Multiple (100+ codes) |
| 12 | Mixed formats in numeric fields (weight, capacity) | High | All |

See **Catalog of Source Data Problems** above for detailed descriptions and remediations.

## Methodology Reference

See [METHODOLOGY.md](METHODOLOGY.md) for the full description of cleaning procedures applied to address these issues.
