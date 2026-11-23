# ZEV-UP — AHP Priority Survey

**A web-based, multi-criteria decision-support questionnaire for evaluating priorities and vehicle concepts in the ZEV-UP research project.**

**Live survey (existing deployment):** [zev-up-ahp-survey.netlify.app](https://zev-up-ahp-survey.netlify.app/)  
**Project:** [ZEV-UP](https://zev-up.eu/) — *Frugal Zero-Emission Vehicles for the Urban Passenger challenge*  
**Programme:** Horizon Europe · Grant Agreement No. **101138721**  
**Status:** Research prototype; benchmark and methodology require final consortium validation.

> **Deployment note:** The live URL is the existing Netlify deployment. Changes in `ZEV-UP_AHP_Survey_REVISIONE.html` will not appear there until that version is configured and redeployed.

## About the project

ZEV-UP investigates **affordable, modular, user-centred battery-electric vehicles (BEVs)** for urban passenger and goods transport. Its research addresses vehicle design, real-world use cases, economic viability, environmental impacts and the uptake of frugal electric mobility in different markets.

This repository contains an **Analytic Hierarchy Process (AHP)** survey designed to support the prioritisation of evaluation criteria and vehicle concepts. The activity relates to **Work Package 5 (Evaluation, Impacts and Market Uptake)**, specifically **Task T5.4 (Economic Impacts and Economies of Scale)**. The Grant Agreement identifies AHP as a method for determining priorities that may differ by **use case, country and stakeholder needs**.

The tool does **not** substitute for the project's separate cost-effectiveness analysis, environmental life-cycle assessment or scale-up modelling. Its purpose is to collect structured preference judgements that can inform those analyses and the project's recommendations.

## What the application does

Respondents:

1. Select their **country**, **stakeholder category** and survey mode.
2. Compare evaluation criteria **pairwise** using an intuitive slider based on the AHP importance scale.
3. In **Full** mode, also compare the sub-criteria within each area; **Quick** mode skips this step.
4. Compare the three ZEV-UP concepts (**CM1**, **CM2**, **CM3**) within each evaluation area, using available benchmark information as context.
5. Receive priority weights, consistency indicators and an **exploratory** ranking of the alternatives after a successful submission.

The questionnaire supports government/regulatory stakeholders, industry and OEM representatives, fleet/logistics operators, end users, researchers and other participants.

### Evaluation hierarchy

| Evaluation area | Sub-criteria |
| --- | --- |
| **Economic viability & market dynamics** | Total cost of ownership; purchase price; Battery-as-a-Service cost-effectiveness; business-model scalability |
| **Operational efficiency & service quality** | Battery range and performance; charging speed and reliability; vehicle availability and uptime; maintenance ease and cost |
| **Sustainability & environmental impact** | GHG/CO₂ reduction; life-cycle environmental impact; local production and supply chain; energy efficiency per kilometre |
| **Technological innovation & performance** | Lightweight materials; battery technology; modularity and scalability; smart connectivity |
| **User experience & social acceptance** | Range-anxiety reduction; charging accessibility; driving comfort and safety; affordability and equity |

**Alternatives:** ZEV-UP CM1, CM2 and CM3. The correspondence between these internal identifiers and the publicly described two-seat urban, four-seat passenger and two-seat commercial configurations is **provisional** pending consortium confirmation.

### AHP calculations

The application constructs reciprocal pairwise-comparison matrices, calculates normalised priority vectors using the **geometric-mean method**, and reports a **consistency ratio (CR)**; the current interface treats **CR < 0.10** as consistent.

The current overall alternative score is:

$$
S_j = \sum_{i=1}^{5} w_i\,p_{j\mid i}
$$

where $w_i$ is the weight of evaluation area $i$ and $p_{j\mid i}$ is the priority of alternative $j$ within that area.

**Important methodological limitation:** Although Full mode collects sub-criterion priorities, **those weights do not currently enter the overall alternative score**. The ranking is therefore an **area-level exploratory result**, not a complete three-level AHP synthesis. Before definitive data collection, the consortium should either implement alternative assessments under each sub-criterion or formally justify the area-level aggregation approach. Unchanged sliders currently represent *equal importance*, so an unanswered comparison is not distinguished from an intentional equality judgement.

## Benchmark data: provenance and limitations

The revised HTML provides source links and explicitly identifies information awaiting technical validation. The public project material supports the broad concept of a modular platform, swappable batteries and three vehicle-use configurations; it does **not** establish final, independently verified specifications for each CM1/CM2/CM3 model.

In particular:

- The original **L6 / 60 km/h** combination was inconsistent with the **45 km/h L6e** regulatory limit; the actual homologation category and speed have **not** been inferred from the regulation.
- Previously displayed prices, ranges, vehicle weights and individual battery specifications are **not presented as confirmed values**.
- A **7–20 kWh** battery range, mentioned at project level in reporting material, must **not** be interpreted as a certified capacity for every variant.
- Missing figures are presented as **“Pending validation”**, not as zero and not as evidence that one concept performs worse than another.
- Partner-approved technical specifications, comparable market references, dated sources and a documented benchmark methodology are required before the survey is used as a final scientific instrument.

Reference material: [ZEV-UP vehicle concept](https://zev-up.eu/zev-up-car/), [ZEV-UP vehicle design update](https://zev-up.eu/zev-up-finalises-complete-digital-design-for-its-modular-vehicles/), [CORDIS project reporting](https://cordis.europa.eu/project/id/101138721/reporting), and [EU Regulation 168/2013](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32013R0168).

## Technology stack

- **Frontend:** static HTML, CSS and vanilla JavaScript; responsive, single-page questionnaire.
- **AHP engine:** client-side matrix construction, weighting, consistency checks and result display.
- **Database and authentication:** [Supabase](https://supabase.com/) (PostgreSQL, REST API, Auth and Row Level Security).
- **Hosting:** [Netlify](https://www.netlify.com/) or another static-site host.
- **Third-party browser libraries:** Supabase JS v2, SheetJS/XLSX, QRCode.js and Google Fonts (loaded from CDNs).
- **Administrator tools:** restricted response viewing and spreadsheet export.

No Node.js build process or server-side application is required to display the survey. A configured Supabase project is required for **persistent submissions** and protected administrator access.

## Repository files

| File | Purpose |
| --- | --- |
| `ZEV-UP_AHP_Survey.html` | Original survey implementation, retained as a reference. |
| `ZEV-UP_AHP_Survey_REVISIONE.html` | Revised survey with benchmark provenance, explicit data limitations and Supabase persistence logic. |
| `ZEV-UP_SUPABASE_SETUP.sql` | PostgreSQL schema, Row Level Security policies and administrator access setup. |
| `ZEV-UP_REVISIONE_NOTE.md` | Detailed revision notes, verification findings and deployment considerations. |
| `README.md` | Project overview and setup instructions. |

## Local preview

Open `ZEV-UP_AHP_Survey_REVISIONE.html` in a modern browser, or use a simple local HTTP server:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000/ZEV-UP_AHP_Survey_REVISIONE.html`.

The interface can be explored without credentials, but **successful submission requires Supabase configuration**.

## Supabase configuration

1. Create or choose a Supabase project and review any existing survey tables **before running migrations**.
2. In the Supabase **SQL Editor**, review and execute `ZEV-UP_SUPABASE_SETUP.sql`. It creates `public.zev_ahp_responses` and `public.zev_ahp_admins` with the relevant RLS policies.
3. Near the beginning of the revised HTML script, set the two **public client configuration values**:

   ```js
   window.ZEV_SUPABASE_CONFIG = {
     url: 'https://YOUR-PROJECT.supabase.co',
     publishableKey: 'YOUR_SUPABASE_PUBLISHABLE_OR_ANON_KEY'
   };
   ```

4. For administrator access, create an account in **Supabase Authentication**, then add its Auth user UUID to `public.zev_ahp_admins` as described in the SQL script.
5. Submit a **test response** and verify that a record appears in `public.zev_ahp_responses`. Confirm that unauthenticated users **cannot read** submitted answers.
6. Deploy the configured HTML as `index.html` to Netlify (or an equivalent static host), then repeat the end-to-end test on the public site.

**Never put a Supabase secret key or `service_role` key in frontend code.** The public `publishable`/legacy `anon` key is intended for browser use together with properly configured RLS.

### Data handling and administrator access

The browser submits the response to `zev_ahp_responses`. The revised interface reports success **only after the Supabase insert succeeds**; if it fails, the user sees an error and can export an unsaved JSON draft. Responses include country, stakeholder category, survey mode, pairwise judgements, weights and consistency information.

RLS is designed to allow public **INSERT** while restricting **SELECT/export** to authenticated users listed in `zev_ahp_admins`; administrator access is available from `#admin`. A public survey still requires safeguards against spam/automated submissions, a suitable privacy notice, retention rules, appropriate access controls and checks for personal-data risks in small respondent groups.

## Current limitations and next steps

This repository is a **research prototype**, not a final validated benchmark or a completed study. Priorities before field deployment are:

1. **Validate** the three concepts, their use-case mapping, technical attributes and external comparison vehicles with authoritative, traceable sources and project partners.
2. **Review the decision model**, including the relationship between macrocriteria, sub-criteria, alternatives and WP5 research questions.
3. **Complete Supabase integration** in the target project and test authenticated administration, exports and deployment.
4. **Pilot-test** the questionnaire for usability, unintentional default responses, inconsistent judgements and accessibility.
5. **Collect and analyse** stakeholder preferences by country, role and use case; undertake consistency screening and sensitivity analysis.
6. **Document** the aggregation methodology, assumptions, benchmark provenance and results for the WP5 evaluation activities.

**Working planning target:** finish the internal AHP analysis by **February 2027**. This is a project-team planning objective and is **not presented as a contractual Grant Agreement deadline**.

## Research context and acknowledgement

This software supports research conducted within **ZEV-UP — Frugal Zero-Emission Vehicles for the Urban Passenger challenge**, funded under the European Union's **Horizon Europe** programme (Grant Agreement **No. 101138721**). The scientific interpretation of survey results and final technical benchmarks remains subject to consortium review.

## Licence

No software licence is specified in the supplied project files. Confirm the intended ownership, reuse permissions and open-source licence with the project team before publishing the repository as openly reusable software.
