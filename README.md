# Mauritius: law history in git

The law of Mauritius as a git history: one file per provision, one commit per law on the day it took effect, and the law that made each change as the commit's author. `git blame` names the law behind every paragraph; `git log` lists every change to a section; the tool below gives the text in force on any day.

It follows the shared design of [lexifina/global-law](https://github.com/lexifina/global-law), which places it at `mu/`.

**What is in it now**

| Folder | Act | Versions | History |
| --- | --- | ---: | --- |
| `constitution/` | The Constitution of Mauritius (GN 54 of 1968) | 4 | From the Attorney General's Office consolidation (in force 12 December 2011, the latest change it records) through Act No. 3 of 2014 (no words changed), Act No. 30 of 2015 and Acts Nos. 28 and 30 of 2016. **Act No. 10 of 2025 is not applied, so today's s. 7 is unconfirmed:** see below |

The Constitution's four versions:

| In force | What changed | Author |
| --- | --- | --- |
| 2011-12-12 | The Attorney General's Office consolidation (Revised Laws of Mauritius), whose latest recorded change is Act No. 35 of 2011 | publisher: oldest version available |
| 2014-07-17 | First Schedule, wording unchanged: for the first general election after 17 July 2014 a candidate may elect not to declare his community, notwithstanding paragraph 3 of the Schedule | Act No. 3 of 2014, The Constitution (Declaration of Community) (Temporary Provisions) Act 2014 |
| 2015-12-10 | s. 8(4): new paragraph (aa), confiscation of property disproportionate to a person's emoluments and other income | Act No. 30 of 2015, The Constitution (Amendment) Act 2015 |
| 2016-12-17 | s. 15(3): paragraphs (a) and (b) qualified and new paragraph (ca), restrictions on movement under a law relating to offences or acts of terrorism | Act No. 28 of 2016, The Constitution (Amendment) Act 2016 |
| 2016-12-17 | s. 16(4): new paragraph (ab), a minimum number of candidates of a particular sex for the Rodrigues Regional Assembly | Act No. 30 of 2016, The Constitution (Amendment No. 2) Act 2016 |

The files are `constitution/s-NNN.md` for each section (`s-017-a.md` for s. 17A) and `constitution/sch-01.md` to `sch-03.md` for the First to Third Schedules: 135 files, sections 1 to 122, ten lettered sections (17A, 30A, 30B, 36A, 75A to 75E, 91A) and three Schedules (the Fourth Schedule was repealed by Act 31 of 2000 and has no file). `constitution/README.md` lists every provision, how often it changed and when.

**Section 7 may not be today's law.** Act No. 10 of 2025, The Constitution (Amendment) Act 2025 (assented 14 May 2025; Legal Supplement to the Government Gazette No. 40 of 2025), "amended by repealing subsection (2)" of section 7. The Act has no commencement section, and the National Assembly's table of Acts gives its date in force as "*", which it explains as "the Act will come into operation on a date to be fixed by Proclamation". No such Proclamation has been found, so the Act is not applied and `constitution/s-007.md` still holds subsection (2) (punishment lawful in Mauritius on 11 March 1964). Until the Proclamation is found and dated, the text of s. 7 from 2025 on is unconfirmed.

## A lawyer's questions as git commands

| Question | Command |
| --- | --- |
| What did s. 15 say on 1 January 2016? | `python tools/on.py 2016-01-01 constitution 15` |
| The same, naming the law behind the text | `python tools/on.py --law 2016-01-01 constitution 15` |
| The same, with raw git (bash) | `git show $(git rev-list -1 --before="2016-01-01T23:59:59Z" main -- constitution/s-015.md):constitution/s-015.md` |
| The same, in PowerShell | `$r = git rev-list -1 --before="2016-01-01T23:59:59Z" main -- constitution/s-015.md; git show "$($r):constitution/s-015.md"` |
| Which law wrote each paragraph of s. 8? | `git blame constitution/s-008.md` |
| Every change to s. 15, word by word | `git log -p --word-diff constitution/s-015.md` |
| Every law in the record, in order | `git log --reverse --format="%ad %an" --date=short main` |
| Everything one Act changed | `git show law/act-2016-28` (tags `law/<key>` mark each law's commit) |
| The whole Constitution on a date | `python tools/on.py 2015-06-01 constitution` |
| The Constitution as of a version | `git show constitution/2015-12-10` (tags `<act>/<in-force day>`) |
| The laws that produced the oldest text | `git log -1 --format=%B constitution/2011-12-12` |
| Laws passed but not yet in force | `git log main..future` (none today; the branch does not exist yet) |

After a rebuild, refresh tags with `git fetch --force --tags`: a plain fetch keeps stale tags.

## Dates

- Each commit is dated at **noon, Indian/Mauritius, on the day the change took effect**. The in-force day is also in the `In-force:` trailer, which the tools read; the dates the Act was enacted and published are trailers too.
- **Default rule.** Constitution, s. 46(4): no law comes into operation until it is published in the Gazette, but Parliament may postpone its coming into operation or make it retrospective. The in-force day used is the National Assembly's "Date in force" column, or the Act's own commencement section: 17 July 2014 (Act 3 of 2014), 10 December 2015 (Act 30 of 2015) and 17 December 2016 (Acts 28 and 30 of 2016), each the day the table gives for publication in the Gazette.
- **The oldest version** is dated 12 December 2011, the day from which Act No. 35 of 2011, the latest change the Attorney General's consolidation records, took effect.

## How it is checked

- Every version is read back from git on the day it took effect and on the day before (`tools/check_repo.py`), and must match `corpus.json`: 0 mismatches. No merge commits on `main`, commit dates never go backwards, nothing is dated in the future, and every commit's trailers parse.
- **The oldest version is the Attorney General's text**, read from the PDF's text layer. The same text is in the Internet Archive's capture of the PDF of 21 December 2015. Every version carries the sha256 of the file it was parsed from.
- **The later versions are replayed from the Acts as gazetted.** The words each Act inserts are cut by code from the National Assembly's PDF of that Act and put where the Act says; nothing is typed by hand or by a model.
- **No other Act amends the Constitution's words since 2011**, as far as the Acts online show: a search of all 251 Act PDFs of 2012 to 2026 on the National Assembly's site finds no amending Act other than those replayed and Act No. 10 of 2025 (other hits only cite sections of the Constitution). The Covid-19 (Miscellaneous Provisions) Act 2020, whose date in force the table gives as "See Section 58", never mentions the Constitution.
- **Every character checked.** Every provision of the oldest version was compared, character for character with spacing ignored, with a second, independent extraction of the Attorney General's PDF (pdftotext); every word each later version inserts was found in the Act that inserts it, and each Act's instruction (where, which words) was read against the result.
- **Not checked against an official text of today:** no official consolidation of the Constitution after 2011 was found (see below).
- The repository is rebuilt from the same data a second time and gives identical hashes. Details in [`REPRODUCE.md`](REPRODUCE.md).

## What "author" means

The author of a commit is the law that made the change, with an e-mail address that cannot exist (`act-2016-28@attorneygeneral.invalid`), so `git blame` and `git log --author` name laws. The committer is always Lexifina.

- **The first commit** is the oldest text available, credited to the publisher, the Attorney General's Office (`ago@attorneygeneral.invalid`), and names the 28 laws its amendment notes list as having produced it: 27 Acts from Act No. 2 of 1982 to Act No. 35 of 2011, and GN No. 139 of 1992.
- **Credits come only from the sources.** Each later change is credited to the Act whose own text makes it. There are no unexplained changes.
- **Act No. 3 of 2014** changed no words. Its commit touches no file; it names the First Schedule (`Words-changed: no`, `Affecting:`).
- Blame credits whole lines (one paragraph per line). Use `git log -p --word-diff` to see exactly which words each change made.

## What it does not show

- **Act No. 10 of 2025** (repeal of s. 7(2)): not applied, because its commencement is unknown (see above). The Act was also reprinted under the Reprint of Laws Act (Reprint No. 1 of 2025, Government Gazette No. 62 of 26 July 2025), with the same two sections; the reprint fixes no commencement date either.
- **No history before 12 December 2011.** The Constitution as enacted in 1968 and its amending laws to 2011 are not online as dated texts, so the 28 laws are named in the first commit but not replayed. The National Assembly's table of Acts as passed starts in 2010.
- **No official text of today.** The Attorney General's Constitution records no change after Act No. 35 of 2011, so it does not include the Acts of 2015 and 2016; its site says the Revised Laws there are "updated up to 30 June 2017". The text from 2015 on is the replay of the gazetted Acts, not a consolidation anyone has published.
- **Amendment notes, editorial notes and footnotes.** The publisher's bracketed notes ("[S. 1 amended by Act 48 of 1991.]") are left out of the text, and so are running heads, page numbers and the footnotes giving the UK chapter numbers of the British Nationality Acts and the Interpretation Act 1889.
- **Section 5(3A) is printed though declared void.** The Attorney General's text keeps s. 5(3A) (no bail for certain terrorism and drug offences) with an editorial note: "The Judicial Committee of the Privy Council has, in State v Khoyratty (2006) MR 210, declared section 5 (3A) of the Constitution void." The note is left out of `constitution/s-005.md`, which holds the words as printed, and the ruling has no commit: it predates the oldest version.
- **Material printed with the Constitution.** The Attorney General's PDF also prints subsidiary regulations and electoral boundary notices. Only sections 1 to 122 and the Schedules are the Constitution; the rest is left out.
- **Other acts.** Only the Constitution is here. The Civil Code, the Criminal Code (Cap. 195; its revised text is available but not parsed) and the Data Protection Act are not built yet.

Only the texts published by the Government of Mauritius are authoritative; the Attorney General's Office names the Revised Laws of Mauritius published by LexisNexis (Pte) Ltd. in September 2016, with their Supplements, as the sole authoritative text. This repository is a research aid, not legal advice.

## How it is built

Lexifina reads the sources listed here and records each act's dated versions, and the laws that made each change, in one corpus file. `tools/build.py` writes this repository from that file, and the same corpus always gives the same commit hashes; `REPRODUCE.md` gives the details and the checks. A model (Claude) wrote Lexifina's code; it wrote no legal text, and no model runs when the history is built.

Lexifina's code reads the sources as follows. `tools/revised.py` and `tools/units.py` read the Attorney General's PDF through its text layer with PyMuPDF (no OCR), rebuild paragraphs from the layout and drop running heads and notes; further code reads the National Assembly's table of Acts, replays the Acts and writes `corpus.json`; the shared builder `tools/build.py` turns that into the commits.

## Sources

| Source | What it gives | Used for | Terms |
| --- | --- | --- | --- |
| [Attorney General's Office, Laws of Mauritius](https://attorneygeneral.govmu.org/) | The Revised Laws of Mauritius, one PDF per law, with bracketed amendment notes; the Constitution's latest recorded change is Act No. 35 of 2011 | The oldest version | "Copyright 2011 – Government of Mauritius": users may copy and print portions of the laws in compliance with the Copyright Act of Mauritius, but should not publish, disseminate or incorporate them for commercial use without the prior approval of the Solicitor General |
| [National Assembly of Mauritius, Acts as passed and gazetted](https://mauritiusassembly.govmu.org/mauritiusassembly/index.php/acts-2/) | Acts as published in the Gazette, 2010 to 2026, as text-layer PDFs, and a table giving the dates each Act was introduced, passed, assented, gazetted and came into force | The amending Acts and their dates | No licence statement recorded; official texts |
| [Internet Archive](https://web.archive.org/) | Captures of the Attorney General's site: the Constitution PDF of 21 December 2015 (the same text); a capture of 21 September 2023 is cut off at 1 MiB and not used | Confirming the oldest version | The terms of the captured site |

The register this repository started from lists the source as "Laws of Mauritius; MauritiusLII" and notes that the e-revised edition is authoritative since 2022. This build found no Laws.Africa (Peachjam) LII for Mauritius, and the Attorney General's terms of use it read still name the LexisNexis edition of 2016; the e-revised edition was not checked here.

Research guides that cover every country: [GlobaLex](https://www.nyulawglobal.org/globalex/) and the [Law Library of Congress Guide to Law Online](https://www.loc.gov/research-centers/law-library-of-congress/researcher-resources/guide-to-law-online/).

## Licence

See [LICENSE](LICENSE). The legal texts are official publications of the Government of Mauritius, reproduced under the Attorney General's terms above; Lexifina's arrangement, messages, tables and tools are CC0.

Every source text here is in English, so there is no summary in French.
