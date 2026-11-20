# Terms Explainer

## Step 3 — EDA

Exploratory data analysis checks that the source covers the selected months and summarizes sales patterns—such as seasonality, intermittent demand, and customer/product concentration—before modeling. This helps identify data or modeling risks; it does not train the forecast.

## Attribute conflict

A series has an attribute conflict when its invoices contain multiple Territory or Customer Type values. The monthly builder chooses the most frequent value for each observed month and carries it through months with no invoices.

Territory and Customer Type are kept for EDA and summaries but are **not inputs to the current LightGBM model**. Their conflicts therefore do not directly affect quantity predictions. In the analyzed data, 615 series had at least one such conflict.

## Steps 4–5 — Features and backtest

Step 4 gives the model past quantity/value (recent months and averages), calendar timing, and the four series identifiers. Features use prior months only; Territory and Customer Type are excluded.

Step 5 evaluates LightGBM against seasonal naive on a user-selected historical holdout in `Accuracy_test.ipynb`. MAE and WAPE compare predicted quantity with actual quantity; lower error is better. Sales value is estimated from recent prices, not separately tested.

## Step 6 — 12-month forecast

The selected model is refit using all data through the cutoff, then predicts the next 12 months one month at a time, using earlier predictions as history. Sales value is estimated from recent prices. Results are saved monthly in Parquet and summarized by customer, product family, territory, and company in Excel, including 1-, 3-, 6-, and 12-month totals.
