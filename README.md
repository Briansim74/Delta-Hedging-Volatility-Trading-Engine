# Delta Hedging & Volatility Trading Simulator
A dynamic delta hedging system for a short NVDA European call option, modeling volatility exposure, gamma risk, and hedging performance under discrete-time rebalancing.

The system replicates option exposure using the underlying asset and cash, continuously adjusting delta to maintain a delta-neutral portfolio under real market conditions.

## Core Idea
The project implements a short volatility trading framework, where option exposure is managed through dynamic hedging in the underlying.

The goal is to evaluate:
- How well delta hedging replicates option exposure in practice
- How volatility and gamma risk evolve under discrete rebalancing
- How hedging error contributes to realized PnL

## Strategy (Short Volatility Exposure)
The portfolio is structured as a short call position:
- Short gamma → exposure to convex losses during large price moves
- Short vega → benefits from volatility compression
- Long theta → captures time decay

This reflects a volatility risk premium strategy, where returns come from option selling while directional exposure is dynamically hedged.

## Hedging Framework
At each time step:
- Compute option delta using Black-Scholes-Merton
- Rebalance underlying position to maintain delta neutrality
- Adjust cash position under a self-financing constraint
- Track portfolio value and PnL evolution

This approximates a continuous-time hedging strategy using discrete execution intervals, introducing realistic replication error.

## Key Components
### Market Data Layer
- Automated NVDA stock and option data ingestion
- Real-time update of underlying price and option parameters
### Pricing Engine
- Black-Scholes-Merton option pricing model
- Greeks computation (delta-focused hedging logic)
- Volatility and interest rate inputs
### Delta Hedging Engine
- Continuous recalculation of hedge ratio
- Dynamic adjustment of underlying exposure
- Self-financing portfolio constraint enforcement
### Portfolio Accounting System
- Real-time PnL tracking
- Exposure decomposition (delta / gamma / vega effects)
- Funding and cash position updates
### Execution Layer
- Cron-based scheduled rebalancing
- Discrete-time hedge updates simulating execution intervals
### Data Persistence Layer
- Azure SQL database for portfolio state tracking
- Historical storage of hedging performance and exposures

## Key Insights
### 1. Discrete hedging introduces structural replication error
Continuous-time hedging assumptions break down under real execution intervals, creating persistent PnL noise.

### 2. Gamma risk dominates near maturity
As expiration approaches, gamma increases sharply, leading to unstable hedge ratios and higher sensitivity to timing.

### 3. Volatility assumptions are primary PnL drivers
Misestimation of implied or realized volatility significantly impacts hedging effectiveness and final PnL.

### 4. Hedging PnL is path-dependent
Final outcomes depend on the realized price trajectory, not just initial conditions or model assumptions.

## Trading Interpretation
This system models core mechanics of:
- Short volatility trading strategies
- Market making with dynamic delta hedging
- Gamma risk management under real execution constraints
- Replication error in options hedging systems

## System Insight
Delta hedging neutralizes directional exposure, but does not eliminate risk — realized PnL is driven by volatility, gamma exposure, and discrete execution effects.

## Summary
This project demonstrates how theoretical delta-hedging replication breaks down in practice due to:
- Discrete rebalancing
- Volatility dynamics
- Gamma exposure
- Execution timing constraints

It provides a practical view of how short-volatility strategies behave under real market conditions.

<br><br><br><br>
<details>
<summary><strong>Detailed Implementation & Usage</strong></summary>


# Delta Hedging Volatility Trading Engine (Python / SQL)
Dynamic options replication system implementing delta hedging for short volatility strategies, with real-time rebalancing, PnL decomposition, and analysis of hedging performance under discrete execution and changing market conditions.

## Overview
This project implements an automated delta-hedged replicating portfolio for a short European call option on NVDA. The system dynamically rebalances exposure in the underlying asset and risk-free asset to replicate the option payoff and evaluate hedging performance under continuously changing market conditions.

## Core Idea
The system implements a dynamic replicating portfolio for a short European call option on NVDA. The position is delta-hedged using the Black-Scholes-Merton framework, with continuous rebalancing of the underlying and cash positions.

This setup is used to evaluate how delta hedging performs under discrete-time execution, market noise, and evolving option sensitivities in real trading conditions.

