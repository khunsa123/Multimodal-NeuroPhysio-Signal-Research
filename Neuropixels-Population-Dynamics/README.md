# Neuropixels Population Dynamics: Choice-Dependent Neural Manifolds, Networks, and Persistent Hubs

## Overview

This project is a Google Colab/Python analysis pipeline for investigating how population-level neural activity, behavioral choice representations, functional connectivity, network organization, and choice-dependent neuronal hubs evolve across time in Neuropixels recordings.

The notebook uses the **Steinmetz et al. (2019) Neuropixels dataset** and analyzes the session:

> **`Cori_2016-12-14`**

The analysis progresses from raw spike and behavioral data to:

1. quality-controlled neuronal population activity,
2. low-dimensional neural state-space representations,
3. choice-dependent neural trajectories,
4. time-resolved behavioral choice decoding,
5. UMAP population manifolds,
6. functional connectivity networks,
7. network density and modularity,
8. functional hub identification and turnover,
9. choice-dependent hub modulation,
10. temporal persistence of choice-dependent neurons, and
11. an integrated statistical assessment of persistent choice-dependent neuronal populations.

The final analytical endpoint is **Step 90B — Cross-Choice Persistence Evidence Comparison & Final Statistical Interpretation**.

---

## Scientific Question

The central scientific question is:

> **How does neural population activity and functional network organization encode behavioral choice across time, and are there neurons whose choice-dependent functional role persists from the early to the late response period?**

The persistence analysis specifically asks whether neurons that are strongly choice-dependent in the **early response epoch** remain strongly choice-dependent in the **late response epoch**, and whether this persistent subset shows convergent evidence of functional importance.

The pipeline therefore distinguishes:

```text
Early choice-dependent neurons
            │
            ├───────────────┐
            │               │
            ▼               ▼
       Persistent        Early-only
       neurons           neurons
            ▲
            │
       Late-only
       neurons
            │
            ▼
Late choice-dependent neurons
```

The persistence analysis is performed independently for **Choice +1** and **Choice -1**.

---

# Dataset

## Source

The notebook downloads the official **Steinmetz et al. (2019) Neuropixels dataset** from Figshare:

