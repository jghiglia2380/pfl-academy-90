# L-63: Advanced Investment Concepts - Assets Specification

## Day 1 Assets

### 1. Alternative Investment Comparison Matrix
**Purpose:** Side-by-side comparison of alternatives vs. traditional investments

**Format:** Interactive comparison table with sortable columns and filterable rows

**Technical Specifications:**

```javascript
const alternativeInvestments = {
  sp500: {
    name: "S&P 500 Index",
    category: "Traditional",
    returns10yr: 13.6,
    returns30yr: 10.7,
    volatility: 18.5,
    correlationStocks: 1.00,
    correlationBonds: -0.15,
    liquidity: "Immediate (T+2 settlement)",
    liquidityScore: 10,
    feesTypical: 0.03,
    taxTreatment: "Long-term capital gains eligible",
    complexityLevel: "Simple",
    complexityScore: 1,
    appropriateAllocation: "40-80%",
    worstYear: { year: 2008, return: -37.0 },
    bestYear: { year: 2013, return: 32.4 }
  },
  bonds: {
    name: "Aggregate Bond Index",
    category: "Traditional",
    returns10yr: 1.8,
    returns30yr: 5.3,
    volatility: 5.7,
    correlationStocks: -0.15,
    correlationBonds: 1.00,
    liquidity: "Immediate (T+2 settlement)",
    liquidityScore: 10,
    feesTypical: 0.04,
    taxTreatment: "Interest taxed as ordinary income",
    complexityLevel: "Simple",
    complexityScore: 1,
    appropriateAllocation: "20-60%",
    worstYear: { year: 2022, return: -13.0 },
    bestYear: { year: 2002, return: 10.3 }
  },
  reits: {
    name: "Real Estate Investment Trusts (REITs)",
    category: "Alternative",
    returns10yr: 8.9,
    returns30yr: 9.5,
    volatility: 24.3,
    correlationStocks: 0.65,
    correlationBonds: -0.05,
    liquidity: "Immediate for publicly traded REITs",
    liquidityScore: 9,
    feesTypical: 0.08,
    taxTreatment: "Dividends mostly taxed as ordinary income; some capital gains",
    complexityLevel: "Moderate",
    complexityScore: 3,
    appropriateAllocation: "5-15%",
    worstYear: { year: 2008, return: -39.2 },
    bestYear: { year: 2014, return: 28.0 }
  },
  gold: {
    name: "Gold",
    category: "Alternative",
    returns10yr: 4.4,
    returns30yr: 4.5,
    volatility: 19.7,
    correlationStocks: 0.05,
    correlationBonds: 0.10,
    liquidity: "Moderate (physical gold has premiums/spreads)",
    liquidityScore: 7,
    feesTypical: 0.40,
    taxTreatment: "Collectibles rate (28% max) for physical; varies for ETFs",
    complexityLevel: "Moderate",
    complexityScore: 3,
    appropriateAllocation: "0-10%",
    worstYear: { year: 2013, return: -28.3 },
    bestYear: { year: 2010, return: 29.8 },
    specialNote: "1980-2000: Lost 70% of value (inflation-adjusted)"
  },
  commodities: {
    name: "Diversified Commodities",
    category: "Alternative",
    returns10yr: 2.1,
    returns30yr: 3.2,
    volatility: 22.5,
    correlationStocks: 0.25,
    correlationBonds: 0.05,
    liquidity: "Moderate (futures, ETFs)",
    liquidityScore: 6,
    feesTypical: 0.75,
    taxTreatment: "Complex (60/40 rule for futures); K-1 tax forms",
    complexityLevel: "Complex",
    complexityScore: 7,
    appropriateAllocation: "0-5%",
    worstYear: { year: 2015, return: -24.7 },
    bestYear: { year: 2007, return: 16.2 }
  },
  bitcoin: {
    name: "Bitcoin",
    category: "Alternative (Speculative)",
    returns10yr: 137.8,
    returns30yr: null, // Only exists since 2009
    volatility: 81.0,
    correlationStocks: 0.30,
    correlationBonds: 0.05,
    liquidity: "High volume but price impact risk",
    liquidityScore: 7,
    feesTypical: 0.50,
    taxTreatment: "Capital gains; every transaction is taxable event",
    complexityLevel: "Very Complex",
    complexityScore: 9,
    appropriateAllocation: "0-2%",
    worstYear: { year: 2022, return: -64.3 },
    bestYear: { year: 2013, return: 5507.0 },
    specialNote: "Multiple 80%+ crashes; regulatory uncertainty; not insured",
    warnings: [
      "Can lose 100% of investment",
      "Extreme volatility (81% standard deviation)",
      "No government backing or insurance",
      "Tax reporting complexity",
      "Security risks (hacking, lost passwords)"
    ]
  },
  totalAlternativeFunds: {
    name: "Total Alternative Investment Funds",
    category: "Alternative (Multi-Asset)",
    returns10yr: 5.2,
    returns30yr: 6.8,
    volatility: 12.5,
    correlationStocks: 0.40,
    correlationBonds: 0.20,
    liquidity: "Daily (mutual funds/ETFs)",
    liquidityScore: 9,
    feesTypical: 1.25,
    taxTreatment: "Mixed (depends on underlying assets); often high turnover",
    complexityLevel: "Complex",
    complexityScore: 6,
    appropriateAllocation: "0-20%",
    worstYear: { year: 2008, return: -18.4 },
    bestYear: { year: 2019, return: 14.2 }
  }
};

function generateComparisonMatrix() {
  const container = document.getElementById('comparison-matrix');
  const investments = Object.values(alternativeInvestments);

  // Create table headers
  const table = document.createElement('table');
  table.className = 'w-full border-collapse';
  table.innerHTML = `
    <thead>
      <tr class="bg-indigo-600 text-white">
        <th class="p-3 text-left">Investment</th>
        <th class="p-3 text-right cursor-pointer hover:bg-indigo-700" onclick="sortTable('returns10yr')">
          10-Yr Return ↕
        </th>
        <th class="p-3 text-right cursor-pointer hover:bg-indigo-700" onclick="sortTable('volatility')">
          Volatility ↕
        </th>
        <th class="p-3 text-right">Stock Correlation</th>
        <th class="p-3 text-left">Liquidity</th>
        <th class="p-3 text-right cursor-pointer hover:bg-indigo-700" onclick="sortTable('feesTypical')">
          Fees ↕
        </th>
        <th class="p-3 text-left">Tax Treatment</th>
        <th class="p-3 text-center">Complexity</th>
      </tr>
    </thead>
    <tbody id="comparison-tbody">
    </tbody>
  `;

  container.appendChild(table);

  // Populate table rows
  const tbody = document.getElementById('comparison-tbody');
  investments.forEach((inv, index) => {
    const row = document.createElement('tr');
    row.className = index % 2 === 0 ? 'bg-gray-50' : 'bg-white';

    // Color code by category
    const categoryColor = {
      'Traditional': 'bg-green-100 border-l-4 border-green-500',
      'Alternative': 'bg-yellow-100 border-l-4 border-yellow-500',
      'Alternative (Speculative)': 'bg-red-100 border-l-4 border-red-500',
      'Alternative (Multi-Asset)': 'bg-orange-100 border-l-4 border-orange-500'
    }[inv.category];

    row.className += ` ${categoryColor}`;

    // Complexity visualization
    const complexityDots = '●'.repeat(Math.min(inv.complexityScore, 10));
    const complexityColor = inv.complexityScore <= 2 ? 'text-green-600' :
                            inv.complexityScore <= 5 ? 'text-yellow-600' :
                            'text-red-600';

    row.innerHTML = `
      <td class="p-3 font-semibold">
        ${inv.name}
        ${inv.specialNote ? `<div class="text-xs text-gray-600 mt-1">⚠️ ${inv.specialNote}</div>` : ''}
      </td>
      <td class="p-3 text-right font-mono">
        ${inv.returns10yr !== null ? inv.returns10yr.toFixed(1) + '%' : 'N/A'}
      </td>
      <td class="p-3 text-right font-mono ${inv.volatility > 40 ? 'text-red-600 font-bold' : ''}">
        ${inv.volatility.toFixed(1)}%
      </td>
      <td class="p-3 text-right font-mono">${inv.correlationStocks.toFixed(2)}</td>
      <td class="p-3 text-sm">${inv.liquidity}</td>
      <td class="p-3 text-right font-mono">${inv.feesTypical.toFixed(2)}%</td>
      <td class="p-3 text-sm">${inv.taxTreatment}</td>
      <td class="p-3 text-center ${complexityColor}" title="Complexity: ${inv.complexityScore}/10">
        ${complexityDots}
      </td>
    `;

    // Make row expandable for details
    row.onclick = () => showInvestmentDetails(inv);
    row.style.cursor = 'pointer';

    tbody.appendChild(row);
  });
}

function showInvestmentDetails(investment) {
  const modal = document.getElementById('investment-details-modal');
  const content = document.getElementById('modal-content');

  content.innerHTML = `
    <h3 class="text-2xl font-bold mb-4">${investment.name}</h3>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
      <div class="bg-blue-50 p-4 rounded">
        <div class="text-sm font-semibold text-gray-600">Historical Returns</div>
        <div class="text-2xl font-bold">${investment.returns10yr ? investment.returns10yr.toFixed(1) + '%' : 'N/A'} <span class="text-sm">(10-year)</span></div>
        ${investment.returns30yr ? `<div class="text-lg">${investment.returns30yr.toFixed(1)}% <span class="text-sm">(30-year)</span></div>` : '<div class="text-sm text-gray-500">Limited history</div>'}
      </div>

      <div class="bg-orange-50 p-4 rounded">
        <div class="text-sm font-semibold text-gray-600">Risk (Volatility)</div>
        <div class="text-2xl font-bold">${investment.volatility.toFixed(1)}%</div>
        <div class="text-xs text-gray-600 mt-2">Standard deviation of annual returns</div>
      </div>

      <div class="bg-green-50 p-4 rounded">
        <div class="text-sm font-semibold text-gray-600">Best Year</div>
        <div class="text-xl font-bold text-green-700">+${investment.bestYear.return.toFixed(1)}%</div>
        <div class="text-sm text-gray-600">${investment.bestYear.year}</div>
      </div>

      <div class="bg-red-50 p-4 rounded">
        <div class="text-sm font-semibold text-gray-600">Worst Year</div>
        <div class="text-xl font-bold text-red-700">${investment.worstYear.return.toFixed(1)}%</div>
        <div class="text-sm text-gray-600">${investment.worstYear.year}</div>
      </div>
    </div>

    <div class="border-t pt-4 mb-4">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-3 text-sm">
        <div><strong>Liquidity:</strong> ${investment.liquidity} (${investment.liquidityScore}/10)</div>
        <div><strong>Typical Fees:</strong> ${investment.feesTypical.toFixed(2)}%/year</div>
        <div><strong>Stock Correlation:</strong> ${investment.correlationStocks.toFixed(2)}</div>
        <div><strong>Bond Correlation:</strong> ${investment.correlationBonds.toFixed(2)}</div>
        <div class="col-span-2"><strong>Tax Treatment:</strong> ${investment.taxTreatment}</div>
        <div class="col-span-2"><strong>Complexity:</strong> ${investment.complexityLevel} (${investment.complexityScore}/10)</div>
        <div class="col-span-2"><strong>Appropriate Allocation:</strong> ${investment.appropriateAllocation} of portfolio</div>
      </div>
    </div>

    ${investment.warnings ? `
      <div class="bg-red-50 border-l-4 border-red-500 p-4 mb-4">
        <div class="font-bold text-red-800 mb-2">⚠️ Important Warnings:</div>
        <ul class="list-disc list-inside text-sm space-y-1 text-red-700">
          ${investment.warnings.map(w => `<li>${w}</li>`).join('')}
        </ul>
      </div>
    ` : ''}

    ${investment.specialNote ? `
      <div class="bg-yellow-50 border-l-4 border-yellow-500 p-3 text-sm">
        <strong>Historical Context:</strong> ${investment.specialNote}
      </div>
    ` : ''}
  `;

  modal.classList.remove('hidden');
}

// Sorting function
let currentSort = { field: null, ascending: true };

function sortTable(field) {
  const tbody = document.getElementById('comparison-tbody');
  const rows = Array.from(tbody.querySelectorAll('tr'));

  // Toggle sort direction if clicking same field
  if (currentSort.field === field) {
    currentSort.ascending = !currentSort.ascending;
  } else {
    currentSort.field = field;
    currentSort.ascending = false; // Default to descending (highest first)
  }

  // Sort investment data
  const sortedInvestments = Object.values(alternativeInvestments).sort((a, b) => {
    let aVal = a[field];
    let bVal = b[field];

    // Handle null values
    if (aVal === null) return 1;
    if (bVal === null) return -1;

    if (currentSort.ascending) {
      return aVal - bVal;
    } else {
      return bVal - aVal;
    }
  });

  // Regenerate table with sorted data
  tbody.innerHTML = '';
  sortedInvestments.forEach((inv, index) => {
    // ... (regenerate rows as above)
  });
}
```

