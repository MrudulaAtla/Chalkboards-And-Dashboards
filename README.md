# Chalkboards and Dashboards
### AI-Augmented Analysis of Massachusetts Public Schools

A data analytics portfolio project combining Python, the OpenAI API and Tableau to generate deeper insights from the Massachusetts Department of Education dataset.

---

## Live Dashboards

View the interactive dashboards on Tableau Public:

**Chalkboards and Dashboards - MA Schools Analysis**

[**Dashboard 1: AI Insights Dashboard**](https://public.tableau.com/views/ChalkboardsandDashboards-MASchoolsAnalysis/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
A state map of all 678 AI classified schools and bar charts showing struggling schools ranked by school type (High School / Middle School / K8 School).

[**Dashboard 2: State Overview Dashboard**](https://public.tableau.com/views/ChalkboardsandDashboards-MASchoolsAnalysis/Dashboard2?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
A scatter plot exploring class size and college attendance, plus top math schools ranked by grade level( Grade 4, Grade 8, Grade 10).

---

## The Most Surprising Finding

**46 Massachusetts high schools are classified as Thriving on student outcomes but simultaneously rated Level 2 by the state on equity metrics.**

Schools like Boston Latin and Andover High were classified as Thriving by my AI system, and rightly so. Boston Latin has a 98.1% graduation rate and 94.1% college attendance rate. Andover High has a 95.7% graduation rate and 89.3% college attendance rate. Anyone looking at the spreadsheet would immediately consider them top performers.

But the Massachusetts state accountability system rated both schools Level 2. This means they are not closing the achievement gap between their high performing student groups and their disadvantaged students. Strong overall numbers can hide unequal outcomes within a school. This is exactly what happened here.

This finding only emerged when I compared my AI classification to the state's official rating system. Looking at either system alone would not have revealed it.
---

## Why I Built This

The Massachusetts Department of Education publishes school performance data for 1,861 schools across 299 columns. 299 columns of data exist for each school, but very little of it has been turned into actionable insight. That is what I intend to do with this project.

This project set out to answer four questions that a Massachusetts school board might actually ask:

1. What is the overall state of the public school system?
2. How does class size affect college attendance?
3. Which schools are the top math performers in the state?
4. Which schools are struggling the most and need urgent attention?

Each question has a dedicated view in the Tableau dashboards. Questions 1 and 4 use the AI enriched dataset of 678 classified schools. Questions 2 and 3 use the full original dataset of 1,861 schools because those questions benefit from the complete picture.

The core question driving the AI work was different though. Can AI surface insights from this dataset that Tableau alone cannot show? The answer turned out to be yes. But only after rigorous validation 
of the AI output.

---

## What This Project Demonstrates

| Skill | Where It Shows Up in This Project |
|---|---|
| Data preparation | The original 1,861 school dataset needed to be split into three groups before any analysis could happen. Each group required its own logic based on the metrics available for that school type. |
| Analytical thinking | High schools, K-8 schools and middle schools are measured completely differently. Building a separate classification framework for each one was a deliberate analytical decision, not a technical shortcut. |
| Prompt engineering | Getting 678 consistent, parseable JSON responses from a language model required careful prompt design, iteration and testing before running the full batch. |
| API integration | The OpenAI API was called programmatically in Python across all 678 schools with rate limit management, error handling and automatic result parsing built in. |
| Critical thinking | I did not trust the AI output until I had validated it three different ways. The validation section of the notebook exists because AI confidence is not the same as AI accuracy. |
| Comparative analysis | Comparing my classification to both Excel IF rules and the state official system was how the most important finding in this project emerged. |
| Insight generation | The equity gap finding, 46 schools Thriving on outcomes but failing on equity, was not something I set out to find. It emerged from the comparison. |
| Communication | The enriched data was designed from the start to feed directly into Tableau with AI generated insights as tooltips and callouts. |
```
---

## About the Dataset

- **Source:** [ Massachusetts Department of Education via Kaggle]()
- **Original data:** profiles.doe.mass.edu/statereport/
- **Size:** 1,861 schools, 299 columns
- **Scope:** Public schools only. Private schools in Massachusetts are not required to report data to the state DOE and are therefore not included.

---

## How I Filtered to 678 Schools

Different school types track completely different metrics. A single classification framework for all 1,861 schools would have been meaningless. You cannot ask an elementary school for its graduation rate.

Instead I built three separate frameworks, each using the metrics that actually apply to that school type.

```
High schools:       343 schools   graduation rate, college attendance, SAT, AP scores
K-8 schools:        254 schools   3rd and 4th grade MCAS math and english scores
Middle schools:      81 schools   6th, 7th and 8th grade MCAS math and english scores
Total classified:   678 schools   (36% of all Massachusetts schools)
Excluded:         1,183 schools   early childhood centers, incomplete data, alternative schools
```

Rather than filtering by grade labels (which had 75 unique combinations in the dataset), I filtered by the presence of outcome data. For example, only schools with graduating classes report graduation rates. This approach captured 343 high schools, including K-12 and 6-12 schools that a simple grade label filter would have missed.

---

## The Four Performance Tiers

All three school types use the same four tiers so the Tableau dashboard stays consistent across school levels.

| Tier | Meaning |
|---|---|
| **Thriving** | Strong outcomes across most metrics |
| **Performing** | Average outcomes, no major red flags |
| **Struggling** | Below average outcomes in multiple key areas |
| **At Risk** | Significantly below average, needs urgent intervention |

### High School Classification Results
```
Performing    157  (46%)
Thriving      120  (35%)
Struggling     39  (11%)
At Risk        25   (7%)
Error           2   (1%)
```

---

## Validating the AI Output

A good analyst never blindly trusts model output. Before using any classifications in Tableau I ran three separate tests.

**Tier Progression Check**
I calculated average metrics for each tier. The results showed a perfect descending pattern.
```
Thriving      96.5% average graduation rate
Performing    90.5%
Struggling    75.7%
At Risk       42.3%
```

**Spot Check**
I looked for obvious errors. No school was falsely labeled Thriving with a low graduation rate. No school was labeled At Risk with a high graduation rate. Zero misclassifications found.

**Consistency Check**
I classified the same five schools twice each. All five got identical results both times. The consistency rate was 100%, which confirmed the prompt was reliable enough to use at scale.

---

## Key Findings

**1. The Outcomes vs Equity Gap**
46 high schools are Thriving on graduation and college attendance but rated Level 2 on the state equity metric. They are serving their advantaged students well. They are not closing achievement gaps for disadvantaged groups. This finding only emerged by comparing two different classification systems side by side.

**2. AI vs Excel IF Rules**
My AI classification and a rule-based Excel IF formula agreed 67.3% of the time. In the cases where they disagreed, the AI consistently caught things Excel missed. Schools with acceptable graduation rates but low SAT scores, high dropout rates and poor AP performance passed the Excel threshold but were flagged by the AI. Excel can only handle two or three metrics cleanly. The AI evaluated fifteen or more simultaneously.

**3. The Most Urgent Schools**
16 schools are simultaneously At Risk on my classification AND Level 3 or 4 on the state equity metric. They are failing on every available measure. These are the schools most in need of immediate intervention.

**4. Math Performance Across Grade Levels**
Average 4th grade math proficiency (60.5%) and 8th grade math proficiency (59.7%) are nearly identical. There is almost no improvement between elementary and middle school years across the state. The jump to 77.3% at 10th grade partly reflects the Massachusetts graduation requirement, which drives intensive test preparation and gives students multiple retake opportunities for the 10th grade MCAS.

**5. Top Math Schools and Economic Advantage**
None of the top 20 schools by 4th grade math proficiency have more than 35% economically disadvantaged students. The correlation between wealth and academic performance is stark and consistent across all three grade levels analyzed.

---

## Files in This Repository

| File | Description |
|---|---|
| `MA_Schools_AI_Analysis_v4.ipynb` | Main analysis notebook with all classification, validation and comparison code |
| `MA_HighSchools_AI_Enriched.csv` | 343 high schools with AI classification, AI insights and Excel IF comparison |
| `MA_K8Schools_AI_Enriched.csv` | 254 K-8 schools with AI classification and MCAS performance columns |
| `MA_MiddleSchools_AI_Enriched.csv` | 81 middle schools with AI classification and MCAS performance columns |
| `MA_AllSchools_AI_Enriched.csv` | All 678 schools combined with common columns for the Tableau state overview |
| `MA_Schools_data_set.xlsx` | Original dataset from the Massachusetts DOE |

---

## How to Run This Notebook

1. Clone this repository
2. Install dependencies: `pip install openai pandas openpyxl`
3. Get an OpenAI API key at platform.openai.com
4. Open `MA_Schools_AI_Analysis_v4.ipynb` in VS Code or Jupyter
5. Replace `your-openai-key-here` in the API setup cell with your actual key
6. Run all cells in order from top to bottom

**Estimated cost:** Approximately $0.50 to $1.00 to classify all 678 schools using gpt-4o-mini.

---

## Why gpt-4o-mini?

It costs about 15 times less than gpt-4o. After running the full classification I validated the output with a consistency check and got 100% reliability across five schools classified twice. The cheaper model was more than capable for this task.

---

## Limitations

1. **Single year snapshot.** This analysis is based on 2017 data only. School performance, accountability levels and demographic compositions may have changed significantly since then.

2. **Coverage is 36%.** 1,183 schools were excluded because they lacked sufficient data for meaningful classification. This is not a complete picture of all Massachusetts schools.

3. **Column selection was judgment based.** The columns passed to the AI were selected based on domain knowledge and reasoning about what matters in education. No formal statistical feature selection was run to validate these choices.

4. **The AI is a black box.** I validated the output but cannot see exactly why the AI made each individual decision. That is why the validation section exists.

5. **Multi-level schools are classified into one bucket.** K-12 and 6-12 schools are classified purely on their high school metrics. Their elementary and middle school performance is not evaluated separately.

---

## Future Scope

- Add multi-year trend analysis to show which schools are improving and which are declining
- Build classification frameworks for early childhood centers and alternative schools to get closer to full coverage
- Run a formal feature selection analysis to statistically validate which columns most strongly predict school quality
- Build a context-adjusted classification that accounts for socioeconomic factors more explicitly
- Automate the pipeline so it updates classifications each year when new DOE data is released

---

## Important Notes

**On AI assistance:**
AI tools were used to scaffold portions of the code in this project. All AI generated code was reviewed, understood, modified and validated by me. The analytical decisions including column selection, prompt design, validation methodology and interpretation of findings were made by me, not the AI.

**On the API key:**
The OpenAI API key has been removed from the notebook before publishing. Replace the placeholder in cell 6 with your own key to run the analysis.

---

## Author

**Mrudula Reddy Atla**

Dataset: Massachusetts Department of Education 2017.
API: OpenAI gpt-4o-mini.
Visualization: Tableau Public.

*Chalkboards and Dashboards - MA Schools Analysis 2017*
