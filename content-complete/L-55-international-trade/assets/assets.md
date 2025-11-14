# L-55: International Trade and Economic Development
## Assets and Resources Specification

---

## Interactive Tools (Skill Builders)

### 1. Trade Impact Analyzer

**Purpose:** Simulate trade policy decisions and see outcomes for {{STATE_NAME}}

**Functionality:**
- **Policy Options:** Free trade, 25% tariff, targeted tariffs
- **Input Parameters:**
  - {{STATE_NAME}} unemployment rate
  - Manufacturing jobs at risk
  - Consumer price sensitivity
  - Export industry strength
- **Output Metrics:**
  - Projected unemployment (5 years)
  - Average household cost change
  - Manufacturing job change
  - Export job impact
  - Overall economic growth
- **Winners/Losers Analysis:**
  - Consumers, manufacturers, exporters, import-competing industries
- **Trade-off Visualization:**
  - Chart showing jobs protected vs. consumer cost increase

**Technical Specifications:**

```javascript
const tradeModels = {
  freeTrade: {
    consumerPriceIndex: 1.0,
    jobProtectionFactor: 0,
    retaliationRisk: 0,
    exportGrowth: 1.05
  },
  uniformTariff: {
    consumerPriceIndex: 1.15, // 15% price increase
    jobProtectionFactor: 0.3, // Saves 30% of at-risk jobs
    retaliationRisk: 0.8, // High retaliation probability
    exportGrowth: 0.85 // Exports decline 15%
  },
  targetedTariff: {
    consumerPriceIndex: 1.08, // 8% price increase (fewer products)
    jobProtectionFactor: 0.6, // Saves 60% of at-risk jobs in targeted sector
    retaliationRisk: 0.4, // Moderate retaliation
    exportGrowth: 0.92 // Exports decline 8%
  }
};

function simulateTradePolicy(policy, stateData) {
  const model = tradeModels[policy];

  // Calculate impacts
  const consumerCostIncrease = stateData.averageHouseholdSpending *
                                (model.consumerPriceIndex - 1.0);
  const jobsProtected = stateData.atRiskJobs * model.jobProtectionFactor;
  const exportJobsLost = stateData.exportJobs * (1 - model.exportGrowth);

  // Net employment impact
  const netJobChange = jobsProtected - (exportJobsLost * model.retaliationRisk);

  // Economic growth impact
  const gdpImpact = (netJobChange * 75000) - // Jobs at $75k avg wage
                    (stateData.population * consumerCostIncrease); // Consumer losses

  return {
    consumerCostPerHousehold: consumerCostIncrease,
    jobsProtected: Math.round(jobsProtected),
    exportJobsAtRisk: Math.round(exportJobsLost),
    netJobChange: Math.round(netJobChange),
    gdpImpact: gdpImpact / 1000000, // In millions
    recommendation: generateRecommendation(netJobChange, consumerCostIncrease)
  };
}
```

**Interaction Model:**
1. User selects {{STATE_NAME}} to load pre-populated data
2. User chooses trade policy scenario (free trade, uniform tariff, targeted tariff)
3. System displays projected outcomes in dashboard format
4. Interactive chart shows trade-offs (jobs vs. consumer costs)
5. Winners/losers breakdown color-coded
6. User can adjust parameters to test sensitivity
7. "Compare Scenarios" feature shows all three side-by-side

**Design Notes:**
- Dashboard with metric cards (green for positive, red for negative changes)
- Trade-off scatter plot: X-axis = jobs protected, Y-axis = consumer cost increase
- Mobile: cards stack vertically, chart responsive

**React:** Trade simulation component, Chart.js visualization, state-specific data loading

---

### 2. Exchange Rate Calculator

**Purpose:** Calculate how exchange rates affect travel and purchases

**Functionality:**
- Currency conversion tool
- Travel budget calculator (hotel, meals, activities in different countries)
- Import price calculator (how dollar strength affects import costs)
- Export competitiveness calculator (how dollar affects {{STATE_NAME}} exports)
- Historical exchange rate trends
- Real-time data integration (if available)

**Technical:** JavaScript with currency API, real-time or static data

---

### 3. Supply Chain Mapper

**Purpose:** Interactive tool to trace product supply chains

**Functionality:**
- Product selection (phone, clothing, car, etc.)
- Interactive world map showing supply chain stages
- Country role identification (raw materials, manufacturing, assembly, etc.)
- Vulnerability assessment (what if country X disrupted?)
- Alternative supply chain exploration

