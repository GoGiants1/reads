# Rethinking Generalization in Reasoning SFT: A Conditional Analysis on Optimization, Data, and Model Capability

- **Paper**: Rethinking Generalization in Reasoning SFT: A Conditional Analysis on Optimization, Data, and Model Capability
- **arXiv**: [2604.06628v1](https://arxiv.org/abs/2604.06628v1)
- **Authors**: Qihan Ren, Peng Wang, Ruikun Cai, Shuai Shao, Dadi Guo, Yuejin Xie, Yafu Li, Quanshi Zhang, Xia Hu, Jing Shao, Dongrui Liu
- **Venue**: arXiv 2026
- **Read date**: 2026-04-21
- **Tags**: supervised-finetuning, reasoning, generalization, chain-of-thought, data-quality

## One-line Summary

이 논문은 reasoning SFT가 단순 memorization에 그친다는 통념을 재검토하며, long-CoT SFT의 cross-domain generalization은 optimization duration, data quality/structure, base-model capability가 맞을 때 나타나는 조건부 현상이라고 주장한다.

## Key Idea

최근 LLM post-training 담론에서는 SFT는 memorization, RL은 generalization에 강하다는 구도가 자주 등장한다. 논문은 이 구도가 reasoning SFT 실험의 under-optimization, 낮은 data quality, 약한 base model, short-CoT/short-training 조건에서 나온 artifact일 수 있다고 본다.

핵심은 reasoning SFT generalization이 "없다"가 아니라 "조건부로 나타난다"는 것이다. 특히 long-CoT traces를 충분히 반복 노출하면 초기에는 OOD 성능이 떨어지다가, 이후 recovery를 거쳐 base model보다 좋아지는 dip-and-recovery 패턴이 나타난다.

## Method

- pretrained base model을 사용해 instruction-tuned/aligned checkpoint의 confound를 줄인다.
- Math-CoT-20k를 기본 dataset으로 사용한다. queries는 OpenR1-Math-220k에서 샘플링하고, Qwen3-32B가 생성한 long CoT responses 중 `math-verify`로 정답을 통과한 예시를 남긴다.
- Math-NoCoT-20k, NuminaMath-20k, Countdown-CoT-20k 등으로 data structure와 quality를 분리해 분석한다.
- Qwen3-14B/8B, InternLM2.5-20B, Qwen2.5 1.5B-14B 등 여러 base model capability를 비교한다.
- in-domain math, OOD reasoning, general capability, safety benchmark를 함께 평가한다.

## Experiments

기본 protocol은 AdamW, learning rate 5e-5, batch size 256, cosine schedule, 8 epochs이며 8 NVIDIA H200 GPU에서 학습한다. short training checkpoint와 extended training checkpoint를 비교해, SFT generalization이 training duration에 따라 어떻게 달라지는지 본다.

## Results

arXiv 초록 기준, cross-domain generalization은 optimization, data, model capability가 함께 맞을 때 나타난다. 초기 checkpoint만 보면 SFT가 in-domain math만 개선하고 OOD/general capability를 해치는 것처럼 보이지만, extended training에서는 OOD 성능이 회복되어 base model을 넘는 경우가 있다.

AlphaXiv 요약 기준, Qwen3-14B-Base에서 1 epoch Math-CoT SFT는 MATH500 +12.7%, AIME24 +29.7%를 얻지만 IFEval -9.8%, TruthfulQA -15.1%처럼 OOD/general metric이 떨어진다. 8 epochs로 늘리면 LCB v2, GPQA, IFEval, AlpacaEval 2.0 등에서 dip 이후 recovery가 나타난다.

## Discussion

이 논문은 data selection보다는 SFT data quality와 optimization dynamics 분석에 가깝다. 그래도 "학습 step마다 도움이 되는 데이터/노출 방식이 다르다"는 관점과 강하게 연결된다. 특히 repeated exposure가 단순히 더 많은 diverse data를 한 번 보는 것보다 long-CoT procedural pattern internalization에 더 중요할 수 있다는 점이 중요하다.

사용자 연구에서는 dynamic selection이나 curriculum을 설계할 때, early checkpoint 성능 하락을 곧바로 negative transfer로 해석하면 안 된다는 cautionary reference로 쓸 수 있다.

## Limitations

- reasoning SFT와 long-CoT math 중심 분석이라 일반 instruction tuning 전체로 바로 일반화하기 어렵다.
- extended SFT가 safety degradation을 동반할 수 있음을 보고하므로 capability gain만 최적화하면 위험하다.
- compute-heavy protocol이어서 작은 연구 환경에서는 full replication이 어렵다.

## References

- arXiv: <https://arxiv.org/abs/2604.06628>
- AlphaXiv: <https://www.alphaxiv.org/abs/2604.06628>
- arXiv HTML: <https://arxiv.org/html/2604.06628>
