# Asset Specifications for L-62: Investment Portfolio Strategies

## Day 1 Assets

### 1. Asset Allocation Visual Calculator
- **Purpose:** Interactive tool demonstrating how different stock/bond splits affect portfolio risk and return
- **Format/Inputs:** Slider-based interface with real-time calculation and visualization
- **Expected Outputs:** Expected return, volatility range, historical worst/best years, allocation pie chart
- **Interaction Model:**
  - Single continuous slider from 0% stocks / 100% bonds to 100% stocks / 0% bonds
  - Real-time updates as slider moves (no "calculate" button needed)
  - Displays four key metrics instantly:
    - Expected annual return (%)
    - Expected volatility (standard deviation %)
    - Historical worst year (%)
    - Historical best year (%)
  - Visual pie chart shows current allocation split
  - Color coding: Stocks (blue), Bonds (green)
  - Hover over metrics for explanatory tooltips
- **Design Notes:**
  - **Slider Design:**
    - Horizontal slider with thumb control
    - Tick marks at 0%, 20%, 40%, 60%, 80%, 100%
    - Current value displayed prominently above slider
    - Common allocations labeled: "Conservative (20/80)", "Moderate (60/40)", "Aggressive (80/20)"
  - **Metric Display Cards:**
    - Four cards in 2×2 grid layout
    - Large primary number (e.g., "7.8%")
    - Descriptive label below ("Expected Return")
    - Icon indicator for good/moderate/concerning levels
    - Color coding: Green (low risk), Yellow (moderate), Orange (high)
  - **Allocation Data (Based on Historical Averages 1926-2023):**
    - 0% stocks / 100% bonds: Return 5.2%, Volatility 5.7%, Worst -8.1% (1969), Best 32.6% (1982)
    - 20% stocks / 80% bonds: Return 5.9%, Volatility 7.1%, Worst -10.1% (1969), Best 32.4% (1982)
    - 40% stocks / 60% bonds: Return 6.7%, Volatility 9.3%, Worst -14.2% (1931), Best 32.3% (1982)
    - 60% stocks / 40% bonds: Return 7.8%, Volatility 12.0%, Worst -26.6% (2008), Best 36.7% (1933)
    - 80% stocks / 20% bonds: Return 8.9%, Volatility 15.4%, Worst -34.9% (2008), Best 45.4% (1933)
    - 100% stocks / 0% bonds: Return 10.1%, Volatility 18.5%, Worst -43.1% (1931), Best 53.9% (1933)
  - **Pie Chart:**
    - Responsive SVG or Canvas element
    - Smooth transitions as allocation changes
    - Percentage labels inside slices
    - Legend below with exact percentages
  - **Educational Callouts:**
    - Below 40% stocks: "Lower risk, more stable, but slower growth"
    - 40-70% stocks: "Balanced approach for most long-term investors"
    - Above 70% stocks: "Higher growth potential but significant volatility"
- **Technical Specifications:**
  ```javascript
  // Allocation data structure
  const allocationData = {
    0: { return: 5.2, volatility: 5.7, worst: -8.1, best: 32.6 },
    10: { return: 5.6, volatility: 6.4, worst: -9.0, best: 32.5 },
    20: { return: 5.9, volatility: 7.1, worst: -10.1, best: 32.4 },
    // ... (data for each 10% increment)
    100: { return: 10.1, volatility: 18.5, worst: -43.1, best: 53.9 }
  };

  // Linear interpolation for intermediate values
  function calculateMetrics(stockPercent) {
    const lower = Math.floor(stockPercent / 10) * 10;
    const upper = Math.ceil(stockPercent / 10) * 10;
    const ratio = (stockPercent - lower) / 10;

    const metrics = {};
    for (let key in allocationData[lower]) {
      metrics[key] = allocationData[lower][key] +
        (allocationData[upper][key] - allocationData[lower][key]) * ratio;
    }
    return metrics;
  }

  // Update display in real-time
  function updateDisplay(stockPercent) {
    const metrics = calculateMetrics(stockPercent);
    document.getElementById('expected-return').textContent =
      metrics.return.toFixed(1) + '%';
    document.getElementById('volatility').textContent =
      metrics.volatility.toFixed(1) + '%';
    document.getElementById('worst-year').textContent =
      metrics.worst.toFixed(1) + '%';
    document.getElementById('best-year').textContent =
      metrics.best.toFixed(1) + '%';

    updatePieChart(stockPercent, 100 - stockPercent);
  }
  ```
- **Accessibility:**
  - Keyboard controls: Arrow keys to adjust slider in 5% increments
  - Screen reader announces metric changes as slider moves
  - High contrast mode support
  - Text alternative: Table showing allocation options with corresponding metrics
  - Focus indicators clearly visible on slider thumb
- **Mobile Optimization:**
  - Touch-friendly slider with larger thumb control
  - Metrics stack vertically on small screens
  - Pie chart scales appropriately
  - Swipe gestures supported for slider adjustment

---

### 2. Lifecycle Allocation Comparison Chart
- **Purpose:** Visual demonstration of how appropriate asset allocation evolves with age and time horizon
- **Format/Inputs:** Interactive multi-line chart with age selector
- **Expected Outputs:** Recommended stock/bond allocation for any age, visual comparison of three investment philosophies
- **Interaction Model:**
  - Line chart with X-axis: Age (20-80), Y-axis: Stock allocation (0-100%)
  - Three lines representing different allocation philosophies:
    - Conservative Path (green line)
    - Moderate Path (blue line)
    - Aggressive Path (orange line)
  - Interactive age selector (slider or input box)
  - Vertical indicator line shows current selected age
  - Tooltip displays recommended allocations for all three paths at selected age
  - Click any line to highlight that path and dim others
  - Toggle between "Stock %" view and "Bond %" view
- **Design Notes:**
  - **Allocation Formulas by Philosophy:**
    - **Conservative Path:** Stock % = 90 - Age (more conservative than traditional)
      - Age 20: 70% stocks
      - Age 40: 50% stocks
      - Age 60: 30% stocks
      - Age 80: 10% stocks
    - **Moderate Path:** Stock % = 110 - Age (traditional rule of thumb)
      - Age 20: 90% stocks
      - Age 40: 70% stocks
      - Age 60: 50% stocks
      - Age 80: 30% stocks
    - **Aggressive Path:** Stock % = 130 - Age (modern high-equity approach)
      - Age 20: 110% stocks (using leverage or concentrated positions)
      - Age 40: 90% stocks
      - Age 60: 70% stocks
      - Age 80: 50% stocks
  - **Chart Design:**
    - Gridlines at 10-year and 10% intervals for easy reading
    - Area shading under each line (semi-transparent) to show difference
    - Age markers at common milestones: 25 (career start), 45 (mid-career), 65 (traditional retirement)
    - Annotations:
      - Age 25: "Long time horizon—can handle volatility"
      - Age 45: "Balance growth and stability"
      - Age 65: "Preserve wealth, reduce risk"
  - **Interactive Age Selector:**
    - Horizontal slider below chart (age 20-80)
    - Or numeric input box for precise entry
    - Vertical line appears at selected age
    - Callout box shows:
      - "At age [X]:"
      - "Conservative: [Y]% stocks"
      - "Moderate: [Z]% stocks"
      - "Aggressive: [W]% stocks"
  - **Comparison Panel:**
    - Side panel or bottom section showing:
      - "Your age: [input]"
      - "Years to retirement: [calculated]"
      - "Recommended path based on risk tolerance: [selection]"
      - "Your allocation should be approximately: [X]% stocks / [Y]% bonds"
- **Technical Specifications:**
  ```javascript
  // Allocation calculation functions
  function conservativeAllocation(age) {
    return Math.max(10, Math.min(70, 90 - age));
  }

  function moderateAllocation(age) {
    return Math.max(30, Math.min(90, 110 - age));
  }

  function aggressiveAllocation(age) {
    return Math.max(50, Math.min(110, 130 - age));
  }

  // Generate chart data
  function generateChartData() {
    const ages = [];
    const conservativeData = [];
    const moderateData = [];
    const aggressiveData = [];

    for (let age = 20; age <= 80; age++) {
      ages.push(age);
      conservativeData.push(conservativeAllocation(age));
      moderateData.push(moderateAllocation(age));
      aggressiveData.push(aggressiveAllocation(age));
    }

    return {
      labels: ages,
      datasets: [
        {
          label: 'Conservative',
          data: conservativeData,
          borderColor: '#22c55e',
          backgroundColor: 'rgba(34, 197, 94, 0.1)'
        },
        {
          label: 'Moderate',
          data: moderateData,
          borderColor: '#3b82f6',
          backgroundColor: 'rgba(59, 130, 246, 0.1)'
        },
        {
          label: 'Aggressive',
          data: aggressiveData,
          borderColor: '#f97316',
          backgroundColor: 'rgba(249, 115, 22, 0.1)'
        }
      ]
    };
  }

  // Update recommendation based on age input
  function updateRecommendation(age, riskTolerance) {
    let allocation;
    switch(riskTolerance) {
      case 'conservative':
        allocation = conservativeAllocation(age);
        break;
      case 'moderate':
        allocation = moderateAllocation(age);
        break;
      case 'aggressive':
        allocation = aggressiveAllocation(age);
        break;
    }

    const yearsToRetirement = Math.max(0, 65 - age);
    displayRecommendation(allocation, yearsToRetirement);
  }
  ```