- Dataset: [Steinmetz et al. 2019 Neuropixels](https://figshare.com/articles/dataset/Steinmetz_et_al_2019_Neuropixels/9598406)
- Download endpoint used by the notebook: `https://ndownloader.figshare.com/files/17387882`

The notebook first downloads the archive, inspects it without fully extracting the large dataset, locates the required session, and extracts only the files needed for the analysis.

## Analyzed session

The pipeline specifically extracts and analyzes:

```text
Cori_2016-12-14
```

The session contributes both neural spike data and behavioral trial information.

## Required neural files

```text
spikes.times.npy
spikes.clusters.npy
spikes.amps.npy
spikes.depths.npy
```

## Required behavioral files

```text
trials.response_choice.npy
trials.visualStim_contrastLeft.npy
trials.visualStim_contrastRight.npy
trials.visualStim_times.npy
trials.feedbackType.npy
trials.goCue_times.npy
trials.response_times.npy
```

The notebook also extracts:

```text
channels.brainLocation.tsv
```

for channel/brain-location information.

---

# Repository Structure

The recommended layout for the existing repository is:

```text
existing-repository/
│
└── neuropixels_population_dynamics/
    │
    ├── README.md
    │
    └── notebooks/
        └── neuropixels_population_dynamics.ipynb
```

The uploaded notebook should therefore be placed at:

```text
neuropixels_population_dynamics/notebooks/
```

The README should sit one level above the notebook.

A larger project can later be expanded to:

```text
neuropixels_population_dynamics/
│
├── README.md
├── notebooks/
│   └── neuropixels_population_dynamics.ipynb
├── data/
├── results/
├── figures/
└── scripts/
```

The current notebook, however, performs data acquisition and intermediate result generation directly within the Colab workflow, so `data/`, `results/`, and `figures/` do not need to be committed to GitHub unless explicitly desired.

---

# Computational Environment

The notebook is designed primarily for **Google Colab**.

It uses:

- Python 3
- NumPy
- pandas
- SciPy
- scikit-learn
- statsmodels
- Matplotlib
- NetworkX
- python-louvain
- UMAP (`umap-learn`)
- joblib
- requests

The notebook explicitly installs:

```python
!pip install -q umap-learn
```

Google Colab is recommended because the notebook works with a large Neuropixels archive and uses memory-efficient loading/extraction strategies.

---

# Running the Notebook

## 1. Open the notebook

Open:

```text
notebooks/neuropixels_population_dynamics.ipynb
```

in Google Colab.

## 2. Mount Google Drive

The notebook begins by mounting Google Drive and creating a project/data directory.

The notebook uses a Colab/Drive-based workflow because the raw archive is large.

## 3. Download the dataset

The notebook downloads the official Steinmetz Neuropixels archive.

The archive is several gigabytes in size, so sufficient temporary and persistent storage is required.

## 4. Inspect the archive

The archive is inspected before extraction.

The notebook accesses the TAR files within the ZIP archive without unnecessarily extracting the complete dataset.

## 5. Extract the target session

Only the required files from:

```text
Cori_2016-12-14
```

are extracted.

This avoids loading the entire multi-session dataset into the analysis environment.

## 6. Execute cells sequentially

The notebook is intentionally structured as a sequential pipeline.

Later stages depend on objects generated by earlier stages.

In particular, the persistence analysis in Steps 86–90 should not be executed independently of the preceding network and choice-dependent analyses.

---

# Analysis Pipeline

The complete notebook contains a long sequence of numbered analysis stages. The major analytical blocks are summarized below.

---

## Phase I — Data Acquisition and Quality Control

### Steps 1–17

The initial stages:

- configure the Colab environment,
- mount Google Drive,
- download the official dataset,
- inspect the ZIP/TAR structure,
- locate the target session,
- extract required neural and behavioral files,
- load spike arrays,
- determine the neuronal population,
- normalize spike arrays,
- inspect firing activity,
- inspect behavioral trial data,
- generate population rasters,
- define a principled analysis population,
- construct population activity matrices, and
- validate the population representation.

The neural data are loaded using NumPy arrays and memory-efficient representations where appropriate.

---

# Phase II — PCA Neural State Space

## Steps 18–23

The population activity is transformed into a low-dimensional representation.

The notebook performs:

- PCA preprocessing,
- batch-wise dense conversion,
- population-statistic estimation,
- standardization,
- Incremental PCA,
- explained-variance analysis,
- PCA model saving,
- projection of the population into PC space,
- 2D/3D neural state-space visualization, and
- temporal trajectory visualization.

This establishes a population-level neural manifold before explicitly connecting neural dynamics to behavioral choice.

---

# Phase III — Behavioral Alignment and Choice-Dependent Trajectories

## Steps 23–30

Neural activity is aligned to behavioral events and separated according to behavioral choice.

The notebook:

- converts trial times into neural/PCA indices,
- constructs trial-aligned neural trajectories,
- averages trajectories across trials,
- separates trials by choice,
- validates the binary choice mapping,
- baseline-corrects trajectories,
- calculates mean trajectories,
- compares Choice -1 and Choice +1,
- quantifies trajectory separation,
- compares pre- and post-stimulus separation, and
- performs permutation-based significance testing.

The temporal trajectory analysis provides a population-level view of when choice information becomes expressed.

---

# Phase IV — Time-Resolved Choice Decoding

## Steps 31–41

Choice information is quantified using supervised decoding.

The notebook implements **Logistic Regression** with:

- cross-validation,
- time-resolved decoding,
- permutation-based null distributions,
- empirical p-values,
- FDR correction, and
- visualization of significant decoding periods.

A second decoder uses a **300-ms sliding window** to provide a temporally smoothed assessment of choice information.

The pipeline also compares:

```text
instantaneous decoding
vs.
300-ms sliding-window decoding
```

and quantifies decoding onset and its relationship to population trajectory separation.

---

# Phase V — UMAP Population Manifold

## Steps 42–47

The notebook applies **UMAP** to investigate nonlinear population structure.

The analysis includes:

- construction of UMAP inputs,
- embedding neural population states,
- visualization by behavioral choice,
- visualization by time,
- restriction to behaviorally relevant periods,
- choice-centroid analysis,
- time-resolved UMAP choice separation,
- comparison of UMAP structure with PCA and decoding, and
- robustness analysis across multiple UMAP embeddings.

This provides a nonlinear complement to the PCA representation.

---

# Phase VI — Neural Dimensionality and Dynamics

## Steps 48–50

The notebook examines additional population-dynamics properties, including:

- effective neural dimensionality,
- PCA scree diagnostics,
- trajectory speed,
- choice-specific trajectory speed, and
- temporal differences in neural dynamics.

These analyses characterize the geometry and dynamical evolution of the neural population beyond simple choice decoding.

---

# Phase VII — Functional Connectivity Networks

## Steps 51–63

The pipeline then moves from population trajectories to neuronal interaction structure.

Trial-level neural activity is organized into a:

```text
trial × neuron × time
```

representation.

Three behavioral epochs are defined relative to the visual stimulus:

| Epoch | Time relative to stimulus |
|---|---:|
| Pre-stimulus | -0.50 to 0.00 s |
| Early response | 0.00 to 0.50 s |
| Late response | 0.50 to 1.50 s |

Only valid behavioral trials are retained.

The notebook constructs epoch-specific activity and calculates trial-to-trial Pearson functional connectivity.

---

# Functional Network Construction

## Step 55

Functional networks are constructed using:

```text
Pearson correlation
```

with an exploratory threshold of:

```text
r > 0.30
```

The resulting weighted graphs are analyzed using NetworkX and Louvain community detection.

For each epoch, the notebook calculates:

- number of nodes,
- number of edges,
- network density,
- mean degree,
- maximum degree,
- mean weighted strength,
- maximum weighted strength,
- number of communities, and
- Louvain modularity.

### Hubness

Neuronal functional importance is quantified using **weighted strength**, i.e. the sum of weighted connections associated with a neuron.

This weighted strength is used as the primary functional hubness measure in subsequent analyses.

---

# Phase VIII — Common-Neuron and Network Reconfiguration Analysis

## Steps 56–73

The notebook progressively controls for neuronal population differences between epochs and choices.

The analysis includes:

- common-neuron functional connectivity,
- matched-neuron network comparisons,
- network-density changes,
- choice-specific network activity,
- choice-specific functional connectivity,
- matched-neuron choice comparisons,
- choice-dependent connectivity differences,
- matched-choice network density,
- trial-label permutation testing,
- canonical neuron alignment,
- corrected permutation testing,
- FDR correction across epoch comparisons,
- network-density null distributions,
- choice-dependent community structure,
- modularity comparison,
- modularity permutation testing,
- FDR correction of modularity tests, and
- modularity null distributions.

This section is designed to distinguish genuine choice/network effects from effects caused simply by comparing different neuron sets.

---

# Phase IX — Functional Hub Analysis

## Steps 74–80

The notebook examines how functional hubs change over time.

Analyses include:

- extraction of hub scores,
- identification of top hubs,
- hub overlap across epochs,
- hub-strength changes,
- hub-strength distributions,
- canonical functional networks,
- canonical top hubs,
- canonical hub overlap,
- hub rank correlations,
- hub turnover across multiple thresholds,
- Jaccard stability,
- changes in functional hub strength,
- emerging late-response hubs,
- statistical tests of hub-strength changes,
- FDR correction,
- effect-size analysis,
- normalized hub strength, and
- relative hubness changes.

This establishes the network-level foundation for the later choice-dependent persistence analysis.

---

# Phase X — Choice-Dependent Hub Analysis

## Steps 80–85

Choice-dependent hub modulation is quantified using matched/canonical neurons.

For each epoch, the notebook calculates:

```text
Δ hub strength = Choice +1 hub strength − Choice -1 hub strength
```

This allows neurons to be ranked according to whether their functional hubness preferentially favors one choice.

The analysis then evaluates:

- choice-dependent hub-strength distributions,
- symmetry of choice-dependent modulation,
- enrichment of choice-dependent neurons among functional hubs,
- permutation-based hub-enrichment significance,
- FDR correction,
- effect sizes,
- robustness across hub thresholds,
- hubness versus choice-dependent modulation,
- robustness to extreme hubs,
- cross-epoch overlap of choice-dependent neurons,
- permutation testing of temporal stability, and
- FDR-corrected temporal-stability results.

---

# Phase XI — Persistent vs. Recruited Choice-Dependent Neurons

## Steps 86–90

This is the final and most integrated part of the pipeline.

The analysis focuses on the **early-response** and **late-response** epochs.

A canonical population is formed from neurons common to both epochs.

For each choice independently, the top:

```text
5%
```

of neurons showing the strongest choice-dependent modulation are selected in each epoch.

For Choice +1, neurons with the largest positive modulation are selected.

For Choice -1, neurons with the largest negative modulation are selected.

The two epoch-specific populations are then intersected.

This produces:

```text
Persistent
Early-only
Late-only
```

populations.

---

# Persistent Population Definition

A neuron is classified as **persistent** when it belongs to the top 5% choice-dependent population in both:

```text
Early response
AND
Late response
```

A neuron is:

- **Early-only** if it is choice-dependent only in the early epoch.
- **Late-only** if it is choice-dependent only in the late epoch.
- **Persistent** if it is choice-dependent in both epochs.

The analysis is performed separately for:

```text
Choice +1
Choice -1
```

---

# Hub-Enrichment Analysis of Persistent Neurons

## Steps 86A–86C

Persistent neurons are tested for enrichment among functional hubs.

The hub population is defined using the top:

```text
10%
```

of hub-strength values.

The analysis compares the observed persistent-hub overlap against a permutation-based null distribution.

The hub-enrichment analysis uses:

```text
N = 5000
```

permutations.

FDR correction is then applied across the hub-enrichment tests.

---

# Functional Persistence Analysis

## Steps 87A–87D

Persistent neurons are compared with non-persistent neurons using their response and functional properties.

The analysis includes:

- persistent vs recruited functional profiles,
- permutation testing of functional persistence,
- FDR correction,
- leave-one-out robustness testing.

The leave-one-out analysis asks whether the persistence result remains supported after systematically removing individual persistent observations.

The key robustness criterion is:

```text
all leave-one-out persistence ratios > 1
```

---

# Integrated Effect-Size Analysis

## Steps 88A–88B

Persistent neurons are compared with non-persistent neurons using:

- early absolute response difference,
- late absolute response difference,
- early signed response difference,
- late signed response difference,
- early hub strength,
- late hub strength.

The primary group comparison is the **Mann–Whitney U test**.

Effect size is quantified using the **rank-biserial correlation**.

FDR correction is applied to the complete set of integrated effect-size tests.

This separates:

```text
statistical significance
```

from:

```text
magnitude and direction of group separation
```

---

# Cross-Epoch Persistence Stability

## Step 88C

Persistent, early-only, and late-only populations are compared directly.

The analysis quantifies:

- cross-epoch stability ratio,
- cross-epoch absolute difference,
- signed cross-epoch change,
- early response magnitude, and
- late response magnitude.

Persistent neurons are compared against both:

```text
persistent vs early-only
persistent vs late-only
```

using non-parametric group comparisons.

FDR correction is applied to the stability comparisons.

---

# Integrated Evidence Synthesis

## Steps 89A–89C

The independent evidence streams are consolidated.

The evidence domains are:

1. **Hub enrichment**
2. **Leave-one-out robustness**
3. **Effect-size evidence**
4. **Stability-ratio evidence**

The pipeline summarizes the number of significant tests within each domain and generates an integrated evidence score.

The classification is intentionally framed as an **evidence synthesis**, not as a replacement for the underlying statistical tests.

---

# Final Persistence Evidence

The completed analysis produced the following final results.

## Choice -1

```text
Persistent neurons:                 N = 8
Integrated evidence score:          3 / 4
Integrated evidence fraction:       75%
Classification:                     STRONG
Minimum LOO persistence ratio:      1.067393
Mean LOO persistence ratio:         1.134482
Maximum LOO persistence ratio:      1.256160
Median |rank-biserial|:             0.733193
```

Evidence by domain:

```text
Hub enrichment:        0 / 2 significant
LOO robustness:        PASS
Effect-size tests:     5 / 6 significant
Stability tests:       2 / 2 significant
```

The result therefore meets the pipeline's **STRONG** integrated-evidence classification, while the hub-enrichment domain itself is not significant for this choice.

---

## Choice +1

```text
Persistent neurons:                 N = 2
Integrated evidence score:          4 / 4
Integrated evidence fraction:       100%
Classification:                     STRONG
Minimum LOO persistence ratio:      1.164841
Mean LOO persistence ratio:         1.547329
Maximum LOO persistence ratio:      1.929817
Median |rank-biserial|:             0.938935
```

Evidence by domain:

```text
Hub enrichment:        2 / 2 significant
LOO robustness:        PASS
Effect-size tests:     6 / 6 significant
Stability tests:       2 / 2 significant
```

Choice +1 therefore satisfies all four integrated evidence domains used by the pipeline.

---

# Final Cross-Choice Comparison

| Metric | Choice -1 | Choice +1 |
|---|---:|---:|
| Persistent N | 8 | 2 |
| Overall evidence score | 3/4 | 4/4 |
| Evidence fraction | 75% | 100% |
| Minimum LOO ratio | 1.067393 | 1.164841 |
| Mean LOO ratio | 1.134482 | 1.547329 |
| Maximum LOO ratio | 1.256160 | 1.929817 |
| Median \|rank-biserial\| | 0.733193 | 0.938935 |
| Classification | STRONG | STRONG |

Numerically, **Choice +1 has the higher integrated evidence score**.

However, the integrated score is a summary of predefined evidence domains and is **not itself a formal statistical test comparing Choice +1 against Choice -1**.

Therefore, the result should not be interpreted as proof that persistence is statistically stronger for Choice +1 than Choice -1.

---

# Main Scientific Interpretation

The complete pipeline provides **strong convergent within-sample evidence** for persistent choice-dependent neuronal populations across the early and late response epochs for both behavioral choices.

The evidence is supported by multiple analytical perspectives:

```text
Choice-dependent modulation
          │
          ▼
Persistent neuronal population
          │
    ┌─────┼──────────┬────────────┐
    ▼     ▼          ▼            ▼
  Hubs   Effect     Stability     LOO
         sizes                     robustness
    └─────┴──────────┴────────────┘
                 │
                 ▼
       Integrated evidence
```

The agreement across independent analyses makes the persistence result less dependent on any single statistical test.

---

# Important Limitation: Small Persistent Populations

The most important limitation is the size of the persistent groups:

```text
Choice -1: N = 8
Choice +1: N = 2
```

These are very small populations.

Consequently, the final conclusion should be interpreted as:

> **robust within-sample evidence of persistence**

rather than:

> **population-level evidence for the prevalence of persistent choice-dependent neurons.**

The small sample size is particularly important when interpreting effect sizes, robustness estimates, and cross-choice differences.

---

# Important Interpretation of the Final Evidence Score

The final evidence score is a **pipeline-level synthesis metric**.

It summarizes whether independent evidence domains met their predefined criteria.

For example:

```text
Choice -1: 3/4
Choice +1: 4/4
```

This does not mean that the probability of persistence is 75% or 100%.

It also does not constitute a single omnibus p-value.

The underlying p-values, FDR-adjusted p-values, effect sizes, and robustness statistics should therefore be reported alongside the evidence score in any scientific publication.

---

# Statistical Methods Summary

| Analysis | Method |
|---|---|
| Choice trajectory separation | Permutation-based comparison |
| Behavioral decoding | Logistic Regression |
| Decoder validation | Stratified cross-validation |
| Decoder null | Trial-label permutation |
| Multiple comparisons | FDR correction |
| Nonlinear manifold | UMAP |
| Dimensionality reduction | Incremental PCA |
| Functional connectivity | Pearson correlation |
| Functional network threshold | `r > 0.30` |
| Community detection | Louvain |
| Hubness | Weighted network strength |
| Choice-dependent hub analysis | Matched/canonical neuron comparison |
| Persistent population | Top 5% in both early and late epochs |
| Hub enrichment | Permutation test |
| Functional group comparison | Mann–Whitney U |
| Effect size | Rank-biserial correlation |
| Persistence robustness | Leave-one-out analysis |
| Stability comparison | Non-parametric group comparisons |
| Multiple comparisons in persistence analysis | FDR correction |
| Persistence evidence | Integrated domain-level score |

---

# Key Parameters

The principal parameters used in the notebook include:

```text
Choice-dependent percentile:      5%
Hub percentile:                   10%
Network correlation threshold:    r > 0.30

Pre-stimulus epoch:              -0.50 to 0.00 s
Early response epoch:             0.00 to 0.50 s
Late response epoch:              0.50 to 1.50 s

Hub-enrichment permutations:      5000
Functional-persistence permutations: 5000
```

The notebook also uses a fixed random seed (`42`) for the major permutation-based analyses where explicitly specified.

---

# Important Generated Results

The notebook generates and stores numerous intermediate arrays, tables, models, and summary DataFrames.

Examples include:

```text
PC_scores.npy
pca_explained_variance.npy
pca_mean.npy
pca_std.npy
population_pca.pkl

mean_trajectory_choice_minus1.npy
mean_trajectory_choice_plus1.npy
trajectory_distance.npy
trajectory_distance_z.npy
observed_choice_distance.npy
permutation_distances.npy

choice_decoding_accuracy.npy
choice_decoding_null.npy
choice_decoding_p_values.npy
choice_decoding_p_corrected.npy
choice_decoding_significant.npy

window_decoding_accuracy.npy
window_decoding_null.npy
window_decoding_p_values.npy
window_decoding_p_corrected.npy
window_decoding_significant.npy

umap_embedding.npy
umap_choice.npy
umap_time.npy
umap_trial.npy
umap_choice_distance.npy
```

The final persistence-analysis DataFrames include:

```text
step86C_df

step87D_summary_df

step88A_df
step88B_df
step88C_df
step88C_effects_df

step89A_evidence_df
step89A_summary_df
step89A_evidence_count_df

step89B_df
step89B_summary_df
step89B_qc_df

step89C_comparison_df
step89C_domain_df
step89C_robustness_df
step89C_final_df
step89C_qc_df

step90A_df
step90A_qc_df

step90B_comparison_df
step90B_domain_df
step90B_summary_df
step90B_final_df
```

These objects provide an auditable path from individual statistical analyses to the final persistence interpretation.

---

# Quality Control

The final persistence pipeline explicitly checks:

- required intermediate objects,
- expected columns,
- consistency of persistent neuron counts across synthesis stages,
- consistency of classification across stages,
- leave-one-out robustness,
- presence of effect-size evidence,
- presence of stability evidence,
- small persistent sample sizes,
- both Choice -1 and Choice +1 conditions,
- and successful final consolidation.

The completed analysis passed the final quality-control requirements.

---

# Reproducibility Notes

Because the notebook downloads a large external dataset and performs memory-intensive neural/network calculations, exact reproducibility depends on:

- the downloaded Steinmetz dataset version,
- available RAM,
- available storage,
- Python/package versions,
- and the execution environment.

The notebook uses deterministic/random-state controls in several analyses, including permutation analyses with a fixed seed where specified.

For publication-quality reproducibility, it is recommended to record the final Colab environment/package versions and the dataset identifier/version alongside the notebook.

---

# Outputs and Version Control

Large raw datasets should **not** normally be committed to GitHub.

Recommended:

```text
GitHub:
    README.md
    notebooks/neuropixels_population_dynamics.ipynb

Local/Colab:
    raw dataset
    extracted neural data
    generated arrays
    intermediate results
```

If selected result files or figures are added to the repository, keep only lightweight, reproducible outputs unless there is a specific reason to version large binary files.

---

# Citation

The underlying dataset should be cited as:

**Steinmetz, N. A., et al. (2019). Distributed coding of choice, action and engagement across the mouse brain. Nature, 576, 266–273.**

Dataset:

[Steinmetz et al. 2019 Neuropixels — Figshare](https://figshare.com/articles/dataset/Steinmetz_et_al_2019_Neuropixels/9598406)

If this analysis pipeline is associated with a manuscript, add the project-specific citation below when available:

```text
<Add project manuscript / preprint citation here>
```

---

# Limitations and Future Work

Potential extensions include:

- replication across additional Neuropixels sessions,
- replication across animals,
- formal hierarchical/mixed-effects analysis across sessions,
- confidence intervals or bootstrap intervals for effect sizes,
- independent validation datasets,
- alternative functional-connectivity definitions,
- sensitivity analysis of the `r > 0.30` network threshold,
- sensitivity analysis of the 5% choice-dependent threshold,
- formal statistical testing of the cross-choice difference,
- and publication-quality automated figure/report generation.

A particularly important next step is to test whether the persistent-neuron phenomenon generalizes across sessions and animals rather than treating the single analyzed session as a population-level sample.

---

# Final Conclusion

This project implements a complete neural-population analysis pipeline linking **behavioral choice, low-dimensional neural dynamics, choice decoding, functional connectivity, network organization, hubness, and temporal persistence** in a Neuropixels recording.

The final persistence analysis identifies small but strongly supported persistent choice-dependent neuronal populations in both choice conditions. The evidence is convergent across hub enrichment, effect sizes, cross-epoch stability, and leave-one-out robustness where applicable, but the very small persistent populations mean that the findings should be interpreted as **robust within-sample evidence rather than population-level prevalence**.

**Final analytical endpoint: Step 90B.**
