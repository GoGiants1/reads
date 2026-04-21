# The Universal Normal Embedding 정리

- **논문**: *The Universal Normal Embedding*
- **arXiv**: [2603.21786v1](https://arxiv.org/abs/2603.21786v1)
- **저자**: Chen Tasker, Roy Betser, Eyal Gofer, Meir Yossef Levi, Guy Gilboa
- **소속/venue**: Technion, CVPR 2026로 표시됨
- **확인 깊이**: AlphaXiv 페이지 + arXiv 원본 LaTeX source

## 한 줄 요약

이 논문은 **CLIP/DINO 같은 비전 인코더의 임베딩 공간과 Stable Diffusion 같은 생성 모델의 noise latent 공간이 사실상 하나의 공통 Gaussian latent geometry를 서로 다른 선형 투영으로 본 것일 수 있다**는 가설, 즉 **Universal Normal Embedding(UNE)**을 제안하고 실험적으로 뒷받침합니다.

## 핵심 아이디어

논문의 중심 가설은 두 단계입니다.

1. **UNE 가설**

   자연 이미지 데이터는 어떤 이상적인 Gaussian latent space `Z ~ N(0, I)`와 정보 보존적으로 연결되어 있고, 그 공간에서는 의미 속성, 예를 들어 미소, 나이, 성별, 수염 등이 **선형 방향**으로 표현된다는 주장입니다.

2. **INE 가설**

   실제 모델들의 latent는 이 이상적 UNE를 완전히 복원한 것이 아니라, 각 모델마다 다른 noisy linear projection이라는 주장입니다.

   즉 대략적으로:

   ```text
   model_i latent = C_i Z + noise
   ```

   여기서 `C_i`는 모델별 선형 변환입니다. 그래서 CLIP 임베딩, DINO 임베딩, DDIM-inverted Stable Diffusion noise가 서로 완전히 다른 것이 아니라, 같은 underlying semantic geometry의 다른 관측값이라는 관점입니다.

## 방법

저자들은 **NoiseZoo**라는 데이터셋을 구성합니다.

- CelebA validation 이미지 19,867장 사용
- train/test: 15,893 / 3,974
- 각 이미지마다 다음 latent를 추출
  - 생성 모델: SD 1.5, SD 2.1, LCMv7의 DDIM-inverted noise latent
  - 인코더: CLIP ViT-B/16, CLIP ViT-L/14, OpenCLIP B/16, OpenCLIP L/14, DINOv3
- Stable Diffusion latent는 `(4, 64, 64)`를 flatten해서 약 16k 차원으로 사용
- CelebA의 40개 attribute에 대해 latent space에서 logistic regression linear probe를 학습

## 주요 실험 결과

가장 중요한 결과는 네 가지입니다.

1. **latent들이 실제로 Gaussian에 가깝다**

   무작위 1D projection에 대해 Anderson-Darling, D'Agostino-Pearson, Shapiro-Wilk normality test를 수행했습니다. 생성 모델 latent는 약 95% 수준으로 Gaussian 판정을 받고, CLIP/OpenCLIP도 대체로 89-92%, DINOv3도 80%대 수준을 보입니다.

2. **생성 모델의 noise latent에도 의미가 선형적으로 들어 있다**

   DDIM inversion으로 얻은 noise만 가지고도 CelebA attribute를 꽤 잘 예측합니다. 즉 diffusion noise가 단순한 무작위 벡터가 아니라, "미소", "나이", "수염" 같은 의미 방향을 담고 있다는 주장입니다.

3. **생성 latent와 encoder latent는 선형 매핑으로 잘 정렬된다**

   SD/LCM latent를 CLIP/OpenCLIP/DINO 공간으로 ridge regression 선형 변환한 뒤, encoder 쪽에서 학습된 classifier를 그대로 적용해도 정확도 하락이 0.3 percentage point 미만입니다. 이건 두 공간이 의미적으로 상당히 compatible하다는 강한 근거입니다.

4. **linear direction으로 이미지 편집이 가능하다**

   학습한 attribute classifier의 weight 방향 `w`를 따라 latent를 이동합니다.

   ```text
   z_edit = z + alpha * w
   ```

   이렇게 하면 prompt나 fine-tuning 없이도 smile, age, gender 같은 속성을 조절할 수 있습니다. 또 특정 방향이 다른 속성과 얽혀 있을 때는 orthogonalization으로 spurious correlation을 줄입니다.

## 왜 중요한가

기존에는 보통 이렇게 나누어 생각했습니다.

- CLIP/DINO: 의미를 잘 담는 representation model
- Stable Diffusion: 이미지를 생성하는 generative model

이 논문은 둘 사이의 경계를 흐립니다. 생성 모델의 noise latent도 encoder embedding처럼 **선형적으로 의미를 담고 있으며**, 둘은 공통 Gaussian geometry의 다른 projection일 수 있다는 것입니다.

이 관점이 맞다면 다음 응용이 가능합니다.

- diffusion latent에서 더 단순한 semantic editing
- encoder와 generator 사이의 latent translation
- 여러 모델의 shared representation 추출
- 생성 모델 내부의 의미 구조 분석
- prompt 없이 attribute-level control

## 왜 이런 현상이 나타나는가

논문은 CLIP, DINO, diffusion이 왜 비슷한 latent geometry를 갖게 되는지에 대해 완전한 이론적 증명을 주지는 않습니다. 대신 다음과 같은 설명 구조를 제시합니다.

CLIP은 text-image pair contrastive learning으로 학습되고, DINO는 self-supervised image representation learning으로 학습되며, diffusion model은 score matching 또는 noise prediction objective로 학습됩니다. 목적함수는 다르지만 세 모델 계열 모두 **같은 자연 이미지 분포**를 다룹니다. 따라서 모델들이 해결해야 하는 공통 문제는 결국 이미지 안의 semantic factor들을 어떤 좌표계로 압축하고 조직할 것인가입니다.

논문의 관점에서는 이 공통 factor space가 대략 Gaussian이고, 각 모델의 latent는 그 공간의 noisy linear projection입니다. 즉 서로 다른 학습 objective는 같은 underlying data distribution을 서로 다른 방식으로 관측하게 만들지만, 핵심 semantic factor는 비슷한 geometry로 수렴할 수 있다는 주장입니다.

특히 DDIM latent 해석이 중요합니다. diffusion sampling의 초기 noise는 무작위 샘플처럼 보이지만, **DDIM inversion으로 특정 real image를 역변환해서 얻은 noise는 단순한 랜덤 벡터가 아니라 그 이미지를 재구성할 수 있는 image-specific coordinate**입니다. 그래서 해당 noise latent 안에 이미지의 의미 속성이 들어 있을 수 있습니다.

논문이 제시하는 주요 근거는 세 가지입니다.

1. **Gaussianization**

   diffusion은 애초에 Gaussian prior와 noising process를 사용합니다. 한편 CLIP, OpenCLIP, DINO 같은 encoder embedding도 경험적으로 Gaussian-like한 좌표 분포를 보입니다. 논문은 random 1D projection normality test로 이를 확인합니다.

2. **semantic factor의 선형성**

   latent `Z`와 속성 `Y`가 joint Gaussian에 가깝다면 `E[Y | Z]`는 선형 함수가 됩니다. 따라서 smile, age, gender 같은 속성이 linear probe로 잘 읽히고, classifier weight 방향으로 이동하면 해당 속성 편집도 가능하다는 설명입니다.

3. **shared geometry와 identifiability**

   contrastive learning, self-supervised learning, generative modeling은 objective는 달라도 모두 데이터 생성 요인을 복원하거나 압축합니다. 기존 연구들에서는 서로 다른 encoder들이 선형 map으로 정렬되거나 representation들이 비슷한 geometry로 수렴한다는 결과가 있었고, 이 논문은 그 범위를 encoder뿐 아니라 diffusion generative latent까지 확장합니다.

다만 중요한 한계가 있습니다. 이 논문은 CLIP의 contrastive loss, DINO의 self-distillation/centering/sharpening, diffusion의 score matching objective 각각이 어떤 내부 메커니즘으로 동일한 Gaussian linear geometry를 강제하는지까지 분석하지는 않습니다. 따라서 답은 다음 정도로 정리할 수 있습니다.

> 서로 다른 학습 objective라도 같은 이미지 분포를 모델링하고, 그 분포의 의미 요인들이 Gaussian-like latent geometry에서 선형적으로 조직되기 때문에, 각 모델 latent는 같은 underlying space의 다른 projection처럼 보인다.

제일 설득력 있는 부분은 DDIM inversion입니다. inversion 후의 diffusion noise는 sampling용 순수 잡음이 아니라 이미지를 재구성하는 좌표가 되므로 semantic 정보가 들어 있는 것이 자연스럽습니다. 다만 그것이 왜 CLIP/DINO와 거의 선형 정렬될 정도로 같은 구조가 되는지는 아직 가설적이며, 저자들도 UNE-like geometry가 생기는 메커니즘 분석을 future work로 남깁니다.

## 전체 실험 과정

전체 실험은 "같은 이미지에 대해 여러 모델의 latent를 모아놓고, 이 latent들이 정말 공통 Gaussian semantic geometry처럼 행동하는지 확인"하는 흐름입니다.

### 1. NoiseZoo 데이터셋 구성

CelebA validation split의 **19,867개 얼굴 이미지**를 사용합니다. 별도 filtering은 하지 않았습니다.

각 이미지마다 두 종류의 latent를 추출합니다.

**생성 모델 latent**

- Stable Diffusion 1.5
- Stable Diffusion 2.1
- LCMv7

각 이미지에 대해 **DDIM inversion**을 수행해 이미지별 noise latent를 얻습니다.

세부 설정은 다음과 같습니다.

- 이미지는 center crop 후 `512 x 512`로 resize
- empty text prompt 사용
- classifier-free guidance 사용
- guidance scale `3.5`
- fixed seed `42`
- SD 1.5 / SD 2.1: 50 DDIM steps
- LCM: 150 steps, DDIMScheduler 사용
- 저장 latent shape: `(4, 64, 64)`, 이후 flatten해서 약 16k 차원 벡터로 사용

**인코더 latent**

- CLIP ViT-B/16: 512차원
- CLIP ViT-L/14: 768차원
- OpenCLIP ViT-B/16: 512차원
- OpenCLIP ViT-L/14: 768차원
- DINOv3 ViT-L/16: 768차원

인코더는 각 모델의 기본 preprocessing으로 이미지를 넣고 embedding을 추출합니다. embedding은 unit norm normalization 없이 그대로 사용합니다.

데이터 split은 다음과 같습니다.

- train: 15,893개
- test: 3,974개

이렇게 만든 per-image latent 모음이 **NoiseZoo**입니다.

### 2. Gaussianity 검증

UNE 가설의 첫 조건은 latent들이 Gaussian-like해야 한다는 것입니다.

각 모델 latent에 대해 다음을 수행합니다.

1. latent space에서 무작위 1D projection을 5,000개 샘플링
2. 각 projection마다 250개 sample 사용
3. normality test 수행
   - Anderson-Darling
   - D'Agostino-Pearson
   - Shapiro-Wilk
4. Gaussian으로 판정되는 projection 비율을 계산

결과적으로 SD 1.5, SD 2.1, LCM은 약 95% 근처, CLIP/OpenCLIP은 약 89-92%, DINOv3은 약 80%대의 Gaussian 판정 비율을 보입니다. 즉 diffusion latent와 encoder latent 모두 상당히 Gaussian-like하다고 주장합니다.

### 3. CelebA attribute linear probing

다음은 "semantic attribute가 latent에서 선형적으로 읽히는가?"를 봅니다.

CelebA의 40개 attribute label에 대해 각 모델 latent에서 attribute별 logistic regression classifier를 학습합니다. 예시는 smiling, male, young, eyeglasses, beard, bangs, blond hair 등입니다.

전처리는 다음과 같습니다.

- 생성 모델 latent: PCA 500 components
- encoder latent: PCA 310 components
- standard scaling
- attribute별 logistic regression
- solver: `saga`
- L2 regularization

비교 대상은 CLIP/OpenCLIP/DINO latent에서의 attribute prediction과 SD/LCM DDIM-inverted noise latent에서의 attribute prediction입니다.

핵심 관찰은 diffusion noise latent에서도 attribute가 꽤 잘 읽히며, attribute별 성능 패턴이 CLIP과 diffusion latent 사이에서 높게 상관된다는 것입니다. 즉 diffusion noise가 단순한 noise가 아니라 semantic information을 선형적으로 담고 있다는 결과입니다.

### 4. Cross-space transfer 실험

이 실험은 "생성 모델 latent와 encoder latent가 선형적으로 정렬 가능한가?"를 확인합니다.

절차는 다음입니다.

1. train set에서 같은 이미지에 대한 source latent와 target latent 쌍을 준비합니다.
   - source: SD 1.5, SD 2.1, LCM latent
   - target: CLIP B/16, OpenCLIP L/14, DINOv3 latent
2. source latent에서 target latent로 가는 ridge regression 선형 map을 학습합니다.

   ```text
   diffusion latent -> encoder latent
   ```

3. test set의 diffusion latent를 encoder space로 변환합니다.
4. 원래 encoder space에서 학습해둔 fixed attribute classifier를 변환된 latent에 적용합니다.

측정 지표는 MSE, cosine similarity, attribute classifier accuracy drop입니다.

결과적으로 cosine similarity가 꽤 높고, MSE가 낮으며, accuracy drop이 0.3 percentage point 미만입니다. 서로 다른 objective로 학습된 diffusion latent와 encoder latent가 단순한 선형 변환만으로 semantic prediction을 유지한다는 점에서 논문의 핵심 증거 중 하나입니다.

### 5. Linear editing 실험

다음은 "latent에서 찾은 semantic direction을 실제 이미지 편집에 쓸 수 있는가?"를 봅니다.

앞에서 학습한 logistic regression classifier의 weight vector를 semantic direction으로 사용합니다.

편집 공식은 단순합니다.

```text
z_edit = z + alpha * w
```

- `z`: DDIM-inverted latent
- `w`: 특정 attribute classifier의 weight direction
- `alpha`: 편집 강도

이후 edited latent를 diffusion decoder/generation process로 다시 이미지화합니다.

실험한 attribute 예시는 smile, age, gender, beard/goatee 등입니다. 결과적으로 `alpha`를 조절하면 attribute 강도가 부드럽게 변합니다. 중요한 점은 prompt engineering, model fine-tuning, architecture 변경 없이 linear direction 이동만 사용한다는 것입니다.

### 6. Spurious correlation 완화

CelebA attribute는 서로 얽혀 있습니다. 예를 들어 특정 수염 attribute를 조절하면 얼굴 형태나 성별 신호도 같이 변할 수 있습니다.

이를 줄이기 위해 저자들은 semantic direction을 다른 attribute direction에 대해 orthogonalization합니다.

예를 들어 attribute 1은 바꾸고 attribute 2는 유지하고 싶다면 다음처럼 target direction에서 spurious direction 성분을 제거합니다.

```text
w1_orth = w1 - projection_of_w1_on_w2
```

결과적으로 raw direction으로 편집할 때보다 원하지 않는 attribute 변화가 줄어든다고 보여줍니다.

### 7. Shared latent space 복원

UNE 가설이 맞다면 여러 모델 latent에는 공통 core subspace가 있어야 합니다.

이를 보기 위해 저자들은 여러 latent source를 묶고, 공통 `k`차원 shared space `X`를 찾습니다. 방법은 Generalized CCA, 정확히는 MAXVAR GCCA와 유사한 multi-view estimator입니다.

목표는 대략 다음과 같습니다.

```text
Z_i A_i ≈ X
```

각 모델 latent `Z_i`에서 선형 projection `A_i`를 거쳐 같은 shared representation `X`를 설명하게 만드는 것입니다.

실험한 shared space 조합은 다음과 같습니다.

- X1: SD 2.1, LCM, CLIP B/16, DINOv3
- X2: SD 1.5, LCM, OpenCLIP B/16, DINOv3
- X3: SD 1.5, SD 2.1, CLIP L/14, OpenCLIP B/16
- X4: SD 1.5, SD 2.1, CLIP L/14, DINOv3
- X5: SD 1.5, SD 2.1, LCM, CLIP L/14, OpenCLIP B/16, DINOv3

그 다음 shared space 차원 `k`를 바꿔가며 CelebA attribute classification을 수행합니다.

관찰은 다음과 같습니다.

- 32-512차원에서는 strong attribute classification
- 16차원에서도 상당한 정보 유지
- shared space들 간 retrieval neighborhood 구조도 Spearman correlation이 높음

즉 여러 모델이 공유하는 저차원 semantic core가 존재할 가능성을 보여줍니다.

### 8. 추가 실험

부록에서는 모델 비교와 AFHQ 일반화 실험을 추가로 수행합니다.

모델 비교에서는 pixel space, SD 1.5, SDXL, CelebA fine-tuned SD, CelebA 전용 작은 diffusion model 등을 비교합니다. 관찰은 pixel space가 latent space보다 linear separability가 낮고, 작은 CelebA 전용 diffusion model은 성능이 떨어지며, SDXL로 커져도 SD 1.5/2.1 대비 큰 향상은 제한적이라는 것입니다. 또한 CelebA에 fine-tune한 모델이 오히려 linear separability가 줄어드는 현상도 보고합니다.

AFHQ 동물 얼굴 데이터셋에서는 Cat, Dog, Wild 분류와 SD 1.5 latent에서의 linear classification, dog 방향 latent editing을 수행합니다. 결과적으로 동물 얼굴에서도 어느 정도 linear semantic structure가 유지된다고 주장합니다.

### 실험 pipeline 요약

```text
CelebA images
  -> DDIM inversion으로 diffusion noise latent 추출
  -> CLIP/OpenCLIP/DINO encoder embedding 추출
  -> NoiseZoo 구성

NoiseZoo
  -> Gaussianity test
  -> CelebA 40 attributes linear probing
  -> diffusion latent -> encoder latent 선형 transfer
  -> classifier direction 기반 linear editing
  -> spurious direction orthogonalization
  -> multi-view shared space 복원
  -> AFHQ와 추가 모델로 보조 검증
```

핵심은 모든 실험이 하나의 질문으로 모인다는 것입니다.

> 서로 다른 objective로 학습된 encoder와 generator의 latent가, 같은 이미지 분포에서 나온 공통 Gaussian semantic geometry의 다른 선형 관측값처럼 행동하는가?

논문의 답은 "완전한 증명은 아니지만, Gaussianity, linear probing, cross-space transfer, editing, shared subspace 실험이 모두 그 방향을 지지한다"입니다.

## 한계

이 논문은 이론적 증명이라기보다 **강한 empirical hypothesis paper**에 가깝습니다. 저자들도 "현재 작업은 가설과 실험 결과를 연결하는 단계"라고 봅니다.

주의할 점은 다음입니다.

- 실험이 주로 CelebA face attribute 중심입니다.
- AFHQ 동물 얼굴 추가 실험은 있지만, 일반 자연 이미지나 복잡한 scene까지 충분히 검증된 것은 아닙니다.
- Gaussianity와 linear separability가 관측된다고 해서 "진짜 하나의 universal latent source가 존재한다"가 증명되는 것은 아닙니다.
- project page에는 code/data가 "coming soon"으로 표시되어 있어, 재현 가능성은 공개 상태 확인이 필요합니다.

## 평가

좋은 점은 **생성 noise latent를 semantic representation으로 직접 다룬다**는 관점이 명확하고, 실험 설계도 Gaussianity, linear probing, cross-space transfer, editing으로 꽤 일관적입니다.

다만 핵심 가설인 UNE는 아직 "설명력 좋은 모델"에 가깝고, 실제로 universal한지 보려면 더 다양한 데이터셋, non-face 도메인, video, 3D, text-conditioned 복합 scene에서 검증이 필요합니다.

## 참고 자료

- [AlphaXiv](https://www.alphaxiv.org/overview/2603.21786v1)
- [Hugging Face paper page](https://huggingface.co/papers/2603.21786)
- [Project page](https://rbetser.github.io/UNE/)
- [arXiv](https://arxiv.org/abs/2603.21786v1)
