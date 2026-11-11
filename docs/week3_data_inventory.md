# Week 3 Data Inventory — Part A

**Project:** FireSmoke Video Robustness Evaluator  
**Status:** Part A source selection and initial Colab inspection complete. Two sample videos and their annotation rows were inspected. This is ingestion/inspection evidence only; transformation and prepared-data export belong to Part B.

## Requirement-to-data mapping

| Topic 2 ID | Needed information | Selected source | Why it fits / constraints |
|---|---|---|---|
| FR-01, RAI-05 | Clip IDs, source/access status, smoke labels, and annotation references | Boreal Forest Fire dataset, Subset B, plus its video-ground-truth CSV | The official Aalto/Fairdata record describes video clips and binary smoke annotations and lists CC BY 4.0. The publisher warns some archived labels have errors; consult the video-label metadata. |
| FR-02 | Prerecorded clips and timestamped frames for an offline detector workflow | Subset B MP4 clips | The clips are prerecorded and suitable for frame sampling. They are 4K and large; Colab upload/storage and processing cost should be considered. Only two examples were used for this first inspection. |
| FR-03, FR-04, RAI-02 | Reference labels to compare with predictions and identify misses | Subset B clip-level smoke labels | The labels are per-video binary values. They support initial clip-level smoke/no-smoke checks but do not provide event intervals or event timestamps, so event recall and detection delay cannot be validated from these labels alone. |
| RAI-04 | Available scene/capture-condition coverage | Dataset metadata: four Finnish prescribed-burn locations | The dataset is wildland/UAV footage; it does not represent indoor, urban, fixed-camera, or all weather conditions. Report these limits and mark unavailable scene attributes as unknown. |

## Selected source record

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| S01 | Boreal Forest Fire dataset, Subset B; [official Etsin record](https://etsin.fairdata.fi/dataset/1dce1023-493a-4d63-a906-f2a44f831898); DOI: https://doi.org/10.23729/fd-72c6cf74-b8eb-3687-860d-bf93a1ab94c9 | 30-second 4K MP4 video clips with per-video smoke labels; candidate offline smoke-detection evaluation data | Released 2025-02-28; creators listed by Aalto/Fairdata | Open; CC BY 4.0 per official record. Preserve attribution. Do not commit raw videos to public GitHub by default. | Two inspected clips opened successfully. `evo_10.mp4`: 370,241,100 bytes; 4096×2160; 47.952 FPS; 1,419 frames; 29.592 s. `evo_11.mp4`: 405,706,410 bytes; 4096×2160; 47.952 FPS; 1,550 frames; 32.324 s. Each matched exactly one smoke label, `true`. |
| S01-LABELS | `Video-Ground-Truth/video-ground-truths.csv` | Semicolon-separated table joining clip filename to binary smoke label | Dataset release metadata; 288 records | Same dataset access terms; original file retained without modification | Parsed with `sep=';'`. Columns: `Filename`, `Has Smoke`; 288 rows; zero missing values; zero duplicate rows. The inspected `evo_10.mp4` and `evo_11.mp4` each matched one row with `Has Smoke=true`. The CSV delimiter was corrected in the notebook after the initial default comma parse returned one combined column. |

## Inspection limitations and preparation note

- These two examples demonstrate readable video ingestion and clip-to-label lookup, not model accuracy or overall dataset quality.
- The source publisher warns of some archived video label errors; use the accompanying metadata/corrections when selecting a broader sample.
- Per-video binary labels do not contain event start/end times. Do not report event-level recall or detection delay from this source alone.
- The handout has no separate video pathway. For later preparation, keep video handling explicit and preserve clip IDs and timestamps; extract frames only when required by the project task. Do not treat video as an unstructured document or discard provenance.

## Part A completion record

- [x] Mapped Topic 2 requirements to a suitable video source.
- [x] Recorded source identity, access/license, data type, and constraints.
- [x] Initialized the Colab workspace and verified uploaded inputs.
- [x] Inspected two representative videos for readability, size, resolution, frame rate, frame count, and duration.
- [x] Inspected the source annotation table, delimiter, columns, missing/duplicate rows, and labels for both selected clips.
- [x] Updated `notebooks/week3_pipeline.ipynb` with the corrected semicolon parsing and observed outputs.
