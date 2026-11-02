# Perceived employability of senior executives

**[Open the interactive dashboard →](https://sudhanshudubey.github.io/ML_class_project/)**

**Python · Exploratory analysis · Linear regression · Logistic regression · Interactive dashboard**

Individual Introduction to Machine Learning course project at emlyon business school, by **Sudhanshu Dubey**. I studied which characteristics are associated with confidence in finding another job, and whether a logistic regression can identify current job seekers.

The project demonstrates data preparation, statistical interpretation and model evaluation, including recognising when a model performs poorly. The public version includes my analysis code and an interactive presentation of aggregate results. The restricted survey dataset and saved notebook outputs are excluded.

## Explore the project

- [Analysis notebook](notebook/Sudhanshu_ML.ipynb): the original analysis code and written observations, with outputs cleared for publication.
- [Dashboard source](index.html): download or clone the repository and open `index.html` in a browser. It is a self-contained interactive page with no server, external scripts or dataset required.
- [Data notice](DATA_NOTICE.md): source attribution and restrictions.

The dashboard has four views: study overview, self-efficacy regression, job-seeker model evaluation and study notes. Its controls explore fixed aggregate results; they do not refit models or make individual predictions.

## Questions and approach

1. **What is associated with professional self-efficacy?** Describe the respondents and twelve employability scores, examine correlations and bivariate relationships, then fit linear regressions using scores alone and scores with personal variables.
2. **Who are the job seekers?** Fit logistic regression and compare validation accuracy with an always-Employed baseline. Inspect the confusion matrix, minority-class recall and odds ratios.

The analysis uses pandas, NumPy, Matplotlib, seaborn, SciPy, pingouin and scikit-learn. It follows the methods covered in the course: frequency tables, histograms, boxplots, Pearson correlations, chi-square with Cramér’s V, ANOVA with η², dummy encoding, linear regression and logistic regression.

Preparation excludes the respondent identifier, groups the two smallest sectors with Other and groups salary into below 60k, 60–99k and above 99k. Categorical predictors use dummy encoding with a reference category. The relative dataset path is preserved.

## Main findings

The survey contains 460 senior executives and twelve employability dimensions measured on a 1–7 scale. Median competence is 6.33, compared with median self-efficacy of 3.33: feeling capable in a current role and feeling confident about finding another job describe different perceptions.

| Linear regression | Predictors | Fitted RMSE | R² | Adjusted R² |
| --- | ---: | ---: | ---: | ---: |
| Scores only | 11 | 1.1373 | 0.4018 | 0.3871 |
| Scores + personal variables | 31 | 1.0785 | 0.4620 | 0.4230 |

Flexibility and value awareness have the largest absolute standardised coefficients in the full model (0.373 and 0.322). These are associations conditional on the other included predictors. Correlated scores limit the interpretation of a predictor ranking. These metrics describe fit to the observed sample, rather than performance on unseen respondents.

The logistic regression uses a stratified 80/20 training–validation split with random seed 42. The validation set contains 79 employed respondents and 13 job seekers.

| Validation measure | Result |
| --- | ---: |
| Model accuracy | 82.61% |
| Always-Employed baseline accuracy | 85.87% |
| Job-seeker recall | 7.69% (1 of 13) |

| Actual / Predicted | Employed | Job seeker |
| --- | ---: | ---: |
| Employed | 75 | 4 |
| Job seeker | 12 | 1 |

The model underperforms the accuracy baseline and misses most job seekers at the default 0.50 cutoff. It is therefore not useful as a job-seeker screening tool on this validation split. This is a central finding of the project, rather than evidence of a successful classifier.

## Interpretation and limits

The survey describes perceived employability in this sample. It does not establish causal effects or represent every senior executive. Rare categories and the small number of validation job seekers limit the stability of some estimates. Training and networking may be responses to a job search; the data cannot establish the direction of that relationship.

For an HR or outplacement audience, the results support discussing transferable skills, flexibility and confidence separately from competence. They do not demonstrate that a particular intervention will improve employment outcomes or justify using this classifier to make decisions about people.

## Reproducing the notebook

**The dataset is not publicly distributed.** The public repository can be read and the dashboard opened without it; rerunning the analysis requires authorised access to the original course dataset. No synthetic data or substitute results are supplied.

Install packages from the repository root:

```bash
python -m pip install -r requirements.txt
```

If you already have permission to use the original `employability.csv`, place it in `data/`. Open `notebook/Sudhanshu_ML.ipynb` in JupyterLab with `notebook/` as the kernel working directory and run the cells in order. The loading cell remains:

```python
from pathlib import Path

DATA_DIR = Path('../data')
df = pd.read_csv(DATA_DIR / 'employability.csv')
```

| Path | Contents |
| --- | --- |
| `notebook/Sudhanshu_ML.ipynb` | Public copy of the analysis notebook; saved outputs cleared. |
| `data/README.md` | Instructions only; no survey data. |
| `index.html` | Interactive aggregate-results dashboard. |
| `requirements.txt` | Notebook and JupyterLab dependencies. |
| `DATA_NOTICE.md` | Source and data restrictions. |

The original course notebook remains unchanged outside this public copy. Lecture slides, course instructions, respondent-level data and the submission report are not included. Dataset file formats are excluded by `.gitignore` to help prevent accidental publication.
