# ICONS: Influence Consensus for Vision-Language Data Selection

- **Paper**: ICONS: Influence Consensus for Vision-Language Data Selection
- **arXiv**: [2501.00654v4](https://arxiv.org/abs/2501.00654v4)
- **Authors**: Xindi Wu, Mengzhou Xia, Rulin Shao, Zhiwei Deng, Pang Wei Koh, Olga Russakovsky
- **Venue**: arXiv 2025
- **Read date**: 2026-04-21
- **Tags**: multimodal-data-selection, visual-instruction-tuning, influence, gradients, icons

## One-line Summary

ICONS는 first-order training dynamics로 각 샘플의 validation influence를 추정하고, task별 influence를 majority voting으로 모아 여러 task에서 일관되게 유용한 visual-language data를 고르는 gradient-based selection 방법이다.

## Key Idea

ICONS의 핵심은 task-agnostic heuristic이 다양한 task mixture에서 잘 작동하지 않는다는 점이다. 각 task에서 좋은 샘플이 다를 수 있고 score scale도 다르므로, task별 influence를 직접 평균내기보다 consensus voting으로 calibration/outlier 문제를 줄인다.

분류상 **offline / model-involved / gradient-based influence / cross-task consensus**다.

## Method

ICONS는 다음 흐름으로 작동한다.

- first-order training dynamics로 training example이 validation performance에 미치는 influence를 추정한다.
- task별 validation set 또는 task signal을 기준으로 influence score를 계산한다.
- task across aggregation은 단순 평균이 아니라 majority voting 기반 consensus로 수행한다.
- 여러 task에서 반복적으로 유용하다고 판단되는 샘플을 선택한다.

## Experiments

논문은 LLAVA-665K, CAMBRIAN-7M, VISION-FLAN-186K에서 각각 20% subset을 선택한다. 선택된 compact subsets로 LLAVA-ICONS-133K, CAMBRIAN-ICONS-1.4M, VISION-FLAN-ICONS-37K를 공개한다.

## Results

arXiv 초록 기준, 선택된 20% subset은 full dataset performance의 98.6%, 98.8%, 99.8%를 각각 유지한다. 또한 selected data가 unseen task와 model architecture로 generalize된다고 보고한다.

## Discussion

사용자 메모의 질문인 "ICONS가 IF-Score의 MLLM 버전인가?"에 대해서는 **부분적으로 맞지만 그대로 같지는 않다**고 보는 게 정확하다.

- 공통점: gradient/influence 신호로 training sample의 유용성을 추정한다.
- 차이점 1: PDS에서 비교되는 IF-Score는 downstream loss에 대한 influence function score가 중심이고, ICONS는 vision-language multitask mixture에서 task별 influence consensus를 만든다.
- 차이점 2: ICONS는 calibration과 outlier sensitivity를 줄이기 위해 majority voting을 핵심 aggregation으로 쓴다.
- 차이점 3: ICONS의 목적은 VLM instruction mixture selection이고, IF-Score baseline은 더 일반적인 LM pretraining/selection context에서 쓰인다.

따라서 "IF-style influence selection을 MLLM multitask setting에 맞게 consensus화한 방법"이라고 요약하는 편이 안전하다.

## Limitations

- gradient/influence 계산이 필요하므로 training-free 방법보다 비용이 크다.
- validation task 구성에 따라 selected subset이 달라질 수 있다.
- majority voting이 score calibration 문제를 줄이지만, rare task가 vote에서 묻히는지는 확인해야 한다.

## References

- arXiv: <https://arxiv.org/abs/2501.00654>
- arXiv HTML: <https://arxiv.org/html/2501.00654>
- Project/data: <https://github.com/princetonvisualai/icons>
