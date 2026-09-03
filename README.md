# Signals and Correlation

This repository teaches statistical correlation in Python from first principles. It begins by deriving the Pearson formula by hand from covariance, then builds up to computing full correlation matrices with NumPy and Pandas, and finally compares Pearson, Spearman, and Kendall-tau side by side so you know which method to reach for when your data has outliers, tied ranks, or non-normal distributions. A paired lesson-and-homework structure reinforces each concept: the lesson notebook introduces theory with worked examples, and the homework notebook applies those tools to environmental health data and the UCI Wine Recognition dataset.

---

## Learning Objectives

- Derive the Pearson correlation coefficient from covariance and explain what each term contributes to the formula
- Compute row-level and column-level correlation matrices using `np.corrcoef` and the `rowvar` parameter
- Run full pairwise and single-pair correlation on Pandas DataFrames using `.corr()` with interchangeable method arguments
- Distinguish Pearson, Spearman, and Kendall-tau and select the appropriate method for a given dataset's shape and outlier profile
- Quantify how a single extreme value shifts Pearson while leaving Spearman and Kendall nearly unchanged
- Recognize when a strong correlation does not imply causation

---

## Data / File Dictionary

| File | Type | Description |
|---|---|---|
| `nb_signals_correlation.ipynb` | Lesson notebook | Introduces correlation theory (Parts 0-4): hand-derived Pearson, NumPy matrix correlation, Pandas pairwise correlation on the mtcars subset, and a three-method outlier comparison using SciPy |
| `hw_signals_correlation.ipynb` | Homework notebook | Applied practice (Sections 1-3): hand calculation and DataFrame correlation on an environmental health dataset, True/False theoretical questions on correlation properties, and a three-method comparison on the UCI Wine Recognition dataset including outlier robustness analysis |
| `requirements.txt` | Dependency list | Python package versions needed to run both notebooks (NumPy, Pandas, Matplotlib, SciPy, scikit-learn, JupyterLab) |

---

## Workflow Diagram

```
nb_signals_correlation.ipynb          hw_signals_correlation.ipynb
        (lesson)          --------->          (homework)
  Part 0: Setup                         Section 1: Basics
  Part 1: Pearson by hand     apply     Section 2: Theory (True/False)
  Part 2: NumPy matrices      ----->    Section 3: Method comparison
  Part 3: Pandas DataFrames             (UCI Wine + outlier analysis)
  Part 4: Pearson vs Spearman
          vs Kendall + outliers
```

---

## Step-by-Step Walkthrough

**1. Part 1 - Derive Pearson before touching any library function.**
The lesson notebook opens by computing Pearson correlation step-by-step using only NumPy arithmetic: subtract the mean, multiply the deviations, divide by the product of standard deviations. Walking through this once makes the formula concrete rather than a black box, and the result is then verified against `np.corrcoef` to confirm both paths agree. The concept check that follows asks you to interpret the sign and magnitude of a negative correlation - practice that pays off in every later section.

**2. Part 2 - Understand the row/column orientation of `np.corrcoef`.**
NumPy treats each row of the input as a separate variable by default, which is the opposite of how data is usually organized (columns as features). Part 2 demonstrates both orientations on the same 4x5 array - first correlating rows directly, then transposing to correlate columns - and shows that passing `rowvar=False` is an explicit, readable alternative to the transpose. Internalizing this distinction prevents a common source of silent errors in matrix-level correlation work.

**3. Part 3 - Switch to Pandas for named columns and readable output.**
When variable names matter (which they do in any real analysis), Pandas `.corr()` is the right tool. Part 3 uses a six-car, six-feature subset of the mtcars dataset to show how `df.corr()` produces a labeled correlation matrix in one call, and how `df['mpg'].corr(df['hp'], method='spearman')` lets you switch methods without rebuilding anything. The note on pairwise-complete cases explains why effective sample sizes can differ across pairs in a DataFrame with missing values - a detail that matters when your data is not fully dense.

**4. Part 4 - See the outlier sensitivity difference with your own eyes.**
The most practically important section. Part 4 compares Pearson, Spearman, and Kendall-tau on the same pair of arrays using `scipy.stats`, then injects a single extreme outlier and runs all three again. The Pearson coefficient moves substantially because it operates on raw values; Spearman and Kendall barely shift because ranks compress extreme values. Running this cell yourself - not just reading the output - is the fastest way to build genuine intuition for when to distrust Pearson.

