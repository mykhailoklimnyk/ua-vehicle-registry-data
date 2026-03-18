# Methodology

This document describes the data processing methodology used to produce the normalized snapshots in this project.

## 1. Source

| Field | Value |
|---|---|
| **Dataset title** | Відомості про транспортні засоби та їх власників |
| **Publisher** | Міністерство внутрішніх справ України |
| **Portal URL** | https://data.gov.ua/dataset/06779371-308f-42d7-895e-5a39833375f0 |
| **Dataset ID** | `06779371-308f-42d7-895e-5a39833375f0` |
| **Format** | CSV (ZIP archives per year) |
| **License** | CC BY 4.0 |
| **Available years** | 2013–2026 |
| **First retrieval date** | 2026-02 |

Source files are downloaded directly from data.gov.ua. Each yearly resource is a ZIP archive containing one or more CSV files.

## 2. Cleaning Stages

### 2.1 Encoding Normalization

Source CSV files have been observed with inconsistent character encodings (Windows-1251, UTF-8 with BOM, UTF-8 without BOM). When read with the wrong encoding, Ukrainian characters produce mojibake (крякозябри). All files are re-encoded to **UTF-8 without BOM** before further processing.

Detection method: automatic encoding detection (e.g., `chardet` / `charset-normalizer`) with manual verification of Ukrainian characters (є, і, ї, ґ).

### 2.2 Leading Zero Restoration

KOATUU codes and other zero-padded fields in the source data had leading zeros stripped — a classic artifact of opening CSV files in Microsoft Excel, which silently converts text cells to numbers (e.g., `0123456789` → `123456789`). These fields are stored as `STRING` in the output and cross-referenced with the official KOATUU registry to restore original values where possible.

> **Note:** Over 100 distinct KOATUU codes in the dataset do not match any entry in publicly available KOATUU dictionaries. These orphan codes are preserved as-is and flagged for ongoing reconciliation against archival KOATUU revisions and the KATOTTG transition tables. See §11 in `DATA_QUALITY_REPORT.md` for details.

### 2.3 Schema Normalization

Source files from different years use varying column names, column order, and delimiters. The normalization stage:

1. **Maps variant column names** to a canonical set (see `schema/schema.md`).
2. **Enforces a fixed column order** across all snapshots.
3. **Standardizes delimiters** — source files use `;` or `,`; all are read consistently.

Column mapping examples:

| Source variant | Canonical name |
|---|---|
| `PERSON` / `person` | `person_type` |
| `REG_ADDR_KOATUU` | `reg_addr_koatuu` |
| `OPER_CODE` | `oper_code` |
| `OPER_NAME` | `oper_name` |
| `D_REG` | `d_reg` |
| `DEP_CODE` | `dep_code` |
| `DEP` / `DEP_NAME` | `dep_name` |
| `BRAND` | `brand` |
| `MODEL` | `model` |
| `VIN` | `vin` |
| `MAKE_YEAR` / `VYP` | `make_year` |
| `COLOR` | `color` |
| `KIND` | `kind` |
| `BODY` | `body` |
| `PURPOSE` | `purpose` |
| `FUEL` | `fuel` |
| `CAPACITY` / `OB_DVYG` | `capacity` |
| `OWN_WEIGHT` / `VLASNA_VAGA` | `own_weight` |
| `TOTAL_WEIGHT` / `POVNA_VAGA` | `total_weight` |
| `N_REG_NEW` | `n_reg_new` |

### 2.4 Deduplication

Exact duplicates (rows identical across all columns) are dropped. The deduplication key is the full row hash.

Statistics on dropped duplicates are reported in `docs/DATA_QUALITY_REPORT.md`.

No fuzzy matching or record linkage is performed — only exact byte-level duplicate removal.

### 2.5 Type Stabilization

| Column | Target type | Notes |
|---|---|---|
| `d_reg` | `DATE` | Parsed from `DD.MM.YYYY` or `YYYY-MM-DD` |
| `make_year` | `INT32` | Four-digit year; nulls for missing/invalid |
| `oper_code` | `INT32` | Numeric operation code |
| `capacity` | `FLOAT64` | Engine displacement in cm³ |
| `own_weight` | `INT32` | Weight in kg |
| `total_weight` | `INT32` | Weight in kg |
| `reg_addr_koatuu` | `STRING` | KOATUU code preserved as string (leading zeros) |
| All other fields | `STRING` | Trimmed, uppercased where appropriate |

**Numeric format normalization:** Source numeric fields (`capacity`, `own_weight`, `total_weight`, `make_year`) frequently contain non-standard representations: mixed decimal separators (`.` and `,`), range values (`1500/2000`, `1500-2000`, `від 1500 до 2000`), embedded units (`1500 кг`, `1.6 л`), boolean-like values (`0`, `1`), and other non-numeric characters. These are handled via multi-stage parsing:

1. Normalize decimal separators (comma → dot)
2. Extract the first valid numeric value from range/multi-value cells
3. Strip embedded units and non-numeric characters
4. Values that remain unparseable are set to `null`

No averaging or interpolation of range values is performed — only the first value is taken.

### 2.6 Missing Value Handling

- Empty strings and whitespace-only values are converted to `null`.
- Fields with the value `"невизначено"` or `"Не визначено"` are converted to `null`.
- The literal string `"NULL"` (and case-insensitive variants `"null"`, `"Null"`) is converted to `null`.
- Dash `"-"`, single space `" "`, and zero `"0"` in non-numeric context are converted to `null`.
- No imputation is performed — missing values remain as nulls.

### 2.7 Brand and Model Normalization

Vehicle brand and model names in the source data are recorded inconsistently — different transliterations, abbreviations, typos, and casing. For example, `MERCEDES-BENZ` may appear as `MERSEDES-BENZ`, `МЕРСЕДЕС БЕНЦ`, etc.

All brand and model values are mapped to canonical forms using a curated lookup table. The normalization is additive — original values can be recovered from the source.

### 2.8 Error Corrections

Obvious data-entry errors are corrected where they can be unambiguously identified:
- **KOATUU codes** — invalid codes corrected against the official KOATUU registry
- **Registration plates** — format violations fixed (e.g., Cyrillic/Latin character confusion)
- **Garbled values** — cells with encoding corruption detected and set to `null`

All corrections are deterministic and documented. No subjective interpretations are applied.

## 3. Exclusions

### What Was Removed
- Exact duplicate records (see §2.4)
- Records with completely empty rows (all columns null)
- Irrecoverable garbled values (set to null)

### What Was Not Included
- No VIN numbers or other non-public identifiers were added
- No geocoding beyond the original KOATUU code
- No owner name normalizing or personal data processing
- Non-public enrichment (e.g., corrected body types) is excluded from this open dataset and available separately via [automoto.ai](https://automoto.ai)

### Confirmation
> All records originate from the source dataset published on data.gov.ua. Dictionaries added from public sources are clearly separated. The transformations applied are: encoding normalization, leading zero restoration, schema normalization, deduplication, type coercion, null standardization, brand/model normalization, and error corrections.

## 4. Output Format

| Property | Value |
|---|---|
| **Format** | Apache Parquet |
| **Compression** | Snappy |
| **Row group size** | Default (typically 128 MB) |
| **Naming convention** | `YYYY-MM.parquet` |

Each monthly snapshot is a single Parquet file representing data retrieved during that calendar month.

## 5. Reproducibility

While the full transformation pipeline code is not published in this repository, the cleaning and normalization steps described above are deterministic and can be independently reproduced by any party with access to the source data.

Schema definitions in `schema/schema.json` provide a machine-readable contract for the output format.
