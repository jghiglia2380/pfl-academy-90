# State Data Layer - Integration Guide for Development Team

## Overview

The state data layer provides personalized, state-specific content for all L-chapters (learning chapters) in the PFL Academy curriculum. This lightweight system uses static JSON files - no database required.

## How It Works

1. **Platform identifies student's state** (from school district, user profile, or IP geolocation)
2. **Load corresponding state JSON file** (`states/texas.json`, `states/california.json`, etc.)
3. **Parse JSON into variable map** (key-value pairs)
4. **When rendering L-chapter markdown:**
   - Find `{{VARIABLE_NAME}}` patterns
   - Replace with value from state JSON
   - If variable not found, log error and show placeholder or generic value

### Example

**Template** (L-chapter markdown):
```markdown
In {{STATE_NAME}}, the state sales tax rate is {{STATE_SALES_TAX}}%,
but local jurisdictions can add up to {{STATE_LOCAL_SALES_TAX_MAX}}%
in additional sales tax.
```

**Data** (`states/texas.json`):
```json
{
  "state_name": "Texas",
  "state_code": "TX",
  "taxes": {
    "sales_tax": 6.25,
    "local_sales_tax_max": 2.0
  }
}
```

**Output**:
```
In Texas, the state sales tax rate is 6.25%, but local jurisdictions
can add up to 2.0% in additional sales tax.
```

## What's Included

### ✅ Complete Documentation
- `README.md` - Comprehensive guide to the data layer
- `schema.json` - JSON schema defining all fields and validation rules
- `STATE_DATA_PROGRESS.md` - Project status and completion tracking
- `validation/README.md` - Validation system usage guide
- `INTEGRATION_GUIDE.md` - This file (integration instructions)

### ✅ Data Files (4 Priority States - 100% Complete)
- `states/texas.json` - All 86+ variables populated
- `states/california.json` - All variables with CA-specific data
- `states/virginia.json` - All variables with VA-specific data
- `states/florida.json` - All variables with FL-specific data

### ✅ Validation System
- `validation/validate.js` - Automated validation script
- `validation/validation-results.json` - Latest test results (100% pass rate)

## Integration Steps

### Step 1: Load State Data

```javascript
// Example: Load state data file
const fs = require('fs');
const path = require('path');

function loadStateData(stateCode) {
  const filePath = path.join(__dirname, 'state-data', 'states', `${stateCode.toLowerCase()}.json`);

  if (!fs.existsSync(filePath)) {
    console.error(`State data not found for: ${stateCode}`);
    return null;
  }

  return JSON.parse(fs.readFileSync(filePath, 'utf8'));
}

// Usage
const texasData = loadStateData('TX');
```

### Step 2: Create Variable Replacement Function

```javascript
function replaceStateVariables(content, stateData) {
  if (!stateData) return content;

  // Replace variables in format {{VARIABLE_NAME}}
  return content.replace(/\{\{([A-Z_0-9]+)\}\}/g, (match, variable) => {
    const value = getNestedValue(stateData, variable);

    if (value === undefined) {
      console.warn(`Variable not found: ${variable} for state ${stateData.state_code}`);
      return match; // Keep original if not found
    }

    return value;
  });
}

// Helper to get nested values from state JSON
function getNestedValue(obj, variable) {
  // Convert STATE_SALES_TAX to obj.taxes.sales_tax
  const path = variableToPath(variable);
  return path.reduce((current, key) => current?.[key], obj);
}

function variableToPath(variable) {
  // Map STATE_SALES_TAX -> ['taxes', 'sales_tax']
  // Map STATE_NAME -> ['state_name']
  // etc.

  const mapping = {
    'STATE_NAME': ['state_name'],
    'STATE_CODE': ['state_code'],
    'STATE_SALES_TAX': ['taxes', 'sales_tax'],
    'STATE_LOCAL_SALES_TAX_MAX': ['taxes', 'local_sales_tax_max'],
    'STATE_MIN_WAGE': ['employment', 'min_wage'],
    'STATE_MEDIAN_HOME_PRICE': ['housing', 'median_home_price'],
    // ... add all 86 variables
  };

  return mapping[variable] || [];
}
```

