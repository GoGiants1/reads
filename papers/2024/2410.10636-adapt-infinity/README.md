# Adapt-infinity: Scalable Continual Multimodal Instruction Tuning via Dynamic Data Selection

- **Paper**: Adapt-infinity: Scalable Continual Multimodal Instruction Tuning via Dynamic Data Selection
- **arXiv**: [2410.10636v2](https://arxiv.org/abs/2410.10636v2)
- **Authors**: Adyasha Maharana, Jaehong Yoon, Tianlong Chen, Mohit Bansal
- **Venue**: ICLR 2025
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, continual-learning, visual-instruction-tuning, online, dynamic-selection

## One-line Summary

Adapt-infinity는 계속 추가되는 multimodal instruction datasets에서 pseudo-skill cluster별 selector expert를 동적으로 고르고, cluster-wise permanent pruning으로 dataset pool 증가를 제어하는 continual multimodal instruction tuning 방법이다.

## Key Idea

이 논문은 static offline selection이 아니라 lifelong instruction tuning을 다룬다. 데이터가 시간에 따라 추가되고 task distribution도 변하기 때문에, 한 번 정한 static importance measure만으로는 충분하지 않다고 본다.

분류상 **online/continual / model-involved / dynamic data selection / pruning**이다. 사용자의 메모처럼 main offline comparison에서는 out of scope로 두는 것이 맞다.

## Method

Adapt-infinity의 구성은 다음과 같다.

- gradient-based sample vectors로 pseudo-skill clusters를 구성한다.
- 각 cluster마다 여러 selector experts 중 가장 적합한 selector를 선택한다.
- 새로 제안한 Image Grounding score도 selector expert pool에 포함된다.
- 선택된 subset으로 continual instruction tuning을 수행한다.
- dataset pool이 계속 커지는 것을 막기 위해 cluster-wise permanent pruning으로 semantic redundancy를 제거한다.

## Experiments

ICLR 2025 논문은 VQA, knowledge VQA, multilingual, grounding, reasoning, language-only, multi-image comprehension 등 다양한 multimodal instruction datasets가 순차적으로 들어오는 설정에서 평가한다.

## Results

Adapt-infinity는 원본 데이터의 일부만 사용하면서 catastrophic forgetting을 완화하고, rare tasks에서 특히 forgetting을 줄이며, forward transfer를 개선한다고 보고한다.

## Discussion

offline data selection과 비교하면 문제 정의가 다르다. ScalSelect, PRISM, COINCIDE 등은 주어진 candidate pool에서 subset을 고르는 반면, Adapt-infinity는 현재까지 학습한 지식과 앞으로 들어오는 stream 사이에서 selection/pruning policy를 갱신한다.

따라서 survey에서는 "online/continual out-of-scope but useful background"로 분류하면 된다.

## Limitations

- continual stream/task order에 민감할 수 있다.
- pseudo-skill clustering과 selector expert 선택이 추가 complexity를 만든다.
- offline VIT 데이터셋 하나를 줄이는 baseline과 직접 비교하면 공정하지 않다.

## References

- arXiv: <https://arxiv.org/abs/2410.10636>
- ICLR proceedings: <https://proceedings.iclr.cc/paper_files/paper/2025/hash/a6610efd6c767f63343a4ab28505212e-Abstract-Conference.html>
- Code: <https://github.com/adymaharana/adapt-inf>
