# Data Selection Matters: Towards Robust Instruction Tuning of Large Multimodal Models

- **Paper**: Data Selection Matters: Towards Robust Instruction Tuning of Large Multimodal Models
- **arXiv**:
- **Authors**: Xu Yang, Chen Liu, Ying Wei
- **Venue**: NeurIPS 2025
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, visual-instruction-tuning, robustness, vlm, ards

## One-line Summary

ARDS는 full-data와 기존 selected-data tuning이 position bias와 spurious correlation을 그대로 학습한다는 문제를 겨냥해, worst-case subgroup에 가까운 training samples를 고르는 robustness-aware visual instruction selection 방법이다.

## Key Idea

대부분의 VIT data selection은 평균 benchmark 성능을 목표로 한다. ARDS는 평균 성능이 유지되어도 robust behavior가 나빠질 수 있다고 보고, selection objective를 worst-case robustness 쪽으로 돌린다.

분류상 ARDS는 **offline / model-involved / robustness-targeted / embedding-nearest selection**이다. 사용자의 메모처럼 일반 평균 성능 비교에는 꼭 넣지 않아도 되지만, "robust instruction tuning" 축에서는 별도 baseline으로 중요하다.

## Method

ARDS의 절차는 다음과 같이 요약된다.

- 먼저 visual/textual task-specific perturbation을 통해 worst-case evaluation subgroup을 찾는다.
- 이 subgroup은 모델이 position bias나 spurious correlation에 취약한 영역을 나타낸다.
- multimodal embedding space에서 training samples와 worst-case subgroup 사이의 semantic proximity를 계산한다.
- robust training mixture는 이 subgroup에 가까운 샘플을 우선 선택해 구성한다.
- downstream data 접근이나 time-consuming gradient computation 없이 robustness target을 반영하는 것이 설계 목표다.

## Experiments

OpenReview/NeurIPS 설명 기준, ARDS는 visual instruction-following data subset으로 LMM instruction tuning을 수행하고, clean 성능뿐 아니라 position bias와 spurious correlation perturbation에서의 worst-case robustness를 평가한다. 작은 모델에서 만든 robust mixture가 더 큰 architecture로 transfer되는지도 확인한다.

## Results

주요 결과는 ARDS가 기존 full-data training과 state-of-the-art selection method가 남기는 dataset bias를 줄이고, 데이터 효율성과 robustness를 함께 개선한다는 것이다. 부록 snippet 기준으로 동적 데이터 availability 설정에서도 LESS, COINCIDE보다 robustness 평균이 높게 보고된다.

## Discussion

ARDS는 "좋은 데이터"를 평균 성능 향상 샘플이 아니라 "현재 모델의 약한 subgroup을 보완하는 샘플"로 정의한다. 따라서 CADC의 capability coverage, ICONS의 cross-task influence consensus, DataTailor의 representativeness와는 target이 다르다.

비교 연구에서는 다음처럼 취급하는 것이 깔끔하다.

- main average-performance table에서는 optional baseline
- robustness-focused table에서는 핵심 baseline
- online continual이나 training-free selection과는 다른 targeted offline selection

## Limitations

- robust subgroup 생성 방식이 evaluation perturbation 설계에 의존한다.
- robustness target이 특정 benchmark bias에 맞춰지면 다른 shift로의 일반화는 별도 검증이 필요하다.
- arXiv ID가 없는 OpenReview paper라 citation/link 관리가 arXiv 논문보다 불편하다.

## References

- OpenReview: <https://openreview.net/forum?id=oLJMsGMfqr>
- NeurIPS poster: <https://neurips.cc/virtual/2025/poster/116045>
- Code: <https://github.com/xyang583/ARDS>