- **Educational Context:**
  - Explanation panel: "Why does age matter?"
    - "Younger investors: More time to recover from losses, can take more risk"
    - "Older investors: Less time to recover, need stability and income"
    - "The 'glide path': Gradually reducing stock allocation as you age"
  - Warning note: "These are guidelines, not rules. Your personal situation (income stability, risk tolerance, other assets) matters more than age alone."
- **Accessibility:**
  - Keyboard navigation for age selector
  - Screen reader describes each line and current allocation at selected age
  - High contrast version available
  - Text table showing allocations at 5-year intervals as alternative
  - Focus indicators on interactive elements
- **Mobile Optimization:**
  - Chart scales to viewport width
  - Touch-friendly age selector
  - Swipe to scrub through ages
  - Simplified view showing one path at a time on very small screens

---

## Day 2 Assets (Learning Lab)

### 1. Advanced Portfolio Builder (PRIMARY SKILL BUILDER)
- **Purpose:** Comprehensive hands-on tool for constructing, testing, and documenting investment portfolios
- **Format/Inputs:** Five-section progressive workflow with save/load functionality
- **Expected Outputs:** Complete portfolio analysis with allocation, backtesting results, rebalancing strategy, tax optimization, and formal Investment Policy Statement
- **Interaction Model:** Sequential five-section process with progress tracking and navigation

---

#### Section A: Lifecycle Portfolio Constructor
- **Purpose:** Build age-appropriate portfolios for three life stages
- **Inputs:**
  - Three investor profiles (automatically loaded)
  - Fund/ETF selection from curated list
  - Allocation percentages for each holding
- **Outputs:**
  - Three complete portfolios (age 25, 45, 60)
  - Portfolio metrics for each
  - Appropriateness scoring
- **Interaction Details:**

  **Investor Profiles (Pre-Loaded):**

  **Profile 1: Alex (Age 25)**
  - Recent college graduate
  - $52,000 annual salary
  - $5,000 emergency fund
  - Contributing $400/month to 401(k)
  - Time horizon: 40 years to retirement
  - Risk capacity: High

  **Profile 2: Jordan (Age 45)**
  - Mid-career professional
  - $95,000 annual salary
  - $180,000 saved in retirement accounts
  - Contributing $1,200/month
  - Time horizon: 20 years to retirement
  - Risk capacity: Moderate

  **Profile 3: Riley (Age 60)**
  - Late-career executive
  - $140,000 annual salary
  - $750,000 saved in retirement accounts
  - Contributing $2,000/month
  - Time horizon: 5 years to retirement
  - Risk capacity: Low-Moderate

  **Fund Selection Menu (12 Options):**
  - **U.S. Stocks:**
    - VTI: Vanguard Total Stock Market (ER: 0.03%, Category: Large blend)
    - VOO: Vanguard S&P 500 (ER: 0.03%, Category: Large blend)
    - VUG: Vanguard Growth (ER: 0.04%, Category: Large growth)
    - VBR: Vanguard Small-Cap Value (ER: 0.07%, Category: Small value)
  - **International Stocks:**
    - VXUS: Vanguard Total International (ER: 0.07%, Category: Foreign large blend)
    - VWO: Vanguard Emerging Markets (ER: 0.08%, Category: Diversified emerging)
  - **Bonds:**
    - BND: Vanguard Total Bond Market (ER: 0.03%, Category: Intermediate core)
    - VGIT: Vanguard Intermediate Treasury (ER: 0.04%, Category: Intermediate government)
    - TIP: iShares TIPS (ER: 0.19%, Category: Inflation-protected)
  - **Alternatives:**
    - VNQ: Vanguard Real Estate (ER: 0.12%, Category: Real estate)
    - IAU: iShares Gold (ER: 0.25%, Category: Commodities)
    - VTIP: Vanguard Short-Term TIPS (ER: 0.04%, Category: Short-term inflation)

  **Portfolio Construction Interface:**
  - For each of three profiles:
    - Select up to 8 funds from menu
    - Assign allocation percentage to each (must total 100%)
    - Live calculation of:
      - Total expense ratio (weighted average)
      - Stock/bond split
      - Domestic/international split
      - Expected return (based on historical averages)
      - Expected volatility
    - Appropriateness meter: "Age-appropriate" / "Too conservative" / "Too aggressive"

  **Appropriateness Scoring Logic:**
  ```javascript
  function calculateAppropriatenessScore(age, stockPercentage) {
    const recommendedStock = 110 - age; // Moderate allocation
    const deviation = Math.abs(stockPercentage - recommendedStock);

    if (deviation < 10) return { score: 'Excellent', color: 'green', message: 'Well-matched to age and time horizon' };
    if (deviation < 20) return { score: 'Good', color: 'blue', message: 'Reasonable allocation with minor deviation' };
    if (deviation < 30) return { score: 'Acceptable', color: 'yellow', message: 'Consider adjusting to better match time horizon' };
    return { score: 'Poor', color: 'red', message: 'Allocation not aligned with age—review risk levels' };
  }

  // Portfolio metrics calculation
  function calculatePortfolioMetrics(holdings) {
    let totalExpenseRatio = 0;
    let stockPercent = 0;
    let bondPercent = 0;
    let expectedReturn = 0;
    let volatility = 0;

    holdings.forEach(holding => {
      const weight = holding.percentage / 100;
      totalExpenseRatio += fundData[holding.ticker].expenseRatio * weight;

      if (fundData[holding.ticker].category.includes('stock')) {
        stockPercent += holding.percentage;
        expectedReturn += 10 * weight; // Simplified: 10% stock return
        volatility += 18 * weight; // Simplified: 18% stock volatility
      } else if (fundData[holding.ticker].category.includes('bond')) {
        bondPercent += holding.percentage;
        expectedReturn += 4 * weight; // Simplified: 4% bond return
        volatility += 5 * weight; // Simplified: 5% bond volatility
      }
    });

    return {
      expenseRatio: totalExpenseRatio,
      stockPercent,
      bondPercent,
      expectedReturn,
      volatility
    };
  }
  ```

  **Visual Display:**
  - Three side-by-side panels (one per investor)
  - Each panel shows:
    - Fund selection dropdowns and percentage inputs
    - Pie chart of current allocation
    - Metrics dashboard (expense ratio, stock/bond split, expected return/risk)
    - Appropriateness gauge with color coding
  - "Compare All Three" button to see side-by-side analysis

- **Technical Specifications:**
  - Real-time calculation (<100ms response)
  - Input validation (percentages must sum to 100%)
  - Save portfolios to LocalStorage
  - Export as PDF showing all three portfolios
- **Accessibility:**
  - Keyboard-accessible dropdown menus
  - Screen reader announces metrics as they update
  - Table view alternative to pie charts

---

#### Section B: Historical Scenario Tester
- **Purpose:** Backtest portfolios through major market crises to understand resilience
- **Inputs:**
  - The three portfolios created in Section A
  - Selection of historical crisis period
- **Outputs:**
  - Performance through crisis: peak-to-trough decline, recovery time, final outcome
  - Visual chart showing portfolio value over time
  - Comparative analysis across three portfolios
- **Interaction Details:**

  **Historical Crisis Scenarios:**

  **Scenario 1: 2008 Financial Crisis**
  - Timeline: October 2007 - March 2009 (17 months)
  - Market decline: S&P 500 down 56.8% peak-to-trough
  - Bond performance: Treasuries +5.2%, Corporate bonds -4.7%
  - Recovery: March 2009 - March 2013 (4 years to new high)
  - Test period: Oct 2007 - Dec 2013 (6.25 years total)

  **Scenario 2: 2020 COVID Pandemic**
  - Timeline: Feb 2020 - March 2020 (1 month)
  - Market decline: S&P 500 down 33.9% peak-to-trough
  - Bond performance: Treasuries +8.0%, Corporate bonds -2.8%
  - Recovery: March 2020 - Aug 2020 (5 months to new high)
  - Test period: Feb 2020 - Dec 2021 (22 months total)

  **Scenario 3: 1970s Stagflation**
  - Timeline: 1973-1974 crash, then 1980-1982
  - Market decline: S&P 500 down 48% (1973-1974), another -27% (1980-1982)
  - Bond performance: Negative real returns due to high inflation
  - Recovery: 1975-1976, then 1983 onward
  - Test period: Jan 1973 - Dec 1982 (10 years total)

  **Testing Process:**
  1. Student selects one of three scenarios
  2. System loads historical monthly return data for stocks and bonds
  3. For each of the three portfolios built in Section A:
     - Calculate month-by-month performance through crisis
     - Track peak value, trough value, recovery date, final value
     - Display results in table and chart
  4. Side-by-side comparison showing which portfolio performed best/worst

  **Results Display:**

  Example for 2008 Financial Crisis with Alex's Portfolio (90% stocks, 10% bonds):
  - Starting value: $100,000
  - Peak value: $102,500 (Nov 2007)
  - Trough value: $44,350 (March 2009) — down 56.7%
  - Recovery date: April 2013 (49 months to recover)
  - Final value (Dec 2013): $125,780
  - Total return: +25.8% over 6.25 years
  - Annualized return: 3.8%

  **Interactive Chart:**
  - Line chart showing portfolio value over test period
  - Toggle to show all three portfolios on same chart
  - Annotated events:
    - Peak marked with green dot
    - Trough marked with red dot
    - Recovery point marked with blue dot
    - Final value marked with checkmark
  - Hover to see exact values at any month
  - Shaded area showing "drawdown period" (peak to recovery)

