# 버스 운행시간 예측 AI 경진대회

<img width="698" alt="image" src="https://github.com/user-attachments/assets/8df59d77-ebea-490c-a112-4714e9a55057">

## 대회 링크
- [대회 페이지 바로가기](https://dacon.io/competitions/open/235940/overview/description)
- **대회 기간**: 2022년 7월 25일 - 7월 29일
- **최종 순위**: 3위 (참가자 수 29명)
<img width="699" alt="image" src="https://github.com/user-attachments/assets/5405d701-10e0-45a0-b644-6780f19c0841">

## 1. 대회 배경
제주도는 최근 주민 증가와 관광객 유입으로 인해 교통체증이 심화되었습니다. 제주도 일부 지역의 교통체증은 서울보다도 심각한 상황입니다. 이러한 문제를 해결하고자 **제주테크노파크**는 제주도 내 버스 운행시간을 예측하는 AI 경진대회를 개최했습니다. 이를 통해 교통 혼잡을 줄이고, 버스 운행의 효율성을 높이고자 합니다.

## 2. 대회 목적
제주도 내의 교통 상황과 버스 운행 데이터를 바탕으로 **버스 운행 시간을 예측하는 AI 모델**을 개발합니다. 이 모델을 통해 교통 체증에 대비하고, 제주도 버스 시스템의 효율적인 운영에 기여하고자 합니다.

## 3. 데이터 개요
- **Train 데이터**: 제주도 내 버스 운행 데이터로, 과거의 운행 시간, 구간별 교통상황 정보 등이 포함됨.
- **Test 데이터**: 특정 구간의 버스 운행시간을 예측하기 위한 미래의 데이터.

## 4. 평가 방식
모델의 예측 성능은 **평균 제곱근 오차 (RMSE, Root Mean Squared Error)** 를 기준으로 평가합니다.
- **Public 평가**: 전체 Test 데이터 중 30% 무작위 평가.
- **Private 평가**: 전체 Test 데이터 중 70%로 최종 성적 결정. 

## 5. 나의 솔루션

### 코드 링크
- [최종 코드 파일 - Final_Solution.ipynb](./code/Final_Submission.ipynb)

### 데이터 전처리
1. **고유 ID 및 위치 데이터 인코딩**: `vh_id`, `now_longitude`, `next_longitude` 등 문자형 위치 데이터를 모델이 처리할 수 있도록 **고유 번호로 인코딩**하였습니다.
2. **도착 시간 전처리**: `now_arrive_time` 데이터를 시간대로 구분하여 `time` 컬럼을 생성하였습니다. 이를 통해 **아침, 낮, 저녁 등 시간대별로 버스 운행 시간을 그룹화**하여 모델의 예측 정확도를 높였습니다.
3. **훈련 데이터 필터링**: `next_arrive_time`이 1000 이하인 데이터를 사용해 **비정상적인 이상치를 제거**하였습니다.

### 모델링 및 예측
1. **모델 선택**:
   - **CatBoost Regressor**: 버스 운행 시간 예측을 위해 `CatBoostRegressor`를 사용하였습니다. CatBoost는 범주형 데이터를 쉽게 처리할 수 있으며, GPU 학습을 통해 빠른 모델 학습이 가능합니다.
   
2. **K-Fold 교차 검증**:
   - **5-폴드 교차 검증**을 사용하여 모델의 성능을 검증하였으며, 각 폴드에서 모델 학습 후 예측 결과를 평균하여 최종 예측 성능을 강화했습니다.

3. **하이퍼파라미터 설정**:
   - `iterations=3000`, `depth=6`, `bootstrap_type='Bernoulli'`, `subsample=0.6` 등의 하이퍼파라미터를 설정하여 모델을 최적화하고, `RMSE`를 평가지표로 사용하여 모델 성능을 평가하였습니다.

### 성능 평가 및 결과
- **평균 제곱근 오차 (RMSE)**: 5-폴드 교차 검증을 통해 최종 RMSE 성능을 확인하였으며, 교차 검증을 통해 모델의 과적합을 방지하고 일반화 성능을 높였습니다.
- **결과 분석**: 시간대별로 그룹화한 `time` 변수와 위치 정보 변수(`now_longitude`, `next_longitude`)가 예측에 중요한 역할을 했음을 확인했습니다.

---

### Keywords

- **데이터 전처리**: 범주형 데이터 인코딩, 시간대 그룹화, 이상치 필터링
- **모델링 기법**: CatBoost Regressor
- **교차 검증**: K-Fold (5-fold)
- **하이퍼파라미터**: `iterations=3000`, `depth=6`, `bootstrap_type='Bernoulli'`, `subsample=0.6`
- **평가 방식**: Root Mean Squared Error (RMSE)
