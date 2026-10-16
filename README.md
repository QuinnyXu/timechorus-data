# TimeChorus data

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23354079.svg)](https://doi.org/10.5281/zenodo.23354079)

The dataset behind **TimeChorus** (https://timechorus.com): an atlas of the centuries (c. 800-200 BCE) in which
the major philosophical, religious and political systems of Eurasia were first written down.
Every record is a range-dated, sourced claim; every quotation names the edition it was read
in and the licence that lets it be quoted.

This repository is exported by a build step from the project's working repository; it is
never edited by hand. Source commit: `c692f50daf8fda4eadafcc3c5ed749537ec3675e`.

## Contents

| path | what |
|---|---|
| `data/csv/` | the 15 tables as CSV (UTF-8, CRLF, byte-for-byte what the quality gate passed) |
| `data/json/` | the same tables as typed JSON, generated from the CSV |
| `schema/` | JSON Schema for every table |
| `SCHEMA.md` | the columns, vocabularies and foreign keys in prose |
| `DATA_LICENSE` | the licence of the compilation and the per-source licences and credit clauses |
| `CONTRIBUTING.md` | how to correct or add a record |
| `SOURCE_COMMIT`, `MANIFEST.sha256` | which commit this tree is, and a checksum of every file |

## Size at this commit

| table | rows |
|---|---|
| `regions` | 7 |
| `polities` | 21 |
| `places` | 70 |
| `people` | 115 |
| `works` | 63 |
| `sources` | 95 |
| `events` | 220 |
| `event_people` | 326 |
| `relations` | 39 |
| `lineages` | 22 |
| `ideas` | 49 |
| `idea_links` | 233 |
| `passages` | 164 |
| `calendars` | 10 |
| `sync_slices` | 55 |

## Rules the data keeps

No source, no row. No text, no idea. No basis, no coordinate. No licence, no verbatim quote.
Dates are ranges with a stated basis. Rows marked `draft` have not been read at their
locator by the recording session; `reviewed` rows have, with the evidence on the row.

## Editorial references

**Editorial references.** Basis and note columns cite the project's internal editorial record — round CHANGES archives, delivery notes, ruling ids (L-nn, D-nn, r-session ids) and reviewer names; these citations are the editors' working register, published as written, and are not resolvable in this repository.

## Licence

Data: CC BY 4.0 for the compilation and the project's own text; third-party quotations keep
their sources' licences. See `DATA_LICENSE`.

## How to cite

Quinny Xu. *TimeChorus data* (axial corpus), source commit `c692f50daf8f`. https://timechorus.com. Concept DOI (all versions, always
resolving to the latest release): [10.5281/zenodo.23354079](https://doi.org/10.5281/zenodo.23354079).

## Contributing

See `CONTRIBUTING.md`. GitHub issues are the preferred route for
corrections and questions.

Contact: xkxuqian@outlook.com
