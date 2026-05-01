**Українська** | **[English](README.md)**

---

# Реєстр ТЗ України — Data Quality Edition

[![Sponsored by automoto.ai](https://img.shields.io/badge/Sponsored%20by-automoto.ai-blue)](https://automoto.ai/open-data/ua-vehicle-registry)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightblue.svg)](https://creativecommons.org/licenses/by/4.0/)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.19099441.svg)](https://doi.org/10.5281/zenodo.19099441)
[![ORCID](https://img.shields.io/badge/ORCID-0009--0005--5463--6981-green.svg)](https://orcid.org/0009-0005-5463-6981)
[![Wikidata](https://img.shields.io/badge/Wikidata-Q138717134-006699.svg)](https://www.wikidata.org/wiki/Q138717134)

Нормалізована та покращена за якістю похідна версія відкритого набору даних українського державного сектору, створена для відтворюваного аналітичного використання.

## Джерело даних

| Поле | Значення |
|---|---|
| **Назва** | Відомості про транспортні засоби та їх власників |
| **Розпорядник** | Міністерство внутрішніх справ України |
| **Портал** | [data.gov.ua](https://data.gov.ua/dataset/06779371-308f-42d7-895e-5a39833375f0) |
| **ID набору** | `06779371-308f-42d7-895e-5a39833375f0` |
| **Ліцензія** | [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/) |
| **Частота оновлення** | Щомісяця |

## Навіщо цей проєкт

Вихідний набір даних на data.gov.ua охоплює **2013–2026 роки** і становить приблизно **~50 ГБ** необроблених CSV-файлів у ZIP-архівах за кожен рік. Це один із найцінніших відкритих наборів даних в Україні, але в сирому вигляді з ним надзвичайно складно працювати:

- **Зламане кодування** — файли змішують Windows-1251 та UTF-8 (з BOM і без), що дає крякозябри в українському тексті
- **Непослідовна схема** — назви стовпців, їх порядок та регістр змінюються між роками
- **Різні роздільники** — частина файлів використовує `;`, інша `,`; подекуди роздільники використані невірно
- **Дублікати записів** — точні дублі зустрічаються як між файлами, так і всередині них
- **Невалідні типи даних** — числові поля збережені як текст, дати в різних форматах (`DD.MM.YYYY`, `YYYY-MM-DD` тощо)
- **Втрачені провідні нулі** — коди КОАТУУ та інші поля з нулями на початку були обрізані (ймовірно, при відкритті CSV в Excel), перетворивши `0123456789` на `123456789`
- **Неконсистентні назви марок/моделей** — одна й та сама марка чи модель записана десятками різних способів
- **Плейсхолдери замість null** — `"невизначено"`, `"Не визначено"`, літеральний текст `"NULL"`, порожні рядки використовуються як попало
- **Змішані числові формати** — поля ваги, об’єму та інші числові поля містять крапки, коми, діапазони через слеш (`1500/2000`), вбудовані одиниці (`1500 кг`) та інші нечислові артефакти
- **Сирітські коди КОАТУУ** — 100+ кодів регіонів, яких немає в жодному публічному словнику КОАТУУ, що потребують ручного узгодження (див. [ua-administrative-codes](https://github.com/mykhailoklimnyk/ua-administrative-codes) — найповніший довідник КОАТУУ)

Цей проєкт вирішує всі ці проблеми і видає чисті, типізовані, готові до аналізу знімки.

Дивіться [docs/DATA_QUALITY_REPORT.uk.md](docs/DATA_QUALITY_REPORT.uk.md) для повного каталогу знайдених проблем.

## Що робить цей проєкт

Цей проєкт забезпечує **покращення якості даних** відкритого набору даних реєстру транспортних засобів України:

- **Нормалізація кодування** — єдине кодування UTF-8 для всіх файлів
- **Стабілізація схеми** — уніфіковані назви стовпців, типи та порядок у всіх річних знімках
- **Дедуплікація** — видалення точних дублікатів записів
- **Приведення типів** — дати, цілі числа та категоріальні поля приведені до правильних типів
- **Нормалізація марок і моделей** — приведення до єдиної форми (в оригіналі записано хто як хоче)
- **Виправлення помилок** — корекція очевидних помилок введення в КОАТУУ, номерних знаках та інших полях
- **Довідники** — додано словники на основі публічних джерел (адреси ТСЦ/департаментів тощо)
- **Формат Parquet** — стиснутий, колонковий формат для ефективних аналітичних запитів (рекомендовано). Також доступна версія у форматі CSV.

### Чого цей проєкт НЕ робить

- **Непублічне збагачення не виконується.** Усі додані довідники базуються виключно на відкритих джерелах.
- **Це не офіційний реєстр.** Це курована, нормалізована похідна відкрито опублікованих даних.
- **Це не інструмент правоохоронних органів.**

> Розширені дані (наприклад, уточнений тип кузова) доступні окремо через [automoto.ai](https://automoto.ai).

## Модель публікації даних

Знімки публікуються щомісяця у форматах Apache Parquet (рекомендовано) та CSV.

| Канал | Опис |
|---|---|
| [GitHub Releases](../../releases) | Parquet та CSV знімки, прикріплені до релізів |
| [automoto.ai](https://automoto.ai/open-data/ua-vehicle-registry) | Data hub з інтерактивним доступом та розширеними даними |

- Схема стабільна в межах мажорної версії.
- Файли даних **не** зберігаються в історії Git.
- Кожен реліз містить **звіт Data Quality** (DQ) з результатами валідації.

## Завантаження

<!-- DOWNLOADS:START -->
### Yearly

| Year | Release | CSV | Parquet | DQ Report |
|------|---------|-----|---------|-----------|
| 2013 | [v2013.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2013.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2013/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2013/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2013/report.md) |
| 2014 | [v2014.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2014/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2014/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2014/report.md) |
| 2015 | [v2015.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2015/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2015/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2015/report.md) |
| 2016 | [v2016.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2016/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2016/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2016/report.md) |
| 2017 | [v2017.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2017/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2017/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2017/report.md) |
| 2018 | [v2018.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2018/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2018/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2018/report.md) |
| 2019 | [v2019.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2019/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2019/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2019/report.md) |
| 2020 | [v2020.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2020/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2020/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2020/report.md) |
| 2021 | [v2021.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2021/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2021/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2021/report.md) |
| 2022 | [v2022.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2022/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2022/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2022/report.md) |
| 2023 | [v2023.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2023/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2023/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2023/report.md) |
| 2024 | [v2024.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2024/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2024/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2024/report.md) |
| 2025 | [v2025.full](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.full) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/yearly/2025/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/yearly/2025/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/yearly/2025/report.md) |

### Monthly

<details><summary>2026</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2026-01 | [v2026.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2026.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2026/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2026/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2026/01/report.md) |
| 2026-02 | [v2026.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2026.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2026/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2026/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2026/02/report.md) |
| 2026-03 | [v2026.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2026.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2026/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2026/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2026/03/report.md) |
| 2026-04 | [v2026.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2026.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2026/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2026/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2026/04/report.md) |

</details>

<details><summary>2025</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2025-01 | [v2025.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/01/report.md) |
| 2025-02 | [v2025.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/02/report.md) |
| 2025-03 | [v2025.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/03/report.md) |
| 2025-04 | [v2025.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/04/report.md) |
| 2025-05 | [v2025.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/05/report.md) |
| 2025-06 | [v2025.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/06/report.md) |
| 2025-07 | [v2025.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/07/report.md) |
| 2025-08 | [v2025.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/08/report.md) |
| 2025-09 | [v2025.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/09/report.md) |
| 2025-10 | [v2025.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/10/report.md) |
| 2025-11 | [v2025.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/11/report.md) |
| 2025-12 | [v2025.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2025.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2025/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2025/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2025/12/report.md) |

</details>

<details><summary>2024</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2024-01 | [v2024.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/01/report.md) |
| 2024-02 | [v2024.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/02/report.md) |
| 2024-03 | [v2024.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/03/report.md) |
| 2024-04 | [v2024.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/04/report.md) |
| 2024-05 | [v2024.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/05/report.md) |
| 2024-06 | [v2024.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/06/report.md) |
| 2024-07 | [v2024.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/07/report.md) |
| 2024-08 | [v2024.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/08/report.md) |
| 2024-09 | [v2024.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/09/report.md) |
| 2024-10 | [v2024.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/10/report.md) |
| 2024-11 | [v2024.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/11/report.md) |
| 2024-12 | [v2024.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2024.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2024/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2024/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2024/12/report.md) |

</details>

<details><summary>2023</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2023-01 | [v2023.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/01/report.md) |
| 2023-02 | [v2023.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/02/report.md) |
| 2023-03 | [v2023.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/03/report.md) |
| 2023-04 | [v2023.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/04/report.md) |
| 2023-05 | [v2023.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/05/report.md) |
| 2023-06 | [v2023.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/06/report.md) |
| 2023-07 | [v2023.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/07/report.md) |
| 2023-08 | [v2023.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/08/report.md) |
| 2023-09 | [v2023.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/09/report.md) |
| 2023-10 | [v2023.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/10/report.md) |
| 2023-11 | [v2023.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/11/report.md) |
| 2023-12 | [v2023.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2023.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2023/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2023/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2023/12/report.md) |

</details>

<details><summary>2022</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2022-01 | [v2022.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/01/report.md) |
| 2022-02 | [v2022.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/02/report.md) |
| 2022-03 | [v2022.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/03/report.md) |
| 2022-04 | [v2022.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/04/report.md) |
| 2022-05 | [v2022.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/05/report.md) |
| 2022-06 | [v2022.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/06/report.md) |
| 2022-07 | [v2022.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/07/report.md) |
| 2022-08 | [v2022.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/08/report.md) |
| 2022-09 | [v2022.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/09/report.md) |
| 2022-10 | [v2022.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/10/report.md) |
| 2022-11 | [v2022.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/11/report.md) |
| 2022-12 | [v2022.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2022.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2022/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2022/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2022/12/report.md) |

</details>

<details><summary>2021</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2021-01 | [v2021.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/01/report.md) |
| 2021-02 | [v2021.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/02/report.md) |
| 2021-03 | [v2021.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/03/report.md) |
| 2021-04 | [v2021.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/04/report.md) |
| 2021-05 | [v2021.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/05/report.md) |
| 2021-06 | [v2021.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/06/report.md) |
| 2021-07 | [v2021.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/07/report.md) |
| 2021-08 | [v2021.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/08/report.md) |
| 2021-09 | [v2021.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/09/report.md) |
| 2021-10 | [v2021.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/10/report.md) |
| 2021-11 | [v2021.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/11/report.md) |
| 2021-12 | [v2021.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2021.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2021/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2021/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2021/12/report.md) |

</details>

<details><summary>2020</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2020-01 | [v2020.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/01/report.md) |
| 2020-02 | [v2020.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/02/report.md) |
| 2020-03 | [v2020.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/03/report.md) |
| 2020-04 | [v2020.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/04/report.md) |
| 2020-05 | [v2020.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/05/report.md) |
| 2020-06 | [v2020.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/06/report.md) |
| 2020-07 | [v2020.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/07/report.md) |
| 2020-08 | [v2020.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/08/report.md) |
| 2020-09 | [v2020.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/09/report.md) |
| 2020-10 | [v2020.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/10/report.md) |
| 2020-11 | [v2020.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/11/report.md) |
| 2020-12 | [v2020.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2020.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2020/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2020/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2020/12/report.md) |

</details>

<details><summary>2019</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2019-01 | [v2019.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/01/report.md) |
| 2019-02 | [v2019.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/02/report.md) |
| 2019-03 | [v2019.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/03/report.md) |
| 2019-04 | [v2019.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/04/report.md) |
| 2019-05 | [v2019.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/05/report.md) |
| 2019-06 | [v2019.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/06/report.md) |
| 2019-07 | [v2019.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/07/report.md) |
| 2019-08 | [v2019.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/08/report.md) |
| 2019-09 | [v2019.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/09/report.md) |
| 2019-10 | [v2019.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/10/report.md) |
| 2019-11 | [v2019.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/11/report.md) |
| 2019-12 | [v2019.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2019.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2019/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2019/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2019/12/report.md) |

</details>

<details><summary>2018</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2018-01 | [v2018.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/01/report.md) |
| 2018-02 | [v2018.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/02/report.md) |
| 2018-03 | [v2018.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/03/report.md) |
| 2018-04 | [v2018.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/04/report.md) |
| 2018-05 | [v2018.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/05/report.md) |
| 2018-06 | [v2018.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/06/report.md) |
| 2018-07 | [v2018.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/07/report.md) |
| 2018-08 | [v2018.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/08/report.md) |
| 2018-09 | [v2018.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/09/report.md) |
| 2018-10 | [v2018.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/10/report.md) |
| 2018-11 | [v2018.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/11/report.md) |
| 2018-12 | [v2018.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2018.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2018/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2018/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2018/12/report.md) |

</details>

<details><summary>2017</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2017-01 | [v2017.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/01/report.md) |
| 2017-02 | [v2017.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/02/report.md) |
| 2017-03 | [v2017.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/03/report.md) |
| 2017-04 | [v2017.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/04/report.md) |
| 2017-05 | [v2017.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/05/report.md) |
| 2017-06 | [v2017.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/06/report.md) |
| 2017-07 | [v2017.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/07/report.md) |
| 2017-08 | [v2017.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/08/report.md) |
| 2017-09 | [v2017.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/09/report.md) |
| 2017-10 | [v2017.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/10/report.md) |
| 2017-11 | [v2017.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/11/report.md) |
| 2017-12 | [v2017.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2017.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2017/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2017/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2017/12/report.md) |

</details>

<details><summary>2016</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2016-01 | [v2016.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/01/report.md) |
| 2016-02 | [v2016.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/02/report.md) |
| 2016-03 | [v2016.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/03/report.md) |
| 2016-04 | [v2016.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/04/report.md) |
| 2016-05 | [v2016.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/05/report.md) |
| 2016-06 | [v2016.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/06/report.md) |
| 2016-07 | [v2016.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/07/report.md) |
| 2016-08 | [v2016.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/08/report.md) |
| 2016-09 | [v2016.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/09/report.md) |
| 2016-10 | [v2016.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/10/report.md) |
| 2016-11 | [v2016.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/11/report.md) |
| 2016-12 | [v2016.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2016.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2016/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2016/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2016/12/report.md) |

</details>

<details><summary>2015</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2015-01 | [v2015.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/01/report.md) |
| 2015-02 | [v2015.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/02/report.md) |
| 2015-03 | [v2015.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/03/report.md) |
| 2015-04 | [v2015.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/04/report.md) |
| 2015-05 | [v2015.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/05/report.md) |
| 2015-06 | [v2015.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/06/report.md) |
| 2015-07 | [v2015.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/07/report.md) |
| 2015-08 | [v2015.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/08/report.md) |
| 2015-09 | [v2015.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/09/report.md) |
| 2015-10 | [v2015.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/10/report.md) |
| 2015-11 | [v2015.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/11/report.md) |
| 2015-12 | [v2015.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2015.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2015/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2015/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2015/12/report.md) |

</details>

<details><summary>2014</summary>

| Month | Release | CSV | Parquet | DQ Report |
|-------|---------|-----|---------|-----------|
| 2014-01 | [v2014.01](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.01) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/01/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/01/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/01/report.md) |
| 2014-02 | [v2014.02](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.02) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/02/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/02/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/02/report.md) |
| 2014-03 | [v2014.03](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.03) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/03/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/03/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/03/report.md) |
| 2014-04 | [v2014.04](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.04) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/04/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/04/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/04/report.md) |
| 2014-05 | [v2014.05](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.05) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/05/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/05/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/05/report.md) |
| 2014-06 | [v2014.06](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.06) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/06/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/06/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/06/report.md) |
| 2014-07 | [v2014.07](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.07) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/07/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/07/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/07/report.md) |
| 2014-08 | [v2014.08](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.08) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/08/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/08/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/08/report.md) |
| 2014-09 | [v2014.09](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.09) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/09/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/09/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/09/report.md) |
| 2014-10 | [v2014.10](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.10) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/10/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/10/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/10/report.md) |
| 2014-11 | [v2014.11](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.11) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/11/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/11/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/11/report.md) |
| 2014-12 | [v2014.12](https://github.com/mykhailoklimnyk/ua-vehicle-registry-data-quality/releases/tag/v2014.12) | [CSV](https://m1.automoto.ua/opendata-hub/mvs_opendata/csv/monthly/2014/12/data.csv) | [Parquet](https://m1.automoto.ua/opendata-hub/mvs_opendata/parquet/monthly/2014/12/data.parquet) | [DQ](https://m1.automoto.ua/opendata-hub/mvs_opendata/dq/monthly/2014/12/report.md) |

</details>

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

*Last updated: 2026-05-01 21:52 UTC*
<!-- DOWNLOADS:END -->

## Звіти Data Quality

<!-- DQ_REPORTS:START -->
### Yearly

| Year | DQ Report |
|------|-----------|
| 2013 | [report_2013.md](dq/yearly/report_2013.md) |
| 2014 | [report_2014.md](dq/yearly/report_2014.md) |
| 2015 | [report_2015.md](dq/yearly/report_2015.md) |
| 2016 | [report_2016.md](dq/yearly/report_2016.md) |
| 2017 | [report_2017.md](dq/yearly/report_2017.md) |
| 2018 | [report_2018.md](dq/yearly/report_2018.md) |
| 2019 | [report_2019.md](dq/yearly/report_2019.md) |
| 2020 | [report_2020.md](dq/yearly/report_2020.md) |
| 2021 | [report_2021.md](dq/yearly/report_2021.md) |
| 2022 | [report_2022.md](dq/yearly/report_2022.md) |
| 2023 | [report_2023.md](dq/yearly/report_2023.md) |
| 2024 | [report_2024.md](dq/yearly/report_2024.md) |
| 2025 | [report_2025.md](dq/yearly/report_2025.md) |

### Monthly

<details><summary>2026</summary>

| Month | DQ Report |
|-------|-----------|
| 2026-01 | [report_2026_01.md](dq/monthly/2026/report_2026_01.md) |
| 2026-02 | [report_2026_02.md](dq/monthly/2026/report_2026_02.md) |
| 2026-03 | [report_2026_03.md](dq/monthly/2026/report_2026_03.md) |
| 2026-04 | [report_2026_04.md](dq/monthly/2026/report_2026_04.md) |

</details>

<details><summary>2025</summary>

| Month | DQ Report |
|-------|-----------|
| 2025-01 | [report_2025_01.md](dq/monthly/2025/report_2025_01.md) |
| 2025-02 | [report_2025_02.md](dq/monthly/2025/report_2025_02.md) |
| 2025-03 | [report_2025_03.md](dq/monthly/2025/report_2025_03.md) |
| 2025-04 | [report_2025_04.md](dq/monthly/2025/report_2025_04.md) |
| 2025-05 | [report_2025_05.md](dq/monthly/2025/report_2025_05.md) |
| 2025-06 | [report_2025_06.md](dq/monthly/2025/report_2025_06.md) |
| 2025-07 | [report_2025_07.md](dq/monthly/2025/report_2025_07.md) |
| 2025-08 | [report_2025_08.md](dq/monthly/2025/report_2025_08.md) |
| 2025-09 | [report_2025_09.md](dq/monthly/2025/report_2025_09.md) |
| 2025-10 | [report_2025_10.md](dq/monthly/2025/report_2025_10.md) |
| 2025-11 | [report_2025_11.md](dq/monthly/2025/report_2025_11.md) |
| 2025-12 | [report_2025_12.md](dq/monthly/2025/report_2025_12.md) |

</details>

<details><summary>2024</summary>

| Month | DQ Report |
|-------|-----------|
| 2024-01 | [report_2024_01.md](dq/monthly/2024/report_2024_01.md) |
| 2024-02 | [report_2024_02.md](dq/monthly/2024/report_2024_02.md) |
| 2024-03 | [report_2024_03.md](dq/monthly/2024/report_2024_03.md) |
| 2024-04 | [report_2024_04.md](dq/monthly/2024/report_2024_04.md) |
| 2024-05 | [report_2024_05.md](dq/monthly/2024/report_2024_05.md) |
| 2024-06 | [report_2024_06.md](dq/monthly/2024/report_2024_06.md) |
| 2024-07 | [report_2024_07.md](dq/monthly/2024/report_2024_07.md) |
| 2024-08 | [report_2024_08.md](dq/monthly/2024/report_2024_08.md) |
| 2024-09 | [report_2024_09.md](dq/monthly/2024/report_2024_09.md) |
| 2024-10 | [report_2024_10.md](dq/monthly/2024/report_2024_10.md) |
| 2024-11 | [report_2024_11.md](dq/monthly/2024/report_2024_11.md) |
| 2024-12 | [report_2024_12.md](dq/monthly/2024/report_2024_12.md) |

</details>

<details><summary>2023</summary>

| Month | DQ Report |
|-------|-----------|
| 2023-01 | [report_2023_01.md](dq/monthly/2023/report_2023_01.md) |
| 2023-02 | [report_2023_02.md](dq/monthly/2023/report_2023_02.md) |
| 2023-03 | [report_2023_03.md](dq/monthly/2023/report_2023_03.md) |
| 2023-04 | [report_2023_04.md](dq/monthly/2023/report_2023_04.md) |
| 2023-05 | [report_2023_05.md](dq/monthly/2023/report_2023_05.md) |
| 2023-06 | [report_2023_06.md](dq/monthly/2023/report_2023_06.md) |
| 2023-07 | [report_2023_07.md](dq/monthly/2023/report_2023_07.md) |
| 2023-08 | [report_2023_08.md](dq/monthly/2023/report_2023_08.md) |
| 2023-09 | [report_2023_09.md](dq/monthly/2023/report_2023_09.md) |
| 2023-10 | [report_2023_10.md](dq/monthly/2023/report_2023_10.md) |
| 2023-11 | [report_2023_11.md](dq/monthly/2023/report_2023_11.md) |
| 2023-12 | [report_2023_12.md](dq/monthly/2023/report_2023_12.md) |

</details>

<details><summary>2022</summary>

| Month | DQ Report |
|-------|-----------|
| 2022-01 | [report_2022_01.md](dq/monthly/2022/report_2022_01.md) |
| 2022-02 | [report_2022_02.md](dq/monthly/2022/report_2022_02.md) |
| 2022-03 | [report_2022_03.md](dq/monthly/2022/report_2022_03.md) |
| 2022-04 | [report_2022_04.md](dq/monthly/2022/report_2022_04.md) |
| 2022-05 | [report_2022_05.md](dq/monthly/2022/report_2022_05.md) |
| 2022-06 | [report_2022_06.md](dq/monthly/2022/report_2022_06.md) |
| 2022-07 | [report_2022_07.md](dq/monthly/2022/report_2022_07.md) |
| 2022-08 | [report_2022_08.md](dq/monthly/2022/report_2022_08.md) |
| 2022-09 | [report_2022_09.md](dq/monthly/2022/report_2022_09.md) |
| 2022-10 | [report_2022_10.md](dq/monthly/2022/report_2022_10.md) |
| 2022-11 | [report_2022_11.md](dq/monthly/2022/report_2022_11.md) |
| 2022-12 | [report_2022_12.md](dq/monthly/2022/report_2022_12.md) |

</details>

<details><summary>2021</summary>

| Month | DQ Report |
|-------|-----------|
| 2021-01 | [report_2021_01.md](dq/monthly/2021/report_2021_01.md) |
| 2021-02 | [report_2021_02.md](dq/monthly/2021/report_2021_02.md) |
| 2021-03 | [report_2021_03.md](dq/monthly/2021/report_2021_03.md) |
| 2021-04 | [report_2021_04.md](dq/monthly/2021/report_2021_04.md) |
| 2021-05 | [report_2021_05.md](dq/monthly/2021/report_2021_05.md) |
| 2021-06 | [report_2021_06.md](dq/monthly/2021/report_2021_06.md) |
| 2021-07 | [report_2021_07.md](dq/monthly/2021/report_2021_07.md) |
| 2021-08 | [report_2021_08.md](dq/monthly/2021/report_2021_08.md) |
| 2021-09 | [report_2021_09.md](dq/monthly/2021/report_2021_09.md) |
| 2021-10 | [report_2021_10.md](dq/monthly/2021/report_2021_10.md) |
| 2021-11 | [report_2021_11.md](dq/monthly/2021/report_2021_11.md) |
| 2021-12 | [report_2021_12.md](dq/monthly/2021/report_2021_12.md) |

</details>

<details><summary>2020</summary>

| Month | DQ Report |
|-------|-----------|
| 2020-01 | [report_2020_01.md](dq/monthly/2020/report_2020_01.md) |
| 2020-02 | [report_2020_02.md](dq/monthly/2020/report_2020_02.md) |
| 2020-03 | [report_2020_03.md](dq/monthly/2020/report_2020_03.md) |
| 2020-04 | [report_2020_04.md](dq/monthly/2020/report_2020_04.md) |
| 2020-05 | [report_2020_05.md](dq/monthly/2020/report_2020_05.md) |
| 2020-06 | [report_2020_06.md](dq/monthly/2020/report_2020_06.md) |
| 2020-07 | [report_2020_07.md](dq/monthly/2020/report_2020_07.md) |
| 2020-08 | [report_2020_08.md](dq/monthly/2020/report_2020_08.md) |
| 2020-09 | [report_2020_09.md](dq/monthly/2020/report_2020_09.md) |
| 2020-10 | [report_2020_10.md](dq/monthly/2020/report_2020_10.md) |
| 2020-11 | [report_2020_11.md](dq/monthly/2020/report_2020_11.md) |
| 2020-12 | [report_2020_12.md](dq/monthly/2020/report_2020_12.md) |

</details>

<details><summary>2019</summary>

| Month | DQ Report |
|-------|-----------|
| 2019-01 | [report_2019_01.md](dq/monthly/2019/report_2019_01.md) |
| 2019-02 | [report_2019_02.md](dq/monthly/2019/report_2019_02.md) |
| 2019-03 | [report_2019_03.md](dq/monthly/2019/report_2019_03.md) |
| 2019-04 | [report_2019_04.md](dq/monthly/2019/report_2019_04.md) |
| 2019-05 | [report_2019_05.md](dq/monthly/2019/report_2019_05.md) |
| 2019-06 | [report_2019_06.md](dq/monthly/2019/report_2019_06.md) |
| 2019-07 | [report_2019_07.md](dq/monthly/2019/report_2019_07.md) |
| 2019-08 | [report_2019_08.md](dq/monthly/2019/report_2019_08.md) |
| 2019-09 | [report_2019_09.md](dq/monthly/2019/report_2019_09.md) |
| 2019-10 | [report_2019_10.md](dq/monthly/2019/report_2019_10.md) |
| 2019-11 | [report_2019_11.md](dq/monthly/2019/report_2019_11.md) |
| 2019-12 | [report_2019_12.md](dq/monthly/2019/report_2019_12.md) |

</details>

<details><summary>2018</summary>

| Month | DQ Report |
|-------|-----------|
| 2018-01 | [report_2018_01.md](dq/monthly/2018/report_2018_01.md) |
| 2018-02 | [report_2018_02.md](dq/monthly/2018/report_2018_02.md) |
| 2018-03 | [report_2018_03.md](dq/monthly/2018/report_2018_03.md) |
| 2018-04 | [report_2018_04.md](dq/monthly/2018/report_2018_04.md) |
| 2018-05 | [report_2018_05.md](dq/monthly/2018/report_2018_05.md) |
| 2018-06 | [report_2018_06.md](dq/monthly/2018/report_2018_06.md) |
| 2018-07 | [report_2018_07.md](dq/monthly/2018/report_2018_07.md) |
| 2018-08 | [report_2018_08.md](dq/monthly/2018/report_2018_08.md) |
| 2018-09 | [report_2018_09.md](dq/monthly/2018/report_2018_09.md) |
| 2018-10 | [report_2018_10.md](dq/monthly/2018/report_2018_10.md) |
| 2018-11 | [report_2018_11.md](dq/monthly/2018/report_2018_11.md) |
| 2018-12 | [report_2018_12.md](dq/monthly/2018/report_2018_12.md) |

</details>

<details><summary>2017</summary>

| Month | DQ Report |
|-------|-----------|
| 2017-01 | [report_2017_01.md](dq/monthly/2017/report_2017_01.md) |
| 2017-02 | [report_2017_02.md](dq/monthly/2017/report_2017_02.md) |
| 2017-03 | [report_2017_03.md](dq/monthly/2017/report_2017_03.md) |
| 2017-04 | [report_2017_04.md](dq/monthly/2017/report_2017_04.md) |
| 2017-05 | [report_2017_05.md](dq/monthly/2017/report_2017_05.md) |
| 2017-06 | [report_2017_06.md](dq/monthly/2017/report_2017_06.md) |
| 2017-07 | [report_2017_07.md](dq/monthly/2017/report_2017_07.md) |
| 2017-08 | [report_2017_08.md](dq/monthly/2017/report_2017_08.md) |
| 2017-09 | [report_2017_09.md](dq/monthly/2017/report_2017_09.md) |
| 2017-10 | [report_2017_10.md](dq/monthly/2017/report_2017_10.md) |
| 2017-11 | [report_2017_11.md](dq/monthly/2017/report_2017_11.md) |
| 2017-12 | [report_2017_12.md](dq/monthly/2017/report_2017_12.md) |

</details>

<details><summary>2016</summary>

| Month | DQ Report |
|-------|-----------|
| 2016-01 | [report_2016_01.md](dq/monthly/2016/report_2016_01.md) |
| 2016-02 | [report_2016_02.md](dq/monthly/2016/report_2016_02.md) |
| 2016-03 | [report_2016_03.md](dq/monthly/2016/report_2016_03.md) |
| 2016-04 | [report_2016_04.md](dq/monthly/2016/report_2016_04.md) |
| 2016-05 | [report_2016_05.md](dq/monthly/2016/report_2016_05.md) |
| 2016-06 | [report_2016_06.md](dq/monthly/2016/report_2016_06.md) |
| 2016-07 | [report_2016_07.md](dq/monthly/2016/report_2016_07.md) |
| 2016-08 | [report_2016_08.md](dq/monthly/2016/report_2016_08.md) |
| 2016-09 | [report_2016_09.md](dq/monthly/2016/report_2016_09.md) |
| 2016-10 | [report_2016_10.md](dq/monthly/2016/report_2016_10.md) |
| 2016-11 | [report_2016_11.md](dq/monthly/2016/report_2016_11.md) |
| 2016-12 | [report_2016_12.md](dq/monthly/2016/report_2016_12.md) |

</details>

<details><summary>2015</summary>

| Month | DQ Report |
|-------|-----------|
| 2015-01 | [report_2015_01.md](dq/monthly/2015/report_2015_01.md) |
| 2015-02 | [report_2015_02.md](dq/monthly/2015/report_2015_02.md) |
| 2015-03 | [report_2015_03.md](dq/monthly/2015/report_2015_03.md) |
| 2015-04 | [report_2015_04.md](dq/monthly/2015/report_2015_04.md) |
| 2015-05 | [report_2015_05.md](dq/monthly/2015/report_2015_05.md) |
| 2015-06 | [report_2015_06.md](dq/monthly/2015/report_2015_06.md) |
| 2015-07 | [report_2015_07.md](dq/monthly/2015/report_2015_07.md) |
| 2015-08 | [report_2015_08.md](dq/monthly/2015/report_2015_08.md) |
| 2015-09 | [report_2015_09.md](dq/monthly/2015/report_2015_09.md) |
| 2015-10 | [report_2015_10.md](dq/monthly/2015/report_2015_10.md) |
| 2015-11 | [report_2015_11.md](dq/monthly/2015/report_2015_11.md) |
| 2015-12 | [report_2015_12.md](dq/monthly/2015/report_2015_12.md) |

</details>

<details><summary>2014</summary>

| Month | DQ Report |
|-------|-----------|
| 2014-01 | [report_2014_01.md](dq/monthly/2014/report_2014_01.md) |
| 2014-02 | [report_2014_02.md](dq/monthly/2014/report_2014_02.md) |
| 2014-03 | [report_2014_03.md](dq/monthly/2014/report_2014_03.md) |
| 2014-04 | [report_2014_04.md](dq/monthly/2014/report_2014_04.md) |
| 2014-05 | [report_2014_05.md](dq/monthly/2014/report_2014_05.md) |
| 2014-06 | [report_2014_06.md](dq/monthly/2014/report_2014_06.md) |
| 2014-07 | [report_2014_07.md](dq/monthly/2014/report_2014_07.md) |
| 2014-08 | [report_2014_08.md](dq/monthly/2014/report_2014_08.md) |
| 2014-09 | [report_2014_09.md](dq/monthly/2014/report_2014_09.md) |
| 2014-10 | [report_2014_10.md](dq/monthly/2014/report_2014_10.md) |
| 2014-11 | [report_2014_11.md](dq/monthly/2014/report_2014_11.md) |
| 2014-12 | [report_2014_12.md](dq/monthly/2014/report_2014_12.md) |

</details>

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

*Last updated: 2026-05-01 21:52 UTC*
<!-- DQ_REPORTS:END -->

## Структура репозиторію

```
/docs
    METHODOLOGY.md              # Методологія обробки даних (EN)
    METHODOLOGY.uk.md           # Методологія обробки даних (UK)
    DATA_QUALITY_REPORT.md      # Метрики якості та висновки (EN)
    DATA_QUALITY_REPORT.uk.md   # Метрики якості та висновки (UK)
    PUBLISHING.md               # Стратегія публікації та розповсюдження
/dq
    /monthly                    # Місячні DQ-звіти (комітяться автоматично при релізі)
        report_2013_01.md
        report_2013_02.md
        ...
    /yearly                     # Річні DQ-звіти (комітяться автоматично при релізі)
        report_2013.md
        report_2014.md
        ...
/schema
    schema.md                   # Документація схеми (EN)
    schema.uk.md                # Документація схеми (UK)
    schema.json                 # Визначення схеми (машинозчитуване)
CITATION.cff                    # Метадані цитування (кнопка Cite в GitHub)
LICENSE                         # Текст ліцензії CC BY 4.0
README.md                       # Англійська версія
README.uk.md                    # Цей файл (українська)
datapackage.json                # Дескриптор Frictionless Data
codemeta.json                   # Метадані CodeMeta
.zenodo.json                    # Метадані депозиту Zenodo
```

## Атрибуція

Цей набір даних є похідним від відкритих даних, опублікованих **Міністерством внутрішніх справ України** на [data.gov.ua](https://data.gov.ua) за ліцензією [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

**Обов'язкова атрибуція (вимога ліцензії):**

> Джерело: «Відомості про транспортні засоби та їх власників» — Міністерство внутрішніх справ України, опубліковано на data.gov.ua.
> https://data.gov.ua/dataset/06779371-308f-42d7-895e-5a39833375f0

### Цитування цього похідного набору даних

Якщо ви використовуєте цю версію у дослідженнях або аналітиці, будь ласка, цитуйте:

> Klimnyk. (2026). UA Vehicle Registry — Data Quality Edition [Dataset]. GitHub. https://github.com/mykhailoklimnyk/ua-vehicle-registry-data

Машинозчитуване цитування доступне через файл `CITATION.cff` (активує кнопку «Cite this repository» на GitHub).

## Ліцензія

Цей похідний набір даних розповсюджується за ліцензією [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

Ви можете вільно поширювати та адаптувати матеріал для будь-яких цілей, включно з комерційними, за умови належної атрибуції.

## Як долучитися

Цей проєкт відкритий для внесків. Якщо ви знайшли помилку в даних, хочете покращити словники або маєте пропозиції — будь ласка, створіть **Pull Request** або відкрийте **Issue**.

Особливо цінні внески:
- Виправлення помилок у назвах марок/моделей
- Доповнення словників (адреси ТСЦ, коди КОАТУУ)
- Повідомлення про знайдені дублікати або аномалії
- Покращення документації

## Застереження

Цей проєкт є незалежною ініціативою з покращення якості даних. Він **не пов'язаний з Міністерством внутрішніх справ України та не підтримується ним** чи будь-яким іншим державним органом. Дані надаються «як є» без будь-яких гарантій. Користувачі несуть повну відповідальність за використання даних.

## Правова основа вихідних даних

Вихідний набір даних опубліковано відповідно до:
- Закону України «Про дорожній рух»
- Постанови КМУ від 25.03.2016 № 260 «Деякі питання надання інформації про зареєстровані транспортні засоби та їх власників»
- Закону України «Про доступ до публічної інформації»
