# Lab 08: Pattern discovery

## Preparation

- Read/ annotate: [Recipe \#8](https://qtalr.github.io/qtalrkit/articles/recipe-8.html). You can refer back to this document to help you at any point during this lab activity.

## Objectives

- Review skills and knowledge on performing diagnostic checks on data
- Learn to frame research questions to explore for insights
- Apply skills and knowledge to explore these questions using descriptive and/ or unsupervised methods

## Instructions

### Getting started

In this lab, you will be using Git and Github to fork, clone, commit, and push changes to a repository. The repository you will select to use as the repository to fork to your own Github account can be one of the following:

- [Lab 08 repository](https://github.com/qtalr/lab-08)
- [Minimal template](https://github.com/qtalr/project)
- [Web-based project template](https://github.com/qtalr/project_web)
- Other (consult your instructor)

If you are starting with a new repository, fork and clone the repository you selected to your local machine. Then orient yourself to the repository by opening the README file and reviewing the template configuration.

If you are using an existing reproducible research project repository, open that project on your local machine, and `pull` the latest changes from the remote repository to ensure that your local and remote repositories are in sync.

Open a Quarto document in the process directory and name it accordingly (e.g., `4_analysis.qmd`, `analysis.qmd`, *etc.*).

The data you select to explore should be in a format conducive for exploratory analysis. The options include the following:

- British and American Literature of the mid-19th century (see [below for details](#british-and-american-literature-of-the-mid-19th-century))
- The dataset you transformed in Lab 07
- Other (consult your instructor)

### Exploratory analysis

In your analysis process file,

1. add a section which provides a brief description of the dataset you will be exploring and what your primary research questions are. Include:

  - the name and/ or source of the data
  - the nature of the data
  - the primary research question(s) you will be exploring
  - the unit of observation and key variables of interest in the dataset

2. add a section which provides a description of the analytical process you will be using to explore this(ese) question(s). Include:

  - a description of the high-level analytical process you will be using to explore the question(s) (e.g. frequency analysis, clustering, *etc.*)
  - for each analytical process, a description of the specific analytical process you will be using to explore the question(s) (e.g. frequency analysis of the top 10 most frequent words, *etc.*)

3. add a section for each analytical process you will be using to explore the question(s). In this section, you will document with code, code comments, and prose the process of exploring the data. This is where you will craft the code to explore the data. Feel free to use existing R packages and functions as you see fit.

4. Make sure to organize your analysis process in a way that is reproducible. This means that you should be able to run the code in your  process file and reproduce the process (use `set.seed()` for any sampling process, for example). Use the `data/analysis` (or similar) directory to store any derived datasets used in the analysis.

5. Make sure that your code is well documented with code comments and that you have included prose to describe the process of analyzing the dataset.

6. Include a section to describe the results of your analysis.

7. Confirm that your code runs without errors and that the code, visualizations, and/ or tables are displayed as expected.

8. Finally, commit and push your changes to your Github repository. *Make sure to include files or directories that you do not have permission to share in your `.gitignore` file.*

### Assessing your progress

1. In your repository on Github, open an issue to provide feedback on your experience with this lab (Click on the 'Issues' tab and then click the 'New issue' button). Title the issue "Lab 08 feedback" and provide your feedback in the body of the issue.

Some questions to consider:

  - What did you learn?
  - What did you find most/ least challenging?
  - What resources did you consult?
    - Instructor? R or Quarto documentation, Websites (provide links)?
  - What more would you like to know about exploratory analysis?
    - Find potential resources you might consult to continue your learning. Provide links and a brief description of the resource.

## Submission for feedback

- Provide a link to your GitHub repository

## British and American Literature of the mid-19th century

To acquire the dataset, you may use the `get_gutenberg_works()` function from the `qtalrkit` package. (See the documentation)

The Library of Congress codes for British and American Literature are "PR" and "PS" respectively. You can then use the birth year and death year for the authors as 1800 and 1880.

Run the following code to acquire the dataset[^1]:

[^1]: Note you will need and internet connection to run this code and it make take a few minutes to run.

```r
library(qtalrkit)

# Acquire ---------------

# Get the American works
get_gutenberg_works(
  target_dir = "../data/original",
  lcc_subject = "PS",
  birth_year = 1800,
  death_year = 1880
)

# Read `works_ps.csv`
works_ps <- readr::read_csv("../data/original/works_ps.csv")

# Get the British works
get_gutenberg_works(
  target_dir = "../data/original",
  lcc_subject = "PR",
  birth_year = 1800,
  death_year = 1880
)

# Read `works_pr.csv`
works_pr <- readr::read_csv("../data/original/works_pr.csv")

# Transform ---------------

# Combine the two datasets
works <- dplyr::bind_rows(works_ps, works_pr)

# Collapse `text` by `gutenberg_id`
works <-
  filter(works, !is.na(text)) |>
  dplyr::group_by(gutenberg_id, lcc, author, title) |>
  dplyr::summarize(works, text = paste(text, collapse = " ")) |>
  dplyr::ungroup()
```


## License

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.
