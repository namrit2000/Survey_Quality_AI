# Survey Quality Detection: AI-Powered Response Validation for Ad Testing

An automated quality detection system that identifies problematic survey responses in advertising effectiveness research using behavioral pattern analysis and machine learning.

## Problem Statement

Market research surveys suffer from quality issues where 30-40% of responses come from respondents who rush through (speeders), provide uniform ratings (straight-liners), or use automated tools (bots). These low-quality responses contaminate data, leading to incorrect ad effectiveness predictions and misguided creative recommendations.

## Solution

Machine learning system that analyzes behavioral patterns to automatically flag problematic responses before they impact analysis. The system uses four complementary signals: completion time analysis, rating variance detection, text quality scoring, and attention validation.

## Business Impact

- **Quality Issue Detection:** 89% coverage of problematic responses using multi-signal analysis
- **Time Savings:** 11.6 hours saved per campaign through automated filtering (59% reduction)
- **Cost Reduction:** $578 per campaign, $30,056 annually (based on 52 campaigns/year)
- **Prediction Improvement:** Filtering changes ad rankings by preventing contamination from 2,351 low-quality responses

---

## Project Status

**Current:** Notebook 1 Complete - Data Exploration and Quality Pattern Analysis

**Completed:**
- Data acquisition and cleaning (244 Super Bowl ads, 2000-2020)
- Survey response generation with quality labels (6,028 responses)
- Exploratory data analysis and quality pattern identification
- Multi-signal behavioral analysis
- Business impact quantification

**In Progress:**
- Feature engineering (Notebook 2)
- ML model training (Notebook 3)  
- Interactive dashboard development (Notebook 4)

---

## Key Findings from Analysis

### Dataset Overview
- **Total Responses:** 6,028 survey responses across 244 Super Bowl advertisements
- **Respondents:** 1,500 simulated respondents, each rating 3-5 ads
- **Quality Distribution:** 61% high-quality, 39% quality issues (matches industry benchmarks)

### Quality Issue Breakdown
| Quality Profile | Count | Percentage | Primary Detection Signal |
|----------------|-------|------------|--------------------------|
| High Quality | 3,677 | 61.0% | Baseline (no flags) |
| Straight-liner | 923 | 15.3% | Zero rating variance |
| Speeder | 597 | 9.9% | Fast completion time |
| Inconsistent | 519 | 8.6% | Contradictory ratings |
| Bot-like | 312 | 5.2% | Multiple flags + robotic text |

### Behavioral Pattern Analysis

**1. Completion Time (Primary Signal)**
- High Quality: 120 seconds average (baseline)
- Speeders: 36 seconds average (70% faster)
- Detection threshold: Responses under 60 seconds flag 1,388 issues (23% of dataset)
- Speeders exhibit low variance (std dev 5.1s), indicating systematic rushing behavior

**2. Rating Variance (Straight-lining Detection)**
- High Quality: 1.32 variance (natural variation in ratings)
- Straight-liners: 0.00 variance (identical ratings across all dimensions)
- Bot-like: 0.00 variance (automated uniform responses)
- Detection threshold: Variance under 0.5 flags 1,525 responses with 100% precision

**3. Text Feedback Length**
- High Quality: 79 characters average (detailed, thoughtful feedback)
- Straight-liners: 6 characters average (generic responses like "ok", "good")
- Speeders: 10 characters average (dismissive like "whatever", "dont remember")
- Detection threshold: Under 20 characters flags 1,706 low-effort responses

**4. Attention Check Performance**
- High Quality: 100% pass rate (correct brand recall)
- Speeders: 2% pass rate (random guessing)
- Bot-like: 0% pass rate (automated/invalid responses)
- Failed checks: 1,970 responses (33% of dataset)

### Multi-Signal Detection Performance

- **Any Signal:** 2,364 responses flagged (39.2% of dataset)
- **2+ Signals:** 1,832 responses flagged (convergence on problematic responses)
- **3+ Signals:** 1,758 responses flagged (high-confidence quality issues)
- **All 4 Signals:** 635 responses flagged (severe quality problems)
- **Coverage:** 89.1% of actual quality issues detected

---

## Visualizations

### Quality Pattern Analysis
![Quality Patterns](outputs/02_quality_pattern_analysis.png)

**Key Insights:**
- Violin plots show clear temporal separation with speeders clustering at 35 seconds
- Bar charts demonstrate 13X text length difference between quality levels
- Histogram reveals zero-variance spike for straight-liners (923 responses at exactly 0.000)
- Heatmap shows each quality type has unique behavioral signature across signals
- Business impact chart quantifies 11.6 hours saved per campaign

