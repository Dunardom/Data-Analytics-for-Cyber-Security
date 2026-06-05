# Data Analytics for Cyber Security — UNSW-NB15

![Python](https://img.shields.io/badge/Python-3.8%2B-3776AB?logo=python&logoColor=white)
![Made with Jupyter](https://img.shields.io/badge/Made%20with-Jupyter-F37626?logo=jupyter&logoColor=white)
![Dataset](https://img.shields.io/badge/Dataset-UNSW--NB15-orange)

A cybersecurity **data-analytics pipeline** built on the **UNSW-NB15** network
traffic dataset. The project loads and explores the data, detects anomalies
(Z-Score and Isolation Forest), trains supervised classifiers (Random Forest
and Decision Tree) to separate normal traffic from attacks, and presents the
findings in a security dashboard. Developed for the MIT516 unit.

> **This guide explains how to run the project on your own machine.** The
> dataset CSVs are **not** stored in the repository (they exceed GitHub's
> 100 MB per-file limit), so the steps below include how to download and place
> them.

---

## What's in this repository

```
Data-Analytics-for-Cyber-Security/
├── README.md                          # this file
├── MIT516_D_Notebook.ipynb            # main analysis pipeline
├── MIT516_D_Presentation.ipynb        # presentation / summary notebook
├── The UNSW-NB15 description.pdf       # official dataset documentation
├── UNSW-NB15_features.csv              # 49-feature data dictionary (kept in repo)
│
│   ── the files below are NOT in the repo; download them (see step 3) ──
├── UNSW-NB15_1.csv  …  UNSW-NB15_4.csv # full raw capture (~2.54M rows total)
├── UNSW-NB15_GT.csv                    # ground-truth attack records
├── UNSW-NB15_LIST_EVENTS.csv           # list of attack events
└── Training and Testing Sets/
    ├── UNSW_NB15_training-set.csv      # ~175,341 rows (used by the notebook)
    └── UNSW_NB15_testing-set.csv       # ~82,332 rows  (used by the notebook)
```

The notebook works mainly from the **`Training and Testing Sets/`** split — the
standard machine-learning partition. The raw `UNSW-NB15_1…4.csv`, `GT`, and
`LIST_EVENTS` files are the full capture and are optional unless you re-run the
raw-data steps. `UNSW-NB15_features.csv` is the small column dictionary and is
the only CSV tracked in the repo.

---

## Prerequisites

- **Python 3.8 or newer** — check with `python --version`
- **Git** — to clone the repository
- **~1–2 GB free disk space** for the dataset
- A browser (Jupyter runs in the browser)

---

## Getting started (local setup)

### 1. Clone the repository

```bash
git clone https://github.com/Dunardom/Data-Analytics-for-Cyber-Security.git
cd Data-Analytics-for-Cyber-Security
```

### 2. Create a virtual environment and install dependencies

**Windows (PowerShell):**

```powershell
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

<details>
<summary>No <code>requirements.txt</code> yet? Install the packages directly:</summary>

```bash
pip install pandas numpy scipy matplotlib seaborn scikit-learn imbalanced-learn jupyter notebook
```
</details>

### 3. Download the dataset

The UNSW-NB15 CSVs are not included in this repository. Download them and place
them so your folder matches the **"What's in this repository"** layout above.

1. Open the official UNSW page and follow the **Download → CSV Files** link:
   **https://research.unsw.edu.au/projects/unsw-nb15-dataset**
   *(Alternative mirror: the Kaggle dataset `mrwellsdavid/unsw-nb15`.)*
2. Place the files like this:
   - **into the repo root:** `UNSW-NB15_1.csv` … `UNSW-NB15_4.csv`,
     `UNSW-NB15_GT.csv`, `UNSW-NB15_LIST_EVENTS.csv`
   - **into `Training and Testing Sets/`:** `UNSW_NB15_training-set.csv` and
     `UNSW_NB15_testing-set.csv`

> The old AARNet **Cloudstor** download links have been decommissioned — ignore
> any tutorial pointing at `cloudstor.aarnet.edu.au` and use the official UNSW
> page above.

### 4. Launch Jupyter

```bash
jupyter notebook
```

Your browser opens the Jupyter file list. Open a notebook (see below) and run
all cells with **Cell → Run All**.

---

## Running the notebooks

| Notebook | Purpose | How to run |
|----------|---------|------------|
| **`MIT516_D_Notebook.ipynb`** | The full pipeline: data loading, EDA, preprocessing, anomaly detection, ML classification, and the dashboard. | Open it, then **Cell → Run All** (top to bottom). |
| **`MIT516_D_Presentation.ipynb`** | A condensed presentation/summary of the results. | Run it **after** the main notebook so the data and figures are available. |

**Run from the repository root.** The notebooks use relative paths such as
`Training and Testing Sets/UNSW_NB15_training-set.csv`, which only resolve when
Jupyter is started from inside the project folder.

**If a notebook hard-codes an absolute path** (e.g. `D:\Assessment\...`), change
it to a relative path so the project runs on any machine:

```python
from pathlib import Path

DATA_DIR  = Path("Training and Testing Sets")
TRAIN_CSV = DATA_DIR / "UNSW_NB15_training-set.csv"
TEST_CSV  = DATA_DIR / "UNSW_NB15_testing-set.csv"
```

---

## The pipeline at a glance

```
Data Loading → EDA → Preprocessing → Anomaly Detection → ML Classification → Dashboard
```

1. **Data loading** — read the UNSW-NB15 train/test CSVs into pandas.
2. **EDA** — feature distributions, attack-category counts, class imbalance.
3. **Preprocessing** — encode categoricals, scale numerics, rebalance classes
   (`imbalanced-learn`).
4. **Anomaly detection** — Z-Score (statistical) and Isolation Forest
   (unsupervised ML).
5. **Classification** — Random Forest and Decision Tree (normal vs attack),
   evaluated with precision/recall/F1.
6. **Dashboard** — a consolidated security visualisation of the results.

---

## About the dataset

UNSW-NB15 was generated in the Cyber Range Lab of the Australian Centre for
Cyber Security (ACCS), UNSW Canberra. It has **49 features** plus two labels
(`attack_cat` and `label`) and **nine attack categories** — Fuzzers, Analysis,
Backdoors, DoS, Exploits, Generic, Reconnaissance, Shellcode, Worms — alongside
normal traffic. Full details are in **`The UNSW-NB15 description.pdf`** and the
column dictionary **`UNSW-NB15_features.csv`** included in this repository.

If you use this dataset, please cite both source papers:

- Moustafa, N., & Slay, J. (2015). *UNSW-NB15: a comprehensive data set for
  network intrusion detection systems (UNSW-NB15 network data set).* MilCIS 2015,
  IEEE. https://doi.org/10.1109/MilCIS.2015.7348942
- Moustafa, N., & Slay, J. (2016). *The evaluation of Network Anomaly Detection
  Systems: Statistical analysis of the UNSW-NB15 data set and the comparison
  with the KDD99 data set.* Information Security Journal: A Global Perspective,
  25(1–3), 18–31. https://doi.org/10.1080/19393555.2015.1125974

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `FileNotFoundError` / "No such file" when loading a CSV | The dataset isn't downloaded or is in the wrong folder — recheck **step 3** and the expected layout. |
| `ModuleNotFoundError: No module named '…'` | Your virtual environment isn't active or deps aren't installed. Re-activate (`.venv\Scripts\activate`) and run `pip install -r requirements.txt`. |
| `MemoryError` loading `UNSW-NB15_1…4.csv` | Use the smaller `Training and Testing Sets/` split instead of the full raw files. |
| Notebook won't preview on GitHub ("something went wrong") | Paste the notebook's URL into **https://nbviewer.org**. |
| `LF will be replaced by CRLF` warnings on Windows | Harmless line-ending normalisation — safe to ignore. |

---

## Licence

- **Code & notebooks:** intended under the **MIT License** — add a `LICENSE`
  file to the repo if you haven't already.
- **UNSW-NB15 dataset:** **not** covered by that licence. It is free for
  **academic research use only** (commercial use prohibited) and must be cited
  as above. The dataset is not redistributed in this repository.

---

## Author

- Om Prakash Kandel — MIT516, NAPS
- GitHub: [@Dunardom](https://github.com/Dunardom)
