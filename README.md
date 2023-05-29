# The Stock Synthesis Data-limited Tool (SS-DL tool)

The SS-DL tool was developed by Dr. Jason Cope (NWFSC - NOAA) and use Stock Synthesis (Methot and Wetzel 2013) to implement several standard data-limited assessment methods all in one modeling framework. Under a unified modeling framework, additional data can be added as it becomes available. The tool builds Stock Synthesis files for provided data and life history information. It produces full plots and tables for each model run via the r4ss package and additional screen output for straightforward interpretation

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
<div>
<p style = 'text-align:center;'>
<img src="https://github.com/EidiK/SS-DL_example/assets/102596784/36aee7f9-2188-40e6-8fac-0892a75f4ec8" alt="JuveYell" width="650px">
</p>
</div>


2. Extract the folder **SS-DL-tool-master** and open the <img src="https://github.com/EidiK/SS-DL_example/assets/102596784/f959b660-8cee-48a7-a54b-42d2c2f91065" alt="JuveYell" width="70px"> or <img src="https://github.com/EidiK/SS-DL_example/assets/102596784/8e4e9532-7275-4f14-b9ff-d49babd79d52" alt="JuveYell" width="50px">files 

**Obs.** Before running the first time, open both `server.r` and `ui.r` files to make sure all packages are installed. If any package is not installed, RStudio will signal..

<div>
<p style = 'text-align:center;'>
<img src="https://github.com/EidiK/SS-DL_example/assets/102596784/6fb5fa85-f00d-413c-bc35-9aa7c38ad53e" alt="JuveYell" width="600px">
</p>
</div>

in RStudio and push the "`Run App`" button 

I recommend using the "`Run External`" option within the "`Run App`" button 
(see small arrow in button to change options)

<div>
<p style = 'text-align:center;'>
<img src="https://github.com/EidiK/SS-DL_example/assets/102596784/94312609-9610-4baa-9c8d-0d5775999ea4" alt="JuveYell" width="600px">
</p>
</div>


3. Shiny will open and you can start the example

