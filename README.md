# Loan Default Risk Segmentation

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Notebook-Jupyter-F37626?logo=jupyter&logoColor=white)
![scikit--learn](https://img.shields.io/badge/scikit--learn-clustering-F7931E?logo=scikit-learn&logoColor=white)
![License](https://img.shields.io/badge/license-GPL--3.0-green)

> An exploratory clustering study of peer-to-peer loan applications and their default-risk profiles.

This project applies unsupervised learning to a dataset of **13,794 loan applications**. The analysis groups borrowers according to loan amount, annual income, debt-to-income ratio, and FICO score, then compares the resulting segments with the observed default outcome.

The goal is not to train a supervised default classifier. Instead, the project investigates whether naturally occurring borrower segments reveal meaningful differences in default rates.

## Results at a glance

- **Three clusters** were selected as a practical compromise between the elbow curve, Davies–Bouldin index, and silhouette coefficient.
- **Min–max scaling** was applied before K-Means because the input variables have substantially different units and ranges.
- The clusters are primarily separated by **loan amount** and **annual income**; debt-to-income ratio and FICO score provide additional context.
- The observed default rate varies across segments, but the unsupervised clusters should be interpreted as descriptive profiles rather than causal or predictive risk categories.
- **PCA** is used to visualise the four-dimensional feature space in two dimensions; clustering itself is evaluated both before and after dimensionality reduction.

## Analysis workflow

1. Load and inspect the loan dataset.
2. Analyse numerical distributions, correlations, and categorical frequencies.
3. Select the four numerical variables used for distance-based clustering.
4. Rescale the features with `MinMaxScaler`.
5. Evaluate candidate values of $k$ from 2 to 9 using:
   - the elbow/inertia curve,
   - the Davies–Bouldin index,
   - the silhouette coefficient.
6. Fit K-Means with the selected number of clusters.
7. Describe each segment in the original feature scale.
8. Compare observed default proportions across clusters.
9. Use PCA for visual exploration of the resulting segmentation.

## Repository contents

| File | Description |
| --- | --- |
| [`loan-default-clustering.ipynb`](loan-default-clustering.ipynb) | Complete exploratory analysis and clustering study |
| [`prestamos.csv`](prestamos.csv) | Loan application dataset |
| [`LICENSE`](LICENSE) | Project license |

## Dataset

The dataset contains peer-to-peer loan applications with the following relevant fields:

| Feature | Meaning |
| --- | --- |
| `loan_amnt` | Requested loan amount in US dollars |
| `purpose` | Purpose of the loan |
| `revenue` | Applicant's annual income |
| `dti_n` | Debt-to-income ratio |
| `fico_n` | FICO credit score |
| `home_ownership_n` | Housing situation |
| `emp_length_n` | Employment-length category |
| `Default` | Observed repayment outcome used for post-hoc analysis |

The clustering model uses only `loan_amnt`, `revenue`, `dti_n`, and `fico_n`. Categorical variables and `Default` are retained for interpretation and validation after the unsupervised fit.

## Getting started

```bash
git clone https://github.com/sergiio8/Clustering.git
cd Clustering
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
jupyter notebook loan-default-clustering.ipynb
```

The notebook uses fixed random states for the K-Means experiments so that the reported clusters and visualisations are reproducible.

## Interpretation notes

Clustering is exploratory and unsupervised: the `Default` column is not used to create the clusters. Comparing default proportions afterwards is useful for interpretation, but it does not establish that cluster membership causes repayment behaviour or that the segments are suitable for automated lending decisions.

## Contributors

- **Sergio Martínez Olivera**
- **Daniel Roldán Serrano** — [@danirold](https://github.com/danirold)

## License

This project is distributed under the [GNU General Public License v3.0](LICENSE).
