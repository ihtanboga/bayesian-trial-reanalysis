# Bayesian Trial Reanalysis Tool

A single-page clinical trial workbench for **Bayesian re-analysis** of superiority and non-inferiority RCTs, based on the framework described by [Zampieri et al. (2021)](https://doi.org/10.1164/rccm.202006-2381CP).

**[Live Demo](https://ihtanboga.github.io/bayesian-trial-reanalysis/)**

![screenshot](https://img.shields.io/badge/status-stable-green) ![license](https://img.shields.io/badge/license-MIT-blue)

---

## What it does

When a clinical trial reports *"OR 0.85, 95% CI 0.70–1.03, p = 0.09 — not statistically significant"*, this tool answers the real question: **What is the actual probability that the treatment is beneficial or harmful?**

Instead of a binary yes/no, you get results like:

> *"There is an 88% probability that this treatment reduces mortality."*

## Features

- **Superiority & Non-Inferiority** trial designs
- **Multiple input formats**: OR, RR, HR with 95% CI; 2×2 event counts; log(OR) + SE
- **Zampieri 9-prior grid**: 3 beliefs (neutral/optimistic/pessimistic) × 3 strengths (weak/moderate/strong)
- **Custom prior support**: add your own prior from meta-analyses or expert opinion
- **NI-specific outputs**: P(Non-Inferiority), P(Superiority), NI margin visualization
- **Interactive plots**: posterior density, CDF, forest plot, heterogeneity assessment (Plotly.js)
- **Automatic interpretation**: narrative text summary with clinical thresholds
- **Prior sensitivity**: Bayesian I² heterogeneity with DuMouchel prior
- **Zero dependencies**: single HTML file, runs entirely in the browser

## Quick start

1. Open `index.html` in any browser (or use the [live demo](https://ihtanboga.github.io/bayesian-trial-reanalysis/))
2. Select study design (Superiority or Non-Inferiority)
3. Enter the reported effect measure (OR/RR/HR + 95% CI)
4. Choose outcome type (bad outcome = mortality; good outcome = survival)
5. Set the expected ratio from the study's sample size calculation
6. Select a prior scenario or customize individual priors
7. Click **Run Bayesian Re-Analysis**

## Example: ART Trial

The ART trial (2017) reported OR 1.27 (95% CI 0.99–1.63, p = 0.057) for 28-day mortality — *"not statistically significant."*

Bayesian re-analysis shows **>93% probability of harm** despite p = 0.057. Click "Load ART Example" in the tool to see this analysis.

## Method

The tool implements **conjugate normal–normal Bayesian updating** on the log-ratio scale:

1. **Likelihood**: log(ratio) ~ N(observed log-ratio, SE²), derived from the reported CI
2. **Prior**: N(μ₀, σ₀²), where μ₀ and σ₀ depend on the selected belief and strength
3. **Posterior**: N(μ₁, σ₁²), computed analytically via conjugate update

Prior construction follows **Table 2** of Zampieri et al.:

| Belief | Center | Interpretation |
|--------|--------|----------------|
| Neutral (Skeptical) | log(1.0) = 0 | No prior expectation |
| Optimistic | log(expected ratio) | Treatment works as planned |
| Pessimistic | −log(expected ratio) | Treatment causes harm |

| Strength | SD calibration | Data influence |
|----------|---------------|----------------|
| Weak | P(crossing null) = 30% | Data dominate |
| Moderate | P(crossing null) = 15% | Balanced |
| Strong | P(crossing null) = 5% | Prior dominates |

**Heterogeneity** across priors is assessed using a Bayesian random-effects model with a DuMouchel half-Cauchy prior on τ (Appendix 3, Zampieri et al.).

## Non-Inferiority analysis

For NI trials, the tool provides:

- **P(NI)**: probability that the ratio stays within the pre-specified NI margin
- **P(Superiority)**: probability that the treatment is actually superior (ratio < 1 for bad outcomes)
- NI margin visualization on density plots, CDF plots, and forest plots
- Support for both ratio-scale and absolute risk-difference NI margins

## Reference

Zampieri FG, Casey JD, Shankar-Hari M, Harrell FE Jr, Harhay MO. *Using Bayesian Methods to Augment the Interpretation of Critical Care Trials.* Am J Respir Crit Care Med. 2021;203(5):543-552. [doi:10.1164/rccm.202006-2381CP](https://doi.org/10.1164/rccm.202006-2381CP)

## License

MIT
