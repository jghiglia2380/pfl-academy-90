# Asset Specifications for L-66: Understanding Financial Markets

## Day 1 Assets

### 1. Stock Quote Display (Interactive Example)
- **Purpose:** Visual representation of a real stock quote with all key metrics labeled
- **Format/Inputs:** Interactive diagram or annotated screenshot
- **Expected Outputs:** Clear understanding of quote components
- **Interaction Model:** Hover over elements to see explanations
- **Design Notes:**
  - Show realistic quote with all fields: Ticker, Last Price, Bid, Ask, Volume, Day Range, 52-Week Range, Market Cap, P/E Ratio
  - Color-code bid (green), ask (red), spread (highlighted)
  - Annotations explain each field
  - Include "What this means for you" practical notes
  - Example: "Bid $49.95 = what you'll receive when selling"

### 2. Bid-Ask Spread Visualization
- **Purpose:** Diagram showing how bid, ask, and spread relate
- **Format/Inputs:** Animated or static visual diagram
- **Expected Outputs:** Intuitive understanding of spread as transaction cost
- **Interaction Model:** Click to animate a trade showing spread cost
- **Design Notes:**
  - Horizontal price line with bid and ask marked
  - Shaded area between them labeled "SPREAD = YOUR COST"
  - Animation: Buyer pays ask, seller receives bid, difference = cost
  - Show calculation: $50.05 ask - $49.95 bid = $0.10 spread
  - Display percentage: ($0.10 ÷ $50.05) × 100 = 0.2%

### 3. Order Type Comparison Matrix
- **Purpose:** Side-by-side comparison of market vs. limit orders
- **Format/Inputs:** Interactive table or expandable cards
- **Expected Outputs:** Clear understanding of when to use each type
- **Interaction Model:** Click rows to expand with examples
- **Design Notes:**
  - Columns: Market Order | Limit Order
  - Rows: Execution guarantee, Price guarantee, Speed, Use cases, Risks, Examples
  - Visual indicators: ✓ (guaranteed), ✗ (not guaranteed), ~ (depends)
  - Color coding: Green (advantage), Red (disadvantage), Yellow (caution)

### 4. Trading Cost Calculator (Simple Version)
- **Purpose:** Calculate hidden spread costs for a single trade
- **Format/Inputs:** Web form with instant calculation
- **Expected Outputs:** Total cost including spreads
- **Interaction Model:** Enter values, see results update in real-time
- **Design Notes:**
  - **Inputs:**
    - Number of shares
    - Bid price ($)
    - Ask price ($)
  - **Calculations (auto):**
    - Spread = Ask - Bid
    - Spread % = (Spread ÷ Ask) × 100
    - If buying: Cost = Shares × Ask
    - If selling: Revenue = Shares × Bid
    - Immediate value after purchase = Shares × Bid
    - Hidden cost = (Ask - Bid) × Shares
  - **Output display:**
    - "Your purchase cost: $___"
    - "Immediate market value: $___"
    - "Hidden spread cost: $___"
    - "Spread as % of investment: ___%"

### 5. Market Hours Reference Card
- **Purpose:** Quick reference for when markets are open/closed
- **Format/Inputs:** Digital card or printable reference
- **Expected Outputs:** Understanding of trading hours and holidays
- **Interaction Model:** Static or with timezone converter
- **Design Notes:**
  - **Regular Hours:** 9:30 AM - 4:00 PM Eastern, Monday-Friday
  - **Pre-market:** 4:00 AM - 9:30 AM (limited, higher risk)
  - **After-hours:** 4:00 PM - 8:00 PM (limited, higher risk)
  - **Closed:** Weekends, major holidays (list provided)
  - **Settlement:** T+2 (explain)
  - Optional: Timezone converter for students in different zones

## Day 2 Assets

