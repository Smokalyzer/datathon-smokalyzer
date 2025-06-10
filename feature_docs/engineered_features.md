## Binary Classification of Smoker Status using Bio-Signals

### 파생 피처 (engineered features)

#### 외부 데이터셋 기반 성별(gender) 피처 생성

원본 데이터에는 `gender` 정보가 존재하지 않았지만, 성별은 흡연과 매우 강한 통계적 연관성을 갖는 변수입니다.

우리는 외부 생체 신호 데이터셋을 활용해 성별 예측 모델을 만들고, 남성일 확률(0~1)이라는 연속형 피처를 생성하였습니다.

- 성별 예측 모델: XGBoost 기반 분류기
- 학습용 데이터 출처: Kaggle 공개 생체 신호 데이터
- 예측값을 train/test 모두에 적용하여 `gender` 피처 생성

<br>

#### 기본 계산 기반 파생 피처

| 피처명 | 계산 방식 | 도메인 해석 |
| --- | --- | --- |
| `BMI` | `weight / (height/100)^2` | 체질량지수 → 비만도 |
| `WHtR` | `waist / height` | 허리둘레 대비 키 비율 → 복부비만 지표 |
| `Pulse Pressure` | `systolic - relaxation` | 혈관 탄성 / 노화와 관련 |
| `MAP` | `relaxation + (systolic - relaxation)/3` | 평균 동맥압 → 순환계 부하 |
| `LDL_ratio` | `LDL / cholesterol` | 나쁜 콜레스테롤 비율 |

<br>

#### 위험군 분류 기반 범주형 피처

| 피처명 | 생성 방식 | 의미 |
| --- | --- | --- |
| `BP_level`, `BP_category` | 수축기/이완기 기준 고혈압 단계화 | 혈압 이상 여부 |
| `HDL_risk`, `LDL_risk`, `total_cholesterol_risk` | HDL/LDL/Cholesterol 기준 구간화 | 심혈관 리스크 분류 |
| `age_risk` | `age >= 45 → 1` | 중년 이상 여부 기준 위험군 구분 |

> 범주형 파생 피처는 모델이 수치의 절대값이 아닌 ‘위험 구간’의 의미를 학습할 수 있도록 도와줍니다.

<br>

#### 상호작용 기반 피처 (Interaction Features)

흡연은 단일 지표보다는 복합적인 신체 반응의 결과입니다.
- 예를 들어, 간 수치(Gtp)가 높고 혈색소(hemoglobin)도 높다면 이는 단순한 간 문제를 넘어 흡연으로 인한 생리학적 보상 반응일 수 있습니다.

| 피처명 | 계산 방식 | 의미 |
| --- | --- | --- |
| `hemoglobin_sq` | `hemoglobin^2` | 산소운반 능력의 비선형 변화 반영 |
| `hemoglobin × height/weight/Gtp/creatinine` | 혈액 상태 × 체격/간/신장 기능 | 흡연에 의한 복합 생리 반응 고려 |
| `BMI_AST`, `waist_LDL` | 비만 × 간수치 or 지질 | 생활습관병 및 간 기능 동시 반영 |

> 이와 같은 피처들은 단순 변수로는 포착되지 않는 신체 내 상호작용을 모델이 인식할 수 있도록 설계된 것입니다.

