# MonteCarlo Wealth Lab

A sophisticated, browser-based Monte Carlo simulation tool for retirement planning and wealth accumulation analysis. This single-file HTML application models investment growth under uncertainty using correlated asset returns, tax considerations, and strategic cash management.

## 🎯 Overview

The MonteCarlo Wealth Lab helps investors and financial planners understand the probability distribution of future wealth outcomes by running thousands of simulated investment scenarios. It accounts for:

- **Correlated asset returns** across multiple investment funds
- **Systematic investment plans (SIP)** with step-up rates
- **War chest strategy** with tactical deployment rules
- **Tax implications** (LTCG calculations)
- **Inflation adjustments** for real purchasing power
- **Age-based milestones** and target corpus planning

## 🚀 Quick Start

1. **Open the simulator**: Simply open `MonteCarlo.html` in any modern web browser
2. **Configure your inputs**: Adjust the simulation parameters in the left sidebar
3. **Set up your portfolio**: Configure fund allocations, expected returns, and volatility
4. **Run simulation**: Click "RUN SIMULATION" to generate 5,000+ Monte Carlo paths
5. **Analyze results**: Review the comprehensive output including fan charts, probability curves, and milestone analysis

**No installation required** - this is a standalone HTML file that runs entirely in your browser.

## 📊 Key Features

### Simulation Engine
- **5,000+ Monte Carlo simulations** by default (configurable up to 50,000)
- **Correlated asset modeling** using Cholesky decomposition
- **Seeded random number generation** for reproducible results
- **Monthly time-step resolution** for accurate compounding

### Investment Strategy Modeling
- **Multi-fund portfolio allocation** (up to 10 funds)
- **Systematic Investment Plans (SIP)** with customizable step-up rates
- **War chest strategy** with tactical deployment based on market drawdowns
- **Age-based milestone tracking** (20%, 50%, 80%, 100% of target age)

### Financial Calculations
- **LTCG tax calculations** based on current Indian tax laws
- **Inflation-adjusted real returns**
- **After-tax corpus projections**
- **Cost basis tracking** for tax calculations

### Comprehensive Visualization
- **Fan chart** showing percentile ranges over time
- **Histogram distribution** at target age
- **Probability curves** for hitting various wealth targets
- **Milestone tables** with detailed statistics
- **Outcome distribution** breakdown by wealth brackets

## 🛠️ User Guide

### Simulation Parameters

#### Basic Settings
- **Simulations**: Number of Monte Carlo paths (500-50,000)
- **Current Age**: Your current age (20-50)
- **Target Age**: Age at which you want to achieve your target corpus (30-65)
- **Random Seed**: For reproducible results (1-999)

#### Starting Position
- **Corpus**: Initial investment amount in lakhs (₹L)
- **War Chest Initial**: Initial cash reserve in lakhs (₹L)

#### Monthly SIP Configuration
- **Base SIP**: Monthly investment amount in thousands (₹K)
- **Step-up Rate**: Annual percentage increase in SIP (0-30%)
- **Step-up Every**: Frequency of SIP increases in months (6-24)

#### War Chest Strategy
- **Monthly Addition**: Monthly cash addition to war chest in thousands (₹K)
- **Parking Return**: Annual return on parked cash (3-12%)
- **Cooldown**: Minimum months between deployments (1-18)
- **Deployment Rules**: Percentage of war chest to deploy at various drawdown levels (-5%, -10%, -20%, -30%)

#### Tax & Inflation
- **LTCG Rate**: Long-term capital gains tax rate (5-30%)
- **Annual Exemption**: Tax-free LTCG amount in thousands (₹K)
- **Inflation Rate**: Expected annual inflation (2-15%)

#### Target
- **Target Corpus**: Desired wealth at target age in crores (₹Cr)

### Fund Configuration

The simulator supports up to 10 different investment funds:

1. **Add/Remove Funds**: Use the "Add Fund" button or remove existing funds
2. **Weight Allocation**: Each fund's percentage of total portfolio (must sum to 100%)
3. **Expected Return**: Annual expected return for the fund (5-35%)
4. **Volatility**: Annual standard deviation of returns (5-65%)

**Note**: The simulator includes a predefined correlation matrix to model realistic relationships between different asset classes.

### Interpreting Results

#### Key Metrics
- **Median @ Age**: Most likely outcome at specified ages
- **P(≥Target) @ Age**: Probability of achieving your target corpus
- **Worst 5%**: Floor protection level (5th percentile)

#### Charts Explained
- **Fan Chart**: Shows wealth distribution over time with percentile bands
- **Histogram**: Distribution of final wealth outcomes
- **Probability Curve**: Likelihood of hitting various wealth targets
- **Milestone Table**: Detailed statistics at key ages

#### Risk Analysis
- **Downside Risk**: Probability of poor outcomes
- **Outcome Distribution**: Percentage of simulations in different wealth brackets
- **Real vs Nominal**: Inflation-adjusted purchasing power

## 🔧 Technical Documentation

### Architecture Overview

The application is built as a single HTML file with three main components:

1. **HTML Structure**: Semantic markup with comprehensive styling
2. **CSS Styling**: Custom dark theme with responsive design
3. **JavaScript Logic**: Complete simulation engine and visualization

### Mathematical Models

