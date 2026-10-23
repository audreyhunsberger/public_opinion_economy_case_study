# How partisanship shapes Americans' views of the economy

A data journalism case study using four surveys (CES, ANES, Pew, University of Michigan) and federal
economic data, 2016–2026. The full story can be found on the "report" pdf.

## Findings

- Americans are about 40 percentage points more likely to say the economy improved when their party holds the White House, comparing people surveyed in the same year.
- The shift starts the month of the presidential election, not the inauguration.
- Midterms barely move it: the partisan gap shifted 2–3 points after the 2018 and 2022 House flips, versus about 60 after the 2020 and 2024 presidential elections.
- The partisan gap is at least twice as large for "the economy" as for people's own finances, with or without income, employment and homeownership controls.

## Repository Layout

- report: The published story (PDF)
- slides: The slideshow for presentation
- data: Raw source files plus codebooks
- notebooks: The pipeline, run in order from 01 to 06
- plan: The analysis plan written before the work began
- .databricks: Stuff from Databricks (where the project was completed)

## Pipeline

| Notebook | Language | What it Does |
| --- | --- | --- |
| 01_staging_models | SQL | One clean table per raw file: renamed, typed, survey codes labeled, missing codes set to NULL |
| 02_intermediate_layer | SQL | Harmonizes CES and ANES, builds a gap-free monthly calendar of economic data, event-time panels |
| 03_mart_models_sql | SQL | Aggregate tables behind every chart and number |
| 04_mart_models_python | Python | Regressions and bootstraps (co-partisan premium, swing timing, wallet-vs-economy gap) |
| 05_full_analysis | Python | Full analysis: 17 charts, robustness battery, a number sheet tracing every figure to a table |
| 06_article_charts | Python | Only the six charts in the story, with a check of every published number |

Each notebook ends with QA cells that reconcile its tables against the raw files or the previous layer.

## Data Sources

Downloaded October 2026. FRED and BEA series are revised over time, so later downloads may differ slightly.

| Dataset Name | Description |
| --- | --- |
misery_index | Created by economist Arthur Okun in the 1970s, the misery index is calculated by adding the seasonally adjusted unemployment rate to the annual inflation rate. While it is not a perfect measure of economic hardship (it doesn't take into account GDP growth, financial situations from past months, and other factors), it can be used as a supplement to other economic indicators. Data is aggregated from the [Bureau of Labor Statistics (BLS)]([url](http://bls.gov/)). |
pew_economy_rating_by_party | Data comes from [Pew Research's statistics]([url](https://www.pewresearch.org/chart/americans-give-the-economy-negative-ratings-as-republicans-positive-ratings-continue-to-slip/)) on the % of Americans who say economic conditions in the country today are excellent/good, by political party. |
cumulative_2006-2025_zstd.feather | Data comes from the [Cooperative Election Study (CES)]([url](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910%2FDVN%2FII2DB6)), which includes select standardized variables including demographics, geography, vote choice, validated vote, representative approval, and views on the economy from 2006 to 2025. By far the most comprehensive and detailed dataset in this analysis. |
anes_cdf.parquet | The ANES Time Series Cumulative [Data File]([url](https://electionstudies.org/data-center/anes-time-series-cumulative-data-file/)). |
cpi_data.csv | Contains BLS [Consumer Price Index (CPI) data]([url](https://data.bls.gov/dataQuery/search)) for important goods |
codebook_short_guide.csv | A table of important ANES and CCES variables. |
employed_fulltime_median_usual_weekly_real_earnings.csv | Employed full time: Median usual weekly real earnings: Wage and salary workers: 16 years and over ([LES1252881600Q]([url](https://fred.stlouisfed.org/series/LES1252881600Q))), U.S. Bureau of Labor Statistics via FRED® |
personal_saving_as_percent_of_disposable_income.csv | Personal saving as a percentage of disposable personal income ([A072RC1Q156SBEA]([url](https://fred.stlouisfed.org/series/A072RC1Q156SBEA))), U.S. Bureau of Economic Analysis via FRED® |
personal_saving_rate.csv | Personal Saving Rate ([PSAVERT]([url](https://fred.stlouisfed.org/series/PSAVERT))), U.S. Bureau of Economic Analysis via FRED® |
household_debt_service_payments_as_percent_of_disposable_personal_income.csv | Household Debt Service Payments as a Percent of Disposable Personal Income ([TDSP]([url](https://fred.stlouisfed.org/series/TDSP))),  Board of Governors of the Federal Reserve System |
30year_fixed_mortgage_rate.csv | 30-Year Fixed Rate Mortgage Average in the United States ([MORTGAGE30US]([url](https://fred.stlouisfed.org/series/MORTGAGE30US))), Freddie Mac via FRED® |
commercial_interest_rate_on_credit_card_plans.csv | Commercial Bank Interest Rate on Credit Card Plans, All Accounts ([TERMCBCCALLNS]([url](https://fred.stlouisfed.org/series/TERMCBCCALLNS))), Board of Governors of the Federal Reserve System (US) via FRED® |
48_month_auto_loan_rates.csv | Finance Rate on Consumer Installment Loans at Commercial Banks, New Autos 48 Month Loan ([TERMCBAUTO48NS]([url](https://fred.stlouisfed.org/series/TERMCBAUTO48NS))), Board of Governors of the Federal Reserve System (US) via FRED® |
DJIA.csv | Dow Jones Industrial Average ([DJIA]([url](https://fred.stlouisfed.org/series/DJIA))), S&P Dow Jones Indices LLC via FRED® |
NASDAQCOM.csv | NASDAQ Composite ([NASDAQCOM]([url](https://fred.stlouisfed.org/series/NASDAQCOM))), Nasdaq, Inc. via FRED® |
SP500.csv | S&P 500 ([SP500]([url](https://fred.stlouisfed.org/series/SP500))), S&P Dow Jones Indices LLC via FRED® |
real_personal_cons_exp.csv | Real Personal Consumption Expenditures ([PCEC96]([url](https://fred.stlouisfed.org/series/PCEC96))), U.S. Bureau of Economic Analysis via FRED® | 
consumer_sentiment_by_party.csv | [Index]([url](https://data.sca.isr.umich.edu/charts.php)) of Consumer Sentiment Within Political Party, University of Michigan Surveys of Consumers |
umich_consumer_sentiment_index_data.csv | UMich Surveys of Consumers data, [index scores]([url](https://data.sca.isr.umich.edu/subset/subset.php)) (percent favorable - percent unfavorable + 100). | 
inauguration_election_dates.csv | Hand-built table of presidential inauguration and election dates, with parties, since 2000 |
monthly_inflation_rates_2000_2026.csv | Monthly inflation rate |

## Limitations

- Only three changes of party in the White House fall in the data, so the 40-point estimate is an
association, not a precisely identified causal effect.
- CES is an opt-in online panel (YouGov), weighted to national benchmarks; ANES uses probability samples.
- National spending totals can't show whether one party's supporters cut back while the other's spent more.
October 2025 has no CPI or unemployment data (government shutdown); it is left missing, never filled.
