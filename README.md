# tennis-ranking-predictor

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jackhenry02/tennis-ranking-predictor/blob/main/notebooks/tennis_ranking_forecast.ipynb)

Forecast ATP player rankings one month ahead with single-layer and stacked LSTM models trained on Jeff Sackmann's tennis data. Lightweight CSV samples follow the real column names (`ranking_date`, `rank`, `player`, `points` for rankings and the ATP match schema), in order to check code runs. Notebook now defaults to using the real Sackmann repository (auto-cloned to `data/tennis_atp_raw`). By default the workflow keeps only seasons from 2000 onward, fits on <2024 data, and scores the models on the 2024 ranking window.
- The pipeline filters and scales ranking + match aggregates, builds sliding supervision windows, and trains both per-player and global recurrent models.
- Sequence construction uses 8-week histories with a 1-step (~4 week) horizon, across the top 25 players with the richest records in the filtered dataset.
- Outcomes are reported in raw ranking points so the MAE numbers are directly comparable with official ATP standings.

## 2024 evaluation summary
- Single-layer global LSTM delivered the lowest mean absolute error on the 2024 hold-out set (`mae_rank_points ≈ 0.0059`), edging out the stacked variant (`≈ 0.0087`).
- Per-player runs show stacked LSTM narrowly ahead on average MAE (`≈ 0.0248` vs `≈ 0.0261`), though the single-layer model wins outright for several athletes (e.g. Nadal, Gasquet, Monfils).
- The final plot in the notebook inverts the y-axis (lower ranks are better) and overlays true vs predicted weekly rankings for a representative player so you can visually inspect forecast tracking.


## Notebook overview

- Initialization cell where you can toggle `use_sample_data` (default `False` for the full dataset), `match_year_start`, and the hold-out year (default 2024).
- Automated cloning of [Jeff Sackmann's dataset](https://github.com/JeffSackmann/tennis_atp) when `use_sample_data=False`, pulling `atp_rankings_00s/10s/20s/current.csv` plus yearly match files only for the requested year range (2000+ by default).
- Feature prep with `pandas` + `MinMaxScaler`, including rolling rank trends, velocity, and match-derived form features aggregated per player/month.
- Training + evaluation of both single-layer and stacked LSTMs for:
  - **Player-specific** models (one model per athlete) as long as the player has enough history.
  - **Global** models spanning up to 25 of the most documented players (configurable).
- 2024 rankings act as the explicit test window (training uses <2024), and the notebook plots sample predictions vs. truth with the “Reading The Results” markdown cell explaining the tables and chart.

## Local usage

1. Create and activate a Python 3.10+ environment with PyTorch, pandas, numpy, scikit-learn, and matplotlib (see `requirements.txt`).
2. Open `notebooks/tennis_ranking_forecast.ipynb` in VS Code, Jupyter Lab, or Colab.
3. Keep `use_sample_data = True` for the embedded toy data, or flip it to `False` to clone the full ATP repository the first time the cell runs.
4. Adjust `match_year_start`, `selected_player_limit`, or `test_year` if you want different training spans.
5. Execute the remaining cells to train the models and review the comparison tables + plots.

## Colab workflow

1. Click the **Open in Colab** badge above.
2. In Colab, run `!pip install -r requirements.txt` (or the equivalent `pip install torch pandas numpy scikit-learn matplotlib`).
3. Execute the initialization cell and toggle `use_sample_data` / `test_year` as desired.
4. Continue through the notebook to reproduce the results.


Enjoy!
