Attempting to perform the InitializeDefaultDrives operation on the 'FileSystem' provider failed.
# FireSmoke Video Robustness Evaluator

> A proposed offline evaluation tool for reviewing how video-based fire and smoke detectors behave across public research clips and difficult scenes.

**Status:** Week 1 individual project scope. This repository does not contain a working detector yet.

## Problem

Video fire and smoke detectors can behave differently when the visible fire is small or distant, when the background is moving, or when scene conditions vary. A 2024 literature review surveyed 153 papers and 17 public datasets, and organized video-detection scenarios by fire size and background activity. This supports careful, scenario-based evaluation, while not proving that a particular model is reliable.

## Proposed project

Build a small offline tool that runs a baseline video detector on a limited, publicly documented evaluation set. It will summarize frame-level and event-level results, highlight false detections and missed events, and compare performance across available scene labels. The first version will focus on video only. Sensor fusion is a possible future extension, outside the current scope.

The tool is for student research and model evaluation. It will not connect to live cameras, trigger alarms, advise people during emergencies, or claim that a model is suitable to protect life or property. It does not replace certified smoke alarms or official emergency procedures.

## Initial related work / existing approaches

1. **Fire and smoke detection from videos: a literature review under a novel taxonomy** — surveys video-detection methods and datasets, and groups scenarios by fire size and background activity. Limitation: it is a review and dataset analysis, not a small student-facing evaluation tool.
2. **Boreal Forest Fire video dataset** — offers annotated video and images of smoke from prescribed burns. Limitation: the setting is narrow (wildland smoke from four Finnish burn events), so it cannot represent all indoor or urban fire scenes.
3. **FireAndSmoke open dataset** — contains annotated fire/smoke imagery drawn from video sequences and difficult negative cases. Limitation: its main released benchmark is image-based, so it does not alone measure temporal event behaviour.
4. **NIST smoke-alarm performance research** — documents controlled testing of physical smoke alarms. Limitation: those results do not validate camera-based AI detectors; this project will use it only as safety context.

## Engineering gap

Published video datasets and evaluation papers provide useful test material, but a small FYP-aligned workflow can make temporal behaviour easier to inspect: how long an event takes to detect, whether detections persist across frames, and where false alarms or misses occur. The project will test a limited offline evaluation pipeline and clearly report dataset coverage and uncertainty.

## Objectives

- **O1:** Select a small set of publicly accessible, appropriately licensed fire/smoke video or video-derived sources and document their labels, conditions and limits.
- **O2:** Build an offline evaluation workflow that samples prerecorded clips, runs a baseline video detector, aggregates detections over time, and presents errors with source clips or frames.
- **O3:** Evaluate event recall, false alarms, missed events, detection delay, and performance across available scene conditions; compare frame-only scoring with temporal aggregation where feasible.

## Target users

Student researchers developing or evaluating video-based fire and smoke detectors, including the author of the related FYP.

## Public sources to investigate

- [Boreal Forest Fire dataset — Aalto University research portal](https://research.aalto.fi/en/datasets/boreal-forest-fire-uav-collected-wildfire-detection-and-smoke-seg/)
- [Fire and smoke detection from videos: a literature review under a novel taxonomy](https://www.sciencedirect.com/science/article/pii/S0957417424016506)
- [A Comparative Performance Evaluation of YOLO-Type Detectors on a New Open Fire and Smoke Dataset](https://www.mdpi.com/1424-8220/24/17/5597)
- [NIST: Performance of New and Aged Residential Fire Smoke Detectors](https://www.nist.gov/publications/performance-new-and-aged-residential-fire-smoke-detectors)

Before selecting a dataset for implementation, verify its access terms, research-use license, annotation format, and whether its scenes match the intended evaluation. Do not commit restricted video or third-party data without permission.

## Similar to EduAdvisor

Both projects use a defined evidence base, make their limits visible, and evaluate output quality against a manually reviewed reference set.

## Different from EduAdvisor

This project evaluates video detections and temporal behaviour rather than answering questions over academic documents. It is an offline research tool, not a deployed safety system.

## Responsible AI and safety risks

- **Missed fire/smoke event:** users could over-trust a high benchmark score. Mitigation: report misses and dataset limits; make no real-world safety claim.
- **False detection:** non-fire visuals may be flagged in the research report. Mitigation: measure false alarms and inspect difficult negative examples.
- **Dataset coverage bias:** results may not transfer across indoor, outdoor, day/night, camera or sensor conditions. Mitigation: stratify by available conditions and state missing coverage.
- **Privacy and licensing:** surveillance video may contain identifiable people or restricted footage. Mitigation: use only permitted public research data; do not collect private CCTV.
- **Future sensor fusion:** sensors may be considered later, but are excluded from this version so their reliability and integration risks are not implied.

## Initial evaluation plan

Use a manually checked, video-level split to avoid placing near-identical neighboring frames in both training and evaluation sets. Report event recall, false alarms per evaluated duration, missed events, detection delay, and class-wise precision/recall where labels support them. Include error examples and document the dataset's scene coverage. Do not interpret these research metrics as certification or operational readiness.

## Week 1 repository checklist

- [x] Individual project name and initial scope drafted.
- [ ] Create the individual GitHub repository and publish this README and `docs/week1_project_scope.md`.
- [x] Copy the group's Part A observation sheet, approved Part B system-map PNG and three supporting screenshots into `docs/group_evidence/`.
- [ ] Submit the individual repository URL on the course portal.

## Planned structure

```text
README.md
docs/
  week1_project_scope.md
  group_evidence/
data/
src/
notebooks/
evaluation/
prompts/
```

