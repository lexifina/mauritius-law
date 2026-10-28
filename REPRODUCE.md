# How this repository was made (Mauritius)

This file is written by the build on 2026-10-28, the day the source data was retrieved. Everything below is enough to rebuild this repository hash for hash.

## Sources

| Source | Address | Retrieved | Requests | Licence |
| --- | --- | --- | ---: | --- |
| Attorney General's Office, Laws of Mauritius | <https://attorneygeneral.govmu.org/> | 2026-10-28 | 0 | Copyright Government of Mauritius; non-commercial copying allowed; commercial use needs Solicitor General approval |
| National Assembly of Mauritius, Acts as passed and gazetted | <https://mauritiusassembly.govmu.org/mauritiusassembly/index.php/acts-2/> | 2026-10-28 | 0 | Official texts |
| Internet Archive (Wayback) captures of attorneygeneral.govmu.org | <https://web.archive.org/> | 2026-10-28 | 0 | Captures of the AG site; same terms |

Every response is cached by the sha256 of its URL, and the table above lists every source with its retrieval date. Lexifina keeps its fetch scripts privately; `tools/parse.py` reads only the cache and writes `corpus.json`, so the same cache always gives the same corpus.

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

## Commands, in order

```sh
# fill the cache from the sources listed above (network; Lexifina's fetch scripts are kept privately)
python -X utf8 tools/parse.py                  # cache -> corpus.json (no network)
python -X utf8 tools/build.py <folder> --repo <empty folder> --docs-date 2026-10-28
python -X utf8 tools/check_repo.py <repo> --corpus <folder>/corpus.json --write
python -X utf8 tools/build.py <folder> --repo <another empty folder> --docs-date 2026-10-28 --compare <repo>
```

In the main Lexifina repository these files live as follows:
- `fetch.py`, `parse.py` and `repo/`: in `scripts/mauritius-law/`.
- `build.py`, `check_repo.py` (`check.py` there) and `on.py`: in `scripts/gitlaw_common/`, with the corpus format in `SCHEMA.md`.

## Exact inputs

- `corpus.json` sha256 `1c34fd6f568df94cb189a064113aa2766a4d6021ac8c366fe8cd87eb8f2f11a2` (714815 bytes)
- Script blob hashes (`git hash-object`):

| Script | Blob |
| --- | --- |
| build.py | `2bfc4ac9df193a7be844556d7f69cdb9d93ec3f2` |
| check.py | `cd346c7ff9996d450240e3bab52bcae03b2bc865` |
| on.py | `644a6c0857fbcc72562032b58d57aa113fee0641` |
| fetch.py | `652ed982d1f83657f41425b73b076d84fa23f210` |
| parse.py | `38b8a197c03d0313692e2c1c74fa0f755c12c0d1` |

- Tools:
  - Python 3.14.0
  - git version 2.52.0.windows.1
  - curl 8.21.0 (Windows) libcurl/8.21.0 Schannel zlib/1.3.2 WinIDN WinLDAP

## Checks

4 versions of 1 acts read back from git on the day each took effect and on the day before: 0 mismatches. Trailers: 0 commits whose trailers git cannot parse. Health: 0 merge commits on main, 0 commit dates that go backwards, 0 commits dated after the build.

A second build from the same corpus into an empty folder gave identical hashes for all 5 history commits and 8 tags (history tip `5f3271d5ba4db98aa8288ed8bb2cb5c132f22a3c`).

These results are from the check run on the build before this one, from the same corpus.

## Decisions made by hand

- Mauritius has no Peachjam LII; sources are the Attorney General's Office consolidations and the National Assembly's Acts as passed.
- The AG Constitution text (unchanged since the 2015-12-21 Wayback capture apart from the electoral boundaries GN, now a separate file) is dated 2011-12-12, the commencement of the latest change it records (Act 35 of 2011); it is credited to the publisher as the oldest version available.
- Only sections 1-122 and the First to Fourth Schedules are the Constitution; the subsidiary regulations and boundary notices in the same PDF are left out.
- The publisher's bracketed amendment notes and the Khoyratty editorial note on s. 5(3A) are kept out of the text.
- Acts 30 of 2015, 28 and 30 of 2016 are replayed from the gazetted Act PDFs; their in-force dates are the National Assembly table's 'Date in force' (10 Dec 2015; 17 Dec 2016).
- Act 3 of 2014 changes no words; it is recorded as a note on the First Schedule.
- Act 10 of 2025 (repeals s. 7(2)) is NOT applied: the Act has no commencement section but the National Assembly table marks it '*' (by Proclamation), and no Proclamation was found. Today's s. 7 may therefore be out of date.

## What AI did and did not do

The fetch, parse and check scripts were written by Claude Code running Claude Opus 5.5 (`claude-opus-5-5`) under Lexifina's direction.

No model wrote, translated or corrected legal text. No model runs at build time. Every word of law in this repository comes from the official sources above, through code that anyone can rerun.

The commit messages are written by `build.py` from the source data.

Model use at data-build time:
- none at data-build time: parsing and replay are deterministic code.