- **Technical Specifications:**
  ```javascript
  // Historical return data structure
  const historicalReturns = {
    '2008_crisis': {
      '2007-10': { stocks: 0.015, bonds: 0.002 },
      '2007-11': { stocks: -0.044, bonds: 0.015 },
      // ... monthly data through 2013-12
    },
    '2020_pandemic': {
      '2020-02': { stocks: -0.084, bonds: 0.011 },
      // ... monthly data through 2021-12
    },
    '1970s_stagflation': {
      '1973-01': { stocks: -0.017, bonds: -0.003 },
      // ... monthly data through 1982-12
    }
  };

  // Backtest calculation
  function backtestPortfolio(portfolio, scenario) {
    const returns = historicalReturns[scenario];
    let portfolioValue = 100000; // Starting value
    let peak = portfolioValue;
    let trough = portfolioValue;
    let recoveryDate = null;
    const valueHistory = [];

    Object.keys(returns).forEach(month => {
      const monthReturn =
        (portfolio.stockPercent / 100) * returns[month].stocks +
        (portfolio.bondPercent / 100) * returns[month].bonds;

      portfolioValue *= (1 + monthReturn);
      valueHistory.push({ month, value: portfolioValue });

      if (portfolioValue > peak) {
        peak = portfolioValue;
      }
      if (portfolioValue < trough) {
        trough = portfolioValue;
      }
      if (!recoveryDate && portfolioValue >= peak && valueHistory.length > 1) {
        recoveryDate = month;
      }
    });

    return {
      peak,
      trough,
      maxDrawdown: ((trough - peak) / peak) * 100,
      recoveryDate,
      finalValue: portfolioValue,
      totalReturn: ((portfolioValue - 100000) / 100000) * 100,
      valueHistory
    };
  }
  ```

- **Educational Insights Panel:**
  - "What this teaches us:"
    - Higher stock allocation = larger losses but better recovery
    - Bonds provide stability during crises
    - Time horizon matters: All eventually recovered
    - Emotional discipline required to hold through crashes
- **Accessibility:**
  - Screen reader describes peak, trough, recovery sequence
  - Text table showing monthly values as alternative to chart
  - High contrast chart mode

---

#### Section C: Rebalancing Simulator
- **Purpose:** Demonstrate the value of systematic rebalancing through interactive simulation
- **Inputs:**
  - Target allocation (carried forward from Section A or custom)
  - Initial investment amount
  - Simulation period (default: 5 years)
- **Outputs:**
  - Comparison of rebalanced vs. non-rebalanced portfolio outcomes
  - Year-by-year tracking of drift and rebalancing transactions
  - Quantified rebalancing benefit
- **Interaction Details:**

  **Setup:**
  - Select one of the three portfolios from Section A as starting point
  - Or create custom allocation:
    - Stock allocation: __%
    - Bond allocation: __%
    - Cash allocation: __%
  - Initial investment: $______ (default: $10,000)
  - Rebalancing threshold: ±5% drift triggers rebalance (user adjustable: 1%, 5%, 10%, or never)

  **Simulation:**
  - System simulates 5 years of market returns using randomized historical data
  - Each year, stocks and bonds have different returns (based on historical range)
  - Shows two side-by-side portfolios:
    - Portfolio A: Rebalanced annually
    - Portfolio B: Never rebalanced (buy and hold)

  **Year-by-Year Display:**

  Example with 60% stock / 40% bond starting allocation:

  **Year 0 (Start):**
  - Both portfolios: $10,000 ($6,000 stocks, $4,000 bonds)

  **Year 1: Stocks +18%, Bonds +3%**
  - Portfolio A (rebalanced):
    - Before rebalance: $11,200 ($7,080 stocks = 63.2%, $4,120 bonds = 36.8%)
    - Triggered rebalance (>5% drift)
    - After rebalance: $11,200 ($6,720 stocks = 60%, $4,480 bonds = 40%)
    - Action: Sold $360 of stocks, bought $360 of bonds
  - Portfolio B (not rebalanced):
    - End of year: $11,200 ($7,080 stocks = 63.2%, $4,120 bonds = 36.8%)
    - Action: None

  **Year 2: Stocks -8%, Bonds +4%**
  - Portfolio A (rebalanced):
    - Before: $10,845 ($6,182 stocks = 57.0%, $4,659 bonds = 43.0%)
    - Within 5% threshold—no rebalance needed
    - End of year: $10,845
  - Portfolio B (not rebalanced):
    - End of year: $10,798 ($6,514 stocks = 60.3%, $4,285 bonds = 39.7%)
    - Action: None

  [Continue through Year 5]

  **Final Comparison:**
  - Portfolio A (rebalanced): $14,832
  - Portfolio B (not rebalanced): $14,615
  - Rebalancing benefit: $217 (1.5% better)
  - Volatility reduction: 12.3% vs. 14.7%

  **Visual Display:**
  - Dual line chart showing both portfolio values over 5 years
  - Markers indicating when rebalancing occurred
  - Annual allocation pie charts showing drift
  - Transaction log table showing buy/sell actions

- **Technical Specifications:**
  ```javascript
  // Annual return generator (randomized from historical ranges)
  function generateAnnualReturns() {
    const returns = [];
    for (let year = 1; year <= 5; year++) {
      returns.push({
        stocks: randomInRange(-0.30, 0.40), // -30% to +40%
        bonds: randomInRange(-0.05, 0.15)    // -5% to +15%
      });
    }
    return returns;
  }

  // Rebalancing logic
  function simulateYear(portfolio, annualReturn, shouldRebalance, threshold) {
    // Apply returns
    portfolio.stocks *= (1 + annualReturn.stocks);
    portfolio.bonds *= (1 + annualReturn.bonds);

    const total = portfolio.stocks + portfolio.bonds;
    const stockPercent = (portfolio.stocks / total) * 100;
    const bondPercent = (portfolio.bonds / total) * 100;

    // Check if rebalancing needed
    const stockDrift = Math.abs(stockPercent - portfolio.targetStockPercent);

    if (shouldRebalance && stockDrift > threshold) {
      // Rebalance to target allocation
      portfolio.stocks = total * (portfolio.targetStockPercent / 100);
      portfolio.bonds = total * (portfolio.targetBondPercent / 100);
      return { rebalanced: true, soldStocks: /* calculation */, boughtBonds: /* calculation */ };
    }

    return { rebalanced: false };
  }

  // Run full simulation
  function runSimulation(initialAmount, targetAllocation, rebalanceStrategy) {
    const returns = generateAnnualReturns();
    const portfolioA = createPortfolio(initialAmount, targetAllocation, true);
    const portfolioB = createPortfolio(initialAmount, targetAllocation, false);

    const results = { portfolioA: [], portfolioB: [] };

    for (let year = 0; year < 5; year++) {
      const resultA = simulateYear(portfolioA, returns[year], true, 5);
      const resultB = simulateYear(portfolioB, returns[year], false, null);

      results.portfolioA.push({ year, value: portfolioA.total(), actions: resultA });
      results.portfolioB.push({ year, value: portfolioB.total(), actions: resultB });
    }

    return results;
  }
  ```

- **Educational Insights:**
  - "Why rebalancing helps:"
    - Forces you to sell high (winners) and buy low (losers)
    - Maintains risk level (prevents drift to too-risky allocation)
    - Provides discipline during volatile markets
  - "Rebalancing frequency:"
    - Annual or semi-annual typically optimal
    - More frequent = higher transaction costs
    - Threshold-based (5% drift) often better than calendar-based
- **Accessibility:**
  - Table view showing year-by-year results
  - Screen reader describes rebalancing actions
  - Keyboard navigation for all controls
- **Mobile Optimization:**
  - Stacked layout for portfolios on small screens
  - Simplified chart showing key milestones

---

#### Section D: Tax Efficiency Optimizer
- **Purpose:** Teach asset location strategy by showing tax impact of different account placements
- **Inputs:**
  - Available account types (taxable, traditional IRA, Roth IRA, 401k)
  - Investment holdings to be allocated
  - Tax bracket (current)
- **Outputs:**
  - Recommended placement of each investment in optimal account
  - 20-year after-tax wealth projection comparing strategies
  - Tax savings calculation
