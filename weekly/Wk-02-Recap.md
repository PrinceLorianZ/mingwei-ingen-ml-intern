# Weekly Recap: Week 02

## RFM Dimension Parallels
* The feature engineering methodology translates the CRM RFM (Recency, Frequency, Monetary) framework into predictive maintenance telemetry for the Aido Rover fleet.
* The Recency metric ($T_{current} - T_{last\_FAULT}$) establishes the most direct mathematical and logical parallel to the original CRM dimension.
* Within commercial contexts, Recency measures the elapsed time since a customer's last transaction to forecast account churn probability.
* Within this sensor dataset, Recency measures the elapsed time since a specific robot's last operational FAULT state to forecast impending hardware failure.
* Recency functions as the most robust analogue because it provides a continuous, monotonically increasing baseline of system risk without requiring arbitrary temporal bounding.
* The Frequency analogue calculates the absolute count of ALERT modes over a 7-day rolling window, quantifying the recent density of system anomalies and minor errors.
* The Monetary analogue integrates the battery State of Charge (SoC) reduction over a 7-day period, directly mapping financial expenditure to cumulative robotic energy consumption.
* High Monetary values indicate anomalous power drainage caused by latent mechanical friction or electrical inefficiencies within the rover chassis.

## Exploratory SHAP Teaser Insights
* An exploratory Random Forest classifier processed the 15 engineered telemetry features to validate the proposed physical rationales prior to full pipeline integration.
* The resulting SHAP (SHapley Additive exPlanations) summary bar chart defined a definitive hierarchy of feature importance for predicting critical FAULT states.
* The SHAP analysis confirmed that internal power dynamics and mechanical load metrics serve as the primary indicators of system degradation.
* The model identified a non-linear threshold effect for the Recency feature, where immediate post-fault recovery periods and prolonged operation without maintenance both elevate fault probabilities.
* Cross-unit standardization features, such as Cross_Unit_Z_Current, successfully isolated abnormal individual rover behavior from fleet-wide baseline fluctuations.
* Environmental and localization features demonstrated lower global predictive power compared to direct electromechanical telemetry.

## SHAP Feature Importance Evaluation

| Feature | SHAP Importance Tier | Physical Rationale Validation |
| :--- | :--- | :--- |
| Current_SoC_Ratio | High | Simultaneous high motor current and low SoC forces internal battery resistance spikes, directly triggering critical undervoltage hardware faults. |
| Motor_Current_RollMean_5 | High | Sustained elevations in rolling mean current definitively indicate continuous motor overload, mechanical binding, or severe external terrain resistance. |
| Recency | High | Temporal distance from previous faults correlates directly with cumulative mechanical wear and the statistical degradation cycle of the robotic platform. |
| IMU_Magnitude | Moderate | Total tri-axial acceleration magnitude effectively captures the intensity of mechanical vibrations and physical impacts sustained during patrol operations. |
| RSSI_RollMean_5 | Low | Network signal strength degradation primarily initiates localized communication interruptions rather than global electromechanical or hardware system failures. |
