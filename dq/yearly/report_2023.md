# DQ Report: mvs_opendata — 2023

**Status:** WARN  
**Rows:** 2,118,178  
**Checks:** 10/14 passed, 4 failed  
**Timestamp:** 2026-10-03T10:19:10.399609

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 1943937 | 13 | [0, 20000] |
| own_weight | range | info | 2116743 | 1102 | [40, 45000] |
| total_weight | range | info | 2117622 | 214 | [60, 90000] |
| payload | range | info | 2114837 | 4098 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_id | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 23727 |
| oper_code | distinct_count | INFO | 106 |
| oper_name | distinct_count | INFO | 106 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 365 |
| dep_code | distinct_count | INFO | 165 |
| dep_name | distinct_count | INFO | 165 |
| brand | distinct_count | INFO | 2152 |
| model | distinct_count | INFO | 15158 |
| vin | distinct_count | INFO | 1651555 |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 89 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 10 |
| body | distinct_count | INFO | 70 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3298 |
| power_kwt | range | PASS | [0, 2000] |
| power_kwt | distinct_count | INFO | 596 |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 9863 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5173 |
| n_reg_new | distinct_count | INFO | 1831728 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 15228 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 1831676 |
| raw_vin | distinct_count | INFO | 1651563 |
| _table_ | row_count | PASS |  |