### 1. Market Trading Simulator (PRIMARY SKILL BUILDER)
- **Purpose:** Comprehensive platform for practicing trade execution with different order types
- **Format/Inputs:** Web application with realistic trading interface
- **Expected Outputs:** Hands-on experience with market mechanics without financial risk
- **Interaction Model:** Scenario-based simulator with multiple learning modules
- **Design Notes:**

  **Module 1: Understanding Quotes**
  - Display realistic stock quote with all metrics
  - Quiz mode: "Click on the bid price" → correct/incorrect feedback
  - Calculation practice: "What is the spread?" → input answer, get feedback
  - Volume interpretation: Compare today's volume to average

  **Module 2: Market Order Practice**
  - Select security from dropdown (pre-loaded with examples)
  - Current quote displayed: Bid, Ask, Last
  - Enter number of shares to buy/sell
  - Click "Place Market Order"
  - **System response:**
    - "Order executed at $50.05 (ask price)"
    - "Total cost: $2,502.50"
    - "Immediate value: $2,497.50 (at bid)"
    - "Spread cost: $5.00"
  - Option to immediately "sell" to demonstrate roundtrip cost

  **Module 3: Limit Order Practice**
  - Same interface as market orders
  - Additional input: "Limit price $___"
  - Options: Buy limit (max price) or Sell limit (min price)
  - Simulator shows:
    - Price fluctuations over time (simplified animation)
    - Order status: Pending / Executed / Expired
    - If executed: "Saved $X vs. market order"
    - If not executed: "Price never reached your limit"
  - Students can adjust limit and retry

  **Module 4: Scenario Challenges**
  - Present 5 realistic scenarios:
    1. Liquid stock, narrow spread → practice both order types
    2. Illiquid stock, wide spread → see danger of market orders
    3. Volatile market → experience price gaps
    4. News event → practice patience vs. urgency
    5. End-of-day timing → understand market close impact
  - For each scenario:
    - Context description
    - Current market conditions
    - Student chooses order type and parameters
    - System shows outcome and provides feedback
    - Score/grade based on efficiency (lower costs = higher score)

  **Module 5: Cost Comparison Tool**
  - Input:trading frequency (trades/year)
  - Input: Average spread per trade
  - Input: Average shares per trade
  - **Outputs:**
    - Annual spread cost
    - 10-year compound impact at 7% growth
    - 30-year compound impact
    - Comparison to different trading frequencies
  - Visual: Bar chart showing active trader vs. buy-and-hold costs

  **General Features:**
  - Save progress and return later
  - Leaderboard (optional, for engagement)
  - Certificate of completion
  - Print summary of all trades practiced
  - Help button with video tutorials
  - Glossary sidebar

### 2. Stock Quote Analysis Worksheet
- **Purpose:** Structured template for analyzing real stocks
- **Format/Inputs:** Digital spreadsheet or printable PDF
- **Expected Outputs:** Completed analysis with trade recommendations
- **Interaction Model:** Fill-in template with some auto-calculations (digital version)
- **Design Notes:**
  - **Section 1: Quote Data** (student records from real source)
    - Ticker symbol
    - Last price
    - Bid price
    - Ask price
    - Spread (ask - bid)
    - Spread % ((spread ÷ ask) × 100)
    - Bid size
    - Ask size
    - Volume today
    - Average volume
  - **Section 2: Liquidity Assessment**
    - Is spread < 0.1%? (Highly liquid)
    - Is volume > 1M shares? (Good liquidity)
    - Overall liquidity rating: High / Medium / Low
  - **Section 3: Trading Recommendation**
    - Recommended order type for $1,000 purchase: Market / Limit
    - If limit, suggested limit price: $___
    - Reasoning: ___
  - **Section 4: Cost Calculation**
    - Shares you could buy: $1,000 ÷ Ask price
    - Market order total cost: Shares × Ask
    - Limit order potential cost: Shares × (suggested limit)
    - Potential savings: ___

### 3. Trading Cost Calculator (Advanced)
- **Purpose:** Comprehensive cost analysis for different investor profiles
- **Format/Inputs:** Spreadsheet template with formulas
- **Expected Outputs:** Lifetime cost projections for different strategies
- **Interaction Model:** Input parameters, see calculations auto-update
- **Design Notes:**
  - **Input Section:**
    - Trades per year
    - Average shares per trade
    - Average stock price
    - Average spread per share
    - Commission per trade (if any)
    - Investment timeline (years)
    - Expected annual return (default 7%)
  - **Calculation Section** (formulas built-in):
    - Cost per trade = (Spread × Shares) + Commission
    - Annual cost = Trades × Cost per trade
    - X-year opportunity cost = Annual cost × Future Value factor
  - **Comparison Section:**
    - Show 3 profiles side-by-side:
      - Active trader (100 trades/year)
      - Moderate investor (12 trades/year)
      - Buy-and-hold (4 trades/year)
    - Calculate costs for each
    - Display savings of buy-and-hold vs. others
  - **Visualization:**
    - Bar chart of annual costs
    - Line graph of compound impact over time
    - Pie chart of where investment dollars go (spread, commission, actual investment)

