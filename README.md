## Problem Framing

The CFM challenge focuses on predicting the direction of US equity returns during the final two hours of the trading session (2:00 pm to 4:00 pm). This period is relevant for large scale trade execution due to its market liquidity and potential execution cost advantages.

The objective is to predict the end of session price movement using the preceding intraday price history. To reduce sensitivity to market noise, the task is formulated as a **three class classification problem**:

- **−1 (Decrease):** End of session return below −25 basis points
- **0 (Neutral):** End of session return between −25 and +25 bps
- **+1 (Increase):** End of session return above +25 bps

The model uses **53 five minute return observations** expressed in bps, covering the first 4.5 hours of the trading session (9:30 am to 2:00 pm)

This is a low signal to noise prediction problem, where short term financial returns can be difficult to predict reliably. meaning even modest improvements in out of sample performance require careful validation and  prevention of data leakage.

The project aims to evaluate machine learning approaches against the following reference points:

- **Random classification baseline:** Approximately 33.33% assuming balanced classes 
- **Published challenge benchmark:** 41.74% test accuracy

The objective is to investigate whether intraday return patterns and engineered features can provide useful predictive information for end of session return classification