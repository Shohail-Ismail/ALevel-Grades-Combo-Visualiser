# Department for Education: A-Level Grade Combinations Analyser

   * [Background information](#background-information)
     * [Summary](#summary)
     * [Causal analysis](#causal-analysis)
     * [Visualiser use-cases](#visualiser-use-cases)
   * [Running the app](#running-the-app)
   * [Core features](#core-features)
   * [Limitations](#limitations)

---

## Background information
### <ins>Summary</ins>

The Department for Education (DfE) A-Level Grade Combinations Analyser is an R Shiny application prototype developed during my internship at the DfE. 18769/2534172 19/2000

- Explores the correlations in student performance between A-Level subjects, based on national AS and A-Level results from 2022 - 2024 (~4.5M rows of data representing ~2.5M unique students nationwide), accounting for pandemic/post-pandemic influence.
- Supports decision-making for University admissions boards, policymakers, parents, and students.
- All data is precomputed for reduced latency within the website, in addition to increased confidentiality and reproducibility.

Presented at the end of my internship to a group of interdepartmental policymakers and Ofqual analysts, where it was adopted and fed more data to give the [current page as it exists today](https://analytics.ofqual.gov.uk/apps/Alevel/SubjectCombinations/).

<br>

### <ins>Causal analysis</ins>
Analysis of the data was done beforehand for causality using research and analysis methods from a [2024 Ofqual paper on this topic](https://assets.publishing.service.gov.uk/media/66a1022dfc8e12ac3edb03ee/ISC_2023.pdf).
  - Statistical analysis was used to quantify whether the data defended inter-subject comparability.
      - (i.e., whether there is a real 'ability' dimension that allows meaningful comparisons to be made between students' grades in completely-different subjects)
  - Found strong correlations between prior attainment and grade outcomes across a signficant number of subject combinations, but this is not the only/primary factor at play for the result and there are limitations with model used (discussed in Ofqual's paper).
  - Therefore, <ins>**this website should only be used as supplementary (not primary) information for decision-making**</ins> (as stated on the website).

>Note: My final report for the internship emphasised this point that the statistical difficulty measures used were not direct measures of performance standards due to differences that may arise from many other factors; facts that were mentioned in the Ofqual paper along with the intrinsic stat-model limitations.

<br>

### <ins>Visualiser use-cases</ins>
Uses of the app include:

- Exploring the impacts between grades in certain A-Level subjects on others, based on historical data. Used best by chaining results to answer questions of varying complexities, for example:
    - **Simple**: *Which subject(s) pairing is most common with Physics?*
    - **Intermediate**: *How does choosing Art vs choosing Biology correlate to final grade in Mathematics?*
    - **Complex**: *What is the average grade point improvement in Further Mathematics for high performers vs low performers, given they took Mathematics and Physics?*
- Viewing grade distribution plots for each query across individual subjects and grades.
- Comparing normalised average grade point and percentage-wise comparisons across student performance and/or subjects.

---

## Running the app

- **R version required**: 4.2.0 or above
- **Input data required**:
  - `SubjectComb_Final_RANDOMISED.csv` (placed in the working directory).

**Due to GDPR regulations, the actual data has been removed. An anonymised mockup of the data ~2% the original size has been included for demo.**

You must download R to run this program, or use a conda environment with R 4.2

### R:

```r
# install packages
install.packages(c("shiny", "dplyr", "readr", "DT", "shinyalert", "plotly"))

# then run the app
shiny::runApp("app.R")
```
### R with conda:

```bash
# create environment with R 4.2 and pkgs
conda create -n shiny-env r-base=4.2 r-shiny r-dplyr r-readr r-dt r-shinyalert r-plotly -c conda-forge
conda activate shiny-env
R

# then run the app in R console
> shiny::runApp("app.R")
```

## Core features

- **Interactive grade distribution visualisation**  
  - Users can select a subject and view a dynamic bar chart of grade distributions using traditional UK grade bands (A*, A and above, etc.).
  - Built with Plotly for interactive tooltips and accessibility.

- **Explore subject pairings and performance (all grades)**  
  - Users select a subject to view precomputed pairings showing:
    - Number of students taking both subjects
    - Difference in average PPE scores between students who did and didn’t take the second subject
  - Performance differences are presented in neutral, non-inferential terms (e.g. `+5.1 PTS`)
  - Clicking a row opens a ShinyAlert popup with full details

- **Filter subject pairings by grade band**  
  - Enables viewing of subject pairings restricted to a selected grade level (e.g. “B and above”)
  - **Performance comparisons are hidden in this view to avoid misinterpretation**
  - A clear in-app explanation is displayed to reinforce this

- **Designed for non-techincal users**  
  - No policy knowledge is required
  - Neutral UI language (e.g. avoids “correlation”, “impact”, “increase”)
  - Clean layout using `fluidPage()` and `tabsetPanel()` with scrollable, readable tables
  - Dark mode available for accessibility and usability purposes
 

### App preview

#### Tab 1: Grade distribution checker
![Tab 1](assets/DfE-App-Tab1.png)

#### Tab 2: Overall grade combinations
![Tab 2](assets/DfE-App-Tab3.png)

#### Tab 3: Grade-filtered grade combinations
![Tab 3](assets/DfE-App-Tab2.png)

---

## Limitations

- **No statistical inference**  
  - The app is explicitly non-inferential - no significance testing or causal claims
  - PPE comparisons are descriptive only and based on precomputed values

- **No specific grades**
  - Cannot see how many students got a specific grade, (i.e., cannot see how many students got exactly a B in Mathematics, only how many got B and above). This is largely due to this specific functionality not being needed during the project, however this has been added in the [current page](https://analytics.ofqual.gov.uk/apps/Alevel/SubjectCombinations/)

- **Grade filter disables comparisons**  
  - When filtering by grade (Tab 3), performance differences are removed to avoid misleading conclusions based on subgroups

- **Static dataset dependency**  
  - All insights rely on the provided `.csv` file; dynamic or streaming data updates are not currently supported

- **Limited accessibility enhancements**  
  - Basic screen reader compatibility and contrast considered, but future improvements could include ARIA labels, tab key navigation cues, and full WCAG compliance
