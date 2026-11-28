# 🎓 Capstone: The Out-of-Sample Forecasting Challenge



In the previous notebook, *Using Futures to Isolate Sources of Risk and Return*, we explored how to synthetically replicate the risk profile of benchmark equity portfolios. We established a toolkit of four highly liquid futures contracts (ES, NQ, RTY, and YM) and discussed how out-of-sample forecasting is the true test of a model's real-world viability.

We also noted that this framework has **Universal Application**. Once you understand the mechanics of return spanning, you can use these exact same liquid contracts to synthesize a wide variety of custom portfolios.

Now, it is time to put that theory into practice.

### 🎯 The Mission
You want to grab a partner or partners to work with, because in this capstone challenge, you will step into the roles of quantitative researchers. Your objective is to collaborate and use our four-contract futures toolkit to replicate the risk and return profiles of two distinct sector portfolios: **S&P 500 Financials** and **S&P 500 Information Technology**.


:::{admonition} 📚 Learning Objectives
:class: tip

By the end of this challenge, you will be able to:

**Data Pipeline & Return Preparation**
* **Acquire and align** multi-source time-series data for futures contracts, target sector indices, and the SOFR index into a clean, analysis-ready dataset.
* **Handle** time-series data inconsistencies—such as mismatched trading calendars, missing values, and date alignment—when joining futures, index, and rate series.
* **Calculate** periodic excess returns by stripping out the estimated SOFR funding rate to isolate each asset's underlying risk premium.

**Portfolio Replication & Modeling**
* **Formulate** a synthetic portfolio replication problem by regressing target sector index excess returns against a basket of futures excess returns.
* **Estimate** optimal futures hedge ratios and portfolio weights using Ordinary Least Squares (OLS) regression, and interpret the resulting factor loadings and fit statistics.

**Out-of-Sample Validation & Performance Evaluation**
* **Generate** out-of-sample return forecasts by applying in-sample OLS weights to unseen test data to simulate real-world portfolio execution.
* **Quantify and visualize** the cumulative tracking error between the synthetic futures portfolio and the target sector index to evaluate replication accuracy over time.
:::
---

### 🏁 The Final Destination
If you're ready to go, get started! Otherwise, five guided steps are suggested below. Each step provides some guidance to help you navigate; however, if you get stuck, you can click the solution button at the bottom of the document.

Before you get started, click below to see the final destinations.

<details>
<summary><b>📈 The Financial Portfolio Graph</b></summary>
  <img src="https://raw.githubusercontent.com/PatrickJHess/Static_Data_Repo/main/images/SPFIN.png" alt="Financial Portfolio Tracking Error" width="800"/>
</details>

<details>
<summary><b>💻 The Information Technology Portfolio Graph</b></summary>
  <img src="https://raw.githubusercontent.com/PatrickJHess/Static_Data_Repo/main/images/SPINFOTECH.png" alt="Information Technology Portfolio Tracking Error" width="800"/>
</details>

<br>

*Ready to build your models? Setup your environment, and let's get to work!*



### 🛠️ The Starting Point

In order to complete the challenge, you'll need access to the modules and functions from the notebook **Using Futures to Isolate Sources of Risk and Return**.

Here is a reminder of what you need to set up:

**1. Imports and Instantiations**
* Import `financial_quant` as `fq`
* Import `seaborn` as `sns`
* Import `statsmodels.api` as `sm`
* Instantiate the following:
  * `fred_data` using `fq.FredReader()`
  * `massive_data` using `fq.MASSIVEReader(calls_per_minute=5)`
  * `static_futures_data` from `fq.static_futures_data`
  * `fq.futures_session_end_index`

**2. Required Functions**
Ensure the following custom functions defined in the **Using Futures to Isolate Sources of Risk and Return** notebook are loaded into this:
* `futures_data`
* `predict_actual`
* `plot_tracking_error`

*Tip: It is highly recommended that you run the setup and import these functions before you attempt to complete this challenge.*



### 2 &ensp;🧮 Create Excess Return DataFrame

#### 2.a&ensp; Calculate Percentage Changes
<blockquote>
Because the futures contracts overlap, you will actually generate two percentage changes on the roll date. Here is how to handle them:

- **The first instance** is the correct return (calculated purely on the outgoing contract).
- **The second instance** is a bad data point (it calculates a "return" by mixing the price of the old contract with the new contract).

You need to drop that second row. You can easily filter it out by negating the duplicated dates in your index using <code>~df.index.duplicated(keep='first')</code>. Notice how easy Pandas makes it to manipulate time-series data!
</blockquote>


#### 2.b&ensp; ➖💸 Calculate Excess Returns
<blockquote>
Here is your checklist:

1. **Isolate the Futures:** Copy the four futures columns into a new DataFrame (these are already excess returns).
2. **Subtract SOFR:** Use the Pandas <code>sub()</code> method to subtract the <code>sofr_index</code> column from the two sector portfolio columns (<code>SPFINTR_Index</code> and <code>SPTECHTR_Index</code>) along the row axis (<code>axis=0</code>).
3. **Combine and Clean:** Add these newly calculated excess returns to your DataFrame and drop any missing values.
</blockquote>


### 3.&ensp;⚖️ Use OLS to Estimate Futures Allocations
<blockquote>
When you fit the modelyou only need to extract three specific attributes to measure your success:

- **The Allocations:** Use <code>results.params</code> to get the constant and the computed weights for each futures contract.
- **The Standard Errors:** Use <code>results.bse</code> to check the statistical standard error of those allocations.
- **The R-Squared:** Use <code>results.rsquared</code> to measure how accurately your futures basket replicates the target portfolio.

*Run this regression process for both the Financials and Information Technology portfolios to compare their weights.*
</blockquote>


### 4.&ensp;🛤️ Calculate the Cumulative Tracking Error
<blockquote>
First, you need to set your training window. Calculate the number of observations (<code>NOBS</code>) by slicing your actual excess returns up to and including the roll date of March 12, 2026. This ensures the model initially trains on data outside of your forecast period. Keep <code>dropna()</code> so you don't accidentally include missing data.

Here is your checklist:

- **Calculate NOBS:** Slice your target portfolio data up to <code>'2026-03-12'</code> and get the length using <code>len()</code>.
- **Generate Forecasts:** Pass your target port        - title: 'Chapters Four: 📊 Fixed Income Futures'
          url: https://patrickjhess.github.io/Derivatives-Futures-Chapter-Four/folio and futures data into <code>predict_actual()</code> to get the forecasted and actual returns.
- **Calculate Cumulative Error:** Subtract the forecast from the actual returns, add 1, and use Pandas <code>cumprod()</code> to construct an index representing your tracking error over time.
- **Repeat:** Do this for both the Financials and Information Technology portfolios.
</blockquote>

### 5.&ensp;📊 Ready To Graph
<blockquote>
You have made it to the finish line! All the heavy lifting is done.

Now, just pass your cumulative tracking error data into the custom <code>plot_tracking_error</code> function provided earlier in the notebook. Be sure to label them clearly using the <code>portfolio_name</code> argument.

If your data pipeline and forecasting models worked correctly, you ought to get the exact same charts shown at the very top of this challenge. Great work!
</blockquote>

:::{dropdown} 🔑 Here Is A Solution (Click to Expand)

```{include} Chapter_Three.md

:::
