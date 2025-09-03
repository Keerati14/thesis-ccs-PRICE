# PRICE — Policy Revenue for Industrial CCS Enablement (Thesis Code)

This repository hosts the code and notebooks used in my MSc Environmental Technology thesis to identify **policy price levels** (a baseline **carbon tax** plus an **ETS coverage‑proxy**) that make **industrial CCS** projects **feasible** in ASEAN heavy‑industry sectors. Key outputs include feasibility frontiers and robustness heatmaps, expressed in **constant 2024 USD/tCO₂**.

**Repository:** https://github.com/Keerati14/thesis-ccs-PRICE

---

## What’s inside

- **`(Scenarios)Optimisation_model_pyomo_Gurobi.ipynb`** — main Jupyter notebook to run scenario optimisation and reproduce figures (frontier & robustness heatmap).
- `environment-gurobi.yml` — Conda environment (Python 3.11) with **Pyomo**, **Gurobi** (licensed), and open‑source solvers (**HiGHS**, **GLPK**).
- (Optional) `data/` — put large/original datasets in `data/raw/` (gitignored). Provide minimal samples in `data/processed/`.
- (Optional) `figures/`, `results/` — auto‑saved plots and CSV/Parquet outputs.

> If you don’t use Gurobi, you can still run with **HiGHS** or **GLPK** (see “Choose a solver” below).

---

## Quick start (Conda)

> Works on Windows/macOS/Linux. The steps below assume you’ve installed **Anaconda/Miniconda** and can open an **Anaconda Prompt** (Windows) or **Terminal** (macOS/Linux).

### 1) Clone the repo
```bash
git clone https://github.com/Keerati14/thesis-ccs-PRICE.git
cd thesis-ccs-PRICE
```

### 2) Create and activate the environment
```bash
conda env create -f environment-gurobi.yml
conda activate price-gurobi
```

### 3) Register a Jupyter kernel (once)
```bash
python -m ipykernel install --user --name price-gurobi --display-name "PRICE (py311 Gurobi)"
```

### 4) Launch JupyterLab
```bash
jupyter lab
```
Open **`(Scenarios)Optimisation_model_pyomo_Gurobi.ipynb`** and select the **“PRICE (py311 Gurobi)”** kernel.

---

## Choose a solver (Gurobi / HiGHS / GLPK)

In the notebook, near the top where the solver is selected, use one of:

```python
from pyomo.opt import SolverFactory

# Preferred if you have a Gurobi academic license
solver = SolverFactory("gurobi")

# Open‑source alternatives (no license required)
# solver = SolverFactory("highs")       # HiGHS
# solver = SolverFactory("appsi_highs") # Pyomo Appsi interface to HiGHS
# solver = SolverFactory("glpk")        # GLPK
```

If you choose Gurobi, ensure your license is active (run `grbgetkey <YOUR-KEY>` once and follow prompts).

---

## How to use the notebook

1. **Configure paths and inputs**  
   - If the notebook reads an Excel/CSV with global carbon price stats (for IQR/medians overlays), update the path variable at the top of the notebook.
   - Place large/original data under `data/raw/` and small reproducible samples under `data/processed/`.

2. **Run scenarios**  
   Execute the notebook cells top‑to‑bottom. The model will sweep policy prices and capture parameters to mark **feasible** vs **infeasible** combinations.

3. **Reproduce key figures**
   - **Fig. 1 — Feasibility frontier (annotated):** minimum ETS (coverage proxy) required at each Tax on baseline; shaded feasible region; anchors at **Tax‑only ≈ 50**, **ETS‑only ≈ 80**, **balanced ≈ 30/30**; overlays global price **IQR bands** (semi‑transparent) and **medians** (dashed). Axes 0–100 USD/tCO₂.
   - **Fig. 5 — Robustness heatmap:** binary feasibility across **capture fraction f (0–1)** and **capture cost (USD/tCO₂)** at **emission cap = 0.40** (priced share = 0.60). Safe band rectangle: **f≈0.6–0.7, c₍cap₎ ≤ 60**. Grey cells include “no solution within bounds” (administrative, not physical).

4. **Outputs**  
   Figures and tables are saved under `figures/` and `results/` if those paths are configured in the notebook; otherwise use “Save As” from the plot outputs.

---

## Data & units

- **Units:** prices in **constant 2024 USD/tCO₂**. Axes default to **0–100** for Tax/ETS unless otherwise noted; capture cost commonly **60–120**.
- **Data policy:** do **not** commit large or licensed data. Use `data/README.md` to point to official sources and include checksums. Provide **small sample inputs** in `data/processed/` so the notebook runs end‑to‑end.

---

## Troubleshooting

- **Environment not found:** you may have activated the wrong name. Use `conda info --envs` and then `conda activate price-gurobi`.
- **Gurobi unavailable:** activate your license (`grbgetkey …`) or switch to `highs` / `appsi_highs`.
- **No solution / grey grids:** often indicates administrative bounds (time limit, tolerance). Try increasing time limits or widening the policy grid.
- **IQR overlays missing:** check the path and column names in the price stats spreadsheet loaded by the notebook.

---

## How to cite

If you use this repository or a tagged release, please cite:

**Chukorn, K. (2025). PRICE — Policy Revenue for Industrial CCS Enablement.** GitHub.  
Repository: https://github.com/Keerati14/thesis-ccs-PRICE

(Optionally cite a specific release tag or DOI if minted via Zenodo.)

---

## License

- **Code:** MIT License  
- **Text & figures in this repo:** CC BY 4.0 unless otherwise noted  
- **Third‑party data:** subject to their original licences/terms
