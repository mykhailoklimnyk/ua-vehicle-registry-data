# DQ Report: mvs_opendata — 2020

**Status:** PASS  
**Rows:** 1,823,904  
**Checks:** 12/16 passed, 4 failed  
**Timestamp:** 2026-03-19T14:27:21.324070

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 1736885 | 27 | [0, 20000] |
| own_weight | range | info | 1822675 | 965 | [40, 45000] |
| total_weight | range | info | 1823720 | 148 | [60, 90000] |
| payload | range | info | 1822601 | 1606 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_ids | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 22531 |
| oper_code | distinct_count | INFO | 93 |
| oper_name | distinct_count | INFO | 97 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 332 |
| dep_code | not_null | PASS | 100.00% |
| dep_code | distinct_count | INFO | 178 |
| dep_name | distinct_count | INFO | 186 |
| brand | distinct_count | INFO | 1934 |
| model | distinct_count | INFO | 12662 |
| vin | distinct_count | INFO | 0 |
| make_year | not_null | PASS | 100.00% |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 88 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 10 |
| body | distinct_count | INFO | 66 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3120 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 8887 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5118 |
| n_reg_new | distinct_count | INFO | 1698562 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 13400 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 1681041 |
| is_valid_plate | not_null | PASS | 100.00% |
| raw_vin | distinct_count | INFO | 0 |
| _table_ | row_count | PASS |  |