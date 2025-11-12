# tennis-ranking-predictor

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jackhenry02/tennis-ranking-predictor/blob/main/tennis_ranking_forecast_project/notebooks/tennis_ranking_forecast.ipynb)

Forecast ATP player rankings one month ahead with single-layer and stacked LSTM models trained on Jeff Sackmann's tennis data. The repository ships with lightweight CSV samples so you can demo the workflow instantly, then switch to the full dataset inside the notebook for richer experiments.

## Project layout

```
tenis_ranking_forecast_project/
├── data/
│   ├── sample_matches.csv
│   └── sample_rankings.csv
├── notebooks/
│   └── tennis_ranking_forecast.ipynb
└── README.md
```

## Notebook highlights

- Personal, narrative introduction plus an initialization cell where you can toggle `use_sample_data`.
- Automated cloning of [Jeff Sackmann's dataset](https://github.com/JeffSackmann/tennis_atp) when `use_sample_data=False`.
- Feature prep with `pandas` + `MinMaxScaler`, including rolling rank trends, velocity, and match-derived form features.
- Training + evaluation of both single-layer and stacked LSTMs for:
  - **Player-specific** models (one model per athlete).
  - **Global** models spanning all selected players.
- 2024 rankings serve as an explicit hold-out test window, and the notebook plots sample predictions vs. truth.

## Local usage

1. Create and activate a Python 3.10+ environment with PyTorch, pandas, numpy, scikit-learn, and matplotlib.
2. Open `notebooks/tennis_ranking_forecast.ipynb` in VS Code, Jupyter Lab, or Colab.
3. Keep `use_sample_data = True` for the embedded toy data, or flip it to `False` to clone the full ATP repository the first time the cell runs.
4. Execute the remaining cells to train the models and review the comparison tables + plots.

## Colab workflow

1. Click the **Open in Colab** badge above.
2. In Colab, run `!pip install torch pandas numpy scikit-learn matplotlib` if needed.
3. Execute the initialization cell and toggle `use_sample_data` as desired.
4. Continue through the notebook to reproduce the results.

## Packaging

Zip up the folder for sharing or upload directly to GitHub/Colab:

```bash
zip -r tennis_ranking_forecast_project.zip tennis_ranking_forecast_project
```

Once you push to GitHub, update the Colab badge link to point to your repository path.
