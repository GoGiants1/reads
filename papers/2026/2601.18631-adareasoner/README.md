# AdaReasoner: Dynamic Tool Orchestration for Iterative Visual Reasoning

- **Paper**: AdaReasoner: Dynamic Tool Orchestration for Iterative Visual Reasoning
- **arXiv**: [2601.18631v2](https://arxiv.org/abs/2601.18631v2)
- **Authors**: Mingyang Song, Haoyu Sun, Jiawei Gu, Linjie Li, Ranjay Krishna, Yu Cheng
- **Venue**: ICLR 2026
- **Read date**: 2026-04-26
- **Tags**: mllm, tool-use, reinforcement-learning, trajectory-data, visual-reasoning, data-curation

## One-line Summary

AdaReasoner는 multi-turn visual tool use를 위한 trajectory data curation과 Tool-GRPO를 결합해, MLLM이 어떤 도구를 언제 조합할지 동적으로 학습하게 만든다.

## Key Idea

시각 추론에서 도구를 붙이는 것만으로는 부족하다. 모델은 문제 상태에 따라 어떤 도구가 필요한지, 도구 출력이 실패했을 때 어떻게 backtrack할지, 여러 도구를 어떤 순서로 조합할지 배워야 한다.

AdaReasoner는 이를 도구별 supervised behavior가 아니라 일반적인 tool planning skill로 학습한다. 데이터 관점에서 핵심은 고품질 multi-turn tool trajectory를 만들어 모델에게 "무슨 도구를 호출할지"뿐 아니라 "왜 그 도구를 호출하고 결과를 어떻게 해석할지"를 보여주는 것이다.

## Method

- Tool suite는 POINT, DRAW2DPATH, ASTAR, DETECTBLACKAREA, INSERTIMAGE, OCR, CROP 등 perception, manipulation, calculation 계열을 포함한다.
- Tool Cold Start 단계에서는 task별 abstract trajectory blueprint를 먼저 설계한다. VSP는 perception-planning-verification, Jigsaw는 trial-and-error, GUIQA는 focus-then-extract 흐름을 따른다.
- Abstract trajectory를 실제 tool execution으로 grounding하고, 강한 LLM으로 step 간 CoT를 생성해 multi-turn tool-augmented trajectory data를 만든다.
- Reflection/backtracking과 explicit tool failure 사례를 포함해, 모델이 perfect path만 암기하지 않고 실패한 도구 출력이나 suboptimal intermediate result를 처리하게 한다.
- Tool-GRPO는 multi-turn reward accumulation과 adaptive tool reward를 사용한다. Format은 모든 step에서 맞아야 하고, tool reward는 각 tool call의 structure/name/parameter/content를 평가하며, final accuracy reward와 함께 최적화된다.

## Experiments

Qwen2.5-VL-3B/7B 기반으로 VSPO, VSP, Jigsaw, BLINK-J, GUIChat, WebMMU 등을 평가한다. 비교 대상은 base model, direct SFT, direct GRPO, Tool Cold Start, Tool GRPO, 그리고 둘을 결합한 모델이다.

## Results

OpenReview 최종본 기준 AdaReasoner는 7B에서 평균 +38.7% 수준의 baseline 개선을 보고하고, VSP 97.6% 같은 복잡한 tool reasoning benchmark에서 near-perfect 성능을 보인다. GPT-5, Claude Sonnet 4 같은 proprietary system보다 일부 VSP/Jigsaw task에서 높은 성능을 보인다고 주장한다.

분석에서는 모델이 필요한 도구는 채택하고, 불필요하거나 방해되는 도구는 사용 빈도를 낮추며, task에 따라 tool invocation frequency를 조절하는 self-adaptive behavior를 보인다.

## Discussion

이 논문은 "data for MLLM"의 단위가 image-question pair에서 trajectory로 확장되는 사례다. COMPACT가 질문의 compositional complexity를 키운다면, AdaReasoner는 문제 해결 과정 자체의 complexity를 데이터화한다.

현재 레포의 data selection 축으로 보면 selection signal은 sample score가 아니라 trajectory design quality다. 데이터 생성은 task blueprint, tool execution, CoT generation, failure/backtracking coverage로 구성되며, RL reward와 분리해서 볼 수 없다.

arXiv v2와 OpenReview final 사이에 저자 표기가 다르다. arXiv는 Luxin Xu를 포함한 7명으로 표시되지만, OpenReview ICLR 최종 페이지는 6명으로 표시된다. 이 노트의 상단 metadata는 venue final 기준을 따랐다.

## Limitations

- Tool trajectory blueprint가 task별로 설계되어야 하므로 완전 자동 일반화는 아직 아니다.
- 성능 한계가 모델 scale보다 tool accuracy와 tool availability로 이동한다.
- 새로운 도구가 주어졌을 때 zero-shot으로 도움이 될 수 있지만, irrelevant tool은 interference를 만들 수 있어 RL 중 tool pruning이 필요하다.
- 더 강한 tool-using agent는 dual-use와 bias 위험을 가진다.

## References

- arXiv: <https://arxiv.org/abs/2601.18631>
- OpenReview: <https://openreview.net/forum?id=nUGPEmQ2ut>
- AlphaXiv: <https://www.alphaxiv.org/overview/2601.18631>
- Project: <https://adareasoner.github.io/>