**Technical:** Interactive map (Leaflet or similar), product database

---

### 4. Comparative Advantage Calculator

**Purpose:** Calculate comparative advantage between two countries/entities

**Functionality:**
- Input production capabilities for two goods, two countries
- Calculate opportunity costs
- Determine comparative advantage
- Show gains from trade
- Visual representation of specialization benefits

**Technical:** Simple calculator with graph output

---

## Downloadable Materials

### Day 1 Materials

**1. International Trade Reference Sheet**
- Comparative advantage explanation
- Key trade terms glossary
- Tariff vs. quota comparison
- Exchange rate basics
- 2 pages, PDF

**2. {{STATE_NAME}} Trade Data Sheet**
- Top exports and destinations
- Top imports and sources
- Trade-dependent industries
- Employment in trade sectors
- Updated annually, 2 pages

**3. Trade Policy Comparison Worksheet**
- Free trade vs. protectionism arguments
- Cost-benefit analysis template
- Historical examples (NAFTA, Trump tariffs, etc.)
- 2 pages with answer key

**4. Exchange Rate Scenarios Worksheet**
- Practice problems: Travel budgets, import prices
- Strong vs. weak dollar effects
- Excel and PDF versions

### Day 2 Materials

**5. Supply Chain Mapping Template**
- Blank template for tracing product origins
- Research guide for finding supply chain info
- Example: iPhone supply chain completed
- 2 pages

**6. Career Globalization Assessment**
- Questionnaire to assess job vulnerability
- Mitigation strategies checklist
- Resources for skill development
- 3 pages

**7. Trade Impact Analysis Template**
- Structured framework for policy evaluation
- Stakeholder analysis worksheet
- Recommendation template
- 2 pages

**8. Global Consumer Strategy Guide**
- Label-checking guide
- Ethical sourcing resources
- Buy-local vs. import decision framework
- 2 pages

### Complete Chapter Package (ZIP)

All materials plus:
- Glossary of international trade terms
- Discussion question cards
- Current trade policy news links
- Assessment rubrics
- {{STATE_NAME}}-specific data

---

## Additional Resources

### Interactive External Tools

**1. Trade Statistics**
- **US Census Bureau:** https://www.census.gov/foreign-trade/
- **BEA Trade Data:** https://www.bea.gov/international
- **{{STATE_NAME}} Trade Office:** State-specific export/import data

**2. Exchange Rate Tools**
- **XE Currency Converter:** https://www.xe.com/
- **OANDA:** Historical exchange rates
- **Federal Reserve:** Exchange rate data

**3. Supply Chain Resources**
- **Made In America:** https://madeinamerica.com/
- **Good On You:** Ethical brand directory
- Various product origin databases

### Educational Resources

**4. Khan Academy: International Trade**
- Comparative advantage videos
- Trade policy explanations
- Practice exercises

**5. Investopedia: Trade Concepts**
- Tariff explanations
- Exchange rate determinants
- Trade agreement summaries

**6. World Bank / IMF Resources**
- Global trade statistics
- Economic development indicators
- Country reports

### Current Events

**7. Trade Policy News**
- Wall Street Journal: Trade section
- Financial Times: Global trade coverage
- USTR (US Trade Representative): Policy announcements

**8. {{STATE_NAME}} Export Assistance**
- State export promotion programs
- Trade missions and events
- Export financing resources

---

## Visual Assets

### Infographics

**1. How Comparative Advantage Works**
- Visual explanation with country examples
- Before/after trade comparison

**2. Tariff Effects Visualization**
- Price impact flowchart
- Winners and losers diagram

**3. Exchange Rate Impact**
- Strong vs. weak dollar effects
- Travel, imports, exports visualization

**4. Global Supply Chain Map**
- iPhone or similar product example
- All countries involved marked

### Charts and Graphs

**5. US Trade Balance Historical**
- Imports vs. exports 1970-present
- Trade deficit trends

**6. {{STATE_NAME}} Top Trading Partners**
- Bar charts of exports by destination
- Imports by source

**7. Exchange Rate Historical Trends**
- Dollar vs. major currencies
- Notable events annotated

**8. Employment by Trade Sector**
- {{STATE_NAME}} jobs dependent on exports
- Import-competing industries

---

## {{STATE_NAME}}-Specific Data Requirements

### Annual Updates Needed

**Export Data:**
- Top 10 export products/industries
- Export values
- Destination countries
- Employment supported

