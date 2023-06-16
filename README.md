# LG-AI_Radar [(Link)](https://dacon.io/competitions/official/235927/overview/description)

## 🏆 Result
# **Public score 1st** 1.89089 | **Private score 1st** 1.909

주최 : LG AI Research

주관 : DACON

규모 : LB 1등/975팀 (총 1700여명 참가)

=================================================

🔥
**ISSUE**

코드 검증 통과 후 발표까지 진행하였지만 주최측 내부 찬반 논의 끝에 최종적으로는 data leakage 판정을 받게 되었습니다. 

data leakage 판정을 받은 부분은 moving_avg와 moving_median 부분으로 이 점 참고하고 코드 확인해주시면 감사하겠습니다. (data leakage 판정 받은 파생변수를 제거하더라도 스코어 1등은 유지합니다)

*moving_avg와 moving_mean을 사용한 이유

데이터 분석은 분석의 목적을 반영해야한다고 생각합니다. 

저희는 대회 개요에 명시되어 있는 것과 마찬가지로 실제 공정 과정에서의 상황을 고려하여 공정을 마친 데이터(이전 데이터)는 사용해도 된다고 판단하였습니다.

저희는 moving_avg, moving_median을 30을 기준으로 생성하였습니다. 

1)이는 동일 확률을 가진 샘플을 30개 이상 뽑으면 그 분포는 정규분포를 이룬다는 중심극한정리에 의해 정하였습니다. 공정데이터가 주기를 띄고 있는 것을 보아 어느정도 잘 통제된 공정이라 판단했고, 

2)잘 통제된 공정은 보통 정규분포를 이룹니다. 

1)+2)를 통해 30이라는 숫자를 도출했고, 편의상 moving_avg를 사용하긴 했지만 사실 앞에 랜덤의 29개만 있으면 상관없습니다.

매우 아쉬운 대회라고 생각듭니다.

===========================================================================

✏️
**대회 느낀점**
 - 아이디어를 제일 잘 살렸던 대회라고 생각합니다.
   공정 과정 중에는 여러가지 상황이 존재할 것으로 가정하고 데이터에 접근하였는데
   1. **기계 마모**: 기계가 오래되거나 마모되면, 그 성능이 저하되고, 이로 인해 수집된 데이터의 신뢰성이 떨어질 수 있습니다.
   2. **데이터 일관성 부족**: 공정이 변경되거나, 장비가 교체되거나, 원재료 품질이 변동되는 등의 이유로 데이터 간의 일관성이 떨어질 수 있습니다. 이는 분석을 복잡하게 만들며, 잘못된 결론을 내릴 수 있게 합니다.
   3. **센서 오류 또는 결함**: 기계에서 데이터를 수집하는 주된 방법 중 하나는 센서를 사용하는 것입니다. 센서가 결함이 있거나 오작동하면, 데이터가 왜곡
   4. **환경적 요인**: 환경적 노이즈도 문제가 될 수 있습니다. 예를 들어, 온도, 습도, 기압 등의 변화는 센서의 성능에 영향을 미치고, 이는 데이터에 노이즈를 더하는 결과
  
   위와 같은 이유들로 데이터의 노이즈를 줄이는게 제일 중요한 대회라고 판단을 하여서
   **kalman filter, moving average, low-pass filter**와 같은 여러 스무딩 기법들을 사용하였고,
   그 중에서 kalman filter 적용 시 성능이 가장 좋게 상승하였습니다.
   kalman filter는 '예측'과 '업데이트'라는 두 단계를 반복하는데, 시스템의 동적 모델과 가우시안 노이즈의 통계적 특성을 이용해 '예측' 단계에서는 시스템의 다음 상태를 예측하고, '업데이트' 단계에서는 새로운 측정 데이터를 사용해 이 예측을 조정하기 때문에
   현재 노이즈가 있는 데이터라는 가설 상황에서 제일 최적의 필터라고 생각하여 사용하였습니다.
   
   
===========================================================================


## 🧐 About
생육 환경 생성 AI 모델 결과를 바탕으로 상추의 일별 최대 잎 중량을 도출할 수 있는 최적의 생육 환경 조성


