

## 머신러닝
방대한 데이터를 바탕으로 스스로 패턴을 학습하고 예측/결정을 내리게 하는 AI 기술
전통적인 conventional? programming is rule based 
In contrast, 컴퓨터에게 데이터/정답(label)을 입력하여 그 안에 내재된 논리와 규칙(Model)을 기계가 직접 찾아내도록 훈련시킨다 
학습 단계 - 방대한 양의 훈련 데이터를 분석해 알고리즘이 매개변수를 조정하며 모델 생성
- 지도 학습 Supervised Learning : 입력 데이터와 정답(라벨)이 함꼐 주어진 상태에서 모델을 학습시킴. 스팸 메일 분류, 주택 가격 예측
- 비지도 학습 Unsupervised Learning: 정답 없이 입력 데이터만 주어지며, 기계가 데이터의 숨겨진 구조, 패턴, 그룹을 스스로 파악 : 고객 세분화, 추천 시스템, 데이터 압축
- 강화 학습 Reinforcement Learning: 컴퓨터가 어떤 환경에서 행동을 취하고, 그에 따른 보상이나 벌점을 통해 시행착오를 겪으며 최적의 행동 방식을 학습 

예측 단계 - 완성된 모델에 새로운 미지의 데이터를 입력하여 분류나 수치 예측 등 결과를 도출 

## Machine Learning vs Deep Learning
- 딥러닝이 머신러닝의 한 종류. 딥러닝은 인공 신경망 기반
- 머신 러닝은 사람이 직접 데이터의 특징을 추출해야 하지만, 딥 러닝은 데이터양이 매우 많을 경우 기계가 스스로 특징을 추출하고 학습할 수 있어 이미지 인식, 음성 인식, 대규모 언어 모델 등에 직접 쓰임

## 과적합 
과적합 : 학습 데이터는 잘 맞추는데, 실제 새로운 데이터는 못 맞추는 상태 
- 정상적인 모델은 "관심사가 겹칠수록 성공 확률이 높네"같은 패턴을 배우지만, 과적합된 모델은 "철수 + 영희 = 성공"처럼 데이터를 통째로 외워버림 -> 모델이 학습 데이터의 일반적인 패턴이 아니라 노이즈나 개별 사례까지 과도하게 학습해서, 새로운 데이터에 대한 성능이 떨어지는 현상이다.
- Overfitting is a phenomenon where a model learns not only the general patterns in the training data but also the noise and specific cases, resulting in poor performance on new, unseen data. 
- Train 정확도에 비해 Test 정확도가 너무 낮으면 과적합 의심
- 머신러닝의 목표는 학습 데이터 맞추기가 아닌 새로운 데이터에서도 잘 맞추기


Feature : 모델이 보는 정보
나이차 = 3
거리 = 5km
관심사 겹침 = 4

Label : 정답
매칭 성공 = 1
매칭 실패 = 0

Dataset : feature + label 
| age_gap | distance | interest_overlap | label |
| ------- | -------- | ---------------- | ----- |
| 2       | 3        | 5                | 1     |
| 10      | 30       | 0                | 0     |


Cold start : 학습에 필요한 데이터가 거의 없어서 추천을 제대로 못 하는 상태 
ex 신규 유저 가입 -> 좋아요 기록 없음, 매칭 기록 없음, 대화 기록 없음 -> AI가 취향을 몰라서 추천을 못함 
tinder 나 hinge는 이거 해결 위해서 온보딩 설문을 한다. 

Cold Start is a problem in recommendation systems where there is insufficient historical data about new users or items, making it difficult to generate accurate recommendations.


## 머신러닝 파이프라인 
라벨링된 데이터를 받아서, Catboost 모델을 학습시키고 결과를 평가할 수 있다  
- Raw data -> feature -> label -> Train/Test split -> Model Training -> Evaluation -> Prediction

