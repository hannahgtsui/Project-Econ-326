# AI Exposure, Human Skills, and Occupational Wages

An ECON 326 project examining AI exposure, skill requirements, and wages across **624 U.S. occupations**. The main analysis uses 2025 data, with exploratory comparisons of wages and employment between 2019 and 2025.

## Why this research matters

Rapid advances in AI raise questions about career opportunities and the skills employers value. These questions are particularly relevant to young people entering the workforce. In Canada, unemployment among people aged 15–24 reached **13.7% in October 2026**, highlighting the challenges facing young job seekers. [Statistics Canada](https://www150.statcan.gc.ca/n1/daily-quotidien/261106/dq261106a-eng.htm)

The International Labour Organization estimates that **one in four workers worldwide** holds an occupation with some exposure to generative AI, although exposure does not necessarily imply job loss. [International Labour Organization](https://www.ilo.org/publications/generative-ai-and-jobs-2025-update)

Our research examines how wages and employment changes relate to AI exposure and selected human skill requirements. The findings provide context for discussions about workforce preparation; they do not establish that AI causes unemployment or predict individual career outcomes.

The ILO estimate provides background context. Our analysis uses general AI Occupational Exposure scores rather than a generative-AI-specific index.

## Research question

**How is AI exposure correlated with occupational wages, and how does this relationship change after accounting for communication and problem-solving requirements?**

We estimate wage regressions controlling for occupational employment and broad occupation groups. We also compare communication and problem-solving requirements and examine their interactions with AI exposure. The study examines correlations rather than causal effects.

### Exploratory subquestions

1. Does the relationship between AI exposure and occupational wages differ between 2019 and 2025?
2. Are employment changes between 2019 and 2025 associated with occupational AI exposure?

## Outline of our project

We first match the datasets, construct variables, and present summary statistics and plots. We then estimate the 2025 wage regressions, comparing specifications before and after adding skills and controls. Further analysis separates communication and problem-solving requirements and examines interactions with AI exposure.

Finally, we investigate the two exploratory subquestions, assess robustness, and discuss limitations. The 2025 wage analysis remains the main focus.

## Data sources

| Source | Data used | Purpose |
|---|---|---|
| [BLS Occupational Employment and Wage Statistics](https://www.bls.gov/oes/tables.htm) | May 2019 and May 2025 national estimates | Annual mean occupational wages, employment, and occupation codes |
| [CPI-U via FRED](https://fred.stlouisfed.org/series/CPIAUCNS) | May 2019 and May 2025 observations from CPIAUCNS | Express wages in 2025 dollars |
| [AI Occupational Exposure index](https://github.com/AIOE-Data/AIOE) | Appendix A of `AIOE_DataAppendix.xlsx` | Occupational AI exposure scores from Felten, Raj, and Seamans (2021) |
| [O*NET Database 27.0](https://www.onetcenter.org/db_releases.html) | Importance ratings in `Skills.xlsx` | Selected cognitive and communication skill requirements |
| [O*NET 2010–2019 crosswalk](https://www.onetcenter.org/taxonomy/2019/walk.html) | Occupation-code mappings | Identify unchanged, one-to-one six-digit occupation codes |

AIOE measures potential exposure of occupational abilities to AI. It does not measure actual adoption, displacement, or specifically ChatGPT use.

## Sample construction

We select detailed national, cross-industry, all-ownership occupations and retain unchanged, one-to-one six-digit occupation-code mappings. We require positive, available wages and employment, an AI exposure score, and complete ratings for all five selected skills.

| Stage | Occupations |
|---|---:|
| Detailed 2025 BLS occupations | 830 |
| After the occupation-code screen | 661 |
| Matched to AI exposure scores | 628 |
| With usable wages, employment, and AI scores | 625 |
| With all five skill ratings | 624 |

Psychologists, All Other (`19-3039`) is excluded at the final stage because its selected skill ratings are unavailable. No values are imputed or replaced with zero.

All 624 retained occupations also have usable 2019 wage and employment estimates. The two-year comparison therefore contains **1,248 occupation-year observations representing 624 occupations**, rather than 1,248 independent occupations. Complete records are not required for the intervening years.

Each observation in the main analysis represents an occupation, not an individual worker. Each occupation receives equal weight.

## Variables

| Variable | Definition and purpose |
|---|---|
| Log annual wage | Natural logarithm of 2025 mean annual occupational wage; the main dependent variable |
| AI exposure | AIOE standardized across the final 624 occupations |
| Human skills | Standardized index combining five selected O*NET Importance ratings |
| Communication | Standardized index combining Speaking and Active Listening |
| Problem-solving | Standardized index combining Critical Thinking, Complex Problem Solving, and Judgment and Decision Making |
| Log employment | Natural logarithm of employment, controlling for occupation size in the wage models |
| Occupation groups | 21 indicators for 22 broad SOC groups, with Management as the reference category |
| Squared scores | Squared exposure and skill indices used to examine nonlinear relationships |
| Skill interactions | AI × Skills, or AI × Communication and AI × Problem-solving in separate specifications |
| 2019 log real wage | Natural logarithm of 2019 annual mean wage expressed in 2025 dollars |
| 2019 log employment | Baseline occupation size for the employment-change model |
| Employment log change | Cumulative log employment change between 2019 and 2025 |
| Year indicator | Equals 0 in 2019 and 1 in 2025 in the pooled wage model |

### Constructing skill measures

We use five O*NET Importance ratings:

- Speaking
- Active Listening
- Critical Thinking
- Complex Problem Solving
- Judgment and Decision Making

Suboccupation ratings are equally averaged within six-digit occupations. Each rating is standardized before averaging, and the combined index is standardized again. The communication and problem-solving indices are constructed from their respective standardized ratings and standardized separately.

The two component indices replace the combined Skills index when entered together. These measures represent selected skills, not all human abilities.

AI exposure and skill indices are held fixed across years using the final sample. They are not separately observed 2019 skill measures.

### Inflation adjustment

May CPI-U equals **256.092 in 2019** and **321.465 in 2025**. We convert 2019 wages into 2025 dollars:

$$
\text{2019 wage in 2025 dollars}
=
\text{2019 nominal wage}\times\frac{321.465}{256.092}
$$

2025 wages remain unchanged. Employment counts do not require inflation adjustment.

### Employment change

The employment extension uses:

$$
\Delta Employment_i
=
100\left[\ln(Employment_{i,2025})-\ln(Employment_{i,2019})\right]
$$

This is cumulative log employment change, not an exact percentage change. Positive values indicate increases and negative values indicate decreases.

## Proposed regression

The main 2025 model is:

$$
\ln(Wage_i)
=
\beta_0+\beta_1AI_i+\beta_2Skills_i
+\beta_3\ln(Employment_i)
+\text{Group controls}+\varepsilon_i
$$

Here, i represents an occupation. Group controls consist of 21 occupation-group indicators, each with its own coefficient, with Management as the reference category.

We plan to estimate ordinary least squares regressions with heteroskedasticity-robust standard errors (HC3), two-sided tests, and 95% confidence intervals.

For a standardized regressor, a one-standard-deviation increase is associated with an approximate wage difference of $100\beta$ percent, holding other variables constant. The exact percentage difference is $100(e^\beta-1)$.

## Exploratory extensions

### Skill differences and interactions

We will replace Skills with Communication and Problem-solving and directly test whether their coefficients differ.

A separate specification will add AI × Skills. Another will include both component indices and their interactions with AI exposure. The corresponding main terms remain included.

These interactions test whether skill–wage associations vary with AI exposure; they do not establish causal complementarity.

### Comparing 2019 and 2025 wages

We will compare log real wages for the same occupations in both years. The pooled model will include a year indicator and interactions allowing AI, skills, employment, and occupation-group coefficients to differ by year.

The AI × Year coefficient directly tests whether the AI–wage association differs between years. Standard errors will be clustered by occupation.

### Employment changes

We will regress employment log changes on AI exposure, Skills, occupation-group indicators, 2019 log real wages, and 2019 log employment.

This extension uses HC3 robust standard errors and examines employment changes rather than AI-caused job losses.

## Model evaluation

We will assess multicollinearity, residual patterns, influential observations, and functional form.

Robustness checks will:

- Compare equally weighted and employment-weighted models.
- Include squared exposure and skill scores.
- Examine sensitivity to extreme observations.
- Use fixed 2019 employment weights in cross-year weighting checks.

All analyses are exploratory because preliminary results informed the research design. We will report uncertain and specification-sensitive findings.

## Summary statistics

The tables report:

- Number of observations
- Mean
- Sample standard deviation
- Minimum
- Maximum

The main summary table includes the 2025 analysis variables and occupation-group indicators.

The extension table currently includes eight variables: three skill interactions, two squared component indices, 2019 log real wages, 2019 log employment, and employment log change. The year indicator will be added after constructing the pooled dataset.

The wage comparison table reports annual occupational wages for both years in **2025 U.S. dollars**.

## Repository contents

| File or folder | Contents |
|---|---|
| `Econ326_Data/` | Downloaded source datasets |
| `Econ326_Data/oesm19nat/national_M2019_dl.xlsx` | Extracted 2019 BLS national data |
| `Econ326_Data/CPIAUCNS.csv` | Monthly CPI-U data |
| `Econ326_Data/processed/analysis_data_2025.csv` | Cleaned 2025 dataset |
| `Econ 326 Project.ipynb` | R notebook for data preparation and analysis |
| `summary_statistics.csv` | Main 2025 summary table |
| `extension_summary_statistics.csv` | Summary statistics for exploratory variables |
| `comparison_data_2019_2025.csv` | Matched data containing both years’ values |
| `wage_comparison_summary.csv` | Wage summaries in 2025 dollars |
| `README.md` | Project overview and instructions |

The exported comparison dataset has **624 rows**, with both years’ values in separate columns. This differs from the pooled regression dataset, which will have **1,248 rows**.

## Running the analysis

Open the notebook in JupyterLab using an **R kernel**.

Install the required packages once if needed:

```r
install.packages(c(
  "readxl",
  "dplyr",
  "tidyr",
  "ggplot2",
  "sandwich",
  "lmtest"
))
```

Update file paths to match your local repository, then run the notebook from top to bottom.

- Read Excel files with `read_excel()`.
- Read `CPIAUCNS.csv` with `read.csv()`.
- Use O*NET **27.0** to reproduce the skill measures.
- Inspect nonnumeric BLS entries before excluding missing values; do not replace them with zero.

GitHub displays saved notebook outputs. Calculations run in the local JupyterLab R session.

## Limitations

The analysis compares occupations rather than individual workers and cannot establish individual returns to skills or training. AI exposure measures potential exposure rather than actual adoption. Education, experience, licensing, worker composition, and other omitted characteristics may influence the estimated relationships.

2019 is a reference year before widespread public generative-AI adoption, not a year without AI. Comparisons with 2025 may reflect pandemic disruptions and BLS classification and methodological changes. CPI adjustment accounts for inflation but does not resolve these comparability issues.

O*NET 27.0 skill ratings have mixed collection dates and are held fixed across years rather than measured separately in 2019 and 2025.

Employment declines cannot be attributed to AI. The matched sample requires usable data in both years and cannot identify occupations that disappeared. Findings may not generalize directly to Canada. Canadian youth unemployment motivates the discussion but is not an outcome examined by this project.

## References

Bureau of Labor Statistics. (n.d.-a). *Consumer Price Index databases*. https://www.bls.gov/cpi/data.htm

Bureau of Labor Statistics. (n.d.-b). *Frequently asked questions: Occupational Employment and Wage Statistics*. https://www.bls.gov/oes/oes_ques.htm

Bureau of Labor Statistics. (n.d.-c). *Occupational Employment and Wage Statistics tables*. https://www.bls.gov/oes/tables.htm

Felten, E. W., Raj, M., & Seamans, R. (2021). Occupational, industry, and geographic exposure to artificial intelligence: A novel dataset and its potential uses. *Strategic Management Journal, 42*(12), 2195–2217. https://doi.org/10.1002/smj.3286

International Labour Organization. (2025). *Generative AI and jobs: A 2025 update*. https://www.ilo.org/publications/generative-ai-and-jobs-2025-update

National Center for O*NET Development. (2022). O*NET database (Version 27.0) [Data set]. https://www.onetcenter.org/db_releases.html

National Center for O*NET Development. (n.d.). *Crosswalk 2010 to 2019*. https://www.onetcenter.org/taxonomy/2019/walk.html

Statistics Canada. (2026, November 6). *Labour Force Survey, October 2026*. https://www150.statcan.gc.ca/n1/daily-quotidien/261106/dq261106a-eng.htm