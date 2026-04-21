# Data Selection via Optimal Control for Language Models

- **Paper**: Data Selection via Optimal Control for Language Models
- **arXiv**: [2410.07064v2](https://arxiv.org/abs/2410.07064v2)
- **Authors**: Yuxian Gu, Li Dong, Hongning Wang, Yaru Hao, Qingxiu Dong, Furu Wei, Minlie Huang
- **Venue**: ICLR 2025 Oral
- **Read date**: 2026-04-21
- **Tags**: data-selection, language-models, optimal-control, pds, pretraining

## One-line Summary

PDS는 LM pretraining data selection을 optimal control 문제로 공식화하고, Pontryagin's Maximum Principle 조건을 proxy setting에서 근사해 downstream loss를 줄이는 high-quality corpus를 선택하는 방법이다.

## Key Idea

이 논문은 데이터 선택을 heuristic filtering이 아니라 training dynamics를 제어하는 문제로 본다. 목표는 massive corpus에서 어떤 데이터를 선택해야 LM training trajectory가 downstream capability에 유리해지는지를 이론적으로 도출하는 것이다.

분류상 **offline / model-involved / optimal-control / proxy training dynamics / LM pretraining data selection**이다. MLLM VIT 논문은 아니지만, RHO-Loss, DSIR, IF-Score와 함께 baseline 관점을 제공한다.

## Method

PDS의 흐름은 다음과 같다.

- data selection을 generalized optimal control 문제로 세운다.
- Pontryagin's Maximum Principle로 optimal data selection과 LM training dynamics 사이의 necessary conditions를 얻는다.
- 작은 proxy LM과 proxy dataset에서 PMP 조건을 근사해 data quality score를 계산한다.
- data scorer를 학습해 큰 CommonCrawl-scale corpus에 score를 transfer한다.
- score가 높은 corpus를 선택해 LM pretraining에 사용한다.

## Experiments

논문은 CommonCrawl에서 pretraining data를 선택하고, Mistral-family architecture의 160M, 470M, 1B, 1.7B LM으로 downstream evaluation을 수행한다. 또한 scaling-law extrapolation으로 약 400B model, 10T tokens 규모의 효과를 추정한다.

주요 baselines는 conventional pretraining, RHO-Loss, DSIR, IF-Score다.

## Results

arXiv 초록 기준, PDS-selected corpus는 다양한 model size에서 learning을 가속하고 downstream performance를 지속적으로 개선한다. 제한된 pretraining data 상황에서는 data demand를 1.8배 줄인다고 보고한다. ICLR 2025 Oral로 채택되었다.

## Discussion

MLLM data selection 비교에서 PDS 자체를 직접 baseline으로 쓰기는 어렵다. 그러나 optimal-control 논문에서 사용한 baseline 정의를 정리하는 데 중요하다.

- **RHO-Loss**: downstream model과 current model의 reducible loss 차이를 사용한다. 적용은 상대적으로 쉽지만 downstream proxy와 current loss가 필요하다.
- **DSIR**: target distribution과 raw corpus의 feature distribution matching이다. VIT에서는 image-text instruction distribution feature 설계가 까다로워 적용이 어렵다.
- **IF-Score**: downstream loss에 대한 influence function score다. ICONS와 가장 가깝지만, ICONS는 VLM multitask setting에서 cross-task consensus를 추가한다.

따라서 PDS note는 MLLM comparison table의 "optimal-control baseline family"를 설명하는 anchor로 쓰면 된다.

## Limitations

- LM pretraining corpus selection이므로 VLM instruction tuning으로 바로 옮기기 어렵다.
- proxy LM과 proxy data에서 얻은 score transfer가 핵심 assumption이다.
- PMP 근사는 구현이 복잡하고, MLLM training dynamics로 확장하려면 vision-language loss와 task mixture를 다시 정의해야 한다.

## References

- arXiv: <https://arxiv.org/abs/2410.07064>
- arXiv HTML: <https://arxiv.org/html/2410.07064>
- Code/model/data: <https://github.com/microsoft/LMOps/tree/main/data_selection>
