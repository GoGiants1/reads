# OASIS: Online Sample Selection for Continual Visual Instruction Tuning

- **Paper**: OASIS: Online Sample Selection for Continual Visual Instruction Tuning
- **arXiv**: [2506.02011v2](https://arxiv.org/abs/2506.02011v2)
- **Authors**: Minjae Lee, Minhyuk Seo, Tingyu Qu, Tinne Tuytelaars, Jonghyun Choi
- **Venue**: arXiv 2025
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, continual-learning, visual-instruction-tuning, online-selection, oasis

## One-line Summary

OASIS는 continual instruction tuning stream에서 batch-wise top-k 대신 이전까지 본 전체 데이터와의 상대적 informativeness와 informative redundancy를 함께 고려해 adaptive online subset을 고르는 방법이다.

## Key Idea

Continual instruction tuning에서는 future data를 알 수 없고, reference model을 미리 준비하기도 어렵다. 기존 online sample selection이 batch마다 고정 k개를 고르면 batch distribution shift에 취약하다. OASIS는 sample informativeness를 batch 내부가 아니라 과거 전체 context에 대해 평가한다.

분류상 **online/continual / reference-model-free / adaptive sample selection**이다.

## Method

OASIS의 핵심은 두 가지다.

- 각 샘플의 informativeness를 모든 previously seen data와 비교해 추정한다.
- 선택된 샘플 사이의 informative redundancy를 줄이기 위해 iterative score update를 수행한다.

이 방식은 batch별 고정 quota보다 distribution shift에 대응하기 쉽다.

## Experiments

다양한 large foundation models에서 continual visual instruction tuning을 평가한다. 핵심 budget은 전체 데이터의 25% 사용이다.

## Results

arXiv 초록 기준, OASIS는 25% 데이터만 사용해 full-data training과 comparable한 성능을 달성하고, state-of-the-art sampling methods를 능가한다.

## Discussion

OASIS는 offline comparison의 직접 baseline은 아니다. 다만 online setting을 제외한다는 근거를 세울 때 Adapt-infinity와 함께 언급하기 좋다.

- Adapt-infinity: selector expert + pseudo-skill cluster + permanent pruning
- OASIS: reference-model-free online informativeness + redundancy update

둘 다 static candidate pool을 전제로 하지 않는다.

## Limitations

- online stream order와 distribution shift pattern에 따라 성능이 달라질 수 있다.
- 이전 데이터 전체와의 상대 informativeness 계산을 효율적으로 유지하는 구현이 중요하다.
- offline visual instruction subset selection과 직접 비교하기 어렵다.

## References

- arXiv: <https://arxiv.org/abs/2506.02011>
- arXiv HTML: <https://arxiv.org/html/2506.02011>
