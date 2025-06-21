# 📋 Smokalyzer: Bio-Signal Based Smoker Classification

본 프로젝트는 생체 신호(Bio-signals)를 기반으로 개인의 흡연 여부(Smoker or Non-smoker)를 예측하는 이진 분류 문제를 해결합니다.

주어진 데이터는 딥러닝을 통해 합성된 생체 지표로 구성되어 있으며, 일부 변수에는 노이즈 또는 비선형 상관관계가 포함되어 있는 것이 특징입니다. 이에 따라 정보 밀도가 낮은 고차원 데이터에서 신호를 효과적으로 추출하고, 일반화 가능한 모델을 구성하는 것을 목표로 합니다.

>프로젝트 기간: 2025.05.30 - 2025.06.04

###### Data Analysis & BI

<p>
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white"/>
  <img src="https://img.shields.io/badge/Numpy-013243?style=flat&logo=numpy&logoColor=white"/>
  <img src="https://img.shields.io/badge/Matplotlib-11557C?style=flat&logo=plotly&logoColor=white"/>
  <img src="https://img.shields.io/badge/Seaborn-4C72B0?style=flat&logo=python&logoColor=white"/>
</p>

###### Data Modeling & ML

<p>
  <img src="https://img.shields.io/badge/scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white"/>
  <img src="https://img.shields.io/badge/XGBoost-FF7033?style=flat&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/LightGBM-9ACD32?style=flat&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/CatBoost-f1ca1c?style=flat&logo=python&logoColor=white"/>
</p>

###### Collaboration & Tools

<p>
  <img src="https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white"/>
  <img src="https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white"/>
  <img src="https://img.shields.io/badge/Notion-000000?style=flat&logo=notion&logoColor=white"/>
  <img src="https://img.shields.io/badge/GoogleColab-F9AB00?style=flat&logo=Google-Colab&logoColor=white"/>
</p>

<br>
<br>

## 팀원 소개

<div align="center">

