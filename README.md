# tennis-ranking-predictor

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jackhenry02/tennis-ranking-predictor/blob/main/notebooks/tennis_ranking_forecast.ipynb)

Forecast ATP player rankings one month ahead with single-layer and stacked LSTM models trained on Jeff Sackmann's tennis data. Lightweight CSV samples follow the real column names (`ranking_date`, `rank`, `player`, `points` for rankings and the ATP match schema), so the notebook behaves the same whether you use the sample bundle or clone the full repository.

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

- Personal, narrative introduction plus an initialization cell where you can toggle `use_sample_data` and choose the hold-out year (default 2023).
- Automated cloning of [Jeff Sackmann's dataset](https://github.com/JeffSackmann/tennis_atp) when `use_sample_data=False`.
- Feature prep with `pandas` + `MinMaxScaler`, including rolling rank trends, velocity, and match-derived form features aggregated per player/month.
- Training + evaluation of both single-layer and stacked LSTMs for:
  - **Player-specific** models (one model per athlete).
  - **Global** models spanning all selected players.
- 2023 rankings act as the explicit test window (training uses <2023), and the notebook plots sample predictions vs. truth.

## Local usage

1. Create and activate a Python 3.10+ environment with PyTorch, pandas, numpy, scikit-learn, and matplotlib (see `requirements.txt`).
2. Open `notebooks/tennis_ranking_forecast.ipynb` in VS Code, Jupyter Lab, or Colab.
3. Keep `use_sample_data = True` for the embedded toy data, or flip it to `False` to clone the full ATP repository the first time the cell runs.
4. Execute the remaining cells to train the models and review the comparison tables + plots.

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