The structure reflects a volatility-selling approach commonly used in market making, where option premium is collected while directional exposure is continuously managed through hedging in the underlying asset.

### Short Call Rationale and Volatility Exposure
The decision to short the call option is based on the structured payoff characteristics of options and the decomposition of risk into delta, gamma, vega, and theta components.

By shorting the call, the portfolio:
- Gains positive theta exposure, benefiting from time decay as the option approaches maturity
- Assumes short gamma exposure, meaning the portfolio benefits from low volatility regimes but is exposed to convex losses during large price movements
- Assumes short vega exposure, where declines in implied volatility result in portfolio gains

This structure reflects a common market-making perspective, where options are sold to collect premium (theta), while risk is managed through dynamic delta hedging in the underlying asset.

### Hedging Objective
The system focuses on understanding the behavior of a short volatility position under dynamic hedging, rather than directional exposure to NVDA.

In particular, it highlights:
- How effectively delta hedging neutralizes directional risk in practice
- How theta decay interacts with gamma-driven hedging adjustments
- How continuous-time replication assumptions break down under real market dynamics

The portfolio is constructed as a self-financing system that continuously offsets changes in option value through adjustments in the underlying exposure.
### Hedging Mechanism
At each discrete time step, the system implements a delta hedging procedure to replicate the short option exposure:
- Computes option Greeks (primarily delta) using the Black-Scholes-Merton framework
- Rebalances the underlying position to match current delta exposure
- Adjusts the cash position to enforce a self-financing portfolio constraint
- Tracks portfolio value evolution and hedging performance over time

This process forms a discrete-time approximation of a continuous-time delta hedging strategy, capturing the hedging error introduced by discrete rebalancing and real market dynamics.

### Model Assumptions
1. The NVDA option contract is assumed to have a contract multiplier of 1 share per option for simplicity of replication.
2. The option is treated as a European-style call option.
3. Markets are assumed to allow frictionless trading (no transaction costs or slippage in the model).
4. Continuous dividends are approximated via observed dividend yield.

## Delta Hedging Framework
The replicating portfolio is constructed as:
- Short 1 NVDA European call option
- Long Δ shares of NVDA (dynamic)
- Cash position adjusted via risk-free borrowing / lending

Where:
- Δ is computed using the Black-Scholes-Merton model
- Rebalancing occurs at fixed discrete intervals
- Portfolio is maintained under self-financing constraints

The hedging error arises from:
- Discrete rebalancing frequency
- Non-linear gamma exposure
- Volatility misestimation
- Market microstructure noise (simulated via real market data feeds)

## System Architecture
### Real-Time Market Data Pipeline
Market data is retrieved using automated web scraping from Yahoo Finance, capturing underlying price, option price, and implied parameters.

### Option Pricing & Greeks Engine
Black-Scholes-Merton framework is used to compute option price and sensitivities, including delta and implied volatility estimation.

### Dynamic Hedging Engine
At each iteration, the system recalculates delta exposure and adjusts the underlying position accordingly to maintain hedging neutrality.

### Portfolio Accounting System
Tracks:
- Underlying exposure
- Borrowing/lending from risk-free asset
- Option valuation
- Hedging PnL decomposition

### Automated Execution Layer
System is deployed on a scheduled pipeline using Linux Cron jobs, enabling periodic recalculation and rebalancing of the hedge.

## Automation & Data Pipeline
The system is fully automated using a scheduled execution framework:
- Selenium-based data extraction from Yahoo Finance
- Python-based computation of Greeks and portfolio state
- SQL-based persistence layer (Azure SQL Database)
- Cron-based scheduling for periodic execution
- BCP utility for efficient batch updates

The pipeline enables near real-time recalculation of hedge ratios and portfolio PnL under evolving market conditions.

## Key Insights
### 1. Discrete Hedging Introduces Structural PnL Noise
- Even though delta hedging is theoretically risk-neutral under continuous rebalancing, discrete execution introduces unavoidable PnL fluctuations.
- This effect becomes more pronounced in high gamma regimes where delta changes rapidly.

### 2. Gamma Risk Becomes Dominant Near Maturity
- As option maturity approaches, gamma increases significantly, causing sharp adjustments in hedge ratios.
- This leads to higher sensitivity to timing and rebalancing frequency.