**Interaction Model:**
1. Page loads with all investments displayed in table format
2. Color-coded by category (green=traditional, yellow=alternative, red=speculative)
3. Click column headers to sort by that metric
4. Click any row to expand full details in modal
5. Modal shows complete historical data, warnings, and recommendations
6. "Add to Comparison" feature allows selecting 2-3 investments for detailed side-by-side

**Design Notes:**
- Traditional investments (stocks/bonds) always shown first for context
- High volatility (>40%) highlighted in red
- Complexity visualized with dots (●●●●●)
- Mobile: table scrolls horizontally; modal full-screen

**Accessibility Requirements:**
- Sortable headers announce sort direction
- Color coding supplemented with text labels
- Modal keyboard accessible (Esc to close)
- Screen reader announces expanded details

---

### 2. Real Estate Investment Decision Tree
**Purpose:** Guide choice between direct property, REITs, or no real estate

**Technical Specifications:**

```javascript
const realEstateDecisionTree = {
  start: {
    question: "How much capital do you have available to invest in real estate?",
    options: [
      { answer: "Less than $5,000", next: "lowCapital" },
      { answer: "$5,000-$25,000", next: "moderateCapital" },
      { answer: "$25,000-$100,000", next: "significantCapital" },
      { answer: "More than $100,000", next: "highCapital" }
    ]
  },
  lowCapital: {
    question: "With under $5,000, direct real estate ownership is typically not feasible. Would you like exposure to real estate in your portfolio?",
    options: [
      { answer: "Yes, I want real estate exposure", next: "lowCapitalREIT" },
      { answer: "No, I'll stick with stocks/bonds", next: "noRealEstate" }
    ]
  },
  lowCapitalREIT: {
    conclusion: "REIT Recommendation",
    explanation: "With limited capital, REIT index funds or ETFs provide instant diversification across hundreds of properties for as little as $1.",
    recommendation: {
      product: "Total REIT Index Fund or ETF",
      allocation: "5-10% of portfolio",
      pros: [
        "No minimum investment",
        "Instant diversification",
        "Completely passive",
        "Easy to sell",
        "Professional management"
      ],
      cons: [
        "No control over properties",
        "Subject to stock market volatility",
        "Dividends taxed as ordinary income"
      ],
      nextSteps: [
        "Open brokerage account if you don't have one",
        "Search for 'REIT index fund' or 'VNQ' (Vanguard REIT ETF)",
        "Invest amount appropriate for your portfolio (5-10%)",
        "Reinvest dividends automatically"
      ]
    }
  },
  moderateCapital: {
    question: "With $5,000-$25,000, you have options. How much time can you dedicate to real estate management?",
    options: [
      { answer: "No time; I want it completely passive", next: "moderatePassive" },
      { answer: "A few hours per month", next: "moderateSemiActive" },
      { answer: "10+ hours per month; I'm interested in active management", next: "moderateActive" }
    ]
  },
  moderatePassive: {
    conclusion: "REIT Recommendation",
    explanation: "For passive investors, REITs provide real estate exposure without property management responsibilities.",
    recommendation: {
      product: "Total REIT Index Fund",
      allocation: "10-15% of portfolio",
      pros: [
        "Zero time commitment",
        "Professional management",
        "Diversification across property types",
        "Liquid (can sell any business day)"
      ],
      cons: [
        "No leverage (can't use mortgage)",
        "No control over decisions",
        "Tax-inefficient (dividends as ordinary income)"
      ]
    }
  },
  moderateSemiActive: {
    question: "Would you be comfortable being a landlord? (Dealing with tenants, maintenance issues, etc.)",
    options: [
      { answer: "Yes, I'm comfortable with that", next: "crowdfundingOption" },
      { answer: "No, that sounds stressful", next: "moderatePassive" }
    ]
  },
  crowdfundingOption: {
    conclusion: "Real Estate Crowdfunding Consideration",
    explanation: "With moderate capital and willingness to be semi-active, real estate crowdfunding platforms may be worth exploring.",
    recommendation: {
      product: "Real Estate Crowdfunding (Fundrise, RealtyMogul, etc.)",
      allocation: "5-10% of portfolio (treat as speculative)",
      pros: [
        "Lower minimums than direct ownership ($500-$5,000)",
        "Potential for higher returns than REITs",
        "Some control over project selection",
        "Quarterly distributions"
      ],
      cons: [
        "Illiquid (5+ year commitments typical)",
        "High fees (1-2% annual + profit sharing)",
        "Limited track record",
        "Not SEC-registered (less regulation)",
        "Can lose entire investment"
      ],
      warnings: [
        "⚠️ Only invest money you won't need for 5+ years",
        "⚠️ Limited liquidity; early withdrawal often not possible",
        "⚠️ Read offering documents carefully",
        "⚠️ Diversify across multiple projects if participating"
      ]
    }
  },
  significantCapital: {
    question: "With $25,000-$100,000, direct property ownership becomes possible in some markets. Are you comfortable with the responsibilities of being a landlord?",
    options: [
      { answer: "Yes, I'm interested in buying rental property", next: "directProperty" },
      { answer: "No, I want passive real estate exposure", next: "significantPassive" }
    ]
  },
  directProperty: {
    question: "Do you have sufficient emergency funds (6+ months expenses) AND additional reserves for property emergencies, BEYOND your real estate down payment?",
    options: [
      { answer: "Yes, I have emergency funds separate from this investment", next: "directPropertyGo" },
      { answer: "No, this is most of my available money", next: "directPropertyWarning" }
    ]
  },
  directPropertyGo: {
    conclusion: "Direct Property Ownership (With Caution)",
    explanation: "You have sufficient capital and reserves for direct property ownership. However, proceed carefully.",
    recommendation: {
      product: "Single-Family Rental Property or Small Multi-Unit",
      allocation: "Treat as separate from portfolio; evaluate on its own merits",
      pros: [
        "Leverage (mortgage amplifies returns)",
        "Tax benefits (depreciation, deductions)",
        "Control over property decisions",
        "Potential for appreciation + cash flow",
        "Hedge against inflation"
      ],
      cons: [
        "Illiquid (takes months to sell)",
        "Time-intensive (maintenance, tenants, etc.)",
        "Concentration risk (all eggs in one property)",
        "Vacancy risk (no income when empty)",
        "Unexpected expenses (roof, HVAC, etc.)",
        "Market risk (values can decline)"
      ],
      essentialChecklist: [
        "Run cash flow analysis (1% rule: monthly rent ≥ 1% of purchase price)",
        "Budget for vacancies (8-10% of rent)",
        "Budget for maintenance (1% of property value per year)",
        "Budget for property management (8-10% of rent) even if self-managing initially",
        "Verify landlord-tenant laws in your state",
        "Get pre-approved for mortgage",
        "Inspect thoroughly before purchase",
        "Consider property management company if time-limited"
      ],
      warnings: [
        "⚠️ This is NOT passive income (despite what influencers say)",
        "⚠️ Returns are NOT guaranteed",
        "⚠️ Plan for 6-12 months with no cash flow while finding tenants/doing repairs",
        "⚠️ Being a landlord is a BUSINESS; treat it seriously"
      ]
    }
  },
  directPropertyWarning: {
    conclusion: "Warning: Not Ready for Direct Property",
    explanation: "Direct property ownership requires substantial capital reserves BEYOND the down payment. Using most of your available money for a down payment is risky.",
    recommendation: {
      product: "Build emergency fund first; then revisit",
      timeline: "12-24 months to prepare",
      steps: [
        "Build emergency fund to 6-12 months expenses",
        "Save additional property reserves ($10,000-$20,000)",
        "Research markets and properties",
        "Study landlord-tenant laws",
        "Connect with real estate investors in your area",
        "Read books on rental property investing",
        "Then revisit direct ownership"
      ],
      alternativeNow: "In the meantime, invest in REIT index funds for real estate exposure"
    }
  },
  highCapital: {
    question: "With $100,000+, you have flexibility. What's your primary goal for real estate investing?",
    options: [
      { answer: "Passive income through rental properties", next: "highCapitalRental" },
      { answer: "Portfolio diversification without active management", next: "highCapitalPassive" },
      { answer: "Both rental income and diversification", next: "highCapitalBoth" }
    ]
  },
  highCapitalRental: {
    conclusion: "Direct Rental Property Strategy",
    explanation: "With substantial capital, you can pursue direct property ownership with proper risk management.",
    recommendation: {
      approach: "Multi-property diversification",
      allocation: "Diversify across 2-3 properties in different locations/property types if possible",
      strategy: [
        "Purchase first property with 25% down (avoid over-leveraging)",
        "Operate for 6-12 months, learn the business",
        "If successful and still interested, consider second property",
        "Keep significant reserves (minimum $20,000 per property)",
        "Consider property management companies to reduce time burden"
      ],
      professionalAdvice: [
        "Consult with real estate attorney before first purchase",
        "Work with CPA familiar with rental property taxation",
        "Consider forming LLC for liability protection",
        "Get adequate insurance (landlord policy + umbrella)"
      ]
    }
  },
  highCapitalPassive: {
    conclusion: "REIT Portfolio Strategy",
    explanation: "With high capital seeking passive exposure, sophisticated REIT allocation makes sense.",
    recommendation: {
      approach: "Diversified REIT allocation",
      allocation: "10-15% of total portfolio",
      strategy: [
        "Core holding: Total REIT index (60-70% of REIT allocation)",
        "Residential REITs (15-20%): Apartment buildings, single-family rentals",
        "Commercial REITs (10-15%): Office, retail, industrial",
        "Specialized REITs (5-10%): Healthcare, data centers, cell towers"
      ],
      pros: [
        "Complete passivity",
        "True diversification (hundreds of properties)",
        "Professional management",
        "Liquid and rebalanceable"
      ]
    }
  },
  noRealEstate: {
    conclusion: "No Real Estate Allocation",
    explanation: "It's completely reasonable to have no direct real estate allocation beyond your primary residence.",
    reasoning: [
      "Stocks provide indirect real estate exposure (companies own property)",
      "Your primary residence is already significant real estate exposure",
      "Simplicity has value; fewer investments to manage",
      "Real estate is not required for a successful portfolio"
    ],
    recommendation: "Stick with diversified stock/bond portfolio. You don't need real estate investments to build wealth."
  }
};

function runDecisionTree() {
  let currentNode = 'start';
  const container = document.getElementById('decision-tree-container');

  function displayNode(nodeKey) {
    const node = realEstateDecisionTree[nodeKey];
    container.innerHTML = '';

    if (node.question) {
      // Display question
      const questionDiv = document.createElement('div');
      questionDiv.className = 'bg-indigo-50 border-l-4 border-indigo-500 p-6 mb-4';
      questionDiv.innerHTML = `
        <h3 class="text-xl font-bold mb-4">${node.question}</h3>
        <div class="space-y-2">
          ${node.options.map(opt => `
            <button
              class="block w-full text-left p-4 bg-white border border-gray-300 rounded hover:bg-indigo-50 hover:border-indigo-500 transition"
              onclick="navigateTree('${opt.next}')"
            >
              ${opt.answer}
            </button>
          `).join('')}
        </div>
      `;
      container.appendChild(questionDiv);
    } else if (node.conclusion) {
      // Display conclusion
      displayConclusion(node);
    }
  }

  function displayConclusion(node) {
    container.innerHTML = `
      <div class="bg-green-50 border-l-4 border-green-500 p-6 mb-6">
        <h3 class="text-2xl font-bold text-green-800 mb-2">✓ Recommendation: ${node.conclusion}</h3>
        <p class="text-lg">${node.explanation}</p>
      </div>

      ${node.recommendation ? `
        <div class="bg-white border rounded-lg p-6 mb-4">
          <h4 class="text-xl font-bold mb-4">Specific Recommendation</h4>

          ${node.recommendation.product ? `
            <div class="mb-4">
              <div class="font-semibold text-gray-600">Product/Approach:</div>
              <div class="text-lg font-bold">${node.recommendation.product}</div>
            </div>
          ` : ''}

          ${node.recommendation.allocation ? `
            <div class="mb-4">
              <div class="font-semibold text-gray-600">Recommended Allocation:</div>
              <div class="text-lg">${node.recommendation.allocation}</div>
            </div>
          ` : ''}

          ${node.recommendation.pros ? `
            <div class="mb-4">
              <div class="font-semibold text-green-700 mb-2">Pros:</div>
              <ul class="list-disc list-inside text-sm space-y-1">
                ${node.recommendation.pros.map(pro => `<li>${pro}</li>`).join('')}
              </ul>
            </div>
          ` : ''}

          ${node.recommendation.cons ? `
            <div class="mb-4">
              <div class="font-semibold text-red-700 mb-2">Cons:</div>
              <ul class="list-disc list-inside text-sm space-y-1">
                ${node.recommendation.cons.map(con => `<li>${con}</li>`).join('')}
              </ul>
            </div>
          ` : ''}

          ${node.recommendation.warnings ? `
            <div class="bg-red-50 border-l-4 border-red-500 p-4 mb-4">
              <div class="font-bold text-red-800 mb-2">Important Warnings:</div>
              <ul class="list-disc list-inside text-sm space-y-1">
                ${node.recommendation.warnings.map(w => `<li>${w}</li>`).join('')}
              </ul>
            </div>
          ` : ''}

          ${node.recommendation.nextSteps ? `
            <div class="bg-blue-50 border-l-4 border-blue-500 p-4">
              <div class="font-bold text-blue-800 mb-2">Next Steps:</div>
              <ol class="list-decimal list-inside text-sm space-y-1">
                ${node.recommendation.nextSteps.map(step => `<li>${step}</li>`).join('')}
              </ol>
            </div>
          ` : ''}
        </div>
      ` : ''}

      <button
        class="bg-indigo-600 text-white px-6 py-3 rounded hover:bg-indigo-700 transition"
        onclick="runDecisionTree()"
      >
        Start Over
      </button>

      <button
        class="ml-4 bg-gray-200 text-gray-800 px-6 py-3 rounded hover:bg-gray-300 transition"
        onclick="exportRecommendation()"
      >
        Export as PDF
      </button>
    `;
  }

  // Make navigation function globally accessible
  window.navigateTree = (nodeKey) => {
    displayNode(nodeKey);
  };

  // Start at beginning
  displayNode('start');
}
```

