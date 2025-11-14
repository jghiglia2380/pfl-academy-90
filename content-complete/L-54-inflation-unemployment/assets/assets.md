# Asset Specifications for L-54: Inflation, Unemployment, and Personal Finance

## Day 1 Assets

### 1. Inflation Through Time Comparison Tool
- **Purpose:** Interactive visualization showing how prices have changed over time and how inflation erodes purchasing power
- **Format/Inputs:** Year selector, item selector, amount input
- **Expected Outputs:** Equivalent prices across decades, purchasing power comparison charts, "what if" scenarios
- **Interaction Model:**
  - Select base year (1950-2024)
  - Select comparison year (1950-2024)
  - Choose common item category (housing, food, transportation, etc.)
  - View equivalent prices and purchasing power
  - See visual comparison of dollar value over time

- **Design Notes:**

  **Common Items with Historical Prices:**

  **Housing:**
  - 1970: Median home price $23,000 → 2024: $420,000 (18.3x increase)
  - 1970: Monthly rent $108 → 2024: $1,700 (15.7x increase)

  **Food:**
  - 1970: Gallon of milk $1.32 → 2024: $4.20 (3.2x increase)
  - 1970: Loaf of bread $0.25 → 2024: $2.80 (11.2x increase)
  - 1970: Dozen eggs $0.62 → 2024: $3.50 (5.6x increase)

  **Transportation:**
  - 1970: New car $3,500 → 2024: $48,000 (13.7x increase)
  - 1970: Gallon of gas $0.36 → 2024: $3.50 (9.7x increase)

  **Education:**
  - 1970: Public university tuition $325/year → 2024: $10,500/year (32.3x increase)
  - 1970: Private university $1,800/year → 2024: $38,000/year (21.1x increase)

  **Entertainment:**
  - 1970: Movie ticket $1.55 → 2024: $12.00 (7.7x increase)
  - 1970: Concert ticket $5 → 2024: $75 (15x increase)

  **Wages (for context):**
  - 1970: Median household income $9,870 → 2024: $74,580 (7.6x increase)
  - **Key insight:** Income grew 7.6x, but housing grew 18.3x, education 32.3x

  **Visual Display:**
  - Dual bar chart comparing prices then vs. now
  - Purchasing power meter: "Your $100 in 1970 buys what today?"
  - Income vs. expenses growth chart showing divergence
  - Interactive timeline slider: scrub through years to see price evolution

- **Technical Specifications:**
  ```javascript
  // Historical price data structure
  const historicalPrices = {
    housing: {
      home: {
        1950: 7400, 1960: 11900, 1970: 23000, 1980: 76400,
        1990: 123000, 2000: 166000, 2010: 223000, 2020: 348000, 2024: 420000
      },
      rent: {
        1950: 42, 1960: 71, 1970: 108, 1980: 243,
        1990: 447, 2000: 602, 2010: 800, 2020: 1098, 2024: 1700
      }
    },
    food: {
      milk: {
        1950: 0.68, 1960: 0.97, 1970: 1.32, 1980: 2.16,
        1990: 2.78, 2000: 2.79, 2010: 3.33, 2020: 3.50, 2024: 4.20
      },
      bread: {
        1950: 0.14, 1960: 0.20, 1970: 0.25, 1980: 0.51,
        1990: 0.70, 2000: 1.01, 2010: 1.82, 2020: 2.50, 2024: 2.80
      },
      eggs: {
        1950: 0.60, 1960: 0.57, 1970: 0.62, 1980: 0.84,
        1990: 1.00, 2000: 0.96, 2010: 1.77, 2020: 1.47, 2024: 3.50
      }
    },
    // ... other categories
  };

  // Calculate inflation rate between years
  function calculateInflationRate(startYear, endYear, category, item) {
    const startPrice = historicalPrices[category][item][startYear];
    const endPrice = historicalPrices[category][item][endYear];
    const years = endYear - startYear;

    const inflationRate = Math.pow(endPrice / startPrice, 1 / years) - 1;
    return inflationRate * 100;
  }

  // Calculate equivalent purchasing power
  function calculatePurchasingPower(amount, startYear, endYear) {
    // Use CPI data for overall purchasing power
    const cpiStart = cpiData[startYear];
    const cpiEnd = cpiData[endYear];

    const equivalentAmount = amount * (cpiEnd / cpiStart);
    const purchasingPowerLoss = ((equivalentAmount - amount) / amount) * 100;

    return {
      equivalentAmount,
      purchasingPowerLoss,
      realValue: amount / (cpiEnd / cpiStart)
    };
  }

  // Generate comparison visualization
  function generatePriceComparison(category, item, startYear, endYear) {
    const startPrice = historicalPrices[category][item][startYear];
    const endPrice = historicalPrices[category][item][endYear];
    const increase = ((endPrice - startPrice) / startPrice) * 100;

    const chart = new Chart(ctx, {
      type: 'bar',
      data: {
        labels: [startYear, endYear],
        datasets: [{
          label: `${item} price`,
          data: [startPrice, endPrice],
          backgroundColor: ['#3b82f6', '#ef4444']
        }]
      },
      options: {
        scales: {
          y: {
            beginAtZero: true,
            title: { display: true, text: 'Price ($)' }
          }
        },
        plugins: {
          subtitle: {
            display: true,
            text: `${increase.toFixed(1)}% increase over ${endYear - startYear} years`
          }
        }
      }
    });

    return chart;
  }

  // Interactive timeline scrubber
  document.getElementById('year-slider').addEventListener('input', (e) => {
    const year = parseInt(e.target.value);
    updateAllPrices(year);
    highlightYear(year);
  });
  ```

- **Accessibility:**
  - Keyboard-accessible year selector
  - Screen reader announces price changes
  - Data table alternative to charts
  - High contrast mode
  - Print-friendly summary

- **Mobile Optimization:**
  - Vertical stacking of comparison bars
  - Touch-friendly timeline slider
  - Simplified view on small screens

---

