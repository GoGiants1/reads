# Human Uncertainty-Aware Data Selection and Automatic Labeling in Visual Question Answering

- **Paper**: Human Uncertainty-Aware Data Selection and Automatic Labeling in Visual Question Answering
- **arXiv**: [2510.11295v2](https://arxiv.org/abs/2510.11295v2)
- **Authors**: Jian Lan, Zhicheng Liu, Udo Schlegel, Raoyuan Zhao, Yihong Liu, Hinrich Schuetze, Michael A. Hedderich, Thomas Seidl
- **Venue**: ICLR 2026
- **Read date**: 2026-04-26
- **Tags**: mllm, vqa, data-selection, human-uncertainty, calibration, automatic-labeling

## One-line Summary

HaDola는 VQA annotation의 human uncertainty를 데이터 선택과 pseudo-labeling 신호로 사용해, 소량의 seed annotation만으로 더 정확하고 calibrated된 VLM fine-tuning을 만든다.

## Key Idea

VQA 데이터에는 여러 annotator가 서로 다른 답이나 confidence를 보이는 human uncertainty(HU)가 있다. 표준 SFT는 majority label만 맞추기 때문에, high-HU 샘플이 실제로 학습에 도움이 되는지와 모델이 사람의 불확실성 분포를 반영하는지는 놓친다.

논문은 high-HU 샘플이 성능을 거의 개선하지 않거나 오히려 악화할 수 있고, full-data SFT가 calibration을 망칠 수 있음을 보인 뒤, HU를 selection과 labeling에 직접 사용한다.

## Method

HaDola는 네 단계로 동작한다.

- **Discriminate**: 5% 정도의 HU-annotated seed set으로 reference model을 만들고, seed의 human distribution과 model distribution 사이의 KL 기준으로 low/medium-HU trust region을 잡아 high-HU 샘플을 배제한다.
- **Self-Annotate**: retained unlabeled sample에 대해 현재 모델이 pseudo label을 생성한다.
- **Error Trigger**: gradient consistency와 TracIn-mini류 영향 신호로 pseudo label error accumulation을 감지하고 교정한다.
- **Training**: cross entropy와 uncertainty alignment 항을 함께 쓰는 HaDola loss로 모델을 업데이트한다.

목표는 단순히 annotation 수를 줄이는 것이 아니라, "불확실하지만 유익한" 샘플과 "논쟁적이라 학습을 흐리는" 샘플을 구분하는 것이다.

## Experiments

VQAv2와 VizWiz에서 Qwen2.5-VL-7B, LLaVA1.6-7B, InternVL2.5-8B, BEiT3 등을 사용한다. 평가는 일반 VQA accuracy 대신 HU-aware accuracy와 human distribution과의 KL divergence를 함께 본다.

비교 대상은 full SFT, random subset, active learning, DPO-style VQA training, calibration baseline 등이다.

## Results

논문은 low-HU, medium-HU, high-HU subset 순으로 학습 효과가 떨어지고, high-HU 데이터를 추가하면 성능과 calibration이 악화될 수 있음을 보인다. HaDola는 여러 VLM과 두 데이터셋에서 baseline과 같거나 더 높은 accuracy를 보이면서 KL divergence를 낮춘다.

Training round 분석에서는 5-10% 구간에서 성능이 빠르게 오르고, 15% 이후 추가 annotation 이득이 포화되는 경향을 보고한다. Ablation에서는 selector를 random으로 바꾸거나, self-annotation/error trigger/loss를 제거하면 VQAv2와 VizWiz 모두에서 성능이 크게 하락한다.

## Discussion

이 논문은 MLLM/VQA data selection을 "정답 label 품질"이 아니라 "인간 응답 분포의 신뢰도" 문제로 본다. 기존 multimodal data selection 논문들이 diversity, influence, capability coverage를 보았다면, HaDola는 annotation uncertainty 자체를 sample reliability와 calibration target으로 사용한다.

특히 public VQA 데이터처럼 annotator disagreement가 있는 데이터에서는 high-HU를 무조건 버릴지, calibration supervision으로 쓸지의 구분이 중요하다. HaDola는 이를 small seed와 iterative self-training으로 절충한다.

## Limitations

- 잘 구성된 HU-annotated seed set이 필요하다.
- VQA처럼 여러 annotator 답변 분포가 존재하는 데이터셋에 특히 잘 맞으며, 일반 VIT 데이터로 확장하려면 HU proxy가 필요하다.
- high-HU가 항상 나쁜 데이터라는 뜻은 아니며, 실제 ambiguity나 복수 정답을 포함할 수 있다.
- self-annotation 품질은 초기 모델과 error trigger의 신뢰도에 의존한다.

## References

- arXiv: <https://arxiv.org/abs/2510.11295>
- OpenReview: <https://openreview.net/forum?id=LuZjiUNuFL>
- AlphaXiv: <https://www.alphaxiv.org/overview/2510.11295/metadatav2>
