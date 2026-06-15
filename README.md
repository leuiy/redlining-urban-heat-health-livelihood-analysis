# From Redlining to Heat: Urban Environmental Inequality and Its Human Cost in U.S. Cities
🇺🇸 [English](#English) | 🇰🇷 [한국어](#한국어)

---

## Live Reports / 분석 결과
- [holc-heat](https://leuiy.github.io/redlining-urban-heat-health-livelihood-analysis/holc-heat.html)
- [health&livelihood](https://leuiy.github.io/redlining-urban-heat-health-livelihood-analysis/health&livelihood.html)

---

## English
## HOLC Redlining → Vegetation Deficit → Urban Heat → Health and Livelihood Vulnerability
A spatial causal analysis examining how 1930s HOLC redlining classifications have driven regional vegetation disparities over approximately 100 years, and how the resulting surface temperature inequalities further impact health and livelihood vulnerability. The analysis covers 108 U.S. cities and 13,518 census tracts.

## Study Overview — Serial Mediation Model
- __Framework: HOLC Rating > NDVI Variation (M1) > LST Variation (M2) > Health & Livelihood Outcomes__
- __Group comparison__: Kruskal-Wallis + Mann-Whitney U (Bonferroni correction)
- __Feature engineering__: PCA-based social vulnerability index, environmental redlining index (ndvi_holc)
- __Regression__: Forward stepwise OLS → quadratic multiple regression (R²=0.426)
- __Nonlinear inflection point__: Analytically derived LST inflection point at ndvi_diff = −0.416
- __Selection bias check__: Propensity scores estimated for A vs. D grade tracts (AUC=0.903), the two groups are near-perfectly separated — PSM deemed inappropriate; causal validity verified via Bootstrap CI instead
- __Mediation__: 4-step Baron-Kenny serial mediation + Sobel Test + Bootstrap CI
- __Machine learning__: 5-model ensemble (RF, ExtraTrees, GBM, XGBoost, LightGBM) with RandomizedSearchCV + GridSearchCV tuning, city-level Group K-Fold CV (spatial leakage prevention), hard/soft voting (Test R^2=0.485)

## Technologies
Python: pandas, numpy, scipy, statsmodels, scikit-learn, xgboost, lightgbm, matplotlib, seaborn

## Key Findings
__1. Group Differences__
- HOLC D-grade tracts show significantly lower NDVI and higher LST vs. A-grade (p<0.001)
- Mann-Whitney U post-hoc (Bonferroni corrected): A↔D and B↔D significant; A↔B and C↔D not significant — gradient effect concentrated between boundary grades

__2. Regression & ML (LST)__
- Quadratic multiple regression with mean-centering (R^2=0.426)
- Mean-centering applied to resolve multicollinearity between ndvi_diff and ndvi_diff^2 (VIF reduced from 14 to 1.3)
- LST is heavily driven by physical factors; an ML ensemble (Test R^2=0.485) was built to check that the OLS model isn't underfitting — not as a predictive tool
- 5 models tuned via RandomizedSearchCV (XGBoost, LightGBM) then GridSearchCV (all); city-level Group K-Fold CV used to prevent spatial data leakage across train/test splits
- Hard voting and soft voting (R^2-weighted) produced identical results (R^2=0.485, RMSE=1.725), confirming model stability

  __3. Nonlinear Inflection point__
- Inflection point derived analytically as −β₁/2β₂ = −(−13.538)/(2×−24.002) ≈ −0.282 (mean-centered), equivalent to ndvi_diff = −0.416 in original units
- LST deviation spikes sharply as ndvi_diff approaches −0.416, then declines beyond the threshold (high-rise shadowing effect)
- The most heat-vulnerable areas are those near the inflection point, not the densest urban cores
- Regional inflection points vary: the arid West is sensitive to even minor green space loss; the dense-canopy Northeast shows relatively higher resilience

__4. Mediation: HOLC > NDVI > LST (3-step)__
- Mediation ratio: 104% — a suppression effect; controlling for vegetation reverses the sign of HOLC's direct path
- Bootstrap 95% CI [0.436, 0.496] excludes 0, indirect effect is significant
- AUC=0.903 on PSM indicated near-perfect separation between A and D grade tracts, so matched comparison was dropped; Bootstrap CI used as the causal validity check in place of PSM

__5. Health & Livelihood Outcomes: Regression + Mediation (3-step)__
- Multivariate OLS across 15 health outcomes confirmed HOLC grade and LST as significant predictors in 13/15 outcomes each; ndvi_diff shows direction reversal in chronic disease outcomes due to spatial confounding (high-income A-grade suburbs with auto-dependent lifestyles)
- 3-step mediation (ndvi_diff → LST → health&life): full mediation confirmed for sleep deprivation, food insecurity, and housing instability; suppression effects observed for chronic diseases — the LST cooling pathway operates as expected but is masked by confounding

__6. Final 4-step Mediation__
- 4-step serial mediation (HOLC → NDVI → LST → health&life): the serial indirect path (ind3) is positive and significant across all 15 outcomes, the theoretical causal chain holds
- Partial mediation in sleep deprivation (8.1%), food insecurity (5.5%), housing instability (7.6%); suppression effects in 11 chronic disease variables

## Conclusion
- Historical HOLC grades have driven regional vegetation disparities over 100 years, fully mediating their impact on present-day LST
- Vegetation index fully mediates through surface temperature to drive sleep deprivation, food insecurity, and housing instability
- The theoretical serial path (HOLC → NDVI → LST → health&life) is confirmed across all 15 outcomes via the serial indirect effect (ind3)
- Heat vulnerability policy should target areas near the identified inflection point rather than applying uniform linear support
- Long-term cumulative policy effects must be considered, as current interventions may compound over time to influence new variables

## Limitations
- Spatial confounding: historically A-grade areas (high vegetation) overlap with high-income, auto-dependent suburbs, which attenuates or reverses expected associations for chronic disease outcomes
- ndvi_diff in the food insecurity regression is borderline significant (p=0.071); 3-step mediation results for this variable should be interpreted with caution
- Depression shows a near-zero total effect (c_total p=0.538) with direct and indirect pathways nearly canceling out — causal pathway for depression falls outside this model's scope and warrants further investigation

## My Contributions (4-member team)
- Primary: Team lead, Primary code author {preprocessing → regression → ML → mediation(Causal, Sobel test, Bootstrap CI)}
- Supporting: EDA, PCA, regional inflection point derivation

---

## 한국어
## HOLC Redlining → 식생 지수 → 지표면 온도 → 보건 및 생활 취약성
1930년대의 차별적 주거 정책(HOLC 레드라이닝 등급)이 지난 100년간 어떻게 지역별 식생 편차를 야기했는지, 그리고 이로 인한 지표면 온도 불평등이 현대 미국의 보건 및 생활 취약성에 미치는 영향을 확인한 공간 인과 분석 프로젝트입니다. 미국 108개도시, 13518개의 센서스 트랙을 대상으로 분석하였습니다.

## 연구 개요 - 직렬 매개 모형 (serial mediation)
__HOLC 등급 → NDVI 편차 (M1) → LST 편차 (M2) → 건강 및 생활 결과__
- __등급 간 비교__: Kruskal-Wallis + Mann-Whitney U (Bonferroni 보정)
- __파생변수__: PCA 기반 사회취약성 지수, 환경적 레드라이닝 지수 (ndvi_holc)
- __회귀분석__: Forward Stepwise OLS → 이차다중회귀모형 (R^2=0.426)
- __비선형 변곡점__: 중심화 이차모형에서 −β₁/2β₂로 해석적 도출, ndvi_diff = −0.416
- __선택 편향 검토__: A↔D 등급 간 성향점수 추정 결과 AUC 0.903으로 극단적 분리 확인 → PSM 부적합 판단, Bootstrap CI로 인과성 대체 검증
- __매개분석__: 4단계 Baron-Kenny 직렬 매개 + Sobel Test + Bootstrap CI
- __머신러닝__:  5개 모델 앙상블 (RF, ExtraTrees, GBM, XGBoost, LightGBM), RandomizedSearchCV + GridSearchCV 하이퍼파라미터 튜닝, 도시 단위 Group K-Fold CV (공간 누수 방지), 하드/소프트 보팅 (Test R^2=0.485)

## 사용 기술
Python: pandas, numpy, scipy, statsmodels, scikit-learn, xgboost, lightgbm, matplotlib, seaborn

## 주요 결과
__1. 등급 간 차이__
- HOLC D등급 트랙은 A등급 대비 유의하게 낮은 NDVI, 높은 LST 확인 (p<0.001)
- Mann-Whitney U 사후검정 (Bonferroni 보정): A↔D, B↔D 유의 / A↔B, C↔D 비유의 — 등급 경계 간 집중적 격차 패턴

__2. Regression (LST)__
- 중심화를 사용한 이차다중회귀모형 도출 (R^2=0.426)
- ndvi_diff와 ndvi_diff^2 간 다중공선성 해소를 위해 평균 중심화 적용 (VIF 14에서 1.3으로 감소)
- 지표면 온도는 물리적 요인을 크게 받는 변수로, 앙상블 모델은 회귀모형의 과소적합 여부 검증 목적으로만 구축 (예측 도구 아님)
- 5개 모델을 RandomizedSearchCV(XGBoost&LightGBM) + GridSearchCV(전체)로 튜닝 / 도시명 기준 Group K-Fold로 공간 데이터 누수 방지
- 하드보팅과 소프트보팅(R^2 가중) 결과 동일 (R^2=0.485, RMSE=1.725) > 모델 안정성 확인

  __3. 비선형 변곡점__
- 변곡점을 −β₁/2β₂ = −(−13.538)/(2×−24.002) ≈ −0.282 (중심화 단위)로 해석적 도출, 기존 단위 기준 ndvi_diff = −0.416
- LST deviation spikes sharply as ndvi_diff approaches −0.416, then declines beyond the threshold (high-rise shadowing effect)
- ndvi_diff가 −0.416에 가까워질수록 LST 편차 급등, 이후 감소 (고층 건물 그늘 효과)
- 열취약은 녹색 지수에 따라 선형적 관계가 아닌, 변곡점 주변부 급증 형태
- 지역별 변곡점 상이: 서부(건조기후)는 소량 녹지 손실에도 민감, 북동부(수목 밀도 높음)는 상대적 회복력 높음
__4. 매개분석: HOLC > NDVI > LST (3단계)__
- 매개비율 104% — 억제효과: 녹지 통제 시 HOLC 직접 경로의 부호 역전
- Bootstrap 95% CI [0.436, 0.496], 0 미포함 → 간접효과 유의 확인
- PSM AUC=0.903으로 A-D등급 간 극단적 분리 확인 > 매칭 기반 비교 불가 판단, Bootstrap CI를 인과 검증 수단으로 대체 활용

__5.  보건 및 생활 결과: 회귀분석 + 매개분석 (3단계)__
- 15개 보건 결과변수에 대한 다변량 OLS: HOLC 등급·LST 모두 13/15개에서 유의한 양수 예측변수로 확인 / ndvi_diff는 만성질환에서 방향이 역전되는데, 고소득 교외지역과 A등급 지역의 중첩으로 인한 공간 교란이 추정됨
- 3단계 매개 (ndvi_diff → LST → 보건및생활): 수면부족-식량불안정-주거불안정에서 완전매개 확인 / 만성질환에서는 억제효과 — LST 경로는 예상대로 작동하지만, 교란 변수로 인해 총효과 방향이 역전된 것으로 추정됨

__6. 최종 4단계 직렬 매개분석__
- 4단계 직렬 매개 (HOLC → NDVI → LST → 보건및생활): 직렬 간접효과(ind3)가 15개 전 변수에서 양수·유의 > 이론의 인과 경로를 전반으로 지지
- 수면부족(8.1%)·식량불안정(5.5%)·주거불안정(7.6%)에서 부분매개, 만성질환 11개에서 억제효과

## 결론
- 과거 HOLC 등급은 100년동안 지역간 식생 지수의 편차를 야기하여 이를 완전 매개로 현재의 LST에 영향을 주고 있음.
- 식생 지수는 지표면온도를 완전 매개로 하여 수면부족, 식량불안정 경험률, 주거불안정 경험률 등을 야기하고 있음.
- 직렬 간접효과(ind3)를 통해 이론 인과 경로(HOLC → NDVI → LST → 보건및생활)가 전 15개 변수에서 확인됨.
- 폭염 피해 정책은 선형적 지원이 아닌 변곡점을 기준으로 한 취약 지역 타겟화 필요
- 현재 실행되는 정책이 시간의 흐름 속에 더 큰 변화를 야기하여 새로운 변수에 영향을 줄 수 있음을 고려해야 함.

## 한계점
- 공간적 교란: 역사적 A등급 지역(녹지 많음)이 현재 고소득 차량의존 교외지역과 중첩되어 만성질환에서의 예상 연관성이 감쇠 또는 역전됨
- 식량불안정 회귀분석의 ndvi_diff p=0.071로 경계 유의 — 해당 변수의 3단계 매개 결과는 해석 시 주의 필요
- 우울증은 c_total이 통계적으로 비유의(p=0.538)하며 직접-간접 경로가 상쇄되는 구조로, 본 모델의 설명 범위를 벗어나 별도의 연구가 필요해 보임.

## 내 역할 (4인 구성)
- 메인 역할: 팀장, 전반적 코드 구현-(전처리 → 회귀분석 → 머신러닝 → 매개분석(Causal, Sobel Test, Bootstrap CI)
- 보조 역할: EDA, PCA, 지역별 변곡점 도출
