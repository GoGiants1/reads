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

## Working Conclusions

- ICLR 2026 MLLM data papers move beyond static sample selection. The data unit can be a compositional question, uncertainty-aware VQA supervision, active-perception rollout, tool-use trajectory, reward preference pair, or 15M-scale SFT corpus.
- Generator/verifier pipelines are now central. COMPACT, Bee, AdaReasoner, and DeepEyes all depend on some form of model-generated or model-verified data.
- Data quality is being tied to behavior. DeepEyes and AdaReasoner do not just improve benchmark accuracy; they try to induce active perception or adaptive tool orchestration.
- Evaluation needs to separate data quantity, data enrichment, and reward design. Several papers report strong gains with smaller but more structured data, so "data budget" alone is not a sufficient axis.
- Version tracking matters. COMPACT in particular changed title/framing/numbers between the early public version and ICLR final.
