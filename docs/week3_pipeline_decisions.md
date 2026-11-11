# Week 3 Part C — Pipeline Design and Decisions

## Selected pathway and scope

The project selects the **computer-vision/image pathway**, adapted for prerecorded video because the lab does not provide a separate video pathway. This is an offline data-preparation pipeline for the two inspected Boreal Forest Fire clips (`evo_10.mp4`, `evo_11.mp4`) and their clip-level label CSV. It does not run a fire/smoke detector or make safety claims.

## Topic 2 requirements mapped to this pipeline

- **FR-01 and RAI-05:** select identified clips and retain their source, access, and annotation references in the inventory and run manifest.
- **FR-02:** restrict this V1 preparation run to the selected prerecorded clips; it does not accept a live stream or run a detector.
- **NFR-03:** hold configuration fixed, record source/output fingerprints, and rerun the pathway to check repeatability.
- **FR-03, FR-04, and RAI-02:** preserve clip IDs, frame timestamps, and label links for later traceable evaluation. These requirements are not fully evaluated here: the available labels are clip-level only, so event recall, missed-event timing, and detection delay remain unavailable.
- **Modular pipeline:** source selection, ingestion, inspection, preparation, output validation, and lineage recording are separate stages with named inputs and outputs.
- **Reproducibility:** configuration is explicit; inputs and outputs are fingerprinted with SHA-256; output archives use fixed ZIP member timestamps; the notebook reruns the selected pathway and compares source/output fingerprints and mappings.
- **Data quality and validation:** check file presence/readability, label-table schema and clip coverage, video metadata, sampled-frame decode, output presence/non-zero size, archive contents, and output hashes.
- **Provenance:** record the pipeline version, selected pathway, parameters, source hashes, inspection results, outputs, source-to-output mapping, validation result, and known limitation in `pipeline_manifest.json`.
- **Responsible data handling:** raw clips are not copied into the Git repository. The manifest records filenames and hashes rather than embedding source video. Check dataset access/licence terms before redistribution.

## Source and preparation evidence

Inputs are `evo_10.mp4`, `evo_11.mp4`, and `video-ground-truths.csv`. The clips are 4096×2160 at approximately 47.952 fps, with durations about 29.59 s and 32.32 s; the label CSV has 288 rows and columns `Filename` and `Has Smoke`. Part B compared 1-second sampling (63 frames) with 2-second sampling (32 frames). The selected 2-second interval reduces the sample count by about 49% while retaining clip/frame identity and time metadata. The labels are clip-level binary labels, not event intervals.

## Chosen configuration and outputs

- Sample sequentially at 0, 2, 4, … seconds.
- Resize while preserving aspect ratio to a maximum edge of 640 pixels (observed prepared size: 640×338).
- Encode JPEG at quality 90.
- Retain source clip, frame index, timestamp, source/prepared dimensions, and clip label in the frame index.
- Save outputs under `data/pipeline_v1/video/`: `evo_10_frames.zip`, `evo_11_frames.zip`, `frame_manifest.csv`, and `pipeline_manifest.json`.

## Pipeline contracts and failure handling

| Stage | Input → output | Validation and failure response |
|---|---|---|
| Source selection | requested video pathway → selected MP4s and label CSV | Resolve only known files under the dataset root; missing or out-of-root files stop the run with an error. |
| Ingest | selected paths → readable files and SHA-256 fingerprints | Confirm existence and non-zero size; unreadable files or hash failures stop processing. |
| Inspect | video and label sources → metadata and label summary | Open each video and read a frame; require expected label columns and labels for selected clips. Invalid video/CSV stops the run before transformation. |
| Prepare / transform | inspected videos + fixed configuration → sampled JPEGs, ZIPs, and frame CSV | Decode sequentially; skip no silent failures—if no valid sampled frames are produced or encoding fails, raise an error. |
| Validate output | archives and CSV → validation result and output hashes | Reopen ZIPs, check image members and sample decode, require all outputs non-empty, then fingerprint them. Failure blocks lineage registration as a successful run. |
| Record lineage | validated run → `pipeline_manifest.json` | Store source/output hashes and mapping; a write or verification failure is surfaced and the run is not considered complete. |

Two explicit failure points are missing/unreadable input during ingest and invalid/empty/corrupt output during validation. Both fail closed with an error rather than recording a successful run.

## ETL/ELT and batch/streaming decision

This is **ETL**: clips are decoded and transformed into sampled, resized frames before they are consumed by downstream evaluation. Batch processing is appropriate because the inputs are fixed research clips and there is no live-camera requirement. Streaming would add operational complexity and is outside the approved project scope.

## Provenance, access, and limitations

The manifest provides reproducibility evidence, not a grant of redistribution rights. It stores source names and hashes, not the raw clips. Access to the clips depends on the original dataset location and its terms; keep restricted or large raw files outside Git and share them only where their licence allows. The prepared sample and manifest are in this repository, but the source video is not.

Two-second sparse sampling can miss brief events. Binary clip labels provide no event start/end times, so this pipeline cannot measure event timing or establish detector performance. The output is prepared data only; detector inference and evaluation remain later work.

## Repeatability result

The notebook's required repeatability/provenance cell reran the selected video pipeline. It passed equality checks for source fingerprints, prepared-output fingerprints, and source-to-output mappings. The registered manifest identifies pipeline `week3-v1`, and the output ZIP/CSV hashes match the downloaded artifacts.
