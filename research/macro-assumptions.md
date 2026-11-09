# Officially published future macro assumptions for Polish pension projections

Research for issue #5. Checked against primary sources on 2026-11-09. Every number below
was read directly from the cited PDF, XLSX or API response unless marked otherwise.

## TL;DR

| Need of the deterministic core | Best official source | Directly usable? |
|---|---|---|
| CPI path | ZUS FE forecast 2026–2080 (Oct 2025), Tab. 1.1–1.3; MF guidelines (Sept 2026), Tab. 1–2 | **Yes** |
| Real wage growth path | Same two sources | **Yes** (ZUS to 2080, MF to 2060) |
| Unemployment, real GDP, contribution collection | ZUS FE forecast, Tab. 1.1–1.3 | **Yes** |
| Contribution cap (30 × projected average wage) | Statutory formula plus MF average-wage level (zł) to 2030, then grown at MF/ZUS wage growth | **Derive** (simple) |
| Annual account valorisation index (art. 25) | Not published as a forecast by anyone. Derive from ZUS FE results (sum of contribution bases) or from wage growth × employment growth | **Derive** |
| Quarterly valorisation (art. 25a) | Not published | **Derive** (convention needed) |
| Sub-account valorisation (art. 40a sus) | Not published. 5-year average of nominal GDP growth | **Derive** |
| Pension indexation (CPI + 20 % of real wage growth) | ZUS FE forecast assumes the statutory minimum. The index is not tabulated | **Derive** (trivial) |
| Life-expectancy divisor (unisex, months, at retirement age) | No official projection. GUS publishes only the current year's table. EUROPOP2025 (Eurostat) has projected mortality by age and sex to 2100 | **Derive** (non-trivial) |
| Projected life expectancy at 60/65 by sex | Eurostat EUROPOP2025 `proj_25nalexp`; 2027 Ageing Report country fiche (e65) | **Yes** (by sex only, not unisex) |
| Replacement rates | 2024 Ageing Report PL fiche, Tab. 9 (2022–2070, 10-year steps) | Validation only |

Main gaps: (a) no official source publishes **future valorisation indices**; (b) no official source publishes a **projected unisex
life-expectancy divisor**. GUS does **not** publish projected e60 or e65. Its 2023–2060 projection gives only e0 by sex.

---

## 1. ZUS: "Prognoza wpływów i wydatków Funduszu Emerytalnego do 2080 roku"

### Latest edition (as of Nov 2026)

- **Edition:** cover dated *Warszawa, październik 2025 r.*; horizon **2026–2080**; ZUS model **FUS23**. The PDF metadata
  gives a creation date of 2025-10-09. The download URL carries a timestamp of 2025-12-22 (`t=1766410217052`),
  which looks like a re-upload. No newer edition is listed on the ZUS forecasts page.
  ZUS must publish this forecast **every three years** (ustawa o systemie ubezpieczeń społecznych, quoted in the "Wstęp").
  The next edition is therefore due around autumn 2028.
- **Listing pages:**
  - https://www.zus.pl/baza-wiedzy/statystyka/prognozy
  - BIP mirror: https://www.zus.pl/web/biuletyn-informacji-publicznej/finanse-zus-i-fus/prognozy
- **PDF (59 pp.):**
  https://www.zus.pl/documents/10182/167761/Publikacja_Fundusz_Emerytalny_2026_2080.pdf/54000d69-b2f3-96cc-5814-9b6524f3d231?t=1766410217052
- **Previous editions:**
  - Oct 2022, 2023–2080:
    https://www.zus.pl/documents/10182/167761/Publikacja_Fundusz_Emerytalny_2023-2080.pdf/3c2c41c9-6a50-0574-4634-ee9cfa43f286?t=1674049287158
  - 2019, 2020–2080:
    https://www.zus.pl/documents/10182/167761/Prognoza_Fundusz_Emerytalny_2020-2080.pdf/61b7e0d7-7e20-f8f8-170c-6f73870383a4?t=1572523659944
- **Companion 5-year forecast:** "Prognoza wpływów i wydatków FUS na lata 2026–2030" (Oct 2025, same model and assumptions):
  https://www.zus.pl/documents/10182/167761/Publikacja_FUS_2026-2030.pdf/f8bee9a5-c9a2-dd9f-5ddb-c52fbabf665a?t=1761633510772

