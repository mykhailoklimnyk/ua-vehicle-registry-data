# Schema Documentation

This document describes the canonical schema of all snapshots (Parquet and CSV).

## Schema Version

| Property | Value |
|---|---|
| **Schema version** | 3.0 |
| **Effective from** | Re-release of 2026-10 — applies to every snapshot 2013–2026 |

Changes to column names, types, or semantics require a major version increment.

### What changed in 3.0

All snapshots were rebuilt from the MIA files listed in [docs/SOURCES.md](../docs/SOURCES.md),
one source revision per period, with one pipeline. Earlier releases mixed three build
generations; 3.0 replaces them.

1. **`record_id` everywhere.** The `record_ids` array (2013 – 2026-03) is gone. Each row is one
   distinct source row; `record_id` is the MD5 of its source fields and is unique.
2. **No registration plates for 2013–2020.** `n_reg_new`, `n_reg_latin`, `is_valid_plate` are
   published for 2021-01 … 2026-04 only. From 2026-05 the source has no plates.
3. **`power_kwt` is a number** with the source precision (`154.6`), not an integer. Values written
   with a decimal comma were previously lost.
4. **`d_reg` is a Parquet `DATE`**, not a string. Booleans in CSV are `true`/`false`.
5. **Empty columns are omitted.** A column the source does not have for a period is left out of
   that file: VIN columns and `power_kwt` before 2021, plates outside 2021-01 … 2026-04,
   `reg_addr_koatuu` from 2026-05.

Column order is fixed; a file contains the listed columns minus those omitted by rule 5.

## Column Definitions

| # | Column | Type | Nullable | Description |
|---|---|---|---|---|
| 1 | `record_id` | `STRING` | No | MD5 of the source fields (operation name excluded). Unique. |
| 2 | `person_type` | `STRING` | No | `P` — natural person, `J` — legal entity |
| 3 | `reg_addr_koatuu` | `STRING(10)` | Yes | KOATUU of the owner's address, leading zeros kept. Not from 2026-05. |
| 4 | `oper_code` | `INTEGER` | Yes | Operation code. Group by this, not by `oper_name`. |
| 5 | `oper_name` | `STRING` | Yes | Operation name from the source, Latin `I` inside Cyrillic words replaced by `І` |
| 6 | `d_reg` | `DATE` | No | Registration date |
| 7 | `dep_code` | `STRING` | Yes | Service center code. From 2026-05 recovered from the name. |
| 8 | `dep_name` | `STRING` | Yes | Service center name |
| 9 | `brand` | `STRING` | Yes | Brand, Latin; catalogue spelling where the brand is in the catalogue |
| 10 | `model` | `STRING` | Yes | Model, Latin; catalogue model family where one exists |
| 11 | `vin` | `STRING` | Yes | Normalized VIN (see notes). From 2021. |
| 12 | `make_year` | `INTEGER` | Yes | Year of manufacture as in the source; NULL when later than the registration year + 1 |
| 13 | `color` | `STRING` | Yes | Body color |
| 14 | `kind` | `STRING` | Yes | Vehicle kind |
| 15 | `body` | `STRING` | Yes | Body type group |
| 16 | `purpose` | `STRING` | Yes | Vehicle purpose |
| 17 | `fuel` | `STRING` | Yes | Primary fuel |
| 18 | `capacity` | `INTEGER` | Yes | Engine displacement, cm³, as in the source |
| 19 | `power_kwt` | `DOUBLE` | Yes | Engine power, kW, source precision. From 2021 (see notes). |
| 20 | `own_weight` | `INTEGER` | Yes | Curb weight, kg, as in the source |
| 21 | `total_weight` | `INTEGER` | Yes | Gross weight, kg, as in the source |
| 22 | `n_reg_new` | `STRING` | Yes | Registration plate. 2021-01 … 2026-04 only. |
| 23 | `payload` | `INTEGER` | Yes | `total_weight − own_weight`; null if `own_weight > total_weight` |
| 24 | `secondary_fuel` | `STRING` | Yes | Secondary fuel (`Газ`, `Електро`) |
| 25 | `n_reg_latin` | `STRING` | Yes | `n_reg_new` in Latin script. Same periods. |
| 26 | `is_valid_plate` | `BOOLEAN` | Yes | Plate matches a standard civilian format. Same periods. |
| 27 | `raw_vin` | `STRING` | Yes | VIN exactly as in the source. From 2021. |
| 28 | `is_valid_vin` | `BOOLEAN` | Yes | VIN format check. From 2021. |

