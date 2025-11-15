# Asset Specifications for L-6: Understanding Federal and State Taxes

**Scope:** 4 interactive/printable HTML assets for a 55-minute class
**Pattern:** Matches L-1 through L-45 Oklahoma scope (3-5 files per chapter)

## Asset List (4 files total)

### 1. Tax Impact Calculator (PRIMARY INTERACTIVE TOOL)
**File:** `Tax_Impact_Calculator.html`
**Purpose:** Interactive calculator comparing take-home pay across different states and tax scenarios
**Use:** Chapter interactive activity (15-20 minutes)
**Type:** Interactive web application

**Features:**
- Income input field (annual salary)
- State selector dropdown (focus on `{{STATE_NAME}}` plus 3-4 comparison states)
- Filing status (Single, Married Filing Jointly)
- Real-time calculations showing:
  - Federal income tax (progressive brackets)
  - State income tax: `{{STATE_INCOME_TAX_RATE}}`
  - FICA (Social Security + Medicare)
  - Total tax burden
  - Net (take-home) income
- Side-by-side comparison (up to 3 states)
- Visual chart showing tax breakdown
- Printable comparison report

---

### 2. State Tax Comparison Worksheet
**File:** `State_Tax_Comparison_Worksheet.html`
**Purpose:** Guided research and comparison of tax structures across states
**Use:** Chapter worksheet (10-12 minutes)
**Type:** Interactive + printable worksheet

**Sections:**
- State #1: `{{STATE_NAME}}` (pre-populated data)
  - Income tax structure
  - Sales tax rate
  - Property tax (avg)
  - Other notable taxes
- State #2: __________ (student selects and researches)
- State #3: __________ (student selects and researches)
- Comparison summary table
- Analysis questions:
  - Which state has lowest total tax burden?
  - How do tax structures differ (progressive vs. flat vs. none)?
  - What factors besides taxes should influence location decisions?

---

### 3. Tax Bracket Visualization Tool
**File:** `Tax_Bracket_Visualization.html`
**Purpose:** Interactive visual showing how progressive tax brackets work
**Use:** Chapter worksheet (8-10 minutes)
**Type:** Interactive + printable reference

**Features:**
- Current federal tax brackets displayed visually
- Income slider to show which brackets apply
- "Marginal vs. Effective Rate" calculator
- Visual representation:
  - Income filled into bracket "buckets"
  - Each bucket shows tax owed for that portion
  - Total effective rate calculated
- Common misconception addressed: "Moving to higher bracket doesn't reduce total income"
- Printable bracket reference chart

---

### 4. State Tax Reference Sheet
**File:** `State_Tax_Reference_Sheet.html`
**Purpose:** Quick reference for `{{STATE_NAME}}` complete tax structure
**Use:** Reference material
**Type:** Printable 1-page reference

**Content for `{{STATE_NAME}}`:**
- Income tax: `{{STATE_INCOME_TAX_RATE}}`% or `{{STATE_INCOME_TAX_BRACKETS}}`
- Sales tax: `{{STATE_SALES_TAX}}`%
- Property tax: Average rate `{{STATE_PROPERTY_TAX_AVG}}`
- Other state taxes:
  - Vehicle registration: `{{STATE_VEHICLE_TAX}}`
  - Estate/inheritance tax: `{{STATE_ESTATE_TAX}}`
  - Gas tax: `${{STATE_GAS_TAX}}`/gallon
- Federal tax brackets (simplified reference)
- Total tax burden comparison (state rank nationally)
- Links to `{{STATE_NAME}}` Department of Revenue
- Neighboring states quick comparison

---

## State Variables Used Across All Assets

- `STATE_NAME`
- `STATE_CODE`
- `STATE_INCOME_TAX_RATE` or `STATE_INCOME_TAX_BRACKETS`
- `STATE_SALES_TAX`
- `STATE_PROPERTY_TAX_AVG`
- `STATE_VEHICLE_TAX`
- `STATE_ESTATE_TAX`
- `STATE_GAS_TAX`
- `STATE_REVENUE_DEPT_URL`
- `STATE_NATIONAL_TAX_RANK` (where state ranks in tax burden)

## 55-Minute Class Flow

**Chapter Resource Usage Suggestions:**
- 0-5 min: Distribute State Tax Reference Sheet
- 5-25 min: Direct instruction on federal vs. state taxes, progressive vs. regressive systems
- 25-45 min: Tax Impact Calculator (interactive activity - pairs)
- 45-55 min: Debrief and wrap-up

**Additional Activities:**
- 0-10 min: Review and setup
- 10-25 min: State Tax Comparison Worksheet (individual)
- 25-45 min: Tax Bracket Visualization (whole class + reflection)
- 45-55 min: Discussion and wrap-up

## Implementation Notes

- All HTML files must be self-contained (no external CDN dependencies)
- Print-friendly CSS required
- File sizes under 500KB
- Mobile-responsive design
- Meets WCAG 2.1 AA accessibility standards
- PFL Academy color scheme (indigo #6366f1, purple #8b5cf6)