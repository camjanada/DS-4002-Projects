# Review Complexity and Critic Polarization on Rotten Tomatoes

**DS 4002 — Project 1**
**Team:** Shawn Chen, Arya Rajesh, Zichen Luo, Camila Janada

## Contents of This Repository

This repository contains the code, data and results for a study of whether the complexity of a movie review is associated with how polarized the critic's score is. Complexity is measured by review length (word count) and readability (Flesch–Kincaid Grade Level). Polarization is measured as how far a review's score falls from the middle of the scale. We fit a multiple linear regression (controlling for top-critic status, with HC3 robust standard errors), then tested it on a held-out set of reviews and compared its performance with a baseline that predicts the average polarization.

The repository is organized into four folders:

- **SCRIPTS/** holds the Google Colab notebook that runs the full analysis: feature creation, exploratory data analysis, model development and model testing.
- **DATA/** holds the cleaned dataset and directions to acquire the original dataset
- **OUTPUT/** holds every figure and results table produced by the notebook.
- **REFERENCES/** holds the sources cited in the project.

**Main finding:** The model generalized to new data and technically beat the baseline, but the improvement was negligible. Review length, readability and top-critic status together explained about 0.2% of the variation in polarization on held-out data (R² = 0.002), far below our goal of R² > 0.20.

## Section 1: Software and Platform

**Software:** Python 3, run in [Google Colab](https://colab.research.google.com/) (free, browser-based).

**Packages:**

| Package | Used for | Install needed in Colab? |
|---|---|---|
| pandas | loading and manipulating data | No (preinstalled) |
| numpy | numerical calculations | No (preinstalled) |
| textstat | Flesch–Kincaid Grade Level | **Yes**: `%pip install textstat` (included in the notebook) |
| statsmodels | regression, robust standard errors, diagnostics | No (preinstalled) |
| scikit-learn | train/test split and performance metrics | No (preinstalled) |
| matplotlib | plots | No (preinstalled) |
| seaborn | plots | No (preinstalled) |

**Platform:** Google Colab runs on Google's Linux servers, so the analysis works the same on any computer with a web browser. We used [Windows / macOS] with Google Chrome. If you run the notebook outside Colab (for example, in Jupyter), install the packages above with `pip install pandas numpy textstat statsmodels scikit-learn matplotlib seaborn`, and replace the `google.colab` upload/download lines with `pd.read_csv("path/to/file.csv")`.

## Section 2: Map of Documentation

```
DS-4002-Projects/
├── README.md                              # This file: orientation to the repository
├── LICENSE.md                             # MIT License
│
├── SCRIPTS/
│   └── ds_4002_project_1.ipynb            # Full analysis notebook, run top to bottom:
│                                          #   1. Upload cleaned data
│                                          #   2. Create variables (polarization, word count, grade level)
│                                          #   3. Exploratory data analysis and plots
│                                          #   4. Model development (regression, diagnostics)
│                                          #   5. Model testing (held-out data vs. baseline)
│                                          #   6. Save and download all outputs
│
├── DATA/
│   ├── README.md                          # Explanation to acquire original dataset
│   └── rotten_tomatoes_data_with_vars.csv # Cleaned data plus engineered variables
│
├── OUTPUT/
│   ├── eda_histograms.png                 # Distributions of word count and polarization
│   ├── eda_polarization_by_length.png     # Average polarization by review length
│   ├── eda_grade_level_hist.png           # Distribution of Flesch–Kincaid Grade Level
│   ├── summary_statistics.csv             # Descriptive statistics for key variables
│   ├── model_diagnostics.png              # Residual, Q-Q, observed vs. predicted and coefficient plots
│   ├── coefficient_table.csv              # Coefficients, robust SEs, t/p-values, 95% CIs, standardized betas
│   ├── vif_table.csv                      # Variance inflation factors (multicollinearity check)
│   ├── performance_table.csv              # Held-out performance vs. baseline (model development)
│   ├── model_testing_plots.png            # Held-out residuals, observed vs. predicted, error and R² comparisons
│   ├── performance.csv                    # R², MAE, RMSE, accuracy: model vs. baseline, training and held-out
│   ├── generalization.csv                 # Training vs. held-out comparison
│   ├── criteria.csv                       # Pass/fail check of each evaluation criterion in our plan
│   └── margin_table.csv                   # Held-out accuracy at ±0.05, ±0.10, ±0.25 and ±0.5
│
└── REFERENCES/
    └── [reference files]                  # Sources cited in the project
```

## Section 3: Instructions for Reproducing Results

These steps reproduce every figure and table in `OUTPUT/`. The full run takes about 5–10 minutes, most of it spent calculating readability scores.

### Step 1: Get the data
1. Open the `DATA/` folder in this repository.
2. Click `[cleaned_data_file].csv`, then click the **download icon** (↓) in the top right to save it to your computer.
   - If the file is not in the folder because it is too large for GitHub, download it from our Google Drive instead: [Google Drive link]
3. The cleaned file was created from the "Massive Rotten Tomatoes Movies & Reviews" dataset on Kaggle ([link]). Sample selection and inclusion criteria are described in `DATA/README.md`.

### Step 2: Open the notebook in Google Colab
1. Go to [colab.research.google.com](https://colab.research.google.com/) and sign in with a Google account.
2. Click **File → Open notebook**, choose the **GitHub** tab, paste this repository's URL (`https://github.com/camjanada/DS-4002-Projects`), and select `SCRIPTS/ds_4002_project_1.ipynb`.
   - Alternatively, download the notebook from `SCRIPTS/` and use **File → Upload notebook** in Colab.

### Step 3: Run the notebook
1. Click **Runtime → Run all**.
2. The first code cell will pause and show a **Choose Files** button. Click it and select the `[cleaned_data_file].csv` you downloaded in Step 1. The notebook continues once the upload finishes; a large file can take a minute or two to upload.
3. Let the remaining cells run in order. Do not skip cells or run them out of order, because the model testing cell reuses the model fit in the model development cell.
   - The notebook installs `textstat` automatically. If you see `No module named 'textstat'`, run `%pip install textstat` in a new cell, then run all again.
   - The notebook downloads `rotten_tomatoes_data_with_vars.csv` partway through. If your browser asks for permission to download files, click **Allow**.
4. The final cell saves every figure and table into an `OUTPUT` folder and downloads it as `OUTPUT.zip`.

### Step 4: Check your results
The data is split into 80% training and 20% held-out data using `random_state=42`, so your numbers should match ours:

| Result | Expected value |
|---|---|
| Training reviews | 96,497 |
| Held-out reviews | 24,125 |
| Regression equation | polarization = 0.2209 − 0.0065 × log(1 + word count) + 0.0019 × grade level − 0.0033 × top critic |
| Training R² | 0.00327 |
| Held-out R² | 0.00207 |
| Held-out MAE (model vs. baseline) | 0.11142 vs. 0.11167 |
| Held-out RMSE (model vs. baseline) | 0.13210 vs. 0.13223 |
| R² > 0.20 goal met | No |

Unzip `OUTPUT.zip` and compare its figures and tables with those in this repository's `OUTPUT/` folder. Very small differences in the last decimal places can occur if Colab installs a different version of `textstat`, since that changes grade-level scores slightly.

### Notes on the methods
- **Polarization** = |score/100 − 0.5|, ranging from 0 (a middle score) to 0.5 (the most extreme score).
- **Duplicates** are removed by `reviewId` before modeling.
- **Word count and grade level** are winsorized at the 1st and 99th percentiles to limit extreme values, and word count is log-transformed (`log(1 + word count)`).
- **Robust standard errors** (HC3) are used to account for unequal variance in the residuals.
- **The baseline** predicts the mean polarization of the *training* data for every review, so no information from the held-out data is used.
- **Results are associative only** and do not support a causal conclusion.

