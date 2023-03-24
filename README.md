# LG-AI_Radar [(Link)](https://dacon.io/competitions/official/235927/overview/description)

# Public score 1st 1.89089 | Private score 1st 1.909

주최 : LG AI Research

주관 : DACON

규모 : LB 1등/975팀 (총 1700여명 참가)

=================================================

1차 코드 검증 통과 후 발표까지 진행하였지만 주최측 내부 찬반 논의 끝에 최종적으로는 data leakage 판정을 받게 되었습니다. data leakage 판정을 받은 부분은 moving_avg와 moving_median 부분으로 이 점 참고하고 코드 확인해주시면 감사하겠습니다. (data leakage 판정 받은 파생변수를 제거하더라도 스코어 1등은 유지합니다)

*moving_avg와 moving_mean을 사용한 이유

데이터 분석은 분석의 목적을 반영해야한다고 생각합니다. 저희는 대회 개요에 명시되어 있는 것과 마찬가지로 실제 공정 과정에서의 상황을 고려하여 공정을 마친 데이터(이전 데이터)는 사용해도 된다고 판단하였습니다.

저희는 moving_avg, moving_median을 30을 기준으로 생성하였습니다. 

1)이는 동일 확률을 가진 샘플을 30개 이상 뽑으면 그 분포는 정규분포를 이룬다는 중심극한정리에 의해 정하였습니다. 공정데이터가 주기를 띄고 있는 것을 보아 어느정도 잘 통제된 공정이라 판단했고, 2)잘 통제된 공정은 보통 정규분포를 이룹니다. 

1)+2)를 통해 30이라는 숫자를 도출했고, 편의상 moving_avg를 사용하긴 했지만 사실 앞에 랜덤의 29개만 있으면 상관없습니다.