**5. Section 1 of the homework - Apply the hand-calculation process to new data.**
The homework's environmental health dataset (ozone, temperature, humidity, deaths) is different from anything in the lesson, so adapting lesson code directly will not work. Section 1 forces you to reapply the derivation: compute mean, standard deviation, and covariance explicitly before calculating the correlation coefficient. Question 1d then asks you to build a Pandas DataFrame from the four arrays and find the strongest linear relationship in the full correlation matrix - practice at reading a real matrix, not just a toy one.

**6. Section 2 of the homework - Check your theoretical understanding.**
The True/False questions in Section 2 cover the [-1, 1] bound of correlation, what it means for the bound to be tight (perfect correlation), and which method each definition describes. These are checkable by reasoning from the definitions, not by running code - which is exactly the point. Question 3e asks for a LaTeX derivation showing that the bound on correlation implies a bound on covariance; working through it cements why the two quantities are related but not interchangeable.

**7. Section 3 of the homework - Compare methods on real-world data.**
Section 3 uses the UCI Wine Recognition dataset (178 wines, 13 chemical features, 3 cultivars). Questions 4a through 4c compute all three correlation types between `alcohol` and `color_intensity` and ask you to interpret any gap between Pearson and Spearman. Question 4d is the key exercise: filter out high outliers in the `ash` column and compare how much Pearson shifts versus Spearman. If Pearson moves more than Spearman when outliers are removed, that is direct evidence the raw-value method was being pulled by extreme observations.

---

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

Open `nb_signals_correlation.ipynb` first. Use **Kernel > Restart Kernel and Run All Cells** to verify a clean run before beginning the homework. Then open `hw_signals_correlation.ipynb` and work through Sections 1 to 3 in order - each section builds on vocabulary introduced in the previous one.

---

## Key Concepts Glossary

- **Correlation** - a normalized measure of the linear or monotonic relationship between two variables, always bounded in [-1, 1]
- **Pearson r** - standardized covariance; measures linear association; sensitive to outliers because it operates on raw values rather than ranks
- **Covariance** - the average product of mean-centered deviations for two variables; not bounded and scale-dependent, which is why dividing by standard deviations is necessary to produce a comparable coefficient
- **Standard deviation** - the square root of the average squared deviation from the mean; used in Pearson's denominator to remove unit dependency from the covariance
- **Spearman rho** - Pearson applied to the ranks of each variable rather than their raw values; measures monotonic association; robust to outliers and skew because ranking compresses extreme values
- **Kendall tau** - the fraction of concordant pairs minus the fraction of discordant pairs, normalized to [-1, 1]; preferred over Spearman on small samples with many tied ranks because its sampling distribution is better understood in those conditions
- **Concordant pair** - two observations where the orderings of both variables agree (both $x_i > x_j$ and $y_i > y_j$, or both less)
- **Discordant pair** - two observations where the orderings of both variables disagree
- **Monotonic relationship** - one variable consistently increases or decreases as the other increases, but not necessarily at a constant rate; Spearman and Kendall detect monotonic relationships that Pearson may miss
- **Correlation matrix** - a square symmetric matrix where entry (i, j) is the correlation between variable i and variable j; diagonal entries are always 1 because every variable is perfectly correlated with itself
- **rowvar parameter** - the `np.corrcoef` argument controlling whether rows or columns are treated as variables; defaults to `True` (rows are variables), which is opposite to the typical column-as-feature convention in tabular data
- **Outlier sensitivity** - the degree to which a single extreme observation can shift a statistic; Pearson is highly sensitive because squaring deviations magnifies extreme values, while rank-based methods are not because the rank of an outlier is bounded by the sample size

---

## Further Reading

- "The Elements of Statistical Learning" - covers correlation in the context of regression and variable selection
- "Thinking Statistically" - accessible treatment of when statistical summaries mislead
- NumPy documentation: `numpy.corrcoef`
- Pandas documentation: `DataFrame.corr`
- SciPy documentation: `scipy.stats.pearsonr`, `scipy.stats.spearmanr`, `scipy.stats.kendalltau`
- UCI Machine Learning Repository: Wine Recognition Dataset

---

## Credits and Acknowledgements

UCI Wine Recognition dataset from the UCI Machine Learning Repository. Environmental health dataset (ozone, temperature, humidity, deaths) drawn from classic epidemiology teaching examples.

---

## Contact

<div align="center">
  <img src="images/thumbnails/ehcastroh_teach_banner_flower.png" alt="ehcastroh" width="90" style="border-radius: 50%;" />

  <sub>ehcastroh</sub>

  <a href="https://github.com/ehcastroh">GitHub</a> · <a href="https://www.linkedin.com/in/ehcastroh/">LinkedIn</a>
</div>
