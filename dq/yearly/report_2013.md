# DQ Report: mvs_opendata — 2013

**Status:** WARN  
**Rows:** 1,934,341  
**Checks:** 9/13 passed, 4 failed  
**Timestamp:** 2026-10-03T15:29:52.738567

## Failed Checks

| Field | Check | Severity | Total | Failed | Detail |
|-------|-------|----------|------:|-------:|--------|
| capacity | range | info | 1851789 | 53 | [0, 20000] |
| own_weight | range | info | 1865803 | 267 | [40, 45000] |
| total_weight | range | info | 1923700 | 53 | [60, 90000] |
| payload | range | info | 1861611 | 1039 | [0, 35000] |

## All Checks

| Field | Check | Status | Detail |
|-------|-------|--------|--------|
| record_id | not_null | PASS | 100.00% |
| person_type | not_null | PASS | 100.00% |
| person_type | allowed_values | PASS | ['J', 'P'] |
| reg_addr_koatuu | distinct_count | INFO | 23077 |
| oper_code | distinct_count | INFO | 101 |
| oper_name | distinct_count | INFO | 123 |
| d_reg | not_null | PASS | 100.00% |
| d_reg | distinct_count | INFO | 363 |
| dep_code | distinct_count | INFO | 414 |
| dep_name | distinct_count | INFO | 544 |
| brand | distinct_count | INFO | 1738 |
| model | distinct_count | INFO | 10430 |
| vin | column_exists | SKIP | Not published for this period — column omitted |
| make_year | range | PASS | [1900, 2027] |
| make_year | distinct_count | INFO | 81 |
| color | not_null | PASS | 100.00% |
| color | distinct_count | INFO | 11 |
| kind | not_null | PASS | 100.00% |
| kind | distinct_count | INFO | 12 |
| body | distinct_count | INFO | 53 |
| purpose | not_null | PASS | 100.00% |
| purpose | distinct_count | INFO | 3 |
| fuel | distinct_count | INFO | 4 |
| capacity | range | FAIL | [0, 20000] |
| capacity | distinct_count | INFO | 3486 |
| power_kwt | column_exists | SKIP | Not published for this period — column omitted |
| own_weight | range | FAIL | [40, 45000] |
| own_weight | distinct_count | INFO | 7582 |
| total_weight | range | FAIL | [60, 90000] |
| total_weight | distinct_count | INFO | 5956 |
| n_reg_new | distinct_count | INFO | 1682496 |
| payload | range | FAIL | [0, 35000] |
| payload | distinct_count | INFO | 9912 |
| secondary_fuel | distinct_count | INFO | 2 |
| n_reg_latin | distinct_count | INFO | 1681499 |
| raw_vin | column_exists | SKIP | Not published for this period — column omitted |
| is_valid_vin | column_exists | SKIP | Not published for this period — column omitted |
| _table_ | row_count | PASS |  |