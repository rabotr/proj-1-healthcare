
### Visualizations

1. Does Population Size alone affect Hospital Patient Experience?
2. Does # of Survey Responders alone affect Hospital Patient Experience? (total completed surveys vs county avg rating, avg response rate vs county avg rating, avg completed survey vs county avg rating)

--- hospital count per county


```python
# import dependencies
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import scipy.stats as stats
```


```python
# read in final csv
data_df = pd.read_csv('Resources/final_cleaned_data.csv', index_col=0)

data_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>County</th>
      <th>City</th>
      <th>Hospital</th>
      <th>Location</th>
      <th>Patient Rating</th>
      <th>Completed Surveys</th>
      <th>Survey Response Rate (%)</th>
      <th>Deaths</th>
      <th>Population</th>
      <th>Crude Rate</th>
      <th>Age Adjusted Rate</th>
      <th>% of Total Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>MN</td>
      <td>HENNEPIN</td>
      <td>ROBBINSDALE</td>
      <td>NORTH MEMORIAL MEDICAL CENTER</td>
      <td>3300 OAKDALE NORTH\nROBBINSDALE, MN\n(45.01421...</td>
      <td>3</td>
      <td>957</td>
      <td>26</td>
      <td>8557</td>
      <td>1232483</td>
      <td>694.29</td>
      <td>634.22</td>
      <td>0.31</td>
    </tr>
    <tr>
      <th>1</th>
      <td>MN</td>
      <td>HENNEPIN</td>
      <td>MINNEAPOLIS</td>
      <td>HENNEPIN COUNTY MEDICAL CENTER 1</td>
      <td>701 PARK AVENUE\nMINNEAPOLIS, MN\n(44.97285, -...</td>
      <td>2</td>
      <td>694</td>
      <td>13</td>
      <td>8557</td>
      <td>1232483</td>
      <td>694.29</td>
      <td>634.22</td>
      <td>0.31</td>
    </tr>
    <tr>
      <th>2</th>
      <td>MN</td>
      <td>HENNEPIN</td>
      <td>SAINT LOUIS PARK</td>
      <td>PARK NICOLLET METHODIST HOSPITAL</td>
      <td>6500 EXCELSIOR BLVD\nSAINT LOUIS PARK, MN\n(44...</td>
      <td>3</td>
      <td>1199</td>
      <td>34</td>
      <td>8557</td>
      <td>1232483</td>
      <td>694.29</td>
      <td>634.22</td>
      <td>0.31</td>
    </tr>
    <tr>
      <th>3</th>
      <td>MN</td>
      <td>HENNEPIN</td>
      <td>MINNEAPOLIS</td>
      <td>ABBOTT NORTHWESTERN HOSPITAL</td>
      <td>800 EAST 28TH STREET\nMINNEAPOLIS, MN\n(44.951...</td>
      <td>3</td>
      <td>865</td>
      <td>35</td>
      <td>8557</td>
      <td>1232483</td>
      <td>694.29</td>
      <td>634.22</td>
      <td>0.31</td>
    </tr>
    <tr>
      <th>4</th>
      <td>MN</td>
      <td>HENNEPIN</td>
      <td>MINNEAPOLIS</td>
      <td>UNIVERSITY OF MINNESOTA  MEDICAL CENTER, FAIRVIEW</td>
      <td>2450 RIVERSIDE AVENUE\nMINNEAPOLIS, MN\n(44.96...</td>
      <td>3</td>
      <td>515</td>
      <td>25</td>
      <td>8557</td>
      <td>1232483</td>
      <td>694.29</td>
      <td>634.22</td>
      <td>0.31</td>
    </tr>
  </tbody>
</table>
</div>




```python
# make county level df with average rating, total completed surveys, avg completed surveys, avg response rate, 
#population, crude rate, adj crude rate

# group by state and county
grouper = data_df.groupby(["State", "County"])

# get avg rating, survey, and response rate
rating = grouper[["Patient Rating", "Completed Surveys", "Survey Response Rate (%)"]].mean()
rating.reset_index(inplace=True)

# get total completed surveys
survey = grouper["Completed Surveys"].sum().to_frame()
survey.reset_index(inplace=True)

# get mortality and population data
mortality = grouper[["Population", "Crude Rate", "Age Adjusted Rate", "Deaths"]].mean()
mortality.reset_index(inplace=True)

#merge df
county_df = pd.merge(rating, survey, on=["State", "County"], how="inner")
county_df = county_df.rename(columns={"Patient Rating" : "Average Patient Rating", "Completed Surveys_x":"Average Completed Surveys", 
                                      "Survey Response Rate (%)": "Average Survey Response Rate (%)", "Completed Surveys_y":"Total Completed Surveys"})

county_df = pd.merge(county_df, mortality, on =["State", "County"], how="inner")

county_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>County</th>
      <th>Average Patient Rating</th>
      <th>Average Completed Surveys</th>
      <th>Average Survey Response Rate (%)</th>
      <th>Total Completed Surveys</th>
      <th>Population</th>
      <th>Crude Rate</th>
      <th>Age Adjusted Rate</th>
      <th>Deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AL</td>
      <td>AUTAUGA</td>
      <td>4.0</td>
      <td>579.0</td>
      <td>25.0</td>
      <td>579</td>
      <td>55416</td>
      <td>938.36</td>
      <td>884.39</td>
      <td>520</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AL</td>
      <td>BALDWIN</td>
      <td>4.0</td>
      <td>758.0</td>
      <td>33.0</td>
      <td>2274</td>
      <td>208563</td>
      <td>946.48</td>
      <td>716.92</td>
      <td>1974</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AL</td>
      <td>BARBOUR</td>
      <td>3.0</td>
      <td>184.0</td>
      <td>34.0</td>
      <td>184</td>
      <td>25965</td>
      <td>985.94</td>
      <td>800.68</td>
      <td>256</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AL</td>
      <td>BLOUNT</td>
      <td>4.0</td>
      <td>128.0</td>
      <td>29.0</td>
      <td>128</td>
      <td>57704</td>
      <td>1207.89</td>
      <td>989.37</td>
      <td>697</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AL</td>
      <td>BUTLER</td>
      <td>4.0</td>
      <td>158.0</td>
      <td>31.0</td>
      <td>158</td>
      <td>19998</td>
      <td>1375.14</td>
      <td>1014.00</td>
      <td>275</td>
    </tr>
  </tbody>
