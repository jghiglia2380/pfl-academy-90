# Asset Specifications for L-46: Automobile Finance - Buy vs. Lease

**Scope:** 4 interactive/printable HTML assets for a 55-minute class
**Pattern:** Matches L-1 through L-45 Oklahoma scope (3-5 files per chapter)

## Asset List (4 files total)

### 1. Auto Finance Decision Calculator (PRIMARY INTERACTIVE TOOL)
**File:** `Auto_Finance_Decision_Calculator.html`
**Purpose:** Interactive comparison tool for buy vs. lease scenarios
**Use:** Chapter interactive activity (15-20 minutes)
**Type:** Interactive web application

**Features:**
- Simple 3-column comparison: Buy New | Buy Used | Lease
- State-specific inputs pre-populated:
  - Sales tax: `{{STATE_SALES_TAX}}`%
  - Registration: `{{STATE_REGISTRATION_INITIAL}}` / `{{STATE_REGISTRATION_ANNUAL}}`
  - Insurance avg: `${{STATE_INSURANCE_AVG_TEEN}}`/month
  - Gas price: `${{STATE_GAS_PRICE_CURRENT}}`/gallon
  - Loan rates: `{{STATE_AVG_AUTO_LOAN_RATE_NEW}}`% / `{{STATE_AVG_AUTO_LOAN_RATE_USED}}`%
- Output: Side-by-side cost comparison with winner highlighted
- Printable results

---

### 2. Total Cost of Ownership Worksheet
**File:** `Total_Cost_Ownership_Worksheet.html`
**Purpose:** Guided worksheet for calculating complete vehicle costs
**Use:** Chapter worksheet (10-12 minutes)
**Type:** Interactive + printable worksheet

**Sections:**
- Purchase & Financing Costs
- State-Specific Fees (auto-populated for `{{STATE_NAME}}`)
- Ongoing Annual Costs (insurance, fuel, maintenance, registration)
- 6-Year Total Calculation
- Cost per mile

---

### 3. Vehicle Financing Decision Matrix
**File:** `Vehicle_Financing_Decision_Matrix.html`
**Purpose:** Personal decision-making framework
**Use:** Chapter reflection activity (8-10 minutes)
**Type:** Printable worksheet

**Sections:**
- Your Priorities (rank 1-5)
- Current Financial Situation
- State-Specific Cost Factors (auto-populated for `{{STATE_NAME}}`)
- Scenario Analysis Summary
- My Decision Framework
- Personal Rules

---

### 4. State Cost Reference Sheet
**File:** `State_Cost_Reference_Sheet.html`
**Purpose:** Quick reference for all state-specific auto costs
**Use:** Reference material
**Type:** Printable 1-page reference

**Content for `{{STATE_NAME}}`:**
- Sales tax on vehicles: `{{STATE_SALES_TAX}}`%
- Registration fees: Initial `${{STATE_REGISTRATION_INITIAL}}`, Annual `${{STATE_REGISTRATION_ANNUAL}}`
- Average insurance costs by age
- Current gas price: `${{STATE_GAS_PRICE_CURRENT}}`/gallon
- Average auto loan rates
- Inspection requirements: `{{#if STATE_INSPECTION_REQUIRED}}Yes - ${{STATE_INSPECTION_COST}}{{else}}No{{/if}}`
- Links to state DMV and resources

---

## State Variables Used Across All Assets

- `STATE_NAME`
- `STATE_CODE`
- `STATE_SALES_TAX`
- `STATE_REGISTRATION_INITIAL`
- `STATE_REGISTRATION_ANNUAL`
- `STATE_AVG_AUTO_LOAN_RATE_NEW`
- `STATE_AVG_AUTO_LOAN_RATE_USED`
- `STATE_INSURANCE_AVG_TEEN`
- `STATE_GAS_PRICE_CURRENT`
- `STATE_INSPECTION_REQUIRED` (boolean)
- `STATE_INSPECTION_COST`
- `STATE_DMV_URL`

## 55-Minute Class Flow

**Chapter Resource Usage Suggestions:**
- 0-5 min: Distribute State Cost Reference Sheet
- 5-25 min: Direct instruction on buy vs. lease concepts
- 25-45 min: Auto Finance Decision Calculator (interactive activity - pairs)
- 45-55 min: Debrief and wrap-up

**Additional Activities:**
- 0-10 min: Review and setup
- 10-27 min: Total Cost Ownership Worksheet (individual)
- 27-45 min: Vehicle Financing Decision Matrix (reflection)
- 45-55 min: Discussion and wrap-up

## Implementation Notes

- All HTML files must be self-contained (no external CDN dependencies)
- Print-friendly CSS required
- File sizes under 500KB
- Mobile-responsive design
- Meets WCAG 2.1 AA accessibility standards
- PFL Academy color scheme (indigo #6366f1, purple #8b5cf6)