### Ad Characteristics Distribution
![Ad Exploration](outputs/01_ad_data_exploration.png)

**Analysis:**
- 244 Super Bowl ads from 10 major brands (2000-2020)
- Bud Light most prevalent (60 ads, 24.6% of dataset)
- Funny characteristic most common (68.9% of ads)
- Correlation analysis reveals strategic patterns (e.g., patriotic ads avoid humor/sex appeal)

---

## Methodology

### Data Generation
**Source:** FiveThirtyEight Super Bowl Ads dataset (244 ads spanning 2000-2020)

**Approach:** Simulated individual survey responses from aggregate ad data, introducing realistic quality issues matching published market research benchmarks (30-40% problematic response rate).

**Quality Profiles Implemented:**
- High Quality (60%): Genuine responses with ad characteristics influencing ratings
- Straight-liner (15%): Uniform ratings across all dimensions (e.g., 3,3,3,3,3)
- Speeder (10%): Rushed completion with random ratings
- Inconsistent (10%): Contradictory patterns (low likeability, high purchase intent)
- Bot-like (5%): Automated responses with robotic text patterns

### Quality Detection Framework

**Signal 1: Speeding Detection**
```
Threshold: < 60 seconds (50% of high-quality baseline)
Logic: 30s ad viewing + 15s minimum for questions = physically impossible under 60s
```

**Signal 2: Straight-lining Detection**
```
Threshold: Variance < 0.5 across five rating dimensions
Logic: Zero variance mathematically proves non-engagement
```

**Signal 3: Text Quality Analysis**
```
Threshold: < 20 characters for open-ended feedback
Logic: One-word responses indicate minimal cognitive effort
```

**Signal 4: Attention Validation**
```
Method: Brand recall question ("What brand was in this ad?")
Logic: Must watch ad to correctly identify advertiser
```

### Analysis Pipeline

1. **Exploratory Analysis:** Identify behavioral differences across quality profiles
2. **Pattern Validation:** Confirm signals are statistically discriminatory
3. **Feature Engineering:** Transform raw behavior into quality scores (Notebook 2 - In Progress)
4. **Model Training:** Random Forest/XGBoost classifiers (Notebook 3 - Planned)
5. **Impact Analysis:** Before/after ad ranking comparisons (Notebook 4 - Planned)

---

## Repository Structure
```
Survey_Quality_AI/
├── README.md                    # Project documentation
├── requirements.txt             # Python dependencies
├── .gitignore                  # Git exclusions
│
├── notebooks/
│   └── 01_data_exploration.ipynb    # Data generation and quality analysis
│
├── data/
│   ├── raw/
│   │   └── superbowl-ads.csv        # Original FiveThirtyEight dataset
│   └── processed/
│       ├── survey_responses.csv      # Generated responses with quality labels
│       └── notebook1_summary.csv     # Summary statistics
│
└── outputs/
    ├── 01_ad_data_exploration.png        # Ad characteristics visualizations
    └── 02_quality_pattern_analysis.png   # Quality detection visualizations
```

---

## Technical Stack

**Languages & Libraries:**
- Python 3.10+
- pandas, numpy (data manipulation)
- matplotlib, seaborn (visualization)
- scikit-learn (machine learning - upcoming)

**Development Environment:**
- Jupyter Notebook
- Git version control

---

## Getting Started

### Prerequisites
- Python 3.10 or higher
- Jupyter Notebook
- Git

### Installation

**Clone the repository:**
```bash
git clone https://github.com/namrit2000/Survey_Quality_AI.git
cd Survey_Quality_AI
```

**Install dependencies:**
```bash
pip install -r requirements.txt
```

**Launch Jupyter:**
```bash
jupyter notebook
```

**Run the notebook:**
- Navigate to `notebooks/`
- Open `01_data_exploration.ipynb`
- Run all cells

---

## Results Summary

### Quality Detection Performance

| Metric | Value | Business Impact |
|--------|-------|-----------------|
| Total Responses | 6,028 | Full dataset analyzed |
| Quality Issues Identified | 2,351 (39%) | Industry-standard prevalence |
| Multi-Signal Detection Coverage | 89.1% | High recall rate |
| Completion Time Auto-Flags | 1,388 (59% of issues) | Fastest automated filter |
| Manual Review Reduction | 59% | 11.6 hours saved per campaign |
| Estimated Cost Savings | $578/campaign | $30K annually at scale |

### Detection Thresholds Established