### Step 3: Render Content

```javascript
// Example: Render L-chapter with state data
async function renderChapter(chapterId, studentState) {
  // 1. Load chapter content (from DB or markdown file)
  const chapterContent = await loadChapterContent(chapterId);

  // 2. Load state data
  const stateData = loadStateData(studentState);

  // 3. Replace variables
  const personalizedContent = replaceStateVariables(chapterContent, stateData);

  // 4. Render to HTML (or return markdown)
  return markdownToHtml(personalizedContent);
}

// Usage
const html = await renderChapter('L-46', 'TX');
```

## Variable Categories

### All 86+ Variables Organized by Category:

1. **Automotive** (16 variables)
   - Registration fees, insurance rates, loan rates, gas prices
   - Example: `{{STATE_REGISTRATION_ANNUAL}}`, `{{STATE_GAS_PRICE_CURRENT}}`

2. **Taxes** (42 variables)
   - Sales tax, property tax, income tax, assessment processes
   - Example: `{{STATE_SALES_TAX}}`, `{{STATE_PROPERTY_TAX_COUNTY_RATE}}`

3. **Housing** (5 variables)
   - Home prices, rent, market trends, assistance programs
   - Example: `{{STATE_MEDIAN_HOME_PRICE}}`, `{{STATE_MEDIAN_RENT}}`

4. **Employment** (3 variables)
   - Minimum wage, unemployment rate, major industries
   - Example: `{{STATE_MIN_WAGE}}`, `{{STATE_UNEMPLOYMENT_RATE}}`

5. **Education** (1 variable)
   - Public university tuition
   - Example: `{{STATE_TUITION_PUBLIC}}`

6. **Banking** (4 variables)
   - Bank fees, minor account rules, credit unions
   - Example: `{{STATE_AVG_MONTHLY_FEE}}`, `{{STATE_AVG_OVERDRAFT_FEE}}`

7. **Consumer Protection** (2 variables)
   - Agency name and contact URL
   - Example: `{{STATE_CONSUMER_PROTECTION_AGENCY}}`

8. **Local Examples** (15+ variables)
   - Cities, businesses, nonprofits for realistic examples
   - Example: `{{STATE_MAJOR_CITY_1}}`, `{{STATE_LOCAL_CREDIT_UNION_NAME}}`

**Full variable list**: See `schema.json` for complete definitions

## Data Quality Standards

### ✅ All Priority States Include:
- **Real data from official sources** (no estimates or placeholders)
- **URLs to data sources** for verification
- **Local examples** (actual city names, businesses, organizations)
- **Last updated date** (November 2025)
- **State-specific notes** (economic context, unique policies)

### ✅ Validation Ensures:
- All required fields present
- Data types correct (numbers are numbers, not strings)
- Values in reasonable ranges
- No placeholder text
- Data is fresh (< 12 months old)

## Missing Variables - Graceful Handling

Since only 4 states are complete, implement graceful degradation:

```javascript
function replaceStateVariables(content, stateData) {
  return content.replace(/\{\{([A-Z_0-9]+)\}\}/g, (match, variable) => {
    const value = getNestedValue(stateData, variable);

    if (value === undefined) {
      // Strategy 1: Use generic text
      return getGenericText(variable);

      // OR Strategy 2: Use national average
      return getNationalAverage(variable);

      // OR Strategy 3: Keep variable visible
      return `[${variable.replace(/_/g, ' ')}]`;
    }

    return value;
  });
}
```

## File Sizes & Performance

- **Schema**: ~8 KB
- **Average state file**: 15-20 KB
- **Total for 50 states**: ~1 MB (extremely lightweight)
- **Load time**: < 10ms per state file
- **Memory**: < 20 KB per state in RAM

**Recommendation**: Load state data once per user session and cache in memory.

## Testing with Available States

Currently available for testing:
- ✅ Texas (TX)
- ✅ California (CA)
- ✅ Virginia (VA)
- ✅ Florida (FL)

