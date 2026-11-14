# Project-Econ-326
# AI Exposure, Human Skills, and Occupational Wages

An ECON 326 project examining the relationship between AI exposure, human skill requirements, and wages across **624 U.S. occupations**.

## Why this research matters

For students preparing to enter the workforce, rapid advances in AI raise pressing questions: Which careers offer opportunities, and which skills will employers value? These questions are particularly relevant in Canada, where unemployment among people aged 15–24 reached **13.7% in October 2026**, highlighting the challenges facing young job seekers. [Statistics Canada](https://www150.statcan.gc.ca/n1/daily-quotidien/261106/dq261106a-eng.htm)

The uncertainty extends beyond Canada. The International Labour Organization estimates that **one in four workers worldwide** holds an occupation with some exposure to generative AI, although exposure does not necessarily imply job loss. [International Labour Organization](https://www.ilo.org/publications/generative-ai-and-jobs-2025-update?facet=app&mode=light)

Our research contributes to this discussion by examining how AI exposure, communication, and problem-solving requirements relate to occupational wages. It helps distinguish wage patterns associated with AI exposure from those associated with human skill requirements. The findings provide context for discussions about career preparation and workforce training; they do not establish that AI causes unemployment or predict individual career outcomes.

## Research question

**How is AI exposure correlated with occupational wages, and how does this relationship change after accounting for communication and problem-solving requirements?**

We estimate wage regressions controlling for occupational employment and broad occupation groups. We also compare the wage associations of communication and problem-solving requirements. The study examines correlations rather than causal effects.

## Data sources

| Source | Data used | Purpose |
|---|---|---|
| [BLS Occupational Employment and Wage Statistics](https://www.bls.gov/oes/tables.htm) | May 2025 national estimates | Annual mean occupational wages, employment, and occupation codes |
| [AI Occupational Exposure index](https://github.com/AIOE-Data/AIOE) | Appendix A of `AIOE_DataAppendix.xlsx` | Occupational AI exposure scores from Felten, Raj, and Seamans (2021) |
| [O*NET Database 27.0](https://www.onetcenter.org/db_releases.html) | Importance ratings in `Skills.xlsx` | Communication and problem-solving requirements |
| [O*NET 2010–2019 crosswalk](https://www.onetcenter.org/taxonomy/2019/walk.html) | Occupation-code mappings | Match occupation classifications across sources |

The AIOE index measures general potential AI exposure. It is not a measure of actual AI use or specifically of ChatGPT adoption. The ILO’s generative AI estimates provide background context and are not the exposure measure used in our analysis.

## Sample construction

We retain detailed national occupations with unchanged, one-to-one six-digit occupation-code mappings. We then require positive, available wages and employment, an AI exposure score, and all five selected skill ratings.

| Stage | Occupations |
|---|---:|
| Detailed BLS occupations | 830 |
| After the occupation-code screen | 661 |
| Matched to AI exposure scores | 628 |
| With usable wages, employment, and AI scores | 625 |
| With all five skill ratings | 624 |

Psychologists, All Other (`19-3039`) is excluded at the final stage because the selected O*NET skill ratings are unavailable.

Each observation represents an occupation, not an individual worker. The main analysis gives each occupation equal weight. Missing values are not replaced with zero or imputed.

The current sample uses 2025 completeness checks. It replaces the earlier 622-occupation sample, which also required complete historical wage records.

## Variables

| Variable | Definition and purpose |
|---|---|
| Log annual wage | Natural logarithm of the occupation’s 2025 mean annual wage; the dependent variable |
| AI exposure | Standardized AIOE score measuring potential occupational exposure |
| Human skills | Standardized index combining five selected O*NET importance ratings |
| Communication | Standardized index combining Speaking and Active Listening |
| Problem-solving | Standardized index combining Critical Thinking, Complex Problem Solving, and Judgment and Decision Making |
| Log employment | Natural logarithm of occupational employment, controlling for occupation size |
| Occupation groups | Indicators for 22 broad SOC groups; Management is the regression reference category |
| Squared exposure and skills | Squared standardized AI exposure and human-skills scores, used to examine nonlinear relationships |

O*NET suboccupation ratings are first averaged within six-digit occupations. Each of the five skill ratings is standardized before averaging. The combined human-skills index and its two component indices are then standardized separately using the final sample.

Summary statistics report the number of observations, mean, sample standard deviation, minimum, and maximum. Occupation-group indicators are included as separate rows.

## Proposed regression

The main model is:

\[
\ln(Wage_i)
=
\beta_0
+\beta_1 AI_i
+\beta_2 Skills_i
+\beta_3\ln(Employment_i)
+\text{Occupation-group controls}
+\varepsilon_i.
\]

Here, \(i\) represents an occupation. The coefficients on AI exposure and human skills describe their wage associations while holding the included variables constant.

We plan to estimate ordinary least squares regressions with HC3 robust standard errors and report two-sided tests and 95% confidence intervals. For a standardized variable, a one-standard-deviation increase corresponds approximately to a \(100\beta\)% wage difference; the exact transformation is \(100(e^\beta-1)\)%.


## Model evaluation and further analysis

We will compare regressions with and without human skills and controls, assess multicollinearity and influential observations, and examine robustness using employment weights and squared exposure and skill scores.

A further specification will replace the combined human-skills index with the communication and problem-solving indices. We will test whether their coefficients differ. These analyses are exploratory because preliminary results informed the research design.

## Repository contents

- `Econ326_Data/` — downloaded source datasets.
- `Econ 326 Project.ipynb` — R notebook for data preparation and analysis.
- `analysis_data_2025.csv` — cleaned dataset exported by the notebook.
- `summary_statistics.csv` — summary table exported by the notebook.
- `README.md` — project overview and instructions.

The CSV files are generated outputs and must be exported after running the corrected notebook.

## Running the analysis

Open the notebook in JupyterLab using an **R kernel**.

Install the data preparation and plotting packages once:

```r
install.packages(c("readxl", "dplyr", "tidyr", "ggplot2"))
```

For the planned robust regression inference:

```r
install.packages(c("sandwich", "lmtest"))
```

Update the notebook’s file paths to match your local repository, then run the cells from top to bottom. Use the specified O*NET 27.0 release to reproduce the skill measures.

## Limitations

The analysis compares occupations rather than individual workers, so it cannot establish individual returns to skills or training. AI exposure measures potential exposure rather than actual adoption, and the 2025 cross-section cannot identify how AI has changed wages over time.

Education, experience, licensing, and other omitted characteristics may influence the estimated relationships. Robust standard errors do not eliminate these limitations or establish causation.

The occupation-code and completeness requirements produce a selected sample. Findings do not represent every U.S. occupation and cannot be directly generalized to Canadian workers. Canadian youth unemployment motivates the discussion but is not an outcome examined by this project.

## References

Bureau of Labor Statistics. (n.d.). *Occupational Employment and Wage Statistics tables*. https://www.bls.gov/oes/tables.htm

Felten, E. W., Raj, M., & Seamans, R. (2021). Occupational, industry, and geographic exposure to artificial intelligence: A novel dataset and its potential uses. *Strategic Management Journal, 42*(12), 2195–2217. https://doi.org/10.1002/smj.3286

International Labour Organization. (2025). *Generative AI and jobs: A 2025 update*. https://www.ilo.org/publications/generative-ai-and-jobs-2025-update

National Center for O*NET Development. (2022). *O*NET database, version 27.0*. https://www.onetcenter.org/db_releases.html

National Center for O*NET Development. (n.d.). *Crosswalk 2010 to 2019*. https://www.onetcenter.org/taxonomy/2019/walk.html

Statistics Canada. (2026, November 6). *Labour Force Survey, October 2026*. https://www150.statcan.gc.ca/n1/daily-quotidien/261106/dq261106a-eng.htm