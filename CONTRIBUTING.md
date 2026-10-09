
# Contributing to TimeChorus

TimeChorus (timechorus.com) is a sourced atlas of the centuries 800–200 BCE. Every person, event, idea and quotation in this dataset carries the source it was read from, at a locator where you can check it. That rule is the whole method, and contributions live or die by it.

## Start with an issue, not a pull request

Comment on an open issue, or open a new one. Before anything changes in the data, the team re-reads every cited passage at its locator, and every change passes the dataset's validation gate — so a pull request cannot be merged as sent, but a well-sourced comment becomes a change quickly. The open issues marked `good-first-issue` are lookups in a named book, scan or gazetteer: real contributions that need patience rather than a specialty.

## What a contribution must carry

- **The source**, named by edition (title, editor or translator, year, and the URL or scan you read). No source, no change.
- **The locator**: chapter, section, line, page or atlas sheet, where any reader can find it, and the words that settle the point, quoted in the original script where the edition's licence allows.
- **For a date:** a range (a single year is a range whose ends coincide) and the reason for it.
- **For a coordinate:** the basis and its tier (Pleiades, then CHGIS or Tan Qixiang's atlas, then Schwartzberg for South Asia, then a Wikidata modern site). No basis, no coordinate.
- **For a quotation:** the edition's licence. Public-domain and openly licensed texts can be quoted. Copyrighted translations can be cited and paraphrased, but not quoted.

## The rules we hold ourselves to

You will see these in the data, and contributions are weighed against them:

- **No source, no row.** Nothing enters on memory or convention.
- **Honest blanks beat guesses.** An unlocated place, a blank capital, a missing year range — each states why it is blank. A well-cited "this cannot be fixed" is a result we record.
- **Disputes are recorded, not adjudicated.** Where scholars disagree, the dataset states the positions and its choice, with sources. A contribution that re-argues a recorded choice needs new evidence, not preference.
- **Every range carries a basis; unexplained precision widens.**
- **The licence clock:** a quotation's licence is the most restrictive established reading of its edition's grant.

The `basis` and `note` columns cite the project's internal editorial record (round CHANGES archives, ruling ids such as L-nn, reviewer names). Those citations are the editors' working register, published as written; they are not resolvable in this repository.

## What happens to your contribution

The team re-reads your cited passage at its locator, runs the gate, and merges the change with your issue linked in the row's provenance. Corrections are credited in the commit and the issue thread. If your evidence changes a reading, the old reading stays visible in the row's history — this dataset corrects itself in public.

## Licences

The dataset is CC BY 4.0 (see DATA_LICENSE). Quoted original texts carry their own editions' licences, stated per source. Please extend others' work the same courtesy you find here.

## Conduct

Argue about sources, not people. The maintainers close what doesn't.
