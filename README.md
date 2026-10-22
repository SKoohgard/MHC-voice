# MHC heterozygosity and vocal acoustic parameters

Data and analysis code for:

> Koohgard S, Aung T, Arnocky S, Hill AK, Welling LLM, Quillen EE, Cárdenas RA, Dawood K, Shriver MD, Puts DA. MHC heterozygosity and vocal acoustic parameters: no support for the immunocompetence handicap hypothesis in humans. (Submitted to *PLOS Biology*.)

Contact: David A. Puts (dap27@psu.edu)

## Data description

These data were collected to test the immunocompetence handicap hypothesis: the idea that low, male-typical voice pitch and formant frequencies honestly signal immune function. We measured vocal fundamental frequency (f0), formant frequencies (f1–f4, combined into formant position, Pf) and height, and related them to heterozygosity at the major histocompatibility complex (MHC), a genetic marker of immune function. We also tested whether these vocal traits vary more in men than in women and whether they track body growth more strongly in men.

The dataset combines three samples of young adults (N = 960):

- **Sample 1:** 163 U.S. undergraduates from a sibling study (Michigan State University).
- **Sample 2:** 660 unrelated U.S. adults from the ADAPT study (Pennsylvania State University).
- **Sample 3:** 137 Canadian undergraduate men (Nipissing University).

Participants gave voice recordings, height, weight, age and a blood sample for DNA. Acoustic measures were taken in Praat. MHC heterozygosity was measured with 10 HLA-flanking STRs (Sample 1), 114 HLA-tagging SNPs (Sample 2) or direct HLA typing at HLA-A, -B and -DRB1 (Sample 3), and adjusted for genome-wide heterozygosity.

## Repository contents

```
data/MHCdata-final2.xlsx          De-identified participant data
code/MHC-analysis2-code.Rmd       R Markdown script: all analyses and figures
code/MHC-analysis2-code.html      Rendered output of the script
```

### `data/MHCdata-final2.xlsx`

- **Main**: all 960 participants; used for the main analyses.
- **After exclusions**: 903 participants after removing one member of each sibling pair (57 removed); used for the sensitivity analyses. Same variables as Main, except `Family` is dropped and `Sex` is named `Sex1male2female`.
- **VTL_Pf**: vocal tract length and formant position for all 960 participants, in the same row order as Main.

| Variable | Description | Units / coding |
|---|---|---|
| `fileid` | Voice recording file identifier | Numeric ID |
| `Participant_ID` | Anonymized participant identifier | Code |
| `Country` | Country of data collection | `US`, `Canada` |
| `Collaborators` | Sample | `David_MSU` = Sample 1; `Mark &Arslan` = Sample 2; `Arnocky` = Sample 3 |
| `Family` | Family identifier; siblings share a code | Code |
| `Sex` / `Sex1male2female` | Sex | 1 = male, 2 = female |
| `Age` | Age | Years |
| `Height_cm` | Height | cm |
| `Weight_kg` | Body weight | kg |
| `f1new`–`f4new` | Mean formant frequencies f1–f4 (Praat, Burg LPC) | Hz |
| `zF0new` | Mean fundamental frequency (Praat, autocorrelation); raw values, standardized in the code | Hz |
| `minPitch`, `maxPitch`, `sdPitch` | Minimum, maximum and SD of f0 | Hz |
| `voiceBreaks` | Number of voice breaks | Count |
| `jitter`, `jitterRap`, `jitterPpq5`, `jitterDdq` | Jitter (local, RAP, PPQ5, DDP) | Proportion |
| `shimmer`, `shimmerApq3`, `shimmerApq5`, `shimmerApq11`, `shimmerDda` | Shimmer (local, APQ3, APQ5, APQ11, DDA) | Proportion |
| `harmonics` | Harmonics-to-noise ratio | dB |
| `ZMHCTotal` | MHC heterozygosity, z-standardized within sample (main predictor) | z-score |
| `MHC_Het_no` | Number of heterozygous HLA loci (A, B, DRB1); Sample 3 only | 1–3 |
| `conditioned_MHC` | MHC heterozygosity conditioned on genome-wide heterozygosity; Samples 1–2 | Residual |
| `phet_genome` | Proportion of heterozygous genome-wide markers | 0–1 |
| `phet_hla` | Proportion of heterozygous HLA markers | 0–1 |
| `gPC1`–`gPC4` | Genetic principal components (ancestry); Sample 2 only | Unitless |
| `Ethnicity` | Ethnicity indicator | 0/1 |
| `FA` | Fluctuating asymmetry; Sample 2 only | Unitless |
| `VTL.formula` (VTL_Pf sheet) | Vocal tract length (Anikin et al., 2024) | cm |
| `PF` (VTL_Pf sheet) | Formant position: mean of standardized f1–f4 | z-score |

**Missing values** are blank cells. Most are by design (measures collected in only some samples); the rest are acoustic measures that could not be extracted from a recording.

**Abbreviations:** f0 = fundamental frequency; MHC = major histocompatibility complex; HLA = human leukocyte antigen; STR = short tandem repeat; SNP = single nucleotide polymorphism; CODIS = Combined DNA Index System; gPC = genetic principal component; VTL = vocal tract length; Pf = formant position.

## Code/software

- **Praat v6.1.38** (https://www.praat.org) was used to extract acoustic measures. Recordings are not included, so Praat is not needed to reproduce the analyses.
- **R** (https://www.r-project.org), with RStudio or the `rmarkdown` package to knit the script. Packages: `readxl`, `dplyr`, `tidyr`, `lme4`, `lmerTest`, `emmeans`, `car`, `rstatix`, `ggplot2`, `patchwork`, `plotly`.

### Reproducing the analyses

Open `code/MHC-analysis2-code.Rmd` in RStudio and knit it (or run its chunks in order) with `code/` as the working directory; the script reads `../data/MHCdata-final2.xlsx`. The script:

1. Loads the Main sheet and renames `Collaborators` → `sample`, `Height_cm` → `Height`, `Weight_kg` → `Weight`.
2. Z-standardizes f0, height and weight; computes Pf as the mean of standardized f1–f4.
3. Tests sex differences (Cohen's d) and sex differences in variability (Levene's tests).
4. Fits linear mixed-effects models (random intercepts for sample and family) predicting height from MHC heterozygosity, and f0 and Pf from height and from MHC heterozygosity, with sex, age and weight as covariates; tests sex differences in slopes with `emmeans::lstrends`.
5. Fits the exploratory MHC heterozygosity × weight model of male f0 and Pf.
6. Produces figures and descriptive statistics.
7. Repeats the models on the After exclusions sheet (sensitivity analysis).

## License

Data are released under CC0 1.0; code under the MIT License (see `LICENSE`).
