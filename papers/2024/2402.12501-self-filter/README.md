# Your Vision-Language Model Itself Is a Strong Filter: Towards High-Quality Instruction Tuning with Data Selection

- **Paper**: Your Vision-Language Model Itself Is a Strong Filter: Towards High-Quality Instruction Tuning with Data Selection
- **arXiv**: [2402.12501v1](https://arxiv.org/abs/2402.12501v1)
- **Authors**: Ruibo Chen, Yihan Wu, Lichang Chen, Guodong Liu, Qi He, Tianyi Xiong, Chenxi Liu, Junfeng Guo, Heng Huang
- **Venue**: ACL Findings 2024
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, visual-instruction-tuning, self-filter, difficulty, diversity

## One-line Summary

Self-Filter는 VLM 자체와 함께 co-trained scoring network로 instruction difficulty를 측정하고, 어려운 샘플을 우선 선택하되 유사 샘플에는 penalty를 주어 다양성을 확보하는 VLM data selection 방법이다.

## Key Idea

Self-Filter의 관찰은 VLM이 가장 challenging한 instruction으로부터 더 많이 이득을 얻는다는 것이다. 그래서 외부 downstream task나 단일 heuristic score에 의존하지 않고, VLM 자체를 filter로 사용한다.

분류상 **offline / model-involved / co-trained scorer / difficulty + diversity** 방법이다.

## Method

Self-Filter는 두 단계로 구성된다.

1. **Scoring network training**
   - VLM과 scoring network를 함께 학습해 instruction difficulty를 평가한다.

2. **Selection**
   - 학습된 score network로 각 instruction의 difficulty를 측정한다.
   - challenging samples를 우선 선택한다.
   - 서로 유사한 samples에는 penalty를 주어 diversity를 확보한다.

## Experiments

LLaVA와 MiniGPT-4에서 full-data setting 및 competitive baselines와 비교한다. 선택 budget은 약 15%다.

## Results

arXiv 초록 기준, 약 15% samples만으로 full-data setting보다 좋은 결과를 달성할 수 있고, 여러 baseline보다 우수한 성능을 보인다.

## Discussion

Self-Filter는 VLM data selection 연구 초기에 중요한 baseline이다. 이후 논문들과 비교하면 다음 위치다.

- TIVE/ICONS: gradient influence 기반
- COINCIDE: small reference LVLM activation clustering
- DataTailor/CoIDO: multi-criteria 또는 coupled objective
- PRISM/ScalSelect: training-free representation/subspace 기반
- Self-Filter: VLM co-trained difficulty scorer

따라서 "VLM itself as filter"라는 아이디어는 강하지만, 최신 training-free 방법보다 selection 단계가 더 무겁다.

## Limitations

- scorer co-training이 필요해 완전한 사전 선택 방법은 아니다.
- challenging samples를 선호하면 noisy 또는 ambiguous instruction이 같이 선택될 위험이 있다.
- difficulty와 diversity penalty의 trade-off가 dataset에 따라 달라질 수 있다.

## References

- arXiv: <https://arxiv.org/abs/2402.12501>
- ACL Anthology: <https://aclanthology.org/2024.findings-acl.247/>
- DBLP: <https://dblp.org/rec/conf/acl/ChenWCLHXLGH24>
