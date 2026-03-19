# DQ Report: mvs_opendata — 2013

**Status:** PASS  
**Rows:** 1,933,940  
**Checks:** 12/16 passed, 4 failed  
**Timestamp:** 2026-03-19T13:01:29.190728

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 1851403 | 53 | [0, 20000] |
| own_weight | range | info | 1865442 | 265 | [40, 45000] |
| total_weight | range | info | 1923305 | 53 | [60, 90000] |
| payload | range | info | 1863874 | 1039 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_ids | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 23076 |
| oper_code | distinct_count | INFO | 101 |
| oper_name | distinct_count | INFO | 123 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 363 |
| dep_code | not_null | PASS | 100.00% |
| dep_code | distinct_count | INFO | 410 |
| dep_name | distinct_count | INFO | 537 |
| brand | distinct_count | INFO | 1809 |
| model | distinct_count | INFO | 10432 |
| vin | distinct_count | INFO | 0 |
| make_year | not_null | PASS | 100.00% |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 85 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 11 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 12 |
| body | distinct_count | INFO | 51 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3487 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 7381 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5954 |
| n_reg_new | distinct_count | INFO | 1682496 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 9986 |
| secondary_fuel | distinct_count | INFO | 2 |
| n_reg_latin | distinct_count | INFO | 1681499 |
| is_valid_plate | not_null | PASS | 100.00% |
| raw_vin | distinct_count | INFO | 0 |
| _table_ | row_count | PASS |  |