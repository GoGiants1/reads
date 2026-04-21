# Concepts or Skills? Rethinking Instruction Selection for Multi-modal Models

- **Paper**: Concepts or Skills? Rethinking Instruction Selection for Multi-modal Models
- **arXiv**: [2508.10339v1](https://arxiv.org/abs/2508.10339v1)
- **Authors**: Andrew Bai, Justin Cui, Ruochen Wang, Cho-Jui Hsieh
- **Venue**: arXiv 2025
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, instruction-selection, concepts, skills, visual-instruction-tuning

## One-line Summary

이 논문은 VLM instruction tuning에서 benchmark마다 visual concepts가 중요한 경우와 visual skills가 중요한 경우가 다르며, target benchmark의 dominant factor에 맞춰 instruction을 선택해야 한다고 주장한다.

## Key Idea

기존 multimodal instruction selection은 sample embedding을 주로 사용하며, 이는 visual concepts를 잘 담지만 counting, spatial reasoning, commonsense inference 같은 visual skills는 충분히 반영하지 못한다. 논문은 benchmark behavior가 concept-driven인지 skill-driven인지 나뉜다고 보고, target benchmark에 맞는 factor를 골라 instruction selection을 수행한다.

사용자 메모의 "downstream task에 따라 도움이 되는 데이터가 다름"과 직접 연결되지만, 주로 multimodal instruction selection이므로 메인 LLM pretraining selection 범위에서는 보조 관련 연구로 두는 것이 적절하다.

## Method

- benchmark에서 concept와 skill signals를 추출한다.
- 해당 benchmark가 concept similarity와 skill similarity 중 어느 쪽에서 더 이득을 보는지 판별한다.
- target factor에 맞춰 instruction data를 선택한다.
- 선택된 subset으로 VLM instruction tuning을 수행한다.

## Experiments

10개 이상의 multimodal benchmark에서 평가한다. concept-focused benchmark와 skill-focused benchmark를 나눠, 기존 selection baseline과 평균 성능 및 skill subset 성능을 비교한다.

## Results

arXiv 초록 기준, 제안한 targeted selection은 전체 benchmark 평균에서 best existing baseline 대비 +0.9%, skill-focused subset에서 +1.5% 개선을 보인다. OpenReview 버전은 12개 benchmark와 skill-focused subset +1.2% 개선을 보고한다.

## Discussion

이 논문은 target benchmark가 요구하는 능력을 "concept vs skill" 축으로 분석한다는 점에서 유용하다. LESS/BETR/TADS가 target examples와 source examples의 alignment를 묻는다면, 이 논문은 alignment를 어떤 semantic axis에서 봐야 하는지 묻는다.

멀티모달 데이터 선택에서는 단순 visual similarity가 skill transfer를 놓칠 수 있다는 경고로 읽을 수 있다.

## Limitations

- concept/skill 분해가 heuristic 또는 model-dependent할 수 있다.
- benchmark를 하나의 dominant factor로 분류하는 과정이 복합 능력을 단순화할 수 있다.
- LLM-only instruction selection이나 pretraining corpus selection으로 바로 일반화되지는 않는다.

## References

- arXiv: <https://arxiv.org/abs/2508.10339>
- OpenReview: <https://openreview.net/forum?id=yB09CcjoII>
