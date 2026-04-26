# DeepEyes: Incentivizing "Thinking with Images" via Reinforcement Learning

- **Paper**: DeepEyes: Incentivizing "Thinking with Images" via Reinforcement Learning
- **arXiv**: [2505.14362v3](https://arxiv.org/abs/2505.14362v3)
- **Authors**: Ziwei Zheng, Michael Yang, Jack Hong, Chenxiao Zhao, Guohai Xu, Le Yang, Chao Shen, Xing Yu
- **Venue**: ICLR 2026
- **Read date**: 2026-04-26
- **Tags**: mllm, reinforcement-learning, active-perception, data-curation, visual-reasoning

## One-line Summary

DeepEyes는 별도 cold-start reasoning SFT 없이, 선별된 RL 데이터와 조건부 reward를 이용해 모델이 필요한 순간 이미지 영역을 다시 보고 reasoning하는 active perception 능력을 학습시킨다.

## Key Idea

기존 MLLM reasoning은 대부분 text-only CoT에 갇혀 있어 작은 물체, 복잡한 장면, hallucination 상황에서 시각 정보를 충분히 다시 확인하지 못한다. DeepEyes는 모델의 native grounding 능력을 내부 도구처럼 사용해, reasoning 중 bounding box를 생성하고 crop 이미지를 다시 입력받는 interleaved Multimodal CoT(iMCoT)를 학습한다.

핵심은 "도구를 많이 쓰게 하기"가 아니라, 정답으로 이어지는 경우에만 active perception을 보상해 시각적 재확인을 전략적으로 쓰게 만드는 것이다.

## Method

- Qwen2.5-VL 계열 모델이 텍스트 CoT를 생성하다가 필요하면 bounding box를 출력하고, 해당 crop을 다음 reasoning context에 추가한다.
- GRPO 기반 agentic RL을 사용하며 reward는 final answer accuracy, format reward, correct answer와 active perception이 함께 발생할 때 주는 conditional tool reward로 구성된다.
- Training data는 Visual Search/V* 기반 fine-grained perception 데이터, ArxivQA chart 데이터, ThinkLite-VL reasoning 데이터를 결합한다.
- Data selection은 난이도 필터링, open-ended format 정규화, label verification, active perception으로 풀 수 있는지 보는 perception-utility filter를 포함한다.
- 부록 기준 training corpus는 Visual Search 22k(47%), ArxivQA 14k(30%), ThinkLite-VL 11k(23%)로 구성된다.

## Experiments

Qwen2.5-VL-7B를 중심으로 고해상도 벤치마크(V*Bench, HR-Bench 4K/8K), MME-RealWorld-Lite, grounding/hallucination(refCOCO, ReasonSeg, POPE), 수학/추론(MathVista, MathVerse, WeMath 등)을 평가한다.

비교 대상은 GPT-4o/o3 같은 proprietary model, LLaVA-OneVision/Qwen2.5-VL 같은 open model, 그리고 SEAL/DyFo/ZoomEye 같은 workflow 기반 고해상도 perception 방법이다.

## Results

OpenReview 최종본 기준 DeepEyes-7B는 Qwen2.5-VL-7B 대비 V*Bench에서 +18.9, HR-Bench 8K에서 +7.3, MME-RealWorld-Lite overall에서 +10.9를 보인다. Grounding과 hallucination benchmark에서도 전반적으로 개선되며, reasoning benchmark에서는 MathVista, WeMath, LogicVista 등에서 작지만 일관된 이득을 보고한다.

Ablation에서는 conditional tool reward가 중요하다. tool reward가 없으면 active perception 사용이 사라지고, unconditional reward는 불필요한 도구 사용을 유도한다. 정답 조건부 reward가 있어야 시각적 재확인이 탐색에서 효율적 활용으로 이동한다.

## Discussion

이 논문은 "MLLM data" 관점에서 RL 데이터의 품질과 reward design이 perception behavior 자체를 만든다는 점이 중요하다. 데이터 선택은 단순히 좋은 샘플을 고르는 문제가 아니라, 모델이 학습해야 할 행동 공간(active perception)을 실제로 탐색 가능한 샘플만 남기는 역할을 한다.

현재 레포의 offline VIT data selection 논문들이 fixed candidate pool에서 어떤 샘플을 고를지 다룬다면, DeepEyes는 RL rollout이 성공적으로 시작될 수 있도록 data difficulty와 perception utility를 맞추는 쪽에 가깝다.

## Limitations

- Active perception이 crop/grounding 중심으로 설계되어 있어 더 복잡한 시각 도구 조합은 제한적이다.
- 좋은 RL 초기 샘플링을 위해 perception-utility가 높은 데이터를 선별해야 하므로, 데이터 필터 설계가 성능에 직접 영향을 준다.
- RL rollout 비용과 고해상도 이미지 처리 비용이 크다.
- 모델과 데이터 소스의 bias를 상속할 수 있다.

## References

- arXiv: <https://arxiv.org/abs/2505.14362>
- OpenReview: <https://openreview.net/forum?id=xUyMXkI958>
- AlphaXiv: <https://www.alphaxiv.org/overview/2505.14362v2>
- Code: <https://github.com/Visual-Agent/DeepEyes>
