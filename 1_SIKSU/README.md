# 구내식당 식수 인원 예측 AI 경진대회

<img width="700" alt="image" src="https://github.com/user-attachments/assets/9c76617d-9cd6-49ef-8af8-52d544fe7e2e">

## 대회 링크
- [대회 페이지 바로가기](https://dacon.io/competitions/official/)
- **대회 기간**: 2022년 6월 29일 - 7월 12일
- **최종 순위**: 3위 (참가자 수 46명)
<img width="696" alt="image" src="https://github.com/user-attachments/assets/b1b9d15d-53eb-4f09-9a4f-c6860a9d637b">


## 1. 대회 배경
구내식당에서는 지금까지 **단순한 시계열 추세와 담당자의 직관적 경험**을 통해 식수 인원을 예측해 왔습니다. 그러나 잔반을 줄이기 위해 보다 **정확한 데이터 기반의 예측이 필요**하게 되었습니다. 이에 따라 AI 및 빅데이터 분석 기술을 활용하여 정확도를 높이고자 합니다.

## 2. 대회 목적
구내식당의 요일별 점심, 저녁식사 인원을 예측하는 AI 모델을 개발합니다. 이 예측을 통해 구내식당 운영 효율을 높이고 잔반을 줄이는데 기여하고자 합니다.

## 3. 데이터 개요
- **Train 데이터**: 지난 1년간의 요일별 식수 인원 데이터
- **Test 데이터**: 향후 50일간의 요일별 식수 인원 예측

## 4. 평가 방식
모델의 예측 성능은 **평균 절대 오차 (MAE, Mean Absolute Error)** 를 기준으로 평가합니다.
- **Public 평가**: 전체 Test 데이터 중 30% (15일) 무작위 평가
- **Private 평가**: 전체 Test 데이터 중 70% (35일) 평가

## 5. 나의 솔루션

### 코드 링크
- [최종 코드 파일 - Final_Solution.ipynb](./code/Final_Submission.ipynb)

### 데이터 전처리
1. **이상치 제거**: 식사 인원이 0인 날을 삭제하여 분석의 정확도를 높임.
2. **변수 생성**:
   - 날짜 정보를 이용하여 **'요일', '월', '일'** 등의 파생 변수를 추가.
   - **'현재원'** 변수를 생성하여 본사 정원에서 휴가자, 출장자, 재택근무자를 뺀 실제 근무 인원을 계산.
3. **카테고리 변수 변환**: 요일 변수는 숫자로 변환하여 모델에 입력할 수 있도록 처리.

### 모델링 및 예측
1. **모델 선택**:
   - **Decision Tree Regressor**: 초기에 중식과 석식 인원 예측을 위해 각각 Decision Tree 모델을 학습.
   - **Random Forest Regressor**: 모델의 성능을 개선하기 위해 Random Forest를 사용하여 중식과 석식 인원을 예측.
   - **XGBoost Regressor**: 최종적으로 XGBoost를 사용하여 모델 성능을 더욱 향상. GridSearchCV를 통해 하이퍼파라미터 튜닝 진행.

2. **모델링 프로세스**:
   - **하이퍼파라미터 튜닝**: XGBoost 모델에 대해 `max_depth`, `n_estimators`, `min_child_weight` 등의 하이퍼파라미터를 GridSearchCV로 최적화.
   - **특성 선택**: Random Forest와 XGBoost 모두 중식 예측 모델의 결과를 석식 예측 모델의 입력 피처로 활용하여 모델의 예측력을 강화.

### 성능 평가 및 결과
- **평균 절대 오차 (MAE)**: 최종 모델에서 중식과 석식 인원 예측의 MAE를 줄이는 데 성공하여, 효율적인 식수 인원 예측을 달성.
- **결과 분석**: 주중, 공휴일, 계절 요인 등이 반영된 모델로, 공휴일과 평일 패턴을 잘 반영하는 성능을 보임.


---

### Keywords

- **데이터 전처리**: 이상치 제거, 날짜 기반 특성 생성, 카테고리 변수 변환
- **모델링 기법**: Decision Tree Regressor, Random Forest Regressor, XGBoost Regressor
- **하이퍼파라미터 튜닝**: GridSearchCV, `max_depth`, `n_estimators`, `min_child_weight`
- **평가 방식**: Mean Absolute Error (MAE)
