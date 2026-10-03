# DQ Report: mvs_opendata — 2022

**Status:** WARN  
**Rows:** 1,745,152  
**Checks:** 10/14 passed, 4 failed  
**Timestamp:** 2026-10-03T10:13:05.067515

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 1625457 | 17 | [0, 20000] |
| own_weight | range | info | 1744163 | 767 | [40, 45000] |
| total_weight | range | info | 1744768 | 141 | [60, 90000] |
| payload | range | info | 1742073 | 3137 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_id | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 23412 |
| oper_code | distinct_count | INFO | 97 |
| oper_name | distinct_count | INFO | 99 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 347 |
| dep_code | distinct_count | INFO | 172 |
| dep_name | distinct_count | INFO | 172 |
| brand | distinct_count | INFO | 1957 |
| model | distinct_count | INFO | 12995 |
| vin | distinct_count | INFO | 1400085 |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 88 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 10 |
| body | distinct_count | INFO | 68 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3033 |
| power_kwt | range | PASS | [0, 2000] |
| power_kwt | distinct_count | INFO | 529 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 9223 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 4771 |
| n_reg_new | distinct_count | INFO | 1631629 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 14281 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 1631584 |
| raw_vin | distinct_count | INFO | 1400088 |
| _table_ | row_count | PASS |  |