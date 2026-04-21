# Multimodal Data Selection Reading TODO

목표: MLLM/VLM visual instruction tuning 데이터 선택 논문을 하나씩 읽고, 각 논문을 `papers/{year}/{id-slug}/` 노트로 정리한다. 비교 연구의 중심은 offline data selection이며, online continual setting은 필요할 때만 배경으로 본다.

## Workflow

각 논문마다 아래 순서로 처리한다.

- [ ] arXiv ID / 공식 페이지 / venue 확인
- [ ] `papers/{year}/{id-slug}/README.md` 작성
- [ ] `papers/{year}/{id-slug}/metadata.yaml` 작성
- [ ] root `README.md` Papers 테이블 업데이트
- [ ] 이 TODO에서 해당 항목 체크

## Priority Queue

### Offline / Model-Involved

- [x] ScalSelect: Scalable Training-Free Multimodal Data Selection for Efficient Visual Instruction Tuning
  - arXiv: 2602.11636v1
  - note: [papers/2026/2602.11636-scalselect/](papers/2026/2602.11636-scalselect/)
  - angle: target VLM internal attention + global subspace selection, training-free
- [ ] CADC: Uncovering Intrinsic Capabilities: A Paradigm for Data Curation in Vision-Language Models
  - arXiv: 2510.00040
  - angle: intrinsic capability discovery + capability-aware data curation
- [ ] CoIDO: Efficient Data Selection for Visual Instruction Tuning via Coupled Importance-Diversity Optimization
  - arXiv/OpenReview: 2510.17847 / NeurIPS 2025
  - angle: learned lightweight scorer, coupled importance-diversity objective
- [ ] ARDS: Data Selection Matters: Towards Robust Instruction Tuning of Large Multimodal Models
  - OpenReview: NeurIPS 2025
  - user note: 비교는 필요 없을 수 있음
  - angle: robustness-aware visual instruction selection
- [ ] DataTailor: Mastering Collaborative Multi-modal Data Selection: A Focus on Informativeness, Uniqueness, and Representativeness
  - arXiv: 2412.06293 / ICCV 2025
  - angle: informativeness + uniqueness + representativeness
- [ ] COINCIDE: Concept-skill Transferability-based Data Selection for Large Vision-Language Models
  - arXiv: 2406.10995 / EMNLP 2024
  - angle: small reference LVLM, concept-skill clustering, transferability
- [ ] PRISM: Self-Pruning Intrinsic Selection Method for Training-Free Multimodal Data Selection
  - arXiv: 2502.12119
  - user note: 비교는 필요 없을 수 있음
  - angle: training-free intrinsic selection via correlation/similarity
- [ ] ICONS: Influence Consensus for Vision-Language Data Selection
  - arXiv: 2501.00654
  - angle: influence consensus across tasks; possible MLLM version of IF-style selection
- [ ] TIVE: Less is More: High-value Data Selection for Visual Instruction Tuning
  - arXiv: 2403.09559
  - angle: task-level difficulty + instance influence for VIT
- [ ] LESS: Selecting Influential Data for Targeted Instruction Tuning
  - arXiv: 2402.04333 / ICML 2024
  - angle: LLM-targeted influence selection; used as comparison baseline in MLLM papers
- [ ] Self-Filter: Your Vision-Language Model Itself Is a Strong Filter: Towards High-Quality Instruction Tuning with Data Selection
  - arXiv: 2402.12501 / ACL Findings 2024
  - angle: VLM self-filtering for high-quality instruction tuning

### Offline / Model-Free

- [ ] 조사 필요: MLLM visual instruction tuning에서 truly model-free offline selection baseline이 있는지 확인
  - candidate keywords: heuristic, length, random, dataset statistics, deduplication, diversity without model embeddings

### Hybrid

- [ ] 조사 필요: offline/online hybrid MLLM data selection 방법이 실제로 있는지 확인
  - user note: 딱히 안보임

### Online / Out of Scope

- [ ] Adapt-infinity: Adapt-infinity: Scalable Continual Multimodal Instruction Tuning via Dynamic Data Selection
  - arXiv: 2410.10636 / ICLR 2025
  - status: out of main comparison scope
- [ ] OASIS: Online Sample Selection for Continual Visual Instruction Tuning
  - arXiv: 2506.02011
  - status: out of main comparison scope

### Optimal-Control Paper Baselines

- [ ] Data Selection via Optimal Control for Language Models
  - arXiv: 2410.07064
  - angle: PDS main paper; compare baseline setup and what it uses as RHO-Loss / DSIR / IF-Score
- [ ] RHO-Loss: Prioritized Training on Points that are Learnable, Worth Learning, and not yet Learnt
  - arXiv: 2206.07137 / ICML 2022
  - user note: 적용 용이
- [ ] DSIR: Data Selection for Language Models via Importance Resampling
  - arXiv: 2302.03169
  - user note: 적용 어려움
- [ ] IF-Score / influence-function baseline
  - source to pin down while reading PDS and ICONS
  - question: ICONS가 IF-Score의 MLLM 버전인지 비교 정리

## Comparison Axes

- setting: online continual / offline static / hybrid
- selection timing: pre-selection / during training / after warm-up
- model access: model-free / proxy-model / target-model forward / target-model gradients
- signal: loss, gradient, influence, embedding diversity, task/capability, instruction-conditioned attention, subspace leverage
- cost: forward-only / backward / warm-up training / pairwise similarity / SVD or clustering
- selected budget: 5%, 15%, 16%, 20%, 25%, etc.
- evaluation: VLM architecture, training dataset, downstream benchmarks, robustness, transferability
