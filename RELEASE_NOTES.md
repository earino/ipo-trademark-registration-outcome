<!-- dataset-factory:links -->
**Dataset Factory · v2026.10**

[Dataset website](https://earino.github.io/ipo-trademark-registration-outcome/versions/v2026.10/) · [GitHub release](https://github.com/earino/ipo-trademark-registration-outcome/releases/tag/v2026.10) · [Hugging Face](https://huggingface.co/datasets/earino/ipo-trademark-registration-outcome/tree/v2026.10) · [Catalogue](https://earino.github.io/dataset-factory-site/)

Artifact: 2c1dade2913d0a8b643c89e628095a18e03223a6167beb9363316389486a544d

### Suitability and evaluation requirements

Retained for retrospective model-development research. Each row is one distinct trade mark application whose features are its own filing-time attributes as recorded in the single frozen 2018-02-13 IPO snapshot; the prediction instant is the filing event (event_time) and no point-in-time or operational claim is made. The publisher serves exactly one frozen vintage and no historical vintages, so training-label timing and full-row historical availability are unverified and none is claimed. Measured finding carried beside every figure: the stock runner baseline (eval AUC 0.6224, Brier 0.1318) exceeds every measured simple reference on the identical 97,274 eval rows - mark-type rate 0.5650 (+0.0574, CI95 [0.0533, 0.0605]), nice-class rate 0.5421 (+0.0803, [0.0748, 0.0862]), calendar-month rate 0.4972 (+0.1252, [0.1174, 0.1315]) - all paired row-level intervals excluding zero, with the advantage holding in both eval filing years at flat prevalence: demonstrated headroom over these references under this protocol, with its scope recorded (row-level intervals only - no dependence cluster is measurable from the shipped columns; one model configuration, no re-tuning; two filing years of eval coverage). Persistence is not applicable (every scored row is a new application).

**Supported uses:** Retrospective research and method comparison on the pinned 2018-02-13 snapshot, predicting row-wise from the shipped filing-time columns; Agent evaluation with the labeled eval split as development/validation data and the holdout kept outside agent development until a design is frozen; Study of filing-cohort prevalence and of which filing-time attributes carry registration-outcome signal

**Not supported:** Operational or live filing-time screening claims: no historical operational-availability evidence exists and none is claimed; Point-in-time historical reconstruction: the publisher serves one frozen snapshot; a later re-fetch is a different vintage, not what was knowable earlier; Quoting the model AUC (0.6224) without the reference comparison (mark-type 0.5650, nice-class 0.5421, calendar 0.4972, with the paired intervals) beside it; Treating the snapshot-vintage owner fields (country/region/postcode area) or class columns as as-of-filing facts: ownership can be assigned after filing and classes can be narrowed

**Evaluation access:** isolated rows. The labeled eval split is development/validation data. Keep the final holdout outside agent development; prevent later rows from influencing earlier predictions.

**Historical availability:** unverified. **Training-label timing:** unverified. **Vintage:** current snapshot. **Selection:** prediction time.

**Cross-row audit:** untested. No cross-row exposure is declared and none is structurally available in the shipped columns: every feature is a filing-time attribute of the application itself (mark text, class booleans, category, type, series, owner fields, filing date), no history or aggregate feature crosses rows, tm_number is unique per row, and the label source columns are excluded from every shipped CSV. The panel-style reverse-history probes declared for panel-shaped candidates therefore do not apply; no donor-join audit has been run against this artifact. The isolation contract is the row-per-application grain itself; the shipped illustrative runner passes the whole eval feature table to the predictor, which does not create an exposure here but is also not an enforcement mechanism.

Structural qualification and baseline scores do not certify operational forecasting validity.

- Single frozen vintage (2018-02-13 snapshot, sha256-pinned); the publisher revises in place and retains no historical vintages, so no historical operational-availability claim is possible
- Training-label timing and full-row historical availability are unverified; the task is retrospective research only
- Owner and class columns are snapshot-vintage: assignment can move ownership after filing and classes can be narrowed post-filing - a uniform-vintage limitation declared in quality.json, never an as-of claim
- Censoring is outcome-observability conditioning recorded as a limitation: marks unresolved at the snapshot (21/172/809 per split) are excluded and counted, never labelled negative
- The headroom finding's scope: row-level intervals only (no dependence cluster measurable from the shipped columns), one model configuration with no re-tuning, and two filing years of eval coverage; beating the measured rate references does not enumerate all simple rules and does not prove a causal signal
- The holdout was never evaluated; no holdout figure exists and its first use must follow a frozen design

Review: 2026-10-30; applies to v2026.10 / `2c1dade2913d0a8b643c89e628095a18e03223a6167beb9363316389486a544d`. Frozen data and tags are unchanged.
<!-- /dataset-factory:links -->

# Release notes - v2026.10

## What this is

`ipo-trademark-registration-outcome` is a binary prediction task: will a UK domestic
trade mark application filed 2010-2016 reach registration, predicted from what is
fixed at filing (mark text, Nice class coverage, category, mark type, series
membership/size, owner country/region/postcode area, filing date), as resolved in the
IPO's frozen 2018-02-13 open-data snapshot. This is the first packaged release; there
is no prior version.

The prediction instant is the **filing event** (`event_time`). The task is a
clearly-labelled retrospective single-snapshot construction: the publisher exposes
exactly one frozen vintage, no historical vintages exist, and none are reconstructed.
Training-label timing is UNVERIFIED by construction; the evaluation contract in
`quality.json` is retrospective research under an isolated-rows design intent - the
shipped benchmark validator passes the whole feature table to the predictor, so the
row-per-application isolation is the artifact's contract, not a runtime-enforced
protocol. No operational screening claim is made.

## Artifact identity

- Artifact version (gate digest over the five files): `2c1dade2913d0a8b643c89e628095a18e03223a6167beb9363316389486a544d`
- Gate version: 1.5.0, 45/45 checks on the full artifact, exact-byte `--accept`
  ACCEPTED on the worker (job `ipo-tm-004`) and re-accepted in jobs
  `ipo-tm-baseline-001`, `ipo-tm-diag-001`, `ipo-tm-diag-002`.
- Source snapshot: `opendatadomestic.zip` (63,238,846 B, sha256
  `3b6063bed36a78e8a04f10a5383f2881f4072a1be13e706a81ab097fb56ee571`,
  Last-Modified 2018-02-13T13:19:48Z).
- Splits (filings-year cohorts, scored rows / positives / positive rate):
  train 2010-2012 **106,990 / 92,668 / 0.866137**; eval 2013-2014 **97,274 / 81,699 /
  0.839885**; holdout 2015-2016 **115,172 / 99,385 / 0.862927**. Censored (unresolved
  at the snapshot, excluded and counted, never labelled): 21 / 172 / 809.

## Measured evidence (identical eval rows, after acceptance)

All figures on the same 97,274 eval rows; the holdout was never read and stays sealed.

| Reference / model | Eval AUC | Brier |
| --- | --- | --- |
| Full model (stock runner XGBoost, verbatim) | **0.6224** | 0.1318 |
| Mark-type rate (train-fitted, dominant simple reference) | 0.5650 | 0.1341 |
| Nice-class rate (train-fitted, required comparator) | 0.5421 | 0.1359 |
| Series-flag rate | 0.5213 | - |
| Owner-country rate | 0.5191 | - |
| Category rate | 0.5031 | - |
| Calendar-month rate (calendar control) | 0.4972 | - |
| Majority-class accuracy on this split | 0.8399 | - |

Persistence: **not applicable** - every scored row is a new application; no prior
observation of the same entity exists at the filing instant. Recorded, not skipped.

Paired differences (row-level bootstrap, 150 resamples, job `ipo-tm-diag-002`, the
fixed resampler with the alignment invariant; the precursor `ipo-tm-diag-001`
intervals were INVALID - positional-weight defect, preserved as the defect they were -
and its point figures are unchanged by the fix):

- model - mark_type_rate: **+0.0574**, CI95 [0.0533, 0.0605] - excludes zero
- model - nice_class_rate: **+0.0803**, CI95 [0.0748, 0.0862] - excludes zero
- model - calendar_month_rate: **+0.1252**, CI95 [0.1174, 0.1315] - excludes zero

Per filing year the model's advantage holds in both eval years: 2013 (46,293 rows)
0.6233 vs mark_type 0.5639; 2014 (50,981 rows) 0.6216 vs 0.5660; prevalence is flat
across the two years (0.8409 -> 0.8390), so the advantage is not a prevalence artefact.
A pandas second implementation in the diagnostic job replicated all 10 recorded
reference figures exactly.

**Interpretation.** Demonstrated headroom over every measured simple reference under
this protocol. The scope is explicit: row-level intervals only (no dependence cluster
is measurable from the shipped columns - `tm_number` is unique per row), one model
configuration with no re-tuning, and two filing years of eval coverage. This is an
observed result, not a bound on other models and not an operational-validity claim.

## What is not claimed

- No point-in-time or historical availability claim: one frozen vintage exists, none
  is reconstructed, and training-label timing stays unverified.
- No claim that beating these rate references proves a causal signal; other simple
  rules were not exhaustively enumerated.
- No agent/harness comparison was run; the runner-contract baseline says nothing
  about how a coding agent would score on the task.
- The labelled holdout ships as an asset but stays outside evaluated agents'
  workspaces; no job in this record has read it.
- The derived dataset is not an Intellectual Property Office product.

## Provenance and licence

Source terms preserved (OGL-UK-3.0, attribution required); our code MIT; our
compilation contribution CC0-1.0 - see `LICENSE.md`. Register fidelity was
cross-checked in-build against the IPO's own facts-and-figures workbook (national UK
applications 2012-2016 agree within 0.12% every year) and against three named marks
at the file head and tail.
