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

# Data dictionary - ipo-trademark-registration-outcome

One row per distinct domestic UK trade mark application filed 2010-01-01..2016-12-31
(first occurrence per stripped mark-number string; 16,909 duplicate rows removed).
319,436 scored rows: train 106,990 (filings 2010-2012) / eval 97,274 (2013-2014) /
holdout 115,172 (2015-2016). Source: the frozen IPO `opendatadomestic.zip` snapshot of
2018-02-13 (UTF-16LE, pipe-separated, 62 documented columns).

## Identifier

| Column | Type | Meaning |
| --- | --- | --- |
| `tm_number` | string (id) | The mark's application number, e.g. `UK00003041794`. Assigned at filing. Unique per row (declared as the entity key; the gate verified no duplicates and no cross-split reuse). |

## Features (filing-time attributes of the application itself)

| Column | Type | Meaning |
| --- | --- | --- |
| `category` | string | Register category, normalised (3 levels measured). |
| `mark_type` | string | Type of mark (5 levels measured: Word, Device, ...). The strongest single simple reference (train-fitted positive rate per mark type scores eval AUC 0.5650). |
| `series_flag` | integer (0/1) | 1 if the application is part of a series of marks presented and accepted as a series. |
| `series_marks_count` | integer | Number of marks in the series (1 for a non-series application). |
| `nice_class_count` | integer | Number of distinct Nice classes the application covers (derived from the class columns). |
| `class1`..`class45` | integer (0/1) | Whether the application covers Nice class N (the international goods-and-services classification; class 1 = chemicals, 45 = legal services). 45 binary columns. |
| `owner_country` | string | Owner's country as recorded in the snapshot (133 levels measured). Snapshot-vintage: ownership can be assigned after filing. |
| `owner_region` | string | Owner's UK region (empty for non-UK owners). Snapshot-vintage. |
| `mark_text` | string | The mark as filed. High-cardinality (declared, not a leak: the gate scored it as an identifier, not a memorising feature). 94 rows in the source carry embedded pipe characters inside the mark text; these are recovered by right-anchored parsing (asserted in-build, counted). |
| `owner_postcode_area` | string | The owner's UK postcode area (the leading letters), empty for non-UK addresses. High-cardinality, declared. Snapshot-vintage. |
| `filed_date` | date (YYYY-MM-DD) | The application's filing date - the event time of this task. Carried as a feature and used for cohort membership. |

## Target

| Column | Type | Meaning |
| --- | --- | --- |
| `registered` | integer (0/1) | 1 iff the snapshot records a non-empty Registered date (the mark reached registration, whatever its current post-registration status). 0 iff the Registered date is empty AND the status is `Refused` or `Withdrawn`. Applications whose outcome was still unresolved at the snapshot (examination, opposition, appeal, and the undocumented `Deleted` / `Deemed Not Filed` / blank statuses) are **censored** - excluded from scoring and counted per cohort (21 / 172 / 809), never labelled negative. |

## Columns deliberately not shipped

| Source column | Reason |
| --- | --- |
| `Status`, `Published`, `Registered`, `Expired`, `Renewal Due Date` | The label source and its post-filing companions (publication, expiry and renewal all happen after the outcome resolves). `quality.json` records them as `label_source_columns` / `post_hoc_columns` and the gate asserts their absence from every shipped CSV. |
| `Hyperlink` | An Excel formula wrapping the mark number - pure duplication of the id. |
| `Name` (owner name) | Free-text owner identity, excluded by design: removing it also removes the trivial same-owner outcome-rate join a solver could otherwise build. |

Everything else in the 62-column source (owner address parts, representative details,
international-registration fields) is either covered by the shipped owner fields or not
used, per `code/build.py`'s explicit column plan.
