# When AI Teammates Meet Code Review  
## Collaboration Signals Shaping the Integration of Agent-Authored Pull Requests  

[![Conference](https://img.shields.io/badge/Conference-MSR%202026-blue)]
[![Status](https://img.shields.io/badge/Status-Accepted-yellow)]

📄 **Accepted at:**  
23rd International Conference on Mining Software Repositories (MSR 2026)  
Rio de Janeiro, Brazil  

**Status:** Accepted, to appear.

**Authors:**  
Costain Nachuma (Idaho State University)  
Minhaz F. Zibran (Idaho State University)

---

## 🧠 Research Context

Autonomous coding agents are increasingly submitting pull requests on GitHub.  
This study investigates how agent-authored pull requests integrate into human-driven review workflows.

Rather than treating agent contributions purely as code artifacts, we analyze their integration as a socio-technical process, examining:

- Integration outcomes (merge vs. non-merge)
- Decision latency
- Review-time collaboration signals
- Coordination stability
- Reviewer engagement dynamics

---

## 🔬 Research Questions

### RQ1  
What proportions of agent-authored PRs are merged, closed without merging, or remain open — and how long do they take to reach a decision?

### RQ2  
Which review-time collaboration signals are associated with successful integration?

Signals include:

- Iteration intensity  
- Change magnitude (ΔLOC, files)  
- Force pushes  
- Reviewer presence  
- Time to first review  
- Testing behavior  

---

## 🏗 Methodology Overview

- Dataset: **AIDev v3**
- 33,596 agent-authored PRs
- 2,807 repositories
- Logistic regression with repository-clustered standard errors
- Qualitative coding of 60 PRs
- DuckDB-based analytical pipeline

All models are associational, not causal.

---

## 📊 Data Source (AIDev v3)

Dataset DOI:  
https://doi.org/10.5281/zenodo.16919272  

Authors: Hao Li, Haoxiang Zhang, Ahmed E. Hassan  

The dataset is not redistributed with this repository.  
Download AIDev v3 and place the required Parquet files under the `AIDev/` directory.

---

## ⚙️ Reproducibility Instructions

### Requirements

- Python 3.10+
- DuckDB >= 0.10
- pandas
- numpy
- pyarrow
- statsmodels
- matplotlib

---

## 🚀 Execution Guide

### 1️⃣ Create Virtual Environment and Install Dependencies

```bash
python -m venv .venv
source .venv/bin/activate        # macOS/Linux
# .venv\Scripts\activate         # Windows
pip install -r requirements.txt
```

---

### 2️⃣ Download and Place AIDev v3 Dataset

Ensure the following directory structure exists:

```
AIDev/
  pull_request.parquet
  pr_task_type.parquet
  pr_commits.parquet
  pr_commit_details.parquet
  pr_reviews.parquet
  pr_timeline.parquet
  repository.parquet
  user.parquet
```

---

### 3️⃣ Configure Dataset Path

Ensure `config.yaml` contains:

```yaml
data_dir: "AIDev"
db_path: "aid_dev.duckdb"
```

---

### 4️⃣ Run RQ1: Integration Outcomes and Decision Latency

```bash
python src/run_rq1.py --config config.yaml
```

Outputs:

```
out/rq1/
```

---

### 5️⃣ Build RQ2 Feature Set

```bash
python src/build_rq2_features.py --config config.yaml
```

Output:

```
out/rq2/rq2_features.csv
```

---

### 6️⃣ Run RQ2 Logistic Regression Models

```bash
python src/run_rq2.py \
  --features out/rq2/rq2_features.csv \
  --out_dir out/rq2
```

Outputs:

```
out/rq2/
  rq2_logit_logcommits_cluster.csv
  rq2_logit_bucket_cluster.csv
  rq2_modelA_summary.txt
  rq2_modelB_summary.txt
```

---

### 7️⃣ Qualitative Analysis (Table 1)

Run:

```bash
python src/make_qual_sample.py --config config.yaml
python src/make_qual_sample_from_features.py --config config.yaml
python src/make_rq2_qual_ready.py --config config.yaml
```

Generated files:

```
rq2_qual_ready.csv
coding_sheet_60Classified.csv
```

---

### 8️⃣ Additional Statistical Checks (Optional)

```bash
python src/run_stats.py --config config.yaml
```

---

## 📦 Sample Output

For quick inspection without running the full pipeline:

```
sample_output.zip
```

---

## 📈 Reproducibility Notes

- Analyses are deterministic given identical input data.
- Random seeds are fixed.
- Results are associational, not causal.
- Post-merge signals (e.g., reverts) are intentionally excluded.

---

## 📚 Citation

If you use this artifact, please cite:

Nachuma, C., Zibran, M.F.  
*When AI Teammates Meet Code Review: Collaboration Signals Shaping the Integration of Agent-Authored Pull Requests.*  
MSR 2026 (Accepted, to appear).

---

## 👤 Contact

Costain Nachuma  
PhD Candidate, Idaho State University  
Email: costainnachuma@isu.edu  
GitHub: https://github.com/costain
