# Walmart Weekly Sales Analysis

Exploratory data analysis of weekly sales across 45 Walmart stores (February 2010 to October 2012). The project asks what actually drives weekly sales: the economy (CPI, unemployment, fuel price), temperature, holidays, seasonality, or the store itself. Built with Python (pandas, seaborn, matplotlib).

## Dataset

- **Source:** Walmart Store Sales dataset on Kaggle. *(Add the link to the dataset page here.)*
- **Size:** 6,435 rows and 8 columns (45 stores x 143 weeks)
- **Period:** weekly data from 5 February 2010 to October 2012
- **Missing values:** none

| Column | Type | Description |
|---|---|---|
| `Store` | integer | Store number (1 to 45) |
| `Date` | date | Week of the sales record (day-month-year) |
| `Weekly_Sales` | decimal | Sales for the week, about $210,000 to $3.82 million (mean about $1.05 million) |
| `Holiday_Flag` | 0 / 1 | 1 if the week contains a flagged holiday (about 7% of weeks) |
| `Temperature` | decimal | Regional temperature, -2 to 100 (assumed degrees Fahrenheit) |
| `Fuel_Price` | decimal | Regional fuel price, 2.47 to 4.47 |
| `CPI` | decimal | Consumer Price Index, 126 to 227 |
| `Unemployment` | decimal | Regional unemployment rate (%), 3.9 to 14.3 |

## Methods

1. **Loaded and inspected** the data with `head()`, `info()`, `describe()` and `dtypes`.
2. **Parsed dates** with `pd.to_datetime(..., format="%d-%m-%Y")` and derived `Year` and `Month` columns.
3. **Compared yearly sales**, first using all months and then using a like-for-like comparison of February to October only, since 2010 and 2012 are incomplete years.
4. **Measured seasonality** with average weekly sales by month for each year.
5. **Compared holiday weeks** with non-holiday weeks, split by holiday (Super Bowl, Labor Day, Thanksgiving, Christmas).
6. **Correlated** weekly sales with CPI, unemployment and temperature (Pearson and Spearman), and plotted each with scatter plots and regression lines.
7. **Analysed stores:** average sales and variability per store.
8. **Removed the store effect** by dividing each week's sales by that store's own average (a sales index), then re-tested the correlations with CPI, unemployment, temperature and fuel price.

**Tools:** Python, pandas, seaborn, matplotlib, Jupyter Notebook

## Key findings

**1. Economic indicators barely relate to sales.** Raw correlations with weekly sales are all weak:

| Factor | Pearson | Spearman |
|---|---|---|
| CPI | -0.073 | -0.055 |
| Unemployment | -0.106 | -0.062 |
| Temperature | -0.064 | -0.071 |

**2. Store differences are the main driver.** Average weekly sales range from about $2.11 million at Store 20 to roughly $260,000 at Store 33, around eight times higher. After adjusting each store's sales to its own average, the correlations with CPI (0.002), unemployment (-0.018) and fuel price (-0.033) drop to almost zero. The earlier weak relationships mostly reflected which stores sit in which regions, not the economy. Temperature stays weakly negative (-0.079), which is consistent with it acting as a proxy for the time of year.

**3. Sales are strongly seasonal.** Average weekly sales jump in November and especially December (about $1.28 million, against roughly $1.0 to $1.07 million in most other months), and are lowest in January. The pattern repeats in all three years.

**4. The holiday effect depends on the holiday.** Compared with a non-holiday average of about $1.04 million:

| Holiday week | Average weekly sales | Difference |
|---|---|---|
| Thanksgiving | $1,471,273 | +41% |
| Super Bowl | $1,079,128 | +4% |
| Labor Day | $1,042,427 | about 0% |
| Christmas | $960,833 | -8% |

Thanksgiving is the only holiday with a large boost. The flagged Christmas weeks are below average, probably because the flag falls on the last week of December, after the main pre-Christmas shopping rush (this is an inference and should be checked against the flagged dates).

**5. There was no real sales decline.** Using all months, average weekly sales appear to fall from about $1.06 million (2010) to $1.03 million (2012), but 2012 has no November or December weeks. Comparing only February to October gives $1.024 million (2010), $1.018 million (2011) and $1.043 million (2012): essentially flat, with 2012 about 2% higher than 2010.

## Limitations

- **Short period:** about 2.7 years of data, so long-term trends cannot be assessed.
- **No store details:** store size, format and location are not included, so the reasons for store differences cannot be tested.
- **Regional economic data:** CPI and unemployment change slowly within a region, so they act partly as location labels.
- **Holiday flags:** the notebook maps flagged weeks to named holidays by month, which assumes the flag covers only those four holidays.
- **Temperature units** are assumed to be Fahrenheit.
- **Correlation is not causation.** This is descriptive analysis.

## How to run

```bash
pip install -r requirements.txt
jupyter notebook Walmart-Analysis.ipynb
```

Place `Walmart_Sales.csv` in the same folder as the notebook.

## Possible next steps

- Build a baseline forecast (for example "same week last year") and compare it with a machine learning model split by time
- Test each factor's predictive power with a random forest (store only vs economy only vs store plus season)
- Analyse the most volatile stores and the pre-Christmas weeks separately
- Add a store-level dashboard
