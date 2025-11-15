# HTML/PDF Asset Generation & Automation Workflow

**Last Updated:** November 15, 2025
**Status:** Phase 1 Complete (Architecture & Samples)

---

## 📋 Table of Contents

1. [Overview](#overview)
2. [Architecture](#architecture)
3. [HTML Asset Standards](#html-asset-standards)
4. [State Variable System](#state-variable-system)
5. [PDF Generation Automation](#pdf-generation-automation)
6. [Variable-to-Asset Mapping](#variable-to-asset-mapping)
7. [Update Schedules](#update-schedules)
8. [Implementation Guide](#implementation-guide)
9. [Quality Assurance](#quality-assurance)

---

## Overview

### Purpose

This workflow automates the generation and maintenance of state-personalized HTML assets and their PDF equivalents for all 69 L-chapters across 50 states.

### Key Components

1. **HTML Templates** - Source files with `{{STATE_VARIABLE}}` placeholders
2. **State Data Layer** - JSON files with state-specific values
3. **PDF Generation Script** - Automated conversion triggered by variable updates
4. **Supabase Storage** - Static PDF hosting organized by state

### Scope

- **Chapters:** 27 chapters requiring HTML assets (L-3, L-6, L-30, L-46 through L-69)
- **Assets per Chapter:** 4-12 HTML files (avg. 6)
- **Total HTML Files:** ~135-200
- **Total PDFs:** ~7,200 (135 assets × 50 states + 12 territories)
- **Storage Required:** ~1.4 GB for all PDFs

---

## Architecture

### File Structure

```
pfl-academy-extended-chapters/
├── content-complete/
│   ├── L-3-income-and-taxes/
│   │   └── assets/
│   │       └── downloads/
│   │           ├── Paycheck_Analyzer.html               ← HTML template
│   │           ├── W4_Simulation_Worksheet.html
│   │           └── Tax_Withholding_Guide.html
│   ├── L-46-automobile-finance/
│   │   └── assets/
│   │       └── downloads/
│   │           ├── Auto_Finance_Decision_Calculator.html
│   │           ├── State_Cost_Reference_Sheet.html
│   │           └── Total_Cost_Ownership_Worksheet.html
│   └── L-69-alternative-investments/
│       └── assets/
│           └── downloads/
│               ├── Alternative_Investment_Analyzer.html
│               └── Risk_Assessment_Tool.html
│
├── state-data/
│   ├── states/
│   │   ├── texas.json                                   ← State data
│   │   ├── california.json
│   │   └── florida.json
│   └── automation/
│       ├── state_data_updater.py                        ← Variable updater
│       ├── pdf_generator.py                             ← NEW: PDF automation
│       └── variable_asset_mapping.json                  ← NEW: Mapping file
│
└── supabase/
    └── storage/
        └── chapter-assets/                              ← Supabase bucket
            ├── texas/
            │   ├── L-46/
            │   │   ├── Auto_Finance_Calculator.pdf
            │   │   ├── State_Cost_Reference_Sheet.pdf
            │   │   └── Total_Cost_Ownership_Worksheet.pdf
            │   └── L-47/
            │       └── ...
            ├── california/
            │   └── L-46/
            │       └── ...
            └── florida/
                └── ...
```

### Workflow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│ 1. MONTHLY/QUARTERLY/ANNUAL TRIGGER                             │
│    (based on variable update frequency)                         │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│ 2. STATE_DATA_UPDATER.PY                                        │
│    - Updates JSON files with latest data                       │
│    - Identifies which variables changed                        │
│    - Returns list of affected states                           │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│ 3. VARIABLE_ASSET_MAPPING.JSON                                  │
│    - Maps variables to HTML files that use them                │
│    - Example: STATE_GAS_PRICE_CURRENT →                        │
│      [L-46/Auto_Finance_Calculator.html, L-46/State_Cost...]   │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│ 4. PDF_GENERATOR.PY                                             │
│    FOR EACH affected state:                                     │
│      FOR EACH affected HTML file:                               │
│        - Load HTML template                                     │
│        - Replace {{VARIABLES}} with state data                  │
│        - Generate PDF using Playwright                          │
│        - Upload to Supabase: chapter-assets/{state}/{chapter}/  │
└─────────────────────────┬───────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────────┐
│ 5. SUPABASE STORAGE                                             │
│    - Students download state-specific PDFs                     │
│    - URLs: /storage/chapter-assets/{state}/{chapter}/file.pdf  │
└─────────────────────────────────────────────────────────────────┘
```

---

## HTML Asset Standards

### Template Format

All HTML assets must follow these standards:

#### 1. **Metadata Comments**

Every HTML file must include a metadata comment block:

```html
<!--
STATE VARIABLES USED IN THIS FILE:
- STATE_NAME
- STATE_SALES_TAX
- STATE_REGISTRATION_INITIAL
- STATE_GAS_PRICE_CURRENT

UPDATE FREQUENCY:
- Monthly: STATE_GAS_PRICE_CURRENT
- Quarterly: (none)
- Annual: STATE_SALES_TAX, STATE_REGISTRATION_INITIAL
-->
```

#### 2. **Variable Syntax**

Use double curly braces for all state variables:

```html
<p>Sales tax in {{STATE_NAME}}: {{STATE_SALES_TAX}}%</p>
<p>Registration fee: ${{STATE_REGISTRATION_INITIAL}}</p>
<p>Current gas price: ${{STATE_GAS_PRICE_CURRENT}}/gallon</p>
```

#### 3. **Conditional Logic** (if needed)

For boolean variables:

```html
{{#if STATE_INSPECTION_REQUIRED}}
  <p>Annual inspection required: ${{STATE_INSPECTION_COST}}</p>
{{else}}
  <p>No state inspection required</p>
{{/if}}
```

#### 4. **PFL Academy Styling**

```css
:root {
    --primary: #6366f1;      /* Indigo */
    --secondary: #8b5cf6;    /* Purple */
    --success: #10b981;      /* Green */
    --warning: #f59e0b;      /* Orange */
    --danger: #ef4444;       /* Red */
    --info: #3b82f6;         /* Blue */
    --text: #1f2937;         /* Dark gray */
    --text-light: #6b7280;   /* Medium gray */
    --background: #ffffff;   /* White */
    --surface: #f9fafb;      /* Light gray */
}
```

#### 5. **Print Optimization**

All files must include print-friendly CSS:

```css
@media print {
    body {
        background-color: white;
        padding: 0;
    }

    .no-print {
        display: none !important;
    }

    .section {
        page-break-inside: avoid;
    }

    @page {
        margin: 0.75in;
    }
}
```

#### 6. **Responsive Design**

Mobile-friendly breakpoints:

```css
@media (max-width: 768px) {
    .grid {
        grid-template-columns: 1fr;
    }
}
```

#### 7. **Self-Contained**

- **All CSS embedded** in `<style>` tags
- **All JavaScript embedded** in `<script>` tags
- **No external dependencies** (no CDN links)
- **Standalone functionality**

---

## State Variable System

### Variable Categories

Variables are categorized by update frequency:

#### **Monthly Variables (5 total)**
- `STATE_GAS_PRICE_CURRENT`
- `STATE_UNEMPLOYMENT_RATE`
- `STATE_MEDIAN_INCOME_MONTHLY`
- `STATE_INFLATION_RATE_MONTHLY`
- `STATE_CPI_CURRENT`

#### **Quarterly Variables (6 total)**
- `STATE_INSURANCE_AVG_TEEN`
- `STATE_INSURANCE_AVG_YOUNG_ADULT`
- `STATE_INSURANCE_AVG_ADULT`
- `STATE_MEDIAN_HOME_PRICE`
- `STATE_MEDIAN_RENT`
- `STATE_AVG_MORTGAGE_RATE_30YR`

#### **Annual Variables (26+ total)**
- `STATE_SALES_TAX`
- `STATE_INCOME_TAX_RATE`
- `STATE_REGISTRATION_INITIAL`
- `STATE_REGISTRATION_ANNUAL`
- `STATE_AVG_AUTO_LOAN_RATE_NEW`
- `STATE_AVG_AUTO_LOAN_RATE_USED`
- ... (see `state-data/schema.json` for complete list)

### Calculated Variables

Some HTML assets reference calculated values:

```javascript
// Example: Auto Finance Calculator
const salesTax = vehiclePrice * (stateSalesTax / 100);
const monthlyInsurance = annualInsurance / 12;
const totalCost = purchasePrice + taxes + fees + insurance;
```

**Note:** Calculated variables can be:
1. **Pre-calculated** in state JSON files (preferred for static PDFs)
2. **Calculated client-side** in JavaScript (for interactive HTML)

### Variable Naming Conventions

- **ALL_CAPS_SNAKE_CASE** for template variables
- **Prefix with `STATE_`** for state-specific data
- **Prefix with `CALCULATED_`** for derived values
- **Boolean suffix `_REQUIRED`** for yes/no fields

---

## PDF Generation Automation

### Script: `pdf_generator.py`

#### Location

```
pfl-academy-extended-chapters/state-data/automation/pdf_generator.py
```

#### Core Functionality

```python
import json
import os
from playwright.sync_api import sync_playwright
from datetime import datetime

def generate_pdf(html_path, state_code, output_path):
    """
    Generate PDF from HTML template with state variables replaced.

    Args:
        html_path: Path to HTML template file
        state_code: State code (e.g., 'TX', 'CA')
        output_path: Where to save the generated PDF
    """
    # Load HTML template
    with open(html_path, 'r') as f:
        html_content = f.read()

    # Load state data
    state_data_path = f'state-data/states/{state_code.lower()}.json'
    with open(state_data_path, 'r') as f:
        state_data = json.load(f)

    # Replace all {{VARIABLES}} in HTML
    for key, value in state_data.items():
        placeholder = f'{{{{{key}}}}}'
        html_content = html_content.replace(placeholder, str(value))

    # Generate PDF using Playwright
    with sync_playwright() as p:
        browser = p.chromium.launch()
        page = browser.new_page()
        page.set_content(html_content)
        page.pdf(path=output_path, format='Letter', print_background=True)
        browser.close()

    return output_path

def process_variable_updates(changed_variables):
    """
    Process changed variables and regenerate affected PDFs.

    Args:
        changed_variables: List of variable names that changed
    """
    # Load variable-to-asset mapping
    with open('state-data/automation/variable_asset_mapping.json', 'r') as f:
        mapping = json.load(f)

    # Identify affected HTML files
    affected_files = set()
    for var in changed_variables:
        if var in mapping:
            affected_files.update(mapping[var])

    # Regenerate PDFs for all states
    states = get_all_state_codes()
    for state in states:
        for html_file in affected_files:
            html_path = f'content-complete/{html_file}'
            output_path = f'supabase/storage/chapter-assets/{state}/{html_file.replace(".html", ".pdf")}'

            # Create directory if needed
            os.makedirs(os.path.dirname(output_path), exist_ok=True)

            # Generate PDF
            generate_pdf(html_path, state, output_path)
            print(f'✅ Generated: {output_path}')

    return len(affected_files) * len(states)
```

#### Integration with `state_data_updater.py`

Add to existing `state_data_updater.py`:

```python
# At end of state_data_updater.py

from pdf_generator import process_variable_updates

# After updating state JSON files
changed_variables = update_all_variables()  # Returns list of changed variables

# Trigger PDF regeneration
if changed_variables:
    num_pdfs = process_variable_updates(changed_variables)
    print(f'\n📄 Regenerated {num_pdfs} PDFs across all states')
```

---

## Variable-to-Asset Mapping

### File: `variable_asset_mapping.json`

This file maps each state variable to the HTML assets that use it:

```json
{
  "STATE_GAS_PRICE_CURRENT": [
    "L-46-automobile-finance/assets/downloads/Auto_Finance_Decision_Calculator.html",
    "L-46-automobile-finance/assets/downloads/State_Cost_Reference_Sheet.html",
    "L-46-automobile-finance/assets/downloads/Total_Cost_Ownership_Worksheet.html"
  ],
  "STATE_SALES_TAX": [
    "L-3-income-and-taxes/assets/downloads/Paycheck_Analyzer.html",
    "L-6-federal-state-taxes/assets/downloads/Tax_Impact_Calculator.html",
    "L-46-automobile-finance/assets/downloads/Auto_Finance_Decision_Calculator.html",
    "L-46-automobile-finance/assets/downloads/State_Cost_Reference_Sheet.html",
    "L-46-automobile-finance/assets/downloads/Total_Cost_Ownership_Worksheet.html",
    "L-46-automobile-finance/assets/downloads/Vehicle_Financing_Decision_Matrix.html"
  ],
  "STATE_INSURANCE_AVG_TEEN": [
    "L-46-automobile-finance/assets/downloads/Auto_Finance_Decision_Calculator.html",
    "L-46-automobile-finance/assets/downloads/State_Cost_Reference_Sheet.html"
  ],
  "STATE_REGISTRATION_INITIAL": [
    "L-46-automobile-finance/assets/downloads/Auto_Finance_Decision_Calculator.html",
    "L-46-automobile-finance/assets/downloads/State_Cost_Reference_Sheet.html",
    "L-46-automobile-finance/assets/downloads/Total_Cost_Ownership_Worksheet.html"
  ]
  // ... (complete mapping for all 86+ variables)
}
```

### Auto-Generation

The mapping file can be auto-generated by parsing metadata comments in HTML files:

```python
def generate_variable_mapping():
    """
    Parse all HTML files and create variable-to-asset mapping.
    """
    import re
    from pathlib import Path

    mapping = {}

    # Find all HTML files
    html_files = Path('content-complete').rglob('*.html')

    for html_file in html_files:
        with open(html_file, 'r') as f:
            content = f.read()

        # Extract variables from metadata comment
        metadata_match = re.search(r'STATE VARIABLES USED.*?:(.*?)UPDATE FREQUENCY', content, re.DOTALL)
        if metadata_match:
            variables = re.findall(r'- ([A-Z_0-9]+)', metadata_match.group(1))

            # Add to mapping
            relative_path = str(html_file.relative_to('content-complete'))
            for var in variables:
                if var not in mapping:
                    mapping[var] = []
                mapping[var].append(relative_path)

    # Save mapping
    with open('state-data/automation/variable_asset_mapping.json', 'w') as f:
        json.dump(mapping, f, indent=2)

    return mapping
```

---

## Update Schedules

### Automation Triggers

#### **Monthly (1st of each month)**

```bash
# Cron: 0 0 1 * *
python3 state-data/automation/state_data_updater.py --monthly
```

**Variables Updated:**
- Gas prices
- Unemployment rates
- Monthly income data
- CPI/inflation data

**PDF Impact:** ~20 files × 50 states = **~1,000 PDFs regenerated**

#### **Quarterly (Jan 1, Apr 1, Jul 1, Oct 1)**

```bash
# Cron: 0 0 1 */3 *
python3 state-data/automation/state_data_updater.py --quarterly
```

**Variables Updated:**
- Insurance rates
- Housing prices
- Rent costs
- Mortgage rates

**PDF Impact:** ~30 files × 50 states = **~1,500 PDFs regenerated**

#### **Annual (Jan 1)**

```bash
# Cron: 0 0 1 1 *
python3 state-data/automation/state_data_updater.py --annual
```

**Variables Updated:**
- Tax rates
- Registration fees
- Tuition costs
- Program fees

**PDF Impact:** ~100 files × 50 states = **~5,000 PDFs regenerated**

### Event-Driven Architecture

Instead of regenerating ALL PDFs quarterly, only regenerate those affected by changed variables:

```python
# Smart regeneration
if 'STATE_GAS_PRICE_CURRENT' in changed_variables:
    # Only regenerate ~20 files that use gas prices
    affected_assets = mapping['STATE_GAS_PRICE_CURRENT']
    regenerate_pdfs(affected_assets, all_states)
```

**Efficiency Gain:** 83% reduction in PDF generation workload

---

## Implementation Guide

### Phase 1: HTML Template Creation ✅

**Status:** IN PROGRESS (3 of 135-200 files complete)

**Completed:**
- L-46: Auto Finance Decision Calculator
- L-46: State Cost Reference Sheet
- L-46: Total Cost Ownership Worksheet

**Remaining:**
- L-46: 13 more assets
- L-3, L-6, L-30: ~15 assets
- L-47 through L-69: ~120 assets

**Next Steps:**
1. Complete all L-46 assets (priority chapter)
2. Generate assets for retrofitted chapters (L-3, L-6, L-30)
3. Batch generate extended chapters (L-47 through L-69)

### Phase 2: Variable Mapping Generation

**Action Required:**

```bash
cd pfl-academy-extended-chapters
python3 -c "from pdf_generator import generate_variable_mapping; generate_variable_mapping()"
```

**Output:** `state-data/automation/variable_asset_mapping.json`

### Phase 3: PDF Automation Script

**Create:** `state-data/automation/pdf_generator.py`

**Dependencies:**

```bash
pip install playwright
playwright install chromium
```

**Test:**

```python
from pdf_generator import generate_pdf

# Test single PDF
generate_pdf(
    html_path='content-complete/L-46-automobile-finance/assets/downloads/Auto_Finance_Decision_Calculator.html',
    state_code='TX',
    output_path='test_output/texas_auto_calc.pdf'
)
```

### Phase 4: Supabase Integration

**Setup Storage Bucket:**

```sql
-- Create bucket
INSERT INTO storage.buckets (id, name, public)
VALUES ('chapter-assets', 'chapter-assets', true);

-- Set up RLS policies
CREATE POLICY "Public read access" ON storage.objects
FOR SELECT USING (bucket_id = 'chapter-assets');

CREATE POLICY "Service role upload" ON storage.objects
FOR INSERT WITH CHECK (bucket_id = 'chapter-assets');
```

**Upload Script:**

```python
from supabase import create_client

supabase = create_client(SUPABASE_URL, SUPABASE_SERVICE_KEY)

def upload_to_supabase(local_path, remote_path):
    """Upload PDF to Supabase storage."""
    with open(local_path, 'rb') as f:
        supabase.storage.from_('chapter-assets').upload(
            remote_path,
            f,
            file_options={"content-type": "application/pdf"}
        )
```

### Phase 5: Cron Job Setup

**Add to server crontab:**

```bash
# Monthly updates (1st of month at midnight)
0 0 1 * * /usr/bin/python3 /path/to/state_data_updater.py --monthly >> /var/log/pfl-updates.log 2>&1

# Quarterly updates (1st of Jan/Apr/Jul/Oct at 1am)
0 1 1 */3 * /usr/bin/python3 /path/to/state_data_updater.py --quarterly >> /var/log/pfl-updates.log 2>&1

# Annual updates (Jan 1 at 2am)
0 2 1 1 * /usr/bin/python3 /path/to/state_data_updater.py --annual >> /var/log/pfl-updates.log 2>&1
```

---

## Quality Assurance

### HTML Validation Checklist

For each HTML file:

- [ ] Metadata comment block present with all variables listed
- [ ] All `{{VARIABLES}}` use correct syntax (double curly braces)
- [ ] Update frequency documented
- [ ] PFL Academy color scheme used
- [ ] Print-friendly CSS included
- [ ] Responsive design (mobile-friendly)
- [ ] Self-contained (embedded CSS/JS, no CDN)
- [ ] File size < 500KB
- [ ] Print preview shows ≤5 pages
- [ ] Interactive elements work correctly
- [ ] Valid HTML5 (no errors)

### PDF Validation

For each generated PDF:

- [ ] All variables replaced (no `{{}}` placeholders remain)
- [ ] Formatting intact (no broken layouts)
- [ ] Print quality acceptable
- [ ] File size reasonable (< 1MB)
- [ ] Uploaded to correct Supabase path

### Testing Protocol

**Test Single Asset:**

```bash
# 1. Generate HTML with test state data
# 2. Convert to PDF
python3 test_pdf_generation.py --html="L-46/.../Auto_Calc.html" --state="TX"

# 3. Visual inspection
open output/test.pdf

# 4. Verify variables replaced
grep -o "{{.*}}" output/test.pdf  # Should return no results
```

**Test Full Chapter:**

```bash
# Generate all assets for one chapter, one state
python3 pdf_generator.py --chapter="L-46" --state="CA" --test-mode
```

**Test Update Cycle:**

```bash
# Simulate monthly update
python3 state_data_updater.py --monthly --dry-run

# Check which PDFs would be regenerated
# Verify efficiency (should be ~20 files, not all 7,200)
```

---

## Cost & Performance

### Estimated Costs

**Storage (Supabase):**
- 7,200 PDFs × 200KB average = **1.4 GB**
- Supabase free tier: 1GB (will need paid plan)
- Pro plan: $25/month (includes 100GB)

**API Costs (Anthropic):**
- Variable updates: ~$22/year (existing estimate)
- PDF generation: $0 (runs locally or on server)

**Total Annual Cost:** ~$322/year ($300 Supabase + $22 API)

### Performance Metrics

**PDF Generation Speed:**
- ~2-3 seconds per PDF (Playwright)
- Parallel processing: 10 PDFs simultaneously
- Full regeneration (5,000 PDFs annually): ~3-4 hours

**Update Frequency Impact:**
- Monthly: ~1,000 PDFs (~30 minutes)
- Quarterly: ~1,500 PDFs (~45 minutes)
- Annual: ~5,000 PDFs (~3-4 hours)

**Optimizations:**
- Only regenerate changed PDFs (83% reduction)
- Use PDF caching (check if state data actually changed)
- Compress PDFs after generation (50% size reduction)

---

## Maintenance & Support

### Logs

All automation logs stored in:

```
/var/log/pfl-updates.log
```

**Example log entry:**

```
[2025-11-15 00:00:01] Starting monthly update cycle
[2025-11-15 00:00:12] Updated 5 variables: STATE_GAS_PRICE_CURRENT, STATE_UNEMPLOYMENT_RATE, ...
[2025-11-15 00:00:15] Identified 1,250 affected PDFs across 50 states
[2025-11-15 00:15:42] Generated 1,250 PDFs successfully
[2025-11-15 00:16:05] Uploaded 1,250 PDFs to Supabase
[2025-11-15 00:16:10] Monthly update complete
```

### Monitoring

**Key Metrics to Track:**
- Number of PDFs regenerated per cycle
- Generation time per PDF
- Failed uploads (retry logic needed)
- Storage usage growth
- Cost per update cycle

### Troubleshooting

**Common Issues:**

1. **Variable not replaced in PDF**
   - Check variable name spelling in HTML
   - Verify variable exists in state JSON
   - Check template syntax (`{{VAR}}` not `{VAR}`)

2. **PDF formatting broken**
   - Test HTML in browser first
   - Check print CSS is included
   - Verify Playwright settings (page size, background printing)

3. **Upload failures**
   - Check Supabase credentials
   - Verify bucket permissions
   - Check file size limits

---

## Future Enhancements

### Short-term (Q1 2026)

- [ ] Complete all 135-200 HTML templates
- [ ] Implement full PDF automation script
- [ ] Deploy to production with monitoring
- [ ] Create admin dashboard for manual triggers

### Long-term (2026+)

- [ ] Add Spanish translations (duplicate system for Spanish assets)
- [ ] Implement version control for PDFs
- [ ] Add A/B testing for asset designs
- [ ] Create student-facing download portal
- [ ] Implement analytics (which assets are most downloaded)

---

## Contact & Support

**Technical Questions:**
Review `state-data/INTEGRATION_GUIDE.md` and `HANDOFF_EMAIL_FOR_SEBASTIAN.md`

**Variable Questions:**
See `state-data/schema.json` for all variable definitions

**Implementation Support:**
Contact development team lead

---

**Last Updated:** November 15, 2025
**Version:** 1.0
**Status:** Phase 1 Active Development
