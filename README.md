### Please note that this ETL-Project-0 repository was created to overcome a problem with Mike's local Git repository. All the collaborative project work was conducted in the  MikeMurf/ETL-Project-1 repository. Please refer to that repository for a view of the project team's collaborative work. Many thanks.

### Extract, Transform, and Load Project for the Monash Data Analytical Bootcamp

### 1.	Project Overview:   
The project was initiated to satisfy the requirements of the Extract, Transform, and Load assignment for the Monash Data Analytical Bootcamp.
These requirements are:
*   “You must have two (minimum) or more sources of data
*   Recommended sources: 
*       Kaggle
*       Data.world 
*       Google Dataset Search (https://datasetsearch.research.google.com/) 

*    APIs may be used as an alternative source
*    Once your datasets are identified, perform ETL and create documentation.
*    Documentation must have:
*        Datasets used and their sources
*        Types of data wrangling performed (data cleaning, joining, filtering, and aggregating)
*        The schemata used in the final production database”

The “Project-2” directory contains the following files submitted for assessment:
*    “create-covid-db” – the Jupyter Notebook used to conduct the extract, transform and load,
*    “eda” – the Jupyter Notebook used to conduct the Pandas Profiling analysis on the Covid_Cases, Vaccinations and World_Population datasets produced by “create-covid-db”, 
*    “Project-2-report” – the project report describing the process used and the outputs produced by the extract, transform and load,
*    “Project-2” – the Powerpoint presentation for the project,
*    "project-2-project-proposal" - the original proposal for the project.

The files can be run from this current directory.

*    NOTE: the "exported_CSVs" directory contains the Covid_Cases, Vaccinations and World_Population datasets produced by “create-covid-db”. 

### 2.	Team Members: 
		Megan Greenhill 
		Hesh Kuruppuge 
		Jacqueline Xia 
		Mike Murphy

### 3.	Project Brief Description:
The project uses John Hopkins University Covid-19 datasets sourced from the Google Dataset Search, and related datasets to extract the required data to create an integrated database that can be used to provide ongoing analysis of Covid_19 and its global impact. 

### 4.	Project Rationale:
In addition to satisfying the requirements of the assignment, the project provides a much-needed solution for analysing global Covid-19 trends.
There is currently no single source that provides a comprehensive, integrated view of the global Covid-19 pandemic. To get a comprehensive overview you have to use multiple data sources and combine their outputs to get a single view. 
This project combines the main sources of global Covid data, global vaccination data and global population data to provide a single, integrated source of information in a relational database and to provide basic queries to retrieve views of  the data. These queries can then be  adapted to provide ad hoc query capability.
The database can be updated as frequently as required and will facilitate analysis that currently requires searches of disparate source datasets.
 
### 5.	Project Datasets: 
  The datasets for the project can be found at the following links. 
*    “JHU – Time Series Daily Reports” 
*         https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv  

*    “World population data” 
*        https://www.worldometers.info/world-population/population-by-country/ 

*    “Vaccination rates per country”
*        https://ourworldindata.org/covid-vaccinations?		
 
### 5.	Database QuickDB Code 
The QuickDB code used to create the data base schema follows.  

	country_codes
	-
	country_id VARCHAR(255) PK
	country_name VARCHAR(255)
	continent_name VARCHAR(255)
	
	covid_cases
	-
	country_id VARCHAR(255) FK - country_codes.country_id
	date VARCHAR(255)
	confirmed INT
	deaths INT
	recovered INT
	active INT
	new_cases INT
	new_deaths INT
	new_recovered INT

	population
	-
	country_id VARCHAR(255) FK - country_codes.country_id
	population INT

	vaccinations
	-
	country_id VARCHAR(255) FK - country_codes.country_id
	date VARCHAR(255)
	vaccinated_per_hundred INT
	fully_vaccinated_per_hundred INT
	boosted_per_hundred INT
	not_fully_vaccinated_per_hundred INT

### 6.		Database Schema – Entity Relationship Diagram 

![image](https://user-images.githubusercontent.com/89948865/152876605-b5288a80-fbd8-4f25-87dc-96fc2a649dd2.png)
				
### 7.	Database Description
The  key to the data base was to use the International Standards Organisation (iso_code: ISO 3166-1 alpha-3 – three-letter country code) henceforth referred to as “iso-code”, to create relationships between the tables. 
The “country-codes” table contains the  “iso-code” and matching “country-name” for all countries covered by the “iso-code” and was generated during the Extraction phase of the project.
The “covid-cases” table contains the basic cleansed data from the JHU Data Sets which form the basis of the global view of Covid-19 cases.
The “vaccinations” table contains vaccination status from the Our World in data Vaccination data set.
    
### 8.	Database Meta Data   
“country” table
*    country-id: 	the iso_code: ISO 3166-1 alpha-3 – three-letter country code
*    country-name: 	the name of the country in the ISO data set

“covid-cases” table
*    country-id: 	the iso_code: ISO 3166-1 alpha-3 – three-letter country code
*    date: 		date of the observation
*    confirmed:	the total number of cumulative confirmed Covid-19 cases regardless of the variant
*    deaths:	the total number of cumulative deaths attributed to Covid-19 regardless of the variant
*    recovered:	the total number of cumulative recovered Covid-19 cases
*    active:		the total number of cumulative active Covid-19 cases
*    new_cases:	the total number of incremental new  Covid-19 cases
*    new_deaths:	the total number of incremental new Covid-19 deaths
*    new_ recovered:	the total number of incremental new recovered Covid-19 cases

“population” table
*    country-id: 	the iso_code: ISO 3166-1 alpha-3 – three-letter country code
*    population: 	the population of the country at 31/12/2020

“vaccinations” table
*    country-id: 	the iso_code: ISO 3166-1 alpha-3 – three-letter country code
*    date: 		date of the observation
*    people_vaccinated_per_hundred:
*        total number of people who received at least one vaccine dose. If a person receives the first dose of a 2-dose vaccine, this metric goes up by 1. If they receive the *        second dose, the metric stays the same i.e., 1.
*    fully_vaccinated_per_hundred:
*        people vaccinated per 100 people in the total population of the country. If a person receives the first dose of a 2-dose vaccine, this metric stays the same. If they *        receive the second dose, the metric goes up by 1.
*    not_fully_vaccinated_per_hundred:
*        people not vaccinated per 100 people in the total population of the country
*    boosted_per_hundred:
    *    people who have received their booster dose per 100 people in the total population of the country

### 9.	Data Extract:

The Extract phase of the assignment uses urls / wget downloads in place of API calls are they are not available for the datasets needed. The three JHU time series data sets are retrieved using this method.

The Vaccination and Population data sets are downloaded from their respective sites as CSV files using the Pandas pd.read_csv function.

### 10.	Data Transform:

The data Transform steps are as follows:
1)	Save DFs to CSVs to do exploratory data analysis.
2)	Conduct exploratory data analysis.
3)	Use melt() to unpivot DataFrames from current wide format 265 rows × 749 columns              into long format 208600 rows × 6 columns.
4)	Remove recovered data for Canada due to mismatch issue. Canada recovered data is counted for the whole Country instead of by Province/State which is how Canada and           the rest of the world count data for "Confirmed Cases" and "Deaths".
        We could apportion recovered data for the country across the Province/States in the same ratio as confirmed cases but this is not recommended as best practice as it
	arbitrarily interferes with source data.
