# Week 2 Requirements Specification

## Project

**FireSmoke Video Robustness Evaluator**

This specification refines the Week 1 project scope into a testable, offline research workflow. The numeric thresholds below are initial coursework prototype targets. They must be checked against the selected dataset, baseline detector, and evaluation environment; they are not fire-safety standards or evidence of operational readiness.

## Section 0 — Project Framing and Application Scope

### Problem evidence

1. A 2024 review of video fire and smoke detection surveyed 153 papers and 17 public datasets, and identified scenario factors such as fire size and background activity. This supports evaluating performance across available scene conditions; it does not establish that any particular detector is reliable. [Review](https://www.sciencedirect.com/science/article/pii/S0957417424016506).
2. The Aalto Boreal Forest Fire resource provides annotated video and imagery from prescribed burns. It is useful research data, but represents a specific wildland setting rather than the full range of indoor, urban, camera, and lighting conditions. [Dataset description](https://research.aalto.fi/en/datasets/boreal-forest-fire-uav-collected-wildfire-detection-and-smoke-seg/).

### Existing approaches

1. **Video fire and smoke detection literature review:** summarizes methods and datasets and organizes evaluation scenarios. It does not provide a small student workflow for tracing frame and event errors in a chosen baseline detector. [Source](https://www.sciencedirect.com/science/article/pii/S0957417424016506).
2. **Boreal Forest Fire dataset:** supplies annotated smoke video and imagery from prescribed burns. Its wildland coverage does not represent every indoor or urban setting. [Source](https://research.aalto.fi/en/datasets/boreal-forest-fire-uav-collected-wildfire-detection-and-smoke-seg/).
3. **FireAndSmoke dataset and detector comparison:** provides annotated fire/smoke images, including difficult negative examples. Image-level results alone do not measure temporal event detection or detection delay. [Source](https://www.mdpi.com/1424-8220/24/17/5597).

### Project / engineering gap

An aggregate detector score can hide when an event was missed, how long detection took, or which scenes caused false alarms. This project will provide a small offline workflow that compares detector outputs with reviewed labels over time, summarizes errors and available scene conditions, and reports dataset limits. It will not claim research novelty or safety performance from a limited benchmark.

### Project objectives

- **O1 — Data and scope:** Select a small set of publicly accessible, permitted fire/smoke video or video-derived data and document its labels, scene coverage, source, access terms, and limitations.
- **O2 — Evaluation workflow:** Develop an offline workflow that processes prerecorded clips with a baseline detector, records timestamped predictions, aggregates predictions into events where labels permit, and makes errors inspectable.
- **O3 — Evaluation:** Evaluate event recall, false alarms, missed events, detection delay, and class-wise precision/recall where annotations permit, including comparisons across available scene conditions.

### Intended users

Student researchers who are developing or evaluating video-based fire and smoke detectors, including the author of the related smoke and fire detection FYP.

### Supported tasks

- Check that an evaluation clip list and its annotation metadata are usable.
- Run a baseline detector on a selected set of prerecorded clips.
- Compare timestamped predictions with reviewed reference labels.
- Summarize frame-level and event-level results where the annotations support them.
- Inspect false detections, missed events, and results for documented scene conditions.
- Export a research summary with the evaluation setup and data limitations.

### Information / knowledge domain

Prerecorded fire, smoke, and difficult-negative video clips; associated reference annotations; clip-level source and access information; available scene/capture-condition labels; and detector predictions. Sensor data are not part of this version.

### Operating context

The application is an offline student research tool that processes saved, permitted research clips on a declared evaluation computer. Results are for model analysis and coursework. It is not a live camera service.

### Important constraints

- Public datasets may differ in license, annotation format, scene coverage, and event definitions; access terms and intended use must be checked before use.
- Some sources are image-based or lack temporal annotations, so not every metric will be available for every source.
- The project is a limited student prototype with finite time and compute.
- Raw video must not be added to the public repository unless its terms explicitly permit redistribution.
- Sensor fusion and deployment on live cameras are outside the current version.

### Explicit out-of-scope tasks

The system will not monitor live cameras, issue alarms or emergency instructions, dispatch responders, certify a detector, assess building compliance, or claim that a model can protect people or property. It will not use private CCTV or sensor fusion in this version.

### Evaluation focus

Can a reproducible offline workflow account for every selected clip, compare predictions with available reviewed annotations, calculate supported frame/event metrics correctly, and make missed events, false alarms, scene coverage, and limitations visible to a student researcher?

## Section 1 — Functional Requirements

| ID | Objective | Requirement | Verification method and pass condition |
|---|---|---|---|
| FR-01 | O1 | The system shall accept an evaluation manifest that identifies each clip, its source and access status, its labels or annotation reference, and any available scene-condition labels. It shall report missing or invalid required fields before evaluation. | Submit one complete manifest and fixtures with a missing clip ID, source/access field, or annotation reference. The complete manifest is accepted; each invalid fixture is rejected with the field identified. |
| FR-02 | O2 | The system shall process only the prerecorded clips selected in the evaluation manifest and produce timestamped fire/smoke detector predictions under a recorded evaluation configuration. It shall not claim to process a live camera stream. | Run a small clip set and check that each output prediction identifies its clip, timestamp, predicted class, and confidence where the detector provides one; confirm no live stream input is accepted. |
| FR-03 | O2, O3 | The system shall compare predictions with reviewed reference annotations and calculate supported frame-level and event-level measures, including class-wise precision/recall, event recall, false alarms per evaluated duration, missed-event count, and detection delay where the annotation format supports them. | Use a hand-labelled fixture with known matches, false positives, missed events, and event times. Required supported counts and metric values are produced; a metric without the necessary labels is marked unavailable rather than estimated. |
| FR-04 | O2, O3 | The system shall make representative false detections and missed events traceable to a clip identifier and timestamp, and shall summarize results by documented scene conditions when those labels are available. | Use a fixture containing a known false detection, miss, and two scene labels. Each error points to the correct clip/time; each labelled group appears in the summary. |
| FR-05 | O2, O3 | The system shall produce a human-readable evaluation summary that identifies the data subset, detector/configuration, supported metrics, coverage, and known limitations. | Review a generated report against its run manifest and results. All listed setup fields and supported metrics are present, and unsupported metrics are explicitly identified. |

## Section 2 — Non-Functional Requirements

The following thresholds are prototype targets for the declared test environment and selected evaluation subset. They are intended to make later testing concrete, not to define safe operational performance.

| ID | Quality dimension | Objective | Requirement | Measurement method | Metric and initial threshold |
|---|---|---|---|---|---|
| NFR-01 | Correctness | O3 | The evaluation calculations shall agree with independently checked reference results on controlled fixtures. | Compare output counts and metrics with hand-calculated fixtures covering true positives, false positives, false negatives, and event timing. | Exact agreement for all confusion/event counts; absolute difference no greater than 0.001 for derived ratios on every fixture. |
| NFR-02 | Reliability | O2 | The workflow shall account for every manifest clip with either a completed result or an explicit failure record; it shall not silently omit a clip. | Run a fixture containing valid, unreadable, and annotation-incomplete entries and reconcile the manifest with the run summary. | 100% of manifest entries have a terminal status; zero silent omissions. |
| NFR-03 | Reproducibility | O3 | With the same clips, detector version, configuration, and reference annotations, repeated runs shall produce reproducible evaluation outputs. | Run the same fixed fixture three times and compare outputs, excluding wall-clock timing. | Identical clip/event/confusion counts and metric differences no greater than 0.001. Record configuration and version information for each run. |
| NFR-04 | Resource use | O2 | The workflow shall process a clip incrementally rather than retaining every decoded frame in memory. | Process one 10-minute test clip at the selected sampling setting on the declared evaluation computer and record peak process memory. | Peak host memory no greater than 2 GB for the evaluator process, including the loaded baseline detector; record the computer and model used. |
| NFR-05 | Maintainability | O1, O2 | A new dataset that follows the documented manifest format shall be evaluable without changes to the core metric-calculation logic. | Add a second small conforming fixture dataset and run the same evaluation workflow. | Zero edits to core metric-calculation logic are needed; both fixture datasets produce the required outputs. |

## Section 3 — Responsible AI Requirements

| ID | Risk addressed | Requirement / control | Measurement method | Metric and acceptance threshold |
|---|---|---|---|---|
| RAI-01 | Over-trust or unsafe use | The system shall label every published evaluation summary as offline research only and state that it is not for live fire monitoring or life-safety decisions. It shall not trigger an alarm or issue emergency advice. | Inspect each report/export template and exercise its available output actions. | 100% of report types include the limitation statement; zero alarm, dispatch, or emergency-advice actions. |
| RAI-02 | Missed fire/smoke events hidden by aggregate scores | The system shall report false-negative/missed-event counts and event recall when event annotations support them, and shall make each reference event's matched or missed status traceable. | Reconcile a fixture containing known missed and detected events with its report. | 100% of annotated reference events are accounted for as matched or missed; event recall and missed-event count are shown when computable. |
| RAI-03 | False detections hidden by a high recall score | The system shall report false-positive/false-alarm counts separately from recall and normalize false alarms by evaluated duration when duration is known. | Reconcile predicted events against annotated non-event intervals in a fixture. | 100% of unmatched predicted events are counted; the report presents false alarms per evaluated hour when duration is available. |
| RAI-04 | Dataset bias and domain shift | The system shall report the evaluated clips and their available scene/capture-condition coverage. It shall mark missing condition labels as unknown and shall not imply performance for unrepresented conditions. | Compare the coverage summary with the manifest, including entries with missing scene labels. | 100% of processed clips appear in the coverage accounting; unknown labels remain explicit; no subgroup result is presented without a supporting label. |
| RAI-05 | Privacy and data rights | The system shall require source and access/license information for each selected clip and shall not publish raw video unless redistribution is explicitly permitted. | Audit the manifest and repository artifacts before publication. | 100% of selected clips have documented source/access status; zero raw clips lacking redistribution permission are published. |

### Part A reflection

**The dataset-bias requirement will be the hardest to measure and enforce.** The workflow can check that scene labels and unknown values are reported, but it cannot prove that a small public dataset represents all indoor, outdoor, lighting, camera, or weather conditions. The practical control is to preserve unknowns, report coverage beside the metrics, and avoid claims beyond the evaluated clips. Broader coverage would require additional suitable datasets and later validation.

## Part A completion check

- [x] Refined problem evidence, existing approaches, engineering gap, objectives, users, scope, constraints, and evaluation focus.
- [x] Wrote observable functional requirements.
- [x] Included at least four non-functional requirements with methods, metrics, and thresholds.
- [x] Converted relevant Responsible AI risks into measurable requirements.
- [x] Linked requirements to objectives and later evidence where applicable.

