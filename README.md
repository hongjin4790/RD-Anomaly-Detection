# Reverse Distillation을 활용한 이상 탐지(AD) 모델 - 백본 교체 프로젝트

# 프로젝트 개요
  이상 탐지(Anomaly Detection,AD)는 어떤 데이터 집합에서 예상치 못한 데이터 패턴을 찾아내는 기법으로 제조 품질검사, 보안, 결제 시스템등 다양한 분야에서 중요한 역할을 한다.
  
  본 프로젝트는 Reverse Distillation을 활용한 이상 탐지(AD) 논문에서 사용된 모델의 백본(backbone)을 교체하여 성능을 향상시키는 것을 목표로 한다. Reverse Distillation 기법은 Teacher로부터 Student로 지식을 증류(distillation)하여 학습하는 방식이다. 기존의 방식과의 차이점이라면 Teacher와 Student의 정보가 동일한 뱡향으로 흘러서 지식이 전달된다. 반면, Reverse Distillation 방식은 Teacher를 통해 압축된 정보가 Student로 흘러가면서 지식이 전달된다.

# RD 모델 아키텍처
  ![image](https://github.com/user-attachments/assets/2eeb1627-2aa5-4f0c-b5a4-794debd4258f)

- **Teacher Encoder**
  - 사전 학습된 모델로 각 해상도 별로 피쳐맵을 추출
 
- **MFF(Multi scale Feature Fusion Block)**
  - Teacher에서 온 다양한 크기의 피쳐맵을 합쳐주는 역할

- **OCE(One Class Embedding Block)**
  - MFF에서 합쳐진 피쳐맵 정보를 압축하는 역할 수행

- **Student Decoder**
  - 압축된 피쳐맵을 다시 Student가 복원하고 Teacher의 피쳐맵과 비교

- loss function은 코사인 유사도를 사용하여 Encoder와 Decoder가 추출한 피쳐맵이 동일해지도록 구성


# RD 모델의 백본인 ResNet을 ConvNeXt로 교체

- RD 모델의 백본인 ResNet을 ConvNeXt로 교체한 이유는 다음과 같다:
  - 성능 향상: ConvNeXt가 기존 ResNet보다 높은 정확도와 성능을 제공
  - Transformer 기반 설계: Transformer의 요소를 통합하여 더 나은 표현력과 피처 추출 능력
  - 최신 기술 반영: ConvNeXt는 최신 모델 트렌드를 반영한 아키텍처로 성능 개선 가능성

![image](https://github.com/user-attachments/assets/2553eeac-ef8c-4b84-bbb9-92c782deade7)

- 논문의 의도대로 대칭구조로 작성
  - ResNet의 4개의 Stage 중 3개의 Stage로 구성 -> ConvNeXt도 같은 구조로 교체
  - ConvNeXt의 4번째 stage로 OCE를 구성
  - ConvNeXt의 Tiny, Small, Base, Large 중 Base 선택
 
# 모델 결과
<img src="https://github.com/user-attachments/assets/493d7a24-5c15-41a4-b133-23349a1c896d" width="600" height="300"/>

- RD(ConvNeXt)가 모든 클래스에 대한 성능이 좋지 않은 것으로 보아 구조적으로 문제가 있을것으로 판단

<img src="https://github.com/user-attachments/assets/a0b50fba-fca1-4b8e-89e2-b6c84fd16ff3" width="500" height="300"/>

<img src="https://github.com/user-attachments/assets/4c5fa235-1253-41bb-b66d-a7b4a79c8818" width="500" height="300"/>

# 결론
ResNet에서 ConvNext로 백본을 교체하여 성능 개선을 시도했으나, 전체적으로 ResNet보다 성능이 낮아지는 결과를 얻었다.
이를 개선하기 위해 향후 성능을 더 끌어올리기 위해 구조적인 수정을 위한 추가 연구가 필요하다.