1. 상추의 일별 잎중량을 예측하는 **AI 예측 모델** 개발 
2. 1번의 예측 모델을 활용하여 생육 환경 **생성 AI 모델** 개발 
  - 생성 AI 모델 결과로부터 상추의 일별 최대 잎 중량을 도출할 수 있는 최적의 생육 환경 조성 및 제안 

---
## 🖥️ Development Environment
```
Google Colab
OS: Ubuntu 18.04.6 LTS
CPU: Intel(R) Xeon(R) CPU @ 2.20GHz
RAM: 13GB
```
---
## 🔖 Project structure

```
Project_folder/
|- data/  # required data (csv)
|- feature/  # feature engineering (py)
|- garbage/  # garbage 
|- generative_model/  # CTGAN Model (pkl)
|- predict_model/  # Autogluon Model (pkl)
|- config  # Setting (py)
|- *model  # notebook (ipynb)
|_ [Dacon]상추의-생육-환경-생성-AI-경진대회_상추세요  # ppt (pdf) 
```
## 📖 Dataset
**Data Source**  [Train Test Dateset](https://dacon.io/competitions/official/236033/data)
```
Dataset Info.

1. 학습(Train) 데이터셋 (39607개)

설명: ID, X Feature(56개), Y Feature(14개)

2. 테스트(Test) 데이터셋 (39608개)

설명: ID, X Feature(56개)

```


## 🔧 Feature Engineering
```
Feature selection.

누적값
- 구간별 시간에 대한 feature의 누적값
ex x 분무량
- 전체 평균에 대한 ec관측치와 분무량의 곱
수분량
- 자체 수분량 공식 사용
하루 평균
- 온도, 습도, co2, ec, 분무량, 적생광에 대한 하루 평균
Low-pass filter
- 누적값, ec x 분무량, 일평균에 적용
Kalman filter
- 누적값, ec x 분무량, 일평균에 적용
이동 평균
- 누적값, ec x 분무량, 수분량, 일평균에 적용
이동 중앙값
- 누적값, ec x 분무량, 수분량, 일평균에 적용
```

## 🎈 Modeling

**Predict Model**
```
AutoML: Autogluon, pycarat
Catboost
```
**Generative Model**
```
CTGAN
GAN
```


## 😃 Result
- **Public score** 1st 3.16772 | **Private score** 4th 7.65751
- https://dacon.io/competitions/official/236033/overview/description

## 📖 Reference
CTGAN  
https://arxiv.org/abs/1907.00503  
https://github.com/sdv-dev/CTGAN  

생육환경  
https://scienceon.kisti.re.kr/srch/selectPORSrchArticle.do?cn=NPAP08069532&dbt=NPAP

===========================================================================

상추의 생육 환경 데이터(0일~27일치)를 바탕으로 전처리와 피처 엔지니어링을 통해 예측 모델을 학습시켜 먼저 예측 모델의 성능을 publice score 1등으로 끌어올렸습니다. 

예측 모델의 정확도를 올린 후 생성 모델(CTGAN)을 통해 가짜 환경 데이터를 만든 후 학습 시켰던 예측 모델로 예측을 진행하였습니다.

예측 모델의 정확도가 public score 상 1등이었기 때문에 가짜 환경 데이터에 대한 예측값은 신뢰할 수 있는 데이터라 판단하였고 그 후 주최사인 KIST에 최적의 생육환경을 제시하기 위한 Flow를 생각해야 했습니다. 

먼저, KIST 연구원분들은 각종 생육 환경 관련 논문을 바탕으로 실험을 해보았을 것이라는 전제를 바탕으로 저희만의 unique한 생육 환경을 제시해보자 판단하였고, Feature Engineering을 진행하며 중요한 데이터들을 알아냈었고 최저 잎 중량이었던 환경 데이터들을 직접 조작하여 생성 모델에 학습시킨 후 가짜 환경 데이터를 예측 모델이 예측하여 최저 잎 중량에서 최대 잎 중량을 도출하였습니다.

그렇게 최종 2등으로 수상하였습니다.
