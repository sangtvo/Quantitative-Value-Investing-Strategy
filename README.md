# Quantitative Value Investing Strategy
> A robust algorithmic quantitative value investing strategy that selects 100 value stocks with the best value metrics using a universe of stocks in S&P 500 and [IEX Cloud API](https://iexcloud.io/docs/api/).

Table of Contents
---
1. [General Information](#general-information)
2. [Summary](#summary)
3. [Tech Stack](#tech-stack)
4. [Code Examples](#code-examples)

<a name="https://github.com/sangtvo/Quantitative-Value-Investing-Strategy#general-information"/>
<a name="https://github.com/sangtvo/Quantitative-Value-Investing-Strategy#summary"/>
<a name="https://github.com/sangtvo/Quantitative-Value-Investing-Strategy#tech-stack"/>
<a name="https://github.com/sangtvo/Quantitative-Value-Investing-Strategy#code-examples"/>


General Information
---
A project on using batch [IEX Cloud API](https://iexcloud.io/docs/api/) call to retrieve financial data, `input()` function, and output results in a customizable excel format. The results are not investment advice, but for educational purposes.

Summary
---
Value strategy is an investment strategy that selects a "universe of stocks", S&P 500 in this case, that appears to be trading for less than their intrinsic or book value. In other words, we seek undervalued businesses that are expected to increase in value over time. By eliminating high risk stocks and considering stock value through company value metrics, we will identify the best 100 value stocks to invest in based on a $10M equal-weight portfolio.

The value metrics for this project are:
* Debt-to-Equity Ratio
    * Proportion of equity to debt a company is using to finance its assets.
* Price-to-Earnings Ratio
    * What the market is willing to pay for a stock based on past or future earnings.
* Price-to-Book Ratio
    * Measures whether a stock is over/undervalued by comparing the net value (assets - liabilities) to its market capitalization.
* Price-to-Sales Ratio
    * Compares stock price to its revenues.
* Price-to-Earnings-to-Growth Ratio
    * Measures the relationship between the price/earnings ratio and earnings growth.
* Enterprise Value/Earnings Before Interest, Taxes, Depreciations, and Amortization
    * Comparing the value of a company, debt included, to the company’s cash earnings less non-cash expenses.
* Enterprise Value/Gross Profit
    * How many dollars of enterprise value are generated for every dollar of gross profit earned--the higher the ratio, the higher the company's net worth.


Tech Stack
---
* Excel
* Python
    * math
    * numpy
    * pandas
    * requests
    * scipy
    * xlsxwriter
* Jupyter Notebook
* Visual Studio Code 

Code Examples
---
```python
# loop data from batch API call and applying it to the empty df
for symbol_string in symbol_strings:
    batch_api_call_url = f'https://sandbox.iexapis.com/stable/stock/market/batch?symbols={symbol_string}&types=quote,advanced-stats&token={IEX_CLOUD_API_TOKEN}'
    data = requests.get(batch_api_call_url).json()
    
    for symbol in symbol_string.split(','):
        ev = data[symbol]['advanced-stats']['enterpriseValue']
        ebitda = data[symbol]['advanced-stats']['EBITDA']
        gp = data[symbol]['advanced-stats']['grossProfit']

        try:
            ev_to_ebitda = ev/ebitda
        except:
            ev_to_ebitda = np.NaN

        try:
            ev_to_gross_profit = ev/gp
        except:
            ev_to_gross_profit = np.NaN
        
        df = df.append(
            pd.Series(
                [ 
                symbol,
                data[symbol]['quote']['latestPrice'],
                'N/A',
                data[symbol]['advanced-stats']['pegRatio'],
                'N/A',
                data[symbol]['advanced-stats']['debtToEquity'],
                'N/A',
                data[symbol]['quote']['peRatio'],
                'N/A',
                data[symbol]['advanced-stats']['priceToBook'],
                'N/A',
                data[symbol]['advanced-stats']['priceToSales'],
                'N/A',
                ev_to_ebitda,
                'N/A',
                ev_to_gross_profit,
                'N/A',
                'N/A'
                ],
                index = my_columns
            ),
            ignore_index = True
        )
```

To see the full code, check out [quantitative_value_strategy_ipynb](https://github.com/sangtvo/Quantitative-Value-Investing-Strategy/blob/main/code/quantitative_value_strategy.ipynb).

To see the best 100 value stocks in excel, check out [100_value_stock_strategy.xlsx](https://github.com/sangtvo/Quantitative-Value-Investing-Strategy/blob/main/code/100_value_stock_strategy.xlsx).