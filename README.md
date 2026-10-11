# Student Performance Analysis

CS 133 final project, SJSU, Fall 2026

## Authors

Tristan Lowe (working solo, so I wrote both plan sections below)

## Project description

For this project I'm using the UCI Student Performance dataset, which has records for students at two secondary schools in Portugal. Each row is one student, with their grades in either math or Portuguese plus survey answers like weekly study time, absences, past class failures, parents' education and how often they go out with friends. The question I want to answer is which of these things actually predict a student's final grade (G3). I'll clean the data, run a few statistical tests, and make a set of static charts that walk through what I find. At the end I'll build an interactive Plotly dashboard where you can filter by subject, school and student group to see how grades change.

## Project plan

### Interface plan

The analysis will be done in Jupyter notebooks, one per milestone, using pandas, matplotlib, seaborn and scipy. The interactive part will be a Plotly dashboard saved as an HTML file, so it opens in a browser without installing anything.

What the dashboard will have:

- A dropdown to switch between the math and Portuguese data
- Filters for school, sex and number of past failures
- A scatter plot of absences vs. final grade and a bar chart of average G3 by group, both updating with the filters
- Hover info on each student (age, study time, absences, G1/G2/G3)

I'm not planning to deploy it. If I have time at the end I might put it on GitHub Pages for the bonus.

Planned folder layout:

```
data/raw/        original CSVs, never edited
data/clean/      cleaned CSVs from the cleaning notebook
notebooks/       one notebook per milestone
figures/         exported PNGs for the static charts
dashboard/       the Plotly HTML file
report/          final report and slides
```

### Data collection and storage plan

The data comes from the UCI Machine Learning Repository: https://archive.ics.uci.edu/dataset/320/student+performance (Cortez and Silva, 2008). It was collected during the 2005-2006 school year from school reports and a student questionnaire. The download is a zip with two files, `student-mat.csv` (395 students, math) and `student-por.csv` (649 students, Portuguese). Both have the same 33 columns and use semicolons instead of commas, so I'll need `sep=';'` when loading them.

The original files go in `data/raw/` and I won't edit them. All the cleaning happens in one notebook that reads from `data/raw/` and saves to `data/clean/`, so anyone can rerun it and get the same output. The files are small (under 100 KB) so it's fine to keep them in the repo.

Things I'm going to check and clean:

- Missing values. The UCI page says there aren't any, but I'll confirm it myself.
- Duplicate rows and data types. A lot of columns are yes/no strings that I may convert to 0/1.
- Students with a final grade of 0. These look like students who dropped out or skipped the final exam rather than real scores, so I need to decide whether to drop them or analyze them separately.
- Outliers in absences (the max in the math file is 75).
- Overlap between the files. Around 380 students are in both, so I'm keeping the subjects separate instead of stacking them, which would count those students twice.

I'll write the data dictionary for all 33 columns in the Milestone 0 notebook.

### Data analysis and visualization plan

I'll start with descriptive stats (mean, median, min, max, std) for the grades and the main numeric columns, broken down by subject and school, and by sex and age where it makes sense. Grades are on a 0-20 scale and 10 is passing, so I'll also add a pass/fail column.

Statistical tests I'm planning:

- t-test: final grade for students who want to go to college vs. those who don't
- ANOVA: final grade across the four weekly study time levels
- Chi-square: pass/fail vs. number of past failures
- Correlation between absences, study time, G1, G2 and G3

Static charts (at least 5):

1. Histogram of final grades for each subject
2. Box plot of G3 by number of past failures
3. Bar chart of average G3 by study time, with error bars
4. Scatter plot of absences vs. G3
5. Correlation heatmap of the numeric columns
6. Pass rate by weekend alcohol use

For accessibility I'll use seaborn's `colorblind` palette, label every axis, and run the PNGs through a colorblindness simulator before turning them in.

Some bias and limitation issues I already know about. The data is from only two schools in one country almost 20 years ago, so it may not generalize. Things like alcohol use and family relationships are self-reported. G1 and G2 are earlier grades in the same class, so they predict G3 almost automatically and could make other factors look less important than they are. And everything here is correlation, not cause.

If I get to the optional ML part, I'll try a linear regression or random forest that predicts G3 without using G1 and G2.

## License

MIT, see [LICENSE](LICENSE).
