# Behavioural differences between ornamented and unornamented male Red-backed Fairywrens (*Malurus melanocephalus*) in the nonbreeding season
 This is the code for the following paper: 
 
 >  Hendrix, T.C., F. Fernandez-Duque, S. Toner, L.G. Hitt, R.G. Thady, M. Massa, S.J. Hagler, M. Armfield, N. Clarke, P. Honscheid, S. Khalil, C.E. Hawkins, S.M. Lantz, J.F. Welklin, J.P. Swaddle, M.S. Webster, and J. Karubian (In press) Behavioural differences between ornamented and unornamented male Red-backed Fairywrens (*Malurusmelanocephalus*) in the nonbreeding season. *Emu - Austral Ornithology*. [https://doi.org/10.1080/01584197.2023.2182224](https://doi.org/10.1080/01584197.2023.2182224)
 
 
## Project Abstract
The project is an analysis of behavioural differences between ornamented and unornamented male Red-backed Fairywrens (*Malurus melanocephalus*) in the nonbreeding season using logistic mixed-effects models. Abstract from the paper:

> During the breeding season, male Red-backed Fairywrens (*Malurus melanocephalus*) can exhibit ornamented (red-black) or unornamented (brown, resembling females and juveniles) plumage. These distinct plumage types represent alternative reproductive tactics and are associated with behavioural diﬀerences during the breeding season. However, we lack an understanding of whether and how these plumage types may be associated with behavioural diﬀerences during non-reproductive parts of the year. To ﬁll this knowledge gap, we carried out behavioural observations during the nonbreeding season across three years. We hypothesised that ornamented plumage remains associated with mate attraction behaviours outside of the breeding season. We examined the investment of ornamented, moulting, and unornamented males in social behaviours and found that the three plumage types were largely similar in their behaviour except ornamented males courted and, to a lesser extent, allopreened at higher rates than unornamented males. Since concurrent work in the same study population demonstrates increased extra-pair ﬁtness for males who moult into ornamented plumage early, we speculate that ornamentation and courtship behaviour may serve a mate attraction function outside of the breeding season. We argue that future studies should consider individual-level behavioural monitoring throughout the annual cycle to better quantify the complex selection pressures that lead to the coevolution of plumage moult and alternative reproductive tactics in this system.


## How to use this repository
This project is written entirely in the R language. The code has been organized such that a single R script can reproduce the statistical results, tables, figures, and supplemental materials for the *Emu* publication. This R script is available in several different formats that are identical in their content but have different use cases (described below). 

### Repository Structure

* `Data/`: contains the .csv files necessary to reproduce analyses. Also contains a README file ("README\_RBFW3_Data.md") that details the contents of these .csv files. 
* `fairywren-nonbreeding-behavior.Rproj`: an R project to facilitate running scripts. 
* `Output/`: A folder containing all the figures and tables presented in the publication and its supplemental materials. These are generated/updated by the R scripts in the `Scripts/` folder. 
* `Scripts/`: Contains three formats of the R code for the publication:
	+ `RBFW3_R_Code.md`: This is a Markdown document designed to be viewed on the GitHub website. It may be helpful to those interested in viewing the R code and its output (e.g., tables and figures) but not editing or running the code. The `RBFW3_R_Code_figures_for_markdown/` folder contains the .png files necessary to render the Markdown document.
	+ `RBFW3_R_Code.Rmd`: an R Markdown document that produces the `RBFW3_R_Code.md` document when "knit." This document may be of interest to those wanting to edit the code or run it in small code "chunks."
	+ `RBFW3_R_Code.R`: an alternative version of the above R Markdown document generated using the `purl` function from the `knitr` R package. It may be helpful to users who want to rerun analyses or edit code but prefer working with .R files.     


## Figshare 
The data, supplemental materials, and R code found in this GitHub repository have also been deposited in the figshare repository as a collection with the following DOI: [https://doi.org/10.6084/m9.figshare.c.5538159.v1](https://doi.org/10.6084/m9.figshare.c.5538159.v1)
