# InGen Dynamics ML Landscape: Platform Profiles & PIC 2.0 Implications

## 1. InGen Platforms: Machine Learning Analyst Profiles

### Sentinel Prime AI
* **Target Use Case:** Enterprise-grade physical perimeter defense and multi-sensor coordinated surveillance. Identifies active security events (e.g., intrusions, weapons, fire, property damage) via layered sensor fusion (video, infrared, acoustic) for real-time threat detection and pre-warning.
* **Primary ML Task:** Anomaly Detection.
* **Most Important Constraint:** Real-Time latency. Millisecond-level inference is required to intercept threats before they escalate.
* **Scikit-learn Baseline:** `sklearn.ensemble.IsolationForest`
* **Why:** Isolation Forest efficiently handles high-dimensional, multi-sensor data streams without the massive computational overhead of distance or density metrics. It recursively partitions the sample space; since security threats are rare anomalies, they are isolated in shallow leaf nodes. Its highly parallelized nature and low memory footprint perfectly suit ultra-low latency filtering on edge security hardware.

### Senpai
* **Target Use Case:** Adaptive teaching and emotional companion robot for K-12 children. Senses focus levels and psychological fatigue by analyzing facial micro-expressions, vocal pauses, and touch interactions to dynamically adjust educational strategies.
* **Primary ML Task:** Classification.
* **Most Important Constraint:** Safety (compliance with privacy and ethical standards like COPPA, GDPR-K).
* **Scikit-learn Baseline:** `sklearn.ensemble.RandomForestClassifier`
* **Why:** Random Forest offers high classification accuracy while preserving mathematical interpretability. Through Gini Impurity, developers can trace decision paths and feature contributions, satisfying rigorous safety audits and ensuring no hidden biases exist. It is also highly robust against the noise inherent in children's physical interactions.

### Fari
* **Target Use Case:** Daily living assistance, psychological support, and clinical-grade health monitoring for the elderly (home or care facility). Uses LiDAR SLAM and 3D depth cameras to monitor physical behaviors and trigger precise alerts for falls or physiological anomalies.
* **Primary ML Task:** Anomaly Detection.
* **Most Important Constraint:** Latency. Sub-second response times are critical for triggering emergency alerts during falls or cardiovascular events.
* **Scikit-learn Baseline:** `sklearn.svm.OneClassSVM`
* **Why:** By establishing a compact hyperplane envelope bounding the "normal" behavioral patterns of an elderly individual (using an RBF kernel), it circumvents the inability to generalize due to the lack of negative samples (rare fall events). Its highly sparse mathematical solution requires minimal support vector calculations during inference, easily achieving the sub-second latency required for emergency response.

### Aido Rover
* **Target Use Case:** Autonomous outdoor security patrols and physical equipment inspection across diverse, complex terrains (airports, agricultural land, solar/wind farms).
* **Primary ML Task:** Classification.
* **Most Important Constraint:** Real-Time performance. Needs high-frequency (100Hz+) updates to classify terrain and avoid obstacles dynamically to prevent structural damage or accidents.
* **Scikit-learn Baseline:** `sklearn.ensemble.HistGradientBoostingClassifier`
* **Why:** Outdoor environments introduce high noise and missing multi-modal data. This algorithm bins continuous features into integer histograms, drastically reducing tree splitting overhead. Its splitting criteria natively handle missing values without prior imputation, allowing it to maintain ultra-high frequency classification on single-chip edge computing units (MCUs) amidst dynamic outdoor conditions.

### Aido Humanoid
* **Target Use Case:** General-purpose commercial humanoid robot designed for complex physical environments, executing precise hand-eye coordination, stable bipedal walking, and heavy payload handling (e.g., industrial assembly, logistics, advanced home service).
* **Primary ML Task:** Regression.
* **Most Important Constraint:** Safety & Latency. Sub-millisecond latency and smooth torque outputs are required to prevent catastrophic physical instability.
* **Scikit-learn Baseline:** `sklearn.linear_model.Ridge` (combined with `MultiOutputRegressor`)
* **Why:** Bipedal locomotion requires deterministic calculation times. Ridge regression provides a closed-form solution involving only matrix multiplication at the edge, guaranteeing sub-millisecond latency. Furthermore, the L2 regularization penalty eliminates numerical jitter caused by severe multicollinearity among dense joint sensors, ensuring extremely smooth and continuous kinetic torque output to prevent structural collapse.

## 2. PIC 2.0 Foundation Models: Implications and Data Requirements

| Foundation Model | Closest Open-Literature ML Concept | Data Requirement (Labelled vs. Unlabelled) |
| :--- | :--- | :--- |
| **GRPO** (Group Relative Policy Optimisation) | Proximal Policy Optimization (PPO) variant, specifically DeepSeek's Group Relative Policy Optimization (GRPO). | **Unlabelled Data:** Relies on environmental feedback, relative performance scores within a cohort, and trajectory state transitions to compute policy gradients without manually defined absolute values. |
| **STUM** (Spatiotemporal Uncertainty Model) | Bayesian Neural Networks (BNNs) with Monte Carlo Dropout (MCD) combined with Recurrent Neural Networks (LSTMs/GRUs). | **Unlabelled / Semi-supervised Data:** Uses long-term sequential logs of learner performance or robot trajectories to model uncertainty boundaries autonomously via variance distribution on feature activation maps. |
| **SEOM** (Safety-Embedded Objective Model) | Constitutional AI (Anthropic) utilizing Reinforcement Learning from AI Feedback (RLAIF). | **Minimal Labelled Data (Initial) / Unlabelled Preference Data (RL Phase):** Requires a small set of human-written constitutional rules initially, but the RL phase relies entirely on unlabelled preference data generated by the model's self-critique. |
| **AMDC** (Adaptive Motivation and Dynamics Calibration) | Temporal-Behavioral Calibration in Learning Analytics (similar to survival analysis models like XGBoost AFT). | **Labelled Data:** Strictly depends on highly structured, precisely recorded baseline interaction data explicitly mapped (labelled) to specific engagement states for normalization. |
| **HTD-IRL** (Hierarchical Task Decomposition via Inverse RL) | Hierarchical Inverse Reinforcement Learning (HIRL). | **Labelled Data:** Absolutely reliant on high-quality "expert trajectories" (e.g., real-world career paths of top professionals or optimal robot operation sequences) acting as the ground truth. |
| **CRL-MRS** (Cooperative RL for Multi-Robot Systems) | Cooperative Multi-Agent Reinforcement Learning (MARL) using Centralized Training for Decentralized Execution (CTDE). | **Unlabelled Data:** Relies on massive trial-and-error interaction logs, using a globally shared team reward signal to implicitly optimize joint policy distributions without explicit step-by-step labels. |
