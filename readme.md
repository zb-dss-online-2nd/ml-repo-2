![image](https://user-images.githubusercontent.com/104780664/174287758-265973a4-a997-4e63-a52b-52e1c611c2db.png)


# 📑 목차
* [1. 프로젝트 목표](#-1-프로젝트-목표)
* [2. 기획 의도 및 배경](#-2-기획-의도-및-배경)
* [3. 데이터 소개](#-3-데이터-소개)
* [4. EDA](#-4-EDA)
* [5. 모델 선정 및 분석 기법](#-5-모델-선정-및-분석-기법)
* [6. 결론 및 한계, 보완 방법](#-6-결론-및-한계-보완-방법)
* [7. 참고 자료](#-7-참고-자료)

<br></br>


----------
## ❓ 1. 프로젝트 목표
<img width="926" alt="image" src="https://user-images.githubusercontent.com/104750108/175439553-5c6a9beb-f23f-493c-96ec-634fb0350215.png">


<br></br>


----------
## ⚾ 2. 기획 의도 및 배경
* 야구는 기록의 스포츠라는 별명이 붙을 정도로 많은 개인별/팀별 데이터를 보유하고 있음. (<a href="https://dbr.donga.com/article/view/1203/article_no/9397/ac/magazine" target="_blank">Link</a>)
* 야구의 제반 데이터 분석은 다양한 분야에서 폭넓은 관심을 끌고 있음.
* KBO 우승자 예측은 이러한 각계의 요구를 충족시키는 현실적인 방안으로 평가될 것.
* 우승 팀을 예측하는 것은 프로 야구 마케팅에 매우 긍정적인 효과를 줄 수 있을 것으로 기대됨.
* 또한 각 구단의 합리적 투자 규모 결정에도 긍정적인 영향을 줄 것으로 기대됨.
* 딥러닝 기법을 활용하여 모델을 구축한 선행연구 소개.
* 한편 기존의 연구는 개별 경기의 승자를 예측하는 데에 그치고 있어 우리 프로젝트는 한 단계 나아가 최종적인 시즌 우승 팀을 예측하고자 함.
* 추가적으로, 머신러닝 학습한 내용을 토대로 기존 연구로 주로 사용했던 딥러닝 기법이 아닌 머신러닝 기법을 최대한 활용하고자 함.


<br></br>


----------
## 🔧 3. 데이터 소개

### (1) 데이터 수집 방법 
* KBO 공식 사이트의 기록실 (<a href="https://www.koreabaseball.com/Record/Player/HitterBasic/Basic1.aspx" target="_blank">Link</a>)
* 한국 프로야구 전문 기록 사이트 STATIZ의 자료 (<a href="http://www.statiz.co.kr/community_list.php" target="_blank">Link</a>)

### (2) 변수 설명
* 구단별 특이사항
    - NC 다이노스 : 2011년 창단/ 2013년부터 경기 참여.
    - kt wiz : 2013년 창단/ 2015년부터 경기 참여.
    - 키움 히어로즈 : 2019년부터 변경됨 (전: 넥센 히어로즈)
    - SSG 랜더스: 2022년부터 변경됨

|항목명|ITEM|Type|Comment|
|:--------------------|:---|:--------|:-------------------------------------------------------|
|구단|team|int|- 구단별 No. : 2021년 순위기준으로 Numbering함</br> 1. kt wiz</br> 2. 두산 베어스</br> 3. 삼성 라이온즈</br> 4. LG 트윈스</br> 5. 키움 히어로즈</br> 6. SSG 랜더스</br> 7. NC 다이노스</br> 8. 롯데 자이언츠</br> 9. KIA 타이거즈</br> 10. 한화 이글스|
|년도|year|int|- 2015~2022년 data|
|월|month|int|- 3~10월 data</br> - 현재 경기 진행 중으로 2022년는 5월까지 데이터 수집|
|승률|pov|float|- 승수/(승수＋패수)|
|타자-출루율</br>on-base percentage|obp|float|- 타석에 나왔을 때 아웃을 당하지 않고 주자로 살아남는 확률 </br>  - (안타+볼넷+몸에 맞은 공) / (타수+볼넷+몸에 맞은 공+희생플라이) </br> - 베이스에서 많이 살아남는다는 증거가 되기 때문에 중요한 요인으로 선정|
|타자-장타율</br>Slugging Percentage|slg|float|- 총 누타수/타수 </br> - 좀 더 정확하게는 "(단타 수 + 2루타 수*2 + 3루타 수*3 + 홈런 수*4)/타수"|
|타자-타율</br>Batting Average|ba|float|- 안타/타수 </br> - 득점과 직결되는 변수이므로 가장 중요한 요인이기에 선정|
|투수-평균자책점</br>ERA|era|float|- 야구에서 투수가 한 게임(9이닝) 당 내준 평균 자책점 </br> - 타자의 타율과 마찬가지로 투수의 역량을 재는 가장 유명하고 고전적이어서 상징성이 있는 비율 스탯으로, 숫자가 낮을수록 좋은 것 </br> - 투수로서의 능력을 직접적으로 보여주는 지표(실점과도 연관)로 이해할 수 있음|
|투수-WHIP|whip|float|- (피안타 + 볼넷) / 이닝 ( 고의사구는 포함시키며 몸에 맞는 볼은 포함하지 않음) </br> - 투수의 안정감을 측정하는 지표 (낮을 수록 좋음) </br> - 볼넷 : 타자가 타석에서 4개의 볼 카운트를 얻어내 1루로 나가는 것  </br> - 이닝 : 야구 또는 소프트볼에서 양팀이 한 번씩의 공격을 주고 받는 단위.|


<br></br>


----------
## 📒 4. EDA

### (1) Grade
<p align="center"><img width="981" alt="image" src="https://user-images.githubusercontent.com/104750108/174301365-8c8eb009-9060-41d5-9412-6cf9c629abb0.png"></p>

* 승률별로 등급을 나누어 높은 Grade를 부여받는 팀이 우승할 수 있는 확률이 높은팀으로 선정
* 이전 데이터(1983-2021)의 우승팀의 승률을 확인 해봤을 때 0.508-0.706의 범위로 고르게 분포가 되어있었고, 승률이 높은팀이 우승을 한 확률이 높게 나타났으나, 반드시 승률이 높다고해서 우승을 한 팀이 되지 않았음. 그래서 이 데이터의 정확도를 높이기 위한 방법으로 총 승률을 4단계로 나누고, Grade 기준으로 봤을 때 해당 팀이 우승할 확률이 있는지 알기 위해 해당 라벨을 선정

### (2) Corrleation and heatmap
<p align="center"><img width="687" alt="image" src="https://user-images.githubusercontent.com/104750108/175441478-4e89c00d-8d57-4934-a052-3ded79fd4812.png"></p>

* 컬럼간의 상관관계를 통해 서로 얼마나 연관도가 있는지 파악하기 위해서 해당 조건을 확인함
* grade와 양의 상관관계를 가지는 항목은 pov(승률), obp(출루율), slg(장타율), ba(타율)로 나타남.

### (3) Pairplot
<p align="center"><img width="1152" alt="pairplot" src="https://user-images.githubusercontent.com/104750108/175445945-47d900d1-9a86-444f-91d6-5bfdebcf74ef.png"></p>

* 특정 컬럼의 경우 (ex) team, year, month)의 경우에는 연관도가 다른 컬럼에 비해 상대적으로 적은편이다.
* 유의미한 상관관계가 있는 컬럼들(pov, obp, slg, ba, era, whip)만 표시

### (4) Boxplot
<p align="center"><img width="794" alt="image" src="https://user-images.githubusercontent.com/104750108/174303873-d6c3315e-c214-4381-9554-94059a5341de.png"></p>

* era (평균자책점)의 경우 값의 범위가 다른 값에 비해 차이가 큰 것을 확인할 수 있음


### (5) 팀별 월 평균 승률 비교
<p align="center"><img width="70%" height="70%" alt="image" src="https://user-images.githubusercontent.com/104780664/174304025-83b5a4a6-8064-4ae6-947a-a3da89fa91dc.png"></p>

* 각 팀의 승률은 월에 상관없이 박스권 내 month는 승률에 영향을 미치지 않는 것으로 보임


<br></br>


----------
## 📉 5. 모델 선정 및 분석 기법
### (1) 모델 선정 기준
![image](https://user-images.githubusercontent.com/104780664/174300732-7c1ba5d0-1d35-4857-975d-e0a02b6f9f0d.png)

 * 승률을 기준으로 Grade라는 컬럼을 추가하여 승률에 따른 등급을 확인할 수 있게 지정
 * 변수간의 상관관계, 히트맵을 통해 얼마나 변수간에 영향을 미치는지 확인
 * 추가적으로 관계 그래프(Pairplot)를 통해 변수의 분포도 및 상관관계를 추가분석
 * 팀, 월, 연도를 모델에 넣고 분석을 진행해보고 결과에 크게 영향을 미치지 않는 경우엔 해당 컬럼은 제외
 * Boxplot 기준으로 평균자책점(ERA)은 다른 값 대비 차이가 큼
 * 총 6개의 모델(각각의 Grade 범위 상이)분석을 통해 최적의 모델을 선정

 

#### 1) Decision Tree
#### ① Test_size 및 best max_depth
<p align="center"><img alt="image" src="https://user-images.githubusercontent.com/104780664/174306840-3c45e830-1efc-42ef-8f04-6a622172d38d.png"></p>
<p align="center"><img width="450" alt="image" src="https://user-images.githubusercontent.com/104750108/175449371-c89aaacd-2bbc-4183-8e2b-ccecea807ea9.png">
</p>

* Decision Tree의 Test Size는 0.2와 0.3을 비교하여 
  0.3인 경우에 Train, Test Accuracy가 모두 높게 나와 0.3을 채택했고,
  추가적으로 Best Max Depth를 찾기 위해 GridSearchCV를 사용하여 1~15까지의 과정을 실행한 결과
  random_state가 13인 경우에 최적의 max_depth는 7, Test size는 0.3인 것을 확인 할 수 있었다.

#### ② Confusion Matrix
<p align="center"><img width="749" alt="image" src="https://user-images.githubusercontent.com/104750108/175449559-06082552-4a34-482f-b0c3-2911a03b9fa0.png"></p>

* 우리가 조사한 데이터의 특징상 grade를 총 4개로 나누었고 각 등급의 비율이 편향되는 것을 확인할 수 없어서 
  Confusion Matrix를 통한 accuracy, precision, recall, f1_score, auc를 알아보고 싶었다.
  Confusion Maxtrix 분석기법은 average 옵션을 두개로 나누어 진행했다.(micro - '전체평균', macro - '라벨 별 각 합의 평균')
  두 옵션을 비교해봤을 때 유의미한 수치의 차이는 발견되지 않았다.

#### ③ Classification Report
<p align="center"><img alt="image" src="https://user-images.githubusercontent.com/104780664/174307361-fc71e952-79ae-404d-a5df-d8c3030401a0.png"></p>

* Decision Tree의 분류 리포트를 분석해봤을 때
  Grade 비율이 얼마인지를 확인해보고
  Test Data에 대한 비율을 확인해봤는데 거의 비슷한 비율인 것을 확인할 수 있었다.

#### ④ 실제 값으로 Test 한 예측 값 및 예측 비율 (2022년 6월 13일 기준 data)
<p align="center"><img src="https://user-images.githubusercontent.com/104780664/174307408-c3a90018-2e52-4f84-85a6-947d97928f03.png"></p>

* 실제 값으로 Test한 예측값 및 예측비율을 구해보니
  롯데와 한화를 제외한 팀이 모두 포스트시즌에 진출할 가능성이 있는 팀이었으나
  예측비율은 모델에 한계점이 있어 신뢰도 높은 예측비율을 나타내지는 못했다.

#### 2) 그 외 (Logistic Regression / Random Forest / LightGBM)
<p align="center"><img width="70%" height="70%" alt="image" src="https://user-images.githubusercontent.com/104780664/174307555-2eb022c1-7e51-49ea-b7e0-ad3749650787.png"></p>

* 그 외에 추가적으로 다른 Model을 돌린결과 RandomForest가 가장 모델로서 적합한 모델인 것을 확인할 수 있었다.

<br></br>


----------
## ❗ 6. 결론 및 한계, 보완 방법
### (1) 결론
- 다양한 분석기법을 사용했을 때 RandomForest 기법이 가장 적합한 모델인 것을 확인할 수 있었음.
- 롯데, 한화를 제외한 8개 구단이 포스트 시즌 진출 가능할 것으로 예측됨.
- 데이터 분석 및 현재 진행상황을 고려했을 때 2022년 우승팀은 SSG로 예상됨.
### (2) 한계 및 보완 방법
- 데이터 부족의 문제는 KBO 이외의 리그에서 정보를 끌어오는 것으로 보완이 가능함.
- 소속한 구단의 선수들의 데이터를 포함시키면 좀 더 정교한 데이터분석 및 결과값을 볼 수 있을 것으로 보임.
- 상대 승률, 포스트 시즌 대진표 등 중요 변수 고려하지않음.
    -  이는 진출 가능성이 있는 팀을 추리거나, 혹은 정규 리그 -포스트 시즌 시기 두 단계로 나눠서 분석하는 것으로 보다 정교한 분석이 가능하게 될 것.
- 승차 등의 문제는 새로운 변수, 혹은 대리변수 설정 등으로 해결할 수 있을 것으로 보이며 이는 추가적인 연구 및 시도가 필요해 보인다.


<br></br>


----------
## 📌 7. 참고 자료
- KBO 공식 사이트의 기록실 (<a href="https://www.koreabaseball.com/Record/Player/HitterBasic/Basic1.aspx" target="_blank">Link</a>)
- 한국 프로야구 전문 사이트 (<a href="http://www.statiz.co.kr/community_list.php" target="_blank">Link</a>)

- 한국 프로야구 전문 사이트 kbreport의 자료 (<a href="http://www.kbreport.com/teams/pitcher/main?rows=20&order=TPCT&orderType=DESC&teamId=1&pitcher_type=&year_from=2021&year_to=2021&split01=opposite&split02_1=2&split02_2=" target="_blank">Link</a>)
- Betman (<a href="https://www.betman.co.kr/main/mainPage/gamebuy/closedGameSlip.do?gmId=G024&gmTs=1281" target="_blank">Link</a>)
- 장원석 책임연구원, '예전의 '명감독'은 잊어라, 데이터가 우승을 이끈다', DBR donga기술, 2019년 12월 286호 (<a href="https://dbr.donga.com/article/view/1203/article_no/9397/ac/magazine" target="_blank">Link</a>)
- 김홍재 칼럼니스트, '야구는 숫자놀음? “승부를 지배하는 숫자들”', 과학기술, 2021년 5월 25일자. (<a href="https://www.sciencetimes.co.kr/news/%EC%95%BC%EA%B5%AC%EB%8A%94-%EC%88%AB%EC%9E%90%EB%86%80%EC%9D%8C-%EC%8A%B9%EB%B6%80%EB%A5%BC-%EC%A7%80%EB%B0%B0%ED%95%98%EB%8A%94-%EC%88%AB%EC%9E%90%EB%93%A4/" target="_blank">Link</a>)

- 노언석 최재현, "기계학습을 활용한 프로야구 승부예측에 관한 연구", 2017
- 김태훈 외 3인, "인공지능 모델에 따른 한국 프로야구의 승패 예측 분석에 관한 연구", 2020
- 김원종 외 2인, "데이터 마이닝을 활용한 한국 프로야구 구단의 승패예측과 승률향상을 위한 전략 도출 연구", 2018  


