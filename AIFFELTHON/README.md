# Joymen

## 차량 내부 오염도 판별

### 1. 프로젝트 배경
- 다음 고객이 사용하기 전 차량 내부 청결 상태 확인을 통한 고객 경험 개선

### 2.프로젝트 수행 과정 및 결과
- 결과 제시 1.  탐색적 분석 및 전처리
- 데이터 확인
  - 오분류된 학습 데이터 확인 정제
  - 컵홀더 카테고리에 계기판 사진, 차량 부스 사진 등이 들어있음
- 데이터 증강
   - Random horizontal flip
   - Random vertical flip
   - Color jitter
   
- 결과 제시 2.  모델 개요
    - CNN 딥러닝 모델: ResNet<br>
      <img width="80" src="https://github.com/yhs2773/AIFFELTHON/issues/2"/>

- 오염의 정의:  외부 물질이 차량 내부 안에 존재하는것을 오염되었다로 정의

- 해결방법 1 OpenMax : 차량 내부 이미지파일이 주어졌을때,<br>
  오염되지 않은 내부 이미지들의 카테고리 분류 + 오염된 내부 이미지를 reject로 분류
    - 결과
    - 평가지표: Test Accuracy, Reject Accuracy<br>
      <img width="80" src="https://github.com/yhs2773/AIFFELTHON/issues/3"/>

- 해결방법 2  : 오염되지 않은 이미지와 오염된 이미지를 분류시키는 학습 구조로 변경

    - Case 1, Case 2, Case 3
         - Binary classification (부분) <br>
            Case 1 : 컵홀더 데이터 <br>
            train & test 데이터 : 컵홀더(2500장), 오염된 컵홀더(1092장)<br>
            Case 2 : 앞 좌석 vs 오염된 매트<br>
            train & test 데이터 : 앞 좌석(2500장), 오염된 매트(2500장)<br>
            Case 3 : 뒷 좌석 vs 오염된 매트<br>
            train & test 데이터 : 뒷 좌석(2500장, 오염된 매트(2500장)<br>

    - Case 4
        - Multi-class classification <br>
          train & test 데이터 : 컵홀더(2500장), 오염된 컵홀더(1092장), 앞 좌석(2500장), 뒷 좌석(2500장), 오염된 매트(2500장)

    - Case 5
        - Binary classification <br>
          train & test 데이터 : 전체 오염되지 않은 데이터를 하나의 폴더(7500장), 전체 오염된 데이터를 하나의 폴더(3592장)
    - 결과
    - 평가지표:  Accuracy, F1-Score, Recall<br>
      <img width="80" src="https://github.com/yhs2773/AIFFELTHON/issues/4"/>