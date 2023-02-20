R Code for “Behavioural differences between ornamented and unornamented
male Red-backed Fairywrens (*Malurus melanocephalus*) in the nonbreeding
season”
================
Trey C. Hendrix
Updated on 19 February 2023

- <a href="#required-packages" id="toc-required-packages">Required
  Packages</a>
- <a href="#data" id="toc-data">Data</a>
- <a href="#logistic-mixed-effects-models"
  id="toc-logistic-mixed-effects-models">Logistic Mixed-effects Models</a>
  - <a href="#preparing-data-for-logistic-mixed-effects-models"
    id="toc-preparing-data-for-logistic-mixed-effects-models">Preparing Data
    for Logistic Mixed-Effects Models</a>
  - <a href="#logistic-mixed-effects-models-1"
    id="toc-logistic-mixed-effects-models-1">Logistic Mixed-effects
    Models</a>
  - <a href="#table-1" id="toc-table-1">Table 1</a>
  - <a href="#figure-1" id="toc-figure-1">Figure 1</a>
  - <a href="#reproducing-statistics-reported-in-text"
    id="toc-reproducing-statistics-reported-in-text">Reproducing Statistics
    Reported in Text</a>
  - <a href="#assessing-model-fit-using-the-dharma-package"
    id="toc-assessing-model-fit-using-the-dharma-package">Assessing Model
    Fit Using the DHARMa Package</a>
- <a href="#supplemental-material"
  id="toc-supplemental-material">Supplemental Material</a>
  - <a href="#table-s1" id="toc-table-s1">Table S1</a>
  - <a href="#table-s2" id="toc-table-s2">Table S2</a>
  - <a href="#figure-s1" id="toc-figure-s1">Figure S1</a>
  - <a href="#figure-s2" id="toc-figure-s2">Figure S2</a>
  - <a href="#figure-s3" id="toc-figure-s3">Figure S3</a>
  - <a href="#figure-s4" id="toc-figure-s4">Figure S4</a>
  - <a href="#figure-s5" id="toc-figure-s5">Figure S5</a>
  - <a href="#figure-s6" id="toc-figure-s6">Figure S6</a>

This document contains the R code used to produce the analyses, figures,
and supplemental material for the article published in *Emu - Austral
Ornithology* entitled “Behavioural differences between ornamented and
unornamented male Red-backed Fairywrens (*Malurus melanocephalus*) in
the nonbreeding season”. The complete list of authors is: Trey C.
Hendrix, Facundo Fernandez-Duque, Sarah Toner, Lauren G. Hitt, Robin G.
Thady, Megan Massa, Samantha J. Hagler, Margaux Armfield, Nathalie
Clarke, Phoebe Honscheid, Sarah Khalil, Carly E. Hawkins, Samantha M.
Lantz, Joseph F. Welklin, John P. Swaddle, Michael S. Webster, and
Jordan Karubian. The DOI for the journal article is:
[10.1080/01584197.2023.2182224](http://dx.doi.org/10.1080/01584197.2023.2182224)

# Required Packages

``` r
library(readr)
library(ggplot2)
library(dplyr)
library(tidyr)
library(purrr)
library(lme4)
library(MuMIn)
library(broom.mixed)
library(lubridate)
library(stringr)
library(knitr)
library(DHARMa)
library(gridExtra)
library(ggpubr)
library(lemon)
```

This document was created using:

``` r
sessionInfo()
```

    ## R version 4.2.2 (2022-10-31)
    ## Platform: x86_64-apple-darwin17.0 (64-bit)
    ## Running under: macOS Big Sur ... 10.16
    ## 
    ## Matrix products: default
    ## BLAS:   /Library/Frameworks/R.framework/Versions/4.2/Resources/lib/libRblas.0.dylib
    ## LAPACK: /Library/Frameworks/R.framework/Versions/4.2/Resources/lib/libRlapack.dylib
    ## 
    ## locale:
    ## [1] en_US.UTF-8/en_US.UTF-8/en_US.UTF-8/C/en_US.UTF-8/en_US.UTF-8
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ##  [1] lemon_0.4.6         ggpubr_0.5.0        gridExtra_2.3      
    ##  [4] DHARMa_0.4.6        knitr_1.41          stringr_1.5.0      
    ##  [7] lubridate_1.9.0     timechange_0.1.1    broom.mixed_0.2.9.4
    ## [10] MuMIn_1.47.1        lme4_1.1-31         Matrix_1.5-1       
    ## [13] purrr_0.3.5         tidyr_1.2.1         dplyr_1.0.10       
    ## [16] ggplot2_3.4.0       readr_2.1.3        
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] Rcpp_1.0.9        lattice_0.20-45   listenv_0.9.0     assertthat_0.2.1 
    ##  [5] digest_0.6.31     utf8_1.2.2        parallelly_1.33.0 plyr_1.8.8       
    ##  [9] R6_2.5.1          backports_1.4.1   stats4_4.2.2      evaluate_0.19    
    ## [13] pillar_1.8.1      rlang_1.0.6       rstudioapi_0.14   minqa_1.2.5      
    ## [17] car_3.1-1         furrr_0.3.1       nloptr_2.0.3      rmarkdown_2.19   
    ## [21] splines_4.2.2     munsell_0.5.0     broom_1.0.2       compiler_4.2.2   
    ## [25] xfun_0.35         pkgconfig_2.0.3   globals_0.16.2    htmltools_0.5.4  
    ## [29] tidyselect_1.2.0  tibble_3.1.8      codetools_0.2-18  fansi_1.0.3      
    ## [33] future_1.30.0     tzdb_0.3.0        withr_2.5.0       MASS_7.3-58.1    
    ## [37] grid_4.2.2        nlme_3.1-160      gtable_0.3.1      lifecycle_1.0.3  
    ## [41] DBI_1.1.3         magrittr_2.0.3    scales_1.2.1      carData_3.0-5    
    ## [45] cli_3.4.1         stringi_1.7.8     ggsignif_0.6.4    ellipsis_0.3.2   
    ## [49] generics_0.1.3    vctrs_0.5.1       boot_1.3-28       tools_4.2.2      
    ## [53] forcats_0.5.2     glue_1.6.2        hms_1.1.2         abind_1.4-5      
    ## [57] parallel_4.2.2    fastmap_1.1.0     yaml_2.3.6        colorspace_2.0-3 
    ## [61] rstatix_0.7.1

# Data

This manuscript uses two datasets: (1) focal behavioural observations,
which is a file named “RBFW3_Anon_Data_210720.csv” and (2) opportunistic
behavioural observations, which is a file named
“RBFW3_Anon_Opportunistic_Data_230116.csv”

Both datasets have been uploads to Figshare using the following DOI
links:

- Focal data: <https://doi.org/10.6084/m9.figshare.15088014>
- Opportunistic data: <https://doi.org/10.6084/m9.figshare.21790187>

