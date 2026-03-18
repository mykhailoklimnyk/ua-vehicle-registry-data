# DQ Report: mvs_opendata — 2023

**Status:** PASS  
**Rows:** 2,154,518  
**Checks:** 12/16 passed, 4 failed  
**Timestamp:** 2026-03-19T14:29:39.320553

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 2043063 | 14 | [0, 20000] |
| own_weight | range | info | 2152756 | 1445 | [40, 45000] |
| total_weight | range | info | 2153681 | 313 | [60, 90000] |
| payload | range | info | 2152484 | 4163 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_ids | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 23726 |
| oper_code | distinct_count | INFO | 106 |
| oper_name | distinct_count | INFO | 113 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 365 |
| dep_code | not_null | PASS | 100.00% |
| dep_code | distinct_count | INFO | 165 |
| dep_name | distinct_count | INFO | 168 |
| brand | distinct_count | INFO | 2238 |
| model | distinct_count | INFO | 15161 |
| vin | distinct_count | INFO | 1651582 |
| make_year | not_null | PASS | 100.00% |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 89 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 10 |
| body | distinct_count | INFO | 67 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3298 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 9756 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5173 |
| n_reg_new | distinct_count | INFO | 1852943 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 15270 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 1831699 |
| is_valid_plate | not_null | PASS | 100.00% |
| raw_vin | distinct_count | INFO | 1651594 |
| _table_ | row_count | PASS |  |