## Notes

- **Duplicates.** Rows identical in every source column are published once. Two rows that differ
  in any source field are two rows, even if they describe what looks like the same event.
- **`vin`**: upper case; spaces, `/`, `-`, quotes removed; Cyrillic look-alikes replaced by Latin
  (`А В Е К М Н О Р С Т Х І` → `A B E K M H O P C T X I`, `З` → `3`). Other Cyrillic letters are
  **kept**, so a value with them is visibly not a valid VIN. `vin` matches
  `^[A-HJ-NPR-Z0-9]{17}$` exactly where `is_valid_vin` is true.
- **`is_valid_vin`** checks the format only (17 characters, no `I O Q`, not starting with `0`).
  The check digit is not verified: most VINs outside North America do not use it.
- **`is_valid_plate`** is true for `AA0000AA` (2004+), `00000AA` (1995–2004) and online-registration
  codes. Older and special formats (`16ВР3029`, `АЕАА4125`, diplomatic `CD001158`) are false — in
  every year alike.
- **`power_kwt`**: the source publishes it from 2026-05. It is carried to other registrations of
  the same VIN in the source, so it can appear on 2021 – 2026-04 rows; there are no VINs before 2021.
- **`capacity`, `own_weight`, `total_weight`** are not corrected. Electric vehicles keep the
  source capacity (often empty).
- **`make_year`** is as in the source, except a year later than the registration year + 1 —
  an impossible value — which is published as NULL. Four obvious typos of 2013 are corrected
  (2088 → 2008, 2033 → 2003 twice, 2036 → 2006). `make_year = 1900` is a source placeholder.
- **Placeholders** kept as values: `color`/`kind` `Невизначений`, `body` `Невизначений`. They are
  the source's own value, not a missing one.
- **`color`**: the 2025+ source spelling `ПОМАРАНЧЕВИЙ (ОРАНЖЕВИЙ)` is published as `Оранжевий`,
  matching 2013–2024.
- **`oper_name`**: several codes have more than one wording in the source (e.g. 308, 100, 540).

## Source Column Mapping

| Canonical name | Source column |
|---|---|
| `person_type` | `PERSON` |
| `reg_addr_koatuu` | `REG_ADDR_KOATUU` (to 2026-04) |
| `oper_code`, `oper_name` | `OPER_CODE`, `OPER_NAME`; from 2026-05 one column `CD.OPER_CODE\|\|'-'\|\|CD.OPERAS` |
| `d_reg` | `D_REG` (`YYYY-MM-DD`, `DD.MM.YYYY` or `DD.MM.YY`) |
| `dep_code` | `DEP_CODE` (to 2026-04) |
| `dep_name` | `DEP` |
| `brand`, `model` | `BRAND`, `MODEL` (2013–2018: `BRAND` holds brand and model) |
| `vin`, `raw_vin` | `VIN` (from 2021) |
| `make_year` | `MAKE_YEAR` |
| `color`, `kind`, `body`, `purpose`, `fuel` | `COLOR`, `KIND`, `BODY`, `PURPOSE`, `FUEL` |
| `capacity` | `CAPACITY` |
| `power_kwt` | `POWER_KWT` (from 2026-05) |
| `own_weight`, `total_weight` | `OWN_WEIGHT`, `TOTAL_WEIGHT` |
| `n_reg_new` | `N_REG_NEW` (to 2026-04) |

Column names are matched case-insensitively. `record_id`, `payload`, `secondary_fuel`,
`n_reg_latin`, `is_valid_plate` and `is_valid_vin` are computed.
