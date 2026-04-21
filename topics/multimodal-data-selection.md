# Multimodal Data Selection

Visual instruction tuning 데이터 선택 논문을 비교하기 위한 작업용 인덱스다. 현재 묶음의 중심은 offline MLLM/VLM instruction tuning이며, online continual selection과 LM pretraining selection은 비교 축을 잡기 위한 보조 그룹으로 둔다.

## Quick Taxonomy

| Method | Setting | Model access | Main signal | Notes |
|---|---|---|---|---|
| [ScalSelect](../papers/2026/2602.11636-scalselect/) | offline | target VLM forward/internal activations | instruction-conditioned visual subspace leverage | training-free, pairwise similarity avoided |
| [CADC](../papers/2025/2510.00040-cadc/) | offline | gradient trajectory/influence | intrinsic capability attribution | capability-aware balanced selection + curriculum |
| [CoIDO](../papers/2025/2510.17847-coido/) | offline | learned lightweight scorer | coupled importance-diversity | scorer trained on small random subset |
| [ARDS](../papers/2025/yang-2025-data-selection-matters/) | offline | multimodal embeddings | proximity to worst-case robust subgroups | robustness-focused, optional for average-performance comparison |
| [DataTailor](../papers/2024/2412.06293-datatailor/) | offline | model-derived scoring | informativeness, uniqueness, representativeness | interpretable multi-criteria selection |
| [COINCIDE](../papers/2024/2406.10995-coincide/) | offline | small reference LVLM activations | concept-skill cluster density + transferability | proxy/reference model assumption |
| [PRISM](../papers/2025/2502.12119-prism/) | offline | visual features | anisotropy correction / intrinsic visual semantics | training-free, geometry-focused |
| [ICONS](../papers/2025/2501.00654-icons/) | offline | gradients/influence | cross-task influence consensus | IF-style VLM extension with voting consensus |
| [TIVE](../papers/2024/2403.09559-tive/) | offline | gradients/influence | task difficulty + instance influence | task-aware allocation |
| [LESS](../papers/2024/2402.04333-less/) | offline | LLM gradients | low-rank gradient similarity to target examples | LLM baseline, not VLM-specific |
| [Self-Filter](../papers/2024/2402.12501-self-filter/) | offline | VLM + co-trained scorer | instruction difficulty + diversity penalty | early VLM data selection baseline |
| [Adapt-infinity](../papers/2024/2410.10636-adapt-infinity/) | online/continual | gradient sample vectors + selector experts | pseudo-skill cluster dynamic selection | out of main offline scope |
| [OASIS](../papers/2025/2506.02011-oasis/) | online/continual | reference-model-free stream scoring | inter-batch informativeness + redundancy update | out of main offline scope |
| [PDS](../papers/2024/2410.07064-pds/) | offline LM pretraining | proxy LM training dynamics | PMP-derived data quality | optimal-control anchor for baselines |
| [RHO-Loss](../papers/2022/2206.07137-rho-loss/) | prioritized training | current + holdout/downstream losses | reducible holdout loss | easier to adapt conceptually |
| [DSIR](../papers/2023/2302.03169-dsir/) | offline LM pretraining | feature distributions | importance resampling to target distribution | hard to adapt directly to multimodal VIT |

## Working Conclusions

- **Main comparison group**: ScalSelect, CADC, CoIDO, DataTailor, COINCIDE, PRISM, ICONS, TIVE, Self-Filter.
- **Optional or scoped comparison**: ARDS for robustness, LESS for LLM influence baseline, PDS/RHO-Loss/DSIR for optimal-control baseline context.
- **Out of scope for offline table**: Adapt-infinity and OASIS, because both assume streaming/continual arrival rather than a fixed candidate pool.
- **Model-free status**: in this list, truly model-free offline MLLM data selection mostly appears as simple baselines such as random, length, heuristic filtering, or feature-statistic filtering. The stronger methods generally use a model, proxy model, target activation, gradient, or learned scorer.
- **Hybrid status**: recent survey framing uses hybrid as a broad paradigm, but in this target list there is no obvious single "hybrid" method that needs a separate main bucket. Several methods are hybrid in a loose sense, such as CoIDO combining learned scoring with diversity and DataTailor combining multiple factors.

## Sources

- Visual instruction data selection survey: <https://openreview.net/forum?id=JVrTvE3QEi>
- arXiv and OpenReview links are recorded in each paper note.
