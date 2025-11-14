# PFL Academy State Data Layer

## Overview

This directory contains state-specific data files that personalize L-chapter content for all 50 U.S. states. Sebastian's template engine reads these JSON files and replaces `{{VARIABLE_NAME}}` placeholders in markdown content with state-specific values.

## How It Works

1. **District/School identifies their state** → Platform knows which state file to load
2. **Content loads** → L-chapter markdown templates contain `{{STATE_NAME}}`, `{{STATE_MIN_WAGE}}`, etc.
3. **Template engine replaces variables** → Platform swaps placeholders with values from state JSON
4. **Personalized content displayed** → Students see content specific to their state

## Directory Structure

```
state-data/
├── schema.json              # JSON schema defining all possible fields
├── README.md                # This file
├── states/
│   ├── alabama.json
│   ├── alaska.json
│   ├── arizona.json
│   ├── ...
│   ├── texas.json
│   └── wyoming.json
├── validation/
│   ├── validate.js          # Validation script
│   └── test-results.json    # Latest validation results
└── UPDATE_SCHEDULE.md       # Annual update checklist
```

## State File Format

Each state file follows the schema defined in `schema.json`. Example structure:

```json
{
  "state_name": "Texas",
  "state_code": "TX",
  "last_updated": "2025-11-14",
  "data_sources": {
    "dmv_url": "https://www.txdmv.gov",
    "tax_authority_url": "https://comptroller.texas.gov"
  },
  "automotive": {
    "registration_initial": 50.75,
    "registration_annual": 50.75,
    "avg_auto_loan_rate_new": 6.5,
    "insurance_avg_teen": 245
  },
  "taxes": {
    "sales_tax": 6.25,
    "local_sales_tax_min": 0,
    "local_sales_tax_max": 2.0
  },
  "housing": {
    "median_home_price": 300000,
    "median_rent": 1400
  },
  "employment": {
    "min_wage": 7.25,
    "unemployment_rate": 4.1,
    "major_industries": "Energy, Technology, Agriculture, Healthcare"
  }
}
```

## Variable Categories

### Automotive (L-46: Automobile Finance)
- Registration fees, insurance rates, loan rates
- Inspection requirements and costs
- Current gas prices

### Taxes (L-46, L-49, L-50, L-59)
- Sales tax (state and local)
- Property tax (assessment, rates, exemptions)
- Local income tax (where applicable)
- Tax incentive programs

### Housing (L-50, L-59)
- Median home prices and rent
- Market trends
- First-time homebuyer programs
- Mortgage rates

### Employment (L-49, L-50)
- Minimum wage
- Unemployment rate
- Major industries

### Education (L-49, L-50)
- Public university tuition
- State financial aid programs

### Banking (L-56)
- Average fees
- Minor account rules
- Credit union associations

### Consumer Protection (L-61)
- State consumer protection agency
- Complaint resources

### Local Examples
- Major retailers, telecom providers
- Local credit unions and nonprofits
- Example cities (major, suburban, rural)

## Data Sources

All data should come from official state sources:

- **Automotive**: State DMV/Motor Vehicles website
- **Taxes**: State Department of Revenue / Tax Commission
- **Housing**: U.S. Census Bureau, state housing authority
- **Employment**: Bureau of Labor Statistics (state data), state labor department
- **Education**: State higher education commission, NCES
- **Banking**: FDIC, NCUA, state banking commission
- **Consumer Protection**: State Attorney General, consumer affairs office

## Update Schedule

### Annual Updates (Required)
**Target Date**: September 1st each year (before fall semester)

**Priority Fields** (must be updated):
- `tuition_public` - Public university tuition
- `min_wage` - Minimum wage (changes often)
- `median_home_price` / `median_rent` - Housing data
- `unemployment_rate` - Current unemployment
- `gas_price_current` - Current gas prices
- Tax rates (if changed by legislature)

**Secondary Fields** (verify still accurate):
- Registration fees
- Insurance rates
- Average loan rates
- Local examples (are businesses still operating?)

### Quarterly Updates (Recommended)
- Gas prices (highly volatile)
- Unemployment rates
- Mortgage rates
- Insurance rates

### As-Needed Updates
- When state legislature changes tax rates
- When major industries shift
- When DMV fee structure changes
- When consumer protection agencies reorganize

## Validation

Before deploying state files, run validation:

```bash
cd state-data/validation
node validate.js
```

Validation checks:
- All required fields present
- Data types correct
- Values within reasonable ranges
- URLs accessible
- No placeholder/dummy data
- `last_updated` is recent

## Creating a New State File

1. Copy `states/texas.json` as template
2. Research official state sources (see schema for URLs)
3. Fill in all required fields (marked in schema)
4. Fill in as many optional fields as possible
5. Add data source URLs
6. Set `last_updated` to current date
7. Add state-specific notes if needed
8. Run validation script
9. Test in platform with L-chapter content

## Priority States

Prioritize these states for initial buildout (largest populations / early adopters):

**Tier 1** (Complete first):
- California
- Texas
- Florida
- Virginia

**Tier 2** (High priority):
- New York
- Pennsylvania
- Illinois
- Ohio
- Georgia
- North Carolina
- Michigan

**Tier 3** (Medium priority):
- All remaining states

## Common Issues

### Issue: Variable not found
**Cause**: Variable used in L-chapter but not in state file
**Solution**: Add variable to state JSON; update schema if it's a new variable

### Issue: Invalid data type
**Cause**: String provided where number expected (or vice versa)
**Solution**: Check schema for correct type; update state file

### Issue: Outdated data
**Cause**: State file not updated annually
**Solution**: Follow update schedule; check `last_updated` field

### Issue: Missing local examples
**Cause**: Generic placeholders like "major retailer" left in
**Solution**: Research state-specific businesses and organizations

## Quality Standards

Each state file should be:
- **Accurate**: Data from official sources, verified
- **Current**: Updated within last 12 months
- **Complete**: All required fields + as many optional as possible
- **Specific**: Real local examples, not generic placeholders
- **Sourced**: Data source URLs included for verification

## Questions?

Contact development team or see:
- `/Users/justin/Library/Mobile Documents/com~apple~CloudDocs/Documents/pfl-academy/Data-layer-buildout-instructions.md`
- Sebastian's template engine documentation

---

**Status**: Schema complete, example state files in progress
**Last Updated**: 2025-11-14
