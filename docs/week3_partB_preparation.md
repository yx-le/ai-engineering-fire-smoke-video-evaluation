# Week 3 Part B — Video Preparation Record

## Selected pathway and reason

The project uses prerecorded smoke videos. The lab has no dedicated video pathway, so this uses the computer-vision pathway with a video-specific frame-sampling step. It preserves clip identity and frame timestamps for later offline inspection. Sampling is not treated as evidence of detector accuracy.

## Comparison and decision

Two sampling configurations were compared using the inspected durations and the same 640-pixel maximum image edge:

| Configuration | `evo_10.mp4` | `evo_11.mp4` | Total frames |
|---|---:|---:|---:|
| One frame per second | 30 | 33 | 63 |
| One frame every two seconds (selected) | 15 | 17 | 32 |

The two-second setting reduces the prepared sample by about 49% compared with one-second sampling while retaining clip ID, source frame index, timestamp, original dimensions, prepared dimensions, and the clip-level smoke label. The accepted limitation is that sparse sampling can miss brief changes and cannot support event timing. The source labels are clip-level only.

## Preparation applied

- Sequentially decode each selected clip and sample frames at 0, 2, 4, ... seconds.
- Resize to a maximum edge of 640 pixels, preserving aspect ratio (the selected frames are 640 × 338).
- Encode frames as JPEG at quality 90; leave the source MP4s unchanged.
- Write a CSV frame index containing source clip, frame index, timestamp, source/prepared dimensions, label, and processing configuration.
- Bundle each clip's prepared frames in its own ZIP archive.
- Record source and prepared SHA-256 values, access rights, and limitations in `data/week3_manifest.json`.

## Observed output

- 32 prepared frames total: 15 from `evo_10.mp4` and 17 from `evo_11.mp4`.
- `data/processed/week3_video/frame_manifest.csv`: 32 rows.
- `data/processed/week3_video/evo_10_frames.zip`: 15 images; 1,024,962 bytes (Colab run).
- `data/processed/week3_video/evo_11_frames.zip`: 17 images; 1,330,910 bytes (Colab run).
- Both archive checksums match the Part B lineage records.

The executed Colab notebook, its `week3_manifest.json`, and both ZIP archives are now in the project. The copied archives match the manifest checksums and byte sizes. Their image names also match the 32-row frame index, and sampled images decode at 640 × 338. Do not commit the raw videos to the repository.
