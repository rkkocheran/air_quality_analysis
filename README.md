# What’s In Your Air: A Comparative Study of Nashville’s Air Quality
This is a repository for my NSS Data Analytics capstone project analysis about air quality across the US.

## Table of Contents
* [Motivation](#Motivation)
* [Data Questions](#Data-Questions)
* [Data Sources and Tools](#Data-Sources-and-Tools)
* [Methods](#Methods)
* [Conclusions](#Conclusions)
* [Final Remarks](#Final-Remarks)

## Motivation
<details>  
  <summary>Expand</summary>   
     
  When my family and I moved to Nashville in 2016, we experienced a slew of respiratory issues we had never faced before. Yet we were not alone in this phenomenon. Rumor had it that Nashville was notorious for causing breathing troubles for those who move there from other states. Naturally, this project presented an excellent opportunity to look into Nashville’s air quality.  
    
  My personal journey began in San Diego, CA, where both my husband and I lived for the first twenty-five years. There, I only experienced breathing troubles during the occasional flu or cold. My husband, who is predisposed to respiratory issues, experienced mild allergies throughout the year. We then moved to Phoenix, AZ, where we lived for about two years. There, things more or less remained the same regarding our health. Then we moved to Nashville, where everything changed. I began experiencing allergy symptoms and my husband’s breathing became so compromised that anytime he got the flu or a cold, he would have to go to urgent care. In performing this analysis, I aim to equip myself and others with knowledge about potential causes for the common increase in respiratory issues for those who move to Nashville from out of state.

</details>

## Data Questions
<details>  
  <summary>Expand</summary>   

  1.	How is Nashville’s air quality different from other cities in which I’ve lived?
  2.	How does Nashville compare to other highly-populated cities?

</details>

## Data Sources and Tools
<details>  
  <summary>Expand</summary>   
  
  Data was extracted from three main sources:
  1.	EPA.com
  2.	Pollen.com
  3.	Wikipedia.com

  Using Python and Jupyter Notebooks, API requests were made from the EPA’s Outdoor Air Quality website, which contains historical records of levels of airborne particles. Data about EPA’s Air Quality Index categories was also obtained in the form of a CSV file. Webscraping was also used to first collect data from Pollen.com about pollen-producing plants in cities throughout the US and then from Wikipedia, for general information about the cities chosen for this study.  

  Due to the compact nature of the datasets, the collected data was imported as Excel files into Tableau. Using Tableau, a many-to-many relationship was established between the datasets based on the city associated with each record. The data was then analyzed and transformed into multiple visualizations, and finally a presentation, to convey a meaningful story about the conclusions I drew from the data.  

  #### Data Sources
  * [EPA.gov](https://www.epa.gov/outdoor-air-quality-data)
    * [Air Quality System, Annual Summaries](https://aqs.epa.gov/aqsweb/documents/data_api.html#annual)
    * [Air Quality Index, Category Breakpoints](https://aqs.epa.gov/aqsweb/documents/codetables/aqi_breakpoints.html)
  * [Pollen.com](https://www.pollen.com/research/)
  * [Wikipedia.com](https://en.wikipedia.org/wiki/)
  #### Tools
  * [Tableau](https://www.tableau.com/)
  * [Python](https://www.python.org/)
    * [Jupyter Notebooks](https://jupyter.org/)
    * [Anaconda](https://anaconda.org/)
    * Modules:
      * [Pandas](https://pandas.pydata.org/)
      * [PandaSQL](https://pypi.org/project/pandasql/)
      * [Selenium WebDriver](https://www.selenium.dev/documentation/webdriver/)
      * [Requests](https://requests.readthedocs.io/en/latest/)
      * [BeautifulSoup](https://beautiful-soup-4.readthedocs.io/en/latest/)
  * [Microsoft Excel](https://www.microsoft.com/en-us/microsoft-365/excel)

  [Back to [Table of Contents](#Table-of-Contents)]

</details>  

## Methods
<details>  
  <summary>Acquiring the Data</summary>  
    
  I decided to look at nine cities, including the three I’ve lived in. While my focus was on those three cities, the others included provided a comparative baseline and covered different geographies, climates, population densities and flora.  

  The nine cities were split into three groups covering Western, Middle and Eastern US. The Western regions included Seattle, Washington; San Diego, California; and Phoenix, Arizona. The Mid-US regions included Minneapolis, Minnesota; Denver, Colorado; and Austin, Texas. The Eastern regions included Phildelphia, Pennsylvania; Nashville, Tennessee; and Jacksonville, Florida.  

#### 1. EPA.org
  Collection of particulate pollution data began with understanding the EPA (US Environmental Protection Agency) AQS (Air Quality System) Data Dictionary, available as a PDF [here](https://www.epa.gov/aqs/aqs-data-dictionary). I then looked at understanding the measurement parameters for the AQS using the reference table, available as a CSV file [here](https://aqs.epa.gov/aqsweb/documents/codetables/parameters.html). I decided to cast a wide net and include, in addition to parameters used in NAAQS (National Ambient Air Quality Standards) decisions, a few other parameters I knew were also notable respiratory irritants. Of the 1,477 available parameters, I narrowed my list down to 10, which optimized the processing time spent retrieving API data due to the EPA website’s limit of 5 parameters per request. The parameters were chosen based on the following criteria:
  -   Compounds that are used in the AQI (Air Quality Index) Reports, including ground level ozone (O3), carbon monoxide (CO), nitrogen dioxide (NO2), sulfur dioxide (SO2), and particle pollution PM10 and PM2.5 which are “the most common ambient air pollutants regulated under the Clean Air Act” **(1)**. I chose to exclude parameter “88502”, characterized as “ACCEPTABLE PM2.5 AQI & SPECIATION MASS1”, which is not used in NAAQS decisions **(2)**.
  -   Other simple compounds that are known respiratory irritants such as smoke, carbon dioxide (CO2), nitric oxide (NO) and benzene.

  In order to follow EPA’s guidelines, time between requests was kept to a minimum of 5 seconds. This prevented any disabling of the account due to a violation of their Terms of Service. Data from the Annual Summaries tables was used, which contains “calculated values of concentrations of monitor samples, which have been summarized for a year, sampling duration, and exceptional data indicator combination. Annual summaries are computed for each calendar year. They may be computed for both sample measurements and NAAQS_Averages. They may include statistics based on any of the lower level summaries (Daily or Quarterly) or sample measurements. Part of the key is the sample measurement durations summarized (e.g., hourly, daily or NAAQS Average.)” **(3)**

  Using Python, data was retrieved through the use of nested loops governed by the following flow of iteration:
  1.	Across the last five years (2017-2021).
  2.	Across each city’s associated county.
  3.	Across each subset of the main list of parameters, maintaining the required limit of 5 parameters per request.

  The data from each request was added to a common “AQS” DataFrame which contained a total of 56 columns and 5,664 rows, once data from all the API requests had been retrieved.  

#### 2. Pollen.com  
  
  Collection of pollen data was achieved using webscraping. The data encompassed pollen-producing tree, grass and ragweed plants documented to grow in each city. The plants are categorized using flowering seasons (Spring, Summer, Fall and Winter), pollen type (Tree, Grass, Ragweed) and allergenicity levels (Mild, Moderate or Severe.) 

  Webscraping algorithms were created using Python, Jupyter Notebooks and the Anaconda environment. The resulting dataset was saved as a DataFrame, which was cleaned and prepared and then exported as both a CSV and a Microsoft Excel file.

  The main challenge was the need to simulate “mouse-click” behavior within the website to iterate through the different chart categories (e.g. season and type.) This challenge was overcome by incorporating the Python module, Selenium, into the webscraping algorithm. After researching the Selenium WebDriver documentation, installing the module, and incorporating its functionality, the algorithm successfully iterated through each of the website’s charts and retrieved the data needed for each city.

  Tools used: _Webscraping, Python (modules: pandas, requests, bs4 (BeautifulSoup), selenium webdriver, time), Anaconda, Jupyter Notebooks._

#### 3. Wikipedia.org
  
  Collection of general city information was achieved by webscraping each city’s Wikipedia website using Python and Jupyter Notebooks. The categories gleaned included city name, county, city land area, elevation, population census year, population density, metro population, population rank and climate type. This data was converted into a pandas DataFrame which was exported as both a CSV and a Microsoft Excel file.

  There were many challenges encountered while obtaining data from Wikipedia, most of which involved either the html tags or the formatting of the values. The specific challenges were:
  -	The wiki data was not consistently tagged or titled (e.g. several states had unique or mislabeled tags.)
  -	Some cities had multiple climates and/or counties listed.
  -	UNICODE characters and unexpected symbols were embedded in many values. 
  -	Population and elevation data included both metric and standard measurements.
  -	Some elevations contained a range of values instead of a single value. 

  All issues were handled in the webscraping algorithm using conditional statements, string manipulation (splitting, slicing, concatenation and character replacement), averages for ranges, and consolidation via common wording or locale. The resulting DataFrame retained only a few missing values which were manually entered into the exported Excel file, where final formatting of the data also took place.

  Tools used: _Webscaping, Excel, Python (modules: requests, pandas, numpy, bs4 (BeautifulSoup), and re (regex)), Anaconda, Jupyter Notebooks._  

  [Back to [Table of Contents](#Table-of-Contents)]    

</details>

<details>
  <summary>Cleaning and Merging the Data</summary>  
    
  Much of the cleaning and data preparation took place either within the data collection algorithm or within the Python script before each dataset’s export, and involved only minor manipulations, such as converting a city and state field into two separate fields, or minor corrections of general city data, which were manually corrected in the exported Microsoft Excel file.  
     
  Exploration of the AQS DataFrame revealed the need for the following manipulations:
  1.	Filter for parameters that had data for all nine cities/counties for at least three years.
    Note: This unfortunately elimated measurements of smoke, Carbon dioxide (CO2), and Benzene (BZ), but retained measurements for Carbon monoxide (CO), Sulfur dioxide (SO2), Nitric oxide (NO), Nitrogen dioxide (NO2), Ozone (O3), PM10 and PM2.5. After some analysis of the data, however,  Nitric oxide was also eliminated to maintain a more consistent comparision for analysis regarding Air Quality Index categories, which does not include Nitric oxide in its dataset.
  2.	Add a column containing the average of the four max values for each record.
  3.	Format the date columns to include just the date (i.e. no timestamp).
  4.	Add an associated (main) city column for potential merging with other data tables.
  5.	Subset for only the necessary fields.

  After cleaning, the AQS DataFrame had 26 columns and 5,366 rows containing particulate pollution data spanning 5 years, 9 cities and 6 parameters. 

  To add an additional layer of relevance to the AQS DataFrame, I wanted to add AQI Categories (Good, Unhealthy, Hazardous, etc.) to each AQS record to determine the proportion of each category for each city. This was accomplished by converting the AQI Breakpoints dataset from its CSV format to a pandas DataFrame. I then performed an Inner Join, keeping only records that occur in both the AQS DataFrame and the AQI Breakpoints DataFrame.

  This merged dataset was saved as a new dataframe to preserve the originals and avoid losing data for measurement durations that were unique to the AQS table. The AQS and AQI DataFrames were joined on five common fields, one of which was joined based on a value from the AQS DataFrame falling between a range of two values (the low and high breakpoints) located in two separate columns in the AQI Breakpoints DataFrame. Due to the complication of joining two tables based on a range of values, the Inner Join was performed using the pandasql module to take advantage of the straightforward functionality of SQL. In the SQL query, fields of the merged DataFrame were subset even further, and rows not matching in duration type were dropped, resulting in a DataFrame with 11 columns and 4,154 rows.

  Merging the two DataFrames was one of the greatest challenges I faced during this task, but an elegant solution was found with enough research. I came across several solutions to merging DataFrames in Python based on the value in the column of the first table falling within a range given by two columns (a max and min column) in a second table, but all of these were more complex than I felt was necessary. I knew the problem could easily be solved using SQL and that Python and SQL had a degree of interfacing. Therefore I kept looking until I stumbled upon the best and obvious solution: using the pandasql module to simulate a SQL join of the two pandas DataFrames.

  Tools used: _APIs, Python (modules: pandas, pandasql, numpy, math, requests, json, regex, time, datetime, itertools (islice), collections (Counter)), Anaconda, Jupyter Notebooks._

  [Back to [Table of Contents](#Table-of-Contents)]

</details>

<details>  
  <summary>The Analysis</summary>  
    
  In total, four tables were imported into Tableau, each in the form of a Microsoft Excel file. CSVs for each table were also available, however the dataset was sufficiently small to use with Excel to attempt to preserve a level of information about the tables’ various datatypes. In Tableau, a many-to-many relationship between the four tables was established based on the city name associated with each record.

  #### Pollen Analysis
  I began my analysis by looking at pollens, specifically at a breakdown of pollen-producing plants by flowering season. For this purpose, I used a stacked barchart to show the differences in both count and percentage form of the number of pollen-producting plants there are per season for Nashville and other cities. I was able to conclude that in Spring, Nashville has about 1-2% more pollen-producing species than Phoenix and San Diego but for the rest of the seasons, Nashville has less. In Summer, Nashville has 1-3% less. In Fall, that increases to 4-7% less and in Winter, that drop back to 2-4% less than the Southwest region in which I previously lived. This is an interesting contrast to the majority of cities because, overall, Nashville has 3-7% more species throughout the year.

  I also looked at the breakdown of pollens according to source, the three sources being grasses, ragweeds and trees. As before, I used a stacked barchart and included the option to view the data either as total counts or percentages of the total. Focusing again on Nashville, I determined that concerning ragweeds, Nashville has 9-12% less varieties than Phoenix and San Diego, respectively. This shifts concerning grasses, with Nashville having 2-4% more grass varieties than San Diego and Phoenix, respectively, and for trees that increases to 7-8% more.
  
  Compared to other cities, Nashville has about 3% less flowering trees varieties than Philadelphia, but 2-13% more than the remaining regions. Nashville also has 3% less pollen-producing grass varieties than Philadelphia, and 2-9% more than the remaining regions. For ragweeds, Nashville has 1-4% less species than Minneapolis and Philadelphia, respectively, but only 0-3% less species than the remaining regions.
  
  #### Air Pollution Analysis
  I then looked at levels airborne particulate matter, also known as air pollution. I chose to look at 6 specific parameters: Nitrogen dioxide, Sulfur dioxide, Carbon monoxide, Ozone, PM2.5 and PM10. To help summarize these parameters, the Tableau presentation includes a table that gives each parameter’s number, common name, units of measure, and molecular structure. PM2.5 refers to fine particles up to 2.5 micrometers in diameter such as combustion particles like CO and organic compounds like formaldehyde and benzene. PM10 includes compounds up to 10 micrometers in diameter such as dust, pollen, and mold.

  In order to understand Nashville’s air pollution relative to other cities, I put together a graph showing the average maximum measurement for each parameter for each city’s associated county over the last 5 years (from 2017 to 2021.) Nashville, represented by Davidson county, falls in the bottom third of cities, when ranking average maximum measurements for each parameter from highest to lowest. The only notable rise in particle pollution for Nashville occurs in late May of 2017 with Sulfur dioxide; but its measured values in the previous and following years decrease to be in line with other counties. Concerning PM2.5 and PM10, Nashville’s measurements for particle pollution remain on the low side. For measurements of fine particles, Nashville is 2nd to lowest, lower than both Phoenix and San Diego. Regarding coarse particles, it remains in the bottom 50% and has significantly lower levels of particle pollution than Phoenix but similar levels to San Diego.

  I also decided to look at AQI (Air Quality Index) categories, of which there are 6, ranging from “Good” in green to “Hazardous” in maroon. All categories except “Hazardous” appeared in the data at which I looked.

  To help visualize this data, I created a chart that shows the percentage of records that fall in each category for each county. Like most cities, the majority of Nashville’s records fell in the “Moderate” category, with 85% of its records qualifying as either “Moderate,” “Unhealthy to Sensitive” or “Unhealthy”. However, most cities followed this pattern with the exception of Phoenix, which had a majority of “Unhealthy” days, and San Diego and Seattle, for which about 25% of their records are classified as “Unhealthy.” So, although Nashville exhibits a fair amount of air pollution, those who move there from the West and especially the Southwest, will experience a 13-45% decrease in exposure to “Unhealthy” levels of air pollution. Those moving to Nashville from elsewhere are likely to only experience a small, 0-2% increase in exposure to “Unhealthy” levels of air pollution.

  [Back to [Table of Contents](#Table-of-Contents)]

</details>


## Conclusions
<details>
  <summary>Expand</summary>  

  In conclusion, those moving to Nashville from the far Southwest will be exposed to 7.5% more varieties of tree pollens, 3% more varieties of grass pollens, 10.5% less varieties of ragweed pollens and a 13-45% decrease in exposure to Unhealthy levels of air pollution. Those arriving from the Philadelphia region will be exposed to a lower variety of all types of pollens and only a small increase in air pollution. Those arriving from anywhere else will be exposed to a higher variety of all types of pollens, and either a decrease or a minor increase in exposure to Unhealthy levels of air pollution when moving from the North or South, respectively.  

  [Back to [Table of Contents](#Table-of-Contents)]

</details>

## Final Remarks
<details>
  <summary>Expand</summary>

  #### I would like to thank...
  * [Nashville Software School](https://nashvillesoftwareschool.com/)
  * [The NSS DDA8 Cohort](https://nss-full-time-data-analytics-8.github.io/)
  * My academic instructors, advisors and supporters at Nashville Software School
  * Everyone whose encouragement and expertise helped transform this project from an idea into a reality

  #### Credits

  * AQS images were obtained from: https://www.epa.gov/pmcourse/what-particle-pollution
  * AQI images were obtained from: https://www.epa.gov/pmcourse/patient-exposure-and-air-quality-index
  * Molecular structure images were obtained from: https://depositphotos.com/portfolio-1711722.html
  * Topographical maps were obtained from: https://en-us.topographic-map.com/
  * Street maps were obtained from: https://www.openstreetmap.org/

  #### Sources
  1.	Patient Exposure and the Air Quality Index, available at https://www.epa.gov/pmcourse/patient-exposure-and-air-quality-index
  2.	Technical Note on Reporting PM2.5 Continuous Monitoring and Speciation Data to the Air Quality System (AQS), available at https://www.epa.gov/sites/default/files/2017-02/documents/contrept.pdf
  3.	AQS Data Dictionary [Version 2.28], section 3-22, available at as a PDF from https://www.epa.gov/aqs/aqs-data-dictionary

  [Back to [Table of Contents](#Table-of-Contents)]

</details>

## Link to the Project
[Explore the project dashboards by clicking here!](https://github.com/rkkocheran/air_quality_analysis) There are many interactive features throughout the dashboards in the presentation. I encourage you to find out for yourself how Nashville’s air quality compares to other major cities around the country.

[Back to [Table of Contents](#Table-of-Contents)]
