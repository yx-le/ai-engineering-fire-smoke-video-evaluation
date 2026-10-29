# Group Part A/B evidence

The canonical Week 1 deliverables are in the parent `docs/` folder:

- `../week1_observation_sheet.md` — the group's ChatGPT observation sheet.
- `../week1_system_map.png` — the group's approved conceptual system map.
- `evidence/` — three screenshots selected from the five observed tests. The lab asks for 2–3 evidence pieces; Q1, Q3, and Q5 show factual accuracy, ambiguity handling, and safety behaviour.
- `part_b/week1_system_map.drawio` — editable source for the approved map.
- `part_b/week1_system_map_notes.md` — component descriptions, six failure scenarios, and the Part B reflection.

The files are in the group repository:

- [Observation sheet](https://github.com/YANKEY-CODE/ai-engineering-sortsmart-ai/blob/main/docs/week1_observation_sheet.md)
- [Approved map PNG](https://github.com/YANKEY-CODE/ai-engineering-sortsmart-ai/blob/main/docs/week1_system_map.png)

The lecturer confirmed that a group of six is allowed, the exact group number does not matter, and the cross-group reflection may be skipped. The personal copy of the observation sheet records these clarifications; the observed ChatGPT findings and approved map are unchanged.

## Part B reflection

The model-to-output validation path has the largest cluster of related failure scenarios: hallucination (F3), a missed safety check (F4), and a user mistaking guidance for a completed action (F6). Engineering effort should therefore emphasize checking generated responses, applying safety checks, and stating clearly whether an external action was completed. This connects to accuracy and reliability, safety, and transparency; feedback monitoring can also help catch recurring problems.

