# 🧠 Neuropixels Population Dynamics: Choice-Dependent Neural Manifolds, Networks, and Persistent Hubs

### Population Coding • Functional Connectivity • Network Hubs • Temporal Persistence

📂 **Part of:** *Computational Neuroscience & Multimodal Signal Research*
📓 **Notebook:** `neuropixels_population_dynamics.ipynb`

---

# ⭐ Overview

This project is a Google Colab / Python analysis pipeline investigating how **population-level neural activity**, **behavioral choice representations**, **functional connectivity**, **network organization**, and **choice-dependent neuronal hubs** evolve across time in Neuropixels recordings.

The notebook uses the **Steinmetz et al. (2019) Neuropixels dataset** and analyzes the session:

**`Cori_2016-12-14`**

The analysis progresses from raw spike and behavioral data to:

* Quality-controlled neuronal population activity
* Low-dimensional neural state-space representations
* Choice-dependent neural trajectories
* Time-resolved behavioral choice decoding
* UMAP population manifolds
* Functional connectivity networks
* Network density and modularity
* Functional hub identification and turnover
* Choice-dependent hub modulation
* Temporal persistence of choice-dependent neurons
* An integrated statistical assessment of persistent choice-dependent neuronal populations

The final analytical endpoint is **Step 90B — Cross-Choice Persistence Evidence Comparison & Final Statistical Interpretation**.

---

# 🧠 Scientific Question

> **How does neural population activity and functional network organization encode behavioral choice across time, and are there neurons whose choice-dependent functional role persists from the early to the late response period?**

The persistence analysis specifically asks whether neurons that are strongly choice-dependent in the early response epoch remain strongly choice-dependent in the late response epoch, and whether this persistent subset shows convergent evidence of functional importance.

The pipeline distinguishes:

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

# 📁 Dataset

**Steinmetz et al. (2019) Neuropixels dataset** — downloaded directly from Figshare.

* Download endpoint used by the notebook: `https://ndownloader.figshare.com/files/17387882`
* The notebook downloads the archive, inspects it without fully extracting the large dataset, locates the required session, and extracts only the files needed for the analysis.

**Analyzed session:** `Cori_2016-12-14` (neural spike data + behavioral trial information)

**Required neural files**

* `spikes.times.npy`
* `spikes.clusters.npy`
* `spikes.amps.npy`
* `spikes.depths.npy`

**Required behavioral files**

* `trials.response_choice.npy`
* `trials.visualStim_contrastLeft.npy`
* `trials.visualStim_contrastRight.npy`
* `trials.visualStim_times.npy`
* `trials.feedbackType.npy`
* `trials.goCue_times.npy`
* `trials.response_times.npy`

The notebook also extracts `channels.brainLocation.tsv` for channel/brain-location information.

---

# 📂 Repository Structure

Recommended layout for this project folder:

```text
Neuropixels-Population-Dynamics/
│
├── README.md
│
└── notebooks/
    └── neuropixels_population_dynamics.ipynb
```

A larger project can later be expanded to:

```text
Neuropixels-Population-Dynamics/
│
├── README.md
├── notebooks/
│   └── neuropixels_population_dynamics.ipynb
├── data/
├── results/
├── figures/
└── scripts/
```

The current notebook performs data acquisition and intermediate result generation directly within the Colab workflow, so `data/`, `results/`, and `figures/` do not need to be committed to GitHub unless explicitly desired.

---

# 💻 Computational Environment

The notebook is designed primarily for **Google Colab**.

**Core libraries:**

* Python 3
* NumPy
* pandas
* SciPy
* scikit-learn
* statsmodels
* Matplotlib
* NetworkX
* python-louvain
* UMAP (umap-learn)
* joblib
* requests

The notebook explicitly installs:

```bash
!pip install -q umap-learn
```

Google Colab is recommended because the notebook works with a large Neuropixels archive and uses memory-efficient loading/extraction strategies.

---

# ☁️ Running the Notebook

