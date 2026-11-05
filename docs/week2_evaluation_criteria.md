# Week 2 Evaluation Criteria

## Purpose

This table selects five high-priority requirements from the Week 2 specification for later testing. It evaluates whether the offline tool produces complete, trustworthy research evidence; it does not set a minimum fire-detector accuracy or certify the detector for safety use.

The system is a computer-vision evaluation workflow, so the relevant capability buckets are **Domain-specific capability** and **Cost and latency**. Generation and instruction-following are not part of this project's current scope.

## Evaluation criteria table

| Criterion ID | Requirement and objective | Evaluation criterion | Capability bucket | Measurement method | Metric | Usefulness threshold | Expected user outcome |
|---|---|---|---|---|---|---|---|
| EC-01 | NFR-01; O3 | Correctness of frame-level and event-level metric calculations. | Domain-specific capability | Prepare at least 10 controlled cases with hand-calculated labels, including true positives, false positives, missed events, and event timing. Compare the tool outputs with the independent calculations. | Exact match for count values; absolute difference between tool and reference ratios. | All 10 cases have exact counts and ratio differences no greater than 0.001. | A student can trust that reported scores reflect the reference labels rather than a calculation error. |
| EC-02 | FR-03; O2, O3 | Coverage and correctness of supported metrics on a manually reviewed clip subset. | Domain-specific capability | Run the baseline on a small subset with temporal annotations where available. Independently check class-wise precision/recall, event recall, false alarms per evaluated duration, missed-event count, and detection delay when supported. Also include an input without temporal labels to check metric availability handling. | Share of eligible metrics reported correctly; count agreement; ratio difference; detection-delay difference. | 100% of metrics supported by the annotations are present; unsupported metrics are marked unavailable. Counts match exactly, ratios differ by no more than 0.001, and detection delay is within one sampling interval of the independent reference. | The student can compare detector behaviour over time and knows when the data cannot support a metric. |
| EC-03 | RAI-02; O3 | Visibility and traceability of missed reference events. | Domain-specific capability | Use a labelled fixture or manually reviewed subset containing detected and missed events. Reconcile every reference event with the event-level output and inspect clip/time links for misses. | Percentage of reference events with a correct matched/missed status and traceable clip/time. | 100% of reference events are accounted for; every known miss is shown with its clip and timestamp. | A strong aggregate score cannot conceal specific fire/smoke events the detector missed. |
| EC-04 | FR-01, RAI-04, RAI-05; O1, O3 | Integrity of dataset provenance, scene coverage, and unknown-label reporting. | Domain-specific capability | Reconcile source/access fields in the evaluation manifest with the coverage summary, including clips whose scene-condition fields are missing. Check every subgroup result against the manifest labels. | Percentage of selected clips with source/access information; percentage of processed clips represented in coverage; unknown labels retained; unsupported subgroup claims. | 100% of selected clips have source/access fields and 100% of processed clips appear in coverage; missing labels remain marked unknown; zero subgroup results are reported without supporting labels. | The student can check that the data are documented, see which conditions the results cover, and avoid generalizing to untested scenes. |
| EC-05 | NFR-04; O2 | Resource demand for offline evaluation on the declared student environment. | Cost and latency | Process a 10-minute test input at the selected sampling setting on the declared computer. Record peak evaluator memory and elapsed processing time, including the loaded baseline detector. | Peak host memory in GB; elapsed minutes per 10 minutes of video. | Peak host memory is no greater than 2 GB. Record processing time for feasibility; no real-time latency target applies to this offline research tool. | The student can judge whether the workflow fits the available computer and estimate the time needed for a batch run. |

## Test data and reporting notes

- Use only clips with documented source and access terms. Do not add raw video to the public repository unless redistribution is explicitly permitted.
- Keep clip-level identifiers and reference annotations with the test results so each metric can be checked.
- Record the detector version, configuration, sampling setting, data subset, and computer used for every measurement.
- Report results only for metrics supported by the reference annotations; do not substitute estimates for missing labels.
- Treat thresholds as initial coursework usefulness targets, not external safety benchmarks. If a target must change after selecting the dataset or environment, record the reason before reporting results.

## Part B reflection

**EC-01, metric calculation correctness, is the most important criterion for usefulness.** The project's main job is to help a student interpret a detector's performance. If event counts, missed events, or rates are calculated incorrectly, the student could draw the wrong conclusion even when the report looks complete. Correct calculations are therefore the foundation for the coverage and error-inspection criteria as well.

## Part B completion check

- [x] Selected five important, testable requirements from Part A.
- [x] Linked each criterion to a requirement and project objective.
- [x] Classified each criterion into a relevant capability bucket.
- [x] Defined measurement methods, metrics, usefulness thresholds, and expected user outcomes.
- [x] Explained why the most important criterion matters to the intended user.

