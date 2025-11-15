# Asset Specifications for L-3: Income and Taxes

**Scope:** 4 interactive/printable HTML assets for a 55-minute class
**Pattern:** Matches L-1 through L-45 Oklahoma scope (3-5 files per chapter)

## Asset List (4 files total)

### 1. Paycheck Analyzer Calculator (PRIMARY INTERACTIVE TOOL)
**File:** `Paycheck_Analyzer_Calculator.html`
**Purpose:** Interactive tool to understand gross vs. net income with state-specific deductions
**Use:** Chapter interactive activity (15-20 minutes)
**Type:** Interactive web application

**Features:**
- Profession selector (5-6 common entry-level jobs) or custom salary input
- State-specific tax calculations:
  - Federal income tax (basic bracket calculation)
  - State income tax: `{{STATE_INCOME_TAX_RATE}}`%
  - FICA (Social Security 6.2%, Medicare 1.45%)
  - Optional: State SDI/disability where applicable
- Adjustable W-4 withholding (0-3 allowances)
- Visual breakdown showing:
  - Gross income bar
  - Each deduction as segment
  - Net (take-home) highlighted
- Side-by-side comparison (2 scenarios)
- Printable results

---

### 2. W-4 Withholding Simulator
**File:** `W4_Withholding_Simulator.html`
**Purpose:** Practice completing W-4 form and seeing impact on take-home pay
**Use:** Chapter worksheet (10-12 minutes)
**Type:** Interactive + printable worksheet

**Features:**
- Simplified W-4 form (2020+ version)
- 3 scenarios with different situations:
  - Single job, no dependents
  - Married, spouse works
  - Single with one child
- Real-time calculation showing:
  - Weekly/biweekly take-home pay
  - Estimated annual tax owed
  - Estimated refund or balance due
- State-specific withholding considerations for `{{STATE_NAME}}`
- Explanation of when to adjust W-4

---

### 3. Income Management Worksheet
**File:** `Income_Management_Worksheet.html`
**Purpose:** Comprehensive worksheet for planning net income usage
**Use:** Chapter reflection activity (8-10 minutes)
**Type:** Printable worksheet with calculation fields

**Sections:**
- Gross Income Calculation (hourly × hours or annual salary)
- Deductions Breakdown (using `{{STATE_NAME}}` tax rates)
- Net Income Summary
- 50/30/20 Budget Allocation
- Monthly Savings Goals
- Tax Planning Considerations
- Personal Action Plan

---

### 4. State Tax Reference Sheet
**File:** `State_Tax_Reference_Sheet.html`
**Purpose:** Quick reference for `{{STATE_NAME}}` income tax structure
**Use:** Reference material
**Type:** Printable 1-page reference

**Content for `{{STATE_NAME}}`:**
- State income tax structure: `{{#if STATE_INCOME_TAX_RATE}}{{STATE_INCOME_TAX_RATE}}% flat rate{{else}}{{#if STATE_INCOME_TAX_BRACKETS}}Progressive brackets{{else}}No state income tax{{/if}}{{/if}}`
- Federal tax brackets (simplified)
- FICA breakdown (Social Security + Medicare)
- Standard deduction amounts
- Common pre-tax deductions (401k, HSA, etc.)
- State-specific considerations:
  - Local income taxes (if applicable): `{{STATE_LOCAL_INCOME_TAX}}`
  - State disability insurance: `{{STATE_SDI_RATE}}`
- Quick calculation examples
- Links to `{{STATE_NAME}}` Department of Revenue

---

## State Variables Used Across All Assets

- `STATE_NAME`
- `STATE_CODE`
- `STATE_INCOME_TAX_RATE` (or `STATE_INCOME_TAX_BRACKETS` for progressive states)
- `STATE_LOCAL_INCOME_TAX` (if applicable)
- `STATE_SDI_RATE` (State Disability Insurance, where applicable)
- `STATE_TAX_FILING_PORTAL_URL`
- `STATE_REVENUE_DEPT_URL`

## 55-Minute Class Flow

**Chapter Resource Usage Suggestions:**
- 0-5 min: Distribute State Tax Reference Sheet
- 5-25 min: Direct instruction on gross vs. net income, deductions, W-4 basics
- 25-45 min: Paycheck Analyzer Calculator (interactive activity - pairs)
- 45-55 min: Debrief and wrap-up

**Additional Activities:**
- 0-10 min: Review and setup
- 10-25 min: W-4 Withholding Simulator (individual)
- 25-40 min: Income Management Worksheet (reflection)
- 40-55 min: Discussion and wrap-up

## Implementation Notes

- All HTML files must be self-contained (no external CDN dependencies)
- Print-friendly CSS required
- File sizes under 500KB
- Mobile-responsive design
- Meets WCAG 2.1 AA accessibility standards
- PFL Academy color scheme (indigo #6366f1, purple #8b5cf6)