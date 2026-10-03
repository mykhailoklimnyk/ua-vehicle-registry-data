# Methodology

How the snapshots in this repository are produced from the MIA open registry.

## 1. Source

| Field | Value |
|---|---|
| **Dataset** | Відомості про транспортні засоби та їх власників |
| **Publisher** | Міністерство внутрішніх справ України |
| **Portal** | https://data.gov.ua/dataset/06779371-308f-42d7-895e-5a39833375f0 |
| **License** | CC BY 4.0 |
| **Years** | 2013–2026 |

The exact files used — resource ID, file name, SHA-256 and row count per period — are listed in
[SOURCES.md](SOURCES.md).

## 2. One revision per period

The publisher overwrites the current-year file in place and has, over the years, published
several partial and cumulative files. Mixing revisions puts the same event into the output twice
whenever the publisher re-spells a field between revisions (plate in Cyrillic vs. Latin, a body
type filled in later, a reworded operation name). Therefore each period is taken from **exactly
one** source file, and the periods do not overlap (see SOURCES.md).

## 3. Processing

1. **Read.** `;`-separated UTF-8 CSV, every value as text. Columns are matched by name,
   case-insensitively; the three source layouts (19 columns 2013–2020, 20 columns with `VIN`
   2021 – 2026-04, 17 columns from 2026-05 with a merged operation column) map to one set.
2. **Empty values.** Empty strings and the literal text `NULL`/`None` become null. Other values,
   including source placeholders (`НЕВИЗНАЧЕНИЙ`, year `1900`), are kept.
3. **Dates.** `d_reg` is parsed from `YYYY-MM-DD`, `DD.MM.YYYY` or `DD.MM.YY` into `DATE`.
4. **Numbers.** `oper_code`, `make_year`, `capacity`, `own_weight`, `total_weight` are taken as
   integers; a non-integer value becomes null. `power_kwt` accepts a decimal comma or point.
   Values are not corrected or rescaled.
5. **Identifier.** `record_id` = MD5 over the trimmed source fields (operation name excluded,
   because the source words one operation in several ways).
6. **Duplicates.** Rows with the same `record_id` — identical in every source field — are
   published once. No fuzzy matching.
7. **Normalization of categorical values** with public dictionaries:
   * brand → Latin, catalogue spelling for brands present in the reference catalogue;
   * model → catalogue model family where it exists, matched on the source model with and
     without spaces and hyphens; otherwise the source model transliterated to Latin;
   * fuel → primary + secondary fuel (`ЕЛЕКТРО АБО ДИЗЕЛЬНЕ ПАЛИВО` → `Дизель` + `Електро`);
   * body → body group; color `ПОМАРАНЧЕВИЙ (ОРАНЖЕВИЙ)` → `ОРАНЖЕВИЙ`;
   * operation name: Latin `I` inside Cyrillic words → Cyrillic `І`.
   Each dictionary has exactly one entry per source value.
8. **Derived fields.** `payload` = `total_weight − own_weight` (null if negative); `vin`
   normalization and `is_valid_vin`; plate transliteration and `is_valid_plate` — see
   [schema.md](../schema/schema.md).
9. **Power by VIN.** `power_kwt` is carried to other rows of the same valid VIN in the source.
   No external data is used.

## 4. What is not published

* Registration plates for 2013–2020.
* Anything that is not in the source files: only values present in the MIA files are published.

## 5. Output

| Property | Value |
|---|---|
| **Formats** | Parquet (Snappy) and CSV (UTF-8, `,`) |
| **Files** | `mvs_opendata_YYYY_MM.{parquet,csv}` (month), `mvs_opendata_YYYY.{parquet,csv}` (year) |
| **Where** | GitHub Releases `vYYYY.MM` and `vYYYY.full`; Zenodo |

A monthly file contains registrations with `d_reg` in that calendar month, first to last day.

## 6. Verification

For every period the published rows were matched against the source file: the MD5 of each source
row was computed independently from the published one, and the published set equals the set of
distinct source rows — none missing, none extra. Per-period numbers are in
[DATA_QUALITY_REPORT.md](DATA_QUALITY_REPORT.md).
