# Execution Guide

## Full Reproduction (Run in Order)

```bash
# -------------------------------------------------
# 1. Create and activate virtual environment
# -------------------------------------------------
python -m venv .venv
source .venv/bin/activate        # macOS/Linux
# .venv\Scripts\activate         # Windows

# -------------------------------------------------
# 2. Install dependencies
# -------------------------------------------------
pip install -r requirements.txt

# -------------------------------------------------
# 3. Ensure AIDev v3 dataset is downloaded
#    (https://doi.org/10.5281/zenodo.16919272)
#    and placed under:
#
#    AIDev/
#      pull_request.parquet
#      pr_task_type.parquet
#      pr_commits.parquet
#      pr_commit_details.parquet
#      pr_reviews.parquet
#      pr_timeline.parquet
#      repository.parquet
#      user.parquet
#
#    Also ensure config.yaml contains:
#      data_dir: "AIDev"
#      db_path: "aid_dev.duckdb"
# -------------------------------------------------

# -------------------------------------------------
# 4. Run RQ1 (Integration Outcomes & Latency)
# -------------------------------------------------
python run_rq1.py --config config.yaml

# -------------------------------------------------
# 5. Build RQ2 feature set
# -------------------------------------------------
python build_rq2_features.py --config config.yaml

# -------------------------------------------------
# 6. Run RQ2 logistic regression models
# -------------------------------------------------
python run_rq2.py \
  --features out/rq2/rq2_features.csv \
  --out_dir out/rq2

# -------------------------------------------------
# 7. Generate qualitative sample (optional)
# -------------------------------------------------
python make_qual_sample.py --config config.yaml

# OR
python make_qual_sample_from_features.py --config config.yaml

# Prepare qualitative coding sheet
python make_rq2_qual_ready.py --config config.yaml

# -------------------------------------------------
# 8. Run additional statistical checks (optional)
# -------------------------------------------------
python run_stats.py --config config.yaml
