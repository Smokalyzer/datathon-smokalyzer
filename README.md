```
2025.06.04 v1_README.md 작성 중
```

# 📋 Smokalyzer: Bio-Signal Based Smoker Classification

본 프로젝트는 생체 신호(Bio-signals)를 기반으로 개인의 흡연 여부(Smoker or Non-smoker)를 예측하는 이진 분류 문제를 해결합니다.

주어진 데이터는 딥러닝을 통해 합성된 100개 이상의 생체 지표로 구성되어 있으며, 일부 변수에는 노이즈 또는 비선형 상관관계가 포함되어 있는 것이 특징입니다. 이에 따라 정보 밀도가 낮은 고차원 데이터에서 신호를 효과적으로 추출하고, 일반화 가능한 모델을 구성하는 것을 목표로 합니다.

>프로젝트 기간: 2025.05.30 - 2025.06.04

> 🛠️ **Tech Stack**  
>Language: &nbsp;Python  
Data Analysis & EDA: &nbsp;pandas, numpy, Jupyter Notebook  
Visualization: &nbsp;matplotlib, seaborn  
Machine Learning:<br>- Modeling: &nbsp;`scikit-learn`, `LightGBM`, `XGBoost`, `CatBoost`<br>- Ensemble: &nbsp;Voting, Stacking<br>- Optimization: &nbsp;Optuna

<br>
<br>

## 프로젝트 팀원 소개

<br>
<br>

## README 목차

<br>
<br>

## Dataset Overview

- **출처**: Binary Prediction of Smoker Status using Bio-Signals (AI Factory)
- **형식**: 딥러닝 기반 합성 데이터 (synthetic data)
- **크기**: 약 16만 건 (train/test 포함)
- **타겟 변수**: `smoking` (흡연 여부, 0: 비흡연자, 1: 흡연자)
- **주요 특징**
    - `age`, `height`: 5 단위로 구간화된 변수
    - `height(cm)`, `weight(kg)`, `waist(cm)`: 기본 신체 지표
    - `systolic`, `relaxation`, `fasting blood sugar`, `Gtp` 등: 주요 건강 지표
    - `gender`: 성별 파생 컬럼 (0 = 여성, 1 = 남성) 으로 남성일 확률값
    - `smoking`: 타겟 변수 (0 = 비흡연자, 1 = 흡연자)


<br>
<br>

## 폴더 구조

```
📁 bio-signal-smoker-classification/
│

├── 📁 notebooks/                           # 실험 노트북 정리 폴더
│   ├── final_submission.ipynb                            # 최종 제출 코드 (.ipynb)
│   ├── v_1.0_smokalyzer_baseline.ipynb                   # 초기 베이스라인
│   ├── v_2.0_smokalyzer_feature_engineering.ipynb        # 피처 엔지니어링 실험
│   ├── v_2.1_smokalyzer_submission_refactor.ipynb        # submission 생성 방식 수정
│   └── v_3_0_smokalyzer_improve_model_performance.ipynb  # 모델 성능 개선
│
├── 📁 eda/                          # EDA, 시각화 전용 노트북
│   ├── eda_visualization.ipynb      # 전체 변수 분석 및 시각화
│   └── correlation_heatmap.ipynb    # 변수 간 상관관계 시각화
│
├── 📁 feature_docs/                 # 피처 설명 및 생성 방식 문서
│   ├── 📄 original_features.md      # 원본 피처 설명
│   └── 📄 engineered_features.md    # 파생 피처 설명 + 수식/코드 예시
│
├── 📁 presentation/              
│   └── project_presentation.pptx    # 발표용 PPT 또는 PDF
│
├── 📁 outputs/                      
│   └── 📄 submission.csv             # 제출 파일
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
    - 도메인 기반 파생 피처 생성 (e.g., BMI, 체중/신장 비율 등)
    - 다중공선성 제거 및 변수 선택 기법 적용
3. 모델링 및 튜닝
    - 주요 모델: XGBoost, LightGBM, CatBoost
    - Soft Voting 앙상블 구성
    - 수동 설정 vs 최적화 튜닝 파라미터 성능 비교
4. 최종 모델 선택
    - 세 모델을 기반으로 한 Soft Voting (수동 설정 하이퍼파라미터 사용)
    - AUC: **0.87382**

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

## 인사이트

데이터 이해부터 성능 향상까지 전 과정을 직접 수행하여 문제 해결 프로세스를 단계별로 경험하고 적용할 수 있었음.

데이터 분석을 통해 의미 있는 파생 변수를 생성하여 피처별 성능 기여도 분석의 중요성을 직접 체감함. 예측력 향상에는 도메인 기반 해석이 핵심적임을 확인할 수 있었음.

수치만으로는 놓치기 쉬운 이상치와 변수 간 분포 차이를 히트맵, 바이올린플롯, 페어플롯 등의 시각화 기법을 통해 직관적으로 파악하여 데이터를 더욱 정밀하게 분석할 수 있었음.

데이터 전처리 과정에서 테스트 파일의 ID와 행 순서가 변형될 수 있음을 경험하였고, 모델 성능에 예상치 못한 영향을 줄 수 있어 테스트와 제출 파일 검토가 반드시 필요함을 깨달음.

LightGBM, XGBoost 등 다양한 모델을 실험하고 성능을 비교하여 모델 특성과 결과 해석을 바탕으로 앙상블 전략의 당위성과 효과를 확인하여 적용할 수 있었음.

모델의 성능은 알고리즘보다는 데이터의 품질과 정보량에 크게 좌우되며 고차원 데이터라 하더라도 라벨을 설명할 수 있는 핵심 정보가 없다면 모델링은 의미를 갖기 어려움. 특히 합성 데이터의 경우, 피처에 내재된 의미가 부족하거나 도메인 기반 해석이 어려워 모델 성능 향상에 한계가 있을 수 있음을 확인함.

<br>
<br>

## 향후 계획

1. 합성 데이터의 한계를 객관적으로 평가하기 위해 원본 데이터와의 비교 분석 필요
2. `gender` 피처 생성 시 확률값의 작용 고려
3. 모델링 이전에 데이터의 품질을 개선하고 정보량을 확보하는 데 초점
4. 흡연/비흡연자 간 분포 차이 시각화로 구조적 구분 가능성 탐색
5. 변수 재구성(압축 또는 조합)을 통한 잠재 신호 추출

<br>

>본 프로젝트를 통해 