### Train/Test split은 과적합을 막기 위해 필요 
### Train과 Test를 분리해야 하는 이유 : 모델이 학습하지 않은 새로운 데이터에서도 잘 동작하는지 검증하기 위해 

## 평가 
- Accuracy, Precision, Recall, AUC의 차이?
- Accuracy: 전체 중 몇개 맞췄냐   
- Precision : 성공이라고 예측한 사람 중 진짜 성공 비율 
- Recall : 실제 성공할 사람을 얼마나 놓치지 않았는가 
* Precision vs Recall : Precision 높은 모델은 10명 추천해서 8명 성공, Recall 낮은 모델은 성공할 사람 50명인데 8명만 찾음 
- AUC : 성공할 사람과 실패할 사람을 얼마나 잘 구분하는지 
추천 시스템의 경우 Precision, Recall, AUC 균형


## Confusion Matrix : TP, TN, FP, FN
| 실제 | 예측 | 의미 |
| -- | -- | -- |
| 성공 | 성공 | TP |
| 실패 | 실패 | TN |
| 실패 | 성공 | FP |
| 성공 | 실패 | FN |
true positive, true negative, false postivie, false negative



## CatBoost 
Feature들을 보고 Label을 예측하는 머신러닝 모델 
장점 : 표 데이터에 강함, 범주형 처리 강함, 튜닝 적어도 성능 좋음 

## LightGBM 
Light Gradient Boosting Machine 
표 형태에서 엄청 강력한 머신러닝 모델 

캣부스트 라이트지비엠 다 gradient boosting 계열 
결정 트리를 엄청 많이 만들어서 결과를 합침 


## Gradient Boosting 
여러 개의 약한 모델(결정 트리)을 순서대로 만들어서, 이전 모델의 실수를 계속 보완하는 방법 
그래서 강화/오차 줄이기 

## Random Forest와 차이?
Random Forest: 트리 여러 개를 독립적으로 생성
Gradient Boosting : 트리 여러 개를 순차적으로 생성하여 앞 트리 실수를 뒤 트리가 보완하는 앙상블 기법 
Gradient Boosting is an ensemble learning method that improves prediction performance by sequentially adding decision trees, with each tree focusing on correcting the mistakes of the previous ones.
그래서 보통 Gradient Boosting > Random Forest 인 경우가 많음 

## 결정트리
- 결정트리는 질문을 계속 하는 모델 (if/else, if/else를 자동으로 만드는 모델)
- 데이터를 보고 어떤 질문이 가장 잘 구분하는지 스스로 찾음 
- 결정트리는 데이터를 여러 조건으로 반복 분할하면서 최종 예측값에 도달하는 트리 형태의 머신러닝 모델이다.
- A decision tree is a machine learning model that makes predictions by recursively splitting data based on a series of decision rules.

### 그럼 결정트리는 어떤 기준으로 질문을 선택하는가?
- 결정트리는 무슨 질문을 해야 성공/실패를 가장 잘 구분할 수 있을까를 고민하는데, 이를 수치화한게 Entropy/Gini
- Entropy : 데이터가 얼마나 섞여있는가  -> 높으면 뒤죽박죽
- Gini : 데이터를 잘못 분류할 확률 
- Information Gain : 질문을 했을 떄 얼마나 불확실성이 줄어드는가?
- 그래서 결정트리는 Information Gain이 가장 큰 질문을 반복 선택하며 데이터를 분할하는 모델. 
- Decision trees select the feature and split point that maximize information gain (or minimize impurity such as Gini impurity or entropy).


## Feature Engineering 
원본 데이터를 모델이 학습하기 좋은 형태로 변환하는 과정 
ex 모델이 궁금한거 28살인가 25살인가가 아니라, 둘의 나이차이가 얼마나 나나? -> 나이차 데이터 넣기 

좋은 Feature의 특징 : 비즈니스 의미가 있음, 결과와 관련 있음
