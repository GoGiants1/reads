# MATES: Model-Aware Data Selection for Efficient Pretraining with Data Influence Models

- **Paper**: MATES: Model-Aware Data Selection for Efficient Pretraining with Data Influence Models
- **arXiv**: [2406.06046v2](https://arxiv.org/abs/2406.06046v2)
- **Authors**: Zichun Yu, Spandan Das, Chenyan Xiong
- **Venue**: NeurIPS 2024
- **Read date**: 2026-04-21
- **Tags**: data-selection, pretraining, dynamic-selection, data-influence, model-aware

## One-line Summary

MATES는 pretraining 중 모델의 데이터 선호가 계속 바뀐다고 보고, 현재 모델을 local probing해 얻은 oracle influence를 작은 data influence model로 근사한 뒤 다음 stage 데이터를 동적으로 선택한다.

## Key Idea

기존 pretraining data selection은 hand-crafted rules나 큰 reference model에 의존하고, 대부분 학습 전에 고정된다. MATES는 pretraining model의 현재 상태에 따라 어떤 데이터가 도움이 되는지 달라진다고 본다.

따라서 data influence model을 주기적으로 fine-tune해, 현재 pretraining progress에서 가장 유효한 데이터를 다음 stage에 선택한다.

## Method

- pretraining 중 현재 모델을 local probing해 oracle data preference/influence signal을 수집한다.
- 작은 data influence model을 fine-tune해 oracle signal을 근사한다.
- 이 influence model이 전체 corpus의 sample influence를 예측한다.
- 예측 influence가 높은 데이터를 다음 pretraining stage에 사용한다.

## Experiments

Pythia 계열 410M 및 1B 모델을 C4 dataset에서 pretraining하며, zero-shot/few-shot downstream tasks로 평가한다. random selection, larger reference model 기반 data selection 등과 비교한다.

## Results

NeurIPS/arXiv 초록 기준, MATES는 random data selection보다 크게 우수하고, larger reference model 기반 selection의 gain을 두 배 수준으로 높인다. 특정 성능에 도달하는 데 필요한 total FLOPs를 절반가량 줄인다고 보고한다.

## Discussion

사용자의 "학습 step마다 도움이 되는 데이터가 다르다"는 주장에 가장 직접적인 근거가 되는 논문이다. MATES는 selection을 offline static filtering으로 보지 않고, model state에 conditioned된 dynamic policy로 본다.

다만 완전 online이라기보다는 stage-wise probing과 influence model update를 반복하는 방식이다. 이후 TiKMiX의 semi-offline dynamic mixture와 함께 보면 dynamic data selection의 설계 공간이 잘 보인다.

## Limitations

- local probing과 influence model update 비용이 있다.
- probing signal이 noisy하거나 stage 간 transfer가 약하면 selection quality가 떨어질 수 있다.
- C4/Pythia 중심 실험이므로 더 큰 heterogeneous corpus와 frontier-scale 모델에서 추가 검증이 필요하다.

## References

- arXiv: <https://arxiv.org/abs/2406.06046>
- NeurIPS: <https://proceedings.neurips.cc/paper_files/paper/2024/hash/c4bec0d2fd217e6c2c3eafeced432582-Abstract-Conference.html>
- Code: <https://github.com/cxcscmu/MATES>