1. **Open the notebook** — `notebooks/neuropixels_population_dynamics.ipynb` in Google Colab.
2. **Mount Google Drive** — the notebook begins by mounting Google Drive and creating a project/data directory, since the raw archive is large.
3. **Download the dataset** — the official Steinmetz Neuropixels archive (several gigabytes), requiring sufficient temporary and persistent storage.
4. **Inspect the archive** — the notebook accesses the TAR files within the ZIP archive without unnecessarily extracting the complete dataset.
5. **Extract the target session** — only the required files from `Cori_2016-12-14` are extracted, avoiding loading the entire multi-session dataset.
6. **Execute cells sequentially** — the notebook is intentionally structured as a sequential pipeline; later stages depend on objects generated by earlier stages. In particular, the persistence analysis in **Steps 86–90** should not be executed independently of the preceding network and choice-dependent analyses.

---

# 🔬 Analysis Pipeline

The complete notebook contains a long sequence of numbered analysis stages, organized into eleven phases.

### Phase I — Data Acquisition and Quality Control *(Steps 1–17)*

Configures the Colab environment, mounts Google Drive, downloads the official dataset, inspects the ZIP/TAR structure, locates the target session, extracts required neural and behavioral files, loads spike arrays, determines the neuronal population, normalizes spike arrays, inspects firing activity and behavioral trial data, generates population rasters, defines a principled analysis population, constructs population activity matrices, and validates the population representation.

### Phase II — PCA Neural State Space *(Steps 18–23)*

Population activity is transformed into a low-dimensional representation via PCA preprocessing, batch-wise dense conversion, population-statistic estimation, standardization, Incremental PCA, explained-variance analysis, PCA model saving, projection into PC space, and 2D/3D neural state-space and temporal trajectory visualization.

### Phase III — Behavioral Alignment and Choice-Dependent Trajectories *(Steps 23–30)*

Neural activity is aligned to behavioral events and separated by choice: converting trial times into neural/PCA indices, constructing trial-aligned trajectories, averaging across trials, separating by choice, validating the binary choice mapping, baseline correction, comparing Choice -1 and Choice +1, quantifying trajectory separation, comparing pre- and post-stimulus separation, and permutation-based significance testing.

### Phase IV — Time-Resolved Choice Decoding *(Steps 31–41)*

Choice information is quantified using **Logistic Regression** with cross-validation, time-resolved decoding, permutation-based null distributions, empirical p-values, FDR correction, and visualization of significant decoding periods. A second decoder uses a **300-ms sliding window** for temporally smoothed assessment, compared against instantaneous decoding, and decoding onset is related to population trajectory separation.

### Phase V — UMAP Population Manifold *(Steps 42–47)*

UMAP is applied to investigate nonlinear population structure: constructing UMAP inputs, embedding neural population states, visualizing by choice and by time, restricting to behaviorally relevant periods, choice-centroid analysis, time-resolved UMAP choice separation, comparison with PCA/decoding, and robustness across multiple embeddings.

### Phase VI — Neural Dimensionality and Dynamics *(Steps 48–50)*

Examines effective neural dimensionality, PCA scree diagnostics, trajectory speed, choice-specific trajectory speed, and temporal differences in neural dynamics.

### Phase VII — Functional Connectivity Networks *(Steps 51–63)*

Trial-level neural activity is organized into a **trial × neuron × time** representation. Three behavioral epochs are defined relative to the visual stimulus:

| Epoch | Time relative to stimulus |
|---|---|
| Pre-stimulus | −0.50 to 0.00 s |
| Early response | 0.00 to 0.50 s |
| Late response | 0.50 to 1.50 s |

Only valid behavioral trials are retained. Epoch-specific activity is constructed and trial-to-trial **Pearson functional connectivity** is calculated.

**Functional Network Construction (Step 55):** Networks are built using Pearson correlation with an exploratory threshold of **r > 0.30**. The resulting weighted graphs are analyzed using NetworkX and Louvain community detection. For each epoch: number of nodes, number of edges, network density, mean degree, maximum degree, mean weighted strength, maximum weighted strength, number of communities, and Louvain modularity.

**Hubness:** Neuronal functional importance is quantified using **weighted strength** (sum of weighted connections associated with a neuron) — the primary functional hubness measure used in subsequent analyses.

### Phase VIII — Common-Neuron and Network Reconfiguration Analysis *(Steps 56–73)*