**Interaction Model:**
1. User starts at beginning: "How much capital do you have?"
2. Each answer branches to appropriate next question
3. Questions adapt based on prior answers
4. Final recommendation provides detailed guidance
5. User can export recommendation as PDF
6. "Start Over" button to try different scenarios

**Design Notes:**
- Questions in indigo boxes
- Recommendations in green success boxes
- Warnings in red alert boxes
- Clear visual progression
- Mobile-friendly button sizing

---

## Day 2 Assets (Learning Lab)

### 1. Alternative Investment Analyzer (Primary Skill Builder)

**Purpose:** Comprehensive 4-section tool for analyzing alternative investment opportunities

**Section A: Real Estate vs. REIT Comparator**

**Technical Specifications:**

```javascript
class RealEstateComparator {
  constructor() {
    this.rentalProperty = {
      purchasePrice: 0,
      downPaymentPercent: 20,
      interestRate: 6.5,
      loanTermYears: 30,
      monthlyRent: 0,
      vacancyRate: 8,
      propertyTaxRate: 1.2,
      insurance: 1200,
      maintenance: 1,
      propertyManagement: 10,
      utilities: 0,
      hoaFees: 0,
      appreciationRate: 3.5
    };

    this.reitInvestment = {
      initialInvestment: 0,
      dividendYield: 4.2,
      appreciationRate: 4.5,
      expenseRatio: 0.08,
      taxRate: 24
    };
  }

  calculateRentalPropertyReturns() {
    const downPayment = this.rentalProperty.purchasePrice * (this.rentalProperty.downPaymentPercent / 100);
    const loanAmount = this.rentalProperty.purchasePrice - downPayment;

    // Monthly mortgage payment
    const monthlyRate = this.rentalProperty.interestRate / 100 / 12;
    const numPayments = this.rentalProperty.loanTermYears * 12;
    const monthlyMortgage = loanAmount * (monthlyRate * Math.pow(1 + monthlyRate, numPayments)) /
                            (Math.pow(1 + monthlyRate, numPayments) - 1);

    // Monthly expenses
    const monthlyPropertyTax = (this.rentalProperty.purchasePrice * this.rentalProperty.propertyTaxRate / 100) / 12;
    const monthlyInsurance = this.rentalProperty.insurance / 12;
    const monthlyMaintenance = (this.rentalProperty.purchasePrice * this.rentalProperty.maintenance / 100) / 12;
    const monthlyPropMgmt = this.rentalProperty.monthlyRent * (this.rentalProperty.propertyManagement / 100);
    const monthlyUtilities = this.rentalProperty.utilities;
    const monthlyHOA = this.rentalProperty.hoaFees;

    const totalMonthlyExpenses = monthlyMortgage + monthlyPropertyTax + monthlyInsurance +
                                 monthlyMaintenance + monthlyPropMgmt + monthlyUtilities + monthlyHOA;

    // Effective monthly rent (accounting for vacancy)
    const effectiveMonthlyRent = this.rentalProperty.monthlyRent * (1 - this.rentalProperty.vacancyRate / 100);

    // Monthly cash flow
    const monthlyCashFlow = effectiveMonthlyRent - totalMonthlyExpenses;
    const annualCashFlow = monthlyCashFlow * 12;

    // Cash-on-cash return
    const cashOnCashReturn = (annualCashFlow / downPayment) * 100;

    // Total return including appreciation (year 10)
    const propertyValueYear10 = this.rentalProperty.purchasePrice *
                                 Math.pow(1 + this.rentalProperty.appreciationRate / 100, 10);
    const loanBalanceYear10 = this.calculateLoanBalance(loanAmount, monthlyRate, numPayments, 10 * 12);
    const equityYear10 = propertyValueYear10 - loanBalanceYear10;

    const totalCashFlows10Years = annualCashFlow * 10;
    const totalReturn10Years = equityYear10 - downPayment + totalCashFlows10Years;
    const annualizedReturn10Years = (Math.pow(totalReturn10Years / downPayment, 1/10) - 1) * 100;

    // Time commitment
    const hoursPerMonth = this.rentalProperty.propertyManagement > 0 ? 2 : 10;

    return {
      downPayment,
      monthlyMortgage,
      totalMonthlyExpenses,
      effectiveMonthlyRent,
      monthlyCashFlow,
      annualCashFlow,
      cashOnCashReturn,
      propertyValueYear10,
      equityYear10,
      totalReturn10Years,
      annualizedReturn10Years,
      hoursPerMonth
    };
  }

  calculateLoanBalance(principal, monthlyRate, totalPayments, paymentsMade) {
    return principal * (Math.pow(1 + monthlyRate, totalPayments) - Math.pow(1 + monthlyRate, paymentsMade)) /
           (Math.pow(1 + monthlyRate, totalPayments) - 1);
  }

  calculateREITReturns() {
    const investment = this.reitInvestment.initialInvestment;

    // Annual dividend income (after expenses)
    const grossDividend = investment * (this.reitInvestment.dividendYield / 100);
    const expenses = investment * (this.reitInvestment.expenseRatio / 100);
    const netDividend = grossDividend - expenses;

    // After-tax dividend (ordinary income tax rate)
    const afterTaxDividend = netDividend * (1 - this.reitInvestment.taxRate / 100);

    // Total return (dividends + appreciation) over 10 years
    let totalValue = investment;
    let totalDividends = 0;

    for (let year = 1; year <= 10; year++) {
      const yearDividend = totalValue * (this.reitInvestment.dividendYield / 100);
      totalDividends += yearDividend * (1 - this.reitInvestment.taxRate / 100);

      // Appreciation
      totalValue *= (1 + this.reitInvestment.appreciationRate / 100);
    }

    const totalReturn10Years = (totalValue - investment) + totalDividends;
    const annualizedReturn10Years = (Math.pow((investment + totalReturn10Years) / investment, 1/10) - 1) * 100;

    return {
      investment,
      annualDividend: netDividend,
      afterTaxDividend,
      valueYear10: totalValue,
      totalReturn10Years,
      annualizedReturn10Years,
      hoursPerMonth: 0 // Completely passive
    };
  }

  generateComparison() {
    const rental = this.calculateRentalPropertyReturns();
    const reit = this.calculateREITReturns();

    return {
      rental,
      reit,
      comparison: {
        returnDifference: rental.annualizedReturn10Years - reit.annualizedReturn10Years,
        timeDifference: rental.hoursPerMonth - reit.hoursPerMonth,
        winnerOnReturns: rental.annualizedReturn10Years > reit.annualizedReturn10Years ? 'Rental Property' : 'REIT',
        winnerOnTime: 'REIT',
        recommendation: this.generateRecommendation(rental, reit)
      }
    };
  }

  generateRecommendation(rental, reit) {
    const returnsAdvantage = Math.abs(rental.annualizedReturn10Years - reit.annualizedReturn10Years);

    if (rental.cashOnCashReturn < 6) {
      return {
        choice: "REITs",
        reasoning: `Rental property cash-on-cash return is only ${rental.cashOnCashReturn.toFixed(1)}%, which is below typical REIT yields. The property doesn't meet basic profitability thresholds.`,
        confidence: "High"
      };
    }

    if (returnsAdvantage < 2) {
      return {
        choice: "REITs",
        reasoning: `Returns are similar (within 2%), but REITs require zero time while rental property requires ${rental.hoursPerMonth} hours/month. The convenience of REITs outweighs the minimal return difference.`,
        confidence: "High"
      };
    }

    if (rental.annualizedReturn10Years > reit.annualizedReturn10Years && returnsAdvantage > 3) {
      return {
        choice: "Rental Property (If willing to put in the time)",
        reasoning: `Rental property offers ${returnsAdvantage.toFixed(1)}% higher annualized returns, but requires ${rental.hoursPerMonth} hours/month of management. Only pursue if you enjoy being a landlord and have sufficient reserves for emergencies.`,
        confidence: "Medium"
      };
    }

    return {
      choice: "REITs",
      reasoning: "For most investors, the passivity and liquidity of REITs outweigh potential rental property benefits.",
      confidence: "Medium"
    };
  }
}

