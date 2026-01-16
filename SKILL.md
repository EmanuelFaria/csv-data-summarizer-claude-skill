---
name: csv-data-summarizer
description: >
  Analyzes CSV files, generates summary stats, and plots quick visualizations using Python and pandas. Use when user says "summarize csv", "analyze dataset", "data profile", "csv statistics", "visualize data", or uploads a CSV for analysis. ALSO triggers when Claude thinks "I should analyze this data first", "Need to understand the data structure", "Generating data summary", or "Profiling this dataset". Hook detection: Read tool on .csv files when analysis requested, user uploads CSV attachment. Automatically generates comprehensive summaries without asking what to do.
metadata:
  version: 2.1.0
  dependencies: python>=3.8, pandas>=2.0.0, matplotlib>=3.7.0, seaborn>=0.12.0
---

# CSV Data Summarizer

*Produces: Comprehensive CSV analysis with statistics, visualizations, and actionable insights automatically*
*Companion to: csv-protocol, csv-demo-generator, csv-diff-html*

---

## When to Use

- **User uploads or references** a CSV file
- **Data exploration** requests (summarize, analyze, visualize)
- **Data quality assessment** needs (missing values, distributions)
- **Quick insights** from tabular data
- **Before deeper analysis** to understand data structure
- **Sales/operational/survey data** requiring statistical summaries

## When NOT to Use

| Situation | Use Instead |
|-----------|-------------|
| CSV structural validation (61-column, ROW_ID format) | csv-protocol skill |
| Generating interactive HTML demos from CSV | csv-demo-generator skill |
| Comparing two CSV versions for changes | csv-diff-html skill |
| Behavioral content analysis (identity arc, psychology) | AI-council with CSV expertise |
| Creating/editing CSV content | Direct Edit tool with csv-protocol |
| Security validation (XSS, template literals) | csv-protocol skill |

---

## Prerequisites

| Resource | Location | Purpose |
|----------|----------|---------|
| **Python 3.8+** | `/opt/homebrew/bin/python3` | Analysis execution |
| **pandas 2.0+** | Python package | Data manipulation |
| **matplotlib 3.7+** | Python package | Visualization generation |
| **seaborn 0.12+** | Python package | Statistical plotting |
| **analyze.py** | `./analyze.py` | Core analysis script |
| **requirements.txt** | `./requirements.txt` | Dependency specification |
| **Sample data** | `./resources/sample.csv` | Testing reference |

---

## How It Works

## ⚠️ CRITICAL BEHAVIOR REQUIREMENT ⚠️

**DO NOT ASK THE USER WHAT THEY WANT TO DO WITH THE DATA.**
**DO NOT OFFER OPTIONS OR CHOICES.**
**DO NOT SAY "What would you like me to help you with?"**
**DO NOT LIST POSSIBLE ANALYSES.**

**IMMEDIATELY AND AUTOMATICALLY:**
1. Run the comprehensive analysis
2. Generate ALL relevant visualizations
3. Present complete results
4. NO questions, NO options, NO waiting for user input

**THE USER WANTS A FULL ANALYSIS RIGHT AWAY - JUST DO IT.**

### Automatic Analysis Steps:

**The skill intelligently adapts to different data types and industries by inspecting the data first, then determining what analyses are most relevant.**

1. **Load and inspect** the CSV file into pandas DataFrame
2. **Identify data structure** - column types, date columns, numeric columns, categories
3. **Determine relevant analyses** based on what's actually in the data:
   - **Sales/E-commerce data** (order dates, revenue, products): Time-series trends, revenue analysis, product performance
   - **Customer data** (demographics, segments, regions): Distribution analysis, segmentation, geographic patterns
   - **Financial data** (transactions, amounts, dates): Trend analysis, statistical summaries, correlations
   - **Operational data** (timestamps, metrics, status): Time-series, performance metrics, distributions
   - **Survey data** (categorical responses, ratings): Frequency analysis, cross-tabulations, distributions
   - **Generic tabular data**: Adapts based on column types found

4. **Only create visualizations that make sense** for the specific dataset:
   - Time-series plots ONLY if date/timestamp columns exist
   - Correlation heatmaps ONLY if multiple numeric columns exist
   - Category distributions ONLY if categorical columns exist
   - Histograms for numeric distributions when relevant
   
5. **Generate comprehensive output** automatically including:
   - Data overview (rows, columns, types)
   - Key statistics and metrics relevant to the data type
   - Missing data analysis
   - Multiple relevant visualizations (only those that apply)
   - Actionable insights based on patterns found in THIS specific dataset
   
6. **Present everything** in one complete analysis - no follow-up questions

**Example adaptations:**
- Healthcare data with patient IDs → Focus on demographics, treatment patterns, temporal trends
- Inventory data with stock levels → Focus on quantity distributions, reorder patterns, SKU analysis  
- Web analytics with timestamps → Focus on traffic patterns, conversion metrics, time-of-day analysis
- Survey responses → Focus on response distributions, demographic breakdowns, sentiment patterns

### Behavior Guidelines

✅ **CORRECT APPROACH - SAY THIS:**
- "I'll analyze this data comprehensively right now."
- "Here's the complete analysis with visualizations:"
- "I've identified this as [type] data and generated relevant insights:"
- Then IMMEDIATELY show the full analysis

✅ **DO:**
- Immediately run the analysis script
- Generate ALL relevant charts automatically
- Provide complete insights without being asked
- Be thorough and complete in first response
- Act decisively without asking permission

