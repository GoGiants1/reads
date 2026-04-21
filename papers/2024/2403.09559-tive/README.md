# Less is More: High-value Data Selection for Visual Instruction Tuning

- **Paper**: Less is More: High-value Data Selection for Visual Instruction Tuning
- **arXiv**: [2403.09559v4](https://arxiv.org/abs/2403.09559v4)
- **Authors**: Zikang Liu, Kun Zhou, Wayne Xin Zhao, Dawei Gao, Yaliang Li, Ji-Rong Wen
- **Venue**: arXiv 2024
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, visual-instruction-tuning, tive, influence, task-difficulty

## One-line Summary

TIVE는 task difficulty와 instance influence를 gradient-based influence function으로 추정해, task별 선택 비율과 task 내부 high-value samples를 함께 결정하는 visual instruction tuning 데이터 선택 방법이다.

## Key Idea

논문은 visual instruction dataset 안에 task-level redundancy가 크며, 일부 task의 instruction을 크게 줄여도 성능이 유지될 수 있다고 관찰한다. 따라서 샘플 단위 품질뿐 아니라 task별로 얼마나 뽑을지도 같이 결정해야 한다.

분류상 **offline / model-involved / gradient influence / task-aware allocation**이다.

## Method

TIVE는 두 종류의 score를 계산한다.

- **Instance influence score**: 해당 task 내에서 개별 instruction sample이 얼마나 유용한지.
- **Task difficulty score**: task 자체가 모델에게 얼마나 어렵고 더 많은 데이터를 필요로 하는지.

이 두 점수를 이용해 먼저 task proportion을 정하고, 각 task 안에서 high-value instances를 선택한다.

## Experiments

다양한 LVLM에서 8개 benchmark를 기준으로 full-data fine-tuned model과 15% selected-data model을 비교한다.

## Results

arXiv 초록 기준, 약 15% 데이터만 사용해 8개 benchmark 평균에서 full-data fine-tuning과 comparable한 성능을 달성하고, 4개 benchmark에서는 full-data model을 넘는다.

## Discussion

TIVE는 task allocation을 명시적으로 다룬다는 점에서 중요하다. ICONS가 task별 influence를 consensus로 합친다면, TIVE는 task difficulty로 선택 비율 자체를 조정한다.

비교 연구에서는 gradient/influence 계열의 early VLM data selection baseline으로 쓰기 좋다. 다만 최신 training-free 방법들과 비교할 때 계산 비용 측면의 disadvantage를 분명히 써야 한다.

## Limitations

- influence function 기반이라 계산 비용과 approximation 품질 문제가 있다.
- task label 또는 task grouping이 필요하므로 dataset annotation structure에 의존한다.
- task difficulty score가 benchmark generalization을 얼마나 잘 예측하는지 별도 확인이 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2403.09559>
- arXiv HTML: <https://arxiv.org/html/2403.09559>
