# Schema

Generated from `schema/v2/` (JSON Schema, draft 2020-12). Every value in the CSV files
is text; `data/json/` carries the typed form (integers for years, floats for coordinates,
lists for `;`-separated id lists, booleans for `is_protagonist`).

Conventions every table shares:

- Dates are **ranges**, never single guesses: `*_min` / `*_max` (a single year is a range
  whose ends coincide). BCE years are negative integers. Each range has a `dating_basis`.
- `review_status` is `draft` or `reviewed`. `reviewed` means the cited passage was read at
  its locator, in the session that recorded the row, with the evidence on the row; anything
  less stays `draft`.
- Foreign keys are listed per table below; the quality gate checks every one.
- **Editorial references.** Basis and note columns cite the project's internal editorial record — round CHANGES archives, delivery notes, ruling ids (L-nn, D-nn, r-session ids) and reviewer names; these citations are the editors' working register, published as written, and are not resolvable in this repository.

Tables (in dependency order): `regions`, `polities`, `places`, `people`, `works`, `sources`, `events`, `event_people`, `relations`, `lineages`, `ideas`, `idea_links`, `passages`, `calendars`, `sync_slices`.

## regions

Regions of the axial corpus; the top-level geographic and cultural division.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `name_en` | string | required column |  |
| `pillar` | string | required column |  |
| `colour` | string | required column |  |
| `description` | string | required column |  |

## polities

Polities (states, kingdoms, leagues) within a region.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `name_en` | string | required column |  |
| `region_id` | string | required column; FK -> `regions` (never blank) |  |
| `polity_type` | string | required column |  |
| `year_min` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `year_max` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `capital_place_id` | string | required column; FK -> `places` |  |
| `wikidata_qid` | string | required column |  |
| `note` | string | required column |  |

## places

Located places, with coordinates and the basis for them.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `name_en` | string | required column |  |
| `name_original` | string | required column |  |
| `region_id` | string | required column; FK -> `regions` (never blank) |  |
| `polity_id` | string | required column; FK -> `polities` |  |
| `modern_location` | string | required column |  |
| `place_type` | string | required column |  |
| `lat` | string | required column | Decimal degrees as written in the CSV; range-checked procedurally. |
| `lng` | string | required column | Decimal degrees as written in the CSV; range-checked procedurally. |
| `coord_certainty` | string | required column; one of: `high`, `medium`, `low`, `` |  |
| `coord_basis` | string | required column |  |
| `pleiades_id` | string | required column |  |
| `wikidata_qid` | string | required column |  |
| `source_ids` | string | required column; list of FK -> `sources` | Semicolon-separated FK list to sources: the citation(s) behind this row's coordinates, companion to coord_basis's prose (schema v2.1, r6-0, L-86(1)). Blank is a validate.py warning (backlog), not an error, until the row is migrated. |
| `description` | string | required column |  |

Gate rules beyond the column constraints (implemented in `tools/validate.py` of the
source repository, not shipped here): `places_pleiades_l8`, `places_coords`, `places_source_ids_backlog_warn`.

## people

Persons, with range-dated birth/death and a historicity grade.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `name_en` | string | required column |  |
| `name_original` | string | required column |  |
| `name_alt` | string | required column |  |
| `region_id` | string | required column; FK -> `regions` (never blank) |  |
| `polity_id` | string | required column; FK -> `polities` |  |
| `role_tag_en` | string | required column |  |
| `historicity` | string | required column; one of: `attested`, `probable`, `traditional`, `legendary`, `anonymous` |  |
| `birth_year_min` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `birth_year_max` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `death_year_min` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `death_year_max` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `dating_basis` | string | required column |  |
| `wikidata_qid` | string | required column |  |
| `is_protagonist` | string | required column |  |
| `short_bio_en` | string | required column |  |
| `map_place_id` | string | required column; FK -> `places` | Nullable FK to places: an explicit display anchor overriding anchorOf()'s computed choice. Requires map_place_basis (schema v2.1, r6-0). |
| `map_place_basis` | string | required column | Why the map_place_id override was made. No reason, no override (conventions.md §1): a non-blank map_place_id with a blank map_place_basis is a validate.py error. |
| `map_place_reason` | string | required column | Reader-register reason for the map_place_id override, shown to readers on the person record line (schema v2.2, r9-2, L-104(2)). Required non-blank wherever map_place_id is set once the column is in use; 60-450 characters, no ruling ids, no file paths. map_place_basis stays the reviewers' working log in the CSV. |
| `review_status` | string | required column; one of: `draft`, `reviewed` |  |

