# What do JOSS and current Python packaging practice require for this project?

Research for issue #6. Researched 2026-11-09. Sources are primary (JOSS docs and blog, PyPA, Astral, tool maintainers, OSI, REUSE, GUS/ZUS, Polish statute) unless marked otherwise. Version numbers come from the PyPI JSON API on 2026-11-09.

## TL;DR: recommendations

| Topic | Recommendation | Why (beginner version) |
|---|---|---|
| JOSS timing | Plan to submit no earlier than **mid-May 2027**, and keep committing steadily in public from now on | JOSS requires more than 6 months of public history and rejects projects whose work is bunched up just before submission |
| JOSS AI policy | Keep a running log of how AI tools (Claude Code) were used and how you checked their output | Every JOSS paper now needs an "AI usage disclosure" section. Vague wording counts as an ethical breach |
| Package manager | **uv** (`uv init --lib`, commit `uv.lock`) | One tool covers Python installs, venvs, locking and building. It has an official GitHub Action and is the fastest to learn |
| Layout | **src/ layout** (`src/polish_pension_model/`) | Tests then run against the installed package, not loose files. `uv init --lib` creates this layout by default |
| Docs generator | **MkDocs Material + mkdocstrings** now, moving to **Zensical** once it settles | Markdown is easy to write. Material gets critical fixes until 2027-05-05 and Zensical reads the same `mkdocs.yml` |
| Docs hosting | **GitHub Pages** via an Actions workflow (Read the Docs if you later want versioned docs) | No extra account, and it deploys from the CI you already have |
| Code licence | **BSD-3-Clause** (MIT is just as good) | Permissive, OSI-approved and familiar to employers and researchers. It also stops others using your name to endorse forks |
| Data licence | Keep data under `data/` with its own notice crediting GUS/ZUS, and annotate it with a **REUSE.toml** | Keeps "our code" and "their numbers" legally separate and lets a tool check it |
| Lint/type/test | ruff (lint + format) + pyright (`standard`, later `strict`) + pytest, all run through `uv run` in GitHub Actions | All configured in `pyproject.toml`, one command each |
| Citation | Add **CITATION.cff** now and switch on the **Zenodo** GitHub integration before the first tagged release | GitHub shows a "Cite this repository" button. Zenodo assigns a DOI for each release, and JOSS needs an archive DOI at acceptance |
| PyPI name | **`polish-pension-model`** (matches repo; all 5 candidates are free) | Repo, import package and PyPI name all match, so there is nothing to remember |

---

## 1. JOSS: review criteria and submission requirements

### What changed in 2026

