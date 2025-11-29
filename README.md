# MicroStrategy Bitcoin Timing Analysis (Monte Carlo Simulation)

This repository evaluates whether MicroStrategy’s 2025 Bitcoin purchases exhibited any timing skill relative to a random-timing benchmark. We use a Monte Carlo simulation with 10,000 iterations to compare MicroStrategy’s actual volume-weighted average purchase price (VWAP) to the distribution of VWAPs generated using randomly selected purchase dates.

The goal: **determine whether MicroStrategy bought Bitcoin at prices better, worse, or no different than what a typical investor would have achieved with random purchase timing**.

---

## 🧠 Background

MicroStrategy is one of the world’s largest institutional holders of Bitcoin. The company frequently purchases BTC in large tranches across different dates.  
A key analytical question arises:

> _Does MicroStrategy demonstrate skill in timing these Bitcoin purchases?_

Or put differently:

> _Are MicroStrategy’s BTC acquisitions systematically cheaper than random purchases made in the same time window?_

This project answers that question using **real transaction data** and **real BTC daily price data** (cleaned from CSV).

---

## 📊 Data Sources

### 1. **MicroStrategy 2025 BTC Purchases**
Extracted from public disclosures (17 transactions between July–November 2025).  
For each transaction we used:

- Date of purchase  
- BTC acquired  
- Acquisition cost in USD (millions → converted to dollars)  

### 2. **BTC Daily Prices (May–November 2025)**  
Cleaned from CSV:

- Fixed corrupted column names (`Close`, `Adj Close`)  
- Removed commas in numerical fields  
- Converted all price fields to floats  
- Parsed and sorted dates  
- Forward-filled any weekend gaps  

Final dataset contained reliable daily close prices.

---

## 🧮 Methodology

We calculate MicroStrategy’s actual VWAP:

\[
\text{VWAP}_{\text{actual}} = 
\frac{\sum \text{Dollar Outlay}_i}
     {\sum \text{BTC Acquired}_i}
\]

Then we simulate **10,000 alternate scenarios**:

### 🔁 Monte Carlo Simulation Steps
For each simulation:

1. Randomly pick **the same number of purchase dates (17)** as MicroStrategy.  
2. Use MicroStrategy’s **real dollar outlays** for each simulated purchase.  
3. Buy BTC at the **BTC price of each randomly selected date**.  
4. Compute the simulated VWAP for that run:

\[
\text{VWAP}_{\text{sim}} = 
\frac{\sum \text{Dollar Outlay}_i}
     {\sum \text{BTC Purchased}_{\text{sim}, i}}
\]

Collect 10,000 simulated VWAPs → this becomes our benchmark distribution.

---

## 📈 Key Results

### 🎯 **Final Statistics**

| Metric | Value |
|-------|-------|
| **Actual VWAP** | **\$113,653** |
| **Mean Simulated VWAP** | **\$109,665** |
| **Std Dev of Simulated VWAP** | **\$3,973** |
| **Percent Simulations Better (Lower VWAP)** | **86.11%** |
| **Percent Simulations Worse (Higher VWAP)** | **13.89%** |

### 🎙 Interpretation

- MicroStrategy’s VWAP lies at the **86th percentile** of the simulated distribution.  
- This means **86%** of random-timing strategies produced a **better (lower)** VWAP than MicroStrategy.  
- The actual VWAP was roughly **one full standard deviation above** the Monte Carlo mean.  
- **Conclusion:**  
  > MicroStrategy did **not** exhibit positive timing skill in this period.  
  > Their purchases were systematically more expensive than most random purchase strategies.

---

## 📊 Visualizations

The notebook includes:

### ✔ Histogram of 10,000 VWAP simulations  
Shows MicroStrategy’s actual VWAP as a vertical red line vs. the distribution.

### ✔ KDE Density Plot  
A smooth, professional curve highlighting how far the actual VWAP sits to the right.

### ✔ CDF Plot (Cumulative Distribution Function)  
Shows percentile ranking of the actual VWAP relative to the simulation.

### ✔ Summary Table (bolded)  
Displays all key results cleanly and concisely.

---

## 📁 Repository Structure