### 3. Financing Assumptions Impact Realized PnL
- Small changes in the risk-free rate assumption introduce drift into the replicating portfolio, which compounds over time and affects final PnL outcomes.

### 4. Hedging Performance is Path-Dependent
- Final PnL depends heavily on the realized price path of the underlying asset, reinforcing that replication quality cannot be assessed in a static framework.

## Summary
This project implements a dynamic delta-hedged replicating portfolio to study the practical limitations of continuous-time hedging theory under discrete execution constraints.

It demonstrates how:
- Theoretical no-arbitrage replication deviates in practice
- Hedging error emerges from discretization and volatility dynamics
- Real-world implementation requires integration of pricing, data, and execution systems

## Real-Time Delta Hedging Simulation Output

<img width="500" height="511" alt="Real-Time Delta Hedging Simulation Output" src="Bash Shell Window of Yahoo Script.jpg" />

Live output of the automated delta hedging system showing portfolio state updates, including dynamic delta recalculation, underlying rebalancing, and real-time PnL tracking under discrete-time hedging.

</br></br></br></br>


<details>
<summary><strong>Detailed Implementation & Usage</strong></summary>

# Automated-Delta-Hedging-Replicating-Portfolio

In this notebook, I will describe the processes of creating an Automated Delta Hedging Replicating Portfolio of shorting an NVDA European call option by trading in the underlying stock and bank account. The portfolio strategy I am attempting to create would be a short gamma / short vega / receiving theta portfolio. 

In this portfolio, I expect the market within this period to have small moves, thus hedging this portfolio from losses due to short gamma / short vega exposure. Theta can be collected from the shorted call, and holding this portfolio till maturity. Market makers might run these delta neutral positions when selling options on the market, collecting theta and hedging delta, provided risk management accounts for volatility.

<br/>

Yahoo Finance will be the website to scrape the financial data.

https://sg.finance.yahoo.com/

<br/>
The Nvidia call option will be assumed to be a European type call option. The risk-free interest rate r is calculated from subtracting the inflation rate of Singapore from the yield of a 1-Year Singapore T-bill (r = 0.0034, as of Oct-2024).

<br/>
<br/><b>Underlying stock and option used in this replicating portfolio:</b>

<br/>
<br/>
Underlying: NVDA (Nvidia Stock, with dividends)<br/>
https://sg.finance.yahoo.com/quote/NVDA/<br />

<br/>
Option: NVDA250117C00000500 (Nvidia Call Option with maturity 17-Jan-2025, assumed to be a European call)
https://sg.finance.yahoo.com/quote/NVDA250117C00000500/

<br />
<br />
The automation of the script can be seen in my personal portfolio:

https://briansim74-portfolio.webflow.io/projects/hedgingportfolio


<br/><b>This script utilises the following programs & languages:</b>

<b>Languages:</b>
1. Python
2. XML
3. Bash
4. SQL

<b>Programs:</b>
1. Google Colab - Developing the script
2. Selenium & ChromeDriver - Automation of XML Web Scraping
3. Ubuntu Linux - Running the script
4. Cron - Automation of script
5. pyodbc (Python Open Database Connectivity) - To truncate outdated data from SQL Azure database using SQL Query
6. BCP (Bulk Copy Program) Utility - Rapid bulk insert updated data into SQL Azure database
7. Microsoft Azure SQL Database - SQL Cloud database for updating replicating portfolio

<br/><b>The following files for reference for this script are:</b>
1. Yahoo_Full_Script.ipynb (Creation of Replicating Portfolio)
2. Yahoo.py (Script)
3. NVDA.csv
4. NVDA_Query.sql
5. cron.txt
6. Delta_Hedge.mp4

<br/><b>Developing the script</b>

First, I installed Selenium and Chromium Driver onto my Google Colab as well as Ubuntu Virtual Machine. Selenium and ChromeDriver are plug-ins to automate the execution of parsing XML data from the Yahoo Finance website.
Using Selenium, I scraped all the relevant data from the NVDA stock as well as the NVDA250117C00000500 call option. These include:

1. Stock Price
2. Strike Price
3. Annual Dividend Yield
4. Maturity of option
5. Annual Volatility (Calculated using 20-day volatility of NVDA stock)

