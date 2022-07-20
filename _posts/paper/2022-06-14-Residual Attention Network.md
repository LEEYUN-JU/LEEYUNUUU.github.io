---
layout: post
title: Residual Attention Network for Image Classification
comments: true
categories : [paper]
published: true

---

#### 2017년도에 발행된 Residual Attention Network 모델에 관한 논문을 적어보고자 한다 (~~최근, 최신 등의 단어는 다 이 논문 시점 기준~~)

### Introduction
이 논문이 작성된 시점을 기준으로 Image Classification 분야에서 attention 메커니즘을 사용한 경우는 없다고 한다. 최근 연구되어 지는 Image Classification 분야에서는 "Very Deep"한 구조를 사용한다. 이러한 깊은 구조에 영감을 받아서 이 논문에서는 Attention Module을 여러개 겹처 더 깊게 만든 Attention 구조를 제안한다. 

이 논문의 Contribution 은 다음과 같다.

(1) **Stacked Network Structure** : Attention Module 을 여러개 겹쳐 놓은 Residual Attention Network 라는 것. 따라서 각 Attention Module 별로 각기 다른 부분을 Attention 하는 것이 가능하다.

(2) **Attention Residual Learning** : Attention Module 을 무작정 겹치기만 할 경우에 성능이 매우 저하된다. 이에, 이것을 방지하는 수백개의 레이어로 구성된 Residual Attention Network를 제안한다.

(3) **Bottom-up top-down feedforward attention** : 이 구조는 Stacked hourglass 와는 다르며, 각 feature들을 가볍게 하기 위해서 Human Pose 나 image Segmentation 에 사용되는 방식을 적용했다.

---

### Related Work
![enter image description here](https://ifh.cc/g/oJg75l.png)

Residual Attention Network 는 자연어 처리에서 주로 사용되는 Attention 기법을 사용하였다. 자연어 처리에서의 Attention 은 어떠한 문장이 주어졌을 때, 전체적인 문락을 이해하기 위해 필수적으로 사용되는, 그 문장을 이해하기 위한 필수 단어들에 더 높은 가중치를 두는 것으로 볼 수 있다. 

---

### Residual Attention Network
다수의 Attention Module을 쌓아서 만든 Residual Attention Network이다. 
![enter image description here](https://ifh.cc/g/kOgrAT.jpg)

#### 식 (1) 의 H 는 Attention  Module 의 출력 값이다.
>c : 입력 이미지의 채널 수
>i : 채널의 index 값
>M : soft mask 함수 의 결과 값
>T : Trunk branch 의 결과 값 
#### 식 (2) 는 soft mask branch 의 input feature 의 gradient를 보여준다.
> θ : mask branch 의 파라미터
> Φ : trunk branch 의 파라미터
#### 식 (3) 은 Attention Residual Module 이다.
> M : [0, 1] 의 값을 가지게 된다
> F : 원본 이미지의 feature 값

---

### Residual Attention Network Architecture
다음 그림은 Attention Residual Network 의 전체적인 구조를 나타낸다.
![enter image description here](https://ifh.cc/g/jkaCXZ.png)

이 네트워크의 특징은 크게 세 가지가 존재한다.
1. Trunk branch
2. Soft Mask branch
3. Residual Module(Unit)

우선 Trunk branch 와 Soft Mask branch를 살펴보자. 다음 사진은 전체적인 구조를 나타낸다.
![enter image description here](https://ifh.cc/g/FFR39V.png)

이 그림이 의미하는 것은 다음과 같다.
>사전 연산에서는 전체 이미지의 주요 정보를 수집하고, 후 연산에서는 이미지의 주요 정보와 원본 이미지의 feature map 을 결합한다.
- Soft Mask Branch
  -- 이미지를 최소한 작게 down sampling 한 다음, 최소한의 이미지를 입력 이미지의 사이즈에 맞게 up sampling 하는 방법을 사용하여 추출한다. 이렇게 0~1 사이의 mask를 생성한다. 
- Trunk Branch
-- 입력 값으로부터 특징을 추출해 낸다.

Soft Mask Branch 과정에서 노이즈를 제거하고(상대적으로 영향이 적은 픽셀들이 제거 됌) Mask가 생성되며, 이를 trunk branch 의 결과 값과 합쳐준다. (그림에서 결과 값: 그림에서 제일 위의 초록색 부분)

- Residual unit
-- Attention Module 을 구성하기 위해서 세 파라미터(p, t, r)을 사용한다.
p : trunk branch 와 mask branch 로 들어가기 전의 전처리 된 Pre-processing Residual Units 의 개수
t : trunk branch의 Residual Unit 개수
r : mask branch 의 인접한 pooling layer 사이의 Residual Unit 개수

  -- Attention Module 은 하나의 특징만 포착하기 때문에 여러 모듈을 쌓아서 사용해 주어야 한다. 

  -- Soft mask Residual Unit 과 trunk branch 의 채널 수는 동일하다.

---

### Result
#### Spatial Attention and Channel Attention 의 결과
![enter image description here](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/benAlO/btq0zvGmTa2/5BfF98VkxTuAKdyJM7iUUk/img.png)

#### Attention Residual Learning
에러 값이 줄었다는 것을 확인 할 수 있다.
>ARL  : Attention Residual Learning
>NAL : Native Attention Learning

둘의 차이는 Residual Unit의 적용 여부라고 볼 수 있다.

![enter image description here](https://ifh.cc/g/V1VP3O.png)

#### Comparison of different mask structures
![enter image description here](https://ifh.cc/g/m098CB.png)

#### Comparisons with state-of-the-art methods
![enter image description here](https://ifh.cc/g/hGzJGZ.png)
![enter image description here](https://ifh.cc/g/1HcQ4l.png)
