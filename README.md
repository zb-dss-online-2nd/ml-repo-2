# ml-repo-2
## 주제 및 목표
* 주제
  * 머신러닝을 이용한 2022년 KBO(Korea Baseball Organization) 리그 우승팀 예측

* 목표 
  1. 2022년 KBO 정규리그 예상 우승팀은?
  2. 2022년 KBO 최종 우승팀은?

https://www.betman.co.kr/main/mainPage/gamebuy/closedGameSlip.do?gmId=G024&gmTs=1281

## 기획의도 및 배경

야구는 기록의 스포츠라는 별명이 붙을 정도로 많은 개인별/팀별 데이터를 보유하고 있음
야구의 제반 데이터 분석은 다양한 관점에서 많은 기술자들의 폭넓은 관심을 끌고 있음

야구에서 발생하는 경제효과는 2조원에 이르는 규모
https://magazine.hankyung.com/business/article/202204133226b
우승 팀을 예측하는 것은 프로 야구 마케팅에 매우 긍정적인 효과를 줄 수 있을 것으로 기대됨

한편 기존의 연구는 개별 경기의 승자를 예측하는 데에 그치고 있음
우리 프로젝트는 한 단계 나아가 최종적인 시즌 우승 팀을 예측하고자 함

## 선행자료 조사

선행 연구는 딥러닝 기법을 활용하여 모델을 구축
이번 프로젝트는 머신러닝을 활용 

  * 노언석 최재현, "기계학습을 활용한 프로야구 승부예측에 관한 연구", 2017

https://www.kci.go.kr/kciportal/ci/sereArticleSearch/ciSereArtiView.kci?sereArticleSearchBean.artiId=ART002357955

## 모델
 * 앙상블 배깅& 부스팅
 * 의사결정나무 
 * 베이즈넷
 
### 데이터 수집 방법 

* KBO 공식 사이트의 기록실 (https://www.koreabaseball.com/Record/Player/HitterBasic/Basic1.aspx)
* 한국 프로야구 전문 사이트 STATIZ의 자료 (http://www.statiz.co.kr/community_list.php)
   이상을 활용한 csv 파일 생성

### 변수 처리
무승부 - 제외 처리( 총 데이터 중 유의미하게 큰 비율을 차지하지 않음, 기존 연구 역시 제외처리하고 있음)


## 참고자료


노언석 최재현, "기계학습을 활용한 프로야구 승부예측에 관한 연구", 2017
김태훈 외 3인, "인공지능 모델에 따른 한국 프로야구의 승패 예측 분석에 관한 연구", , 2020
김원종 외 2인, "데이터 마이닝을 활용한 한국 프로야구 구단의 승패예측과 승률향상을 위한 전략 도출 연구", 2018

 KBO 공식 사이트의 기록실 (https://www.koreabaseball.com/Record/Player/HitterBasic/Basic1.aspx)
 
 한국 프로야구 전문 사이트 STATIZ의 자료 (http://www.statiz.co.kr/community_list.php)