- **Interaction Details:**

  **Scenario Setup:**
  - Student has three accounts:
    - Taxable brokerage: $50,000
    - Traditional IRA: $75,000
    - Roth IRA: $25,000
  - Student wants to hold:
    - U.S. stock index: $75,000 target
    - Bond index: $50,000 target
    - REIT: $25,000 target
  - Current tax bracket: 22%
  - Time horizon: 20 years

  **Tax Characteristics (Pre-Loaded Data):**
  - **U.S. Stock Index:**
    - Qualified dividends: 1.8% yield (taxed at 15%)
    - Long-term capital gains: Taxed at 15% in taxable
    - Tax-efficient in taxable accounts
  - **Bond Index:**
    - Interest income: 4.0% yield (taxed as ordinary income at 22%)
    - Very tax-inefficient in taxable accounts
    - Best in tax-deferred
  - **REIT:**
    - Non-qualified dividends: 5.5% yield (taxed at ordinary income rate 22%)
    - Extremely tax-inefficient in taxable
    - Best in Roth or traditional IRA

  **Strategy Testing:**

  Student can test two or more strategies:

  **Strategy 1: Naive (No optimization)**
  - Taxable: 33% of each (stocks $25k, bonds $16.7k, REIT $8.3k)
  - Traditional IRA: 50% of each (stocks $37.5k, bonds $25k, REIT $12.5k)
  - Roth IRA: 17% of each (stocks $12.5k, bonds $8.3k, REIT $4.2k)

  **Strategy 2: Optimized**
  - Taxable: U.S. stocks $50k (most tax-efficient)
  - Traditional IRA: Bonds $50k, REIT $25k (tax-inefficient, defer taxes)
  - Roth IRA: U.S. stocks $25k (tax-free growth forever)

  **20-Year Projection:**

  System calculates after-tax wealth for each strategy:

  **Strategy 1 (Naive):**
  - Taxable account:
    - Gross growth: $50,000 → $165,280
    - Annual taxes paid: ~$2,200/year (dividends/interest)
    - After-tax ending value: $142,630
  - Traditional IRA:
    - Gross growth: $75,000 → $247,920
    - Taxes on withdrawal (22%): -$54,542
    - After-tax ending value: $193,378
  - Roth IRA:
    - Growth: $25,000 → $82,640
    - Tax-free: $82,640
  - **Total after-tax: $418,648**

  **Strategy 2 (Optimized):**
  - Taxable account:
    - Stocks only: $50,000 → $180,610 (less annual tax drag)
    - Annual taxes paid: ~$1,050/year (qualified dividends only)
    - After-tax ending value: $165,420
  - Traditional IRA:
    - Bonds + REIT: $75,000 → $242,155
    - Taxes on withdrawal (22%): -$53,274
    - After-tax ending value: $188,881
  - Roth IRA:
    - Stocks: $25,000 → $90,305
    - Tax-free: $90,305
  - **Total after-tax: $444,606**

  **Tax Savings from Optimization: $25,958 (6.2% better)**

  **Visual Display:**
  - Three account "buckets" showing current allocation
  - Drag-and-drop interface to move investments between accounts
  - Real-time calculation of tax impact
  - Side-by-side comparison of strategies
  - Chart showing 20-year wealth accumulation

- **Technical Specifications:**
  ```javascript
  // Tax calculation for different account types
  function calculateAfterTaxReturn(investment, accountType, taxBracket, years) {
    const returns = investmentData[investment];
    const annualReturn = returns.totalReturn;
    const dividendYield = returns.dividendYield;
    const dividendTaxRate = returns.qualified ? 0.15 : taxBracket;

    if (accountType === 'taxable') {
      // Calculate after-tax growth with annual dividend taxation
      let value = investment.amount;
      for (let year = 0; year < years; year++) {
        const dividends = value * dividendYield;
        const dividendTax = dividends * dividendTaxRate;
        const capitalGain = value * (annualReturn - dividendYield);
        value += dividends - dividendTax + capitalGain;
      }
      // Capital gains tax on sale
      const capitalGainsTax = (value - investment.amount) * 0.15;
      return value - capitalGainsTax;
    } else if (accountType === 'traditional') {
      // Tax-deferred growth, tax on full amount at withdrawal
      const value = investment.amount * Math.pow(1 + annualReturn, years);
      return value * (1 - taxBracket);
    } else if (accountType === 'roth') {
      // Tax-free growth
      return investment.amount * Math.pow(1 + annualReturn, years);
    }
  }

  // Optimization algorithm
  function optimizeAssetLocation(accounts, investments, taxBracket) {
    // Sort investments by tax-inefficiency
    const sorted = investments.sort((a, b) =>
      taxDrag(b, 'taxable') - taxDrag(a, 'taxable')
    );

    // Place most tax-inefficient in tax-advantaged accounts first
    const allocation = {};

    // Priority: Roth (tax-free) > Traditional (tax-deferred) > Taxable
    let rothSpace = accounts.roth.balance;
    let tradSpace = accounts.traditional.balance;
    let taxableSpace = accounts.taxable.balance;

    sorted.forEach(inv => {
      if (rothSpace >= inv.amount) {
        allocation[inv.name] = 'roth';
        rothSpace -= inv.amount;
      } else if (tradSpace >= inv.amount) {
        allocation[inv.name] = 'traditional';
        tradSpace -= inv.amount;
      } else {
        allocation[inv.name] = 'taxable';
        taxableSpace -= inv.amount;
      }
    });

    return allocation;
  }
  ```

- **Optimization Rules Display:**
  - "General guidelines for asset location:"
    1. Tax-inefficient (bonds, REITs, taxable bonds) → Traditional IRA/401k
    2. Highest growth (stocks, especially small-cap) → Roth IRA (tax-free growth)
    3. Tax-efficient (stock indexes with low turnover) → Taxable (can use long-term capital gains rate)
  - "Why this matters: Small differences compound over decades"
- **Accessibility:**
  - Keyboard-accessible drag-and-drop (or button-based alternative)
  - Screen reader announces tax impact as investments move
  - Table showing allocation as alternative to visual buckets

---

#### Section E: Investment Policy Statement Generator
- **Purpose:** Create formal, personalized Investment Policy Statement documenting strategy
- **Inputs:**
  - Guided questionnaire covering goals, allocation, rules, schedule
  - Selections from previous sections auto-populate fields
- **Outputs:**
  - Complete IPS document in professional format
  - Downloadable PDF
  - Printable version for signing/dating
- **Interaction Details:**

  **Questionnaire Sections:**

  **1. Investment Goals**
  - Primary goal: [Retirement / Major purchase / Wealth accumulation / Other]
  - Target amount: $______
  - Target date: ______
  - Secondary goals: [Optional list]

  **2. Financial Situation**
  - Current age: ___
  - Retirement age: ___
  - Risk tolerance: [Conservative / Moderate / Aggressive] (from Day 2 assessment if completed)
  - Time horizon: ___ years
  - Income stability: [Very stable / Stable / Variable / Unstable]
  - Emergency fund: [Adequate / Building / None]

  **3. Asset Allocation**
  - Target allocation: [Auto-populated from Section A portfolio or manual entry]
    - Stocks: ___%
    - Bonds: ___%
    - Other: ___%
  - Acceptable range: ±___% (default: ±5%)
  - Rationale: [Text box explaining why this allocation is appropriate]

  **4. Investment Selection Criteria**
  - Preference: [Index funds / Active funds / Mix]
  - Maximum expense ratio: ___% (default: 0.20%)
  - Minimum diversification: ___ holdings
  - Specific funds to include: [Auto-populated from Section A or manual]

  **5. Portfolio Management Rules**
  - Rebalancing frequency: [Quarterly / Semi-annually / Annually / Threshold-based]
  - Rebalancing threshold: ±___% drift (if threshold-based selected)
  - New contributions allocation: [Proportional / To underweight assets / To bonds / To stocks]

  **6. Review and Adjustment Schedule**
  - Annual review date: [Select month]
  - Triggers for major changes:
    - ☐ Change in income (>20%)
    - ☐ Change in risk tolerance
    - ☐ Major life event (marriage, child, etc.)
    - ☐ Within 5 years of goal date
    - ☐ Other: _______

  **7. Restrictions and Constraints**
  - Tax considerations:
    - ☐ Minimize capital gains distributions
    - ☐ Tax-loss harvesting allowed
    - ☐ Asset location optimization (Section D strategy)
  - Ethical/values constraints:
    - ☐ No specific restrictions
    - ☐ ESG/sustainable investing preferred
    - ☐ Avoid specific sectors: _______

  **Generated IPS Document:**

  ```
  INVESTMENT POLICY STATEMENT

  Prepared for: [Student Name]
  Date: [Auto-filled]
  Review Date: [Annual anniversary of creation]

  I. EXECUTIVE SUMMARY
  This Investment Policy Statement establishes guidelines for managing my investment portfolio to achieve my financial goals while maintaining appropriate risk levels.

  Primary Goal: [From Section 1]
  Target Amount: [From Section 1]
  Target Date: [From Section 1]
  Time Horizon: [Calculated]
  Risk Tolerance: [From Section 2]

  II. ASSET ALLOCATION
  Target Allocation:
    • Stocks: [__]%
    • Bonds: [__]%
    • Other: [__]%

  Acceptable Ranges:
    • Stocks: [__]% to [__]%
    • Bonds: [__]% to [__]%
    • Other: [__]% to [__]%

  Rationale: [Student's explanation from questionnaire]

  III. INVESTMENT SELECTION CRITERIA
  • Preference for low-cost index funds
  • Maximum expense ratio: [__]%
  • Minimum diversification across [__] holdings
  • Specific fund selections:
    1. [Fund ticker]: [__]% allocation
    2. [Fund ticker]: [__]% allocation
    [etc.]

  IV. PORTFOLIO MANAGEMENT
  Rebalancing:
    • Frequency: [Selection from questionnaire]
    • Threshold: ±[__]% deviation from target
    • Method: [Description based on Section C]

  New Contributions:
    • Allocation method: [Selection from questionnaire]

  Tax Management:
    • Asset location strategy: [Based on Section D if completed]
    • Tax-loss harvesting: [Allowed / Not allowed]

  V. MONITORING AND REVIEW
  • Annual review: [Month] of each year
  • Performance benchmarks:
    - Portfolio benchmark: [Based on allocation, e.g., "60% S&P 500 / 40% Bloomberg Aggregate Bond"]
    - Minimum acceptable return: [Calculated based on goals]

  • Triggers for major review:
    [List from Section 6]

  VI. RESTRICTIONS
  [Any constraints from Section 7]

  VII. ACKNOWLEDGMENT
  I understand this Investment Policy Statement and commit to following these guidelines to achieve my financial goals with discipline and consistency.

  Signature: _______________________ Date: _________

  Annual Review Signatures:
  Year 1: _______________________ Date: _________
  Year 2: _______________________ Date: _________
  Year 3: _______________________ Date: _________
  ```

  **Features:**
  - Live preview as student completes questionnaire
  - Edit any section before finalizing
  - Save as PDF with professional formatting
  - Print version optimized for physical signature
  - Save to LocalStorage to prevent loss

