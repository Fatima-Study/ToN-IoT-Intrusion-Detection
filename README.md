<h1 align="center">TC-317 · Artificial Intelligence & Machine Learning · CEP & OEL</h1>
<p align="center">
 Department of Telecommunication Engineering, NED University of Engineering & Technology, Karachi


<h1 align="center">🛡️ Network Intrusion Detection on ToN-IoT</h1>
<h3 align="center">5 Features · 97.6 % Accuracy · Explainable ML & DL</h3>


<p align="center">
  <img src="https://img.shields.io/badge/Python-3.x-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/Project-NIDS-2ea44f?style=for-the-badge">
  <img src="https://img.shields.io/badge/Dataset-ToN--IoT-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/Focus-IoT%20Security-red?style=for-the-badge">
  <img src="https://img.shields.io/badge/Models-7%20ML%20%2B%202%20DL-8A2BE2?style=for-the-badge">
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge">
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge">
  <img src="https://img.shields.io/badge/Course-TC--317%20AI%20%26%20ML-1F3864?style=for-the-badge">
</p>

> ## Machine Learning and Deep Learning based Network Intrusion Detection on the ToN-IoT dataset using only **five flow features**, with an emulated smart-home monitoring architecture in Cisco Packet Tracer.

---

<a id="table-of-contents"></a>
## 📑 Table of Contents

