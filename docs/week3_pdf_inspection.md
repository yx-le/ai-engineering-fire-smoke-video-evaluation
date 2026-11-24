# Topic 2 Preparation for Week 3 — PDF Inspection

## Selected papers

1. **FIgLib & SmokeyNet: Dataset and Deep Learning Model for Real-Time Wildland Fire Smoke Detection** (arXiv:2112.08598, v3). This paper describes a public wildland smoke image dataset and a spatiotemporal detector. The arXiv record lists a CC BY 4.0 license.
   - [Paper record](https://arxiv.org/abs/2112.08598v3) · [PDF in this repository](../data/arxiv_2112.08598_figlib_smokeynet.pdf)
   - Pages: 15

2. **Constructing a Real-World Benchmark for Early Wildfire Detection with the New PYRONEAR-2025 Dataset** (arXiv:2402.05349, v3). This paper describes a dataset containing images and video for smoke plume detection and sequential evaluation. The arXiv record lists a CC BY 4.0 license.
   - [Paper record](https://arxiv.org/abs/2402.05349v3) · [PDF in this repository](../data/arxiv_2402.05349_pyronear_benchmark.pdf)
   - Pages: 14

## Manual inspection and parsing challenges

I inspected rendered pages 1, 7, and 8 of the FIgLib & SmokeyNet PDF.

- Page 1 has author names with superscript affiliation numbers and a dense abstract. A text extractor may attach affiliation markers to names or split line-wrapped words, such as `arti-` / `ficial`.
- Page 7 contains evaluation formulas, subscripts, and inline symbols. Plain text extraction may lose the relationship between a metric name, its numerator/denominator, and the explanatory prose.
- Page 8 contains a dense comparison table with grouped model rows, multi-line headers, units, and bold best scores. A flattened extraction may shift values into the wrong columns or lose the model-group labels and bold emphasis.
- The local PDF renderer reported missing `Symbol` and `ArialUnicode` display fonts. The rendered pages were readable, but equations and unusual glyphs should be checked visually after extraction.

For the parsing exercise, compare the extracted text and any detected table with the rendered PDF page. Keep the original files unchanged and record any columns, formulas, captions, or reading order that need manual review.

## Use in the lab

The papers were selected for the PDF preparation exercise. The executed Week 3 project uses the video pathway and the sources listed in the [data inventory](week3_data_inventory.md).

Both papers list [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) on their arXiv records. The authors and source versions are credited in the linked records.
