# Maine-BroadBand-Composite-Index
Author: Ariel Anderson

# Introduction 
This project using a composite index to determine best use of grant funds for broadband community centers. Broadband community centers are spaces which offer internet connection, computers, and educational materials to a local community. An example of this space may be
a classroom in an adult education center that is dedicated to internet and computer use, accessible to the local population in a rural setting. Some parts of rural Maine lack broadband connectivity. While infrastructure in Maine is improving, many rural Mainers need 
internet access to apply for jobs, pay bills, and complete school work. Community centers can serve as a bridge as infrastructure is improved. The composite index project helps to answer the question of how grant funds can be best allocated to reach the most individuals
that face the greatest number of barriars to accessing broandband. 

This analysis was conducted in ArcGIS Pro using multiple datasets. The presentation and maps for this project are publicly avaialble. 

# Background
A composite index uses simple methods to combine multiple variables into a single indicator variable. An example of a common composite index used in policy data analysis is the [Center for Disease Control and Prevention’s CDC Social Vulnerability Index (SVI)](https://www.atsdr.cdc.gov/placeandhealth/svi/fact_sheet/fact_sheet.html). 
The SVI uses 16 measures in four dimensions such as poverty, lack of vehicle access, and crowded housing to compute a composite index. Similarly, the Connectivity Hubs analysis uses multiple variables divided into two dimensions to determine a composite index. 
This process will be described in detail in the methodology section of this document. 

Methods for determining a composite index are relatively simple, data must be preprocessed to fit predetermined spatial units (i.e.: county, townships, zipcode). Further, each decision in the process has a consequence on the final composite index. 
Weighting of variables and variable selection can have considerable influence on the final composite index.  


# Technologies
* ArcGIS
* LucidChart

# Methodology

The best practices for determining Composite Index are outlined in this documentation from ESRI. The index question used for this analysis was: what locations are best suited for Connectivity Hubs funds? 
The flow chart shows the variables and dimensions used for this project derived from the driving question of the analysis. The spatial units determined for this project was Maine Townships boundaries.

### Social Vulnerability Dimension: 

Social Vulnerability was the first dimension determined for this analysis. In a composite index analysis each dimension represents an index which is broken down into multiple variables. A Social Vulnerability Index is then calculated which will be combined with a Geographic Factor Index to determine a final composite index. Social Vulnerability was divided into three variables: educational attainment defined as the rate of the population with a high school degree (includes alternatives such as GED) or higher, healthcare access determined by the mean travel time to a primary care provider, and rate of unemployment among the population age 16 and over. Educational attainment and unemployment data were obtained from ACS 5 year 2021 estimate, and healthcare data was provided by JSI Health. 

The first step of this analysis looks at the social vulnerability at the subcounty level in Maine. Subcounties are defined by the Census Bureau, [read more about subcounty divisions](https://www2.census.gov/geo/pdfs/reference/GARM/Ch8GARM.pdf). 

Social Vulnerability includes: 
* Rate of High School Graduation ([ACS Data](https://www.census.gov/programs-surveys/acs))
* Average Driving Distance from a Primary Care Provider (provided by John Snow Institute, this is not publically accessible data) 
* Rate of Unemployment ([ACS Data](https://www.census.gov/programs-surveys/acs))
* Whether or not a sub county is rural by [NTIA](https://www.ntia.gov/). (In Maine all subcounties are considered rural except by NTIA for: Falmouth, Westbrook, Portland, and South Portland) 
* Rate of incarceration ([2020 DEC Redistrciting Data](https://www.census.gov/programs-surveys/decennial-census/about/rdo/summary-files.html)]
 * [Covered Population Data from the American Census Survey](https://www.census.gov/programs-surveys/acs) (Rate of Population over the age of 60, Rate of Veterans, Limited English Households, Disability, & Rate of Ethnic or Racial Minorities)

### Preprocessing Social Vulnerability Data: 

ACS data and Rate of Incraceration data was available by township and did not require preprocessing for spatial units. However, this data was processed to reflect a percentage or rate per township to be used with a SVI scoring system of 0 to 1. Rural NTIA data was available at the township level and was assigned a score of 0 for non rural areas, and 1 for rural areas. 

Healthcare data was provided by zipcode, which required preprocessing to the appropriate spatial unit. It should be noted that healthcare availbility is measured as minutes a person needs to drive to see a physician. [Areal Interpolation](https://pro.arcgis.com/en/pro-app/latest/help/analysis/geostatistical-analyst/what-is-areal-interpolation.htm), as suggested by ESRI best practices for Composite Index calculation, was used to reaggregate the healthcare data to the appropriate spatial unit of Maine Townships. Interpolation is a method to aggregate data through prediction by fitting the data to a statistical model. 

Because Areal Interpolation relies on modeling data, it was determined that a K-Bessel model was an appropriate fit producing a Root Mean Square Standardization of 1.528, which should be ideally close to 1, and neighbor weights of 1 minimum to 10 maximum. 

#### K-Bessel Model for Areal Interpolation 
![alt text](https://github.com/arielfanderson/Maine-BroadBand-Composite-Index/blob/main/areal_interpolation.png "Areal Interpolation")


#### Normal QQ Plot and Summary for K-Bessel Model 
![alt text](https://github.com/arielfanderson/Maine-BroadBand-Composite-Index/blob/main/normalqq.png "Normal QQ Plot") 

### Social Vulnerability Scoring System 

Once all data was preproccessed to reflect the same spatial units, Maine townships, all data was compiled into ArcGIS. For a composite index, it's important that all data is in the same direction. A higher social vulnerability index indicates increased social vulnerability. For example, high rates of rates of unemployment increase social vulnerability. Conversely, high rates of education decrease social vulnerability. So, in the case of educational attainment, directionality must be considered to account for the inverse relationship between educational attainment and social vulnerability. The table below shows each unit of measurement for the Social Vulnerability Index, as well as it's direction in relation to social vulnerability. 

#### Social Vulnerability Scoring System Table

![alt text](https://github.com/arielfanderson/Maine-BroadBand-Composite-Index/blob/main/svtable.png "Social Vulnerability Table") 



