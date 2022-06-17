# README1

# ml-repo-2 # 2조/ 팀원 이종승 이주연 김바램

## 주제 및 목표

- 주제
    - 머신러닝을 이용한 2022년 KBO(Korea Baseball Organization) 리그 우승팀 예측
- 목표
    1. 2022년 KBO 포스트 시즌 진출 5개 팀 예측
    2. 2022년 KBO 최종 우승팀 예측

## 기획의도 및 배경

- 야구는 기록의 스포츠라는 별명이 붙을 정도로 많은 개인별/팀별 데이터를 보유하고 있음 야구의 제반 데이터 분석은 다양한 분야에서 폭넓은 관심을 끌고 있음 KBO 우승자 예측은 이러한 각계의 요구를 충족시키는 현실적인 방안으로 평가될 것 https://dbr.donga.com/article/view/1203/article_no/9397/ac/magazine
- 야구에서 발생하는 경제효과는 2조원에 이르는 규모 https://magazine.hankyung.com/business/article/202204133226b
    - 우승 팀을 예측하는 것은 프로 야구 마케팅에 매우 긍정적인 효과를 줄 수 있을 것으로 기대됨
    - 또한 각 구단의 합리적 투자 규모 결정에도 긍정적인 영향을 줄 것으로 기대됨
- 한편 기존의 연구는 개별 경기의 승자를 예측하는 데에 그치고 있음 우리 프로젝트는 한 단계 나아가 최종적인 시즌 우승 팀을 예측하고자 함

## 선행자료 조사

딥러닝 기법을 활용하여 모델을 구축한 선행연구 소개. 이번 프로젝트는 머신러닝을 활용

- 노언석 최재현, “기계학습을 활용한 프로야구 승부예측에 관한 연구”, 2017
- 김태훈 외 3인, “인공지능 모델에 따른 한국 프로야구의 승패 예측 분석에 관한 연구”, 2020
- 김원종 외 2인, “데이터 마이닝을 활용한 한국 프로야구 구단의 승패예측과 승률향상을 위한 전략 도출 연구”, 2018

https://www.kci.go.kr/kciportal/ci/sereArticleSearch/ciSereArtiView.kci?sereArticleSearchBean.artiId=ART002357955

## 모델

- 앙상블 배깅& 부스팅
- 의사결정나무
- 베이즈넷

## 데이터 수집 방법

- KBO 공식 사이트의 기록실 (https://www.koreabaseball.com/Record/Player/HitterBasic/Basic1.aspx)
- 한국 프로야구 전문 사이트 STATIZ의 자료 (http://www.statiz.co.kr/community_list.php) 이상을 활용한 csv 파일 생성

## 변수 처리


|항목명|ITEM|DataType|Comment|
| :---: | :---: | :---: | :--- |
| 구단 | team | int(1~10) |   |
| 년도 | year | int(2015~2022) |   |
| 월 | month | int(3~10) |   |
| 승률 | pov | float(0~1) | - 승수/(승수＋패수) |
|   | 출루율on-base percentage | obp | float(0~1) |
| 장타율Slugging Percentage | slg | float(0~1) | - '총 누타수/타수.' - 좀 더 정확하게는 '(단타 수[1] + 2루타 수*2 + 3루타 수*3 + 홈런 수*4)/타수.' |
| 타율Batting Average | ba | float | - 안타/타수- 득점과 직결되는 변수이므로 가장 중요한 요인이기에 선정 |
|   | 평균자책점ERA | era | float |
| WHIP | whip | float | - (피안타 + 볼넷) / 이닝 ( 고의사구는 포함시키며 몸에 맞는 볼은 포함하지 않음)- 투수의 안정감을 측정하는 지표 (낮을 수록 좋음)- 볼넷 : 타자가 타석에서 4개의 볼 카운트를 얻어내 1루로 나가는 것  이닝 : 야구 또는 소프트볼에서 양팀이 한 번씩의 공격을 주고 받는 단위. |
| WHIP | whip | float |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |

|  |  |
| --- | --- |
|  |  |
|  |  |
- 무승부 - 제외 처리( 총 데이터 중 유의미하게 큰 비율을 차지하지 않음, 기존 연구 역시 제외처리하고 있음)
- 변수 선정
- 팀 단위 - 승률, 상대승률, 기대승률, 상대팀에 대한 평균자책점
- 상대승률 - 승률의 경우에는 팀 순위와 직접적으로 연관이 있기 때문에 팀 변수중에 가장 중요한 요인으로 보고 선정.
- 개인별 단위
- 타자 - 타율, 출루율, 장타율 타율의 경우에는 득점과 직결되는 변수이므로 가장 중요하다고 볼 수 있음 출루율의 경우에도 베이스에서 많이 살아남는다는 증거가 되기 때문에 중요한 요인으로 선정 가능
- 투수 - 선발승률, 평균자책점, 볼넷/이닝 선발 투수는 팀에서 가장 우수한 인력이기 때문에 실점을 최대한 하지 않기 위해서 뽑아내는 주요요인으로 선정 평균 자책점(방어율)은 투수로서의 능력을 직접적으로 보여주는 지표(실점과도 연관)로 이해할 수 있음
- 각 변수는 직전 연도 시즌 종료를 기준으로 함
- 기타 변수가 제반 사정에 따라서 추가될 수 있음

## 세부 진행 방안

1. 데이터 수집 및 가공 (2017년~2022년 활용)
2. 트레이닝 데이터 및 테스트 데이터 분할
3. 앙상블 배깅& 부스팅, 의사결정나무, 베이즈넷 각 모델을 활용하여 각각의 모델 트레이닝
4. 테스트 데이터를 활용하여 예측 모델 성능 평가
5. 승률 기준 5개 팀 나열 -> 포스트 시즌 진출 팀으로 추정
6. 최종 우승 후보 선정
7. 보고서 작성

## 참고자료

노언석 최재현, “기계학습을 활용한 프로야구 승부예측에 관한 연구”, 2017
김태훈 외 3인, “인공지능 모델에 따른 한국 프로야구의 승패 예측 분석에 관한 연구”, 2020
김원종 외 2인, “데이터 마이닝을 활용한 한국 프로야구 구단의 승패예측과 승률향상을 위한 전략 도출 연구”, 2018

KBO 공식 사이트의 기록실 (https://www.koreabaseball.com/Record/Player/HitterBasic/Basic1.aspx)
한국 프로야구 전문 사이트 STATIZ의 자료 (http://www.statiz.co.kr/community_list.php)
한국 프로야구 전문 사이트 kbreport의 자료 (http://www.kbreport.com/teams/pitcher/main?rows=20&order=TPCT&orderType=DESC&teamId=1&pitcher_type=&year_from=2021&year_to=2021&split01=opposite&split02_1=2&split02_2=)
1. https://www.gallup.co.kr/gallupdb/reportContent.asp?seqNo=1283#A 2. https://www.koreabaseball.com/Reference/Ebook/EbookPublication.aspx 3. https://namu.wiki/w/%EB%B6%84%EB%A5%98:KBO%20%EB%A6%AC%EA%B7%B8/%EC%8B%9C%EC%A6%8C 4. https://namu.wiki/w/KBO%20%ED%95%9C%EA%B5%AD%EC%8B%9C%EB%A6%AC%EC%A6%88

https://www.betman.co.kr/main/mainPage/gamebuy/closedGameSlip.do?gmId=G024&gmTs=1281
