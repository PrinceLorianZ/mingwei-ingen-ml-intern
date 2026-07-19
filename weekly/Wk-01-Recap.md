# Wk-01 Recap: InGen Platform and Classical ML Task Mapping Analysis

## Titanic ML Pipeline (Structured Tabular Classification)
* **Matching Platform:** Senpai.
* **Feature Space Alignment:** The core of the Titanic task involves processing discrete, static, and structured dimensional features. The interaction features collected by the Senpai platform via the AMDC model (e.g., response latency in milliseconds, task switching frequency, error pattern cluster ID) are completely isomorphic to a standard two-dimensional structured tabular dataset after preprocessing.
* **Objective Function Alignment:** Both execute discrete state classification tasks. Titanic predicts a binary survival state, while Senpai partitions the tabular feature space into four discrete cognitive and psychological fatigue state intervals.
* **Baseline Algorithm Alignment:** The classical baseline for Titanic utilizes decision trees and their ensemble methods. Senpai identically selects `sklearn.ensemble.RandomForestClassifier`, utilizing orthogonal partitioning of the feature space to handle non-linear classification boundaries.
* **Compliance and Interpretability Alignment:** The Titanic task routinely requires the output of feature importance to validate business logic. Senpai, constrained by strict privacy regulations such as COPPA, must provide mathematical transparency based on the Gini Impurity $I_{G}(f)=1-\sum_{i=1}^{m}p_{i}^{2}$, entirely excluding black-box decision systems.

## NYSE Stock Prediction Task (High-Frequency Time Series)
* **Matching Platform:** Aido Humanoid.
* **Data Flow Alignment:** High-frequency quantitative US stock trading data exhibits strong non-stationarity, multicollinearity, and ultra-high sampling frequencies. The underlying control loop of the Aido Humanoid receives feedback from plantar six-axis force sensors, IMU attitude angles, and dense motor encoders, constituting a typical high-dimensional, high-frequency coupled time series.
* **Objective Function Alignment:** ML pipelines developed for trading data aim to perform regression to predict continuous asset prices in future time steps. The core task of the Aido Humanoid utilizes the sensor sequence from historical time steps to perform regression predicting the continuous target angular displacement and compensation torque for the next sampling cycle.
* **Algorithm and Penalty Term Alignment:** Quantitative finance systems frequently introduce $L_2$ regularization to handle the multicollinearity of financial indicators to prevent model collapse. The Aido Humanoid utilizes `sklearn.linear_model.Ridge`, introducing the identical regularization penalty term to eliminate numerical jitter between physical sensors.
* **System Risk Control Constraint Alignment:** Millisecond-level delays or abnormal step changes in US stock trading systems cause catastrophic capital drawdowns. The Aido Humanoid relies on the Ridge regression closed-form solution $W=(X^{T}X+\alpha I)^{-1}X^{T}Y$ to ensure absolute deterministic calculation at the sub-millisecond level; its underlying risk control logic is entirely consistent with high-frequency trading infrastructure.

## Core Architecture Comparison Matrix

| Analysis Dimension | Titanic Mapping (Senpai) | NYSE Mapping (Aido Humanoid) |
| :--- | :--- | :--- |
| **Underlying Data Structure** | Discrete, static 2D table | Continuous, high-frequency time series flow |
| **Primary ML Task** | Multi-class Classification | Multi-output Regression |
| **Scikit-learn Baseline** | `RandomForestClassifier` | `Ridge` |
| **Strictest Engineering Constraint** | Algorithmic transparency and compliance safety | Inference latency and temporal stability |
| **Core Mathematical Evaluation/Solution** | $I_{G}(f)=1-\sum_{i=1}^{m}p_{i}^{2}$ | $W=(X^{T}X+\alpha I)^{-1}X^{T}Y$ |
