# ICLR 2026 MLLM Data

ICLR 2026에 나온 MLLM data 중심 논문을 모으는 작업용 인덱스다. 기존 [Multimodal Data Selection](multimodal-data-selection.md)이 offline VIT data selection 방법 비교에 가깝다면, 이 페이지는 data curation, synthetic data, trajectory data, reward/preference data, large-scale SFT corpus까지 넓게 포함한다.

## Papers

| Paper | Data object | Main data signal | Training stage | Note |
|---|---|---|---|---|
| [DeepEyes](../papers/2025/2505.14362-deepeyes/) | RL prompts/active-perception trajectories | difficulty, verification, perception utility, conditional reward | RL for active visual reasoning | [note](../papers/2025/2505.14362-deepeyes/) |
| [HaDola](../papers/2025/2510.11295-hadola/) | VQA samples and labels | human uncertainty, calibration, pseudo-label reliability | SFT/self-training | [note](../papers/2025/2510.11295-hadola/) |
| [COMPACT / Visual Compositional Tuning](../papers/2025/2504.21850-compact/) | synthetic VIT Q/A pairs | atomic capability composition and `k-value` | visual instruction tuning | [note](../papers/2025/2504.21850-compact/) |
| [AdaReasoner](../papers/2026/2601.18631-adareasoner/) | multi-turn tool trajectories | abstract trajectory design, tool execution, reflection/failure coverage | Tool SFT + Tool GRPO | [note](../papers/2026/2601.18631-adareasoner/) |
| [BaseReward](../papers/2025/2509.16127-basereward/) | multimodal/text preference pairs | reward dataset utility, backbone/head recipe, mixture choice | reward modeling + RL | [note](../papers/2025/2509.16127-basereward/) |
| [Bee](../papers/2025/2510.13795-bee/) | large-scale MLLM SFT corpus | noise filtering, dual-level CoT enrichment, fidelity verification | SFT + refinement + GRPO | [note](../papers/2025/2510.13795-bee/) |

## Working Taxonomy

| Axis | Examples in this batch | Interpretation |
|---|---|---|
| Selection | HaDola, DeepEyes | harmful/beneficial samples are filtered before or during training |
| Enrichment | COMPACT, Bee | existing images or QA pairs are rewritten with higher information density |
| Trajectory construction | AdaReasoner, DeepEyes | data includes the reasoning/action path, not only final answers |
| Preference/reward data | BaseReward | data quality controls the reward model used for alignment |
| Pipeline release | Bee, AdaReasoner | reproducibility depends on releasing generators, filters, tools, and evaluation harnesses |

## OpenReview Review Process

All six papers were accepted as ICLR 2026 posters. The public OpenReview threads expose the submission, official reviews, author rebuttal/comments, area-chair meta-review, and final decision. The table below records the public thread state checked on 2026-04-26.

Source threads: [DeepEyes](https://openreview.net/forum?id=xUyMXkI958), [HaDola](https://openreview.net/forum?id=LuZjiUNuFL), [COMPACT](https://openreview.net/forum?id=073WQjmWKU), [AdaReasoner](https://openreview.net/forum?id=nUGPEmQ2ut), [BaseReward](https://openreview.net/forum?id=EuN5iszF0a), [Bee](https://openreview.net/forum?id=IVluwK8q9q).

| Paper | Official reviews | Initial ratings | Public discussion | Meta-review outcome |
|---|---:|---|---|---|
| DeepEyes | 4 | 6, 6, 6, 8 | 7 author replies and 1 reviewer follow-up | Positive from the start. AC noted no major unresolved concerns; rebuttal handled remaining detail questions, with at least one reviewer likely moving upward. |
| HaDola | 4 | 4, 6, 4, 6 | 4 author replies | Mixed initial reviews. Rebuttal addressed baseline choice, contribution framing, and HU-Accuracy validity; AC expected two weak rejects to move to accept. OOD generalization and statistical significance remained minor concerns. |
| COMPACT | 6 | 4, 8, 6, 4, 6, 6 | 17 author replies and 1 reviewer follow-up | Mixed but improved through rebuttal. Authors added open-source generator results, more data-reduction baselines, token-level efficiency, capability ablations, and failure-mode details. Atomic capability theory and single-main-model scope remained limitations. |
| AdaReasoner | 4 | 4, 6, 8, 8 | 19 author replies | Strong rebuttal effect. AC credited added cross-task generalization, zero-shot/unseen-tool experiments, variance analysis, reward sensitivity, and tool-augmented proprietary baselines. Methodological novelty remained a soft concern but not a blocker. |
| BaseReward | 4 | 6, 6, 6, 4 | 13 author replies and 1 reviewer follow-up | Accepted for practical value and comprehensive empirical recipe. Rebuttal mostly addressed concerns, but AC asked authors to avoid overclaiming "optimal" architecture because results are task-dependent. |
| Bee | 4 | 4, 8, 6, 4 | 15 author replies and 1 reviewer follow-up | Accepted after substantial discussion. AC considered most issues non-blocking and asked for cleaner factual writing. Remaining concerns involved overstatement, model-judge dependence, release details, and review-process irregularity around a misplaced review. |

### Review Takeaways

- The accepted papers were not uniformly high-scoring at first. HaDola, COMPACT, AdaReasoner, BaseReward, and Bee all had at least one rating of 4 before rebuttal.
- Rebuttal mattered most when authors added concrete evidence rather than just clarification. COMPACT and AdaReasoner are the clearest cases: added baselines, open-source generator/tool experiments, sensitivity studies, and error analyses shifted the AC recommendation.
- For MLLM data papers, reviewers repeatedly cared about reproducibility of generated/curated data: closed-source generators, LLM-as-judge verification, release details, and dependence on a single base model/dataset.
- The most common conceptual critique was overclaiming. Reviewers pushed authors to separate empirical recipe from theory, data efficiency from token efficiency, and task-specific pipeline design from general methods.
- ACs tended to accept pragmatic data-engineering contributions when the empirical package was strong, even if algorithmic novelty was limited.

## Working Conclusions

- ICLR 2026 MLLM data papers move beyond static sample selection. The data unit can be a compositional question, uncertainty-aware VQA supervision, active-perception rollout, tool-use trajectory, reward preference pair, or 15M-scale SFT corpus.
- Generator/verifier pipelines are now central. COMPACT, Bee, AdaReasoner, and DeepEyes all depend on some form of model-generated or model-verified data.
- Data quality is being tied to behavior. DeepEyes and AdaReasoner do not just improve benchmark accuracy; they try to induce active perception or adaptive tool orchestration.
- Evaluation needs to separate data quantity, data enrichment, and reward design. Several papers report strong gains with smaller but more structured data, so "data budget" alone is not a sufficient axis.
- Version tracking matters. COMPACT in particular changed title/framing/numbers between the early public version and ICLR final.
