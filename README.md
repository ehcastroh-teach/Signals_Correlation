# Signals and Correlation

This repository teaches statistical correlation in Python from first principles. Starting with the Pearson formula derived by hand, it walks through computing correlation matrices with NumPy and Pandas, then compares Pearson, Spearman, and Kendall-tau methods so you know which to reach for when your data has outliers, tied ranks, or non-normal distributions.

## Learning Objectives

- Derive the Pearson correlation coefficient from covariance and explain what each term contributes
- Compute row-level and column-level correlation matrices using NumPy's `corrcoef`
- Run pairwise and single-pair correlation on Pandas DataFrames using `.corr()`
- Distinguish Pearson, Spearman, and Kendall-tau and select the appropriate method for a given dataset
- Recognize when a strong correlation does not imply causation

## Data / File Dictionary

| File | Type | Description |
|---|---|---|
| `nb_signals_correlation.ipynb` | Lesson notebook | Introduces correlation theory, NumPy and Pandas implementations, and all three correlation methods with a worked outlier comparison |
| `hw_signals_correlation.ipynb` | Homework notebook | Applied practice using environmental health data and the UCI Wine dataset - covers hand calculation, matrix reading, and method comparison |
| `requirements.txt` | Dependency list | Python package versions needed to run both notebooks |

## Workflow Diagram

```
nb_signals_correlation.ipynb     hw_signals_correlation.ipynb
      (lesson)           ---->         (homework)
  Theory + examples              Hand calc + method comparison
  Parts 0-4                      Sections 1-3
```

## Step-by-Step Walkthrough

1. **Start with the lesson notebook** (`nb_signals_correlation.ipynb`). Part 1 derives Pearson correlation step-by-step using only NumPy arithmetic before introducing any library functions - this prevents the formula from remaining a black box.

2. **Work through Parts 2 and 3** to see how NumPy and Pandas implement the same computation at different levels of abstraction. The key judgment to internalize: use `np.corrcoef` when working with raw arrays and matrix structure matters; use `df.corr()` when columns have names and you want readable output.

3. **Part 4 is the most important Part for practical data science** - it shows concretely how a single outlier can move the Pearson coefficient while Spearman and Kendall barely shift. Run the outlier comparison cell and examine which method you would trust on each dataset you encounter.

4. **After the lesson, open the homework** (`hw_signals_correlation.ipynb`). Section 1 forces you to compute correlation from scratch on an environmental health dataset - different data than the lesson, so you can't adapt the lesson's code directly.

5. **Section 2 covers theoretical properties** - True/False statements about the range and behavior of correlation coefficients. These are checkable with quick reasoning rather than code.

6. **Section 3 applies all three methods** to the UCI Wine dataset. The key question in Q4d is whether Pearson or Spearman shifts more when outliers are removed from the 'ash' column - run it both ways and explain the difference.

## How to Run

```bash
# Clone and enter the repo
git clone https://github.com/ehcastroh-teach/Signals_Correlation
cd Signals_Correlation

# Install dependencies (requires Python 3.10+)
pip install -r requirements.txt

# Launch JupyterLab
jupyter lab
```

Open `nb_signals_correlation.ipynb` first. Use **Kernel > Restart Kernel and Run All Cells** to verify a clean run before working through the homework.

## Key Concepts Glossary

- **Correlation** - a normalized measure of the linear (or monotonic) relationship between two variables, bounded in [-1, 1]
- **Pearson r** - standardized covariance; measures linear association; sensitive to outliers
- **Covariance** - the average of the product of deviations from the mean for two variables; not bounded, so scale-dependent
- **Spearman rho** - Pearson applied to ranks; measures monotonic association; robust to outliers and skew
- **Kendall tau** - fraction of concordant minus discordant pairs; preferred over Spearman on small samples with many tied ranks
- **Concordant pair** - two observations where the orderings of both variables agree
- **Discordant pair** - two observations where the orderings of both variables disagree
- **Monotonic relationship** - one variable consistently increases (or decreases) as the other increases, but not necessarily at a constant rate
- **Correlation matrix** - a square symmetric matrix where entry (i, j) is the correlation between variable i and variable j; diagonal entries are always 1

## Further Reading

- "The Elements of Statistical Learning" - covers correlation in the context of regression and variable selection
- NumPy documentation: `numpy.corrcoef`
- Pandas documentation: `DataFrame.corr`
- SciPy documentation: `scipy.stats.pearsonr`, `scipy.stats.spearmanr`, `scipy.stats.kendalltau`
- "Thinking Statistically" - accessible treatment of when statistical summaries mislead

## Credits and Acknowledgements

UCI Wine Recognition dataset from the UCI Machine Learning Repository. Environmental health dataset (ozone, temperature, humidity, deaths) drawn from classic epidemiology teaching examples.
---

## Contact

<div align="center">
  <img src="images/thumbnails/ehcastroh_teach_banner_flower.png" alt="ehcastroh" width="90" style="border-radius: 50%;" />

  <sub>ehcastroh</sub>

  <a href="https://github.com/ehcastroh">GitHub</a> · <a href="https://www.linkedin.com/in/ehcastroh/">LinkedIn</a>
</div>