**Test Coverage**: These 4 states represent:
- Different tax structures (income tax vs. no income tax)
- Different cost of living (low, moderate, high, very high)
- Different property tax systems (caps vs. no caps)
- Geographic diversity (South, West, East)

## Remaining States

**Status**: 46 states remaining (0% complete)

**Estimated Timeline**:
- Per state: 2-4 hours (research + data entry)
- Total: ~138 hours
- Can be parallelized with multiple team members

**Recommendation**: Test integration with 4 existing states first, then build remaining states in batches of 5-10.

## Annual Update Process

**Target Date**: September 1st each year (before fall semester)

### Critical Updates (must do annually):
1. Public university tuition
2. Minimum wage (changes frequently)
3. Housing prices (median home price, median rent)
4. Unemployment rate
5. Gas prices

### Data Sources:
- **Automotive**: State DMV websites
- **Taxes**: State revenue/comptroller offices
- **Housing**: U.S. Census, Zillow, state housing authorities
- **Employment**: Bureau of Labor Statistics (state-level data)
- **Education**: State higher education commissions

## Error Handling

### Recommended Error Handling Strategy:

```javascript
try {
  const stateData = loadStateData(studentState);

  if (!stateData) {
    // Fallback to generic national content
    logger.warn(`State data not found for ${studentState}, using generic content`);
    return renderGenericContent(chapterId);
  }

  const content = replaceStateVariables(chapterContent, stateData);
  return content;

} catch (error) {
  logger.error(`Error loading state data for ${studentState}:`, error);
  // Fallback to generic content
  return renderGenericContent(chapterId);
}
```

### Logging Recommendations:

```javascript
// Log missing variables for content improvement
function replaceStateVariables(content, stateData) {
  const missingVars = [];

  const result = content.replace(/\{\{([A-Z_0-9]+)\}\}/g, (match, variable) => {
    const value = getNestedValue(stateData, variable);

    if (value === undefined) {
      missingVars.push(variable);
      return getGenericText(variable);
    }

    return value;
  });

  if (missingVars.length > 0) {
    logger.info(`Missing variables for ${stateData.state_code}:`, missingVars);
  }

  return result;
}
```

## Validation Before Deployment

Always validate state files before deploying:

```bash
# Validate all states
cd state-data/validation
node validate.js

# Validate specific state
node validate.js texas

# Deep validation with URL checks
node validate.js --check-urls
```

**Pass criteria**: 100% pass rate, 0 errors

## Next Steps for Integration Team

### Phase 1: Basic Integration (1-2 days)
1. Implement `loadStateData()` function
2. Create `replaceStateVariables()` function with variable mapping
3. Test with Texas data (most complete example)
4. Verify output matches expectations

### Phase 2: Production Integration (3-5 days)
1. Integrate with user profile system (determine student state)
2. Add caching for state data (per user session)
3. Implement graceful fallbacks for missing states/variables
4. Add logging for missing variables
5. Test with all 4 available states

### Phase 3: Validation & Monitoring (2-3 days)
1. Add validation checks to CI/CD pipeline
2. Set up monitoring for missing state data
3. Create dashboard for missing variable tracking
4. Document state data update procedures

### Phase 4: Scale to All States (ongoing)
1. Build remaining 46 state files
2. Validate each batch
3. Deploy incrementally
4. Monitor for issues

## Questions?

**Technical Questions**: Review `README.md` and `schema.json`
**Validation Questions**: See `validation/README.md`
**Data Source Questions**: Check `data_sources` object in each state file

## Summary

✅ **Foundation Complete**: Schema, docs, 4 priority states, validation system
✅ **Tested**: All 4 states pass validation (100% success rate)
✅ **Ready for**: Integration testing with template engine
⏳ **Remaining**: 46 state files (build after successful integration test)

**Total File Size**: ~1 MB for all 50 states
**Deployment**: Static files, no database required
**Performance**: < 10ms load time per state

---

**Last Updated**: 2025-11-14
**Prepared By**: PFL Academy Development Team
**Contact**: Development team for integration questions
