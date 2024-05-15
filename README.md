Environmental Justice at the Urban-Agricultural Interface: Pesticide Use and Cumulative Burdens in Fresno, CA

Fresno is one of the top Agricultural producing counties in the U.S. and is a major part of the California Central Valley agricultural economy, which produces ¼ of the country’s food. The valley’s topography coupled with land use and air quality policy, distributional inequities, and discriminatory siting have, over time, led to the exposure of low-income families and small BIPOC farmers to pesticide drift and other cumulative air quality and environmental burdens. 

![png](output_38_0.png)


Exposure to pesticides through the air has been linked to a number of serious health conditions. Section 12972 of the California Food and Agriculture Code requires that any pesticide application take necessary precautions to prevent substantial drift to nontarget areas. However, community members have expressed that the County of Fresno and the state of California have failed to adequately enforce this, and it is disproportionately harming farmworkers and their families and residents living near pesticide spraying farms. As one of the most polluted regions in the nation, residents of the San Joaquin Valley are burdened by the cumulative effects of many drivers of pollution. 

Over 500,000 children in California attend schools located near hazardous pesticide applications. Latinx school children are 91% more likely to attend one of the most impacted schools compared to their white peers. This analysis uses the Department of Pesticide Regulation’s 2021 Pesticide Use Reporting (PUR) data set and the Deartment of Education’s school locations to show the proximity between the application of dangerous pesticides and schools. Children are especially vulnerable to the health impacts of pesticides.  

First, I converted the units to square miles to normalize the size of the pesticide applications and plotted them within 1 sq mile townships. 