### 2. CPI Calculator (Consumer Price Index)
- **Purpose:** Demonstrate how CPI is calculated and how it measures inflation
- **Format/Inputs:** Interactive breakdown of CPI basket with weightings
- **Expected Outputs:** Custom CPI calculation, comparison to official CPI, impact visualization
- **Interaction Model:**
  - View standard CPI basket composition
  - Adjust spending allocation to match personal budget
  - Calculate personal inflation rate vs. official CPI
  - See how different lifestyles experience different inflation

- **Design Notes:**

  **Official CPI Basket (2024 Weightings):**

  1. **Housing: 42.4%**
     - Shelter: 33.3%
     - Utilities: 5.1%
     - Household furnishings: 4.0%

  2. **Transportation: 16.8%**
     - Vehicles: 7.1%
     - Gasoline: 3.9%
     - Motor vehicle insurance: 2.6%
     - Public transportation: 1.1%
     - Maintenance/repair: 2.1%

  3. **Food: 13.5%**
     - Food at home: 8.0%
     - Food away from home: 5.5%

  4. **Medical Care: 8.7%**
     - Health insurance: 3.9%
     - Medical services: 3.1%
     - Prescription drugs: 1.7%

  5. **Recreation: 5.3%**

  6. **Education and Communication: 6.4%**

  7. **Apparel: 2.6%**

  8. **Other: 4.3%**

  **Personal CPI Calculator:**

  Student inputs their actual spending:
  - Housing: ___% (vs. 42.4% official)
  - Transportation: ___% (vs. 16.8% official)
  - Food: ___% (vs. 13.5% official)
  - [etc.]

  **Example Scenarios:**

  **College Student:**
  - Housing: 35% (dorm/shared apt)
  - Food: 20% (meal plan/dining out)
  - Education: 25% (tuition, books)
  - Transportation: 10%
  - Other: 10%
  - **Personal inflation:** 6.2% (if education costs rise 10%/year)
  - **vs. Official CPI:** 3.5%
  - **Impact:** Experiences 77% higher inflation than official

  **Retiree:**
  - Housing: 40%
  - Medical: 20% (vs. 8.7% official)
  - Food: 15%
  - Transportation: 15%
  - Other: 10%
  - **Personal inflation:** 5.1% (medical costs rising 7%/year)
  - **vs. Official CPI:** 3.5%
  - **Impact:** Experiences 46% higher inflation

  **Young Professional:**
  - Housing: 40% (rent in expensive city)
  - Transportation: 20% (car payment, gas, insurance)
  - Food: 12%
  - Entertainment: 10%
  - Savings: 10%
  - Other: 8%
  - **Personal inflation:** 4.1%
  - **vs. Official CPI:** 3.5%
  - **Impact:** Close to official rate

  **Visual Display:**
  - Pie chart: Official CPI basket
  - Pie chart: Student's personal basket (editable)
  - Side-by-side comparison
  - Bar chart: Personal inflation rate vs. Official CPI
  - "Why the difference?" explanation panel

- **Technical Specifications:**
  ```javascript
  // CPI category data structure
  const cpiCategories = {
    housing: { weight: 42.4, historicalInflation: [2.5, 3.1, 4.2, 3.8, 2.9] }, // Last 5 years
    transportation: { weight: 16.8, historicalInflation: [1.2, -0.5, 8.3, 2.1, 1.8] },
    food: { weight: 13.5, historicalInflation: [3.1, 3.9, 5.8, 4.2, 3.5] },
    medical: { weight: 8.7, historicalInflation: [4.5, 5.1, 4.8, 5.3, 4.9] },
    recreation: { weight: 5.3, historicalInflation: [1.8, 2.1, 2.5, 2.2, 1.9] },
    education: { weight: 6.4, historicalInflation: [6.5, 7.2, 8.1, 7.8, 7.5] },
    apparel: { weight: 2.6, historicalInflation: [-0.5, 0.2, 1.1, 0.8, 0.5] },
    other: { weight: 4.3, historicalInflation: [2.0, 2.3, 2.8, 2.5, 2.2] }
  };

  // Calculate official CPI
  function calculateOfficialCPI() {
    let weightedInflation = 0;

    Object.values(cpiCategories).forEach(category => {
      const latestInflation = category.historicalInflation[category.historicalInflation.length - 1];
      weightedInflation += (category.weight / 100) * latestInflation;
    });

    return weightedInflation;
  }

  // Calculate personal CPI
  function calculatePersonalCPI(personalWeights) {
    let weightedInflation = 0;

    Object.keys(cpiCategories).forEach(category => {
      const categoryData = cpiCategories[category];
      const personalWeight = personalWeights[category] || 0;
      const latestInflation = categoryData.historicalInflation[categoryData.historicalInflation.length - 1];

      weightedInflation += (personalWeight / 100) * latestInflation;
    });

    return weightedInflation;
  }

  // Generate comparison
  function compareInflationRates(personalWeights) {
    const officialCPI = calculateOfficialCPI();
    const personalCPI = calculatePersonalCPI(personalWeights);
    const difference = personalCPI - officialCPI;
    const percentDifference = (difference / officialCPI) * 100;

    return {
      official: officialCPI,
      personal: personalCPI,
      difference,
      percentDifference,
      impact: generateImpactMessage(percentDifference)
    };
  }

  // Impact message generator
  function generateImpactMessage(percentDiff) {
    if (Math.abs(percentDiff) < 10) {
      return "Your inflation experience is close to the national average.";
    } else if (percentDiff > 0) {
      return `You experience ${percentDiff.toFixed(1)}% higher inflation than average. Your costs are rising faster.`;
    } else {
      return `You experience ${Math.abs(percentDiff).toFixed(1)}% lower inflation than average. Your costs are rising slower.`;
    }
  }

  // Real-time weight adjustment
  document.querySelectorAll('.category-weight').forEach(input => {
    input.addEventListener('input', () => {
      const weights = collectPersonalWeights();
      const total = Object.values(weights).reduce((sum, w) => sum + w, 0);

      // Validate total is 100%
      if (total !== 100) {
        displayWarning(`Weights must total 100% (currently ${total}%)`);
      } else {
        const comparison = compareInflationRates(weights);
        updateDisplay(comparison);
      }
    });
  });
  ```

