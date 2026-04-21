# Approximating Gradient-Based Influence for Scalable Instruction Data Selection

- **Paper**: Approximating Gradient-Based Influence for Scalable Instruction Data Selection
- **arXiv**:
- **Authors**: Mohammad Gharehhasanloo, Yueting Chen, Nick Koudas, Xiao-Hui Yu
- **Venue**: CIKM 2025
- **Read date**: 2026-04-21
- **Tags**: data-selection, instruction-tuning, influence, gradient-approximation, approx-less

## One-line Summary

Approx-LESS는 전체 instruction corpus의 per-sample gradient를 모두 계산하지 않고, 일부 샘플의 LoRA gradient feature로 regression model을 학습해 나머지 샘플의 influence score를 예측하는 scalable instruction data selection 방법이다.

## Key Idea

LESS는 targeted instruction tuning에서 강력하지만, 모든 샘플의 gradient feature를 계산하는 비용이 크다. 이 논문은 influence score가 샘플 feature에서 예측 가능하다고 보고, 작은 fraction에 대해서만 expensive gradient extraction을 수행한 뒤 나머지는 regression으로 근사한다.

핵심 포지션은 "LESS의 target-aware gradient selection을 유지하되, gradient extraction 병목을 줄이는 practical approximation"이다.

## Method

- instruction corpus 중 일부 샘플에 대해 LoRA 기반 gradient feature를 계산한다.
- 이 샘플들의 influence score를 supervision으로 사용한다.
- regression model을 학습해 gradient를 계산하지 않은 나머지 샘플의 influence score를 예측한다.
- 예측 score 상위 샘플을 선택해 instruction tuning을 수행한다.

## Experiments

CIKM 2025 proceedings 소개 기준, 270K instruction corpus와 3개 validation set에서 평가한다. LESS 및 적용 가능한 baseline들과 비교하며, selection overlap과 gradient extraction time을 함께 본다.

## Results

공식 CIKM/ACM 목차 요약 기준, Approx-LESS는 적용 가능한 baseline보다 좋은 성능을 보이고 LESS에 근접한다. gradient extraction time은 3x 이상 감소하며, LESS가 고른 샘플과의 overlap도 높다.

## Discussion

이 논문은 사용자의 benchmark-target selection 관점에서 "좋은데 비싼 LESS를 실제 scale에 맞게 줄이는 방법"으로 중요하다. 영향도 기반 선별이 target validation set에 강하게 의존한다면, Approx-LESS는 그 target signal을 낮은 비용으로 corpus 전체에 확장하는 역할을 한다.

다만 근사 모델이 선택할 수 있는 score는 초기에 gradient를 계산한 subset의 coverage에 의해 제한된다. target distribution이 좁거나 multi-modal이면 sampling strategy가 중요해진다.

## Limitations

- regression model이 influence score를 충분히 일반화하지 못하면 LESS와의 gap이 커질 수 있다.
- 초기 gradient-computed subset의 선택 방식이 성능을 좌우한다.
- 짧은 proceedings paper라 대규모 모델/다양한 target에서의 검증 폭은 추가 확인이 필요하다.

## References

- DOI: <https://doi.org/10.1145/3746252.3760808>
- ACM CIKM 2025 TOC: <https://www.sigweb.hosting.acm.org/toc/cikm25.html>
- CoLab record: <https://colab.ws/articles/10.1145%2F3746252.3760808>
