# Financial Python

## 📚 Volume: Futures Markets

## 📊 Synthetic Portfolios and Benchmark Risk Spanning
This chapter bridges the gap between theoretical index replication and practical risk management by demonstrating how liquid equity futures can be used to synthesize, span, and forecast benchmark portfolios. We begin by constructing a synthetic S&P 500 contract and its mirror—a synthetic cash position—revealing how tracking error is dynamically bound by arbitrage bands tied to market volatility (VIX). Building on these core mechanics, we expand our scope to multi-index risk spanning, utilizing four major equity contracts (S&P 500, Nasdaq 100, Russell 2000, and Dow Jones) to map the risk-return profiles of five key style and size benchmarks, complete with out-of-sample predictions. Finally, a capstone exercise synthesizes these techniques to span the S&P Financials and Information Technology sectors, rigorously calibrating the out-of-sample forecasts to solidify the framework's practical application.

## **🎛️ From Cost of Carry to Risk Control and Synthetic Positions**
This notebook bridges theory and application by taking the cost of carry equation developed in Chapter Two and modifying it to include a crucial real-world friction: the dynamic arbitrage band ($\Delta_t$). We explore how this modified equation serves as the mechanical foundation for using futures to control portfolio risk and build synthetic positions. Crucially, we dive into the mechanics of $\Delta_t$ as a moving target that heavily depends on market sentiment. You will see how the financial infrastructure tilts under pressure, causing the arbitrage bands to widen in the specific direction of market stress:

- **Risk-On (Positive Friction):** When markets surge, futures become expensive as Cash & Carry arbitrageurs drive up prime broker lending rates.

- **Risk-Off (Negative Friction):** When markets panic, futures become cheap as Reverse Cash & Carry arbitrageurs spike the borrow fees on the underlying equities.

Understanding how these dynamic bands expand and contract is the essential first step in mastering synthetic replication and understanding tracking error.

## **🧭 Using Futures to Isolate Sources of Risk and Return**
Moving beyond a single index, this notebook expands our strategy to capture and span the broader risk-return spectrum. While factor investing traditionally relies on directly trading baskets of stocks, we demonstrate how to synthetically replicate these target exposures using a core toolkit of futures.

We filter the universe of U.S. stock index contracts down to four highly liquid instruments: the E-mini S&P 500, Nasdaq 100, Russell 2000, and Dow Jones. Using this four-contract toolkit, we map out the risk profiles of five distinct benchmark portfolios:

- **Market & Size Benchmarks:** The S&P 500, S&P MidCap 400, and S&P SmallCap 600.

- **Style Factors:** S&P 500 Growth and S&P 500 Value.

Crucially, we evaluate the success of this technique using real-world constraints. Rather than relying on static in-sample fitting, we judge the performance based on how accurately rolling estimates forecast future benchmark returns out-of-sample. Once you understand the mechanics of this universal, plug-and-play framework, you can use these exact same liquid contracts to synthesize and forecast a wide variety of other sectors or custom portfolios.

## **🎓 Capstone: The Out-of-Sample Forecasting Challenge**
In the final Notebook, you will put the theory of return spanning to the ultimate test. Stepping into the role of a quantitative researcher, your mission is to apply our four-contract futures toolkit to replicate the risk and return profiles of two distinct equity sectors: S&P 500 Financials and S&P 500 Information Technology.

This collaborative capstone exercise guides you through the complete lifecycle of a real-world quantitative workflow. You will:

- **Build a Data Pipeline:** Fetch, clean, and join time-series data for the futures contracts, target sectors, and the SOFR index.

- **Calculate Excess Returns:** Isolate true risk premiums by stripping out the SOFR-estimated funding rate.

- **Model Asset Allocation:** Use Ordinary Least Squares (OLS) regression to estimate the optimal weights for your synthetic futures basket.

- **Evaluate Real-World Performance:** Move beyond simple in-sample fitting to generate out-of-sample forecasts and visualize the cumulative tracking error of your synthesized sectors.

With guided steps, hidden solutions to unblock you, and clear visual targets to aim for, this notebook solidifies your ability to construct, calibrate, and test synthetic portfolios from the ground up.

## 🎯 The Jupyter Book Experience

Think of this Jupyter Book as a unique hybrid learning tool. It navigates like a modern website and reads with the narrative flow of an ebook, but packs the interactive computational power of Jupyter and Google Colab. By allowing you to manipulate real-world data and execute live code directly on the page, it seamlessly bridges the gap between abstract financial theory and applied modeling.

:::{dropdown} 🪐 How to Interact with Jupyter Books (Click to Expand)
:color: primary

```{include} jupyter_instructions.md
:::
