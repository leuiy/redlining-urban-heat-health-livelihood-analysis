# From Redlining to Heat: Urban Environmental Inequality and Its Human Cost in U.S. Cities
🇺🇸 [English](#English) | 🇰🇷 [한국어](#한국어)

## English
## HOLC Redlining → Vegetation Deficit → Urban Heat → Health and Livelihood Vulnerability
A spatial causal analysis examining how 1930s HOLC redlining classifications have driven regional vegetation disparities over approximately 100 years, and how the resulting surface temperature inequalities further impact health and livelihood vulnerability. The analysis covers 108 U.S. cities and 13,518 census tracts.

## Study Overview — Serial Mediation Model
HOLC Rating → NDVI Variation (M1) → LST Variation (M2) → Health & Livelihood Outcomes
- Framework: 4-step serial mediation (HOLC Grade → NDVI Deviation → LST Deviation → Health & Livelihood Vulnerability)
- Group comparison: Kruskal-Wallis + Mann-Whitney U (Bonferroni correction)
- Feature engineering: PCA-based social vulnerability index, environmental redlining index (ndvi_holc)
- Regression: Forward stepwise OLS → quadratic multiple regression (R²=0.426)
- Nonlinear threshold: LST response inflection point at ndvi_diff = −0.416
- PSM review: Estimated propensity scores for A vs. D grade tracts (AUC=0.903), confirming extreme group separation — PSM was deemed inappropriate; causal validity instead verified via Bootstrap CI
- Mediation: 4-step Baron-Kenny serial mediation + Sobel Test + Bootstrap CI
- Machine learning: GridSearch-tuned ensemble (RF, ExtraTrees, GBM, XGBoost, LightGBM) + city-level Group K-Fold CV + hard/soft voting (Test R²=0.485)

## Technologies
Python: pandas, numpy, scipy, statsmodels, scikit-learn, xgboost, lightgbm, matplotlib, seaborn

## Key Findings
- HOLC D-grade tracts show significantly lower NDVI and higher LST vs. A-grade (p<0.001)
- LST deviation spikes sharply as ndvi_diff approaches −0.416
- Quadratic multiple regression with mean-centering (R²=0.426)
- LST is heavily influenced by physical factors; ML models were built solely to verify the absence of underfitting in the regression model
- A hard-voting ensemble (Test R²=0.485) confirmed the absence of underfitting in the regression model. City-level Group K-Fold CV was implemented for robust cross-validation.
- LST declines beyond the inflection point (high-rise shadowing effect) — the most heat-vulnerable areas are those around the threshold, not the densest cores
- Regional inflection points vary: the arid West is sensitive to even minor green space loss; the dense-canopy Northeast shows relatively high resilience
- HOLC → NDVI → LST mediation ratio: 104% (suppression effect) — controlling for vegetation reverses the sign of HOLC's direct path; Bootstrap 95% CI [0.436, 0.496] excludes 0
- NDVI → LST → Health & Livelihood Vulnerability mediation: full mediation confirmed for sleep deprivation, food insecurity, and housing instability
- Final 4-step serial mediation (HOLC → NDVI → LST → Health & Livelihood Vulnerability): partial mediation confirmed in 14 of 15 variables — independent socioeconomic pathways (economic isolation, healthcare infrastructure gaps) coexist alongside the environmental route

## Conclusion
- Historical HOLC grades have driven regional vegetation disparities over 100 years, fully mediating their impact on present-day LST
- Vegetation index fully mediates through surface temperature to drive sleep deprivation, food insecurity, and housing instability
- Pathways from HOLC grades to current health and livelihood vulnerability include mediating factors beyond vegetation and surface temperature
- Heat vulnerability policy should target areas around the identified inflection point rather than applying uniform linear support
- Long-term policy effects must be considered, as current interventions may compound over time to influence new variables
- 
## My Contributions (4-member team)
- Primary: Team lead, Primary code author {preprocessing → regression → ML → mediation(Causal, Sobel test, Bootstrap CI)}
- Supporting: EDA, PCA, regional inflection point derivation

## 한국어
## HOLC Redlining → 식생 지수 → 지표면 온도 → 보건 및 생활 취약성
1930년대의 차별적 주거 정책(HOLC 레드라이닝 등급)이 지난 100년간 어떻게 지역별 식생 편차를 야기했는지, 그리고 이로 인한 지표면 온도 불평등이 현대 미국의 보건 및 생활 취약성에 미치는 영향을 확인한 공간 인과 분석 프로젝트입니다. 미국 108개도시, 13518개의 센서스 트랙을 대상으로 분석하였습니다.

## 연구 개요 - 직렬 매개 모형 (serial mediation)
HOLC 등급 → NDVI 편차 (M1) → LST 편차 (M2) → 건강 및 생활 결과
- 등급 간 비교: Kruskal-Wallis + Mann-Whitney U (Bonferroni 보정)
- 파생변수: PCA 기반 사회취약성 지수, 환경적 레드라이닝 지수 (ndvi_holc)
- 회귀분석: Forward Stepwise OLS → 이차다중회귀모형 (R²=0.426)
- 비선형 임계점: LST 반응 변곡점 ndvi_diff = −0.416
- 선택 편향 검토: A↔D 등급 간 성향점수 추정 결과 AUC 0.903으로 극단적 분리 확인 → PSM 부적합 판단, Bootstrap CI로 인과성 대체 검증
- 매개분석: 4단계 Baron-Kenny 직렬 매개 + Sobel Test + Bootstrap CI
- 머신러닝: GridSearch 기반 (RF, ExtraTrees, GBM, XGBoost, LightGBM) + 도시 단위 Group K-Fold CV + 하드/소프트 보팅 (Test R²=0.485)

## 사용 기술
Python: pandas, numpy, scipy, statsmodels, scikit-learn, xgboost, lightgbm, matplotlib, seaborn

## 주요 결과
- HOLC D등급 트랙은 A등급 대비 유의하게 낮은 NDVI, 높은 LST 확인 (p<0.001)
- ndvi_diff가 −0.416에 가까워질수록 LST 편차 급등
- 중심화를 사용한 이차다중회귀모형 도출 (R²=0.426)
- 지표면 온도는 물리적 요인을 크게 받는 요소로, 회귀모형의 과소적합 여부 검증을 위한 머신러닝 모델 구축.
- 하드보팅 앙상블 모델(R²=0.485)의 성능을 통해 회귀모형의 과소적합이 없음을 확인. 교차 검증을 위해 도시명을 기준으로 group k-fold 실행.
- 변곡점 이후 LST 감소 (고층 건물 그늘 효과) -> 열취약한 지역은 최고 밀집 지역이 아닌 변곡점 주변부
- 지역별 변곡점 상이: 서부(건조기후)는 소량 녹지 손실에도 민감, 북동부(수목 밀도 높음)는 상대적 회복력 높음
- HOLC → NDVI → LST 매개비율 104% (억압 효과): 녹지 통제 시 HOLC 직접 경로 부호 역전 & Bootstrap 95% CI [0.436, 0.496], 0 미포함
- NDVI → LST → 보건&생활 취약성 매개 분석결과 수면 부족, 식량 불안정, 주거 불안정 변수에서 완전 매개 확인
- 최종 4단계 직접 매개 분석 (HOLC 등급 → NDVI 편차 (M1) → LST 편차 (M2) → 건강 및 생활) 결과: 총 15개 변수 중 14개 변수에서 부분 매개 작용 확인
    - 환경 경로 외 경제적 고립, 의료 인프라 격차 등 독립적 사회경제 경로 병존

## 결론
- 과거 HOLC 등급은 100년동안 지역간 식생 지수의 편차를 야기하여 이를 완전 매개로 현재의 LST에 영향을 주고 있음.
- 식생 지수는 지표면온도를 완전 매개로 하여 수면부족, 식량불안정 경험률, 주거불안정 경험률 등을 야기하고 있음.
- HOLC 등급이 현재의 보건&생활 취약성으로 연결되는 매개에는 '식생 편차 및 지표면 온도' 외에 기타 매개 요인이 있는것으로 보임.
- 폭염 피해 정책의 경우 선형적 지원이 아닌 변곡점을 기준으로 한 취약 지역을 타겟화하여 진행될 필요가 있음.
- 현재 실행되는 정책이 시간의 흐름 속에 더 큰 변화를 야기하여 새로운 변수에 영향을 줄 수 있음을 고려해야 함.

## 내 역할 (4인 구성)
- 메인 역할: 팀장, 전반적 코드 구현 {(전처리 → 회귀분석 → 머신러닝 → 매개분석(Causal, Sobel Test, Bootstrap CI)}
- 보조 역할: EDA, PCA, 지역별 변곡점 도출
