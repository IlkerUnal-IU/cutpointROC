# cutpointROC

> Optimal Cut-Off Points for Multi-Class ROC Analysis

[![R](https://img.shields.io/badge/R-%3E%3D4.0-blue.svg)](https://cran.r-project.org/)

**cutpointROC** implements the nine cut-off determination methods described in:

> Ünal, İ., Ünal, E., Sertdemir, Y., & Kobaner, M. (2025). Defining optimal
> cut-off points for multiple class ROC analysis: generalization of the Index
> of Union method. *Journal of Biopharmaceutical Statistics*.
> <https://doi.org/10.1080/10543406.2025.2528639>

---

## Methods implemented

| Function | Method | Reference |
|----------|--------|-----------|
| `gyi_cutoff()` | Generalized Youden Index (GYI) | Nakas et al. 2013 |
| `ayi_cutoff()` | Adjusted Youden Index (AYI) | Hua & Tian 2020 |
| `cp_cutoff()` | Closest-to-Perfection (CP) | Attwood et al. 2014 |
| `rcp_cutoff()` | Refined Closest-to-Perfection (rCP) | Hua & Tian 2020 |
| `qayi_cutoff()` | Quadratic Adjusted Youden Index (qAYI) | Hua & Tian 2020 |
| `mv_cutoff()` | Maximum Volume / Area (MV) | Liu 2012, Attwood et al. 2014 |
| `madet_cutoff()` | Maximum Absolute Determinant (MADET) | Dong et al. 2017 |
| `giu_cutoff()` | Generalized Index of Union (GIU) | Unal et al. 2025 |
| `agiu_cutoff()` | Adjusted Generalized Index of Union (AGIU) | Unal et al. 2025 |

All methods support K ≥ 2 classes (including binary classification as a special case).

---

## Installation

```r
# From a local source directory:
install.packages("path/to/cutpointROC", repos = NULL, type = "source")

# Or with devtools:
# devtools::install("path/to/cutpointROC")
```

---

## Quick start

```r
library(cutpointROC)

# Simulate three-class data
set.seed(42)
dat <- simulate_roc_data(n = c(100, 100, 100), scenario = "normal_06")

# Run all nine methods
out <- all_cutoffs(dat$marker, dat$response,
                   true_cutoffs = dat$true_cutoffs)
print(out)

# Single method
res <- giu_cutoff(dat$marker, dat$response)
print(res)
plot_spm(res)

# Visualise comparisons
plot_comparison(out, metric = "TCCR")
plot_comparison(out, metric = "MMRDIF")
plot_tcr(out)
```

---

## Performance metrics

| Metric | Function | Description |
|--------|----------|-------------|
| ABSDIFF | `absdiff()` | Sum of \|estimated − true\| cut-offs |
| TCCR | `tccr()` | Total correct classification rate |
| LTCCR | `ltccr()` | % loss vs. true cut-offs |
| MMRDIF | `mmrdif()` | Max–min relative difference (balance) |

---

## Key concepts

### Squared Probability Matrix (SPM)

The K × K matrix **P** where `P[i,j] = P(T=j | D=i)` — the probability that
a subject truly in class *i* is classified as class *j*. Diagonal entries are
correct classification rates; off-diagonal entries are misclassification rates.

```r
P <- compute_spm(marker, response, cutoffs = c(11.4, 13.2))
```

### HUM / VUS / AUC

```r
compute_auc(marker, response)        # K = 2
compute_vus(marker, response)        # K = 3
compute_hum(marker, response, n_mc = 5000)  # any K
```

---

## Simulation study

Replicate the paper's simulation setup:

```r
results <- run_simulation(
  scenarios = c("normal_04", "normal_06", "gamma_06", "mixture_06"),
  n         = c(100, 100, 100),
  n_rep     = 100,
  seed      = 42
)
print(results[["mixture_06"]])
```

Built-in scenarios: `normal_04`, `normal_06`, `lognormal_04`, `lognormal_06`,
`gamma_04`, `gamma_06`, `mixture_06`.

---

## Citation

If you use this package, please cite:

```
Ünal, İ., Ünal, E., Sertdemir, Y., & Kobaner, M. (2025). Defining optimal
cut-off points for multiple class ROC analysis: generalization of the Index
of Union method. Journal of Biopharmaceutical Statistics.
https://doi.org/10.1080/10543406.2025.2528639
```

---

## License

GPL-3
