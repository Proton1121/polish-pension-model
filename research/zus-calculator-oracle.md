# Can the ZUS pension calculator serve as a test oracle, and what other oracles exist?

Research for issue #4. Researched on 2026-11-09. Every claim below links to the primary source it comes from, or is marked **[observed]** (seen directly in an experiment against the live site, described in the appendix) or **[inference]** (my reading, not stated by any source).

## TL;DR

- **The public ZUS calculator can be scripted, but only partly works as an oracle.** It is a server-side form at a stable URL. A plain HTTP POST with no session or CSRF token returns an HTML page with results in grosze, and the same input gives the same output **[observed]**. There is no API and no published terms of use for automated access. The macroeconomic assumptions it uses are not published.
- **Short horizons can be checked exactly.** Take the advanced version, choose retirement early next year, and enter zero future wages. The result then depends only on published inputs: the current GUS table divisor (222.7 months at 65y0m) and the published quarterly valorization indices (110.50% × 101.00%). It matched both to the grosz **[observed]**.
- **Long horizons can only be checked within a tolerance.** Future wage growth, valorization and inflation come from undocumented forecasts that ZUS changes during the year ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura/obliczenia-w-kalkulatorze-emerytalnym)). A result is only meaningful together with the date it was computed.
- **The PUE/eZUS calculator** runs on the same engine, fills in the person's own account data, and since 2023 can simulate retirement in the current year. It needs a login, so it can only be used by hand, with the account owner's consent.
- **No newer ZUS simulator exists.** The HackYeah 2025 winners were "to be evaluated" for ZUS's educational work. I found no evidence that ZUS adopted any of them. The related GitHub repositories are hackathon code and not trustworthy as oracles.
- **The strongest exact oracle is the hypothetical pension in the annual "Informacja o stanie konta".** Its formula is fully set out in statute (art. 50 ust. 2 and 2a of the social insurance system act).
- **Recommended strategy:** golden tests derived by hand from statute and data published in Monitor Polski (exact), plus account statement and award-decision fixtures (exact), plus captured ZUS calculator probes (exact for short horizons, a tolerance band for long ones). Do not call zus.pl from CI.

## 1. ZUS calculators: what exists

| Calculator | Access | Data source | Horizon |
|---|---|---|---|
| zus.pl "Kalkulator emerytalny – prognozowana emerytura", simplified and advanced versions | Public, anonymous | User types in values from their "Informacja o stanie konta" | **Next year onward**; not the current year ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura)) |
| Same calculator in PUE/eZUS, mZUS, and at ZUS advisors (branch, e-wizyta, call centre) | Login needed | Pre-filled from the person's own account | Since March 2023 also **the current year** ([ZUS, 8 Mar 2023](https://www.zus.pl/-/na-pue-zus-mo%C5%BCna-samodzielnie-wyliczy%C4%87-swoje-przysz%C5%82e-%C5%9Bwiadczenie-emerytalne-gdy-planuje-si%C4%99-przej%C5%9B%C4%87-na-emerytur%C4%99-w-bie%C5%BC%C4%85cym-roku)) |
| HackYeah 2025 "Symulator emerytalny" challenge | Not deployed | — | — |

ZUS describes the zus.pl, PUE and advisor calculators as one tool: "Kalkulator emerytalny dostępny u doradców w placówkach ZUS, na Platformie Usług Elektronicznych (PUE) ZUS oraz na stronie www.zus.pl" ([ZUS, Obliczenia w kalkulatorze](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura/obliczenia-w-kalkulatorze-emerytalnym)).

