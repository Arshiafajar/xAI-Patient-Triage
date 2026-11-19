# xAI-Patient-Triage

# I am building an explainable patient triage system using ICU vital sign data. I train an unsupervised anomaly detector on patient vitals to flag high-acuity patients. I apply Shapley-value attribution to explain which vital signs drove each flag. I then investigate whether those explanations remain stable and faithful when concept drift is present — because patient populations and sensor readings change continuously in real clinical settings
Inspired by the 'On the efficient Explanation of Outlier Detection  Ensembles through Shapley Values' by Professor Emmanuel Muller


Applying Shapley-value attribution to patient acuity scoring using ICU vital sign data (MIMIC-III), inspired by Klüttermann 
et al. (PAKDD 2024).
## Research Question
Can Shapley-value explanations of anomaly detectors trained on ICU vital signs surface clinically meaningful feature combinations  to support trustworthy triage decisions?
## Dataset
MIMIC-III Clinical Database Demo (PhysioNet)
## Methods
- Isolation Forest (anomaly detection)
- SHAP (Shapley-value attribution)

## Status
Work in progress — started 20th October 2026

## References
Simon Klu ̈ttermann[0000−0001−9698−4339], Chiara Balestra[0000−0002−3620−9012],  and Emmanuel Mu ̈ller[0000−0002−5409−6875]
