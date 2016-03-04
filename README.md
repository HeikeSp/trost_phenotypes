# trost_phenotypes

## BBCH Data
## Plant Height Data
## FW/DW Ratios
## RWC Data
## Soil Moisture Sensor Data

## Yield Data Analysis

### Yield Data from TROST project (2011-2013)

* `yield_data.Rmd`

### Yield Data from VALDIS project (2014-2015)

* `yield_data_valdis.Rmd`
* use `func_get_yield_data` to retrieve raw data from Phenotyper DB &rarr; `yield_data`
* do some modifications on meta information (treatment, genotype)
* handle NAs, remove dupicate entries, find outlier &rarr; `yield_data_no_duplicates`
* create subsets for tuber FW, starch content (SC) and starch yield (SY) &rarr; examine histograms
* get subsets for single experiments and remove additional duplicates (e.g. `yield_data_67518`)
* extract the commonly used set of lines in 2014:
	* 192 were present in all 4 experiments + AR21 (not in MPI field, but belongs to SP1) &rarr; **finally 193 lines were considered (`lines_2014`)**
* extract the commonly used set of lines in 2015:
	* 60 were present in all 5 experiments (`common_lines_2015`)
	* 3 additional line were present in MPI FGH, MPI field and JKI shelter, but missing in Dethlingen and JKI field (`all_lines_2015`)
* **create correct yield data table**: calculate mean per plant_ID to get rid of replicated measurements (FW, SC) &rarr; **`yield_data_correct`**
* **replace NA values of SC by ZERO** if FW < 0.1kg for the respective plant &rarr; this will influence the calculation of mean SY in further analysis 
	* if FW < 0.1kg, SY will be zero (52 entries)
	* if FW > 0.1kg and if SC is NA, SY will be NA (9 entries)
* get information about subpopulations
	* `sp_infos` &rarr; every line is only listed once 
	* `sp_infos_dup` &rarr; every line that belongs to two different SPs is duplicated
* merge data from correct data from 2014 and 2015 with SP information &rarr; `yield_data_2014_sp_dup` and `yield_data_2015_sp_dup`
* create subsets for tuber FW, SC and SY &rarr; examine histograms
* calculate SY per experiment with `func_starch_yield_feld`
	* results in new table for each experiment (e.g. `mpi_fgh_2014`)
	* calculate coefficient of variation (CV) of SY per line and treatment of each experiment
* get subset for control conditions per experiment (e.g. `mpi_fgh_2014_control`)
	* calculate **mean** of starch yield in g per plant for each line (**only control data!**) (e.g. `mpi_fgh_2014_control_mean_per_line`)
	* calculate overall median of mean values per line (**only control data!**) &rarr; results in one value per experiment (e.g. `mpi_fgh_2014_control_median`)
	* calculate **median** of starch yield in g per plant for each line (**only control data!**) (e.g. `mpi_fgh_2014_control_median_per_line`)
* **normalize starch yield per experiment**: use control median of each experiment to calculate ratio &rarr; results in new column e.g. `mpi_fgh_2014$normalized_starch_yield_per_plant`
* **bind all processed yield data** per year &rarr; `yield_data_final_2014` and `yield_data_final_2015`
* the final dataset contains:
	* `tuber_FW_kg_per_plot`
	* `starch_g_per_kg`
	* `starch_yield_g_per_plant`
	* `starch_yield_g_per_plot`
	* `normalized_starch_yield_per_plant`
* get subset for control conditions of final data &rarr; `yield_data_final_2014_control` and `yield_data_final_2015_control`
	* calculate mean of starch yield per line and experiment (`yield_data_final_2014_control_sy_mean`)
	* calculate mean of NORMALIZED starch yield per line and experiment (`yield_data_final_2014_control_norm_sy_mean`)
* get subsets for subpopulations
* create explorative plots for:
	* abolute starch yield ~ treatment
	* normalized starch yield ~ treatment
	* absolute starch yield ~ treatment * plant line (also ordered)
	* normalized starch yield ~ treatment * plant line (also ordered)
	* absolute starch yield ~ treatment * SP
	* normalized starch yield ~ treatment * SP
* calculate ANOVA for starch yield ~ treatment * plant line
* calculate **stress index (SI)** = `1 - (mean(SY_drought) / mean(SY_control)`
	* `all_SI_list_2014`
	* `all_SI_list_2015`
	* or per experiment, e.g. `mpi_fgh_2014_si`
* calculate **Relative Starch Yield (RelSY)**, e.g. `mpi_fgh_2014_relSY`
	* single value per replicate of drought stress (`mpi_fgh_2014_relSY`)
	* median per line (`mpi_fgh_2014_relSY_median`)
* calculate **DRYM**
	* single value per replicate of drought stress (`mpi_fgh_2014_drym`)
	* median per line (`mpi_fgh_2014_drym_median`)
* combine DRYM median per line per experiment (`drym_experiments_2014` and `drym_experiments_2015`)
	* compare DRYM of SPs by t-test
	* get subset per experiment (`drym_mpi_fgh_2014`)
	* plot DRYM per experiment and SP (also as single lines, grouped by SP)
	* ANOVA of DRYM
	* plot DRYM versus normalized SY &rarr; **yield penalty!**
	* pairs plot of DRYM and cor.test (between experiments)
* plot of starch yield vs. starch content
* calculate **Stress Sensitivity Index (SSI)**
	* High value of SSI corresponds to sensitive genotype
	* Low value of SSI corresponds to tolerant genotype
* plot SSI versus DRYM