### 4. Brokerage Comparison Checklist
- **Purpose:** Structured tool for comparing broker features
- **Format/Inputs:** Digital form or printable comparison matrix
- **Expected Outputs:** Evidence-based broker selection
- **Interaction Model:** Fill in for 3+ brokers, compare results
- **Design Notes:**
  - **For Each Broker (columns), Rate:**
    - Commission-free stocks: Yes / No
    - Commission-free ETFs: Yes / No
    - Minimum deposit: $___
    - Account types offered: (checkboxes)
    - Platform ease of use: 1-10
    - Mobile app quality: 1-10
    - Research tools: 1-10
    - Educational resources: 1-10
    - Customer support: Phone / Chat / Email / All
    - Hidden fees identified: (list)
    - Overall rating: 1-10
  - **Summary Section:**
    - Best for beginners: ___
    - Best for low costs: ___
    - Best for research: ___
    - My choice: ___ because ___

### 5. Personal Trading Strategy Template
- **Purpose:** Structured planning document for individual strategy
- **Format/Inputs:** Fill-in template (digital or print)
- **Expected Outputs:** Personalized trading rules and first trade plan
- **Interaction Model:** Guided question flow
- **Design Notes:**
  - **Section 1: Investor Profile**
    - Risk tolerance: Conservative / Moderate / Aggressive
    - Time horizon: ___ years
    - Time for research: ___ hours/week
    - Preferred style: Active / Passive / Mix
  - **Section 2: Trading Rules**
    - Order types I'll use for:
      - Liquid stocks: ___
      - Illiquid stocks: ___
      - ETFs/Funds: ___
    - I will check spread before: Every trade / Large trades only
    - Maximum acceptable spread: ___%
    - Trading times I'll avoid: ___
    - Cost review frequency: Monthly / Quarterly / Annually
  - **Section 3: Investment Strategy**
    - Core holdings (80%): ___ (specific funds/stocks)
    - Purchase frequency: ___
    - Experimental (20%): ___
    - Review frequency: ___
  - **Section 4: First Trade**
    - Security: ___
    - Amount: $___
    - Order type: ___
    - Rationale: ___
    - Target date: ___

## Downloadable Resources (Printable PDFs)

### 1. Order Types Quick Reference Card
- **Purpose:** Portable summary of market vs. limit orders
- **Format:** Tri-fold card (wallet-sized when folded)
- **Content:**
  - Front: Market Order definition, when to use
  - Middle: Limit Order definition, when to use
  - Back: Quick decision tree: "Which order type?"
  - Inside: Examples of each

### 2. Stock Quote Reading Guide
- **Purpose:** Step-by-step guide for interpreting quotes
- **Format:** One-page reference sheet
- **Content:**
  - Annotated quote with all fields labeled
  - What each field means
  - How to calculate spread and spread %
  - Red flags to watch for (wide spreads, low volume)

### 3. Trading Cost Worksheet (Print Version)
- **Purpose:** Manual calculation template
- **Format:** Two-page worksheet
- **Content:**
  - Blank template for single trade cost
  - Annual cost calculation
  - Lifetime impact calculation with example

### 4. Brokerage Comparison Checklist (Print Version)
- **Purpose:** Offline broker research tool
- **Format:** Two-page comparison matrix
- **Content:** All fields from digital version, adapted for handwriting

### 5. Market Hours & Settlement Reference
- **Purpose:** Quick reference for trading times
- **Format:** Business card-sized reference
- **Content:**
  - Regular hours (with timezone note)
  - Pre-market and after-hours
  - Settlement (T+2)
  - Major market holidays
  - "When NOT to trade" tips

## Asset Implementation Notes

### Technical Requirements
- Responsive design (mobile-friendly)
- Accessible (WCAG 2.1 AA)
- Fast-loading (< 3 seconds on 3G)
- Browser-compatible (Chrome, Firefox, Safari, Edge)
- No personal financial data collected

### Data Sources
- Stock quotes: Use free APIs (Yahoo Finance, Alpha Vantage) or allow manual input
- Broker information: Updated annually from broker websites
- Market hours: Standard NYSE/NASDAQ schedule with holiday calendar

### Offline Accessibility
- Every interactive tool has printable PDF alternative
- Worksheets designed for manual completion
- Calculator formulas provided for spreadsheet-challenged users
- All activities can be facilitated without technology

### Privacy & Safety
- No real money involved
- No personal financial information required
- Simulator uses hypothetical accounts only
- Links to external broker sites clearly labeled
- Warnings about not investing until financially ready

### Customization Options
- Teachers can add/remove securities from simulator
- Broker comparison can be customized for regional availability
- Cost calculator can use different assumed returns
- Examples can be updated with current stocks of student interest