- **Accessibility:**
  - Keyboard navigation for weight inputs
  - Screen reader announces CPI calculations
  - Table view alternative
  - High contrast pie charts

- **Mobile Optimization:**
  - Stacked layout for categories
  - Touch-friendly input controls
  - Simplified charts

---

## Day 2 Assets (Learning Lab)

### 1. Comprehensive Inflation and Unemployment Protection Tool (PRIMARY SKILL BUILDER)
- **Purpose:** Help students build a complete personal financial protection strategy against inflation and unemployment
- **Format/Inputs:** Multi-section interactive tool with real-world scenario analysis
- **Expected Outputs:** Personalized protection plan, financial resilience score, action checklist
- **Interaction Model:** Five-section progressive workflow

---

#### Section A: Salary Inflation Adjuster
- **Purpose:** Analyze job offers and salary negotiations accounting for inflation
- **Inputs:**
  - Job offer details (salary, raise schedule, benefits)
  - Expected inflation rate
  - Time horizon (years)
  - Alternative offer for comparison
- **Outputs:**
  - Real salary trajectory
  - Purchasing power analysis
  - Break-even analysis
  - Negotiation recommendations

- **Interaction Details:**

  **Job Offer Comparison:**

  Student enters details for up to 3 job offers:

  **Offer 1: Startup Tech Company**
  - Starting salary: $75,000
  - Annual raise: 5% guaranteed
  - Benefits value: $8,000/year
  - Expected inflation: 3.5%
  - Real raise: 1.5%/year

  **Offer 2: Established Corporation**
  - Starting salary: $80,000
  - Annual raise: 2.5% (COLA)
  - Benefits value: $15,000/year
  - Expected inflation: 3.5%
  - Real raise: -1.0%/year (losing purchasing power)

  **Offer 3: Government Position**
  - Starting salary: $70,000
  - Annual raise: 3.0% (fixed)
  - Benefits value: $20,000/year (excellent pension)
  - Expected inflation: 3.5%
  - Real raise: -0.5%/year

  **5-Year Projection:**

  | Year | Offer 1 Nominal | Offer 1 Real | Offer 2 Nominal | Offer 2 Real | Offer 3 Nominal | Offer 3 Real |
  |------|-----------------|--------------|-----------------|--------------|-----------------|--------------|
  | 1    | $75,000         | $75,000      | $80,000         | $80,000      | $70,000         | $70,000      |
  | 2    | $78,750         | $73,630      | $82,000         | $76,660      | $72,100         | $67,420      |
  | 3    | $82,688         | $72,290      | $84,050         | $73,470      | $74,263         | $64,970      |
  | 4    | $86,822         | $70,980      | $86,151         | $70,410      | $76,491         | $62,640      |
  | 5    | $91,163         | $69,700      | $88,305         | $67,490      | $78,785         | $60,230      |

  **Including Benefits (Total Compensation):**

  | Year 5 Totals | Nominal | Real (Inflation-Adjusted) |
  |---------------|---------|---------------------------|
  | Offer 1       | $99,163 | $77,700                   |
  | Offer 2       | $103,305 | $82,490                  |
  | Offer 3       | $98,785  | $80,230                  |

  **Visual Display:**
  - Line chart showing nominal salary growth (all three trending up)
  - Line chart showing real salary (Offer 2 & 3 trending down, Offer 1 flat/slight up)
  - Total compensation bars (including benefits)
  - "Best Choice" recommendation based on real compensation
  - Negotiation strategy suggestions

- **Technical Specifications:**
  ```javascript
  // Job offer data structure
  class JobOffer {
    constructor(title, baseSalary, annualRaise, benefitsValue) {
      this.title = title;
      this.baseSalary = baseSalary;
      this.annualRaise = annualRaise; // As decimal (e.g., 0.05 for 5%)
      this.benefitsValue = benefitsValue;
    }

    projectSalary(years, inflationRate) {
      const projections = [];
      let currentSalary = this.baseSalary;

      for (let year = 0; year < years; year++) {
        const nominalSalary = currentSalary;
        const realSalary = nominalSalary / Math.pow(1 + inflationRate, year);
        const totalComp = nominalSalary + this.benefitsValue;
        const realTotalComp = totalComp / Math.pow(1 + inflationRate, year);

        projections.push({
          year: year + 1,
          nominalSalary,
          realSalary,
          totalComp,
          realTotalComp
        });

        currentSalary *= (1 + this.annualRaise);
      }

      return projections;
    }

    getRealRaise(inflationRate) {
      return ((1 + this.annualRaise) / (1 + inflationRate) - 1) * 100;
    }
  }

  // Compare multiple offers
  function compareOffers(offers, years, inflationRate) {
    const comparisons = offers.map(offer => ({
      title: offer.title,
      projections: offer.projectSalary(years, inflationRate),
      realRaise: offer.getRealRaise(inflationRate)
    }));

    // Find best offer based on year 5 real total compensation
    const bestOffer = comparisons.reduce((best, current) => {
      const currentYear5 = current.projections[years - 1].realTotalComp;
      const bestYear5 = best.projections[years - 1].realTotalComp;
      return currentYear5 > bestYear5 ? current : best;
    });

    return { comparisons, bestOffer };
  }

  // Generate recommendation
  function generateNegotiationStrategy(offer, inflationRate) {
    const realRaise = offer.getRealRaise(inflationRate);

    if (realRaise < 0) {
      return {
        priority: "HIGH",
        message: `Your salary will LOSE purchasing power every year. Negotiate for at least ${(inflationRate * 100).toFixed(1)}% annual raises to maintain purchasing power.`,
        script: `"I understand the offered raise is ${(offer.annualRaise * 100)}%, but with inflation at ${(inflationRate * 100).toFixed(1)}%, this represents a real pay cut each year. Can we discuss raises that at least match inflation?"`
      };
    } else if (realRaise < 1) {
      return {
        priority: "MEDIUM",
        message: `Your real raises are minimal (${realRaise.toFixed(1)}%/year). Consider negotiating higher.`,
        script: `"While I appreciate the ${(offer.annualRaise * 100)}% annual raise, I'm concerned about keeping pace with inflation and cost of living. Is there flexibility to increase this to ${((inflationRate + 0.015) * 100).toFixed(1)}%?"`
      };
    } else {
      return {
        priority: "LOW",
        message: `Good news! Your salary will grow ${realRaise.toFixed(1)}% per year after inflation.`,
        script: "Your compensation structure already accounts well for inflation."
      };
    }
  }
  ```

