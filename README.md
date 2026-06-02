# Predicting the Market Potential of Extended Range Electric Vehicles in the United States

This repository contains an exploratory modeling project that estimates the potential first year U.S. sales performance of an Extended Range Electric Vehicle, with a focus on a hypothetical Scout Traveler EREV launch.

The project combines historical monthly U.S. vehicle sales, vehicle specification data, lifecycle cleaning, feature engineering, and LightGBM regression models to forecast early launch demand for an EREV vehicle configuration.

## Project Overview

Extended Range Electric Vehicles operate primarily as electric vehicles while using a gasoline powered generator to recharge the battery when needed. This architecture may help address range anxiety, towing range concerns, long distance travel needs, and charging infrastructure limitations.

Because EREVs have limited direct U.S. sales history, this project estimates demand indirectly by training machine learning models on historical vehicle launches and vehicle attributes. The final model focuses on the first twelve months after launch and excludes lag based sales features so that it can be applied to a new vehicle with no prior sales history.

## Research Question

Can historical U.S. vehicle launch data and vehicle specifications be used to estimate the first year sales potential of a new EREV model?

## Repository Structure

```text
.
├── EREVAnalysis.ipynb
├── EREVAnalysisReport.pdf
├── LICENSE
├── README.md
├── .gitignore
└── EREVFigures
    ├── Actual Avg vs Predicted Avg by Month.png
    ├── Actual vs Predicted Sales by Body Style.png
    ├── Actual vs Predicted Sales by Maker.png
    ├── Actual vs Predicted Sales.png
    ├── EREV First Year Forecast.png
    ├── EREV First Year vs Benchmark.png
    ├── EREV First Year vs Segment.png
    ├── Error by Month.png
    ├── Feature Importance.png
    ├── Residual Distribution.png
    └── Residuals vs Predicted.png
```

## Key Files

| File or Folder | Description |
|---|---|
| `EREVAnalysis.ipynb` | Main analysis notebook containing data preparation, feature engineering, modeling, evaluation, and EREV forecasting workflow. |
| `EREVAnalysisReport.pdf` | Final written report summarizing the motivation, methodology, model results, forecast, limitations, and references. |
| `EREVFigures/` | Exported charts used for model diagnostics, feature importance, benchmark comparisons, and Scout Traveler forecast visualization. |
| `LICENSE` | Repository license. |
| `.gitignore` | Files and folders excluded from version control. |

## Methodology

The project follows four main stages:

1. **Sales data preparation**  
   Historical monthly U.S. vehicle sales were transformed from wide monthly tables into a long format with one row per vehicle, powertrain, month, and year.

2. **Vehicle specification merge**  
   Sales records were joined with vehicle attributes such as acceleration, range, power, torque, weight, payload, cargo volume, body style, seating capacity, price, and powertrain type.

3. **Lifecycle cleaning and feature engineering**  
   Pre launch rows and post discontinuation rows were filtered to avoid treating non availability as poor demand. Additional features were created for launch timing, seasonality, price positioning, performance value, and efficiency.

4. **Modeling and forecast simulation**  
   LightGBM regression models were trained on first year launch observations. The selected model was then used to simulate the first twelve months of sales for a hypothetical Scout Traveler EREV.

## Features Used

The model uses a combination of vehicle specifications, market positioning signals, and launch timing features, including:

| Feature Group | Example Features |
|---|---|
| Vehicle performance | Acceleration, top speed, horsepower, torque, power to weight ratio |
| Vehicle utility | Range, payload, cargo volume, seats, body style |
| Pricing | Launch price, log price, price per horsepower, price relative to segment |
| Market identity | Maker or brand, powertrain type, simplified powertrain type, luxury tier |
| Launch dynamics | Months since launch, month sine, month cosine |
| Efficiency | Energy use measured as kWh per 100 miles |

## Model Summary

Two no lag launch year LightGBM models were evaluated:

| Model | Target | MAE Sales | RMSE Sales | R² |
|---|---:|---:|---:|---:|
| Raw sales model | Monthly sales | 372.60 | 795.92 | 0.822 |
| Log sales model | `log1p(monthly sales)` | 471.88 | 1,104.76 | 0.836 on log scale |

The raw sales model was selected for final forecasting because it produced lower error when evaluated directly in monthly vehicle sales units.

## Main Findings

The selected model suggests that first year launch sales can be meaningfully estimated using vehicle attributes and launch available features. The most influential signals included:

- Months since launch
- Price per horsepower
- Seasonality
- Maker or brand
- Price relative to segment
- Power to weight ratio
- Acceleration
- Efficiency

For the hypothetical Scout Traveler EREV, the model produced a competitive first year forecast relative to selected electrified SUV benchmarks. The result should be interpreted as a directional estimate rather than a precise sales forecast.

## Example Figures

### Actual vs Predicted Sales

![Actual vs Predicted Sales](EREVFigures/Actual%20vs%20Predicted%20Sales.png)

### Feature Importance

![Feature Importance](EREVFigures/Feature%20Importance.png)

### EREV First Year Forecast

![EREV First Year Forecast](EREVFigures/EREV%20First%20Year%20Forecast.png)

### EREV First Year vs Benchmark

![EREV First Year vs Benchmark](EREVFigures/EREV%20First%20Year%20vs%20Benchmark.png)

### EREV First Year vs Segment

![EREV First Year vs Segment](EREVFigures/EREV%20First%20Year%20vs%20Segment.png)

## How to Run

Clone the repository:

```bash
git clone <your-repository-url>
cd <your-repository-name>
```

Create and activate a Python environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

Install common dependencies:

```bash
pip install pandas numpy scikit-learn lightgbm matplotlib jupyter
```

Launch Jupyter Notebook:

```bash
jupyter notebook EREVAnalysis.ipynb
```

## Data Availability

The raw monthly vehicle sales data used in this project is not included in this repository because it may be subject to third party licensing restrictions. The notebook is provided to document the modeling process, feature engineering workflow, and forecasting approach.

## Limitations

This project is a directional modeling exercise and should not be interpreted as a production sales forecast. The model does not fully capture factors such as:

- Production constraints
- Dealer or direct sales strategy
- Marketing spend
- Reservation volume
- Macroeconomic conditions
- Regional demand differences
- Charging infrastructure availability
- Consumer sentiment toward Scout as a revived brand
- Future EREV market adoption trends

## Future Work

Potential extensions include:

- Adding regional sales and infrastructure data
- Separating SUV and pickup forecasts
- Incorporating reservation or waitlist data
- Testing time based validation splits
- Comparing LightGBM against XGBoost, CatBoost, and random forest models
- Building scenario forecasts for optimistic, baseline, and conservative launches
- Adding sensitivity analysis for price, range, brand proxy, and production capacity

## Technologies Used

- Python
- Pandas
- NumPy
- Scikit learn
- LightGBM
- Matplotlib
- Jupyter Notebook

## References

See `EREVAnalysisReport.pdf` for the full reference list, including LightGBM documentation, Scikit learn, MarkLines, Scout Motors, Reuters, Natural Resources Canada, and U.S. EPA sources.

## Author

Alex Ponnraj
