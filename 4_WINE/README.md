# 와인 품질 분류 AI 경진대회

<img width="695" alt="image" src="https://github.com/user-attachments/assets/7e340d44-d688-4169-864a-a5f08dc112d0">

## 대회 링크
- [대회 페이지 바로가기](https://dacon.io/competitions/open/235941/overview/description)
- **대회 기간**: 2022년 8월 1일 - 8월 5일
- **최종 순위**: 2위 (참가자 수 26명)
<img width="693" alt="image" src="https://github.com/user-attachments/assets/5a62600d-2ca1-4a12-9d28-c877184466f3">

## 1. 대회 배경
와인의 품질은 여러 화학적 성분에 따라 결정되며, 소비자 만족도에도 큰 영향을 미칩니다. 하지만 모든 와인을 전문가가 직접 평가하는 것은 시간과 비용이 많이 들기 때문에, 이를 효율적으로 해결할 수 있는 AI 모델의 필요성이 대두되었습니다. 따라서 **와인의 화학적 성분 데이터를 기반으로 와인 품질을 분류하는 AI 경진대회**가 개최되었습니다.

## 2. 대회 목적
와인의 다양한 화학적 특성을 활용하여 와인의 품질을 정확히 예측하는 **분류 모델을 개발**합니다. 이 대회를 통해 화학적 데이터와 머신러닝 기술을 활용한 품질 평가 모델을 구축하고, 이를 통해 효율적인 품질 관리를 실현하고자 합니다.

## 3. 데이터 개요
- **Train 데이터**: 각 와인의 화학 성분 수치와 해당 와인의 품질 라벨 데이터.
- **Test 데이터**: 품질 라벨이 없는 와인의 화학 성분 데이터로, 이를 통해 품질을 예측.

## 4. 평가 방식
모델 성능은 **정확도(Accuracy)** 를 기준으로 평가합니다.
- **Public 평가**: 테스트 데이터의 50%로 평가하여 실시간 순위 산출.
- **Private 평가**: 나머지 50%의 테스트 데이터로 최종 순위를 결정.

## 5. 나의 솔루션

### 코드 링크
- [최종 코드 파일 - Final_Solution.ipynb](./code/Final_Submission.ipynb)

### 데이터 전처리
1. **데이터 증강**: `quality` 값이 극단적인 데이터(`4`, `7`, `8`)의 비율을 높이기 위해 해당 품질 데이터만 복제하여 **클래스 비율을 균형** 있게 맞추었습니다.
2. **데이터 타입 인코딩**: 와인 종류(`type`)를 **원-핫 인코딩**하여 `red`와 `white` 컬럼으로 변환하였습니다.
3. **데이터 스케일링**: `fixed acidity`, `volatile acidity`, `citric acid` 등 수치형 데이터를 **표준화**하여 모델의 수렴 속도를 높이고, 과적합을 줄였습니다.

### 모델링 및 예측
1. **모델 선택 및 설정**:
   - **RandomForestClassifier**: 다양한 결정 트리를 앙상블하여 예측 성능을 높임.
   - **LGBMClassifier**: 경량화된 부스팅 알고리즘으로, 다중 클래스 분류에 적합하여 와인 품질 예측에 사용.
   - **ExtraTreesClassifier**: 많은 결정 트리를 구성해 예측 성능을 높임.
   
2. **하이퍼파라미터 튜닝**:
   - 각 모델별 하이퍼파라미터를 사전 설정하였으며, `StratifiedKFold` 5-폴드 교차 검증과 `GridSearchCV`를 통해 최적 파라미터를 찾았습니다.
   - 설정한 주요 하이퍼파라미터:
      - **RandomForestClassifier**: `n_estimators=2700`, `min_samples_leaf=1`
      - **LGBMClassifier**: `n_estimators=2800`, `max_depth=10`, `num_leaves=31`, `subsample=0.8`
      - **ExtraTreesClassifier**: `n_estimators=3000`, `criterion='entropy'`, `bootstrap=True`
   
3. **결과 앙상블**:
   - 세 모델의 예측 결과를 각각 `pred0`, `pred1`, `pred2`로 저장하고, 최종적으로 **각 모델의 예측 결과의 최빈값**을 도출하여 최종 예측값을 생성하는 **앙상블 방식**을 사용하였습니다.

### 성능 평가 및 결과
- **정확도 (Accuracy)**: 최종 모델의 성능은 테스트 세트에 대한 정확도로 평가되었으며, 복수 모델의 예측을 앙상블한 결과 **모델의 정확도가 향상**됨을 확인했습니다.
- **결과 분석**: 복수 모델을 앙상블하여 `RandomForestClassifier`, `LGBMClassifier`, `ExtraTreesClassifier`의 장점을 조합한 결과 **와인 품질을 안정적이고 정확하게 분류**할 수 있었습니다.

---

### Keywords

- **데이터 전처리**: 데이터 증강, 원-핫 인코딩, 표준화 스케일링
- **모델링 기법**: RandomForestClassifier, LGBMClassifier, ExtraTreesClassifier
- **하이퍼파라미터 튜닝**: GridSearchCV, `StratifiedKFold`, `n_estimators`, `min_samples_leaf`, `max_depth`, `num_leaves`, `subsample`
- **앙상블 방식**: 최빈값(모드) 앙상블
- **평가 방식**: Accuracy (정확도)
