# Week 1 Project Scope — FireSmoke Video Robustness Evaluator

## Project name

FireSmoke Video Robustness Evaluator

## Domain

Computer vision and AI engineering — offline evaluation of video-based fire and smoke detectors.

## Problem statement

Video fire and smoke detectors may behave differently across scene conditions, including fire size and distance, background activity and environmental variation. This project proposes a small offline tool that evaluates a baseline detector on a limited set of public research clips, summarizes frame-level and event-level results, and helps a student inspect false detections and missed events.

The project is related to the author's smoke and fire detection FYP. The first version focuses on video only; integrating sensors is a possible future extension and is not part of this project scope.

## Evidence of the problem

1. A 2024 video-detection literature review surveyed 153 papers and 17 public datasets and organized scenarios by factors including fire size and background activity. This supports careful scenario-based testing; it does not establish the performance of any particular detector. Source: [Fire and smoke detection from videos: a literature review under a novel taxonomy](https://www.sciencedirect.com/science/article/pii/S0957417424016506).
2. Public research data vary in scene coverage and data form. For example, the [Boreal Forest Fire dataset](https://research.aalto.fi/en/datasets/boreal-forest-fire-uav-collected-wildfire-detection-and-smoke-seg/) contains annotated video and smoke imagery from prescribed burns, while the [FireAndSmoke dataset paper](https://www.mdpi.com/1424-8220/24/17/5597) describes annotated fire/smoke imagery drawn from video sequences and challenging negative cases.

These sources support the need to document dataset scope and evaluate conditions separately. They are not evidence that a prototype is ready for real fire-safety use.

## Initial related work / existing approaches

1. **Fire and smoke detection from videos: a literature review under a novel taxonomy.** Reviews methods and 17 public datasets; distinguishes scenario factors such as fire size and background activity. [Source](https://www.sciencedirect.com/science/article/pii/S0957417424016506). Limitation: it is an overview, not an offline evaluation workflow for inspecting a student's detector errors.
2. **Boreal Forest Fire video dataset.** Public research resource with annotated smoke videos/images from prescribed burns at four Finnish locations. [Source](https://research.aalto.fi/en/datasets/boreal-forest-fire-uav-collected-wildfire-detection-and-smoke-seg/). Limitation: its wildland smoke setting does not cover indoor fires or all camera conditions.
3. **FireAndSmoke open dataset and detector comparison.** Provides more than 22,000 annotated images from video sequences and static images, with difficult negative cases. [Source](https://www.mdpi.com/1424-8220/24/17/5597). Limitation: image-level benchmark results do not by themselves measure temporal event detection or detection delay.
4. **NIST physical smoke-alarm performance studies.** Provide controlled evidence about physical alarm response. [Source](https://www.nist.gov/publications/performance-new-and-aged-residential-fire-smoke-detectors). Limitation: they are not tests of computer-vision systems and cannot be used to certify a video model.

## Engineering gap / limitation

Video model scores can hide where a detector fails over time or across different scenes. This project will build a small offline pipeline for temporal aggregation and error inspection, and report results alongside dataset coverage and limitations. It will not infer safety performance from a single benchmark score.

## Initial project objectives

- **O1:** To select a small set of publicly accessible and appropriately licensed video or video-derived sources, and record their labels, capture conditions, licensing and limitations.
- **O2:** To implement an offline evaluation workflow that samples prerecorded clips, runs a baseline detector, aggregates detections over time and presents errors for inspection.
- **O3:** To evaluate event recall, false alarms, missed events, detection delay and class-wise precision/recall where labels support them, including comparisons across available scene conditions.

## Target users

Student researchers working on video-based computer-vision fire and smoke detection, including the author of the related FYP.

## Public data sources

1. **Boreal Forest Fire dataset** — Aalto University research portal: https://research.aalto.fi/en/datasets/boreal-forest-fire-uav-collected-wildfire-detection-and-smoke-seg/
2. **Fire and smoke detection from videos: a literature review under a novel taxonomy** — ScienceDirect: https://www.sciencedirect.com/science/article/pii/S0957417424016506
3. **A Comparative Performance Evaluation of YOLO-Type Detectors on a New Open Fire and Smoke Dataset** — open-access article: https://www.mdpi.com/1424-8220/24/17/5597
4. **NIST smoke-alarm performance report** — safety context only, not computer-vision validation: https://www.nist.gov/publications/performance-new-and-aged-residential-fire-smoke-detectors

Verify current access terms, licenses and coverage before implementation. Do not publish restricted videos or unlicensed data.

## Similar to EduAdvisor

Both systems use a defined evidence base, make limitations visible and evaluate outputs against manually reviewed references.

## Different from EduAdvisor

This project evaluates fire/smoke detector behaviour over video rather than answering questions from academic documents. It is intended for offline student research only.

## Responsible AI risks

1. **Missed event:** A user could over-trust a model that misses smoke or fire in a benchmark. Mitigation: report missed events and dataset coverage; state that the tool is not for operational safety use.
2. **False detection:** Clouds, steam, lights or other difficult negatives may trigger a false prediction. Mitigation: include hard negatives and report false alarms separately from recall.
3. **Dataset bias and domain shift:** Results on prescribed wildland burns may not transfer to indoor or urban scenes, other cameras, or different lighting. Mitigation: stratify results by available conditions and state untested conditions.
4. **Privacy and data rights:** Video may show identifiable people or restricted locations. Mitigation: use only licensed public research material and avoid personal CCTV.
5. **Sensor-fusion overclaim:** Future sensor integration may appear to improve safety without adequate validation. Mitigation: keep sensors out of the current project and treat any later fusion as a separate research question.

## Initial evaluation plan

Use video-level or source-event-level train/test partitions where possible to avoid leakage from near-duplicate neighboring frames. Report event recall, false alarms per evaluated duration, missed events, detection delay, and class-wise precision/recall when labels permit. Compare frame-wise outputs with temporal aggregation and inspect representative errors. Treat results as offline research evidence, not product certification.

## Week 1 status

This is an initial scope, not a working application. The group's Part A observation sheet and approved conceptual system-map PNG are the other two Week 1 files in `docs/`. Three supporting screenshots are stored under `docs/group_evidence/evidence/`.

