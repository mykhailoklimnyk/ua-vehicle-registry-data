# DQ Report: mvs_opendata — 2017

**Status:** PASS  
**Rows:** 1,416,112  
**Checks:** 12/16 passed, 4 failed  
**Timestamp:** 2026-03-19T14:24:47.990491

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 1323775 | 12 | [0, 20000] |
| own_weight | range | info | 1415966 | 1263 | [40, 45000] |
| total_weight | range | info | 1416064 | 204 | [60, 90000] |
| payload | range | info | 1415956 | 996 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_ids | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 23225 |
| oper_code | distinct_count | INFO | 104 |
| oper_name | distinct_count | INFO | 131 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 338 |
| dep_code | not_null | PASS | 100.00% |
| dep_code | distinct_count | INFO | 169 |
| dep_name | distinct_count | INFO | 312 |
| brand | distinct_count | INFO | 1834 |
| model | distinct_count | INFO | 11773 |
| vin | distinct_count | INFO | 0 |
| make_year | not_null | PASS | 100.00% |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 84 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 10 |
| body | distinct_count | INFO | 53 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3182 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 7950 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5278 |
| n_reg_new | distinct_count | INFO | 1261762 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 11331 |
| secondary_fuel | distinct_count | INFO | 2 |
| n_reg_latin | distinct_count | INFO | 1261592 |
| is_valid_plate | not_null | PASS | 100.00% |
| raw_vin | distinct_count | INFO | 0 |
| _table_ | row_count | PASS |  |