Gate rules beyond the column constraints (implemented in `tools/validate.py` of the
source repository, not shipped here): `people_dating_basis`, `people_role_tag_en`, `people_traditional_birth_range`, `people_map_place_override`, `people_map_place_reason`, `people_is_protagonist_enum`.

## works

Works (texts) with a range-dated composition window.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `title_en` | string | required column |  |
| `title_original` | string | required column |  |
| `language` | string | required column |  |
| `region_id` | string | required column; FK -> `regions` (never blank) |  |
| `genre` | string | required column |  |
| `author_person_id` | string | required column; FK -> `people` |  |
| `composition_year_min` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `composition_year_max` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `composition_basis` | string | required column |  |
| `open_translation_source_id` | string | required column; FK -> `sources` |  |
| `wikidata_qid` | string | required column |  |
| `note` | string | required column |  |
| `review_status` | string | required column; one of: `draft`, `reviewed` |  |

Gate rules beyond the column constraints (implemented in `tools/validate.py` of the
source repository, not shipped here): `works_composition_basis`.

## sources

Bibliographic sources, with layer and licence.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `title` | string | required column |  |
| `work_id` | string | required column; FK -> `works` |  |
| `section` | string | required column |  |
| `source_layer` | string | required column; one of: `inscription`, `contemporary_text`, `near_contemporary`, `later_compilation`, `hagiography`, `scripture_final_form`, `modern_scholarship`, `gazetteer` |  |
| `source_type` | string | required column |  |
| `language` | string | required column |  |
| `translator` | string | required column |  |
| `translation_year` | string | required column |  |
| `licence` | string | required column; one of: `public domain`, `CC0`, `CC BY 4.0`, `CC BY 3.0`, `CC BY-SA 4.0`, `CC BY-SA 3.0`, `copyright`, `unconfirmed` |  |
| `licence_note` | string | required column |  |
| `credit_clause` | string | required column | The labelled-segment convention (§6) as a column: the attribution the /about credits generator shows verbatim for a CC BY* source. The generator reads this column first, the labelled ATTRIBUTION CLAUSE segment in licence_note as fallback (schema v2.1, r6-0, L-82/L-88). Blank on a CC BY* row is a validate.py warning (backlog), not an error, until migrated. |
| `url` | string | required column |  |
| `note` | string | required column |  |

Gate rules beyond the column constraints (implemented in `tools/validate.py` of the
source repository, not shipped here): `sources_licence_vocabulary`, `sources_credit_clause_backlog_warn`.

## events

Range-dated events; the spine of the timeline and chronicle.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `year_min` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `year_max` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `year_display` | string | required column |  |
| `dating_basis` | string | required column |  |
| `region_id` | string | required column; FK -> `regions` (never blank) |  |
| `polity_id` | string | required column; FK -> `polities` |  |
| `place_id` | string | required column; FK -> `places` |  |
| `title_en` | string | required column |  |
| `summary_en` | string | required column |  |
| `category` | string | required column; one of: `activity`, `battle`, `birth`, `conquest`, `coup`, `death`, `decree`, `disaster`, `encounter`, `journey`, `meeting`, `monument`, `omen`, `ordination`, `performance`, `reform`, `speech`, `transaction`, `treaty`, `trial`, `vision` |  |
| `source_ids` | string | required column; list of FK -> `sources` (never blank) | Semicolon-separated id list. |
| `reliability` | string | required column; one of: `high`, `medium`, `low` |  |
| `importance` | string | required column; one of: `1`, `2`, `3`, `4`, `5` |  |
| `note` | string | required column | Free-text caveat home (ruling L-28, schema v2.1 / r6-0): a caveat about a row's category, an agentless actor, a locator apparatus note, never reader-facing prose (that is summary_en). |
| `review_status` | string | required column; one of: `draft`, `reviewed` |  |

Gate rules beyond the column constraints (implemented in `tools/validate.py` of the
source repository, not shipped here): `events_required_prose`, `events_reliability_vs_range`, `events_place_warn`, `events_people_warn`.

## event_people

Join table: which people took part in which event.

| column | type | constraints | notes |
|---|---|---|---|
| `event_id` | string | required column; FK -> `events` (never blank) |  |
| `person_id` | string | required column; FK -> `people` (never blank) |  |
| `role_in_event` | string | required column |  |
| `presence` | string | required column; one of: `present`, `related` |  |
| `note` | string | required column |  |

## relations

Person-to-person relations.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `person_a` | string | required column; FK -> `people` (never blank) |  |
| `person_b` | string | required column; FK -> `people` (never blank) |  |
| `rel_type` | string | required column; one of: `kin`, `rival`, `enemy`, `teacher`, `service`, `patron`, `associate` |  |
| `rel_label_en` | string | required column |  |
| `reliability` | string | required column; one of: `high`, `medium`, `low` |  |
| `source_ids` | string | required column; list of FK -> `sources` (never blank) | Semicolon-separated id list. |
| `note` | string | required column |  |