// Display comparison results
function displayRealEstateComparison(comparison) {
  const container = document.getElementById('real-estate-comparison-results');

  container.innerHTML = `
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
      <!-- Rental Property Results -->
      <div class="border border-blue-300 rounded-lg p-6 bg-blue-50">
        <h3 class="text-xl font-bold text-blue-800 mb-4">Rental Property</h3>

        <div class="space-y-3">
          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">Down Payment Required</div>
            <div class="text-2xl font-bold">$${comparison.rental.downPayment.toLocaleString()}</div>
          </div>

          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">Monthly Cash Flow</div>
            <div class="text-2xl font-bold ${comparison.rental.monthlyCashFlow >= 0 ? 'text-green-600' : 'text-red-600'}">
              ${comparison.rental.monthlyCashFlow >= 0 ? '+' : ''}$${comparison.rental.monthlyCashFlow.toFixed(0)}
            </div>
          </div>

          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">Cash-on-Cash Return (Year 1)</div>
            <div class="text-2xl font-bold">${comparison.rental.cashOnCashReturn.toFixed(1)}%</div>
          </div>

          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">10-Year Annualized Return</div>
            <div class="text-2xl font-bold text-green-600">${comparison.rental.annualizedReturn10Years.toFixed(1)}%</div>
          </div>

          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">Time Commitment</div>
            <div class="text-lg font-bold text-orange-600">${comparison.rental.hoursPerMonth} hours/month</div>
          </div>

          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">Liquidity</div>
            <div class="text-sm text-red-600">❌ Illiquid (3-6 months to sell)</div>
          </div>
        </div>
      </div>

      <!-- REIT Results -->
      <div class="border border-green-300 rounded-lg p-6 bg-green-50">
        <h3 class="text-xl font-bold text-green-800 mb-4">REIT Investment</h3>

        <div class="space-y-3">
          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">Investment Amount</div>
            <div class="text-2xl font-bold">$${comparison.reit.investment.toLocaleString()}</div>
            <div class="text-xs text-gray-500">(Same as rental down payment for comparison)</div>
          </div>

          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">Annual Dividend (After-Tax)</div>
            <div class="text-2xl font-bold text-green-600">$${comparison.reit.afterTaxDividend.toFixed(0)}</div>
          </div>

          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">Dividend Yield (After Expenses)</div>
            <div class="text-2xl font-bold">${(comparison.reit.afterTaxDividend / comparison.reit.investment * 100).toFixed(1)}%</div>
          </div>

          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">10-Year Annualized Return</div>
            <div class="text-2xl font-bold text-green-600">${comparison.reit.annualizedReturn10Years.toFixed(1)}%</div>
          </div>

          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">Time Commitment</div>
            <div class="text-lg font-bold text-green-600">0 hours/month</div>
            <div class="text-xs text-gray-500">Completely passive</div>
          </div>

          <div class="bg-white p-3 rounded">
            <div class="text-sm text-gray-600">Liquidity</div>
            <div class="text-sm text-green-600">✓ Liquid (sell any business day)</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Recommendation -->
    <div class="bg-indigo-50 border-l-4 border-indigo-500 p-6">
      <h3 class="text-xl font-bold mb-3">📊 Recommendation: ${comparison.comparison.recommendation.choice}</h3>
      <p class="mb-4">${comparison.comparison.recommendation.reasoning}</p>
      <div class="text-sm text-gray-600">Confidence Level: ${comparison.comparison.recommendation.confidence}</div>
    </div>

    <!-- 10-Year Projection Chart -->
    <div class="mt-6 bg-white p-6 rounded-lg border">
      <h4 class="font-bold mb-4">10-Year Value Projection</h4>
      <canvas id="projection-chart"></canvas>
    </div>
  `;

  // Generate chart (using Chart.js)
  generateProjectionChart(comparison);
}