---

#### Section B: Investment Return Calculator (Inflation-Adjusted)
- **Purpose:** Calculate real returns on investments and build inflation-resistant portfolio
- **Inputs:**
  - Investment amount ($10,000 default)
  - Allocation across asset classes
  - Time horizon
  - Expected inflation rate
- **Outputs:**
  - Real returns for each asset class
  - Portfolio real return
  - Risk/return visualization
  - Rebalancing recommendations

- **Asset Classes with Historical Real Returns:**

  **Stocks:**
  - Nominal return: 10.0%/year (historical average)
  - Real return after 3.5% inflation: 6.5%/year
  - Volatility: High (18% standard deviation)
  - Inflation protection: Excellent (grows above inflation)

  **Bonds (Conventional):**
  - Nominal return: 5.0%/year
  - Real return after 3.5% inflation: 1.5%/year
  - Volatility: Low-Moderate (5% standard deviation)
  - Inflation protection: Poor (barely beats inflation)

  **TIPS (Treasury Inflation-Protected Securities):**
  - Nominal return: 2.0% + inflation
  - Real return after inflation: 2.0%/year (guaranteed)
  - Volatility: Low (3% standard deviation)
  - Inflation protection: Excellent (designed for this)

  **Savings Account:**
  - Nominal return: 1.0%/year (current typical rate)
  - Real return after 3.5% inflation: -2.5%/year (LOSING money!)
  - Volatility: None (FDIC insured)
  - Inflation protection: Terrible (guaranteed purchasing power loss)

  **Real Estate:**
  - Nominal return: 8.0%/year (historical)
  - Real return after 3.5% inflation: 4.5%/year
  - Volatility: Moderate (12% standard deviation)
  - Inflation protection: Good (tangible asset)

  **Allocation Tool:**

  Student allocates $10,000 across five asset classes:
  - Stocks: ___% (slider 0-100%)
  - Bonds: ___% (slider 0-100%)
  - TIPS: ___% (slider 0-100%)
  - Savings: ___% (slider 0-100%)
  - Real Estate: ___% (slider 0-100%)
  - Total: Must equal 100%

  **Example Allocations:**

  **Conservative (Age 60+):**
  - Stocks: 30%
  - Bonds: 20%
  - TIPS: 35%
  - Savings: 10%
  - Real Estate: 5%
  - **Expected real return:** 2.9%/year
  - **Risk:** Low

  **Moderate (Age 40-60):**
  - Stocks: 50%
  - Bonds: 15%
  - TIPS: 20%
  - Savings: 5%
  - Real Estate: 10%
  - **Expected real return:** 3.8%/year
  - **Risk:** Moderate

  **Aggressive (Age 20-40):**
  - Stocks: 70%
  - Bonds: 5%
  - TIPS: 10%
  - Savings: 0%
  - Real Estate: 15%
  - **Expected real return:** 5.0%/year
  - **Risk:** High

  **30-Year Projection ($10,000 initial investment):**

  | Allocation | Real Return | Year 30 Value (Real $) | Purchasing Power |
  |------------|-------------|------------------------|------------------|
  | Conservative | 2.9% | $23,450 | 2.3x original |
  | Moderate | 3.8% | $30,690 | 3.1x original |
  | Aggressive | 5.0% | $43,220 | 4.3x original |
  | Savings Only | -2.5% | $4,670 | 0.47x (LOST 53%!) |

  **Visual Display:**
  - Pie chart of allocation
  - Line chart: 30-year growth projection (nominal vs. real)
  - Risk/return scatter plot comparing allocations
  - "Inflation Protection Score" meter (0-100)
  - Asset class performance table

