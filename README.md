# Quebec_forest_recruitment_and_disturbances
 Repo for all the code of my M2 intership at the IRBV with Marie-Hélène Brice

## Context and summary of the study

*Context:* 

Climate change poses challenges to tree populations, leading to responses such as adaptation, migration, and extinction. In the Quebec ecotone, transition zone between temperate and boreal forest, multiple species reach their northern distribution limits and are predicted to undergo important northern migration. Understanding tree recruitment dynamic, often left out in forest models, could help anticipate distribution range shifts in the ecotone and adapt forest management.

*Method:* 

We examined the impact of disturbances on saplings (diameters [1;3] cm) recruitment using a Bayesian hurdle model with presence and abundance data from 1970 to 2021.

*Results:*

We found a significant range shifts for temperate species. Recruitment was primarily influenced by the presence of conspecific adults. Disturbances had diverse effects, generally benefiting Acer rubrum (red maple), a generalist and opportunistic temperate species. While clearcut and fire had a positive mid-term (10-50 years) impact on pioneer species, partial cut favored temperate species recruitment to a greater extent.

*Conclusion:*

Anthropogenic disturbances could promote certain species and facilitate latitudinal shift.  However, the importance of conspecific adult presence suggests that additional measures, such as assisted migration through plantation, may be necessary to ensure the northward shift of species distribution.


## Repo and analyse structure

### Data

Initial data are from the Minister : https://www.donneesquebec.ca/recherche/dataset/placettes-echantillons-permanentes-1970-a-aujourd-hui

```{r}
download.file("https://diffusion.mffp.gouv.qc.ca/Diffusion/DonneeGratuite/Foret/DONNEES_FOR_ECO_SUD/Placettes_permanentes/PEP_GPKG.zip", destfile = "raw_data/PEP.zip")
```

See beginning of `R/1_Make_data/tree_pep.R`

Then all selection and modification on this data are made with the files from
`R/1_Make_data` (1 to 8 have to bee run in order), the output dataframe is `data/full_data.RDS` and is the one I used for all following analyses

It is not necessary to run all the code to make the data, you can just use the output `data/full_data.RDS` and start with the analyses part.

#### Code in data

Table correspondance soil caracteristics/number :

|Code | Soil caracteristics |
|-----|----------|
| 0 | texture variée et drainage de xérique à hydrique |
| 1 | texture grossière et de drainage xérique ou mésique |
| 2 | texture moyenne et de drainage mésique |
| 3 | texture fine et de drainage mésique |
| 4 | texture grossière et de drainage subhydrique |
| 5 | texture moyenne et de drainage subhydrique |
| 6 | texture fine et de drainage subhydrique |
| 7 | drainage hydrique, ombrotrophe |
| 8 | drainage hydrique, minérotrophe |

### Analyses

#### 1. Analyses of the data : `R/2_Data_analysis`

File :
- `Data_analysis.Rmd` : Analyses of the data and make some figures and tables. Figures for the article made in this document and can be found in `/figures`.

Figures made :
- Map of the study area (Figure 1)
- number of disturbances by types and distribution of time since disturbance for each one
- PCA on soil and climate
- Link between basal area and disturbances
- Map of the presence absence of each species at the first and last inventory (Figure 2)
- link between species presence and climate (CMI and temperature)

#### 2. Analyses of space and time : `R/3_Space_time_model`

Code to make the space time frequentist model with glmmTMB (https://github.com/glmmTMB/glmmTMB).
Figure in my article in this part are made in this document and can be found in /figures.

#### 3. Analyses of disturbance effect : `R/4_Disturbance_model`

##### a. Choice of parameters

Covariable selection for subsequent bayesian model with buildmer (https://github.com/cvoeten/buildmer). Thanks to Cesko Voeten for this package.
Just ran different hurdle models for all species and step them to select important covariables.
Also a little bit of co-linearity analysis between variables.

##### b. Bayesian model

Bayesian model with JAGS (https://mcmc-jags.sourceforge.io/). Thanks to Martyn Plummer for this package.

With :
- `Model.txt` : The model specification
- `function_run.R` : Create the function to run the model with jags.parallel
(Make the specific data for the model)
- `Run.R` : Run the model for all the sppecies in parallel **!!! Uses a lot of core (30) so be careful to have enough on your system or decrease the number in :

```{r}
cl <- makeCluster(30)
```

Analyse of the model output are made in `Analyse.Rmd` and uses `function_analyses.R` to make the figures and tables. Figures in my article in this part are made in this document and can be found in `/figures`.

### Output

Output of the 8 individual model for each species was not uploaded here because of weight but you can find some of the data I extracted from them in `/output` :

- 
- 

# A faire :

IMPORTANT :

- [ ] Finaliser la figure de l'analyse space and time
- [ ] Refaire tourner les modèles avec 
- [ ] et sans BA
- [ ] (+ tableau de comparaison déviance + BIC)
- [ ] Faire des output utilisable : (tableau plus cours des résutats poir pouvoir les uploader sur Github dans output)
- [x] Relire le README
- [x] Relire le code
- [ ] Relire le rapport

ANECDOTIQUE :

- [ ] Simplifier les bordures de la carte (st simplify)
- [ ] Virer result.RDS