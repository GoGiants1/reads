# Paper Note Workflow

This file is the reusable workflow reference for adding paper notes. It replaces the completed `TODO.md` queue; the root `README.md` Papers table is the source of truth for what has already been summarized.

## Intake Checklist

1. Check for duplicates before creating files:
   - Search by arXiv ID, title keyword, and method acronym in `README.md` and `papers/`.
   - If a note already exists, update that note or its `metadata.yaml`; do not create a second directory.
2. Verify metadata from primary sources:
   - Use arXiv API for arXiv ID, version, title, authors, date, categories, and abstract.
   - Use AlphaXiv overview first, then AlphaXiv full markdown if overview is missing or too thin.
   - Use DeepXiv only when the local adapter or CLI is available; otherwise record that arXiv/AlphaXiv fallback was used.
   - For non-arXiv papers, prefer official venue, OpenReview, ACL Anthology, ACM, PMLR, DOI, or project pages.
3. Create the paper directory:
   - `papers/{year}/{arxiv-id}-{short-slug}/README.md`
   - `papers/{year}/{arxiv-id}-{short-slug}/metadata.yaml`
   - Omit arXiv version suffix from the path, but record it in `metadata.yaml`.
4. Write the note from `templates/paper-note.md`:
   - bibliographic metadata
   - one-line summary
   - key idea
   - method
   - experiments
   - results
   - discussion or interpretation
   - limitations
   - references/source links
5. Update root `README.md`:
   - Add one row to the Papers table.
   - Keep links directory-based, for example `[note](papers/2026/2602.05400-opus/)`.
6. Validate before commit:
   - Check README note links resolve to existing directories.
   - Run `git diff --cached --check` after staging.
   - Stage only intended files.
7. Commit and push:
   - Use a short descriptive commit message.
   - Leave unrelated user changes unstaged.

## Source Handling

- arXiv IDs are stored bare in paths and `metadata.yaml` (`2602.05400`, not `2602.05400v2`).
- `links.arxiv` should include the verified version URL when known.
- Add `links.alphaxiv` when AlphaXiv was used.
- If AlphaXiv overview returns 404, use `https://alphaxiv.org/abs/{id}.md` and note the same public AlphaXiv URL in references.
- If DeepXiv is unavailable locally, do not install it just for a note; proceed with arXiv/AlphaXiv and mention the fallback in the final report when relevant.

## Data-Selection Reading Axes

Use these axes to keep data-selection notes comparable:

- setting: offline static, online/dynamic, continual, hybrid, reinforcement learning
- selection timing: pre-selection, warm-up, per-iteration, stage-wise, after proxy training
- model access: model-free, proxy model, target-model forward, target-model gradients, optimizer state
- signal: loss, perplexity, gradient, influence, Shapley, embedding similarity, diversity, task/capability, textual frequency, attention, subspace leverage
- objective: target benchmark, balanced multi-capability, pretraining efficiency, domain adaptation, factual memorization, robustness
- cost: forward-only, backward, rollouts, warm-up training, pairwise similarity, SVD/sketching, optimal control solve
- selected budget: 1%, 5%, 10%, 15%, 20%, 25%, token budget, domain mixture ratio
- evaluation: architecture, dataset, target benchmarks, OOD transfer, robustness, scaling, compute overhead

## Current Reading Map

The completed data-selection notes now cover these buckets:

- benchmark-targeted and validation-targeted selection: Nuggets, DsDm, LESS, BETR, TAROT, ToV, TADS, GIST
- dynamic and optimizer-aware selection: PDS, MATES, TiKMiX, OPUS, Adam-aware In-Run Shapley, GradAlign
- influence theory and scalable attribution: LESS, BIDS, random projection for influence functions, Approx-LESS
- data mixture and pretraining filters: DoReMi, RegMix, DataComp-LM, FineWeb, QuRating, DSIR, RHO-Loss
- diversity and balanced learning: BIDS, DPP diversity, DataTailor, CoIDO, IDEAL
- multimodal/VLM data selection: ScalSelect, CADC, CoIDO, ARDS, DataTailor, COINCIDE, PRISM, ICONS, TIVE, Self-Filter
- supporting theory and adjacent signals: DataRater, Meta-rater, Cram Less to Fit More, Why Less is More, Adam's Law

When starting a new batch, first scan the root `README.md` and this map to decide whether the paper is a new bucket, a direct baseline, or an update to an existing note.
