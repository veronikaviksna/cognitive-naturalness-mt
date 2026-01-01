# Cognitive Naturalness in Machine Translation

This repository contains the code and experiments for a project on **modeling reading difficulty from eye-tracking data and applying it to gaze‑informed machine translation reranking**.

The project combines:

* eye‑tracking data from the **MECO** corpus,
* surprisal‑based predictors,
* regression modeling of reading difficulty,
* and a downstream application to **machine translation (MT) candidate reranking** using predicted cognitive difficulty.

---

## Project structure

```
.
├── data/
│   └── wmt/                 # WMT data (not included, see below)
├── src/
│   ├── meco/                # MECO loading, preprocessing, features, regression
│   ├── surprisal/           # Surprisal models (language‑specific)
│   ├── wmt/                 # WMT loading and preprocessing
│   └── gaze_mt/             # Gaze‑informed MT reranking
├── main_notebook.ipynb      # Main experimental notebook
├── README.md
└── requirements.txt         # (optional) dependencies
```

---

## Data

### MECO

The **MECO L1.2.0** eye‑tracking corpus is used to train a word‑level reading difficulty model. The Russian subset is used in this project.

Preprocessing includes:

* filtering implausible fixation durations,
* z‑normalization of fixation times,
* extraction of lexical predictors (word length, frequency, surprisal),
* construction of both token‑level and type‑level (first occurrence) datasets.

### WMT

The **WMT News Commentary** dataset is used for the machine translation experiments.

> The WMT data is **not included in the repository** due to size constraints.

To reproduce the experiments:

1. Download the relevant WMT News Commentary data from the official WMT website.
2. Place the files in `data/wmt/`.
3. Follow the preprocessing steps in `src/wmt/` or `main_notebook.ipynb`.

---

## Methodology

### Reading difficulty model

Reading difficulty is modeled using ordinary least squares (OLS) regression with:

* dependent variable: z‑normalized fixation duration (`dur_z`),
* predictors:

  * surprisal,
  * log word frequency,
  * word length.

Two models are estimated:

1. **Full MECO** (token‑level observations),
2. **First occurrences only** (type‑level, one observation per unique word).

The type‑level model serves as a robustness check for lexical effects.

### Gaze‑informed MT reranking

Predicted reading difficulty scores are used to rerank MT candidate translations.

The comparison includes:

* baseline MT output,
* gaze‑informed reranked output.

Evaluation metrics:

* mean predicted reading difficulty,
* Wilcoxon signed‑rank test (paired comparisons),
* BLEU score to ensure translation quality is preserved.

---

## Results (summary)

Key findings:

* Surprisal and word length show robust positive effects on reading difficulty.
* Effects are consistent across token‑level and type‑level models.
* Gaze‑informed reranking yields a small but statistically significant reduction** in predicted reading difficulty.
* BLEU scores remain comparable between baseline and gaze‑informed MT outputs.

Detailed regression outputs and MT examples are provided in the project files and logs.

---

## Running the experiments

This project was developed and executed primarily in Google Colab.

### Recommended way (Google Colab)

1. Open `main_notebook.ipynb` in Google Colab.
2. Make sure the notebook is connected to a Python runtime.
3. Install required dependencies (if not already available in Colab):

```bash
pip install -r requirements.txt
```

4. Upload or mount the required datasets (see *Data* section).
5. Run the notebook cells sequentially from top to bottom.

The notebook:

* loads and preprocesses MECO and WMT data,
* fits regression models,
* applies gaze-informed reranking,
* evaluates difficulty and translation quality.

### Local execution (optional)

The project can also be run locally if desired:

```bash
pip install -r requirements.txt
jupyter notebook main_notebook.ipynb
```

The notebook:

* loads and preprocesses MECO and WMT data,
* fits regression models,
* applies gaze‑informed reranking,
* evaluates difficulty and translation quality.

---

## Notes

* The repository focuses on reproducibility and clarity, not optimized runtime.
* Paths and data locations may need to be adapted depending on your local setup.
* Large datasets are intentionally excluded from version control.