❌ **NEVER SAY THESE PHRASES:**
- "What would you like to do with this data?"
- "What would you like me to help you with?"
- "Here are some common options:"
- "Let me know what you'd like help with"
- "I can create a comprehensive analysis if you'd like!"
- Any sentence ending with "?" asking for user direction
- Any list of options or choices
- Any conditional "I can do X if you want"

❌ **FORBIDDEN BEHAVIORS:**
- Asking what the user wants
- Listing options for the user to choose from
- Waiting for user direction before analyzing
- Providing partial analysis that requires follow-up
- Describing what you COULD do instead of DOING it

### Usage

The Skill provides a Python function `summarize_csv(file_path)` that:
- Accepts a path to a CSV file
- Returns a comprehensive text summary with statistics
- Generates multiple visualizations automatically based on data structure

### Example Prompts

> "Here's `sales_data.csv`. Can you summarize this file?"

> "Analyze this customer data CSV and show me trends."

> "What insights can you find in `orders.csv`?"

### Example Output

**Dataset Overview**
- 5,000 rows × 8 columns  
- 3 numeric columns, 1 date column  

**Summary Statistics**
- Average order value: $58.2  
- Standard deviation: $12.4
- Missing values: 2% (100 cells)

**Insights**
- Sales show upward trend over time
- Peak activity in Q4
*(Attached: trend plot)*

---

## Anti-Patterns

| Don't | Do Instead |
|-------|------------|
| Ask "What would you like to do with this data?" | IMMEDIATELY run comprehensive analysis |
| List options for user to choose from | Generate ALL relevant visualizations automatically |
| Wait for user direction before analyzing | Act decisively - analyze first, show results |
| Provide partial analysis requiring follow-up | Complete analysis in one response |
| Create visualizations that don't match data type | Adapt analysis to actual data structure |
| Generate time-series plots without date columns | Check for date columns first |
| Force generic analysis on specific domains | Recognize sales/healthcare/survey patterns |
| Say "I can analyze this if you'd like" | Just do it - "Here's the complete analysis:" |
| Describe what you COULD do | DO IT and show what you DID |
| Ask permission to be thorough | Be thorough by default |
| Generate visualizations manually | Use analyze.py script |
| Skip dependency check | Verify pandas/matplotlib installed |

---

## Reference Files

| File | Purpose | Location |
|------|---------|----------|
| **SKILL.md** | This file - skill definition | `~/.claude/skills/csv-data-summarizer/SKILL.md` |
| **analyze.py** | Core analysis script | `~/.claude/skills/csv-data-summarizer/analyze.py` |
| **requirements.txt** | Python dependencies | `~/.claude/skills/csv-data-summarizer/requirements.txt` |
| **Sample CSV** | Test dataset | `~/.claude/skills/csv-data-summarizer/resources/sample.csv` |
| **Additional docs** | Extended documentation | `~/.claude/skills/csv-data-summarizer/resources/README.md` |
| **Examples** | Usage examples | `~/.claude/skills/csv-data-summarizer/examples/` |

---

## Notes

- Automatically detects date columns (columns containing 'date' in name)
- Handles missing data gracefully
- Generates visualizations only when date columns are present
- All numeric columns are included in statistical summary
- Adapts analysis strategy based on data structure inspection

---

## CLI Tools

### analyze.py — Core Analysis Script

Run comprehensive CSV analysis with statistics and visualizations.

```bash
# Basic analysis
python3 ~/.claude/skills-lazy-loaded/csv-data-summarizer/analyze.py /path/to/data.csv

# Analysis with output directory for visualizations
python3 ~/.claude/skills-lazy-loaded/csv-data-summarizer/analyze.py /path/to/data.csv --output /tmp/analysis

# Quick summary (no visualizations)
python3 ~/.claude/skills-lazy-loaded/csv-data-summarizer/analyze.py /path/to/data.csv --quick
```

---

## Critical Rules

1. **Immediately run analysis** — NEVER ask user what they want to do with data
2. **Generate ALL relevant visualizations** — Automatic based on data structure
3. **Adapt to data type** — Recognize sales/healthcare/survey patterns automatically
4. **Complete in one response** — No partial analysis requiring follow-up
5. **Check date columns first** — Only generate time-series if date columns exist
6. **Verify dependencies** — Ensure pandas/matplotlib installed before running
7. **Act decisively** — Be thorough by default, no permission needed

---

## Common Mistakes

1. **Asking what user wants to do** — FORBIDDEN; immediately run comprehensive analysis
2. **Listing options for user** — FORBIDDEN; generate ALL relevant visualizations
3. **Waiting for user direction** — Act decisively, analyze first
4. **Partial analysis** — Always complete in one response
5. **Generic time-series without date columns** — Check for date columns first
6. **Forcing generic analysis** — Adapt to sales/healthcare/survey patterns
7. **Saying "I can analyze if you'd like"** — Just do it; show what you DID
8. **Skipping dependency check** — Verify pandas/matplotlib before running

---

## Files in This Skill

| File | Purpose |
|------|---------|
| `SKILL.md` | This documentation |
| `analyze.py` | Core analysis script |
| `requirements.txt` | Python dependencies |
| `resources/sample.csv` | Test dataset |
| `resources/README.md` | Extended documentation |
| `examples/` | Usage examples |

---

*Last Updated: 2026-01-15*

