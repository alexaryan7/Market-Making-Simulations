# Market-Making Strategy Simulation

## Overview
This project simulates and evaluates **market-making strategies** in two different environments:
1. **Synthetic Market** — a controlled simulation of price movements using Brownian motion.
2. **Real Market** — replaying historical tick-level data from a real-world security (e.g., AAPL) using `yfinance`.

Market-making is a trading approach that provides liquidity by continuously quoting both buy (bid) and sell (ask) prices. The goal is to capture the bid-ask spread while managing inventory risk and adapting to market conditions.

---

## Objectives
- Implement and test **market-making algorithms** under different conditions.
- Evaluate **profitability, risk, and inventory dynamics**.
- Compare performance between **synthetic** and **real** market data.
- Analyze sensitivity to spread width, inventory risk aversion, and market volatility.

---

## Project Structure
├── market_making_synthetic.ipynb # Synthetic market simulation
├── market_making_real.ipynb # Real market replay (AAPL)
├── README.md # This file
├── requirements.txt # Dependencies

yaml
Copy
Edit

---

## Strategies Implemented

### 1. Naive Market Maker (NaiveMM)
A simple baseline strategy:
- Quotes a **fixed bid** and **fixed ask** around the mid-price.
- Spread is constant throughout the simulation.
- **Pros:** Easy to implement, works well in stable, mean-reverting markets.
- **Cons:** No inventory control — vulnerable to trending markets.

**Formula:**
\[
\text{Bid} = \text{mid} - \frac{\text{spread}}{2}, \quad
\text{Ask} = \text{mid} + \frac{\text{spread}}{2}
\]

---

### 2. Inventory-Aware Market Maker (InventoryAwareMM)
An enhanced strategy that adjusts quotes based on current inventory:
- Introduces a **skew** proportional to the current position size.
- If inventory is long → lowers both bid and ask to encourage selling.
- If inventory is short → raises both bid and ask to encourage buying.
- **Pros:** Controls position risk, more robust to trends.
- **Cons:** Parameter tuning required for `risk_aversion`.

**Formula:**
\[
\text{Skew} = \text{risk\_aversion} \times \text{inventory}
\]
\[
\text{Bid} = \text{mid} - \frac{\text{spread}}{2} - \text{Skew}
\]
\[
\text{Ask} = \text{mid} + \frac{\text{spread}}{2} - \text{Skew}
\]

---

## Simulation Environments

### 1. Synthetic Market
- Price process simulated using **Brownian motion** with configurable drift and volatility.
- Useful for testing strategies in a controlled, repeatable environment.
- Allows extreme scenario testing (high volatility, low liquidity).

**Parameters:**
- Initial price
- Time steps
- Drift
- Volatility

---

### 2. Real Market (AAPL)
- Tick-level (or high-frequency) historical trade data fetched via `yfinance`.
- Replay of historical order flow — strategies place quotes and fill based on mid-price.
- Configurable:
  - **Ticker** (e.g., `AAPL`, `BTC-USD`, `ES=F`)
  - **Start/End date**
  - **Spread** and **risk parameters**

---

## Performance Evaluation
Both synthetic and real data simulations track:
- **P&L** (profit and loss) over time.
- **Inventory levels** — to measure exposure risk.
- **Market impact** — price reaction to large position shifts.
- **Response to liquidity shocks** — how spreads and fills behave when volatility spikes.

Plots generated:
- Price series with executed trades.
- P&L curve.
- Inventory trajectory.

---

## Example Output
**Synthetic Market Example:**
- Smooth, controlled price series.
- NaiveMM generates steady spread capture but large inventory swings in trends.
- InventoryAwareMM shows smaller swings with slightly reduced P&L.

**Real Market Example (AAPL):**
- More volatile, clustered price moves.
- Fill patterns depend on actual historical volatility and order book dynamics.
- InventoryAwareMM outperforms NaiveMM in risk-adjusted returns.

---

## Dependencies
- Python 3.9+
- `numpy`
- `pandas`
- `matplotlib`
- `yfinance`

Install via:
```bash
pip install -r requirements.txt
How to Run
Synthetic Market
bash
Copy
Edit
jupyter notebook market_making_synthetic.ipynb
Real Market
Edit the ticker, start_date, and end_date parameters in:

python
Copy
Edit
ticker = "AAPL"
start_date = "2023-06-01"
end_date = "2023-06-05"
Then run:

bash
Copy
Edit
jupyter notebook market_making_real.ipynb
