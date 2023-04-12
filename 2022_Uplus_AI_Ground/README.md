# 2022 유플러스 AI Ground
### 대회 개요
---
기간: 2022.11.07 ~ 2022.12.02
<br>
<br>
추천시스템 경진대회 by 유플러스 & 업스테이지
##### 평가지표
---
- Recall@K: 추천 시스템에서 K에 대한 Recall은 사용자에게 추천된 아이템 중 해당 사용자에게 관련성이 있는 아이템의 비율을 나타내며 전체 점수 중 75%의 비중을 차지
- NDCG@K: 추천 시스템에서 K에 대한 NDCG는 추천된 아이템들의 관련성과 랭킹을 고려하여 랭킹 알고리즘의 효과를 측정하는 지표로 전체 점수 중 25%의 비중을 차지
### 데이터 전처리
---
- 7개의 파일들의 연결성을 ERD를 통해 확인
- 병합 가능한 파일들은 병합 진행

아래와 같은 전처리 과정을 거침:
<br>
1. 결측치가 과반수 이상 데이터 제거
2. 관련성이 적은 데이터 제거
3. 필요한 피처들의 결측치 보완
4. 너무 세분화된 데이터 제거
5. 에러인 시간 데이터 수정
6. 범주형 데이터는 정수형 데이터 변환
7. 중복 데이터 제거
### Negative Sampling
---
- 시청한 콘텐츠 (pos)는 1, 시청하지 않은 콘텐츠 (neg)는 0으로 반환
##### Random Negative Sampling (RNS)
- 시청하지 않은 콘텐츠들을 같은 확률로 두고 랜덤으로 선택하는 샘플링 기법
1. pos:neg 비율을 1:100을 설정 후 진행 (결과 = 0.2143)
2. 사용된 모델의 논문인 NCF를 참고하여 negative sampling 비율을 3~7로 맞춰서 진행 (결과 =  0.2003, 0.1950)
##### Popularity-biased Negative Sampling (PNS)
- 시청하지 않은 콘텐츠 중 시청률에 따라 다른 가중치를 주고 선택하는 샘플링 기법
1. 결과는 0.1901과 0.1898로 RNS보다 더 낮은 성능을 보임
2. 샘플링의 개인화가 부족
### 모델
---
##### Neural Collaborative Filtering (NCF)
- Matrix factorization과 deep neural network를 결합시킨 NeuMF로 진행
1. 다양한 피처들을 (성별, 시청 시간, 컨텐츠 주인공 등) 집어넣어 진행 (결과 = 0.2167 까지 향상)
2. DNN의 layer 변경 및 최종 hidden layer 차원 조절 후 0.2180까지 향상
3. Optimal hyperparameter를 찾기 위해 ray tune과 grid search를 진행 후 결과를 0.2230까지 향상
##### LightGCN
- Negative sampling과 모델을 바꿔보는 것도 좋을거 같아서 찾아봄
- [Bayesian Negative Sampling for Recommendation](https://arxiv.org/pdf/2204.06520.pdf)이라는 22년 7월에 나온 논문을 찾아 시도해보기로 결정
    - LightGCN과 bayesian negative sampling기법을 결합하는 내용의 논문
- 학습시간이 너무 오래 걸렸고 결과가 PNS + NeuMF보다 안좋아 폐기
### 결과
---
- 216개의 팀 중 26위
- 약 3주 반동안 진행된 추천시스템 대회에 3명이서 참여하여 다양한 시각으로 문제 및 데이터를 바라보고 토론을 했던게 생각의 틀을 넓히는데 도움이 됐다
- 추천시스템 기본 이론, 데이터 전처리, 모델 튜닝 등 배웠던 것을 대회에 적용시킬 수 있었다는게 도움이 됐다
### 개선점
---
##### 데이터 전처리
- 데이터를 병합하는 과정에서 샘플 수를 보존하려고 노력한 시간 대비 성과가 아쉬웠다
- 차원 축소, feature selection/extraction 등의 방법을 사용해 데이터의 차원을 줄이는 것이 논리적일 듯 하다
- PNS를 단순 클릭률 대신 시간/나이/성별 등 함께 고려하여 진행해보면 좋았을 듯 하다
- Dynamic negative sampling, simplify and robustify negative sampling 등 다양한 샘플링 알고리즘의 결과도 궁금하다
##### 모델
- 리소스 제한으로 모델을 더 복잡하게 만들지 못한 것이 아쉽다
- NeuMF는 MF와 DNN을 합친 모델인데 DNN 대신 다른 모델을 사용하는 것이 더 좋은 방법이었을 듯 하다
- LightGCN + BNS를 해봤지만 BNS 대신 다른 negative sampling 기법을 결합했다면 결과가 더 좋았을지 궁금하다