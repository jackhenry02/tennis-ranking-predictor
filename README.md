# tennis-ranking-predictor

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jackhenry02/tennis-ranking-predictor/blob/main/notebooks/tennis_ranking_forecast.ipynb)

Forecast ATP player rankings one month ahead with single-layer and stacked LSTM models trained on Jeff Sackmann's tennis data. Lightweight CSV samples follow the real column names (`ranking_date`, `rank`, `player`, `points` for rankings and the ATP match schema), but the notebook now defaults to using the real Sackmann repository (auto-cloned to `data/tennis_atp_raw`). By default the workflow keeps only seasons from 2000 onward, fits on <2024 data, and scores the models on the 2024 ranking window.

## Project layout

```
project-root/
├── data/
│   ├── sample_matches.csv
│   ├── sample_rankings.csv
│   └── tennis_atp_raw/           # populated when cloning Jeff Sackmann's repo
├── notebooks/
│   └── tennis_ranking_forecast.ipynb
├── requirements.txt
└── README.md
```

## Notebook highlights

- Personal, narrative introduction plus an initialization cell where you can toggle `use_sample_data` (default `False` for the full dataset), `match_year_start`, and the hold-out year (default 2024).
- Automated cloning of [Jeff Sackmann's dataset](https://github.com/JeffSackmann/tennis_atp) when `use_sample_data=False`, pulling `atp_rankings_00s/10s/20s/current.csv` plus yearly match files only for the requested year range (2000+ by default).
- Feature prep with `pandas` + `MinMaxScaler`, including rolling rank trends, velocity, and match-derived form features aggregated per player/month.
- Training + evaluation of both single-layer and stacked LSTMs for:
  - **Player-specific** models (one model per athlete) as long as the player has enough history.
  - **Global** models spanning up to 25 of the most documented players (configurable).
- 2024 rankings act as the explicit test window (training uses <2024), and the notebook plots sample predictions vs. truth.

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

## Packaging

Zip up the folder for sharing or upload directly to GitHub/Colab:

```bash
zip -r tennis_ranking_forecast_project.zip data notebooks README.md requirements.txt setup.sh
```

Once you push to GitHub, update the Colab badge link to point to your repository path.
