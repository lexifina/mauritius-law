# How this repository was made (Mauritius)

This file is written by the build on 2026-10-28, the day the source data was retrieved.

## Sources

| Source | Address | Retrieved | Licence |
| --- | --- | --- | --- |
| Attorney General's Office, Laws of Mauritius | <https://attorneygeneral.govmu.org/> | 2026-10-28 | Copyright Government of Mauritius; non-commercial copying allowed; commercial use needs Solicitor General approval |
| National Assembly of Mauritius, Acts as passed and gazetted | <https://mauritiusassembly.govmu.org/mauritiusassembly/index.php/acts-2/> | 2026-10-28 | Official texts |
| Internet Archive (Wayback) captures of attorneygeneral.govmu.org | <https://web.archive.org/> | 2026-10-28 | Captures of the AG site; same terms |

Every word of law in this repository comes from these sources. Lexifina reads them and records each act's dated versions, and the laws that made each change, in one corpus file. This repository is built from that file.

## What is in it

| Act | Title | Versions | Span |
| --- | --- | ---: | --- |
| `constitution` | The Constitution of Mauritius | 4 | 2011-12-12 to 2016-12-17 |

The build made 5 commits:

| Commits | Kind |
| ---: | --- |
| 1 | first version of an act |
| 4 | one law on its in-force day |
| 0 | changes no amendment note explains, credited to the publisher |

Other counts:
- 0 commits on the branch `future`: laws not yet in force on 2026-10-28.
- 0 commits dated 1 January 1970: in force before 1970, with the real day in their `In-force` trailer.
- 32 laws in the record.
- 8 tags.

## Dates

Each commit is dated at noon (Indian/Mauritius) on the day the change took effect. The other dates, enacted and published, are trailers.

When a law gives no date of its own, the default rule applies: Constitution s. 46(4): no law comes into operation until published in the Gazette; Parliament may postpone or make it retrospective. Dates used are the National Assembly's 'Date in force' column, or the Act's own commencement section. https://attorneygeneral.govmu.org/Documents/Laws%20of%20Mauritius/Constitutions/THE%20CONSTITUTION,%20GN%2054%20of%201968.pdf

## How the history is built

`tools/build.py` turns the corpus into this history in one pass: one commit per law, on the day it took effect. The same corpus always gives the same commit hashes. `tools/check_repo.py` reads every version back from git and compares it with the corpus. `tools/on.py` shows the law as it stood on any date. The corpus format is described in `tools/SCHEMA.md`.

- Corpus: sha256 `f6ce417d2d92815e4d76d3c9afea29d0992c193b47e96ac7b159d05fb33c405e` (714380 bytes).
- Builder blob hashes (`git hash-object`): `build.py` `a4faa25af85facb64b956b372f6b528aafdd7ec1`, `check.py` `3d00541d258745461ab52b1b0cb4e1716a27a95d`, `on.py` `644a6c0857fbcc72562032b58d57aa113fee0641`.
- Python 3.12.10; git version 2.46.0.windows.1.

## Checks

4 versions of 1 acts read back from git on the day each took effect and on the day before: 0 mismatches. Trailers: 0 commits whose trailers git cannot parse. Health: 0 merge commits on main, 0 commit dates that go backwards, 0 commits dated after the build.

A second build from the same corpus into an empty folder gave identical hashes for all 5 history commits and 8 tags (history tip `eae40c76cb7ecafce5fe0565ff5528ca6e5ab046`).

## Decisions made by hand

- Mauritius has no Peachjam LII; sources are the Attorney General's Office consolidations and the National Assembly's Acts as passed.
- The AG Constitution text (unchanged since the 2015-12-21 Wayback capture apart from the electoral boundaries GN, now a separate file) is dated 2011-12-12, the commencement of the latest change it records (Act 35 of 2011); it is credited to the publisher as the oldest version available.
- Only sections 1-122 and the First to Fourth Schedules are the Constitution; the subsidiary regulations and boundary notices in the same PDF are left out.
- The publisher's bracketed amendment notes and the Khoyratty editorial note on s. 5(3A) are kept out of the text.
- The AG PDF's footnotes (superscript markers 1-4 after the British Nationality Acts in ss. 20, 25 and 27, and the footnote lines citing UK chapter numbers in ss. 21, 27 and 112), its running head 'THIRD SCHEDULE—continued', and the heading 'PART II' that followed the repealed Fourth Schedule are left out; the Fourth Schedule (repealed by Act 31 of 2000) has no file.
- Acts 30 of 2015, 28 and 30 of 2016 are replayed from the gazetted Act PDFs; their in-force dates are the National Assembly table's 'Date in force' (10 Dec 2015; 17 Dec 2016).
- Act 3 of 2014 changes no words; it is recorded as a note on the First Schedule.
- Act 10 of 2025 (repeals s. 7(2)) is NOT applied: the Act has no commencement section but the National Assembly table marks it '*' (by Proclamation), and no Proclamation was found. Today's s. 7 may therefore be out of date.

## What AI did and did not do

The scripts that read the sources and build this repository were written by Claude Code running Claude Opus 5.5 (`claude-opus-5-5`) under Lexifina's direction.

No model wrote, translated or corrected legal text. No model runs at build time. Every word of law in this repository comes from the official sources above.

The commit messages are written by `build.py` from the source data.

Model use at data-build time:
- none at data-build time: parsing and replay are deterministic code.