Progressively controls for neuronal population differences between epochs and choices: common-neuron functional connectivity, matched-neuron network comparisons, network-density changes, choice-specific network activity and connectivity, matched-neuron choice comparisons, choice-dependent connectivity differences, matched-choice network density, trial-label permutation testing, canonical neuron alignment, corrected permutation testing, FDR correction across epoch comparisons, network-density null distributions, choice-dependent community structure, modularity comparison and permutation testing, FDR correction of modularity tests, and modularity null distributions. This section distinguishes genuine choice/network effects from effects caused simply by comparing different neuron sets.

### Phase IX — Functional Hub Analysis *(Steps 74–80)*

Examines how functional hubs change over time: extraction of hub scores, identification of top hubs, hub overlap across epochs, hub-strength changes and distributions, canonical functional networks and top hubs, canonical hub overlap, hub rank correlations, hub turnover across multiple thresholds, Jaccard stability, changes in functional hub strength, emerging late-response hubs, statistical tests of hub-strength changes, FDR correction, effect-size analysis, normalized hub strength, and relative hubness changes.

### Phase X — Choice-Dependent Hub Analysis *(Steps 80–85)*

Choice-dependent hub modulation is quantified using matched/canonical neurons:

```text
Δ hub strength = Choice +1 hub strength − Choice -1 hub strength
```

Neurons are ranked according to whether their functional hubness preferentially favors one choice. The analysis evaluates choice-dependent hub-strength distributions, symmetry of modulation, enrichment of choice-dependent neurons among functional hubs, permutation-based hub-enrichment significance, FDR correction, effect sizes, robustness across hub thresholds, hubness versus choice-dependent modulation, robustness to extreme hubs, cross-epoch overlap of choice-dependent neurons, and FDR-corrected temporal-stability results.

### Phase XI — Persistent vs. Recruited Choice-Dependent Neurons *(Steps 86–90)*

The final and most integrated part of the pipeline, focused on the early-response and late-response epochs.

A canonical population is formed from neurons common to both epochs. For each choice independently, the **top 5%** of neurons showing the strongest choice-dependent modulation are selected in each epoch (largest positive modulation for Choice +1, largest negative modulation for Choice -1). The two epoch-specific populations are then intersected, producing **Persistent**, **Early-only**, and **Late-only** populations.

**Persistent Population Definition:** A neuron is classified as:
- **Persistent** if choice-dependent in both early and late response epochs
- **Early-only** if choice-dependent only in the early epoch
- **Late-only** if choice-dependent only in the late epoch

Performed separately for Choice +1 and Choice -1.

**Hub-Enrichment Analysis of Persistent Neurons (Steps 86A–86C):** Persistent neurons are tested for enrichment among functional hubs (top **10%** of hub-strength values), compared against a permutation-based null distribution (**N = 5000** permutations), with FDR correction applied.

**Functional Persistence Analysis (Steps 87A–87D):** Persistent neurons are compared with non-persistent neurons using their response and functional properties: persistent vs. recruited functional profiles, permutation testing of functional persistence, FDR correction, and leave-one-out robustness testing (key criterion: **all leave-one-out persistence ratios > 1**).

**Integrated Effect-Size Analysis (Steps 88A–88B):** Persistent vs. non-persistent neurons are compared using early/late absolute response difference, early/late signed response difference, and early/late hub strength. Primary group comparison: **Mann–Whitney U test**. Effect size: **rank-biserial correlation**. FDR correction applied across the complete set of tests.

**Cross-Epoch Persistence Stability (Step 88C):** Persistent, early-only, and late-only populations are compared directly — cross-epoch stability ratio, cross-epoch absolute difference, signed cross-epoch change, early/late response magnitude. Persistent vs. early-only and persistent vs. late-only comparisons use non-parametric tests, with FDR correction.

**Integrated Evidence Synthesis (Steps 89A–89C):** The independent evidence streams — hub enrichment, leave-one-out robustness, effect-size evidence, and stability-ratio evidence — are consolidated. The pipeline summarizes the number of significant tests within each domain and generates an integrated evidence score, framed as an evidence synthesis rather than a replacement for the underlying statistical tests.

---

# 📊 Final Persistence Evidence

## Choice -1

| Metric | Value |
|---|---|
| Persistent neurons | N = 8 |
| Integrated evidence score | 3 / 4 |
| Integrated evidence fraction | 75% |
| Classification | **STRONG** |
| Minimum LOO persistence ratio | 1.067393 |
| Mean LOO persistence ratio | 1.134482 |
| Maximum LOO persistence ratio | 1.256160 |
| Median \|rank-biserial\| | 0.733193 |