- **Technical Specifications:**
  ```javascript
  // Asset class data
  const assetClasses = {
    stocks: { nominalReturn: 0.10, volatility: 0.18, name: "Stocks" },
    bonds: { nominalReturn: 0.05, volatility: 0.05, name: "Bonds" },
    tips: { nominalReturn: 0.02, inflationProtected: true, volatility: 0.03, name: "TIPS" },
    savings: { nominalReturn: 0.01, volatility: 0.00, name: "Savings" },
    realEstate: { nominalReturn: 0.08, volatility: 0.12, name: "Real Estate" }
  };

  // Calculate portfolio metrics
  function calculatePortfolioMetrics(allocation, inflationRate, years) {
    let expectedReturn = 0;
    let portfolioVolatility = 0;

    Object.keys(allocation).forEach(asset => {
      const assetData = assetClasses[asset];
      const weight = allocation[asset] / 100;

      // Calculate real return
      let realReturn;
      if (assetData.inflationProtected) {
        realReturn = assetData.nominalReturn; // TIPS already real
      } else {
        realReturn = ((1 + assetData.nominalReturn) / (1 + inflationRate)) - 1;
      }

      expectedReturn += weight * realReturn;
      portfolioVolatility += weight * weight * assetData.volatility * assetData.volatility;
    });

    portfolioVolatility = Math.sqrt(portfolioVolatility);

    // Project future value
    const futureValue = (amount, rate, years) => amount * Math.pow(1 + rate, years);
    const realFutureValue = futureValue(10000, expectedReturn, years);

    return {
      expectedReturn: expectedReturn * 100,
      volatility: portfolioVolatility * 100,
      realFutureValue,
      purchasingPowerMultiple: realFutureValue / 10000
    };
  }

  // Inflation protection score
  function calculateInflationProtectionScore(allocation) {
    let score = 0;

    // Stocks: Excellent (40 points max for 100% allocation)
    score += (allocation.stocks / 100) * 40;

    // TIPS: Excellent (40 points max)
    score += (allocation.tips / 100) * 40;

    // Real Estate: Good (20 points max)
    score += (allocation.realEstate / 100) * 20;

    // Bonds: Weak (5 points max)
    score += (allocation.bonds / 100) * 5;

    // Savings: Terrible (0 points)
    score += (allocation.savings / 100) * 0;

    return Math.round(score);
  }

  // Generate recommendations
  function generateAllocationRecommendations(currentAllocation, age) {
    const recommendations = [];

    // Too much in savings
    if (currentAllocation.savings > 15) {
      recommendations.push({
        priority: "HIGH",
        message: `${currentAllocation.savings}% in savings is losing ${(inflationRate * 100).toFixed(1)}% purchasing power per year!`,
        action: "Move most savings into TIPS or stocks for inflation protection."
      });
    }

    // Not enough stocks for young person
    if (age < 40 && currentAllocation.stocks < 60) {
      recommendations.push({
        priority: "MEDIUM",
        message: "At your age, you can handle more stock volatility for better long-term returns.",
        action: `Consider increasing stocks to 60-80% for maximum inflation protection.`
      });
    }

    // No inflation-protected assets
    if (currentAllocation.tips === 0 && currentAllocation.stocks < 30) {
      recommendations.push({
        priority: "MEDIUM",
        message: "You have no guaranteed inflation protection.",
        action: "Add TIPS for stable inflation protection."
      });
    }

    return recommendations;
  }
  ```

---

#### Section C: Unemployment Protection Scorecard
- **Purpose:** Assess personal readiness for potential job loss and build safety net
- **Inputs:**
  - Emergency fund amount
  - Monthly expenses
  - Skills inventory
  - Network strength
  - Job market conditions
- **Outputs:**
  - Unemployment readiness score (0-100)
  - Months of runway
  - Vulnerability assessment
  - Action plan

- **Assessment Categories:**

  **1. Emergency Fund (40 points max)**
  - 0-1 months expenses: 0 points (CRITICAL RISK)
  - 2-3 months: 15 points (HIGH RISK)
  - 4-6 months: 30 points (ADEQUATE)
  - 7+ months: 40 points (EXCELLENT)

  **2. Skills & Employability (30 points max)**
  - In-demand skills: 10 points
  - Recent training/certifications: 5 points
  - Diverse skill set: 5 points
  - Industry growth outlook: 5 points
  - Side income potential: 5 points

  **3. Professional Network (20 points max)**
  - LinkedIn connections in your field: 200+ (5 pts), 500+ (8 pts), 1000+ (10 pts)
  - Active networking (monthly): 5 points
  - Mentors/references: 3+ strong references (5 points)

  **4. Financial Flexibility (10 points max)**
  - Low/no debt: 5 points
  - Adjustable expenses: 3 points
  - Additional income sources: 2 points

  **Total Score Interpretation:**
  - 85-100: Excellent protection (Low vulnerability)
  - 70-84: Good protection (Moderate vulnerability)
  - 50-69: Fair protection (Moderate-high vulnerability)
  - 30-49: Poor protection (High vulnerability)
  - 0-29: Critical risk (Very high vulnerability)

  **Example Student Profile:**

  **Student: Alex (Age 23, Recent College Grad)**
  - Emergency fund: $5,000
  - Monthly expenses: $2,500
  - Months of runway: 2 months
  - Skills: Software development (in-demand)
  - Network: 350 LinkedIn connections, 1 mentor
  - Debt: $25,000 student loans
  - Side income: Freelance coding ($500/month potential)

  **Score Breakdown:**
  - Emergency Fund: 15/40 (2-3 months)
  - Skills: 20/30 (in-demand field, some certs, growth industry)
  - Network: 13/20 (decent connections, 1 mentor, occasional networking)
  - Flexibility: 5/10 (student debt burden, but low expenses, side income possible)
  - **TOTAL: 53/100 (FAIR - Moderate-High Vulnerability)**

  **Recommendations for Alex:**
  1. Priority 1: Build emergency fund to 4-6 months ($10,000-$15,000)
  2. Priority 2: Expand professional network (attend meetups, connect with 10-20 people/month)
  3. Priority 3: Develop freelance income ($500/month → $1,000/month)
  4. Timeline: Reach 70+ score within 12 months