```python
def convert_to_sqmiles(row):
    amount = row["AMOUNT_TRE"]
    unit = row["UNIT_TREAT"]
    
    if unit == "A":
        # Convert acres to square miles
        return amount * 0.0015625  # 1 acre = 0.0015625 square miles
    elif unit == "S":
        # Convert square feet to square miles
        return amount * 3.587e-8  # 1 square foot = 3.587e-8 square miles
    elif unit == "C":
        # Convert cubic feet to square miles (assuming 30 ft height)
        depth_or_height_feet = 30  # Adjust as needed

        # Convert cubic feet to cubic miles
        volume_cubic_miles = amount / (5280 ** 3)

        # Calculate the equivalent area in square miles
        area_square_miles = volume_cubic_miles * depth_or_height_feet
    else:
        # Return None for unsupported units
        return None
# Apply the conversion function to create the new column
pur_gdf["AMOUNT_SQMILES"] = pur_gdf.apply(convert_to_sqmiles, axis=1)

# Print the GeoDataFrame with the new column
print(pur_gdf)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>COUNTY_CD</th>
      <th>MTOWN</th>
      <th>RANGE</th>
      <th>SECTION</th>
      <th>REGIONNAME</th>
      <th>BASE_LN_ME</th>
      <th>TOWNSHIP</th>
      <th>MTRS</th>
      <th>MTR</th>
      <th>CO_MTRS</th>
      <th>REGION</th>
      <th>CO_MTR</th>
      <th>County_CDn</th>
      <th>NAME</th>
      <th>NAMELSAD</th>
      <th>OID_</th>
      <th>ADJUVANT</th>
      <th>YEAR</th>
      <th>DATE</th>
      <th>COUNTY_NAM</th>
      <th>COMTRS</th>
      <th>SITE_NAME</th>
      <th>PRODUCT_NA</th>
      <th>POUNDS_PRO</th>
      <th>CHEMICAL_N</th>
      <th>POUNDS_CHE</th>
      <th>AMOUNT_TRE</th>
      <th>UNIT_TREAT</th>
      <th>AERIAL_GRO</th>
      <th>geometry</th>
      <th>AMOUNT_SQMILES</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10</td>
      <td>M04S</td>
      <td>27E</td>
      <td>20</td>
      <td>LAHONTAN</td>
      <td>M</td>
      <td>04S</td>
      <td>M04S27E20</td>
      <td>M04S27E</td>
      <td>10M04S27E20</td>
      <td>6</td>
      <td>10M04S27E</td>
      <td>10</td>
      <td>Fresno</td>
      <td>Fresno County</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>None</td>
      <td>POLYGON ((86163.850 -47413.974, 86191.328 -474...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10</td>
      <td>M04S</td>
      <td>27E</td>
      <td>19</td>
      <td>CENTRAL VALLEY</td>
      <td>M</td>
      <td>04S</td>
      <td>M04S27E19</td>
      <td>M04S27E</td>
      <td>10M04S27E19</td>
      <td>5</td>
      <td>10M04S27E</td>
      <td>10</td>
      <td>Fresno</td>
      <td>Fresno County</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>None</td>
      <td>POLYGON ((85698.772 -47889.337, 85702.950 -480...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10</td>
      <td>M04S</td>
      <td>27E</td>
      <td>29</td>
      <td>CENTRAL VALLEY</td>
      <td>M</td>
      <td>04S</td>
      <td>M04S27E29</td>
      <td>M04S27E</td>
      <td>10M04S27E29</td>
      <td>5</td>
      <td>10M04S27E</td>
      <td>10</td>
      <td>Fresno</td>
      <td>Fresno County</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>None</td>
      <td>POLYGON ((85826.745 -48097.511, 87004.884 -480...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10</td>
      <td>M04S</td>
      <td>27E</td>
      <td>30</td>
      <td>CENTRAL VALLEY</td>
      <td>M</td>
      <td>04S</td>
      <td>M04S27E30</td>
      <td>M04S27E</td>
      <td>10M04S27E30</td>
      <td>5</td>
      <td>10M04S27E</td>
      <td>10</td>
      <td>Fresno</td>
      <td>Fresno County</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>None</td>
      <td>POLYGON ((85492.147 -48100.514, 85702.950 -480...</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10</td>
      <td>M04S</td>
      <td>27E</td>
      <td>28</td>
      <td>LAHONTAN</td>
      <td>M</td>
      <td>04S</td>
      <td>M04S27E28</td>
      <td>M04S27E</td>
      <td>10M04S27E28</td>
      <td>6</td>
      <td>10M04S27E</td>
      <td>10</td>
      <td>Fresno</td>
      <td>Fresno County</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>None</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>None</td>
      <td>POLYGON ((89043.287 -49493.042, 89047.876 -496...</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

Then I found the Centroid points, and layered them with the location of public and private schools in fresno to show the proximity. 


![png](output_32_0.png)
    
This widget shows the wind speed and direction in real time, showing when and how drift is likely to happen in the direction of schools from near by applications. 

<iframe src="https://www.meteoblue.com/en/weather/maps/widget/fresno_united-states_5350937?windAnimation=0&windAnimation=1&gust=0&gust=1&satellite=0&satellite=1&cloudsAndPrecipitation=0&cloudsAndPrecipitation=1&temperature=0&temperature=1&sunshine=0&sunshine=1&extremeForecastIndex=0&extremeForecastIndex=1&geoloc=fixed&tempunit=C&windunit=km%252Fh&lengthunit=metric&zoom=5&autowidth=auto"  frameborder="0" scrolling="NO" allowtransparency="true" sandbox="allow-same-origin allow-scripts allow-popups allow-popups-to-escape-sandbox" style="width: 100%; height: 720px"></iframe><div><!-- DO NOT REMOVE THIS LINK --><a href="https://www.meteoblue.com/en/weather/maps/fresno_united-states_5350937?utm_source=weather_widget&utm_medium=linkus&utm_content=map&utm_campaign=Weather%2BWidget" target="_blank" rel="noopener">meteoblue</a></div>
Farmworkers and their families are especially exposed to pesticides. The bivariate maps below show pesticide use correlated with charecteristics common in farmworker communities in the Central Valley. Many of these characteristics also serve as barriers to reporting, which is the main way the County Agricultural Commisioners find out about pesticide drift instances.

![png](output_42_0.png)
![png](output_43_0.png)
![png](output_44_0.png)