### Scenarios

There are three variants: **nr 1 – pośredni** (intermediate, the baseline), **nr 2 – pesymistyczny** and
**nr 3 – optymistyczny** (p. 4). All three use:

- the **MF macro assumptions of August 2025** (Departament Polityki Makroekonomicznej);
- the **MF demographic projection of 2025**, prepared for the long-term macro assumptions. ZUS does not use the GUS projection.
  Appendix A compares against the MF 2022 demography.

The variants differ in real wage growth, unemployment, real GDP, collection rate, award frequencies of
pensions and disability pensions, and number of insured. **CPI is identical in all three variants.**

### Macro variables published (Tab. 1.1–1.3, pp. 10–11)

Columns are **annual for 2025–2035, then 2040, 2045 … 2080** (5-year steps). Intermediate years must be interpolated.

| Variable (as named in the PDF) | Unit |
|---|---|
| stopa bezrobocia (stan na koniec roku) | % |
| średnioroczny wskaźnik cen towarów i usług konsumpcyjnych ogółem | index, prev. year = 100 |
| średnioroczny wskaźnik cen … dla gospodarstw domowych emerytów i rencistów | index |
| wskaźnik realnego wzrostu przeciętnego wynagrodzenia | index |
| wskaźnik realnego wzrostu PKB | index |
| ściągalność składek | % |

Variant 1 (baseline), transcribed from Tab. 1.1:

| year | 2025 | 2026 | 2027 | 2028 | 2029 | 2030 | 2035 | 2040 | 2045 | 2050 | 2060 | 2070 | 2080 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| unemployment % | 5.07 | 4.97 | 5.06 | 5.18 | 5.25 | 5.30 | 5.81 | 5.96 | 5.96 | 5.96 | 5.96 | 5.96 | 5.96 |
| CPI | 103.72 | 102.99 | 102.64 | 102.49 | 102.42 | 102.50 | 102.50 | 102.50 | 102.50 | 102.50 | 102.50 | 102.50 | 102.50 |
| CPI pensioners | 104.02 | 103.29 | 102.94 | 102.79 | 102.72 | 102.80 | 102.80 | 102.80 | 102.80 | 102.80 | 102.80 | 102.80 | 102.80 |
| real avg wage | 104.38 | 103.27 | 103.53 | 103.62 | 103.55 | 103.43 | 102.83 | 102.23 | 101.91 | 101.77 | 101.58 | 101.38 | 101.38 |
| real GDP | 103.35 | 103.53 | 102.99 | 102.92 | 102.81 | 102.81 | 101.99 | 101.14 | 100.38 | 100.33 | 100.65 | 100.42 | 100.22 |
| collection % | 99.0 | 99.0 | 99.0 | 99.0 | 99.0 | 99.0 | 99.0 | 99.0 | 99.0 | 99.0 | 99.0 | 99.0 | 99.0 |

The real-wage path in the other variants: variant 2 ends at 101.16 in 2080 and variant 3 at 101.73. Collection is 98.0 %
in variant 2 and 99.5 % in variant 3. Unemployment plateaus at 6.41 % (variant 2) and 5.71 % (variant 3).

### What is not in the forecast

- **No valorisation indices** for contribution accounts (art. 25/25a) or sub-accounts (art. 40a).
  The only valorisation the PDF mentions is *pension indexation*: "wskaźniki waloryzacji świadczeń przyjęliśmy na najniższym
  poziomie, który wynika z przepisów (… cen … zwiększonych o 20% realnego wzrostu przeciętnego wynagrodzenia)",
  applied additively (p. 9).
- **No replacement rates.** None appear in either the 2025 or the 2022 edition.
- **No life expectancy or mortality tables.** Mortality comes from the (unpublished) MF 2025 demographic projection.
  Appendix C gives only a ±5 % mortality-rate sensitivity table.
- **No employment level.** The PDF does give the annual number of insured and the number of pensioners (Tab. 17–18,
  annual 2026–2080, per variant).
- Results tables 4–19 are **annual 2026–2080 for each variant**. They cover inflows, outflows and balance (nominal,
  % of contribution bases, 2024 prices, % of GDP), number of insured and pensioners, and the system dependency ratio.
