# Improving Influence-based Instruction Tuning Data Selection for Balanced Learning of Diverse Capabilities

- **Paper**: Improving Influence-based Instruction Tuning Data Selection for Balanced Learning of Diverse Capabilities
- **arXiv**: [2501.12147v1](https://arxiv.org/abs/2501.12147v1)
- **Authors**: Qirun Dai, Dylan Zhang, Jiaqi W. Ma, Hao Peng
- **Venue**: arXiv 2025
- **Read date**: 2026-04-21
- **Tags**: data-selection, instruction-tuning, influence, balanced-learning, bids

## One-line Summary

BIDS는 influence-based instruction data selection이 특정 high-influence tasks로 치우치는 문제를 지적하고, influence normalization과 underrepresented task 중심 iterative selection으로 diverse capabilities를 균형 있게 학습시키는 방법이다.

## Key Idea

LESS 같은 influence-based selection은 target capability를 잘 끌어올릴 수 있지만, 여러 capability를 동시에 잘 학습해야 하는 instruction tuning에서는 imbalance를 만들 수 있다. 논문은 raw influence magnitude가 task마다 intrinsic하게 다르기 때문에, top-influence examples를 고르면 high-influence task가 과대표집된다고 본다.

흥미로운 점은 이런 과대표집이 low-influence tasks만 해치는 것이 아니라, redundancy 때문에 high-influence tasks 성능까지 해칠 수 있다는 분석이다.

## Method

- training examples와 validation instances 사이의 attribution matrix를 만든다.
- validation set은 여러 task에서 같은 수의 instances를 뽑아 구성한다.
- LESS pipeline처럼 LoRA-based low-dimensional gradient similarity로 influence를 계산한다.
- validation instance별 column-wise standardization으로 influence score를 정규화한다.
- selected subset의 평균 influence distribution을 추적하면서, 현재 underrepresented validation/task 방향을 가장 크게 보완하는 candidate를 greedy하게 추가한다.

## Experiments

UltraInteract dataset에서 Llama-3-8B와 Mistral-7B-v0.3을 instruction tuning하고, coding, math, logical inference, world knowledge, instruction following을 포함한 5개 capability와 7개 benchmarks에서 평가한다. 5%, 10%, 15% selection budget에서 Task-max/LESS, Sum, Instance-max, RDS 등과 비교한다.

## Results

arXiv/AlphaXiv 기준, BIDS는 5%, 10%, 15% budget 모두에서 macro-average performance가 가장 높고, 대부분의 individual benchmark에서도 상위권을 유지한다. 특히 15% BIDS subset으로 4 epochs 학습한 모델이 full 100% dataset training보다 macro-average에서 더 좋은 결과를 보인다.

Ablation은 instance-level normalization과 iterative underrepresented-task selection이 둘 다 중요함을 보인다. 단순히 influence를 많이 받는 task만 고르는 것이 아니라, 선택된 subset의 capability coverage를 동적으로 맞추는 것이 핵심이다.

## Discussion

BIDS는 사용자의 "downstream task에 따라 도움이 되는 데이터들이 상이함"과 직접 연결된다. 다만 단일 benchmark-target selection보다는 multi-capability 균형 학습이 목적이다. 따라서 LESS/GIST 같은 target influence selection의 보완책으로 보면 좋다.

연구 설계상 target validation set을 여러 task로 구성할 때는 raw influence aggregation만 쓰면 안 되고, task별 score scale normalization과 coverage objective가 필요하다는 근거가 된다.

## Limitations

- validation tasks와 benchmark suite 설계에 따라 balanced objective가 달라진다.
- influence normalization이 실제 task 난이도나 중요도를 모두 반영하지는 않는다.
- multi-capability balance가 목적이라, 단일 target task 최적화에서는 LESS/GIST보다 불리할 수 있다.

## References

- arXiv: <https://arxiv.org/abs/2501.12147>
- AlphaXiv: <https://www.alphaxiv.org/abs/2501.12147>
- arXiv HTML: <https://arxiv.org/html/2501.12147>