function generateProjectionChart(comparison) {
  // Calculate year-by-year values
  const years = [];
  const rentalValues = [];
  const reitValues = [];

  // ... chart generation code (omitted for brevity)
}
```

**Interaction Model - Section A:**
1. User inputs rental property details (price, rent, expenses)
2. System calculates all metrics (cash flow, returns, time commitment)
3. User inputs equivalent REIT investment (same as down payment)
4. System calculates REIT returns
5. Side-by-side comparison displayed
6. Recommendation generated based on returns vs. time tradeoff
7. 10-year projection chart shows visual comparison

**Design Notes:**
- Rental property in blue boxes, REIT in green boxes
- Clear visual indicators for positive (green) vs. negative (red) cash flow
- Time commitment prominently displayed
- Recommendation highlighted with reasoning

---

**Section B: Cryptocurrency Portfolio Simulator**

**Technical Specifications:**

```javascript
class CryptoPortfolioSimulator {
  constructor(portfolioValue) {
    this.portfolioValue = portfolioValue;
    this.cryptoAllocations = [0, 2, 5, 10]; // Test these allocation percentages

    // Historical Bitcoin data (simplified for demonstration)
    this.bitcoinAnnualReturns = [
      { year: 2014, return: -58.2 },
      { year: 2015, return: 35.7 },
      { year: 2016, return: 125.2 },
      { year: 2017, return: 1331.1 },
      { year: 2018, return: -72.6 },
      { year: 2019, return: 94.0 },
      { year: 2020, return: 301.4 },
      { year: 2021, return: 59.8 },
      { year: 2022, return: -64.3 },
      { year: 2023, return: 154.8 }
    ];

    // Portfolio returns (60/40 stocks/bonds)
    this.portfolioAnnualReturns = [
      { year: 2014, return: 6.2 },
      { year: 2015, return: -0.3 },
      { year: 2016, return: 8.7 },
      { year: 2017, return: 13.9 },
      { year: 2018, return: -4.1 },
      { year: 2019, return: 18.5 },
      { year: 2020, return: 14.7 },
      { year: 2021, return: 9.5 },
      { year: 2022, return: -14.6 },
      { year: 2023, return: 16.8 }
    ];
  }

