# LG-AI_Radar [(Link)](https://dacon.io/competitions/official/235927/overview/description)

## 🏆 Result
# **Public score 1st** 1.89089 | **Private score 1st** 1.909

주최 : LG AI Research

주관 : DACON

규모 : LB 1등/975팀 (총 1700여명 참가)

=================================================

🔥
## **ISSUE**

코드 검증 통과 후 발표까지 진행하였지만 주최측 내부 찬반 논의 끝에 최종적으로는 data leakage 판정을 받게 되었습니다.    

data leakage 판정을 받은 부분은 moving_avg와 moving_median 부분으로 이 점 참고하고 코드 확인해주시면 감사하겠습니다. (data leakage 판정 받은 파생변수를 제거하더라도 스코어 1등은 유지합니다)   

```moving_avg와 moving_mean을 사용한 이유```   
 
데이터 분석은 분석의 목적을 반영해야한다고 생각합니다.    

저희는 대회 개요에 명시되어 있는 것과 마찬가지로 ```실제 공정 과정에서의 상황을 고려하여 공정을 마친 데이터(이전 데이터)는 사용해도 된다고 판단```하였습니다.   

저희는 ```moving_avg, moving_median을 30을 기준으로 생성```하였습니다.    

1)이는 ```동일 확률을 가진 샘플을 30개 이상 뽑으면 그 분포는 정규분포를 이룬다는 중심극한정리에 의해 정하였습니다.``` 공정데이터가 주기를 띄고 있는 것을 보아 어느정도 잘 통제된 공정이라 판단했고,    

2)```잘 통제된 공정은 보통 정규분포```를 이룹니다.    

1)+2)를 통해 30이라는 숫자를 도출했고, 편의상 moving_avg를 사용하긴 했지만 사실 앞에 랜덤의 29개만 있으면 상관없습니다.   

매우 아쉬운 대회라고 생각듭니다.   

=================================================
  
  
    
  
  
✏️
## **대회 느낀점**
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
   측정 오차 보정 + 시계열 데이터 특성 + 자기 상관성 요소를 반영하기 위해
   kalman filter를 사용하였습니다.

   또한 이러한 데이터 특성을 활용하기 위해
   일반적인 K-Fold 방식이 아닌, Stratify K-Fold 방법을 사용하였습니다.
   데이터의 각 주기에 class를 부여하여 Stratify 형식으로 학습을 진행하였고
   이 전과 비교해서 성능 향상에 큰 도움을 주었습니다.

   또한 Shapely Value를 활용해서 주최사에게 어떠한 변수들이 제일 중요했는지 알려주었는데
   이를 통해 XAI를 처음 접하게 되어 매우 흥미로웠습니다.

   이번 대회를 통해 데이터에 Domain을 활용할 수 있는 능력을 배웠던 것 같습니다.
   가설을 세우고 그걸 성능과 등수를 통해 검증받는 경험은 정말 짜릿했습니다.
   비록 Data-Leakage로 수상을 하지는 못하였지만
   약 2000여명 중 1등의 예측력을 가지는 모델을 만들었다는 사실에
   다음 대회에서는 꼭 Data-Leakage 이슈에 휘말리지 않고
   수상을 하겠다라고 다짐을 하게 되는 대회였습니다.
   
===========================================================================


## 🧐 About
AI 기술을 활용하여 공정 데이터와 제품 성능간 상관 분석을 통해 제품의 불량을 예측/분석하고, 
수율을 극대화하여 불량으로 인한 제품 폐기 비용을 감축 시키기 위한 대회


1. 공정 데이터를 활용하여 Radar 센서의 안테나 성능 예측을 위한 **AI 모델** 개발


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
```
Dataset Info.

1. 학습(Train) 데이터셋 (39607개)

설명: ID, X Feature(56개), Y Feature(14개)

2. 테스트(Test) 데이터셋 (39608개)

설명: ID, X Feature(56개)

```


## 🔧 Feature Engineering
```

smoothing
- kalman filter, moving average, moving median, log
분산 / 합 / 차

```

## 🎈 Modeling

**Predict Model**
```
Autogluon
Catboost
```


