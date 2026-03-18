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

## Звіти Data Quality

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
