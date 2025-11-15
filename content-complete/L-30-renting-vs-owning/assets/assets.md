# Asset Specifications for L-30: Renting vs. Owning a Home

**Scope:** 5 interactive/printable HTML assets for a 55-minute class
**Pattern:** Matches L-1 through L-45 Oklahoma scope (3-5 files per chapter)

## Asset List (5 files total)

### 1. Rent vs. Buy Calculator (PRIMARY INTERACTIVE TOOL)
**File:** `Rent_vs_Buy_Calculator.html`
**Purpose:** Interactive comparison tool for renting vs. buying financial analysis
**Use:** Chapter interactive activity (15-20 minutes)
**Type:** Interactive web application

**Features:**
- Rental scenario inputs:
  - Monthly rent (default: `${{STATE_MEDIAN_RENT}}`)
  - Annual rent increase (default: 3%)
  - Renter's insurance
  - Security deposit
- Purchase scenario inputs:
  - Home price (default: `${{STATE_MEDIAN_HOME_PRICE}}`)
  - Down payment % (5%, 10%, 20%)
  - Mortgage rate (current market rate)
  - Property tax rate (`{{STATE_PROPERTY_TAX_AVG}}`)
  - Homeowner's insurance
  - HOA fees (if applicable)
  - Maintenance costs (1-2% of home value)
- Time horizon selector (5, 10, 15 years)
- Output calculations:
  - Side-by-side monthly cost comparison
  - 5/10/15-year total cost
  - Equity building vs. rent payments
  - Break-even point calculator
- Visual charts showing cumulative costs over time
- Printable comparison report

---

### 2. Housing Affordability Worksheet
**File:** `Housing_Affordability_Worksheet.html`
**Purpose:** Calculate how much housing you can afford based on income
**Use:** Chapter worksheet (10-12 minutes)
**Type:** Interactive + printable worksheet

**Sections:**
- Gross monthly income input
- Debt obligations (student loans, car payment, credit cards)
- 28/36 Rule calculations:
  - Max housing expense (28% of gross income)
  - Max total debt (36% of gross income)
- For renting:
  - Maximum affordable rent
  - With utilities estimate
- For buying:
  - Maximum mortgage payment
  - Maximum home price (based on down payment %, rate, taxes)
- State-specific context for `{{STATE_NAME}}`:
  - Median rent: `${{STATE_MEDIAN_RENT}}`
  - Median home price: `${{STATE_MEDIAN_HOME_PRICE}}`
  - "Your target vs. market reality"

---

### 3. Total Cost of Homeownership Breakdown
**File:** `Total_Cost_Homeownership_Breakdown.html`
**Purpose:** Comprehensive list of all costs associated with homeownership
**Use:** Chapter worksheet (8-10 minutes)
**Type:** Interactive + printable reference

**Categories:**
- Upfront Costs:
  - Down payment
  - Closing costs (2-5% of home price)
  - Inspection and appraisal
  - Moving expenses
- Monthly Costs:
  - Mortgage principal + interest
  - Property taxes (`{{STATE_PROPERTY_TAX_AVG}}`)
  - Homeowner's insurance
  - HOA fees (if applicable)
  - Utilities (higher than renting)
- Ongoing Costs:
  - Maintenance and repairs (1-2% of home value annually)
  - Landscaping/yard care
  - Appliance replacement fund
- State-specific considerations for `{{STATE_NAME}}`:
  - Property tax calculation example
  - Typical closing costs
  - Common HOA fee ranges
- Interactive calculator: Enter home price, see all costs estimated

---

### 4. Rent vs. Own Decision Matrix
**File:** `Rent_vs_Own_Decision_Matrix.html`
**Purpose:** Non-financial factors to consider in housing decision
**Use:** Chapter reflection activity (8-10 minutes)
**Type:** Printable worksheet

**Factors to Rate (1-5 scale):**
- Flexibility/Mobility (Do you plan to move soon?)
- Job stability (How secure is your income?)
- Lifestyle preferences (DIY projects vs. call landlord?)
- Market conditions (`{{STATE_NAME}}` housing market: rising/falling?)
- Family plans (Expanding household size?)
- Maintenance willingness (Time and ability to handle repairs?)
- Long-term roots (Plan to stay 5+ years?)
- Financial readiness (Emergency fund, stable income, credit score?)

**Scoring:**
- Weighted scoring system
- "Rent" vs. "Buy" recommendation based on answers
- Explanation of why each factor matters
- "My situation" summary with action steps

---

### 5. State Housing Market Reference Sheet
**File:** `State_Housing_Market_Reference_Sheet.html`
**Purpose:** Quick reference for `{{STATE_NAME}}` housing market data
**Use:** Reference material
**Type:** Printable 1-page reference

**Content for `{{STATE_NAME}}`:**
- Median home price: `${{STATE_MEDIAN_HOME_PRICE}}`
- Median rent: `${{STATE_MEDIAN_RENT}}`/month
- Price-to-rent ratio (comparison metric)
- Property tax rate: `{{STATE_PROPERTY_TAX_AVG}}`
- Typical closing costs in state
- First-time homebuyer programs available
- Average days on market
- Housing market trend: ⬆️ Rising / ➡️ Stable / ⬇️ Declining
- Rent control policies (if applicable)
- Links to:
  - `{{STATE_NAME}}` Housing Finance Agency
  - Local realtor associations
  - Rental market data sources
- Comparison to national averages

---

## State Variables Used Across All Assets

- `STATE_NAME`
- `STATE_CODE`
- `STATE_MEDIAN_HOME_PRICE`
- `STATE_MEDIAN_RENT`
- `STATE_PROPERTY_TAX_AVG`
- `STATE_CLOSING_COSTS_AVG`
- `STATE_HOUSING_MARKET_TREND` (rising/stable/declining)
- `STATE_FIRST_TIME_BUYER_PROGRAMS`
- `STATE_HOUSING_FINANCE_AGENCY_URL`
- `STATE_RENT_CONTROL_INFO` (if applicable)

## 55-Minute Class Flow

**Chapter Resource Usage Suggestions:**
- 0-5 min: Distribute State Housing Market Reference Sheet
- 5-25 min: Direct instruction on renting vs. owning pros/cons, costs, market factors
- 25-45 min: Rent vs. Buy Calculator (interactive activity - pairs)
- 45-55 min: Debrief and wrap-up

**Additional Activities:**
- 0-10 min: Review and setup
- 10-25 min: Housing Affordability Worksheet (individual)
- 25-38 min: Total Cost of Homeownership review
- 38-50 min: Rent vs. Own Decision Matrix (reflection)
- 50-55 min: Discussion and wrap-up

## Implementation Notes

- All HTML files must be self-contained (no external CDN dependencies)
- Print-friendly CSS required
- File sizes under 500KB
- Mobile-responsive design
- Meets WCAG 2.1 AA accessibility standards
- PFL Academy color scheme (indigo #6366f1, purple #8b5cf6)