- **Technical Specifications:**
  - PDF generation using jsPDF or similar library
  - Professional formatting with headers, sections, bullet points
  - Auto-numbering of sections
  - Signature lines with date fields
  - Watermark: "Educational Document - Not Professional Financial Advice"
- **Educational Value:**
  - "Why have an IPS?"
    - Prevents emotional decision-making during volatility
    - Documents strategy for future reference
    - Provides accountability
    - Used by professional investors
  - "This is your first IPS—you'll update it as your situation changes"
- **Accessibility:**
  - Form fields keyboard accessible
  - Screen reader compatible
  - High contrast PDF option
  - Large print version available

---

### 2. Rebalancing Calculator
- **Purpose:** Calculate exact buy/sell amounts needed to rebalance portfolio back to target allocation
- **Format/Inputs:** Simple calculator with current holdings and target allocation
- **Expected Outputs:** Specific transactions needed, tax impact estimate, step-by-step instructions
- **Interaction Model:**

  **Inputs:**
  - Current portfolio:
    - Asset 1 (e.g., Stocks): Current value $______
    - Asset 2 (e.g., Bonds): Current value $______
    - Asset 3 (optional): Current value $______
  - Target allocation:
    - Asset 1: ____%
    - Asset 2: ____%
    - Asset 3: ____%
  - Account type: [Taxable / Tax-deferred]

  **Calculation:**
  ```javascript
  function calculateRebalancing(currentHoldings, targetAllocations, accountType) {
    const totalValue = currentHoldings.reduce((sum, h) => sum + h.value, 0);

    const transactions = currentHoldings.map((holding, i) => {
      const targetValue = totalValue * (targetAllocations[i] / 100);
      const difference = targetValue - holding.value;

      return {
        asset: holding.name,
        currentValue: holding.value,
        currentPercent: (holding.value / totalValue) * 100,
        targetValue,
        targetPercent: targetAllocations[i],
        action: difference > 0 ? 'buy' : 'sell',
        amount: Math.abs(difference)
      };
    });

    // Tax impact if taxable account
    let taxImpact = 0;
    if (accountType === 'taxable') {
      transactions.forEach(t => {
        if (t.action === 'sell') {
          const costBasis = t.currentValue * 0.8; // Assume 20% gain
          const capitalGain = t.amount * 0.2;
          taxImpact += capitalGain * 0.15; // Long-term capital gains rate
        }
      });
    }

    return { transactions, taxImpact, totalValue };
  }
  ```

  **Output Display:**

  Example:
  - Current portfolio: $50,000 total
    - Stocks: $35,000 (70%)
    - Bonds: $15,000 (30%)
  - Target: 60% stocks / 40% bonds

  **Rebalancing Instructions:**
  1. Total portfolio value: $50,000
  2. Target values:
     - Stocks: $30,000 (60%)
     - Bonds: $20,000 (40%)
  3. **Actions needed:**
     - **Sell $5,000 of stocks** (reduce from $35,000 to $30,000)
     - **Buy $5,000 of bonds** (increase from $15,000 to $20,000)
  4. **Estimated tax impact:** $150
     - Capital gain on $5,000 sale: ~$1,000
     - Tax at 15%: $150
  5. **After rebalancing:**
     - Stocks: $30,000 (60%) ✓
     - Bonds: $20,000 (40%) ✓

  **Visual Display:**
  - Before/after pie charts
  - Transaction summary table
  - Tax impact highlighted (if applicable)
  - "Execute Transactions" button (simulated—provides step-by-step)

- **Technical Specifications:**
  - Real-time calculation
  - Support for up to 5 asset classes
  - Rounding to nearest dollar
  - Warning if target percentages don't sum to 100%
- **Accessibility:**
  - Table format for screen readers
  - Keyboard navigation
  - High contrast mode

---

### 3. Asset Location Optimizer
(This is integrated into Section D of the Advanced Portfolio Builder, but also available as standalone tool)

- **Purpose:** Quick reference tool showing optimal account placement for common investments
- **Format/Inputs:** Investment type selector, available account types
- **Expected Outputs:** Recommendation of best account for each investment
- **Interaction Model:**

  **Quick Reference Table:**
  | Investment Type | Tax Characteristics | Best Account | Rationale |
  |----------------|-------------------|-------------|----------|
  | U.S. Stock Index | Qualified dividends (15% tax), long-term cap gains (15%) | **Taxable or Roth** | Tax-efficient; if Roth, tax-free growth |
  | Bonds | Interest taxed as ordinary income (22-37%) | **Traditional IRA** | Defer taxes on high-taxed income |
  | REITs | Non-qualified dividends (22-37% tax) | **Traditional IRA or Roth** | Shelter high-taxed income; Roth if space available |
  | International Stock Index | Qualified dividends, foreign tax credit available | **Taxable** | Foreign tax credit only applies in taxable accounts |
  | High-Yield Bonds | Interest taxed as ordinary income (22-37%) | **Traditional IRA** | Defer taxes on high-taxed income |
  | Municipal Bonds | Tax-free interest | **Taxable** | Already tax-free; wasted in IRA |
  | Small-Cap Growth | High growth potential, low dividends | **Roth IRA** | Maximize tax-free growth on highest-return asset |

  **Interactive Element:**
  - Student selects investments they hold
  - System prioritizes placement based on tax-efficiency
  - Visual "buckets" showing recommended location

- **Technical Specifications:**
  - Drag-and-drop or click-to-assign interface
  - Real-time updating of recommendations
  - "Why?" tooltip explaining each recommendation
- **Accessibility:**
  - Keyboard controls for all interactions
  - Screen reader describes recommendations
  - Text list as alternative to visual buckets

---

### 4. Historical Return Database
- **Purpose:** Interactive database of historical returns allowing students to research and compare allocations
- **Format/Inputs:** Searchable/filterable database with comparison tools
- **Expected Outputs:** Historical performance data, best/worst periods, growth projections
- **Interaction Model:**

  **Data Coverage:**
  - Annual returns for stocks, bonds, cash (1926-present)
  - Pre-loaded allocation options: 100/0, 90/10, 80/20, 70/30, 60/40, 50/50, 40/60, 30/70, 20/80, 10/90, 0/100
  - Inflation data
  - Real (inflation-adjusted) returns

  **Interactive Features:**

  **1. Time Period Selector:**
  - Full history (1926-present)
  - Custom range: [Start year] to [End year]
  - Pre-set periods:
    - Last 10 years
    - Last 30 years
    - Since 2000
    - 1970s stagflation
    - 2008 crisis and recovery

  **2. Allocation Comparison:**
  - Select up to 3 allocations to compare
  - View side-by-side:
    - Average return
    - Standard deviation
    - Best year
    - Worst year
    - Best 10-year period
    - Worst 10-year period
    - Percentage of positive years

  **3. Growth Calculator:**
  - "What if" tool: $10,000 invested in [year] would be worth $_____ today
  - Select allocation
  - Select time period
  - Accounts for inflation
  - Chart showing growth over time

  **Example Display:**

  **Comparison: 60/40 vs. 80/20 vs. 100/0 (1990-2023)**

  | Metric | 60/40 | 80/20 | 100/0 |
  |--------|-------|-------|-------|
  | Average Annual Return | 8.1% | 9.3% | 10.2% |
  | Standard Deviation | 9.8% | 13.2% | 16.5% |
  | Best Year | +28.7% (1995) | +34.1% (1995) | +37.6% (1995) |
  | Worst Year | -16.9% (2008) | -25.4% (2008) | -37.0% (2008) |
  | Positive Years | 31 of 34 (91%) | 30 of 34 (88%) | 28 of 34 (82%) |
  | $10k → | $99,450 | $128,720 | $164,940 |

  **Visual:**
  - Line chart comparing growth of $10,000 in each allocation
  - Bar chart showing annual returns with color coding
  - Table with detailed year-by-year data (collapsible)

