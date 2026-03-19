# DQ Report: mvs_opendata — 2019

**Status:** PASS  
**Rows:** 2,282,531  
**Checks:** 12/16 passed, 4 failed  
**Timestamp:** 2026-03-19T14:27:11.713476

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 2182909 | 29 | [0, 20000] |
| own_weight | range | info | 2280937 | 691 | [40, 45000] |
| total_weight | range | info | 2282449 | 149 | [60, 90000] |
| payload | range | info | 2280924 | 1643 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_ids | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 24233 |
| oper_code | distinct_count | INFO | 92 |
| oper_name | distinct_count | INFO | 153 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 322 |
| dep_code | not_null | PASS | 100.00% |
| dep_code | distinct_count | INFO | 167 |
| dep_name | distinct_count | INFO | 173 |
| brand | distinct_count | INFO | 1973 |
| model | distinct_count | INFO | 13132 |
| vin | distinct_count | INFO | 0 |
| make_year | not_null | PASS | 100.00% |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 89 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 10 |
| body | distinct_count | INFO | 62 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3363 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 8828 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5378 |
| n_reg_new | distinct_count | INFO | 1868796 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 13514 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 1868315 |
| is_valid_plate | not_null | PASS | 100.00% |
| raw_vin | distinct_count | INFO | 0 |
| _table_ | row_count | PASS |  |