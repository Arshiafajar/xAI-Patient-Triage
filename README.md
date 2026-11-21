# xAI-Patient-Triage

An independent research project exploring whether **Shapley-value explanations of unsupervised anomaly detectors remain stable and clinically meaningful under concept drift** — using real ICU vital 
sign data from MIMIC-III..

Directly inspired by Klüttermann, Balestra & Müller (PAKDD 2024) and  Li & Müller (DKE 2024).


## Research Question

> *Can Shapley-value explanations of anomaly detectors trained on 
> ICU vital signs surface clinically meaningful feature combinations 
> to support trustworthy triage decisions — and do those explanations 
> remain stable when concept drift is present?*

---

## Motivation

During my work as a Research Assistant on a government-funded 
Hospital Patient Registration and Prediction System (PAF-IAST, 
Pakistan), I built pipelines over real-time ICU sensor streams. 
A recurring problem: anomaly flags without explanations cannot be 
acted on by clinicians. A doctor cannot trust or challenge a 
black-box score.

Reading Klüttermann et al. (PAKDD 2024) gave me the theoretical 
framework to investigate this properly. The CelebA experiment in 
that paper  where an unsupervised model surfaced semantically 
meaningful features nobody explicitly labeled  directly parallels 
what I observed in clinical data: subtle vital sign combinations 
carry diagnostic signal that falls below conscious clinical attention.

---

## Dataset

**MIMIC-III Clinical Database Demo** (PhysioNet, Johnson et al. 2016)  
- 75 ICU patients after preprocessing  
- 5 vital signs: Heart Rate, Systolic BP, Diastolic BP, 
  Temperature, Respiratory Rate  
- Item IDs verified against D_ITEMS.csv  

> Note: MIMIC-III shifts dates 100 years forward for 
> de-identification. All temporal analyses use relative 
> ordering, not absolute dates.

---

## Methods

| Step | Method |
|---|---|
| Anomaly Detection | Isolation Forest (scikit-learn) |
| Explainability | SHAP  TreeExplainer (Lundberg & Lee, NeurIPS 2017) |
| Concept Drift Simulation | Temporal train/test split (early vs late admissions) |
| Aggregation | Mean vital signs per ICU stay (long → wide format) |

---

## Key Findings

### 1. Clinically meaningful anomalies detected without labels
8 of 75 patients flagged as anomalous. 7 of 8 were MICU admissions.
Notable cases:
- Patient 298685: Systolic BP = 187 mmHg, LOS = 13.3 days (CCU)
- Patient 279721: Temperature = 94.7°F + hypotension (TSICU) 
  — hypothermia/hypotension combination
- Patient 220016: HR = 52 bpm + critically low diastolic BP 
  — bradycardia/shock pattern

### 2. Balanced feature importance — no single vital dominates
SHAP values show all five vitals contribute meaningfully 
(range: 0.313–0.386), suggesting the model detects anomalies 
through vital sign combinations rather than single-feature 
thresholds  consistent with clinical reality.

<img width="1431" height="681" alt="image" src="https://github.com/user-attachments/assets/2144c336-d7c4-4876-b1e3-96a61945235d" />


### 3. Individual explanations reveal distinct clinical patterns
Each flagged patient has a unique SHAP profile the model is 
not applying a single rule. Patient 204132 was flagged for 
cardiorespiratory stress; Patient 279721 for hypothermia + 
hypotension. This mirrors the finding in Klüttermann et al. 
where semantically meaningful features emerged without 
explicit labeling.

<img width="1388" height="481" alt="image" src="https://github.com/user-attachments/assets/2bc5735d-72f8-43a1-bb6d-2e10b7447a23" />
<img width="1397" height="376" alt="image" src="https://github.com/user-attachments/assets/abf871b8-47dd-4f3c-a67a-54a09782fbcc" />
<img width="1405" height="395" alt="image" src="https://github.com/user-attachments/assets/df59a455-322d-4dc0-91b6-7509b5339403" />

### 4. SHAP explanations drift significantly under concept drift
Applying the early-period model to late-period patients:

| Vital Sign | Change in SHAP Importance |
|---|---|
| Heart Rate | **+33.7%** |
| Respiratory Rate | **+32.2%** |
| Systolic BP | +9.2% |
| Diastolic BP | +3.8% |
| Temperature | +2.3% |


<img width="1412" height="742" alt="image" src="https://github.com/user-attachments/assets/48f3f50b-92cf-498b-9780-8ff2effa110f" />


**Core finding:** SHAP explanations are not stable under concept 
drift. A clinician trusting the early-period explanation framework 
would receive systematically different — potentially misleading 
explanations when the model is applied to later patients.

---

## Open Research Questions

1. Can we detect when SHAP explanations have drifted enough to 
   be clinically misleading — without ground truth labels?
2. Would time-aware features (variance, peak values, trends) 
   produce more stable explanations than mean aggregation?
3. Does Li & Müller's state-transition approach transfer to 
   clinical vital sign streams where drift is slower and noisier?
4. How should the contamination threshold be calibrated in 
   unsupervised clinical settings without labeled anomalies?

---

## Limitations

- 75 patients (demo dataset) —findings are preliminary
- Mean aggregation loses temporal patterns within a stay
- No clinician validation of explanation usefulness
- Contamination parameter (0.1) assumed, not derived from labels

---


## References

- Klüttermann S., Balestra C., Müller E. *"On the Efficient 
  Explanation of Outlier Detection Ensembles Through Shapley 
  Values."* PAKDD 2024.
- Li B., Müller E. *"State-Transition-Aware Anomaly Detection 
  Under Concept Drifts."* DKE 2024.
- Lundberg S., Lee S. *"A Unified Approach to Interpreting 
  Model Predictions."* NeurIPS 2017.
- Johnson A. et al. *"MIMIC-III, a freely accessible critical 
  care database."* Scientific Data 2016.

---

*Independent project — october November 2026*  
*Author: Arshia Fajar | PAF-IAST / FH St. Pölten*  
*Contact: etifah2124@gmail.com*
