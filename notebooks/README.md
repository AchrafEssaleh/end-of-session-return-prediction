# Notebooks:Progress Log

## eda.ipynb: DONE

**Goal:** understand the data before touching the models

**Target distribution**
- 3 classes: -1 (down), 0 (flat), +1 (up)
- Roughly 41% / 30% / 29% : slightly imbalanced toward flat (not extreme) 

**Missing values**
-  approx 10-12% missing per return column, except r0 ( aprrox 6% likely because it's the opening return i.e always captured...)
- Missingness is NOT random (aka MCAR): some equities have no gaps ( or almost ) & others are heavily damaged.
- For damaged equities gaps are scattered across many rows (94% were classified as partial) not whole missing days (only 3%  all empty)
- Rows with missing data are 54% flat class vs 33% for complete rows meaninf missingness itself is predictiv  probably tied to illiquid stocks
- Checked if missingness is just a proxy for low vol.: no!!!!!! rows with missing data  have *higher* realized volatility on their observed points not lower meaning missingness and volatility are separate signals not redundant

**Realized volatility(aka sum of squared returns)**
- Found and removed 29 corrupted rows(0.003%)caused by a single absurd value in r0 per row and confirmed as isolated data errors not market moves (not concentrated in specific equities)
- After cleaning: flat class rows have lower realized volatility (median  at approx 116) than directional classes (median  approx 159) -->  confirms morning volatility helps predict whether the afternoon will be calm or make a clear move (either sides)

**CC / imputation strategy**
- Keep `n_missing` and `n_obs` (count of missing/observed returns per row) as features in order to captures the missingness signal 
- Added `avg_sq_return` (squared returns normalized by `n_obs`) to avoid bias from rows having fewer observed values
- Zero filled the actual missing returns (defend as no trade = no movement) keeping the count features so the model can tell real zeros from imputed ones

## feature_engineering.ipynb : (IN PROGRESS) 

Next steps:
- Study correlation/autocorrelation between r_i's  &check for momentum (trend continuation) v.s mean reversion in the morning returns to decide if a trend feature is worth building or not 
- Build the rest of the feature set (cumulative return , signed features per Module 1's leverage effect reasoning... ) before moving to `modeling.ipynb` :) 