Gate rules beyond the column constraints (implemented in `tools/validate.py` of the
source repository, not shipped here): `relations_self`.

## lineages

Teacher-to-student transmission links.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `teacher_id` | string | required column; FK -> `people` (never blank) |  |
| `student_id` | string | required column; FK -> `people` (never blank) |  |
| `lineage_type` | string | required column; one of: `direct`, `claimed`, `school` |  |
| `reliability` | string | required column; one of: `high`, `medium`, `low` |  |
| `source_ids` | string | required column; list of FK -> `sources` (never blank) | Semicolon-separated id list. |
| `note` | string | required column |  |

## ideas

Ideas, each anchored to at least one passage.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `name_en` | string | required column |  |
| `name_original` | string | required column |  |
| `region_of_origin` | string | required column; FK -> `regions` (never blank) |  |
| `category` | string | required column |  |
| `one_line_en` | string | required column |  |
| `first_attested_year_min` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `first_attested_year_max` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `first_attestation_basis` | string |  | Why the first-attested range is what it is (conventions §2's dating_basis, for ideas). Blank on a row is a warning, not an error, added by ruling L-51(5): red line 5 could not be satisfied for this table until the column existed; the pre-existing rows earn their warning as the backlog marker for the Round-3 backfill. |
| `review_status` | string | required column; one of: `draft`, `reviewed` |  |

Gate rules beyond the column constraints (implemented in `tools/validate.py` of the
source repository, not shipped here): `ideas_one_line_en`, `ideas_need_passage_link`, `ideas_first_attestation_basis`.

## idea_links

Polymorphic links from an idea to a person/passage/event/idea/work.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `idea_id` | string | required column; FK -> `ideas` (never blank) |  |
| `target_type` | string | required column; one of: `person`, `passage`, `event`, `idea`, `work` |  |
| `target_id` | string | required column |  |
| `link_type` | string | required column; one of: `originates`, `develops`, `refutes`, `transmits`, `parallels`, `enacts` |  |
| `reliability` | string | required column; one of: `high`, `medium`, `low` |  |
| `note` | string | required column |  |

Gate rules beyond the column constraints (implemented in `tools/validate.py` of the
source repository, not shipped here): `idea_links_polymorphic_target`.

## passages

Quoted passages, original and translation, with licence provenance.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `work_id` | string | required column; FK -> `works` (never blank) |  |
| `source_id` | string | required column; FK -> `sources` (never blank) |  |
| `original_source_id` | string | required column; FK -> `sources` | Nullable FK to sources: the licensed original's edition, distinct from source_id (the row's reading edition) and translation_source_id (the quoted translation's licence source). Schema v2.1, r6-0 (L-82/L-84(1)/L-86(1)/r5-4 Part 1 §9(4)). |
| `locator` | string | required column |  |
| `quote_original` | string | required column |  |
| `quote_translation` | string | required column |  |
| `translation_source_id` | string | required column; FK -> `sources` |  |
| `quote_type` | string | required column; one of: `annal`, `chronicle`, `fragment`, `hymn`, `inscription`, `oracle`, `paraphrase`, `saying`, `scripture`, `speech`, `verse` |  |
| `note` | string | required column |  |
| `review_status` | string | required column; one of: `draft`, `reviewed` |  |

Gate rules beyond the column constraints (implemented in `tools/validate.py` of the
source repository, not shipped here): `passages_locator`, `passages_not_empty`, `passages_placeholder_translation`, `passages_licence_join`, `passages_original_licence_join`, `passages_original_source_distinct`.

## calendars

Calendar systems and conversion notes.

| column | type | constraints | notes |
|---|---|---|---|
| `id` | string | required column; non-empty |  |
| `system` | string | required column |  |
| `region_id` | string | required column |  |
| `epoch_year_bce` | string | required column |  |
| `year_start` | string | required column |  |
| `conversion_rule` | string | required column |  |
| `note` | string | required column |  |

## sync_slices

One-line 'what was happening here that year' rows, per (year, region).

| column | type | constraints | notes |
|---|---|---|---|
| `year` | string | required column; pattern `^-?\d{1,4}$` | Integer year, BCE negative; blank permitted where the range is optional. |
| `region_id` | string | required column; FK -> `regions` (never blank) |  |
| `one_line_en` | string | required column |  |
| `source_ids` | string | required column; list of FK -> `sources` (never blank) | Semicolon-separated id list. |
| `reliability` | string | required column; one of: `high`, `medium`, `low` |  |
