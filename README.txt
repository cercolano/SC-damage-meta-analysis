SPINAL CORD DAMAGE IN MULTIPLE SCLEROSIS: DATA AND ANALYSIS CODE

Extracted data and Python code for the meta-analysis "Spinal Cord Damage in
Multiple Sclerosis: A Meta-Analysis of Morphometric and Quantitative Imaging".
Running run_all.py regenerates all tables and figures from the data files.


CONTENTS

  data/              extracted data, one CSV file (UTF-8) per analysis
  code/              analysis scripts and shared functions
  run_all.py         runs scripts 00-05 in order
  requirements.txt   Python packages


RUNNING THE ANALYSIS

Python with the packages in requirements.txt (tested with Python 3.12.7,
numpy 2.5, pandas 3.0, scipy 1.18, matplotlib 3.11, statsmodels 0.15 and
pillow 12.3):

  pip install -r requirements.txt
  python run_all.py

Results are written to output/tables/ and output/figures/. The output/ folder
is deleted and rebuilt at each run. Running time is about one minute.


DATA

Each row of the metric files is one group (healthy controls or people with
multiple sclerosis) of one study.

  CSA_SC_upper.csv          cervical cord cross-sectional area (CSA), C1-C4, mm2
  CSA_SC_lower.csv          CSA, C4-C7, mm2
  CSA_SC_overall.csv        CSA, whole cervical cord, mm2
  CSA_SC_pooled.csv         CSA, all strata together, mm2
  FA_SC.csv, FA_WM.csv      fractional anisotropy, whole cord and white matter
  MD_SC.csv, MD_WM.csv      mean diffusivity, whole cord and white matter,
                            10^-3 mm2/s
  MTR_SC.csv, MTR_WM.csv    magnetization transfer ratio, whole cord and white
                            matter, percent units
  EDSS_correlation.csv      correlation between cervical CSA and EDSS, one row
                            per extracted estimate
  metric_comparison.csv     all metrics in one file, used for the comparison
                            across metrics

Columns of the metric files:

  study_id   first author and year (Schilling has no year); a letter is added
             when an author has more than one study in the same year
  group      HC (healthy controls) or MS (multiple sclerosis); a study with
             more than one MS cohort has one MS row per cohort
  mean, sd   mean and standard deviation of the group
  n          number of participants (N in CSA_SC_pooled.csv)
  year       publication year

MTR_SC.csv also contains the offset frequency offset(kHz), the echo time
TE_MT(ms), the repetition time TR_MT(ms) and the scanner manufacturer.

metric_comparison.csv has the same structure, with a metric column (metric and
compartment), the number of participants in N and no year column. Publications
on cohorts already represented by a larger or baseline publication are used
only in this file (Supplementary Table 10 of the article); Oh et al. (2013) has
no control group and does not appear in the data files.

Columns of EDSS_correlation.csv: author, metric, Spearman's rho, Pearson's r,
coefficient from a stepwise multiple regression model, p-value and number of
participants, as reported in each study. Values are stored as reported (for
example "< 0.001"); the script converts them to numbers.

Some values were provided by the authors of the original studies or estimated
from published figures; see the Methods section of the article.


SCRIPTS

  00_ten_meta_analyses_figures.py    pooled normative values, standardized mean
                                     differences, forest and funnel plots for
                                     the ten analyses
  01_main_meta_analyses.py           main results of the ten meta-analyses;
                                     DerSimonian-Laird vs REML with
                                     Knapp-Hartung
  02_small_study_bias.py             Egger's and Begg's tests, trim-and-fill
  03_mtr_acquisition_parameters.py   linear regression of whole-cord MTR on
                                     acquisition parameters
  04_edss_correlation.py             pooled correlation between CSA and EDSS,
                                     sensitivity analyses
  05_metric_comparison_che.py        comparison of effect sizes across metrics
                                     (correlated and hierarchical effects
                                     model, robust variance estimation)

Shared functions are in metalib.py (effect sizes, pooling, small-study tests)
and che_core.py (multilevel model). paths.py defines the folders. The
main-text figures are also saved as 600 dpi TIFF files in
output/figures/journal/.
