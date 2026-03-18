# DQ Report: mvs_opendata — 2025

**Status:** PASS  
**Rows:** 2,207,737  
**Checks:** 12/16 passed, 4 failed  
**Timestamp:** 2026-03-19T14:28:39.734147

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 2133260 | 14 | [0, 20000] |
| own_weight | range | info | 2204444 | 1310 | [40, 45000] |
| total_weight | range | info | 2207082 | 261 | [60, 90000] |
| payload | range | info | 2204216 | 2419 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_ids | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 24004 |
| oper_code | distinct_count | INFO | 120 |
| oper_name | distinct_count | INFO | 126 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 364 |
| dep_code | not_null | PASS | 100.00% |
| dep_code | distinct_count | INFO | 153 |
| dep_name | distinct_count | INFO | 154 |
| brand | distinct_count | INFO | 2081 |
| model | distinct_count | INFO | 14700 |
| vin | distinct_count | INFO | 1735813 |
| make_year | not_null | PASS | 100.00% |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 89 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 14 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 11 |
| body | distinct_count | INFO | 73 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3159 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 9826 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 4905 |
| n_reg_new | distinct_count | INFO | 1832399 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 14339 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 1823140 |
| is_valid_plate | not_null | PASS | 100.00% |
| raw_vin | distinct_count | INFO | 1735818 |
| _table_ | row_count | PASS |  |