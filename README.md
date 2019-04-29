# FEMA Lifelines 
## Define the Problem
Where should FEMA first deploy resources in a disaster?

### Approach to Problem
Build a risk assessment model
Risk = Vulnerability - Resources
Visualize risk on census tract map of MA
## Gather the Data

We initially attempt to get information from the Yelp API but ultimately found that FourSquare had a greater variety of lifeline data. We used data from the Census Bureau and CDC to determine vulnerability.

Vulnerability: CDC social vulnerability data

Resources: social media webscraping with Foursquare API

## Explore the Data
Challenges:
Foursquare limitations
Combining lifeline and vulnerability dataframes in Geopandas
## Model the Data
Created two interactive maps of Massachusetts:
Heatmap with high risk areas in red and low risk areas in green
![alt text](https://raw.githubusercontent.com/username/projectname/branch/path/to/img.png)
Heatmap with lifelines identified with dots
![alt text](https://raw.githubusercontent.com/username/projectname/branch/path/to/img.png)
## Evaluate the Model and Answer the Problem
### Disclaimer
This code makes use of the FourSquare API. Per their [website](https://foursquare.com/legal/api/platformpolicy):
<br>"I. General
Your use of the API and Foursquare Data, including the collection, use, maintenance and disclosure of user data shall comply with all applicable laws, rules and regulations and the terms of this Policy, and any advertising, marketing, privacy, or other self-regulatory code(s) applicable to your industry.
You must not take any action that constitutes unauthorized or unsolicited advertising, junk or bulk e-mail.
You may use the API and Foursquare Data only if your application: (a) is non-exclusive, (b) is publicly available, (c) does not require a fee or subscription, (d) does not exceed the daily call limits, (e) is not intended for academic publications, and (f) is not otherwise commercial in nature. If your application does not meet all of these requirements, you will need to obtain an enterprise license or contact us for assistance."
