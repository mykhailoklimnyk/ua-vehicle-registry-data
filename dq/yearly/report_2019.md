# DQ Report: mvs_opendata — 2019

**Status:** WARN  
**Rows:** 2,078,068  
**Checks:** 9/13 passed, 4 failed  
**Timestamp:** 2026-10-03T15:57:57.112514

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 1971601 | 27 | [0, 20000] |
| own_weight | range | info | 2076517 | 621 | [40, 45000] |
| total_weight | range | info | 2077988 | 133 | [60, 90000] |
| payload | range | info | 2073698 | 1540 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_id | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 24228 |
| oper_code | distinct_count | INFO | 91 |
| oper_name | distinct_count | INFO | 99 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 318 |
| dep_code | distinct_count | INFO | 165 |
| dep_name | distinct_count | INFO | 165 |
| brand | distinct_count | INFO | 1887 |
| model | distinct_count | INFO | 13079 |
| vin | column_exists | SKIP | Not published for this period — column omitted |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 89 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 12 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 10 |
| body | distinct_count | INFO | 65 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3363 |
| power_kwt | column_exists | SKIP | Not published for this period — column omitted |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 9018 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5377 |
| n_reg_new | distinct_count | INFO | 1866009 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 13462 |
| secondary_fuel | distinct_count | INFO | 3 |
| n_reg_latin | distinct_count | INFO | 1865532 |
| raw_vin | column_exists | SKIP | Not published for this period — column omitted |
| is_valid_vin | column_exists | SKIP | Not published for this period — column omitted |
| _table_ | row_count | PASS |  |