- **Technical Specifications:**
  ```javascript
  // Historical data structure
  const historicalData = {
    1926: { stocks: 0.116, bonds: 0.055, inflation: -0.011 },
    1927: { stocks: 0.372, bonds: 0.063, inflation: -0.024 },
    // ... data for each year through 2023
  };

  // Calculate allocation performance
  function calculateAllocationPerformance(stockPercent, bondPercent, startYear, endYear) {
    let portfolioValue = 10000;
    const yearlyReturns = [];

    for (let year = startYear; year <= endYear; year++) {
      const data = historicalData[year];
      const yearReturn =
        (stockPercent / 100) * data.stocks +
        (bondPercent / 100) * data.bonds;

      portfolioValue *= (1 + yearReturn);
      yearlyReturns.push({ year, return: yearReturn, value: portfolioValue });
    }

    const avgReturn = calculateAverage(yearlyReturns.map(y => y.return));
    const stdDev = calculateStdDev(yearlyReturns.map(y => y.return));
    const bestYear = Math.max(...yearlyReturns.map(y => y.return));
    const worstYear = Math.min(...yearlyReturns.map(y => y.return));
    const positiveYears = yearlyReturns.filter(y => y.return > 0).length;

    return {
      avgReturn,
      stdDev,
      bestYear,
      worstYear,
      positiveYears,
      finalValue: portfolioValue,
      yearlyData: yearlyReturns
    };
  }
  ```

- **Educational Notes:**
  - "Past performance doesn't guarantee future results"
  - "Notice: Higher returns always come with higher volatility"
  - "Long time periods show stocks outperforming, but with significant year-to-year variation"
- **Accessibility:**
  - Sortable tables with keyboard navigation
  - Screen reader describes chart data
  - CSV export of data
  - High contrast chart mode

---

## Downloadable Resources (Printable PDFs for Accessibility)

### 1. Portfolio Planning Worksheet (PDF)
- **Purpose:** Paper-based comprehensive portfolio planning guide
- **Contents:**
  - Page 1: Goal Identification
    - Investment goals (primary and secondary)
    - Target amounts and dates
    - Time horizon calculation worksheet
    - Explanation of why goals matter for allocation
  - Page 2: Risk Tolerance Self-Assessment
    - Simplified risk tolerance questions (5 key questions)
    - Scoring guide
    - Conservative/Moderate/Aggressive profile descriptions
    - Age-based allocation guidelines
  - Page 3: Asset Allocation Determination
    - Based on goals and risk tolerance, determine:
      - Target stock percentage: ___%
      - Target bond percentage: ___%
      - Target other: ___%
    - Rationale space: Explain why this allocation is appropriate
  - Page 4: Specific Fund Selection
    - Table with columns: Fund Name, Ticker, Category, Allocation %, Expense Ratio
    - 8 rows for fund selections
    - Guidelines: Diversification, low costs, reputable providers
  - Page 5: Rebalancing Schedule
    - Rebalancing frequency: [Quarterly / Semi-annually / Annually]
    - Threshold if using: ±___% drift
    - Calendar reminder dates
    - Tracking table:
      - Date | Stocks % | Bonds % | Actions Taken | Notes
      - [6 rows for tracking]
  - Page 6: Review and Adjustment Plan
    - Annual review date: ________
    - Triggers for major changes (checklist)
    - Performance evaluation criteria
    - Signature and date line
- **Format:** 6-page PDF, black-and-white printer-friendly
- **Accessibility:** Large font (12pt minimum), high contrast, ample writing space

### 2. Investment Policy Statement Template (PDF)
- **Purpose:** Formal IPS template for completion by hand
- **Contents:**
  - Cover page with title, name, date fields
  - Section I: Executive Summary
    - Goals, time horizon, risk tolerance (fill-in fields)
  - Section II: Asset Allocation
    - Target allocation table
    - Acceptable range specifications
    - Rationale text box
  - Section III: Investment Selection
    - Criteria checklist
    - Fund selection table
  - Section IV: Portfolio Management
    - Rebalancing rules
    - New contribution allocation strategy
  - Section V: Monitoring and Review
    - Review schedule
    - Benchmarks
    - Adjustment triggers
  - Section VI: Restrictions
    - Tax considerations checklist
    - Values/ethical constraints
  - Signature page with multiple annual review signature lines
- **Format:** 8-page PDF, professional formatting
- **Accessibility:** Clear section headers, numbered pages, fillable fields

### 3. Asset Location Quick Reference (PDF)
- **Purpose:** One-page guide for optimal investment placement across account types
- **Contents:**
  - Header: "Asset Location Strategy: Which Investments Go Where?"
  - **Table:**
    | Investment Type | Tax Treatment | Best Account(s) | Avoid Placing In |
    |----------------|---------------|-----------------|------------------|
    | Stock Index Funds | Qualified dividends, LT cap gains | Taxable, Roth | Traditional IRA (wasted tax benefit) |
    | Bonds | Interest as ordinary income | Traditional IRA | Taxable (high tax drag) |
    | REITs | Non-qualified dividends | Traditional IRA, Roth | Taxable (high tax drag) |
    | International Stocks | Qualified divs, foreign tax credit | Taxable | Traditional IRA (lose foreign tax credit) |
    | Small-Cap Growth | High growth, low dividends | Roth IRA | Taxable (will pay cap gains) |
    | Municipal Bonds | Tax-free interest | Taxable | IRA (already tax-free!) |
  - **Decision Flowchart:**
    - Start: "Is this investment tax-inefficient (bonds, REITs, taxable bonds)?"
      - Yes → Traditional IRA/401(k)
      - No → "Is it high-growth (stocks, especially small-cap)?"
        - Yes → Roth IRA (if space available)
        - No → Taxable account
  - **Example Allocation:**
    - "If you have: $50k taxable, $75k traditional IRA, $25k Roth IRA"
    - "And want: 60% stocks, 40% bonds"
    - "Place: Stocks in taxable + Roth, Bonds in traditional IRA"
  - **Tax Savings Note:** "Proper asset location can add 0.2-0.5% annual return"
- **Format:** 1-page PDF, landscape orientation
- **Accessibility:** High contrast table, clear flowchart, large font

### 4. Rebalancing Schedule Template (PDF)
- **Purpose:** Annual tracking sheet for portfolio rebalancing
- **Contents:**
  - Header: Portfolio Rebalancing Log for [Year]
  - Target Allocation Reference:
    - Stocks: ___%
    - Bonds: ___%
    - Other: ___%
  - Rebalancing Trigger: ±___% drift
  - **Tracking Table:**
    | Date | Stocks % | Bonds % | Other % | Drift from Target | Action Taken | Shares/$ Bought | Shares/$ Sold | Notes |
    |------|----------|---------|---------|-------------------|--------------|-----------------|---------------|-------|
    | [12 rows for monthly or quarterly checks]
  - **Annual Summary:**
    - Number of rebalances this year: ____
    - Total transaction costs: $____
    - Portfolio performance vs. benchmark: ____
    - Allocation drift stayed within acceptable range: Yes / No
  - **Next Year Planning:**
    - Continue current strategy: Yes / No
    - Adjustments needed: _____________
  - Calendar reminder checklist:
    - ☐ Jan rebalance check
    - ☐ Apr rebalance check
    - ☐ Jul rebalance check
    - ☐ Oct rebalance check
- **Format:** 2-page PDF (can be printed double-sided)
- **Accessibility:** Table with clear gridlines, checkbox options