![Imagem3b](https://github.com/EidiK/SS-DL_example/assets/102596784/0340d3e8-7740-403c-981d-55f3228f9d7c)

# Length-only models

These are akin to LBSPR (Hordyk et al. 2015) and LIME (Rudd and Thorson 2017). Both styles of length-only models can be performed in this tool (determined by the estimation of recruitment), and include a choice between estimating F (recommended) or constant catch approaches (for stock status only; the estimated F values will not be useful on an absolute scale).

# Example SS-LO Constant Catch

Using constant catch assumes the same catch in all years in order to fit the length composition data (similar to LBSPR, but the model integrates the fit of each year, not each year separately)

It provides a long-term average response (F) to estimating stock status, and thus useful with non-continous years of sampling.

## Import the Length composition

For the example, import the Length composition file "`Lengths.csv`" that is in the "Example data files" folder of the "SS-DL-tool-master".

<div>
<p style = 'text-align:center;'>
<img src="https://github.com/EidiK/SS-DL_example/assets/102596784/101c0440-8d07-4ad9-8966-6a68c732d218" alt="JuveYell" width="400px">
</p>
</div>


**Lengths.csv file setup**

The csv file must be formatted as follows: 
Year,Month,Fleet,Sex,Nsamps,(Vector of length bins associated with the length data).
As in the example:

|Year|Month|Fleet|Sex|Nsamps|20|25|30|35|40|45|50|55|60|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
|1993|7	|1	|1|	59|	0|	2|	7|	3|	15|	23|	7|	2|	0|
|1994|7	|1	|2|	73|	0|	1|	9|	18|	17|	14|	9|	4|	1|
|1993|7	|2	|0|	42|	1|	4|	11|	10|	6|	9|	1|	0|	0|
|...|

**Fleet** = sequential number for each fleet     
**Sex** = 0 means combined male and female    
**Sex** = 1 means female only   
**Sex** = 2 means male only     


If the file is filled in correctly, the compositions referring to the fleets and sexes will be plotted in the "Data and Parameters"


![Imagem4c](https://github.com/EidiK/SS-DL_example/assets/102596784/bad87ed1-d70e-4c79-aa02-d23aeb145564)
 

. In the first fleet the data are separated by sex 
(sex = 1 for females; sex = 2 for males)
. In the second fleet the sexes are grouped (set sex = 0)

## Approach and data weighting

1. Setup for the **Constant Catch** approach

2. **Weight fleet lengths by relative catch**
The relative catch contribution needs specification with multiple length-only fleets
Example: Two fleets, with fleet 2 catching 2 times the amount as fleet 1, the entry would be 1,2.
Each entry will be relative to the highest value.

3. **Data-weighting**
Data weighting balances information content of biological data with model structure
Data weighting balances across factors (e.g, fleets, sex, etc.)
The default value is equally weighting among fleets based on input effective sample size inputs
If using an existing model, chose 'None' to maintain the same weighting as the existing model

<div>
<p style = 'text-align:center;'>
<img src="https://github.com/EidiK/SS-DL_example/assets/102596784/38ae1c57-146a-452f-a9bf-8415bd7d4ec4" alt="JuveYell" width="500px">
</p>
</div>

## Life history inputs

To fill in the life history data use the example of what is in the repository <img src="https://github.com/EidiK/SS-DL_example/assets/102596784/d8f174f7-ddfa-4490-9b2d-80fccbdec9d3" alt="JuveYell" width="400px">

If you do not have separate sex data, it is possible to use only female information.

<div>
<p style = 'text-align:center;'>
<img src="https://github.com/EidiK/SS-DL_example/assets/102596784/ec4c211c-7575-483e-8c0b-51ab80f9c86f" alt="JuveYell" width="500px">
</p>
</div>

The parameters will be plotted in the "Data and Parameters"

![LHc](https://github.com/EidiK/SS-DL_example/assets/102596784/45145f5a-4995-4f83-9a3b-88bc32c272ad)


### Stock-recruitment parameters
Enter the Steepness value

Recruitment can also be estimated

<div>
<p style = 'text-align:center;'>
<img src="https://github.com/EidiK/SS-DL_example/assets/102596784/165cf58d-92cd-4b4f-9460-be229c1359a5" alt="JuveYell" width="400px">
</p>
</div>

## Selectivity

Enter parameter and phase values for each fleet and survey.

Example using 50% selectivity with two fleets: Inputs could be 35,40 and 2,2 for starting values and phases respectively.

The phase input indicates estimated parameters. To fix the parameter, set the phase value to a negative number.

If using a mix of logistic and dome-shaped selectivities, select dome-shaped and fix (i.e., use a negative phase) the provided default values (10000,0.0001,0.9999 for the additonal parameters, respectively) to achieve a logistic shape for any given fleet.

The selectivity data is also in the repository<img src="https://github.com/EidiK/SS-DL_example/assets/102596784/d8f174f7-ddfa-4490-9b2d-80fccbdec9d3" alt="JuveYell" width="400px">

![Sel](https://github.com/EidiK/SS-DL_example/assets/102596784/5b52d140-6261-4957-8dce-ddcb084f0dfa)

## Additional SS options

A list of additional options can be used in the model, for our example we will select the option “`Use only one growth type (default is 5)`”

The number of platoons within a growth type/morph allows exploration of size-dependent survivorship. A value of 1 will not create additional platoons. 

Odd-numbered values (i.e., 3, 5) will break the overall morph into that number of platoons creating a smaller, larger, and mean growth platoon. The higher the number of platoons the slower SS will run. 


<div>
<p style = 'text-align:center;'>
<img src="https://github.com/EidiK/SS-DL_example/assets/102596784/baf4e626-94c9-4ee4-b0ee-5c280b77b6ff" alt="JuveYell" width="400px">
</p>
</div> 

## Fininal step - Run the model

Choose a name for the scenario and the repository

<div>
<p style = 'text-align:center;'>
<img src="https://github.com/EidiK/SS-DL_example/assets/102596784/51794a83-bbae-435c-ab4c-138e8e845458" alt="JuveYell" width="400px">
</p>
</div>

If all the input data are in agreement the following image will appear  <img src="https://github.com/EidiK/SS-DL_example/assets/102596784/8cfd3fe2-b83d-4ff5-a48e-ad9e0ed37baf" alt="JuveYell" width="70px">

## Examining the results
