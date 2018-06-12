# Healthcare Project Proposal

## 1. Title

#### Relationship between Patient Experience rating, Mortality rate, and Total Hospitals (per capita) by County

## 2. Scope of research

#### The objective of our study is to explore relationships between Patient Experience rating, Mortality rate, and Total Hospitals (per capita) by County (Total Hospitals/Estimated Population) for year 2016


## 3. Team Members

#### Rabia Otry, Vilma Santos, Rohan Mohindroo, Angang Li

## 4. Project Description/Outline

- Aggregate data from multiple sources to create one Data Frame (DF).
- Add Patient Experience summary rating by Hospital from HCAPHS survey into DF (Rabia)
- Add mortality rate from CSV file into DF (Rohan)
- Add population size into DF  (Vilma)
- Print Data Frame
- Create visuals in Matplotlib, Seaborn, Tableu, etc.
  - Hospitals in Counties (heat map)
  - Total Hospitals (per capita) by County
  - Patient Experience vs Total Hospitals (per capita) by County
  - Mortality vs Total Hospitals (per capita) by County

## 5. Research Questions to Answer

#### Is there compelling relationship between Total Hospitals and Hospital Patient Experience? Total Hospitals and Mortality Rate? 

Specifically,

- Does # of Hospitals serving the population affect Hospital Patient Experience?
  - Does Population Size alone affect Hospital Patient Experience?
  - Does # of Survey Responders alone affect Hospital Patient Experience?
- Does # of Hospitals serving the population affect Mortality Rate?
  - Does Population Size alone affect Mortality Rate?
  - Does # of Hospitals alone affect Mortality Rate?
- Is there a direct relationship between # of Hospitals and Population Size?  

Note: Evolvement over time not possible since we only have 2016 data

## 6. Data Sets to be Used

- [HCAPHS data](https://data.medicare.gov/Hospital-Compare/Patient-survey-HCAHPS-Hospital/dgck-syfz), queried by hospital name from data.gov
- [Mortality data](https://wonder.cdc.gov/wonder/help/WONDER-API.html)
- [Census data](https://www2.census.gov/programs-surveys/popest/datasets/2010-2016/cities/totals/sub-est2016_all.csv)