They are also available in the
[fairywren-nonbreeding-behavior](https://github.com/treyhendrix/fairywren-nonbreeding-behavior)
GitHub repository in the “Data” folder. The contents of these datasets
are detailed in the descriptions on Figshare and in a metadata/README
file in the “Data” folder of the GitHub repository.

The majority of our analyses use the focal behavioural data, which is
simply referred to as “RBFW” in the following code. The opportunistic
behavioural data is used for generate supplementary figures and tables
and is referred to as “RBFW_sup” in code.

``` r
RBFW <- read_csv(file_path_to_focal_data)
glimpse(RBFW)
```

    ## Rows: 369
    ## Columns: 38
    ## $ Observation_ID     <chr> "FFD001", "FFD002", "FFD006", "FFD007", "FFD010", "…
    ## $ Date               <date> 2016-06-24, 2016-06-24, 2016-06-23, 2016-06-23, 20…
    ## $ Year               <dbl> 2016, 2016, 2016, 2016, 2016, 2016, 2016, 2016, 201…
    ## $ Day_of_year        <dbl> 176, 176, 175, 175, 176, 176, 178, 180, 175, 173, 1…
    ## $ Time               <dbl> 925, 1000, 911, 800, 1150, 1237, 1245, 1150, 1023, …
    ## $ Bird_ID            <chr> "X05", "U07", "Q08", "J02", "X07", "T01", "M04", "U…
    ## $ Observer           <chr> "FFD", "FFD", "FFD", "FFD", "FFD", "FFD", "FFD", "F…
    ## $ Sex                <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "…
    ## $ Plumage_score      <dbl> 100, 100, 0, 0, 0, 100, 0, 0, 0, 0, 0, 40, 0, 0, 0,…
    ## $ Plumage_group      <chr> "BRIGHT", "BRIGHT", "DULL", "DULL", "DULL", "BRIGHT…
    ## $ Age                <dbl> 4, 4, 2, 3, 2, 3, 2, 2, 2, 2, 1, 3, 3, 2, 2, 2, 5, …
    ## $ Age_exact          <chr> "exact", "min", "exact", "min", "exact", "exact", "…
    ## $ Alarm_calling      <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, …
    ## $ Allopreening       <dbl> 0, 75, 0, 0, 0, 8, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,…
    ## $ Calling            <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ Chasing            <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ Chipping           <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ Courtship          <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ Flying             <dbl> 6, 8, 4, 5, 21, 1, 2, 2, 6, 13, 4, 5, 2, 5, 4, 25, …
    ## $ Foraging           <dbl> 101, 45, 247, 161, 49, 49, 224, 163, 0, 0, 143, 140…
    ## $ Other              <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ Preening           <dbl> 0, 43, 0, 0, 17, 107, 4, 0, 0, 0, 0, 0, 0, 0, 0, 17…
    ## $ Scolding           <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ Singing            <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ Sitting            <dbl> 21, 29, 0, 44, 73, 15, 6, 0, 173, 161, 5, 23, 16, 0…
    ## $ U                  <dbl> 172, 100, 47, 87, 137, 120, 59, 134, 119, 117, 148,…
    ## $ Vocalizing         <dbl> 0, 0, 2, 3, 3, 0, 5, 1, 2, 9, 0, 3, 3, 0, 1, 0, 0, …
    ## $ Observation_time   <dbl> 300, 300, 300, 300, 300, 300, 300, 300, 300, 300, 3…
    ## $ Alarm_calling_prop <dbl> 0.00000000, 0.00000000, 0.00000000, 0.00000000, 0.0…
    ## $ Allopreening_prop  <dbl> 0.00000000, 0.37500000, 0.00000000, 0.00000000, 0.0…
    ## $ Chasing_prop       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ Courtship_prop     <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ Flying_prop        <dbl> 0.046875000, 0.040000000, 0.015810277, 0.023474178,…
    ## $ Foraging_prop      <dbl> 0.78906250, 0.22500000, 0.97628458, 0.75586854, 0.3…
    ## $ Other_prop         <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ Preening_prop      <dbl> 0.00000000, 0.21500000, 0.00000000, 0.00000000, 0.1…
    ## $ Sitting_prop       <dbl> 0.16406250, 0.14500000, 0.00000000, 0.20657277, 0.4…
    ## $ Vocalizing_prop    <dbl> 0.000000000, 0.000000000, 0.007905138, 0.014084507,…

``` r
RBFW_sup <- read_csv(file_path_to_opp_data)
glimpse(RBFW_sup)
```

    ## Rows: 728
    ## Columns: 17
    ## $ Observation_ID            <chr> "OPP001", "OPP002", "OPP003", "OPP004", "OPP…
    ## $ Date                      <date> 2015-06-02, 2015-06-03, 2015-06-09, 2015-06…
    ## $ Year                      <dbl> 2015, 2015, 2015, 2015, 2015, 2015, 2015, 20…
    ## $ Time                      <chr> "8H 9M 0S", "10H 45M 0S", "7H 15M 0S", "12H …
    ## $ Behavior                  <chr> "Allopreening", "Courtship", "Allopreening",…
    ## $ Sender_ID                 <chr> "JB4", "JD6", "CI5", "IC7", "GD9", "FB6", "H…
    ## $ Sender_plumage_category   <chr> "Ornamented", "Ornamented", "Unornamented", …
    ## $ Sender_sex                <chr> "M", "M", "F", "M", "F", "M", "F", "M", "M",…
    ## $ Sender_age                <dbl> 3, 3, 5, 3, 3, 1, 1, 2, 2, 2, 3, 4, 1, 2, 4,…
    ## $ Sender_age_exact          <chr> "exact", "min", "exact", "exact", "exact", "…
    ## $ Receiver_ID               <chr> "GG2", "EG1", "ED4", "CH6", "HI9", "AJ6", "H…
    ## $ Receiver_plumage_category <chr> "Ornamented", "Unornamented", "Ornamented", …
    ## $ Receiver_sex              <chr> "M", "F", "M", "F", "F", "F", "M", "F", "M",…
    ## $ Receiver_age              <dbl> 5, 3, 4, 2, 1, 3, 1, 1, 1, 3, 2, 5, 5, 2, 4,…
    ## $ Receiver_age_exact        <chr> "min", "exact", "exact", "exact", "min", "mi…
    ## $ Social_mate               <dbl> 0, 0, 1, 0, 0, 0, 0, NA, 0, 0, 1, 0, 0, 1, 0…
    ## $ Prev_breeding_group       <dbl> 0, 0, NA, 0, NA, 0, 0, 0, 0, 0, 1, 1, 1, 0, …

# Logistic Mixed-effects Models

In our analysis, we use logistic mixed-effects models with a binomial
distribution and logit link to determine which factors (plumage
category, age, and day of year) predict whether or not a behaviour of
interest (allopreening, chasing, courtship, preening, and vocalizing)
was observed during a focal behavioural observation.

The general format of these models (in the notation of the lme4 package)
is:

*Behaviour of interest observed (1 or 0) \~ Plumage category + Year +
Age + Day of year + (1\|Observer) + (1\|Bird ID)*

## Preparing Data for Logistic Mixed-Effects Models

First, it is necessary to add binary variables to our dataset for our
behaviours of interest:

``` r
binaryifier <- function(var){
  ifelse(var > 0, 1, 0)
}

variables_of_interest <- c("Courtship_prop", "Vocalizing_prop", 
                           "Allopreening_prop", "Preening_prop", 
                           "Chasing_prop")

RBFW_bin <- RBFW %>%
  mutate_at(.vars = variables_of_interest , 
            .funs = binaryifier) %>%
  rename_at(.vars = variables_of_interest , 
            .funs = list(~gsub("prop", "bin", .))) %>% 
  select(Observation_ID, contains("bin"))

RBFW <- RBFW %>%
  left_join(RBFW_bin, by = "Observation_ID")
```

All numeric variables (e.g., age and day of year) in models will be
standardized as shown below:

``` r
standardizer <- function(x){
  y <- (x - mean(x))/sd(x)
  return(y)
}

RBFW <- RBFW %>% 
  mutate(Age_st = standardizer(Age), 
         Day_of_year_st = standardizer(Day_of_year))
```

The plumage category and year variables are made to be factors. For
plumage category, DULL is set to be the reference category:

``` r
RBFW <- RBFW %>% 
  mutate(Plumage_group = factor(Plumage_group, levels = c("DULL", "MOULT", "BRIGHT")),
         Year = factor(Year))
```

Additionally, since courtship was only observed in BRIGHT individuals,
it is necessary to compare DULL and MOULTING males together to BRIGHT
males (rather than comparing DULL to MOULT and DULL to BRIGHT) to allow
for a non-zero estimate of variance in the courtship model.

``` r
RBFW <- RBFW %>%
  mutate(Plumage_group_combined = ifelse(Plumage_group == "BRIGHT", 
                                         "BRIGHT", "DULL + MOULT"),
         Plumage_group_combined = factor(Plumage_group_combined, 
                                         levels = c("DULL + MOULT", "BRIGHT")))
```

## Logistic Mixed-effects Models

Here, we create a function to produce the models described above:

``` r
# Formatting for graphs
theme_simple <- theme(panel.grid.major = element_blank(), 
                      panel.grid.minor = element_blank(),
                      panel.background = element_blank(), 
                      axis.line = element_line(colour = "black"), 
                      plot.title = element_text(hjust = 0))

class_colors <- c("navajowhite2", "orange2", "firebrick")
rain_color <- "cyan4"
  
# Fixed Effects 
general_fixed_effects <- paste("Plumage_group", "Year", 
                               "Age_st", "Day_of_year_st", 
                               sep = " + ")
court_fixed_effects <- paste("Plumage_group_combined", "Year", 
                             "Age_st", "Day_of_year_st", 
                             sep = " + ")

# Modeling Function
logistic_modeler <- function(dv, iv, df = RBFW){
  
  formula <- paste(dv, "~", iv, 
                   "+ (1| Observer) + (1 | Bird_ID)", 
                   collapse = " ") %>% 
    as.formula()
  
  model <- glmer(formula, data = df, family = "binomial",
                 control=glmerControl(optimizer="bobyqa", 
                                      optCtrl=list(maxfun=2e5)))
  
  visualization <- model %>%
    tidy() %>%
    filter(effect == "fixed") %>%
    filter(!(term %in% c("(Intercept)", 
                         "sd_(Intercept).Bird_ID", 
                         "sd_(Intercept).Observer"))) %>%
    mutate(lower = estimate - 1.96*std.error,
           upper = estimate + 1.96*std.error) %>%
    mutate(term = case_when(term == "Plumage_groupMOULT" ~ 
                              "Plumage group MOULT",
                            term == "Plumage_groupBRIGHT" ~ 
                              "Plumage group BRIGHT",
                            term == "Year2017" ~ 
                              "Year 2017",
                            term == "Year2018" ~ 
                              "Year 2018", 
                            term == "Age_st" ~ 
                              "Age",
                            term == "Day_of_year_st" ~ 
                              "Day of year",
                            term == "Plumage_group_combinedBRIGHT" ~ 
                              "Plumage group BRIGHT*"))  %>% 
    ggplot(aes(x = estimate, y = term)) + 
    geom_point() + 
    geom_linerange(aes(xmin = lower, xmax = upper)) + 
    theme_bw() +
    theme(plot.title = element_text(hjust = 0.5)) +
    labs(x = "Model Estimate", y = "Fixed Effect") + 
    geom_vline(xintercept = 0, lty = 2, color = "red") + 
    ggtitle(str_remove(dv, fixed("_bin")))
  
  results <- list(summary(model), visualization)
  
  list <- list(model, results)
  
  return(list)
}
```

Applying this function to our variables of interest, produces the
following results:

``` r
logistic_modeler("Allopreening_bin", general_fixed_effects)
```

    ## [[1]]
    ## Generalized linear mixed model fit by maximum likelihood (Laplace
    ##   Approximation) [glmerMod]
    ##  Family: binomial  ( logit )
    ## Formula: Allopreening_bin ~ Plumage_group + Year + Age_st + Day_of_year_st +  
    ##     (1 | Observer) + (1 | Bird_ID)
    ##    Data: df
    ##       AIC       BIC    logLik  deviance  df.resid 
    ##  245.7744  280.9716 -113.8872  227.7744       360 
    ## Random effects:
    ##  Groups   Name        Std.Dev.
    ##  Bird_ID  (Intercept) 0.9592  
    ##  Observer (Intercept) 0.0000  
    ## Number of obs: 369, groups:  Bird_ID, 116; Observer, 12
    ## Fixed Effects:
    ##         (Intercept)   Plumage_groupMOULT  Plumage_groupBRIGHT  
    ##            -3.26471              0.58685              1.12885  
    ##            Year2017             Year2018               Age_st  
    ##             0.25968              0.01959             -0.58101  
    ##      Day_of_year_st  
    ##            -0.37356  
    ## optimizer (bobyqa) convergence code: 0 (OK) ; 0 optimizer warnings; 1 lme4 warnings 
    ## 
    ## [[2]]
    ## [[2]][[1]]
    ## Generalized linear mixed model fit by maximum likelihood (Laplace
    ##   Approximation) [glmerMod]
    ##  Family: binomial  ( logit )
    ## Formula: Allopreening_bin ~ Plumage_group + Year + Age_st + Day_of_year_st +  
    ##     (1 | Observer) + (1 | Bird_ID)
    ##    Data: df
    ## Control: glmerControl(optimizer = "bobyqa", optCtrl = list(maxfun = 2e+05))
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    245.8    281.0   -113.9    227.8      360 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -0.7425 -0.3236 -0.2365 -0.1665  5.2509 
    ## 
    ## Random effects:
    ##  Groups   Name        Variance Std.Dev.
    ##  Bird_ID  (Intercept) 0.9201   0.9592  
    ##  Observer (Intercept) 0.0000   0.0000  
    ## Number of obs: 369, groups:  Bird_ID, 116; Observer, 12
    ## 
    ## Fixed effects:
    ##                     Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)         -3.26471    0.57545  -5.673  1.4e-08 ***
    ## Plumage_groupMOULT   0.58685    0.72451   0.810   0.4179    
    ## Plumage_groupBRIGHT  1.12885    0.55669   2.028   0.0426 *  
    ## Year2017             0.25968    0.68725   0.378   0.7055    
    ## Year2018             0.01959    0.59548   0.033   0.9738    
    ## Age_st              -0.58101    0.32206  -1.804   0.0712 .  
    ## Day_of_year_st      -0.37356    0.19493  -1.916   0.0553 .  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##             (Intr) P_MOUL P_BRIG Yr2017 Yr2018 Age_st
    ## Plmg_gMOULT -0.200                                   
    ## Plmg_BRIGHT -0.295  0.516                            
    ## Year2017    -0.407 -0.410 -0.429                     
    ## Year2018    -0.406 -0.222 -0.428  0.650              
    ## Age_st       0.187 -0.347 -0.567  0.457  0.386       
    ## Day_f_yr_st  0.155  0.087 -0.079  0.033  0.204  0.110
    ## optimizer (bobyqa) convergence code: 0 (OK)
    ## boundary (singular) fit: see help('isSingular')
    ## 
    ## 
    ## [[2]][[2]]

![](RBFW3_R_Code_figures_for_markdown/Model%20Results-1.png)<!-- -->

``` r
allo_model <- logistic_modeler("Allopreening_bin", 
                               general_fixed_effects)[[1]]

logistic_modeler("Courtship_bin", court_fixed_effects)
```

    ## [[1]]
    ## Generalized linear mixed model fit by maximum likelihood (Laplace
    ##   Approximation) [glmerMod]
    ##  Family: binomial  ( logit )
    ## Formula: 
    ## Courtship_bin ~ Plumage_group_combined + Year + Age_st + Day_of_year_st +  
    ##     (1 | Observer) + (1 | Bird_ID)
    ##    Data: df
    ##      AIC      BIC   logLik deviance df.resid 
    ## 125.8506 157.1370 -54.9253 109.8506      361 
    ## Random effects:
    ##  Groups   Name        Std.Dev.
    ##  Bird_ID  (Intercept) 0.000   
    ##  Observer (Intercept) 1.483   
    ## Number of obs: 369, groups:  Bird_ID, 116; Observer, 12
    ## Fixed Effects:
    ##                  (Intercept)  Plumage_group_combinedBRIGHT  
    ##                      -6.8109                        4.9703  
    ##                     Year2017                      Year2018  
    ##                      -1.1721                       -0.8621  
    ##                       Age_st                Day_of_year_st  
    ##                       0.1633                        0.2507  
    ## optimizer (bobyqa) convergence code: 0 (OK) ; 0 optimizer warnings; 1 lme4 warnings 
    ## 
    ## [[2]]
    ## [[2]][[1]]
    ## Generalized linear mixed model fit by maximum likelihood (Laplace
    ##   Approximation) [glmerMod]
    ##  Family: binomial  ( logit )
    ## Formula: 
    ## Courtship_bin ~ Plumage_group_combined + Year + Age_st + Day_of_year_st +  
    ##     (1 | Observer) + (1 | Bird_ID)
    ##    Data: df
    ## Control: glmerControl(optimizer = "bobyqa", optCtrl = list(maxfun = 2e+05))
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    125.9    157.1    -54.9    109.9      361 
    ## 
    ## Scaled residuals: 
    ##    Min     1Q Median     3Q    Max 
    ## -1.364 -0.148 -0.081 -0.019 38.731 
    ## 
    ## Random effects:
    ##  Groups   Name        Variance Std.Dev.
    ##  Bird_ID  (Intercept) 0.0      0.000   
    ##  Observer (Intercept) 2.2      1.483   
    ## Number of obs: 369, groups:  Bird_ID, 116; Observer, 12
    ## 
    ## Fixed effects:
    ##                              Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)                   -6.8109     1.5422  -4.416 1.00e-05 ***
    ## Plumage_group_combinedBRIGHT   4.9703     1.1043   4.501 6.77e-06 ***
    ## Year2017                      -1.1721     2.0535  -0.571    0.568    
    ## Year2018                      -0.8621     1.3713  -0.629    0.530    
    ## Age_st                         0.1633     0.2850   0.573    0.567    
    ## Day_of_year_st                 0.2507     0.2882   0.870    0.384    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##             (Intr) P__BRI Yr2017 Yr2018 Age_st
    ## Plm__BRIGHT -0.643                            
    ## Year2017    -0.397 -0.057                     
    ## Year2018    -0.477 -0.070  0.385              
    ## Age_st      -0.094 -0.192  0.196  0.122       
    ## Day_f_yr_st -0.097  0.013 -0.006  0.172  0.025
    ## optimizer (bobyqa) convergence code: 0 (OK)
    ## boundary (singular) fit: see help('isSingular')
    ## 
    ## 
    ## [[2]][[2]]

![](RBFW3_R_Code_figures_for_markdown/Model%20Results-2.png)<!-- -->

``` r
court_model <- logistic_modeler("Courtship_bin", 
                                court_fixed_effects)[[1]]

logistic_modeler("Chasing_bin", general_fixed_effects)
```

    ## [[1]]
    ## Generalized linear mixed model fit by maximum likelihood (Laplace
    ##   Approximation) [glmerMod]
    ##  Family: binomial  ( logit )
    ## Formula: Chasing_bin ~ Plumage_group + Year + Age_st + Day_of_year_st +  
    ##     (1 | Observer) + (1 | Bird_ID)
    ##    Data: df
    ##      AIC      BIC   logLik deviance df.resid 
    ## 178.7901 213.9873 -80.3951 160.7901      360 
    ## Random effects:
    ##  Groups   Name        Std.Dev.
    ##  Bird_ID  (Intercept) 5.1428  
    ##  Observer (Intercept) 0.5471  
    ## Number of obs: 369, groups:  Bird_ID, 116; Observer, 12
    ## Fixed Effects:
    ##         (Intercept)   Plumage_groupMOULT  Plumage_groupBRIGHT  
    ##             -8.6719               0.1195              -0.7026  
    ##            Year2017             Year2018               Age_st  
    ##              4.3083               2.7561               1.2816  
    ##      Day_of_year_st  
    ##              0.6626  
    ## 
    ## [[2]]
    ## [[2]][[1]]
    ## Generalized linear mixed model fit by maximum likelihood (Laplace
    ##   Approximation) [glmerMod]
    ##  Family: binomial  ( logit )
    ## Formula: Chasing_bin ~ Plumage_group + Year + Age_st + Day_of_year_st +  
    ##     (1 | Observer) + (1 | Bird_ID)
    ##    Data: df
    ## Control: glmerControl(optimizer = "bobyqa", optCtrl = list(maxfun = 2e+05))
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    178.8    214.0    -80.4    160.8      360 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -2.1259 -0.0632 -0.0313 -0.0143  5.4500 
    ## 
    ## Random effects:
    ##  Groups   Name        Variance Std.Dev.
    ##  Bird_ID  (Intercept) 26.4486  5.1428  
    ##  Observer (Intercept)  0.2993  0.5471  
    ## Number of obs: 369, groups:  Bird_ID, 116; Observer, 12
    ## 
    ## Fixed effects:
    ##                     Estimate Std. Error z value Pr(>|z|)   
    ## (Intercept)          -8.6719     3.3460  -2.592  0.00955 **
    ## Plumage_groupMOULT    0.1195     1.3778   0.087  0.93091   
    ## Plumage_groupBRIGHT  -0.7026     1.4449  -0.486  0.62678   
    ## Year2017              4.3083     2.2048   1.954  0.05070 . 
    ## Year2018              2.7561     1.6588   1.662  0.09661 . 
    ## Age_st                1.2816     0.7099   1.805  0.07103 . 
    ## Day_of_year_st        0.6626     0.4355   1.521  0.12817   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##             (Intr) P_MOUL P_BRIG Yr2017 Yr2018 Age_st
    ## Plmg_gMOULT  0.277                                   
    ## Plmg_BRIGHT  0.490  0.624                            
    ## Year2017    -0.706 -0.443 -0.664                     
    ## Year2018    -0.616 -0.342 -0.586  0.743              
    ## Age_st      -0.484 -0.386 -0.653  0.608  0.388       
    ## Day_f_yr_st -0.581 -0.230 -0.488  0.536  0.500  0.433
    ## 
    ## [[2]][[2]]

![](RBFW3_R_Code_figures_for_markdown/Model%20Results-3.png)<!-- -->

``` r
chase_model <- logistic_modeler("Chasing_bin", 
                                general_fixed_effects)[[1]]

logistic_modeler("Preening_bin", general_fixed_effects)
```

    ## [[1]]
    ## Generalized linear mixed model fit by maximum likelihood (Laplace
    ##   Approximation) [glmerMod]
    ##  Family: binomial  ( logit )
    ## Formula: Preening_bin ~ Plumage_group + Year + Age_st + Day_of_year_st +  
    ##     (1 | Observer) + (1 | Bird_ID)
    ##    Data: df
    ##       AIC       BIC    logLik  deviance  df.resid 
    ##  502.9680  538.1652 -242.4840  484.9680       360 
    ## Random effects:
    ##  Groups   Name        Std.Dev.
    ##  Bird_ID  (Intercept) 0.1420  
    ##  Observer (Intercept) 0.3451  
    ## Number of obs: 369, groups:  Bird_ID, 116; Observer, 12
    ## Fixed Effects:
    ##         (Intercept)   Plumage_groupMOULT  Plumage_groupBRIGHT  
    ##            -0.45826              0.84203              0.50909  
    ##            Year2017             Year2018               Age_st  
    ##             0.84211              0.30300             -0.02386  
    ##      Day_of_year_st  
    ##            -0.09409  
    ## 
    ## [[2]]
    ## [[2]][[1]]
    ## Generalized linear mixed model fit by maximum likelihood (Laplace
    ##   Approximation) [glmerMod]
    ##  Family: binomial  ( logit )
    ## Formula: Preening_bin ~ Plumage_group + Year + Age_st + Day_of_year_st +  
    ##     (1 | Observer) + (1 | Bird_ID)
    ##    Data: df
    ## Control: glmerControl(optimizer = "bobyqa", optCtrl = list(maxfun = 2e+05))
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    503.0    538.2   -242.5    485.0      360 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -1.9276 -0.8566  0.5135  0.8816  1.4540 
    ## 
    ## Random effects:
    ##  Groups   Name        Variance Std.Dev.
    ##  Bird_ID  (Intercept) 0.02017  0.1420  
    ##  Observer (Intercept) 0.11913  0.3451  
    ## Number of obs: 369, groups:  Bird_ID, 116; Observer, 12
    ## 
    ## Fixed effects:
    ##                     Estimate Std. Error z value Pr(>|z|)  
    ## (Intercept)         -0.45826    0.27861  -1.645   0.1000  
    ## Plumage_groupMOULT   0.84203    0.43250   1.947   0.0515 .
    ## Plumage_groupBRIGHT  0.50909    0.31697   1.606   0.1082  
    ## Year2017             0.84211    0.56431   1.492   0.1356  
    ## Year2018             0.30300    0.42679   0.710   0.4777  
    ## Age_st              -0.02386    0.14279  -0.167   0.8673  
    ## Day_of_year_st      -0.09409    0.12051  -0.781   0.4349  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##             (Intr) P_MOUL P_BRIG Yr2017 Yr2018 Age_st
    ## Plmg_gMOULT -0.136                                   
    ## Plmg_BRIGHT -0.163  0.439                            
    ## Year2017    -0.439 -0.289 -0.347                     
    ## Year2018    -0.618 -0.174 -0.354  0.491              
    ## Age_st      -0.032 -0.339 -0.572  0.370  0.326       
    ## Day_f_yr_st -0.143  0.017 -0.032  0.095  0.238  0.037
    ## 
    ## [[2]][[2]]

![](RBFW3_R_Code_figures_for_markdown/Model%20Results-4.png)<!-- -->

``` r
preen_model <- logistic_modeler("Preening_bin", 
                                general_fixed_effects)[[1]]

logistic_modeler("Vocalizing_bin", general_fixed_effects)
```

    ## [[1]]
    ## Generalized linear mixed model fit by maximum likelihood (Laplace
    ##   Approximation) [glmerMod]
    ##  Family: binomial  ( logit )
    ## Formula: Vocalizing_bin ~ Plumage_group + Year + Age_st + Day_of_year_st +  
    ##     (1 | Observer) + (1 | Bird_ID)
    ##    Data: df
    ##       AIC       BIC    logLik  deviance  df.resid 
    ##  514.4123  549.6095 -248.2062  496.4123       360 
    ## Random effects:
    ##  Groups   Name        Std.Dev.
    ##  Bird_ID  (Intercept) 0.5415  
    ##  Observer (Intercept) 0.0000  
    ## Number of obs: 369, groups:  Bird_ID, 116; Observer, 12
    ## Fixed Effects:
    ##         (Intercept)   Plumage_groupMOULT  Plumage_groupBRIGHT  
    ##            0.268543            -0.422781            -0.627202  
    ##            Year2017             Year2018               Age_st  
    ##            0.799328             0.195405             0.164559  
    ##      Day_of_year_st  
    ##           -0.003065  
    ## optimizer (bobyqa) convergence code: 0 (OK) ; 0 optimizer warnings; 1 lme4 warnings 
    ## 
    ## [[2]]
    ## [[2]][[1]]
    ## Generalized linear mixed model fit by maximum likelihood (Laplace
    ##   Approximation) [glmerMod]
    ##  Family: binomial  ( logit )
    ## Formula: Vocalizing_bin ~ Plumage_group + Year + Age_st + Day_of_year_st +  
    ##     (1 | Observer) + (1 | Bird_ID)
    ##    Data: df
    ## Control: glmerControl(optimizer = "bobyqa", optCtrl = list(maxfun = 2e+05))
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##    514.4    549.6   -248.2    496.4      360 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -1.6946 -0.9816  0.6319  0.8446  1.4536 
    ## 
    ## Random effects:
    ##  Groups   Name        Variance Std.Dev.
    ##  Bird_ID  (Intercept) 0.2932   0.5415  
    ##  Observer (Intercept) 0.0000   0.0000  
    ## Number of obs: 369, groups:  Bird_ID, 116; Observer, 12
    ## 
    ## Fixed effects:
    ##                      Estimate Std. Error z value Pr(>|z|)  
    ## (Intercept)          0.268543   0.203914   1.317   0.1879  
    ## Plumage_groupMOULT  -0.422781   0.440327  -0.960   0.3370  
    ## Plumage_groupBRIGHT -0.627202   0.336573  -1.863   0.0624 .
    ## Year2017             0.799328   0.411475   1.943   0.0521 .
    ## Year2018             0.195405   0.325230   0.601   0.5480  
    ## Age_st               0.164559   0.154786   1.063   0.2877  
    ## Day_of_year_st      -0.003065   0.120620  -0.025   0.9797  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##             (Intr) P_MOUL P_BRIG Yr2017 Yr2018 Age_st
    ## Plmg_gMOULT -0.249                                   
    ## Plmg_BRIGHT -0.291  0.452                            
    ## Year2017    -0.320 -0.399 -0.459                     
    ## Year2018    -0.440 -0.202 -0.452  0.536              
    ## Age_st       0.025 -0.310 -0.552  0.480  0.372       
    ## Day_f_yr_st -0.137  0.032 -0.070  0.111  0.265  0.049
    ## optimizer (bobyqa) convergence code: 0 (OK)
    ## boundary (singular) fit: see help('isSingular')
    ## 
    ## 
    ## [[2]][[2]]

![](RBFW3_R_Code_figures_for_markdown/Model%20Results-5.png)<!-- -->

``` r
voc_model <- logistic_modeler("Vocalizing_bin", 
                              general_fixed_effects)[[1]]
```

## Table 1

Logistic mixed-effects models predicting the occurrence of each
behaviour of interest. Each model contained plumage category, year, age,
and day of year as fixed effects and the identity of the focal male and
observer as random effects.

``` r
null_model_comparer <- function(model, dv, df = RBFW){
  
  # Null model 
  null_formula <- paste(dv, "~", "1 + (1| Observer) + (1 | Bird_ID)", 
                        collapse = " ") %>% 
    as.formula()
  
  null_model <- glmer(null_formula, data = df, family = "binomial", 
                      control = glmerControl(optimizer="bobyqa", 
                                             optCtrl=list(maxfun=2e5)))
  null_loglik <- logLik(null_model)
  
  # Making dataframe
  model_output <- AICc(model)
  model_df <- tibble(Response_variable = dv, 
                     Formula = "Plumage_group + Year + Age_st + Day_of_year_st",
                     AICc = model_output, 
                     Pseudo_R2 = as.numeric(1 - logLik(model)/null_loglik))
  
  # ANOVA
  anova_df <- anova(model, null_model) %>%
    tidy() %>%
    filter(term == "model") %>% 
    select(statistic, df, p.value)
  
  final_df <- model_df %>% bind_cols(anova_df) 
  
  
  return(final_df)
}


# List of models 
models <- list(allo_model, chase_model, 
               court_model, preen_model, 
               voc_model)


# List of response variables 
variables_of_interest_bin <- list("Allopreening_bin", "Chasing_bin", 
                                  "Courtship_bin", "Preening_bin", 
                                  "Vocalizing_bin")


anova_models <- map2(models, variables_of_interest_bin, 
                     null_model_comparer) %>% 
  bind_rows() %>% 
  select(Response_variable, Formula, 
         everything())


table_1 <- anova_models %>%
  select(Response_variable, statistic, 
         df, p.value, 
         Pseudo_R2) %>%
  mutate(statistic = round(statistic, 3), 
         Pseudo_R2 = round(Pseudo_R2, 3),
         p.value = round(p.value, 3))

table_1 %>%
  kable()
```

| Response_variable | statistic |  df | p.value | Pseudo_R2 |
|:------------------|----------:|----:|--------:|----------:|
| Allopreening_bin  |    13.233 |   6 |   0.039 |     0.055 |
| Chasing_bin       |    10.887 |   6 |   0.092 |     0.063 |
| Courtship_bin     |    49.922 |   5 |   0.000 |     0.312 |
| Preening_bin      |     9.888 |   6 |   0.129 |     0.020 |
| Vocalizing_bin    |     5.451 |   6 |   0.487 |     0.011 |

## Figure 1

The average proportion of focal observations in which a male was
observed engaging in courtship (a) and allopreening (c). Error bars show
95% confidence intervals. b and d. Model estimates from the logistic
mixed-effects models with courtship (b) and allopreening (d) as the
response variables. Error bars show 95% confidence intervals. Confidence
intervals that do not overlap zero are bolded.

``` r
std_error <- function(x){
  
  sd(x)/sqrt(length(x))
}

summarizer_by_group <- function(group, var){
  RBFW %>% 
    select(c(group, var)) %>%
    group_by(!!sym(group)) %>%
    summarize_all(funs(mean, std_error)) %>%
    mutate(behaviour = var) %>%
    ungroup()
}

behaviour_plumage_summary_error_df <- lapply(as.list(c("Allopreening_bin", 
                                                       "Courtship_bin", 
                                                       "Preening_bin", 
                                                       "Vocalizing_bin", 
                                                       "Chasing_bin")), 
                                             function(x){
                                               
                                               summarizer_by_group(group = 
                                                                     "Plumage_group", 
                                                                   var = x)
                                             }) %>% 
  bind_rows() %>%
  mutate(lower = mean - 1.96*std_error,
         upper = mean + 1.96*std_error)


barplot_boundaries <- scale_y_continuous(breaks = seq(0, 0.25, 0.05), 
                                         limits = c(-0.025, 0.257))
axis_text_small <- theme(axis.text = element_text(size = 7))
axis_label_medium <- theme(axis.title = element_text(size = 10))
title_adjustment <- theme(plot.title = element_text(hjust = -0.31))
model_plot_boundaries <- scale_x_continuous(breaks = seq(-5, 7.5, 2.5), 
                                            limits = c(-5.25, 7.55))

allo_freq_plot  <- behaviour_plumage_summary_error_df %>% 
  filter(behaviour == "Allopreening_bin") %>%
  ggplot(aes(x = Plumage_group, y = mean, fill = Plumage_group)) + 
  geom_col(position = "dodge") + 
  geom_errorbar(aes(ymin = lower, ymax = upper), width = 0.5) + 
  scale_fill_manual(values = class_colors) + 
  theme_simple + 
  theme(legend.position = "none") + 
  labs(x = "Plumage Group", 
       y = "Prop. of Observations \n with Allopreening") +
  ggtitle("c.") + 
  barplot_boundaries + 
  axis_label_medium + 
  axis_text_small + 
  theme(plot.title = element_text(hjust = -0.26)) + 
  scale_x_discrete(labels = c("Unornamented", "Moulting", "Ornamented"))

court_freq_plot <- behaviour_plumage_summary_error_df %>% 
  filter(behaviour == "Courtship_bin") %>%
  ggplot(aes(x = Plumage_group, y = mean, fill = Plumage_group)) + 
  geom_col(position = "dodge") + 
  geom_errorbar(aes(ymin = lower, ymax = upper), width = 0.5) + 
  scale_fill_manual(values = class_colors) + 
  theme_simple + 
  theme(legend.position = "none") + 
  labs(x = "Plumage Group", y = "Prop. of Observations \n with Courtship") +
  ggtitle("a.") + 
  barplot_boundaries + 
  axis_text_small +
  axis_label_medium + 
  theme(plot.title = element_text(hjust = -0.26)) + 
  scale_x_discrete(labels = c("Unornamented", "Moulting", "Ornamented"))

label_bolder <- function(labels, labels_to_bold) {
  if (!is.factor(labels)) labels <- factor(labels)              
  labels_levels <- levels(labels)                               
  to_bold <- labels_to_bold %in% labels_levels
  if (all(to_bold)) {
    b_position <- purrr::map_int(labels_to_bold, ~which(.==labels_levels)) 
    b_vector <- rep("plain", length(labels_levels))               
    b_vector[b_position] <- "bold"                                  
    b_vector
  }
}


visualize_allo <- function(model, y_label, title, bold_terms){
  df <- model %>%
    tidy() %>%
    filter(!(term %in% "(Intercept)")) %>%
    filter(str_detect(term, pattern = fixed("sd_"), negate = TRUE)) %>% 
    mutate(lower = estimate - 1.96*std.error,
           upper = estimate + 1.96*std.error) %>%
    mutate(term = case_when(term == "Plumage_groupMOULT" ~ 
                              "Plumage Moulting",
                            term == "Plumage_groupBRIGHT" ~ 
                              "Plumage Ornamented",
                            term == "Year2017" ~ 
                              "Year 2017",
                            term == "Year2018" ~ 
                              "Year 2018", 
                            term == "Age_st" ~ 
                              "Age",
                            term == "Day_of_year_st" ~ 
                              "Day of year",
                            term == "Plumage_group_combinedBRIGHT" ~ 
                              "Plumage Ornamented*"))  %>% 
    mutate(term = factor(term, levels = rev(c("Plumage Ornamented", "Plumage Moulting", 
                                              "Year 2017", "Year 2018", 
                                              "Age", "Day of year"))))
  
  df %>% 
    ggplot(aes(x = estimate, y = term)) + 
    geom_point() + 
    geom_linerange(aes(xmin = lower, xmax = upper)) + 
    theme_simple +
    labs(x = "Model Estimate", y = y_label) + 
    geom_vline(xintercept = 0, lty = 2, color = "red") + 
    ggtitle(title) + 
    theme(axis.text.y = element_text(face = label_bolder(df$term, 
                                                         bold_terms))) + 
    axis_text_small + 
    axis_label_medium + 
    title_adjustment
}

visualize_court <- function(model, y_label, title, bold_terms){
  df <- model %>%
    tidy() %>%
    filter(!(term %in% "(Intercept)")) %>%
    filter(str_detect(term, pattern = fixed("sd_"), negate = TRUE)) %>%
    mutate(lower = estimate - 1.96*std.error,
           upper = estimate + 1.96*std.error) %>%
    mutate(term = case_when(term == "Plumage_groupMOULT" ~ 
                              "Plumage Moulting",
                            term == "Plumage_groupBRIGHT" ~ 
                              "Plumage Ornamented",
                            term == "Year2017" ~ 
                              "Year 2017",
                            term == "Year2018" ~ 
                              "Year 2018", 
                            term == "Age_st" ~ 
                              "Age",
                            term == "Day_of_year_st" ~ 
                              "Day of year",
                            term == "Plumage_group_combinedBRIGHT" ~ 
                              "Plumage Ornamented*"))  %>% 
    mutate(term = factor(term, levels = rev(c("Plumage Ornamented*", "Year 2017", 
                                              "Year 2018", "Age", 
                                              "Day of year"))))
  
  df %>% 
    ggplot(aes(x = estimate, y = term)) + 
    geom_point() + 
    geom_linerange(aes(xmin = lower, xmax = upper)) + 
    theme_simple +
    labs(x = "Model Estimate", y = y_label) + 
    geom_vline(xintercept = 0, lty = 2, color = "red") + 
    ggtitle(title) + 
    theme(axis.text.y = element_text(face = label_bolder(df$term, bold_terms))) + 
    axis_text_small + 
    axis_label_medium + 
    title_adjustment
}

allo_model_plot <- visualize_allo(allo_model, "Fixed Effects", 
                                  "d.", "Plumage Ornamented") + 
  model_plot_boundaries

court_model_plot <- visualize_court(court_model, "Fixed Effects", 
                                    "b.", "Plumage Ornamented*") + 
  model_plot_boundaries


# Combining Plots into a grid 
grid_poportion_and_model_plots <- grid.arrange(court_freq_plot, 
                                               court_model_plot,
                                               allo_freq_plot,
                                               allo_model_plot, 
                                               nrow = 2, 
                                               heights = c(10, 10), 
                                               widths = c(15, 20))
```

![](RBFW3_R_Code_figures_for_markdown/Major%20results%20figure-1.png)<!-- -->

## Reproducing Statistics Reported in Text

``` r
term_remover <- function(old_model, variable_to_remove){
  
  new_model <- update(old_model, as.formula(paste(".~.-", 
                                                  variable_to_remove)))
  return(new_model)
  
}

# Allopreening Model LRT
allo_model_nopl <- term_remover(allo_model, "Plumage_group")
anova(allo_model_nopl, allo_model, test = "LRT")
```

    ## Data: df
    ## Models:
    ## allo_model_nopl: Allopreening_bin ~ Year + Age_st + Day_of_year_st + (1 | Observer) + (1 | Bird_ID)
    ## allo_model: Allopreening_bin ~ Plumage_group + Year + Age_st + Day_of_year_st + (1 | Observer) + (1 | Bird_ID)
    ##                 npar    AIC    BIC  logLik deviance  Chisq Df Pr(>Chisq)
    ## allo_model_nopl    7 245.93 273.30 -115.96   231.93                     
    ## allo_model         9 245.77 280.97 -113.89   227.77 4.1522  2     0.1254

``` r
# Allopreening Wald Z
allo_model %>%
  tidy() %>%
  filter(term == "Plumage_groupBRIGHT")
```

    ## # A tibble: 1 × 7
    ##   effect group term                estimate std.error statistic p.value
    ##   <chr>  <chr> <chr>                  <dbl>     <dbl>     <dbl>   <dbl>
    ## 1 fixed  <NA>  Plumage_groupBRIGHT     1.13     0.557      2.03  0.0426

``` r
# Courtship Model LRT
court_model_nopl <- term_remover(court_model, "Plumage_group_combined")
anova(court_model_nopl, court_model)
```

    ## Data: df
    ## Models:
    ## court_model_nopl: Courtship_bin ~ Year + Age_st + Day_of_year_st + (1 | Observer) + (1 | Bird_ID)
    ## court_model: Courtship_bin ~ Plumage_group_combined + Year + Age_st + Day_of_year_st + (1 | Observer) + (1 | Bird_ID)
    ##                  npar    AIC    BIC  logLik deviance  Chisq Df Pr(>Chisq)    
    ## court_model_nopl    7 160.65 188.02 -73.324   146.65                         
    ## court_model         8 125.85 157.14 -54.925   109.85 36.798  1   1.31e-09 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# Number of observations of allopreening and courtship mentioned in the results 
RBFW %>% 
  group_by(Plumage_group) %>%
  summarize(Number_of_observations = n(), 
            Courtship_observations = sum(Courtship_bin == 1), 
            Allopreening_observations = sum(Allopreening_bin == 1))
```

    ## # A tibble: 3 × 4
    ##   Plumage_group Number_of_observations Courtship_observations Allopreening_obs…¹
    ##   <fct>                          <int>                  <int>              <int>
    ## 1 DULL                             194                      0                 14
    ## 2 MOULT                             42                      1                  5
    ## 3 BRIGHT                           133                     25                 19
    ## # … with abbreviated variable name ¹​Allopreening_observations

``` r
# Rate of courtship displays mentioned in discussion 
RBFW %>% 
  group_by(Plumage_group) %>%
  summarize(display_rate = mean(Courtship_prop))
```

    ## # A tibble: 3 × 2
    ##   Plumage_group display_rate
    ##   <fct>                <dbl>
    ## 1 DULL               0      
    ## 2 MOULT              0.00537
    ## 3 BRIGHT             0.0215

## Assessing Model Fit Using the DHARMa Package

``` r
# Creating a unique time variable to be able to look at temporal autocorrelation
RBFW_unique_times_df <- RBFW %>% 
  arrange(Date, Time) %>% 
  mutate(unique_relative_time = 1:nrow(.)) %>% 
  select(Observation_ID, unique_relative_time)

RBFW <- RBFW %>%
  left_join(RBFW_unique_times_df, by = "Observation_ID")


model_fit_vizualizer <- function(model, fixed_effects, df, label){
  
  # Extracting variable name
  variable_name <- str_extract(as.character(model@call)[2], 
                               "^[A-Z]{1}[a-z]+")
  plot_label <- paste(variable_name, label, sep = " ")
  
  # Running simulation
  sim <- simulateResiduals(fittedModel = model, plot = T, n = 1000)
  title(sub = plot_label, col.sub = "darkgreen")
  
  # General histogram
  hist(sim)
  title(sub = plot_label, col.sub = "darkgreen")
  
  # Plotting against predictors
  lapply(as.list(str_split(fixed_effects, pattern = fixed(" + "), 
                           simplify = TRUE)), 
         function(x){
           predictor <- unlist(df[, x])
           
           if(x == "Age_st"){
             predictor <- round(predictor, 3)
           }
           
           boxplot(sim$scaledResiduals ~ predictor, 
                   xlab = "Predictor", 
                   ylab = "Scaled Residuals")
           title(x, sub = plot_label,
                 col.main = "darkgreen",
                 col.sub = "darkgreen")
         })
  
  # Goodness-of-fit tests
  testResiduals(sim)
  title(sub = plot_label, col.sub = "darkgreen")
  testZeroInflation(sim)
  title(sub = plot_label, col.sub = "darkgreen")
  testTemporalAutocorrelation(sim, df$unique_relative_time)
  title(sub = plot_label, col.sub = "darkgreen")
}
```

``` r
lapply(models[-3], function(x){
  model_fit_vizualizer(x, "Plumage_group + Year + Age_st", RBFW, label = "Model Fit")
})
```

![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-1.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-2.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-3.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-4.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-5.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-6.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-7.png)<!-- -->

    ## $uniformity
    ## 
    ##  Asymptotic one-sample Kolmogorov-Smirnov test
    ## 
    ## data:  simulationOutput$scaledResiduals
    ## D = 0.036695, p-value = 0.7031
    ## alternative hypothesis: two-sided
    ## 
    ## 
    ## $dispersion
    ## 
    ##  DHARMa nonparametric dispersion test via sd of residuals fitted vs.
    ##  simulated
    ## 
    ## data:  simulationOutput
    ## dispersion = 1.0079, p-value = 0.924
    ## alternative hypothesis: two.sided
    ## 
    ## 
    ## $outliers
    ## 
    ##  DHARMa bootstrapped outlier test
    ## 
    ## data:  simulationOutput
    ## outliers at both margin(s) = 0, observations = 369, p-value = 1
    ## alternative hypothesis: two.sided
    ##  percent confidence interval:
    ##  0 0
    ## sample estimates:
    ## outlier frequency (expected: 0 ) 
    ##                                0

![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-8.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-9.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-10.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-11.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-12.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-13.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-14.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-15.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-16.png)<!-- -->

    ## $uniformity
    ## 
    ##  Asymptotic one-sample Kolmogorov-Smirnov test
    ## 
    ## data:  simulationOutput$scaledResiduals
    ## D = 0.079665, p-value = 0.01849
    ## alternative hypothesis: two-sided
    ## 
    ## 
    ## $dispersion
    ## 
    ##  DHARMa nonparametric dispersion test via sd of residuals fitted vs.
    ##  simulated
    ## 
    ## data:  simulationOutput
    ## dispersion = 0.67161, p-value = 0.212
    ## alternative hypothesis: two.sided
    ## 
    ## 
    ## $outliers
    ## 
    ##  DHARMa bootstrapped outlier test
    ## 
    ## data:  simulationOutput
    ## outliers at both margin(s) = 0, observations = 369, p-value = 1
    ## alternative hypothesis: two.sided
    ##  percent confidence interval:
    ##  0 0
    ## sample estimates:
    ## outlier frequency (expected: 0 ) 
    ##                                0

![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-17.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-18.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-19.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-20.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-21.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-22.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-23.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-24.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-25.png)<!-- -->

    ## $uniformity
    ## 
    ##  Asymptotic one-sample Kolmogorov-Smirnov test
    ## 
    ## data:  simulationOutput$scaledResiduals
    ## D = 0.033372, p-value = 0.8057
    ## alternative hypothesis: two-sided
    ## 
    ## 
    ## $dispersion
    ## 
    ##  DHARMa nonparametric dispersion test via sd of residuals fitted vs.
    ##  simulated
    ## 
    ## data:  simulationOutput
    ## dispersion = 1.0086, p-value = 0.82
    ## alternative hypothesis: two.sided
    ## 
    ## 
    ## $outliers
    ## 
    ##  DHARMa bootstrapped outlier test
    ## 
    ## data:  simulationOutput
    ## outliers at both margin(s) = 0, observations = 369, p-value = 1
    ## alternative hypothesis: two.sided
    ##  percent confidence interval:
    ##  0 0
    ## sample estimates:
    ## outlier frequency (expected: 0 ) 
    ##                                0

![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-26.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-27.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-28.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-29.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-30.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-31.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-32.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-33.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-34.png)<!-- -->

    ## $uniformity
    ## 
    ##  Asymptotic one-sample Kolmogorov-Smirnov test
    ## 
    ## data:  simulationOutput$scaledResiduals
    ## D = 0.032157, p-value = 0.84
    ## alternative hypothesis: two-sided
    ## 
    ## 
    ## $dispersion
    ## 
    ##  DHARMa nonparametric dispersion test via sd of residuals fitted vs.
    ##  simulated
    ## 
    ## data:  simulationOutput
    ## dispersion = 1.0015, p-value = 0.96
    ## alternative hypothesis: two.sided
    ## 
    ## 
    ## $outliers
    ## 
    ##  DHARMa bootstrapped outlier test
    ## 
    ## data:  simulationOutput
    ## outliers at both margin(s) = 0, observations = 369, p-value = 1
    ## alternative hypothesis: two.sided
    ##  percent confidence interval:
    ##  0 0
    ## sample estimates:
    ## outlier frequency (expected: 0 ) 
    ##                                0

![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-35.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-36.png)<!-- -->

    ## [[1]]
    ## NULL
    ## 
    ## [[2]]
    ## NULL
    ## 
    ## [[3]]
    ## NULL
    ## 
    ## [[4]]
    ## NULL

``` r
model_fit_vizualizer(models[[3]], "Plumage_group_combined + Year + Age_st",
                     RBFW, label = "Model Fit")
```

![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-37.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-38.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-39.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-40.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-41.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-42.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-43.png)<!-- -->

    ## $uniformity
    ## 
    ##  Asymptotic one-sample Kolmogorov-Smirnov test
    ## 
    ## data:  simulationOutput$scaledResiduals
    ## D = 0.034455, p-value = 0.7734
    ## alternative hypothesis: two-sided
    ## 
    ## 
    ## $dispersion
    ## 
    ##  DHARMa nonparametric dispersion test via sd of residuals fitted vs.
    ##  simulated
    ## 
    ## data:  simulationOutput
    ## dispersion = 1.1856, p-value = 0.57
    ## alternative hypothesis: two.sided
    ## 
    ## 
    ## $outliers
    ## 
    ##  DHARMa bootstrapped outlier test
    ## 
    ## data:  simulationOutput
    ## outliers at both margin(s) = 0, observations = 369, p-value = 1
    ## alternative hypothesis: two.sided
    ##  percent confidence interval:
    ##  0.000000000 0.002710027
    ## sample estimates:
    ## outlier frequency (expected: 0.00024390243902439 ) 
    ##                                                  0

![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-44.png)<!-- -->![](RBFW3_R_Code_figures_for_markdown/Assessing%20model%20fit-45.png)<!-- -->

# Supplemental Material

The following analyses are included in a Microsoft Word document as
Supplemental Material for the manuscript. This document can be accessed
via the following DOI link:
<https://doi.org/10.6084/m9.figshare.15088137>

## Table S1

Description of behavioural categories used during focal observations.

This table includes only qualitative descriptions of behaviours and was
generated entirely in Microsoft Word.

## Table S2

Proportion of the opportunistically observed interactions in which a
dyad of RBFWs were (2) each other’s first (in the case of repairing)
social mate in the breeding season after the nonbreeding season in which
they were observed, (2) members of the same breeding group during the
breeding season preceding the nonbreeding season in which the
interaction occurred, (3) two males that were in different breeding
groups during the previous breeding season, and (4) a male and female
that were neither social mates during the following breeding season nor
members of the same breeding group during the preceding breeding season.
The numbers in parentheses report the number of observations of a
particular behaviour in which complete information for both interacting
birds with respect to the previously described social relationships were
known.

\* Please note that the version of Table S2 produced by the code below
differs slightly from Table S2 in the Word document accessed by the DOI
provided above. Specifically, due to an error in our data that we
noticed after we submitted the Word document of supplemental material to
*Emu* (but before we finalized our R code) a single observation of
courtship is now excluded from Table S2. This changed the last row of
the table slightly in the following way (from left to right):

- 19% (29 of 154) became 19% (29 of 153)
- 11% (17 of 152) became 11% (16 of 151)
- 16% (24 of 152) became 16% (24 of 151)
- 62% (89 of 144) became 62% (89 of 143)

Since the percentages below were not affected (nor were the qualitative
conclusions drawn from this table), we did not update the Microsoft Word
document containing the supplemental material submitted to *Emu*. We
apologize for this error.

``` r
# Adding and defining additional social contexts to opportunistic data
RBFW_sup <- RBFW_sup %>% 
  mutate(Male_male = case_when(Sender_sex == "M" & Receiver_sex == "M" ~ 1, 
                               is.na(Sender_sex) | is.na(Receiver_sex) ~ NA_real_,
                               TRUE ~ 0), 
         Male_female = case_when(Sender_sex == "M" & Receiver_sex == "M" ~ 0,
                                 Sender_sex == "F" & Receiver_sex == "F" ~ 0,
                                 is.na(Sender_sex) | is.na(Receiver_sex) ~ NA_real_,
                                 TRUE ~ 1), 
         Female_female = case_when(Sender_sex == "F" & Receiver_sex == "F" ~ 1, 
                                   is.na(Sender_sex) | is.na(Receiver_sex) ~ NA_real_,
                                   TRUE ~ 0), 
         Extra_group_males = case_when(Male_male == 1 & Prev_breeding_group == 0 ~ 1, 
                                       is.na(Male_male) | is.na(Prev_breeding_group) ~ NA_real_, 
                                       TRUE ~ 0), 
         Potential_extra_pair_mates = case_when(Male_female == 1 & Social_mate == 0 & Prev_breeding_group == 0 ~ 1, 
                                                is.na(Male_female) | is.na(Social_mate) | is.na(Prev_breeding_group) ~ NA_real_,
                                                TRUE ~ 0))

# Function for printing percent and sample size e.g., 45% (90 of 200)
percent_sample_size_calculator <- function(x){
  Sample_size <- sum((!is.na(x)))
  Sum <- sum(x, na.rm = TRUE)
  Percent <- round(Sum/Sample_size*100, 0)
  z <- paste(Percent, "%", " (", Sum, " of ", Sample_size, ")", sep = "")
  return(z)
}

table_s2 <- RBFW_sup %>%
  group_by(Behavior) %>%
  summarize(across(c(Social_mate, Prev_breeding_group, Extra_group_males, Potential_extra_pair_mates), percent_sample_size_calculator))

table_s2 %>% 
  kable()
```

| Behavior     | Social_mate      | Prev_breeding_group | Extra_group_males | Potential_extra_pair_mates |
|:-------------|:-----------------|:--------------------|:------------------|:---------------------------|
| Allopreening | 38% (122 of 323) | 43% (124 of 291)    | 9% (27 of 291)    | 21% (57 of 267)            |
| Chasing      | 13% (22 of 169)  | 8% (12 of 157)      | 23% (36 of 157)   | 43% (63 of 146)            |
| Courtship    | 19% (29 of 153)  | 11% (16 of 151)     | 16% (24 of 151)   | 62% (89 of 143)            |

## Figure S1

Histograms of the number of observations conducted on males belonging to
different plumage categories over the course of the three field seasons
of the study. The bin size is one week.

``` r
boundary_dates <- c("20160601", "20170601", "20180601", 
                    "20160701", "20170701", "20180701", 
                    "20160801", "20170801", "20180801")
date_breaks <- tibble(dates = ymd(boundary_dates), 
                      year = year(dates), 
                      week = week(dates), 
                      month = month(dates, label = TRUE)) %>%
  group_by(month) %>%
  summarize(values = mean(week)) %>% pull(values)

figure_s1 <- RBFW %>% 
  mutate(Plumage_group = case_when(Plumage_group == "DULL" ~ "Unornamented", 
                                   Plumage_group == "MOULT" ~ "Moulting", 
                                   Plumage_group == "BRIGHT" ~ "Ornamented"),
         Plumage_group = factor(Plumage_group, 
                                levels = c("Unornamented", "Moulting", "Ornamented"))) %>%
  mutate(Year = year(Date), 
         Week = week(Date)) %>%
  select(Plumage_group, Year, Week) %>%
  group_by(Plumage_group,Year, Week) %>%
  summarize(n = n()) %>%
  ungroup() %>%
  ggplot(aes(x = Week, y = n, fill = Plumage_group)) + 
  geom_col() + 
  facet_wrap(Plumage_group ~Year) + 
  scale_fill_manual(values = class_colors) + 
  theme_bw() +
  theme(legend.position = "none") + 
  ylab("Number of Observations") + 
  xlab("Date") + 
  scale_x_continuous(breaks = date_breaks, labels = c("Jun", "Jul", "Aug"), 
                     limits = c(21, 32))

figure_s1
```

![](RBFW3_R_Code_figures_for_markdown/Seasonality%20Figure-1.png)<!-- -->

## Figure S2

a\. Proportion of colour-banded males observed in each plumage category
during the three years of the study. A single male can be represented in
multiple plumage categories within a year if he was observed moulting
into ornamented plumage. b. Proportion of behavioural observations each
year conducted on the three plumage categories.

``` r
year_unique_males <- RBFW %>%
  mutate(Bird_ID_plumage = paste(Bird_ID, Plumage_group, sep = "")) %>%
  group_by(Year) %>%
  summarize(Sample_size = length(unique(Bird_ID_plumage)))


figure_s2_a <- RBFW %>%
  mutate(Plumage_group = case_when(Plumage_group == "DULL" ~ "Unornamented", 
                                   Plumage_group == "MOULT" ~ "Moulting", 
                                   Plumage_group == "BRIGHT" ~ "Ornamented"), 
         Plumage_group = factor(Plumage_group, 
                                levels = c("Unornamented", "Moulting", "Ornamented"))) %>% 
  left_join(year_unique_males, by = "Year") %>%
  group_by(Year, Plumage_group, Bird_ID, Sample_size) %>% 
  summarize(n = n()) %>%
  ungroup() %>%
  group_by(Year, Plumage_group, Sample_size) %>%
  summarize(n = n()) %>% 
  ungroup() %>%
  mutate(Proportion = n/Sample_size) %>%
  ggplot(aes(x = Year, y = Proportion, fill = Plumage_group)) + 
  geom_col(position = "dodge", color = "black") + 
  scale_fill_manual(values = class_colors, name = "") +
  theme_simple + 
  labs(y = "Proportion of Unique \n Males Observed", 
       fill = "Plumage group") +
  ggtitle("a.") + 
  ylim(0, 1) + 
  theme(plot.title = element_text(hjust = -0.24))


year_sample_size <- RBFW %>%
  group_by(Year) %>%
  summarize(Sample_size = n())


figure_s2_b <- RBFW %>%
  mutate(Plumage_group = case_when(Plumage_group == "DULL" ~ "Unornamented", 
                                   Plumage_group == "MOULT" ~ "Moulting", 
                                   Plumage_group == "BRIGHT" ~ "Ornamented"), 
         Plumage_group = factor(Plumage_group, 
                                levels = c("Unornamented", "Moulting", "Ornamented"))) %>% 
  left_join(year_sample_size, by = "Year") %>%
  group_by(Year, Plumage_group, Sample_size) %>%
  summarize(Year_sum = n()) %>%
  ungroup() %>%
  mutate(Proportion = Year_sum/Sample_size) %>%
  ggplot(aes(x = Year, y = Proportion, 
             fill = Plumage_group)) + 
  geom_col(position = "dodge", color = "black") + 
  theme_simple + 
  scale_fill_manual(values = class_colors, name = "") + 
  labs(y = "Proportion of Observations", fill = "") +
  ggtitle("b.") + 
  ylim(0, 1) + 
  theme(plot.title = element_text(hjust = -0.17))


figure_s2 <- ggarrange(figure_s2_a, figure_s2_b, ncol=2, nrow=1, 
                       common.legend = TRUE, legend="bottom")
figure_s2
```

![](RBFW3_R_Code_figures_for_markdown/Year%20plot-1.png)<!-- -->

## Figure S3

Timeline of focal males included in 5-minute behavioural observations. A
male observed in all three years of the study would be presented as a
horizontal line across the entire x-axis. The lines are coloured to
represent the number of plumage categories that a particular male was
observed in during focal behavioural observations. The portion of a line
that is shaded does not reflect the date that a male transitioned
between plumage categories nor the proportion of observations in which
he was observed in a particular plumage category.

``` r
# Summarize how many plumage groups each bird was observed in for each year of the study
heat_map_df <- RBFW %>%
  count(Bird_ID, Year, Plumage_group) %>%
  mutate(across(c(Bird_ID, Plumage_group), ~ as.character(.x)), 
         Year = as.numeric(as.character(Year))) %>%
  #select(-n) %>%
  pivot_wider(names_from = Plumage_group, values_from = n) %>%
  mutate(across(DULL:MOULT, ~ifelse(is.na(.x), 0, 1)),
         DMB = paste0(DULL, MOULT, BRIGHT)) 

heat_map_list <- heat_map_df %>%
  mutate(unique = row.names(.)) %>%
  split(.$unique) 

# This function takes each row from heat_map_df (a single bird in a single year of the study) and transforms it into a new data frame where each row represents a single colored segment in the timeline visualization
timeline_df_maker <- function(x){
  
  year = x$Year
  
  if(x$DMB %in% c("100", "010", "001")){
    new_df <- data.frame(Bird_ID = x$Bird_ID, Year = year, Start = year, End = year + 1, DMB = x$DMB)
    
    if(x$DMB == "100"){
      new_df$Color <- "Unornamented"
    }
    
    if(x$DMB == "010"){
      new_df$Color <- "Moulting"
    }
    
    if(x$DMB == "001"){
      new_df$Color <- "Ornamented"
    }
    
  }
  
  if(x$DMB %in% c("110", "011", "101")){
    
    new_df <- data.frame(Bird_ID = x$Bird_ID, Year = year, Start = c(year, year + 0.5), End = c(year + 0.5, year + 1), DMB = x$DMB)
    
    if(x$DMB == "110"){
      new_df$Color <- c("Unornamented", "Moulting")
    }
    
    if(x$DMB == "011"){
      new_df$Color <- c("Moulting", "Ornamented")
    }
    
    if(x$DMB == "101"){
      new_df$Color <- c("Unornamented", "Ornamented")
    }
    
  }
  
  if(x$DMB == "111"){
    
    new_df <- data.frame(Bird_ID = x$Bird_ID, Year = year, Start = c(year, year + 1/3, year + 2/3), End = c(year + 1/3, year + 2/3, year + 1), Color = c("Unornamented", "Moulting", "Ornamented"), DMB = x$DMB)
    
  }
  
  return(new_df)
}

timeline_df <- lapply(heat_map_list, timeline_df_maker) %>% bind_rows()


# Sorting based on length of row
sort_score_1_df <- timeline_df %>%
  count(Bird_ID, Year) %>%
  pivot_wider(names_from = Year, values_from = n, values_fill = 0, names_prefix = "Y") %>%
  ungroup() %>%
  group_by(Bird_ID) %>%
  summarize(Sort_score_1 = case_when(Y2016 > 0 && Y2017 == 0 && Y2018 == 0 ~ 700, 
                                     Y2016 > 0 && Y2017 > 0 && Y2018 == 0 ~ 600,
                                     Y2016 == 0 && Y2017 > 0 && Y2018 == 0 ~ 500,
                                     Y2016 == 0 && Y2017 > 0 && Y2018 > 0 ~ 400,
                                     Y2016 == 0 && Y2017 == 0 && Y2018 > 0 ~ 300,
                                     Y2016 > 0 && Y2017 > 0 && Y2018 > 0 ~ 100,
                                     Y2016 > 0 && Y2017 == 0 && Y2018 > 0 ~ 200,
                                     TRUE ~ NA_real_)) %>%
  ungroup() %>%
  select(Bird_ID, Sort_score_1)

sort_score_2_df <- timeline_df %>%
  select(Bird_ID, Year, DMB) %>%
  unique() %>%
  mutate(Plumage_diversity_score = case_when(DMB == "100" ~ 7,
                                             DMB == "010" ~ 6,
                                             DMB == "001" ~ 5,
                                             DMB == "110" ~ 4,
                                             DMB == "011" ~ 3, 
                                             DMB == "101" ~ 2, 
                                             DMB == "111" ~ 1, 
                                             TRUE ~ NA_real_)) %>%
  group_by(Bird_ID) %>%
  summarize(Sort_score_2 = sum(Plumage_diversity_score)) %>%
  ungroup()

sorted_birds <- sort_score_1_df %>%
  left_join(sort_score_2_df, by = "Bird_ID") %>%
  mutate(Sort_score = Sort_score_1 + Sort_score_2) %>%
  arrange(Sort_score) %>%
  pull(Bird_ID) %>%
  unique()

figure_s3 <- timeline_df %>%
  mutate(Bird_ID = factor(Bird_ID, levels = sorted_birds), 
         Color = factor(Color, levels = c("Unornamented", "Moulting", "Ornamented"))) %>%
  ggplot(aes(x = Year, y = Bird_ID)) + 
  geom_segment(aes(x = Start, xend = End, color = Color, yend = Bird_ID)) + 
  scale_color_manual(values = c("navajowhite2", "orange2", "firebrick")) + 
  theme_simple + 
  theme(axis.text.y = element_blank(), 
        axis.ticks.y = element_blank(), 
        legend.position = "top", 
        legend.text = element_text(size = 8)) + 
  labs(y = "Focal Bird", color = "") + 
  scale_x_continuous(breaks = c(2016, 2017, 2018) + 0.5, labels = c(2016, 2017, 2018))

figure_s3
```

![](RBFW3_R_Code_figures_for_markdown/Timeline%20Figure-1.png)<!-- -->

## Figure S4

Histograms showing the distribution of the proportion of time focal
males spent engaged in allopreening, chasing, courtship, preening, and
vocalizing.

``` r
figure_s4 <- RBFW %>% 
  select(Plumage_group, Allopreening_prop, Chasing_prop, Courtship_prop, Preening_prop, Vocalizing_prop) %>%
  pivot_longer(cols = ends_with("prop"), names_to = "Behavior", values_to = "Proportion") %>%
  mutate(Plumage_group = case_when(Plumage_group == "DULL" ~ "Unornamented", 
                                   Plumage_group == "MOULT" ~ "Moulting", 
                                   Plumage_group == "BRIGHT" ~ "Ornamented"),
         Plumage_group = factor(Plumage_group, 
                                levels = c("Unornamented", "Moulting", "Ornamented"))) %>%
  mutate(Behavior = str_sub(Behavior, start = 1, end = -5)) %>%
  ggplot(aes(x = Proportion, fill = Plumage_group)) + 
  geom_histogram(bins = 30) + 
  theme_simple + 
  facet_grid(Plumage_group ~ Behavior, scales = "free") + 
  scale_fill_manual(values = class_colors) + 
  theme(legend.position = "None") + 
  labs(x = "Proportion of Observation with Behaviour", y = "Number of Observations") + 
  theme(panel.background = element_rect(fill = NA, color = "black"))

figure_s4 
```

![](RBFW3_R_Code_figures_for_markdown/Histogram%20of%20proportion%20of%20behaviors-1.png)<!-- -->

## Figure S5

The average proportion of focal observations in which a male was
observed engaging in the five behaviours of interest. Error bars show
95% confidence intervals.

``` r
figure_s5 <- behaviour_plumage_summary_error_df %>% 
  mutate(behaviour = str_sub(behaviour, start = 1, end = -5)) %>% 
  ggplot(aes(x = Plumage_group, y = mean, fill = Plumage_group)) + 
  geom_col(position = "dodge") + 
  geom_errorbar(aes(ymin = lower, ymax = upper), width = 0.5) + 
  scale_fill_manual(values = class_colors) + 
  theme_simple + 
  theme(legend.position = "none") + 
  labs(x = "Plumage Group", y = "Proportion of Observations with Behaviour") +
  facet_wrap(~ behaviour) + 
  scale_x_discrete(labels = c("Unornamented", "Moulting", "Ornamented")) +
  theme(axis.text.x = element_text(size = 8))

figure_s5
```

![](RBFW3_R_Code_figures_for_markdown/Proportion%20of%20all%20observations%20containing%20focal%20behaviors-1.png)<!-- -->

## Figure S6

Heat map representing the proportion of opportunistic observations on
allopreening, chasing, and courtship that were between RBFW females and
the three male plumage categories. For chasing and courtship, a sender
(e.g., the RBFW chasing another or giving a courtship display) and
receiver are identified. For allopreening (mutual grooming), it was not
possible to assign a sender and receiver, so each of the interacting
RBFWs was arbitrarily assigned either “Bird A” or “Bird B” and the order
of the categories in the heat map is not meaningful.

``` r
# Adding more general categories (combining plumage category and sex)
RBFW_sup <- RBFW_sup %>%
  mutate(Sender_category = ifelse(Sender_sex == "M", paste(Sender_plumage_category, "male"), "Female"), 
         Receiver_category = ifelse(Receiver_sex == "M", paste(Receiver_plumage_category, "male"), "Female"))

plumage_figure_df <- RBFW_sup %>%
  select(Behavior, Sender_category, Receiver_category) %>%
  filter(complete.cases(.)) %>% # Filter out NA values
  count(Behavior, Sender_category, Receiver_category) %>%
  pivot_wider(names_from = Receiver_category, values_from = n) %>% 
  mutate(across(where(is.numeric), ~ ifelse(is.na(.x), 0, .x))) %>%
  mutate(Row_sum = Female + `Moulting male` + `Ornamented male` + `Unornamented male`) %>%
  ungroup() %>%
  group_by(Behavior) %>%
  mutate(Behavior_sum = sum(Row_sum)) %>%
  ungroup() %>%
  select(-Row_sum) %>%
  mutate(across(c("Female", "Moulting male", "Ornamented male", "Unornamented male"), ~ .x/Behavior_sum)) %>%
  select(-Behavior_sum) %>%
  pivot_longer(Female:`Unornamented male`, names_to = "Receiver_category", values_to = "Percent_observations")

# Modifed theme
s6_theme <- theme_simple + 
  theme(legend.position = "none", 
        axis.text.x = element_text(angle = 45, vjust = 1, hjust = 1, size = 8), 
        axis.text.y = element_text(size = 8), 
        plot.title = element_text(hjust = 0.5))

gradient_scale <- scale_fill_gradient(low = "#ffffff", high = "#7e2704", limits = c(0, max(plumage_figure_df$Percent_observations)))

# Chasing Figure
chasing_figure <- plumage_figure_df %>%
  filter(Behavior == "Chasing") %>%
  mutate(across(c(Sender_category, Receiver_category), ~ factor(.x, levels = c("Female", "Unornamented male", "Moulting male", "Ornamented male")))) %>%
  ggplot(aes(x = Sender_category, y = Receiver_category, fill = Percent_observations)) + 
  geom_tile(color = "black") + 
  s6_theme + 
  gradient_scale + 
  labs(x = "Sender", y = "Receiver") + 
  ggtitle("Chasing")

# Courtship Figure
courtship_figure <- plumage_figure_df %>%
  mutate(across(c(Sender_category, Receiver_category), ~ factor(.x, levels = c("Female", "Unornamented male", "Moulting male", "Ornamented male")))) %>% 
  mutate(Percent_observations = ifelse(Behavior == "Allopreening", 0, Percent_observations), 
         Behavior = ifelse(Behavior == "Allopreening", "Courtship", Behavior)) %>% # adding in 0 values for missing ones
  filter(Behavior == "Courtship") %>%
  group_by(Behavior, Sender_category, Receiver_category) %>%
  summarize(Percent_observations = sum(Percent_observations)) %>%
  ungroup() %>%
  ggplot(aes(x = Sender_category, y = Receiver_category, fill = Percent_observations)) + 
  geom_tile(color = "black") + 
  s6_theme + 
  gradient_scale + 
  labs(x = "Sender", y = "Receiver") + 
  ggtitle("Courtship")

# Allopreening figure
allo_matrix <- plumage_figure_df %>%
  filter(Behavior == "Allopreening") %>%
  mutate(across(c(Sender_category, Receiver_category), ~ factor(.x, levels = c("Female", "Unornamented male", "Moulting male", "Ornamented male")))) %>%
  select(-Behavior) %>%
  pivot_wider(names_from = Receiver_category, values_from = Percent_observations) %>%
  arrange(Sender_category) %>%
  select(Sender_category, Female, `Unornamented male`, `Moulting male`, `Ornamented male`) %>%
  as.matrix()

rownames(allo_matrix) <- allo_matrix[,1]
allo_matrix <- allo_matrix[,-1]
class(allo_matrix) <- "numeric"

# Matrix for inserting NA values 
null_matrix <- upper.tri(allo_matrix, diag = TRUE)
null_matrix[null_matrix == 0] <- NA_integer_

allo_matrix_final <- (allo_matrix + t(allo_matrix))*null_matrix
class(allo_matrix_final) <- "numeric"

allopreening_figure <- data.frame(allo_matrix_final) %>%
  mutate(Sender_category = rownames(.)) %>%
  pivot_longer(Female:`Ornamented.male`, names_to = "Receiver_category", values_to = "Percent_observations") %>%
  mutate(Receiver_category = str_replace_all(Receiver_category, fixed("."), " ")) %>%
  mutate(across(c(Sender_category, Receiver_category), ~ factor(.x, levels = c("Female", "Unornamented male", "Moulting male", "Ornamented male")))) %>% 
  ggplot(aes(x = Sender_category, y = Receiver_category, fill = Percent_observations)) + 
  geom_tile(color = "black") + 
  gradient_scale +
  s6_theme + 
  labs(x = "Bird A", y = "Bird B", fill = "Percent of Observations", color = "No data") + 
  ggtitle("Allopreening") 


mylegend <- g_legend(allopreening_figure + theme(legend.position = "right"))

figure_s6 <- ggarrange(allopreening_figure, chasing_figure, courtship_figure, mylegend,  ncol=2, nrow = 2)

figure_s6 
```

![](RBFW3_R_Code_figures_for_markdown/Heat%20map-1.png)<!-- -->