**Evidence by domain:**

| Domain | Result |
|---|---|
| Hub enrichment | 0 / 2 significant |
| LOO robustness | PASS |
| Effect-size tests | 5 / 6 significant |
| Stability tests | 2 / 2 significant |

The result meets the pipeline's STRONG integrated-evidence classification, while the hub-enrichment domain itself is not significant for this choice.

## Choice +1

| Metric | Value |
|---|---|
| Persistent neurons | N = 2 |
| Integrated evidence score | 4 / 4 |
| Integrated evidence fraction | 100% |
| Classification | **STRONG** |
| Minimum LOO persistence ratio | 1.164841 |
| Mean LOO persistence ratio | 1.547329 |
| Maximum LOO persistence ratio | 1.929817 |
| Median \|rank-biserial\| | 0.938935 |

**Evidence by domain:**

| Domain | Result |
|---|---|
| Hub enrichment | 2 / 2 significant |
| LOO robustness | PASS |
| Effect-size tests | 6 / 6 significant |
| Stability tests | 2 / 2 significant |

Choice +1 satisfies all four integrated evidence domains used by the pipeline.

## Final Cross-Choice Comparison

| Metric | Choice -1 | Choice +1 |
|---|---|---|
| Persistent N | 8 | 2 |
| Overall evidence score | 3/4 | 4/4 |
| Evidence fraction | 75% | 100% |
| Minimum LOO ratio | 1.067393 | 1.164841 |
| Mean LOO ratio | 1.134482 | 1.547329 |
| Maximum LOO ratio | 1.256160 | 1.929817 |
| Median \|rank-biserial\| | 0.733193 | 0.938935 |
| Classification | STRONG | STRONG |

Numerically, Choice +1 has the higher integrated evidence score. However, the integrated score is a summary of predefined evidence domains and is not itself a formal statistical test comparing Choice +1 against Choice -1. Therefore, the result should **not** be interpreted as proof that persistence is statistically stronger for Choice +1 than Choice -1.

---

# 🧩 Main Scientific Interpretation

The complete pipeline provides strong convergent within-sample evidence for persistent choice-dependent neuronal populations across the early and late response epochs for both behavioral choices, supported by multiple analytical perspectives:

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

# ⚠️ Important Limitation: Small Persistent Populations

The most important limitation is the size of the persistent groups:

* Choice -1: N = 8
* Choice +1: N = 2

These are very small populations. Consequently, the final conclusion should be interpreted as **robust within-sample evidence of persistence**, rather than **population-level evidence for the prevalence of persistent choice-dependent neurons**. The small sample size is particularly important when interpreting effect sizes, robustness estimates, and cross-choice differences.

---

# ⚠️ Interpretation of the Final Evidence Score

The final evidence score is a pipeline-level synthesis metric summarizing whether independent evidence domains met their predefined criteria (e.g., Choice -1: 3/4, Choice +1: 4/4). This does **not** mean that the probability of persistence is 75% or 100%, nor does it constitute a single omnibus p-value. The underlying p-values, FDR-adjusted p-values, effect sizes, and robustness statistics should therefore be reported alongside the evidence score in any scientific publication.

---

# 📈 Statistical Methods Summary

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
| Functional network threshold | r > 0.30 |
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

# 🔑 Key Parameters

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

The notebook also uses a fixed random seed (**42**) for the major permutation-based analyses where explicitly specified.

---

# 📦 Important Generated Results

The notebook generates and stores numerous intermediate arrays, tables, models, and summary DataFrames, including:

**PCA / trajectory outputs:** `PC_scores.npy`, `pca_explained_variance.npy`, `pca_mean.npy`, `pca_std.npy`, `population_pca.pkl`, `mean_trajectory_choice_minus1.npy`, `mean_trajectory_choice_plus1.npy`, `trajectory_distance.npy`, `trajectory_distance_z.npy`, `observed_choice_distance.npy`, `permutation_distances.npy`