- **Technical Specifications:**
  ```javascript
  // Unemployment protection calculator
  function calculateUnemploymentScore(profile) {
    let score = 0;

    // Emergency Fund (40 points max)
    const monthsOfRunway = profile.emergencyFund / profile.monthlyExpenses;
    if (monthsOfRunway >= 7) score += 40;
    else if (monthsOfRunway >= 4) score += 30;
    else if (monthsOfRunway >= 2) score += 15;
    else score += 0;

    // Skills & Employability (30 points max)
    if (profile.inDemandSkills) score += 10;
    if (profile.recentTraining) score += 5;
    if (profile.diverseSkills) score += 5;
    if (profile.industryGrowth === 'strong') score += 5;
    if (profile.sideIncomeSkills) score += 5;

    // Network (20 points max)
    if (profile.linkedinConnections >= 1000) score += 10;
    else if (profile.linkedinConnections >= 500) score += 8;
    else if (profile.linkedinConnections >= 200) score += 5;
    else score += 2;

    if (profile.activeNetworking) score += 5;
    if (profile.strongReferences >= 3) score += 5;

    // Financial Flexibility (10 points max)
    if (profile.debtLevel === 'low') score += 5;
    if (profile.adjustableExpenses) score += 3;
    if (profile.additionalIncome) score += 2;

    return score;
  }

  // Generate action plan
  function generateActionPlan(profile, score) {
    const actions = [];

    // Emergency fund priority
    if (profile.emergencyFund < profile.monthlyExpenses * 4) {
      const targetFund = profile.monthlyExpenses * 6;
      const needed = targetFund - profile.emergencyFund;
      actions.push({
        priority: 1,
        category: "Emergency Fund",
        goal: `Build to ${targetFund.toFixed(0)} (6 months expenses)`,
        action: `Save $${(needed / 12).toFixed(0)}/month for 12 months`,
        impact: `+${40 - (score >= 30 ? 30 : score >= 15 ? 15 : 0)} points`
      });
    }

    // Network building
    if (profile.linkedinConnections < 500) {
      actions.push({
        priority: 2,
        category: "Professional Network",
        goal: "Reach 500+ LinkedIn connections",
        action: "Connect with 20 people/month in your field, attend 1 networking event/month",
        impact: "+5-8 points"
      });
    }

    // Skills development
    if (!profile.recentTraining) {
      actions.push({
        priority: 3,
        category: "Skills & Certifications",
        goal: "Earn relevant certification",
        action: "Complete online course/certification in your field (Coursera, Udemy, etc.)",
        impact: "+5 points"
      });
    }

    // Side income
    if (!profile.sideIncomeSkills) {
      actions.push({
        priority: 4,
        category: "Income Diversification",
        goal: "Develop freelance/side income",
        action: "Identify marketable skill, create profile on Upwork/Fiverr, aim for $500/month",
        impact: "+5-7 points (skills + flexibility)"
      });
    }

    return actions.sort((a, b) => a.priority - b.priority);
  }

  // Vulnerability assessment
  function assessVulnerability(score, profile) {
    if (score >= 85) {
      return {
        level: "LOW",
        message: "You're well-protected against unemployment.",
        timeToRecover: "1-3 months likely",
        warnings: []
      };
    } else if (score >= 70) {
      return {
        level: "MODERATE",
        message: "You have decent protection but room for improvement.",
        timeToRecover: "3-6 months likely",
        warnings: ["Build emergency fund further", "Expand network"]
      };
    } else if (score >= 50) {
      return {
        level: "MODERATE-HIGH",
        message: "You're vulnerable to job loss. Take action now.",
        timeToRecover: "6-12 months possible",
        warnings: ["PRIORITY: Build 4-6 month emergency fund", "Strengthen professional network", "Diversify income"]
      };
    } else if (score >= 30) {
      return {
        level: "HIGH",
        message: "You're at high risk if you lose your job.",
        timeToRecover: "12+ months, significant hardship likely",
        warnings: ["URGENT: Start emergency fund immediately", "Begin active job networking now (before you need it)", "Reduce expenses where possible"]
      };
    } else {
      return {
        level: "CRITICAL",
        message: "Job loss would cause immediate financial crisis.",
        timeToRecover: "18+ months, potential severe hardship",
        warnings: ["CRITICAL: Any job loss will cause crisis", "Start emergency fund this week", "Consider additional job/income immediately", "Create financial survival plan"]
      };
    }
  }
  ```

---

#### Section D: Wage Negotiation Simulator
- **Purpose:** Practice negotiating salary increases that account for inflation
- **Inputs:**
  - Current salary
  - Years at company
  - Performance level
  - Market data
  - Inflation rate
- **Outputs:**
  - Target salary calculation
  - Negotiation script
  - Alternative strategies
  - Practice scenarios

- **Negotiation Framework:**

  **Step 1: Calculate What You Deserve**

  **Baseline: Inflation Adjustment**
  - Current salary: $65,000
  - Years since last raise: 2
  - Cumulative inflation: 7.2% (3.5% + 3.7%)
  - Inflation-adjusted salary: $69,680
  - **Just to maintain purchasing power: $4,680 raise needed**

  **Add Performance Premium:**
  - Exceeds expectations: +3-5%
  - Meets expectations: +1-2%
  - Below expectations: 0-1%

  **Add Market Adjustment:**
  - Research market rate for your role
  - If underpaid vs. market: +additional %

  **Example Calculation:**
  - Inflation adjustment: +7.2% ($4,680)
  - Performance premium (exceeds): +4% ($2,600)
  - Market adjustment (underpaid): +3% ($1,950)
  - **Target raise: 14.2% ($9,230) → New salary: $74,230**

  **Step 2: Build Your Case**

  Evidence to gather:
  - [ ] Performance metrics (sales, projects completed, efficiency gains)
  - [ ] Market data (Glassdoor, Payscale, LinkedIn Salary)
  - [ ] Inflation data (CPI official numbers)
  - [ ] Competitive offers (if applicable)
  - [ ] Company financial performance (if strong, leverage it)

  **Step 3: Practice Scripts**

  **Opening Statement:**
  "I'd like to discuss my compensation. Over the past two years, I've [specific accomplishments]. I've researched market rates and considered inflation's impact. I'm requesting a [X]% increase to $[target]."

  **Addressing Inflation:**
  "I want to highlight that inflation over the past two years has been 7.2%. My current salary hasn't kept pace, which means I'm effectively earning [calculated percentage] less than two years ago. To maintain my purchasing power, an adjustment of at least $[inflation amount] is necessary."

  **Addressing Performance:**
  "Beyond inflation, I've exceeded expectations by [specific examples]. I've saved the company $[amount] through [efficiency], generated $[amount] in [sales/revenue], and [other achievements]."

  **Addressing Market:**
  "I've researched salaries for my role in our market. The median is $[amount], putting me [X]% below market rate. An increase to $[target] would align my compensation with industry standards."

  **Handling Objections:**
  - "Budget constraints": "I understand. Can we commit to a partial increase now and revisit in [timeframe]? Or perhaps additional benefits?"
  - "Standard raise is 3%": "While I appreciate the standard, my performance has been above standard, and inflation requires more than 3% just to maintain current purchasing power."
  - "Need to wait until review cycle": "I'd like to propose [earlier date]. The inflation impact is ongoing, and waiting will compound the purchasing power loss."

  **Step 4: Alternative Strategies**

  If salary increase is declined:
  - Sign-on bonus (one-time payment)
  - Additional PTO
  - Flexible work arrangements
  - Professional development budget
  - Earlier next review
  - Performance-based bonus structure
  - Equity/stock options

