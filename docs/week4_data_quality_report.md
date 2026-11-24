# Data Quality, Bias, and Responsible Data Report — FireSmoke Video Robustness Evaluator

The audit covers 32 sampled frames from two prerecorded Boreal Forest Fire Subset B clips. The input is Week 3 pipeline `week3-v1`, generated on 11 November 2026 UTC. The audit checked the frame index and both ZIPs against the [run manifest](../data/pipeline_v1/video/pipeline_manifest.json). Results are in the [executed notebook](../notebooks/week4_quality.ipynb).

## 1. Data Quality Scorecard

Ratings apply to the audited subset. A 5 means the stated check is fully met within that scope. N.V. means Not Verified.

| Dimension | Rating (1–5 / N.V.) | Evidence | Action or limitation |
| --- | --- | --- | --- |
| Completeness | 5 | All 32 indexed JPEGs are present and all archive image members are indexed. Both clips have labels. | This rates the integrity of the delivered frame index. Negative clips and event annotations are missing from the evaluation scope. |
| Consistency | 5 | All rows match the run manifest clip labels, archive identities, timestamps and decoded JPEG dimensions. | Repeat the checks after changing the source, annotations or sampling configuration. |
| Accuracy | N.V. | The two inspected stills show visible plumes. The source provides clip-level labels, and its metadata flags errors in some archived labels. | Independently verify reference labels and review frame or event annotations for the intended metrics. |
| Timeliness | N.V. | The inventory records the 28 February 2025 release and the run records its generation time. | Check for newer annotations or errata before using the clips for scoring. |
| Uniqueness | 5 for exact checks | All clip/frame keys and decoded RGB hashes are unique across the 32 records. | Visual near-duplicate status is N.V. Keep frames from the same clip together when splitting data. |

## 2. Validation and Data-Defect Findings

| Rule | Observable condition and example | Failures | Decision on failure |
| --- | --- | --- | --- |
| Clip label agreement | Each clip is in the run manifest and the index label agrees. Both selected clips carry `has_smoke=true`. | 0/32 (0%) | Hold and reconcile clip and label sources. |
| Archive member presence | Each `prepared_image` path resolves to an archived member. | 0/32 (0%) | Hold and restore the missing member or rebuild the index. |
| Clip and frame lineage | The JPEG filename encodes the indexed timestamp and frame number and is in its own clip ZIP. For example, `evo_10_t002002_f000096.jpg` corresponds to frame 96 at 2,002 ms. | 0/32 (0%) | Hold and reconcile the image reference with the Week 3 export. |
| Timestamp alignment | The frame index is in bounds and the timestamp is in the clip duration and within 1 ms of index divided by FPS. The tolerance allows integer-millisecond rounding. | 0/32 (0%) | Hold and recheck FPS and timestamp conversion. |
| JPEG readability and dimensions | The JPEG decodes, matches the indexed dimensions and stays within the configured 640-pixel long edge. Both preview JPEGs are 640 × 338. | 0/32 (0%) | Hold and regenerate the affected image or correct its metadata. |
| Unique clip/frame key | Each clip and source frame index pair occurs once. | 0/32 (0%) | Hold and reconcile repeated or conflicting index rows. |

The six Pandera rules passed. The additional archive check found 0/32 unindexed images. No missing indexed members, unreadable JPEGs, or label inconsistencies with the run manifest were found. Subtle visual noise, official annotation freshness, and frame-level label accuracy remain unverified. The [failure CSV](week4_validation_failures.csv) contains headers and zero failure rows.

The notebook records three review warnings. Positive-only coverage is acceptable for a preparation check and requires reviewed negatives for false-alarm evaluation. Missing event intervals leave event timing unresolved, so retain the clip labels and defer temporal scoring. Neighboring frames from the same clip may be visually similar, so retain their provenance and keep related observations in the same data split.

## 3. Duplicate and Anomaly Findings

Exact SHA-256 hashes of decoded RGB pixels and dimensions yielded zero exact duplicate observations. The input and reviewed counts are both 32. There were no conflicting labels within exact-image groups. The [duplicate CSV](week4_duplicate_review.csv) contains headers and zero duplicate rows. Each retained record preserves clip, frame index, timestamp, label and archive member path. Frames 0 and 96 of `evo_10.mp4`, separated by 2,002 ms, illustrate temporal dependence. Their visual similarity and the independence of the two source clips at incident level remain unverified.

## 4. Representation, Coverage, and Bias Assessment

| Required evidence | Actual evidence | Interpretation |
| --- | --- | --- |
| Reviewed positive clips and negative clips, including smoke-like conditions | Two smoke-positive clips and zero negative clips | Genuine gap for evaluating false alarms on negative clips. |
| Reviewed event start times or intervals | Clip-level labels only | Genuine gap for event recall, missed events and detection delay. |
| Scene and capture-condition labels | Prescribed-burn UAV footage at dataset level. Per-clip condition and incident tags are unknown. | Coverage by individual condition and incident is undetermined. |
| Traceable distribution by clip | `evo_10.mp4` has 15 frames (46.88%) and `evo_11.mp4` has 17 (53.12%). | Difference in sample size. Class counts use clip-level source labels. [Coverage chart](week4_coverage_by_source.png) |

