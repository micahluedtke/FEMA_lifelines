# FEMA Lifelines
## Problem Statement
Where should FEMA first deploy resources during a disaster? 

## Approach to Problem
Our project focuses on census tract data in Massachusetts.  This code can be adapted for any location as long as the data is available.  Our first challenge was to build a risk assessment model. The CDC has already defined disaster management risk per the below:

 - Risk = Hazard*(Vulnerability - Resources)
 
For the purposes of this project, we do not have the hazard metric.  We have defined risk as follows.  When the disaster occurs, the hazard can be applied to the formula.

 - Risk = Vulnerability - Resources

## Gather and Explore Data
Vulnerability: The CDC provides calculations for the vulnerability data based on various social risk metrics by census tract in Massachusetts.  These metrics are outlined as shown below:

![alt text](https://github.com/micahluedtke/FEMA_lifelines/blob/master/maps-and-images/lifeline_count.png)

Resources: We used social media webscraping for the Foursquare API to pull resource locations.  We then categorized those locations by their FEMA lifeline as follows:
1. Safety and Security: Police Department, Fire Department
2. Food, Water, and Shelter: Grocery Store, City Hall, Courthouse, Town Hall, Library, School, Spiritual Center
3. Health and Medical: Hospital, Urgent Care Center
4. Energy: Not included in this analysis due to lack of readily available and interpretable data
5. Communication: Not included in this analysis due to lack of readily available and interpretable data
6. Transportation: Gas Station
7. Hazardous Waste: Hazardous Waste Facilities (from www.mass.gov)
![alt text](https://github.com/micahluedtke/FEMA_lifelines/blob/master/maps-and-images/lifeline_count.png)

## Challenges with the Data
**Foursquare limitations:**

While Foursquare provided a large sample of lifeline data to build our model, it still represents only a subset of the actual lifeline locations in Massachusetts.

**Energy data:**

We were able to locate and download energy grid data for the state of Massachusetts; however, this information was insufficient for determining a meaningful metric for energy for each census tract.

**Combining lifeline and vulnerability dataframes:**

Initially, we had the social vulnerability data by census tract and lifeline locations by gps coordinates - we needed to find a way to combine this data in order to map the risk.  To do this, we leveraged Geopandas and followed the steps below:
1. Imported social vulnerability data with Geopandas to include geometry polygon values.
2. Created geometry point values for each lifeline from gps coordinates.
3. Created a function to iterate through census tracts to bucket lifeline points by specific census tract.
4. After this process, we had a single dataframe that for each census tract observation included the social vulnerability calculation and the total number of lifeline locations in that tract by lifeline type.

## Model with Data
**Calculation of risk metric:**
1.  Normalize the lifeline data by census tract between 0 - 0.25 in order to scale it to the social vulnerability data (which was on a 0 - 1 scale with 1 being most risky).
2. Create a column that subtracts the scaled lifeline data from the social vulnerability rating except for the hazardous materials lifeline which was added, which is our *y* variable risk value.

**Created two maps of Massachusetts using Geopandas and Bokeh:**

Heatmap with high risk areas in red and low risk areas in green:
![alt text](https://github.com/micahluedtke/FEMA_lifelines/blob/master/maps-and-images/mass_risk_only.png)
Heatmap with lifelines identified with dots:
![alt text](https://github.com/micahluedtke/FEMA_lifelines/blob/master/maps-and-images/mass_with_lifeline.png)
## Evaluate the Model and Answer the Problem
FEMA will use the map to identify high risk areas to send first responders and prioritize resource allocation.
Assessing the riskiest census tracts in Massachusetts:
![alt text](https://github.com/micahluedtke/FEMA_lifelines/blob/master/maps-and-images/risky_tracts_table.png)

## Steps to Execute the Code
1. Open FEMA_lifelines notebook.
2. Install python ‘geopandas’ and ‘geopy’ packages if not already installed.
3. Register with Foursquare API (https://foursquare.com/developers/apps).
4. Insert Foursquare API Client_ID and Client_Secret into notebook.
5. Execute all cells.
6. To view interactive maps based on output, download HTML files from the repo. 

## Resources
https://svi.cdc.gov/A%20Social%20Vulnerability%20Index%20for%20Disaster%20Management.pdf
https://www.census.gov/cgi-bin/geo/shapefiles/index.php?year=2018&layergroup=Census+Tracts
https://svi.cdc.gov/data-and-tools-download.html
https://developer.foursquare.com/
https://www.mass.gov/guides/hazardous-waste-facilities-recyclers
https://hifld-geoplatform.opendata.arcgis.com/datasets/electric-power-transmission-lines/data?geometry=-73.3%2C41.958%2C-69.626%2C42.669&page=7

### Disclaimer
This code makes use of the FourSquare API. Per their [website](https://foursquare.com/legal/api/platformpolicy):
<br>"I. General
Your use of the API and Foursquare Data, including the collection, use, maintenance and disclosure of user data shall comply with all applicable laws, rules and regulations and the terms of this Policy, and any advertising, marketing, privacy, or other self-regulatory code(s) applicable to your industry.
You must not take any action that constitutes unauthorized or unsolicited advertising, junk or bulk e-mail.
You may use the API and Foursquare Data only if your application: (a) is non-exclusive, (b) is publicly available, (c) does not require a fee or subscription, (d) does not exceed the daily call limits, (e) is not intended for academic publications, and (f) is not otherwise commercial in nature. If your application does not meet all of these requirements, you will need to obtain an enterprise license or contact us for assistance."
