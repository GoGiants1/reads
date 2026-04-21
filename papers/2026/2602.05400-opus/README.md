# OPUS: Towards Efficient and Principled Data Selection in Large Language Model Pre-training in Every Iteration

- **Paper**: OPUS: Towards Efficient and Principled Data Selection in Large Language Model Pre-training in Every Iteration
- **arXiv**: [2602.05400v2](https://arxiv.org/abs/2602.05400v2)
- **Authors**: Shaobo Wang, Xuan Ouyang, Tianyi Xu, Yuzheng Hu, Jialin Liu, Guo Chen, Tianyu Zhang, Junhao Zheng, Kexin Yang, Xingzhang Ren, Dayiheng Liu, Linfeng Zhang
- **Venue**: arXiv 2026
- **Read date**: 2026-04-21
- **Tags**: data-selection, pretraining, dynamic-selection, optimizer-aware, llm

## One-line Summary

OPUS는 raw gradient가 아니라 AdamW/Muon 같은 현대 optimizer가 만드는 effective update space에서 sample utility를 정의하고, 매 iteration마다 target proxy 방향과 잘 맞는 pretraining data를 선택하는 dynamic LLM data selection 방법이다.

## Key Idea

논문은 LLM pretraining이 "더 많은 token"보다 "더 좋은 token"을 골라야 하는 Data Wall 국면에 들어섰다고 본다. 기존 static filter는 학습 중 모델 상태 변화를 무시하고, 기존 dynamic selection은 raw gradient 기반이라 현대 optimizer의 preconditioning을 반영하지 못한다.

OPUS의 핵심은 데이터의 유용성을 실제 optimizer-induced update space에서 재정의하는 것이다. 즉 sample gradient 자체가 아니라, optimizer가 실제 parameter update로 바꾼 effective update가 target proxy direction에 얼마나 잘 정렬되는지를 본다.

## Method

- BENCH-PROXY: benchmark validation set과 pretraining corpus 문서를 frozen text encoder로 embedding하고, benchmark와 가까운 in-distribution documents를 proxy pool로 만든다.
- Optimizer-induced utility: 후보 sample의 effective update를 proxy gradient direction에 projection해 alignment score를 계산한다.
- Redundancy penalty: 이미 선택된 samples의 누적 update direction과 중복되는 후보를 낮게 평가한다.
- Ghost technique: full gradient를 materialize하지 않고 linear layer gradient의 low-rank factor를 사용한다.
- CountSketch: optimizer-preconditioned update를 저차원 sketch space로 project해 inner product 계산 비용을 줄인다.
- Boltzmann sampling: greedy top-k 대신 soft sampling을 사용해 high-utility data를 선호하면서 diversity collapse를 줄인다.

## Experiments

from-scratch pretraining에서는 FineWeb과 FineWeb-Edu에서 GPT-2 Large/XL을 30B update tokens로 학습하고, random/static filter/dynamic baseline과 비교한다. optimizer는 Muon과 AdamW를 모두 다룬다. continued pretraining에서는 Qwen3-8B-Base를 SciencePedia로 domain adaptation하며, 0.5B token OPUS와 3B token full/random training을 비교한다.

## Results

arXiv 초록 기준, OPUS는 FineWeb/FineWeb-Edu 30B-token pretraining에서 industrial-level baseline과 full 200B-token training까지 능가한다. AlphaXiv 요약 기준으로는 GPT-2 XL + Muon에서 FineWeb 평균 accuracy 41.75%를 기록해 random 40.29%보다 높고, FineWeb-Edu에서는 낮은 quality tier에서 골라도 GPT-2 XL + Muon 평균 44.99%로 baseline을 앞선다.

continued pretraining에서는 Qwen3-8B-Base가 SciencePedia 0.5B token만으로 3B token full training보다 우수한 SciAssess/OlympicArena 성능을 보여, domain adaptation에서 약 6x data efficiency를 보고한다. 전체 overhead는 Ghost/CountSketch 덕분에 약 4.7% 추가 compute로 제한된다.

## Discussion

OPUS는 MATES/TiKMiX 계열의 dynamic pretraining selection과 직접 연결되지만, 중요한 차이는 optimizer-aware라는 점이다. MATES가 model-aware influence model, TiKMiX가 group influence 기반 mixture update라면, OPUS는 매 iteration candidate의 optimizer-shaped update가 target proxy direction을 얼마나 밀어주는지를 본다.

사용자 연구 관점에서는 "학습 step마다 도움이 되는 데이터가 다르다"를 더 강하게 지지하는 최신 reference다. 특히 dynamic selection이 practical하지 않다는 반론에 대해 Ghost + CountSketch + Boltzmann sampling으로 overhead를 4.7%까지 낮춘 설계가 설득력 있다.

## Limitations

- BENCH-PROXY는 benchmark validation에 가까운 corpus 문서를 proxy로 쓰므로, proxy construction과 decontamination 설계가 중요하다.
- Hessian/redundancy 항은 근사되므로, utility score가 실제 long-horizon training effect를 완전히 보장하지는 않는다.
- 매 iteration selection을 실제 대규모 distributed pretraining pipeline에 넣을 때 data loading, buffering, reproducibility 비용을 별도 관리해야 한다.

## References

- arXiv: <https://arxiv.org/abs/2602.05400>
- AlphaXiv: <https://www.alphaxiv.org/abs/2602.05400>
- arXiv HTML: <https://arxiv.org/html/2602.05400>
