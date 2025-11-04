# Deep-Learning

# CIFAR-10 Image Classification with MLP

## 📘 Overview  
본 프로젝트는 **PyTorch**를 사용하여 CIFAR-10 데이터셋을 분류하는  
다층 퍼셉트론(**Multi-Layer Perceptron, MLP**) 모델을 구현한 예제입니다.  
CNN이 아닌 완전연결층 기반 신경망만으로 이미지 분류 성능을 실험하는 데 초점을 두었습니다.

---

## 🖼 Dataset: CIFAR-10  
- **구성:** 60,000장의 32×32 RGB 컬러 이미지 (Train: 50,000 / Test: 10,000)  
- **클래스(10개):** airplane, automobile, bird, cat, deer, dog, frog, horse, ship, truck

---

## 🏗 Model Architecture

| Layer | Input → Output | Activation | Dropout | Normalization |
|-------|----------------|-------------|----------|---------------|
| Linear 1 | 3072 → 1024 | GELU | 0.3 | BatchNorm1d |
| Linear 2 | 1024 → 512 | GELU | 0.3 | BatchNorm1d |
| Linear 3 | 512 → 256 | GELU | 0.2 | BatchNorm1d |
| Linear 4 | 256 → 128 | GELU | 0.2 | BatchNorm1d |
| Linear 5 | 128 → 10 | Softmax(Logit) | — | — |

> ⚙️ **총 파라미터 수:** 약 3.5M  
> **활성함수:** GELU  
> **정규화:** Batch Normalization  
> **정규화 비율:** Dropout(0.2~0.3)

---

## ⚙️ Optimizer & Scheduler  

모델 학습에는 **AdamW 옵티마이저**와 **CosineAnnealingLR 스케줄러**를 사용했습니다.  
AdamW는 L2 정규화(`weight_decay`)를 안정적으로 적용할 수 있어 과적합 방지에 효과적이며,  
CosineAnnealingLR은 학습률을 코사인 곡선 형태로 점진적으로 감소시켜 **부드러운 수렴**을 유도합니다.  

| 구성요소 | 설정값 | 설명 |
|-----------|----------|------|
| **Optimizer** | AdamW | Adam 기반 옵티마이저로, 가중치 감쇠(`weight_decay`)를 통한 일반화 강화 |
| **Learning Rate** | 0.001 | 초기 학습률 |
| **Weight Decay** | 1e-4 | 과적합 방지를 위한 정규화 항 |
| **Scheduler** | CosineAnnealingLR | 주기적으로 학습률을 조정하여 안정적인 학습 유도 |
| **T_max** | 30 | 한 주기 내에서 학습률이 최솟값에 도달하는 epoch 수 |
| **Loss Function** | CrossEntropyLoss | 다중 클래스 분류를 위한 표준 손실 함수 |
