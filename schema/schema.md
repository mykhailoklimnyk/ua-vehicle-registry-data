# Schema Documentation

This document describes the canonical schema used in all normalized Parquet snapshots.

## Schema Version

| Property | Value |
|---|---|
| **Schema version** | 1.0 |
| **Effective from** | v0.1.0 |

Schema version follows the project release versioning. Changes to column names, types, or semantics require a major version increment.

## Column Definitions

| # | Column | Type | Nullable | Description |
|---|---|---|---|---|
| 1 | `record_ids` | `ARRAY[STRING]` | No | Source record IDs aggregated into this row |
| 2 | `person_type` | `STRING` | No | Type of owner: `P` — natural person, `J` — legal entity |
| 3 | `reg_addr_koatuu` | `STRING(10)` | Yes | KOATUU code of the owner's registered address. Preserved as string to retain leading zeros. |
| 4 | `oper_code` | `INTEGER` | Yes | Numeric code of the registration operation |
| 5 | `oper_name` | `STRING` | Yes | Human-readable name of the registration operation |
| 6 | `d_reg` | `DATE` | No | Date of the registration operation (YYYY-MM-DD) |
| 7 | `dep_code` | `STRING` | No | Service center code |
| 8 | `dep_name` | `STRING` | Yes | Name of the service center |
| 9 | `brand` | `STRING` | Yes | Vehicle brand (Latin) |
| 10 | `model` | `STRING` | Yes | Vehicle model (Latin) |
| 11 | `vin` | `STRING(17)` | Yes | Vehicle Identification Number (Latin). `null` for records predating 2021-01-01. |
| 12 | `make_year` | `INTEGER` | No | Year of manufacture (1900–current+1) |
| 13 | `color` | `STRING` | No | Vehicle body color |
| 14 | `kind` | `STRING` | No | Vehicle kind (e.g., passenger, cargo) |
| 15 | `body` | `STRING` | Yes | Body type (e.g., sedan, hatchback) |
| 16 | `purpose` | `STRING` | Yes | Vehicle purpose |
| 17 | `fuel` | `STRING` | Yes | Primary fuel type |
| 18 | `capacity` | `INTEGER` | Yes | Engine displacement in cm³ |
| 19 | `own_weight` | `INTEGER` | Yes | Vehicle curb weight in kg |
| 20 | `total_weight` | `INTEGER` | Yes | Vehicle gross weight in kg |
| 21 | `n_reg_new` | `STRING` | Yes | Registration plate number |
| 22 | `payload` | `INTEGER` | Yes | Payload capacity in kg |
| 23 | `secondary_fuel` | `STRING` | Yes | Secondary fuel type (LPG/CNG) |
| 24 | `n_reg_latin` | `STRING` | Yes | Registration plate transliterated to Latin |
| 25 | `is_valid_plate` | `BOOLEAN` | No | Whether the registration plate passes format validation |
| 26 | `raw_vin` | `STRING` | Yes | Original VIN from source. `null` for records predating 2021-01-01. |
| 27 | `is_valid_vin` | `BOOLEAN` | Yes | Whether the VIN passes validation. `null` for records predating 2021-01-01. |

## Notes

- All string fields are trimmed of leading/trailing whitespace.
- `person_type` values are normalized to uppercase single characters: `P` or `J`.
- `reg_addr_koatuu` is intentionally stored as `STRING` to preserve leading zeros in KOATUU codes.
- `d_reg` is parsed from various source formats (`DD.MM.YYYY`, `YYYY-MM-DD`) into a standard `DATE` type.
- `null` is used for all missing or indeterminate values. No sentinel values (e.g., `0`, `-1`, `невизначено`) are preserved.
- Column order is fixed and must be consistent across all snapshots within the same schema version.
- `vin` and `raw_vin` are `null` for all records from source files predating 2021-01-01 (the VIN column was added to the source dataset at that point). `is_valid_vin` is also `null` for these records.
- `brand` and `model` are normalized to Latin characters.
- `record_ids` contains an array of source record IDs that were aggregated into a single row via `GROUP BY` + `ARRAY_AGG`. Raw dataset: ~29M records; after aggregation: ~24M records.
- `n_reg_latin` is a transliteration of `n_reg_new` to Latin characters.
- `is_valid_plate` indicates whether `n_reg_new` passes Ukrainian plate format validation.

## Source Column Mapping

| Canonical name | Known source variants |
|---|---|
| `person_type` | `PERSON`, `person` |
| `reg_addr_koatuu` | `REG_ADDR_KOATUU`, `reg_addr_koatuu` |
| `oper_code` | `OPER_CODE`, `oper_code` |
| `oper_name` | `OPER_NAME`, `oper_name` |
| `d_reg` | `D_REG`, `d_reg` |
| `dep_code` | `DEP_CODE`, `dep_code` |
| `dep_name` | `DEP`, `dep`, `DEP_NAME` |
| `brand` | `BRAND`, `brand` |
| `model` | `MODEL`, `model` |
| `vin` | `VIN`, `vin` |
| `make_year` | `MAKE_YEAR`, `make_year`, `VYP`, `rik_vypusku` |
| `color` | `COLOR`, `color` |
| `kind` | `KIND`, `kind` |
| `body` | `BODY`, `body` |
| `purpose` | `PURPOSE`, `purpose` |
| `fuel` | `FUEL`, `fuel` |
| `capacity` | `CAPACITY`, `capacity`, `OB_DVYG` |
| `own_weight` | `OWN_WEIGHT`, `own_weight`, `VLASNA_VAGA` |
| `total_weight` | `TOTAL_WEIGHT`, `total_weight`, `POVNA_VAGA` |
| `n_reg_new` | `N_REG_NEW`, `n_reg_new` |

> **Note:** `record_ids`, `payload`, `secondary_fuel`, `n_reg_latin`, `is_valid_plate`, `raw_vin`, and `is_valid_vin` are computed fields with no direct source column mapping.