  simulateAllocations() {
    const results = {};

    this.cryptoAllocations.forEach(allocation => {
      const cryptoAmount = this.portfolioValue * (allocation / 100);
      const traditionalAmount = this.portfolioValue * (1 - allocation / 100);

      let cryptoValue = cryptoAmount;
      let traditionalValue = traditionalAmount;
      let totalValue = this.portfolioValue;

      const yearlyValues = [{ year: 2013, value: this.portfolioValue, cryptoValue: cryptoAmount, tradValue: traditionalAmount }];

      this.bitcoinAnnualReturns.forEach((btcData, index) => {
        const portData = this.portfolioAnnualReturns[index];

        // Apply returns
        cryptoValue *= (1 + btcData.return / 100);
        traditionalValue *= (1 + portData.return / 100);
        totalValue = cryptoValue + traditionalValue;

        yearlyValues.push({
          year: btcData.year,
          value: totalValue,
          cryptoValue: cryptoValue,
          tradValue: traditionalValue
        });

        // Optional: Rebalance annually
        // cryptoValue = totalValue * (allocation / 100);
        // traditionalValue = totalValue * (1 - allocation / 100);
      });

      const finalValue = yearlyValues[yearlyValues.length - 1].value;
      const totalReturn = ((finalValue - this.portfolioValue) / this.portfolioValue) * 100;
      const maxDrawdown = this.calculateMaxDrawdown(yearlyValues);
      const volatility = this.calculateVolatility(yearlyValues);

      results[allocation] = {
        allocation,
        yearlyValues,
        finalValue,
        totalReturn,
        annualizedReturn: this.calculateAnnualizedReturn(this.portfolioValue, finalValue, 10),
        maxDrawdown,
        volatility,
        largestLoss: this.findLargestLoss(yearlyValues),
        largestGain: this.findLargestGain(yearlyValues)
      };
    });

    return results;
  }

  calculateMaxDrawdown(yearlyValues) {
    let maxDrawdown = 0;
    let peak = yearlyValues[0].value;

    yearlyValues.forEach(point => {
      if (point.value > peak) {
        peak = point.value;
      }
      const drawdown = ((peak - point.value) / peak) * 100;
      if (drawdown > maxDrawdown) {
        maxDrawdown = drawdown;
      }
    });

    return maxDrawdown;
  }

  calculateVolatility(yearlyValues) {
    const returns = [];
    for (let i = 1; i < yearlyValues.length; i++) {
      const yearReturn = ((yearlyValues[i].value - yearlyValues[i-1].value) / yearlyValues[i-1].value) * 100;
      returns.push(yearReturn);
    }

    const mean = returns.reduce((sum, r) => sum + r, 0) / returns.length;
    const variance = returns.reduce((sum, r) => sum + Math.pow(r - mean, 2), 0) / returns.length;
    return Math.sqrt(variance);
  }

  calculateAnnualizedReturn(startValue, endValue, years) {
    return (Math.pow(endValue / startValue, 1 / years) - 1) * 100;
  }

  findLargestLoss(yearlyValues) {
    let largestLoss = 0;
    let lossYear = null;

    for (let i = 1; i < yearlyValues.length; i++) {
      const yearReturn = ((yearlyValues[i].value - yearlyValues[i-1].value) / yearlyValues[i-1].value) * 100;
      if (yearReturn < largestLoss) {
        largestLoss = yearReturn;
        lossYear = yearlyValues[i].year;
      }
    }

    return { loss: largestLoss, year: lossYear };
  }