- [📌 Overview](#overview)
- [🎯 Objective](#objective)
- [🌐 Dataset](#dataset)
- [🏆 Key Results](#key-results)
- [🧬 Selected Features](#selected-features)
- [🛠️ Tools and Technologies](#tools-and-technologies)
- [🔬 Methodology](#methodology)
- [🏠 Emulated Architecture](#emulated-architecture)
- [📂 Project Structure](#project-structure)
- [📊 Results](#results)
- [💡 Key Insights](#key-insights)
- [🚀 How to Run This Project](#how-to-run-this-project)
- [⚖️ Ethical Considerations](#ethical-considerations)
- [⚠️ Limitations](#limitations)
- [🔭 Future Work](#future-work)
- [✅ Conclusion](#conclusion)
- [👥 Team](#team)
- [👩‍💻 Author & Contact](#author--contact)

---

<a id="overview"></a>
## 📌 Overview

IoT devices are everywhere, and so are the attacks on them. A good intrusion detector must be **accurate** and **lightweight**.

This project asks one question: **how small can the input of a detector be?** From the **38 usable features** of the ToN-IoT network dataset (**211,043 flows, 10 classes**), only **five** were selected by sequential forward selection. Seven classical ML models and two deep learning models were then compared on ten-class and binary (normal / attack) detection.

The detector is placed in an emulated smart-home network where a switch **mirrors traffic (SPAN)** to a dedicated IDS server, so inference never slows the live network.

---

<a id="objective"></a>
## 🎯 Objective

- Model a smart-home IoT edge network where an adversary can launch a ping flood
- Monitor it out-of-band through SPAN port mirroring
- Build a reproducible pipeline for ten-class and binary detection
- Compare a forward-selected five-feature set with a reference five-feature set
- Explain model decisions and find the classes that remain difficult

---

<a id="dataset"></a>
## 🌐 Dataset

| Item | Detail |
|---|---|
| Name | ToN-IoT network dataset (UNSW Canberra) |
| File | `train_test_network.csv` |
| Flows | 211,043 |
| Columns | 44 (38 usable features after removing IDs and ports) |
| Classes | normal, backdoor, ddos, dos, injection, password, ransomware, scanning, xss, mitm |
| Split | Stratified 80/20 (168,834 train / 42,209 test, seed 42) |


> 📦 The dataset is Download it from the official site that is Kaggle and search this word : ToNIoT then you will see TON_IoT Network Dataset of UNSW Canberra

---

<a id="key-results"></a>
## 🏆 Key Results

| 🎯 Metric | Result |
|---|---|
| Ten-class accuracy | **97.61 %** (Random Forest, macro-F1 0.9479) |
| Binary accuracy | **99.71 %** (Bagging) |
| Features | **38 → 5**, only 0.22 points lost vs all features |
| Fastest model | Decision Tree, **1.10 ms** per flow |
| Feature selection time | Forward **0.13 min**, backward **1.62 min** (Colab) |
| Hardest class | `mitm` (1,043 flows), F1 ≈ 0.70 |

---

<a id="selected-features"></a>
## 🧬 Selected Features

| Feature | Meaning |
|---|---|
| `src_bytes` | Payload bytes sent by the originator |
| `conn_state` | Connection-state code of the flow (e.g. S0, SF) |
| `src_ip_bytes` | IP-packet bytes sent by the originator ⭐ most influential |
| `dst_ip_bytes` | IP-packet bytes sent by the responder |
| `dns_RD` | DNS recursion-desired flag |

---

<a id="tools-and-technologies"></a>
## 🛠️ Tools and Technologies

| Tool / Technology | Purpose |
|---|---|
| Python | Development of the full pipeline |
| pandas / NumPy | Data loading and preprocessing |
| scikit-learn | Decision Tree, KNN, Random Forest, Gradient Boosting, Bagging, Voting, SFS |
| XGBoost | Gradient boosting model |
| TensorFlow / Keras | Dense network and 1D CNN |
| SHAP | Explainable AI |
| Matplotlib | Graphs and figures |
| Cisco Packet Tracer | Emulated smart-home network and SPAN monitoring |
| Google Colab | Experiments and timing |

---

<a id="methodology"></a>
## 🔬 Methodology

The NIDS follows this workflow:

```
ToN-IoT Network Dataset (211,043 flows | 44 columns)
                  │
                  ▼
Data Pre-processing (drop IDs, encode, log1p, clean)
                  │
                  ▼
Stratified 80/20 Split (168,834 train | 42,209 test)
                  │
                  ▼
Forward and Backward SFS → Top 5 Features + Time Analysis
                  │
                  ▼
Train 7 ML + 2 DL Models (early stopping, epochs up to 80)
                  │
        ┌─────────┼───────────────┐
        ▼         ▼               ▼
  Explainability  Multiclass vs   Diagnostics
  (Impurity,      Binary          (Confusion Matrix,
  Permutation,    (10-class vs    Latency,
  SHAP)           2-class)        Throughput)
        │         │               │
        └─────────┼───────────────┘
                  ▼
Final Benchmark: Paper vs Proposed
(Accuracy, Precision, Recall, F1, Kappa)
```

### Process

1. Remove identifier columns (`src_ip`, `dst_ip`, `src_port`, `dst_port`) and encode text columns.
2. Split the data 80/20, stratified on the ten-class label.
3. Run forward and backward sequential selection with a Decision Tree on a 15,000-row sample.
4. Train 7 ML models and 2 DL models (Dense network, 1D CNN) on the five features.
5. Evaluate with accuracy, precision, recall, macro-F1 and Cohen's kappa.
6. Explain the models with impurity importance, permutation importance and SHAP.
7. Benchmark against the published reference study.

**Models:** Decision Tree · KNN · Random Forest · Gradient Boosting · XGBoost · Bagging · Voting · Dense NN · 1D CNN

---

<a id="emulated-architecture"></a>
## 🏠 Emulated Architecture

```
   IoT edge: Light · Webcam · Motion Detector
                      │ (wireless)
                      ▼
              Home Gateway (DLC100)
                      │ Fa0/1
                      ▼
ATTACKER_PC ─ Fa0/2 ─▶ Switch0 (2960) ─ Gig0/1 ─▶ Router0 (2911)
VICTIM_PC   ─ Fa0/3 ─▶     │
                           │ SPAN session 1 (Fa0/1 + Fa0/2, both directions)
                           ▼
                        Fa0/24 ─▶ IDS_Server (192.168.1.99)
```

> ℹ️ Packet Tracer validates the **placement and traffic path** only. The trained classifier is not connected to live traffic in the simulation.

---

<a id="project-structure"></a>
## 📂 Project Structure

```
NIDS-ToN-IoT/
│
├── README.md
├── requirement.txt
│
├── report/
│   ├── NIDS_ToN-IoT_Report_Final.pdf
│   └── NIDS_ToN-IoT_Report_Final.docx
│
├── notebooks/
│   └── CEP_OEL_AIML_ToNIoT.ipynb
│
├── Images/
│   ├── pipeline.png
│   ├── all_samples.png
|
├── simulation/
│   ├── cisco_packet_tracer.png
|
├── features_importance/
│   ├── feature_selection.png
│   ├── feature_importance.png
│   ├── permutation_importance.png
│   ├── SHAP_plot.png
│   ├── SHAP_plot_mitm
|
├── Output_compare/
|   ├── ML_vs_DL.png
|   ├── comparison.png
|   ├── accuracy_vs_loss.png
|   ├── multiclass_vs_binary.png
|   ├── time_taken_compariosn.png
│   ├── confusion_matrix.png
│   └── latency_throughput.png
│
└── data/            (not uploaded, add train_test_network.csv locally)
```

---

<a id="results"></a>
## 📊 Results

### Paper vs Ours (ten-class)

| Model | Accuracy (Paper → Ours) | F1 (Paper → Ours) |
|---|---|---|
| Random Forest | 0.9734 → **0.9761** | 0.9292 → **0.9479** |
| Voting | 0.9721 → **0.9757** | 0.9279 → **0.9471** |
| Bagging | 0.9729 → **0.9753** | 0.9245 → **0.9458** |
| XGBoost | 0.9689 → **0.9749** | 0.9187 → **0.9445** |
| Decision Tree | 0.9720 → **0.9748** | 0.9189 → **0.9450** |
| Gradient Boosting | 0.9677 → **0.9683** | 0.9071 → **0.9271** |
| KNN | 0.9720 → 0.9678 | 0.9150 → **0.9333** |

---

<a id="key-insights"></a>
## 💡 Key Insights

- Five well-chosen features are enough for strong detection, with only a small loss versus 38 features.
- Tree ensembles matched or beat deep learning on this tabular data.
- `src_ip_bytes`, `conn_state` and `dst_ip_bytes` drive most decisions (SHAP and permutation importance).
- Preprocessing mattered far more than network depth for the DL models.
- The rare `mitm` class is the main source of errors.

---

<a id="how-to-run-this-project"></a>
## 🚀 How to Run This Project

### 1. Clone the Repository

```bash
git clone https://github.com/Fatima-Study/<your-repo-name>.git
```

### 2. Open the Project Folder

```bash
cd <your-repo-name>
```

### 3. Install Required Libraries

```bash
pip install -r requirements.txt
```

### 4. Add the Dataset

Download `train_test_network.csv` (ToN-IoT, UNSW Canberra) and place it in a `data/` folder.

### 5. Run the Notebooks

Open the files in `notebooks/` with Jupyter or Google Colab and run them in order.

---

<a id="ethical-considerations"></a>
## ⚖️ Ethical Considerations

- Only a public benchmark dataset was used.
- The attack scenario ran only inside a Packet Tracer simulation.
- No real user, company or unauthorized system was targeted.

---

<a id="limitations"></a>
## ⚠️ Limitations

1. Single stratified 80/20 split with a fixed seed; session-level independence was not verified.
2. Feature selection used a 15,000-row sample.
3. No extensive hyperparameter tuning.
4. Packet Tracer timing and loss figures show relative trends only.
5. The classifier is not connected to live traffic.

---

<a id="future-work"></a>
## 🔭 Future Work

- Repeated cross-validation
- SMOTE or cost-sensitive learning for `mitm`
- Additional informative features
- Live inference on mirrored traffic with a higher-rate traffic generator
- Evaluation on other intrusion datasets

---

<a id="conclusion"></a>
## ✅ Conclusion

Five carefully chosen flow features reached **97.61 %** ten-class and **99.71 %** binary accuracy on ToN-IoT, improved on the published reference results for nearly all models and metrics, and kept the detector small and explainable. The SPAN-based architecture shows how such a compact detector can sit on a separate server without loading the switch or router.

---

<a id="team"></a>
## 👥 Team

| Name |
|---|
| Fatima Taufique |
| Aleena Faiz |
| Shehzeen Rizwan |
| Areeba Zehra |

---

<a id="author--contact"></a>
## 👩‍💻 Author & Contact

<p align="center">
  <img src="https://github.com/Fatima-Study.png" width="120" alt="Fatima">
</p>

<p align="center">
  <strong>Fatima</strong><br>
  Artificial Intelligence & Machine Learning | CEP & OEL | Project
</p>

<p align="center">
  <a href="https://github.com/Fatima-Study">GitHub Profile</a> •
  <a href="https://www.linkedin.com/in/fatima-taufique-1313b633b/">LinkedIn</a>
</p>
