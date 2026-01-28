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

The Department for Education (DfE) A-Level Grade Combinations Analyser is an R Shiny application prototype developed during my internship at the DfE.

- Explores the correlations in student performance between A-Level subjects, based on national AS and A-Level results from 2022 to 2024 accounting for pandemic/post-pandemic influence.
- Supports decision-making for University admissions boards, policymakers, parents, and students.
- All data was precomputed for reduced latency within the website, in addition to increased confidentiality and reproducibility.

Presented at the end of my internship to a group of interdepartmental policymakers and Ofqual analysts, where it was adopted and fed more data to give the [current page as it exists today](https://analytics.ofqual.gov.uk/apps/Alevel/SubjectCombinations/).

> The original dataset's size (1.87GB CSV representing all ~2.5M A-Level students from 2022 - 2024) and Official‑Sensitive class prohibit it's inclusion, however an anonymised mockup of the data (~1% the original size) and corresponding code is available to allow demoing of the program.

<br>

### <ins>Causal analysis</ins>
Analysis of the data was done beforehand for causality using research and analysis methods from a [2024 Ofqual paper on this topic](https://assets.publishing.service.gov.uk/media/66a1022dfc8e12ac3edb03ee/ISC_2023.pdf).
  - Statistical analysis was used to quantify whether the data defended inter-subject comparability.
      - (i.e., whether there is a real 'ability' dimension that allows meaningful comparisons to be made between students' grades in completely-different subjects)
  - Found strong correlations between prior attainment and grade outcomes across a signficant number of subject combinations, but this is not the only/primary factor at play for the result and there are limitations with model used (discussed in Ofqual's paper).
  - Therefore, <ins>**this website should only be used as supplementary (not primary) information for decision-making**</ins> (as stated on the website).

>Note: My final report for the internship emphasised this point that the statistical difficulty measures used were not direct measures of performance standards due to differences that may arise from many other factors; facts that were mentioned in the Ofqual paper along with the inherent limitations of their model.

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

- **R version**: 4.2.0 or above
- **Input data**:
  - `SubjectComb_Final_RANDOMISED.csv`

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

- Interactive grade distribution visualisation: users can select a subject and view a dynamic bar chart of grade distributions.
- Explore individual, 2-subject and 3-subject pairings between all 137 A-Level subjects, and performance differences for all of them across all grade bands.
- Filter by grade band to see what combinations the high-achievers got (though performance comparisons are hidden in this view and a clear in-app explanation is displayed to reinforce it)
- Designed for non-technical users, including students, parents, academic institutions, and policymakers.
 

### App preview

#### Tab 1: Grade distribution checker
![Tab 1](assets/DfE-App-Tab1.png)

#### Tab 2: Overall grade combinations
![Tab 2](assets/DfE-App-Tab3.png)

#### Tab 3: Grade-filtered grade combinations
![Tab 3](assets/DfE-App-Tab2.png)

---

## Limitations

- No causal results and PPE comparisons (Tab 3) are descriptive-only.
    - The app is explicitly stated as using non-inferential data, though further research is/will be conducted to quantify external factors.
- Cannot see how many students got a specific grade, (i.e., cannot see how many got exactly a B in Mathematics, only how many got B and above). This is largely due to this specific functionality not being needed during the project, however this was one of the features added by Ofqual in the [current page](https://analytics.ofqual.gov.uk/apps/Alevel/SubjectCombinations/).
- Streamed data updates were not part of the original prototype due to scope, though, as above, this has been added by Ofqual. 

- Accessibility features were implemented (screen reader compatibility, WCAG colours/contrast, dark mode), though future improvements could include ARIA labels, tab key navigation cues, and full WCAG compliance.