#### Monte Carlo Simulation
```javascript
// Core simulation loop
for (let s = 0; s < numSims; s++) {
  let corpus = startCorpus;
  let wc = wcInit;
  
  for (let m = 1; m <= totalMonths; m++) {
    // Generate correlated returns
    const returns = generateCorrelatedReturns();
    
    // Apply investment strategy
    corpus = corpus * (1 + blendedReturn) + monthlySIP;
    wc = wc * (1 + parkingReturn) + monthlyWCAddition;
    
    // Tactical deployment logic
    if (marketDrawdown > threshold && cooldownPeriodPassed) {
      deployFromWarChest();
    }
  }
}
```

#### Correlation Modeling
The simulator uses Cholesky decomposition to generate correlated random variables:

```javascript
function cholesky(C) {
  const n = C.length;
  const L = Array.from({length:n}, () => new Float64Array(n));
  // Cholesky factorization implementation
  return L;
}
```

#### Tax Calculations
LTCG tax is calculated as:
```
Taxable Gain = Gross Corpus - Cost Basis
Tax = max(0, (Taxable Gain/15 - Exemption)) * LTCG Rate * 15
After-Tax = Gross - Tax
```

### Code Structure

#### Core Functions
- `runMonteCarlo()`: Main simulation engine
- `generateCorrelatedReturns()`: Multi-asset return generation
- `calculateMilestoneMonths()`: Age-based milestone calculation
- `renderResults()`: Visualization and output generation

#### Utility Functions
- `mulberry32()`: Seeded random number generator
- `boxMuller()`: Normal distribution sampling
- `pct()`: Percentile calculation
- `mean()`, `std()`: Statistical functions

#### Chart Integration
Uses Chart.js for all visualizations:
- Line charts for fan plots and probability curves
- Bar charts for histograms and distributions
- Custom styling to match the application theme

### Performance Considerations

- **Chunked Processing**: Simulations run in chunks to prevent browser freezing
- **Web Workers**: Could be implemented for very large simulation counts
- **Memory Management**: Charts are destroyed and recreated to prevent memory leaks
- **Responsive Design**: Charts resize automatically on window changes

## 💡 Use Cases & Examples

### Retirement Planning
**Scenario**: 30-year-old investor targeting ₹10 crore by age 60
- Initial corpus: ₹0
- Monthly SIP: ₹50K with 10% annual step-up
- War chest: ₹25K monthly addition
- Portfolio: 60% equity funds, 40% debt funds

**Analysis**: The simulator shows probability of success, median outcomes, and downside protection.

### Wealth Accumulation
**Scenario**: 35-year-old with existing ₹50L corpus targeting ₹5 crore in 15 years
- Starting corpus: ₹50L
- Monthly SIP: ₹30K
- Step-up rate: 8% annually
- War chest strategy for market timing

**Analysis**: Compare different asset allocations and step-up strategies.

### Education Fund Planning
**Scenario**: Parent planning for child's education in 18 years
- Target: ₹2 crore
- Time horizon: 18 years
- Conservative portfolio allocation
- Regular monthly contributions

**Analysis**: Assess probability of meeting education funding goals.

## ⚠️ Limitations & Assumptions

### Model Limitations
- **Normal Distribution**: Returns are modeled as normally distributed (real markets have fat tails)
- **Constant Correlations**: Correlation matrix is static over time
- **No Black Swans**: Extreme market events are not explicitly modeled
- **Tax Simplification**: Actual tax calculations may be more complex

### Financial Assumptions
- **Constant Inflation**: Inflation rate is assumed constant over the simulation period
- **No Additional Contributions**: Beyond SIP, no lump sum additions are modeled
- **No Withdrawals**: No systematic withdrawals during the accumulation phase
- **Fixed Asset Allocation**: Portfolio weights remain constant over time

### Technical Constraints
- **Browser Limitations**: Very large simulation counts may cause performance issues
- **Single-threaded**: JavaScript runs on main thread (no parallel processing)
- **Memory Usage**: Large simulation counts consume significant browser memory

## 🔍 Troubleshooting

### Common Issues

#### Simulation Won't Start
- **Cause**: Fund weights don't sum to 100%
- **Solution**: Adjust fund allocations to total exactly 100%

#### Slow Performance
- **Cause**: Too many simulations (over 10,000)
- **Solution**: Reduce simulation count or use a more powerful device

#### Charts Not Displaying
- **Cause**: Browser blocking external script loading
- **Solution**: Ensure Chart.js CDN is accessible or use offline version

#### Unexpected Results
- **Cause**: Extreme input values (very high returns or volatility)
- **Solution**: Use realistic input parameters based on historical data

### Validation System
The simulator includes comprehensive input validation:
- Age ranges and relationships
- Financial input reasonableness
- Fund allocation constraints
- Extreme parameter warnings

## 🤝 Contributing

While this is a single-file application, contributions are welcome for:

1. **Bug Reports**: Issues with calculations or visualizations
2. **Feature Requests**: New simulation capabilities or chart types
3. **Documentation**: Improvements to this README or inline code comments
4. **Performance**: Optimizations for large simulation counts

## 📄 License

This project is provided as-is for educational purposes. Users should consult with qualified financial advisors before making investment decisions.

## 🙏 Acknowledgments

- **Chart.js**: For excellent charting capabilities
- **Mulberry32**: For high-quality seeded random number generation
- **Financial Modeling Community**: For established Monte Carlo methodologies

## 📞 Support

For questions about using the simulator:
1. Check the troubleshooting section above
2. Review the input parameter explanations
3. Ensure all validation requirements are met
4. Start with the default configuration and make gradual changes

---

**⚠️ Disclaimer**: This tool is for educational purposes only. It does not constitute SEBI-registered investment advice. Actual investment returns may differ significantly from simulated results. Always consult a qualified financial advisor and tax professional before making investment decisions.