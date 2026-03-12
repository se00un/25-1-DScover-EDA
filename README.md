# 25-1-DScover_EDA

자세한 데이터 분석 과정과 후기는 블로그에 있습니다.
  <br/> 👉 블로그 링크: https://blog.naver.com/develop0420/224185083087
  
**Competition**  
Kaggle – Playground Series Season 4 Episode 2  
Multi-Class Prediction of Obesity Risk  
https://www.kaggle.com/competitions/playground-series-s4e2


---


## Exploratory Data Analysis

EDA의 목표는 **변수 간 관계를 이해하고 feature engineering 방향을 설정하는 것**이었다.

### Correlation Analysis

#### Pearson Correlation (연속형 변수)

연속형 변수 간 상관관계를 확인하였다.

- 강한 상관관계는 거의 없음
- 다중공선성 위험은 낮다고 판단

#### Cramér’s V (범주형 변수)

target(`NObeyesdad`)과의 관계를 확인한 결과

상대적으로 높은 관계를 보인 변수

- `gender`
- `family_history_with_overweight`

---

### Target vs Feature Analysis

각 범주에서 obesity가 얼마나 발생하는지 시각화를 통해 확인하였다.

특히 다음 변수들이 중요한 패턴을 보였다.

- Family history with obesity
- Gender
- High calorie food consumption

---

### ANOVA

연속형 변수와 target 간 관계를 확인하기 위해 **ANOVA 검정**을 수행하였다.

- 대부분 수치형 변수들이 **p < 0.05**
- target과 통계적으로 유의미한 관계 존재

---

## Feature Engineering

EDA 결과를 바탕으로 여러 파생변수를 생성하고 검증하였다.

### BMI Feature

비만 예측에서 중요한 지표이므로 직접 생성하였다.
BMI = Weight / Height²



### Healthy Eating Score

식습관 관련 변수들을 결합하여 새로운 지표를 생성하였다.

사용 변수

- FAVC
- FCVC
- NCP
- CAEC
- CH2O
- CALC

통계적 검증

- Pearson correlation: **0.263**
- Spearman correlation: **0.255**
- p-value: **< 0.001**

→ 통계적으로 유의미한 관계 확인


### Sedentary Score

좌식 생활 가설 기반 파생 변수
Sedentary Score = TUE - FAF

상관계수

- Pearson: **0.034**
- Spearman: **0.061**

→ 관계가 거의 없어 **feature drop**

---

### Clustering

Age와 BMI 기반 클러스터링 수행

- k = **3**

그러나 모델에서 **feature importance가 낮아 drop**

---

### Polynomial Features

Age와 BMI의 **비선형 관계 학습**을 위해

- Polynomial degree = **2**

적용

---

## Feature Selection

LightGBM feature importance 기반으로 일부 변수 제거

drop features

- BMI²
- Age²
- SMOKE
- FAVC

---
## Modeling

모델 비교는 **PyCaret AutoML**을 사용하였다.

Top models

1. LightGBM  
2. XGBoost  
3. Gradient Boosting

---

### LightGBM

Random Search 기반 하이퍼파라미터 튜닝 수행

최적 파라미터
- num_leaves = 20
- n_estimators = 100
- max_depth = 5
- learning_rate = 0.1

---

## Result

최종 성능

- PyCaret baseline accuracy: **0.9040**
- Test accuracy: **0.89**
- Tuned LightGBM accuracy: **0.9066**

