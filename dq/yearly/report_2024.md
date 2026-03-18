# DQ Report: mvs_opendata — 2024

**Status:** PASS  
**Rows:** 2,347,761  
**Checks:** 12/16 passed, 4 failed  
**Timestamp:** 2026-03-19T14:29:24.906912

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 2259604 | 15 | [0, 20000] |
| own_weight | range | info | 2346076 | 1863 | [40, 45000] |
| total_weight | range | info | 2346748 | 334 | [60, 90000] |
| payload | range | info | 2345794 | 2688 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_ids | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 24026 |
| oper_code | distinct_count | INFO | 114 |
| oper_name | distinct_count | INFO | 116 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 366 |
| dep_code | not_null | PASS | 100.00% |
| dep_code | distinct_count | INFO | 158 |
| dep_name | distinct_count | INFO | 162 |
| brand | distinct_count | INFO | 2177 |
| model | distinct_count | INFO | 14908 |
| vin | distinct_count | INFO | 1832743 |
| make_year | not_null | PASS | 100.00% |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 91 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 10 |
| body | distinct_count | INFO | 68 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3281 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 9737 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5163 |
| n_reg_new | distinct_count | INFO | 1933916 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 14713 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 1924787 |
| is_valid_plate | not_null | PASS | 100.00% |
| raw_vin | distinct_count | INFO | 1832752 |
| _table_ | row_count | PASS |  |