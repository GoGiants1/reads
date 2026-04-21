# Cram Less to Fit More: Training Data Pruning Improves Memorization of Facts

- **Paper**: Cram Less to Fit More: Training Data Pruning Improves Memorization of Facts
- **arXiv**: [2604.08519v1](https://arxiv.org/abs/2604.08519v1)
- **Authors**: Jiayuan Ye, Vitaly Feldman, Kunal Talwar
- **Venue**: arXiv 2026
- **Read date**: 2026-04-21
- **Tags**: data-selection, data-pruning, factual-memorization, information-theory, llm

## One-line Summary

이 논문은 모델 capacity보다 많은 고엔트로피 facts를 학습시키면 fact accuracy가 capacity limit 아래로 떨어질 수 있으며, loss 기반 pruning으로 fact 수와 frequency skew를 조절하면 작은 모델도 더 많은 facts를 정확히 기억할 수 있음을 보인다.

## Key Idea

LLM hallucination의 한 원인은 factual knowledge를 parameter 안에 충분히 정확히 저장하지 못하는 것이다. 논문은 이를 단순한 모델 크기 문제가 아니라 training data distribution 문제로 본다. 데이터에 포함된 fact 정보량이 모델 capacity를 넘거나, fact frequency가 power-law처럼 심하게 치우치면 standard training은 일부 facts도 완전히 정확히 기억하지 못한다.

따라서 "더 많이 cram"하는 대신, 모델 capacity에 맞게 fact 수를 제한하고 반복 빈도를 flatten하는 data pruning이 필요하다는 것이 핵심 주장이다.

## Method

- facts를 question-answer mapping으로 정의하고, fact memorization을 facts와 trained model 사이 mutual information으로 formalize한다.
- Fano's inequality를 사용해 model parameter space가 fact accuracy에 주는 capacity bound를 도출한다.
- semi-synthetic phonebook facts와 real-world annotated Wikipedia facts로 standard training의 suboptimal fact accuracy를 측정한다.
- training loss만을 이용하는 LossH/LossHF selection을 제안해, high-loss/head facts와 frequency flattening 효과를 결합한다.
- GPT-2 style decoder-only model과 LoRA finetuning setting에서 pruning 효과를 검증한다.

## Experiments

합성 phonebook facts에서는 fact 수와 frequency distribution을 uniform/power-law로 조절한다. real-world setting에서는 annotated Wikipedia corpus를 사용해 entity facts memorization을 평가한다. 모델 크기는 GPT2-Small 110M부터 1.3B급까지 비교하며, LoRA adapter capacity 제한 상황도 분석한다.

## Results

arXiv 초록 기준, loss 기반 selection은 high-entropy facts가 있는 semi-synthetic dataset에서 fact accuracy를 theoretical capacity limit까지 끌어올린다. Wikipedia pretraining에서는 GPT2-Small 110M이 standard training 대비 1.3배 더 많은 entity facts를 기억하며, full dataset으로 학습한 10배 큰 1.3B 모델과 맞먹는 성능을 보인다.

AlphaXiv 요약 기준, full data에서 110M 모델은 fact 수가 capacity를 넘으면 accurate fact count가 급락하고, 더 오래 학습해도 거의 회복되지 않는다. 반면 1.4B 모델은 같은 데이터를 거의 완전히 기억해 capacity bottleneck임을 뒷받침한다.

## Discussion

이 논문은 data selection을 "quality filtering"이 아니라 "model capacity에 맞춘 information allocation" 문제로 해석한다. OPUS/MATES처럼 dynamic training utility를 보는 계열과는 다르지만, 왜 적은 데이터가 더 좋은지 이론적 근거를 제공한다.

사용자 연구 관점에서는 benchmark-target selection에서 target facts/skills가 너무 많이 섞이면 작은 모델이 오히려 아무것도 제대로 못 배울 수 있다는 경고로 읽을 수 있다. selection budget은 단순 compute budget이 아니라 memorization capacity와도 맞춰야 한다.

## Limitations

- fact boundary와 fact entropy를 명시하기 어려운 일반 web corpus에서는 loss proxy의 해석이 불완전할 수 있다.
- factual memorization 개선이 reasoning, instruction following, safety 등 다른 capability와 trade-off를 만들 수 있다.
- capacity bound와 pruning rule은 factual QA 중심이라, broader pretraining utility로 일반화하려면 추가 검증이 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2604.08519>
- AlphaXiv: <https://www.alphaxiv.org/abs/2604.08519>
- arXiv HTML: <https://arxiv.org/html/2604.08519>