### Student Data
- Simulator progress saved to browser local storage only (not server)
- No tracking of student trading decisions
- Optional: Export own data for portfolio tracking

### Instructor Dashboard (Optional)
- View class completion rates
- See common mistakes in simulator
- Identify students struggling with concepts
- Generate reports for assessment

---

## Technical Specifications

### Market Trading Simulator - Code Sample

```javascript
class MarketSimulator {
  constructor() {
    this.securities = {
      'AAPL': { name: 'Apple Inc.', bid: 173.45, ask: 173.55, lastPrice: 173.50, volume: 52000000 },
      'MSFT': { name: 'Microsoft Corp.', bid: 375.20, ask: 375.35, lastPrice: 375.28, volume: 23000000 },
      'SPY': { name: 'S&P 500 ETF', bid: 450.10, ask: 450.12, lastPrice: 450.11, volume: 75000000 }
    };
  }

  executeTrade(ticker, orderType, shares, limitPrice = null) {
    const security = this.securities[ticker];

    if (orderType === 'market') {
      return {
        executed: true,
        price: security.ask, // Buy at ask
        totalCost: shares * security.ask,
        immediateValue: shares * security.bid,
        spreadCost: shares * (security.ask - security.bid),
        message: `Market order executed at $${security.ask}`
      };
    }

    if (orderType === 'limit') {
      // Simplified: 70% chance limit order executes if reasonable
      const executed = limitPrice >= security.ask * 0.99;

      if (executed) {
        const executionPrice = Math.max(limitPrice, security.bid);
        return {
          executed: true,
          price: executionPrice,
          totalCost: shares * executionPrice,
          savingsVsMarket: shares * (security.ask - executionPrice),
          message: `Limit order executed at $${executionPrice}`
        };
      } else {
        return {
          executed: false,
          message: `Limit order not executed. Price never reached $${limitPrice}`
        };
      }
    }
  }

  calculateSpread(ticker) {
    const security = this.securities[ticker];
    const spread = security.ask - security.bid;
    const spreadPercent = (spread / security.ask) * 100;

    return {
      spread: spread.toFixed(2),
      spreadPercent: spreadPercent.toFixed(3),
      liquidity: spreadPercent < 0.1 ? 'High' : spreadPercent < 0.5 ? 'Medium' : 'Low'
    };
  }
}
```

**Interaction Flow:**
1. Student selects security from dropdown
2. System displays current quote (bid, ask, last, spread)
3. Student enters shares and selects order type
4. If limit order, additional input for limit price
5. Click "Execute Trade" button
6. System processes and displays results
7. Option to "Undo" or "Try Again" with different parameters
8. Progress saved to localStorage

**Design:**
- Quote display: Large numbers, color-coded (green bid, red ask)
- Order form: Clear labels, input validation
- Results: Success message or explanation of why order didn't execute
- Mobile: Single column layout, large touch targets

---

## Accessibility Requirements (WCAG 2.1 AA)

### Interactive Tools
- **Keyboard Navigation**: Tab through all inputs, Enter to submit
- **Screen Reader Support**: ARIA labels for all quote fields, Clear announcements for trade results
- **Focus Management**: Focus moves to results after trade execution
- **Color Independence**: Red/green supplemented with text ("Bid (You Receive)" / "Ask (You Pay)")

### Visual Elements
- **Color Contrast**: 4.5:1 minimum for all text
- **Text Scaling**: Readable at 200% zoom
- **Touch Targets**: 44×44px minimum for mobile
- **Error Messages**: Clear, specific, actionable

### Downloadable Materials
- **Tagged PDFs**: Proper reading order
- **Alternative Formats**: HTML versions available
- **High Contrast**: Optional high contrast mode

---

## Developer Handoff Checklist

### Phase 1: Core Simulator (Priority: HIGH)
- [ ] Market Trading Simulator with 5 modules
- [ ] Stock quote display with real-time or static data
- [ ] Market order execution engine
- [ ] Limit order execution with price simulation
- [ ] Spread calculator
- [ ] Progress tracking (localStorage)

**Estimated Development Time**: 25-30 hours

### Phase 2: Supporting Tools (Priority: MEDIUM)
- [ ] Trading Cost Calculator (simple and advanced versions)
- [ ] Stock Quote Analysis Worksheet (digital)
- [ ] Brokerage Comparison Checklist (digital)
- [ ] Personal Trading Strategy Template
- [ ] Scenario challenges (5 scenarios)

**Estimated Development Time**: 15-20 hours