### 5. Fund Selection Criteria Checklist (PDF)
- **Purpose:** Guide for evaluating and selecting investment funds
- **Contents:**
  - **Section 1: Essential Criteria (Must-Haves)**
    - ☐ Expense ratio < 0.20% (preferably < 0.10%)
    - ☐ Reputable fund company (Vanguard, Fidelity, Schwab, BlackRock)
    - ☐ Broad diversification (not single-sector or single-stock)
    - ☐ Adequate liquidity (can buy/sell easily)
    - ☐ Tax-efficient (for taxable accounts)
  - **Section 2: Evaluation Criteria**
    - Expense ratio: ____% (Lower is better)
    - Fund size (assets under management): $____ billion (Larger is usually better)
    - Track record: ___ years (Prefer 10+ years for index funds)
    - Index vs. Active: [Index funds typically better for beginners]
    - Turnover ratio: ____% (Lower is better for tax efficiency)
  - **Section 3: Top Recommended Funds by Category**
    - **U.S. Stocks:**
      - VTI (Vanguard Total Stock Market): ER 0.03%
      - FSKAX (Fidelity Total Market Index): ER 0.015%
      - SWTSX (Schwab Total Stock Market Index): ER 0.03%
    - **International Stocks:**
      - VXUS (Vanguard Total International): ER 0.07%
      - FZILX (Fidelity International Index): ER 0.06%
    - **Bonds:**
      - BND (Vanguard Total Bond Market): ER 0.03%
      - FXNAX (Fidelity U.S. Bond Index): ER 0.025%
    - **Balanced (One-Fund Solution):**
      - VBIAX (Vanguard Balanced Index): ER 0.07% (60% stocks / 40% bonds)
  - **Section 4: Red Flags to Avoid**
    - ☒ Expense ratio > 1.0%
    - ☒ Sales loads or commissions
    - ☒ High turnover (> 100% for taxable accounts)
    - ☒ Narrow focus (single country, single industry)
    - ☒ Past performance marketed heavily (focus on costs instead)
  - **Section 5: Your Fund Selections**
    - Table to fill in:
      - Fund 1: ______ (ticker) | Category: ______ | ER: ____% | Allocation: ____%
      - Fund 2: ______ (ticker) | Category: ______ | ER: ____% | Allocation: ____%
      - [6 rows total]
    - Total expense ratio (weighted): ____%
    - Diversification check: ☐ U.S. stocks ☐ International ☐ Bonds ☐ Other
- **Format:** 2-page PDF
- **Accessibility:** Clear checklists, tables with gridlines, examples

---

## Technical Implementation Notes for Developers

### Development Priorities

**Phase 1: Core Functionality (Must-Have for Launch)**
1. **Advanced Portfolio Builder (Sections A-E)** - Primary skill builder
   - Estimated development time: 80 hours
   - Priority: HIGHEST
   - Complexity: High (5 integrated sections)
   - Dependencies: Chart library, PDF generation

2. **Asset Allocation Visual Calculator** (Day 1)
   - Estimated development time: 12 hours
   - Priority: HIGH
   - Complexity: Medium
   - Dependencies: Chart library for pie chart

3. **Lifecycle Allocation Comparison Chart** (Day 1)
   - Estimated development time: 16 hours
   - Priority: HIGH
   - Complexity: Medium
   - Dependencies: Chart library for multi-line chart

**Phase 2: Supporting Tools (High Priority)**
4. **Rebalancing Calculator** (Day 2 standalone)
   - Estimated development time: 8 hours
   - Priority: MEDIUM-HIGH
   - Complexity: Low-Medium

5. **Historical Return Database** (Day 2)
   - Estimated development time: 20 hours
   - Priority: MEDIUM
   - Complexity: Medium-High
   - Dependencies: Historical data file, chart library

**Phase 3: Enhanced Features (Nice-to-Have)**
6. **Asset Location Optimizer** (Day 2 standalone)
   - Note: Integrated in Advanced Portfolio Builder Section D
   - Standalone version: 8 hours
   - Priority: LOW (if standalone)

### Technical Requirements

**Browser Compatibility:**
- Chrome 90+ (latest)
- Firefox 88+ (latest)
- Safari 14+ (latest)
- Edge 90+ (latest)
- Mobile browsers: iOS Safari 14+, Chrome Mobile 90+

**Responsive Design Breakpoints:**
- **Desktop:** 1920×1080 and above
  - Full feature set, side-by-side comparisons
- **Laptop:** 1366×768 to 1919×1079
  - Slightly condensed layouts
- **Tablet:** 768×1024 (landscape and portrait)
  - Stacked layouts, simplified charts
- **Mobile:** 375×667 minimum (iPhone SE size)
  - Vertical stacking, touch-optimized controls

**Performance Targets:**
- **Initial load:** <3 seconds on 3G connection
- **Interactive tool response:** <100ms for calculations
- **Chart rendering:** <500ms even with 100+ data points
- **Smooth animations:** 60fps for transitions
- **PDF generation:** <2 seconds for complete IPS document

**Accessibility (WCAG 2.1 AA Compliance):**
- **Keyboard Navigation:**
  - All interactive elements accessible via Tab/Shift+Tab
  - Arrow keys for sliders and selectors
  - Enter/Space to activate buttons
  - Escape to close modals
  - Skip navigation links provided
- **Screen Reader Compatibility:**
  - ARIA labels on all form inputs
  - ARIA live regions for dynamic updates
  - Descriptive alt text for charts (or text alternatives)
  - Semantic HTML (proper heading hierarchy)
  - Role attributes where needed
- **Visual Accessibility:**
  - Minimum contrast ratio: 4.5:1 for text
  - Focus indicators clearly visible (3px outline)
  - Color not sole means of conveying information
  - Text resizable to 200% without loss of functionality
  - High contrast mode support
- **Alternative Formats:**
  - Printable PDF versions of all interactive tools
  - Text table alternatives to charts
  - CSV data export options

**Data Storage:**
- **LocalStorage:**
  - Save student progress across sections
  - Store portfolio selections
  - Preserve IPS questionnaire responses
  - Maximum storage: 5MB (monitor usage)
  - Clear data functionality provided
- **No Backend Required:**
  - All calculations client-side
  - No personal data transmitted
  - Privacy-preserving design
- **Export Functionality:**
  - PDF export for IPS and portfolio reports
  - CSV export for data tables
  - JSON export for portfolio configurations (optional)

**Security:**
- **No Backend Vulnerabilities:**
  - Client-side only = no server-side attacks
- **Input Validation:**
  - Sanitize all text inputs to prevent XSS
  - Validate numeric inputs (positive numbers, valid ranges)
  - Prevent injection attacks via DOMPurify or similar
- **Privacy:**
  - No collection of personal financial data
  - No analytics tracking without consent
  - No third-party data sharing
  - Clear privacy notice provided

### Calculation Accuracy

**Critical: Financial calculations must be mathematically precise**

**Best Practices:**
- **Use integer arithmetic for currency:**
  ```javascript
  // WRONG (floating-point errors)
  const result = 10000.50 * 0.10; // May give 1000.0499999999

  // RIGHT (integer math in cents)
  const amountCents = 1000050; // $10,000.50 in cents
  const resultCents = Math.round(amountCents * 0.10);
  const resultDollars = resultCents / 100; // Convert back to dollars for display
  ```

- **Rounding:**
  - Always round to 2 decimal places for currency display
  - Round percentages to 1 decimal place (e.g., 7.8%)
  - Use `Math.round()` not `toFixed()` for calculations

- **Input Validation:**
  - Percentages must be 0-100
  - Allocations must sum to exactly 100%
  - Dollar amounts must be positive
  - Years must be valid (1926-2025 for historical data)

- **Edge Cases:**
  - Division by zero: Check denominators before dividing
  - Negative standard deviation: Impossible, validate inputs
  - Time horizon = 0: Prevent or show warning
  - Empty portfolio: Require at least one holding

**Formula Verification:**
All financial formulas cross-checked against:
- Investopedia definitions
- Morningstar methodology
- CFA Institute standards
- Bogleheads wiki

### Chart and Visualization Libraries

**Recommended Library: Chart.js**
- **Why:** Free, open-source, well-documented, responsive
- **License:** MIT (compatible with educational use)
- **Features:** Line, bar, pie charts; animations; tooltips; legends
- **Installation:** CDN or npm
- **Documentation:** https://www.chartjs.org/

**Alternative: D3.js**
- **When to use:** If complex custom visualizations needed
- **Pros:** Extremely flexible and powerful
- **Cons:** Steeper learning curve, more development time
- **Use case:** Only if Chart.js insufficient

**Chart Requirements:**
- **Responsive Sizing:**
  - Charts scale with viewport width
  - Maintain aspect ratio
  - Mobile-friendly (touch interactions)
- **Print-Friendly:**
  - Charts visible in PDF exports
  - High resolution (300 DPI for PDF)
  - Black-and-white version available
- **Accessible:**
  - Text alternative describing data
  - ARIA labels on chart elements
  - Keyboard navigation for interactive charts
  - Data tables as alternatives
- **Color-Blind Friendly:**
  - Use patterns in addition to colors
  - Avoid red-green combinations
  - Blue-orange palette preferred
  - Test with color-blind simulators

**Example Chart.js Implementation:**
```javascript
const ctx = document.getElementById('allocationChart').getContext('2d');
const allocationChart = new Chart(ctx, {
  type: 'pie',
  data: {
    labels: ['Stocks', 'Bonds'],
    datasets: [{
      data: [60, 40],
      backgroundColor: ['#3b82f6', '#22c55e'], // Blue and green
      borderWidth: 2
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: true,
    plugins: {
      legend: {
        position: 'bottom'
      },
      tooltip: {
        callbacks: {
          label: function(context) {
            return context.label + ': ' + context.parsed + '%';
          }
        }
      }
    },
    // Accessibility
    accessibility: {
      enabled: true
    }
  }
});
```

