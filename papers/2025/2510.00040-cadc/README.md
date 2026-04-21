# Uncovering Intrinsic Capabilities: A Paradigm for Data Curation in Vision-Language Models

- **Paper**: Uncovering Intrinsic Capabilities: A Paradigm for Data Curation in Vision-Language Models
- **arXiv**: [2510.00040v2](https://arxiv.org/abs/2510.00040v2)
- **Authors**: Junjie Li, Ziao Wang, Jianghong Ma, Xiaofeng Zhang
- **Venue**: arXiv 2025
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, visual-instruction-tuning, vlm, capability-analysis, curriculum

## One-line Summary

CADC는 VLM instruction tuning 데이터를 task heuristic으로 고르기보다, 모델의 gradient learning trajectory에서 발견되는 intrinsic capability에 귀속시키고 capability 균형과 curriculum으로 subset을 구성하는 방법이다.

## Key Idea

논문의 출발점은 visual instruction tuning 데이터셋을 black-box benchmark mixture로 보는 관점이 부족하다는 것이다. 데이터 축소가 성능 저하를 일으키는 이유를 단순히 "어떤 샘플이 어려운가"로 보지 않고, 모델이 실제로 학습하는 latent capability들이 충분히 커버되는지의 문제로 재정의한다.

비교 분류상 CADC는 **offline / model-involved / gradient trajectory / capability-aware curriculum**에 해당한다. 단순 scoring selector라기보다는 데이터-능력 attribution과 순서화까지 포함하는 curation framework다.

## Method

CADC는 크게 세 부분으로 읽을 수 있다.

1. **Capability discovery**
   - gradient 기반 learning trajectory를 사용해 모델 내부의 intrinsic capability 구조를 unsupervised 방식으로 찾는다.
   - task label이나 사람이 정한 카테고리보다 모델이 실제로 학습하는 방향을 우선한다.

2. **Capability attribution**
   - 각 training sample이 어떤 capability에 기여하는지를 influence estimation으로 계산한다.
   - 샘플 품질을 scalar 하나로만 두지 않고, capability별 기여 벡터로 본다.

3. **Capability-aware curriculum**
   - capability coverage가 한쪽으로 쏠리지 않도록 balanced selection을 수행한다.
   - staged sequencing으로 어떤 capability를 언제 학습시킬지 조절한다.

## Experiments

arXiv 초록 기준으로 CADC는 multimodal benchmark에서 full-data instruction tuning과 low-budget selected tuning을 비교한다. 핵심 주장은 원본 데이터의 5%만 사용해도 full-data training을 넘는 결과를 얻을 수 있다는 것이다.

## Results

가장 중요한 결과는 매우 낮은 budget에서도 capability-aware selection이 full-data training을 상회할 수 있다는 점이다. 이는 데이터 수 자체보다 capability coverage와 학습 순서가 instruction tuning 성능을 좌우할 수 있음을 보여준다.

## Discussion

CADC는 ScalSelect나 PRISM처럼 forward-only representation으로 subset을 고르는 방법보다 무겁다. 대신 "왜 이 샘플이 중요한가"를 capability attribution으로 설명할 수 있어, robustness나 long-tail skill coverage를 주장하는 논문과 비교하기 좋다.

비교 포인트:

- **vs. COINCIDE**: COINCIDE는 small reference LVLM의 activation cluster를 concept-skill composition으로 사용한다. CADC는 gradient learning trajectory에서 capability를 찾는 쪽에 더 가깝다.
- **vs. ICONS/TIVE/LESS**: 모두 influence/gradient 신호를 쓰지만, CADC는 단일 downstream target influence보다 capability 구조와 curriculum을 강조한다.
- **vs. ARDS**: ARDS는 robustness subgroup에 가까운 샘플을 고르는 targeted selection이고, CADC는 intrinsic capability coverage를 고르는 broader curation이다.

## Limitations

- gradient trajectory와 influence estimation을 요구하므로 training-free 방법보다 계산량과 구현 복잡도가 높다.
- "intrinsic capability"가 얼마나 안정적으로 발견되는지, 모델/데이터/초기화에 따라 얼마나 달라지는지 확인이 필요하다.
- capability-aware curriculum의 이득과 단순 balanced selection의 이득을 분리해 보는 ablation이 중요하다.

## References

- arXiv: <https://arxiv.org/abs/2510.00040>
- arXiv HTML: <https://arxiv.org/html/2510.00040>
- alphaXiv: <https://www.alphaxiv.org/abs/2510.00040>
