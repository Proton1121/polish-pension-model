# ZUS old-age pension (NDC) — what current law prescribes

Research for issue #2, "What does current law say about calculating a ZUS old-age pension?".
Scope: persons born **after 31 Dec 1948** (the "new rules", defined-contribution / NDC formula).
Legal state checked: **9 November 2026**.

## Sources and how they were read

Primary sources only. The texts were downloaded from the Sejm ELI API (`api.sejm.gov.pl/eli`, the ISAP back-end) and read directly.

| Short name | Act | Consolidated text (tekst jednolity) used | Amendments after that text, checked separately |
|---|---|---|---|
| **FUS act** | Ustawa z 17 grudnia 1998 r. o emeryturach i rentach z Funduszu Ubezpieczeń Społecznych | **Dz.U. 2025 poz. 1749** (legal state 22 Oct 2025), [ELI](https://eli.gov.pl/eli/DU/2025/1749/ogl), [ISAP](https://isap.sejm.gov.pl/isap.nsf/DocDetails.xsp?id=WDU20250001749) | Dz.U. 2026 poz. 26 (in force 27 Jan / 13 Apr 2026 / 1 Oct 2026 / 1 Jan 2027, by provision); Dz.U. 2026 poz. 425 (in force 14 Apr 2026) |
| **SUS act** | Ustawa z 13 października 1998 r. o systemie ubezpieczeń społecznych | **Dz.U. 2026 poz. 199** (announced 9 Feb 2026), [ELI](https://eli.gov.pl/eli/DU/2026/199/ogl), [ISAP](https://isap.sejm.gov.pl/isap.nsf/DocDetails.xsp?id=WDU20260000199) | Dz.U. 2026 poz. 252, 426, 473, 507, 734: none of them touches art. 16, 19, 22 or 40–40e |
| **OFE act** | Ustawa z 28 sierpnia 1997 r. o organizacji i funkcjonowaniu funduszy emerytalnych | **Dz.U. 2026 poz. 1070** (legal state 21 Jul 2026), [ELI](https://eli.gov.pl/eli/DU/2026/1070/ogl) | — |

Annual parameters come from the announcements in Monitor Polski (M.P.) cited inline. ZUS web pages were used only to confirm how ZUS applies the law, and are marked as such.

"Art. X FUS" means the FUS act in the Dz.U. 2025 poz. 1749 text. "Art. X SUS" means the SUS act in the Dz.U. 2026 poz. 199 text.

---

## 1. The pension formula

**Art. 26 ust. 1 FUS:** the pension equals the **calculation base** (*podstawa obliczenia*, art. 25) divided by the **average further life expectancy** (*średnie dalsze trwanie życia*) for persons aged the same as the insured on the day they retire.

```
emerytura = podstawa_obliczenia / średnie_dalsze_trwanie_życia[wiek w latach i miesiącach]
```

### 1.1 Calculation base (art. 25 ust. 1 FUS)

The base is the sum of:

1. the **contributions recorded on the account** (*konto*), valorized up to **the end of the month before the month from which the pension is paid**;
2. the **valorized initial capital** (*kapitał początkowy*, art. 173–175);
3. the **amounts recorded on the sub-account** (*subkonto*, art. 40a SUS).

The base is adjusted as follows (art. 25 ust. 1a–1g):

- If the person **received an early or partial pension**, the base is **reduced by the sum of the gross pensions already paid** (ust. 1b, 1c).
- The **sub-account is excluded** in three cases (ust. 1d–1f):
  - the "mixed" pension under art. 183;
  - the pension under art. 184;
  - when the person applies for and qualifies for an *okresowa emerytura kapitałowa* (periodic capital pension; see §7.3).
- The base is **not inheritable** (ust. 2). The exception is the sub-account "guaranteed payment" of art. 25b: if the pensioner dies within 3 years, a designated person receives the sub-account amount minus 1/37 of it for each month already paid.

**Payment start.** The right arises on the day all conditions are met (art. 100 ust. 1). Payment starts from that day, but **not earlier than the month in which the application is filed** (art. 129 ust. 1). So in practice the application month drives both the valorization cut-off and the age used.

### 1.2 Life expectancy: which table and which age

- **Unisex and in months.** Life expectancy is set jointly for men and women and expressed in months (art. 26 ust. 3).
- **Age** is expressed in **completed years and months** (art. 26 ust. 2).
- **Publication.** The GUS President publishes the table in M.P. **by 31 March each year** (art. 26 ust. 4).
- **Which table.** A table is the basis for pensions on **applications filed from 1 April to 31 March of the following year** (art. 26 ust. 5). In other words, it is the table in force on the **application date**.
- **More favourable table.** If it gives a higher pension, ZUS uses instead the table in force on the day the insured reached **60 (women) / 65 (men)** (art. 26 ust. 6). This has applied since 1 Oct 2017; it was added by Dz.U. 2017 poz. 38.
- **ZUS practice** ([ZUS, "Sposób wyliczenia emerytury"](https://www.zus.pl/swiadczenia/emerytury/emerytura-dla-osob-urodzonych-po-31-grudnia-1948/sposob-wyliczenia-emerytury)):
  - If the application is filed in a month **after** the month the person reached retirement age, ZUS uses the age on the **application date**. If it is filed in that month or earlier, ZUS uses the age on the day retirement age was reached.
  - If the application is filed before 1 April but retirement age is reached after 31 March, ZUS uses the table in force from 1 April.
  - *These age and table rules are ZUS's reading of art. 26 together with art. 129. The statute itself only says "wiek przejścia na emeryturę".*
- **Current table:** [Komunikat Prezesa GUS z 25 marca 2026 r., M.P. 2026 poz. 319](https://eli.gov.pl/eli/MP/2026/319/ogl). It applies to applications filed from 1 Apr 2026 to 31 Mar 2027.
  - It covers ages 30–90+ in steps of years plus months 0–11.
  - Values are given to **0.1 month**.
  - Examples: **60 y 0 m = 268.9**, 62 y 0 m = 250.0, **65 y 0 m = 222.7**, 67 y 0 m = 205.4.
  - A model needs the whole table for each application year, not one number per age.

---

## 2. Contributions

### 2.1 Rate and who pays

- **Art. 22 ust. 1 pkt 1 SUS:** the pension contribution is **19.52 %** of the contribution base. For comparison: disability 8.00 %, sickness 2.45 %, accident 0.40–8.12 %.
- **Art. 16 ust. 1 SUS:** for employees and the similar groups listed there, the pension contribution is paid **in equal parts by the insured and the payer**, i.e. 9.76 % + 9.76 %.
- **Art. 16 ust. 4:** self-employed persons pay the whole contribution themselves.

### 2.2 Split between account, sub-account and OFE (art. 22 ust. 3 SUS, art. 111 SUS)

The split depends on whether the person is in the "split" regime at all. That is decided by birth year and OFE history (art. 111):

| Situation | Account (*konto*) | Sub-account (*subkonto*) | OFE | Legal basis |
|---|---|---|---|---|
| Born after 31 Dec 1968, **or** born 1949–1968 and joined OFE in 1999; contributions **not** sent to OFE (never chose OFE, or stopped / switched under the 2014 rules) | **12.22 %** | **7.3 %** | 0 | art. 22 ust. 3 pkt 2, ust. 3d SUS |
| Same group, contributions **sent to OFE** (OFE member who declared to keep paying in) | **12.22 %** | **4.38 %** | **2.92 %** | art. 22 ust. 3 pkt 1 SUS |
| Born 1949–1968 who **never joined OFE** | **19.52 %** (whole contribution) | none (no sub-account) | 0 | art. 111 ust. 7 SUS ("cała składka … przekazywana jest na FUS"); art. 40 ust. 1 SUS |

- **The 12.22 % figure** is not written as a rate in art. 22. It is the remainder, 19.52 − 7.3. It does appear literally in art. 184 ust. 3 FUS ("wskaźnik korygujący 19,52/12,22").
- **ZUS confirms the birth-year rule** ([ZUS, "Subkonto"](https://www.zus.pl/pracujacy/twoje-konto-w-zus/subkonto)): "Założyliśmy Ci subkonto, jeżeli jesteś osobą urodzoną po 1968 r. Jeżeli urodziłeś się przed 1969 r., masz u nas subkonto tylko wtedy, gdy należysz do otwartego funduszu emerytalnego (OFE)."
- **Demographic Reserve Fund (FRD).** 0.35 % of the base is passed to the FRD (art. 22 ust. 4 and art. 112 ust. 4 SUS). Art. 40 ust. 1 SUS excludes only the OFE and sub-account parts from what is recorded on the account. So the FRD transfer does **not** appear to reduce the 12.22 % recorded on the account. *This is inferred from the wording; I found no explicit ZUS statement.*
- **Stopping contributions.** Nothing goes to OFE or the sub-account once a pension has been granted (art. 22 ust. 3g SUS). The exceptions are listed in ust. 3h: periodic capital pension, bridging pension, teacher's compensation benefit.

### 2.3 Annual cap on the contribution base (art. 19 SUS)

- **The cap.** The annual pension and disability contribution base cannot exceed **30 × the projected average monthly wage** for that year, as set in the budget act or its draft (art. 19 ust. 1). If no projection exists, the Q3 average wage of the previous year is used (ust. 2).
- **Above the cap** no pension or disability contributions are due (ust. 3, 5). Months above the cap still count as **insurance periods** (ust. 7).
- **Announcement.** The minister announces the cap in M.P. by the end of the preceding year (ust. 10).
  - **2026: 282 600 zł** (projected wage 9 420 zł). [Obwieszczenie MRPiPS z 19 XI 2025, M.P. 2025 poz. 1206](https://eli.gov.pl/eli/MP/2025/1206/ogl).
  - **2027: 300 990 zł** (projected wage 10 033 zł). [Obwieszczenie MRPiPS z 3 XI 2026, M.P. 2026 poz. 1098](https://eli.gov.pl/eli/MP/2026/1098/ogl).

---

## 3. Valorization of the account (art. 25 and 25a FUS)

### 3.1 Annual valorization

- **When.** Every year **from 1 June**, starting with the valorization "for 2000" (art. 25 ust. 3).
- **What is valorized** (art. 25 ust. 4): the contributions recorded on the account **as of 31 January of the year "for which" valorization is done**, plus earlier valorizations.
  - So the 1 June Y+1 valorization uses the index "for year Y" and applies to the balance at 31 Jan Y. That balance is essentially contributions for work up to December Y−1.
  - Contributions recorded after 31 Jan Y first earn an annual index one year later.
  - *This lag matters for a model.*
- **How:** balance × index (art. 25 ust. 5).
- **Index** (art. 25 ust. 6–8):
  - The index equals the CPI (*ogółem*) of the previous calendar year, "powiększony o" the **real growth of the total assessed pension contributions** (*wzrost realny sumy przypisu składek na ubezpieczenie emerytalne*) in that year.
  - Real growth = nominal growth of the contribution total ÷ CPI (ust. 7).
  - Nominal growth = this year's contribution total ÷ last year's (ust. 8).
  - So the measure is the **growth of the total pension contributions assessed** (roughly the covered wage bill). It is **not** the average wage and not "the contribution base" of one person.
  - Since real = nominal ÷ CPI, CPI × real equals the nominal growth of the contribution total. *Whether "powiększony o" is applied multiplicatively or additively is not spelled out. Use the published index as authoritative.*
- **Lower bounds** (two of them):
  1. the index may **not be lower than the CPI** of the previous year (art. 25 ust. 6, last sentence), so the real component is floored at zero;
  2. the account balance **cannot decrease** as a result of valorization (art. 25 ust. 3).
- **Precision:** the index is set **to 0.01 percentage point** (art. 25 ust. 10).
- **Announcement:** the minister announces the index in M.P. by the 25th of the month before the valorization date (art. 25 ust. 12).
  - Latest: **index for 2025 = 109.81 %**, applied from 1 June 2026. [M.P. 2026 poz. 506](https://eli.gov.pl/eli/MP/2026/506/ogl).

### 3.2 Quarterly valorization for retirements during the year (art. 25a FUS)

- **What.** When a pension is calculated, contributions recorded after 31 January of the year of the last annual valorization are **valorized quarterly** (art. 25a ust. 1).
- **Last quarter applied** depends on the calendar quarter in which the pension is calculated (ust. 2):

  | Pension calculated in | Last quarterly valorization is for |
  |---|---|
  | Q1 of year Y | Q3 of Y−1 |
  | Q2 of year Y | Q4 of Y−1 |
  | Q3 of year Y | Q1 of Y |
  | Q4 of year Y | Q2 of Y |

- **June rule** (ust. 2a–2b):
  - A pension calculated **in June** may be valorized **as if calculated in May**, if that is more favourable.
  - The same applies to someone who **reached retirement age after 31 May** of that year.
  - These paragraphs are absent from the 2022 consolidated text and present in the 2023 one (Dz.U. 2023 poz. 1251). The only FUS-act amendment between them is Dz.U. 2022 poz. 2461, in force 1 Mar 2023, so that is almost certainly the source. *I did not open that act to check its article list.*
- **What is valorized quarterly** (ust. 3): contributions recorded as of the last day of the first month of the quarter, plus earlier quarterly valorizations.
- **Index** (ust. 5): the same construction as the annual index, applied to a quarter. It is floored at the quarterly CPI, and the balance cannot decrease (ust. 4).
  - Precision: 0.01 % (ust. 8).
  - Latest examples: Q1 2026 = **110.50 %** ([M.P. 2026 poz. 625](https://eli.gov.pl/eli/MP/2026/625/ogl)). The index for Q2 2026 is in M.P. 2026 poz. 904.
  - *Observation, not verified:* the published quarterly indices are of annual size (about 10 %). The statute says "w stosunku do poprzedniego kwartału". In practice the quarterly figure appears to compare with the same quarter a year earlier. I found no primary source explaining the method.
- **Partial pension.** Quarterly gains from an earlier partial pension are not carried into the later art. 24 pension (art. 25 ust. 1a).

---

## 4. Valorization of the sub-account (art. 40c and 40d SUS)

### 4.1 Annual valorization

- **When and what:** annually **from 1 June**, applied to the sub-account balance at **31 January** of the year for which it is done, plus earlier valorizations (art. 40c ust. 1–3). This is the same timing as the account.
- **Index** (ust. 4): the **average annual nominal GDP growth (current prices) over the last 5 years** before the valorization date.
- **Precision:** 0.01 % (ust. 5).
- **Announcement:** published by GUS by 15 May (ust. 6).
  - Latest: **110.61 %**, valorization from 1 June 2026. [Komunikat Prezesa GUS z 14 V 2026, M.P. 2026 poz. 487](https://eli.gov.pl/eli/MP/2026/487/ogl).
- **Lower bound:** only that the **balance cannot decrease** (art. 40c ust. 1). There is **no CPI floor**, unlike the account.

### 4.2 Quarterly valorization for retirements during the year (art. 40d SUS)

- **Quarter mapping:** the same Q1→Q3 of Y−1 … Q4→Q2 of Y table as for the account (art. 40d ust. 2).
- **Different index formula** (ust. 5):

  ```
  WKW = (WRW − 100 %) / 4 + 100 %
  ```

  - WRW is the last published annual sub-account index.
  - Check: (110.61 − 100)/4 + 100 = 102.65 %, which matches ZUS's Q1 2026 figure in [M.P. 2026 poz. 550](https://eli.gov.pl/eli/MP/2026/550/ogl).
  - Precision: 0.01 % (ust. 6). The balance cannot decrease (ust. 4).
- **No June rule.** Art. 40d has **no equivalent of the account's June/May rule** (art. 25a ust. 2a–2b).

---

## 5. Initial capital (*kapitał początkowy*, art. 173–175, 185a FUS)

- **Who has it.** Persons born after 1948 who were insured before 1 Jan 1999 (art. 173 ust. 1, 1a, 1b).
- **What it is.** The value of a hypothetical "old-rules" pension at 1 Jan 1999 (art. 173 ust. 3), **multiplied by the life expectancy in months at age 62** (art. 173 ust. 2).
  - The hypothetical pension is computed under art. 53, as modified by art. 174:
    - **24 % of the base amount** (kwota bazowa 1 220.89 zł, art. 174 ust. 7 and art. 177), multiplied by a coefficient *p* for age and service (art. 174 ust. 8–13);
    - plus **1.3 %** of the assessment base for each contributory year before 1999;
    - plus **0.7 %** for each non-contributory year (art. 53 ust. 1).
  - The assessment base is a wage index from 10 consecutive years before 1999, capped at **250 %**, multiplied by the base amount (art. 174 ust. 3, art. 15 ust. 5).
  - **p = √((age on 31 XII 1998 − 18)/(60 or 65 − 18)) × (service / 20 or 25)**, rounded to 0.01 % and capped at 100 % (art. 174 ust. 8, 12).
  - Service and age for *p* are counted in full years. More than 6 extra months round up (art. 174 ust. 9, 10). Service may be counted in days if that is more favourable (ust. 9a).
  - *Which GUS table gives the "age 62" figure is not stated in the text. ZUS is commonly reported to use 209 months, but I did not verify this from a primary source.*
- **Valorization** (art. 173 ust. 4–6a):
  - 1 June 2000: by average-wage growth.
  - Every year from 1 June 2002: **by the same index and the same quarterly rules as the account** (art. 25 ust. 3–8, 10 and art. 25a).
  - It is recorded on the account (ust. 6) and cannot decrease (ust. 6a).
- **Determination.** Initial capital is set by a ZUS **decision** (art. 175 ust. 3). It can be redetermined on new evidence (art. 175 ust. 4, art. 114).
  - At retirement it is **recalculated** to add pre-1999 higher-education periods (art. 7 pkt 9 FUS) as non-contributory periods, up to 1/3 of contributory periods (art. 185a).
- **Can a model take it as an input?** Yes.
  - The law fixes it as a per-person value decided by ZUS.
  - After 1999 it evolves only through the account index (annual and quarterly).
  - A model can accept "valorized initial capital at date D" as input and carry it forward with the account indices. Insured persons see this figure in the annual account statement ZUS must provide by 31 August (art. 50 SUS).
  - Recomputing it from pre-1999 wage history is possible from art. 174, but it needs the 1998 base amount, the wage indicator and the age-62 divisor.

---

## 6. OFE and the "suwak"

- **Gradual transfer starts at retirement age minus 10 years** (art. 100c ust. 1 pkt 1 OFE act). ZUS notifies the OFE when the member reaches the age 10 years below their art. 24 retirement age, i.e. 50 for women and 55 for men.
  - From then on, **each month** the fund redeems this many units: the units on the member's account ÷ the number of months left to retirement age (art. 100c ust. 2).
  - The money goes to FUS (art. 111c ust. 1). By the month of retirement age **all units are redeemed** (art. 100c ust. 3).
- **Where the money goes.** The transferred amounts are recorded on the **sub-account** (art. 40a ust. 1 SUS). After that they earn the sub-account (GDP) index.
- **Contributions stop going to OFE.** Once ZUS has notified the fund, the 2.92 % stops going to OFE and is recorded on the sub-account instead (art. 22 ust. 3d–3f SUS).
- **Teachers.** For a teacher applying under art. 88a Karta Nauczyciela, everything is transferred at once (art. 100c ust. 1 pkt 2, ust. 2a).
- **Modelling consequence.** From retirement age − 10 years, the OFE balance converts month by month into sub-account money. The OFE balance's market value at each redemption date is an exogenous input.

---

## 7. Retirement age, minimum pension, working past retirement age, indexation

### 7.1 Retirement age

- **Art. 24 ust. 1 FUS:** the pension is due on reaching **at least 60 (women) / at least 65 (men)**, subject to the special regimes in art. 46, 47, 50, 50a, 50e, 184 and art. 88a Karta Nauczyciela.
- **In force since 1 Oct 2017.** Dz.U. 2017 poz. 38 reversed the gradual rise to 67.
- **Not automatic.** "Co najmniej" means the person may retire later, and a pension is granted only on application (art. 129).
- **Employment.** No condition requires ending employment. However, the pension is **suspended** while the person keeps working for the **same employer** without first ending the employment relationship (art. 103a, applies at any age).
- **Earnings tests.** Income-related reductions do not apply after 60/65 (art. 103 ust. 2).

### 7.2 Minimum pension (art. 85, 87 FUS)

- **Amount.** Art. 85 ust. 2 still prints the historical figure 1 588.44 zł. The amount is indexed every 1 March (art. 85 ust. 3) and announced by ZUS (art. 94 ust. 2).
  - **From 1 March 2026: 1 978.49 zł/month.** [Komunikat Prezesa ZUS z 17 II 2026, M.P. 2026 poz. 220](https://eli.gov.pl/eli/MP/2026/220/ogl).
- **Mechanism.** It is a **top-up**: the art. 26 pension (plus any periodic capital pension) is raised so that the total reaches the minimum (art. 87 ust. 1). The top-up is refunded from the state budget (ust. 2).
- **Conditions:**
  - **men:** age 65 and **≥ 25 years** of contributory and non-contributory periods;
  - **women:** age 60 and **≥ 20 years** (art. 87 ust. 1).
  - Non-contributory periods count only **up to 1/3 of the proven contributory periods** (art. 87 ust. 1 last sentence, art. 5 ust. 2).
  - **After 1998**, a month whose contribution base was **below the minimum wage** counts only **in proportion** to it (art. 87 ust. 3). Exceptions are listed in ust. 4: sick pay and benefits, carers' allowances, etc.
  - **No top-up** if the pensioner earns from insured activity more than the top-up amount (ust. 5), or if the person also has a military, police or farmer's pension (ust. 5a).
  - Farmer (KRUS) contribution periods credited under art. 26a also count (ust. 1a).
- **Counting service** (new **art. 5a FUS**, added by Dz.U. 2026 poz. 26, art. 12 pkt 1):
  - Years, months and days are added separately.
  - Days are converted to months at **30 days = 1 month**, and months to years at **12 months = 1 year**.
  - Working-day records use 22 working days per month (25 before 1981).
  - In force **14 days after publication on 12 Jan 2026, i.e. from 27 Jan 2026.** *That date is computed from art. 43 pkt 1 of the amending act.*

### 7.3 Women who were OFE members: periodic capital pension (60 → 65)

- **Who.** A person who meets the statutory conditions for an *okresowa emerytura kapitałowa* (Ustawa z 21 XI 2008 o emeryturach kapitałowych).
- **Effect.** The sub-account is **not** added to the FUS base at 60 (art. 25 ust. 1e FUS; art. 40 ust. 1e–1g SUS). It is paid out separately until 65.
- **At 65** the FUS pension is recalculated ex officio with the age-65 table and the sub-account added (art. 26c FUS). It cannot fall below the previous FUS pension plus periodic capital pension (art. 26c ust. 3).
- **Men** retire at 65, so the sub-account always goes into the base.
- *I did not read the Emerytury kapitałowe act itself. The conditions and the formula for the periodic pension are not verified here.*

### 7.4 Working after retirement: recalculation (art. 108 FUS)

- **Formula.** A pensioner who keeps paying pension contributions may apply to have the pension **increased** by: **(valorized contributions recorded after the pension was granted) ÷ (life expectancy for the person's age on the date of the recalculation application)** (art. 108 ust. 1–2).
  - The contributions counted are those recorded from the month payment first started to the month before the application (ust. 4).
  - The rule applies to each later application (ust. 5).
- **When the application can be filed:** after the end of a calendar year, or after insurance ends (ust. 3).
- **ZUS practice** ([ZUS, "Przeliczanie emerytur i rent"](https://www.zus.pl/swiadczenia/emerytury/przeliczanie-emerytur-i-rent)): the recalculated amount is paid from the month of application.
- **OFE members.** The application also counts as an application to recalculate the periodic capital pension (art. 108 ust. 2a).
- **Not covered here:** a full re-calculation "from scratch" with a newer table for someone who was granted a pension but never drew it. I found no statutory basis beyond art. 26 ust. 6 and art. 24a, and this is *not verified*.

### 7.5 Indexation of pensions in payment (art. 88–94 FUS)

- **When and what:** every year **from 1 March**. The pension amount *and* its assessment base are multiplied by the index, and the amount cannot decrease (art. 88 ust. 1–3).
- **Index** (art. 89 ust. 1–2):
  - the **average annual CPI of the previous year**, increased by **at least 20 % of the real growth of the average wage** in that year;
  - the CPI used is the **pensioner-household CPI or the general CPI, whichever is higher**.
- **Setting the "at least 20 %".** It is negotiated in the Social Dialogue Council (RDS) in June. Without agreement, the Council of Ministers sets it by regulation (art. 89 ust. 3–5).
  - **2027: 20 %**. [Rozporządzenie RM z 28 VII 2026, Dz.U. 2026 poz. 1018](https://eli.gov.pl/eli/DU/2026/1018/ogl).
- **2026 index: 105.3 %**. [Komunikat MRPiPS z 11 II 2026, M.P. 2026 poz. 202](https://eli.gov.pl/eli/MP/2026/202/ogl).
- **Ad hoc rules.** Some years had special statutory rules, e.g. 2023 had a minimum increase of 250 zł (Dz.U. 2022 poz. 2461, art. 6). A model should allow such overrides.

---

## 8. Rounding

What the statute does say:

| Item | Rule | Basis |
|---|---|---|
| Account annual and quarterly index | to 0.01 percentage point | art. 25 ust. 10; art. 25a ust. 8 FUS |
| Sub-account annual and quarterly index | to 0.01 % | art. 40c ust. 5; art. 40d ust. 6 SUS |
| Life expectancy | published to 0.1 month (GUS table format) | M.P. 2026 poz. 319 |
| Initial-capital coefficient *p* | to 0.01 %, max 100 % | art. 174 ust. 12 FUS |
| Service for *p* and age for *p* | full years; more than 6 months rounds up (or days if more favourable) | art. 174 ust. 9, 9a, 10 FUS |
| Service for minimum-pension conditions | 30 days = month, 12 months = year | art. 5a FUS (from 27 Jan 2026) |
| Old-rules periods (art. 53) | full months | art. 53 ust. 2 FUS |

**Not found:** I found **no general statutory rule on rounding the computed pension amount**, the valorized account balances, or the indexed pension. The FUS act rounds specific amounts only:
- funeral grant: up to the full złoty (art. 80 ust. 5);
- maximum reduction amounts: up to 10 groszy (art. 104);
- the per-person share of the amount exempt from enforcement on a shared family pension: up to the full grosz (art. 141 ust. 5).

ZUS decisions and announcements show amounts **in groszy**, which suggests rounding to 0.01 zł. *Where ZUS rounds (each valorization step or only the final amount) and how (half-up or other) is **not verified** from a primary source.* A model should make this configurable.

---

## 9. The owner's claims, checked

| # | Claim | Verdict | Correction / precision |
|---|---|---|---|
| 1 | Contribution 19.52 % | **Correct** | Art. 22 ust. 1 pkt 1 SUS. For employees it is split 9.76 % payer / 9.76 % insured (art. 16 ust. 1). |
| 2 | 12.22 % to account and 7.3 % to sub-account for non-OFE | **Partly correct, needs correction** | True only for people **in the split regime** who do not send money to OFE: born after 1968, or born 1949–68 who joined OFE in 1999 and later stopped. **People born 1949–1968 who never joined OFE have no sub-account: all 19.52 % goes to the account** (art. 111 ust. 7 SUS; ZUS). OFE contributors: 12.22 % account + 4.38 % sub-account + 2.92 % OFE (art. 22 ust. 3 pkt 1). |
| 3 | Account valorized by inflation + real growth of contribution base, with a lower bound | **Essentially correct, two precisions** | (a) The growth measure is the **total of assessed pension contributions economy-wide** (*suma przypisu składek*), not "the contribution base" of a person or the average wage. (b) There are **two floors**: the index is ≥ the previous year's CPI, and the balance cannot fall. Also note the **timing**: from 1 June, applied to the balance at 31 Jan of the year "for which" it is done. |
| 4 | Sub-account valorized by 5-yr average nominal GDP growth | **Correct** | Art. 40c ust. 4 SUS. The only floor is that the balance cannot fall (no CPI floor). The quarterly rate is (WRW−100)/4+100, not the account's quarterly method. There is no June/May option. |
| 5 | Pension = base / life expectancy in months from the annual GUS table | **Correct, with detail** | The table is unisex. The age is in completed **years and months**. The table is the one in force on the **application date** (1 Apr–31 Mar cycle), or the one in force on reaching 60/65 if more favourable (art. 26 ust. 2–6). Values are given to 0.1 month. |
| 6 | Retirement age 60 W / 65 M | **Correct** | Art. 24 ust. 1 FUS, since 1 Oct 2017. It is the *minimum* age, and retirement requires an application. |
| 7 | Minimum pension requires 20 yrs (W) / 25 yrs (M) | **Correct, with conditions** | The 20/25 years are contributory + non-contributory periods, with non-contributory periods capped at **1/3 of contributory**. **After 1998, months with a contribution base below the minimum wage count pro rata** (art. 87 ust. 3). The guarantee is a **top-up** to 1 978.49 zł (from 1 Mar 2026). It is lost if earnings exceed the top-up or another uniformed or farmer's pension is received. |
| 8 | Indexation = inflation + at least 20 % of real wage growth | **Correct** | Art. 89 ust. 1. The CPI is the **higher of the pensioner-household CPI and the general CPI**. Indexation is yearly from 1 March. The 20 %+ share is set by RDS or regulation (20 % for 2027). |

---

## 10. Law changes in 2025–2026 found

**In the FUS act:**

- **Dz.U. 2024 poz. 1243** (in force 1 Jan 2025): "renta wdowia". A widow or widower may draw their own pension plus 25 % of the survivor's pension, or the reverse, capped at 3 × the minimum pension (art. 95a, 95b). It does not change the NDC formula but matters for household-level models.
- **Dz.U. 2025 poz. 718**: funeral grant raised to 7 000 zł and indexed (art. 80, from 1 Jan 2026). It also added art. 118a, which allows decisions by automated processing (from 4 Jun 2025). Not formula-relevant.
- **Dz.U. 2025 poz. 769** (13 Jul 2025, deregulation) and **Dz.U. 2025 poz. 1159** (1 Jan 2026, funeral-grant procedure): procedural only.
- **Dz.U. 2026 poz. 26:**
  - from 27 Jan 2026, **new art. 5a**, the rule for counting service periods (§7.2);
  - from 13 Apr 2026 and 1 Jan 2027, changes to how medical certification is done; not formula-relevant.
- **Dz.U. 2026 poz. 425** (14 Apr 2026): blood-donation leave counts toward the art. 50e periods (special regime). Not formula-relevant.

**In the SUS act:**
- 2025–2026 amendments (Dz.U. 2025 poz. 620 … 1423, consolidated into Dz.U. 2026 poz. 199; and Dz.U. 2026 poz. 252, 426, 473, 507, 734). **None changes art. 19, 22 or 40–40e.** The rate, split, cap rule and valorization rules are unchanged.

**Parameter announcements for 2026:**
- cap 282 600 zł;
- indexation 105.3 %;
- minimum pension 1 978.49 zł;
- account index for 2025: 109.81 %;
- sub-account index: 110.61 %;
- new GUS table from 1 Apr 2026 (60 y = 268.9 months);
- cap for 2027: 300 990 zł;
- 2027 indexation share: 20 %.

---

## 11. Not verified from a primary source (summary)

- How the pension amount and valorized balances are rounded, and at which steps (§8).
- Which GUS table gives the "age 62" divisor for initial capital, reportedly 209 months (§5).
- How the quarterly account index is computed in practice: the published values look year-on-year (§3.2).
- Whether the account index applies CPI and real growth multiplicatively or additively ("powiększony o"). The published index should be used (§3.1).
- That art. 25a ust. 2a–2b came from Dz.U. 2022 poz. 2461 (§3.2).
- Conditions and formula of the *okresowa emerytura kapitałowa*; that act was not read (§7.3).
- Full re-calculation with a newer table for someone who was granted a pension but never drew it (§7.4).
- Whether the 0.35 % FRD transfer reduces the 12.22 % recorded on the account; it appears not to (§2.2).
