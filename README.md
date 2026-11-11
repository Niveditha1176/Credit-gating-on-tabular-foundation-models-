# Context-Stable Explanations and Conformal Deferral for Credit Decisions with Tabular Foundation Models

### Conformal Deferral, Real-Time Shapley Explanations, and Explanation Stability

> **Author:** Niveditha Venkatesh  
> **Affiliation:** School of Electronics and Computer Science and Engineering, Vellore Institute of Technology (VIT), Chennai  

---

##  Executive Summary

Modern automated credit underwriting systems require high accuracy, transparent explanations, and accountable human oversight. While classical systems rely on retrained tree ensembles and post-hoc Tree SHAP, this research establishes an auditable **three-tier credit decision framework** powered by in-context Tabular Foundation Models (**TabPFN** and **TabICLv2**).

Instead of ad-hoc score intervals, the system introduces:

1. **Uncertainty Gate (U):** Conformal prediction sets providing strict, distribution-free error guarantees (1 - alpha).
2. **Attribution Gate (A):** Real-time monitoring of cumulative Shapley credit attributed to protected demographic features.
3. **Stability Gate (S, Novel Headline Contribution):** Auditing the semantic stability of adverse rejection reasons when the model's context set updates.

---

##  Interactive Google Colab Notebook

Execute the entire benchmark suite and experiment reproduction directly in your browser:

 [**Open Pipeline in Google Colab**](https://colab.research.google.com/drive/1qr8WRlTR41lFReRQOSqWJYLoH0aZcXvJ)

* **Hardware Recommendation:** Free NVIDIA T4 GPU runtime.
* **Pre-configured:** Automatically installs required dependencies (`tabpfn`, `tabicl`, `optuna`, `xgboost`, `lightgbm`, `catboost`) and runs experiments E1 through E6.
* You can also run the bundled repository notebook: [`run_on_colab.ipynb`](./run_on_colab.ipynb).

---

## System Architecture
<img width="1786" height="906" alt="image" src="https://github.com/user-attachments/assets/f88d7fad-92ee-4276-ad53-2f57fa5ee025" />

---

##  Experimental Results & Research Questions

### E1: Ranking Accuracy & Cost Minimization (RQ1)

* **TabPFN outperformed all baselines across 10/10 stratified splits**, achieving an average **ROC-AUC of 0.804 ± 0.035** (vs. Random Forest 0.790 ± 0.033, paired Wilcoxon p = 0.002).
* Expected misclassification loss per applicant dropped from 0.559 to 0.528 (approx. 6% cost reduction, p = 0.041).
* **TabICLv2** achieved competitive zero-shot parity with Optuna-tuned tree baselines (0.793 ± 0.034).

### E2 & E3: Conformal Deferral & Disparate Impact (RQ2)

* Conformal prediction strictly held its promised coverage guarantee (approx. 0.90 empirical coverage at alpha = 0.10).
* At equivalent human review workloads, conformal sets yield error curves equivalent to score windows; its primary value is a **statistically provable safety guarantee**.
* **Age Gap:** Applicants under 25 faced deferral rates nearly twice as high as older applicants (60.3% vs 30.0% for TabPFN; 95% bootstrap CI of gap: [0.25, 0.37]). Group-wise Mondrian conformal calibration equalized coverage rates across cohorts, but could not close the deferral gap.

### E4: Explanation Speed vs. Fidelity Trade-off (RQ3)

* Achieving >= 0.90 top-3 reason Jaccard agreement relative to an exact 2,048-evaluation reference required a budget of B = 128 **for TabICLv2** (38 s per single applicant) and B = 256 **for TabPFN** (236 s).
* When batching 100 applicants simultaneously, amortized latency dropped to 0.45 s **(TabICLv2)** and 2.4 s **(TabPFN)**, demonstrating that tabular foundation model explanations are best deployed in batch processing pipelines.

### E5: Explanation Instability Under Context Shifts (RQ4 — Headline Result)

* Because in-context foundation models formulate beliefs based on their exemplar tables, altering the context set alters the adverse reasons given to loan applicants.
* Under full bootstrap context resampling (600 rows), explanations shared only about two of three reasons (**mean Jaccard 0.49 - 0.52** vs. the 0.89 computational noise ceiling).
* **The #1 top rejection reason mutated for 51% - 62% of applicants**.
* Under a realistic 90% context retention update, the #1 reason still shifted for 38% - 41% **of applicants** (vs. 24% for retrained Random Forest).

### E6: Multi-Gate Full System Performance

* The optimal configuration selected on calibration data is **Uncertainty + Stability (U + S)**, minimizing decision loss to 0.514 on TabPFN (down from 0.570 with no gating).
* Applicants flagged by the **Stability Gate (S)** exhibited a default error rate of 53% when forced through automated decisions (vs. 38% - 41% for unflagged applicants), confirming that unstable explanations serve as a direct indicator of classification uncertainty.

---

##  Repository Structure

```
.
├── README.md                           # Project documentation and reproduction guide
├── run_on_colab.ipynb                  # Executable Jupyter/Colab experimental pipeline
├── Credit_TFM_Final (1).pptx           # Complete slide deck (Architecture, Maths, E1-E6)
└── Paper2_Credit_TFM_Final_Draft (1).docx # Comprehensive research manuscript draft
```

---

##  Regulatory Alignment

* **EU Artificial Intelligence Act (Regulation EU 2024/1689):**
  * **Annex III 5(b):** Governs credit scoring as a High-Risk AI use case.
  * **Article 13:** Mandates disclosure of technical boundaries and explanation stability limits.
  * **Article 14:** Tier 3 establishes auditable human-in-the-loop intervention protocols.
  * **Article 86:** Guarantees affected applicants a right to clear, verified adverse explanations.
* **Reserve Bank of India (RBI) FREE-AI Framework (August 2025):**
  * Implements transparent auditing, accountable oversight gates, and documented calibration thresholds for financial AI systems.

---

##  Citation

If you use this work, presentation material, or experimental results in your research, please cite:

```bibtex
@article{nivedithavenkatesh2026credittfm,
  author    = {Niveditha Venkatesh},
  title     = {Context-Stable Explanations and Conformal Deferral for Credit Decisions with Tabular Foundation Models},
  institution = {Vellore Institute of Technology (VIT), Chennai},
  year      = {2026} 
}
```