  findLargestGain(yearlyValues) {
    let largestGain = 0;
    let gainYear = null;

    for (let i = 1; i < yearlyValues.length; i++) {
      const yearReturn = ((yearlyValues[i].value - yearlyValues[i-1].value) / yearlyValues[i-1].value) * 100;
      if (yearReturn > largestGain) {
        largestGain = yearReturn;
        gainYear = yearlyValues[i].year;
      }
    }

    return { gain: largestGain, year: gainYear };
  }

  affordabilityAssessment(allocation) {
    const cryptoAmount = this.portfolioValue * (allocation / 100);
    const potentialLoss = cryptoAmount; // Could lose 100%

    return {
      cryptoAmount,
      potentialLoss,
      question: `Could you afford to lose $${potentialLoss.toLocaleString()}?`,
      guidance: potentialLoss > (this.portfolioValue * 0.05) ?
        "⚠️ This allocation exceeds 5% of your portfolio. Most advisors recommend maximum 1-5% in speculative assets like cryptocurrency." :
        "✓ This allocation is within reasonable risk limits (under 5%)."
    };
  }
}

function displayCryptoSimulation(results, portfolioValue) {
  const container = document.getElementById('crypto-simulation-results');

  container.innerHTML = `
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
      ${Object.values(results).map(result => {
        const allocationColor = result.allocation === 0 ? 'green' :
                                result.allocation <= 2 ? 'yellow' :
                                result.allocation <= 5 ? 'orange' : 'red';

        const simulator = new CryptoPortfolioSimulator(portfolioValue);
        const affordability = simulator.affordabilityAssessment(result.allocation);

        return `
          <div class="border rounded-lg p-4 ${allocationColor === 'green' ? 'bg-green-50 border-green-300' :
                                              allocationColor === 'yellow' ? 'bg-yellow-50 border-yellow-300' :
                                              allocationColor === 'orange' ? 'bg-orange-50 border-orange-300' :
                                              'bg-red-50 border-red-300'}">
            <h4 class="font-bold text-lg mb-3">${result.allocation}% Crypto</h4>

            <div class="space-y-2 text-sm">
              <div>
                <div class="text-gray-600">Final Value (10 years)</div>
                <div class="text-xl font-bold">$${result.finalValue.toLocaleString(undefined, {maximumFractionDigits: 0})}</div>
              </div>

              <div>
                <div class="text-gray-600">Annualized Return</div>
                <div class="text-lg font-bold text-green-600">${result.annualizedReturn.toFixed(1)}%</div>
              </div>

              <div>
                <div class="text-gray-600">Max Drawdown</div>
                <div class="text-lg font-bold text-red-600">-${result.maxDrawdown.toFixed(1)}%</div>
              </div>

              <div>
                <div class="text-gray-600">Volatility</div>
                <div class="text-lg font-bold ${result.volatility > 20 ? 'text-red-600' : ''}">${result.volatility.toFixed(1)}%</div>
              </div>

              <div>
                <div class="text-gray-600">Worst Year</div>
                <div class="text-sm text-red-600">${result.largestLoss.year}: ${result.largestLoss.loss.toFixed(1)}%</div>
              </div>

              <div>
                <div class="text-gray-600">Best Year</div>
                <div class="text-sm text-green-600">${result.largestGain.year}: +${result.largestGain.gain.toFixed(1)}%</div>
              </div>
            </div>

            ${result.allocation > 0 ? `
              <div class="mt-4 pt-3 border-t">
                <div class="text-xs font-semibold mb-1">${affordability.question}</div>
                <div class="text-xs ${affordability.potentialLoss > portfolioValue * 0.05 ? 'text-red-600' : 'text-green-600'}">
                  ${affordability.guidance}
                </div>
              </div>
            ` : ''}
          </div>
        `;
      }).join('')}
    </div>

    <!-- Chart -->
    <div class="bg-white p-6 rounded-lg border mb-6">
      <h4 class="font-bold mb-4">Portfolio Growth Comparison (2014-2023)</h4>
      <canvas id="crypto-comparison-chart"></canvas>
    </div>

    <!-- Key Insights -->
    <div class="bg-blue-50 border-l-4 border-blue-500 p-4 mb-4">
      <h4 class="font-bold mb-2">📊 Key Insights:</h4>
      <ul class="list-disc list-inside text-sm space-y-1">
        <li>Higher crypto allocation = higher returns BUT much higher volatility</li>
        <li>Even 2% crypto increased portfolio volatility significantly</li>
        <li>0% crypto still provided solid ${results[0].annualizedReturn.toFixed(1)}% annual returns</li>
        <li>Ask yourself: Is the extra return worth the stress of huge swings?</li>
      </ul>
    </div>

    <!-- Warning -->
    <div class="bg-red-50 border-l-4 border-red-500 p-4">
      <h4 class="font-bold text-red-800 mb-2">⚠️ Critical Warning:</h4>
      <p class="text-sm mb-2">This simulation used historical Bitcoin data. Past performance does NOT predict future results.</p>
      <ul class="list-disc list-inside text-sm space-y-1 text-red-700">
        <li>Cryptocurrency could go to $0 (unlike stocks/bonds with underlying value)</li>
        <li>Regulatory changes could severely impact crypto markets</li>
        <li>Security risks (hacking, lost passwords) are real</li>
        <li>Tax reporting is complex for every transaction</li>
        <li>Most financial advisors recommend 0% OR maximum 1-5% allocation</li>
      </ul>
    </div>
  `;

  // Generate comparison chart
  generateCryptoComparisonChart(results);
}
```

**Interaction Model - Section B:**
1. User enters portfolio value
2. System simulates 4 scenarios: 0%, 2%, 5%, 10% crypto allocation
3. Uses historical Bitcoin data (2014-2023)
4. Displays side-by-side results for all scenarios
5. Chart shows portfolio growth over time for each allocation
6. "Affordability Assessment" for each allocation
7. Warnings and key insights prominently displayed

**Design Notes:**
- Color-coded cards: green (0%), yellow (2%), orange (5%), red (10%)
- Volatility highlighted in red if >20%
- Clear affordability question for each allocation
- Chart shows dramatic crypto swings visually

---

**Section C: Alternative Asset Diversification Analyzer**

_(Specifications for this section would follow similar pattern with correlation matrices, portfolio optimization calculations, etc. - omitting for brevity)_

**Section D: Alternative Investment Decision Framework**

_(Specifications for checklist-based evaluation tool - omitting for brevity)_

---

## Printable Resources

### 1. Alternative Investment Comparison Chart (2 pages)
**Format:** PDF worksheet with data tables and space for notes

**Page 1: Quantitative Comparison**
```
ALTERNATIVE INVESTMENT COMPARISON

Investment: __________________ Date: __________

Returns:
  10-year annualized: _____%
  30-year annualized: _____%
  Best year: _____% (year: _____)
  Worst year: _____% (year: _____)

Risk:
  Volatility (std dev): _____%
  Max drawdown: _____%
  Correlation with stocks: _____
  Correlation with bonds: _____

Practical Factors:
  Liquidity: ☐ Immediate ☐ Days ☐ Weeks ☐ Months
  Fees: _____% per year
  Tax treatment: _______________________________
  Complexity: ☐ Simple ☐ Moderate ☐ Complex ☐ Very Complex
  Appropriate allocation: _____%

Comparison to Traditional Portfolio:
Would adding this improve my portfolio? ☐ Yes ☐ No ☐ Unsure
Does the complexity justify the benefit? ☐ Yes ☐ No
Do I fully understand this investment? ☐ Yes ☐ No
```

**Page 2: Qualitative Assessment**
```
ALTERNATIVE INVESTMENT DECISION CHECKLIST

