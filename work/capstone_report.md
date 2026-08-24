# Capstone Report — Search Intelligence & Content Refresh Lane

- **Author:** Alvin George
- **Lane:** Refresh / Content Opportunity Scoring
- **Repo:** https://github.com/AlvinGeorge-AG/flyrank-ML-internship
- **Date:** 2026-08-24

## 1. Problem framing
Over time, web pages lose search rankings and organic traffic decays. Because a client portfolio can contain thousands of pages, manual review of every page is impossible. This project builds a decision-support machine learning model that ranks declining pages, producing a prioritized action queue for human content editors to refresh high-value content.

## 2. Data safety
Data was queried from the FlyRank Search Intelligence Warehouse (`fact_content_daily_performance`). All client domain names, raw URLs, and search queries were anonymized into hashes (`client_hash_id`, `content_hash_id`). Label-derived trend fields were strictly excluded from feature inputs to prevent data leakage.

## 3. Baseline
A deterministic hand-written rule baseline was built using past 90-day impressions and rank position. On unseen test data, the human baseline rule achieved a Precision@50 score of **0.320** and Precision@20 of **0.550**.

## 4. Model / analysis
I evaluated Logistic Regression and Random Forest models. Features included `impressions_90d`, `clicks_90d`, `avg_position`, `sessions_90d`, and `ctr`. The target label is binary page decline (`is_declining_label`).

## 5. Evaluation
Data was split using a client-grouped design (`GroupShuffleSplit`) holding out 20% of clients. The learned Random Forest model achieved a **Precision@50 of 0.580** (and **Precision@20 of 0.650**), representing a **~1.81× lift** over the human baseline rule (0.320).

## 6. Interpretation
Feature importance analysis revealed that `impressions_90d` and `avg_position` are the primary drivers of decline risk predictions. High-visibility Page 1 assets carry the highest priority when traffic drops occur.

## 7. Recommendation
Outputs are mapped into a Content Action Playbook with reason codes (`page_one_decay_risk`, `stale_visible`) and prescribed actions (`refresh_stats_and_links`, `expand_content`). New pages (<30 days) and legal disclaimers are placed on a strict No-Go list.

## 8. Reproducibility
* **Run command:** `python scripts/run_all.py`
* **Master Notebook:** `work/notebooks/capstone.ipynb`
* **Data Source:** Built on the FlyRank ML Internship dataset.

---

## Showcase Presentation & Shareable Cuts

### 5-Minute Demo Outline
1. **The Problem:** Web pages decay in Google traffic; manual rules flag noise.
2. **Data & Safety:** 79M+ warehouse records queried via DuckDB with client-grouped splits.
3. **The Baseline vs ML:** Human rule baseline (Precision@50 = 0.320) vs Random Forest.
4. **Key Result:** Random Forest achieved **0.580 Precision@50** (~1.81× lift over baseline).
5. **Recommendation:** Decision-support playbook queue exported for editor workflows.

### Shareable Cuts
* **Technical Post:** Built an end-to-end search intelligence model on 79M+ daily records using DuckDB and Random Forests. Client-grouped splits verified a 1.81× lift in Precision@50 over rule baselines without leakage.
* **Employer Summary:** Built a machine learning content refresh queue on 79M+ search records. Achieved a 1.81× Precision@50 lift over human baseline rules on unseen client portfolios.
