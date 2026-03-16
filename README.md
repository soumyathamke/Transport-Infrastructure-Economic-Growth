# Transport Infrastructure & Economic Growth — MENA Region

## Overview

This project analyses the relationship between transport infrastructure quality and economic growth across 8 MENA countries from 2010 to 2022. OLS regression is used to quantify how logistics performance and air connectivity drive GDP per capita, and findings are framed as investment recommendations for infrastructure development in the Gulf.

**Tools:** Python (Pandas, NumPy, Matplotlib, Seaborn, Scipy)  
**Data:** World Bank World Development Indicators (WDI)  
**Period:** 2010–2022 | **Countries:** 8 MENA nations | **Observations:** 104 rows

---

## Key Findings

### OLS Regression Results
- **R² = 0.828 | Adjusted R² = 0.815** — the model explains 82.8% of variance in log(GDP per capita)
- **LPI Score** — statistically significant at 10% level (p=0.071). A 1-point increase in logistics performance is associated with an 80.9% increase in GDP per capita
- **Unemployment** — strongest predictor (p<0.001). Higher unemployment strongly correlates with lower GDP per capita

### Correlation Analysis
| Relationship | Correlation |
|---|---|
| LPI vs GDP per capita | r = 0.637 |
| Air passengers vs GDP per capita | r = 0.447 |
| Unemployment vs GDP per capita | r = -0.816 |

### Country Averages (2010–2022)
| Country | Avg GDP/cap (USD) | Avg LPI | Avg Unemployment |
|---|---|---|---|
| Qatar | $79,328 | 3.39 | 0.23% |
| UAE | $47,165 | 3.81 | 2.50% |
| Kuwait | $37,217 | 3.06 | 2.39% |
| Saudi Arabia | $27,439 | 3.19 | 5.93% |
| Jordan | $4,099 | 2.76 | 15.45% |
| Tunisia | $3,972 | 2.73 | 15.96% |
| Morocco | $3,343 | 2.75 | 9.50% |
| Egypt | $3,092 | 2.94 | 10.49% |

---

## Investment Recommendations

1. **Egypt & Tunisia** — logistics infrastructure gap is the highest marginal return on investment in the region
2. **UAE & Qatar** — airport capacity expansion justified by strong air-GDP correlation
3. **Jordan** — rising unemployment (12% → 18%) despite stable infrastructure signals structural labour market reforms are needed beyond infrastructure alone


## Methodology

### Data Loading & Cleaning
- World Bank WDI export has non-standard formatting — indicators stored as rows, year columns named `2010 [YR2010]`, missing values represented as `..`
- Custom `load_wb_data()` function handles all cleaning: drops footer rows, extracts clean year numbers, converts `..` to NaN, and pivots from long to wide format
- Air passengers converted to millions; GDP per capita log-transformed for regression

### OLS Regression
- **Model:** log(GDP per capita) ~ LPI + Air Passengers (M) + Unemployment (%)
- **Why log(GDP)?** GDP per capita is highly skewed across countries (Qatar $88k vs Egypt $3k) — log transformation linearises the relationship and makes coefficients interpretable as percentage changes
- Solved using NumPy linear algebra: β = (XᵀX)⁻¹ Xᵀy
- Standard errors, t-statistics and p-values computed manually from residuals
- Only country-years with LPI data included (LPI is a biennial survey — ~half the rows have NaN)

### Visualisations (5-panel chart)
| Panel | Description |
|---|---|
| Top-left | GDP per capita trends over time — all 8 countries |
| Top-right | Correlation heatmap (lower triangle) |
| Bottom-left | LPI score vs GDP per capita scatter + OLS trend line |
| Bottom-centre | Air passengers vs GDP per capita scatter + OLS trend line |
| Bottom-right | Average GDP per capita bar chart ranked by country |

### Excel Report (4 sheets)
| Sheet | Contents |
|---|---|
| Raw Data | Cleaned dataset — one row per country per year |
| Country Summary | Average indicators per country across 2010–2022 |
| Correlation Matrix | Pearson correlations between all key variables |
| OLS Regression | Coefficients, standard errors, t-statistics, p-values |

---

## Data Source

**Source:** World Bank World Development Indicators (WDI)  
**Download:** https://databank.worldbank.org/source/world-development-indicators  
**Last Updated:** 02/24/2026

**Indicators used:**
| Code | Indicator |
|---|---|
| NY.GDP.PCAP.CD | GDP per capita (current US$) |
| IS.AIR.PSGR | Air transport, passengers carried |
| SL.UEM.TOTL.ZS | Unemployment, total (% of labour force, ILO) |
| NY.GDP.MKTP.KD.ZG | GDP growth (annual %) |
| LP.LPI.OVRL.XQ | Logistics Performance Index (1=low to 5=high) |

**Note:** LPI is a biennial survey — data available approximately every 2 years. Morocco and Jordan have limited LPI coverage in WDI.
