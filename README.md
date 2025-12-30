# 이미지 기반 사과 품종 분류 프로젝트

CNN 모델을 활용하여 주어진 이미지가 세 종류의 사과 품종 중 어느 카테고리에 속하는지 알아내는 딥러닝 프로젝트

## 1. 프로젝트 개요

* **목적**: 사과 품종으로 인한 오분류 문제를 해결 가능한 경량화된 분류 모델 구축 


* **기간**: 2025년 12월 29일 - 30일 


* **참여 인원**: 1명 (개인 프로젝트) 



## 2. 사용 기술 (Tech Stack)

* **Language**: Python 3.13.9 


* **Framework**: Keras 


* **Libraries**: NumPy, Matplotlib, Pathlib, Pillow 



## 3. 데이터 개요 및 전처리

### 3.1 데이터 개요

* **출처**: Kaggle Fruit recognition by Chris Gorgolewski 


* **구성**: 총 2,384장의 RGB 이미지 


  * 품종 A: 692장 
  
  
  * 품종 D: 1,033장 
  
  
  * 품종 E: 659장 





### 3.2 전처리 과정

1. **크기 조절 (Resizing)**: 가장 짧은 가로 또는 세로 길이를 구한 뒤, 해당 길이보다 한 변의 길이가 짧은 정사각형으로 리사이징 


2. **라벨링 및 분리**: 품종에 따라 라벨링 후 20%를 Validation 용도로 분리 


3. **정규화 (Normalization)**: RGB 값(0~255)을 0~1 사이 값으로 픽셀 스케일링 



## 4. 모델 및 학습 설계

### 4.1 CNN 아키텍처

* **특징 추출 (Feature Extraction)**:
  * 활성화 함수: ReLU 
  
  
  * 합성곱 층: Separable Convolution 사용 
  
  
  * 풀링 층: 1/2로 다운 샘플링 


* **주요 기법**:
  * Pre-activation 방식을 사용하여 기울기 소실 문제 방지 
  
  
  * Iteration을 통해 채널 개수를 점차 증가시킴 
  
  
  * Skip Connection (Add) 구조 사용




* **분류기 (Classifier)**:
  * GlobalAveragePooling2D로 1차원 벡터화 
  
  
  * 25% Dropout으로 과적합 방지 
  
  
  * 확률 대신 Logit을 사용하여 언더/오버플로우 방지 및 수치 안정성 확보 





### 4.2 학습 설정

  * **Optimizer**: Adam (learning rate: 0.0003) 
  
  
  * **Loss function**: Sparse Categorical Crossentropy 
  
  
  * **Callback**: 5번 이상 검증 손실 발생 시 종료
  
  
  * **Epoch**: 100 



## 5. 학습 결과 및 분석

### 5.1 시도별 결과

* **1차 시도**: Accuracy는 높아지고 loss는 낮아졌으나, Validation 과정에서 문제 발생 (과적합 추정) 


* **1차 코드 수정**:
  * Dropout을 50%로 상향 
  
  
  * Learning rate를 0.0001로 하향 
  
  
  * 데이터 증강(회전/반전) 적용 




* **2차 시도**: 여전히 같은 문제 지속, Validation accuracy가 0.4559로 고정됨 


* **2차 코드 수정**:
  * 채널 수를 [64, 128, 256] 순으로 증가하도록 낮춤 
  
  
  * 데이터 개수에 따른 보정치 추가로 불균형 해소 시도 




* **3차 시도**: Validation accuracy 하락 및 Loss 상승 문제 지속 



### 5.2 실패 원인 분석

1. **이미지 데이터 문제**: 배경인 금속 판으로 인한 빛 반사가 발생하여 색상 정보를 왜곡하고 경계선 검출을 어렵게 함. 또한 수집 환경이 고정되어 있어 과적합 발생 


2. **단순 분류의 한계**: 사과 품종 간 특징 차이가 적어 단순 분류가 아닌 미세 분류 방식으로 접근했어야 함 



## 6. 결론 및 개선 방안

### 6.1 모델 개선 방안

* **전처리 강화 및 재수집**:
  * 사진에서 타겟(사과)을 제외한 배경 제거 
  
  
  * 편광 필터를 적용하여 빛 반사를 줄인 데이터 재수집 



* **전이 학습 (Transfer Learning) 도입**: 사전에 큰 데이터셋으로 학습된 모델을 사용하여 복잡한 분류 문제 해결 및 특징 추출 효율화 


* **이미지 증강**: 밝기나 대조 수치를 조절해 빛 관련 노이즈에 강한 모델 제작 



### 6.2 최종 결론

이미지 데이터를 통해 사과 품종을 분류하는 모델을 구축했으나 배경 문제, 데이터 불균형, 과적합 등으로 인해 성능이 저조했음
향후 유사한 물체를 분류할 때는 전이 학습 모델을 사용하거나 데이터 전처리를 강화할 필요가 있음 

---

**GitHub Repository**: [https://github.com/imbesky/apple-distinguish](https://github.com/imbesky/apple-distinguish)