**Decoding outputs:** `choice_decoding_accuracy.npy`, `choice_decoding_null.npy`, `choice_decoding_p_values.npy`, `choice_decoding_p_corrected.npy`, `choice_decoding_significant.npy`, `window_decoding_accuracy.npy`, `window_decoding_null.npy`, `window_decoding_p_values.npy`, `window_decoding_p_corrected.npy`, `window_decoding_significant.npy`

**UMAP outputs:** `umap_embedding.npy`, `umap_choice.npy`, `umap_time.npy`, `umap_trial.npy`, `umap_choice_distance.npy`

**Persistence-analysis DataFrames:** `step86C_df`, `step87D_summary_df`, `step88A_df`, `step88B_df`, `step88C_df`, `step88C_effects_df`, `step89A_evidence_df`, `step89A_summary_df`, `step89A_evidence_count_df`, `step89B_df`, `step89B_summary_df`, `step89B_qc_df`, `step89C_comparison_df`, `step89C_domain_df`, `step89C_robustness_df`, `step89C_final_df`, `step89C_qc_df`, `step90A_df`, `step90A_qc_df`, `step90B_comparison_df`, `step90B_domain_df`, `step90B_summary_df`, `step90B_final_df`

These objects provide an auditable path from individual statistical analyses to the final persistence interpretation.

---

# ✅ Quality Control

The final persistence pipeline explicitly checks:

* Required intermediate objects
* Expected columns
* Consistency of persistent neuron counts across synthesis stages
* Consistency of classification across stages
* Leave-one-out robustness
* Presence of effect-size evidence
* Presence of stability evidence
* Small persistent sample sizes
* Both Choice -1 and Choice +1 conditions
* Successful final consolidation

The completed analysis passed the final quality-control requirements.

---

# 🔁 Reproducibility Notes

Because the notebook downloads a large external dataset and performs memory-intensive neural/network calculations, exact reproducibility depends on:

* The downloaded Steinmetz dataset version
* Available RAM
* Available storage
* Python/package versions
* The execution environment

The notebook uses deterministic/random-state controls in several analyses, including permutation analyses with a fixed seed where specified. For publication-quality reproducibility, it is recommended to record the final Colab environment/package versions and the dataset identifier/version alongside the notebook.

---

# 📤 Outputs and Version Control

Large raw datasets should not normally be committed to GitHub.

**Recommended:**

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

# 📚 Citation

The underlying dataset should be cited as:

> Steinmetz, N. A., et al. (2019). *Distributed coding of choice, action and engagement across the mouse brain.* Nature, 576, 266–273.

**Dataset:** Steinmetz et al. 2019 Neuropixels — Figshare

If this analysis pipeline is associated with a manuscript, add the project-specific citation here when available:

`<Add project manuscript / preprint citation here>`

---

# 🚀 Limitations and Future Work

Potential extensions include:

* Replication across additional Neuropixels sessions
* Replication across animals
* Formal hierarchical/mixed-effects analysis across sessions
* Confidence intervals or bootstrap intervals for effect sizes
* Independent validation datasets
* Alternative functional-connectivity definitions
* Sensitivity analysis of the r > 0.30 network threshold
* Sensitivity analysis of the 5% choice-dependent threshold
* Formal statistical testing of the cross-choice difference
* Publication-quality automated figure/report generation

A particularly important next step is to test whether the persistent-neuron phenomenon generalizes across sessions and animals rather than treating the single analyzed session as a population-level sample.

---

# 🏁 Final Conclusion

This project implements a complete neural-population analysis pipeline linking behavioral choice, low-dimensional neural dynamics, choice decoding, functional connectivity, network organization, hubness, and temporal persistence in a Neuropixels recording.

The final persistence analysis identifies small but strongly supported persistent choice-dependent neuronal populations in both choice conditions. The evidence is convergent across hub enrichment, effect sizes, cross-epoch stability, and leave-one-out robustness where applicable, but the very small persistent populations mean that the findings should be interpreted as robust within-sample evidence rather than population-level prevalence.

**Final analytical endpoint:** Step 90B.

---

# 📬 Contact

**Khunsa Iftikhar**

**Data Scientist • NeuroAI Researcher**

📧 **Email:** [khunsaiftikhar123@gmail.com](mailto:khunsaiftikhar123@gmail.com)

💻 **GitHub:** https://github.com/khunsa123

🎓 **Google Scholar:** https://scholar.google.com/citations?hl=en&user=Q-mM508AAAAJ
