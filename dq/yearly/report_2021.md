# DQ Report: mvs_opendata — 2021

**Status:** PASS  
**Rows:** 2,226,786  
**Checks:** 12/16 passed, 4 failed  
**Timestamp:** 2026-03-19T14:27:38.608056

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 2135055 | 36 | [0, 20000] |
| own_weight | range | info | 2225313 | 1284 | [40, 45000] |
| total_weight | range | info | 2226212 | 233 | [60, 90000] |
| payload | range | info | 2225118 | 2002 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_ids | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 24390 |
| oper_code | distinct_count | INFO | 97 |
| oper_name | distinct_count | INFO | 104 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 345 |
| dep_code | not_null | PASS | 100.00% |
| dep_code | distinct_count | INFO | 172 |
| dep_name | distinct_count | INFO | 175 |
| brand | distinct_count | INFO | 2048 |
| model | distinct_count | INFO | 13747 |
| vin | distinct_count | INFO | 1814549 |
| make_year | not_null | PASS | 100.00% |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 89 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 11 |
| body | distinct_count | INFO | 68 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3354 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 9371 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5267 |
| n_reg_new | distinct_count | INFO | 2111156 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 14050 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 2087216 |
| is_valid_plate | not_null | PASS | 100.00% |
| raw_vin | distinct_count | INFO | 1814557 |
| _table_ | row_count | PASS |  |