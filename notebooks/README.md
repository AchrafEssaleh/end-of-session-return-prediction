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

## feature_engineering.ipynb: IN PROGRESS

**Autocorrelation study (done)**
- Removed 29 rows with corrupted values (23 of 29 in r0, the rest in r1 to r3) before computing correlations.
- Raw returns lag 1: correlation -0.0056. Statistically real (huge sample) but explains 0.003% of variance meaning direction of past returns is not predictable
- Absolute returns, lag 1: +0.22 (Pearson) and +0.23 (Spearman)...Volatility clustering is real: size is predictable (direction isn't )
- Absolute returns, lags 1 to 52: fast decay until lag  approx 10 then slow decay until approx 45 ( meaning Long memory) The end of the curve is partly a composition artifact (only opening returns are compared)

**Next steps**
1. Separate row level volatility from within row clustering: subtract each row's mean |r| and recompute the curv
2. Build log spaced realized volatility windows (last 1, 3, 6, 12, 24 returns and the full morning), plus ratios short/long.
3. Add signed features (cumulative return) and keep r52 as a standalone feature
4. Validate the choices with GroupKFold in `modeling.ipynb`.
