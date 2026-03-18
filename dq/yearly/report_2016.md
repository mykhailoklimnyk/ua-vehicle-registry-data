# DQ Report: mvs_opendata — 2016

**Status:** PASS  
**Rows:** 1,430,887  
**Checks:** 12/16 passed, 4 failed  
**Timestamp:** 2026-03-19T14:24:00.170851

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 1344718 | 22 | [0, 20000] |
| own_weight | range | info | 1430788 | 3342 | [40, 45000] |
| total_weight | range | info | 1430880 | 491 | [60, 90000] |
| payload | range | info | 1430784 | 856 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_ids | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 23215 |
| oper_code | distinct_count | INFO | 92 |
| oper_name | distinct_count | INFO | 115 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 350 |
| dep_code | not_null | PASS | 100.00% |
| dep_code | distinct_count | INFO | 161 |
| dep_name | distinct_count | INFO | 249 |
| brand | distinct_count | INFO | 1748 |
| model | distinct_count | INFO | 10975 |
| vin | distinct_count | INFO | 0 |
| make_year | not_null | PASS | 100.00% |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 83 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 9 |
| body | distinct_count | INFO | 50 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3233 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 7514 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5355 |
| n_reg_new | distinct_count | INFO | 1252000 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 10385 |
| secondary_fuel | distinct_count | INFO | 2 |
| n_reg_latin | distinct_count | INFO | 1251755 |
| is_valid_plate | not_null | PASS | 100.00% |
| raw_vin | distinct_count | INFO | 0 |
| _table_ | row_count | PASS |  |