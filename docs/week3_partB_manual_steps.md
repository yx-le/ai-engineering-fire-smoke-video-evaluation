# Week 3 Part B — Completed Colab run

Part B has been run in Colab and its deliverables have been copied into this project. The saved notebook includes the executed preparation, validation, and lineage cells, followed by the completed Part C pipeline.

Verified project files:

- `notebooks/week3_pipeline.ipynb` — Part A, B, and C execution outputs.
- `data/week3_manifest.json` — two source-to-output records with matching SHA-256 values.
- `data/processed/week3_video/evo_10_frames.zip` — 15 sampled frames; 1,024,962 bytes.
- `data/processed/week3_video/evo_11_frames.zip` — 17 sampled frames; 1,330,910 bytes.
- `data/processed/week3_video/frame_manifest.csv` — 32 rows, with image names present in the prepared archives.
- `data/processed/week3_video/preparation_comparison.json` — comparison of the one-second and two-second sampling settings.

The raw 4K source videos remain outside the repository. Part C uses the same selected two-second sampling configuration and preserves source IDs and timestamps.
