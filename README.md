# Football Match Outcome Prediction

## Overview

A machine learning project that predicts **home wins, draws, and away wins** across five major European football leagues using historical match data and engineered performance features.

## Dataset

- **Source:** Understat, accessed using `soccerdata`
- **Leagues:** Premier League, La Liga, Bundesliga, Serie A, Ligue 1
- **Seasons:** 2021/22–2025/26
- **Matches:** 8,908
- **Target:** Home Win, Draw, or Away Win

## Features

The model uses 14 features across four categories:

- **Expected goals:** Rolling averages of xG and xGA over the previous five matches
- **Team strength:** Home and away Elo ratings and their difference
- **Recent form:** Points per game and goal difference over the previous five matches
- **Rest:** Days since each team's previous match and the difference between them

Rolling features use previous matches only, helping prevent future information from leaking into predictions.

## Methodology

1. Collected and cleaned match data across five leagues and five seasons.
2. Engineered rolling performance statistics, Elo ratings, and rest-day features.
3. Split the data chronologically by season rather than randomly.
4. Compared Logistic Regression, Random Forest variants, and XGBoost using expanding-window validation.
5. Selected a Random Forest model with increased weight for draws.
6. Evaluated the final model on the held-out 2025/26 season.

The final model was trained on seasons 2021/22–2024/25 and tested on 2025/26.

## Final Test Results

| Metric | Score |
|---|---:|
| Accuracy | 47.55% |
| Macro F1-score | 45.46% |
| Log loss | 1.024 |
| Test matches | 1,752 |

The model performs best at identifying home wins, while draws remain the most difficult outcome to predict.

## Feature Importance

Elo ratings were among the most influential features, with **the difference between home and away Elo ratings** ranking highest. Individual team Elo ratings and recent expected-goals performance also contributed substantially.

These results suggest that team strength and recent attacking and defensive performance provide useful signals for match outcome prediction. Feature importance indicates predictive contribution, not causation.

## Elo Ablation Study

To evaluate the value of Elo ratings, the model was compared with and without Elo features on the 2024/25 validation season.

| Model | Accuracy | Macro F1-score |
|---|---:|---:|
| With Elo | 48.52% | 45.98% |
| Without Elo | 41.67% | 39.39% |

Including Elo features improved both metrics in this validation experiment, supporting their usefulness for estimating relative team strength.

## Limitations

- Football outcomes are inherently uncertain, and a model cannot reliably predict every match.
- Draws are difficult to distinguish from wins using pre-match statistics alone.
- Results from one held-out season do not guarantee future performance.
- Feature importance describes model behavior and does not establish causal relationships.
