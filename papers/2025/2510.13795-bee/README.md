# Bee: A High-Quality Corpus and Full-Stack Suite to Unlock Advanced Fully Open MLLMs

- **Paper**: Bee: A High-Quality Corpus and Full-Stack Suite to Unlock Advanced Fully Open MLLMs
- **arXiv**: [2510.13795v4](https://arxiv.org/abs/2510.13795v4)
- **Authors**: Yi Zhang, Bolin Ni, Xin-Sheng Chen, Heng-Rui Zhang, Yongming Rao, Houwen Peng, Qinglin Lu, Han Hu, Meng-Hao Guo, Shi-Min Hu
- **Venue**: ICLR 2026
- **Read date**: 2026-04-26
- **Tags**: mllm, dataset, data-curation, cot, fully-open, supervised-finetuning

## One-line Summary

Bee는 HoneyPipe라는 공개 data curation pipeline으로 noise를 제거하고 dual-level CoT를 주입한 Honey-Data-15M을 만들고, 이를 통해 fully open MLLM Bee-8B를 강한 수준으로 끌어올린다.

## Key Idea

Fully open MLLM이 proprietary/semi-open 모델보다 뒤처지는 핵심 이유 중 하나는 SFT 데이터 품질과 투명한 data pipeline의 부족이다. Bee는 단순히 데이터셋 하나를 공개하는 대신, 데이터셋, curation pipeline, DataStudio framework, training recipe, evaluation harness, model weights를 함께 제공하는 full-stack suite를 목표로 한다.

논문의 중심 주장은 "데이터 양"보다 "노이즈 제거, reasoning enrichment, 검증 가능한 pipeline"이 fully open MLLM 격차를 줄이는 더 현실적인 경로라는 것이다.

## Method

HoneyPipe는 약 24M image-text pair에서 시작해 다음 과정을 거친다.

- **Aggregation and deduplication**: LLaVA-OneVision, PixMo, MAmmoth-VL 등 공개 소스에서 데이터를 모으고 중복을 제거한다.
- **Data classification**: General, Chart, OCR, STEM 등 domain label을 붙여 이후 처리를 다르게 한다.
- **Noise and irrelevance filtering**: rule-based filter와 Qwen2.5-VL-72B 기반 model filter로 formatting, 반복, 이미지-질문 불일치, answerability 문제를 제거한다.
- **Short CoT enrichment**: 중간 난이도 샘플에 짧은 step-by-step reasoning을 추가한다.
- **Fidelity verification**: 생성된 CoT 결론과 원래 답변의 의미 일치 여부를 LLM-as-judge로 검증한다.
- **Long CoT enrichment loop**: 더 복잡한 샘플이나 short CoT 검증 실패 샘플에 긴 multi-step reasoning을 생성하고 다시 검증한다.

최종 Honey-Data-15M은 약 12.1M short CoT와 2.9M long CoT 샘플을 포함한다. Bee-8B는 MLP warmup, vision-language alignment, Honey-Data-15M SFT, Honey-Data-1M refinement, GRPO의 5-stage training으로 학습된다.

## Experiments

Bee-8B를 fully open 및 semi-open MLLM들과 비교한다. Benchmark는 MMMU, MMMU-Pro, MMStar, ChartQA, CharXiv, MathVerse, LogicVista, DynaMath, OCR/Document QA 등 광범위하다. Ablation은 HoneyPipe의 filtering/CoT enrichment와 Honey-Data-1M refinement 효과를 분리한다.

## Results

OpenReview/AlphaXiv 기준 Bee-8B는 fully open MLLM 중 SOTA를 주장하며, 일부 semi-open 8B 모델과도 경쟁한다. 주요 수치로 MMMU 66.8, MMMU-Pro 50.7, MMStar 71.4, ChartQA 86.7, CharXiv-DQ 84.8, CharXiv-RQ 57.3, MathVerse 67.0 등이 보고된다.

Ablation은 `D_curated > D_no-CoT > D_raw` 계층을 보인다. 즉, noise filtering/data selection만으로도 raw data 대비 이득이 있고, CoT enrichment가 reasoning-heavy benchmark에서 추가 이득을 만든다. Honey-Data-1M도 random 1M subset보다 우수해, 작은 refinement subset의 curated distribution이 중요함을 보인다.

## Discussion

Bee는 이번 묶음에서 가장 "dataset engineering" 색이 강하다. COMPACT가 small synthetic VIT data recipe라면, Bee는 대규모 open SFT corpus와 pipeline 공개를 통해 fully open ecosystem의 data gap을 줄이려는 시도다.

특히 정적 데이터셋 공개를 넘어 pipeline과 filtering prompt, verification, evaluation harness까지 공개하려는 점이 중요하다. 향후 MLLM data 연구에서는 dataset 자체보다 "어떤 curation process가 재현 가능하고 확장 가능한가"가 더 중요한 비교 축이 될 수 있다.

## Limitations

- 공개 데이터 소스의 bias와 licensing 제약을 상속한다.
- LLM-as-judge 기반 fidelity verification은 strict해서 유효한 CoT도 버릴 수 있다.
- 대규모 filtering/enrichment에는 강한 MLLM과 상당한 계산 비용이 필요하다.
- Honey-Data-15M은 CC-BY-NC-4.0 계열 제약이 있어 상업적 사용과 재배포 조건을 확인해야 한다.

## References

- arXiv: <https://arxiv.org/abs/2510.13795>
- OpenReview: <https://openreview.net/forum?id=IVluwK8q9q>
- AlphaXiv: <https://www.alphaxiv.org/hi/overview/2510.13795v3>
- Project: <https://open-bee.github.io>
