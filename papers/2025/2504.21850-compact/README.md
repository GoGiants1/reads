# Visual Compositional Tuning

- **Paper**: Visual Compositional Tuning
- **arXiv**: [2504.21850v2](https://arxiv.org/abs/2504.21850v2)
- **Authors**: Xindi Wu, Hee Seung Hwang, Polina Kirichenko, Esin Tureci, Olga Russakovsky
- **Venue**: ICLR 2026
- **Read date**: 2026-04-26
- **Tags**: mllm, visual-instruction-tuning, data-curation, synthetic-data, compositionality, compact

## One-line Summary

COMPACT는 VIT 샘플 수를 늘리는 대신 한 질문에 여러 atomic visual capability를 조합해 sample complexity를 높임으로써, LLaVA-665K의 10% 데이터로 full VIT 수준의 성능을 달성하는 visual compositional tuning recipe다.

## Key Idea

기존 VIT 데이터는 "차 색이 무엇인가?"처럼 단일 또는 낮은 복잡도의 질문이 많아 이미지 안의 풍부한 시각 정보를 충분히 쓰지 못한다. COMPACT는 visual reasoning을 구성하는 10개 atomic capability를 정의하고, `k-value`를 "한 질문을 풀기 위해 필요한 atomic capability 수"로 둔다.

데이터 효율성의 핵심은 샘플 수가 아니라 샘플당 정보 밀도다. 즉, 더 많은 이미지-질문 pair를 모으는 대신 각 이미지에서 더 복합적인 질문을 합성한다.

## Method

- Atomic capabilities를 attribution, recognition, relation 계열로 나누고 color, shape, object recognition, action recognition, text recognition, counting, spatial recognition, spatial relationship, object interaction, scene understanding을 사용한다.
- 각 이미지에 대해 `k_gen`을 1, 2, 3 중에서 샘플링하고, 선택된 capability들이 자연스럽게 모두 필요하도록 Gemini-2.0-Flash로 question-answer pair를 생성한다.
- Quality verification으로 질문이 시각 정보에 기반하는지, capability 요구사항을 만족하는지, 답이 타당한지 확인한다.
- 생성된 compositional tuning data를 LLaVA-665K의 작은 VIT subset과 섞어 instruction-following 형식을 보존한다.

## Experiments

LLaVA-665K VIT setting에서 8개 multimodal benchmark를 사용해 random subset, 기존 data reduction/selection method, full VIT와 비교한다. MM-Vet과 MMStar처럼 복잡한 visual reasoning benchmark를 특히 중요하게 본다.

## Results

OpenReview 최종본 기준 COMPACT는 LLaVA-665K의 10% 데이터 예산으로 full VIT 대비 100.2% relative performance를 달성한다. 같은 조건에서 비교한 기존 SOTA data reduction method는 97.5%로 보고된다. 특히 MM-Vet은 full LLaVA-665K 29.2에서 COMPACT 31.7로, MMStar는 35.1에서 36.1로 오른다.

Ablation은 complexity range와 VIT subset mixing이 중요함을 보인다. 복합 질문만 쓰면 instruction following이 약해질 수 있고, 원래 VIT subset을 적절히 섞으면 response format과 compositional learning을 함께 얻는다.

## Version Notes

이 케이스는 이전 공개본과 ICLR 최종본 차이가 있다.

- **초기 arXiv v1 / VisCon 공개본(2025-04-30)**: 제목은 `COMPACT: COMPositional Atomic-to-Complex Visual Capability Tuning`이고 저자는 4명이다. 초록은 "MLLM이 simple task는 잘하지만 multiple capability가 필요한 complex task에서 어렵다"는 문제 제기로 시작하며, `k >= 4` 복잡도 slice에서 MMStar +83.3%, MM-Vet +94.0% 개선을 강하게 강조한다.
- **arXiv v2(2025-12-23)**: Esin Tureci가 저자에 추가되고, 초록/서론이 "VIT sample informativeness와 data curation" 관점으로 재작성된다. 핵심 숫자도 90% data budget reduction, 100.2% full VIT performance, 97.5% SOTA baseline, MM-Vet +8.6%, MMStar +2.9%처럼 전체 benchmark 평균 중심으로 바뀐다.
- **ICLR 2026 OpenReview 최종본**: 제목은 `Visual Compositional Tuning`으로 바뀌고, COMPACT expansion도 `Visual Compositional Tuning`으로 정리된다. 본문은 arXiv v2와 거의 같은 구조이며, "visual capability tuning"보다 "visual compositional tuning data recipe"가 전면에 온다.

따라서 아이디어 자체는 유지되지만, 논문의 포지셔닝은 "complex capability를 직접 키우는 방법"에서 "informativeness-aware VIT data curation / data-efficient synthetic recipe"로 이동했다.

## Discussion

현재 레포의 ScalSelect, CADC, CoIDO, DataTailor류가 기존 VIT pool에서 어떤 데이터를 고를지 다룬다면, COMPACT는 pool을 고르는 대신 각 image에서 더 informative한 synthetic question을 만들어 sample complexity를 높인다. 그래서 strict data selection이라기보다 data rewriting/data enrichment에 가깝다.

다만 비교 축은 여전히 비슷하다. selection signal은 gradient나 embedding이 아니라 capability composition이고, cost는 생성/검증용 VLM 호출이며, objective는 data-efficient VIT와 complex visual reasoning 성능이다.

## Limitations

- Gemini-2.0-Flash 같은 강한 generator/verifier에 의존해 비용과 재현성 부담이 있다.
- `k_gen <= 3` 중심이라 더 높은 복잡도 질문 생성과 검증은 제한적이다.
- atomic capability taxonomy가 완전하거나 상호 배타적이라고 보기 어렵다.
- 지식집약적 task에는 개선이 제한적일 수 있다.

## References

- arXiv: <https://arxiv.org/abs/2504.21850>
- OpenReview: <https://openreview.net/forum?id=073WQjmWKU>
- AlphaXiv: <https://www.alphaxiv.org/abs/2504.21850>
- Project: <https://princetonvisualai.github.io/compact/>
- Earlier VisCon OpenReview: <https://openreview.net/forum?id=kfynvrcC0L>
