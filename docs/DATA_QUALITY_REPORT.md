# Data Quality Report

This document catalogs the data quality problems found in the source dataset and summarizes metrics for each processed snapshot.

## Source Data Overview

| Property | Value |
|---|---|
| **Source** | [data.gov.ua](https://data.gov.ua/dataset/06779371-308f-42d7-895e-5a39833375f0) |
| **Time span** | 2013–2026 |
| **Raw data volume** | ~46.8 GB (CSV) |
| **Number of source CSV files** | 146 (143 valid, 3 excluded) |
| **Yearly archives** | 14 |
| **Unique records (after dedup)** | ~29 million |
| **Records after aggregation** | ~24 million |
| **Aggregation method** | `GROUP BY d_reg, oper_code, n_reg_cleaned, brand_lat` |
| **Merged Parquet size** | ~907 MB |
| **Compression ratio (CSV → Parquet)** | ~10.8% |

### Excluded Files

Three source files were excluded during validation:

| Reason | Count | Details |
|---|---|---|
| Extra column (`BIRTHDAY`) | 1 | One 2019 file shipped with an additional personal data column |
| Corrupt / wrong field count | 2 | Two 2022 files with ~107 K corrupt rows and ~930 wrong-field rows |

> **Note:** Six early-2021 files (Jan–Jun) that predate the VIN column addition are **no longer excluded**. The `vin` column is added with `null` values for schema consistency.

## Catalog of Source Data Problems

All yearly source files (2013–2026) were analyzed. The following issues were found systematically across the dataset. Each issue includes a description, affected years where observed, and the remediation applied.

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

**Problem:** Exact duplicate rows (identical across all columns) appear both within a single yearly file and across overlapping year boundaries. The dataset is published as cumulative snapshots (January–March, January–April, etc.), so later snapshots fully contain earlier ones — producing massive cross-file duplication.

**Affected:** All years from 2018 onward. In extreme cases, over 85% of raw rows across a year's files are duplicates. For example, 2023 raw data totals ~32.3 M rows but contains only ~3.7 M unique records (88.4% duplication). 2013–2017 have one file per year with no cross-file duplication.

**Remediation:** Full-row hash deduplication (SHA-256). Only exact byte-identical duplicates are removed — no fuzzy matching.

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

**Remediation:** Normalization mapping to canonical brand/model names. Curated manually with automated matching assistance.

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

These anomalies are present in the raw MIA source data and are inherited in the export as-is. They are not errors introduced by the pipeline.

| # | Anomaly | Condition | Description |
|---|---|---|---|
| 1 | Negative weight | `own_weight < 0` or `total_weight < 0` | Data-entry errors in the source |
| 2 | Vehicles from the "future" | `make_year > 2027` | Data-entry errors (year of manufacture beyond plausible range) |
| 3 | Weight > 200 tonnes | `own_weight > 200000` | Data-entry errors (weight in grams instead of kg, or similar) |
| 4 | Curb weight > gross weight | `own_weight > total_weight` | Logically impossible: curb weight cannot exceed gross weight |
| 5 | Engine capacity > 50 L | `capacity > 50000` | Data-entry errors (capacity in mL instead of cm³, or similar) |
| 6 | Duplicate source records | — | Consolidated via `GROUP BY` + `ARRAY_AGG` into `record_ids` |

> **Note:** These anomalies are flagged in the per-release Data Quality report (Markdown file included in each GitHub Release).

## Per-Release Data Quality Reports

Each GitHub Release includes a Data Quality (DQ) report in Markdown format. The report contains:

- Record counts (source vs. output)
- Null-rate statistics per column
- Anomaly detection results (see table above)
- Validation pass/fail summary for plates and VINs

Reports are generated automatically by the pipeline and attached to the release alongside data files.

---

## Snapshot Metrics

| Metric | Description |
|---|---|
| **Total records (source)** | Total rows in the source CSV before cleaning |
| **Total records (output)** | Total rows in the Parquet snapshot after cleaning and aggregation |
| **Exact duplicates removed** | Count of bit-identical duplicate rows dropped |
| **Empty rows removed** | Count of rows where all columns were null/empty |
| **Encoding issues fixed** | Count of files requiring re-encoding |
| **Date parse failures** | Count of `d_reg` values that could not be parsed |
| **Column name variants mapped** | Number of non-canonical column names remapped |
| **Leading zeros restored** | Count of KOATUU codes with leading zeros restored |

> _Snapshot metrics will be populated upon first release._

---

## Known Data Quality Issues in Source (Summary)

| # | Issue | Severity | Years affected |
|---|---|---|---|
| 1 | Broken encoding (mojibake) | Critical | 2019 (CP1251), others UTF-8 |
| 2 | Inconsistent column names / schema change | High | All; VIN added mid-2021 |
| 3 | Delimiter inconsistency / Cyrillic extensions | Medium | 2018, 2019–2020, 2023 |
| 4 | Duplicate records (cumulative snapshots) | High | 2018–2026 (up to 88%) |
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
