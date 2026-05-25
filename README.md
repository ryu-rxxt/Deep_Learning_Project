# 🌸 Flower Classification with CNN

> CNN 기반 5종 꽃 이미지 분류 모델 — 팀프로젝트  

## Course

**Information System Design(IMEN335)** — Industrial & Management Engineering, Korea University

| 이름 | 학번 | 역할 |
|------|------|------|
| 김시현 | 2021170807 | 모델 구현 및 보고서 작성 |
| 류근호 | 2021170814 | 하이퍼 파라미터 및 모델 Layer 튜닝을 통한 분류 정확도 향상 |

---

## 📌 프로젝트 개요

꽃 이미지 데이터를 입력받아 **5가지 꽃 종류**(Lilly, Lotus, Orchid, Sunflower, Tulip)를 분류하는 CNN(Convolutional Neural Network) 모델을 구현한 프로젝트

| 항목 | 내용 |
|------|------|
| 과목 | 정보시스템설계 |
| 모델 | CNN (TensorFlow / Keras) |
| 데이터 | Kaggle — 5 Flower Types Classification Dataset |

---

## 🌼 분류 대상 클래스

| Lilly | Lotus | Orchid | Sunflower | Tulip |
|-------|-------|--------|-----------|-------|
| 백합속 | 연꽃 | 난초 | 해바라기 | 튤립 |

---

## 📂 파일 구조

```
├── Deep_Learning.ipynb     # 전체 모델 학습 및 평가 코드
├── README.md
├── 딥러닝 보고서.pdf
└── flower_images/          # 데이터셋 (아래 데이터 준비 섹션 참고)
    ├── Lilly/
    ├── Lotus/
    ├── Orchid/
    ├── Sunflower/
    └── Tulip/
```

---

## 📊 데이터셋

- **출처**: [Kaggle — 5 Flower Types Classification Dataset](https://www.kaggle.com/datasets/kausthubkannan/5-flower-types-classification-dataset)
- **구성**: 5개 클래스 × 각 1,000장 = **총 5,000장**
- **분할**: Train 3,500장 (70%) / Validation 1,500장 (30%)

> ⚠️ 데이터셋 파일 크기가 크므로 이 레포지토리에는 포함되어 있지 않습니다.  
> 위 Kaggle 링크에서 직접 다운로드 후 `flower_images/` 폴더에 위치시켜 주세요.

---

## 🏗️ 모델 아키텍처

```
Input: (128, 128, 3)
  → Conv2D(32, 3×3, relu)  → MaxPooling2D(2×2)
  → Conv2D(64, 3×3, relu)  → MaxPooling2D(2×2)
  → Conv2D(128, 3×3, relu) → MaxPooling2D(2×2)
  → Flatten
  → Dense(128, relu)
  → Dense(5, softmax)

Optimizer : Adam
Loss      : Sparse Categorical Crossentropy
Metrics   : Accuracy
Total Params: 3,305,285
```

---

## ⚙️ 주요 하이퍼파라미터

| 파라미터 | 값 |
|----------|----|
| Image Size | 128 × 128 |
| Batch Size | 64 |
| Epoch | 20 (EarlyStopping 적용) |
| Validation Split | 0.3 |
| Early Stopping | monitor=`val_accuracy`, patience=1 |

---

## 🔬 모델 개선 과정

### 방법 1 — Input Size & Batch Size 탐색

| 모델 | Image Size | Batch | Train Acc | Val Acc |
|------|-----------|-------|-----------|---------|
| 모델 1 | 128×128 | 64 | 0.9874 | **0.8160** |
| 모델 2 | 28×28 | 32 | 0.7800 | 0.7227 |

→ 성능이 우수한 **모델 1 설정** 채택

### 방법 2 — Data Augmentation (ImageDataGenerator)

rotation, width/height shift, shear, zoom, horizontal flip 등을 적용하였으나 기존 모델 대비 성능 저하 → **미적용**

### 방법 3 — EarlyStopping으로 적정 Epoch 탐색

고정 epoch 설정 시 과적합 발생 epoch이 매 실험마다 달라 변동이 컸음.  
`EarlyStopping(monitor='val_accuracy', patience=1)` 적용 → **최적 Epoch = 7** 확인

---

## 📈 최종 성능

| 지표 | 값 |
|------|----|
| Train Accuracy | 0.9503 |
| Validation Accuracy | 0.7840 |
| **Test Accuracy** | **0.8000** |

> 꽃 종류별 5장, 총 25장의 test 이미지로 평가 (test accuracy 0.8 달성)

---

## 🛠️ 사용 기술

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat&logo=tensorflow&logoColor=white)
![Keras](https://img.shields.io/badge/Keras-D00000?style=flat&logo=keras&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)

---

## ⚠️ 한계 및 개선 방향

- 3,305,285개의 파라미터 대비 학습 데이터(5,000장)가 부족하여 일반화 성능에 한계 존재
- Transfer Learning(VGG16, ResNet 등) 적용 시 더 높은 성능 기대 가능
- Dropout, Batch Normalization 등 추가 정규화 기법 도입 여지 있음
