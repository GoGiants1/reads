# Train on Validation (ToV): Fast Data Selection with Applications to Fine-Tuning

- **Paper**: Train on Validation (ToV): Fast data selection with applications to fine-tuning
- **arXiv**: [2510.00386v1](https://arxiv.org/abs/2510.00386v1)
- **Authors**: Ayush Jain, Andrea Montanari, Eren Sasoglu
- **Venue**: ICLR 2026
- **Read date**: 2026-04-21
- **Tags**: data-selection, fine-tuning, validation-set, influence, instruction-tuning

## One-line Summary

ToV는 target validation set을 평가용으로만 쓰지 않고 그 위에서 먼저 fine-tuning한 뒤, training pool sample의 prediction 변화가 큰 샘플을 target에 유용한 데이터로 선택하는 빠른 data selection 방법이다.

## Key Idea

기존 방법은 target samples를 validation set으로 두고, candidate training sample을 넣거나 뺐을 때 validation loss가 얼마나 바뀌는지 추정한다. ToV는 역할을 뒤집는다. target validation set으로 모델을 잠깐 fine-tuning하고, 이 변화가 training pool의 어떤 샘플 prediction에 크게 반영되는지 본다.

직관은 target set으로 학습했을 때 민감하게 바뀌는 training samples가 target distribution에 가까우며, 실제 fine-tuning에도 도움이 된다는 것이다.

## Method

- base model에서 training pool에 대한 prediction을 저장한다.
- small target validation set으로 모델을 fine-tuning한다.
- fine-tuned model이 training pool sample에 대해 얼마나 prediction을 바꾸는지 측정한다.
- prediction change가 큰 샘플을 선택해 target fine-tuning에 사용한다.

## Experiments

instruction tuning과 named entity recognition task에서 평가한다. target distribution sample이 적은 상황을 중심으로, influence-function 계열 및 기존 data selection method와 test log-loss를 비교한다.

## Results

arXiv/OpenReview 초록 기준, ToV는 대부분의 경우 state-of-the-art selection method보다 낮은 target test log-loss를 달성한다. 또한 기존 influence estimation보다 단순하고 빠르며, 이 현상을 이론 분석으로도 뒷받침한다.

## Discussion

ToV는 benchmark-targeted selection에서 매우 직접적인 validation-driven 방법이다. "검증셋에 도움이 되는 training data"를 계산하려는 기존 접근과 목표는 같지만, 계산 방향을 뒤집어 비용을 줄인다.

사용자 연구 아이디어와 연결하면, validation/golden set을 target으로 둘 때 online gradient influence를 매번 계산하지 않고도 빠른 proxy를 만들 수 있는 후보 baseline이다.

## Limitations

- target validation set이 작고 편향되어 있으면 선택도 그 편향을 따른다.
- prediction change가 큰 샘플이 항상 positive transfer를 보장하지는 않는다.
- validation set을 학습에 쓰는 설계이므로 benchmark contamination/evaluation split을 엄격히 관리해야 한다.

## References

- arXiv: <https://arxiv.org/abs/2510.00386>
- OpenReview: <https://openreview.net/forum?id=fWHd3yYicX>
