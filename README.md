# supply-chain-delivery-analysis

# Supply Chain Delivery Performance Analysis

An end-to-end analysis of late deliveries in a global e-commerce supply chain: how often orders arrive late, what it costs, where the bottlenecks are, and whether late orders can be predicted before they ship.

## Key results

| Metric | Value |
|---|---|
| Orders analysed | 172,765 (Jan 2015 – Jan 2018) |
| Late delivery rate | 54.71% (94,523 orders) |
| Total profit (profitable orders) | $7.5M |
| Profit at risk (delayed orders) | $2.1M |
| Random Forest accuracy | 74% (precision 0.78, recall 0.75 on late orders) |

- **Shipping mode is the biggest lever.** Delay rates are 100% for First Class, 79.8% for Second Class, 39.8% for Standard Class and 0% for Same Day.
- **The problem is systemic.** Delay rates sit between 55% and 59% across all regions and customer segments, so it is not a local failure.
- **Profit per order barely changes with delay length** (about $21–$23), so the damage comes from the volume of late orders, not from individual order economics.
- **Peak delay periods:** August, September and December, and the late-evening hours.

## Figures

![Profitability distribution](images/fig1_profitability_distribution.png)
*Figure 1. Distribution of orders by profitability tier.*

![Delay distribution and profit by delay days](images/fig2_delay_distribution_profit.png)
*Figure 2. Delay distribution (left) and profit analysis by delay days (right).*

![Bottleneck detection](images/fig3_bottleneck_detection.png)
*Figure 3. Late delivery percentage across six operational dimensions.*

![Top drivers in Central Africa](images/fig4_central_africa_drivers.png)
*Figure 4. Top 10 drivers of late delivery in Central Africa.*

![Time-based delay patterns](images/fig5_time_based_patterns.png)
*Figure 5. Late delivery percentage by month, day of week and hour of day.*

## Method

1. **Cleaning:** dropped personal and irrelevant columns, removed cancelled shipments, parsed order and shipping dates.
2. **Delay definition:** `Delay = (shipping date − order date) − scheduled shipping days`. An order is late when `Delay > 0`.
3. **Analysis:** KPIs, profitability tiers, profit versus delay, delay percentage by region, segment, shipping mode, order status, payment type and department, a root-cause drill-down for the worst region (Central Africa), and month, weekday and hour patterns.
4. **Modelling:** frequency encoding of categorical features, stratified 80/20 train/test split, SMOTE on the training set only, Random Forest classifier (`random_state=42`).

## Repository structure

```
.
├── notebooks/
│   └── supply-chain-performance-report.ipynb   # full analysis and model
├── reports/
│   └── Delivery_Performance_Report.docx        # written report
├── images/                                     # figures used in this README
├── requirements.txt
└── README.md
```

## Run it yourself

1. Download the **DataCo Smart Supply Chain** dataset (`DataCoSupplyChainDataset.csv`) from [Kaggle](https://www.kaggle.com/datasets?search=DataCo+supply+chain) or Mendeley Data. The file is not included here because of its size.
2. Put the CSV in a `data/` folder at the project root.
3. In the notebook, change the `pd.read_csv(...)` path (it currently points to a Kaggle input directory) to `data/DataCoSupplyChainDataset.csv`.
4. Install and run:

```bash
pip install -r requirements.txt
jupyter notebook notebooks/supply-chain-performance-report.ipynb
```

## Limitations

- The shipping-mode delay rates are extreme (First Class at 100%, Same Day at 0%), which suggests the delays in this dataset follow built-in rules and may be partly synthetic. Treat the findings as a demonstration of the method rather than as evidence about a real company.
- The model uses scheduled shipping days and shipping mode, which are closely tied to the late-delivery label. The 74% accuracy should be read with that in mind.
- Only a single Random Forest with default settings was tested; no tuning or comparison with other models.

## Tools

Python, pandas, NumPy, matplotlib, seaborn, scikit-learn, imbalanced-learn.

## Dataset

DataCo Smart Supply Chain dataset. Check the licence on the source page before reusing or redistributing the data.