</table>
</div>




```python
county_df["Population (Millions)"] = county_df["Population"] / 1000000
county_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>State</th>
      <th>County</th>
      <th>Average Patient Rating</th>
      <th>Average Completed Surveys</th>
      <th>Average Survey Response Rate (%)</th>
      <th>Total Completed Surveys</th>
      <th>Population</th>
      <th>Crude Rate</th>
      <th>Age Adjusted Rate</th>
      <th>Deaths</th>
      <th>Population (Millions)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AL</td>
      <td>AUTAUGA</td>
      <td>4.0</td>
      <td>579.0</td>
      <td>25.0</td>
      <td>579</td>
      <td>55416</td>
      <td>938.36</td>
      <td>884.39</td>
      <td>520</td>
      <td>0.055416</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AL</td>
      <td>BALDWIN</td>
      <td>4.0</td>
      <td>758.0</td>
      <td>33.0</td>
      <td>2274</td>
      <td>208563</td>
      <td>946.48</td>
      <td>716.92</td>
      <td>1974</td>
      <td>0.208563</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AL</td>
      <td>BARBOUR</td>
      <td>3.0</td>
      <td>184.0</td>
      <td>34.0</td>
      <td>184</td>
      <td>25965</td>
      <td>985.94</td>
      <td>800.68</td>
      <td>256</td>
      <td>0.025965</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AL</td>
      <td>BLOUNT</td>
      <td>4.0</td>
      <td>128.0</td>
      <td>29.0</td>
      <td>128</td>
      <td>57704</td>
      <td>1207.89</td>
      <td>989.37</td>
      <td>697</td>
      <td>0.057704</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AL</td>
      <td>BUTLER</td>
      <td>4.0</td>
      <td>158.0</td>
      <td>31.0</td>
      <td>158</td>
      <td>19998</td>
      <td>1375.14</td>
      <td>1014.00</td>
      <td>275</td>
      <td>0.019998</td>
    </tr>
  </tbody>
</table>
</div>




```python
# export to csv
county_df.to_csv("Resources/county_data.csv")
```


```python
# Does Population Size alone affect Hospital Patient Experience?
sns.regplot(x="Population (Millions)", y="Average Patient Rating", data=county_df)
plt.title("Population vs. Average Patient Rating")
print(f"Linear regression: {stats.linregress(county_df['Population (Millions)'], county_df['Average Patient Rating'])}.")
plt.ylim(0, 5.5)
plt.show()
None
```

    Linear regression: LinregressResult(slope=-0.3406491975942604, intercept=3.5043403549026775, rvalue=-0.2123895540850802, pvalue=1.6571034180512237e-18, stderr=0.038352423139387415).



![png](output_6_1.png)



```python
# Does # of Survey Responders alone affect Hospital Patient Experience? 
# (total completed surveys vs county avg rating)

sns.regplot(x="Total Completed Surveys", y="Average Patient Rating", data=county_df)
plt.title("Total Completed Surveys vs. Average Patient Rating")
print(f"Linear regression: {stats.linregress(county_df['Total Completed Surveys'], county_df['Average Patient Rating'])}.")
plt.ylim(0, 5.5)
plt.show()
None
```

    Linear regression: LinregressResult(slope=-3.158565134727058e-05, intercept=3.498679616259108, rvalue=-0.17866617549163005, pvalue=1.8459394466483316e-13, stderr=4.256420489482902e-06).



![png](output_7_1.png)



```python
# Does # of Survey Responders alone affect Hospital Patient Experience? 
# (avg response rate vs county avg rating)

sns.regplot(x="Average Survey Response Rate (%)", y="Average Patient Rating", data=county_df)
plt.title("Average Survey Response Rate (%) vs. Average Patient Rating")
print(f"Linear regression: {stats.linregress(county_df['Average Survey Response Rate (%)'], county_df['Average Patient Rating'])}.")
plt.ylim(0, 5.5)
plt.show()
None
```

    Linear regression: LinregressResult(slope=0.0443520223376119, intercept=2.210180107759389, rvalue=0.43975458323103156, pvalue=5.1640771726862174e-80, stderr=0.002216553449834355).



![png](output_8_1.png)



```python
# Does # of Survey Responders alone affect Hospital Patient Experience? 
# (avg completed survey vs county avg rating)

sns.regplot(x="Average Completed Surveys", y="Average Patient Rating", data=county_df)
plt.title("Average Completed Surveys vs. Average Patient Rating")
print(f"Linear regression: {stats.linregress(county_df['Average Completed Surveys'], county_df['Average Patient Rating'])}.")
plt.ylim(0, 5.5)
plt.show()
None
```

    Linear regression: LinregressResult(slope=-0.00029635930368832145, intercept=3.6299376627286026, rvalue=-0.2482663649227558, pvalue=6.633230540475891e-25, stderr=2.8296196984905402e-05).



![png](output_9_1.png)

