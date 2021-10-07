---
description: '- 오늘 보고 싶은 영화는 혹시 이것?'
---

# Movie recommendation

## 프로젝트 소개

사용자 평점을 이용하여 영화 추천시스템 모형을  R의 recommenderlab 패키지를 사용하여 모델을 구축하고 과거에 보았던 영화 몇 가지를 선택하면 이와 유사한 영화를 추천해주는 서비스 기능을 rshiny로 구현한 프로젝트입니다. 

협업필터링과 하이브리드 알고리즘 중심으로 분석을 수행하였고 영화 추천이라는 다소 진부한 주제일 수 있으나 이 프로젝트를 통해 추천 알고리즘의 원리와 사용자 맞춤형 추천 시스템에 대한 비즈니스적인 이해를 높일 수 있는 계기가 되었습니다.

\[ㅇ\]\(\#\)

## 기술스택

1. Front-End : `Shiny`
2. Back-End : `R`
3. 프로젝트 및 코드 관 : `google drive`

| 구분 | 소프트웨어 | 버전 |
| :--- | :--- | :--- |
| Language | R | 4.0.5 |
| IDE | R-studio | 1.4.1106 |
| Main Library | ggplot2 | 3.3.3 |
|  | wordcloud | 2.6 |
|  | recommenderlab | 0.2-7 |
| Web App | Shiny | 1.6.0 |

## 핵심기술

협업필터링 중에서도 이웃과 모델 기반을 고려하여 진행. 이웃 기반 협업필터링은 UBCF, IBCF를 사용하였고 모델 기반 협업필터링으로 SVD를 사하였다.

| 구분 | 종류 |
| :--- | :--- |
| 협업필터: 이웃기반 | UBCF, IBCF |
| 협업필터링: 모델기 | SVD |
| 하이브리 | Cascade |

모델 성능 평가 지표로 Confusion matrix 통해 Precision, Recall, F1-score, AUROC를 고려하였다.

| 평가지표 |  |  |  |
| :--- | :--- | :--- | :--- |
| Precision | Recall | F1-score | AUROC |

## 분석결과

1.사용자-영화평점 데이터

2.사용자-장르평점 데이터

| F1 성능지표 | 평 | 장 |
| :--- | :--- | :--- |
| SVD\_C |  |  |
|  |  |  |
|  |  |  |

## 담당 파트 

### Back-End

1. 추천 알고리즘 구현
2. 평가 지표 결과 정확도가 높은 추천 알고리즘 선정 

## 협업 방식 