**HackYeah 2025.** ZUS ran a "symulator emerytalny" challenge with about 60 teams. The winners were No Signal ("Emerytalna przyszłość"), Wojanki ("PSE") and Prawe Buty ("Symulator emerytalny"). ZUS said only: "Teraz będziemy oceniać zwycięskie projekty pod kątem możliwości włączenia ich do planu działań edukacyjnych" ([ZUS, 6 Oct 2025](https://www.zus.pl/-/zus-po-raz-pierwszy-na-hackyeah-z-projektami-edukacyjnymi)). The goal was education, not computing exact benefits. A search of zus.pl found no later announcement of an adopted simulator.

## 2. The public calculator in detail

### Inputs **[observed]**, from the form served on 2026-11-09

Fields of the simplified version:
- `lastInfoYear` (2023–2025): year of the last "Informacja o stanie konta"
- `sex`, birth `month` and `year`
- `valorizedContrib`: valorized contributions, excluding OFE and the sub-account
- `valorizedCapital`: valorized initial capital
- `subValorizedCapital`: sub-account balance
- `oneYearContrib`: contributions over the last 12 months
- `retirementAge` (60–90) and `retirementAgeMonth` (0–11)
- `startOfJobYear`
- `currentSalary`
- OFE membership and contribution flags, OFE balance and date
- `futureSalary`: future wage as a % of the average wage, held constant

The advanced version replaces `futureSalary` with a per-year `futureSalaryMap[YYYY]`. A 0 in a year excludes that year from coverage ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura)). Wages are capped at 250% of the average wage, as the result page states **[observed]**.

**Coverage gap [observed]:** the birth-year dropdown only offers **1949–1999**. Anyone born in 2000 or later cannot use the public calculator.

### Outputs **[observed]**, in grosze

- Account plus sub-account at retirement, split into valorized contributions, valorized initial capital and sub-account.
- Forecast pension, nominal and gross, **not** raised to the minimum pension.
- The same pension as a % of the average wage in the last year of work.
- The same pension discounted by inflation to the current year.
- An echo of every parameter used.

### Assumptions

- **Formula:** basis (valorized contributions + valorized initial capital + sub-account) ÷ average life expectancy at the retirement age, following art. 24–26 of the FUS act ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura); [art. 25–26, Dz.U. 2025 poz. 1749](https://api.sejm.gov.pl/eli/acts/DU/2025/1749/text.pdf)).
- **Life tables:** "Kalkulator korzysta z tablicy średniego dalszego trwania życia … z komunikatu Prezesa GUS ogłaszanego pod koniec marca każdego roku" ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura)).
  - **[observed]** It used the 2026 table value for 65y0m (222.7, [M.P. 2026 poz. 319](https://eli.gov.pl/eli/MP/2026/319/ogl/pol/pdf)) for a retirement in Feb 2027 **and** for one in Jan 2045. It therefore does not appear to project mortality improvement for distant retirements.
  - Separately, ZUS says forecast pensions can differ between months of the same year because of "szacunki dotyczące tego, jak wydłuża się średnie dalsze trwanie życia" ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura/obliczenia-w-kalkulatorze-emerytalnym)). This needs more probes.
- **Macroeconomic assumptions:** "przyjętych założeń makroekonomicznych", including forecasts of "inflacji, realnego wzrostu wynagrodzeń, wskaźników waloryzacji składek i kapitału początkowego". The calculator also applies **quarterly valorization** ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura)). **The actual values are not published.**
  - It is plausible that they come from ZUS's long-term FUS forecast ([ZUS, prognoza do 2080](https://www.zus.pl/-/zus-prognoza-wp%C5%82yw%C3%B3w-i-wydatk%C3%B3w-funduszu-emerytalnego-do-2080-r.)), but ZUS does not say so **[inference]**.