5)	Merge the three JHU dataframes, Confirmed Cases, Deaths, Recovered Cases.
*        a.	merge confirmed_df_long and deaths_df_long into full_table
*        b.	merge full_table and recovered_df_long
6)	Check Canada data in "full_table" - "recovered" should be 0 and a check of CSV file confirms that it is.
7)	Convert date from string to datetime.
8)	Detect missing values NaN.
9)	Replace 'recovered' NaNs with zero.
10)	Three cruise ships need to be treated differently to the rest of the cases.So extract and remove data for these ships.
11)	Calculate active cases = confirmed cases - deaths – recovered cases.
12)	Aggregate data into Country/Region and group by Date and Country/Region.
13)	Calculate daily New cases, New deaths and New recovered by deducting the corresponding accumulative data on the previous day
14)	Use pd.merge to group the final data frame on Country/Region / Date.
15)	Fix the new data types as integer.
16)	The final data frame is sorted by Date and Country/Region ascending where: -	
*           Confirmed Cases, Deaths, Recovered and Active are cumulative data for the   entire period, and, 
*           New cases, New deaths and New Recovered are daily incremental data.
17)	Convert data frame to a csv file for backup.
18)	Select Australia to check that data is correct. Validate the final data frame against the JHU Dashboard for 06/02/2022.
*           Both showed Confirmed Cases = 2,704,275 and Deaths = 4,154 for Australia.
19)	Read the Vaccination dataset - csv file into a data frame.
20)	Derive the “people_not_vaccinated” from the “people_fully_vaccinated”.
21)	Detect missing values NaN 
22)	Replace NaNs with zero
23)	Data cleansing replace ”United States” with “US” to standardise data.
24)	Save cleansed vaccination data to a CSV for backup.
25)	Read the Population data set - csv file into a data frame.
26)	Detect missing values NaN 
27)	Replace NaNs with zero
28)	Save cleansed Population data to a CSV for backup.
29)	Copy OWID Vaccination data frame, as we want to use OWID country codes.
30)	Add Africas to match population data frame.
31)	Edit “full_grouped” covid case data frame to include country ID.
32)	Change structure of data frames to match structure of tables created in the database.
33)	Set index of country codes data frame and remove null index row.
34)	Covid Cases table - copy only the columns needed into a new Data Frame.
35)	Rename columns to fit the tables created in the database.
36)	Vaccinations table - copy only the columns needed into a new Data Frame.
37)	Rename columns to fit the tables created in the database.
38)	Create PostgreSQL database connection.
39)	Confirm database tables.
40)	Load data frames to database tables


### 11.	 Exploratory Data Analysis Using Pandas Profiling:
We tried using Pandas Profiling to analyse the merged JHU data frames but the analysis ran for 15 hours and only completed 18% of the analysis 
in that time so it was aborted. At that rate it would have taken in excess of 83 hours to complete the analysis. This will be revisited after the Project-2 assignment has been submitted.
Instead, we ran Pandas Profiling on the 3 data frames produced from the Extract / Transform process to validate their integrity. 

#### 1)	EDA – Confirmed Cases

The exploratory data analysis for Confirmed Cases ran successfully. The Output Report is located in the first appended Chrome HTML document. 

#### 2)	EDA – Vaccinations

The exploratory data analysis for Vaccinations ran successfully. The Output Report is located in the second appended Chrome HTML document. 

#### 3)	EDA – World Population

The exploratory data analysis for World Population  ran successfully. The Output Report is located in the third appended Chrome HTML document.

______________________________________________________________________________________________________________________________________________________________________________





