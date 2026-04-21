# Concept-skill Transferability-based Data Selection for Large Vision-Language Models

- **Paper**: Concept-skill Transferability-based Data Selection for Large Vision-Language Models
- **arXiv**: [2406.10995v2](https://arxiv.org/abs/2406.10995v2)
- **Authors**: Jaewoo Lee, Boyang Li, Sung Ju Hwang
- **Venue**: EMNLP 2024
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, visual-instruction-tuning, vlm, coincide, transferability

## One-line Summary

COINCIDE는 small reference LVLM의 internal activation으로 visual instruction data를 concept-skill cluster로 묶고, cluster density와 transferability를 고려해 target LVLM fine-tuning subset을 고르는 방법이다.

## Key Idea

COINCIDE는 좋은 visual instruction subset이 다양한 visual-language concept-skill composition을 포함해야 한다고 본다. 이를 사람이 정의한 task label이 아니라 작은 reference LVLM의 activation cluster로 근사한다.

분류상 **offline / model-involved / reference-model activation / clustering + transferability** 방법이다.

## Method

COINCIDE의 흐름은 다음과 같다.

- small LVLM을 reference model로 사용한다.
- training data를 reference model internal activation 기반으로 clustering한다.
- cluster는 target LVLM이 배워야 할 concept-skill composition을 나타낸다고 해석한다.
- 각 cluster의 density와 다른 composition으로의 transferability를 고려해 sampling한다.
- target LVLM은 선택된 subset으로 instruction tuning한다.

## Experiments

논문은 LLaVA-1.5와 Vision-Flan 두 데이터셋에서 8개 strong baseline과 비교한다. LLaVA-1.5에서는 20% subset, Vision-Flan에서는 16.7% subset 설정이 핵심이다.

## Results

LLaVA-1.5 데이터셋의 20%만 사용해 전체 데이터 fine-tuning과 비슷한 성능을 달성하고, wall-clock running time을 70% 줄였다고 보고한다. Vision-Flan에서는 16.7% 데이터로도 우수한 결과를 얻는다.

## Discussion

COINCIDE는 ScalSelect/PRISM 같은 training-free 또는 target-forward 방법과 다르게 reference LVLM을 사용한다. 작은 모델의 activation structure가 큰 target LVLM에도 유용한 skill composition을 포착한다는 transfer assumption이 핵심이다.

비교 포인트:

- **vs. CADC**: CADC는 gradient trajectory에서 capability를 찾고, COINCIDE는 activation clustering으로 concept-skill을 찾는다.
- **vs. DataTailor**: DataTailor는 세 가지 해석 가능한 score를 조합하고, COINCIDE는 cluster density/transferability를 쓴다.
- **vs. ICONS**: ICONS는 gradient influence consensus, COINCIDE는 activation cluster transferability다.

## Limitations

- reference LVLM의 representation bias가 selection에 직접 반영된다.
- target LVLM과 reference LVLM의 architecture/domain 차이가 클 때 transferability가 약해질 수 있다.
- clustering granularity와 representation layer 선택이 성능에 민감할 가능성이 있다.

## References

- arXiv: <https://arxiv.org/abs/2406.10995>
- arXiv HTML: <https://arxiv.org/html/2406.10995>
- Code: <https://github.com/GJWLee/COINCIDE_code>
