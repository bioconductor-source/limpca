limpca
================

<!-- badges: start -->

[![R-CMD-check](https://github.com/ManonMartin/limpca/actions/workflows/check-standard.yaml/badge.svg)](https://github.com/ManonMartin/limpca/actions/workflows/check-standard.yaml)
<!-- badges: end -->

The web page of the package can be accessed here:
<https://manonmartin.github.io/limpca/>

## Installation

Installation from GitHub:

``` r
remotes::install_github("ManonMartin/limpca", dependencies = TRUE)
library("limpca")
```

Note that if you would like to build the vignettes, you have to install
`BiocStyle` (from [Bioconductor](https://www.bioconductor.org/)) and
`rmarkdown` packages before installing `limpca` with the following
command:
`remotes::install_github("ManonMartin/limpca", dependencies = TRUE, build_vignettes = TRUE)`.

## 

## Basic usage

Here is a quick start based on the `UCH` dataset (see `?UCH` for more
information on this dataset).

### Data object

`UCH` contains a multivariate matrix (`outcomes`), a `design` data.frame
and a `formula` as a character string:

``` r
str(UCH)
```

The design can be visualised with `plotDesign()`.

``` r
# design
plotDesign(design = UCH$design, x = "Hippurate", 
           y = "Citrate", rows = "Time",
           title = "Design of the UCH dataset")
```

### PCA

Before applying ASCA, one can explore the multivariate structure of the
outcomes with Principal Component Analysis (PCA).

``` r
ResPCA = pcaBySvd(UCH$outcomes)
pcaScreePlot(ResPCA, nPC = 6)
pcaScorePlot(resPcaBySvd = ResPCA, axes = c(1,2), 
             title = "PCA scores plot: PC1 and PC2", 
             design = UCH$design,
             color = "Hippurate", shape = "Citrate",
             points_labs_rn = FALSE)
```

### Model estimation and effect matrix decomposition

``` r
# Model matrix generation
resMM = lmpModelMatrix(UCH)

# Model estimation and effect matrices decomposition
resEM = lmpEffectMatrices(resMM)
```

### Effect matrix test of significance and importance measure

``` r
# Effects importance
resEM$varPercentagesPlot

# Bootstrap tests
resBT = lmpBootstrapTests(resLmpEffectMatrices = resEM, nboot=100)
resBT$resultsTable
```

### ASCA decomposition

``` r
# ASCA decomposition
resASCA = lmpPcaEffects(resLmpEffectMatrices = resEM, method="ASCA")

# Scores Plot for the hippurate
lmpScorePlot(resASCA, effectNames = "Hippurate", 
             color = "Hippurate", shape = "Hippurate")

# Loadings Plot for the hippurate
lmpLoading1dPlot(resASCA, effectNames = c("Hippurate"), 
                              axes = 1, xlab = "ppm")

# Scores ScatterPlot matrix
lmpScoreScatterPlotM(resASCA,PCdim=c(1,1,1,1,1,1,1,2),
                     modelAbbrev = TRUE,
                     varname.colorup = "Citrate",
                     varname.colordown  = "Time",
                     varname.pchup="Hippurate",
                     varname.pchdown="Time",
                     title = "ASCA scores scatterplot matrix")
```

## Additional information

For any enquiry, you can open an
[issue](https://github.com/ManonMartin/limpca/issues) on Github or send
an email to the package authors: <bernadette.govaerts@uclouvain.be> ;
<michel.thiel@uclouvain.be> or <manon.martin@uclouvain.be>.
