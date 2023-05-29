# SS-DL_exemaple
# The Stock Synthesis Data-limited Tool (SS-DL tool)

The SS-DL tool was developed by Dr. Jason Cope (NWFSC - NOAA) and use Stock Synthesis (Methot and Wetzel 2013) to implement several standard data-limited assessment methods all in one modeling framework. Under a unified modeling framework, additional data can be added as it becomes available. The tool builds Stock Synthesis files for provided data and life history information. It produces full plots and tables for each model run via the r4ss package and additional screen output for straightforward interpretation
<br></br>
# Installing libraries 
```R
packages<-c("devtools","shiny","shinyjs","ggplot2","reshape2","dplyr",
"tidyr","rlist","viridis","shinyWidgets","shinyFiles","plyr","shinybusy",
"truncnorm","ggpubr","flextable","officer","gridExtra","wesanderson",
"data.table","adnuts","shinystan")

installed_packages <- packages %in% rownames(installed.packages())
if (any(installed_packages == FALSE)) {
  install.packages(packages[!installed_packages])
}

Make sure the following packages are using the most recent versions:
library(devtools)
devtools::install_github("shcaba/SSS", build_vignettes = TRUE)

install.packages("remotes")
remotes::install_github("r4ss/r4ss")
remotes::install_github("chantelwetzel-noaa/HandyCode")
remotes::install_github("nwfsc-assess/nwfscDiag")

It is recommended to make sure all additional open windows of R or Rstudio 
(beside the one being used) are closed prior to updating libraries, and that one 
restarts Rstudio after all new installations. 
Many of the errors when running the SS-DL tool arise from keeping libraries updated 
or installed (especially r4ss).
```

# Running the SS-DL tool

Running the tool can be accomplished inthe following way:

1. Access the repository [SS-DL-tool](https://github.com/shcaba/SS-DL-tool)

 - In "< > Code" Download the ZIP file
 - ![Imagem1](https://github.com/EidiK/SS-DL_example/assets/102596784/36aee7f9-2188-40e6-8fac-0892a75f4ec8)
