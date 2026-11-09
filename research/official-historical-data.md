# Official historical data for the deterministic ZUS old-age pension core

Research for issue #3: *Which official historical data does the core need, and where is it published?*
Researched 2026-11-09. Every value quoted below was read from the primary source cited next to it
(Monitor Polski PDF text, statute text, or the publishing body's own page), not from press coverage.

## TL;DR

- **Every series the core needs is published in the official journal _Monitor Polski_ (M.P.).** M.P. is the
  primary source. Each announcement is a one-paragraph act ("ogłasza się, że wskaźnik … wynosi X %"), except the
  life-expectancy table, which is a large annex.
- **The Sejm ELI API (`api.sejm.gov.pl/eli`) makes M.P. machine-discoverable.** You can search by title and
  download the PDF for each announcement, back to 1998/1999. The values still have to be pulled out of the PDF
  text. They are not structured data.
- **ZUS republishes most series as HTML pages on zus.pl, and each value cites its M.P. position.** The pages are
  easy to scrape, but they are secondary and sometimes incomplete. For example, the sub-account list leaves out
  2012, and the historical wage table stops at 2022.
- **GUS has a real JSON API (BDL, CC BY 4.0), but it does not carry the statutory series.** The BDL wage figure
  does not match the statutory "przeciętne wynagrodzenie w gospodarce narodowej". BDL has no pension
  life-expectancy table and no valorization indices. dane.gov.pl has no datasets for these indices either.
- **Redistribution in an open-source repo is legally safe.** M.P. announcements are official documents, and art. 4
  of the copyright act puts them outside copyright entirely. The raw numbers are facts. GUS and ZUS ask for
  attribution, and so does the open-data act's permitted-conditions regime. Recommendation: commit the values as
  CSV with a source column (M.P. year/position) and a NOTICE crediting GUS, ZUS and the Ministry.
- **Pre-2008 M.P. PDFs are scanned with an OCR text layer, and the OCR has real errors.** Transcribing them by hand
  needs double-entry checks (see Pitfalls).

## 1. What the core needs, and the statutory basis for each series

Statute references are to the consolidated texts:
- Pension act (*ustawa o emeryturach i rentach z FUS*): Dz.U. 2025 poz. 1749,
  <https://api.sejm.gov.pl/eli/acts/DU/2025/1749/text.pdf>
- Social insurance system act (*ustawa o systemie ubezpieczeń społecznych*): Dz.U. 2026 poz. 199,
  <https://api.sejm.gov.pl/eli/acts/DU/2026/199/text.pdf>

| Series | Formula / role in the calculation | Who announces, legal basis, deadline | First value |
|---|---|---|---|
| Annual account valorization index (contributions + initial capital) | Contributions on the account as of 31 Jan of year X, × index "za rok X", applied from **1 June of year X+1**. Formula: CPI + real growth of the contribution sum, floored at CPI. 2 decimals. (pension act art. 25 ust. 3–10) | Minister for social security, **obwieszczenie**, by the 25th of the month before valorization (art. 25 ust. 12) | za 2000: 112,72 % |
| Quarterly account valorization index | Used when a pension is granted mid-year, for contributions recorded after 31 Jan. Art. 25a ust. 2 fixes which quarter is "last" for each award quarter. | Same minister and basis (art. 25 ust. 12: "za poprzedni rok i kwartał") | Q2 1999 (ZUS komunikat, M.P. 1999 poz. 440); the ZUS table starts at Q1 2007 |
| Annual sub-account valorization index | Average 5-year nominal GDP dynamics. Applied from 1 June. (system act art. 40c ust. 1–5) | **Prezes GUS**, komunikat, by 15 May (art. 40c ust. 6) | 2012: 107,54 (M.P. 2012 poz. 299). Sub-accounts exist since 1 May 2011. |
| Quarterly sub-account valorization index | **Derived**: `WKW = (WRW − 100 %)/4 + 100 %` from the latest annual index, 2 decimals (art. 40d ust. 5–6). The core can compute it and only needs the annual series. | Prezes ZUS, komunikat (art. 40d ust. 7) | Q4 2011 (M.P. 2011 poz. 1199) |
| Initial capital valorization | First valorization (1 June 2000, "za 1999") uses its own index: wage growth 1999/1998 net of employee contributions (art. 173 ust. 4) = **115,60 %**. From "za 2000" on, the account indices apply (art. 173 ust. 5, 5a). | 115,60 % is stated on the ZUS page (see §2.5). I found **no M.P. announcement** of it via ELI title search. | 1999 |
| Life-expectancy divisor ("tablica średniego dalszego trwania życia") | Pension = base / life expectancy in months at retirement age (years + completed months), **one unisex table** (art. 26 ust. 1–3). Initial capital uses the value at age 62 (art. 173 ust. 2). | **Prezes GUS**, komunikat, by **31 March** each year (art. 26 ust. 4). Applies to applications filed **1 April – 31 March next year** (art. 26 ust. 5). The table in force when the person reached 60 (women) / 65 (men) is used if more favourable (art. 26 ust. 6). | 1999 (M.P. 1999 poz. 173) |
| 30× contribution-base cap + projected average wage | Annual pension/disability contribution base ≤ 30 × projected monthly average wage from the budget act (system act art. 19 ust. 1) | Minister, obwieszczenie, by end of the previous year (art. 19 ust. 10). Since 2009 the title also includes the projected wage. | 1999: 50 375,22 zł (M.P. 1998 poz. 658) |
| Pension indexation index (waloryzacja emerytur) | From 1 March. Average CPI (pensioner-household or general, whichever is higher) + at least 20 % of real wage growth (pension act art. 88–89). | Minister, **komunikat**, within 3 working days of the GUS CPI/wage komunikaty (art. 89 ust. 6). The real-wage "top-up" is agreed by the Social Dialogue Council (M.P.) or set by Council of Ministers regulation (Dz.U.) (art. 89 ust. 4–5). | The ministerial komunikat series in M.P. starts 2009. Earlier years used other instruments (see §2.6). |
| Minimum pension | Amount fixed in art. 85 ust. 2 (currently 1588,44 zł), raised each year by the indexation index (art. 85 ust. 3) | Prezes ZUS, komunikat, ≥ 7 working days before 1 March (art. 94 ust. 2). In some years a special statute set the amount instead (see §2.7). | 1999 (M.P. 1999 poz. 256) |
| Average wage in the national economy | Statutory annual figure (pension act art. 20 pkt 1a). Used for replacement-rate definitions and for initial capital (wage ratios). | Prezes GUS, komunikat, by the 7th working day of February (art. 20) | ZUS table from 1950 |
| Base amount (kwota bazowa) | Needed for initial capital: fixed at **1220,89 zł** = average wage Q2 1998 (art. 174 ust. 7, art. 177). It is a statutory constant, not a series. | — | — |

## 2. Where each series is published: URLs, format, update rhythm

### 2.0 Monitor Polski, the master source, and how to reach it by machine

- **Legally binding e-edition:** <https://monitorpolski.gov.pl/> (Government Legislation Centre, RCL). Per-act URL
  pattern: `https://monitorpolski.gov.pl/MP/{year}/{pos}` (e.g. <https://monitorpolski.gov.pl/MP/2026/506>,
  HTTP 200). The PDFs are electronically signed.
- **Sejm ELI API (ISAP), recommended for automation.** JSON search plus a PDF per act, no key needed:
  - Search: `https://api.sejm.gov.pl/eli/acts/search?publisher=MP&title=<words>&limit=500`
    (also accepts `dateFrom`, `dateTo`).
  - Metadata: `https://api.sejm.gov.pl/eli/acts/MP/2026/506`
  - Text: `https://api.sejm.gov.pl/eli/acts/MP/2026/506/text.pdf`. M.P. items are PDF only (`textHTML: false` in
    metadata).
  - Mirror: `https://eli.gov.pl/eli/MP/2026/487/ogl/pol/pdf`
  - Human UI: `https://isap.sejm.gov.pl/isap.nsf/DocDetails.xsp?id=WMP20260000506`
- **Title queries that return complete series** (counts as of 2026-11-09):

| Query (`publisher=MP`, `title=`) | Hits | Covers |
|---|---|---|
| `trwania życia` | 29 | Life tables 1999–2026, plus a corrected 2010 table (M.P. 2010 poz. 195 "zmienionej tablicy") |
| `ograniczenia rocznej podstawy` | 29 | 30× cap for 1999–2027 |
| `rocznej waloryzacji` | 15 | GUS sub-account annual index 2012–2026 |
| `wskaźnika waloryzacji składek` | 150 | Ministry annual and quarterly account indices (2008→), ZUS sub-account quarterly komunikaty, the 1999 ZUS Q2 komunikat |
| `waloryzacji` + `dateTo=2009-06-01` | — | Older annual account indices: **M.P. 2006 poz. 453** (2000–2005 in one act), 2007 poz. 453 (2006), 2008 poz. 388 (2007), 2009 poz. 469 (2008). Titles say "waloryzacji składki" (singular) or "składek za lata". |
| `waloryzacji emerytur i rent` | 34 | Ministry indexation komunikaty 2009–2026 (plus pre-1999 items) |
| `najniższej emerytury` | 42 | ZUS minimum-pension komunikaty, **with gaps** (see §2.7) |
| `przeciętnego wynagrodzenia w gospodarce narodowej w` | 81 | GUS annual wage komunikaty (plus regional/half-year obwieszczenia, which can be filtered out) |

- **Format:** one PDF per announcement. From about 2012 the PDFs are born-digital, and `pdftotext -layout` gives
  clean text. The value is always in a fixed sentence, so a regex can capture it, e.g.
  `wynosi\s+([0-9]+,[0-9]+)\s*%`. 2009–2012 PDFs carry an RCL watermark whose letters leak into the extracted
  text. 2006–2008 PDFs use a broken font encoding (`ł→∏`, `ę→´`, `ą→à`). Pre-2006 issues are **scans with an OCR
  layer** (`Producer: Adobe Acrobat 9.0 Paper Capture Plug-in`), with several acts per page.
- **Update rhythm:** fixed statutory deadlines (table in §1). Annual account index: May. GUS sub-account index:
  by 15 May. Quarterly indices: about 3 months after the quarter. Life table: by 31 March. Cap: November–December.
  Pension indexation and minimum pension: February.

### 2.1 Annual and quarterly account valorization indices

- Primary: Ministry obwieszczenia in M.P. (see §2.0).
- ZUS mirror (HTML `<table>` with columns *rok / wskaźnik waloryzacji / publikacja*, each row citing M.P.):
  - Annual 2000–2025:
    <https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/wskazniki/waloryzacja-skladki-i-kapitalu-poczatkowego/wskazniki-waloryzacji-rocznej>
  - Quarterly Q1 2007 → present (78 rows):
    <https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/wskazniki/waloryzacja-skladki-i-kapitalu-poczatkowego/wskazniki-waloryzacji-kwartalnej>
  - Explanation page (rules, 115,60 % note):
    <https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/wskazniki/waloryzacja-skladki-i-kapitalu-poczatkowego>
- I spot-checked the ZUS table against M.P. for 2000–2008 and 2022–2025, and every value matched.
- Quarterly indices before 2007 (1999–2006) exist only in M.P. The 2008 obwieszczenie (M.P. 2008 poz. 823)
  back-filled Q1 2007–Q3 2008 in one act. Quarterly indices for 1999–2006 were not checked in depth.

### 2.2 Sub-account valorization indices

- Annual: GUS komunikaty in M.P. Also on stat.gov.pl as a per-type archive, URL pattern
  `…/komunikat-w-sprawie-wskaznika-rocznej-waloryzacji-rowny-…,279,N.html`.
- Quarterly: ZUS komunikaty in M.P. These can be derived exactly from the annual value: 109,49 → 102,37;
  110,61 → 102,65. Both match the published figures.
- ZUS mirror (**prose list, not a table**; annual list 2013–2026, quarterly 2012–2026):
  <https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/wskazniki/wysokosc-wskaznika-kwartalnej-i-rocznej-waloryzacji-skladek-na-ubezpieczenie-emerytalne-i-odsetek-za-zwloke-zewidencjonowanych-na-subkoncie>.
  **The ZUS annual list leaves out the first GUS index (2012: 107,54, M.P. 2012 poz. 299).** Use M.P. as the
  master.

### 2.3 Life-expectancy tables (the pension divisor)

- Primary: GUS komunikat in M.P., one per year since 1999 (M.P. 1999 poz. 173 … M.P. 2026 poz. 319). The annex is
  a **matrix of age in completed years × completed months 0–11**, with values in months to 1 decimal.
  - The age range has changed over time: **30–80** in the 1999 table, **30–90** in the 2026 table.
  - The 2026 PDF is about 22 pages of annex. The 1999 PDF is 2 scanned pages.
- stat.gov.pl republishes the table **as an inline HTML table** (no file attachment) for 2014–2026:
  - 2026:
    <https://stat.gov.pl/sygnalne/komunikaty-i-obwieszczenia/lista-komunikatow-i-obwieszczen/komunikat-w-sprawie-tablicy-sredniego-dalszego-trwania-zycia-kobiet-i-mezczyzn,285,14.html>
  - Earlier years: same slug with `,285,N.html`, N = 2 (2014) … 13 (2025).
- Validity rule: the table published by 31 March year Y applies to applications filed 1 April Y – 31 March Y+1
  (art. 26 ust. 5), with the 60/65 most-favourable rule (art. 26 ust. 6). **2010 had two tables**: M.P. 2010
  poz. 194 and its correction poz. 195 ("zmienionej tablicy"). The core must decide which one governs which
  applications (not researched).
- GUS's general "Trwanie życia" publications and BDL life-expectancy variables are **sex-specific demographic life
  tables**. They are not the unisex statutory table and must not be used as the divisor.

### 2.4 Contribution-base cap (30×) and projected average wage

- Primary: Ministry obwieszczenia in M.P., 1999–2027 (29 acts). Since the 2009 act the projected average wage is
  stated alongside the cap. For earlier years only the cap is given (e.g. 1999: 50 375,22 zł). The projected
  wage then has to be derived as cap/30 or taken from that year's budget act.
- ZUS mirror (prose list, each value citing M.P.):
  <https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/wskazniki/roczna-podstawa-wymiaru-skladek-na-ubezpieczenia-emerytalne-i-rentowe-od-1999-r>
  (1999–2026).

### 2.5 Initial capital valorization

- The 1999 index (115,60 %) comes from the ZUS explanation page linked in §2.1. Quote: *"pierwsza waloryzacja
  przeprowadzana jest od dnia 1 czerwca 2000 r. za rok 1999 - według odrębnego wskaźnika waloryzacji kapitału
  początkowego za rok 1999 wynoszącego 115,60% (wyliczonego zgodnie z zapisem art. 173 ust. 4 ustawy
  emerytalnej). Kolejne waloryzacje kapitału początkowego, tj. za rok 2000 i lata następne, przeprowadzane są
  według wskaźników waloryzacji ogłaszanych dla składek"*.
- I could not find a Monitor Polski act announcing 115,60 %. ELI title search for "kapitału początkowego" returns
  0 hits, and the 2000 wage announcements contain no such index. **This is the one value whose primary source is
  ZUS, not M.P.** Follow-up: check the 2000 M.P./Dz.U. issues by hand, or derive it from the art. 173 ust. 4
  formula.
- Inputs needed if the core computes initial capital itself (not just valorizes it):
  - base amount 1220,89 zł (statute);
  - life expectancy at age 62 from the 1999 table (209,0 months, M.P. 1999 poz. 173);
  - historical annual average wages, available as a ZUS HTML table for 1950–2022:
    <https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/wskazniki/przecietne-wynagrodzenie-w-latach>.

### 2.6 Pension indexation index

- 2009 → present: Ministry komunikat "w sprawie wskaźnika waloryzacji emerytur i rent w {Y} r." in M.P.
  (art. 89 ust. 6).
- The top-up decision appears either as a Council of Ministers regulation in **Dziennik Ustaw** ("w sprawie
  wysokości zwiększenia wskaźnika waloryzacji emerytur i rent w {Y} r.", 2009–2027 via ELI `publisher=DU`), or as
  an RDS-agreement komunikat.
- **Irregular years** use other instruments. ELI shows a 2003 indexation statute (Dz.U. 2002 poz. 2054), RM
  regulations for 2004, 2006 and 2008, and an obwieszczenie for 2001 (M.P. 2001 poz. 102). The ELI title search
  finds no ministry komunikat for 2012 (a year with a lump-sum "kwotowa" indexation, not verified here). Years
  before 2009 need per-year manual sourcing.
- Indexation is only needed if the core models benefits after the award. The issue lists it, so it is included.

### 2.7 Minimum pension amounts

- Primary: ZUS komunikat under art. 94 ust. 2 in M.P., each February (since 2002), or May (1999–2002).
- **Gaps:** ELI title search finds no such komunikat for 2005, 2007, 2012, 2015, 2017, 2019, 2020 or 2023. I
  listed all ZUS M.P. acts for Feb–Mar 2019 and Feb–Mar 2023, and neither month contains one. In at least some
  years a special statute set the amount instead:
  - 2021: ZUS komunikat issued under art. 12 of the amending act of 21 Jan 2021 (M.P. 2021 poz. 223), title
    "w sprawie kwoty najniższej renty … kwoty najniższej emerytury …";
  - the current art. 85 ust. 2 hard-codes 1588,44 zł.
  - Each gap year needs manual sourcing from the amending statute.
- ZUS's own page shows **current values only**:
  <https://www.zus.pl/swiadczenia/emerytury/kwoty-najnizszych-swiadczen-emerytalno-rentowych>.

### 2.8 Average wage series

- Statutory annual figure: GUS komunikat "w sprawie przeciętnego wynagrodzenia w gospodarce narodowej w {Y} r." in
  M.P., under art. 20 pkt 1a of the pension act.
- ZUS table (HTML, 1950–2022, **not updated past 2022** as of today):
  <https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/wskazniki/przecietne-wynagrodzenie-w-latach>.
  - **Note the 1999 break:** 1998 = 1239,49 zł/month, 1999 = 1706,74 zł/month. This is the gross-up for employee
    social contributions introduced with the reform. Replacement-rate definitions must handle it.
- dane.gov.pl dataset 1755 "Przeciętne miesięczne wynagrodzenie w gospodarce narodowej od roku 1950" (GUS,
  CC BY 4.0) is **only a link** to the stat.gov.pl publication page
  (`https://api.dane.gov.pl/1.4/datasets/1755/resources`).
- **GUS BDL API is not a drop-in source.** For Poland in 2024, BDL subject P2497, variable 64428 ("przeciętne
  miesięczne wynagrodzenia brutto, ogółem") gives **8630,27 zł**, while the statutory figure is **8181,72 zł**
  (M.P. 2025 poz. 125). BDL's series also starts only in 2002. The difference is probably a scope difference (BDL
  regional wage data typically excludes small units), but I did not verify the methodology. Use M.P. figures for
  anything statutory.

## 3. Machine-readability summary

| Source | What you get | Machine-readable? |
|---|---|---|
| Sejm ELI API (`api.sejm.gov.pl/eli`) | JSON index of every M.P./Dz.U. act plus PDF per act | **Index: yes. Values: no.** PDF text extraction plus regex. Clean from about 2012. OCR/encoding problems before that. |
| monitorpolski.gov.pl | Signed PDFs (legally binding version) | No (PDF) |
| zus.pl knowledge base | HTML tables (annual and quarterly account indices, average wage 1950–2022). Prose lists (sub-account, cap, base amount). | **Scrapable**, with M.P. citations per row. Secondary, may lag or omit. Pages are about 1 MB each because of the navigation chrome. |
| stat.gov.pl komunikaty archive | HTML page per komunikat (life tables 2014–2026 as inline `<table>`) | Scrapable |
| GUS BDL API (`bdl.stat.gov.pl/api/v1`) | JSON, CC BY 4.0. Anonymous limits: 5 req/s, 100/15 min, 1 000/12 h, 10 000/7 days; higher with a free `X-ClientId` key (<https://api.stat.gov.pl/Home/BdlApi>). | Yes, but **none of the statutory pension series are there** in statutory form. |
| dane.gov.pl (`api.dane.gov.pl/1.4`) | ZUS benefit-distribution datasets, GUS publication links | Not useful for these indices. Searches for "wskaźnik waloryzacji składek" and "trwania życia" return no relevant dataset. |

Practical conclusion: build the dataset **once by hand-verified transcription from M.P.**, using the ELI API to
enumerate and fetch acts. Use the ZUS tables as an independent cross-check (double entry). Then add each year's
handful of new values (about 10 per year) by hand, citing the M.P. position.

## 4. Reuse terms: can we commit these numbers to a public repo?

**Yes.** The legal reasoning, layer by layer:

1. **Copyright does not apply to the announcements.** Copyright act art. 4 (consolidated text Dz.U. 2025 poz. 24,
   <https://api.sejm.gov.pl/eli/acts/DU/2025/24/text.pdf>) reads: *"Nie stanowią przedmiotu prawa autorskiego:
   1) akty normatywne lub ich urzędowe projekty; 2) urzędowe dokumenty, materiały, znaki i symbole; …"*.
   - Komunikaty and obwieszczenia of the Minister, Prezes GUS and Prezes ZUS published in M.P. are official
     documents, so they are not works under copyright at all.
   - The life-expectancy table is an annex to such a komunikat.
   - Individual numbers (index values, amounts) are facts and would not be protected in any case.
2. **Open-data / PSI re-use act** (*ustawa o otwartych danych i ponownym wykorzystywaniu informacji sektora
   publicznego*, Dz.U. 2021 poz. 1641; consolidated text Dz.U. 2023 poz. 1524,
   <https://api.sejm.gov.pl/eli/acts/DU/2023/1524/text.pdf>):
   - art. 5: everyone may re-use public-sector information published in BIP, the data portal, or another ICT
     system of an obliged body;
   - art. 14 ust. 1: re-use is **unconditional** unless the act says otherwise;
   - art. 17: re-use is **free of charge**;
   - art. 15 ust. 1: the body *may* impose only these conditions: cite the source and the time of
     creation/acquisition, state that the information was processed, and limit the body's liability;
   - art. 11 ust. 5: no stated conditions means re-use without conditions.
   - GUS, ZUS and the Ministry are obliged bodies (art. 3 pkt 1, public-finance-sector units).
3. **ZUS's stated conditions** (BIP page,
   <https://www.zus.pl/web/biuletyn-informacji-publicznej/obsluga-klienta/ogolne-zasady-zalatwiania-spraw/ponowne-wykorzystywanie-informacji-sektora-publicznego>):
   - Re-users must **cite the source, the date of creation and the date of acquisition**.
   - They must say whether the information is used in whole or in part.
   - They must **flag any processing**.
   - ZUS takes no responsibility for processed information.
   - Re-use of information published in BIP or the central repository is free.
   - The page still cites the repealed 2016 PSI act (Dz.U. 2016 poz. 352), but its conditions fall within
     art. 15 ust. 1 of the 2021 act.
4. **GUS terms** (<https://stat.gov.pl/copyright>): GUS *"nie ma zastrzeżeń co do kopiowania plików i stron oraz
   dokonywania wydruków, w tym do własnych opracowań, pod warunkiem, że będzie podane ich źródło"*, and accepts
   *"prezentowanie własnych opracowań (zmian treści) w oparciu o dane GUS"*. BDL API data are explicitly **CC BY
   4.0**. GUS-published dane.gov.pl datasets are CC BY 4.0.
5. **Sejm ELI/ISAP** only republishes official acts. The underlying texts fall under art. 4 as above.

**Recommendation for the repo:**
- Store each series as CSV with columns such as `period, value, unit, source_act` (e.g. `M.P. 2026 poz. 506`),
  `source_url`, `retrieved`.
- Add a `NOTICE` / data README stating: the source bodies (GUS, ZUS, Minister of Family, Labour and Social Policy);
  that the values are transcribed (i.e. processed) from Monitor Polski; the retrieval date; and that the source
  bodies bear no responsibility.
- This meets every condition ZUS and GUS impose and is compatible with any OSI licence for the code. CC BY 4.0
  would be the natural label for the data directory.
- Do **not** commit the signed RCL PDFs unless needed. Linking to them is enough, and it keeps the repo small.
  Committing them is also legally fine under art. 4.

## 5. Sample values (format demonstration)

All values were read from the PDF text of the cited M.P. act, fetched via
`https://api.sejm.gov.pl/eli/acts/MP/{year}/{pos}/text.pdf`.

| Series | Period | Value | Source |
|---|---|---|---|
| Account annual index | za 2023 (applied 1 VI 2024) | 114,87 % | M.P. 2024 poz. 382 |
| Account annual index | za 2024 (applied 1 VI 2025) | 114,41 % | M.P. 2025 poz. 449 |
| Account annual index | za 2025 (applied 1 VI 2026) | 109,81 % | M.P. 2026 poz. 506 |
| Account annual index | za 2000 … 2005 | 112,72 / 106,68 / 101,90 / 102,00 / 103,63 / 105,55 % | M.P. 2006 poz. 453 (scan) |
| Account quarterly index | Q3 2025 | 100,30 % | M.P. 2025 poz. 1253 |
| Account quarterly index | Q1 2007 … Q3 2008 | 112,73 / 101,20 / 100,00\* / 101,81 / 113,72 / 103,93 / 100,20 % | M.P. 2008 poz. 823 (\*Q3 2007 per ZUS table, cut off in my excerpt) |
| Sub-account annual (GUS) | announced May 2024 / 2025 / 2026 | 109,91 / 109,49 / 110,61 | M.P. 2024 poz. 370 / 2025 poz. 453 / 2026 poz. 487 |
| Sub-account annual (GUS) | first, 2012 | 107,54 | M.P. 2012 poz. 299 |
| Sub-account quarterly (ZUS) | Q3 2025 | 102,37 % (= (109,49−100)/4+100) | M.P. 2025 poz. 1195 |
| Initial capital index | za 1999 | 115,60 % | ZUS page (§2.5). No M.P. act found. |
| Life table 2026 (applications 1 IV 2026 – 31 III 2027) | age 60 y 0 m / 65 y 0 m | 268,9 / 222,7 months | M.P. 2026 poz. 319 |
| Life table 2025 | age 60 y 0 m / 65 y 0 m | 266,4 / 220,8 months | M.P. 2025 poz. 279 |
| Life table 1999 | age 60 / 62 / 65 (0 m) | 226,1 / 209,0 / 184,5 months | M.P. 1999 poz. 173 (scan) |
| 30× cap / projected avg wage | 2024 | 234 720 zł / 7824 zł | M.P. 2023 poz. 1342 |
| 30× cap / projected avg wage | 2025 | 260 190 zł / 8673 zł | M.P. 2024 poz. 1051 |
| 30× cap / projected avg wage | 2026 | 282 600 zł / 9420 zł | M.P. 2025 poz. 1206 |
| 30× cap | 1999 | 50 375,22 zł (no wage stated) | M.P. 1998 poz. 658 |
| Pension indexation | 2024 / 2025 / 2026 | 112,12 % / 105,5 % / 105,3 % | M.P. 2024 poz. 114 / 2025 poz. 129 / 2026 poz. 202 |
| Minimum pension (from 1 March) | 2024 / 2025 / 2026 | 1780,96 / 1878,91 / 1978,49 zł | M.P. 2024 poz. 133 / 2025 poz. 152 / 2026 poz. 220 |
| Minimum pension | 2021 | 1250,88 zł | M.P. 2021 poz. 223 (special-statute komunikat) |
| Average wage, national economy | 2023 / 2024 / 2025 | 7155,48 / 8181,72 / 8903,56 zł | M.P. 2024 poz. 110 / 2025 poz. 125 / 2026 poz. 192 |

Typical announcement text, e.g. M.P. 2026 poz. 506: *"Na podstawie art. 25 ust. 12 ustawy … ogłasza się, że
wskaźnik waloryzacji składek na ubezpieczenie emerytalne za 2025 r. wynosi 109,81 %."*

## 6. Pitfalls found while reading the sources

- **OCR errors in scanned M.P. issues.** In the 1999 life table (M.P. 1999 poz. 173) the extracted row labels read
  `… 53 64 55 66 57 … 75 78 77 …`. That means 54→"64", 56→"66" and 76→"78", and the age-64 row was garbled.
  Digits inside the matrix may be wrong in the same way. Every pre-2006 value needs manual double-entry against
  the page image.
- **Font-encoding garbage** in 2006–2008 PDFs and **watermark letters** in 2009–2012 PDFs. Numbers survive, but
  regexes must be tolerant.
- **Naming offsets.** The account index "za rok X" is applied from 1 June X+1. The GUS sub-account komunikat names
  no year and is applied from 1 June of the year it is published. The life table published in year Y applies from
  1 April Y. The CSVs should store both the "period" label and the "applied from" date.
- **Percent vs index.** Ministry and ZUS values are written "114,41 %". The GUS sub-account value is written
  "109,49 (wzrost o 9,49 %)". Decimal commas throughout.
- **ZUS mirrors are incomplete or lagging.** The sub-account list omits 2012, and the average-wage table ends at
  2022. Treat ZUS as a cross-check only.
- **Minimum pension and pension indexation have legislative irregularities.** In some years special statutes
  overrode the standard procedure, so the title-based series has holes that need manual sourcing.
- **2010 has two life tables** (original plus a corrected one, M.P. 2010 poz. 194/195).
- **BDL ≠ statutory wage**, and BDL/GUS demographic life tables ≠ the unisex statutory divisor table.

## 7. Open follow-ups

1. Find the primary act, if any, behind the 115,60 % initial-capital index for 1999, or reproduce it from the
   art. 173 ust. 4 formula.
2. Source the minimum pension for the gap years (2005, 2007, 2012, 2015, 2017, 2019, 2020, 2023) from the
   amending statutes.
3. Source the pension indexation index for 1999–2008 and 2012, where it was not a standard M.P. komunikat.
4. Collect the quarterly account indices for 1999–2006 from M.P. (not on the ZUS page).
5. Decide which 2010 life table governs which applications.