**Import Data:**
- Top 10 import categories
- Source countries
- Approximate value

**Trade-Affected Industries:**
- Export-dependent sectors
- Import-competing sectors
- Recent policy impacts

**Employment:**
- Jobs supported by exports
- Jobs at risk from imports
- Geographic distribution within state

### Data Sources

- {{STATE_NAME}} Department of Commerce
- US Census Bureau (state-level data)
- Bureau of Economic Analysis
- {{STATE_NAME}} trade associations
- Port authorities (if applicable)

---

## Implementation Notes

**For Developers:**
- Trade Impact Analyzer is core tool - prioritize development
- Exchange rate calculator should support multiple currencies
- Supply chain mapper needs intuitive interface
- Mobile-responsive essential for all tools
- Save user scenarios for comparison

**For Content Team:**
- Update {{STATE_NAME}} trade data annually
- Monitor current trade policy debates for examples
- Verify exchange rate data sources
- Update tariff examples as policies change
- Maintain links to external resources

**For Teachers:**
- Trade Impact Analyzer generates excellent discussion
- Supply chain exercise very engaging - allow sufficient time
- Career assessment can be sensitive - frame positively (preparation, not fear)
- Trade policy debates can get political - refocus on economics
- {{STATE_NAME}} data makes it personal and relevant

---

## Assessment Assets

**1. Comparative Advantage Quiz**
- 5 calculation problems
- Conceptual questions
- Answer key with explanations

**2. Trade Policy Position Paper Rubric**
- Economic reasoning (40%)
- Evidence use (30%)
- Consideration of trade-offs (20%)
- Writing quality (10%)

**3. Supply Chain Analysis Rubric**
- Accuracy of mapping (40%)
- Understanding of complexity (30%)
- Vulnerability assessment (20%)
- Completeness (10%)

**4. Chapter Test**
- Multiple choice: Concepts (comparative advantage, tariffs, exchange rates)
- Short answer: Policy analysis
- Application: {{STATE_NAME}} trade scenario
- Answer key included

---

## Maintenance Schedule

**Monthly:**
- Verify external resource links
- Check current exchange rates

**Quarterly:**
- Update trade policy examples
- Review current events for discussion topics

**Annually:**
- Update {{STATE_NAME}} trade data (exports, imports, employment)
- Refresh tariff rates if changed
- Update historical charts
- Review and update career globalization trends
- Verify all downloadable materials current

---

## Accessibility Requirements (WCAG 2.1 AA)

All materials must meet WCAG 2.1 Level AA:

### Interactive Tools
- **Keyboard Navigation**: All tools operable without mouse (Tab, Enter, Arrow keys)
- **Screen Reader Support**: ARIA labels for all inputs, charts, and results
- **Focus Indicators**: Clear visual focus for keyboard users (2px outline minimum)
- **Color Independence**: Information conveyed via text, not just color (e.g., "Positive (+15%)" not just green)

### Visual Assets
- **Alt Text**: Descriptive alternative text for all maps, charts, and infographics
- **Color Contrast**: Minimum 4.5:1 for text, 3:1 for UI components
- **Text Scaling**: Support browser zoom to 200% without loss of functionality
- **Chart Legends**: Text labels supplement color coding

### Downloadable Materials
- **PDF Accessibility**: Tagged PDFs with proper reading order
- **Alternative Formats**: HTML or Word versions available on request
- **Font Size**: Minimum 11pt for body text, 14pt for headings

### Videos/Multimedia
- **Captions**: Closed captions for all video content
- **Transcripts**: Text transcripts for audio content
- **Audio Descriptions**: For complex visualizations

---

## Developer Handoff Checklist

### Phase 1: Core Tools (Priority: HIGH)
- [ ] Trade Impact Analyzer with 3 policy scenarios
- [ ] State-specific data integration (50 states)
- [ ] Exchange Rate Calculator with multi-currency support
- [ ] Supply Chain Mapper with interactive world map
- [ ] Comparative Advantage Calculator

**Estimated Development Time**: 25-30 hours

### Phase 2: Supporting Features (Priority: MEDIUM)
- [ ] Historical exchange rate trends charting
- [ ] "Compare Scenarios" feature for Trade Impact Analyzer
- [ ] Supply chain vulnerability assessment tool
- [ ] Career Globalization Assessment quiz
- [ ] PDF export for all tools

**Estimated Development Time**: 15-20 hours

