# Quantitative Finance Toolkit

A comprehensive Python toolkit for option pricing, portfolio simulation, and risk analysis.

## 🎯 Overview

This repository combines **two core quantitative finance frameworks** that share the same underlying mathematical foundation: **Geometric Brownian Motion (GBM)**.

| Module | Purpose | Key Features |
|--------|---------|--------------|
| **Option Pricing** | Black-Scholes-Merton model | Call/Put pricing, Greeks, Implied Volatility |
| **Monte Carlo Simulation** | Portfolio risk analysis | Price path simulation, VaR, Distribution analysis |

## 🧮 Mathematical Foundation

Both modules rely on **Geometric Brownian Motion**:

$$dS = \mu S \, dt + \sigma S \, dW$$

Where:
- $S$ : Asset price
- $\mu$ : Drift (expected return)
- $\sigma$ : Volatility
- $W$ : Wiener process (Brownian motion)

**Black-Scholes** uses GBM to derive closed-form option prices, while **Monte Carlo** simulates thousands of possible price paths to estimate portfolio distributions.

---

## 📁 Project Structure

```
quantitative-finance/
├── option_pricing/
│   ├── black_scholes.ipynb    # Full BS implementation with examples
│   └── README.md
│
├── monte_carlo_simulation/
│   ├── monte_carlo.py         # GBM simulator & portfolio analysis
│   ├── monte_carlo_demo.ipynb # Interactive demo notebook
│   └── README.md
│
├── notebooks/
│   └── combined_analysis.ipynb  # Cross-module examples
│
├── requirements.txt
└── README.md
```

---

## 🚀 Quick Start

### Installation

```bash
git clone https://github.com/YOUR_USERNAME/quantitative-finance.git
cd quantitative-finance
pip install -r requirements.txt
```

### Option Pricing (Black-Scholes)

```python
from option_pricing.black_scholes import BlackScholesModel

# Price a European Call
bs = BlackScholesModel(S=100, K=105, T=1, r=0.05, sigma=0.2, q=0.02)

print(f"Call Price: {bs.call_price():.4f}")  # 6.9869
print(f"Put Price:  {bs.put_price():.4f}")   # 8.8461

# Greeks
greeks = bs.greeks("call")
print(f"Delta: {greeks['delta']:.4f}")  # 0.4925
```

### Monte Carlo Simulation

```python
from monte_carlo_simulation.monte_carlo import MonteCarloPortfolio

# Simulate S&P 500 portfolio
mc = MonteCarloPortfolio(ticker="^GSPC", period="5y")
mc.estimate_parameters()

results = mc.run_simulation(
    initial_value=10000,
    T=1.0,
    n_simulations=10000
)

mc.print_report(results)
mc.plot_results(results)
```

---

## 📈 Module Details

### 1. Option Pricing (Black-Scholes-Merton)

Implements the classic BSM model for European options with continuous dividends.

**Features:**
- ✅ Call & Put pricing
- ✅ Greeks: Delta, Gamma, Vega, Theta, Rho
- ✅ Implied Volatility (Newton-Raphson)
- ✅ Put-Call Parity verification
- ✅ Payoff visualization

**Key Formulas:**

$$d_1 = \frac{\ln(S/K) + (r - q + \sigma^2/2)T}{\sigma\sqrt{T}}$$

$$d_2 = d_1 - \sigma\sqrt{T}$$

$$C = S e^{-qT} N(d_1) - K e^{-rT} N(d_2)$$

### 2. Monte Carlo Simulation

Simulates future portfolio values using GBM to estimate risk metrics.

**Features:**
- ✅ Historical data download (via yfinance)
- ✅ Parameter estimation (μ, σ)
- ✅ GBM path simulation
- ✅ Value at Risk (VaR)
- ✅ Distribution visualization

**GBM Discrete Approximation:**

$$S_{t+\Delta t} = S_t \exp\left[\left(\mu - \frac{\sigma^2}{2}\right)\Delta t + \sigma\sqrt{\Delta t}\,Z\right]$$

where $Z \sim \mathcal{N}(0,1)$

---

## 📊 Sample Output

### Monte Carlo Results

```
============================================================
MONTE CARLO SIMULATION REPORT
============================================================

Parameters:
  Ticker: ^GSPC
  Initial Investment: $10,000.00
  Time Horizon: 1 year(s)
  Simulations: 10,000
  Annual Return (μ): 12.45%
  Annual Volatility (σ): 18.32%

            Portfolio Value Statistics            
--------------------------------------------------
  Mean:     $   11,245.32
  Median:   $   10,987.15
  VaR 95%:  $    8,234.56

            Risk Metrics            
--------------------------------------------------
  Prob. of Profit:     68.42%
  Prob. of >10% Loss:  12.15%
```

---

## 🔗 How the Modules Connect

1. **Shared Foundation**: Both use GBM for asset dynamics
2. **Complementary Use Cases**:
   - Black-Scholes: Closed-form pricing for vanilla options
   - Monte Carlo: Pricing exotic options, portfolio simulation
3. **Parameter Consistency**: Use the same μ and σ across modules

```python
# Example: Use historical vol in Black-Scholes
mc = MonteCarloPortfolio("AAPL")
mu, sigma = mc.estimate_parameters()

bs = BlackScholesModel(S=150, K=155, T=0.5, r=0.05, sigma=sigma)
print(f"Option price with historical vol: {bs.call_price():.2f}")
```

---

## 📚 References

- Black, F., & Scholes, M. (1973). *The Pricing of Options and Corporate Liabilities*
- Merton, R. C. (1973). *Theory of Rational Option Pricing*
- Glasserman, P. (2003). *Monte Carlo Methods in Financial Engineering*

---

## ⚠️ Disclaimer

This toolkit is for **educational purposes only**. The Black-Scholes model has well-known limitations:
- Assumes constant volatility
- Assumes log-normal returns
- Ignores market microstructure

As Warren Buffett noted, these models can give "an illusion of precision" — always combine quantitative analysis with fundamental understanding.

---

## 📝 License

MIT License - feel free to use and modify.

---

## 👤 Author

**Rayan** - Masters in Statistics & Econometrics, University of Strasbourg

*Building quantitative tools for finance applications*
