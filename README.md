# 🚀 NSGA Multi-objective Optimal

[![Dataset](https://img.shields.io/badge/Dataset-KAMP%20AI-lightgrey)](https://www.kamp-ai.kr/aidataDetail?AI_SEARCH=&page=4&DATASET_SEQ=49&EQUIP_SEL=&GUBUN_SEL=&FILE_TYPE_SEL=&WDATE_SEL=)

---

## 📚 Table of Contents

- [🚀 NSGA Multi-objective Optimal](#-nsga-multi-objective-optimal)
  - [📚 Table of Contents](#-table-of-contents)
  - [✨ Overview](#-overview)
  - [🎯 Problem Statement](#-problem-statement)
  - [🔍 Exploratory Data Analysis (EDA)](#-exploratory-data-analysis-eda)
    - [상관행렬 및 히트맵](#상관행렬-및-히트맵)
    - [Pair KDE Plot (Before Cleansing)](#pair-kde-plot-before-cleansing)
    - [Pair KDE Plot (After Outlier Removal)](#pair-kde-plot-after-outlier-removal)
  - [🧠 Model Training](#-model-training)
    - [주요 시각화](#주요-시각화)
    - [Feature Importance Table](#feature-importance-table)
  - [🎮 Simulation Setup](#-simulation-setup)
    - [노이즈 적용 프로세스](#노이즈-적용-프로세스)
      - [기존 데이터 분포](#기존-데이터-분포)
      - [프로세스 단계](#프로세스-단계)
  - [🔗 References](#-references)

---

## ✨ Overview

이 프로젝트는 **소성가공 공정의 품질 불량 문제**를 해결하기 위한 다중 목적 최적화 프로젝트입니다.  
제공된 데이터셋을 바탕으로 결함 판정 및 주요 변수 확인뿐만 아니라, 각 변수의 변형 시뮬레이션을 통해 공정 난이도, 제품 품질, 가격을 **동시에 최적화**하는 것을 목표로 합니다.

---

## 🎯 Problem Statement

- **목표**: 양품과 불량에 영향을 주는 변수 간의 상관관계를 분석하고, AI 모델을 활용하여 불량 판정 및 품질 개선에 기여합니다.
- **추가 과제**: 변수 변형 시뮬레이션을 통해 **공정 난이도, 품질, 가격의 다중 목적 최적화**를 달성합니다.

---

## 🔍 Exploratory Data Analysis (EDA)

<details>
  <summary><strong>1. EDA 상세 분석</strong></summary>
  
### 상관행렬 및 히트맵

- **설명**: 데이터 클렌징 이전에 확인한 상관행렬에서, 우측 하단 feature들 간 높은 상관성을 확인할 수 있습니다.
- **시각화**:
  <details>
    <summary>Show Heatmap</summary>
    
    ![Heatmap](./img/heatmap.png)
  </details>

---

### Pair KDE Plot (Before Cleansing)

- **설명**: 우측 하단 feature들의 밀도 플롯을 확인합니다.
- **시각화**:
  <details>
    <summary>Show Pair KDE Plot (Before)</summary>
    
    ![Pair KDE Before](./img/pair_kde_plot_before_clean.png)
  </details>

> **참고**: 좌측 상단 feature들은 상관관계가 명확하지만, 우측 하단 feature들은 이상치로 인한 높은 상관성이 있음이 의심됩니다.

---

### Pair KDE Plot (After Outlier Removal)

- **설명**: 이상치를 제거한 후, `EX1.MD-TQ`는 단일 값을 가지며 분산이 0임을 확인하였습니다.
- **시각화**:
  <details>
    <summary>Show Pair KDE Plot (After)</summary>
    
    ![Pair KDE After](./img/pair_kde_plot.png)
  </details>

- **결론**: 통제 가능한 변수와 변형 가능한 변수들을 성공적으로 구분하였습니다.
  - **통제할 변수**: `EX1.H4_PV`, `EX1.H2O_PV`, `EX1.MELT_P_PV`
  
</details>

---

## 🧠 Model Training

<details>
  <summary><strong>2. 모델 훈련</strong></summary>

모델 훈련에는 **AutoGluon**의 AutoML 모듈을 사용하였습니다.  
모델 평가 지표는 아래와 같습니다.

### 주요 시각화

- **혼동행렬 (Confusion Matrix)**
  <details>
    <summary>Show Confusion Matrix</summary>
    
    ![Confusion Matrix](./img/confusion_matrix.png)
  </details>

- **ROC Curve**
  <details>
    <summary>Show ROC Curve</summary>
    
    ![ROC Curve](./img/roc_curve.png)
  </details>

---

### Feature Importance Table

AutoGluon을 통해 각 변수의 중요도와 통계 지표를 아래 표로 확인할 수 있습니다:

| Feature           | Importance  | Std Dev   | P-Value  | n  | P99 High  | P99 Low   |
|-------------------|-------------|-----------|----------|----|-----------|-----------|
| EX1.MD_PV         | 0.463756    | 0.026445  | 0.000001 | 5  | 0.518207  | 0.409306  |
| EX1.MELT_P_PV     | 0.038641    | 0.028176  | 0.018708 | 5  | 0.096655  | -0.019373 |
| EX1.Z1_PV         | 0.021422    | 0.011535  | 0.007116 | 5  | 0.045173  | -0.002330 |
| EX1.H2O_PV        | 0.017881    | 0.014669  | 0.026337 | 5  | 0.048084  | -0.012322 |
| EX1.A1_PV         | 0.007319    | 0.008252  | 0.059176 | 5  | 0.024309  | -0.009671 |
| EX1.A2_PV         | 0.003299    | 0.004521  | 0.089050 | 5  | 0.012608  | -0.006010 |
| EX1.H1_PV         | 0.002655    | 0.011458  | 0.315860 | 5  | 0.026246  | -0.020936 |
| EX1.H4_PV         | 0.002333    | 0.005217  | 0.186950 | 5  | 0.013076  | -0.008409 |
| EX1.Z2_PV         | 0.001814    | 0.004056  | 0.186950 | 5  | 0.010166  | -0.006538 |
| EX1.Z4_PV         | 0.001502    | 0.003358  | 0.186950 | 5  | 0.008417  | -0.005413 |
| EX1.H3_PV         | 0.000000    | 0.000000  | 0.500000 | 5  | 0.000000  | 0.000000  |
| EX5.MELT_TEMP     | 0.000000    | 0.000000  | 0.500000 | 5  | 0.000000  | 0.000000  |
| EX1.H2_PV         | 0.000000    | 0.000000  | 0.500000 | 5  | 0.000000  | 0.000000  |
| EX4.MELT_TEMP     | 0.000000    | 0.000000  | 0.500000 | 5  | 0.000000  | 0.000000  |
| EX1.Z3_PV         | 0.000000    | 0.000000  | 0.500000 | 5  | 0.000000  | 0.000000  |
| EX2.MELT_TEMP     | 0.000000    | 0.000000  | 0.500000 | 5  | 0.000000  | 0.000000  |
| EX3.MELT_TEMP     | 0.000000    | 0.000000  | 0.500000 | 5  | 0.000000  | 0.000000  |
| EX1.MD_TQ         | 0.000000    | 0.000000  | 0.500000 | 5  | 0.000000  | 0.000000  |

</details>

---

## 🎮 Simulation Setup

시뮬레이션 단계에서는 선정된 변수들을 기반으로 전체 시스템 모델링과 최적화 시뮬레이션을 진행합니다.

- **시뮬레이션 제외 변수**:  
  - `EX1.H4_PV`, `EX1.H2O_PV`, `EX1.MELT_P_PV`  
    → 상관관계가 높아 결과 신뢰성에 영향을 줄 수 있음.
  - `"EX1.H3_PV"`, `"EX5.MELT_TEMP"`, `"EX1.H2_PV"`, `"EX4.MELT_TEMP"`, `"EX1.Z3_PV"`, `"EX2.MELT_TEMP"`, `"EX3.MELT_TEMP"`, `"EX1.MD_TQ"`  
    → 분산이 0인 상수열

- **목표**: 공정 난이도, 제품 품질 및 가격을 최적화하여 불량률을 최소화하고, 새로운 상/하한을 최대화합니다.

---

### 노이즈 적용 프로세스

#### 기존 데이터 분포
<details>
  <summary><strong>Show Raw Distribution</strong></summary>
  
  ![Raw Distribution](./img/raw_dist.png)
</details>

기존 데이터는 특정 분포를 따른다고 보기 어렵습니다.  
따라서, 가우시안 노이즈와 KDE 노이즈를 각각 적용한 분포를 확인하였습니다.

- **시각화**:
  <details>
    <summary>Show Noise Visualizations</summary>
    
    ![Gaussian Noise](./img/Gaussian_noise.png)  
    ![KDE Noise](./img/KDE_noise.png)
  </details>

> **선택 이유**: 해당 데이터에서는 KDE 노이즈가 더욱 자연스러운 결과를 제공하여 이를 채택하였습니다.

#### 프로세스 단계

1. **Shift 적용**: Train 데이터의 최소/최대 값을 기준으로 shift 값을 적용해 새로운 구간을 설정합니다.
2. **정규화**: 해당 구간에서 데이터를 정규화합니다.
3. **로짓 변환**: 정규화한 데이터에 대해 로짓 변환(`log(x_norm/(1-x_norm))`)을 수행합니다.
4. **KDE 샘플링**: KDE 알고리즘을 사용해 데이터를 샘플링합니다.
5. **역 로짓 변환**: 최종 데이터를 생성하여 노이즈를 추가합니다.

> 이 과정을 통해 보다 자연스러운 노이즈가 추가된 데이터를 생성할 수 있습니다.

---

## 🔗 References

- **데이터 출처**: [KAMP AI](https://www.kamp-ai.kr/aidataDetail?AI_SEARCH=&page=4&DATASET_SEQ=49&EQUIP_SEL=&GUBUN_SEL=&FILE_TYPE_SEL=&WDATE_SEL=)
- **AutoML Tool**: [AutoGluon](https://auto.gluon.ai/)

---

✨ Contributions and enhancements are welcome!  
Happy optimizing and stay creative!