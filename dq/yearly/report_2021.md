# DQ Report: mvs_opendata — 2021

**Status:** WARN  
**Rows:** 2,200,466  
**Checks:** 9/14 passed, 5 failed  
**Timestamp:** 2026-10-03T10:14:19.575236

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 2091336 | 36 | [0, 20000] |
| power_kwt | range | info | 50375 | 1 | [0, 2000] |
| own_weight | range | info | 2199065 | 1161 | [40, 45000] |
| total_weight | range | info | 2199946 | 212 | [60, 90000] |
| payload | range | info | 2196437 | 1989 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_id | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 24389 |
| oper_code | distinct_count | INFO | 96 |
| oper_name | distinct_count | INFO | 100 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 345 |
| dep_code | distinct_count | INFO | 172 |
| dep_name | distinct_count | INFO | 172 |
| brand | distinct_count | INFO | 1970 |
| model | distinct_count | INFO | 13740 |
| vin | distinct_count | INFO | 1814530 |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 89 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 11 |
| body | distinct_count | INFO | 70 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3354 |
| power_kwt | range | FAIL | [0, 2000] |
| power_kwt | distinct_count | INFO | 535 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 9537 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5267 |
| n_reg_new | distinct_count | INFO | 2087354 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 13993 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 2087194 |
| raw_vin | distinct_count | INFO | 1814534 |
| _table_ | row_count | PASS |  |