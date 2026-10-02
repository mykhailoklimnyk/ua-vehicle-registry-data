# Source Files

Every published snapshot is built from exactly one revision of the MIA (МВС) open
registry per period. The table lists those revisions. Anyone can download the same
files from data.gov.ua, check the SHA-256 and reproduce the row counts.

Dataset: [Відомості про транспортні засоби та їх власників](https://data.gov.ua/dataset/06779371-308f-42d7-895e-5a39833375f0)
(`06779371-308f-42d7-895e-5a39833375f0`). Files were retrieved on 2026-10-02.

| Period | data.gov.ua resource | File in ZIP | ZIP SHA-256 | ZIP bytes | Rows in file |
|---|---|---|---|---:|---:|
| 2013 | `86a9548b-8323-4fa2-972e-0692edf6959f` | `tz_opendata_z01012013_po31122013.csv` | `7dba62baa278adbdbe91071271423b3f3f9ef8d98c6c5eb34857f2d1f528c819` | 97 367 430 | 1 935 496 |
| 2014 | `80a115ae-61df-4a13-8771-36c2826268df` | `tz_opendata_z01012014_po31122014.csv` | `0bd4221360e3684e49ce4b8e09b6c654ba9f9c1724fd0c17eeaf72db30ce2a31` | 72 196 197 | 1 439 551 |
| 2015 | `09c606dc-d740-40db-96f0-e679eeca6ace` | `tz_opendata_z01012015_po31122015.csv` | `e8b31a94bcdb7d1fbc33319b14f28d1abd7f9a85364e86fec469398f48f5343b` | 62 248 522 | 1 296 256 |
| 2016 | `7bdc2a1b-5399-4ab0-97e0-633e68837b04` | `tz_opendata_z01012016_po31122016.csv` | `3fd32e77b1ebfc5a954ddd04e5a93ea27b043769d28d8e3d33155d699b8c5f9e` | 59 325 373 | 1 432 560 |
| 2017 | `9ce32352-bd11-4324-a2b4-5addbd228b1b` | `tz_opendata_z01012017_po31122017.csv` | `dbd62b4ceb84a3bed685e6203e2c7002818aad5bf2c3c73058bd27287a2868fe` | 58 960 358 | 1 417 655 |
| 2018 | `01323740-88df-46c2-b06e-fbb58c89fe17` | `tz_opendata_z01012018_po01012019.csv` | `d97fc190076ef15dc82dddc7ff2500cfa6dccb4f2162df7abd9383fdc6cb0a6f` | 65 881 530 | 1 547 418 |
| 2019 | `7a58e8f7-9323-47d4-a21d-19486e014eb4` | `tz_opendata_z01012019_po01012020.ßsv` ¹ | `da7c0bcce7903364ef276d5f67aa07ea8c50fe9acadcbaed104b820a565c1b67` | 86 785 142 | 2 079 481 |
| 2020 | `ebeb92fe-424c-41d1-aacf-288e91049dc9` | `tz_opendata_z01012020_po01012021.csv` | `fcd7c0e108e2cdbc2ff5a193f9e7ef948a2d03fbda9b8ae2edcf8ed7461e2749` | 70 487 994 | 1 771 329 |
| 2021 | `c5cb530d-0533-40be-b9ad-f03e06c94b10` | `tz_opendata_z01012021_po01012022.csv` | `ec862840d31581d29b7c811eeb14fbf33b0859b0939ad23f396c10ec6a09a609` | 104 318 094 | 2 201 307 |
| 2022 | `b1bcb4a9-8e60-4a1c-91c0-00faae008816` ² | `tz_opendata_z01012022_po01012023.csv` | `0ffb0276f33fa9981059a9c2e4e49b380e7c2f3ee88923a88661cd0272ff1df0` | 90 364 240 | 1 745 908 |
| 2023 | `c3a12388-55c2-4546-8b71-b4b7ff0d8b16` | `tz_opendata_z01012023_po01012024.csv` | `2e13cd2c0a3b42288a7735d32e464c3a13ac7140f144df81882e05d38ed4a0ce` | 110 950 138 | 2 124 732 |
| 2024 | `c3ffecc4-bb5c-4102-b761-6dcfeb60b4fe` | `tz_opendata_z01012024_po01012025.csv` | `2caffbd9c8f383a258f76902551261bac34114ddc2172c521bfc5611f8e82cc5` | 112 148 826 | 2 344 544 |
| 2025 | `b7e72d22-55f5-4545-87dc-94e6c8ee03ef` | `reestrtz31.12.2025.csv` | `8f45007b47440d614f90bd1a117bd24263e0ae4307ed0a5de0c26dee1ef2bd0f` | 113 782 855 | 2 229 904 |
| 2026-01-01 … 2026-04-29 | `3f13166f-090b-499e-8e23-e9851c5a5f67`, revision 508698 ³ | `reestrtz01.05.2026.csv` | — | — | 693 929 |
| 2026-04-30 … | `3f13166f-090b-499e-8e23-e9851c5a5f67` (current) | `reestrtz31.08.2026.csv` | `00c9a73e37f04366998a68b86b109a4e9f1fedafe06f133cbc52374fc50e9459` | 49 217 894 | 1 221 092 |

¹ The extension is spelled `.ßsv` inside the MIA archive; the file is a regular `;`-separated CSV.

² data.gov.ua carries three 2022 resources. The other two (`bef7b47b…`, `fb6d9eb4…`) are partial
uploads from September 2022 and are superseded by the full-year file above; they are not used.

³ For January–April 2026 the snapshots keep the revision published on 2026-05-01, which still
contains registration plates and owner KOATUU codes. The current 2026 file (schema v2) no longer
carries those fields for any month, so it is used only from 2026-04-30, the first day that
revision 508698 does not cover. The 2026 resource is overwritten in place by the publisher, so
revision 508698 is not downloadable as a separate file; the snapshot files themselves are the record.

## How rows map to the source

* One row in a snapshot is one distinct source row. Rows that are identical in every source
  column are published once. `record_id` is the MD5 of the source fields (operation name excluded,
  see the schema) and is unique within the dataset.
* Each period comes from exactly one revision, so the same registration event cannot appear twice
  with different spellings from different revisions.
* Registration plates are not published for 2013–2020. From 2026-05 the source no longer
  contains them.