- **Practice Scenarios:**

  **Scenario 1: Standard Annual Review**
  - Company offers: 3.5% raise
  - Inflation: 3.5%
  - Your ask: 8% (inflation + performance)
  - Manager response: [simulated, varies]

  **Scenario 2: Job Offer Negotiation**
  - Offer: $70,000
  - Market rate: $75,000
  - Your ask: $76,000 (market + inflation buffer)
  - Recruiter response: [simulated]

  **Scenario 3: Promotion Negotiation**
  - Current: $65,000
  - New role market: $80,000-$90,000
  - Proposed: $78,000
  - Your ask: $85,000
  - Manager response: [simulated]

- **Technical Specifications:**
  ```javascript
  // Calculate target salary
  function calculateTargetSalary(profile) {
    const currentSalary = profile.salary;
    const yearsWithoutRaise = profile.yearsSinceRaise;
    const performanceRating = profile.performance; // 'exceeds', 'meets', 'below'
    const marketRate = profile.marketRate;
    const inflationRates = profile.inflationByYear; // Array of annual rates

    // Cumulative inflation
    let cumulativeInflation = 1;
    inflationRates.forEach(rate => {
      cumulativeInflation *= (1 + rate);
    });
    const totalInflationPercent = (cumulativeInflation - 1) * 100;
    const inflationAdjustedSalary = currentSalary * cumulativeInflation;
    const inflationRaise = inflationAdjustedSalary - currentSalary;

    // Performance premium
    const performancePremiums = {
      'exceeds': 0.04,
      'meets': 0.015,
      'below': 0.005
    };
    const performanceRaise = currentSalary * performancePremiums[performanceRating];

    // Market adjustment
    const marketGap = marketRate - currentSalary;
    const marketAdjustment = Math.max(0, marketGap * 0.5); // Close half the gap

    // Target salary
    const targetSalary = currentSalary + inflationRaise + performanceRaise + marketAdjustment;
    const totalRaisePercent = ((targetSalary - currentSalary) / currentSalary) * 100;

    return {
      currentSalary,
      inflationAdjustedSalary,
      inflationRaise,
      performanceRaise,
      marketAdjustment,
      targetSalary,
      totalRaisePercent,
      breakdown: {
        inflation: totalInflationPercent,
        performance: performancePremiums[performanceRating] * 100,
        market: (marketAdjustment / currentSalary) * 100
      }
    };
  }

  // Generate negotiation script
  function generateNegotiationScript(calculation) {
    return {
      opening: `I'd like to discuss my compensation. I'm requesting a ${calculation.totalRaisePercent.toFixed(1)}% increase to $${calculation.targetSalary.toFixed(0)}.`,

      inflationPoint: `Over the past ${calculation.yearsWithoutRaise} years, inflation has totaled ${calculation.breakdown.inflation.toFixed(1)}%. Just to maintain my current purchasing power, an adjustment of $${calculation.inflationRaise.toFixed(0)} is necessary.`,

      performancePoint: `Beyond inflation, I've exceeded expectations through [your specific accomplishments]. This merits an additional ${calculation.breakdown.performance.toFixed(1)}% increase.`,

      marketPoint: calculation.marketAdjustment > 0
        ? `Market research shows I'm currently ${((calculation.marketRate - calculation.currentSalary) / calculation.currentSalary * 100).toFixed(1)}% below market rate. The adjustment I'm requesting brings me to market-competitive levels.`
        : null,

      closing: `I'm committed to [company] and believe this adjustment reflects my contributions and market realities. Can we move forward with this increase?`
    };
  }

  // Simulate negotiation scenario
  function simulateNegotiation(studentAsk, scenario) {
    // Simulate manager/recruiter responses based on scenario parameters
    const responses = [
      "That's higher than our budget allows. What if we meet in the middle?",
      "I appreciate your research. Let me discuss with HR and get back to you.",
      "Our standard raise is 3%. I'll need strong justification to go higher.",
      "That's reasonable given your performance. I'll approve it."
    ];

    // Select response based on ask vs. realistic range
    const response = selectResponse(studentAsk, scenario);
    return response;
  }
  ```

---

#### Section E: Economic Resilience Plan Generator
- **Purpose:** Create comprehensive personal plan for inflation and unemployment protection
- **Inputs:**
  - Data from previous sections
  - Goals and timeline
  - Risk tolerance
- **Outputs:**
  - Complete action plan
  - Timeline with milestones
  - Progress tracking checklist
  - Downloadable PDF

- **Plan Components:**

  **1. Emergency Fund Target & Strategy**
  - Current: $____
  - Target: $____ (6 months expenses)
  - Monthly savings needed: $____
  - Timeline to goal: ___ months
  - Strategies: [Automatic transfers, high-yield savings account, expense cuts]

  **2. Inflation Protection**
  - Current investment allocation: ___% stocks, ___% bonds, ___% TIPS, etc.
  - Recommended allocation: ___% (based on age/risk tolerance)
  - Inflation protection score: ___/100
  - Action: Rebalance by [date], add $____ to TIPS

  **3. Income Protection**
  - Unemployment readiness score: ___/100
  - Primary gaps: [Emergency fund, Network, Skills, etc.]
  - Actions:
    - Build emergency fund: $____/month for ___ months
    - Network expansion: Connect with ___ people/month
    - Skills development: Earn [certification] by [date]
    - Side income: Launch [freelance/business] earning $____/month

  **4. Salary Optimization**
  - Current salary: $____
  - Inflation-adjusted equivalent: $____
  - Target negotiation salary: $____
  - Next review date: ______
  - Preparation checklist: [Complete by review date]

  **5. Quarterly Milestones**
  - Q1: Emergency fund to $____, Network to ___ connections
  - Q2: Complete [certification], Side income to $___/month
  - Q3: Emergency fund to $____, Rebalance investments
  - Q4: Salary negotiation, Review and adjust plan

  **PDF Export Format:**
  - Title page with name and date
  - Executive summary (1 page)
  - Detailed action plan (3-4 pages)
  - Progress tracking worksheet (1 page)
  - Resources and templates (1 page)

- **Technical Specifications:**
  ```javascript
  // Generate comprehensive plan
  function generateResiliencePlan(studentData) {
    const plan = {
      emergencyFund: {
        current: studentData.emergencyFund,
        target: studentData.monthlyExpenses * 6,
        monthlyContribution: calculateMonthlyContribution(studentData),
        timeline: calculateTimeline(studentData),
        strategies: ["Automatic transfer on payday", "High-yield savings (4-5% APY)", "Cut subscription expenses"]
      },

      inflationProtection: {
        currentAllocation: studentData.allocation,
        recommendedAllocation: getRecommendedAllocation(studentData.age, studentData.riskTolerance),
        score: calculateInflationProtectionScore(studentData.allocation),
        actions: generateRebalancingActions(studentData)
      },

      incomeProtection: {
        score: studentData.unemploymentScore,
        gaps: identifyGaps(studentData),
        actions: generateIncomeProtectionActions(studentData)
      },

      salaryOptimization: {
        current: studentData.salary,
        inflationAdjusted: studentData.inflationAdjustedSalary,
        target: studentData.targetSalary,
        nextReview: studentData.nextReviewDate,
        preparationChecklist: generateSalaryNegotiationChecklist()
      },

      milestones: generateQuarterlyMilestones(studentData)
    };

    return plan;
  }

  // Export as PDF
  function exportPlanAsPDF(plan) {
    const doc = new jsPDF();

    // Title page
    doc.setFontSize(24);
    doc.text("Economic Resilience Plan", 105, 50, { align: "center" });
    doc.setFontSize(16);
    doc.text(`Prepared for: ${plan.studentName}`, 105, 70, { align: "center" });
    doc.text(`Date: ${new Date().toLocaleDateString()}`, 105, 85, { align: "center" });

    // Executive summary
    doc.addPage();
    doc.setFontSize(18);
    doc.text("Executive Summary", 20, 20);
    doc.setFontSize(12);
    doc.text(`Emergency Fund Target: $${plan.emergencyFund.target.toFixed(0)}`, 20, 35);
    doc.text(`Current: $${plan.emergencyFund.current.toFixed(0)} | Needed: $${(plan.emergencyFund.target - plan.emergencyFund.current).toFixed(0)}`, 20, 45);
    doc.text(`Timeline: ${plan.emergencyFund.timeline} months`, 20, 55);
    // ... more summary

    // Detailed sections
    // ... (continue PDF generation for all plan components)

    doc.save(`Economic_Resilience_Plan_${plan.studentName}.pdf`);
  }
  ```

---

## Downloadable Resources (Printable PDFs for Accessibility)

### 1. Inflation Calculator Worksheet (PDF, 2 pages)
- Historical price comparison tables
- "Your $100 in [year] buys what today?" calculator
- Space for calculations
- Inflation rate reference table (1950-2024)

### 2. Personal CPI Calculator (PDF, 1 page)
- Budget category breakdown
- Weight assignment grid
- Personal vs. official CPI comparison
- Interpretation guide

### 3. Job Offer Comparison Template (PDF, 2 pages)
- Side-by-side comparison table for 3 offers
- Nominal vs. real salary calculator
- Benefits valuation worksheet
- Total compensation summary

### 4. Investment Allocation Worksheet (PDF, 2 pages)
- Asset class overview with historical returns
- Allocation planning grid
- Risk tolerance quiz
- Rebalancing schedule

### 5. Unemployment Protection Checklist (PDF, 2 pages)
- Emergency fund calculator
- Skills inventory
- Network strength assessment
- Action plan template

### 6. Salary Negotiation Script Template (PDF, 2 pages)
- Research checklist
- Calculation worksheets (inflation, performance, market)
- Script templates for opening, objections, closing
- Alternative strategies list

### 7. Economic Resilience Plan (PDF, 6 pages)
- Comprehensive planning template
- All sections from Section E
- Quarterly milestone tracker
- Progress checklist

---

## Technical Implementation Notes for Developers

### Development Priorities

**Phase 1: Core Tools (Must-Have)**
1. Comprehensive Inflation/Unemployment Tool (Sections A-E) - 70 hours
2. Inflation Through Time Tool (Day 1) - 12 hours
3. CPI Calculator (Day 1) - 10 hours

**Phase 2: Supporting Features**
4. Historical data integration - 8 hours
5. PDF export functionality - 6 hours

### Technical Requirements

**Browser Compatibility:** Chrome 90+, Firefox 88+, Safari 14+, Edge 90+

**Performance:** Real-time calculations <100ms, Chart rendering <300ms

**Accessibility:** Full WCAG 2.1 AA compliance, keyboard navigation, screen reader support

**Data Sources:**
- Bureau of Labor Statistics (BLS) CPI data
- Historical price databases
- Investment return data (historical averages)

### Calculation Accuracy

All inflation calculations use official CPI data. Investment projections use historical averages with clear disclaimers about future uncertainty.

---

**Asset Specifications Status:** ✅ Complete and ready for development

**Total Interactive Assets:**
- **Day 1:** 2 tools
- **Day 2:** 1 comprehensive skill builder (5 sections)
- **Printable Resources:** 7 PDFs

**Primary Skill Builder:** Comprehensive Inflation and Unemployment Protection Tool

**Accessibility:** Full WCAG 2.1 AA compliance with printable alternatives

**Student Engagement Time:**
- Day 1: ~30 minutes
- Day 2: ~90 minutes
- Total: ~2 hours hands-on financial protection planning