- **The assumptions change during the year:** "Kalkulator jest zasilany danymi co roku. Najpierw wprowadza się do niego dane prognozowane, a w dalszej części roku są one zastępowane danymi faktycznymi … Moment, w którym wykonujesz obliczenie … wpływa na prognozowaną wysokość" ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura/obliczenia-w-kalkulatorze-emerytalnym)).
- **It is not the account statement's hypothetical pension:** "Ze względu na różne sposoby obliczania, prognozowana wysokość emerytury … różni się od wysokości hipotetycznej emerytury, podawanej w «Informacji o stanie konta»" ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura)).
- **Accuracy has been challenged in Parliament.** A 2018 interpellation claimed the calculator "nie podaje prawidłowych wyliczeń", for example because it forced retirement ages of 61/66 ([podatki.biz, secondary, 2018](https://www.podatki.biz/artykuly/rozne-kalkulatory-emerytalne-zus-rozne-wyniki_33_39194.htm)). That is old and secondary, but it is a reminder that the calculator is not infallible.

### Repeatability and terms

- **Mechanism [observed].** The calculator is a Liferay portlet (`zuspensioncalculatorportlet`). The form posts to its own page URL with `p_p_id=zuspensionnewportlet_WAR_zuspensioncalculatorportlet_INSTANCE_<id>&p_p_lifecycle=0&..._action=save`.
  - The form takes plain field names. Numbers use a decimal comma.
  - No cookie, session or `p_auth` token is needed.
  - The page JavaScript (`/o/zus-pension-calculator-portlet/js/newRulesCalculator.js`) only handles the UI. **All computation happens on the server.**
- **Determinism [observed].** The same POST gave identical results with and without a prior session.
  - Across days it will **not** be stable, because the assumptions are updated (see above).
- **Fragility [inference].** The portlet instance IDs (`iTexi6uGKrps` for simplified, `7XACN8Fz4kD1` for advanced) and the HTML layout can change without notice. The advanced form template still contains placeholder years 2013–2015, which the JS rewrites. Scraping is possible but brittle.
- **Terms.** `robots.txt` on www.zus.pl is `User-Agent: * / Disallow:` (empty, so nothing is disallowed) ([zus.pl/robots.txt](https://www.zus.pl/robots.txt)). I found no terms of use covering automated use of the calculator or reuse of its results. The only stated condition is that the result "nie może być podstawą do jakichkolwiek roszczeń" ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/kalkulator-emerytalny-prognozowana-emerytura)).
  - Saving a handful of numeric results for synthetic inputs as test fixtures carries little risk **[inference, not legal advice]**.
  - Bulk or frequent automated querying is neither covered nor invited, so keep captures rare, slow and triggered by hand.
- **PUE/eZUS** needs authentication and shows personal data. It cannot be used in automation. Use it only by hand, by consenting account owners.

## 3. Other independent oracles

Ranked by how much each can be trusted for **exact** checks of the deterministic core.

1. **The statute plus official data: exact and primary.**
   - Formula and valorization: art. 25, 25a (quarterly) and 26 (divisor, years+months, unisex table, applicability window 1 Apr–31 Mar, the art. 26 ust. 6 more-favourable-table rule) ([Dz.U. 2025 poz. 1749](https://api.sejm.gov.pl/eli/acts/DU/2025/1749/text.pdf)).
   - Life tables: GUS communiqués in Monitor Polski, for example [M.P. 2026 poz. 319](https://eli.gov.pl/eli/MP/2026/319/ogl/pol/pdf). It gives 222.7 months at 65y0m and 214.1 at 66y0m.
   - Annual indices: [ZUS table](https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/wskazniki/waloryzacja-skladki-i-kapitalu-poczatkowego/wskazniki-waloryzacji-rocznej), for example 109.81% for 2025 (M.P. 2026 poz. 506).
   - Quarterly indices: [ZUS table](https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/wskazniki/waloryzacja-skladki-i-kapitalu-poczatkowego/wskazniki-waloryzacji-kwartalnej), for example Q1 2026 110.50% ([M.P. 2026 poz. 625](https://api.sejm.gov.pl/eli/acts/MP/2026/625/text.pdf)) and Q2 2026 101.00%.
   - Sub-account index: 110.61 ([M.P. 2026 poz. 487](https://eli.gov.pl/eli/MP/2026/487/ogl/pol/pdf)).
   - Worked examples built from these are the project's primary oracle, as the issue already intends.
2. **The hypothetical pension in the "Informacja o stanie konta" (IOSK): exact, formula in statute.**
   - Art. 50 of the social insurance system act ([Dz.U. 2026 poz. 199](https://api.sejm.gov.pl/eli/acts/DU/2026/199/text.pdf)) requires it for insured people aged 35+ (ust. 1ba).
   - It is computed in two ways (ust. 1c). (1) The current account balance alone. (2) The balance plus hypothetical future monthly contributions equal to the person's average historical monthly contribution, nominal and not valorized.
   - Both are divided by the life expectancy at 60/65, or at the person's age plus up to five years (ust. 1d–1e). The life table used is the one in force on 31 Dec of the statement year (ust. 2a).
   - ZUS also shows variants with and without the sub-account and does **not** raise the result to the minimum pension ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/wyjasnienie-danych-zawartych-w-informacji-o-stanie-konta/wysokosc-hipotetycznej-emerytury)).
   - ZUS gives a published worked number: for the 2016 statement, age 66 → 207.8 months ([ZUS](https://www.zus.pl/swiadczenia/emerytury/kalkulatory-emerytalne/emerytura-na-nowych-zasadach/wyjasnienie-danych-zawartych-w-informacji-o-stanie-konta/pogladowa-informacja-o-wysokosci-przyszlej-emerytury-z-i-filaru)). This can be checked against the 2016 GUS table.
   - Real statements, the maintainer's own or volunteers' anonymized ones, give account → printed amount pairs with no forecast assumptions at all.
   - Open point: the rounding rule to grosze is not stated in the act.
3. **ZUS award decisions ("decyzja o przyznaniu emerytury"): exact real cases.** They are the authoritative outcome for a real person. They cover the real valorization chain, including quarterly valorization, and the table that applied. **[Not verified in this research]**: I did not get a primary source showing how much of the calculation a decision prints. Collecting them needs volunteers and anonymization.
4. **The PUE/eZUS calculator in current-year mode: nearly exact, by hand.** Few forecast inputs are involved when retirement is in the current year ([ZUS 2023](https://www.zus.pl/-/na-pue-zus-mo%C5%BCna-samodzielnie-wyliczy%C4%87-swoje-przysz%C5%82e-%C5%9Bwiadczenie-emerytalne-gdy-planuje-si%C4%99-przej%C5%9B%C4%87-na-emerytur%C4%99-w-bie%C5%BC%C4%85cym-roku)). Login only, own data.
5. **The public ZUS calculator.** See sections 2 and 4. Exact for short-horizon probes, a tolerance band for long horizons.
6. **Coarse, documented-assumption benchmarks.** OECD *Pensions at a Glance* replacement rates for Poland ([OECD 2025 country note](https://www.oecd.org/en/publications/pensions-at-a-glance-2025-country-notes_8a53ef12-en/poland_33765ee7-en.html)) and the EU *Pension Adequacy Report* theoretical replacement rates ([EC](https://employment-social-affairs.ec.europa.eu/pension-adequacy-report-current-and-future-income-adequacy-old-age-eu_en)). They suit a later sanity check of the stochastic layer, roughly matching replacement rates under their stated wage and career assumptions. They are not precise enough for the deterministic core.
7. **Secondary press and blogs** (infor.pl, freenance.io and others) repeat official figures. They are useful to find sources, not to serve as oracles.
8. **Open-source calculators: not trustworthy as oracles.** A GitHub search on 2026-11-09 found only zero- or one-star repositories with no tests against official data. Examples:
   - HackYeah 2025 entries: [Konex25/symulator-emerytalny](https://github.com/Konex25/symulator-emerytalny) (describes itself as the winner), [kguzek/emerytownik](https://github.com/kguzek/emerytownik), [radekrzepka/hack-yeah-2025](https://github.com/radekrzepka/hack-yeah-2025), [egorus1/RetirementSimulator-HackYeah2025-Memento](https://github.com/egorus1/RetirementSimulator-HackYeah2025-Memento)
   - Small personal projects: [sergrog/polish-pension-calc](https://github.com/sergrog/polish-pension-calc), [kermit610/emerytura](https://github.com/kermit610/emerytura) (2019)
   - I did not audit their code. At most they show how others read the rules. They do not verify anything.
9. **Not found in this pass:** actuarial textbook or exam problems specific to the ZUS formula, and published academic replications of individual ZUS benefits. Mark these as a gap, not as evidence that none exist.

## 4. Recommended oracle strategy

**Tier A: exact, runs in CI, no network.**
- Hand-derived golden examples from statute and published indices and tables, with each input traced to an M.P. or Dz.U. citation.
- Life-table lookup tests against the M.P. PDFs: years + months, unisex, the 1 Apr–31 Mar window, the art. 26 ust. 6 more-favourable rule.
- The IOSK hypothetical pension formula, including ZUS's "66 y, 2016 table → 207.8" figure.
- Tolerance: 0.00 zł after a documented rounding step.

**Tier B: exact, recorded fixtures from the ZUS calculator ("short-horizon probes").**
- Use the advanced version. Set retirement in the next calendar year, all future wages 0, and sub-account and OFE 0.
- The result should then equal inputs × published valorization ÷ current GUS divisor. Experiment 2 matched this to the grosz.
- Vary the age and month (60y0m … 70y11m) to exercise the divisor lookup. Vary the retirement month across quarters to exercise the art. 25a quarterly logic.
- A small script, run by hand, captures the raw HTML plus the inputs and the capture date into fixture files. Tests read the fixtures offline.
- Re-capture after each GUS table (1 April), annual index (late May/June) and quarterly index.
- An old fixture stays valid for the parameters in force on its date, so pin each fixture to those.

**Tier C: tolerance, ZUS calculator for long horizons.**
- For multi-decade scenarios, first back out ZUS's hidden assumptions. Difference zero-wage probes that retire in year N and N+1 to get the implied annual factor, and use sub-account-only probes for the sub-account path **[inference: technique not yet tried beyond one probe]**.
- Feed those implied paths into the model's core and compare. The residual should then be small.
- Without that step, only a wide band is defensible, because ZUS's forecasts are unknown and change during the year. Document the tolerance per scenario, for example ±1% after backing out and ±10–20% without.
- Never fail CI on drift. Flag it for review.

**Tier D: manual, opportunistic.** Anonymized IOSK statements, award decisions and PUE current-year screenshots from consenting people, stored as fixtures with personal data removed.

### Limits
- The ZUS calculator is a black box with undocumented, time-varying assumptions. It checks results, not reasons.
- It does not handle birth years ≥ 2000, the minimum-pension top-up, or old-system pensions. The public version also does not cover current-year retirement.
- No API and no terms of use: captures may break whenever zus.pl changes, and permission for scripted use is unclear. Keep captures by hand and rare.
- Grosze rounding rules (the order of rounding in valorization chains) are not stated in the statute. Expect cent-level differences until a ZUS decision or statement pins them down.
- How exactly art. 25a quarterly valorization interacts with the IOSK balance date is left to the valorization ticket. The calculator's behaviour is evidence, not authority.

## Appendix: experiments run against zus.pl on 2026-11-09

Three POSTs in total, plain `curl`, no session needed.

**Experiment 1, simplified version.**
- Inputs: male, born 01/1980, statement for 2025, valorized contributions 300 000,00, capital 0, sub-account 80 000,00, 12-month contributions 18 000,00, retirement at 65y0m (01/2045), start 2003, salary 7 500,00, future wage 100%.
- Results: account at retirement 2 061 670,33 (contributions 1 482 613,47, sub-account 579 056,86). Pension **9 257,61**, 38,32% of the average wage, 5 932,78 in 2026 money.
- Implied divisor: 2 061 670,33 / 9 257,61 = **222.700**, the 2026 GUS value for 65y0m.
- A second identical POST returned identical numbers.

**Experiment 2, advanced version.**
- Inputs: male, born 02/1962, statement for 2025, valorized contributions 400 000,00, capital 200 000,00, sub-account 0, 12-month contributions 0, retirement at 65y0m (**02/2027**), future wages 2026 and 2027 = 0%.
- Results: contributions 446 420,00, capital 223 210,00, total 669 630,00, pension **3 006,87**, 3 000,35 in 2026 money.
- Implied factor: 446 420 / 400 000 = 223 210 / 200 000 = **1.11605** = 1.1050 × 1.0100, the product of the published Q1 2026 and Q2 2026 quarterly indices.
- Implied divisor: 669 630 / 3 006,87 = **222.700**, the M.P. 2026 poz. 319 value for 65y0m.
- Art. 25a ust. 2 pkt 1 says that for a Q1 award the last quarterly valorization is for Q3 of the previous year ([Dz.U. 2025 poz. 1749](https://api.sejm.gov.pl/eli/acts/DU/2025/1749/text.pdf)). The Q3 2026 index was not yet on ZUS's list, so the calculator either assumed 100% or left it out. I cannot tell which.
