# Flow-Based Network Anomaly Detection using AI
### AI 모델을 이용한 Flow 로그 기반 네트워크 이상탐지

---

## 👥 팀원
- **김민서** (소프트웨어학부, 20203088)
- **송우용** (소프트웨어학부, 20213126)
- **안준엽** (소프트웨어학부, 20211794)
- **이수아** (소프트웨어학부, 20211702)

---

## 📌 프로젝트 소개

본 프로젝트는 네트워크 환경에서 수집되는 **Flow 로그 데이터**를 기반으로  
정상 트래픽과 비정상 트래픽을 구분하는 **AI 기반 네트워크 이상 탐지 시스템**을 연구한 캡스톤디자인1 결과물입니다.

기존의 패킷 기반 NIDS는 TLS/암호화 환경에서 payload 분석에 한계가 있고,  
고성능 장비를 요구하며 실시간 처리 부담이 크다는 문제가 있습니다.  
이에 본 프로젝트에서는 **패킷 단위가 아닌 Flow 단위 메타데이터**를 활용하여,  
실제 운영 환경에 더 적합한 **경량형 이상 탐지 구조**를 설계하는 것을 목표로 하였습니다.

또한 규칙 기반 탐지 방식의 한계를 보완하기 위해,  
정상/공격 패턴을 데이터 기반으로 학습하는 **AI 기반 이상 탐지 접근**을 적용하였습니다.

---

## 🎯 연구 목표

- Flow 로그 기반 이상 탐지 시스템 설계
- 실시간 적용이 가능한 경량 구조 검토
- 공격 여부 탐지 및 공격 유형 분류 가능성 분석
- 서로 다른 탐지 관점을 가진 모델 간 성능 비교
- 실험 결과를 바탕으로 실제 적용 가능한 구조 제안

---

