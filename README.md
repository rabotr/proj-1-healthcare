# Healthcare Project - Insights from Patient Experience Rating

We used open source healthcare data to explore relationships between patient experience rating, mortality rate, population, and the total number hospitals at county level for year 2016. Check out the presentation slides [here](Presentation/presentation_final.pptx)!

## 1. Data Sources

- [Hospital Consumer Assessment of Healthcare Providers and Systems (HCAPHS)](https://data.medicare.gov/Hospital-Compare/Patient-survey-HCAHPS-Hospital/dgck-syfz): Over 4,000 hospitals nationwide with patient experience ratings
- [Centers for Disease Control and Prevention (CDC)](https://wonder.cdc.gov/controller/datarequest/D77): nationwide mortality data by county
- [FIPS data](https://www.census.gov/geographies/reference-files/2016/demo/popest/2016-fips.html)
- [Land area data](https://www.census.gov/support/USACdataDownloads.html#LND)

## 2. Visualizations

### 2.1. Maps

<img src="Results/population_per_square_mile_by_county.png" alt="population_per_square_mile_by_county" width="600" height="300">

<img src="Results/number_of_completed_surveys_by_county.png" alt="number_of_completed_surveys_by_county" width="600" height="300">

### 2.2. Trends and Statistics

#### Number of hospitals vs population

<img src="Results/hospitals_vs_population.png" alt="hospitals_vs_population" width="600" height="300">

#### Average patient rating vs population

<img src="Results/rating_vs_population.png" alt="rating_vs_population" width="450" height="300">

#### Mortality vs population

<img src="Results/mortality_vs_population.png" alt="mortality_vs_population" width="450" height="300">

#### Number of hospitals vs mortality rate

<img src="Results/Number_of_Hospitals_vs_Age_Adjusted_Rate.png" alt="Number_of_Hospitals_vs_Age_Adjusted_Rate" width="450" height="300">

<img src="Results/Number_of_Hospitals_vs_Crude_Rate.png" alt="Number_of_Hospitals_vs_Crude_Rate" width="450" height="300">



## Conclusions

1. Is there a direct relationship between Number of Hospitals and Population Size?

  - The survey data are more reliable (larger sample size) for places with larger population.
  - The number of hospital increases with population size.

2. Does Number of Hospitals serving the population affect Hospital Patient Experience?

  - As population increased, patient experience ratings decreased.
  - As the total of completed surveys increased, patient experience ratings decreased; and as average completed surveys increased, patient experience ratings decreased.

3. Does Number of Hospitals serving the population affect Mortality Rate?

  - As population increases, deaths increase.
  - As Hospital per Capita (# of hospitals per population) and Number of Hospitals in a County increased, Mortality Rates (Crude Rate & Age Adjusted Rate) decreased. 
  - Further investigation is needed, for example, on hospitals by bed size to understand if overcapacity is a factor.
