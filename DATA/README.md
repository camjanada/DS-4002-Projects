# Data

This folder contains the data used in **Relationship between Movie Review Complexity and Polarization** (The Pink Tigers — DS 4002, Project 1).

| File | Description |
|---|---|
| `rotten_tomatoes_data_with_vars.csv` | The cleaned sample plus the engineered variables used in modeling (polarization, word count, grade level) |

## Data Summary

The data come from the **Massive Rotten Tomatoes Movies & Reviews** dataset, which is hosted publicly on Kaggle. It contains information on more than 140,000 movies and over 1.4 million professional critic reviews, scraped from Rotten Tomatoes on April 12, 2023.

The dataset is organized into two CSV files, which can be linked through the movie `id`:

- **`rotten_tomatoes_movies.csv`** contains movie-level attributes, such as title, genre, director, runtime, theatrical and streaming release dates, box-office revenue, audience score and Tomatometer score.
- **`rotten_tomatoes_movie_reviews.csv`** contains review-level information, including the associated movie ID, critic name, publication, review text, original score, sentiment, review date and whether the reviewer is a top critic.

We did not use every observation because our project focuses on movie reviews from the post-COVID era. We define this as any review published from **January 1, 2021** through the date the data were scraped (April 12, 2023), which gives approximately 145,000 reviews. After removing rows with missing review text or scores, standardizing scores and removing duplicate reviews, **120,622 reviews** were used for modeling.

The dataset can be downloaded directly from Kaggle using the **Download** button or retrieved through the Kaggle API
## Provenance

The dataset was published on Kaggle in 2023 by the user **andrezaza** under the title "Massive Rotten Tomatoes Movies & Reviews." It is sourced from Rotten Tomatoes, a movie review website where critics' ratings and written reviews are submitted and published.

The dataset includes the original rating score given by each critic, the text of the review and the score sentiment (labeled "Positive" or "Negative"). These are the variables most relevant to our analysis. We accessed the dataset through Kaggle and selected the review information relevant to our research question.

Because this is a secondary data source, the original collection and processing procedures used to compile the Rotten Tomatoes information are not fully documented in the Kaggle dataset description.

## License

The dataset is published on Kaggle under a **CC0: Public Domain** license. Under CC0, the creator gives up their copyright and related rights in the dataset as fully as the law allows, so that others can use the data freely. The license allows the public to build on, modify, reuse and redistribute the work for essentially any purpose, including a project like this one.

## Ethical Statement

The reviews on Rotten Tomatoes do not reflect the opinions of everyone who watches movies. The critic reviews in this dataset do not even reflect the views of everyone who reviews movies on Rotten Tomatoes, since audience reviews are not included. Our findings therefore may not generalize to the opinions of all moviegoers or reviewers.

In addition, word count and Flesch–Kincaid Grade Level do not perfectly capture the complexity, thoughtfulness, intelligence or quality of a review. Any findings from this dataset should be interpreted as **associative, not causal**.

## Data Dictionary

### Original variables (`rotten_tomatoes_movie_reviews.csv`)

| Feature | Description | Type / Uncertainty |
|---|---|---|
| `id` | Unique identifier for each movie (matches `id` in `rotten_tomatoes_movies.csv`) | String/categorical identifier; not a measurement and should not be treated numerically |
| `reviewId` | Unique identifier for each critic review | Identifier; should not be treated as a quantitative variable |
| `creationDate` | Date the review was published | Date; the dataset was scraped in 2023, and older reviews may contain inconsistencies in historical metadata |
| `criticName` | Name of the critic who wrote the review | Categorical text; critics may appear multiple times |
| `isTopCritic` | Whether the critic is designated a Rotten Tomatoes top critic | Boolean (True/False); a Rotten Tomatoes classification rather than an objective measure of quality |
| `originalScore` | Score given by the critic | Mixed grading systems (mostly scores out of 5 or 10, and letter grades with +/−); required cleaning and standardization |
| `reviewState` | Status of the review | Binary categorical (fresh/rotten) |
| `publicationName` | Name of the publication where the review appeared | Categorical text; publications may differ considerably in style, audience and scoring practice |
| `reviewText` | Text of the critic's review | Unstructured text; varies in length, and some entries are excerpts rather than full reviews |
| `scoreSentiment` | Sentiment of the critic's score | Binary categorical (Positive/Negative) |
| `reviewUrl` | URL of the original review | Text/URL |

### Engineered variables (`rotten_tomatoes_data_with_vars.csv`)

| Feature | Description | Type / Uncertainty |
|---|---|---|
| `standardizedScore` | `originalScore` converted to a common 0–100 scale | Float; conversion of letter grades and differing scales involves judgment calls |
| `normalized_score` | `standardizedScore` / 100 | Float, 0–1 |
| `polarization` | Distance of `normalized_score` from the scale midpoint: \|normalized_score − 0.5\| | Float, 0 (middle score) to 0.5 (most extreme score); **response variable**. Takes a limited set of values because critics use discrete rating scales |
| `word_count` | Number of words in `reviewText` | Integer; **predictor** (review length). Many entries are excerpts, so length may understate the full review |
| `grade_level` | Flesch–Kincaid Grade Level of `reviewText`, calculated with the `textstat` package | Float; **predictor** (readability). Can be negative or extreme for very short texts, so it was winsorized at the 1st/99th percentiles for modeling |

## Explanatory Plots

### Distribution of review length and polarization

![Histograms of word count and polarization](../DATA/EDA1)

Reviews are short, with a median length of 24 words. Polarization values are concentrated just under 0.1 and above 0.3, and their clustering at specific values reflects the discrete rating scales critics use.

### Average polarization by review length

![Average polarization by review length](../DATA/EDA2)

Average polarization stays around 0.2 across review lengths, suggesting at most a weak relationship between how long a review is and how polarized its score is.


### Additional exploratory finding: Top Critics vs. Other Critics

We also explored whether Top Critics and Other Critics differ in how polarized their scores are across review lengths. Other Critics were consistently more polarized than Top Critics across nearly all review lengths, although the relationship was not perfectly linear. Polarization dipped for medium-length reviews and rose again for longer ones, especially among Other Critics. This suggests critic type may matter when examining the relationship between review length and polarization, which is why top-critic status is included as a control in our model.

## References

[1] andrezaza, "Massive Rotten Tomatoes Movies & Reviews," Kaggle, 2023. [Kaggle link]