## 📑 목차
- [프로젝트 배경](#-프로젝트-배경)
- [데이터셋](#-데이터셋)
- [비교 모델](#-비교-모델)
- [실험 방법](#-실험-방법)
- [주요 결과](#-주요-결과)
- [모델 해석 및 분석](#-모델-해석-및-분석)
- [하이브리드 구조 제안](#-하이브리드-구조-제안)
- [설치 및 실행](#-설치-및-실행)
- [프로젝트 구조](#-프로젝트-구조)
- [한계 및 향후 계획](#-한계-및-향후-계획)
- [참고 자료](#-참고-자료)

---

## 🔍 프로젝트 배경

### 기존 패킷 기반 NIDS의 한계
- TLS/암호화 환경에서 payload 분석이 어려움
- 고성능 장비가 필요하여 비용 부담이 큼
- 실시간 처리 시 성능 부담이 큼

### Flow 로그 기반 접근 필요성
- 패킷 전체가 아닌 세션 단위 메타데이터를 활용
- 저장 및 연산 비용이 상대적으로 낮음
- 실제 운영 환경에 적용하기 쉬운 경량 구조 설계 가능

### AI 기반 이상 탐지 필요성
- 규칙 기반 탐지의 한계 보완
- 정상 분포로부터 이탈한 비정상 행위를 탐지
- 변형 공격 및 새로운 유형의 공격에 대응 가능성 확보

---

## 🗂 데이터셋

본 프로젝트의 모델 비교 실험은 동일한 조건에서 수행되었으며,  
발표 자료 기준 주요 실험 데이터셋으로 **NF-UNSW-NB15-v3**를 사용하였습니다.

### 데이터셋 선정 이유
- Flow 기반 NIDS 연구에서 활용 가능한 벤치마크 데이터셋
- 시계열 기반 모델(LSTM, TCN)과 이상 탐지 모델(OCSVM, Isolation Forest)을 함께 비교하기에 적합
- 단순 정확도 비교를 넘어, 탐지 구조 및 모델 특성 분석에 활용 가능

> 보고서에서는 공공 Flow 데이터셋으로 CICIDS2017, CSE-CIC-IDS2018 등도 검토 대상으로 제시하였으며,  
> 발표 자료에서는 실제 모델 비교 실험에 NF-UNSW-NB15-v3를 중심으로 사용하였다.

---

## 🤖 비교 모델

프로젝트에서는 서로 다른 탐지 관점을 가진 모델들을 비교 대상으로 선정하였다.

### 시계열 기반 모델
- **LSTM**
- **TCN (Temporal Convolutional Network)**

→ 시간 흐름에 따른 트래픽 패턴 학습에 초점

### 이상 탐지 기반 모델
- **One-Class SVM (OCSVM)**
- **Isolation Forest**

→ 정상 데이터 분포로부터의 이탈 탐지에 초점

---

## 🧪 실험 방법

### 공통 실험 기준
- 동일 데이터셋 사용
- 동일 평가 지표 사용
- Threshold 기반 성능 평가 수행
- 운영 환경을 고려하여 Precision / Recall / F1-score를 함께 비교

### 비교 방식
1. **동일 Threshold(0.5) 기준 비교**
2. **모델별 최적 Threshold 적용 비교**
3. **공격 유형별 탐지 성능 비교**
4. **XAI / SHAP 기반 특징 중요도 분석**

---

## 📊 주요 결과

## 1. 동일 Threshold 기준 모델 비교

발표 자료 기준, 동일 Threshold(0.5)에서 모델별 성능은 다음과 같이 비교되었다.

| 모델 | Threshold | Accuracy | Precision(Attack) | Recall(Attack) | F1(Attack) |
|------|-----------|----------|-------------------|----------------|------------|
| TCN | 0.5 | 0.9993 | 0.9937 | 0.9925 | 0.9931 |
| LSTM | 0.5 | 0.9737 | 0.9684 | 0.5373 | 0.6912 |
| Isolation Forest | 0.5 | 0.7921 | 0.9470 | 0.6595 | 0.7775 |
| One-Class SVM | 0.5 | 0.9800 | 0.7300 | 1.0000 | 0.8400 |

→ 동일 Threshold 기준에서는 **TCN이 가장 높은 F1-score**를 보였고,  
→ OCSVM은 Recall은 매우 높았지만 Precision이 상대적으로 낮았다.

---

## 2. OCSVM 최적화 결과

보고서 기준 OCSVM은 threshold를 0.0~1.0 범위에서 정밀 탐색하여  
**F1-score가 최대가 되는 지점**을 최적 threshold로 선정하였다.

### OCSVM 최적 결과
- **Best Threshold:** 0.8990
- **Precision:** 0.9849
- **Recall:** 0.9985
- **F1-score:** 0.9917

### 해석
- 정상 데이터 분포를 기준으로 이상 여부를 빠르게 판단할 수 있음
- 공격 탐지 Recall이 매우 높아 공격을 놓칠 가능성이 낮음
- 상대적으로 단순한 구조로 빠른 1차 판별에 적합함

---

## 3. TCN 최적화 결과

보고서 기준 TCN은 precision과 recall이 모두 일정 수준 이상인 threshold 후보 중에서  
가장 높은 F1-score를 보이는 값을 최종 threshold로 선정하였다.

### TCN 최적 결과
- **Best Threshold:** 0.881
- **Precision:** 0.9955
- **Recall:** 0.9981
- **F1-score:** 0.9968

### 해석
- 전체 성능 지표는 매우 우수함
- 시간적 패턴을 반영할 수 있어 공격 유형 분류에 강점을 보임
- 다만 전체 트래픽을 모두 처리하는 구조에서는 연산 부담이 커질 수 있음

---

## 4. 공격 유형별 관찰 결과

### TCN
- 대부분의 공격 클래스에서 Recall 99% 이상
- 공격 유형을 거의 놓치지 않는 강점 확인
- 반면 정상(BENIGN) 클래스 Recall이 매우 낮아, 정상 트래픽을 공격으로 오탐하는 경향이 관찰됨

### OCSVM
- 공격 유형별 세부 분류보다는 **정상/비정상 1차 탐지**에 더 적합
- 공격 여부 판별 관점에서는 높은 Recall과 안정적인 경계 형성이 장점

---

## 🧠 모델 해석 및 분석

프로젝트에서는 SHAP 기반 XAI 분석을 통해  
모델이 어떤 feature를 근거로 공격 여부를 판단했는지 해석하였다.

### 공통적으로 중요하게 나타난 feature
- **MIN_TTL**
- **MAX_TTL**

### 해석
- TTL 값이 커질수록 공격으로 분류될 가능성이 증가하는 경향이 관찰됨
- TTL 값이 낮으면 정상 트래픽으로 판단될 가능성이 증가하는 경향이 나타남
- 일부 feature는 모델별로 추가적인 영향 차이를 보였으나, 전반적으로 TTL 관련 feature가 핵심 변수로 해석되었다

---

## 🧩 하이브리드 구조 제안

실험 결과, 단일 모델만으로는  
**탐지와 분류를 동시에 모두 만족시키기 어렵다**는 한계를 확인하였다.

### 단일 모델 접근의 한계
- 탐지와 분류의 목적이 서로 다름
- 희소한 공격 유형에 대한 분류 성능 저하 가능성 존재
- 고성능 모델을 전체 트래픽에 적용하면 실시간 처리 부담 증가

### 제안 구조: 2단계 하이브리드 구조

#### 1단계: 공격 여부 판별
- 모든 네트워크 트래픽에 대해 빠른 이진 분류 수행
- 정상/공격 트래픽을 효율적으로 구분
- 경량 모델 사용
- 후보 모델: **SVM / OCSVM 계열**

#### 2단계: 공격 유형 분류
- 1단계에서 공격으로 분류된 트래픽만 정밀 분석
- 세부 공격 유형 식별 및 분류 수행
- 시간적 패턴 인식이 가능한 고급 모델 사용
- 후보 모델: **TCN**

### 기대 효과
- 단계적 필터링으로 시스템 부하 감소
- 각 단계별 모델 독립 최적화 가능
- 탐지 성능과 연산 효율을 함께 확보
- 새로운 공격 유형에 대한 확장성 확보

---

## ⚙️ 설치 및 실행

### 개발 환경
- Python 3.10
- Google Colab
- PyTorch
- TensorFlow / Keras

### 예시 패키지 설치
```bash
pip install numpy pandas matplotlib seaborn scikit-learn torch shap joblib
```

---

## 🚧 한계 및 향후 계획

### 현재 한계

본 프로젝트는 Flow 로그 기반 이상 탐지의 가능성을 확인하고, 서로 다른 탐지 관점을 가진 모델들의 성능을 비교하는 데 초점을 두었다.  
다만 캡스톤디자인1 단계의 결과물인 만큼 다음과 같은 한계가 존재한다.

- **단일 모델 접근의 한계**  
  실험 결과, 공격 여부 탐지와 공격 유형 분류를 하나의 모델로 동시에 만족시키는 데에는 구조적인 한계가 있었다. 발표 자료에서도 탐지와 분류의 목적 차이, 희소한 공격 유형에 대한 분류 성능 저하 가능성이 주요 한계로 정리되었다.

- **TCN의 연산 부담**  
  TCN은 매우 높은 Precision, Recall, F1-score를 보였지만, 전체 네트워크 트래픽에 직접 적용할 경우 처리 부담이 커질 수 있다. 따라서 실시간 운영 환경에서 항상 단독으로 사용하기에는 효율성 측면의 검토가 필요하다.

- **OCSVM의 분류 한계**  
  OCSVM은 빠른 이상 탐지와 높은 Recall 측면에서는 강점을 보였지만, 공격 유형을 세부적으로 분류하는 데에는 한계가 있다. 즉, 1차 이상 여부 판별에는 적합하지만 다중 공격 유형 분류 모델로 사용하기에는 제약이 있다.

- **실험 환경의 제한성**  
  본 프로젝트의 실험은 공개 데이터셋 기반으로 수행되었기 때문에, 실제 기업망·클라우드·IoT 환경의 다양한 트래픽 패턴을 완전히 반영하지는 못한다. 따라서 운영 환경 일반화 성능은 추가 검증이 필요하다.

- **서비스 통합의 미완성**  
  보고서에서는 웹 대시보드, CLI, Collector, DB, AI 추론 구조까지 포함한 전체 시스템을 설계하였지만, 현재 결과물은 실험 중심의 모델 검증 단계에 가깝다. 실제 배포형 서비스 수준의 완전한 통합은 후속 개발이 필요하다.

### 향후 계획

최종발표 자료에서 제안한 방향에 따라, 이후에는 단순 모델 비교를 넘어 실제 적용 가능한 구조로 확장하는 것을 목표로 한다.

- **하이브리드 구조 구현 및 검증**  
  1단계에서 SVM/OCSVM 계열 모델로 정상·공격 여부를 빠르게 판별하고, 2단계에서 TCN으로 공격 유형을 세부 분류하는 하이브리드 구조를 실제로 구현하고 검증할 예정이다.

- **데이터셋 확장 및 일반화 성능 평가**  
  NF-UNSW-NB15-v3 외에도 CICIDS2017, CSE-CIC-IDS2018 등 다양한 Flow 기반 데이터셋을 활용하여 모델의 일반화 성능을 재검증할 예정이다.

- **실제 환경 연동**  
  Zeek / NetFlow 기반 Collector와 연동하여 실시간 Flow 수집-전처리-추론-시각화 파이프라인을 고도화하고, 웹 대시보드 및 로그 관리 기능과 연결하는 방향으로 발전시킬 계획이다.

- **오탐 감소 및 임계값 최적화**  
  운영 환경에서는 단순한 높은 Recall보다도 오탐(False Positive)을 줄이는 것이 중요하므로, threshold 최적화와 feature 개선을 통해 실사용성을 높일 예정이다.

- **설명 가능한 AI(XAI) 고도화**  
  현재 SHAP 기반 분석에서 MIN_TTL, MAX_TTL 등 주요 feature의 영향을 확인하였으므로, 후속 연구에서는 탐지 근거를 더 직관적으로 시각화하여 관리자 의사결정에 활용할 수 있도록 개선할 예정이다.

---

## 📚 참고 자료

### 주요 참고 문헌
- 네트워크 플로우 데이터 기반 이상징후 탐지 인공지능 모델 성능 비교
- 머신러닝과 딥러닝을 활용한 악성 패킷 탐지 기술 연구
- Machine learning-based network intrusion detection for big and imbalanced data using oversampling, stacking feature embedding and feature extraction
- Impact of Machine Learning on Intrusion Detection Systems for the Protection of Critical Infrastructure
- Evaluating machine learning-based intrusion detection systems with explainable AI: enhancing transparency and interpretability
- Packet-Level and Flow-Level Network Intrusion Detection Based on Reinforcement Learning and Adversarial Training
- Survey on Intrusion Detection Systems Based on Machine Learning Techniques for the Protection of Critical Infrastructure
- Network Anomaly Detection: Flow-based or Packet-based Approach?
- Flow-based intrusion detection: Techniques and challenges
- Flow-Based and Packet-Based Intrusion Detection Using BLSTM
- An Overview of IP Flow-Based Intrusion Detection
- NetFlow Datasets for Machine Learning-based Network Intrusion Detection Systems
- A flow-based IDS using Machine Learning in eBPF
- A comprehensive review of AI based intrusion detection system

### 데이터셋 및 관련 자료
- NF-UNSW-NB15-v3
- CICIDS2017
- CSE-CIC-IDS2018
- UNSW-NB15
- AIT Netflow Dataset
- UWF-ZeekData22 / UWF-ZeekDataFall22
- CAIDA Dataset

### 비고
본 README의 참고 자료 목록은 최종보고서와 최종발표 자료에 포함된 문헌 및 데이터셋을 바탕으로 정리하였다.  
GitHub 공개 시에는 위 항목들 중 실제로 프로젝트 수행에 직접 활용한 논문, 데이터셋, 보고서만 선별하여 최종 반영하는 것을 권장한다.

## 🔗 캡스톤디자인2 링크
https://github.com/kms0707/ssucaptone2