| 고주리 | 김귀연 | 문선영 | 채승희 |
| :--------------------------------------------------------------: | :--------------------------------------------------------------: | :--------------------------------------------------------------------------: | :-----------------------------------------------------------: |
|           [@jul-ee](https://github.com/팀원1아이디)           |           [@GwiYeonKim](https://github.com/GwiYeonKim)           |                 [@sunyoungmoon012](https://github.com/sunyoungmoon012)                 |         [@shchae12](https://github.com/shchae12)          |
|                            - Modeling<br>- Model Tuning                      |                           - Feature Engineering<br>- Modeling Tuning                            |                                  - EDA<br>- Data Preprocessing                                  |                          - EDA<br>- Feature Engineering |

</div>

<br>
<br>


## Dataset Overview

- **출처**: [Kaggle - Binary Prediction of Smoker Status using Bio-Signals](https://www.kaggle.com/competitions/playground-series-s3e24)
- **형식**: 딥러닝 기반 합성 데이터 (synthetic data)
- **크기**: 약 16만 건 (train/test 포함)
- **타겟 변수**: `smoking` (흡연 여부, 0: 비흡연자, 1: 흡연자)
- **주요 특징**
    - `age`, `height(cm)`, `weight(kg)`: 5 단위로 구간화된 변수
    - `eyesight(left)`, `hearing(left)` 등: 기본 신체 지표
    - `systolic`, `relaxation`, `fasting blood sugar`, `Gtp` 등: 주요 건강 지표
    - `gender`: 성별 파생 컬럼 (0 = 여성, 1 = 남성) 으로 남성일 확률값
    - `smoking`: 타겟 변수 (0 = 비흡연자, 1 = 흡연자)


<br>
<br>

## 폴더 구조

```
📁 datathon-smokalyzer/
│
├── 📁 notebooks/                           # 실험 노트북 정리 폴더
│   ├── final_submission.ipynb                            # 최종 제출 코드 (.ipynb)
│   ├── v_1.0_smokalyzer_baseline.ipynb                   # 초기 베이스라인
│   ├── v_2.0_smokalyzer_feature_engineering.ipynb        # 피처 엔지니어링 실험
│   ├── v_2.1_smokalyzer_submission_refactor.ipynb        # submission 생성 방식 수정
│   └── v_3_0_smokalyzer_improve_model_performance.ipynb  # 모델 성능 개선
│
├── 📁 eda/                          # EDA, 시각화 전용 노트북
│   └── exploratory_data_analysis.ipynb
│
├── 📁 feature_docs/                 # 피처 설명 및 생성 방식 문서
│   ├── 📄 original_features.md      # 원본 피처 설명
│   └── 📄 engineered_features.md    # 파생 피처 설명
│
├── 📁 presentation/              
│   └── project_presentation.pptx    # 발표용 PPT
│
└── 📄 README.md
```

<br>
<br>

## 분석 프로세스

**평가 지표**: ROC-AUC

1. EDA 및 전처리
    - 비정상치 확인 및 단위 통일
    - 이상 분포 시각화 및 수치적 파악
2. Feature Engineering
    - 확률 기반 `gender` 피처 생성을 위한 예측 모델 생성
    - 도메인 기반 파생 피처 생성 (e.g., BMI, 체중/신장 비율 등)
    - 다중공선성 제거 및 변수 선택 기법 적용

4. 모델링 및 튜닝
    - 베이스라인 모델 성능 비교 실험
    - <img width="768" alt="스크린샷 2025-06-21 오후 2 49 21" src="https://github.com/user-attachments/assets/12930bc9-8089-459d-900d-f7442fc39704" />
    - 수동 설정 vs 최적화 튜닝 파라미터 성능 비교 실험
    - 앙상블 모델 비교 실험
      - Soft Voting, Bagging, Stacking, Weighted Stacking(Optuna)
6. 최종 모델 선정
   "Soft Voting 기반 앙상블"
    - XGBOOST + LIGHTGBM + CATBOOST  
      → &nbsp;스케일링 및 로그 변환 수행 X  
      → &nbsp;THRESHOLD 조정 없이 예측 확률값 그대로 사용  
      → &nbsp;최종 AUC: 0.87382
    - 모델 선정 근거  
       1. 다양한 모델을 활용한 보완적 예측 구조에 기반한 안정적 성능 확보
       2. 단순하면서도 모델 간 학습 시간과 구조적 복잡도를 최소화할 수 있는 설계
       3. 실제 테스트셋에서 최고 수준의 AUC(0.8688)을 기록하며 다른 고비용 구조(STACKING, OPTUNA) 대비 성능 차이가 거의 없음을 확인

<br>
<br>

## 주요 실험 결과 및 인사이트

| 실험 방식 | AUC 성능 | 특징 |
| --- | --- | --- |
| `gender` 단일 피처 | 가장 높음 | label과 통계적으로 직접 연관 |
| 전체 피처 사용 | 중간 | 잡음과 유효정보 혼재 |
| 생성 + 선택된 피처 사용 | 가장 낮음 | 유효 신호 부족, 과적합 가능성 |
| 하이퍼파라미터 튜닝 모델 | 낮음 (0.86507) | 자동 튜닝보다 보수적 수동 설정이 우수 |
- 유효 정보량이 극도로 적은 데이터에서는 복잡한 튜닝보다 보수적이고 간단한 설정이 오히려 효과적임
- Soft Voting에 사용된 트리 계열 모델들은 스케일링, 로그 변환 없이도 적절히 작동하며 해당 변환이 성능 향상에 기여하지 않았음

<br>
<br>

## 데이터 한계 및 해석

- 합성 데이터 특성으로 인해 변수 간의 상관관계가 실제 생리학 구조와 어긋날 수 있음
- 다수의 지표들이 흡연 여부와의 인과적 연관성이 희박
- `gender` 외에는 label 예측에 실질적 기여도가 낮은 변수가 대부분
- 모델링보다 데이터 설계 및 신호 존재 여부가 성능 결정 요인

<br>
<br>

## 프로젝트 인사이트

<img width="763" alt="스크린샷 2025-06-21 오후 2 50 33" src="https://github.com/user-attachments/assets/76c4ff89-3495-433f-b61f-b7ca3b2f054a" />

<br>
<br>


## 향후 계획

1. 합성 데이터의 한계를 객관적으로 평가하기 위해 원본 데이터와의 비교 분석 필요
2. `gender` 피처 생성 시 확률값의 작용 고려
3. 모델링 이전에 데이터의 품질을 개선하고 정보량을 확보하는 데 초점
4. 흡연/비흡연자 간 분포 차이 시각화로 구조적 구분 가능성 탐색
5. 변수 재구성(압축 또는 조합)을 통한 잠재 신호 추출

<br>

>해당 repository는 모두의 연구소 데이터사이언티스트 4기 과정의 데이터톤 수행 과정을 기록합니다.