### User Experience Guidelines

**Progressive Disclosure:**
- Don't show all features at once
- Guide students through multi-step processes
- "Help" icons with tooltips for technical terms
- Example: Advanced Portfolio Builder shows one section at a time

**Error Prevention:**
- **Input Validation in Real-Time:**
  - Show error message immediately if input invalid
  - Example: "Allocations must sum to 100% (currently 95%)"
  - Disable submit button until inputs valid
- **Constraints:**
  - Use sliders with min/max bounds
  - Dropdowns for limited choices
  - Numeric inputs with step values
- **Confirmation Dialogs:**
  - Before destructive actions: "Reset All?"
  - Before leaving page with unsaved work
  - Before generating final IPS

**Feedback:**
- **Loading Indicators:**
  - Show spinner for calculations >200ms
  - Progress bar for multi-step processes
  - "Generating PDF..." message
- **Success Messages:**
  - "Portfolio saved successfully"
  - "IPS generated and ready to download"
  - Auto-dismiss after 3 seconds or user click
- **Immediate Validation:**
  - Green checkmark when input valid
  - Red X when input invalid
  - Helpful error messages, not generic "Error"
- **Progress Indicators:**
  - Show "Section 2 of 5" in Advanced Portfolio Builder
  - Progress bar at top of multi-step forms
  - Breadcrumb navigation

**Autosave:**
- Save to LocalStorage every 30 seconds
- Save on blur event (when leaving a form field)
- "Last saved: 2 minutes ago" indicator
- Restore on page reload with notification

### Testing Recommendations

**User Testing:**
- **Before Launch:**
  - Test with 5-10 actual high school students
  - Observe where they get confused or stuck
  - Time how long each section takes
  - Ask for clarity ratings on instructions
- **Iteration:**
  - Fix identified issues
  - Re-test changed sections
  - Collect feedback on improvements
- **Ongoing:**
  - Anonymous feedback form in tools
  - Track common errors/support requests
  - Update based on real usage patterns

**Device Testing:**
- **Low-End Devices:**
  - Test on 3-4 year old phones (not just latest iPhone)
  - Test on budget Android devices
  - Verify performance on older browsers (Chrome 85, Safari 13)
- **Screen Sizes:**
  - iPhone SE (375×667) - smallest supported
  - iPad (768×1024) - tablet
  - 13" laptop (1366×768)
  - 24" monitor (1920×1080)
- **Input Methods:**
  - Touch (mobile/tablet)
  - Mouse (desktop)
  - Keyboard only (accessibility)
  - Trackpad (laptop)

**Accessibility Testing:**
- **Screen Reader Testing:**
  - NVDA (Windows, free)
  - JAWS (Windows, paid - if available)
  - VoiceOver (macOS/iOS, built-in)
  - Test every interactive element
- **Keyboard Navigation:**
  - Tab through entire tool without mouse
  - Verify logical tab order
  - Ensure all functions accessible
  - Test Escape, Enter, Arrow keys
- **Visual Testing:**
  - High contrast mode (Windows/macOS built-in)
  - Zoom to 200% and verify usability
  - Color-blind simulation (Chrome extension)
  - Grayscale mode (verify patterns used, not just color)

**Calculation Testing:**
- **Formula Verification:**
  - Test against known correct values
  - Cross-check with professional calculators
  - Validate edge cases (zero, negative, very large numbers)
- **Edge Cases:**
  - 0% stocks / 100% bonds
  - 100% stocks / 0% bonds
  - Single holding
  - 10+ holdings
  - Extreme market returns (-50%, +100%)
- **Rounding:**
  - Verify proper rounding (not truncation)
  - Check that percentages sum to exactly 100%
  - Test with amounts that don't divide evenly
- **Financial Accuracy:**
  - Compare to Vanguard, Fidelity calculators
  - Verify tax calculations against IRS tables
  - Check compound interest formulas

**Performance Testing:**
- **Load Time:**
  - Measure time to interactive on 3G connection
  - Test with throttled network in Chrome DevTools
  - Optimize images, minify code if slow
- **Calculation Speed:**
  - Measure calculation time for portfolio metrics
  - Test with large datasets (100 years of data)
  - Profile JavaScript performance in DevTools
- **Memory Usage:**
  - Check for memory leaks (repeated use)
  - Monitor LocalStorage usage (5MB limit)
  - Test on devices with limited RAM

### Documentation Needed

**For Developers:**
- **Setup Guide:**
  - How to install dependencies (npm install, etc.)
  - How to run local development server
  - How to run tests
  - How to build for production
- **Component Library:**
  - Reusable UI components (buttons, forms, charts)
  - Code examples for each component
  - Styling guidelines (CSS classes, colors)
- **API Documentation:**
  - If building backend: API endpoints, request/response formats
  - If client-side only: Module structure, function documentation
- **Calculation Formulas:**
  - Mathematical formulas with references
  - Code implementation of each formula
  - Test cases with expected results
- **Deployment Guide:**
  - How to deploy to production server
  - Environment variables needed
  - CDN configuration (if applicable)

**For Teachers:**
- **Quick Start Guide:**
  - How to access tools (URL)
  - Overview of each tool and its purpose
  - How students use tools in learning lab
  - Estimated time for each activity
- **Troubleshooting:**
  - Common technical issues and solutions
  - Browser compatibility notes
  - How to clear cache/LocalStorage
  - Who to contact for support
- **Interpreting Student Results:**
  - How to review student IPS documents
  - Red flags in student portfolios
  - Discussion prompts based on student allocations
- **Privacy and Security:**
  - What data is stored (locally only)
  - How to clear student data
  - Privacy policy information

**For Students:**
- **User Guide:**
  - How to use each tool (step-by-step)
  - What each metric means (glossary)
  - Tips for effective use
  - How to save/export work
- **Help System:**
  - Tooltips on every technical term
  - "?" icons with explanations
  - Context-sensitive help panels
- **Tutorial Videos (Optional):**
  - 2-3 minute walkthrough of each tool
  - Screen recording with voiceover
  - Hosted on YouTube or embedded
- **FAQ:**
  - "Why can't I select more than 100%?"
  - "What if I don't have a Roth IRA?"
  - "How do I interpret my risk tolerance score?"
  - "Where can I open an account to implement this?"

---

## Maintenance and Updates

**Quarterly Updates Required:**
- **Historical Data:**
  - Add latest quarter's returns to database
  - Update "present" year throughout documentation
  - Verify data source links still active
- **Fund Expense Ratios:**
  - Check current expense ratios for recommended funds
  - Update if funds have changed fees
  - Remove funds that have closed
  - Add new low-cost options if available
- **Tax Rates:**
  - Update tax brackets if changed
  - Update capital gains rates
  - Update risk-free rate (3-month T-bill rate)

**Annual Updates Required:**
- **Student Testing:**
  - User testing with new cohort of students
  - Identify confusion points
  - Update instructions based on feedback
- **Technology Updates:**
  - Update Chart.js or other libraries to latest stable versions
  - Test for breaking changes
  - Verify browser compatibility
- **Content Review:**
  - Review examples for relevance
  - Update dollar amounts for inflation
  - Refresh investor profile scenarios

**Bug Fix Protocol:**
- **Critical Bugs (Incorrect calculations, security issues):**
  - Fix immediately within 24 hours
  - Notify all users if deployed
  - Document fix in changelog
- **Minor Bugs (UI glitches, typos):**
  - Fix within 1 week
  - Batch with other minor fixes
  - Update documentation

**Feature Requests:**
- **Evaluation Criteria:**
  - Does it align with learning objectives?
  - Is it requested by multiple teachers/students?
  - Is development effort reasonable?
  - Does it improve educational outcomes?
- **Prioritization:**
  - High: Directly improves learning outcomes
  - Medium: Nice-to-have enhancement
  - Low: Cosmetic or minor convenience

---

**Asset Specifications Status:** ✅ Complete and ready for development

**Total Interactive Assets:**
- **Day 1:** 2 tools
- **Day 2:** 5 major tools (1 primary skill builder with 5 sections, 3 standalone tools, 1 database)
- **Printable Resources:** 5 comprehensive PDFs

**Primary Skill Builder:** Advanced Portfolio Builder (5-section comprehensive tool)

**Development Estimate:** 144 hours for all tools (Phase 1: 108 hours, Phase 2: 28 hours, Phase 3: 8 hours)

**Accessibility:** Full WCAG 2.1 AA compliance with printable alternatives for all digital tools

**State Variables:** None required (L-62 is Tier 3, fully universal content)

**Student Engagement Time:**
- Day 1: ~30 minutes (2 interactive visualizations)
- Day 2: ~90 minutes (Advanced Portfolio Builder + supporting tools)
- Total: ~2 hours of hands-on portfolio construction and analysis