After gathering all the relevant data from the underlying and call, I proceeded to calculate the remaining variables needed to create a replicating portfolio. These are:

1. Black-Scholes-Merton (BSM) price of the call option
2. Delta of option
3. Time to maturity of option (Years)

To calculate the time to maturity of the option, I used the pytz library to compute the current local time in New York and pegging it to the maturity time of 11:59:00 on 25-Jan-2025 as the maturity date and time.

<br/><b>Creating the replicating portfolio</b>

To create the replicating portfolio, I first calculated the delta of the option, which turned out to be 0.507 on 24-Oct-2024 11:21:59 (GMT -4). The replicating portfolio is made by shorting 1 NVDA NVDA250117C00000500 call option, and buying long 0.507 shares of the underlying. The remaining difference would be covered by shorting (borrowing from) the bank.

<br/>
Current call price: 14.70
<br/>
Delta: 0.534355093723429
<br/>
Value of shares = 0.534355093723429 x 139.79 = 74.69749855
<br/>
Amount borrowed from bank = 69.98 - Call Price = 74.69749855 - 14.70 = 59.99749855

<br />
The value of the replicating portfolio X(0) would then be delta number of shares - amount borrowed from bank = 0.534355093723429 * 139.79 - 59.99749855 = 14.70.

<br/><br/>
After that, I processed the relevant data into a Pandas DataFrame with relevant column names. I then exported the DataFrame into a CSV file to be stored in my Ubuntu Desktop.

Using the pyodbc driver, I then connected to the Microsoft Azure SQL cloud database where the dbo.NVDA SQL Table exists.

In the same script, I added a command to execute the BCP utility to bulk copy the updated CSV file into the dbo.NVDA SQL Table in the Microsoft Azure SQL database, whereby the database would then be updated with the most recent CSV file.

<br/><b>Running and Automation of the script</b>

I utilised Ubuntu Linux for the automation of the script. I started a new Cronjob on Crontab, an automatic task scheduler, whereby I set the Python script to run every minute, thus updating the Azure SQL database every minute with new data.

Each time the script is ran, it will rebalance the hedge, by calculating the current stock price, call price, time to maturity and delta. Each time the delta changes, the current number of shares would be updated to the value of the new delta, and the profit from selling the change in delta of shares would be paid back to the bank to cover the debt incurred, with interest.

<br/>
Current stock price = 144.10
<br/>
Current call price = 16.75
<br/>
Current delta: 0.599720535384939 (25-Oct-2024 11:22:27)
<br/>
New delta: 0.600312444512223 (25-Oct-2024 11:27:25)
<br/>
Change in delta = current delta - new delta = 0.599720535384939 - 0.600312444512223 = -0.0005919091273
<br/>
Profit from selling -0.0005919091273 shares = -0.0005919091273 * 144.10 = -0.08529410524
<br/>
Current amount owed to bank before / after interest (r = 0.0034) = 69.35388371 / 69.35388594
<br/>
New amount owed to bank = Amount after interest - profit from selling shares = 69.35388594 - (-0.08529410524) = 69.439180045658
<br/>
Current value of replicating portfolio = 0.600312444512223 * 144.10 - 69.439180045658 = 17.0658432085533
<br/>
Profit without hedging = Initial call price - current call price = 14.70 - 16.75 = -2.05
<br/>
Profit with hedging = Replicating portfolio - current call price = 17.0658432085533 - 16.75 = 0.3158432086

<br/><br/>
Each time, the profit would be calculated from the cost of closing the short position of the NVDA250117C00000500 option, by subtracting the current value of the replicating portfolio (wealth) by the current call price of the option.

Delta_Hedge.mp4 shows the automation of the script.

As shown above, if there was no hedging, the current profit of the portfolio after closing the short position of the call would be much less or even negative compared with the hedged position.

The current profit can be then analysed and set to automatically close this position once it reaches above a certain threshold (Eg. profit >= $12.00)

Finally, the automation of the Web Scraping Script can be seen by the updating of the Azure SQL database every 5 minutes by quering the Security Wise Holdings SQL Table from the Microsoft Azure Portal, preferably set to operating during the trading hours of the US stock market (9:30am to 4:00pm GMT-04).

</details>
</br></br>

</details>
</br></br>