On 2026-01-05 JOSS replaced its old effort proxy ("substantial scholarly effort", roughly three months of developer time, lines of code) with evidence of **impact, design thinking and open development**. The reason given is that generative AI has pushed "the marginal cost of code generation … rapidly approaching zero". Submissions still awaiting review fall under the new scope. ([JOSS blog, 2026-01](https://blog.joss.theoj.org/2026/01/preparing-joss-for-a-generative-ai-future))

### Hard requirements ([Submitting](https://joss.readthedocs.io/en/latest/submitting.html), [Review criteria](https://joss.readthedocs.io/en/latest/review_criteria.html))

- **Research application:** "The software must have an obvious research application." The paper must not focus on new research results obtained with the software. This matters for the separate research paper: the JOSS paper describes the *software*, and the research results belong in the other paper.
- **Not too small:** "'Minor utility' packages, including 'thin' API clients, and single-function packages are not acceptable". The software also must not be "a one-off tool for a single analysis". There is no numeric threshold (hours, LOC or contributors). ([Submitting](https://joss.readthedocs.io/en/latest/submitting.html))
- **Open development history:** "The repository must have been public for more than six months prior to submission." Reviewers look for commits spanning 6+ months. Red flag: "All or most commits concentrated in the last few weeks before submission." ([Submitting](https://joss.readthedocs.io/en/latest/submitting.html), [Review criteria](https://joss.readthedocs.io/en/latest/review_criteria.html))
  - *This repo:* first commit 2026-11-08, so the earliest eligible date is about **2027-05-09**, assuming the repo is public now.
- **Research impact evidence:** realised impact or "credible near-term significance", such as publications, documented adoption or integration into workflows. ([JOSS blog](https://blog.joss.theoj.org/2026/01/preparing-joss-for-a-generative-ai-future), [Submitting](https://joss.readthedocs.io/en/latest/submitting.html))
- **Licence:** an OSI-approved licence, with "an actual license file present in the repository not just a reference to the license". A line saying "MIT license" in the README is explicitly not enough. ([Review criteria](https://joss.readthedocs.io/en/latest/review_criteria.html))
- **Hosting:** outsiders must be able to open issues and propose changes without manual approval or payment. GitHub qualifies. ([Submitting](https://joss.readthedocs.io/en/latest/submitting.html))
- **Documentation:** the README or docs must give a statement of need, installation instructions with automated dependency handling, example usage and API documentation. "API is undocumented" is listed as not acceptable. ([Review criteria](https://joss.readthedocs.io/en/latest/review_criteria.html))
- **Functionality:** reviewers install the package and check its core functionality. ([Review criteria](https://joss.readthedocs.io/en/latest/review_criteria.html))
- **Tests:** automated tests with CI are preferred, though documented manual checks are allowed. Red flag: "No way for you, the reviewer, to objectively assess whether the software works." ([Review criteria](https://joss.readthedocs.io/en/latest/review_criteria.html))
- **Community guidelines:** clear instructions on how to contribute, report issues and get support. In practice this means CONTRIBUTING.md plus an issue template. For a single-author project, the screening signals JOSS lists are public commit history, tagged releases or a changelog, tests and CI, clear docs and "a CONTRIBUTING file". ([Submitting](https://joss.readthedocs.io/en/latest/submitting.html))
- **Single-author projects** are allowed but face extra scrutiny. Red flag: "Single author with no evidence of community engagement, external use, or collaborative input." Engagement from non-authors is a "strong positive signal". ([Review criteria](https://joss.readthedocs.io/en/latest/review_criteria.html), [Submitting](https://joss.readthedocs.io/en/latest/submitting.html))
- **Authorship:** the submitting author must be a major contributor. Project direction and other non-code contributions count, purely financial ones do not, and every co-author must agree to be listed. ([Submitting](https://joss.readthedocs.io/en/latest/submitting.html))
- **Prior work:** re-implementing something that already exists is acceptable if similar software is cited. ([Review criteria](https://joss.readthedocs.io/en/latest/review_criteria.html))

### The paper ([Paper format](https://joss.readthedocs.io/en/latest/paper.html))

- `paper.md` plus a BibTeX file, kept in the repo. The paper "should be between 750-1750 words".
- Required sections: **Summary, Statement of need, State of the field, Software design, Research impact statement, AI usage disclosure**, plus authors and affiliations, references and funding acknowledgement.

### AI-assisted code policy ([Policies](https://joss.readthedocs.io/en/latest/policies.html))

- "The use of generative AI is permitted for most aspects of a JOSS submission."
- The paper's AI disclosure must name the tools and versions, say where they were used, and state that "human team members thoroughly reviewed, modified, and validated all AI-generated content". If no AI was used, say so explicitly.
- Inaccurate or incomplete disclosure "may be considered an ethical breach", with consequences ranging from desk rejection to institutional notification. Authors may not use AI to talk to editors or reviewers, except for translation.
- *Implication for this repo:* development uses Claude Code, so keep the `Co-Authored-By` trailers and note in PRs/ADRs what you verified by hand, for example tests derived from official ZUS examples. That evidence makes the disclosure section easy and credible to write.

### After acceptance ([Submitting](https://joss.readthedocs.io/en/latest/submitting.html))

Make a tagged release, deposit it in Zenodo or figshare, get a DOI and post the version and DOI in the review thread. Authors are asked to respond to reviews within 2 weeks and to finish changes within 4 to 6 weeks.

---

## 2. Package manager: uv vs Poetry vs Hatch vs pip

Latest versions on PyPI (2026-11-09): uv 0.13.7, Poetry 2.5.1, Hatch 1.18.2, PDM 2.29.7.

| | uv | Poetry 2.x | Hatch 1.17+ | pip (+venv) |
|---|---|---|---|---|
| Standard `[project]` metadata | yes | yes since 2.0 ([docs](https://python-poetry.org/docs/basic-usage/)) | yes | yes |
| Lock file | `uv.lock`, universal across platforms and Python versions. "should be checked into version control" ([docs](https://docs.astral.sh/uv/concepts/projects/layout/)) | `poetry.lock`. The docs suggest libraries *may* omit it ([docs](https://python-poetry.org/docs/basic-usage/)) | `pylock.toml` (PEP 751) via `hatch env lock`, added in 1.17.0, May 2026 ([blog](https://hatch.pypa.io/dev/blog/2026/05/30/hatch-v1170/)) | `pip lock` produces pylock.toml (marked experimental in pip, unverified here) |
| PEP 751 `pylock.toml` | export target (`uv export -o pylock.toml`) ([docs](https://docs.astral.sh/uv/concepts/projects/layout/)) | not mentioned in the docs | native | native |
| Installs Python itself | yes (`uv python install`) | no | yes | no |
| CI | official `astral-sh/setup-uv@v9` with built-in caching, `uv sync --locked` fails on a stale lock ([docs](https://docs.astral.sh/uv/guides/integration/github/)) | community actions | via pip/pipx | `actions/setup-python` |
| Beginner friendliness | one binary, few concepts, very fast | mature and well documented, but has its own concepts | env matrix is powerful but more to learn | the most manual |

**Recommendation: uv.** Use `uv init --lib`, commit `uv.lock`, run everything through `uv run …`, and use `uv sync --locked` in CI. *Rationale:* one fast tool replaces pyenv, venv, pip and pip-tools, and the lock file makes a reviewer's install match yours. That reproducibility is what JOSS reviewers check when they "install the software". Because uv can export `pylock.toml`, the lock data is not tied to uv for good.

---

## 3. src/ layout vs flat layout

PyPA lists these advantages of src layout: it "helps prevent accidental usage of the in-development copy of the code", avoids "subtle misconfiguration of the project's packaging tooling", and makes sure "an editable installation is only able to import files that were meant to be importable". The cost is that you must install the project before running it. PyPA does not formally prefer either layout. ([PyPA discussion](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/))

`uv init --lib` produces `src/<pkg>/__init__.py` plus `py.typed`, using the `uv_build` backend, and the uv docs say "A `src` layout is particularly valuable when developing libraries." ([uv docs](https://docs.astral.sh/uv/concepts/projects/init/))

**Recommendation: src layout.** *Rationale:* pytest then tests the package as users will install it, which catches packaging mistakes before a JOSS reviewer does. uv already handles the extra install step automatically.

---

## 4. Documentation: MkDocs Material + mkdocstrings vs Sphinx, and hosting

### Maintenance status: important recent changes

- **Material for MkDocs is in maintenance mode.** "As we're shifting our efforts to Zensical, Material for MkDocs is entering maintenance mode." Feature work has stopped. An October 2026 update extended critical and security fixes: "Critical maintenance now continues through May 5, 2027." ([squidfunk blog](https://squidfunk.github.io/mkdocs-material/blog/2025/11/11/insiders-now-free-for-everyone/)) The latest release is mkdocs-material 9.8.1 (2026-11-07, PyPI).
- **MkDocs core is stagnant, and "MkDocs 2.0" is incompatible.** mkdocs 1.6.1 (2024-08-30) is still the latest PyPI release. The Material team says MkDocs 2.0 (pre-release) drops plugin support, moves to TOML configuration, has no licence at present, and that Material "will cease to work with MkDocs 2.0". Starting with 9.7.2, Material builds print a warning about this. ([squidfunk blog, MkDocs 2.0](https://squidfunk.github.io/mkdocs-material/blog/2026/02/18/mkdocs-2.0))
- **Zensical** is the successor from the same team, MIT-licensed. Zensical 0.1.0 was released on 2026-11-05 (PyPI). It builds existing projects from `mkdocs.yml` "with few or no changes" and offers a `classic` theme that matches Material ([compatibility](https://zensical.org/compatibility/)). mkdocstrings support arrived in 0.0.11, with cross-references in 0.0.17 and objects.inv in 0.0.19. The docs page still calls it "preliminary", noting for example that backlinks are missing. (Sources: [Zensical monthly, 2026-01](https://mail.zensical.org/monthly/2026/01/), [mkdocstrings page](https://zensical.org/docs/setup/extensions/mkdocstrings/))
- **Sphinx** is actively maintained (9.2.0, 2026-10-21). It is the long-standing scientific Python standard and uses reStructuredText by default, with Markdown available through MyST. Its learning curve is steeper.

**Recommendation: MkDocs Material + mkdocstrings-python (2.0.10) now, written so it can move to Zensical.** Pin `mkdocs<2`, stick to standard Material features and plain Markdown, and try `zensical build` around Q1–Q2 2027, before Material's fix window closes on 2027-05-05. *Rationale:* Markdown plus NumPy-style docstrings is the least you need to learn, and it satisfies the JOSS "API documentation" check. The migration path is built in. If you would rather never migrate, Sphinx + MyST is the conservative alternative.

### Hosting

- **GitHub Pages:** free, deployed from a GitHub Actions workflow, no extra account. Serves one version of the docs.
- **Read the Docs:** free for open source, with versioned docs and PR previews. It has an official guide for Zensical, which recommends uv for reproducible builds ([RTD Zensical guide](https://docs.readthedocs.com/platform/latest/intro/zensical.html)), and supports custom build commands ([build.jobs](https://docs.readthedocs.com/platform/stable/config-file/v2.html)).

**Recommendation: GitHub Pages.** *Rationale:* one less service to manage. Switch to Read the Docs only if you need docs for each version, for example to match the release that JOSS archives.

---

## 5. Licences

### What JOSS accepts

Any OSI-approved licence, present as a full licence file ([Review criteria](https://joss.readthedocs.io/en/latest/review_criteria.html)). MIT, BSD-3-Clause, Apache-2.0, GPL-3.0, AGPL-3.0 and MPL-2.0 are all on the OSI list. **CC-BY-4.0 and CC0 are not OSI-approved for software** ([OSI list](https://opensource.org/licenses)), so do not use them for the code.

### Trade-offs for portfolio and research use

| Licence | What it means | Portfolio / employer view | Research view |
|---|---|---|---|
| MIT | do anything, keep the notice | most familiar, zero friction | very common in JOSS |
| BSD-3-Clause | like MIT, plus "no use of the author's name to endorse" | familiar (NumPy, SciPy, pandas use it) | the scientific-Python norm |
| Apache-2.0 | permissive, plus an explicit patent grant and a NOTICE file | liked by companies | slightly more boilerplate |
| GPL-3.0 | derivatives must stay GPL | some firms, including insurers, avoid embedding GPL code | protects openness, limits reuse in proprietary actuarial tools |
| AGPL-3.0 | GPL, plus network use counts as distribution | often banned outright by corporate policy | rarely needed for a modelling library |

**Recommendation: BSD-3-Clause.** *Rationale:* it matches the NumPy/SciPy ecosystem you depend on, lets insurers and consultancies try your code without legal review (which helps the portfolio goal), and is OSI-approved for JOSS. MIT is an equally good choice. Avoid GPL/AGPL unless you specifically want copyleft.

Declare it in `pyproject.toml` as an SPDX expression per PEP 639: `license = "BSD-3-Clause"` with `license-files = ["LICENSE", "LICENSES/*"]`. The old table form (`{file=…}`/`{text=…}`) is deprecated. ([PyPA guide](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/))

### Licensing bundled GUS/ZUS data separately from code

- **Legal status.** Polish copyright law, art. 4 pkt 2 of the Act of 4 Feb 1994 (consolidated text Dz.U. 2025 poz. 24), says that "urzędowe dokumenty, materiały, znaki i symbole" (official documents, materials, marks and symbols) are not subject to copyright. Whether a particular statistical table counts as an "official material" is a question of legal interpretation ([text, eli.gov.pl](https://eli.gov.pl/api/acts/DU/2022/2509/text.html); [UJ analysis](https://ruj.uj.edu.pl/entities/publication/a439856a-6a5b-4dbb-a6cb-4db9401885ed)). *Not legal advice.*
- **GUS terms:** GUS does not object to copying and using its material "w tym do własnych opracowań" (including in your own studies) "pod warunkiem, że będzie podane ich źródło" (provided the source is given). No named licence, and nothing about commercial use. ([stat.gov.pl/copyright](https://stat.gov.pl/copyright))
- **ZUS terms:** re-use of information published on the ZUS BIP and on dane.gov.pl is free of charge. Other information can be requested, possibly for a fee. ([ZUS BIP: ponowne wykorzystywanie](https://www.zus.pl/web/biuletyn-informacji-publicznej/obsluga-klienta/ogolne-zasady-zalatwiania-spraw/ponowne-wykorzystywanie-informacji-sektora-publicznego))
- **REUSE 3.3** (2024-11-14) gives a machine-checkable way to mark licensing per file. Licence texts go in `LICENSES/<SPDX-ID>.txt`. Files that can't carry comment headers, such as CSV data, are annotated in a top-level `REUSE.toml` ([spec](https://reuse.software/spec-3.3/)):

  ```toml
  version = 1

  [[annotations]]
  path = "data/gus/**"
  precedence = "override"
  SPDX-FileCopyrightText = "Główny Urząd Statystyczny (Statistics Poland)"
  SPDX-License-Identifier = "LicenseRef-GUS-terms"
  ```

  `LicenseRef-…` is REUSE's mechanism for custom terms: put the GUS wording and your attribution text in `LICENSES/LicenseRef-GUS-terms.txt`.

**Recommendation:** code under BSD-3-Clause; each data source in its own `data/<source>/` folder with a `data/README.md` giving source URL, download date, publisher and attribution line; a REUSE.toml annotation per source; and a `reuse lint` step in CI later on. *Rationale:* reviewers and users can see at a glance what you own and what you are redistributing, and the attribution GUS asks for is in place by construction.

---

## 6. Typing, linting and testing (ruff + pyright + pytest + GitHub Actions)

Latest versions: ruff 0.17.4, pyright 1.1.414, pytest 9.2.0.

- **ruff:** already used for formatting. Turn on linting with `[tool.ruff.lint] extend-select = [...]` on top of the defaults ([ruff config](https://docs.astral.sh/ruff/configuration/)). Sensible additions are `I` (isort), `B` (bugbear), `UP` (pyupgrade), `D` with `[tool.ruff.lint.pydocstyle] convention = "numpy"` (docstrings, which feeds mkdocstrings and the JOSS API-docs check), `NPY` and `PT`.
- **pyright:** configured under `[tool.pyright]` in `pyproject.toml`. Modes are `off`, `basic`, `standard` and `strict`, and the default is `"standard"` ([pyright config](https://raw.githubusercontent.com/microsoft/pyright/main/docs/configuration.md)). Start on `standard` and move to `strict` once the core API settles. The `py.typed` marker from `uv init --lib` tells users that the package ships types.
- **pytest:** `[tool.pytest.ini_options]` in `pyproject.toml`, tests in `tests/`. Add `pytest-cov` if you want a coverage badge.
- **GitHub Actions:** one workflow on push/PR using `astral-sh/setup-uv@v9` with `enable-cache: true`, then `uv sync --locked`, `uv run ruff format --check`, `uv run ruff check`, `uv run pyright` and `uv run pytest`, with a Python version matrix ([uv GitHub guide](https://docs.astral.sh/uv/guides/integration/github/)). The uv guide recommends pinning actions to commit SHAs.

**Recommendation:** exactly the stack above, all configured in `pyproject.toml` and run identically locally and in CI through `uv run`. *Rationale:* one config file and one command prefix means CI never disagrees with your laptop.

---

## 7. CITATION.cff and Zenodo DOI

- **CITATION.cff** (CFF 1.2.0) is a plain-text file in the repo root. GitHub links it from the repo page and offers BibTeX. When you release through the Zenodo–GitHub integration, Zenodo uses it to fill in the record. Minimal fields are `cff-version`, `message`, `authors` (with `orcid`), `title`, `version`, `date-released` and `identifiers`. ([citation-file-format.github.io](https://citation-file-format.github.io/))
- **Zenodo:** log in with GitHub, switch the repo **On**, and every GitHub release then gets archived with a DOI. The repo must be public and should have a licence. ([GitHub docs](https://docs.github.com/en/repositories/archiving-a-github-repository/referencing-and-citing-content)) Zenodo mints a **concept DOI**, which covers all versions and resolves to the latest, plus a **version DOI** per release. Cite the version DOI for reproducibility ([Zenodo versioning FAQ](https://zenodo.org/help/versioning)). JOSS asks for the DOI of the archived accepted version ([Submitting](https://joss.readthedocs.io/en/latest/submitting.html)).

**Recommendation:** add CITATION.cff with your ORCID early, and switch on Zenodo before tagging `v0.1.0`. Put the concept DOI badge in the README. *Rationale:* every release becomes citable at no cost, which counts toward JOSS's "research impact" evidence and looks professional on a CV.

---

## 8. PyPI name availability (checked 2026-11-09)

Checked against both `https://pypi.org/pypi/<name>/json` and `https://pypi.org/simple/<name>/`:

| Name | Status |
|---|---|
| polish-pension-model | **available** (404) |
| pension-pl | **available** (404) |
| zusmodel | **available** (404) |
| polpension | **available** (404) |
| emerytura | **available** (404) |

Caveat: PyPI can still refuse a free name at upload time, for example when it is too similar to an existing project. Nothing is reserved until the first upload.

**Recommendation: `polish-pension-model`** (import name `polish_pension_model`). *Rationale:* it matches the GitHub repo and is self-explanatory to non-Polish reviewers. Consider claiming it early with a `0.0.1` release once there is real code; do not upload an empty placeholder.

---

## Open questions / not verified

- Whether `pip lock` has left experimental status in current pip. Not checked against the pip docs.
- Whether the 2024-07-26 amendment changed art. 4 of the Polish copyright act. The search did not confirm, so check the current consolidated text on ISAP before relying on it.
- Exact attribution wording GUS expects. Its copyright page gives none beyond "give the source".
- Zensical's stability guarantees at 0.1.0. The roadmap gives no dates.
