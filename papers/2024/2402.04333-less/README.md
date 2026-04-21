# LESS: Selecting Influential Data for Targeted Instruction Tuning

- **Paper**: LESS: Selecting Influential Data for Targeted Instruction Tuning
- **arXiv**: [2402.04333v3](https://arxiv.org/abs/2402.04333v3)
- **Authors**: Mengzhou Xia, Sadhika Malladi, Suchin Gururangan, Sanjeev Arora, Danqi Chen
- **Venue**: ICML 2024
- **Read date**: 2026-04-21
- **Tags**: data-selection, instruction-tuning, llm, influence, gradients

## One-line Summary

LESS는 targeted instruction tuning을 위해 low-dimensional gradient features로 reusable gradient datastore를 만들고, 목표 capability few-shot examples와 gradient similarity가 높은 training data를 고르는 LLM data selection 방법이다.

## Key Idea

LESS는 일반 챗봇 능력을 위한 전체 instruction mixture가 아니라 특정 downstream skill, 예를 들어 reasoning, 을 강화하는 targeted instruction tuning을 목표로 한다. 어떤 데이터가 필요한지는 목표 capability를 대표하는 few-shot examples와 gradient 방향이 얼마나 비슷한지로 판단한다.

분류상 **offline / model-involved / gradient datastore / targeted LLM instruction tuning**이다. MLLM 논문들에서는 직접적인 VLM 방법은 아니지만 influence/gradient baseline으로 자주 비교된다.

## Method

LESS의 핵심은 Low-rank gradiEnt Similarity Search다.

- instruction data의 low-dimensional gradient feature를 미리 계산해 datastore로 저장한다.
- Adam optimizer와 variable-length instruction data에 맞게 influence formulation을 조정한다.
- 특정 capability를 대표하는 few-shot target examples의 gradient와 similarity를 계산한다.
- similarity가 높은 데이터를 선택해 targeted instruction tuning에 사용한다.

## Experiments

LLM instruction tuning 환경에서 다양한 downstream tasks와 target capability를 평가한다. 선택 budget은 대표적으로 5%다.

## Results

arXiv 초록 기준, LESS가 고른 5% 데이터로 학습한 모델이 여러 downstream task에서 full dataset training을 능가하는 경우가 많다. 선택 데이터는 작은 모델에서 큰 모델로, 다른 model family 사이로도 transfer된다고 보고한다.

## Discussion

MLLM 비교에서 LESS는 "LLM에서 검증된 gradient influence selection"의 대표 baseline이다. 그러나 VLM에서는 image-token, multimodal alignment, task mixture 문제가 추가되므로 ICONS/TIVE가 이를 확장하거나 변형한 계열로 볼 수 있다.

ICONS와의 관계:

- LESS: target capability few-shot examples와 gradient similarity
- ICONS: VLM multitask validation influence와 cross-task majority consensus

즉 LESS는 targeted skill selection, ICONS는 multitask robust consensus selection에 가깝다.

## Limitations

- target capability examples가 필요하다.
- gradient feature 계산과 datastore 구축 비용이 있다.
- MLLM visual instruction tuning에 그대로 적용하면 visual modality와 task calibration 문제가 남는다.

## References

- arXiv: <https://arxiv.org/abs/2402.04333>
- arXiv HTML: <https://arxiv.org/html/2402.04333>
- Code/data: <https://github.com/princeton-nlp/LESS>
