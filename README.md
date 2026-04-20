# Why didn't more people see it? Recommendation Transparency for Providers




## Overview

Transparency in recommender systems has been widely studied from the perspective of individual users, yet the needs of **item providers** — the creators whose content is distributed through these platforms — remain largely unexplored. Providers often lack any insights into why their items receive little or no exposure, leading them to develop speculative folk theories about how recommendation algorithms operate.

This work addresses this gap by proposing a **surrogate modeling approach** to explain item exposure at a global level. Rather than explaining individual user–item pairs, we train a proxy model on a set of engineered structural features to approximate the exposure distribution produced by a black-box recommender. By quantifying the contribution of each feature, our approach enables providers to understand which factors drive the recommendation model's decisions across the entire user base.



---

## Recommendation Models

| Model | Description |
|-------|-------------|
| **NCF** | Neural Collaborative Filtering 
| **VAE** | Variational Autoencoder 
| **BPR** | Bayesian Personalized Ranking
---

## Datasets

| Dataset | Users | Items | Sparsity |
|---------|-------|-------|----------|
| [MovieLens-1M](https://grouplens.org/datasets/movielens/1m/) | 6,040 | 3,953 | 96% |
| [Last.fm](http://millionsongdataset.com/lastfm/) | 24,631 | 6,584 | 99% |


---

## Features

We engineer features across three categories:

### Frequency-Based
| Feature | Description |
|---------|-------------|
| **POP** | Item popularity — max-normalized interaction frequency across all users |
| **SZ** | Average profile size of users who received the item in their Top-K recommendations |

### Item-Centric
| Feature | Description |
|---------|-------------|
| **YR** | Release year — temporal indicator of item age |
| **GD** | Genre density — average number of same-genre items in recipient users' profiles |
| **AE** | Audio energy — perceived intensity of the track *(Last.fm only)* |
| **INS** | Instrumentalness — likelihood of no vocal content *(Last.fm only)* |

### Neighborhood-Based
| Feature | Description |
|---------|-------------|
| **ND** | Neighborhood density — frequency of item appearance in similar users' histories |
| **RI** | Related items — fraction of similar items within recipient users' profiles |

---

## Project Structure

```
ProviderSideTransparency/
│
├── recommender_training.ipynb        # Train NCF, VAE, and BPR recommender models
├── recommenders_architecture.ipynb   # Model architecture definitions
├── Fitting_regression.ipynb          # Train surrogate (XGBoost) model on ML-1M
├── BPR-Fitting_regression.ipynb      # Train surrogate model for BPR
├── activation.ipynb                  # Activation analysis
├── plot.ipynb                        # Generate result figures
├── help_functions.ipynb              # Shared utility functions
├── TMP.ipynb                         # Scratch/experimental notebook
│
├── data/                             # Raw datasets (not included — download separately)
│   └── LASTFM/
├── dataset/                          # Processed dataset files
├── checkpoints/                      # Saved recommender model checkpoints
├── saved/                            # Saved surrogate model outputs
├── log/                              # Training logs
├── log_tensorboard/                  # TensorBoard logs
│
└── .gitignore
```

---

## How to Run

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

### 2. Prepare the data

Download the datasets and place them in `data/`:
- [MovieLens-1M](https://grouplens.org/datasets/movielens/1m/)
- [Last.fm Million Song Dataset](http://millionsongdataset.com/lastfm/)

### 3. Train the recommender models

Open and run `recommender_training.ipynb` to train NCF, VAE, and BPR on both datasets. Checkpoints will be saved in `checkpoints/`.

### 4. Train the surrogate model

Open and run `Fitting_regression.ipynb` (or `BPR-Fitting_regression.ipynb` for BPR) to:
- Engineer structural features from the dataset
- Train the XGBoost surrogate model on item exposures
- Evaluate with R² and MSE metrics
- Compute feature importance scores

### 5. Visualize results

Open `plot.ipynb` to reproduce all figures, including the backward feature elimination curves.

---





