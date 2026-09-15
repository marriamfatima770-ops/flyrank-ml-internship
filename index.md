# Capstone Report — Ranking Signal Analysis

- **Author:** Marriam Fatima
- **Lane:** Ranking Signal Analysis
- **Repo:** FlyRank ML Internship repository
- **Date:** September 2026

## 0. Abstract

This study examines which safe search-performance signals are associated with click-through rate (CTR) in the FlyRank ML Internship dataset. The analysis uses 30,000 page-level observations and focuses on measurable content and search-related signals. Exploratory analysis was combined with a Random Forest regression model and a simple median-CTR baseline. The baseline achieved an MAE of 0.511, while the Random Forest achieved an MAE of 0.714, so the baseline performed better on the validation split. The output is therefore used as directional, decision-support evidence for prioritizing content signals for further review rather than as a guaranteed prediction of search performance.

## 1. Problem framing

The decision supported by this work is which measurable search-performance signals should receive priority during content and SEO review.

The unit of analysis is the individual content item/page. The main output is a ranked set of signals based on exploratory evidence and model feature importance.

Content and SEO teams can use these findings to identify pages or characteristics that deserve further investigation and optimization.

A wrong recommendation could waste time and resources on low-potential pages while higher-potential opportunities are overlooked.

Machine learning helps provide a consistent way to compare several measurable signals, but the results remain decision-support evidence and do not replace human judgment.

## 2. Data safety

This study uses the anonymized FlyRank ML Internship dataset containing 30,000 rows and 44 columns.

The analysis focuses on safe, aggregated page-level information. Pseudonymous content and client IDs were not used as model features. They are treated only as identifiers for grouping or validation purposes.

The selected model features were:

- search_volume
- competition
- cpc
- word_count
- char_count
- content_age_days
- days_since_last_update

CTR was used as the target.

Fields that could create leakage or ambiguous interpretation, including clicks, impressions, trend_direction, trend_pct, and other target-derived performance fields, were not used as predictive features.

No client names, domains, URLs, private search queries, credentials, or identifying client information are included in the paper.

## 3. Baseline

The baseline predicts the median CTR of the training data for every validation observation.

This is a transparent and simple comparison because it requires no learned relationship between the input features and CTR.

Using the same validation data and metric:

- **Baseline MAE:** 0.511
- **Random Forest MAE:** 0.714

Lower MAE indicates better performance, so the baseline performed better than the current Random Forest model.

## 4. Model / analysis

A Random Forest regression model was used to examine whether the selected content and search-related features could provide useful CTR predictions.

The feature list was:

- search_volume
- competition
- cpc
- word_count
- char_count
- content_age_days
- days_since_last_update

Missing numeric values were handled using median imputation.

CTR was the target variable.

The model used 100 trees with a fixed random seed of 42.

Features such as clicks, impressions, trend_direction, and trend_pct were excluded because they can directly reflect or be derived from search-performance outcomes and could create leakage or unclear predictive timing.

## 5. Evaluation

The dataset was divided into training and validation sets using an 80/20 split with random_state=42.

A temporal split was not used because the starter dataset does not provide a suitable explicit date field for a reliable time-aware evaluation.

The Random Forest and baseline were evaluated on exactly the same validation set using mean absolute error (MAE).

The results were:

| Approach | MAE |
|---|---:|
| Median baseline | 0.511 |
| Random Forest | 0.714 |

The baseline performed better. This negative result is important because it shows that the selected features and model did not provide useful predictive improvement over a simple reference rule.

## 6. Interpretation

The Random Forest feature importance results identified the following signals:

| Signal | Model importance |
|---|---:|
| Word count | 40.5% |
| Character count | 37.4% |
| Content age | 15.2% |
| Days since last update | 4.8% |
| Search volume | 1.1% |
| Competition | 0.5% |
| CPC | 0.4% |

Word count and character count were the strongest model-identified signals, followed by content age.

These values describe the behavior of this particular model on this dataset. They should not be interpreted as confirmed Google ranking factors.

The most important negative result is that the Random Forest did not outperform the simple baseline. Therefore, the current model should not be presented as a successful CTR prediction system.

The findings are best treated as observed and directional evidence for further content investigation.

## 7. Recommendation

The analysis supports the following ranked actions:

1. **Review content depth and completeness**  
   Word count was the strongest model-identified signal. Content teams can review whether important pages have sufficient depth and coverage.

2. **Review content length and structure**  
   Character count was also strongly represented in the model. Editors can review whether page content is appropriately structured and complete.

3. **Review older content**  
   Content age showed meaningful model importance. Older pages can be reviewed for possible improvement opportunities.

4. **Review recently unchanged content**  
   Days since last update showed smaller model importance and can be used as a secondary review signal.

These recommendations are decision-support actions, not guarantees of improved rankings or CTR.

Confidence should remain moderate-to-low because the Random Forest did not outperform the baseline and the dataset is observational.

## 8. Reproducibility

The analysis was developed in Python using pandas, scikit-learn, and matplotlib.

The main notebook is:

`work/notebooks/capstone.ipynb`

The dataset used by the notebook is:

`data/raw/content_refresh_anonymized.csv`

The main model settings were:

- Train/test split: 80/20
- random_state: 42
- Random Forest trees: 100
- Missing-value strategy: median imputation
- Evaluation metric: MAE

Required Python packages can be installed with:

```bash
pip install pandas scikit-learn matplotlib
```

## 9. Acknowledgments & Data Credit

Built on the FlyRank ML Internship dataset. The analysis follows the public-safety and anonymization rules provided with the internship dataset.