- Appendix B has **sensitivity tables** (±1 pp on inflation, unemployment, real wages, collection, OFE return and
  pension indexation). These are useful for sizing stochastic shocks later.

### Format and reuse

- **PDF only.** No XLSX or CSV annex is linked on either the ZUS statistics page or the BIP page; I grepped both listings
  for `.xls` links. Tables are text-extractable (`pdftotext -layout` works cleanly), and the charts are images.
- **Reuse terms:** ZUS BIP, "Ponowne wykorzystywanie informacji sektora publicznego"
  (https://www.zus.pl/web/biuletyn-informacji-publicznej/obsluga-klienta/ogolne-zasady-zalatwiania-spraw/ponowne-wykorzystywanie-informacji-sektora-publicznego):
  - reuse of information published in ZUS BIP is **free of charge** (*bezpłatne*);
  - the reuser **must state the source**, the date the information was created and the date it was obtained;
  - the reuser must state whether the information is used in whole or in part.

  Practical consequence: ship a provenance record (URL, "październik 2025", retrieval date, "transcribed in part")
  next to any transcribed data.

## 2. Ministry of Finance: long-term macro guidelines (art. 50a ustawy o finansach publicznych)

- **Page:** https://www.gov.pl/web/finanse/wytyczne-sytuacja-makroekonomiczna
- **Latest:** *Aktualizacja – wrzesień 2026*, signed 15.09.2026, page version 28.0 dated 28.09.2026. It follows the
  draft 2027 budget adopted by the Council of Ministers on 28.08.2026.
  The update history shows roughly two to three revisions a year: May 2025, July 2025, Nov 2025, May 2026, Sept 2026.
- **Files:**
  - Guidelines PDF: https://www.gov.pl/attachment/9977c077-5d6e-47ea-bd4c-261dd41eda37
  - **Tab. 1 (XLSX), 2025–2030:** https://www.gov.pl/attachment/3696e628-6941-42cf-bec6-b1d2985b52cf
    - growth indices: GDP and demand components, CPI, PPI, GDP deflator;
    - levels: nominal GDP (mld zł), **average gross wage in the national economy (zł)**, average wage in the enterprise sector (zł);
    - labour market: employment (thousand FTE), **registered** unemployment (count and rate);
    - PLN/EUR exchange rate and policy interest rate.
  - **Tab. 2 (XLSX), 2031–2060, annual:** https://www.gov.pl/attachment/36f14069-bb7f-4be2-9044-c62259613ec0
    - real GDP, CPI (flat 102.5), **real gross wage growth**, registered unemployment rate (plateaus at 6.6 %), PLN/EUR plus a ±15 % band.
- **Selected values:**
  - Average wage: 2026 = 9 473 zł; 2027 = 10 033 zł; 2030 = 11 615 zł.
  - Real wage growth: 101.8 in 2031, peaking at 102.1 in 2041–2043, then easing to 101.6 by 2060.
  - Long-run assumptions stated in the text: CPI at the NBP target of 102.5; real wages follow labour productivity.
- **There is only one scenario** (no pessimistic or optimistic variant), and it **stops at 2060**.
- **Caveats:**
  - Unemployment is **registered** (BAEL/LFS differs). The ZUS table's "stopa bezrobocia" is a separate MF series.
  - The **ZUS forecast used the August 2025 vintage** of MF assumptions. That vintage is *not* the same as these
    September 2026 tables (for example, ZUS's 2040 real wage is 102.23 against 102.0 here). Don't mix vintages within one baseline.
- **Reuse:** gov.pl terms of use (https://www.gov.pl/web/gov/warunki-korzystania) put text content under **CC BY-SA 4.0**.
  The page does not separately license XLSX attachments; I assume the same terms but have not verified this.
- **The MF 2025 demographic projection** that ZUS uses does not appear on this page, and I found no public release of it.

### Contribution cap: the statutory link to MF wages (verified)

- Art. 19 ust. 1 ustawy o sus sets the annual cap at 30 × the *prognozowane przeciętne wynagrodzenie* from the budget
  documents. The minister announces it under art. 19 ust. 10.
- **The 2027 cap is 300 990 zł, based on a projected wage of 10 033 zł** (Obwieszczenie MRPiPS z 3.11.2026, M.P. 2026 poz. 1098:
  https://eli.gov.pl/api/acts/MP/2026/1098/text/I/M20261098.pdf). This is exactly 30 × the MF Tab. 1 value for 2027.
- For the 2026 cap, secondary sources cite 282 600 zł = 30 × 9 420 zł (M.P. 2025 poz. 1206). I did not open that
  announcement. The 9 420 zł figure comes from the 2026 budget vintage; Tab. 1 now shows a revised 9 473 zł.
- **Derivation rule:** `cap_t = 30 × avg_wage_projected_t`. Use the MF level to 2030, then grow it at the chosen real wage growth × CPI.

## 3. Valorisation indices: statutory definitions, so the core derives them

The statute was read from the consolidated text of *ustawa o emeryturach i rentach z FUS*, Dz.U. 2025 poz. 1749
(https://api.sejm.gov.pl/eli/acts/DU/2025/1749/text.pdf).

- **Art. 25 ust. 6–8 (annual account valorisation, every 1 June):**
  - index = CPI(prev. year) × real growth of the **sum of old-age contributions charged** (*suma przypisu składek na
    ubezpieczenie emerytalne*);
  - this equals the nominal growth of that sum, floored at CPI (*nie może być niższy niż wskaźnik cen*);
  - the account balance may not fall (ust. 3); the index is set to 0.01 %.
- **Art. 25a (quarterly valorisation at award):** the same construction on quarter-on-quarter CPI and quarterly contribution sums.
  Which quarter applies depends on the award month (ust. 2).
- **Art. 26 (benefit formula):**
  - pension = (valorised contributions + valorised initial capital + sub-account) ÷ **średnie dalsze trwanie życia**;
  - the divisor is **common to men and women, in months** (ust. 3);
  - GUS announces the tables by **31 March** each year, and they apply to claims from 1 April to 31 March (ust. 4–5);
  - ust. 6 lets the claimant use the table in force when they reached 60 (women) or 65 (men), if that is more favourable.
- **Sub-account (art. 40a ustawy o sus):** valorised by the 5-year average of nominal GDP growth (as summarised in
  the 2024 AR PL fiche, p. 8). I did not open art. 40a itself.

How to derive each one from the published paths:

1. **Annual account index (preferred route):**
   - The ZUS FE forecast publishes annual *wpływy* in mln zł (Tab. 5) and *wpływy as % of the sum of annual
     contribution bases* (Tab. 8). Both include the "suwak" inflows, so they are consistent with each other.
   - So `sum_of_bases_t = wpływy_t / wpływy_pct_t`, and `valorisation_index_{t+1} ≈ max(CPI_t, sum_of_bases_t / sum_of_bases_{t-1})`.
   - The sum of bases is proportional to *przypis* at a constant contribution rate.
   - Rounding of the % to two decimals adds roughly ±0.05 pp of noise.
   - Example, variant 1: 2026 gives 258 501 / 0.1954 ≈ 1 322 900; 2027 gives 275 406 / 0.1958 ≈ 1 406 600. That is ≈ +6.3 %
     nominal in 2027, against CPI 2.64 % and real wages 3.53 %.
2. **Annual account index (simpler route):** `(1+g_real_wage) × (1+CPI) × (insured_t / insured_{t-1})`, using ZUS Tab. 17
   for the number insured. Collection is flat in every variant, so it cancels out.
3. **Quarterly index:** needs a convention, for example splitting the annual index geometrically.
4. **Sub-account:** a 5-year rolling mean of `(1+g_real_GDP)(1+deflator)`.
   - ZUS gives real GDP but no deflator.
   - MF gives the deflator only to 2030.
   - Beyond 2030, use CPI as the deflator proxy (MF and AR both assume 2.5 % inflation).
5. **Pension indexation:** `CPI_pensioners + 0.2 × real_wage_growth` (additive, as in ZUS p. 9). Only needed if the core
   models the post-retirement path.

## 4. Mortality and life expectancy

### GUS "Prognoza ludności na lata 2023–2060" (published 31.08.2023; still the current official projection)

- **Page:** https://stat.gov.pl/obszary-tematyczne/ludnosc/prognoza-ludnosci/prognoza-ludnosci-na-lata-2023-2060,11,1.html
  - PDF: https://stat.gov.pl/download/gfx/portalinformacyjny/pl/defaultaktualnosci/5469/11/1/1/1-prognoza_ludnosci_na_lata_2023-2060_.pdf
  - Results XLSX: https://stat.gov.pl/download/gfx/portalinformacyjny/pl/defaultaktualnosci/5469/11/1/1/2-wyniki_polska.xlsx
- **Variants:** three for each component (low, medium, high). Scenarios are main/medium, low and high, and there are
  extra alternative-scenario files.
- **Life expectancy is published only as e0 by sex** (PDF Tab. 3, p. 24):

  | | 2030 | 2040 | 2050 | 2060 |
  |---|---|---|---|---|
  | men, medium | 75.16 | 76.35 | 77.55 | 78.63 |
  | women, medium | 82.49 | 83.47 | 84.34 | 85.14 |

  Low and high variants are also given.
- **Mortality method:** qx come from UN model life tables (p. 24). They appear **only as a chart** (Wykres 6), not as a table.
- **Results XLSX** contents: population by single age and sex, population by age groups, and vital statistics
  (births, deaths, migration). **It has no e60/e65 and no qx.**
- So **GUS does not publish projected life expectancy at 60/65.**
- A later GUS experimental simulation (lower fertility, high life-expectancy variant) is not an official projection:
  https://stat.gov.pl/obszary-tematyczne/ludnosc/prognoza-ludnosci/symulacja-liczby-ludnosci-do-2060-r-opracowanie-eksperymentalne,17,1.html
- **Current divisor tables:** GUS's annual *tablice trwania życia* (art. 26 ust. 4) are historical/period tables for
  the current year only, not projections.
- **GUS reuse terms** (https://stat.gov.pl/copyright/): copying and use in your own work is allowed **on condition
  that the source is given** ("pod warunkiem, że będzie podane ich źródło").

### Eurostat EUROPOP2025: the best official projected-mortality source

These are the demographic inputs to the 2027 Ageing Report. EUROPOP2023, the input to the 2024 Ageing Report, is
still online as `proj_23*`. All datasets are annual to 2100 and by single age and sex, with a baseline plus a
lower-mortality sensitivity variant.

| Dataset | Content | API (JSON-stat) |
|---|---|---|
| `proj_25nalexp` | projected life expectancy by age and sex (**M/F only, no total**) | https://ec.europa.eu/eurostat/api/dissemination/statistics/1.0/data/proj_25nalexp?geo=PL&projection=BSL&age=Y65 |
| `proj_25naasmr` | projected **mortality rates** by single age and sex | https://ec.europa.eu/eurostat/api/dissemination/statistics/1.0/data/proj_25naasmr?geo=PL&projection=BSL |
| `proj_23np` / EUROPOP2025 equivalent | projected population by age and sex | (needed only for unisex weighting) |

`proj_25nalexp` and `proj_25naasmr` were last updated on 2026-06-11.

PL baseline life expectancy (`proj_25nalexp`, BSL), in years:

| | 2025 | 2030 | 2040 | 2050 | 2060 | 2070 | 2080 |
|---|---|---|---|---|---|---|---|
| men e60 | 19.9 | 20.4 | 21.5 | 22.7 | 24.0 | 25.2 | 26.6 |
| women e60 | 24.7 | 25.2 | 26.1 | 27.2 | 28.2 | 29.3 | 30.4 |
| men e65 | 16.4 | 16.9 | 17.9 | 19.0 | 20.1 | 21.3 | 22.5 |
| women e65 | 20.5 | 20.9 | 21.9 | 22.8 | 23.8 | 24.8 | 25.9 |

The e65 values match the 2027 AR country fiche below. EUROPOP2023 was noticeably more optimistic: e65 in 2060 was
21.2 for men and 25.0 for women.

**Reuse:** Eurostat data may be reused free of charge with attribution, under Commission Decision 2011/833/EU
(CC BY 4.0). The same licence statement is printed in the AR 2027 report.

**Deriving the ZUS divisor:**

1. Build a **unisex period life table** for the retirement year from `proj_25naasmr`:
   - take male and female mx at each age;
   - combine them, weighted by projected population at that age (or derive a combined qx);
   - compute e(x) at the exact retirement age in years and months (art. 26 ust. 2), and multiply by 12.
2. **Calibrate** to the latest actual GUS divisor table, for example by applying a level offset or ratio at
   60 and 65 in the base year. GUS's methodology for the unisex table is not reproduced exactly by this construction.
3. This is a modelling choice, not an official number. Document it as an assumption.

## 5. EU Ageing Report (EC DG ECFIN / EPC Ageing Working Group)

### 2027 Ageing Report, assumptions volume (published 4 Nov 2026)

- **Report:** "2027 Ageing Report. Underlying Assumptions & Projection Methodologies", Institutional Paper 347, 186 pp.
  - Page: https://economy-finance.ec.europa.eu/publications/2027-ageing-report-underlying-assumptions-projection-methodologies_en
  - PDF: https://economy-finance.ec.europa.eu/document/download/65373d1f-3667-4326-ae65-644f740fd67d_en?filename=ip347_en.pdf
  - **Country fiches XLSX** (one sheet per country, sheet `PL`, **annual 2025–2080**):
    https://economy-finance.ec.europa.eu/document/download/856f1862-1355-4868-83d1-3c9ac5f1f7b1_en?filename=2027%20Ageing%20Report%20-%20Country%20fiches.xlsx
- **Contents of the PL sheet:**
  - demography (EUROPOP2025): fertility, e0 and e65 by sex, net migration, population;
  - macro: potential GDP growth, hours worked, labour productivity per hour, TFP, capital deepening, potential GDP per
    capita and per worker, **HICP**, nominal interest rate;
  - labour: participation, employment and unemployment rates by age and sex, and average labour-market exit age;
  - dependency ratios.
- **Selected PL values:**

  | | 2025 | 2030 | 2040 | 2050 | 2060 | 2070 | 2080 |
  |---|---|---|---|---|---|---|---|
  | potential GDP growth % | 3.21 | 2.30 | 2.29 | 1.06 | 1.00 | 0.89 | 0.59 |
  | potential GDP per worker growth % | 4.75 | 3.02 | 2.94 | 2.50 | 1.84 | 1.56 | 1.46 |
  | unemployment (20–64) % | 3.05 | 2.89 | 3.61 | 2.86 | 3.19 | 3.31 | 3.13 |
  | employment (20–64), millions | 17.32 | 17.19 | 16.18 | 14.33 | 12.80 | 12.04 | 11.06 |

  HICP is 2.5 % from 2030 onward.
- **No wage series is published.** In the AR the average wage = GDP × labour share × (wages/compensation) ÷ employment,
  with constant shares (IP347, pp. 76–77). Real wage growth is therefore ≈ **GDP-per-worker growth**, which needs
  derivation. Unemployment is LFS-based (15–74 / 20–64), not the registered or ZUS definition.
- The **budgetary projections** (pension spending, benefit ratios, replacement rates, an updated PL pension fiche) are
  scheduled for **spring 2027** and **are not yet available**.
- **Reuse:** CC BY 4.0 (IP347 imprint).

### 2024 Ageing Report (latest complete edition with pension results)

- **Page:** https://economy-finance.ec.europa.eu/publications/2024-ageing-report-economic-and-budgetary-projections-eu-member-states-2022-2070_en
- **Files:**
  - Main report (IP279): https://economy-finance.ec.europa.eu/document/download/971dd209-41c2-425d-94f8-e3c3c3459af9_en?filename=ip279_en.pdf
  - **Poland pension fiche** (MF + NBP, dated 01.12.2023, 43 pp.):
    https://economy-finance.ec.europa.eu/document/download/568baf8e-3a50-4859-8507-f0d963411213_en?filename=2024-ageing-report-country-fiche-Poland.pdf
  - Statistical annex XLSX, all country fiches:
    https://economy-finance.ec.europa.eu/document/download/e248db46-f876-4e72-8821-efae678e81ea_en?filename=2024_Ageing_Report-Statistical_annex_all_countryfiches.xlsx
- **Key PL outputs, usable as validation targets** (fiche Tab. 9):
  - old-age earnings-related **replacement rate at retirement**: 58 % (2022), 47 % (2030), 32 % (2040), 27 % (2050–2070);
  - benefit ratio: 47 % falling to 26 %.
  - The fiche attributes the fall to the NDC formula with rising life expectancy and a constant retirement age (p. 21).
  - Tab. A1 gives the economy-wide average wage at retirement, in thousand EUR.
- Horizon is 2022–2070. Demography is EUROPOP2023. The legal state is as of 30.06.2023.
- The fiche describes the PL NDC rules (valorisation, divisor, indexation), consistent with section 3 above.

## 6. OECD Pensions at a Glance 2025 (Nov 2025)

- **Pages:**
  - https://www.oecd.org/en/publications/pensions-at-a-glance-2025_e40274c1-en.html
  - Methodology: https://www.oecd.org/en/publications/2025/11/pensions-at-a-glance-2025_76510fe4/full-report/methodology-and-assumptions_21329d69.html
- The OECD site returned **HTTP 403** to automated fetches. The assumptions below come from OECD-domain search excerpts,
  not from a full read of the page:
  - **Common across all countries:** real earnings growth 1.25 %/yr; inflation 2 %; real discount rate 1.5 %; net real
    DC return 2.5 %.
  - **Model worker:** single worker entering at age 22 in 2024, full career.
  - **Mortality:** UN population database projections for 2024–2100, cohort-based. For NDC schemes (Poland included)
    the annuity factor uses the country's own mortality estimates.
  - **Sensitivity scenario:** real wages 0.75 %, discount rate 1 %, inflation 1 %.
- **Not Poland-specific.** These are stylised assumptions that are useful only for cross-checking a replacement-rate
  output. They are not a baseline path.

## 7. What to use for the deterministic baseline

| Variable | Recommended baseline | Status |
|---|---|---|
| CPI | ZUS FE 2026–2080 variant 1, Tab. 1.1 (interpolate between 5-year points; flat 102.5 after 2030 anyway) | direct |
| Real wage growth | ZUS FE variant 1, Tab. 1.1 (linear interpolation 2035→2040→…) | direct + interpolation |
| Unemployment, real GDP, collection | ZUS FE variant 1 | direct |
| Number insured / employment growth | ZUS FE Tab. 17 (annual) | direct |
| Average wage level (zł) | MF Tab. 1 to 2030, then grown at the ZUS real wage × CPI | derive |
| Contribution cap | 30 × projected average wage | derive (statutory) |
| Account valorisation (annual) | from ZUS sum of bases (Tab. 5 ÷ Tab. 8), floored at CPI | derive |
| Quarterly valorisation | convention, from the annual index | derive |
| Sub-account valorisation | 5-year mean of nominal GDP growth (ZUS real GDP × CPI proxy) | derive |
| Divisor | unisex e(x) × 12 from EUROPOP2025 mortality, calibrated to the latest GUS table | derive (assumption) |
| Pessimistic / optimistic | ZUS variants 2 and 3 (the only official alternative macro paths); EUROPOP `LMRT` for mortality | direct |

Reasons for choosing ZUS variant 1 as the macro anchor:

- it is the only official source that reaches **2080 with three variants**;
- it is internally consistent with the FUS actuarial model;
- it is itself built on MF assumptions.

Re-anchor when ZUS publishes its next edition (due around 2028). The MF guidelines refresh more often but stop at 2060
and have only one scenario.

## Open points / not verified

- The OECD methodology page could not be fetched directly (403). Its values come from search excerpts of oecd.org pages.
- I did not open art. 40a ustawy o sus. The sub-account rule is taken from the 2024 AR PL fiche.
- I did not open the 2026 contribution-cap announcement (M.P. 2025 poz. 1206). Its 282 600 zł / 9 420 zł comes from
  secondary sources.
- The gov.pl licence (CC BY-SA 4.0) is stated for "treści tekstowe". It is not explicit for XLSX attachments.
- The MF 2025 demographic projection used by ZUS was not found published.
- GUS projection files:
  - **Checked, no life-expectancy rows:** the PDF, the main results XLSX and `5-tabela_zbiorcza.xlsx`.
  - **Only listed, not opened:** `6-scenariusze_alternatywne.zip`. Its file list shows `Data table.xlsx` plus
    per-powiat population workbooks for each scenario.