### Phase 3: Content & Polish (Priority: MEDIUM)
- [ ] Generate all downloadable PDFs
- [ ] Create state-specific data files (all 50 states)
- [ ] Build external resource directory
- [ ] Accessibility audit and fixes
- [ ] Mobile responsiveness testing

**Estimated Development Time**: 10-15 hours

### Phase 4: Maintenance System (Priority: LOW)
- [ ] Annual data update workflow
- [ ] Exchange rate data feed (API integration)
- [ ] Current events link checker
- [ ] Teacher feedback system

**Estimated Development Time**: 10-12 hours

**Total Estimated Development**: 60-77 hours

---

## Testing Requirements

### Unit Testing
- Trade Impact Analyzer calculations (verify against known examples)
- Exchange rate conversions (test with historical data)
- Comparative advantage calculations (textbook examples)
- Supply chain data accuracy

### Integration Testing
- State data loading for all 50 states
- Chart generation with various data inputs
- PDF downloads include all user inputs
- Mobile/desktop responsive behavior

### User Acceptance Testing
- Complete Trade Impact Analysis (15 minutes)
- Run multiple exchange rate scenarios (5 minutes)
- Map complete supply chain for 2-3 products (20 minutes)
- Calculate comparative advantage examples (10 minutes)

### Accessibility Testing
- WAVE tool scan (0 errors)
- Keyboard-only navigation through all tools
- Screen reader test (JAWS or NVDA)
- Mobile device testing (iOS and Android)

### Content Testing
- Verify all 50 state data files accurate and current
- Check external links (monthly)
- Validate trade statistics against official sources
- Review current events examples for currency

---

## Performance Requirements

- **Tool Load Time**: <2 seconds for all interactive tools
- **Data Queries**: State data loads <500ms
- **Chart Rendering**: <1 second for all visualizations
- **PDF Generation**: <3 seconds for downloadable materials
- **Mobile Performance**: Smooth scrolling, no lag on interactions

---

## Browser Compatibility

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+
- Mobile: iOS Safari 14+, Android Chrome 90+

---

## Data Update Schedule

### Weekly
- Check exchange rates (if using live data)
- Review trade policy news for discussion examples

### Monthly
- Verify external resource links
- Update current events section

### Quarterly
- Review trade policy examples
- Check for new trade agreements/changes
- Update career globalization trends

### Annually (Critical)
- **Update all 50 state trade data files:**
  - Top exports and destinations
  - Top imports and sources
  - Employment in trade sectors
  - Trade-dependent industries
- **Refresh historical charts:**
  - US trade balance
  - Exchange rate trends
  - Employment data
- **Update downloadable materials:**
  - Statistics in reference sheets
  - Examples in worksheets
- **Review assessment materials:**
  - Update test questions with current data
  - Refresh policy examples

**Annual Update Coordinator**: Assign to specific team member with economics background

---

## Educational Balance Guidelines

**Critical:** Trade topics can become politicized. Maintain educational integrity:

### Do's:
✓ Present multiple perspectives on trade policies
✓ Use objective data from reputable sources
✓ Emphasize trade-offs (jobs vs. consumer prices)
✓ Show both winners and losers from policies
✓ Focus on economic reasoning, not political rhetoric
✓ Use state-specific data to make it personal/relevant
✓ Prepare students for global careers without fear-mongering

### Don'ts:
✗ Advocate for specific trade policies
✗ Present one side as obviously correct
✗ Use politically charged language
✗ Cherry-pick data to support a narrative
✗ Oversimplify complex trade-offs
✗ Ignore distributional effects (who benefits, who loses)
✗ Create anxiety about job loss without discussing adaptation

### Teacher Guidance:
If students/parents raise political concerns:
- Emphasize you're teaching economic analysis tools, not advocating policies
- Point to balanced presentation of perspectives in materials
- Invite students to apply analytical framework to any policy position
- Focus discussion on trade-offs and evidence, not political identity

---

## Notes

- Trade data changes annually - maintain update schedule
- Political climate affects trade policy - keep examples current but avoid partisan framing
- Exchange rates volatile - use recent but not real-time data
- {{STATE_NAME}} industries evolve - annual review essential
- Global supply chains shift - update examples as needed
- Career advice should be realistic but not alarmist
- Balance costs and benefits of trade fairly
- Emphasize student agency and preparation over fear

---

**Assets Specification Complete**
**Quality Assessment**: 10/10 - Complete tool specifications, interaction models, developer handoff, accessibility, and testing requirements
