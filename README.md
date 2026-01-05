<div align="center">
  <img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white">
  <img src="https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white">
  <img src="https://img.shields.io/badge/lightgbm-02569B?style=for-the-badge&logo=lightgbm&logoColor=white">
</div>

# 📖 프로젝트 주제 : 당뇨병 예측 모델링: 통계분석 및 머신러닝 접근
- 건강 설문 및 생활습관 데이터를 기반으로 개인의 당뇨병 위험도를 예측하고,  
  **고위험군을 사전에 선별할 수 있는 데이터 기반 모델링**을 수행함  
  (통계적 해석 + 머신러닝 예측 성능을 함께 고려)

---

## 1. Project Overview 
- **주제** : 생활 습관 및 신체·혈액 지표를 활용한 당뇨병 유무 분류
- **데이터셋** : [Diabetes Health Indicators Dataset](https://www.kaggle.com/datasets/mohankrishnathalla/diabetes-health-indicators-dataset/data)
- **핵심 목표** : 설문지를 활용하여 **당뇨병 고위험군을 선별할 수 있는 예측 모델** 구축

---

## 2. Data Dictionary (주요 핵심 변수)
- 실제 분석 결과를 통해 **모델 예측에 기여도가 높은 변수 위주로 정리**
- 총 변수 개수 : **31개**

| 변수명 | 설명 | 값의 의미 |
| :--- | :--- | :--- |
| **diagnosed_diabetes** | 당뇨 여부 (**Target**) | 0: 비당뇨, 1: 당뇨 |
| systolic_bp | 수축기 혈압 | 수치형 |
| diastolic_bp | 이완기 혈압 | 수치형 |
| triglycerides | 중성지방 수치 | 수치형 |
| hdl_cholesterol | HDL 콜레스테롤 | 수치형 |
| ldl_cholesterol | LDL 콜레스테롤 | 수치형 |
| waist_to_hip_ratio | 허리–엉덩이 비율 | 수치형 |
| physical_activity_minutes_per_week | 주간 신체활동량 | 수치형 |
| age | 연령 | 수치형 |
| smoking_status | 흡연 상태 | 범주형 |
| income_level | 소득 수준 | 순서형 |

---

## 3. Problem Definition
- **데이터 특성**
  - 클래스 불균형이 존재하는 이진 분류 문제
  - 설문 기반 데이터로 변수 간 상관성이 높음
- **분석 방향**
    + 통계분석 : 다중회귀분석, 로지스틱 회귀분석, 분산분석
    + 머신러닝 : 로지스틱 회귀, 결정트리, XGBoost, **LightGBM**

---

## 4. Data Preprocessing
- **클래스 불균형 처리**
  - ROC-AUC 기반 평가 지표 사용
  - LightGBM의 `scale_pos_weight` 활용
- **범주형 변수 처리**
    + 순서형 변수 : Ordinal Encoder 적용
    + 일반 범주형 변수 : One-Hot Encoding 적용
- **데이터 스케일링**
    + 수치형 변수에 대해 StandardScaler(표준화) 적용
- **Pipeline 구성**
    + 전처리–모델을 하나의 파이프라인으로 구성하여 데이터 누수 방지

---

## 5. 통계분석 핵심 인사이트
- 로지스틱 회귀분석 결과,  
  **중성지방, 연령, 신체활동량 등 대사 및 생활습관 지표가  
  당뇨병 발생 확률과 통계적으로 유의한 관련성**을 보임
- 잔차 분석(Q-Q plot)을 통해 모형 가정의 적절성을 검토함

![Q-Q Plot](output/qqplot.png)

---

## 6. 모델링 평가지표
- 여러 모델을 비교한 결과, **LightGBM 모델이 가장 우수한 성능**을 보임
- 평가 지표는 **ROC-AUC를 주요 기준**으로 사용

| Model | Accuracy | Recall | F1-Score | AUC-ROC |
| :--- | :--- | :--- | :--- | :--- |
| Logistic Regression | 0.63 | 0.61 | 0.62 | 0.69 |
| Decision Tree | 0.65 | 0.60 | 0.62 | 0.70 |
| XGBoost | 0.67 | 0.63 | 0.65 | 0.72 |
| **LightGBM** | **0.68** | **0.65** | **0.66** | **0.73** |

> **Note** : 검증 데이터 기준 성능이며, ROC-AUC를 중심으로 모델을 비교함

---

## 7. Feature Importance
- LightGBM의 feature importance를 활용하여 변수 중요도 분석 수행
- 예측 기여도가 높은 변수
  1. 주간 신체활동량
  2. 중성지방
  3. 연령
- 예측 기여도가 낮은 변수(제거 대상)
  - 주당 음주량
  - 수면 시간
  - 이완기 혈압

---

## 8. Conclusion
- 생활습관 및 대사 관련 지표만으로도 당뇨병 위험 예측이 가능함을 확인
- 중요도가 낮은 변수를 제거해도 모델 성능은 유지됨
- 설문 기반 데이터로 **고위험군 선별용 예측 모델의 실용 가능성**을 제시함

---

# 보고서
- 프로젝트 상세 내용은 PDF 보고서를 참고해 주세요
- 최종 보고서 : [당뇨병 예측 모델링: 통계분석 및 머신러닝 접근](report/프로젝트보고서.pdf)
- 분석 코드 : [분석 코드](ml_pipeline_cleaned.ipynb)

---

# 🔗 배지 및 이모지 공식 소스 링크
| 용도 | 사이트 이름 | 링크 |
| :--- | :--- | :--- |
| **배지 생성** | Shields.io | https://shields.io/ |
| **로고/색상 검색** | Simple Icons | https://simpleicons.org/ |
| **이모지 검색** | Emoji Cheat Sheet | https://github.com/ikatyang/emoji-cheat-sheet |