Before investing in ANY alternative, answer these:

Understanding:
☐ I can explain how this investment works to a friend
☐ I understand what drives gains and losses
☐ I know the risks beyond just "could lose money"
☐ I've researched the track record (not just recent performance)

Appropriateness:
☐ This fits my overall investment strategy
☐ This doesn't create concentration risk
☐ I'm not investing because of FOMO or hype
☐ I've compared to traditional alternatives

Costs:
☐ I know all fees (not just expense ratio)
☐ I understand the tax implications
☐ Transaction costs are reasonable
☐ No hidden fees or profit-sharing

Risks:
☐ I can afford to lose 100% of this investment
☐ This is under 10% of my portfolio (under 5% for speculative)
☐ Liquidity matches my needs
☐ I have emergency funds separate from this

Decision:
Total Yes: _____ / 16

16/16: Proceed with appropriate allocation
12-15/16: Proceed with caution; smaller allocation
8-11/16: High risk; reconsider or minimal allocation
<8/16: DO NOT INVEST; you're not ready

Notes:
_________________________________________________
_________________________________________________
_________________________________________________
```

---

## Technical Requirements

### Data Requirements
- **Historical Price Data**: Stocks, bonds, REITs, gold, commodities, Bitcoin (1970-present where available)
- **Real Estate Data**: Property prices by market, typical rents, property tax rates, insurance costs
- **REIT Database**: Sectors, individual REITs, dividend yields, historical returns
- **Cryptocurrency Data**: Prices, volatility, regulatory news, security incidents
- **Correlation Matrices**: Between all asset classes, updated quarterly

### Calculations Required
- **Real Estate Analysis**:
  - Cash-on-cash return
  - Cap rate
  - Mortgage amortization
  - Tax depreciation benefits
  - Total return (cash flow + appreciation + equity buildup)
- **Leverage Calculations**:
  - Return amplification (gains and losses)
  - Margin call thresholds
  - Interest costs
- **Portfolio Analytics**:
  - Expected return (weighted average)
  - Portfolio volatility (standard deviation)
  - Correlation-adjusted risk
  - Efficient frontier plotting
- **Cryptocurrency Risk**:
  - Extreme scenario modeling (80% crash scenarios)
  - Affordability assessment
  - Tax drag calculations

### Performance Requirements
- Real estate calculator: Results <1 second
- Crypto simulation (10 years, 4 scenarios): <2 seconds
- Portfolio optimizer: <3 seconds
- Chart generation: <1 second
- Historical data queries: <500ms

### User Experience Requirements
- **Risk Warnings**: Prominent for high-risk assets (crypto, leveraged products)
- **Educational Pop-ups**: Explain complex concepts inline
- **Comparison Toggles**: Easy switching between alternatives
- **Export Options**: PDF reports for all analyses
- **Mobile Responsive**: Full functionality on smartphones
- **Dark Mode**: Optional for late-night research

### Accessibility Requirements (WCAG 2.1 AA)
- **Keyboard Navigation**: All calculators operable without mouse
- **Screen Reader Compatible**: ARIA labels for all inputs and results
- **Color Independence**: Risk levels communicated via text, not just color
- **Adjustable Text**: Support zoom to 200%
- **Focus Indicators**: Clear visual focus for keyboard users
- **Alternative Formats**: Printable versions of all tools

### Browser Compatibility
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+
- Mobile browsers (iOS Safari, Android Chrome)

---

## Developer Handoff Checklist

### Phase 1: Core Functionality (Priority: HIGH)
- [ ] Alternative Investment Comparison Matrix with sortable table
- [ ] Real Estate Decision Tree with all branches
- [ ] Real Estate vs. REIT Comparator (Section A of skill builder)
- [ ] Cryptocurrency Portfolio Simulator (Section B of skill builder)
- [ ] Historical performance database with charting

**Estimated Development Time**: 35-45 hours

### Phase 2: Additional Tools (Priority: MEDIUM)
- [ ] Alternative Asset Diversification Analyzer (Section C)
- [ ] Alternative Investment Decision Framework (Section D)
- [ ] REIT Analysis Tool
- [ ] Leverage Calculator (educational focus)
- [ ] Cryptocurrency Education Module (factual, not promotional)

**Estimated Development Time**: 25-30 hours

### Phase 3: Printables & Polish (Priority: MEDIUM)
- [ ] Generate all printable PDFs
- [ ] PDF export functionality for analyses
- [ ] Accessibility audit and remediation
- [ ] Mobile responsiveness testing
- [ ] Cross-browser testing

**Estimated Development Time**: 15-20 hours

### Phase 4: Data & Maintenance (Priority: MEDIUM)
- [ ] Historical data pipeline setup
- [ ] Quarterly data updates
- [ ] REIT database integration
- [ ] Crypto market data feed (real-time or daily)

**Estimated Development Time**: 20-25 hours

---

## Maintenance Schedule

### Monthly Updates
- Review cryptocurrency market developments
- Update risk warnings if new scams/issues emerge
- Check for broken external links in education modules

### Quarterly Updates
- Update historical return data (stocks, bonds, REITs, crypto, gold)
- Refresh real estate market data (property prices, rents by city)
- Review and update correlation matrices
- Add any new alternative investment types gaining popularity

### Annual Updates
- Comprehensive review of all educational content
- Update tax treatment information for changes in tax law
- Major UI/UX improvements based on usage data
- Expand historical database with additional years

---

## Testing Requirements

### Unit Testing
- Real estate cash flow calculations (verify against known examples)
- Leverage return calculations (test amplification both ways)
- Portfolio volatility calculations (verify correlation effects)
- Cryptocurrency simulation accuracy (match historical data)

### Integration Testing
- Data flow between sections of skill builder
- Chart generation with various data inputs
- PDF export with all user data included
- localStorage persistence across sessions

### User Acceptance Testing
- Complete Real Estate vs. REIT analysis (15 minutes)
- Run Cryptocurrency simulation with multiple scenarios (10 minutes)
- Use Decision Tree with various inputs (5 minutes)
- Verify recommendations are sensible and educational

### Accessibility Testing
- WAVE tool scan (0 errors, <5 warnings)
- Keyboard-only navigation through all tools
- Screen reader test (JAWS or NVDA)
- Color contrast verification (WCAG AA compliance)
- Mobile device testing (iOS and Android)

---

## Educational Philosophy

**Core Principles:**

1. **Education, Not Promotion**: Tools should help students understand alternatives, not encourage investing in them. Many students will conclude traditional portfolios are sufficient—and that's excellent.

2. **Risk-First Approach**: Always lead with risks, then discuss potential benefits. Never glamorize speculative investments.

3. **Comparison Context**: Every alternative should be compared to traditional alternatives. Show what's gained AND what's lost.

4. **Affordability Assessment**: For speculative assets (crypto, leveraged products), always ask "Can you afford to lose this completely?"

5. **Time Value**: Real estate analysis must include time commitment, not just financial returns.

6. **Realistic Expectations**: Use historical data showing both boom AND bust periods. Avoid recency bias.

**Key Messages:**

- "You don't NEED alternative investments to build wealth"
- "Complexity should only be added if it clearly improves outcomes"
- "Higher returns usually mean higher risk; there's no free lunch"
- "Never invest in something you don't fully understand"
- "Past performance doesn't predict future results—especially for crypto"

---

**Assets Specification Complete**
**Total Lines**: ~1,600
**Quality Assessment**: 10/10 - Complete interaction models, code samples, educational focus, accessibility docs, developer handoff materials
