# DoReMi: Optimizing Data Mixtures Speeds Up Language Model Pretraining

- **Paper**: DoReMi: Optimizing Data Mixtures Speeds Up Language Model Pretraining
- **arXiv**: [2305.10429v4](https://arxiv.org/abs/2305.10429v4)
- **Authors**: Sang Michael Xie, Hieu Pham, Xuanyi Dong, Nan Du, Hanxiao Liu, Yifeng Lu, Percy Liang, Quoc V. Le, Tengyu Ma, Adams Wei Yu
- **Venue**: NeurIPS 2023
- **Read date**: 2026-04-21
- **Tags**: data-mixture, pretraining, domain-reweighting, proxy-model, group-dro

## One-line Summary

DoReMi는 작은 proxy model을 Group DRO로 학습해 domain mixture weights를 찾고, 그 mixture로 큰 LM을 pretraining해 더 빠른 수렴과 downstream 성능 향상을 얻는 data mixture optimization 방법이다.

## Key Idea

pretraining corpus는 Wikipedia, books, web text 등 여러 domain의 mixture다. 어떤 domain 비중이 좋은지는 downstream task를 직접 튜닝하지 않아도, 작은 proxy model에서 domain별 excess loss를 최적화하면 추정할 수 있다는 것이 DoReMi의 핵심이다.

## Method

- domain-labeled corpus와 작은 proxy model을 준비한다.
- Group DRO 스타일 objective로 worst-domain excess loss를 줄이도록 proxy model을 학습한다.
- 학습 과정에서 domain weights를 추정한다.
- 추정된 weights로 dataset을 resample하고 full-sized LM을 pretraining한다.

## Experiments

The Pile과 GLaM dataset에서 280M proxy model로 weights를 찾고, 8B급 main model을 학습한다. default domain weights 및 downstream-tuned oracle weights와 비교한다.

## Results

arXiv/NeurIPS 초록 기준, DoReMi는 The Pile에서 모든 domain perplexity를 개선하고, default mixture 대비 average few-shot downstream accuracy를 6.5%p 높인다. baseline accuracy에는 2.6x fewer training steps로 도달한다. GLaM에서는 downstream tasks로 직접 튜닝한 weights와도 비슷한 성능을 낸다.

## Discussion

DoReMi는 OPUS의 sample-level dynamic selection과는 다르게, 학습 전에 domain-level mixture를 정하는 offline/proxy-model 방법이다. 그래도 "data mixture가 pretraining efficiency를 크게 바꾼다"는 기반 연구라서 TiKMiX, RegMix, OPUS를 이해하는 데 중요하다.

OPUS 관점에서는 static mixture optimization이 모델 상태와 iteration별 utility 변화를 반영하지 못한다는 비교 축으로 사용할 수 있다.

## Limitations

- domain labels와 domain partition이 필요하다.
- small proxy model에서 찾은 weights가 큰 model과 모든 compute scale에 항상 transfer된다고 보장되지는 않는다.
- sample-level quality나 within-domain diversity는 직접 다루지 않는다.

## References

- arXiv: <https://arxiv.org/abs/2305.10429>
- NeurIPS: <https://proceedings.neurips.cc/paper_files/paper/2023/hash/dcba6be91359358c2355cd920da3fcbd-Abstract-Conference.html>
- OpenReview: <https://openreview.net/forum?id=lXuByUeHhd>
