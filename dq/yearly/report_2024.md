# DQ Report: mvs_opendata — 2024

**Status:** WARN  
**Rows:** 2,338,951  
**Checks:** 11/15 passed, 4 failed  
**Timestamp:** 2026-10-03T01:05:34.319187

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 2153099 | 15 | [0, 20000] |
| own_weight | range | info | 2337272 | 1798 | [40, 45000] |
| total_weight | range | info | 2337938 | 333 | [60, 90000] |
| payload | range | info | 2334864 | 2673 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_id | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 24028 |
| oper_code | distinct_count | INFO | 108 |
| oper_name | distinct_count | INFO | 109 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 366 |
| dep_code | distinct_count | INFO | 156 |
| dep_name | distinct_count | INFO | 156 |
| brand | distinct_count | INFO | 2081 |
| model | distinct_count | INFO | 14910 |
| vin | distinct_count | INFO | 1832536 |
| make_year | not_null | PASS | 100.00% |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 91 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 10 |
| body | distinct_count | INFO | 72 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3280 |
| power_kwt | range | PASS | [0, 2000] |
| power_kwt | distinct_count | INFO | 675 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 9854 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5162 |
| n_reg_new | distinct_count | INFO | 1924805 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 14670 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 1924778 |
| raw_vin | distinct_count | INFO | 1832542 |
| _table_ | row_count | PASS |  |