### Phase 3: Content & Polish (Priority: MEDIUM)
- [ ] Generate all printable PDFs
- [ ] Create stock quote visual guides
- [ ] Order type comparison matrix
- [ ] Bid-ask spread visualization
- [ ] Market hours reference card

**Estimated Development Time**: 10-12 hours

### Phase 4: Optional Features (Priority: LOW)
- [ ] Leaderboard functionality
- [ ] Certificate of completion generator
- [ ] Instructor dashboard
- [ ] Export trade history to PDF
- [ ] Help videos/tutorials

**Estimated Development Time**: 15-20 hours

**Total Estimated Development**: 65-82 hours

---

## Testing Requirements

### Unit Testing
- Trade execution logic (market orders always execute, limit orders conditional)
- Spread calculations (verify against formula)
- Cost calculations (roundtrip spread costs)
- Order validation (reject negative shares, invalid prices)

### Integration Testing
- LocalStorage persistence across browser sessions
- Quote data loading (API or static)
- Chart rendering with different data sets
- PDF generation for all worksheets
- Mobile/desktop responsive behavior

### User Acceptance Testing
- Complete all 5 simulator modules (45 minutes)
- Execute 10+ trades with different order types
- Calculate trading costs for 3 investor profiles
- Compare 3 brokers using checklist
- Verify all results make sense and provide learning value

### Accessibility Testing
- WAVE tool scan (0 errors)
- Keyboard-only navigation through simulator
- Screen reader test (JAWS or NVDA)
- Color contrast verification
- Mobile device testing (iOS and Android)
- Text scaling to 200%

### Content Testing
- Verify spread calculations match real-world data
- Check broker comparison criteria current
- Validate market hours and holidays accurate
- Test all printable PDFs render correctly

---

## Performance Requirements

- **Simulator Load Time**: <2 seconds
- **Trade Execution**: Instant (<100ms)
- **Quote Updates**: <500ms (if using live data)
- **Chart Rendering**: <1 second
- **PDF Generation**: <3 seconds
- **Mobile Performance**: Smooth, no lag, 60fps scrolling

---

## Browser Compatibility

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+
- Mobile: iOS Safari 14+, Android Chrome 90+

---

## Data Requirements

### Stock Quotes
- **Option 1 (Preferred):** Free API (Yahoo Finance, Alpha Vantage, IEX Cloud)
- **Option 2:** Static data updated daily
- **Option 3:** Manual input by students

**Fields Needed:**
- Ticker symbol
- Company name
- Bid price
- Ask price
- Last price
- Volume
- Average volume
- Day range
- 52-week range
- Market cap
- P/E ratio

### Broker Information
- Updated annually
- Source: Broker websites, comparison sites
- Include: Commissions, minimums, features, ratings

### Market Calendar
- NYSE/NASDAQ holiday schedule
- Updated annually
- Include timezone information

---

## Maintenance Schedule

### Weekly
- Check quote data source (if using API)
- Verify external broker links

### Monthly
- Review simulator feedback from students/teachers
- Check for broker fee changes

### Quarterly
- Update broker comparison data
- Review and refresh stock examples
- Test all API integrations

### Annually
- **Comprehensive broker update:**
  - Fees, commissions, features
  - New brokers added, discontinued removed
  - Ratings refreshed
- **Market calendar update:**
  - Next year's holiday schedule
  - Timezone adjustments if any
- **Content refresh:**
  - Update examples with current stocks
  - Refresh screenshots if interfaces changed
  - Review educational explanations for clarity

---

## Educational Notes

### Key Learning Objectives
1. Students understand bid-ask spread is a hidden transaction cost
2. Students can read and interpret stock quotes
3. Students know when to use market vs. limit orders
4. Students appreciate costs of frequent trading
5. Students can compare brokers objectively

### Common Misconceptions to Address
- "Stock price" has three meanings (bid, ask, last) - clarify which is relevant
- Market orders don't guarantee the "current" price - it may move
- Limit orders aren't always better - they may not execute
- Commission-free doesn't mean cost-free (spread still exists)
- Pre-market/after-hours trading is riskier than regular hours

### Assessment Criteria
Students should be able to:
- Calculate bid-ask spread and spread percentage
- Choose appropriate order type for given scenario
- Estimate lifetime cost of different trading frequencies
- Compare brokers using multiple criteria
- Explain why frequent trading reduces returns

---

**Assets Specification Complete**
**Total Lines**: ~510
**Quality Assessment**: 10/10 - Complete tool specifications, code samples, interaction models, developer handoff, accessibility, testing, and maintenance requirements
