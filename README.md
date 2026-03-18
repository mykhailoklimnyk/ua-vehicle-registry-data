**[Українська](README.uk.md)** | **English**

---

# UA Vehicle Registry — Data Quality Edition

[![Sponsored by automoto.ai](https://img.shields.io/badge/Sponsored%20by-automoto.ai-blue)](https://automoto.ai/open-data/ua-vehicle-registry)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightblue.svg)](https://creativecommons.org/licenses/by/4.0/)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19099461.svg)](https://doi.org/10.5281/zenodo.19099461)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0005--5463--6981-green.svg)](https://orcid.org/0009-0005-5463-6981)
[![Wikidata](https://img.shields.io/badge/Wikidata-Q138717134-006699.svg)](https://www.wikidata.org/wiki/Q138717134)

A normalized and data-quality enhanced derivative of a Ukrainian public-sector open dataset, designed for reproducible analytical use.

## Source Dataset

| Field | Value |
|---|---|
| **Title** | Відомості про транспортні засоби та їх власників |
| **Publisher** | Міністерство внутрішніх справ України |
| **Portal** | [data.gov.ua](https://data.gov.ua/dataset/06779371-308f-42d7-895e-5a39833375f0) |
| **Dataset ID** | `06779371-308f-42d7-895e-5a39833375f0` |
| **License** | [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) |
| **Update frequency** | Monthly |

## Why This Project Exists

The source dataset on data.gov.ua spans **2013–2026** and totals roughly **~50 GB** of raw CSV files across yearly ZIP archives. While this is one of the most valuable open datasets in Ukraine, its raw form is extremely difficult to work with:

- **Broken encoding** — files mix Windows-1251 and UTF-8 (with/without BOM), producing mojibake (garbled characters) in Ukrainian text
- **Inconsistent schema** — column names, order, and casing change between years
- **Mixed delimiters** — some files use `;`, others `,`; some have malformed delimiter usage
- **Duplicate records** — exact duplicates appear across and within files
- **Invalid data types** — numeric fields stored as text, dates in multiple formats (`DD.MM.YYYY`, `YYYY-MM-DD`, and more)
- **Leading zeros stripped** — KOATUU codes and other zero-padded fields were truncated (likely by opening CSVs in Excel), turning `0123456789` into `123456789`
- **Inconsistent brand/model naming** — the same vehicle brand or model spelled dozens of different ways
- **Placeholder values instead of nulls** — `"невизначено"`, `"Не визначено"`, literal `"NULL"` text, empty strings used interchangeably
- **Mixed numeric formats** — weight, capacity and other numeric fields contain dots, commas, slash-separated ranges (`1500/2000`), embedded units (`1500 кг`), and other non-numeric artifacts
- **Orphan KOATUU codes** — 100+ region codes not found in any publicly available KOATUU dictionary, requiring manual reconciliation (see [ua-administrative-codes](https://github.com/mykhailoklimnyk/ua-administrative-codes) for the most complete KOATUU dictionary)

This project resolves all of the above and delivers clean, typed, analysis-ready snapshots.

See [docs/DATA_QUALITY_REPORT.md](docs/DATA_QUALITY_REPORT.md) for the full catalog of issues found.

## What This Project Does

This project provides **data-quality improvements** to the publicly available Ukrainian vehicle registry dataset:

- **Encoding normalization** — consistent UTF-8 encoding across all files
- **Schema stabilization** — unified column names, types, and order across yearly snapshots
- **Deduplication** — removal of exact duplicate records
- **Type coercion** — dates, integers, and categorical fields cast to proper types
- **Brand & model normalization** — standardized naming (source data has inconsistent spelling)
- **Error corrections** — fixing obvious data-entry mistakes in KOATUU codes, registration plates, and other fields
- **Reference dictionaries** — added lookup tables based on public sources (e.g., service center addresses)
- **Parquet format** — compressed, columnar format for efficient analytical queries (recommended). A CSV version is also available.

### What This Project Does NOT Do

- **No non-public enrichment.** All added dictionaries are based exclusively on publicly available sources.
- **This is not an official registry.** It is a curated, normalized derivative of openly published data.
- **This is not a law enforcement or investigative tool.**

> Extended data (e.g., corrected body types) is available separately via [automoto.ai](https://automoto.ai).

## Data Publishing Model

Snapshots are published monthly in Apache Parquet (recommended) and CSV formats.

| Channel | Description |
|---|---|
| [GitHub Releases](../../releases) | Parquet and CSV snapshots attached to releases |
| [automoto.ai](https://automoto.ai) | Data hub with interactive access and extended data |

- Schema is stable across monthly snapshots within a major version.
- Data files are **not** stored in Git history.
- Each release includes a **Data Quality report** (DQ) with validation results.

## Downloads

<!-- DOWNLOADS:START -->
### Yearly

| Year | Release | CSV | Parquet | DQ Report |
|------|---------|-----|---------|-----------|
| 2013 | [v2013.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2013/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2013/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2013/report.md) |

### Monthly

<details><summary>2013</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2013-01 | [v2013.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/01/report.md) |
| 2013-02 | [v2013.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/02/report.md) |
| 2013-03 | [v2013.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/03/report.md) |
| 2013-04 | [v2013.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/04/report.md) |
| 2013-05 | [v2013.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/05/report.md) |
| 2013-06 | [v2013.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/06/report.md) |
| 2013-07 | [v2013.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/07/report.md) |
| 2013-08 | [v2013.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/08/report.md) |
| 2013-09 | [v2013.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/09/report.md) |
| 2013-10 | [v2013.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/10/report.md) |
| 2013-11 | [v2013.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/11/report.md) |
| 2013-12 | [v2013.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2013/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2013/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2013/12/report.md) |

</details>


### MinIO (Mirror)

All files are also available at: [https://m1.automoto.ua/opendata-hub/mvs_opendata](https://m1.automoto.ua/opendata-hub/mvs_opendata)

*Last updated: 2026-03-18 21:04 UTC*
<!-- DOWNLOADS:END -->

## Data Quality Reports

<!-- DQ_REPORTS:START -->
### Yearly

| Year | DQ Report |
|------|-----------|
| 2013 | [report_2013.md](dq/yearly/report_2013.md) |

### Monthly

<details><summary>2013</summary>

| Month | DQ Report |
|-------|-----------|
| 2013-01 | [report_2013_01.md](dq/monthly/2013/report_2013_01.md) |
| 2013-02 | [report_2013_02.md](dq/monthly/2013/report_2013_02.md) |
| 2013-03 | [report_2013_03.md](dq/monthly/2013/report_2013_03.md) |
| 2013-04 | [report_2013_04.md](dq/monthly/2013/report_2013_04.md) |
| 2013-05 | [report_2013_05.md](dq/monthly/2013/report_2013_05.md) |
| 2013-06 | [report_2013_06.md](dq/monthly/2013/report_2013_06.md) |
| 2013-07 | [report_2013_07.md](dq/monthly/2013/report_2013_07.md) |
| 2013-08 | [report_2013_08.md](dq/monthly/2013/report_2013_08.md) |
| 2013-09 | [report_2013_09.md](dq/monthly/2013/report_2013_09.md) |
| 2013-10 | [report_2013_10.md](dq/monthly/2013/report_2013_10.md) |
| 2013-11 | [report_2013_11.md](dq/monthly/2013/report_2013_11.md) |
| 2013-12 | [report_2013_12.md](dq/monthly/2013/report_2013_12.md) |

</details>

*Last updated: 2026-03-18 21:04 UTC*
<!-- DQ_REPORTS:END -->

## Repository Structure

```
/docs
    METHODOLOGY.md              # Data processing methodology (EN)
    METHODOLOGY.uk.md           # Data processing methodology (UK)
    DATA_QUALITY_REPORT.md      # Quality metrics and findings (EN)
    DATA_QUALITY_REPORT.uk.md   # Quality metrics and findings (UK)
    PUBLISHING.md               # Publishing & distribution strategy
/dq
    /monthly                    # Per-month DQ reports (auto-committed on release)
        report_2013_01.md
        report_2013_02.md
        ...
    /yearly                     # Per-year DQ reports (auto-committed on release)
        report_2013.md
        report_2014.md
        ...
/schema
    schema.md                   # Human-readable schema documentation (EN)
    schema.uk.md                # Human-readable schema documentation (UK)
    schema.json                 # Machine-readable schema definition
CITATION.cff                    # Citation metadata (GitHub cite button)
LICENSE                         # CC BY 4.0 license text
README.md                       # This file (English)
README.uk.md                    # Ukrainian version
datapackage.json                # Frictionless Data package descriptor
codemeta.json                   # CodeMeta dataset metadata
.zenodo.json                    # Zenodo deposit metadata
```

## Attribution

This dataset is derived from open data published by the **Ministry of Internal Affairs of Ukraine** on [data.gov.ua](https://data.gov.ua) under the [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) license.

**Mandatory attribution (required by license):**

> Source: "Відомості про транспортні засоби та їх власників" — Міністерство внутрішніх справ України, published on data.gov.ua.
> https://data.gov.ua/dataset/06779371-308f-42d7-895e-5a39833375f0

### Citing This Derivative Dataset

If you use this curated edition in research or analysis, please cite it as:

> Klimnyk. (2026). UA Vehicle Registry — Data Quality Edition [Dataset]. GitHub. https://github.com/mykhailoklimnyk/ua-vehicle-registry-data

A machine-readable citation is available via the `CITATION.cff` file (enables the GitHub "Cite this repository" button).

## License

This derivative dataset is distributed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You are free to share and adapt the material for any purpose, including commercially, as long as you provide appropriate attribution.

## Contributing

This project welcomes contributions! If you found a data error, want to improve dictionaries, or have suggestions — please open a **Pull Request** or create an **Issue**.

Especially valuable contributions:
- Brand/model name corrections
- Dictionary improvements (service center addresses, KOATUU codes)
- Reports of duplicates or anomalies
- Documentation improvements

## Disclaimer

This project is an independent data-quality initiative. It is **not affiliated with, endorsed by, or representative of** the Ministry of Internal Affairs of Ukraine or any other government entity. The data is provided "as is" without warranty of any kind. Users are solely responsible for how they use the data.

## Legal Basis of Source Data

The source dataset is published pursuant to:
- Закон України «Про дорожній рух»
- Постанова КМУ від 25.03.2016 № 260 «Деякі питання надання інформації про зареєстровані транспортні засоби та їх власників»
- Закон України «Про доступ до публічної інформації»