| Potential bias | Origin and propagation | Intervention and Topic 8 test |
| --- | --- | --- |
| Historical | The fixed collection of Finnish prescribed burns constrains prepared examples. Clip selection and inference use this capture context, which could lead readers to overgeneralize results to newer cameras or uncontrolled fires. | Record capture context and compare held-out sources and periods when metadata supports comparison. |
| Representation | Selecting two positive clips leaves negatives absent after preparation. Inference and scoring cannot expose false triggers on fog or similar confounders in this subset, which can give readers an incomplete picture. | Add independently reviewed negatives and difficult confounders. Measure false alarms on held-out negatives with documented durations. |
| Measurement | A clip label and two-second sampling leave frame-level truth and short events unresolved. A temporal matcher could miscount events or distort delay if these labels were used as event references. | Preserve label granularity. With reviewed intervals, compare event recall and delay at multiple sampling intervals. |

Detector performance and fairness outcome evidence remain pending Topic 8 evaluation.

## 5. Privacy, Consent, Rights/Access, and Data Governance

| Review | Status and evidence | Responsibility |
| --- | --- | --- |
| Ownership, source and version | Traceable to Boreal Forest Fire Subset B, the 2025-02-28 release and the recorded run hashes. Source creators are credited through the publisher citation in the inventory. [Week 3 source inventory](week3_data_inventory.md) | The student maintainer records the dataset release, label revision and input hashes for each import. |
| Rights and access | The inventory records CC BY 4.0 for the dataset. Derived frames were sampled, resized and re-encoded. | Preserve the publisher citation, licence and transformation notes. Verify asset terms for each new import and hold files with unresolved rights before publication. [Week 3 source inventory](week3_data_inventory.md) |
| Privacy | The two inspected stills show aerial forest scenes. Personal or sensitive content across the full retained set is N.V. | Review all candidate exports and metadata before expanding publication. Quarantine unnecessary identifiers and sensitive details. |
| Consent and collection | No new collection took place. Consent for any identifiable subject in source footage is N.V. | Hold any affected footage until the reuse basis is established. |
| Retention, updates and removal | Version and hashes are recorded. | The student maintainer checks for revisions before scoring, records any replacement, reruns validation, retires superseded versions from active use and applies removal obligations under the source terms and course requirements. |

## 6. Mitigation Plan and Residual Risk

| Issue | Evidence | Action | Owner and timing | Residual risk |
| --- | --- | --- | --- | --- |
| Positive-only coverage | 2 positive and 0 negative clips | Add reviewed negatives and smoke-like confounders. Record evaluated duration. | Student maintainer, before Topic 8 scoring | New clips may still underrepresent some confounders. |
| Unverified annotations and missing timing | Two visual samples and no event intervals | Check current source metadata and independently review clip labels and event times. Mark unsupported metrics unavailable. | Student maintainer, before benchmark scoring | Onset remains subject to annotation uncertainty. |
| Correlated frames and unknown conditions | Sequential samples and missing per-clip condition and incident tags | Split by clip, group by incident where documented, and record unknown scene conditions explicitly. | Student maintainer, before partitioning or subgroup evaluation | Without incident IDs, related clips may still occur in different splits. |
| Rights and privacy across expanded data | Recorded licence and limited manual inspection | Retain attribution, check each new source and review files and metadata before publication. | Student maintainer, at each ingestion and release | Identifying details can be missed in visual review. |

## 7. Impact on Topic 2 Requirements and Evaluation Criteria

- **FR-01, EC-04, RAI-04 and RAI-05.** Both audited clips are traceable and both appear in the coverage summary. The later system must account for 100% of selected and processed clips, preserve unknown condition labels and avoid unsupported subgroup results.
- **FR-02.** The handoff supports timestamped offline processing. Timestamped predictions still require a detector run.
- **FR-03 and EC-02.** Report 100% of metrics supported by the reference annotations and mark unsupported metrics unavailable. For supported tests, compare exact counts, ratios within 0.001 and delay within one sampling interval. This subset lacks the negative and temporal evidence required for the corresponding benchmark tests.
- **FR-04, RAI-02 and EC-03.** The target is traceability for 100% of reference events, including clip and timestamp for every miss. Event annotations are needed to test this criterion.
- **EC-01 and EC-05.** Controlled metric fixtures and the 10-minute memory benchmark remain separate evaluation tests. [Requirements](week2_requirements.md) · [Evaluation criteria](week2_evaluation_criteria.md)

## 8. Topic 4 Quality Gate Decision

**GO WITH CONDITIONS** for offline pipeline integration and preparation checks. All structural checks passed and the reviewed corpus preserves its provenance. Before benchmark scoring, the student maintainer must verify source annotations, add reviewed negatives and event timing for the intended metrics, and keep related frames together in data splits. Until that evidence is available, mark unsupported metrics and missing condition labels explicitly. Keep the results scoped to these clips and documented capture conditions.