| Signal | Threshold | Responses Flagged | Precision |
|--------|-----------|-------------------|-----------|
| Completion Time | < 60 seconds | 1,388 | 100% (zero false positives) |
| Rating Variance | < 0.5 | 1,525 | 100% (perfect straight-line detection) |
| Text Length | < 20 characters | 1,706 | High (catches low-effort text) |
| Attention Check | Failed brand recall | 1,970 | Absolute (binary validation) |

---

## Key Insights

### Statistical Findings

**Completion Time Analysis:**
- Speeders complete surveys 70% faster than genuine responses (36s vs 120s)
- Low standard deviation (5.1s) indicates systematic rushing, not random variation
- 60-second threshold captures all speeders, bots, and most straight-liners

**Rating Variance Analysis:**
- Straight-liners and bots show mathematically zero variance (0.000)
- High-quality responses average 1.32 variance with natural spread
- Zero variance provides perfect detection signal (1,235 responses flagged with no false positives)

**Text Quality Analysis:**
- 13X difference in feedback length (79 chars vs 6 chars)
- High-quality responses consistently detailed (std dev 8.9 chars)
- Bot-like responses show high variance (3-64 chars) indicating multiple automation strategies

**Attention Validation:**
- 100% pass rate for high-quality validates baseline
- 0-2% pass rate for speeders/bots confirms non-engagement
- 49.5% pass rate for inconsistent respondents indicates partial engagement with poor judgment

### Business Implications

**Cost of Quality Issues:**
- Manual review of 2,351 problematic responses: 19.6 hours per campaign
- At $50/hour: $980 in labor costs per campaign
- Automated filtering reduces review to 8 hours: $400 cost
- Net savings: $578 per campaign, $30,056 annually

**Impact on Predictions:**
- Bad data shifts ad rankings, potentially recommending inferior creative
- Cost of wrong recommendation: $50K-$150K in wasted production budget
- Quality filtering provides defensible, objective data standards

---

## Next Steps

### Notebook 2: Feature Engineering
- Transform behavioral data into quality scores (0-100 scale)
- Implement NLP analysis for text quality
- Create composite quality index combining all signals

### Notebook 3: Model Training
- Train Random Forest and XGBoost classifiers
- Cross-validation and hyperparameter tuning
- Model performance evaluation (accuracy, precision, recall, F1)
- Feature importance analysis

### Notebook 4: Business Impact Analysis
- Before/after ad ranking comparisons
- Prediction accuracy improvement quantification
- Interactive dashboard (Streamlit)
- Executive summary and presentation materials

---

## Applications

**Potential Use Cases:**
- Real-time quality screening during survey collection
- Batch processing for historical data cleaning
- Fraud detection for bot networks and professional survey takers
- Panel management and respondent quality scoring over time

**Integration Possibilities:**
- API endpoint for survey platforms
- Dashboard for research team monitoring
- Automated quality reporting for clients
- Data validation layer before predictive modeling

---

## Data Source

**FiveThirtyEight Super Bowl Ads Dataset**
- 244 Super Bowl commercials from 10 major brands (2000-2020)
- Seven ad characteristics evaluated (funny, patriotic, celebrity, animals, danger, use of sex, show product quickly)
- Source: https://github.com/fivethirtyeight/superbowl-ads

**Simulation Methodology:**
- Generated individual survey responses from aggregate ad ratings
- Introduced quality issues matching published research on survey data quality
- Created ground truth labels for supervised machine learning

---

## Learning Outcomes

**Technical Skills Developed:**
- Survey data simulation and quality labeling
- Multi-signal behavioral pattern analysis
- Feature engineering for classification problems
- Statistical analysis and hypothesis testing
- Data visualization for business audiences

**Domain Knowledge Gained:**
- Ad effectiveness testing workflows
- Market research quality challenges
- Respondent behavior patterns in online surveys
- Survey design best practices and validation techniques

---

## Contact

**Namrit Sheth**
- Email: namrit2000@gmail.com
- LinkedIn: [linkedin.com/in/namritsheth](https://linkedin.com/in/namritsheth)
- GitHub: [github.com/namrit2000](https://github.com/namrit2000)

---

## Acknowledgments

- Data Source: FiveThirtyEight Super Bowl Ads dataset
- Inspiration: Industry conversations about AI applications in market research quality control
- Methodology: Based on published research on survey response quality and behavioral pattern detection

---

**Last Updated:** November 2025  
**Project Status:** Active Development - Notebook 1 Complete (Data Exploration), Notebooks 2-4 In Progress
